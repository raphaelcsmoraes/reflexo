# Documentação: CSS (Estilo)

Este documento descreve o **CSS** do Reflexo — tudo que define a aparência: cores, tamanhos, espaçamentos e layout. O CSS vive dentro do bloco `<style>` no `<head>` do `index.html`.

---

## Variáveis de Cor (`:root`)

No topo do CSS há um bloco `:root` que define todas as cores e medidas do jogo em um só lugar. Isso se chama "CSS custom properties" (variáveis CSS). A vantagem: para mudar uma cor em todo o jogo, basta alterar em um único ponto.

```css
:root {
  --bg: #F6F3EE;            /* Fundo da página (bege claro) */
  --card-bg: #FFFFFF;       /* Fundo do card (branco) */
  --border: #E4DFD6;        /* Bordas suaves */
  --accent: #1B5FAD;        /* Cor principal (azul) */
  --sun: #B5740E;           /* Cor do sol (âmbar) */
  --moon: #1B5FAD;          /* Cor da lua (azul) */
  --star: #231F1A;          /* Cor da estrela (quase preto) */
  --danger: #B23A33;        /* Cor de erro (vermelho) */
  --success: #3C7A4B;       /* Cor de sucesso (verde) */
  --radius-md: 8px;         /* Cantos arredondados médios */
  --radius-lg: 14px;        /* Cantos arredondados grandes */
}
```

Para usar uma variável em qualquer lugar do CSS: `color: var(--accent);`

### Como Mudar o Visual do Jogo
Quer um tema diferente? Altere os valores acima. Por exemplo, trocar `--accent` muda a cor do título, dos botões focados e da linha do espelho de uma vez só.

---

## O Card Principal (`.card`)

O retângulo branco que contém todo o jogo. Pontos importantes:

- `max-width: 380px` — limita a largura, então em telas grandes o jogo não estica demais
- `border-radius` — cantos arredondados
- `padding` — espaço interno entre a borda e o conteúdo

O card é centralizado na tela pelo `body`, que usa `display: flex` com `justify-content: center`.

---

## O Tabuleiro (`#board`)

O coração do layout. Usa **CSS Grid** para criar a grade 6×6:

```css
#board {
  display: grid;
  grid-template-columns: repeat(6, 1fr);  /* 6 colunas iguais */
  gap: 4px;                                /* espaço entre células */
  max-width: 280px;
}
```

- `repeat(6, 1fr)` — cria 6 colunas de largura igual (`1fr` = uma fração do espaço)
- `gap` — o espacinho entre as células

### As Células (`.mirror-cell`)

Cada quadradinho do tabuleiro:

- `aspect-ratio: 1/1` — força a célula a ser sempre quadrada (largura = altura)
- `display: flex` centralizado — mantém o ícone no meio da célula
- `cursor: pointer` — mostra a "mãozinha" ao passar o mouse

### Estados Especiais das Células

| Classe | O que faz |
|--------|-----------|
| `.fixed` | Célula já preenchida (pista). Fundo cinza, não clicável |
| `.error-row` | Célula numa linha com erro. Fundo vermelho suave |
| `.mirror-edge` | Célula na borda do espelho. Desenha a linha tracejada azul |

> **Detalhe técnico importante:** A linha do espelho (`.mirror-edge::before`) e o destaque de erro (`.error-row`) foram feitos para **não alterar o tamanho da célula**. A linha do espelho é um elemento sobreposto (`position: absolute`) e o erro é só cor de fundo. Isso evita que células mudem de tamanho e se desalinhem — um problema comum quando se usa `border` para esses destaques.

---

## Os Botões (`button`)

Estilo padrão para todos os botões (Dica, Verificar, Reiniciar):

- Borda fina, fundo branco, cantos arredondados
- `:hover` — fica levemente cinza ao passar o mouse
- `:active` — encolhe um pouquinho ao clicar (`transform: scale(0.98)`)
- `:disabled` — fica semi-transparente e sem cursor (usado no botão de dica durante o cooldown)

---

## O Painel de Regras (`.rules-panel`)

O guia "Como Jogar". Características:

- `display: none` por padrão — fica escondido até clicar no botão "i"
- Tem estilos próprios para títulos (`h3`), listas (`ul`, `ol`) e os ícones inline (`.rule-icon`)
- Fundo levemente diferente do card para se destacar

---

## A Tela de Conclusão (`.completed-screen`)

- `display: none` por padrão — só aparece ao resolver o puzzle
- `.big-number` — o número gigante da sequência de dias (56px, em azul)
- `.countdown-time` — a contagem regressiva em fonte monoespaçada (para os números não "dançarem")

---

## Responsividade

O jogo se adapta a diferentes telas sem precisar de muitas "media queries", graças a:

- `max-width` no card e no tabuleiro (não estica demais em telas grandes)
- `aspect-ratio` nas células (sempre quadradas, em qualquer tamanho)
- `1fr` nas colunas do grid (dividem o espaço disponível igualmente)
- `padding` no `body` (garante respiro nas bordas em celulares)

---

## Acessibilidade Visual

- `:focus` nas células e botões — mostra um contorno azul ao navegar por teclado
- `.sr-only` — esconde visualmente um elemento mas o mantém para leitores de tela (técnica padrão de acessibilidade)
