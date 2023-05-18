# Arduino_PWM_Controller

1. [Introdução ao PWM](#introdução-ao-pwm)

    A Modulação por Largura de Pulso (PWM) é uma técnica em que um sinal digital é convertido em um sinal analógico pulsante. O sinal é composto por pulsos de largura variável, onde a relação entre o tempo em que o pulso está em nível alto (ligado) e o tempo em que está em nível baixo (desligado) determina a potência/velocidade média transmitida.
2. [Componentes necessários](#componentes-necessários)
   - Arduino Nano
   - Motor DC 5V
   - Bateria 5V
   - Botão momentâneo
   - Ponte H L293D
   - Resistor 10K
3. [Esquemático](#esquemático)


![Arduino_PWM_Controller](https://github.com/brunocatani/Arduino_PWM_Controller/assets/94939071/faa356e3-f7d6-45cc-90af-e1123b4cf3c5)

4. [Código-fonte](#código-fonte)
````
#include <Arduino.h>

#define BUTTON_PIN 2
#define PWM 9

int estado_botao = 0;
int pwm = 0;
int ultimo_estado_botao = 0;
unsigned long tempo_acionado = 0;
unsigned long tempo_delay = 50;

void setup() {
  // Configura os pinos como entrada/saída
  pinMode(PWM, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);
}

void loop() {
  int leitura = digitalRead(BUTTON_PIN);

  // Verifica se houve uma mudança no estado do botão
  if (leitura != ultimo_estado_botao) {
    ultimo_estado_botao = leitura;
    if (leitura == HIGH) {  
      // Armazena o tempo em que o botão foi acionado
      tempo_acionado = millis();
    }
  }

  // Verifica se o botão está pressionado e se já passou o tempo limitador de atraso
  if (leitura == HIGH && ((millis() - tempo_acionado) > tempo_delay)) {
    // Incrementa o valor do PWM
    pwm += 64;
    
    //Ao atingir um valor maior de 255 o "contador é resetado a 0"
    if (pwm > 255) {
      pwm = 0;
    }
  }

  // Aplica o valor do PWM ao motor
  analogWrite(PWM, pwm);

  // Aguarda um pequeno intervalo antes de reiniciar o loop
  delay(50);
}


````

5. [Funcionamento do projeto](#funcionamento-do-projeto)

    Ao acionar o botão o motor recebe um sinal PWM que aumenta 64 bits a cada acionamento, equivalente a 0%, 25%, 50%, 75% e 100% respectivamente do range total de velocidade relativa possivel do motor DC.
    Ao atingir um valor acima de 255 o contador é resetado ao valor 0, reiniciando o ciclo.
