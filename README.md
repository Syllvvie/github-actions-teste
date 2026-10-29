# github-actions-teste

> Trabalho 1 — DevOps Avançado: **Changelog e Release com GitHub Actions**

## 👥 Integrante

| Aluno | Nome completo |
|-------|---------------|
| Aluno | **Joao Vitor Paiva Borges / R.A: 250256** |

## 📌 Sobre o projeto

Este repositório demonstra o uso do **GitHub Actions** para automatizar duas tarefas comuns no ciclo de vida de um projeto:

1. **Documentar as alterações** (changelog) a partir das mensagens de commit;
2. **Publicar versões** do projeto na seção *Releases* do GitHub, com um pacote `.zip`.

O projeto em si é simples e serve apenas de base para os testes: contém um arquivo de exemplo (`Receita.md`, uma receita de miojo) e os workflows em `.github/workflows/`.

| Arquivo | Função |
|---------|--------|
| `.github/workflows/changelog.yml` | Action que atualiza o `CHANGELOG.md` |
| `CHANGELOG.md` | Histórico de alterações gerado automaticamente |
| `.github/workflows/zip-and-release.yml` | Action que gera o ZIP e publica a release |

## 📝 Action de Changelog

**Arquivo:** `.github/workflows/changelog.yml`  
**Disparo:** automático, a cada `push` na branch `main`.

Funcionamento:

1. O workflow faz o checkout do repositório com o histórico completo (`fetch-depth: 0`).
2. Lê o histórico com `git log` e monta uma lista com **data, mensagem do commit, autor e hash curto** de cada commit.
3. Gera (sobrescreve) o arquivo **`CHANGELOG.md`** com essa lista.
4. Faz um commit automático com o usuário `github-actions[bot]` e dá `push` na `main`.

O commit do bot contém `[skip ci]` na mensagem e é filtrado da listagem, evitando que o workflow dispare a si mesmo em loop.

> 💡 Para o changelog ficar legível, use mensagens de commit descritivas, de preferência no padrão *Conventional Commits* (`feat:`, `fix:`, `docs:` …).

## 🚀 Action de Release

**Arquivo:** `.github/workflows/zip-and-release.yml`  
**Disparo:** manual (`workflow_dispatch`), informando o número da versão.

Como publicar uma versão:

1. Acesse a aba **Actions** → workflow **Release Package** → **Run workflow**.
2. Informe a versão no campo de entrada (ex.: `v1.0.0`) e execute.
3. O workflow faz o checkout do código e gera o pacote `release.zip` com `git archive`, contendo os arquivos do projeto.
4. Em seguida, usa o GitHub CLI (`gh release create`) com o `GITHUB_TOKEN` para criar a release com a tag informada, anexar o `release.zip` e gerar as notas automaticamente (`--generate-notes`).
5. A versão aparece na seção **Releases** do repositório, com o ZIP disponível para download.

> ⚠️ Cada execução precisa de uma versão **nova** — o GitHub não permite criar duas releases com a mesma tag.

## ✅ Evidências de execução

- Histórico de execuções: aba **Actions** do repositório.
- Versões publicadas: seção **Releases** do repositório.
- Histórico de alterações gerado automaticamente: arquivo [`CHANGELOG.md`](CHANGELOG.md).
