1. Quantos pipes serão criados após as linhas de código a seguir? Por quê?

(a)
```C
int pid;
int fd[2];
pipe(fd);
pid = fork();
```
Somente um, pois foi criado antes do fork.

(b)
```C
int pid;
int fd[2];
pid = fork();
pipe(fd);
```

Dois pipes, um para cada processo, pois foi criado após o fork.

2. Apresente mais cinco sinais importantes do ambiente Unix, além do `SIGSEGV`, `SIGUSR1`, `SIGUSR2`, `SIGALRM` e `SIGINT`. Quais são suas características e utilidades?

	SIGHUP (hangup) Corte: sinal emitido aos processos associados a um terminal quando este se “desconecta".
	SIGQUIT (quit) Abandono: sinal emitido aos processos do terminal quando com a tecla de abandono (quit ou CTRL+D) são acionadas.
	SIGILL  Instrução ilegal: emitido quando uma instrução ilegal é detectada.
	SIGTRAP Problemas com trace: emitido após cada instrução em caso de geração de traces dos processos (utilização da primitiva ptrace()).
	SIGIOT Problemas de instrução de E/S: emitido em caso de problemas de hardware.

3. Considere o código a seguir:

```C
#include <signal.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

void tratamento_alarme(int sig)
{
	system("date");
	alarm(1);
}

int main()
{
	signal(SIGALRM, tratamento_alarme);
	alarm(1);
	printf("Aperte CTRL+C para acabar:\n");
	while(1);
	return 0;
}
```

Sabendo que a função `alarm()` tem como entrada a quantidade de segundos para terminar a contagem, quão precisos são os alarmes criados neste código? De onde vem a imprecisão? Este é um método confiável para desenvolver aplicações em tempo real?

São mais confiáveis quando se comparados com o sleep, pois mantém uma contagem sem intervalos.
