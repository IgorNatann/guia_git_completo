# Git Cheatsheet - Referência Rápida

> Guia compacto de comandos essenciais do Git

---

## ⚙️ Configuração

```bash
# Configurar identidade
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"

# Configurar editor
git config --global core.editor "code --wait"

# Aliases úteis
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --graph --oneline --all"

# Ver configurações
git config --list
```

---

## 📦 Criar/Clonar Repositório

```bash
git init                          # Inicializar repositório
git init nome-projeto             # Criar e inicializar
git clone <url>                   # Clonar repositório
git clone <url> <pasta>           # Clonar com nome específico
```

---

## 📝 Operações Básicas

### Status e Informações

```bash
git status                        # Ver estado atual
git status -s                     # Status resumido
git log                           # Ver histórico
git log --oneline                 # Histórico resumido
git log --graph --oneline --all   # Histórico visual
git log -n 5                      # Últimos 5 commits
git show <hash>                   # Detalhes do commit
git diff                          # Ver diferenças
git diff --staged                 # Diferenças no staging
```

### Adicionar e Commitar

```bash
git add <arquivo>                 # Adicionar arquivo
git add .                         # Adicionar todos
git add -p                        # Adicionar interativo
git commit -m "mensagem"          # Fazer commit
git commit -am "mensagem"         # Add + commit (tracked)
git commit --amend                # Alterar último commit
```

### Remover e Mover

```bash
git rm <arquivo>                  # Remover arquivo
git rm --cached <arquivo>         # Remover do Git (manter local)
git mv <antigo> <novo>            # Renomear/mover arquivo
```

---

## 🌿 Branches

### Gerenciar Branches

```bash
git branch                        # Listar branches locais
git branch -a                     # Listar todas (+ remotas)
git branch <nome>                 # Criar branch
git branch -m <antigo> <novo>     # Renomear branch
git branch -d <nome>              # Deletar branch
git branch -D <nome>              # Forçar deleção
```

### Navegar entre Branches

```bash
git checkout <branch>             # Trocar de branch
git checkout -b <branch>          # Criar e trocar
git switch <branch>               # Trocar (moderno)
git switch -c <branch>            # Criar e trocar (moderno)
```

### Merge e Rebase

```bash
git merge <branch>                # Mesclar branch
git merge --no-ff <branch>        # Merge sem fast-forward
git merge --squash <branch>       # Merge com squash
git rebase <branch>               # Rebase
git rebase -i HEAD~3              # Rebase interativo
git rebase --continue             # Continuar rebase
git rebase --abort                # Cancelar rebase
```

---

## 🔄 Repositórios Remotos

### Gerenciar Remotos

```bash
git remote                        # Listar remotos
git remote -v                     # Listar com URLs
git remote add origin <url>       # Adicionar remoto
git remote remove origin          # Remover remoto
git remote show origin            # Detalhes do remoto
```

### Sincronizar

```bash
git fetch origin                  # Buscar alterações
git fetch --all                   # Buscar de todos remotos
git pull origin <branch>          # Baixar e mesclar
git pull --rebase                 # Pull com rebase
git push origin <branch>          # Enviar alterações
git push -u origin <branch>       # Enviar e set upstream
git push --force-with-lease       # Push forçado seguro
git push --tags                   # Enviar tags
git push origin --delete <branch> # Deletar branch remota
```

---

## ↩️ Desfazer Alterações

### Descartar Mudanças

```bash
git restore <arquivo>             # Descartar mudanças
git restore --staged <arquivo>    # Remover do staging
git checkout -- <arquivo>         # Descartar (antigo)
git reset HEAD <arquivo>          # Remover staging (antigo)
```

### Desfazer Commits

```bash
git reset --soft HEAD~1           # Desfazer commit (manter mudanças)
git reset --mixed HEAD~1          # Desfazer commit + staging
git reset --hard HEAD~1           # Desfazer commit + mudanças
git revert <hash>                 # Reverter commit (novo commit)
git commit --amend                # Alterar último commit
```

---

## 💾 Stash

```bash
git stash                         # Guardar alterações
git stash save "descrição"        # Guardar com descrição
git stash -u                      # Incluir arquivos untracked
git stash list                    # Listar stashes
git stash show                    # Ver conteúdo
git stash apply                   # Aplicar (manter stash)
git stash pop                     # Aplicar (remover stash)
git stash drop stash@{0}          # Remover stash
git stash clear                   # Remover todos
```

---

## 🏷️ Tags

```bash
git tag                           # Listar tags
git tag <nome>                    # Criar tag
git tag -a v1.0 -m "versão 1.0"   # Tag anotada
git show <tag>                    # Ver detalhes da tag
git push origin <tag>             # Enviar tag
git push origin --tags            # Enviar todas tags
git tag -d <tag>                  # Deletar tag local
git push origin :refs/tags/<tag>  # Deletar tag remota
```

---

## 🔍 Buscar e Inspecionar

```bash
git log --grep="texto"            # Buscar em mensagens
git log -S "código"               # Buscar em código
git log --author="nome"           # Filtrar por autor
git log --since="2 weeks ago"     # Filtrar por data
git log -- <arquivo>              # Histórico de arquivo
git blame <arquivo>               # Ver quem modificou linhas
git show <hash>:<arquivo>         # Ver arquivo em commit
```

---

## 🗑️ Limpeza

```bash
git clean -n                      # Ver o que seria removido
git clean -f                      # Remover arquivos untracked
git clean -fd                     # Remover arquivos e pastas
git clean -fdx                    # Incluir arquivos ignorados
git fetch --prune                 # Limpar refs remotas
git remote prune origin           # Limpar refs de origin
git gc                            # Garbage collection
```

---

## 🆘 Recuperação

```bash
git reflog                        # Ver histórico de referências
git checkout <hash>               # Recuperar commit
git checkout -b <branch> <hash>   # Criar branch de commit
git fsck --lost-found             # Encontrar objetos perdidos
```

---

## 🔧 Comandos Avançados

```bash
git cherry-pick <hash>            # Aplicar commit específico
git bisect start                  # Iniciar busca binária
git bisect bad                    # Marcar commit ruim
git bisect good <hash>            # Marcar commit bom
git bisect reset                  # Finalizar bisect
git archive --format=zip HEAD     # Exportar como ZIP
git submodule add <url>           # Adicionar submódulo
git submodule update --init       # Inicializar submódulos
```

---

## 📌 Conceitos Importantes

### Estados dos Arquivos
```
Untracked → Unmodified → Modified → Staged → Committed
```

### Áreas do Git
```
Working Directory → Staging Area → Repository
```

### Referências
- **HEAD**: Commit atual
- **HEAD~1**: Commit anterior
- **HEAD^**: Primeiro pai
- **origin**: Remoto padrão
- **main/master**: Branch principal

---

## 🎯 Workflows Comuns

### Workflow Básico
```bash
git pull origin main              # Atualizar
git checkout -b feature/nova      # Criar branch
# ... fazer alterações ...
git add .                         # Adicionar
git commit -m "feat: nova feature" # Commitar
git push -u origin feature/nova   # Enviar
# ... criar Pull Request ...
git checkout main                 # Voltar main
git pull origin main              # Atualizar
git branch -d feature/nova        # Deletar branch
```

### Workflow de Hotfix
```bash
git checkout main                 # Ir para main
git pull origin main              # Atualizar
git checkout -b hotfix/bug        # Criar hotfix
# ... corrigir bug ...
git add .
git commit -m "fix: corrige bug"
git checkout main
git merge hotfix/bug              # Merge em main
git tag -a v1.0.1 -m "Hotfix"     # Tag de versão
git push origin main --tags       # Enviar
git checkout develop
git merge hotfix/bug              # Merge em develop
git push origin develop
```

---

## ✨ Mensagens de Commit

### Formato (Conventional Commits)
```
<tipo>(<escopo>): <descrição>

[corpo opcional]

[rodapé opcional]
```

### Tipos Comuns
- **feat**: Nova funcionalidade
- **fix**: Correção de bug
- **docs**: Documentação
- **style**: Formatação
- **refactor**: Refatoração
- **test**: Testes
- **chore**: Manutenção

### Exemplos
```bash
git commit -m "feat(auth): adiciona login com Google"
git commit -m "fix(api): corrige timeout em requisições"
git commit -m "docs: atualiza README com instalação"
git commit -m "test: adiciona testes unitários"
```

---

## 🚫 .gitignore Comum

```gitignore
# Dependências
node_modules/
vendor/
venv/

# Build
dist/
build/
*.o
*.exe

# Ambiente
.env
.env.local
secrets.yml

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Logs
*.log
logs/
```

---

## ⚠️ Cuidados Importantes

### Nunca Force Push em Branches Compartilhadas
```bash
# ❌ PERIGOSO
git push --force origin main

# ✅ SEGURO
git push --force-with-lease origin feature/minha
```

### Não Rebase Commits Públicos
```bash
# ❌ NÃO rebase depois de push
git push origin feature
git rebase main  # EVITAR!

# ✅ Use merge para branches públicas
git merge main
```

### Sempre Pull Antes de Push
```bash
# ✅ Fluxo correto
git pull origin main
git push origin main

# ❌ Evitar conflitos
git push origin main  # pode dar erro
```

---

## 🔗 Aliases Recomendados

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.lg 'log --graph --oneline --all'
git config --global alias.undo 'reset --soft HEAD~1'
git config --global alias.visual 'log --graph --pretty=format:"%C(yellow)%h%Creset -%C(auto)%d%Creset %s %C(green)(%cr) %C(bold blue)<%an>%Creset" --abbrev-commit'
```

---

## 📚 Recursos

- **Documentação**: https://git-scm.com/doc
- **Pro Git Book**: https://git-scm.com/book/pt-br
- **Learn Git Branching**: https://learngitbranching.js.org/
- **Git Explorer**: https://gitexplorer.com/

---

## 🎓 Dicas Finais

1. ✅ Commit frequentemente com mensagens claras
2. ✅ Use branches para features e experimentos
3. ✅ Sempre revise com `git status` antes de commit
4. ✅ Escreva mensagens descritivas
5. ✅ Faça pull antes de iniciar trabalho
6. ✅ Use .gitignore para arquivos sensíveis
7. ✅ Teste antes de fazer push
8. ✅ Documente decisões importantes
9. ✅ Aprenda a resolver conflitos
10. ✅ Mantenha histórico limpo e legível

---

## 📄 Sobre

**Versão**: 1.0  
**Atualizado**: Dezembro 2024  
**Licença**: Domínio Público

Este cheatsheet é um guia de referência rápida. Para informações detalhadas, consulte a documentação oficial do Git.

---

**💡 Dica**: Imprima este cheatsheet e mantenha por perto durante o desenvolvimento!