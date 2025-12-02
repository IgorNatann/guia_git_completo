# 📂 Exemplos Práticos de Git

Esta pasta contém exemplos práticos e cenários reais de uso do Git.

## 📋 Índice de Exemplos

1. [Exemplo 01: Primeiro Repositório](#exemplo-01-primeiro-repositório)
2. [Exemplo 02: Trabalhando com Branches](#exemplo-02-trabalhando-com-branches)
3. [Exemplo 03: Resolvendo Conflitos](#exemplo-03-resolvendo-conflitos)
4. [Exemplo 04: Rebase Interativo](#exemplo-04-rebase-interativo)
5. [Exemplo 05: Desfazendo Alterações](#exemplo-05-desfazendo-alterações)
6. [Exemplo 06: Contribuindo para Projeto Open Source](#exemplo-06-contribuindo-para-projeto-open-source)
7. [Exemplo 07: Hotfix em Produção](#exemplo-07-hotfix-em-produção)
8. [Exemplo 08: Stash para Salvar Trabalho](#exemplo-08-stash-para-salvar-trabalho)

---

## Exemplo 01: Primeiro Repositório

**Cenário**: Você está iniciando um novo projeto e quer versionar com Git.

### Passo a Passo

```bash
# 1. Criar pasta do projeto
mkdir meu-primeiro-projeto
cd meu-primeiro-projeto

# 2. Inicializar Git
git init

# 3. Configurar informações (se ainda não fez globalmente)
git config user.name "Seu Nome"
git config user.email "seu@email.com"

# 4. Criar arquivo inicial
echo "# Meu Primeiro Projeto" > README.md
echo "console.log('Hello World');" > index.js

# 5. Criar .gitignore
cat > .gitignore << EOF
node_modules/
.env
*.log
EOF

# 6. Adicionar arquivos ao staging
git add .

# 7. Ver status
git status

# 8. Fazer primeiro commit
git commit -m "feat: commit inicial do projeto"

# 9. Ver histórico
git log

# 10. Criar repositório no GitHub e conectar
git remote add origin https://github.com/seu-usuario/meu-primeiro-projeto.git
git branch -M main
git push -u origin main
```

**Resultado**: Repositório criado e enviado para o GitHub! 🎉

---

## Exemplo 02: Trabalhando com Branches

**Cenário**: Você precisa desenvolver uma nova funcionalidade sem afetar o código principal.

### Passo a Passo

```bash
# 1. Verificar branch atual
git branch

# 2. Atualizar branch main
git checkout main
git pull origin main

# 3. Criar nova branch para feature
git checkout -b feature/login

# 4. Desenvolver a funcionalidade
cat > login.js << EOF
function login(username, password) {
  // Implementação do login
  console.log('Login realizado');
}
EOF

git add login.js
git commit -m "feat: adiciona função de login"

# 5. Fazer mais alterações
cat > auth.js << EOF
function authenticate(token) {
  // Validação do token
  return true;
}
EOF

git add auth.js
git commit -m "feat: adiciona autenticação por token"

# 6. Ver histórico da branch
git log --oneline

# 7. Enviar branch para remoto
git push -u origin feature/login

# 8. Voltar para main e fazer merge
git checkout main
git merge feature/login

# 9. Enviar main atualizada
git push origin main

# 10. Deletar branch (opcional)
git branch -d feature/login
git push origin --delete feature/login
```

**Resultado**: Feature desenvolvida isoladamente e integrada com sucesso! ✅

---

## Exemplo 03: Resolvendo Conflitos

**Cenário**: Dois desenvolvedores modificaram o mesmo arquivo e há conflito no merge.

### Simulação do Conflito

```bash
# === Desenvolvedor 1 ===
git checkout main
git checkout -b dev1/update-readme

# Modificar README.md
echo "## Instalação" >> README.md
echo "npm install" >> README.md

git add README.md
git commit -m "docs: adiciona instruções de instalação"
git push -u origin dev1/update-readme

# === Desenvolvedor 2 (simultaneamente) ===
git checkout main
git checkout -b dev2/update-readme

# Modificar mesma parte do README.md
echo "## Como Instalar" >> README.md
echo "Execute: npm install" >> README.md

git add README.md
git commit -m "docs: adiciona seção de instalação"
git push -u origin dev2/update-readme

# === Merge e Conflito ===
git checkout main
git merge dev1/update-readme  # OK
git merge dev2/update-readme  # CONFLITO!

# Git mostra:
# Auto-merging README.md
# CONFLICT (content): Merge conflict in README.md
```

### Arquivo com Conflito

```markdown
# Meu Primeiro Projeto

<<<<<<< HEAD
## Instalação
npm install
=======
## Como Instalar
Execute: npm install
>>>>>>> dev2/update-readme
```

### Resolvendo o Conflito

```bash
# 1. Abrir arquivo e resolver manualmente
# Editar README.md para:

# Meu Primeiro Projeto

## Instalação
Execute o comando abaixo para instalar as dependências:
```
npm install
```

# 2. Adicionar arquivo resolvido
git add README.md

# 3. Finalizar merge
git commit -m "merge: resolve conflito em README.md"

# 4. Enviar para remoto
git push origin main
```

**Resultado**: Conflito resolvido mantendo o melhor dos dois códigos! 🤝

---

## Exemplo 04: Rebase Interativo

**Cenário**: Você fez vários commits pequenos e quer organizá-los antes do Pull Request.

### Histórico Bagunçado

```bash
git log --oneline

# a1b2c3d fix typo
# d4e5f6g add tests
# g7h8i9j fix tests
# j1k2l3m add feature
# m4n5o6p fix feature
# p7q8r9s update docs
```

### Organizando com Rebase Interativo

```bash
# 1. Iniciar rebase dos últimos 6 commits
git rebase -i HEAD~6

# 2. Editor abre com:
pick m4n5o6p add feature
pick p7q8r9s fix feature
pick j1k2l3m update docs
pick g7h8i9j add tests
pick d4e5f6g fix tests
pick a1b2c3d fix typo

# 3. Reorganizar e combinar commits
pick m4n5o6p add feature
squash p7q8r9s fix feature
pick j1k2l3m update docs
pick g7h8i9j add tests
squash d4e5f6g fix tests
squash a1b2c3d fix typo

# 4. Salvar e fechar editor

# 5. Editar mensagens dos commits combinados
# Primeiro commit:
feat: implementa funcionalidade de busca

# Segundo commit:
test: adiciona testes completos para busca

# 6. Ver resultado
git log --oneline

# Agora temos apenas 3 commits limpos:
# x1y2z3a test: adiciona testes completos para busca
# b4c5d6e docs: atualiza documentação
# f7g8h9i feat: implementa funcionalidade de busca
```

**Resultado**: Histórico limpo e profissional! ✨

---

## Exemplo 05: Desfazendo Alterações

**Cenário**: Você cometeu erros e precisa desfazer alterações de diferentes formas.

### Caso 1: Descartar Mudanças Não Commitadas

```bash
# Você modificou arquivo.js mas não quer manter as mudanças
git status
# modified:   arquivo.js

# Descartar mudanças
git restore arquivo.js
# ou
git checkout -- arquivo.js

# Verificar
git status
# nothing to commit, working tree clean
```

### Caso 2: Remover do Staging

```bash
# Você adicionou arquivo errado ao staging
git add senha.txt
git status
# Changes to be committed:
#   new file:   senha.txt

# Remover do staging (mantém arquivo)
git restore --staged senha.txt

# Adicionar ao .gitignore
echo "senha.txt" >> .gitignore
git add .gitignore
git commit -m "chore: adiciona senha.txt ao gitignore"
```

### Caso 3: Corrigir Último Commit

```bash
# Você esqueceu de adicionar um arquivo
git commit -m "feat: adiciona validação"

# Ops! Esqueceu validator.js
git add validator.js
git commit --amend --no-edit

# Ou alterar a mensagem
git commit --amend -m "feat: adiciona validação de formulários"
```

### Caso 4: Desfazer Último Commit (Mantendo Alterações)

```bash
# Commit feito na branch errada
git log --oneline
# abc123 feat: nova funcionalidade
# def456 fix: corrige bug

# Desfazer último commit
git reset --soft HEAD~1

# Alterações voltam para staging
git status
# Changes to be committed: ...

# Ir para branch correto
git stash
git checkout branch-correto
git stash pop
git commit -m "feat: nova funcionalidade"
```

### Caso 5: Reverter Commit Público

```bash
# Commit já foi enviado para remoto e compartilhado
git log --oneline
# xyz789 feat: funcionalidade com bug
# abc123 feat: funcionalidade anterior

# NUNCA use reset --hard em commits públicos!
# Use revert para criar novo commit que desfaz o anterior
git revert xyz789

# Mensagem automática:
# Revert "feat: funcionalidade com bug"
# This reverts commit xyz789...

git push origin main
```

**Resultado**: Problemas resolvidos de forma segura! 🔧

---

## Exemplo 06: Contribuindo para Projeto Open Source

**Cenário**: Você quer contribuir para um projeto open source no GitHub.

### Workflow Completo

```bash
# 1. Fork o projeto no GitHub (botão Fork)

# 2. Clonar SEU fork
git clone https://github.com/seu-usuario/projeto-open-source.git
cd projeto-open-source

# 3. Adicionar repositório original como upstream
git remote add upstream https://github.com/autor-original/projeto-open-source.git
git remote -v
# origin    https://github.com/seu-usuario/projeto-open-source.git
# upstream  https://github.com/autor-original/projeto-open-source.git

# 4. Criar branch para sua contribuição
git checkout -b fix/corrige-bug-123

# 5. Fazer as alterações
# ... editar arquivos ...
git add .
git commit -m "fix: corrige bug de validação #123"

# 6. Adicionar testes
# ... criar testes ...
git add .
git commit -m "test: adiciona testes para validação"

# 7. Antes de enviar, atualizar com upstream
git fetch upstream
git rebase upstream/main

# 8. Enviar para SEU fork
git push origin fix/corrige-bug-123

# 9. Criar Pull Request no GitHub
# - Ir até seu fork no GitHub
# - Clicar em "Compare & pull request"
# - Preencher descrição detalhada
# - Referenciar issue: "Fixes #123"

# 10. Após feedback, fazer ajustes
# ... fazer modificações ...
git add .
git commit -m "fix: ajusta validação conforme revisão"
git push origin fix/corrige-bug-123

# 11. Manter fork atualizado
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```

**Resultado**: Contribuição feita seguindo boas práticas! 🌟

---

## Exemplo 07: Hotfix em Produção

**Cenário**: Bug crítico em produção precisa ser corrigido urgentemente.

### Workflow de Hotfix

```bash
# 1. Situação: main está em produção, develop em desenvolvimento

# 2. Criar branch de hotfix a partir de main
git checkout main
git pull origin main
git checkout -b hotfix/corrige-erro-pagamento

# 3. Corrigir o bug
# Editar payment.js
git add payment.js
git commit -m "fix: corrige cálculo de juros no pagamento"

# 4. Adicionar teste
git add payment.test.js
git commit -m "test: adiciona teste para cálculo de juros"

# 5. Testar localmente
npm test

# 6. Merge em main
git checkout main
git merge hotfix/corrige-erro-pagamento
git tag -a v1.2.3 -m "Hotfix: correção cálculo pagamento"
git push origin main --tags

# 7. Merge em develop (para não perder o fix)
git checkout develop
git merge hotfix/corrige-erro-pagamento
git push origin develop

# 8. Deletar branch de hotfix
git branch -d hotfix/corrige-erro-pagamento

# 9. Deploy de main para produção
# ... processo de deploy ...
```

**Resultado**: Bug crítico corrigido rapidamente sem afetar desenvolvimento! 🚨

---

## Exemplo 08: Stash para Salvar Trabalho

**Cenário**: Você está no meio de um trabalho mas precisa mudar de contexto urgentemente.

### Usando Stash

```bash
# 1. Você está trabalhando em uma feature
git status
# modified:   feature.js
# modified:   style.css

# 2. Bug urgente! Precisa trocar de branch mas não quer commitar
git stash save "WIP: implementando dashboard"

# 3. Verificar que working directory está limpo
git status
# nothing to commit, working tree clean

# 4. Ir para outra branch corrigir bug
git checkout main
git checkout -b fix/bug-urgente

# 5. Corrigir e commitar
# ... correções ...
git add .
git commit -m "fix: corrige bug urgente"
git push origin fix/bug-urgente

# 6. Voltar para trabalho anterior
git checkout feature/dashboard
git stash list
# stash@{0}: On feature/dashboard: WIP: implementando dashboard

# 7. Recuperar trabalho
git stash pop
# ou
git stash apply stash@{0}  # mantém stash

# 8. Continuar trabalhando
git status
# modified:   feature.js
# modified:   style.css
```

### Stash Avançado

```bash
# Stash incluindo arquivos não rastreados
git stash -u

# Ver conteúdo do stash
git stash show
git stash show -p  # com diff completo

# Criar branch a partir de stash
git stash branch nova-branch-temp stash@{0}

# Limpar stashes antigos
git stash clear
```

**Resultado**: Contexto salvo e recuperado sem perder trabalho! 💾

---

## 🎯 Exercícios Práticos

### Exercício 1: Simulação Completa
1. Crie um repositório local
2. Faça 3 commits
3. Crie uma branch
4. Faça 2 commits na branch
5. Volte para main e faça merge
6. Visualize o histórico com `git log --graph`

### Exercício 2: Resolução de Conflitos
1. Crie duas branches a partir de main
2. Modifique o mesmo arquivo em ambas
3. Faça merge da primeira
4. Tente merge da segunda (conflito!)
5. Resolva o conflito manualmente

### Exercício 3: Rebase Interativo
1. Faça 5 commits pequenos
2. Use `git rebase -i HEAD~5`
3. Combine alguns commits com squash
4. Reordene os commits
5. Compare o histórico antes e depois

---

## 📚 Recursos Adicionais

- [Try Git](https://try.github.io/) - Tutorial interativo
- [Git Katas](https://github.com/eficode-academy/git-katas) - Exercícios práticos
- [Oh My Git!](https://ohmygit.org/) - Jogo para aprender Git

---

## 🤝 Contribuindo

Tem um exemplo prático interessante? Contribua com esta coleção!

1. Fork este repositório
2. Adicione seu exemplo em um novo arquivo
3. Envie um Pull Request

---

**Dica**: Execute esses exemplos em um repositório de teste para praticar sem medo! 🚀