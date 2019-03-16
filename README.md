# Hackfest
Autonomous waiter/clerk
#include <ESP8266WiFi.h>
int h = 0;
const char* ssid = "Mr.A";
const char* password = "ayan1999";
int ir1 = 12, ir2 = 13, ir3 = 15, m1a = 16, m1b = 5, m2a = 4, m2b = 2;
void setup() {
  Serial.begin(115200);

  pinMode(ir1, INPUT);
  pinMode(ir2, INPUT);
  pinMode(ir3, INPUT);
  pinMode(m1a, OUTPUT);
  pinMode(m1b, OUTPUT);
  pinMode(m2a, OUTPUT);
  pinMode(m2b, OUTPUT);
  digitalWrite(m1a, LOW);
  digitalWrite(m1b, LOW);
  digitalWrite(m2b, LOW);
  digitalWrite(m2a, HIGH);
  delay(100);
  digitalWrite(m2a, LOW);
}
void loop() {
  int b; int value = LOW;
  if (Serial.available() > 0)
    b = Serial.read();
  if (b == 1)
  { Serial.println("1");
    if (h < 1)
      junction(1 - h);
    else
    {
      turn();
      junction(h - 1);
    }
  }
  else if (b == 2) {
    Serial.println("2");
    if (h < 2)
      junction(2 - h);
    else
    {
      turn();
      junction(h - 2);
    }
  }
  else if (b == 3) {
    Serial.println("3");
    junction(3 - h);
  }
  else if (b == 4) {
    turn();
    junction(h);
  }
  else if (b == 5) {
    stopb();
  }
}
void junction(int n)
{ /*  pinMode(ir1, INPUT);
    pinMode(ir2, INPUT);
    pinMode(ir3, INPUT);
    pinMode(m1a, OUTPUT);
    pinMode(m1b, OUTPUT);
    pinMode(m2a, OUTPUT);
    pinMode(m2b, OUTPUT);
    digitalWrite(ledPin, LOW);*/
  int c = 0;

  while (c == (n + 1))
  {

    int a = 0;
    if (digitalRead(ir1) == HIGH && digitalRead(ir3) == HIGH && digitalRead(ir2) == HIGH)
      a = 1;
    while (digitalRead(ir1) == LOW && digitalRead(ir3) == LOW && digitalRead(ir2) == HIGH)
    {
      forward();
    }
    while (digitalRead(ir1) == HIGH && digitalRead(ir3) == LOW && digitalRead(ir2) == HIGH)
    {
      right();
    }
    while (digitalRead(ir1) == LOW && digitalRead(ir3) == HIGH && digitalRead(ir2) == HIGH)
    {
      left();
    }
    while ((digitalRead(ir1) == HIGH) && (digitalRead(ir3) == HIGH) && digitalRead(ir2) == HIGH)
    { if (a == 1)
        c++;
      a = 0; forward();
    }
    stopb();
    h = n;
  }
  return;
}

void turn()
{
  right();
  delay(1000);
  while (!(digitalRead(ir1) == LOW && digitalRead(ir3) == LOW && digitalRead(ir2) == HIGH))
    right();
}
void forward()
{
  digitalWrite(m1a, HIGH);
  digitalWrite(m2a, HIGH);
  digitalWrite(m1b, LOW);
  digitalWrite(m2b, LOW);
  delay(10);
}
void right()
{
  digitalWrite(m1a, LOW);
  digitalWrite(m2a, HIGH);
  digitalWrite(m1b, LOW);
  digitalWrite(m2b, LOW);
  delay(100);
}
void left()
{
  digitalWrite(m1a, HIGH);
  digitalWrite(m2a, LOW);
  digitalWrite(m1b, LOW);
  digitalWrite(m2b, LOW);
  delay(100);
}
void back()
{
  digitalWrite(m1a, LOW);
  digitalWrite(m2a, LOW);
  digitalWrite(m1b, HIGH);
  digitalWrite(m2b, HIGH);
  delay(100);
}
void stopb()
{
  digitalWrite(m1a, LOW);
  digitalWrite(m2a, LOW);
  digitalWrite(m1b, LOW);
  digitalWrite(m2b, LOW);
  delay(100);

}
