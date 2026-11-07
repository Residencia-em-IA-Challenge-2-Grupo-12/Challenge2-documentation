# Semana 2 — Base de Conhecimento e Mecanismo RAG

Esta secao documenta a especificacao tecnica, a arquitetura e as estrategias adotadas no desenvolvimento do primeiro mecanismo de **Retrieval-Augmented Generation (RAG)** do projeto Challenge 2 (Residencia em IA - Grupo 12).

---

## Objetivo da Entrega

Desenvolver um pipeline modular, funcional e auditavel de recuperacao e geracao fundamentada voltado para orientacao de estudos, combate a procrastinacao e preparacao tecnica por editais e trilhas autodidatas.

O foco central desta etapa consiste na **qualidade da recuperacao da informacao**, na **fundamentacao estrita das respostas (*grounding*)** e na **rastreabilidade das fontes citadas**, sem priorizacao de interfaces graficas de usuario nesta fase.

---

## Estado Geral da Implementacao

!!! info "Estado dos Componentes"
    - **Base de Conhecimento:** Implementado (20 artigos cientificos curados em `./data/`).
    - **Pipeline de Ingestao ETL:** Implementado e verificado no notebook de referencia.
    - **Segmentacao de Texto (Chunking):** Implementado e calibrado (`chunk_size=500`, `chunk_overlap=50`).
    - **Modelos de Embeddings:** Implementado com suporte a Google Gemini, OpenAI e fallback offline.
    - **Banco Vetorial (ChromaDB):** Implementado, persistido localmente e indexado sob a colecao `residencia_ia_semana2`.
    - **RAG Chain e Grounding:** Implementado via LCEL com prompt restritivo anti-alucinacao e testado com 3 personas reais.

---

## Sumario da Secao

A documentacao da Semana 2 esta dividida nos seguintes topicos:

1. [Arquitetura da Solucao](arquitetura.md): Visao geral dos fluxos de ingestao e inferencia, diagrama do sistema e especificacao de cada componente.
2. [Pipeline de Ingestao de Dados](pipeline-ingestao.md): Diagrama de sequencia do ETL, tratamento de excecoes por arquivo e indexacao idempotente.
3. [Estrategias de Chunking e Embeddings](estrategias-rag.md): Justificativas tecnicas dos parametros de segmentacao, modelos vetoriais suportados, metricas de similaridade e validacao pratica com as personas.

