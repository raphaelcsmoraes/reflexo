# Documentação: JavaScript (Lógica)

Este documento descreve o **JavaScript** do Reflexo — toda a lógica que faz o jogo funcionar. O código vive dentro do bloco `<script>` no final do `index.html`, envolvido por uma função que executa sozinha `(function(){ ... })()` para não interferir com nada fora dele.

As funções estão agrupadas por responsabilidade abaixo.

---

## 1. Fuso Horário (Brasília)

O puzzle muda à meia-noite **no horário de Brasília**, independente de onde o jogador esteja. Essas funções cuidam disso:

| Função | O que faz |
|--------|-----------|
| `getBrasiliaNow()` | Retorna a data/hora atual convertida para o fuso de Brasília (UTC-3) |
| `getTodayKeyBrasilia()` | Retorna a data de hoje como texto, ex: `"2026-06-16"`. Usada como "chave do dia" |
| `getSecondsUntilMidnightBrasilia()` | Quantos segundos faltam até a próxima meia-noite (para a contagem regressiva) |
| `formatTimeRemaining(seconds)` | Formata segundos em `HH:MM:SS` |

---

## 2. Dados do Jogador (Persistência)

O progresso é salvo no navegador via `localStorage` — nada vai para servidores.

| Função | O que faz |
|--------|-----------|
| `loadPlayerData()` | Lê os dados salvos do jogador (sequência e último dia jogado) |
| `savePlayerData(data)` | Salva os dados do jogador |
| `updateStreak()` | Atualiza a sequência de dias: +1 se jogou ontem, volta a 1 se pulou um dia |

### O que é salvo no localStorage

```javascript
// Dados do jogador
'reflexo_player' → { lastPlayedKey: "2026-06-16", streak: 5 }

// Estado do jogo de cada dia
'reflexo_game_2026-06-16' → { completed: true, time: 342 }

// Horário de início do cronômetro de cada dia
'reflexo_timer_start_2026-06-16' → 1718560000000
```

---

## 3. Geração de Puzzles

Esta é a parte mais sofisticada. Em vez de ter puzzles fixos, o jogo **gera um tabuleiro novo a cada dia**.

| Função | O que faz |
|--------|-----------|
| `makeRng(seed)` | Cria um gerador de números aleatórios "com semente" — a mesma semente sempre gera a mesma sequência |
| `shuffleArr(arr, rng)` | Embaralha uma lista usando o gerador acima |
| `enumerateValidRows(cracks)` | Lista todas as linhas válidas possíveis para uma dada configuração |
| `countRowSolutions(cracks, given)` | Conta quantas soluções uma linha tem (usado para garantir solução única) |
| `generatePuzzle(seed, config)` | Monta o tabuleiro completo do dia |

### Como funciona a geração

1. A data de hoje vira uma "semente" numérica
2. Como a semente é sempre a mesma para um dado dia, **todos os jogadores veem o mesmo puzzle** e ele não muda se a página for atualizada
3. O gerador monta um tabuleiro válido respeitando todas as regras
4. Em seguida, revela algumas células como pistas, garantindo que exista **apenas uma solução possível**

### As Regras Embutidas

Cada linha do tabuleiro deve satisfazer:
- **Pares espelhados iguais** (regra normal) ou **diferentes** (pares "raio")
- **Cada lado da linha do espelho (3 quadrados) tem exatamente 1 sol, 1 lua e 1 estrela**

> **Curiosidade matemática:** uma linha só pode ter 0, 2 ou 3 pares "raio". Ter exatamente 1 par-raio torna impossível satisfazer a regra de "1 de cada por lado". Por isso o gerador usa sempre 2 cracks em cada linha de raio.

### Dificuldade por Dia da Semana

```javascript
var difficultyByWeekday = {
  1: {crackRows:1, extra:3}, // Segunda - mais fácil
  2: {crackRows:1, extra:2}, // Terça
  3: {crackRows:2, extra:2}, // Quarta
  4: {crackRows:2, extra:1}, // Quinta
  5: {crackRows:3, extra:1}, // Sexta
  6: {crackRows:3, extra:0}, // Sábado
  0: {crackRows:4, extra:0}  // Domingo - mais difícil
};
```

- `crackRows` — quantas linhas têm pares "raio" (mais = mais difícil)
- `extra` — células reveladas além do mínimo (mais = mais fácil)

---

## 4. O Cronômetro

| Função | O que faz |
|--------|-----------|
| `getTimerStartKey()` | A chave do localStorage do cronômetro de hoje |
| `getTimerStart()` | Lê (ou cria) o horário em que o jogador começou hoje |
| `formatTime(s)` | Formata segundos em `MM:SS` |
| `updateTimerDisplay()` | Atualiza o número na tela |
| `startTimer()` | Inicia a contagem |
| `stopTimer()` | Para a contagem |

### Por que o tempo sobrevive a refreshes

O cronômetro não conta "tique a tique". Ele guarda o **horário de início** e calcula a diferença para agora. Assim:
- **Atualizar a página** → recupera o horário salvo e continua de onde estava
- **Clicar em Reiniciar** → limpa o tabuleiro mas o tempo segue (não chama `startTimer` de novo)
- **Próximo dia** → como a chave inclui a data, um novo cronômetro começa automaticamente

---

## 5. Renderização e Interação

| Função | O que faz |
|--------|-----------|
| `iconSvg(name, size, color)` | Gera o código SVG de um ícone (sol, lua, estrela, raio, relógio) |
| `render()` | Desenha as 36 células do tabuleiro na tela |
| `handleClick(r, c)` | Trata o clique numa célula: alterna vazio → sol → lua → estrela → vazio |

### Como o tabuleiro é desenhado

A `<div id="board">` começa vazia no HTML. A função `render()` cria as 36 células via JavaScript, define o ícone de cada uma, marca quais têm "raio", aplica o destaque de erro quando necessário, e conecta o clique. É chamada toda vez que algo muda no tabuleiro.

---

## 6. Verificação e Conclusão

| Função | O que faz |
|--------|-----------|
| `verify()` | Confere se o tabuleiro está correto e mostra erros, se houver |
| `resetBoard()` | Limpa as células preenchidas pelo jogador (mantém as pistas e o tempo) |
| `showCompletedScreen(data)` | Mostra a tela de conclusão com tempo, sequência e contagem regressiva |
| `checkAndShowCompletedIfNeeded()` | Ao abrir, verifica se o jogador já completou hoje e mostra a tela certa |

### Como a verificação funciona

A função `verify()` checa cada linha separadamente:
1. As células espelhadas seguem a regra (iguais, ou diferentes nos pares "raio")?
2. Cada lado da linha tem exatamente 1 sol, 1 lua e 1 estrela?

Se uma linha viola alguma regra, ela é destacada com fundo vermelho e a mensagem informa quantas linhas estão erradas. Se tudo está certo, o cronômetro para, a sequência é atualizada e a tela de conclusão aparece.

---

## 7. Sistema de Dicas

| Função | O que faz |
|--------|-----------|
| `getRandomEmptyCell()` | Escolhe uma célula vazia aleatória |
| `useHint()` | Revela essa célula com o símbolo correto e inicia o cooldown |
| `updateHintButton()` | Controla o botão: durante os 15s mostra a contagem e fica desabilitado |

### Como funciona o cooldown

Ao usar uma dica, o jogo guarda o horário em que ela poderá ser usada de novo (agora + 15 segundos). O botão mostra a contagem regressiva e fica desabilitado até o tempo passar, quando volta a funcionar automaticamente.

---

## 8. Carregamento Inicial

A função `loadPuzzle()` junta tudo: descobre o dia, escolhe a dificuldade, gera o tabuleiro, prepara as células e inicia o cronômetro.

No final do script, o jogo decide o que mostrar ao abrir:
- Se o jogador **já completou hoje** → mostra a tela de conclusão
- Senão → carrega o puzzle do dia

```javascript
if(!checkAndShowCompletedIfNeeded()){
  loadPuzzle();
}
```

---

## Resumo do Fluxo

```
Abrir página
   ↓
Já jogou hoje? ──Sim──→ Tela de conclusão (tempo + streak + contagem)
   ↓ Não
Gera puzzle do dia (com dificuldade do dia da semana)
   ↓
Jogador preenche células (clicando)
   ↓
(Opcional) Pede dicas (com cooldown de 15s)
   ↓
Clica em Verificar
   ↓
Erros? ──Sim──→ Destaca linhas e mostra mensagem
   ↓ Não
Para o cronômetro, atualiza streak, salva progresso
   ↓
Tela de conclusão
```
