1. Crie dois programas em C para criar um cliente e um servidor. Faça com que o cliente envie os valores 1, 2, 3, 4, 5, 6, 7, 8, 9 e 10 para o servidor, com intervalos de 1 segundo entre cada envio. Depois de o cliente enviar o número 10, ele aguarda 1 segundo e termina a execução. O servidor escreve na tela cada valor recebido, e quando ele receber o valor 10, ele termina a execução.

```C
// Servidor Local
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <signal.h>
#include <sys/socket.h>
#include <sys/un.h>
char socket_name[50];
int socket_id;
void sigint_handler(int signum);
void print_client_message(int client_socket);
void desliga_servidor(void);
void desliga_servidor(void)
{
	// Apagando "socket_name" do sistema
	unlink(socket_name);
	// Fechando o socket local
	close(socket_id);
	exit(0);
}
void sigint_handler(int signum)
{
	fprintf(stderr, "\nRecebido o sinal CTRL+C... vamos desligar o servidor!\n");
	desliga_servidor();
}
int main (int argc, char* const argv[]) { 
	struct sockaddr socket_struct; //cria a estrutura
	strcpy(socket_name, argv[1]); //salva o caminho no socket_name
	socket_id = socket(PF_LOCAL, SOCK_STREAM, 0); //cria o socket e retorna o descritor do arquivo
	socket_struct.sa_family = AF_LOCAL; //especifica a familia de protocolos para local
	strcpy(socket_struct.sa_data, socket_name); //copia o caminho(endereço) passado para sa_data
	bind(socket_id, &socket_struct, sizeof(socket_struct)); //associa o socket de de um servidor a um endereço
	listen(socket_id, 10); //torna o socket passivo, para virar o servidor
	while(1)
	{
		struct sockaddr cliente;
		int socket_id_cliente;
		int msg;
		socklen_t cliente_len;
		//Aguardando a conexao de um cliente
		socket_id_cliente = accept(socket_id, &cliente, &cliente_len);
		read(socket_id_cliente, &msg, 1);
		fprintf(stderr,"\n\n   Mensagem = %d\n\n", msg);
		if (msg == 10)
		{
			fprintf(stderr, "Cliente pediu para o servidor fechar.\n");
			desliga_servidor();
		}
		fprintf(stderr, "Fechando a conexao com o cliente... \n");
		close(socket_id_cliente);
	}
return 0;
}
```
```C
//Cliente
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <sys/un.h>
#include <unistd.h>
int main (int argc, char* const argv[])
{
	char *socket_name;
	socket_name = argv[1]; //recebemos o nome do socket pelo terminal
	int socket_id;
	struct sockaddr name;
	for (int i = 0; i < 11; ++i)
	{	
		//fprintf(stderr, "Abrindo o socket local... ");  //printar no terminal com uso de stderr
		socket_id = socket(PF_LOCAL, SOCK_STREAM,0);    //abrindo o socket local
	    //fprintf(stderr, "Conectando o socket ao servidor no endereco local \"%s\"... ", socket_name);
		name.sa_family = AF_LOCAL;
		strcpy(name.sa_data, socket_name); // passando o endereço local salvo em socket_name para a stratuct name.sa_data endereço propriamente dito
		connect(socket_id, &name, sizeof(name)); // conecta 2 sockets
		//fprintf(stderr, "Mandando mensagem ao servidor... ");
		write(socket_id, &i, sizeof(i));
		sleep(1);
	} 	
	fprintf(stderr, "Fechando o socket local... ");
	close(socket_id);
	fprintf(stderr, "Feito!\n");
return 0;
}
```
2. Crie dois programas em C para criar um cliente e um servidor. Execute a seguinte conversa entre cliente e servidor:

```
CLIENTE: 	Pai, qual é a verdadeira essência da sabedoria?
SERVIDOR: Não façais nada violento, praticai somente aquilo que é justo e equilibrado.
CLIENTE: Mas até uma criança de três anos sabe disso!
SERVIDOR: Sim, mas é uma coisa difícil de ser praticada até mesmo por um velho como eu...
```
```C
//Servidor Local
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <signal.h>
#include <sys/socket.h>
#include <sys/un.h>
char socket_name[50];
int socket_id;
void sigint_handler(int signum);
void print_client_message(int client_socket);
void desliga_servidor(void)
{
	// Apagando "socket_name" do sistema
	unlink(socket_name);
	// Fechando o socket local
	close(socket_id);
	exit(0);
}
void sigint_handler(int signum)
{
	fprintf(stderr, "\nRecebido o sinal CTRL+C... vamos desligar o servidor!\n");
	desliga_servidor();
}
void print_client_message(int client_socket)
{
	int length;
	char* text;
	read(client_socket, &length, sizeof (length));
	text = (char*) malloc (length);
	read(client_socket, text, length);
	fprintf(stderr,"Cliente: %s\n\n", text);
	if (!strcmp (text, "sair"))
	{
		free (text);
		fprintf(stderr, "Cliente pediu para o servidor fechar.\n");
		desliga_servidor();
	}
	free (text);
}
int main (int argc, char* const argv[]) { 
	struct sockaddr socket_struct; //cria as estruct
	strcpy(socket_name, argv[1]);  //salva o caminho no socket_name
	socket_id = socket(PF_LOCAL, SOCK_STREAM, 0); // cria o socket e retorna o descritor do arquivo
	socket_struct.sa_family = AF_LOCAL;           //espeficica a familia de protocolos para local
	strcpy(socket_struct.sa_data, socket_name);   //copia o caminho(endereço) passado para sa_data
	bind(socket_id, &socket_struct, sizeof(socket_struct));	 //associa o socket de de um servidor a um endereço
	listen(socket_id, 10);								     //torna o socket passivo, para virar o servidor
	while(1)
	{
		struct sockaddr cliente;
		int socket_id_cliente;
		int msg;
		socklen_t cliente_len;
		//Aguardando a conexao de um cliente
		socket_id_cliente = accept(socket_id, &cliente, &cliente_len);
		print_client_message(socket_id_cliente);
		printf("Servidor:Não façais nada violento, praticai somente aquilo que é justo e equilibrado.\n\n");
		sleep(2);
		print_client_message(socket_id_cliente);
		printf("Sim, mas é uma coisa difícil de ser praticada até mesmo por um velho como eu...\n\n");
		if (msg == 10)
		{
			fprintf(stderr, "Cliente pediu para o servidor fechar.\n");
			desliga_servidor();
		}
		fprintf(stderr, "Fechando a conexao com o cliente... \n");
		close(socket_id_cliente);
	}
return 0;
}
```
```C
//Cliente
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <sys/un.h>
#include <unistd.h>
int main (int argc, char* const argv[])
{
	int length;
	char *socket_name;
	socket_name = argv[1]; // recebemos o nome do socket pelo terminal
	int socket_id;
	struct sockaddr name;
	char mensagem1[50]="Pai, qual é a verdadeira essência da sabedoria?";
	char mensagem2[50]="Mas até uma criança de três anos sabe disso!";
	socket_id = socket(PF_LOCAL, SOCK_STREAM,0);    	    
	name.sa_family = AF_LOCAL;
	strcpy(name.sa_data, socket_name); 
	connect(socket_id, &name, sizeof(name)); 
	length = strlen(mensagem1) + 1;
	write(socket_id, &length, sizeof(length));
	write(socket_id, mensagem1, length);
	sleep(1);
	length = strlen(mensagem2) + 1;
	write(socket_id, &length, sizeof(length));
	write(socket_id, mensagem2, length);
	fprintf(stderr, "Fechando o socket local... ");
	close(socket_id);
	fprintf(stderr, "Feito!\n");
return 0;
}
```