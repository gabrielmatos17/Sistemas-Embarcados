1. Escreva um código em C para gerar uma onda quadrada de 1 Hz em um pino GPIO do Raspberry Pi.
```C
#include <wiringPi.h>
#include <unistd.h>
#define SAIDA 7
void sqwv(int freq_hz)
{
	int T_us_2 = (500000 + (freq_hz/2))/freq_hz;
	while(1)
		{
			digitalWrite(SAIDA, HIGH);
			usleep(T_us_2);
			digitalWrite(SAIDA, LOW);
			usleep(T_us_2);
		}
}
int main(void)
{
	wiringPiSetup();
	pinMode(SAIDA, OUTPUT);
	sqwv(1);
	return 0;
}
```
2. Generalize o código acima para qualquer frequência possível.
```C
#include <wiringPi.h>
#include <unistd.h>
#define SAIDA 7
void sqwv(int freq_hz)
{
	int T_us_2 = (500000 + (freq_hz/2))/freq_hz;
	while(1)
		{
			digitalWrite(SAIDA, HIGH);
			usleep(T_us_2);
			digitalWrite(SAIDA, LOW);
			usleep(T_us_2);
		}
}
int main(void)
{
	int a;
	wiringPiSetup();
	pinMode(SAIDA, OUTPUT);
	scanf("%d",&a);
	sqwv(a);
	return 0;
}
```
3. Crie dois processos, e faça com que o processo-filho gere uma onda quadrada, enquanto o processo-pai lê um botão no GPIO, aumentando a frequência da onda sempre que o botão for pressionado. A frequência da onda quadrada deve começar em 1 Hz, e dobrar cada vez que o botão for pressionado. A frequência máxima é de 64 Hz, devendo retornar a 1 Hz se o botão for pressionado novamente.
```C
#include <wiringPi.h>
#include <unistd.h>
#include <signal.h>
#define SAIDA 7
#define ENTRADA 2
void sqwv(int freq_hz)
{
	int T_us_2 = (500000 + (freq_hz/2))/freq_hz;
	while(1)
	{
		digitalWrite(SAIDA, HIGH);
		usleep(T_us_2);
		digitalWrite(SAIDA, LOW);
		usleep(T_us_2);
	}
}
int main(void)
{
	int f = 1, pid_filho;
	wiringPiSetup();
	pinMode(SAIDA, OUTPUT);
	pinMode(ENTRADA, INPUT);
	while(1)
	{
		pid_filho = fork();
		if(pid_filho==0)
			sqwv(f);
		while(digitalRead(ENTRADA)>0);
		f = (f>32) ? 1 : 2*f;
		kill(pid_filho, SIGKILL);
		while(digitalRead(ENTRADA)==0);
		usleep(300000);
	}
	return 0;
}
```