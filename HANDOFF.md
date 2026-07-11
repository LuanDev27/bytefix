# ByteFix — Handoff do projeto

Resumo de tudo que foi construído, para continuar no Claude Code e publicar.

## O que é
Site estático (HTML + CSS + JavaScript puro, **sem build, sem dependências**) da **ByteFix**
— empresa de **manutenção de impressoras, notebooks e desktops** e **criação de sites**.

## Identidade da marca
- Nome: **ByteFix**
- Cores: azul-marinho `#0A2540` / `#123A63`, azul claro `#2E9BE6` / `#4FC3F7`, branco `#FFFFFF`
- Fonte: **Poppins** (Google Fonts)
- Logo: engrenagem + `</>` (une "manutenção" e "sites"); embutido no site como SVG inline

## Dados de contato (já configurados no site)
- WhatsApp: **(98) 98189-8155** — o link usa `5598981898155`
- Cidade: **São Luís – MA**
- Horário: **Seg a Sex, 9h às 18h**
- Instagram: **@bytefix** (⚠️ registrar/renomear o perfil antes de divulgar)

## Arquivos
- `index.html` — site principal
- `demos.html` — galeria de exemplos
- `demo-pizzaria.html`, `demo-barbearia.html`, `demo-estetica.html`, `demo-academia.html` — landing pages de exemplo (portfólio)
- `art-pizzaria.svg`, `art-barbearia.svg`, `art-estetica.svg`, `art-academia.svg` — ilustrações
- `README.md`, `.gitignore`

## Recursos já implementados no index.html
- Seções: serviços, diferenciais, "como funciona" (funil), preços, portfólio, depoimentos, FAQ
- **Preços**: Manutenção Avulsa a partir de **R$ 100** · Upgrade + Reparo a partir de **R$ 170** · Criação de Site a partir de **R$ 800**
- **Portfólio com prévia ao vivo**: cada card mostra o próprio site de exemplo em miniatura (iframe) — funciona melhor publicado
- **Depoimentos com avatares ilustrados** (neutros, não são fotos reais)
- **Números com contagem crescente** (ex.: 0→24h) ao entrar na tela
- **Controle de videogame** no topo que se ajeita conforme a rolagem
- Botões de **WhatsApp** em todos os CTAs + botão flutuante
- Responsivo (desktop e celular)

## Pendências
1. **Trocar os depoimentos de exemplo por clientes reais** (nome + texto). Estão marcados com ⚠️ no código.
2. Registrar/renomear o Instagram para **@bytefix**.
3. Publicar (ver abaixo).

## Como publicar (no Claude Code)
Dentro desta pasta, peça ao Claude Code:
> "Crie um repositório no meu GitHub chamado `bytefix` e faça push desta pasta."

Ou manualmente no terminal, dentro da pasta:
```
git init
git add .
git commit -m "ByteFix: site institucional + landing pages"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/bytefix.git
git push -u origin main
```
Depois: importe o repositório em **vercel.com/new** (Framework: **Other**) → Deploy.
A cada push, a Vercel republica sozinha.

## Observação técnica
Os arquivos HTML estão **minificados numa linha só**. Ao editar, evite formatadores automáticos
que quebrem o bloco `<script>`; ou peça ao Claude Code para reformatar com segurança antes de editar.
