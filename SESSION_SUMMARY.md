# Resumo da sessão — Site Pizzaria Recanto do Sossego

> Documento de contexto para retomar o trabalho em sessões futuras.

## 🎯 Objetivo do projeto

SPA estático (HTML único + Tailwind CDN + JS vanilla) para servir como **cardápio digital e ponto de pedido** da Pizzaria Recanto do Sossego, hospedado no **GitHub Pages**. Sem build pipeline, sem framework, sem backend — só commit e deploy.

**Localização**: Valença, BA. Horário: Quinta a Domingo, 18h às 23h.

## 🎨 Decisões de design (definitivas)

### Direção visual confirmada
- **Creme + verde, aconchegante** (não dark mode)
- Estilo: "pizzaria de família com horta nos fundos", não "trattoria minimalista urbana"
- Inspiração: a marca já existe e é ilustrada/flat sobre fundo verde com texto vermelho — o site segue o mesmo DNA

### Paleta (extraída do logo, configurada no Tailwind como `sossego.*`)
```
sossego.100:       #FFF4DC  ← creme massa (fundo principal)
sossego.500:       #F4C24A  ← amarelo queijo (acentos)
sossego.red:       #E63027  ← vermelho da marca (CTAs, destaques)
sossego.redDark:   #B81F18
sossego.green:     #2D6B3F  ← verde sossego (fundo seção cardápio)
sossego.greenDark: #1F4A2C  ← verde profundo (cabeçalhos, hero)
sossego.greenDeep: #142E1B  ← mais escuro ainda (footer)
sossego.brown:     #5C3A1E  ← marrom forno (texto sobre creme)
sossego.ink:       #1A1410  ← preto profundo (footer ink)
```

### Tipografia (Google Fonts)
- **Display**: `Fraunces` — serif variável com italic dramático, usado em títulos grandes
- **Body**: `DM Sans` — sans humanista, texto corrido
- **Acento manuscrito**: `Caveat` — para frases tipo "feita na hora", subtítulos divertidos

### Ilustrações
- **Decisão**: híbrido confirmado pelo usuário — SVGs ilustrados flat (combinando com o logo) com gradientes radiais sutis dando profundidade. **Sem 3D real**.
- Todas as ilustrações são SVG inline (pizza, calzone, forno, bebidas). Pasta `assets/` foi removida — não é necessária por enquanto.
- Futuro: o usuário pode substituir SVGs por renders 3D de Blender/Spline quando quiser. Estrutura preparada pra isso.

## 🛠️ Stack técnica

- **HTML único** + **Tailwind via CDN** (Play CDN: `https://cdn.tailwindcss.com`)
- **JS vanilla** — sem framework, sem build
- **Google Fonts** carregado via `<link>`
- **JSON externo** em `/data/menu.json` carregado via `fetch()`, com **fallback inline** (`INLINE_MENU`) caso o fetch falhe (ex: abrir via `file://` localmente)

### Detalhe importante sobre sandbox
- `cdn.tailwindcss.com` **NÃO está nas allowed_domains do container Claude**, então para preview local foi necessário compilar Tailwind offline (`tailwind.css`, 19KB).
- No GitHub Pages do usuário (internet pública) o CDN funciona normal — o site renderiza idêntico aos screenshots.
- O `tailwind.css` compilado é mantido no entregável como **opção offline** (caso o usuário queira não depender do CDN).

## 📁 Estrutura de arquivos do entregável

```
recanto-do-sossego/
├── index.html          ← 60KB, site completo (HTML + Tailwind config inline + JS)
├── data/
│   └── menu.json       ← cardápio editável (fonte da verdade)
├── tailwind.css        ← 19KB, opcional, pré-compilado (fallback sem CDN)
└── README.md           ← instruções de deploy + edição
```

**Não estão no entregável** (apenas durante desenvolvimento):
- `index-preview.html` (cópia com link local pro CSS, usada só para screenshots)
- `tailwind.config.js`, `input.css`, `package.json` (em `/tmp/` no container)
- `shot.py` (script Playwright pra screenshots)
- `screenshots/` (9 PNGs de validação visual)

## 🧩 Seções do site (ordem)

1. **Navbar fixa** — logo SVG inline + nome em duas linhas ("recanto" / "do sossego"). **Cores adaptativas via JS no scroll** (IDs `logoTop`/`logoBot`): creme+amarelo sobre hero verde escuro, verde escuro+vermelho sobre creme depois. Badge contador no botão "Pedido".
2. **Hero** — fundo verde escuro com gradient radial + grain SVG noise. Título Fraunces gigante em 3 partes ("o sabor / que mora / longe da pressa"). Pizza SVG ilustrada à direita girando lentamente (`animate-slow-spin`, 60s). Vapor SVG animado. Selo "feita na hora" rotacionado. CTAs vermelho/outline. Pílulas de info (horário, local, WhatsApp). Onda SVG divisora no rodapé.
3. **Nosso Recanto** (creme) — forno SVG ilustrado à esquerda + manifesto à direita. Título com wavy underline vermelho em "desacelera". 3 mini-stats (25+/100%/4 noites).
4. **Cardápio** (verde escuro) — 4 tabs: Salgadas / Calzones / Doces / Bebidas. Cards "postcard" com SVG da categoria, ingredientes em itálico, linha pontilhada estilo cardápio rústico separando nome de preço. Salgadas e doces têm 2 botões (`+Média` verde, `+Grande` vermelho); calzones e bebidas têm preço único (`+Adicionar`). Badge "ESPECIAL" rotacionado em sabores premium (Camarão Gorgonzola, Da Casa, Sertaneja).
5. **Monte a sua** (creme) — pizza SVG ao vivo à esquerda + controles à direita. Tamanho M/G (base R$ 40/50), molho (tomate caseiro / branco), até 5 ingredientes (R$ 4 cada). Total dinâmico. IDs SVG principais: `bSauce`, `bCheeseLayer`, `bSauceSpots`, `bToppings`. Função-chave: `toppingSvgPart(t, cx, cy, i)` renderiza cada topping.
6. **Contato** (verde profundo) — info (horário, WhatsApp 5575999759797, Instagram @pizzariarecantodosossego) + "cartão postal" decorativo rotacionado + selo "FEITO COM FOGO" + CTA WhatsApp.
7. **Footer** ink — copyright + nota sobre `/data/menu.json`.
8. **Drawer carrinho** lateral (slide da direita via `translateX`), com itens, total, botão "Finalizar no WhatsApp" que gera mensagem formatada e abre `wa.me`.
9. **FAB WhatsApp** flutuante com pulse animation.

## 🔧 Detalhes técnicos relevantes

### Animações CSS customizadas
- `slow-spin` — pizza do hero gira (60s)
- `float` — usado em selos
- `steamRise` — vapor da pizza
- `fadeUp` — entrada de elementos
- `wapulse` — pulse no botão flutuante

### Animações JS
- **Reveal on scroll** via IntersectionObserver (`.reveal` → `.visible`)
- **Fly-to-cart** — bolinha vermelha voa do botão "+" até o ícone do carrinho ao adicionar item

### Carrinho — formato da mensagem WhatsApp
```
🍕 *PEDIDO · RECANTO DO SOSSEGO* 🍕

1. Pizza Da Casa (G)
   _R$ 60,00_
2. Pizza Montada M · molho tomate (pepperoni, manjericao)
   _R$ 48,00_

*Total: R$ 108,00*

Obrigado! 🌿
```

### Configuração do contato (em `data/menu.json` → `pizzaria`)
```json
{
  "nome": "Pizzaria Recanto do Sossego",
  "horarios": "Quinta a Domingo | 18h–23h",
  "whatsapp": "5575999759797",
  "instagram": "pizzariarecantodosossego"
}
```

## 🍕 Estrutura do cardápio (esquema do menu.json)

```json
{
  "pizzaria": { "nome", "horarios", "whatsapp", "instagram" },
  "calzones": [{ "nome", "preco" }],
  "salgadas": [{ "nome", "ingredientes", "precoG", "precoM" }],
  "doces":    [{ "nome", "precoG", "precoM" }],
  "bebidas":  [{ "nome", "preco" }]
}
```

**Totais no momento da entrega**: 10 calzones, 15 salgadas, 5 doces, 3 bebidas.

## ✅ Status atual

**Pronto e entregue**:
- Design completo e validado visualmente (9 screenshots: 4 sections desktop + carrinho + builder + mobile hero)
- Todas as funcionalidades funcionando: tabs do cardápio, carrinho, monte sua pizza, integração WhatsApp
- Responsivo (testado em 1440x900 e 390x844)
- Logo do navbar com cores adaptativas no scroll (corrigido — antes ficava invisível sobre hero verde)
- Builder de pizza com toppings em tamanho legível (corrigido — antes estavam muito pequenos)
- Camadas do builder ajustadas (queijo + molho dão tom alaranjado realista de pizza assada, manchas de molho dão textura)

**Issues resolvidos durante a sessão**:
1. ✅ "recanto" invisível sobre hero verde → resolvido com JS adaptativo via IDs `logoTop`/`logoBot`
2. ✅ Molho do builder cobrindo queijo demais → camadas equilibradas + manchas elípticas
3. ✅ Toppings pequenos/sumidos → pepperoni r=22, tomate r=13, catupiry r=15, etc

## 🔮 Próximos passos sugeridos (NÃO implementados, abertos para futuras sessões)

- [ ] **Substituir SVGs ilustrativos por renders 3D** (Blender/Spline → PNG na pasta `assets/`) quando o usuário tiver os renders prontos
- [ ] **PWA** — adicionar `manifest.json` e service worker para instalável no celular
- [ ] **Google Analytics ou Plausible** para tracking
- [ ] **Foto real da fachada/forno** na seção "Nosso Recanto"
- [ ] **Galeria do ambiente** (foi oferecida como opção mas o usuário não selecionou)
- [ ] **Filtros e busca** no cardápio (oferecido, não selecionado — pode ser útil se cardápio crescer muito)
- [ ] **Histórico de pedidos** localStorage (não pedido, mas seria útil)
- [ ] **Sistema de cupons** (não pedido)

## 🤝 Como retomar em sessão futura

Se o usuário pedir ajustes/extensões:

1. **Lugar do código fonte de referência**: o `index.html` entregue é a fonte da verdade. Pedir para o usuário compartilhar a versão atual dele (caso já tenha customizado).
2. **Para teste local**: o Tailwind CDN não está nas allowed_domains do container Claude, então usar a versão pré-compilada (`tailwind.css`) + remover bloco `tailwind.config = {...}` inline + trocar `<script src="cdn">` por `<link href="./tailwind.css">`. Isso é só para validar visualmente; **não enviar essa versão modificada como entregável** — sempre manter o CDN no `index.html` final que vai pro usuário.
3. **Para tirar screenshots**: subir servidor local com `setsid python3 -m http.server 8765 < /dev/null` (importante o `setsid` + redirect, senão o processo morre quando a sessão de bash retorna), depois usar Playwright (`/usr/local/lib/python3.12/dist-packages/playwright`).
4. **Para mudar cores**: editar o objeto `colors.sossego` na config Tailwind inline no `<head>`. Se for usar o CSS pré-compilado, precisa recompilar com `npx tailwindcss -i input.css -o tailwind.css --minify`.
5. **Idioma do usuário**: pt-BR. Tom: descontraído, "manhê" mas profissional. Aceita gírias leves ("tá ficando uma beleza"). Usa "você" não "tu".

## 📦 Como o entregável foi empacotado

- ZIP: `recanto-do-sossego.zip` (~24KB com compressão de 76%)
- Arquivos individuais também disponíveis em `/mnt/user-data/outputs/recanto-do-sossego/`
- Apresentado via `present_files` com o ZIP em primeiro lugar

## 🧠 Notas sobre o usuário

- Tem o logo da pizzaria pronto (PNG flat, pizza ilustrada sobre fundo verde com texto vermelho)
- Sabe o que quer mas é receptivo a recomendações fundamentadas
- Inicialmente sugeri direção "trattoria minimalista" → quando viu o logo, percebi que pedia "pizzaria de bairro aconchegante" e reorientei. Lição: **olhar o brand existente antes de propor direção**.
- Quer GitHub Pages explicitamente (zero build, simples)
- Cardápio é dele, completo, em JSON pronto pra usar
