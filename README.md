# 🍕 Pizzaria Recanto do Sossego

Site cardápio digital · SPA estático · pronto para GitHub Pages

## Estrutura

```
recanto-do-sossego/
├── index.html         ← site completo (HTML + Tailwind via CDN + JS vanilla)
├── data/
│   └── menu.json      ← cardápio editável (fonte da verdade)
└── README.md
```

## 🚀 Deploy no GitHub Pages

1. Crie um repositório no GitHub (ex: `recanto-do-sossego`)
2. Faça upload de todos os arquivos para o repositório:
   ```bash
   git init
   git add .
   git commit -m "primeiro deploy"
   git branch -M main
   git remote add origin https://github.com/SEU_USUARIO/recanto-do-sossego.git
   git push -u origin main
   ```
3. No GitHub, vá em **Settings → Pages**
4. Em **Source**, selecione `Deploy from a branch`
5. Em **Branch**, selecione `main` e pasta `/ (root)` → **Save**
6. Aguarde 1-2 min. O site estará em `https://SEU_USUARIO.github.io/recanto-do-sossego/`

> 💡 Se preferir um domínio próprio (ex: `recantodosossego.com.br`), você pode configurar em **Settings → Pages → Custom domain**.

## ✏️ Como editar o cardápio

Tudo está em **`data/menu.json`**. Edite no GitHub direto pelo navegador, ou pelo seu editor, e faça commit. O site se atualiza sozinho.

### Adicionar uma pizza salgada
```json
{
  "nome": "Margherita Trufada",
  "ingredientes": "queijo, molho, tomate, manjericão, óleo de trufa",
  "precoG": 70.00,
  "precoM": 55.00
}
```

### Adicionar um calzone (preço único)
```json
{ "nome": "Novo Sabor", "preco": 52.00 }
```

### Mudar telefone do WhatsApp
Em `pizzaria.whatsapp` — formato `5575999999999` (código país + DDD + número, sem espaços/símbolos).

## 🧪 Testar localmente

Como o site usa `fetch()` para ler o JSON, **abrir o `index.html` direto no navegador (file://) não funciona**. Use um servidor local:

```bash
# opção 1: python (já vem instalado em Mac/Linux)
python3 -m http.server 8000

# opção 2: node
npx serve

# opção 3: VS Code → extensão "Live Server"
```

Abra: `http://localhost:8000`

> Já incluímos um fallback inline: se o fetch falhar, o site usa o menu hardcoded — então mesmo aberto via `file://` ele funciona, mas as edições no JSON externo não aparecerão. Para ver as edições, use servidor local.

## 🎨 Personalização rápida

As cores estão em `index.html` na config do Tailwind (procure por `colors: { sossego: ...`). Tudo usa essas variáveis, então mudar um valor lá repinta o site inteiro.

```js
sossego: {
  100: '#FFF4DC',   // fundo creme
  red: '#E63027',   // vermelho da marca
  green: '#2D6B3F', // verde sossego
  // ...
}
```

## 🛠️ Tech

- **HTML + Tailwind CDN** — zero build, zero dependências instaladas
- **Vanilla JS** — sem framework, bundle minúsculo
- **SVG inline** — todos os ícones e ilustrações de pizza
- **Google Fonts**: Fraunces (display), DM Sans (texto), Caveat (acento)

### Sobre o `tailwind.css`

Já incluímos uma versão **pré-compilada** do Tailwind (`tailwind.css`, ~19KB) caso queira deixar o site 100% offline / sem depender do CDN. Para usar:

1. No `index.html`, substitua a linha:
   ```html
   <script src="https://cdn.tailwindcss.com"></script>
   ```
   por:
   ```html
   <link rel="stylesheet" href="./tailwind.css">
   ```
2. Remova o bloco `<script>tailwind.config = {...}</script>` logo abaixo (já não é mais necessário).

Por padrão recomendamos manter o CDN, é mais flexível pra trocar cores no futuro.

## 📝 Próximos passos sugeridos

- [ ] Substituir SVGs ilustrativos por renders 3D (Blender/Spline → PNG) na pasta `assets/` quando estiverem prontos
- [ ] Adicionar Google Analytics ou Plausible
- [ ] Adicionar `manifest.json` para tornar instalável como PWA
- [ ] Foto real da fachada/forno na seção "Nosso Recanto"
