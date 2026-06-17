# Changelog

Todas as mudanças notáveis do Reflexo são documentadas neste arquivo.

O formato é baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/),
e este projeto segue o [Versionamento Semântico](https://semver.org/lang/pt-BR/).

## [1.2.0] - 2026-06-16

### Adicionado
- **Gerador automático de puzzles** — Cada dia gera um tabuleiro novo e único, derivado da data. Os puzzles nunca se repetem e nunca acabam
- **Curva de dificuldade semanal** — A dificuldade sobe suavemente de segunda (mais fácil) a domingo (mais difícil), ajustando o número de pares "raio" e de células reveladas
- **Garantia de solução única** — Todo tabuleiro gerado tem exatamente uma solução possível

### Alterado
- **Nova regra de símbolos** — Antes cada linha precisava de 2 sóis, 2 luas e 2 estrelas no total. Agora cada lado da linha do espelho (3 quadrados) deve ter exatamente 1 de cada símbolo. Isso evita situações como dois sóis do mesmo lado e torna o reflexo mais elegante
- O indicador do topo agora mostra o dia da semana ("Reflexo de Quarta") em vez de um número
- O texto do guia "Como Jogar" foi atualizado para refletir a nova regra

### Removido
- Banco fixo de 3 puzzles (substituído pelo gerador)

## [1.1.0] - 2026-06-16

### Adicionado
- **Sistema de dicas** — Botão "Dica" que revela uma célula vazia aleatória, com cooldown de 15 segundos e contagem regressiva no próprio botão
- **Cronômetro persistente** — O tempo é calculado a partir de um horário de início salvo, então sobrevive a refreshes da página
- **Destaque de erros** — Ao verificar, as linhas que violam as regras são destacadas com fundo vermelho suave, e a mensagem informa quantas linhas têm erro
- **Guia "Como Jogar" completo** — O painel de regras (botão "i") agora traz uma explicação detalhada com seções (O Tabuleiro, As Regras, Como Ganhar) e os ícones reais do jogo

### Alterado
- O botão "Reiniciar" agora limpa o tabuleiro mas mantém o cronômetro rodando (antes zerava o tempo)
- A linha do espelho central foi reimplementada como elemento sobreposto, evitando qualquer alteração no tamanho das células
- O indicador do dia não mostra mais o total de puzzles (antes: "nº 2 de 3", agora: "nº 2")

### Corrigido
- Células com erro não aumentam mais de tamamho nem se sobrepõem (causado por uma margem residual de uma versão anterior)
- Ícones que antes dependiam de uma fonte externa foram substituídos por SVGs inline, eliminando o problema de ícones quebrados

## [1.0.0] - 2026-06-16

### Adicionado
- Lançamento inicial do Reflexo
- Quebra-cabeça diário de simetria com tabuleiro 6×6
- Regras de simetria espelhada com exceções (pares com raio)
- Restrição de 2 sóis, 2 luas e 2 estrelas por linha
- Rotação diária de puzzles baseada no horário de Brasília
- Rastreamento de sequência de dias consecutivos (streak)
- Tela de conclusão com tempo e contagem regressiva para o próximo puzzle
- Progresso salvo localmente no navegador
- Design responsivo e sem dependências externas

[1.2.0]: https://github.com/raphaelcsmoraes/reflexo/releases/tag/v1.2.0
[1.1.0]: https://github.com/raphaelcsmoraes/reflexo/releases/tag/v1.1.0
[1.0.0]: https://github.com/raphaelcsmoraes/reflexo/releases/tag/v1.0.0
