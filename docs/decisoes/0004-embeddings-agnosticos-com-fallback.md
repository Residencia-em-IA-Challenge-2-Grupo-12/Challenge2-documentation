# ADR-0004: Embeddings agnósticos a provedor, com fallback offline

- **Status:** Aceita
- **Origem:** [Chunking e Embeddings](../mecanismo-rag/chunking-e-embeddings.md); [Pipeline de Ingestão](../mecanismo-rag/pipeline-ingestao.md)

## Contexto

O pipeline precisa gerar embeddings em português técnico e, ao mesmo tempo, rodar em ambientes sem credenciais de API ou sem rede, para testes reproduzíveis.

## Decisão

Configurar o provedor via arquivo `.env`, com suporte a:

| Provedor | Modelo | Dimensões |
|:---|:---|:---:|
| Google Gemini | `models/text-embedding-004` | 768 |
| OpenAI | `text-embedding-3-small` | 1536 |
| Fallback local | `LocalKeywordHashEmbeddings` | 256 |

Sem credenciais ativas, o sistema aciona o fallback automaticamente: vetores determinísticos baseados em hashing de n-grams de palavras, com normalização $L_2$.

## Alternativas Consideradas

Outros modelos de embedding não são avaliados na documentação.

## Consequências

- Troca de provedor sem alterar o pipeline e testes de integração sem custo.
- Os vetores de provedores diferentes têm dimensões diferentes e não são comparáveis: trocar de provedor exige reindexar a coleção.
- O fallback é por palavras-chave, sem semântica densa. Ele serve a testes e execução local, não à avaliação de qualidade de recuperação.
- A escolha final do provedor (Gemini ou OpenAI) continua em aberto.
