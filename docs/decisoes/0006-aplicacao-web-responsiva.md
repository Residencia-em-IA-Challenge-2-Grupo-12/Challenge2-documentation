# ADR-0006: Aplicação web responsiva com Web Push

- **Status:** Aceita, com ponto em aberto (widget, RNF03)
- **Origem:** [RNF10](../requisitos/requisitos-nao-funcionais.md#RNF10), [RNF11](../requisitos/requisitos-nao-funcionais.md#RNF11), [RNF03](../requisitos/requisitos-nao-funcionais.md#RNF03); premissa [P-02](../definicao-do-projeto/premissas-e-riscos.md#P-02)

## Contexto

O público (graduação e concursos) usa computador, tablet e smartphone. O pacote de definição parte da premissa de que os usuários têm acesso à internet e a um navegador compatível.

## Decisão

Entregar o produto como aplicação web responsiva, usável em navegadores de desktop e de dispositivos móveis. Lembretes e notificações usam Web Push no navegador, após permissão do usuário.

## Alternativas Consideradas

- Aplicativo nativo: não é citado como alternativa avaliada; o pacote apenas estabelece o sistema como web responsivo.

## Consequências

- Uma única base de código atende desktop e mobile.
- As notificações dependem de o usuário conceder permissão no navegador e de o navegador suportar Web Push; a premissa P-02 deve ser validada com usuários.
- **Ponto em aberto:** [RNF03](../requisitos/requisitos-nao-funcionais.md#RNF03) pede widget para smartphone, o que normalmente exige recurso nativo ou instalável. O pacote não explica como isso se concilia com uma aplicação apenas web; o grupo precisa definir.
