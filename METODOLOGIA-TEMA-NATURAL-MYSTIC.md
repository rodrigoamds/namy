# Metodologia do tema Natural Mystic

Este documento registra o processo usado para personalizar a loja Natural Mystic no plano gratuito da Nuvemshop, utilizando o campo de CSS avançado como ponto de entrada para uma folha de estilos externa.

## 1. Contexto e limitações

Loja analisada:

- Home: <https://naturalmysticlab.lojavirtualnuvem.com.br/>
- Produtos: <https://naturalmysticlab.lojavirtualnuvem.com.br/produtos/>
- Contato: <https://naturalmysticlab.lojavirtualnuvem.com.br/contato/>
- Produto de teste: <https://naturalmysticlab.lojavirtualnuvem.com.br/produtos/produto-teste-123/>
- Carrinho: <https://naturalmysticlab.lojavirtualnuvem.com.br/comprar/>

Como o plano utilizado não oferece edição completa dos arquivos do tema, a personalização foi concentrada em CSS. Para contornar o limite do editor, o campo de CSS avançado contém apenas um `@import`, e o arquivo completo fica hospedado externamente.

```css
@import url("https://vectorequilibrium.com/namy/natural-mystic-theme.css?v=4");
```

O parâmetro `v=4` serve para invalidar o cache. Sempre que o arquivo externo for substituído, esse número deve ser incrementado.

## 2. Arquivos usados

### CSS

- `natural-mystic-theme.css`: arquivo principal, legível e recomendado para manutenção e publicação.
- `natural-mystic-theme.min.css`: cópia minificada; funciona da mesma forma, mas é menos conveniente para editar.

O arquivo normal é o padrão do projeto. A economia gerada pela minificação é pequena para este caso.

### Fundos

- Desktop: <https://d1a9qnv764bsoo.cloudfront.net/stores/007/966/713/rte/namyfd02.png>
- Mobile: <https://d1a9qnv764bsoo.cloudfront.net/stores/007/966/713/rte/namyfd01.png>

Foram utilizadas imagens diferentes porque a composição vertical do celular precisa preservar o sol no topo e as ilustrações nas laterais, enquanto o desktop precisa preencher uma área horizontal larga.

## 3. Princípios visuais adotados

O tema foi construído ao redor de quatro decisões:

1. A ilustração deve continuar sendo o elemento visual principal.
2. Painéis escuros e translúcidos são usados sobre áreas coloridas.
3. Painéis claros são usados onde existe muito conteúdo, como produtos, formulários e carrinho.
4. O dourado é um acento para botões, foco, estado ativo e pequenos indicadores — não uma moldura em todos os blocos.

As principais variáveis ficam no início do CSS:

```css
:root {
  --nm-night: #050318;
  --nm-night-soft: rgba(5, 3, 24, 0.88);
  --nm-night-glass: rgba(5, 3, 24, 0.78);
  --nm-cream: #fffaf2;
  --nm-cream-soft: rgba(255, 250, 242, 0.95);
  --nm-gold: #ffb21c;
  --nm-gold-deep: #f28a0c;
  --nm-text: #fff9ed;
  --nm-ink: #24162f;
  --nm-shadow: 0 18px 48px rgba(0, 0, 0, 0.38);
}
```

Alterar essas variáveis é a maneira mais segura de mudar a identidade visual global sem editar dezenas de regras.

## 4. Método de investigação

Cada ajuste seguiu o mesmo processo.

### 4.1 Navegar pela página publicada

A página era aberta em um navegador novo, evitando que uma versão antiga do CSS permanecesse no cache. A inspeção era feita em pelo menos estas larguras:

- Desktop: `1440 × 900`.
- Celular comum: `390 × 844`.
- Celular pequeno, quando necessário: largura de `320 px`.

### 4.2 Identificar o HTML real

Os seletores foram baseados nas classes e atributos emitidos pela própria Nuvemshop, por exemplo:

```html
<section class="page-header">
<div class="js-textbanner-image-container">
<form data-store="cart-form">
<div id="shoppingCartPage">
```

Sempre que possível, foram priorizados identificadores semânticos como `data-store`, IDs e classes `js-*`. Eles tendem a ser mais específicos que seletores puramente estruturais.

### 4.3 Medir antes de alterar

Foram medidos:

- posição e dimensões dos blocos;
- largura total da página e possível rolagem horizontal;
- cor de fundo e cor do texto computadas;
- proporção renderizada das imagens;
- `object-fit`, largura e altura aplicados pelo navegador;
- comportamento em carregamento normal e lazy loading.

Isso permitiu diferenciar problemas de CSS de problemas na imagem ou na configuração do módulo.

### 4.4 Testar o CSS sem publicar

O CSS local era aplicado temporariamente em uma sessão de navegador. Dessa forma, as mudanças podiam ser comparadas com a página pública sem alterar a loja.

Quando o comportamento dependia do momento em que o CSS era carregado — especialmente imagens com lazy loading — a requisição do arquivo público era substituída pela versão local desde o início da navegação. Isso reproduziu o resultado que ocorreria depois da publicação.

### 4.5 Registrar evidências

Foram geradas capturas em `output/playwright/`, incluindo home, produtos, contato, produto individual e carrinho em desktop e mobile.

## 5. Construção do fundo responsivo

No desktop, o fundo utiliza a imagem horizontal, cobre a viewport e permanece fixo:

```css
body {
  background-image: url("https://d1a9qnv764bsoo.cloudfront.net/stores/007/966/713/rte/namyfd02.png") !important;
  background-repeat: no-repeat !important;
  background-position: center top !important;
  background-size: cover !important;
  background-attachment: fixed !important;
}
```

No mobile, a arte vertical é aplicada com largura de 100% e rolagem normal:

```css
@media (max-width: 767px) {
  body {
    background-image: url("https://d1a9qnv764bsoo.cloudfront.net/stores/007/966/713/rte/namyfd01.png") !important;
    background-size: 100% auto !important;
    background-attachment: scroll !important;
  }
}
```

Também foi aplicado `overflow-x: hidden` em `html` e `body` para impedir pequenos estouros horizontais provocados por componentes de terceiros e cálculos de largura do tema.

## 6. Painéis e contraste

Inicialmente, vários painéis receberam bordas douradas de `1px`. Durante a auditoria visual, essas linhas se encontravam de maneira desalinhada entre cabeçalhos, filtros e formulários.

A solução foi:

- retirar bordas dos painéis grandes;
- manter cantos arredondados;
- usar sombra e transparência para separar planos;
- reservar bordas douradas para campos, foco e pequenos controles.

Exemplo do padrão escuro:

```css
background: var(--nm-night-glass) !important;
color: var(--nm-text) !important;
border: 0 !important;
border-radius: 16px !important;
box-shadow: var(--nm-shadow) !important;
backdrop-filter: blur(8px);
```

Exemplo do padrão claro:

```css
background: var(--nm-cream-soft) !important;
color: var(--nm-ink) !important;
border: 0 !important;
border-radius: 18px !important;
box-shadow: var(--nm-shadow) !important;
```

## 7. Cabeçalho e navegação

O logo original foi ocultado porque a identidade já aparece na composição visual da página:

```css
#logo {
  display: none;
}
```

### Menu desktop

Os links estavam sobre o sol, onde texto branco fino perdia contraste. Em vez de texto preto com contorno branco, foi criada uma cápsula escura translúcida ao redor do conjunto. O item ativo e o hover usam dourado.

Essa solução funciona tanto sobre o sol claro quanto sobre as nuvens escuras e evita o efeito visual de legenda contornada.

### Ícones utilitários

Hambúrguer, busca, conta e sacola receberam o mesmo tratamento:

- botão circular de `36 × 36 px`;
- fundo escuro translúcido;
- borda branca discreta;
- sombra;
- ícone claro;
- hover dourado;
- contador do carrinho dourado com texto escuro.

O objetivo foi aumentar o contraste sem esconder a arte de fundo.

## 8. Módulos de imagem e texto

A primeira versão do CSS havia fixado o módulo em proporção `5:2`, adequada ao banner anterior. Quando foram inseridas imagens em proporção `4:3`, essa regra passou a cortá-las com `object-fit: cover`.

O diagnóstico mostrou que a própria Nuvemshop já marcava o módulo como imagem original:

```html
<div class="js-home-module" data-image="original">
```

A correção consistiu em:

- remover a proporção fixa;
- expandir a coluna para 100% quando ela for o único elemento da linha;
- preservar a proporção natural com `height: auto`;
- usar `object-fit: contain`;
- centralizar a imagem;
- manter largura responsiva.

Regra central:

```css
.section-modules-home .js-textbanner-image {
  width: 100% !important;
  max-width: 100% !important;
  height: auto !important;
  object-fit: contain !important;
  margin: 0 auto !important;
}
```

Com isso, imagens de outras proporções podem ser enviadas sem corte. Elas são reduzidas proporcionalmente para caber na largura disponível.

## 9. Páginas tratadas

### Produtos

- Cabeçalho escuro e translúcido.
- Barra de filtros escura.
- Área dos produtos em painel claro.
- Remoção das bordas externas que criavam linhas desalinhadas.

### Contato

- Cabeçalho escuro.
- Formulário escuro com campos claros.
- Espaço vertical entre cabeçalho e formulário.
- Largura e padding adaptados ao mobile.
- Verificação específica em `320 px` por causa do reCAPTCHA.

### Produto individual

- Conteúdo principal em painel escuro.
- Galeria em fundo claro.
- Quantidade e cálculo de frete em caixas claras.
- Botão de compra em gradiente dourado.

### Carrinho

- Cabeçalho e breadcrumb em painel escuro.
- Formulário e conteúdo do carrinho em painel claro.
- Mensagens e alertas com contraste adequado.
- Tratamento responsivo específico para manter margens laterais no celular.

## 10. Estratégia responsiva

O breakpoint principal usado pelo tema é `767 px`:

```css
@media (max-width: 767px) {
  /* ajustes para celular */
}
```

No mobile, a maioria dos painéis utiliza:

```css
width: calc(100% - 24px) !important;
margin-left: auto !important;
margin-right: auto !important;
```

Isso cria uma margem visual de `12 px` em cada lateral e evita que os painéis encostem na borda da tela.

O menu em cápsula é restrito ao desktop:

```css
@media (min-width: 768px) {
  /* navegação desktop */
}
```

## 11. Publicação e cache

Fluxo recomendado para cada atualização:

1. Editar `natural-mystic-theme.css`.
2. Testar localmente em desktop e mobile.
3. Substituir o arquivo em `vectorequilibrium.com/namy/`.
4. Incrementar a versão no editor da Nuvemshop.
5. Salvar o editor.
6. Abrir a loja em janela anônima ou sessão limpa.
7. Conferir home, produtos, contato, produto e carrinho.

Exemplo de atualização:

```css
/* antes */
@import url("https://vectorequilibrium.com/namy/natural-mystic-theme.css?v=4");

/* depois */
@import url("https://vectorequilibrium.com/namy/natural-mystic-theme.css?v=5");
```

Alterar apenas o arquivo no servidor sem mudar a versão pode fazer o navegador continuar exibindo a cópia antiga.

## 12. Checklist para futuras alterações

Antes de publicar:

- [ ] A imagem de fundo continua alinhada no desktop?
- [ ] O sol e os elementos laterais continuam visíveis no celular?
- [ ] Textos sobre a arte têm contraste suficiente?
- [ ] Nenhum painel voltou a apresentar bordas que se encontram de forma irregular?
- [ ] Imagens mantêm a proporção e não são cortadas?
- [ ] Não existe rolagem horizontal?
- [ ] O formulário de contato cabe em `320 px`?
- [ ] Os ícones do cabeçalho continuam visíveis sobre áreas claras e escuras?
- [ ] Produtos e carrinho continuam legíveis com conteúdo real?
- [ ] O número da versão do `@import` foi incrementado?

## 13. Como adaptar uma nova página

Quando uma nova página apresentar problemas:

1. Abrir a página publicada em desktop e mobile.
2. Identificar o contêiner principal e os atributos `data-store`.
3. Verificar cores computadas, dimensões e overflow.
4. Aplicar o padrão escuro para títulos e navegação sobre a arte.
5. Aplicar o padrão claro em áreas densas de conteúdo ou formulários.
6. Evitar bordas externas em blocos grandes.
7. Criar uma regra mobile apenas quando o comportamento realmente divergir.
8. Testar o CSS antes de publicar.

## 14. Regra de manutenção

Antes de acrescentar uma nova regra, deve-se confirmar se o problema pertence:

- ao CSS personalizado;
- ao CSS nativo da Nuvemshop;
- à configuração do módulo;
- à proporção do arquivo enviado;
- ao carregamento tardio da imagem;
- ou ao cache do navegador.

Essa separação foi a parte mais importante da metodologia: medir o comportamento real antes de tentar corrigi-lo.
