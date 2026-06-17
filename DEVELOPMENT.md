# Guia de Desenvolvimento do Reflexo

Este documento fornece informações técnicas para desenvolvedores trabalhando no Reflexo.

## 🏗️ Arquitetura do Projeto

### Estrutura de Arquivo Único

O Reflexo é uma **aplicação HTML/CSS/JavaScript standalone** em um único arquivo (`index.html`). Isso oferece:

✅ **Vantagens:**
- Zero dependências
- Fácil deploy
- Funciona offline
- Rápido carregamento
- Sem build process

⚠️ **Limitações:**
- Arquivo deve ser mantido < 50KB
- Lógica mais complexa em um só lugar
- Sem modularização

---

## 📚 Estrutura do Código

### HTML
```
<body>
  <main class="card">
    <div id="game-screen">        <!-- Tela do jogo -->
    <div id="completed-screen">   <!-- Tela de conclusão -->
  </main>
</body>
```

### CSS
Usa **CSS custom properties** (variáveis) para fácil customização:

```css
:root {
  --bg: #F6F3EE;           /* Fundo da página */
  --accent: #1B5FAD;       /* Cor principal */
  --sun: #B5740E;          /* Cor do sol */
  --moon: #1B5FAD;         /* Cor da lua */
  --star: #231F1A;         /* Cor da estrela */
  --danger: #B23A33;       /* Cor de erro */
  --success: #3C7A4B;      /* Cor de sucesso */
}
```

### JavaScript

#### Organização Lógica

```javascript
// 1. Timezone helpers
function getBrasiliaNow() { ... }
function getTodayKeyBrasilia() { ... }

// 2. Player data management
function loadPlayerData() { ... }
function savePlayerData() { ... }
function updateStreak() { ... }

// 3. Game data
var puzzleBank = [ ... ]  // 3 puzzles padrão
var cracks = [ ... ]      // Posições com "raio"
var pairCols = [ ... ]    // Pares espelhados

// 4. UI elements
var boardEl, statusEl, gameScreen, completedScreen

// 5. Game state
var board, solution, errorCells
var timerInterval, elapsedSeconds

// 6. Game functions
function render() { ... }
function handleClick(r, c) { ... }
function verify() { ... }
function showCompletedScreen() { ... }
```

---

## 🔧 Como Modificar

### Adicionar um Novo Puzzle

1. Abra `index.html` em um editor
2. Procure por `var puzzleBank = [`
3. Adicione um novo array 6×6:

```javascript
var puzzleBank = [
  // Puzzle 1
  [
    ['sun','moon','star','star','moon','sun'],
    ['moon','star','sun','sun','star','moon'],
    // ... 4 linhas mais
  ],
  // Puzzle 2
  [ ... ],
  // Seu novo puzzle aqui:
  [
    ['moon','star','sun','sun','star','moon'],
    ['sun','moon','star','star','moon','sun'],
    // ... 4 linhas mais
  ],
];
```

### Mudar as Regras

#### 1. Modificar Pares "Crack" (Raio)

Procure por:
```javascript
var cracks = [[],[],[1,2],[0,2],[],[]];
```

Explique:
- Cada índice = uma linha (0-5)
- Valores = índices de pares que devem ser **diferentes**
- `cracks[2] = [1,2]` significa linha 2, pares 1 e 2 têm raio

#### 2. Modificar Contagem de Símbolos

Procure por na função `verify()`:
```javascript
if(filled && (counts.sun!==2 || counts.moon!==2 || counts.star!==2)){
  // Linha violou regra
}
```

Mude os números para diferentes totais.

### Mudar Cores

Abra o bloco `<style>` e modifique `:root`:

```css
:root {
  --sun: #FF8C00;      /* De laranja para laranja escuro */
  --moon: #4169E1;     /* De azul para azul real */
  --star: #FFD700;     /* De preto para ouro */
}
```

---

## 💾 LocalStorage Schema

### Dados do Jogador
```javascript
// Chave: 'reflexo_player'
{
  lastPlayedKey: "2026-06-16",  // Data do último dia jogado
  streak: 5                      // Sequência de dias consecutivos
}
```

### Estado do Jogo (por dia)
```javascript
// Chave: 'reflexo_game_2026-06-16'
{
  completed: true,   // Se completou hoje
  time: 342         // Tempo em segundos
}
```

### Horário de Início do Cronômetro (por dia)
```javascript
// Chave: 'reflexo_timer_start_2026-06-16'
1718560000000   // Timestamp em ms de quando o puzzle foi aberto
```

O cronômetro é calculado como a diferença entre `Date.now()` e este
timestamp. Por isso o tempo sobrevive a refreshes e o botão "Reiniciar"
não zera o relógio. A chave inclui a data, então um novo cronômetro é
criado automaticamente a cada dia.

### Como Acessar no Console

```javascript
// Ver dados do jogador
JSON.parse(localStorage.getItem('reflexo_player'))

// Ver estado do jogo de hoje
JSON.parse(localStorage.getItem('reflexo_game_' + getTodayKeyBrasilia()))

// Limpar tudo (CUIDADO!)
localStorage.clear()
```

---

## ⏰ Timezone Handling

### Função Principal
```javascript
function getBrasiliaNow() {
  var now = new Date();
  var utc = now.getTime() + (now.getTimezoneOffset() * 60000);
  return new Date(utc + (3600000 * -3));  // UTC-3 = Brasília
}
```

### Por Que Isso Importa
- O puzzle muda **exatamente às 00:00 de Brasília**
- Usuários em outros timezones veem o mesmo puzzle
- Funciona corretamente mesmo em `new Date().getTimezoneOffset()` diferente

### Testando Diferentes Timezones
Modifique temporariamente em `getBrasiliaNow()`:
```javascript
// Simular outro timezone
return new Date(utc + (3600000 * -5));  // Teste como se fosse EDT (UTC-5)
```

---

## 🎮 Fluxo do Jogo

### Inicialização
```
1. Carrega dados do jogador (lastPlayedKey, streak)
2. Verifica se já completou hoje
   - SIM → Mostra tela de "Completado" (completed-screen)
   - NÃO → Carrega puzzle novo (game-screen)
3. Inicia timer
```

### Jogando
```
1. Usuário clica em célula → handleClick(r, c)
2. Cicla entre: vazio → sol → lua → estrela → vazio
3. (Opcional) Clica "Dica" → useHint() revela uma célula
   - Inicia cooldown de 15s via updateHintButton()
4. Clica "Verificar" → verify()
5. Valida todas as regras
   - Erros → linhas destacadas com classe .error-row
   - Válido → showCompletedScreen()
```

### Sistema de Dicas
```
1. useHint() escolhe célula vazia aleatória (getRandomEmptyCell)
2. Preenche com o valor da solução
3. Define hintCooldownEnd = agora + 15000ms
4. updateHintButton() desabilita o botão e mostra a contagem
5. Quando o cooldown acaba, o botão reativa automaticamente
```

### Cronômetro
```
1. getTimerStart() lê (ou cria) o timestamp de início do dia
2. elapsedSeconds = (agora - início) / 1000
3. Atualiza a cada segundo via setInterval
4. Reiniciar NÃO chama startTimer → tempo continua
5. Refresh recupera o timestamp salvo → tempo continua
```

### Persistência
```
1. Ao completar → updateStreak()
2. Salva em localStorage:
   - reflexo_player (streak + lastPlayedKey)
   - reflexo_game_DATA (completed + time)
   - reflexo_timer_start_DATA (timestamp de início)
3. Na próxima visita → checkAndShowCompletedIfNeeded()
```

---

## 🧪 Testando

### Testar Nova Feature Localmente

```bash
# Inicie servidor local
python -m http.server 8000

# Abra
http://localhost:8000
```

### Testar em Diferentes Browsers

- Chrome/Chromium ✅
- Firefox ✅
- Safari ✅
- Edge ✅
- Mobile (iOS Safari, Chrome Android) ✅

### Testar Offline

1. Abra DevTools (F12)
2. Vá para "Application" → "Service Workers"
3. Ou simplesmente:
   ```bash
   # Baixe o arquivo
   # Abra localmente sem servidor
   open index.html
   ```

### Testar LocalStorage

```javascript
// No console do navegador:

// 1. Simular primeiro jogo
localStorage.setItem('reflexo_player', JSON.stringify({
  lastPlayedKey: null,
  streak: 0
}))
location.reload()

// 2. Simular completado hoje
localStorage.setItem('reflexo_game_2026-06-16', JSON.stringify({
  completed: true,
  time: 180
}))
location.reload()

// 3. Simular perdida de streak
localStorage.setItem('reflexo_player', JSON.stringify({
  lastPlayedKey: '2026-06-14',  // 2 dias atrás
  streak: 5
}))
location.reload()
```

---

## 📦 Performance

### Tamanho do Arquivo
- HTML + CSS + JS: ~15 KB (não minificado)
- Objetivo: Manter < 30 KB

### Tempos de Carregamento
- First Paint: < 500ms
- Interactive: < 1s
- Funciona bem em conexões 3G

### Optimizações Aplicadas
- Inline CSS (sem arquivo separado)
- Inline SVG icons (sem CDN)
- Sem imagens externas
- Zero dependências

---

## 🐛 Debug Comuns

### Problema: Puzzle não muda no próximo dia
**Causa:** Erro na função `getPuzzleIndex()`  
**Solução:** Verifique se `getTodayKeyBrasilia()` está retornando a data certa
```javascript
// No console
getTodayKeyBrasilia()  // Deve retornar "2026-06-16" (hoje)
```

### Problema: Streak não incrementa
**Causa:** `updateStreak()` não está sendo chamado  
**Solução:** Verifique se `verify()` chama `updateStreak()` antes de `showCompletedScreen()`

### Problema: Timer não para
**Causa:** `stopTimer()` não sendo chamado  
**Solução:** Verifique se `verify()` chama `stopTimer()` antes de completar

### Problema: Layout quebrado no mobile
**Causa:** Media queries não cobrem tamanho do dispositivo  
**Solução:** Adicione media query no CSS

---

## 🚀 Build & Deploy

### Seu Computador
```bash
# Não precisa de build
# Apenas abra index.html no navegador
```

### Netlify
```bash
# 1. Commit e push para GitHub
git add .
git commit -m "Update features"
git push origin main

# 2. Netlify detecta automaticamente
# 3. Deploy em alguns segundos

# 4. Verifique em
https://jogo-reflexo.netlify.app
```

### Qualquer Servidor HTTP
```bash
# Copie index.html para seu servidor
scp index.html user@seu-servidor.com:/var/www/html/

# Pronto! Acesse
https://seu-dominio.com/index.html
```

---

## 📝 Convenções de Código

### Variáveis
```javascript
// ✅ BOM
var boardEl = document.getElementById('board');
var elapsedSeconds = 0;
var fixedSet = new Set();

// ❌ RUIM
var b = document.getElementById('board');
var sec = 0;
var fixed = new Set();
```

### Funções
```javascript
// ✅ BOM
function updateTimerDisplay() { ... }
function showCompletedScreen(playerData) { ... }

// ❌ RUIM
function update() { ... }
function show(data) { ... }
```

### Comentários
```javascript
// ✅ BOM: Explica POR QUÊ
// Calcula dias desde epoch para derivar índice do puzzle
var daysSinceEpoch = Math.floor(brasilia.getTime() / 86400000);

// ❌ RUIM: Óbvio demais
var x = Math.floor(brasilia.getTime() / 86400000);
```

---

## 📚 Recursos Úteis

- [MDN Web Docs](https://developer.mozilla.org/)
- [JavaScript Info](https://javascript.info/)
- [Caniuse.com](https://caniuse.com/) - Compatibilidade de browsers
- [Web.dev](https://web.dev/) - Web performance

---

## 🤝 Antes de Submeter um PR

- [ ] Testar em Chrome, Firefox, Safari
- [ ] Testar em mobile
- [ ] Testar offline
- [ ] Verificar tamanho do arquivo
- [ ] Adicionar comentários se complexo
- [ ] Atualizar README se mudar features

---

**Última atualização:** Junho 2026  
**Versão:** 1.1.0
