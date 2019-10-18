1. Cite as vantagens e desvantagens das comunicação serial:

(a) Assíncrona (UART).
```
Vantagens: Simplicidade do protocolo, full-duplex.
Desvantagens: Baixa taxa de transmissão.
```
(b) SPI.
```
Vantagens: Não há limite para o número de escravos, a comunicação é full-duplex e possui boa velocidade de comunicação.
Desvantagens: Muitos fios.
```
2. Considere o caso em que a Raspberry Pi deve receber leituras analógico/digitais de um MSP430, e que a comunicação entre os dois é UART. É tecnicamente possível que o MSP430 mande os resultados da conversão A/D a qualquer hora, ou ele deve aguardar a Raspberry Pi fazer um pedido ao MSP430? Por quê?
```
Ela tem que esperar a Raspberry estar em nível alto, para poder receber.
```
3. Considere o caso em que a Raspberry Pi deve receber leituras analógico/digitais de um MSP430, que a comunicação entre os dois seja SPI, e que o MSP430 seja o escravo. É tecnicamente possível que o MSP430 mande os resultados da conversão A/D a qualquer hora, ou ele deve aguardar a Raspberry Pi fazer um pedido ao MSP430? Por quê?
```
Pode, pois não há espera pelo envio.
```
4. Se o Raspberry Pi tiver de se comunicar com dois dispositivos via UART, como executar a comunicação com o segundo dispositivo?
```
Precisa de setar o bit de endereço, após o bit de start.
```
5. Se o Raspberry Pi tiver de se comunicar com dois dispositivos via SPI, como executar a comunicação com o segundo dispositivo?
```
Há duas formas, a individual, onde é necessário duas portas seletoras para escolher o escravo, e a Daisy chain, onde a miso do primeiro escravo se conecta com a mosi do segundo escravo e a miso do segundo se conecta com o miso do mestre.
```