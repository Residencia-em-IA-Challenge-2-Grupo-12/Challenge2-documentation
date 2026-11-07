# Estrategias de Chunking, Embeddings e Banco Vetorial

Esta pagina documenta as decisoes de engenharia relacionadas a segmentacao de texto (*chunking*), selecao de modelos de *embeddings*, persistencia vetorial e a validacao empirica com as personas do projeto.

---

## 1. Estrategia de Segmentacao de Texto (Chunking)

### 1.1. Justificativa Tecnica
A segmentacao de documentos e obrigatoria para contornar dois gargalos criticos de sistemas RAG:
1. **Diluicao Semantica:** Documentos longos agrupam conceitos variados, gerando vetores de embedding que representam uma media imprecisa do texto.
2. **Precisao de Resposta:** Ao recuperar blocos curtos e focados, o LLM recebe estritamente a evidencia necessaria, reduzindo custos de inferencia e eliminando alucinacoes.

### 1.2. Algoritmo e Parametros

| Parametro | Valor | Justificativa |
| :--- | :---: | :--- |
| **Algoritmo** | `RecursiveCharacterTextSplitter` | Preserva a integridade sintatica de paragrafos e periodos atraves de separadores hierarquicos (`\n\n`, `\n`, `. `, ` `, `""`). [implementado] |
| **`chunk_size`** | **500 caracteres** | Tamanho calibrado para isolar proposicoes atomicas (cerca de 70 a 90 palavras em portugues). Permite separar o cabecalho de metadados do corpo cientifico do texto. [implementado] |
| **`chunk_overlap`** | **50 caracteres** (10%) | Margem de sobreposicao que previne a quebra abrupta de conceitos situados na borda entre dois fragmentos consecutivos. [implementado] |
| **Rastreamento** | `add_start_index = True` | Injeta o deslocamento inicial do caractere no documento original para possibilitar auditoria precisa. [implementado] |

---

## 2. Estrategia de Embeddings

### 2.1. Modelos Suportados e Configuracao
O pipeline implementa uma arquitetura agnostica a provedores, configurada via arquivo `.env`:

```
Provedor Ativo: Google Gemini ou OpenAI
Modelo Primario Google: models/text-embedding-004 (768 dimensoes)
Modelo Primario OpenAI: text-embedding-3-small (1536 dimensoes)
Fallback Local: LocalKeywordHashEmbeddings (256 dimensoes, offline)
```

- **Google Gemini (`text-embedding-004`):** Otimizado para recuperacao multilíngue, com alta sensibilidade para termos tecnicos e vocabulario academico em lingua portuguesa. [implementado]
- **OpenAI (`text-embedding-3-small`):** Alta capacidade discriminativa em benchmarks de *information retrieval*, permitindo recuperacao densa eficiente. [implementado]
- **Fallback Determinístico Local:** Classe proprietaria baseada em hashing de n-grams de palavras e normalizacao de norma Euclidiana ($L_2$). Garante a reprodutibilidade dos testes mesmo em ambientes isolados de rede. [implementado e testado]

### 2.2. Metrica de Espaco Vetorial
- **Metrica Adotada:** Distancia Euclidiana ($L_2$) no espaco vetorial gerado pelo ChromaDB.
- **Interpretacao:** Valores menores indicam maior proximidade e aderencia semantica entre o vetor da consulta e os vetores dos fragmentos indexados.

---

## 3. Implementacao do Banco Vetorial (ChromaDB)

- **Engine:** ChromaDB local embutido (*in-process*).
- **Persistencia:** Gravacao automatica dos indices vetoriais e metadados no diretorio `./chroma_db`. [implementado]
- **Nome da Colecao:** `residencia_ia_semana2`. [implementado]
- **Indice Vetorial:** Algoritmo HNSW (*Hierarchical Navigable Small World*), que permite buscas aproximadas em tempo sublinear $O(\log N)$.
- **Rotina de Idempotencia:** O pipeline executa `vectorstore.get(ids=...)` antes da gravacao, expurgando registros redundantes antes de efetuar novas insercoes. [implementado]

---

## 4. Validacao Pratica com as Personas

Para demonstrar a qualidade da recuperacao e a fidelidade da sintese, o sistema foi avaliado com consultas representativas das dores das tres personas do projeto:

### 4.1. Cenario 1 — Lucas (Profissional CLT / Dupla Jornada)
- **Consulta:** *"Trabalho 8 horas por dia e sinto que nao consigo render nos estudos. Como devo dividir meus blocos de estudo diarios e cuidar do sono?"*
- **Chunks Recuperados:**
  - `artigo_19_dual_burden_clt_martins.txt` (Pobreza de tempo na jornada CLT e blocos matinais protegidos).
  - `artigo_05_sleep_consolidation_walker.txt` (Consolidação da memoria durante o sono NREM e REM).
  - `artigo_09_timeboxing_allen.txt` (Metodologia de timeboxing com janelas fechadas de 60 a 90 minutos).
- **Resultado:** Resposta sintetizada estritamente fundamentada nos tres artigos citados, sem alucinacoes. [testado]

### 4.2. Cenario 2 — Lisa (Estudante / Overplanning e Procrastinacao)
- **Consulta:** *"Eu perco horas montando planilhas e cronogramas que sempre atrasam quando surge um imprevisto. Quais tecnicas praticas posso aplicar para evitar o overplanning?"*
- **Chunks Recuperados:**
  - `artigo_12_perfectionism_flett.txt` (Perfeccionismo clinico e paralisia por analise).
  - `artigo_17_study_cycle_perez.txt` (Ciclos de estudo flexíveis em fila sequencial vs. grades horarias rigidas).
  - `artigo_08_implementation_intentions_gollwitzer.txt` (Intencoes de implementacao 'se-entao').
- **Resultado:** Resposta fundamentada orientando a substituicao de grades rigidas por ciclos sequenciais e limitando o tempo de planejamento. [testado]

### 4.3. Cenario 3 — Bob (Universitario / Autodidata e Editais Extensos)
- **Consulta:** *"Como devo priorizar os topicos de um edital extenso ou trilha autodidata em vez de estudar tudo de forma linear, e como validar meu aprendizado pratico?"*
- **Chunks Recuperados:**
  - `artigo_16_pareto_engineering_box.txt` (Principio de Pareto 80/20 aplicado a ementas tecnicas).
  - `artigo_18_project_based_learning_thomas.txt` (Aprendizagem baseada em projetos e superacao do 'tutorial hell').
  - `artigo_20_feynman_technique_gleick.txt` (Tecnica Feynman para identificacao de lacunas conceituais).
- **Resultado:** Resposta priorizando os 20% de topicos de maior impacto e validacao por meio de prototipos funcionais. [testado]

---

!!! info "Fidelidade e Mitigacao de Alucinacoes"
    O prompt de sistema opera com temperatura zero (`temperature = 0.0`) e instrucao explicita de recusa para questoes fora do dominio ou nao cobertas pelo contexto recuperado.
