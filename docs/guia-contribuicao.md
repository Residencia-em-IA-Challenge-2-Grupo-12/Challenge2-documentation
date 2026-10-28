# Guia de Contribuição

Este repositório é dedicado inteiramente à documentação do projeto Challenge 2 (Residência em IA - Grupo 12).

## Ambiente local

O projeto usa [uv](https://docs.astral.sh/uv/) para gerenciar dependências e o ambiente Python.

```bash
uv sync
uv run mkdocs serve
```

O comando acima sobe o site de documentação localmente com hot-reload.

## Branches

Crie uma branch a partir da `main` seguindo o padrão:

```
<tipo>/<descricao-curta>
```

Exemplo: `docs/guia-de-instalacao`

## Commits

As mensagens de commit devem seguir o padrão:

```
tipo: descricao
```

Tipos mais comuns:

- `docs`: alteração de conteúdo de documentação
- `chore`: tarefas de manutenção (configuração, dependências, etc.)
- `fix`: correção de erro em algum conteúdo ou configuração

## Pull Requests

- Abra o PR sempre contra a branch `main`.
- Descreva brevemente o que foi alterado e por quê.
- Garanta que o site builda sem erros antes de abrir o PR:

```bash
uv run mkdocs build --strict
```
