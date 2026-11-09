# Métricas de Sucesso

!!! warning "Pendente"
    No documento-base, a pergunta central cita M-01 a M-03 e "M-Proteção-01/02", enquanto a tabela de métricas usa M-01 a M-04 e M-P01/M-P02, e três resultados esperados da categoria principal aparecem sem código visível. Esta página segue a numeração da tabela (M-01 a M-04 para as principais) e atribui os códigos na ordem em que aparecem; o grupo deve confirmar a numeração.

## Principais

| Código | Resultado esperado | Métrica e definição | Fonte de dados | Meta / referência |
|:---|:---|:---|:---|:---|
| M-01 | Adesão contínua e sustentável à rotina de estudos | **Taxa de Cumprimento do Ciclo Semanal:** % de sessões concluídas versus planejadas na semana ([RF02](../requisitos/requisitos-funcionais.md#RF02), [RF08](../requisitos/requisitos-funcionais.md#RF08)) | Logs de conclusão de sessões no banco de dados | ≥ 75% das sessões semanais concluídas |
| M-02 | Sincronia entre esforço cognitivo e ritmo biológico | **Taxa de Alinhamento Circadiano:** % de tópicos complexos executados no pico de alerta ([RF05](../requisitos/requisitos-funcionais.md#RF05), [RF07](../requisitos/requisitos-funcionais.md#RF07)) | Cruzamento entre horário de início da sessão e cronograma cadastrado | ≥ 70% das tarefas densas no pico de energia |
| M-03 | Recuperação autônoma de atrasos sem abandono | **Aceitação do Recálculo Automático:** % de cronogramas replanejados mantidos ou cumpridos ([RF09](../requisitos/requisitos-funcionais.md#RF09)) | Logs de acionamento do algoritmo de replanejamento | ≥ 80% de aceitação da nova distribuição |
| M-04 | Agilidade e utilidade na extração de conteúdo | **Conversão e SLA do RAG:** % de PDFs/imagens de até 50 páginas processados em ≤ 60 s que viram tarefas ([RF03](../requisitos/requisitos-funcionais.md#RF03), [RNF06](../requisitos/requisitos-nao-funcionais.md#RNF06)) | Telemetria de latência do backend e blocos criados | ≥ 95% dos arquivos em ≤ 60 s com ≥ 60% de conversão |

## Proteção

| Código | Resultado esperado | Métrica e definição | Fonte de dados | Meta / referência |
|:---|:---|:---|:---|:---|
| M-P01 | Preservação biológica do sono (evitar burnout) | **Índice de Invasão do Sono:** % de sessões de estudo que invadem a janela biológica de repouso programada | Horários de término das sessões registradas | < 5% de invasão da janela de sono |
| M-P02 | Experiência de uso sem fadiga de preenchimento | **Taxa de Abandono de Check-ins:** proporção de formulários de humor/energia ignorados ou fechados ([RF06](../requisitos/requisitos-funcionais.md#RF06)) | Logs de exibição versus envio dos check-ins diários | < 25% de rejeição dos prompts diários |

## Complementares

| Código | Resultado esperado | Métrica e definição | Fonte de dados | Meta / referência |
|:---|:---|:---|:---|:---|
| M-C01 | Monitoramento do estresse em semanas críticas | **Evolução do Humor/Energia:** média das notas de bem-estar nos 90 dias e pré-provas ([RF06](../requisitos/requisitos-funcionais.md#RF06), [RF13](../requisitos/requisitos-funcionais.md#RF13), [RNF07](../requisitos/requisitos-nao-funcionais.md#RNF07)) | Histórico de check-ins de bem-estar | Sem quedas abruptas de energia pré-provas |
| M-C02 | Retenção de longo prazo com revisão espaçada | **Adesão às Revisões Automáticas:** % de sessões de 1, 7 e 30 dias efetivamente concluídas ([RF15](../requisitos/requisitos-funcionais.md#RF15)) | Tabela de revisões automáticas no banco de dados | ≥ 65% de sessões de revisão cumpridas |
