# ADR-0001: RAG com ingestão offline separada da consulta online

- **Status:** Aceita
- **Origem:** [Arquitetura da Solução](../mecanismo-rag/arquitetura.md); [RF03](../requisitos/requisitos-funcionais.md#RF03)

## Contexto

O projeto precisa extrair conteúdo de materiais e responder de forma fundamentada, com foco em qualidade da recuperação, grounding e rastreabilidade das fontes, sem priorizar interface gráfica nesta fase.

## Decisão

Separar o mecanismo RAG em dois pipelines independentes:

1. **Ingestão (offline/batch):** leitura de documentos, normalização, segmentação, enriquecimento de metadados, cálculo de embeddings e gravação no banco vetorial.
2. **Recuperação e inferência (online):** conversão da pergunta em vetor, recuperação dos fragmentos mais próximos, montagem de prompt com regras de grounding e geração da resposta pelo LLM.

A orquestração da consulta usa LangChain Expression Language (LCEL), com `RunnableParallel` para manter pergunta e documentos recuperados na saída.

## Alternativas Consideradas

Não registradas na documentação.

## Consequências

- A indexação pode ser repetida sem afetar a consulta; por isso a ingestão é idempotente e tolerante a falhas por arquivo (ver [Pipeline de Ingestão](../mecanismo-rag/pipeline-ingestao.md)).
- A base só reflete novos documentos depois de uma nova execução da ingestão.
- A ingestão atual cobre `.txt` e `.pdf` locais; o upload de PDFs e imagens pelo usuário (RF03) e o SLA de 60 s (RNF06) ainda não estão resolvidos por esta arquitetura.
