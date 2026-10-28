# Convenções de Documentação

> Rascunho inicial — sujeito a ajustes conforme o projeto evolui.

## Tom e Estilo

- Estilo técnico e direto, sem emojis.
- Cada afirmação deve deixar claro o estado: planejado, implementado, testado ou verificado em produção.

## Estrutura

- Cabeçalhos com identificadores explícitos quando precisarem ser referenciados por âncora.
- Referências cruzadas usando caminhos relativos entre os arquivos `.md`.
- Imagens com texto alternativo descritivo; se tiverem dados importantes, repetir o conteúdo em texto ou tabela.

## Admonitions

Blocos de destaque (estilo Material for MkDocs) para sinalizar contexto especial:

```
!!! note "Nota"
    Contexto relevante.

!!! warning "Aviso"
    Restrições de seguranca ou atencao especial.

!!! info "Referencia"
    Detalhes tecnicos complementares.
```
