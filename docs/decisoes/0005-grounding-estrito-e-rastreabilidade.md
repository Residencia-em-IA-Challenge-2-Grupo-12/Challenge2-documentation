# ADR-0005: Respostas com grounding estrito e rastreabilidade de fontes

- **Status:** Aceita
- **Origem:** [Arquitetura da Solução](../mecanismo-rag/arquitetura.md); [Chunking e Embeddings](../mecanismo-rag/chunking-e-embeddings.md); [RF03](../requisitos/requisitos-funcionais.md#RF03); stakeholder Engenharia de IA ("pipeline de RAG sem alucinação")

## Contexto

As respostas orientam decisões de estudo do usuário. O risco [R-02](../definicao-do-projeto/premissas-e-riscos.md#R-02) (planejamento inadequado por erros nas recomendações) e [R-07](../definicao-do-projeto/premissas-e-riscos.md#R-07) (confiança excessiva) pedem respostas verificáveis.

## Decisão

- Prompt de sistema restritivo, com temperatura `0.0`.
- Se o contexto recuperado for insuficiente, o modelo responde com a recusa padrão: "Não encontrei informações suficientes na base de conhecimento para responder a essa pergunta."
- O contexto injetado no prompt traz `[Fonte: <arquivo> | Chunk ID: <id>]` em cada trecho, de modo que cada afirmação possa ser conferida no documento de origem.

## Alternativas Consideradas

Não registradas na documentação.

## Consequências

- Respostas auditáveis e menor chance de alucinação; a validação com Lucas, Lisa e Bob mostrou respostas apoiadas nos artigos recuperados.
- Perguntas fora da base recebem recusa em vez de resposta genérica, o que pode frustrar o usuário (ver [R-04](../definicao-do-projeto/premissas-e-riscos.md#R-04) e GQ-11).
- A validação cobre apenas três consultas; não há métrica formal de fidelidade. O LLM final (Gemini 2.5 Flash ou GPT-4o-mini) continua em aberto.
