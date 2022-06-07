#include <IRremote.h>
#include <SimpleSDAudio.h>

int kumandaPin = 2;
int buzzer = 8;

IRrecv kumanda(kumandaPin);
decode_results sonuclar;

int led=10;
int mz80=3;
int mz801=4;
int mz802=5;
int trigPin = 7;
int echoPin = 6;
long sure;
long mesafe;
void setup() {
  pinMode(led,OUTPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(mz80,INPUT);
  pinMode(mz801,INPUT);
  pinMode(mz802,INPUT);
  kumanda.enableIRIn();
  pinMode(buzzer, OUTPUT);

  Serial.begin(9600);

}

void loop() {
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(1000);
  digitalWrite(trigPin, LOW);

  sure = pulseIn(echoPin, HIGH);
  mesafe = (sure/2)/29.1;
if(mesafe <= 25){
  if(mesafe <= 10)
  {
    
    digitalWrite(led, HIGH);
  }
  else if(mesafe <= 15)
  {
    
    digitalWrite(led, HIGH);
    delay(100);
    digitalWrite(led, LOW);
    delay(100);
  }
  else if(mesafe <= 20)
  {
    digitalWrite(led, HIGH);
    delay(150);
    digitalWrite(led, LOW);
    delay(150);
  }
  else if(mesafe <= 25)
  {
    digitalWrite(led, HIGH);
    delay(200);
    digitalWrite(led, LOW);
    delay(200);
  }
  else
  {
    digitalWrite(led, LOW);
  }
}

  else{
  int deger1=digitalRead(mz80);
  int deger2=digitalRead(mz801);
  int deger3=digitalRead(mz802);
  int degertop = deger1 * deger2 * deger3;
  if (degertop==0)
  {
    digitalWrite(led,HIGH);
    }
    else
    {
      digitalWrite(led,LOW);
      }
if(kumanda.decode(&sonuclar))
  {
    Serial.print("Tuş kodu: ");
    Serial.println(sonuclar.value, HEX);
    kumanda.resume();
    if(sonuclar.value == 0xFFFFFFFF)
    {
      digitalWrite(buzzer, HIGH);
      delay(50);
      digitalWrite(buzzer, LOW);
      delay(50);
      digitalWrite(buzzer, HIGH);
      delay(50);
      digitalWrite(buzzer, LOW);
    }
      else
      digitalWrite(buzzer, LOW);
    }
    
    }
  
}
    
