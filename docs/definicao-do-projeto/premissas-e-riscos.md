# Premissas e Riscos

## Riscos

!!! warning "Pendente"
    A probabilidade e a gravidade de cada risco serão classificadas pelo grupo; as mitigações são propostas sujeitas à validação.

| ID | Risco | Impacto | Mitigação |
|:---|:---|:---|:---|
| <a id="R-01"></a>R-01 | Plano insalubre | O planejamento pode impor carga inadequada, comprometer descanso e bem-estar ou favorecer esgotamento. Mesmo um plano inicialmente viável pode tornar-se prejudicial sem que o acompanhamento identifique o problema. | Limites ajustáveis de carga e descanso, acompanhamento proporcional de sinais de sobrecarga e possibilidade de reduzir, pausar ou rever objetivos e planos. |
| <a id="R-02"></a>R-02 | Planejamento inadequado | Informações incompletas, suposições incorretas ou erros nas recomendações podem gerar planos inviáveis ou ineficientes. | Validar restrições e objetivos, explicitar as justificativas e permitir revisão pelo estudante. |
| <a id="R-03"></a>R-03 | Falsa sensação de progresso | A realização de atividades pode ser confundida com aprendizagem ou avanço real. | Distinguir execução, progresso em objetivos e evidências de aprendizagem, evitando indicadores enganosos. |
| <a id="R-04"></a>R-04 | Sobrecarga de interação | Cadastros, questionários, check-ins e atualizações podem consumir tempo e esforço desproporcionais aos benefícios. | Coleta progressiva e contextual, reaproveitamento de informações e interações dispensáveis quando adequado. |
| <a id="R-05"></a>R-05 | Replanejamento contraproducente | Mudanças excessivamente frequentes ou automáticas podem causar instabilidade, perda de controle, culpa ou abandono. | Aprovação por padrão, automação configurável, notificações sobre alterações, explicações e opção de reversão. |
| <a id="R-06"></a>R-06 | Exposição de dados pessoais | Dados sobre rotina, compromissos, desempenho e bem-estar podem ser acessados ou utilizados indevidamente. | Minimizar dados, restringir acessos, proteger armazenamento e definir retenção e exclusão. |
| <a id="R-07"></a>R-07 | Dependência e confiança excessiva | O estudante pode aceitar recomendações inadequadas ou perder autonomia sobre a própria rotina. | Preservar a decisão do usuário, apresentar alternativas, limitações e incertezas. |
| <a id="R-08"></a>R-08 | Complexidade excessiva do produto | Ampliação do escopo pode atrasar a implementação e enfraquecer a validação do valor central. | Priorizar a consolidação do objetivo, proposta de planos, acompanhamento e adaptação antes de funções complementares. |

## Premissas

| ID | Premissa | Motivo | Como validar |
|:---|:---|:---|:---|
| <a id="P-01"></a>P-01 | O público-alvo será composto por estudantes que possuem disciplinas, conteúdos, provas e entregas que precisam ser organizados ao longo de uma rotina de estudos. | Delimitar o público e garantir que as funcionalidades atendam às necessidades acadêmicas identificadas. | Validar o perfil do público por meio de entrevistas ou questionários com estudantes. |
| <a id="P-02"></a>P-02 | Os usuários terão acesso à internet e a um dispositivo com navegador compatível, seja computador, tablet ou smartphone. | A aplicação será desenvolvida como um sistema web responsivo. | Validar com usuários se o acesso por navegador atende aos dispositivos utilizados no cotidiano. |
| <a id="P-03"></a>P-03 | Os estudantes serão responsáveis por informar e manter atualizados seus dados acadêmicos, como matérias, quantidade de sessões semanais, provas, entregas e compromissos fixos. | A geração e a atualização dos cronogramas dependem da disponibilidade e da precisão dessas informações. | Validar o fluxo de cadastro e atualização dos dados por meio de testes de usabilidade. |
| <a id="P-04"></a>P-04 | Os materiais de estudo enviados pelos usuários estarão em formatos compatíveis com o processamento da aplicação e terão qualidade suficiente para leitura e extração de conteúdo. | A geração de resumos e a organização automática dos tópicos dependem da legibilidade dos arquivos enviados. | Realizar testes com arquivos PDF e imagens de diferentes tamanhos e qualidades, considerando o limite previsto de até 50 páginas. |
| <a id="P-05"></a>P-05 | Os usuários estarão dispostos a fornecer informações sobre seu cronotipo e realizar registros pontuais de humor e energia. | Essas informações serão utilizadas para apoiar a distribuição das atividades conforme o ritmo individual do estudante. | Validar a aceitação dos check-ins e do processo de identificação do cronotipo em testes com usuários. |
| <a id="P-06"></a>P-06 | O estudante poderá registrar a conclusão de suas sessões de estudo, permitindo que o sistema acompanhe o cumprimento das metas e recalcule a rotina quando necessário. | O acompanhamento do progresso e o replanejamento dependem dos registros de atividades realizadas e pendentes. | Testar os fluxos de conclusão, atraso e recálculo de sessões durante a semana. |
| <a id="P-07"></a>P-07 | A integração com o Google Calendar dependerá da autorização do usuário e respeitará o escopo de acesso definido para leitura dos compromissos. | A integração deve permitir considerar compromissos externos sem conceder permissões desnecessárias sobre a agenda pessoal. | Validar as permissões da API e testar a sincronização unidirecional. |
| <a id="P-08"></a>P-08 | Os dados pessoais, acadêmicos e comportamentais dos usuários serão utilizados exclusivamente para as finalidades informadas pela aplicação, mediante os consentimentos aplicáveis. | A solução trabalhará com informações de rotina, sono, humor e compromissos, exigindo cuidados com privacidade e proteção de dados. | Validar os fluxos de consentimento, acesso, exclusão de conta e tratamento de dados conforme a LGPD. |
| <a id="P-09"></a>P-09 | O compartilhamento entre estudantes será limitado às ementas e trilhas estruturadas, sem disponibilizar dados pessoais, notas ou calendários individuais de outros usuários. | O compartilhamento deve preservar o isolamento dos dados pessoais. | Testar a geração e a importação de links, verificando o isolamento dos dados pessoais. |
