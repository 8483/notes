# Arduino

It's a micro-controller.

Diagrams can be made with the tool **Fritzing**.

## Setup

We can select the type of board we are working with at tools > board, or add ones with the boards manager.

We define the way we are connecting to the board via a port at tools > port. It changes each time the board is disconnected.

## Programming

The programming language is C/C++.

```c
void setup() {
  // put your setup code here, to run once:
}

void loop() {
  // put your main code here, to run repeatedly:
}
```

## Upload

The code is compiled and uploaded into the chip via the USB cable. The built-in small LED light will blink in the frequency of the loop to indicate that it's working.

## Serial Monitor

We can see what Arduino outputs via tools > serial monitor. If we are getting weird characters, we need to set the **baud** value to the correct one.

**baud** - Data rate in bits per second for serial data transmission.

## digitalWrite vs analogWrite

`digitalWrite` will set the specified pin to one of two states - HIGH/LOW, which equate to 5v (3.3v on some boards) and ground respectively.

`analogWrite` can vary by the type of output used. It will set the pin to a periodic high/low signal, where the percentage of the signal spent high is proportional to the value written. Ex. `analogWrite(PIN,255)`.

pinMode(buzzerPin, OUTPUT);

## Simple blinking LED example

```c
int LED = 12;

void setup() {
  pinMode(LED, OUTPUT);
}

void loop() {
  digitalWrite(LED, HIGH);
  delay(100);
  digitalWrite(LED, LOW);
  delay(100);
}
```

![TEA](../pics/arduino_led.jpg)

## Cop Car

```c
int LED_RED = 12;
int LED_BLUE = 13;
int BUZZER = 10;

void setup() {
  pinMode(LED_RED, OUTPUT);
  pinMode(LED_BLUE, OUTPUT);
  pinMode(BUZZER, OUTPUT);
}

void loop() {
  tone(BUZZER, 200, 250);
  digitalWrite(LED_RED, HIGH);
  delay(500);
  digitalWrite(LED_RED, LOW);

  tone(BUZZER, 400, 250);
  digitalWrite(LED_BLUE, HIGH);
  delay(500);
  digitalWrite(LED_BLUE, LOW);
}
```
