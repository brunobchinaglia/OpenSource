# OpenSource
Repositório destinado para a realização da disciplina de Open Source

O projeto escolhido foi o problema 9 do link:https://github.com/Franzininho/franzininho-c0-exemplos-stm32cubeide

## Código do Pograma
```INO
// O potenciometro como controle do servo motor.

#include <Servo.h>
#include <math.h>

// Definindo pinos de entrada analógica para o potenciômetro
#define potPin A1

// Definindo pino de saída do servo motor
#define servo3Pin 6

// Definindo valores máximos e mínimos do potenciometro
#define value_pot_min 41
#define value_pot_max 1014

// Variaveis de valor do potenciometro
int  value_pot;

// Variaveis do servo motor
Servo s3;
int pos3;
int count = 0, maior, menor, soma,
    posicoes3[20] = {90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90};

void setup() {

  // Declarando o servo motor
  s3.attach(servo3Pin);

  // Setando a posição do servo motor para 90
  s3.write(90);

  Serial.begin(9600);
}

void loop() {

  // Lendo valor do potenciômetro
  value_pot = 0;
  for(int i = 0; i < 5; i++)
    value_pot += analogRead(potPin);
  value_pot /= 5;

  
  // Alterando a posição do servo motor 3 com o potenciômetro
  // A filtragem foi feita para o cálculo da posição deste servo motor
  pos3 = (int) ((50 * log(value_pot)  - 180)/10) * 10 + 10;
  posicoes3[count++] = pos3;
  if(count == 20) count = 0;
  maior = 0;
  menor = 0;
  soma = 0;
  for(int i = 1; i < 20; i++){
    if(posicoes3[i] > posicoes3[maior]){
      maior = i;
    }
    if(posicoes3[i] < posicoes3[menor])
      menor = i;
    soma += posicoes3[i];
  }
  soma -= posicoes3[maior];
  soma -= posicoes3[menor];
  pos3 = soma/18;

  s3.write(pos3);
  delay(100);

}
```
