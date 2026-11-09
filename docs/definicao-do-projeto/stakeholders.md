# Stakeholders

Mapa de stakeholders com prioridade, papel, necessidades (referenciando os [requisitos](../requisitos/index.md)) e conflitos principais.

## Estudante / Usuário Final

Prioridade 1 (Crítica).

- **Papel:** usuário principal e beneficiário direto.
- **Necessidades:** agenda ajustada ao cronotipo ([RF04](../requisitos/requisitos-funcionais.md#RF04), [RF05](../requisitos/requisitos-funcionais.md#RF05)); estudo pesado no pico biológico ([RF07](../requisitos/requisitos-funcionais.md#RF07)); recálculo sem culpa em atrasos ([RF09](../requisitos/requisitos-funcionais.md#RF09)); resumos rápidos de PDF ([RF03](../requisitos/requisitos-funcionais.md#RF03), [RNF06](../requisitos/requisitos-nao-funcionais.md#RNF06)); explicações didáticas para sugestões ([RNF12](../requisitos/requisitos-nao-funcionais.md#RNF12)); privacidade total de notas e rotina ([RNF04](../requisitos/requisitos-nao-funcionais.md#RNF04), [RNF08](../requisitos/requisitos-nao-funcionais.md#RNF08)).
- **Relações:** interage diretamente com a interface do produto e fornece as respostas de humor/energia que alimentam a IA.
- **Conflitos:** quer precisão e adaptabilidade sem fricção; notificações de atraso ([RNF05](../requisitos/requisitos-nao-funcionais.md#RNF05)) podem soar punitivas se mal calibradas, e check-ins frequentes ([RF06](../requisitos/requisitos-funcionais.md#RF06)) geram risco de abandono por cansaço.

## Estudante Colaborador (Colega)

Prioridade 4 (Baixa/Média).

- **Papel:** usuário secundário e canal de tração orgânica.
- **Necessidades:** importar trilhas e ementas via link com ajuste automático da carga horária para a sua própria rotina disponível ([RF16](../requisitos/requisitos-funcionais.md#RF16)).
- **Relações:** conecta-se à comunidade ao reutilizar ementas geradas por outros estudantes.
- **Conflitos:** necessidade de importar conteúdos de terceiros sem violar o isolamento e o sigilo dos calendários e notas pessoais alheios ([RNF04](../requisitos/requisitos-nao-funcionais.md#RNF04)).

## Equipe de Produto e UX/UI Design

Prioridade 2 (Alta).

- **Papel:** definição da experiência, ergonomia e tom de voz da solução.
- **Necessidades:** interface responsiva em web/mobile ([RNF01](../requisitos/requisitos-nao-funcionais.md#RNF01), [RNF10](../requisitos/requisitos-nao-funcionais.md#RNF10)); modo diurno/noturno ([RNF02](../requisitos/requisitos-nao-funcionais.md#RNF02)); widget ágil ([RNF03](../requisitos/requisitos-nao-funcionais.md#RNF03)); coletar dados de humor/energia sem gerar atrito diário ([RF06](../requisitos/requisitos-funcionais.md#RF06)); notificações construtivas de atraso ([RNF05](../requisitos/requisitos-nao-funcionais.md#RNF05)).
- **Relações:** articula a comunicação entre a Engenharia de IA (para traduzir a lógica matemática em telas simples) e o usuário final.
- **Conflitos:** busca um layout minimalista e fluido, mas precisa acomodar justificativas explicativas de estudo ([RNF12](../requisitos/requisitos-nao-funcionais.md#RNF12)) e solicitações recorrentes de feedback de bem-estar ([RF06](../requisitos/requisitos-funcionais.md#RF06)).

## Engenharia de Software (Full Stack e Cloud)

Prioridade 1 (Crítica).

- **Papel:** construtora da infraestrutura web, integração e banco de dados.
- **Necessidades:** web app responsivo ([RNF10](../requisitos/requisitos-nao-funcionais.md#RNF10)); processamento de PDFs/imagens em até 60 s ([RF03](../requisitos/requisitos-funcionais.md#RF03), [RNF06](../requisitos/requisitos-nao-funcionais.md#RNF06)); importação resiliente de .csv ([RF10](../requisitos/requisitos-funcionais.md#RF10), [RNF13](../requisitos/requisitos-nao-funcionais.md#RNF13)); disparos Web Push pontuais ([RNF11](../requisitos/requisitos-nao-funcionais.md#RNF11)); retenção de histórico por 90 dias ([RNF07](../requisitos/requisitos-nao-funcionais.md#RNF07)).
- **Relações:** suporta as demandas de dados da Engenharia de IA e implementa as diretrizes de segurança e privacidade do DPO.
- **Conflitos:** o SLA rígido de 60 segundos para até 50 páginas ([RNF06](../requisitos/requisitos-nao-funcionais.md#RNF06)) gera gargalos em filas de background e consumo elevado de recursos de servidor.

## Engenharia de IA e Algoritmos

Prioridade 2 (Alta).

- **Papel:** desenvolvedora da inteligência de recomendação e regras de negócio.
- **Necessidades:** pipeline de RAG sem alucinação para extrair tópicos de PDFs ([RF03](../requisitos/requisitos-funcionais.md#RF03)); algoritmo matemático leve cruzando cronotipo e horário local ([RF05](../requisitos/requisitos-funcionais.md#RF05), [RF07](../requisitos/requisitos-funcionais.md#RF07)); automação da curva de esquecimento ([RF15](../requisitos/requisitos-funcionais.md#RF15)).
- **Relações:** fornece os insights e blocos de estudo para a interface de UX e consome dados de arquivos preparados pela Engenharia de Software.
- **Conflitos:** precisa de muitos parâmetros comportamentais para refinar a agenda, mas o escopo exige um algoritmo determinístico simples sem sobrecarregar o estudante com perguntas.

## Encarregado de Dados (DPO) / Jurídico

Prioridade 1 (Crítica).

- **Papel:** guardião da governança de dados e conformidade regulatória.
- **Necessidades:** conformidade integral com a LGPD; consentimento explícito para telemetria de sono/humor ([RNF08](../requisitos/requisitos-nao-funcionais.md#RNF08)); fluxo de exclusão total de dados; escopo estrito de leitura na API do Google Calendar ([RNF09](../requisitos/requisitos-nao-funcionais.md#RNF09)).
- **Relações:** audita a coleta de dados da equipe de desenvolvimento e valida os termos de uso antes de qualquer funcionalidade ser liberada aos usuários.
- **Conflitos:** tende a restringir a retenção de dados e vetar coletas indiretas de comportamento, colidindo com a retenção pretendida para histórico de 90 dias ([RNF07](../requisitos/requisitos-nao-funcionais.md#RNF07)).

## Provedores de Infraestrutura (APIs Cloud, LLM e Google)

Prioridade 3 (Média/Alta).

- **Papel:** fornecedores tecnológicos essenciais.
- **Necessidades:** consumo dentro das quotas contratuais; segurança no tráfego de dados; estabilidade de conexões e APIs.
- **Relações:** plataforma base sobre a qual a Engenharia de Software constrói o RAG e a sincronização unidirecional da agenda externa.
- **Conflitos:** picos simultâneos de processamento de PDFs ou alterações de preço por token podem inviabilizar o tempo de resposta estipulado de 60 s ([RNF06](../requisitos/requisitos-nao-funcionais.md#RNF06)) e elevar os custos da operação.
