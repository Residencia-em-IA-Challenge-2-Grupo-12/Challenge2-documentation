# ADR-0003: Segmentação recursiva com `chunk_size=500` e `chunk_overlap=50`

- **Status:** Aceita
- **Origem:** [Chunking e Embeddings](../mecanismo-rag/chunking-e-embeddings.md); [Pipeline de Ingestão](../mecanismo-rag/pipeline-ingestao.md)

## Contexto

Documentos longos geram embeddings que representam uma média imprecisa do texto (diluição semântica). Blocos curtos e focados entregam ao LLM só a evidência necessária.

## Decisão

Usar `RecursiveCharacterTextSplitter` com:

- `chunk_size = 500` caracteres, para isolar proposições atômicas (cerca de 70 a 90 palavras em português) e separar o cabeçalho de metadados do corpo do texto;
- `chunk_overlap = 50` caracteres (10%), para não quebrar conceitos na borda entre fragmentos;
- separadores hierárquicos `["\n\n", "\n", ". ", " ", ""]`;
- `add_start_index = True`, guardando a posição inicial de cada fragmento para auditoria.

## Alternativas Consideradas

Outros tamanhos e estratégias de segmentação não são comparados na documentação.

## Consequências

- Fragmentos focados favorecem precisão da resposta e menor custo de inferência.
- Cada fragmento carrega `source`, `chunk_id`, `topic` e `start_index`, o que sustenta a rastreabilidade da [ADR-0005](0005-grounding-estrito-e-rastreabilidade.md).
- Trechos curtos podem separar uma ideia de seu contexto; a documentação não traz medição desse efeito. Os valores foram validados apenas com três consultas das personas.
