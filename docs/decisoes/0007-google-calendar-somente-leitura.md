# ADR-0007: Integração com Google Calendar somente leitura

- **Status:** Aceita
- **Origem:** [RNF09](../requisitos/requisitos-nao-funcionais.md#RNF09); premissa [P-07](../definicao-do-projeto/premissas-e-riscos.md#P-07); [Stakeholders](../definicao-do-projeto/stakeholders.md) (DPO / Jurídico)

## Contexto

O planejamento deve considerar compromissos externos do usuário, mas o produto lida com dados de rotina, sono e compromissos sujeitos à LGPD ([RNF08](../requisitos/requisitos-nao-funcionais.md#RNF08)) e ao risco [R-06](../definicao-do-projeto/premissas-e-riscos.md#R-06) (exposição de dados pessoais).

## Decisão

A integração com o Google Calendar usa apenas permissão de leitura, com sincronização unidirecional (da agenda do Google para o produto), mediante autorização do usuário.

## Alternativas Consideradas

- Sincronização bidirecional ou permissão de escrita: não adotada, para não conceder permissões desnecessárias sobre a agenda pessoal.

## Consequências

- Menor superfície de risco e alinhamento com a minimização de dados.
- O produto não cria nem altera eventos no Google Calendar; o cronograma gerado fica só no produto (e na exportação para impressão, [RF11](../requisitos/requisitos-funcionais.md#RF11)).
- O escopo de leitura e a sincronização unidirecional ainda precisam ser validados contra a API (premissa P-07).
