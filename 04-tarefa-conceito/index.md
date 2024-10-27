# Conceito de tarefas

## Informações gerais

- Capítulo: [Conceito de tarefas](https://wiki.inf.ufpr.br/maziero/lib/exe/fetch.php?media=socm:socm-04.pdf)
- Disciplina: *sistemas operacionais*
- Livro: [Sistemas Operacionais: Conceitos e Mecanismos](https://wiki.inf.ufpr.br/maziero/doku.php?id=socm:start)

## Aluno

- nome: Fábio Alexandre de Souza Lucena Filho
- matrícula: 20232014040021

## Respostas dos exercícios
01. Time sharing ou tempo compartilhado é basicamente cada tarefa recebe um tempo definido para o processamento,
e isso é importante pois permite que o sistema operacional não fique travado em apenas uma tarefa que não vai 
terminar de ser executada nunca devido algum problema.

02. O tempo de quantum é definido pelo sistema operacional de acordo com o tipo de prioridade da tarefa.

03. ???

04. 
• E → P > É realizada quando o tempo da tarefa acaba;
• E → S > É realizada caso a tarefa peça acesso a recursos não disponíveis;
• S → E > Não é possível que isso ocorra;
• P → N > Não é possível que isso ocorra;
• S → T > Não é possível que isso ocorra;
• E → T > É realizada quando uma tarefa encerra sua execução ou é abortada devido algum erro;
• N → S > Não é possível que isso ocorra;
• P → S > Não é possível que isso ocorra;

05.
[N] O código da tarefa está sendo carregado.
[P] A tarefas são ordenadas por prioridades.
[S] A tarefa sai deste estado ao solicitar uma operação de entrada/saída.
[T] Os recursos usados pela tarefa são devolvidos ao sistema.
[E] A tarefa vai a este estado ao terminar seu quantum.
[P] A tarefa só precisa do processador para poder executar.
[S] O acesso a um semáforo em uso pode levar a tarefa a este estado.
[E] A tarefa pode criar novas tarefas.
[E] Há uma tarefa neste estado para cada processador do sistema.
[S] A tarefa aguarda a ocorrência de um evento externo.

 


