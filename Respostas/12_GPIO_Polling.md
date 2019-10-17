1. Crie dois processos, e faça com que o processo-filho gere uma onda quadrada, enquanto o processo-pai faz polling de um botão no GPIO, aumentando a frequência da onda sempre que o botão for pressionado. A frequência da onda quadrada deve começar em 1 Hz, e dobrar cada vez que o botão for pressionado. A frequência máxima é de 64 Hz, devendo retornar a 1 Hz se o botão for pressionado novamente.
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
	struct pollfd pfd;
	char buffer;
	int btn_press;
	system("echo 4       > /sys/class/gpio/export");
	system("echo falling > /sys/class/gpio/gpio4/edge");
	system("echo in      > /sys/class/gpio/gpio4/direction");
	pfd.fd = open("/sys/class/gpio/gpio4/value", O_RDONLY);
	if(pfd.fd < 0)
	{
		puts("Erro abrindo /sys/class/gpio/gpio4/value");
		puts("Execute este programa como root.");
		return 1;
	}
	pfd.events = POLLPRI | POLLERR;
	pfd.revents = 0;
	
	while(1)
	{
		//Filhão
		pid_filho = fork();
		if(pid_filho==0)
			sqwv(f);
		//Papai
		puts("Pressione o botao...");
		for(btn_press=0; btn_press<5; btn_press++)
		{
			lseek(pfd.fd, 0, SEEK_SET);
			read(pfd.fd, &buffer, 1);
			poll(&pfd, 1, -1);
			puts("Borda de descida!");
			do
			{
				lseek(pfd.fd, 0, SEEK_SET);
				read(pfd.fd, &buffer, 1);
			}while(buffer=='0');
			usleep(300000);
	}
	close(pfd.fd);
	system("echo 4 > /sys/class/gpio/unexport");
	}
	return 0;
}
```