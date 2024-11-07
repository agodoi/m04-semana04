# Atendimento do Professor

## Terças e quintas. Professor de horário parcial. Nesses 2 dias, podem contar comigo!

# Semana 04 - Sensores e Processamento de Valores de Entrada Analógicos para Digital

Conceitos de entradas digitais e analógicas e leitura de valores de sensores analógicos e digitais para sensoriamento/monitoramento automatizado usando o ESP32. Exemplo de LDR com ponte de wheatstone, sensor de gás, sensor de temperatura PTC ou NTC. A instrução também tratará de requisitos não funcionais.


## Impacto no seu Projeto

### Essa aula pode decidir a viabilidade de alguns sensores no seu projeto.

### Você sabe se o sensor que você está usando é suficientemente preciso para a sua aplicação?

### Você sabe se o ESP32 está arredondando valores ao transformar o sinal analógico do sensor em digital?

### Você sabe qual é o mínimo de sensibilidade do ESP32 ao capturar mudanças de valores dos sensores?

### Qual melhor sensor de vibração para o seu projeto? Acelerômetro ou um Piezoelétrico?

![MPU6050](https://github.com/agodoi/m04-semana04/blob/main/imgs/mpu6050.png)
![Piezoelétrico](https://github.com/agodoi/m04-semana04/blob/main/imgs/piezoeletrico.png)


### Para responder essas perguntas, vejamos a nossa trilha de hoje:

## 1. Como digitalizar o sinal analógico
   - Definições
   - Amostragem
   - Quantização
   - Codificação

## 2. Prática com TinkerCad

---

## Por que Digitalizar Sinais?

Os microcontroladores e computadores possuem memórias finitas e padronizadas em bits, enquanto os seres humanos e a natureza utilizam sinais analógicos. 

Como os sinais analógicos não são compatíveis diretamente com os bits, é necessário digitalizá-los para armazenamento e processamento no mundo digital. Este processo envolve três etapas principais:

1. **Amostragem**
2. **Quantização**
3. **Codificação**

---

## Amostragem

A amostragem consiste em capturar "fotos" do sinal analógico em intervalos regulares de tempo.

![Amostragem](https://github.com/agodoi/m04-semana04/blob/main/imgs/amostragem.png)


**Regra de Ouro:**  
A taxa de amostragem (f<sub>A</sub>) deve ser pelo menos duas vezes maior que a frequência máxima (f<sub>MAX</sub>) do sinal analógico para garantir que o sinal possa ser recuperado sem perda de informações.

### f<sub>A</sub> >= 2.f<sub>MAX</sub>

![Amostragem](https://github.com/agodoi/m04-semana04/blob/main/imgs/amostragem2.png)

---

## Quantização

A quantização é o processo de arredondar os valores amostrados para o degrau mais próximo em uma escala discreta, seja no quadrante positivo ou negativo.


![Quantização](https://github.com/agodoi/m04-semana04/blob/main/imgs/quantizacao.png)


![Quantização](https://github.com/agodoi/m04-semana04/blob/main/imgs/quantizacao2.png)


**Regra de Ouro:**  
- O número de degraus = 2<sup>Nbits</sup>
- Mais bits resultam em mais degraus, menos erro de quantização, mas maior tamanho do arquivo.
- Menos bits resultam em menos degraus, mais erro por arredondamento, mas menor tamanho do arquivo.

### degraus = 2<sup>Nbits</sup>
---

## Codificação

A codificação converte o valor quantizado em uma sequência binária para armazenamento e processamento em memória digital.

![Amostragem](https://github.com/agodoi/m04-semana04/blob/main/imgs/codificacao.png)

### Resumo do Processo:
- **Amostragem:** Captura de amostras (fotos) do sinal analógico para evitar aliasing.
- **Quantização:** Arredondamento das amostras para o degrau mais próximo.
- **Codificação:** Conversão dos valores quantizados para uma sequência binária.

---

## Quais pinos estão envolvidos nesse conceito?

![Pinout ESP32](https://github.com/agodoi/m04-semana04/blob/main/imgs/pinoutESP32.png)

* Onde há pinos nomeados de **DAC** indicam que liberam sinal analógico em degraus, que são os pinos 25 e 26.
* Onde há pinos nomeados de **ADC** indicam que recebem sinal analógico verdadeiros e convertem para digital, que são 02 04 15 12 13 14 25 26 27 32 33 34 35 36 e 39.
* Onde há pinos com sinal de til ```~``` indicam que podem emitir sinais equivalentes ao analógico, que são sinais PWM.

![Sinal PWM](https://github.com/agodoi/m04-semana04/blob/main/imgs/pwm.png)

Quanto mais tempo o sinal quadrado se manter, maior o brilho da lâmpada. Quanto menor o tempo o sinal quadrado se manter, menor a tensão média no LED, logo seu brilho se diminui.

## Prática: Comparando Qualidado Arduino vs ESP32

1. Abra o Arduino IDE;
2. Cole o código agaixo;
3. Monte o circuito de exemplo e siga as orientações fornecidas pelo professor.

```
const int analogInPin = A0;  // Analog input pin that the potentiometer is attached to
const int analogOutPin = 9;  // Analog output pin that the LED is attached to
int ref = 0, limite = 1000;

int sensorValue = 0;  // value read from the pot
int outputValue = 0;  // value output to the PWM (analog out)

void setup() {
	Serial.begin(9600); //inicializa a comunicação do Monitor Serial
	pinMode(analogInPin, INPUT);
  	pinMode(analogOutPin, INPUT);
}

void loop() {

   if(millis() - ref >= limite){
      ref = millis();
      sensorValue = analogRead(analogInPin); //lê o pino de forma analógica
      outputValue = map(sensorValue, 0, 1023, 0, 255); //faz regra de três entre duas faixas de valores
      analogWrite(analogOutPin, outputValue); //joga o resultado da regra de 3 no pino do LED
      Serial.print("sensor = ");
      Serial.print(sensorValue);
      Serial.print("\t output = ");
      Serial.println(outputValue);
   }
}
```

4. Agora abra o Wokwi, montem o mesmo circuito, mas usando o ESP32;
5. Vamos adatapr o código anterior para os recursos do ESP32;

```
const int analogInPin = 2;  // Analog input pin that the potentiometer is attached to
const int analogOutPin = 25;  // Analog output pin that the LED is attached to
int ref = 0, limite = 500;

int sensorValue = 0;  // value read from the pot
int outputValue = 0;  // value output to the PWM (analog out)

void setup() {
    Serial.begin(9600); //inicializa a comunicação do Monitor Serial
    pinMode(analogInPin, INPUT);
    pinMode(analogOutPin, INPUT);
}

void loop() {

   if(millis() - ref >= limite){
      ref = millis();
      sensorValue = analogRead(analogInPin); //lê o pino de forma analógica
      outputValue = map(sensorValue, 0, 4095, 0, 255); //faz regra de três entre duas faixas de valores
      analogWrite(analogOutPin, outputValue); //joga o resultado da regra de 3 no pino do LED
      Serial.print("sensor = ");
      Serial.print(sensorValue);
      Serial.print("\t output = ");
      Serial.println(outputValue);
   }
}
```

---

## Desafio

**Objetivo:** Explicar a diferença entre os dois códigos via uma pair teaching

Cada grupo fará a sua apresentação explicando a diferença de recurso entre Arduino e ESP32 nos itens: ADC e DAC e responder as seguintes perguntas:

1) Quantos bits ADC e DAC o Arduino possui?
2) Quantos bits ADC e DAC o ESP32 possui?
3) Qual o impacto disso na leitura de um sensor analógico?
4) E pensando no sensor piezoelétrico para detectar vibração de máquinas, qual o melhor microcontrolador?

A melhor explicação ganhará um prêmio!
