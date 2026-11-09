# Decisões de Arquitetura (ADR)

Registro das decisões técnicas relevantes do projeto, no formato ADR (*Architecture Decision Record*): contexto, decisão, alternativas e consequências.

As ADRs abaixo foram escritas a partir da documentação já existente (mecanismo RAG, requisitos e premissas do [pacote de definição](../definicao-do-projeto/index.md)). Nenhuma decisão foi inventada: quando a documentação não registra uma alternativa ou uma consequência, isso é dito na própria ADR.

## Índice

| ADR | Decisão | Status |
|:---|:---|:---|
| [ADR-0001](0001-rag-em-dois-pipelines.md) | RAG com ingestão offline separada da consulta online | Aceita |
| [ADR-0002](0002-chromadb-local.md) | ChromaDB local embutido como banco vetorial | Aceita |
| [ADR-0003](0003-chunking-recursivo-500-50.md) | Segmentação recursiva com `chunk_size=500` e `chunk_overlap=50` | Aceita |
| [ADR-0004](0004-embeddings-agnosticos-com-fallback.md) | Embeddings agnósticos a provedor, com fallback offline | Aceita |
| [ADR-0005](0005-grounding-estrito-e-rastreabilidade.md) | Respostas com grounding estrito e rastreabilidade de fontes | Aceita |
| [ADR-0006](0006-aplicacao-web-responsiva.md) | Aplicação web responsiva com Web Push | Aceita, com ponto em aberto |
| [ADR-0007](0007-google-calendar-somente-leitura.md) | Integração com Google Calendar somente leitura | Aceita |

## Status possíveis

- **Proposta:** em discussão, ainda não validada pelo grupo.
- **Aceita:** adotada e refletida na documentação ou no código.
- **Substituída:** trocada por outra ADR (indicar qual).

## Como criar uma nova ADR

1. Copie o [modelo](modelo.md) para `NNNN-titulo-curto.md`, com o próximo número da sequência.
2. Preencha todas as seções; use "não registrado" quando a informação não existir em vez de supor.
3. Inclua a ADR no índice acima e no `nav` do `mkdocs.yml`.
4. Não apague ADRs antigas: marque como **Substituída** e aponte a nova.

## Decisões ainda sem ADR

Os documentos atuais não trazem base suficiente para registrar:

- stack de backend e frontend, autenticação (RF01) e banco relacional;
- hospedagem e processamento assíncrono de PDFs (o SLA de 60 s está em [RNF06](../requisitos/requisitos-nao-funcionais.md#RNF06), mas sem solução definida);
- escolha final do LLM e do provedor de embeddings (hoje a documentação lista Gemini e OpenAI como opções);
- algoritmo de geração e recálculo de cronograma (RF04, RF09), do qual só se sabe, pelo mapa de [stakeholders](../definicao-do-projeto/stakeholders.md), que o escopo pede um algoritmo determinístico simples.
