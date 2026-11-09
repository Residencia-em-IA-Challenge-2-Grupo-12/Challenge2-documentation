# ADR-0002: ChromaDB local embutido como banco vetorial

- **Status:** Aceita (para a etapa atual do projeto)
- **Origem:** [Arquitetura da Solução](../mecanismo-rag/arquitetura.md), decisão arquitetural registrada; [Chunking e Embeddings](../mecanismo-rag/chunking-e-embeddings.md)

## Contexto

O mecanismo RAG precisa de armazenamento e busca vetorial. Nesta etapa o objetivo é um pipeline reproduzível localmente, sem custos adicionais de infraestrutura.

## Decisão

Usar o ChromaDB embutido no processo Python, persistido em `./chroma_db`, na coleção `residencia_ia_semana2`. O índice é HNSW com busca por distância euclidiana ($L_2$), e a recuperação usa `similarity_search_with_score` com `k = 3`.

## Alternativas Consideradas

- Servidores ou bancos vetoriais gerenciados em nuvem: descartados nesta etapa por exigirem infraestrutura e gerarem custo. Nenhum produto específico é citado na documentação.

## Consequências

- Execução totalmente local e sem custo de infraestrutura.
- Sem servidor separado, não há compartilhamento entre instâncias nem isolamento por usuário. A documentação não discute como isso se encaixaria em [RNF04](../requisitos/requisitos-nao-funcionais.md#RNF04) em um produto multiusuário.
- A reindexação é idempotente: o pipeline remove IDs já existentes antes de reinserir.
- O nome da coleção carrega o prefixo `semana2`; renomeá-la exige reindexar.
