# Zequi Lanches — Landing Page

Landing page de página única para a **Zequi Lanches** (Cândido Mota/SP), com cardápio,
fotos reais dos lanches e pedido direto pelo WhatsApp. Tema escuro com o vermelho e o
dourado da marca, tipografia pesada estilo lanchonete e layout responsivo.

## Visão geral

- **Um único arquivo** (`zequi-lanches.html`) — HTML, CSS e JavaScript embutidos. Sem build, sem dependências para instalar.
- **Fotos embutidas em base64**, então o arquivo funciona sozinho, sem precisar levar uma pasta de imagens junto.
- Fontes carregadas do Google Fonts (**Anton** para títulos, **Inter** para o texto) — única dependência externa, exige internet para exibir a tipografia certa.

### Seções da página

Cabeçalho fixo · Hero (chamada + card promocional) · Barra de números (25+ anos, 100%, 4.9) ·
Cardápio · Nossa história · Onde estamos · "Bateu a fome?" (CTA) · Rodapé.

### Recursos

- Cardápio montado dinamicamente a partir de um array no JS.
- Botões e itens do cardápio abrem o **WhatsApp com a mensagem já preenchida** (clicar num lanche monta o pedido daquele item).
- Menu hambúrguer no mobile, rolagem suave, animação de entrada ao rolar.
- Acessibilidade básica: foco visível no teclado e respeito a `prefers-reduced-motion`.

## Como executar

É um arquivo estático. Basta abrir no navegador:

```bash
# clicar duas vezes no arquivo, ou:
xdg-open zequi-lanches.html      # Linux
open zequi-lanches.html          # macOS
```

Se quiser servir localmente (recomendado para testar como em produção):

```bash
python3 -m http.server 8000
# depois acesse http://localhost:8000/zequi-lanches.html
```

## Como personalizar

Tudo fica dentro do próprio `zequi-lanches.html`.

### Cardápio

No bloco `<script>`, edite o array `MENU`. Cada item tem nome, preço e taxa de entrega:

```js
const MENU = [
  { ico: "🍔", nome: "X Frango", preco: 28, entrega: 5 },
  // ...adicione, remova ou altere itens aqui
];
```

- `ico` é o emoji usado como **fallback** caso o item não tenha foto.
- Para tirar a taxa de entrega de um item, use `entrega: '--'` (como no MEGA - 1KG).

### Fotos dos lanches

As imagens ficam no objeto `IMG`, logo acima do `MENU`, indexadas pelo **nome exato** do lanche:

```js
const IMG = {
  "X Frango": "data:image/jpeg;base64,....",
  // ...
};
```

Para o item exibir a foto, a chave em `IMG` precisa bater com o `nome` no `MENU`.
Se não houver entrada em `IMG`, o card cai para o emoji automaticamente.

**Trocar ou adicionar uma foto:** gere o `data:` da imagem já otimizada. As atuais são
recorte quadrado 300×300, JPEG qualidade 80. Um jeito rápido:

```bash
# converte uma imagem em data URI (base64), pronto para colar no IMG
python3 - <<'PY'
from PIL import Image, ImageOps
import base64, io
im = ImageOps.exif_transpose(Image.open("foto.jpg")).convert("RGB")
im = ImageOps.fit(im, (300, 300), Image.LANCZOS)   # recorte quadrado central
buf = io.BytesIO(); im.save(buf, "JPEG", quality=80, optimize=True)
print("data:image/jpeg;base64," + base64.b64encode(buf.getvalue()).decode())
PY
```

Preferindo não usar base64, dá para trocar por caminhos de arquivo
(`"X Frango": "imagens/xfrango.jpg"`) e manter uma pasta `imagens/` ao lado do HTML — mais
leve, porém exige subir a pasta junto.

### WhatsApp

O número fica na constante `PHONE` (formato internacional, só dígitos):

```js
const PHONE = "5518997047762";   // (18) 99704-7762
```

As mensagens padrão dos botões estão no atributo `data-msg` de cada elemento com `data-wa`
no HTML; a mensagem de pedido de um item é montada na função de clique do cardápio.

### Contato e endereço

Procure no HTML por:

- Telefone exibido / link `tel:` → seção **CTA** ("Bateu a fome?").
- Endereço e link do mapa → seção **Onde estamos** (o card leva ao Google Maps).

### Cores e marca

A paleta está nas variáveis CSS no topo do `<style>` (`:root`). Ajuste ali para mudar o tema inteiro:

```css
--red:  #c81d1d;   /* vermelho principal   */
--gold: #f5a623;   /* dourado de destaque  */
--bg:   #0d0b0b;   /* fundo escuro         */
```

O texto do card promocional do topo (hero) é fixo no HTML — foi mantido como está,
sem foto, conforme combinado.

## Publicar (deploy)

Por ser um arquivo estático, dá para hospedar em qualquer lugar:

- **GitHub Pages** — suba o arquivo no repositório e ative o Pages nas configurações.
- **Netlify / Vercel** — arraste o arquivo (ou conecte o repositório); publica em segundos.
- **Hospedagem tradicional** — envie o `zequi-lanches.html` via FTP; renomeie para
  `index.html` se quiser que ele seja a página inicial do domínio.

## Tecnologias

HTML5, CSS3 (variáveis, grid, flexbox, `clip-path`), JavaScript puro
(`IntersectionObserver` para as animações). Sem frameworks.

## Estrutura

```
zequi-lanches.html   # a página inteira (HTML + CSS + JS + fotos em base64)
README.md            # este arquivo
```

---

© 2026 Zequi Lanches — 25 anos de tradição.
