# 🚀 Guia Completo Git

> Repositório de referência e consulta rápida para controle de versão com Git

## 📖 Índice

1. [Overview](#-overview)
2. [Conceitos Fundamentais](#-conceitos-fundamentais)
3. [Instalação e Configuração](#-instalação-e-configuração)
4. [Comandos Essenciais](#-comandos-essenciais)
5. [Trabalhando com Branches](#-trabalhando-com-branches)
6. [Colaboração e Repositórios Remotos](#-colaboração-e-repositórios-remotos)
7. [Resolução de Conflitos](#-resolução-de-conflitos)
8. [Boas Práticas](#-boas-práticas)
9. [Workflows Comuns](#-workflows-comuns)
10. [Troubleshooting](#-troubleshooting)

---

## 🎯 Overview

### O que é Git?

Git é um **sistema de controle de versão distribuído** criado por Linus Torvalds em 2005. Ele permite:

- 📝 Rastrear mudanças no código ao longo do tempo
- 👥 Colaborar com múltiplos desenvolvedores simultaneamente
- 🔄 Reverter para versões anteriores quando necessário
- 🌿 Trabalhar em features isoladas sem afetar o código principal
- 🔍 Entender quem fez cada alteração e por quê

### Por que usar Git?

- **Histórico completo**: Cada alteração é registrada com autor, data e motivo
- **Trabalho offline**: Você tem o repositório completo localmente
- **Branches leves**: Crie e mescle ramificações facilmente
- **Integridade**: Tudo é verificado com checksums (SHA-1)
- **Distribuído**: Cada desenvolvedor tem uma cópia completa do histórico

### Git vs GitHub/GitLab

- **Git**: Sistema de controle de versão (software)
- **GitHub/GitLab/Bitbucket**: Plataformas de hospedagem de repositórios Git (serviços)

---

## 🧩 Conceitos Fundamentais

### Estados dos Arquivos

```
Working Directory → Staging Area → Repository
    (modificado)      (preparado)    (commitado)
```

1. **Working Directory**: Onde você edita os arquivos
2. **Staging Area (Index)**: Arquivos preparados para commit
3. **Repository (.git)**: Histórico de commits salvos

### Áreas do Git

```
┌─────────────────┐
│ Working         │  git add
│ Directory       │ ────────► ┌─────────────┐
│ (untracked/     │           │  Staging    │  git commit
│  modified)      │           │  Area       │ ────────► ┌────────────┐
└─────────────────┘           │  (staged)   │           │ Repository │
                              └─────────────┘           │ (.git)     │
                                                        └────────────┘
```

### Tipos de Objetos Git

- **Blob**: Conteúdo dos arquivos
- **Tree**: Estrutura de diretórios
- **Commit**: Snapshot do projeto
- **Tag**: Referência nomeada a um commit

### Referências Importantes

- **HEAD**: Ponteiro para o commit atual
- **main/master**: Branch principal padrão
- **origin**: Nome padrão do repositório remoto
- **HEAD~1**: Commit anterior ao HEAD
- **HEAD^**: Primeiro pai do commit

---

## ⚙️ Instalação e Configuração

### Instalação

```bash
# Linux (Debian/Ubuntu)
sudo apt-get install git

# Linux (Fedora)
sudo dnf install git

# macOS (Homebrew)
brew install git

# Windows
# Baixe em: https://git-scm.com/download/win
```

### Configuração Inicial (Obrigatória)

```bash
# Identidade global
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"

# Editor padrão
git config --global core.editor "code --wait"  # VS Code
git config --global core.editor "vim"           # Vim

# Branch padrão
git config --global init.defaultBranch main
```

### Configurações Úteis

```bash
# Colorir output
git config --global color.ui auto

# Salvar credenciais temporariamente (15 min)
git config --global credential.helper cache

# Aliases úteis
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --graph --oneline --all'

# Ver todas as configurações
git config --list
git config --list --show-origin  # com origem
```

---

## 🎮 Comandos Essenciais

### Criar/Clonar Repositório

```bash
# Criar novo repositório
git init
git init nome-projeto  # cria pasta e inicializa

# Clonar repositório existente
git clone <url>
git clone <url> <nome-pasta>
git clone -b <branch> <url>  # clonar branch específica
```

### Status e Informações

```bash
# Ver estado atual
git status
git status -s  # resumido
git status -sb  # com branch

# Ver histórico
git log
git log --oneline  # uma linha por commit
git log --graph --oneline --all  # visual com branches
git log -n 5  # últimos 5 commits
git log --since="2 weeks ago"
git log --author="Nome"
git log --grep="palavra"  # buscar em mensagens
git log -- arquivo.txt  # histórico de um arquivo

# Ver detalhes de commit
git show <hash>
git show HEAD  # último commit
git show HEAD~2  # 2 commits atrás
```

### Adicionar e Commitar

```bash
# Adicionar ao staging
git add arquivo.txt
git add .  # todos os arquivos
git add *.js  # padrão
git add -A  # tudo (inclusive deletados)
git add -p  # interativo (escolher partes)

# Commitar
git commit -m "mensagem curta"
git commit -m "título" -m "descrição detalhada"
git commit -am "mensagem"  # add + commit (só tracked)
git commit --amend  # alterar último commit
git commit --amend --no-edit  # sem alterar mensagem

# Remover arquivos
git rm arquivo.txt
git rm --cached arquivo.txt  # remove do Git, mantém local
git rm -r pasta/  # recursivo

# Mover/renomear
git mv arquivo-antigo.txt arquivo-novo.txt
```

### Ver Diferenças

```bash
# Comparar working directory com staging
git diff

# Comparar staging com último commit
git diff --staged
git diff --cached

# Comparar commits
git diff HEAD~2 HEAD
git diff <hash1> <hash2>

# Comparar branches
git diff main..feature-branch
git diff main...feature-branch  # desde divergência

# Diferença de arquivo específico
git diff arquivo.txt
git diff HEAD~2 HEAD -- arquivo.txt

# Estatísticas
git diff --stat
```

---

## 🌿 Trabalhando com Branches

### Conceito de Branches

Branches permitem desenvolver features, corrigir bugs ou experimentar ideias de forma isolada sem afetar o código principal.

```
main    ●─────●─────●─────●
              │
feature       └─●─●─●
```

### Comandos de Branch

```bash
# Listar branches
git branch  # locais
git branch -a  # todas (incluindo remotas)
git branch -v  # com último commit
git branch -vv  # com tracking

# Criar branch
git branch nome-branch
git branch nome-branch <hash>  # a partir de commit específico

# Trocar de branch
git checkout nome-branch
git switch nome-branch  # comando moderno (Git 2.23+)

# Criar e trocar
git checkout -b nome-branch
git switch -c nome-branch

# Renomear branch
git branch -m nome-antigo nome-novo
git branch -m nome-novo  # renomear branch atual

# Deletar branch
git branch -d nome-branch  # seguro (verifica merge)
git branch -D nome-branch  # forçar (CUIDADO!)

# Deletar branch remota
git push origin --delete nome-branch
```

### Merge (Mesclar Branches)

```bash
# Merge básico
git checkout main
git merge feature-branch

# Merge com mensagem customizada
git merge feature-branch -m "Merge feature X"

# Merge sem fast-forward (cria commit de merge)
git merge --no-ff feature-branch

# Merge squash (unifica commits em um)
git merge --squash feature-branch
git commit -m "Implementa feature completa"

# Abortar merge
git merge --abort
```

### Rebase (Reorganizar Histórico)

```bash
# Rebase básico
git checkout feature-branch
git rebase main

# Rebase interativo (editar histórico)
git rebase -i HEAD~3  # últimos 3 commits
# Opções: pick, reword, edit, squash, fixup, drop

# Continuar após resolver conflitos
git add arquivo-resolvido.txt
git rebase --continue

# Pular commit problemático
git rebase --skip

# Abortar rebase
git rebase --abort
```

### Merge vs Rebase

**Merge**: Preserva histórico completo, cria commits de merge
```
main    ●─────●─────●─────●─────M
              │               ╱
feature       └─●─●─●────────┘
```

**Rebase**: Cria histórico linear, reescreve commits
```
main    ●─────●─────●─────●─●─●─●
                              └─ feature (rebaseada)
```

**Quando usar cada um?**
- **Merge**: Histórico público, preservar contexto de features
- **Rebase**: Histórico local, manter histórico limpo antes de push

---

## 🤝 Colaboração e Repositórios Remotos

### Gerenciar Remotos

```bash
# Ver remotos
git remote
git remote -v  # com URLs

# Adicionar remoto
git remote add origin <url>
git remote add upstream <url>  # repositório original (forks)

# Renomear remoto
git remote rename origin novo-nome

# Remover remoto
git remote remove origin

# Ver detalhes do remoto
git remote show origin

# Atualizar URL do remoto
git remote set-url origin <nova-url>
```

### Fetch, Pull e Push

```bash
# Fetch (baixar sem merge)
git fetch origin
git fetch origin main
git fetch --all  # todos os remotos
git fetch --prune  # remover referências deletadas

# Pull (fetch + merge)
git pull origin main
git pull  # usa tracking branch
git pull --rebase  # pull com rebase ao invés de merge

# Push (enviar commits)
git push origin main
git push -u origin main  # primeira vez (set upstream)
git push  # usa tracking branch
git push --force  # forçar (CUIDADO!)
git push --force-with-lease  # forçar com segurança
git push --all  # todos os branches
git push --tags  # enviar tags
```

### Tracking Branches

```bash
# Configurar tracking
git branch --set-upstream-to=origin/main main
git branch -u origin/main main

# Ver tracking branches
git branch -vv

# Push e criar tracking automaticamente
git push -u origin feature-branch
```

---

## ⚔️ Resolução de Conflitos

### O que são Conflitos?

Conflitos ocorrem quando Git não consegue mesclar automaticamente mudanças na mesma parte de um arquivo.

### Como Identificar

```bash
# Ver arquivos com conflito
git status

# Ver diferenças do conflito
git diff
```

### Formato de Conflito

```
<<<<<<< HEAD
seu código atual
=======
código da branch sendo mesclada
>>>>>>> feature-branch
```

### Resolver Conflitos

1. **Abrir arquivo** e localizar marcadores de conflito
2. **Editar** para manter código desejado
3. **Remover** marcadores (`<<<<<<<`, `=======`, `>>>>>>>`)
4. **Adicionar** arquivo resolvido: `git add arquivo.txt`
5. **Finalizar** merge/rebase: `git commit` ou `git rebase --continue`

### Ferramentas de Merge

```bash
# Usar ferramenta visual
git mergetool

# Configurar ferramenta padrão
git config --global merge.tool meld
git config --global merge.tool kdiff3
git config --global merge.tool vimdiff

# Aceitar versão específica
git checkout --ours arquivo.txt  # manter sua versão
git checkout --theirs arquivo.txt  # aceitar versão deles
```

---

## ✨ Boas Práticas

### Mensagens de Commit

**Formato recomendado** (Conventional Commits):

```
<tipo>(<escopo>): <descrição curta>

<descrição detalhada opcional>

<rodapé opcional>
```

**Tipos comuns**:
- `feat`: Nova funcionalidade
- `fix`: Correção de bug
- `docs`: Documentação
- `style`: Formatação (sem mudança de código)
- `refactor`: Refatoração
- `test`: Testes
- `chore`: Manutenção

**Exemplos**:
```bash
git commit -m "feat(auth): adiciona login com Google"
git commit -m "fix(api): corrige timeout em requisições"
git commit -m "docs: atualiza README com instruções de instalação"
```

### Quando Commitar?

✅ **BOM**:
- Commits pequenos e focados
- Uma mudança lógica por commit
- Código funcional (compila/executa)
- Testes passando

❌ **RUIM**:
- Commits gigantes com múltiplas mudanças
- Código quebrado
- Mensagens vagas ("fix", "wip", "teste")

### Git Ignore

Criar arquivo `.gitignore` para ignorar arquivos:

```bash
# Dependências
node_modules/
vendor/

# Arquivos de build
dist/
build/
*.o
*.exe

# Arquivos IDE
.vscode/
.idea/
*.swp

# Ambiente
.env
.env.local

# Logs
*.log
logs/

# Sistema
.DS_Store
Thumbs.db
```

### Workflow de Branches

**Gitflow** (projetos grandes):
- `main`: Produção
- `develop`: Desenvolvimento
- `feature/*`: Novas features
- `hotfix/*`: Correções urgentes
- `release/*`: Preparação para release

**GitHub Flow** (projetos menores):
- `main`: Sempre deployável
- `feature/*`: Branches curtos de feature

---

## 🔄 Workflows Comuns

### Workflow Básico (Solo)

```bash
# 1. Iniciar repositório
git init
git add .
git commit -m "feat: commit inicial"

# 2. Adicionar remoto
git remote add origin <url>
git push -u origin main

# 3. Ciclo de trabalho
git add arquivo.txt
git commit -m "feat: adiciona funcionalidade X"
git push

# 4. Repetir conforme necessário
```

### Workflow com Feature Branch

```bash
# 1. Atualizar main
git checkout main
git pull origin main

# 2. Criar branch de feature
git checkout -b feature/nova-funcionalidade

# 3. Desenvolver e commitar
git add .
git commit -m "feat: implementa funcionalidade"
git commit -m "test: adiciona testes"
git commit -m "docs: atualiza documentação"

# 4. Atualizar com main (caso necessário)
git checkout main
git pull origin main
git checkout feature/nova-funcionalidade
git rebase main  # ou git merge main

# 5. Enviar branch
git push -u origin feature/nova-funcionalidade

# 6. Criar Pull Request no GitHub/GitLab

# 7. Após aprovação, fazer merge
git checkout main
git pull origin main
git merge feature/nova-funcionalidade
git push origin main

# 8. Limpar branch
git branch -d feature/nova-funcionalidade
git push origin --delete feature/nova-funcionalidade
```

### Workflow de Fork (Contribuição Open Source)

```bash
# 1. Fork no GitHub e clonar
git clone <url-seu-fork>
cd projeto

# 2. Adicionar upstream
git remote add upstream <url-original>

# 3. Criar branch
git checkout -b fix/corrige-bug

# 4. Fazer alterações
git add .
git commit -m "fix: corrige bug X"

# 5. Atualizar com upstream
git fetch upstream
git rebase upstream/main

# 6. Enviar para seu fork
git push origin fix/corrige-bug

# 7. Criar Pull Request no GitHub

# 8. Manter sincronizado
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## 🆘 Troubleshooting

### Desfazer Alterações

```bash
# Descartar mudanças não commitadas (arquivo)
git checkout -- arquivo.txt
git restore arquivo.txt  # Git 2.23+

# Descartar todas mudanças não commitadas
git checkout -- .
git restore .

# Remover arquivo do staging (manter alterações)
git reset HEAD arquivo.txt
git restore --staged arquivo.txt

# Desfazer último commit (manter alterações)
git reset --soft HEAD~1

# Desfazer último commit (descartar alterações)
git reset --hard HEAD~1

# Desfazer commits e manter working directory limpo
git reset --hard HEAD~3  # voltar 3 commits

# Reverter commit (cria novo commit)
git revert <hash>
git revert HEAD  # reverter último
git revert HEAD~3..HEAD  # reverter últimos 3
```

### Recuperar Trabalho Perdido

```bash
# Ver histórico completo de referências
git reflog

# Recuperar commit "perdido"
git checkout <hash-do-reflog>
git checkout -b branch-recuperada

# Recuperar branch deletada
git reflog
git checkout -b branch-recuperada <hash>
```

### Corrigir Último Commit

```bash
# Alterar mensagem do último commit
git commit --amend -m "nova mensagem"

# Adicionar arquivo esquecido ao último commit
git add arquivo-esquecido.txt
git commit --amend --no-edit

# Alterar autor do último commit
git commit --amend --author="Nome <email@exemplo.com>"
```

### Limpar Arquivos Não Rastreados

```bash
# Ver o que seria removido
git clean -n

# Remover arquivos
git clean -f

# Remover arquivos e diretórios
git clean -fd

# Remover também arquivos ignorados
git clean -fdx
```

### Stash (Guardar Temporariamente)

```bash
# Guardar alterações
git stash
git stash save "descrição do trabalho"
git stash -u  # incluir arquivos untracked

# Listar stashes
git stash list

# Aplicar último stash
git stash apply  # mantém o stash
git stash pop  # remove após aplicar

# Aplicar stash específico
git stash apply stash@{2}

# Ver conteúdo do stash
git stash show
git stash show -p  # com diff

# Remover stash
git stash drop stash@{0}
git stash clear  # remover todos
```

### Problemas Comuns

**Erro: "Your branch is ahead of origin/main"**
```bash
git push origin main
```

**Erro: "Your branch is behind origin/main"**
```bash
git pull origin main
```

**Erro: "fatal: refusing to merge unrelated histories"**
```bash
git pull origin main --allow-unrelated-histories
```

**Erro: "Permission denied (publickey)"**
```bash
# Configurar chave SSH
ssh-keygen -t ed25519 -C "seu@email.com"
# Adicionar chave ao GitHub/GitLab
```

**Commit no branch errado**
```bash
# Salvar alterações
git reset --soft HEAD~1
git stash

# Ir para branch correto
git checkout branch-correto
git stash pop
git commit -m "mensagem"
```

---

## 📚 Recursos Adicionais

### Comandos Avançados

```bash
# Buscar em todo histórico
git log -S "texto" --source --all

# Ver quem modificou cada linha
git blame arquivo.txt
git blame -L 10,20 arquivo.txt  # linhas específicas

# Bisect (encontrar commit que introduziu bug)
git bisect start
git bisect bad  # commit atual tem bug
git bisect good <hash>  # commit antigo sem bug
# testar, depois marcar como good ou bad
git bisect reset  # finalizar

# Cherry-pick (aplicar commit específico)
git cherry-pick <hash>

# Revert múltiplos commits
git revert HEAD~3..HEAD

# Arquivar projeto
git archive --format=zip --output=projeto.zip HEAD

# Contar commits
git rev-list --count HEAD

# Ver tamanho do repositório
git count-objects -vH
```

### Links Úteis

- 📖 [Documentação Oficial](https://git-scm.com/doc)
- 📘 [Pro Git Book](https://git-scm.com/book/pt-br)
- 🎮 [Learn Git Branching](https://learngitbranching.js.org/)
- 📝 [Conventional Commits](https://www.conventionalcommits.org/)
- 🔍 [Git Explorer](https://gitexplorer.com/)

---

## 📝 Checklist de Início de Projeto

```bash
☐ git init
☐ git config user.name e user.email
☐ Criar README.md
☐ Criar .gitignore
☐ git add .
☐ git commit -m "feat: commit inicial"
☐ Criar repositório remoto (GitHub/GitLab)
☐ git remote add origin <url>
☐ git push -u origin main
☐ Configurar branch protection rules (opcional)
☐ Adicionar colaboradores (se equipe)
```

---

## 🎓 Glossário

- **Branch**: Ramificação do código
- **Commit**: Snapshot do projeto em um momento
- **Clone**: Cópia de um repositório
- **Fork**: Cópia independente de um repositório
- **HEAD**: Referência ao commit atual
- **Index**: Staging area
- **Merge**: Mesclar branches
- **Origin**: Repositório remoto padrão
- **Pull Request (PR)**: Solicitação de merge
- **Push**: Enviar commits para remoto
- **Pull**: Baixar e mesclar do remoto
- **Rebase**: Reorganizar histórico de commits
- **Remote**: Repositório em servidor
- **Repository**: Diretório com histórico Git
- **Staging**: Área de preparação para commit
- **Tag**: Marcador de versão
- **Working Directory**: Diretório de trabalho

---

## 📄 Licença

Este guia é de domínio público e pode ser usado livremente para fins educacionais.

## 🤝 Contribuições

Sugestões e melhorias são bem-vindas! Sinta-se à vontade para contribuir.

---

**Última atualização**: Dezembro 2024  
**Versão**: 1.0