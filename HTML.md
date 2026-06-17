# Documentação: HTML (Estrutura)

Este documento descreve a **estrutura HTML** do Reflexo — o "esqueleto" da página. Aqui não há lógica nem estilo, apenas a organização dos elementos na tela.

> **Nota:** O Reflexo é um projeto de arquivo único. Todo o HTML descrito aqui vive dentro de `index.html`, junto com o CSS (no `<head>`) e o JavaScript (antes do `</body>`). Esta documentação separa as camadas apenas para fins didáticos.

---

## Visão Geral

A página é composta por **dois "telões"** que nunca aparecem ao mesmo tempo:

1. **Tela do jogo** (`#game-screen`) — onde o usuário resolve o puzzle
2. **Tela de conclusão** (`#completed-screen`) — aparece depois que o puzzle é resolvido

O JavaScript alterna entre as duas mostrando uma e escondendo a outra.

---

## O `<head>`

Contém as configurações da página, que o usuário não vê diretamente:

- `<meta charset="UTF-8">` — permite acentos e caracteres especiais (ç, ã, etc.)
- `<meta name="viewport">` — faz a página se adaptar à tela do celular
- `<title>` — o nome que aparece na aba do navegador
- `<meta name="description">` — texto usado pelo Google e ao compartilhar o link
- `<meta name="theme-color">` — cor da barra do navegador em celulares
- `<link rel="icon">` — o ícone (favicon) da aba, desenhado como um SVG embutido
- `<style>` — todo o CSS do jogo (documentado separadamente em `CSS.md`)

---

## A Tela do Jogo (`#game-screen`)

### Cabeçalho (`.header-row`)
A faixa do topo do card, com:
- `.avatar` — o círculo azul com o ícone de setas (símbolo do jogo)
- `.game-name` — o texto "Reflexo"
- `#day-indicator` — mostra o dia ("Reflexo de Quarta")
- `#timer` — o cronômetro
- `#rules-btn` — o botão "i" que abre as regras

### Painel de Regras (`#rules-panel`)
O guia "Como Jogar" completo. Começa escondido (`display:none`) e aparece quando o usuário clica no botão "i". Contém três seções: O Tabuleiro, As Regras e Como Ganhar, todas com os ícones reais do jogo embutidos como SVG.

### Legenda (`.legend`)
A linha que explica o que cada ícone significa: Sol, Lua, Estrela e Reflexo quebrado.

### Tabuleiro (`#board`)
Uma `<div>` vazia no HTML. As 36 células são criadas dinamicamente pelo JavaScript (função `render()`), não escritas à mão. Por isso aqui ela aparece vazia.

### Controles (`.controls`)
Os três botões de ação:
- `#hint-btn` — pedir uma dica (contém o texto `#hint-text` que vira contagem regressiva)
- `#verify-btn` — verificar a resposta
- `#reset-btn` — limpar o tabuleiro

### Status (`#status`)
Uma linha de texto abaixo dos botões onde aparecem as mensagens do jogo ("ainda faltam células vazias", "2 linhas com erro", etc.).

---

## A Tela de Conclusão (`#completed-screen`)

Começa escondida e aparece quando o puzzle é resolvido. Contém:

- Um título "Puzzle completo!"
- `#completion-time` — o tempo que o usuário levou
- `#streak-number` — o número grande de dias consecutivos (a sequência/streak)
- `#countdown` — a contagem regressiva até o próximo puzzle do dia seguinte

---

## Acessibilidade

A estrutura HTML inclui recursos para leitores de tela e navegação por teclado:

- `<h1 class="sr-only">` — um título invisível na tela mas lido por leitores de tela
- `aria-label` — descrições em botões e células (ex: "linha 1, coluna 1: vazia")
- `role="button"` — informa que as células do tabuleiro são clicáveis
- `tabIndex` — permite navegar pelas células usando a tecla Tab

---

## Por Que Tudo em Um Arquivo?

O HTML, CSS e JavaScript ficam juntos no `index.html` de propósito:

- **Zero dependências** — não precisa carregar nada de fora
- **Funciona offline** — basta abrir o arquivo
- **Deploy simples** — um arquivo só para hospedar
- **Carregamento rápido** — nada para baixar separadamente

Para projetos maiores, separar em arquivos diferentes faz sentido. Para um jogo enxuto como o Reflexo, manter tudo junto é uma vantagem.
