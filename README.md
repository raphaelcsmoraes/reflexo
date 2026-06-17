# Documentação Técnica do Reflexo

Esta pasta contém a documentação do código, separada por tecnologia. Cada arquivo explica uma camada do jogo.

> **Lembre-se:** O Reflexo é um projeto de **arquivo único** — HTML, CSS e JavaScript vivem todos juntos dentro de `index.html`. Esta separação em documentos é apenas didática, para facilitar o estudo de cada parte.

## Os Três Documentos

### 📄 [HTML.md](HTML.md) — Estrutura
O "esqueleto" da página: quais elementos existem, como estão organizados, as duas telas (jogo e conclusão) e os recursos de acessibilidade.

### 🎨 [CSS.md](CSS.md) — Estilo
A aparência: as variáveis de cor, o layout do tabuleiro em grid, os estados das células (fixa, erro, espelho), os botões e a responsividade.

### ⚙️ [JAVASCRIPT.md](JAVASCRIPT.md) — Lógica
O cérebro do jogo: fuso horário, persistência, o gerador de puzzles, o cronômetro, a verificação, o sistema de dicas e o fluxo completo.

## Por Onde Começar

- Se você quer entender **como o jogo aparece** → comece por `HTML.md`, depois `CSS.md`
- Se você quer entender **como o jogo funciona** → vá direto para `JAVASCRIPT.md`
- Para uma visão geral do projeto → veja o [README principal](../README.md)

## Outros Documentos do Projeto

Na raiz do repositório você encontra também:
- `README.md` — visão geral, como jogar, instalação
- `DEVELOPMENT.md` — guia técnico geral e como modificar o jogo
- `ROADMAP.md` — planos futuros
- `CHANGELOG.md` — histórico de versões
- `CONTRIBUTING.md` — como contribuir
