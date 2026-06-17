# Reflexo — Quebra-Cabeça Diário de Simetria

![Reflexo](https://img.shields.io/badge/Status-Live-brightgreen) ![License](https://img.shields.io/badge/License-MIT-blue) ![Language](https://img.shields.io/badge/Language-JavaScript-yellow)

**Reflexo** é um jogo de puzzle diário minimalista que desafia sua capacidade de reconhecer padrões de simetria. A cada dia, um novo puzzle único o aguarda. Complete a sequência para construir sua sequência de dias consecutivos jogados.

🎮 **Jogue agora:** [jogo-reflexo.netlify.app](https://jogo-reflexo.netlify.app)

---

## 📖 Sobre o Jogo

Reflexo é inspirado em jogos diários populares como Wordle, mas com foco em simetria e padrões lógicos. O tabuleiro é dividido por uma linha espelho central: você precisa preencher as células vazias seguindo as regras de simetria.

### Características

- **Um puzzle por dia** — Um novo desafio diário à meia-noite (horário de Brasília)
- **Sistema de dicas** — Peça uma dica quando travar (com cooldown de 15 segundos)
- **Destaque de erros** — Linhas com erro são destacadas visualmente ao verificar
- **Rastreamento de sequência** — Mantenha sua sequência de dias consecutivos jogados
- **Cronômetro persistente** — O tempo continua mesmo se você atualizar a página
- **Tela de conclusão** — Veja seu tempo e sequência ao completar, com contagem regressiva para o próximo puzzle
- **Guia de regras integrado** — Painel "Como Jogar" completo dentro do jogo
- **Progresso persistente** — Seu progresso é salvo automaticamente no navegador
- **Sem login necessário** — Jogue anonimamente ou em múltiplos dispositivos
- **Design minimalista** — Interface limpa e focada no jogo
- **Sem publicidades** — Experiência pura de jogo

---

## 🎯 Como Jogar

### O Tabuleiro
- Tabuleiro **6×6** com uma linha espelho no centro (coluna 3)
- Algumas células já estão preenchidas (essas não podem ser alteradas)
- Você preenche as células vazias com os símbolos: ☀️ Sol, 🌙 Lua, ⭐ Estrela

### As Regras

1. **Os pares espelhados devem ter o mesmo símbolo**
   - A célula (linha 1, coluna 1) deve ter o mesmo símbolo que (linha 1, coluna 6)
   - A célula (linha 1, coluna 2) deve ter o mesmo símbolo que (linha 1, coluna 5)
   - E assim por diante...

2. **Exceção: Pares com raio (⚡) devem ter símbolos diferentes**
   - Essas células marcadas com um ícone de raio NÃO seguem a regra de simetria
   - Elas devem ter símbolos *diferentes* de seus pares

3. **Cada linha deve conter exatamente:**
   - 2 Sóis ☀️
   - 2 Luas 🌙
   - 2 Estrelas ⭐

### Como Ganhar

1. Preencha todas as células vazias
2. Clique em "Verificar"
3. Se todas as regras forem seguidas, o puzzle estará completo!

---

## 📊 Sistema de Progresso

### Streak (Sequência de Dias)
- Ganhe **+1 dia** cada vez que completa o puzzle
- Se você **pular um dia**, o contador volta a **0**
- Seu streak é armazenado localmente e sincronizado automaticamente

### Cronômetro Persistente
- O tempo começa a contar quando você abre o puzzle do dia
- Continua de onde parou se você **atualizar a página**
- O botão "Reiniciar" limpa o tabuleiro mas **não** zera o tempo
- Recomeça automaticamente do zero no dia seguinte

### Sistema de Dicas
- Clique em "Dica" para revelar uma célula vazia aleatória
- Após usar, há um **cooldown de 15 segundos** antes da próxima dica
- O botão mostra a contagem regressiva durante o cooldown

### Destaque de Erros
- Ao clicar em "Verificar", as linhas que violam as regras são destacadas
- A mensagem informa quantas linhas têm erro
- Ajuda a identificar onde corrigir sem entregar a resposta

### Visualização do Progresso
Quando você completa um puzzle, vê:
- ✓ Seu tempo de conclusão
- 🔥 Sua sequência de dias consecutivos
- ⏱️ Contador regressivo até o próximo puzzle (às 00:00 de Brasília)

---

## 🛠️ Instalação Local

### Requisitos
- Navegador moderno (Chrome, Firefox, Safari, Edge)
- Nenhuma dependência externa (o jogo é totalmente standalone)

### Como Rodar Localmente

1. **Clone o repositório:**
```bash
git clone https://github.com/seu-usuario/reflexo.git
cd reflexo
```

2. **Abra o arquivo no navegador:**
```bash
# Opção 1: Abra o arquivo diretamente
open index.html

# Opção 2: Use um servidor local (recomendado)
python -m http.server 8000
# Ou com Node.js:
npx http-server
```

3. **Acesse:** `http://localhost:8000`

---

## 📁 Estrutura do Projeto

```
reflexo/
├── index.html          # Arquivo único e standalone do jogo
├── README.md           # Este arquivo
├── LICENSE             # Licença MIT
└── docs/
    ├── REGRAS.md       # Explicação detalhada das regras
    ├── ROADMAP.md      # Planos futuros
    └── DESENVOLVIMENTO.md
```

### Arquitetura

O jogo é construído como um **HTML/CSS/JavaScript único e auto-contido**:

- **HTML**: Estrutura da interface com aria-labels para acessibilidade
- **CSS**: Design responsivo com variáveis de cor customizáveis
- **JavaScript**: Lógica do jogo, persistência com localStorage, cálculos de timezone

**Não há dependências externas** — O arquivo funciona offline e em qualquer navegador moderno.

---

## 💾 Armazenamento de Dados

Todos os dados são armazenados **localmente no navegador** usando `localStorage`:

```javascript
// Dados do jogador
localStorage.getItem('reflexo_player')
// {
//   lastPlayedKey: "2026-06-16",
//   streak: 5
// }

// Estado do jogo de hoje
localStorage.getItem('reflexo_game_2026-06-16')
// {
//   completed: true,
//   time: 342  // segundos
// }

// Horário de início do cronômetro (permite que o tempo
// sobreviva a refreshes da página)
localStorage.getItem('reflexo_timer_start_2026-06-16')
// 1718560000000  (timestamp em milissegundos)
```

**Privacidade:** Nenhum dado é enviado para servidores. Tudo fica no seu dispositivo.

---

## 🎨 Customização

### Mudar Cores

Abra `index.html` e procure por `:root { ... }` no bloco `<style>`:

```css
:root {
  --accent: #1B5FAD;        /* Cor principal */
  --sun: #B5740E;           /* Cor do sol */
  --moon: #1B5FAD;          /* Cor da lua */
  --star: #231F1A;          /* Cor da estrela */
  --danger: #B23A33;        /* Cor de erro */
  --success: #3C7A4B;       /* Cor de sucesso */
}
```

### Adicionar Mais Puzzles

Procure pela array `puzzleBank` no bloco `<script>` e adicione novos puzzles:

```javascript
var puzzleBank = [
  // Puzzle 1
  [['sun','moon','star','star','moon','sun'], ...],
  // Puzzle 2
  [['moon','star','sun','sun','star','moon'], ...],
  // Adicione novos aqui
];
```

**Cada puzzle deve ser uma matriz 6×6 com valores: `'sun'`, `'moon'` ou `'star'`**

---

## 📱 Responsividade

O jogo foi otimizado para:
- 📱 Celulares (width < 600px)
- 💻 Tablets (width 600px - 1024px)
- 🖥️ Desktops (width > 1024px)

O tabuleiro se redimensiona automaticamente mantendo proporções.

---

## ⏰ Timezone

O jogo usa **horário de Brasília (UTC-3)** como referência:

- Puzzle muda sempre às **00:00 de Brasília**
- Funciona corretamente em qualquer timezone
- O countdown mostra o tempo até meia-noite local de Brasília

---

## 🚀 Deploy

### Netlify (Recomendado)

1. Faça push do repositório para GitHub
2. Conecte o repositório ao Netlify
3. Configure: `build command: none` (é um arquivo estático)
4. Deploy automático a cada push

### Outras Plataformas

- **GitHub Pages:** Funciona perfeitamente
- **Vercel:** Suporta arquivos estáticos
- **Qualquer servidor HTTP:** Funciona em qualquer lugar

---

## 🗺️ Roadmap

### Próximos Jogos (Planejados)

O Reflexo é o primeiro de uma série de quebra-cabeças diários:

1. **Reflexo** ✅ (Lançado)
   - Simetria espelhada com exceções

2. **Equilíbrio** (Em desenvolvimento)
   - Quebra-cabeça de balanceamento lógico

3. **Mistura** (Planejado)
   - Padrão de cores e combinações

4. **Trilha de Energia** (Planejado)
   - Conectar circuitos com padrões

### Recursos Futuros

- [x] Sistema de dicas (com cooldown)
- [x] Cronômetro persistente
- [x] Destaque visual de erros
- [ ] Limite de dicas por dia
- [ ] Estatísticas detalhadas (tempos, taxas de vitória)
- [ ] Temas customizáveis
- [ ] Temas escuro/claro
- [ ] Compartilhamento de resultados
- [ ] Leaderboard (opcional)
- [ ] Suporte a mais idiomas

---

## 🤝 Contribuindo

Contribuições são bem-vindas! Siga estes passos:

1. **Fork** o repositório
2. **Crie uma branch** para sua feature (`git checkout -b feature/AmazingFeature`)
3. **Commit** suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. **Push** para a branch (`git push origin feature/AmazingFeature`)
5. **Abra um Pull Request**

### Áreas de Contribuição

- 🐛 Reportar bugs
- 💡 Sugerir novas features
- 🌍 Traduzir para outros idiomas
- 🎨 Melhorias de design/UX
- 📝 Melhorias na documentação

---

## 📄 Licença

Este projeto está licenciado sob a **Licença MIT** — veja o arquivo [LICENSE](LICENSE) para detalhes.

---

## 📞 Suporte

Encontrou um bug ou tem uma sugestão?

- 🐛 Abra uma **Issue** no GitHub
- 💬 Discuta no **Discussions** do repositório
- 📧 Entre em contato (se disponível)

---

## 🎓 Aprender com Este Projeto

Este é um excelente exemplo de:

- ✅ **Aplicação Web Standalone** — Sem frameworks, apenas HTML/CSS/JS puro
- ✅ **LocalStorage** — Persistência de dados no navegador
- ✅ **Timezone Handling** — Cálculos de data/hora em diferentes zonas
- ✅ **Game Logic** — Validação de regras e verificação de vitória
- ✅ **Responsive Design** — Layout adaptativo
- ✅ **Acessibilidade** — ARIA labels e navegação por teclado

Se você está aprendendo web development, sinta-se à vontade para explorar o código!

---

## 🙏 Agradecimentos

- Inspiração em jogos diários como Wordle, Semantle e Nerd Sniped
- Design influenciado por interfaces minimalistas
- Comunidade de jogadores por feedback e sugestões

---

## 📊 Estatísticas

- **Tamanho do arquivo:** ~15 KB (minificado)
- **Dependências externas:** 0
- **Requisitos de servidor:** Nenhum (funciona offline)
- **Compatibilidade:** Navegadores modernos (ES6+)

---

## 🎮 Divirta-se!

O objetivo do Reflexo é simples: **aproveite a jornada**, construa sua sequência e volte amanhã para um novo desafio.

Bom jogo! 🎯

---

**Última atualização:** Junho de 2026  
**Versão:** 1.1.0
