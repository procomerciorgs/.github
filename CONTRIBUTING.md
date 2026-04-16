# 🍽️ Etiqueta à Mesa — Padrões de Git da Equipe

> Guia de boas práticas para commits e branches. Leia com atenção antes de subir qualquer código. 🚀

---

## 📋 Sumário

- [Padrão de Commits](#-padrão-de-commits)
  - [Estrutura](#estrutura)
  - [Tipos de Commit](#tipos-de-commit)
  - [Escopo (obrigatório)](#escopo-obrigatório)
  - [Exemplos](#exemplos)
  - [Regras de Ouro](#regras-de-ouro)
- [Padrão de Branches](#-padrão-de-branches)
  - [Estrutura da Branch](#estrutura-da-branch)
  - [Tipos de Branch](#tipos-de-branch)
  - [Exemplos de Branches](#exemplos-de-branches)
  - [Regras para Branches](#regras-para-branches)
- [Fluxo de Trabalho](#-fluxo-de-trabalho)
- [Resumo Rápido](#-resumo-rápido)

---

## ✍️ Padrão de Commits

Seguimos a especificação [Conventional Commits](https://www.conventionalcommits.org/pt-br/v1.0.0-beta.4/). Todos os commits **devem ser escritos em português**.

### Estrutura

```
<tipo>(<escopo>): <descrição em português>

<corpo>

<rodapé>
```

> ⚠️ **Escopo, corpo e rodapé são obrigatórios** nesta equipe. Commits sem esses elementos serão recusados na revisão. A linha em branco separando cada bloco também é obrigatória.

### Tipos de Commit

| Tipo | Quando usar |
|---|---|
| `feat` | Adição de um novo recurso/funcionalidade |
| `fix` | Correção de um bug ou problema |
| `docs` | Alterações apenas em documentação |
| `style` | Formatação, ponto e vírgula, espaços — sem mudança de lógica |
| `refactor` | Refatoração de código que não corrige bug nem adiciona recurso |
| `perf` | Melhoria de performance |
| `test` | Adição ou correção de testes |
| `chore` | Tarefas de build, configs, dependências — sem mudança no código fonte |
| `improvement` | Melhoria em uma implementação existente, sem ser fix nem feat |

### Escopo (obrigatório)

O escopo fica entre parênteses logo após o tipo e indica **qual parte do sistema** foi afetada. Todo commit deve ter um escopo — ele ajuda a rastrear rapidamente onde uma mudança ocorreu.

```
feat(autenticação): adiciona login via Google
fix(carrinho): corrige cálculo de frete com desconto
```

### Exemplos

**Commit de nova funcionalidade:**
```
feat(autenticação): adiciona login via Google

Implementado o fluxo OAuth2 com o provedor Google para permitir
que usuários façam login sem precisar criar uma senha na plataforma.
Utilizada a biblioteca `passport-google-oauth20`.

closes #34
revisado-por: Carlos
```

**Commit de correção de bug:**
```
fix(checkout): corrige cálculo de desconto com frete grátis

O valor final estava sendo calculado incorretamente quando o usuário
aplicava um cupom de desconto junto com o benefício de frete grátis.
A função subtraía o frete duas vezes, gerando um valor negativo.

closes #87
revisado-por: Ana Paula
```

**Commit de refatoração:**
```
refactor(banco): reorganiza consultas do módulo de relatórios

As queries estavam duplicadas em três arquivos diferentes.
Centralizamos tudo no repositório de relatórios para facilitar manutenção
e evitar inconsistências futuras.

ref #102
revisado-por: João
```

**Commit com breaking change:**
```
feat(auth)!: altera formato do token de autenticação

O token agora é gerado em JWT ao invés de UUID simples, permitindo
carregar informações do usuário sem consultas adicionais ao banco.

BREAKING CHANGE: todos os clientes que consomem a API precisam atualizar
a lógica de leitura do token. Ver documentação em /docs/migracao-jwt.md.

closes #110
revisado-por: Mariana
```

**Commit de documentação:**
```
docs(readme): atualiza instruções de instalação para Node 20

As instruções anteriores eram para Node 16 e causavam erros na instalação
de dependências com versões mais recentes do npm.

ref #95
revisado-por: Pedro
```

### Regras de Ouro

1. **Escreva sempre em português** — a descrição, o corpo e o rodapé.
2. **Use o imperativo** na descrição: "adiciona", "corrige", "remove" — não "adicionado" ou "adicionando".
3. **Seja direto e objetivo** — a descrição deve caber em até 72 caracteres.
4. **Escopo é obrigatório** — sempre indique qual módulo ou área foi afetada.
5. **Corpo é obrigatório** — explique *o que* foi feito e *por que*, não apenas *como*.
6. **Rodapé é obrigatório** — referencie a issue relacionada (`closes #X`, `ref #X`) e o revisor (`revisado-por: Nome`).
7. **Separe os blocos com uma linha em branco** — descrição, corpo e rodapé devem ter uma linha vazia entre eles.
8. **Um commit, uma responsabilidade** — se precisar usar "e" para descrever o que fez, provavelmente são dois commits.
9. **Nunca suba código quebrado** — o repositório principal deve sempre estar funcional.
10. **Não faça commit de arquivos desnecessários** — configure o `.gitignore` corretamente.

---

## 🌿 Padrão de Branches

### Estrutura da Branch

```
<tipo>/<descrição-curta-em-kebab-case>
```

A descrição deve ser **curta, em minúsculas e com hífens** separando as palavras (kebab-case).

### Tipos de Branch

| Tipo | Finalidade |
|---|---|
| `main` | Código em produção — **nunca commite direto aqui** |
| `develop` | Branch de integração — base para novas features |
| `feature/` | Nova funcionalidade |
| `fix/` | Correção de bug em desenvolvimento |
| `hotfix/` | Correção urgente direto para produção |
| `release/` | Preparação de uma nova versão para deploy |
| `docs/` | Alterações apenas de documentação |
| `chore/` | Configurações, dependências, tarefas de infraestrutura |

### Exemplos de Branches

```
feature/tela-de-cadastro
feature/integracao-gateway-pagamento
fix/calculo-frete-desconto
fix/erro-login-usuario-inativo
hotfix/falha-critica-autenticacao
release/v1.2.0
docs/atualiza-guia-de-contribuicao
chore/atualiza-dependencias-outubro
```

### Regras para Branches

1. **Nunca commite direto na `main` ou `develop`** — sempre abra uma branch.
2. **Nomes em português são incentivados**, mas use kebab-case sem acentos ou caracteres especiais.
   - ✅ `feature/tela-de-recuperacao-de-senha`
   - ❌ `feature/TelaDeRecuperaçãoDeSenha`
3. **Uma branch, um objetivo** — branches muito antigas ou com muitas responsabilidades são difíceis de revisar.
4. **Delete a branch após o merge** — mantenha o repositório organizado.
5. **Nomeie de forma descritiva** — o nome da branch deve deixar claro o que está sendo feito sem precisar abrir o código.

---

## 🔄 Fluxo de Trabalho

```
main
 └── develop
      ├── feature/nova-funcionalidade   → merge em develop
      ├── fix/corrige-bug-x             → merge em develop
      └── release/v1.0.0               → merge em develop e main
           └── hotfix/falha-critica    → merge em main e develop
```

**Passo a passo para uma nova tarefa:**

1. Atualize sua `develop` local:
   ```bash
   git checkout develop
   git pull origin develop
   ```

2. Crie sua branch a partir da `develop`:
   ```bash
   git checkout -b feature/minha-funcionalidade
   ```

3. Desenvolva, faça commits frequentes e bem descritos.

4. Antes de abrir PR, sincronize com a `develop` novamente:
   ```bash
   git fetch origin
   git rebase origin/develop
   ```

5. Abra um **Pull Request** para `develop` e solicite revisão de ao menos um colega.

6. Após aprovação e merge, delete a branch.

---

## 📌 Resumo Rápido

```
✅ feat(cadastro): adiciona tela de cadastro de usuário

   Criada a tela com formulário de nome, e-mail e senha,
   com validação em tempo real nos campos obrigatórios.

   closes #21
   revisado-por: Ana

✅ fix(checkout): corrige valor total com cupom de desconto

   O desconto estava sendo aplicado antes do frete, causando
   divergência no valor exibido ao usuário.

   closes #58
   revisado-por: Carlos

❌ ajustes
❌ fix
❌ WIP
❌ commits finais
❌ arrumei o bug do login
❌ feat: adiciona tela     ← sem escopo, sem corpo, sem rodapé
```

---

> 💡 **Dúvidas?** Consulte a [especificação oficial do Conventional Commits](https://www.conventionalcommits.org/pt-br/v1.0.0-beta.4/) ou chame alguém da equipe. Ninguém nasce sabendo — mas todo mundo aprende! 😄