# Requisitos Funcionais

<p align="center">Tabela 1: Requisitos Funcionais</p>

| ID | Descrição | Implementado |
|:---|:---|:---|
| <a id="RF01"></a>RF01 | Permitir que o usuário crie uma conta e faça login com e-mail e senha. | Não |
| <a id="RF02"></a>RF02 | Criar ciclo de estudos semanal em que cada matéria tem uma quantidade de sessões a cumprir na semana, sem vínculo com dia ou horário. | Não |
| <a id="RF03"></a>RF03 | Permitir o upload e processamento de materiais (PDF e imagem). | Parcial (mecanismo RAG, ver [Mecanismo RAG](../mecanismo-rag/index.md)) |
| <a id="RF04"></a>RF04 | Geração automática de cronograma de estudos. | Não |
| <a id="RF05"></a>RF05 | Identificar o cronotipo do usuário (matutino, vespertino ou noturno). | Não |
| <a id="RF06"></a>RF06 | O sistema deve solicitar um feedback rápido de humor e energia do usuário em momentos-chave do dia. | Não |
| <a id="RF07"></a>RF07 | Alocar os tópicos mais complexos nos horários de maior pico de alerta biológico do usuário e tarefas leves nos momentos de baixa energia. | Não |
| <a id="RF08"></a>RF08 | Registrar a conclusão de uma sessão do ciclo em qualquer dia da semana corrente, descontando-a da meta semanal da matéria. | Não |
| <a id="RF09"></a>RF09 | Recálculo automático de nova rotina caso não haja cumprimento da agenda (sem prejudicar o estudante). | Não |
| <a id="RF10"></a>RF10 | Importação de provas e entregas a partir de planilha .csv com as colunas título, matéria, tipo e data. | Não |
| <a id="RF11"></a>RF11 | Exportação de agenda para impressão para estudantes que preferem a organização física. | Não |
| <a id="RF12"></a>RF12 | Cadastro de compromissos fixos recorrentes com dias da semana, hora de início e hora de término. | Não |
| <a id="RF13"></a>RF13 | Exibir as provas e entregas dos próximos 7 dias, ordenadas da data limite mais próxima para a mais distante. | Não |
| <a id="RF14"></a>RF14 | Exibir as provas e entregas pendentes do dia, ordenadas da data limite mais próxima para a mais distante. | Não |
| <a id="RF15"></a>RF15 | O sistema deve agendar sessões de revisão automática (ex.: 1, 7, 30 dias) nos espaços livres do calendário, baseadas nas sessões de estudo já concluídas. | Não |
| <a id="RF16"></a>RF16 | O sistema deve permitir a geração de links para compartilhamento de ementas e trilhas estruturadas, ajustando a carga horária automaticamente para quem importar. | Não |
