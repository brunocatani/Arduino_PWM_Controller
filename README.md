# Arduino_PWM_Controller

1. [Introdução ao PWM](#introdução-ao-pwm)
    \n A Modulação por Largura de Pulso (PWM) é uma técnica em que um sinal digital é convertido em um sinal analógico pulsante. O sinal é composto por pulsos de largura variável, onde a relação entre o tempo em que o pulso está em nível alto (ligado) e o tempo em que está em nível baixo (desligado) determina a potência média transmitida.
3. [Componentes necessários](#componentes-necessários)
   - Arduino Nano
   - Motor DC 5V
   - Bateria 5V
   - Botão momentâneo
   - Ponte H L293D
   - Resistor 10K
5. [Esquemático](#esquemático)
![Arduino_PWM_Controller](https://github.com/brunocatani/Arduino_PWM_Controller/assets/94939071/faa356e3-f7d6-45cc-90af-e1123b4cf3c5)

6. [Código-fonte](#código-fonte)
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
  pinMode(PWM, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);
}

void loop() {

  int leitura = digitalRead(BUTTON_PIN);

  if (leitura != ultimo_estado_botao) {
    ultimo_estado_botao = leitura;
    if (leitura == HIGH) {  
      tempo_acionado = millis();
    }
  }

  if (leitura == HIGH && ((millis() - tempo_acionado) > tempo_delay)) {
    pwm += 64;
    if (pwm > 255) {
          pwm = 0;
          }
  }

  analogWrite(PWM, pwm);
  delay(50);
}

````

8. [Instruções de montagem](#instruções-de-montagem)
9. [Funcionamento do projeto](#funcionamento-do-projeto)
