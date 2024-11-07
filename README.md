# Atendimento do Professor

## Terças e quintas. Professor de horário parcial. Nesses 2 dias, podem contar comigo!

# Semana 04 - Sensores e Processamento de Valores de Entrada Analógicos para Digital

Conceitos de entradas digitais e analógicas e leitura de valores de sensores analógicos e digitais para sensoriamento/monitoramento automatizado usando o ESP32. Exemplo de LDR com ponte de wheatstone, sensor de gás, sensor de temperatura PTC ou NTC. A instrução também tratará de requisitos não funcionais.


## Impacto no seu Projeto

### Essa aula pode decidir a viabilidade de alguns sensores no seu projeto.

### Você sabe se o sensor que você está usando é suficientemente preciso para a sua aplicação?

### Você sabe se o ESP32 está arredondando valores ao transformar o sinal analógico do sensor em digital?

### Você sabe qual é o mínimo de sensibilidade do ESP32 ao capturar mudanças de valores dos sensores?


## 1. Como digitalizar o sinal analógico
   - Amostragem
   - Quantização
   - Codificação

## 2. Prática com TinkerCad

---

## Por que Digitalizar Sinais?

Os microcontroladores e computadores possuem memórias finitas e padronizadas em bits, enquanto os seres humanos e a natureza utilizam sinais analógicos. Como os sinais analógicos não são compatíveis diretamente com os bits, é necessário digitalizá-los para armazenamento e processamento no mundo digital. Este processo envolve três etapas principais:

1. **Amostragem**
2. **Quantização**
3. **Codificação**

---

## Amostragem

A amostragem consiste em capturar "fotos" do sinal analógico em intervalos regulares de tempo.

**Regra de Ouro:**  
A taxa de amostragem (f<sub>A</sub>) deve ser pelo menos duas vezes maior que a frequência máxima (f<sub>MAX</sub>) do sinal analógico para garantir que o sinal possa ser recuperado sem perda de informações.

---

## Quantização

A quantização é o processo de arredondar os valores amostrados para o degrau mais próximo em uma escala discreta, seja no quadrante positivo ou negativo.

**Regra de Ouro:**  
- O número de degraus = 2^N Bits
- Mais bits resultam em mais degraus, menos erro de quantização, mas maior tamanho do arquivo.
- Menos bits resultam em menos degraus, mais erro por arredondamento, mas menor tamanho do arquivo.

---

## Codificação

A codificação converte o valor quantizado em uma sequência binária para armazenamento e processamento em memória digital.

### Resumo do Processo:
- **Amostragem:** Captura de amostras (fotos) do sinal analógico para evitar aliasing.
- **Quantização:** Arredondamento das amostras para o degrau mais próximo.
- **Codificação:** Conversão dos valores quantizados para uma sequência binária.

---

## Prática

1. Abra o Arduino IDE.
   - Vá em `File → Examples → Analog → AnalogInOutSerial`.
2. Importe o exemplo para o TinkerCad.
3. Monte o circuito de exemplo e siga as orientações fornecidas pelo professor.

---

## Prática / Desafio

**Objetivo:** Criar um programa no ESP32 utilizando LDR (sensor de luz), LED e resistor em série.  

1. **Exercício:**  
   - Faça um código para o Arduino Uno que controle o brilho do LED com base no nível de luz capturado pelo LDR.
   - Monte o circuito conforme instruído.

---

Pronto para ser utilizado no GitHub como uma aula prática e teórica! Ajuste conforme necessário para seu estilo de ensino.

---
