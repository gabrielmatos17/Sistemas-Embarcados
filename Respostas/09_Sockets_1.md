1. Com relação ao modelo cliente-servidor, responda:

	(a) Quais são as características básicas deste modelo?
```
	É uma extensão do conceito de pipe com a possibilidade de comunicação através de rede de computadores.
	É possível utilizar um socket para se comunicar	com outro processo utilizando um modelo	cliente/servidor tanto através da rede quanto internamente em uma mesma máquina.	
```
	(b) Quais são as características básicas do servidor?
```
	Aguarda	passivamente;	
 	Responde aos clientes;
	Socket passivo.
```
	(c) Quais são as características básicas do cliente?
```
	Inicia a comunicação;	
	Deve saber o endereço e	a porta	do servidor;	
	Socket ativo.
```
2.  Com relação ao protocolo de comunicação da internet, responda:

	(a) O que são protocolos de comunicação?
```
	São as formas com que são feitas as comunicações entre o servidor e o cliente.
```
	(b) Quais são as características básicas de protocolos de comunicação?
```
	TCP: Os dados são enviados respeitando uma ordem. Ex: Youtube, Netflix.
	Datagram: Não importa a ordem, mas o conteúdo chega. Ex: Televisão.
```
3. Com relação ao protocolo TCP, responda:

	(a) O que são portas no protocolo TCP?
```
	São números enviados pelo cliente que indicam os tipos de açoes a serem executadas.
```
	(b) Para que servem estas portas?
```
	Para diferenciar os tipos de ações, como email, mensagem, etc.
```