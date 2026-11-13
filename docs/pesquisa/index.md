# Relatório Geral de Métricas do RAG

Consolidação de todas as avaliações do mecanismo RAG do StudyBuddy, das Semanas 2 e 3: o que foi medido, como foi medido, o que os números mostraram e qual configuração foi adotada. Esta página junta as duas linhas de avaliação da Semana 3 (modelos locais e API do Gemini) e explica por que elas chegam a vereditos diferentes.

!!! info "Estado (13/11/2026)"
    - **Implementado:** T3 + porteiro no notebook `pesquisa/rag_pipeline_semana2.ipynb` (`TOP_K=5`, prompt do notebook e porteiro de distância com τ = 1,026). Ver [Implementação](semana3/04-implementacao.md).
    - **Medido:** as duas avaliações da Semana 3 estão completas. A Ref (k=3) da avaliação local não foi reavaliada e usa os valores da rodada anterior.
    - **Deduzido, não medido:** a linha "T3 + porteiro", obtida a partir de T3 e T4 (ver [Resultados](semana3/02-resultados.md#t3--porteiro-a-combinação-que-sai-dos-dados)).
    - **Pendente:** conferência manual por amostragem do juiz de fidelidade à base.

## Resumo

| Pergunta | Resposta | Onde está a evidência |
| :-- | :-- | :-- |
| O RAG vale a pena? | Sim. Sem a base, o LLM nunca cita a fonte e cobre menos do que os artigos recomendam (completude 0,59 contra 0,91). As duas avaliações concordam | [Resultados › T1](semana3/02-resultados.md#t1--só-llm-o-que-muda-sem-a-base-e-por-quê), [Gemini §6.1](semana3/comparacao-api-gemini.md#61-o-que-muda-e-por-que-muda-entre-as-4-abordagens) |
| O LLM vale a pena? | Sim. Sem ele, o estudante recebe trechos crus e 4 das 10 perguntas fora de escopo recebem trechos sem relação. As duas avaliações concordam | [Resultados › T2](semana3/02-resultados.md#t2--só-busca-o-que-muda-sem-o-llm-e-por-quê) |
| Qual embedding e qual chunking? | `bge-m3`, chunk de 500 com overlap de 50 | [Cap. 5](05-resultados.md#conclusão) |
| Quantos trechos (k)? | k=5. A completude multi-fonte sobe de 0,53 para 0,87. As duas avaliações concordam | [Cap. 8](08-geracao-e-llm-juiz.md#comparação-k3--k5) |
| Qual configuração o produto adota? | **T3 + porteiro**: RAG + LLM com k=5, o prompt do notebook e o porteiro de distância | [Decisão](semana3/03-decisao.md) |
| O que fica em aberto? | O prompt CoT e o re-rank de cabeçalhos da avaliação via Gemini não foram medidos com o gabarito de 28 perguntas | [Onde as avaliações divergem](#onde-as-avaliações-divergem) |

## Linha do tempo das avaliações

| Etapa | O que mudou | Conjunto de avaliação | Resultado principal | Relatório |
| :-- | :-- | :-- | :-- | :-- |
| Semana 2 · baseline | Pipeline RAG inicial: chunk 500 / 50, k=3, distância L2, prompt com *grounding* | — | Ponto de partida das medições | [Mecanismo RAG](../mecanismo-rag/index.md) |
| Camada de busca | Embedding offline x `bge-m3` x `qwen3-embedding`; chunks 300, 500 e 800 | Gabarito de 20 perguntas, depois 28 (18 em escopo e 10 fora), mais 36 paráfrases | `bge-m3` com chunk 500. Hit@3 vai de 0,44 (offline) para 1,00 | [Cap. 5](05-resultados.md), [Cap. 7](07-metricas-da-busca.md) |
| Camada de geração | Gerador `qwen3:8b` e juiz `gemma3:12b`; k=3 x k=5 | 28 perguntas | k=5: completude de 0,81 para 0,91, fidelidade de 0,93 para 0,97 | [Cap. 8](08-geracao-e-llm-juiz.md) |
| Semana 3 · local | 4 testes (só LLM, só busca, RAG + LLM, RAG + LLM otimizado) e métricas novas | 28 perguntas, juiz `gemma3:12b` | T3 tem a maior taxa de sucesso (0,93). O porteiro do T4 fica; o prompt do T4 não | [Semana 3](semana3/index.md) |
| Semana 3 · API | Os mesmos 4 rótulos, com o Gemini e o RAGAS | 3 perguntas de personas (12 execuções) | O T4 (k=5 + re-rank + CoT) tem as maiores médias de RAGAS | [Avaliação via Gemini](semana3/comparacao-api-gemini.md) |

## Como foi avaliado

### As duas metodologias da Semana 3

| | Avaliação local | Avaliação via API do Gemini |
| :-- | :-- | :-- |
| Gerador | `qwen3:8b` (Ollama), `temperature=0` | Configurado `gemini-2.5-flash`; o endpoint ativo na execução foi `gemini-3.5-flash-lite`, `temperature=0` |
| Embedding | `bge-m3` | Configurado `text-embedding-004`; o endpoint ativo foi `gemini-embedding-001` |
| Base | 20 artigos, 49 chunks (500 / 50), ChromaDB L2 | A mesma |
| Juiz | `gemma3:12b`, outra família e maior que o gerador; prompts próprios calibrados em 3 versões | RAGAS (*LLM-as-a-judge*) |
| Perguntas | 28: 4 diretas, 9 parafraseadas, 5 multi-fonte e 10 fora de escopo, com pontos-chave no gabarito | 3: uma por persona (Lucas, Lisa e Bob). São a q02, a q03 e a q04 do gabarito |
| Fora de escopo | 10 perguntas, 4 delas "perigosas" (próximas do tema) | Nenhuma |
| T3 | RAG + LLM com k=5 | RAG com k=3 (o baseline da Semana 2) |
| T4 | k=5 + porteiro de distância + prompt revisado | k=5 com re-rank de cabeçalhos (busca 6 e prioriza chunks com mais de 225 caracteres) + prompt CoT |
| Ambiente | Apple M4, 24 GB, custo de API igual a zero | API do Google, custo por token |

!!! warning "Os rótulos T3 e T4 não significam a mesma coisa nas duas avaliações"
    No T3, só o k é diferente (5 no local, 3 no Gemini). No T4, as duas otimizações são diferentes. Por isso, um "T4 vence" de uma avaliação não contradiz diretamente o "T4 perde" da outra: são testes diferentes.

### Métricas por camada

**Busca** (a recuperação trouxe os trechos certos?). Detalhes e matemática no [Cap. 7](07-metricas-da-busca.md):

| Grupo | Métricas |
| :-- | :-- |
| Acerto e ranking | Hit@k, MRR@k, nDCG@k |
| Ruído e cobertura | Precision@k, Recall@k, cobertura de pontos-chave@k, distintas@k |
| Recusa pela distância | Margem, AUC-ROC da distância, recusa sem erro |
| Robustez | Hit@3 com erros de digitação, Hit@3 e MRR em paráfrases, Jaccard entre a original e as paráfrases |
| Custo | Latência p50 e p95, tokens de contexto@k, tempo de indexação e tamanho dos vetores |

**Geração** (o LLM usou bem o que recebeu?). Detalhes no [Cap. 8](08-geracao-e-llm-juiz.md#métricas-da-geração) e na [Metodologia da Semana 3](semana3/01-metodologia.md#métricas):

| Métrica | Quem mede | O que responde |
| :-- | :-- | :-- |
| Recusa correta / indevida | Regra | Recusou quando devia? Recusou sem motivo? |
| Tem citação, precisão da citação, cita artigo correto | Regra | O estudante consegue conferir a fonte? |
| Fidelidade ao contexto | Juiz | O que foi dito está nos trechos recebidos? |
| Fidelidade à base (nova na Semana 3) | Juiz, uma afirmação por chamada | O que foi dito está em algum dos 20 artigos? Vale para os 4 testes |
| Completude | Juiz, com evidência | A resposta cobre os pontos-chave do gabarito? |
| Relevância (nova na Semana 3) | Juiz | Cada afirmação ajuda a responder a pergunta? |
| Taxa de sucesso (nova na Semana 3) | Regra sobre as anteriores | Recusou quando era fora de escopo; ou respondeu com completude ≥ 2/3, fidelidade à base ≥ 0,8 e citação correta |
| Custo | Medição | Latência p50, tokens e chamadas ao LLM |

**RAGAS** (avaliação via Gemini):

| Métrica | O que responde |
| :-- | :-- |
| Faithfulness | Proporção das afirmações da resposta sustentadas pelos chunks recuperados |
| Answer Relevance | Quanto a resposta é semanticamente aderente à pergunta |
| Context Precision | Se os chunks relevantes para o *ground truth* ficaram no topo do ranking |

### Equivalência entre as métricas

| Avaliação local | RAGAS | Comparáveis? |
| :-- | :-- | :-- |
| Fidelidade ao contexto | Faithfulness | Medem a mesma ideia, com juízes e prompts diferentes. No T1 (sem contexto), o RAGAS dá 0 por construção. A avaliação local criou a fidelidade à base justamente para medir o T1 |
| Relevância | Answer Relevance | Parecidas na intenção. A local julga afirmação por afirmação; o RAGAS usa similaridade semântica |
| Cobertura de pontos-chave, nDCG | Context Precision | Parcialmente: as duas olham a qualidade da recuperação, com réguas diferentes |
| Completude | Sem equivalente | Só a local mede se a resposta cobre o que o gabarito espera |
| Recusa correta / indevida | Sem equivalente | Só a local tem perguntas fora de escopo |
| Citação | Sem equivalente | Só a local mede |

!!! note "Escalas"
    Os números das duas avaliações **não devem ser comparados diretamente** (por exemplo, a fidelidade 0,98 local contra a Faithfulness 0,81 do Gemini). Os juízes, os prompts e os geradores são diferentes. O que se compara é a **direção** dentro de cada avaliação: qual teste sobe e qual cai.

## Resultados

### Camada de busca

Configuração do notebook (chunk 500, k=3), [Cap. 5](05-resultados.md):

| Modelo | Hit@1 | Hit@3 | MRR@3 | Recall@3 | Margem de recusa | Latência p50 |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| Offline (hash) | 0,39 | 0,44 | 0,42 | 0,41 | — | — |
| **bge-m3** | **0,94** | **1,00** | **0,97** | **0,93** | +0,014 | 126 ms |
| qwen3-embedding:0.6b | 0,94 | 0,94 | 0,94 | 0,84 | **+0,071** | **91 ms** |

- O chunk 500 é o ponto eficiente: cobertura por 100 tokens de 0,29, contra 0,16 do chunk 800.
- O ponto fraco são as perguntas multi-fonte: a cobertura@3 é de 0,53, e com k=5 a cobertura geral sobe para 0,89.
- O `qwen3-embedding` tem 5 vezes mais margem para recusar pela distância e ordena melhor as paráfrases (MRR 0,76 contra 0,68), mas foi testado sem o prefixo de instrução recomendado.

### Camada de geração: k=3 x k=5

Gerador `qwen3:8b` e juiz `gemma3:12b`, 28 perguntas, [Cap. 8](08-geracao-e-llm-juiz.md#comparação-k3--k5):

| Métrica | k=3 | k=5 |
| :-- | :-- | :-- |
| Recusa correta / indevida | 100% / 0% | 100% / 0% |
| Citação (tem / precisão / cita a esperada) | 100% / 100% / 100% | 100% / 100% / 100% |
| Fidelidade | 0,93 | **0,97** |
| Completude | 0,81 | **0,91** |
| Completude multi-fonte | 0,53 | **0,87** |

### Semana 3 · avaliação local (28 perguntas)

[Resultados completos](semana3/02-resultados.md#visão-geral):

| Métrica | T1 · Só LLM | T2 · Só busca | T3 · RAG + LLM | T4 · otimizado |
| :-- | :-- | :-- | :-- | :-- |
| **Taxa de sucesso** | 0,32 | 0,75 | **0,93** | 0,75 |
| Completude | 0,59 | 0,87 | **0,91** | 0,76 |
| Fidelidade à base | 0,81 (teto) | 1,00 (por construção) | 0,98 (piso) | 0,86 (piso) |
| Relevância | 0,89 | 0,57 | **0,94** | 0,79 |
| Cita um artigo correto | 0,00 | 1,00 | 1,00 | 1,00 |
| Recusa correta (10) | 0,90 | 0,60 | 1,00 | 1,00 |
| Recusa indevida (18) | 0,00 | 0,00 | 0,00 | 0,00 |
| Latência p50 das respostas | 54,7 s | 0,1 s | 20,8 s | 29,3 s |
| Chamadas ao LLM (de 28) | 28 | 0 | 28 | **22** |

### Semana 3 · avaliação via API do Gemini (3 personas)

Médias das 3 personas, [§4 do relatório](semana3/comparacao-api-gemini.md#4-resumo-executivo-médias-por-abordagem-arquitetural):

| Métrica | T1 · Vanilla LLM | T2 · Busca pura | T3 · RAG k=3 | T4 · k=5 + re-rank + CoT |
| :-- | :-- | :-- | :-- | :-- |
| Faithfulness | 0,00 | 1,00 | 0,78 | **0,81** |
| Answer Relevance | **0,85** | 0,71 | 0,80 | 0,80 |
| Context Precision | 0,00 | 0,67 | 0,67 | **0,73** |
| Latência média | 6,5 s | 0,5 s | 4,5 s | 5,4 s |
| Tokens estimados | 288 | 37 | 648 | 827 |

### Onde as avaliações concordam

1. **RAG é melhor que LLM puro.** Sem a base, nenhuma resposta é rastreável (citação 0,00 no local, Faithfulness 0 no Gemini), e o LLM responde com conselhos genéricos em vez do que os artigos recomendam. Exemplo das duas avaliações: para quem trabalha 8 horas, o LLM puro não menciona os micro-blocos matinais de 45 a 60 minutos do artigo 19.
2. **Busca pura não serve como produto principal.** Ela é a mais rápida e é fiel por construção, mas tem a menor relevância nas duas avaliações (0,57 no local, 0,71 no Gemini) e transfere a síntese para o estudante. No local, ela ainda entrega trechos sem relação para 4 das 10 perguntas fora de escopo, inclusive uma sobre remédio para dormir. Ela fica como modo de contingência, para quando o LLM estiver fora do ar.
3. **k=5 é melhor que k=3.** No local, a completude multi-fonte sobe de 0,53 para 0,87. No Gemini, a Context Precision sobe de 0,67 para 0,73. As perguntas das personas são compostas, e o k=3 deixa artigos complementares de fora.

## Onde as avaliações divergem

A avaliação via Gemini escolhe o seu T4. A avaliação local rejeita o prompt do seu T4 e escolhe o T3 + porteiro. A divergência tem três causas.

**1. Testes diferentes com o mesmo nome.** O T4 do Gemini compara k=5 + re-rank + CoT contra um T3 com **k=3**. Parte do ganho dele é o próprio k=5, que a avaliação local já tinha medido e adotado no T3 dela. O T4 local compara contra um T3 que **já usa k=5**, e a única diferença em escopo é o prompt.

**2. Tamanho da amostra.** Com 3 perguntas, o T4 do Gemini ganha do T3 por diferenças movidas por uma persona: a Faithfulness da Lisa vai de 0,43 para 0,60, enquanto a do Lucas cai de 1,00 para 0,83. A avaliação local mostra o mesmo efeito. Restrita às mesmas 3 perguntas (q02, q03 e q04), o T3 e o T4 locais **empatam**:

| q02, q03 e q04 (avaliação local) | T1 | T2 | T3 | T4 |
| :-- | :-- | :-- | :-- | :-- |
| Completude média | 0,56 | 0,89 | 0,78 | 0,78 |
| Fidelidade à base média | 0,57 | 1,00 | 0,94 | 0,93 |
| Sucesso | 0 de 3 | 3 de 3 | 3 de 3 | 3 de 3 |

Valores por pergunta tirados de `pesquisa/metricas/relatorios/comparacao_<teste>.json`.

O prejuízo do prompt revisado só aparece nas 18 perguntas em escopo: 2 perguntas melhoram e 8 pioram, e a completude cai de 0,91 para 0,76. Com 3 perguntas, nenhuma das duas avaliações teria visto isso.

**3. O que cada uma mede.** Só a avaliação local tem perguntas fora de escopo e mede completude contra pontos-chave. São justamente as medidas que separam o T3 do T4 local (a completude) e que justificam o porteiro (a recusa). O RAGAS não mede nenhuma das duas.

!!! note "Por que a decisão segue a avaliação local"
    Ela tem mais perguntas (28 contra 3), um gabarito com pontos-chave, perguntas fora de escopo, um juiz calibrado em três versões e testes que mudam uma peça por vez. A avaliação via Gemini confirma as direções gerais (RAG > LLM puro, LLM > busca crua, k=5 > k=3), mas não tem poder para decidir entre as otimizações do T4.

**O que a avaliação via Gemini deixa como hipótese**, ainda não medida com o gabarito:

- **Re-rank de cabeçalhos**: priorizar chunks com mais de 225 caracteres. Ele ataca os chunks que só têm título e autores, um problema que a avaliação local não testou.
- **Prompt CoT**: é diferente do prompt revisado do T4 local, que fez a completude cair.
- **Gerador via API**: já estava nos próximos passos da avaliação local.

## Decisão final

**T3 + porteiro**, implementado no notebook em 13/11 ([Implementação](semana3/04-implementacao.md)):

| | Valor |
| :-- | :-- |
| Busca | `bge-m3`, chunk 500 / overlap 50, k=5 |
| Porteiro | Recusa sem chamar o LLM quando d₁ > 1,026, calibrado nas 36 paráfrases (não no gabarito de teste). Só fica ligado com o `bge-m3` |
| Prompt | O do notebook (`RAG_SYSTEM_PROMPT`), sem as regras do T4 |
| Resultado esperado | Completude 0,91, fidelidade 0,98, recusa 10/10, recusa indevida 0/18, citação correta em 100%, 6 de 10 recusas em 0,14 s sem LLM |
| Modo de contingência | Busca pura (T2) com o porteiro, quando o LLM estiver indisponível |

## Próximos passos

1. Atacar a diluição da q12 com uma mudança menor no prompt (só a regra 3 do T4) e medir nas 18 perguntas.
2. Medir o **re-rank de cabeçalhos** e o **prompt CoT** da avaliação via Gemini com o gabarito de 28 perguntas e o mesmo juiz.
3. Testar um gerador via API (Gemini ou GPT-4o-mini) com o mesmo gabarito, para comparar completude, latência e custo.
4. Fazer a conferência manual do juiz de fidelidade à base, por amostragem.
5. Recalibrar o porteiro com perguntas reais de estudantes, quando houver uso.
6. Repetir o `qwen3-embedding` com o prefixo de instrução.
7. Medir a latência de T3 e T4 na mesma sessão.

## Limitações

- **Amostras pequenas.** No local, são 18 perguntas em escopo (cada uma vale ~0,056), e diferenças menores que ~0,1 podem ser ruído. O T3 contra o T4 dá p ≈ 0,11 no teste do sinal. No Gemini, são 3 perguntas, sem nenhuma inferência possível.
- **Gabarito escrito pelo time.** As perguntas, as paráfrases, os pontos-chave e as fontes esperadas foram definidos por julgamento.
- **Juízes também erram.** A fidelidade à base (v3) tende a ser generosa em afirmações vagas, o que favorece o T1. O juiz varia ±0,1 a 0,2 numa mesma pergunta. O RAGAS não foi calibrado contra conferência manual.
- **Modelos da avaliação via Gemini.** Os modelos executados (`gemini-3.5-flash-lite`, `gemini-embedding-001`) não são os configurados no código, o que dificulta reproduzir a avaliação.
- **Porteiro dependente do embedding.** O corte de 1,026 só vale para o `bge-m3`, porque cada modelo mede distância numa escala própria.
- **Tudo local na avaliação principal.** Os resultados valem para `qwen3:8b` + `gemma3:12b` num Apple M4.

## Mapa de leitura

| Quero entender... | Ler |
| :-- | :-- |
| O que é RAG e o que são métricas | [Fundamentos](fundamentos.md), caps. [1](01-o-que-e-rag.md) a [4](04-exemplo-simples.md) |
| A matemática de cada métrica de busca | [Cap. 7 · Métricas da busca](07-metricas-da-busca.md) |
| A escolha do embedding e do chunking | [Cap. 5 · Resultados](05-resultados.md) |
| O LLM-juiz e o k=3 x k=5 | [Cap. 8 · Geração e LLM-juiz](08-geracao-e-llm-juiz.md) |
| Como testar à mão | [Cap. 6 · Teste manual](06-teste-manual.md) |
| Os 4 testes da Semana 3 (local) | [Metodologia](semana3/01-metodologia.md), [Resultados](semana3/02-resultados.md), [Decisão](semana3/03-decisao.md) e [Implementação](semana3/04-implementacao.md) |
| A avaliação via API do Gemini | [Avaliação via Gemini](semana3/comparacao-api-gemini.md) |
