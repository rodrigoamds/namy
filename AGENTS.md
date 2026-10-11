# AGENTS.md — tema Natural Mystic

Este diretório contém a personalização CSS da loja Natural Mystic na Nuvemshop. Estas instruções se aplicam a qualquer trabalho relacionado ao tema, às páginas da loja ou aos arquivos `natural-mystic-theme*.css` neste diretório.

## Comece por aqui

Antes de alterar o tema, leia integralmente:

1. `METODOLOGIA-TEMA-NATURAL-MYSTIC.md`
2. `natural-mystic-theme.css`

O documento de metodologia explica as decisões visuais, limitações da plataforma, seletores, testes responsivos e fluxo de publicação. O CSS normal é a fonte de verdade da implementação.

## Arquivos principais

- `natural-mystic-theme.css`: fonte principal, legível e destinada à manutenção e publicação.
- `natural-mystic-theme.min.css`: versão minificada, gerada a partir do arquivo principal.
- `METODOLOGIA-TEMA-NATURAL-MYSTIC.md`: documentação do processo e das decisões.
- `output/playwright/`: capturas usadas para comparação e validação visual.

Não trate arquivos `compact` ou `limit`, se existirem, como fonte atual do tema. Eles pertencem a tentativas anteriores de contornar o limite do editor antes da adoção do CSS externo.

## Loja e páginas de teste

- Home: <https://naturalmysticlab.lojavirtualnuvem.com.br/>
- Produtos: <https://naturalmysticlab.lojavirtualnuvem.com.br/produtos/>
- Contato: <https://naturalmysticlab.lojavirtualnuvem.com.br/contato/>
- Produto de teste: <https://naturalmysticlab.lojavirtualnuvem.com.br/produtos/produto-teste-123/>
- Carrinho: <https://naturalmysticlab.lojavirtualnuvem.com.br/comprar/>

## Hospedagem e importação

O arquivo público fica em:

<https://vectorequilibrium.com/namy/natural-mystic-theme.css>

O editor de CSS avançado da Nuvemshop deve conter somente uma importação versionada:

```css
@import url("https://vectorequilibrium.com/namy/natural-mystic-theme.css?v=N");
```

O número `N` é incrementado depois de cada publicação para invalidar o cache.

Não publique, substitua o arquivo remoto ou altere o editor da Nuvemshop sem autorização explícita do usuário. Preparar e testar uma versão local não equivale a publicá-la.

## Imagens de fundo

- Desktop: <https://d1a9qnv764bsoo.cloudfront.net/stores/007/966/713/rte/namyfd02.png>
- Mobile: <https://d1a9qnv764bsoo.cloudfront.net/stores/007/966/713/rte/namyfd01.png>

Preserve a separação entre as duas imagens e o breakpoint atual. A composição desktop é horizontal; a mobile é vertical.

## Regras de manutenção

### Preserve alterações externas

O proprietário pode editar o CSS hospedado entre sessões. Antes de começar uma nova rodada:

1. Baixe o CSS público atual.
2. Compare-o com `natural-mystic-theme.css`.
3. Preserve mudanças feitas pelo usuário.
4. Aplique a nova alteração sobre a versão mais recente.

Nunca sobrescreva silenciosamente uma edição remota mais nova com uma cópia local antiga.

### Edite somente a fonte principal

Faça alterações manuais em `natural-mystic-theme.css`. Depois, regenere `natural-mystic-theme.min.css` mecanicamente a partir dele. Não mantenha as duas versões à mão separadamente.

### Use seletores específicos

Prefira, nesta ordem:

1. IDs próprios da página, como `#shoppingCartPage`.
2. Atributos semânticos, como `[data-store="cart-form"]`.
3. Classes estáveis do tema, como `.js-textbanner-image`.
4. Seletores estruturais somente quando necessários.

Evite regras genéricas que possam afetar modais, checkout, plugins ou componentes futuros da Nuvemshop.

### Mantenha o sistema visual

- Fundo ilustrado é o elemento visual principal.
- Painéis escuros translúcidos são usados para títulos e navegação sobre a arte.
- Painéis claros são usados em produtos, formulários e áreas densas de conteúdo.
- Dourado é acento para botões, foco, hover, estado ativo e indicadores.
- Evite bordas douradas em painéis grandes; elas já causaram linhas desalinhadas entre blocos.
- Use sombra e transparência para separar planos.
- Preserve as variáveis `--nm-*` como fonte central de cores e sombras.

### Imagens dos módulos

As imagens de módulos devem:

- preservar a proporção original;
- usar `height: auto`;
- usar `object-fit: contain`;
- ficar centralizadas;
- caber na largura disponível;
- nunca voltar a uma proporção global fixa como `5 / 2`.

Se um módulo tiver apenas imagem, a coluna de imagem pode ocupar 100% da linha. Se também houver texto, não force a coluna a 100%; o seletor atual usa `:only-child` para distinguir os casos.

## Validação obrigatória

Antes de entregar uma mudança, teste pelo menos:

- Desktop: `1440 × 900`.
- Mobile: `390 × 844`.

Quando a alteração envolver formulário, reCAPTCHA, menu ou componentes apertados, teste também largura de `320 px`.

Valide:

- ausência de rolagem horizontal;
- contraste de texto e ícones;
- imagem inteira, sem corte inesperado;
- margens laterais no mobile;
- cabeçalho e rodapé;
- banner de cookies sobre conteúdo fixo;
- estado vazio e, quando possível, estado preenchido do componente alterado.

## Teste com navegador

Use uma sessão limpa para evitar CSS em cache. Para uma inspeção comum, aplique o CSS local temporariamente sobre a página.

Para componentes com lazy loading, `srcset` ou cálculo de tamanho durante o carregamento, substitua a resposta de `natural-mystic-theme.css` pela versão local antes de navegar. Injetar o CSS somente depois da página carregar pode produzir medidas incorretas.

Salve capturas relevantes em `output/playwright/` com nomes descritivos contendo página, viewport e estado, por exemplo:

```text
preview-cart-mobile.png
preview-home-desktop.png
preview-contact-320.png
```

## Fluxo recomendado por alteração

1. Ler este arquivo e a metodologia.
2. Verificar se o CSS remoto mudou.
3. Reproduzir o problema na página publicada.
4. Inspecionar o HTML e os estilos computados.
5. Identificar a causa antes de escrever CSS.
6. Alterar `natural-mystic-theme.css`.
7. Regenerar `natural-mystic-theme.min.css`.
8. Testar desktop e mobile com o CSS local.
9. Registrar uma captura quando o resultado for visual.
10. Informar claramente se a mudança está apenas local ou já foi publicada.
11. Se o usuário autorizar a publicação, substituir o arquivo remoto e incrementar `?v=N`.
12. Reabrir as páginas em sessão limpa e verificar o resultado público.

## Critério de conclusão

Uma alteração não está concluída apenas porque o CSS foi escrito. Ela está pronta para entrega quando:

- a causa do problema foi identificada;
- o resultado foi verificado nas viewports relevantes;
- não houve regressão óbvia em outras páginas;
- os arquivos normal e minificado estão sincronizados;
- o estado de publicação foi comunicado ao usuário;
- e a documentação foi atualizada quando a mudança introduziu um novo padrão ou decisão duradoura.
