# ByteFix — Handoff do projeto

Estado do projeto para quem for continuar. **Atualizado em 19/07/2026.**

## O que é
Site estático (HTML + CSS + JavaScript puro, **sem build, sem dependências**) da **ByteFix**
— manutenção de impressoras, notebooks e desktops, criação de sites e chat bot de
automação no WhatsApp.

## Está no ar
- **Produção: https://bytefix.net.br** (domínio próprio, SSL, `www` → 308 → apex)
- Hospedagem: **Vercel**, projeto `bytefix`, time `drastea`
- Repo: **github.com/LuanDev27/bytefix** — **push na `main` publica sozinho** (~45s)
- O antigo `bytefix-eta.vercel.app` continua respondendo, mas o endereço oficial é o domínio.

## Identidade da marca
- Nome: **ByteFix**
- Cores: azul-marinho `#0A2540` / `#123A63`, azul claro `#2E9BE6` / `#4FC3F7`, branco `#FFFFFF`
- Fonte: **Poppins** (Google Fonts)
- Logo: engrenagem + `</>`; embutido como SVG inline na constante `LOGO` do `<script>`

## Dados de contato (configurados no `CONFIG` de cada página)
- WhatsApp: **(98) 98189-8155** — o link usa `5598981898155`
- Cidade: **São Luís – MA**
- Horário: **Seg a Sex, 9h às 18h**
- Instagram: **@lbytefixl** (é esse mesmo, com os "l" nas pontas — `@bytefix` não era disponível)

⚠️ O `CONFIG` está **repetido em cada arquivo HTML**. Mudou número ou handle? Mude em todos.

## Arquivos
- `index.html` — site principal
- `criacao-de-sites-sao-luis.html` — página de serviço (SEO)
- `chatbot-whatsapp-sao-luis.html` — página de serviço (SEO)
- `manutencao-de-computadores-sao-luis.html` — página de serviço (SEO)
- `demos.html` — galeria de exemplos
- `demo-pizzaria.html`, `demo-barbearia.html`, `demo-estetica.html`, `demo-academia.html` — portfólio
- `img/port-*.jpg` — screenshots do portfólio · `img/*-hero.jpg`, `img/vitrine/` — artes
- `img/pizza-*.jpg`, `img/barbearia-*.jpg`, `img/academia-*.jpg`, `img/estetica-*.jpg` —
  fotos dos sliders das demos (Unsplash). **Procedência e licença em `img/CREDITOS-FOTOS.md`**
  (esse .md está no `.vercelignore`, fica no repo mas não é publicado).
- `sitemap.xml` (9 URLs), `robots.txt`, `favicon.svg`, `site.webmanifest`, `og-image.jpg`
- `vercel.json` — headers de segurança (CSP, HSTS). **A CSP não permite script/CSS externo** —
  tudo tem que ser inline ou do próprio domínio; Google Fonts está liberado.

## Slider do hero das demos (feito em 19/07/2026)

As 4 demos tinham um hero de 2 colunas (texto + foto). Agora o hero é **centralizado, com um
slider** que troca sozinho a cada 4,2s, tem setas, dots, swipe no touch, pausa no hover e
quando a aba perde o foco, e respeita `prefers-reduced-motion`. Tudo em CSS + Web Animations,
**sem dependência** — a CSP continua bloqueando script/CSS externo.

- **Pizzaria** (`.pz-*`): leque de fatias. Cada cunha é recortada com `clip-path` **a partir do
  centro da pizza dentro da foto**, não é um triângulo sobre a imagem inteira. Por isso cada
  foto tem `cx/cy/r` (centro e raio da pizza, em fração da largura/altura) no array `P` do
  `<script>`. **Trocou a foto? Reajuste esses três números**, senão a cunha pega tábua e fundo.
- **Barbearia, academia e estética** (`.cf-*`): coverflow de cards em arco 3D (`rotateY` +
  perspectiva). Reaproveita a classe `.hero` de cada demo, então herda o tema — e por usar só
  as variáveis (`--pri`, `--line`, `--bg`) funciona tanto no escuro quanto no claro (a
  estética é tema claro).

### Regras que não podem ser quebradas

1. **Número ímpar de cards.** Com número par não existe posição central exata: o leque fica
   torto e nenhum card fica "ativo". Todas usam 5.
2. **O rótulo tem que bater com o que a foto mostra.** Ao montar isso, várias fotos não eram o
   que o termo de busca prometia. Quando não havia foto livre do serviço, **o rótulo foi mudado
   para bater com a foto, nunca o contrário**: na pizzaria "Quatro Queijos" e "Portuguesa"
   viraram **Funghi** e **Vegetariana**; barbearia e estética ganharam um 5º serviço
   (Finalização & Pezinho, Extensão de Cílios).
3. **O slider e a lista de serviços de baixo têm que continuar batendo.** Mexeu num, mexa no
   outro — senão o site anuncia algo que o cardápio não tem.
4. **Antes de usar qualquer foto do Unsplash, cheque se é Unsplash+ (paga).** A busca mistura
   as duas coisas sem avisar; três candidatas foram descartadas por isso. Comando em
   `img/CREDITOS-FOTOS.md`.
5. **Se alguma demo virar site de cliente real, troque as fotos pelas dele.** Foto de banco num
   cardápio real é o mesmo problema do rótulo mentiroso, só que mais convincente.

### Regerar os screenshots do portfólio

`img/port-*.jpg` são 960x720, geradas com Chrome headless a 1280x960 e reduzidas com PIL
(JPEG q82 progressivo):

```bash
python -m http.server 8899 &
chrome --headless=new --disable-gpu --hide-scrollbars \
  --screenshot=shot.png --window-size=1280,960 --virtual-time-budget=2500 \
  http://localhost:8899/demo-pizzaria.html
```

⚠️ **O `--virtual-time-budget` tem que ficar entre 1600ms e 4200ms.** Abaixo de 1600 a animação
dos contadores ainda não terminou (a imagem antiga da pizzaria estava assim, mostrando
"+17 sabores" e "2.8★" em vez de "+30" e "4.9★"). A partir de ~4000 a captura cai no fade do
primeiro avanço do autoplay e sai **sem o nome do sabor/serviço**. 2500 funciona.

## Preços atuais no site
| Item | Valor |
|---|---|
| **Plano Master** (Chat Bot + Site + Artes) | de R$ 2.499 por **3x R$ 399** ou **R$ 1.199 à vista** |
| Manutenção Avulsa | a partir de **R$ 99**/serviço |
| Upgrade + Reparo | a partir de **R$ 169**/serviço |
| Criação de Site | a partir de **R$ 899,99**/projeto |
| Agente Chat (avulso) | **R$ 1.499** de adesão |
| Assinatura Essencial / Profissional / Premium | **R$ 79,99** / **R$ 169,99** / **R$ 199,99** por mês |

**O Plano Master é condição de lançamento, válida só para os 3 primeiros clientes.** Isso
aparece em **4 lugares** que precisam continuar batendo entre si: o badge do card, o rodapé
do card, o FAQ visível e o `FAQPage` do JSON-LD.

**Troca de tela/teclado de notebook** saiu do card de R$ 169 (dá trabalho demais pro preço),
mas continua sendo oferecida **sob orçamento** — está dita assim na página de manutenção.

## SEO (feito em 15/07/2026)
- **Search Console**: propriedade de **Domínio** (`sc-domain:bytefix.net.br`), verificada por
  registro TXT no Registro.br. Cobre páginas novas sem reverificar. Sitemap enviado.
- Indexação solicitada para as 3 páginas de serviço + a home. **Indexar leva de dias a 2
  semanas** — conferir em Search Console → Páginas. Não reenviar as mesmas URLs todo dia.
- **Perfil da Empresa no Google**: cadastrado como **área de serviço** (atende em domicílio,
  endereço oculto), sem CNPJ, categoria principal "Serviço de reparo de computadores".
- Todas as páginas têm title, description, canonical, OG/Twitter e JSON-LD. O `index.html`
  tem `LocalBusiness` + `FAQPage`; as de serviço têm `Service` + `BreadcrumbList`.

## Pendências
1. **Depoimentos são inventados** (Mariana S., Ricardo A., Juliana P.). Estão no ar com o
   aviso visível *"Exemplos editáveis — troque por depoimentos de clientes reais antes de
   divulgar"*. Decisão do dono em 15/07/2026: manter por enquanto. **Se for trocar por reais,
   troque os dois juntos** — apagar só o aviso e deixar os falsos transforma um site
   desajeitado num site que engana. Não existe `aggregateRating` no JSON-LD, e é bom que
   continue assim enquanto não houver avaliação real.
2. **Pedir avaliações reais no Perfil da Empresa.** É o que mais pesa no mapa do Google para
   "manutenção de notebook São Luís" — mais que qualquer coisa no código.
3. Conferir a indexação a partir de ~20/07/2026.
4. **6 arquivos sem uso em `img/`**, sobras da construção do slider, deixados fora dos commits
   de propósito (não são referenciados por página nenhuma, e subir só engorda o deploy):
   `barbearia-degrade.jpg` e os 5 `fatia-*.svg` (versão ilustrada da pizzaria, substituída
   pelas fotos). Ficam como *untracked*, então **o `git status` aparece sujo** — é esperado.
   Decidir se apaga.

## Notas técnicas para quem for editar
- **Os HTML NÃO estão minificados.** O `index.html` tem **~4000 linhas**, indentação normal de
  2 espaços, já em formato prettier. **Não precisa reformatar.** O `Read` falha nele sem
  argumentos só porque passa das 2000 linhas — use `offset`/`limit`, ou `grep -n` para achar
  o trecho antes.
- **O portfólio usa imagens estáticas** (`img/port-*.jpg`), não iframes. Já foi iframe e
  **travava o iPhone** — não volte atrás.
- **As 3 páginas de serviço têm CSS próprio inline**, não compartilhado com o `index.html`.
  Foi decisão consciente (não mexer no CSS de um site no ar). O custo: **mudou o design da
  home, tem que replicar nas três**.
- No mobile, as páginas de serviço escondem os links de texto do header mas **mantêm o botão
  de WhatsApp** — elas não têm o menu hambúrguer do index. É proposital.
- **Nas demos, a faixa `.demoflag` e o logo disputam o topo-esquerdo.** A faixa é `fixed` no
  canto e chegava a cobrir os ~12px superiores do logo. Por isso o `header` das demos tem
  `padding-top` grande (**42px**, e **40px** no estado `.sc` do scroll) — não reduza sem
  medir de novo `.brand`/`.demoflag`, ou o logo volta a ficar cortado.
- Pelo mesmo motivo, o dropdown do menu mobile das demos usa **`top:100%`**, não um valor fixo:
  ele tem que acompanhar a altura do header. Com o `top:60px` antigo ele abria por cima do logo.
