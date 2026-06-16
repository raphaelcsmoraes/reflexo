# Contribuindo para Reflexo

Obrigado por seu interesse em contribuir para o Reflexo! Este documento fornece diretrizes e instruções para contribuir ao projeto.

## Código de Conduta

Por favor, note que este projeto é lançado com um [Código de Conduta do Contribuidor](CODE_OF_CONDUCT.md). Ao participar neste projeto, você concorda em aderir aos seus termos.

## Como Contribuir

### Reportar Bugs

Bugs são rastreados como GitHub issues. Crie uma issue e inclua as seguintes informações:

- **Título claro e descritivo** para a issue
- **Descrição detalhada** do comportamento observado
- **Exemplo específico** para reproduzir o problema
- **Comportamento esperado** vs o comportamento atual
- **Screenshots** se aplicável
- **Seu navegador e versão**
- **Sistema operacional**

### Sugerir Enhancements

Enhancements são rastreados como GitHub issues. Ao sugerir um enhancement, por favor inclua:

- **Título claro e descritivo**
- **Descrição detalhada** do enhancement sugerido
- **Possível implementação**
- **Por que você acha que seria útil**

### Pull Requests

- Siga as [styleguides](#styleguides) do JavaScript/CSS/HTML
- Depois de enviar o pull request, verifique que todos os status checks passaram

## Styleguides

### Commits

- Use o tempo presente ("Add feature" não "Added feature")
- Use o modo imperativo ("Move cursor to..." não "Moves cursor to...")
- Limite a primeira linha a 72 caracteres ou menos
- Referencie issues e pull requests liberalmente

### JavaScript

- Use `const` e `let` em vez de `var`
- Use nomes descritivos para variáveis
- Adicione comentários para lógica complexa
- Siga a convenção camelCase para nomes de variáveis
- Use funções arrow quando apropriado

### CSS

- Use variáveis CSS (`:root { --cor: #xyz; }`)
- Siga a ordem: position, display, tamanho, cor, fonte
- Use nomes de classe significativos (BEM quando possível)
- Mobile-first: escreva estilos para mobile primeiro, depois adicione media queries

### HTML

- Use semantic HTML quando possível
- Adicione `aria-label` para elementos interativos
- Use `role="button"` quando apropriado
- Mantenha o indentação consistente (2 espaços)

## Processo de Desenvolvimento

1. **Fork** o repositório
2. **Clone** seu fork localmente:
   ```bash
   git clone https://github.com/seu-usuario/reflexo.git
   cd reflexo
   ```

3. **Crie uma branch** para sua feature:
   ```bash
   git checkout -b feature/sua-feature-incrivel
   ```

4. **Faça suas mudanças** e teste localmente
5. **Commit** suas mudanças:
   ```bash
   git commit -m 'Add feature: sua-feature-incrivel'
   ```

6. **Push** para sua branch:
   ```bash
   git push origin feature/sua-feature-incrivel
   ```

7. **Abra um Pull Request** no repositório original

## Testando Localmente

```bash
# Abra em um servidor local
python -m http.server 8000

# Acesse no navegador
http://localhost:8000
```

## Área de Prioridade

Estamos particularmente interessados em contribuições nas seguintes áreas:

- 🐛 **Bug fixes** — Corrija bugs encontrados
- 🌍 **Internacionalização** — Adicione suporte a novos idiomas
- 🎨 **Temas** — Crie novos esquemas de cores
- 📱 **Responsividade** — Melhore suporte para diferentes dispositivos
- ♿ **Acessibilidade** — Melhore ARIA labels e navegação
- 📝 **Documentação** — Melhore este README e documentação

## Perguntas?

Sinta-se à vontade para:
- Abrir uma issue para discussão
- Usar a aba "Discussions" do repositório
- Comentar em issues existentes

## Reconhecimento

Todos os contribuidores serão reconhecidos no arquivo [CONTRIBUTORS.md](CONTRIBUTORS.md).

---

Obrigado por contribuir! 🎉
