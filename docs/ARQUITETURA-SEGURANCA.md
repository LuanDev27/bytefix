# ByteFix — Arquitetura de Segurança & Blueprint de Upgrade

> Documento de referência. O site **hoje** é estático (HTML/CSS/JS puro, sem build).
> Este arquivo descreve (1) a segurança já aplicada e (2) o esquema para quando o
> ByteFix ganhar backend — momento em que os princípios **SOLID** passam a valer.

---

## 1. Estado atual (site estático)

Não há backend, autenticação nem dados do usuário. A "segurança" se resume à entrega:

| Camada | Situação |
|---|---|
| HTTPS/TLS | Automático pela Vercel |
| Security headers | ✅ Configurados em `vercel.json` (CSP, HSTS, nosniff, Referrer-Policy, Permissions-Policy, X-Frame-Options) |
| Segredos | `.env.local` (token OIDC) coberto pelo `.gitignore` (`.env*`) — nunca commitar |
| Superfície de ataque | Praticamente nula (conteúdo estático) |

**Não faz sentido avaliar SOLID hoje** — não existe código orientado a objetos nem
arquitetura de domínio. SOLID vale a partir da Fase 2.

### Headers aplicados (`vercel.json`)
- **CSP** — trava origens de script/estilo/fonte/imagem/iframe. Hoje permite
  `'unsafe-inline'` porque os scripts/estilos são inline e minificados. **Ao migrar
  para backend, remover `'unsafe-inline'` e adotar nonce/hash por request.**
- **HSTS** — força HTTPS (com `preload`).
- **X-Content-Type-Options: nosniff**, **Referrer-Policy**, **Permissions-Policy**,
  **X-Frame-Options: SAMEORIGIN** (os previews de portfólio usam iframe same-origin).

---

## 2. Blueprint do backend (gatilhos: formulário de contato, agendamento, painel, pagamentos)

Stack sugerida: **Next.js (App Router) + Fluid Compute na Vercel**, banco via
Marketplace (Neon Postgres) e auth por provedor gerenciado (Clerk/Auth0). Zero infra
custom antes do necessário.

### 2.1 Camadas (arquitetura hexagonal / ports & adapters)

```
  [ UI / Rota (Server Component, Route Handler) ]   ← entrega
                     │  DTO validado (Zod)
                     ▼
  [ Aplicação: Casos de Uso / Services ]            ← orquestra regras
                     │  depende de INTERFACES (ports)
                     ▼
  [ Domínio: Entidades + regras de negócio ]        ← puro, sem framework
                     ▲
                     │  implementa as interfaces (adapters)
  [ Infra: Repositórios, Auth, Email, Pagamentos ]  ← detalhes trocáveis
```

A regra de dependência aponta **sempre para dentro**: Infra → Aplicação → Domínio.
O Domínio não conhece Next.js, Postgres nem Stripe.

### 2.2 Onde cada princípio SOLID entra

- **S — Single Responsibility**: cada módulo com um motivo para mudar.
  Ex.: `LeadValidator`, `LeadRepository`, `NotificationService`, `RateLimiter` —
  nunca uma "God class" de contato que valida + salva + envia e-mail + loga.
- **O — Open/Closed**: novos canais de notificação (WhatsApp, e-mail, SMS) entram
  como **novos adapters** que implementam `Notifier`, sem editar o caso de uso.
- **L — Liskov**: qualquer `Notifier`/`Repository` é substituível sem quebrar o
  chamador (mesmos contratos, mesmas garantias de erro).
- **I — Interface Segregation**: interfaces pequenas e focadas
  (`ReadLeads`, `WriteLeads`) em vez de um `IRepositoryTudo` inchado.
- **D — Dependency Inversion**: casos de uso dependem de **interfaces**; as
  implementações concretas são injetadas na borda (composição na rota). Facilita
  testar (mock) e trocar provedor sem tocar na regra de negócio.

Exemplo de estrutura de pastas:

```
src/
  domain/            # entidades + regras puras (sem imports de framework)
    lead.ts
    ports/           # interfaces: LeadRepository, Notifier, Clock...
  application/        # casos de uso: CreateLead, ScheduleAppointment...
  infra/              # adapters: PostgresLeadRepository, WhatsappNotifier...
  app/                # Next.js: rotas, Server Actions (composition root)
```

### 2.3 Segurança que passa a ser obrigatória com backend

1. **Validação de entrada** — todo input validado no servidor com **Zod** antes de
   tocar no domínio. Nunca confiar no cliente.
2. **AuthN/AuthZ** — autenticação por provedor gerenciado; autorização checada no
   caso de uso (não só na UI). Princípio do menor privilégio.
3. **Rate limiting / anti-abuso** — em formulários e endpoints públicos. Considerar
   **Vercel BotID** e **Vercel WAF** (Firewall) para bots e DDoS.
4. **Segredos** — apenas em `vercel env` (nunca no repo). OIDC para acesso a
   terceiros quando possível, evitando chaves de longa duração.
5. **CSP sem `'unsafe-inline'`** — migrar para nonce/hash por request (Next.js
   suporta via middleware). Endurecer conforme os scripts deixam de ser inline.
6. **SQL seguro** — sempre via ORM/queries parametrizadas (ex.: Drizzle/Prisma),
   nunca concatenação de string.
7. **Proteção de sessão/CSRF** — cookies `HttpOnly` + `SameSite`; CSRF em mutações
   (Server Actions do Next.js já mitigam parte disso).
8. **Logs & auditoria** — sem PII sensível em log; trilha de auditoria para ações
   críticas (pagamentos, alteração de dados).
9. **Pagamentos** — delegar a provedor (Stripe); nunca armazenar dados de cartão.
10. **Dependências** — `npm audit` / Dependabot; travar versões.

### 2.4 Ordem sugerida de evolução
1. Formulário de contato → Route Handler + Zod + rate limit + notificação (1 adapter).
2. Persistência de leads → banco (Neon) por trás de `LeadRepository`.
3. Auth + painel administrativo (menor privilégio).
4. Agendamento / pagamentos (novos casos de uso, mesmos princípios).

---

## Resumo
- **Hoje:** headers de segurança em `vercel.json` + segredo protegido = feito. SOLID
  não se aplica a site estático.
- **No upgrade:** arquitetura hexagonal com SOLID no núcleo e as 10 práticas de
  segurança acima como requisito de cada endpoint.
