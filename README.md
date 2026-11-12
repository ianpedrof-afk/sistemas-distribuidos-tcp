# sistemas-distribuidos-tcp

Simulação de um sistema distribuído em Python com sockets TCP, com inclusão do quarto nó D e detecção de falhas por heartbeat.

## O desafio do nó D

O coordenador controla o estoque de livros. Cada trabalhador roda em um processo independente e envia pulsos periódicos (*heartbeats*) ao coordenador. A resposta inclui os dados do estoque quando a versão local do trabalhador difere da versão central.

A inclusão de D envolve três trechos do [código da prática](./pratica1_tcp_processos.py). O método `EstadoCompartilhado.tratar` aceita o identificador D nas operações `REGISTRAR` e `PULSO` e inclui D na consulta `STATUS`. A função `main` inicia o quarto processo usando a mesma função `trabalhador` de A, B e C.

## Como executar no VS Code

Com Python 3.9 ou superior instalado, abra a pasta do projeto no VS Code. Selecione **Terminal → Novo Terminal** e execute:

```bash
python pratica1_tcp_processos.py
```

No Windows, também é possível usar `py pratica1_tcp_processos.py`. O programa usa apenas a biblioteca padrão e escolhe uma porta disponível em `127.0.0.1`. A demonstração termina automaticamente.

## O que observar no terminal

| Etapa | Comportamento esperado |
| --- | --- |
| Registro | A, B, C e D aparecem como `ATIVO`. O estoque começa em 10 livros (versão 0). |
| Atualização | No próximo pulso, cada trabalhador recebe o estoque de 7 livros (versão 1). |
| Falha de B | O programa encerra B. Após 2 segundos sem pulsos, a consulta de status indica `SEM CONTATO` para B. D continua ativo com A e C. |
| Recuperação | O estoque muda para 5 livros (versão 2). Ao reiniciar, B recebe o valor atual. Procure as linhas de B e D com `versão=2` para conferir que ambos receberam a atualização. |

A ordem das mensagens pode variar porque os processos executam de forma concorrente. O status mede o tempo desde o último contato. Nesta prática, a causa da ausência de pulsos de B é conhecida porque o próprio programa encerra o processo.
