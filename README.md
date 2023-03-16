# Tímaverkefni 2

## 1. Teljari

### 1.

### 2.

```cpp
#include "SevSeg.h"
SevSeg sevseg;

int i = 0;

void setup()
{
	//Set to 1 for single digit display
	byte numDigits = 1;

	//defines common pins while using multi-digit display. Left empty as we have a single digit display
	byte digitPins[] = {};

	//Defines arduino pin connections in order: A, B, C, D, E, F, G, DP
	byte segmentPins[] = {3, 2, 8, 7, 6, 4, 5, 9};
	bool resistorsOnSegments = true;

	//Initialize sevseg object. Uncomment second line if you use common cathode 7 segment
	//sevseg.begin(COMMON_ANODE, numDigits, digitPins, segmentPins, resistorsOnSegments);
	sevseg.begin(COMMON_CATHODE, numDigits, digitPins, segmentPins, resistorsOnSegments);

	sevseg.setBrightness(90);
}

void loop()
{ 
   //Display numbers one by one with 2 seconds delay
  sevseg.setNumber(i % 10);
  sevseg.refreshDisplay(); 
  delay(2000);
  i++;
}
```

## 2. Teningur

### 1.

```cpp
#include "SevSeg.h"
SevSeg sevseg;

const int takki = 10;
bool takka_stada = HIGH;

void setup()
{
  pinMode(takki, INPUT_PULLUP);
	//Set to 1 for single digit display
	byte numDigits = 1;

	//defines common pins while using multi-digit display. Left empty as we have a single digit display
	byte digitPins[] = {};

	//Defines arduino pin connections in order: A, B, C, D, E, F, G, DP
	byte segmentPins[] = {3, 2, 8, 7, 6, 4, 5, 9};
	bool resistorsOnSegments = true;

	//Initialize sevseg object. Uncomment second line if you use common cathode 7 segment
	//sevseg.begin(COMMON_ANODE, numDigits, digitPins, segmentPins, resistorsOnSegments);
	sevseg.begin(COMMON_CATHODE, numDigits, digitPins, segmentPins, resistorsOnSegments);

	sevseg.setBrightness(90);

  randomSeed(analogRead(A0));

}

void loop()
{
  takka_stada = digitalRead(takki);
  if(takka_stada == LOW) {
    int tala = random(1, 6);
    sevseg.setNumber(tala);
    sevseg.refreshDisplay();
    delay(1000);
  }
}
```

### 2.

```cpp
#include "SevSeg.h"
SevSeg sevseg;

const int takki = 10;
bool sidasta_takka_stada = LOW;
bool takka_stada = LOW;

void setup()
{
  pinMode(takki, INPUT_PULLUP);
	//Set to 1 for single digit display
	byte numDigits = 1;

	//defines common pins while using multi-digit display. Left empty as we have a single digit display
	byte digitPins[] = {};

	//Defines arduino pin connections in order: A, B, C, D, E, F, G, DP
	byte segmentPins[] = {3, 2, 8, 7, 6, 4, 5, 9};
	bool resistorsOnSegments = true;

	//Initialize sevseg object. Uncomment second line if you use common cathode 7 segment
	//sevseg.begin(COMMON_ANODE, numDigits, digitPins, segmentPins, resistorsOnSegments);
	sevseg.begin(COMMON_CATHODE, numDigits, digitPins, segmentPins, resistorsOnSegments);

	sevseg.setBrightness(90);

  randomSeed(analogRead(A0));
}

bool debounce(bool sidasta_takka_stada) {
  bool takka_stada = digitalRead(takki);
  if (sidasta_takka_stada != takka_stada) {
    delay(5);
    takka_stada = digitalRead(takki);
  }
  return takka_stada;
}

void loop()
{
  takka_stada = debounce(sidasta_takka_stada);
  if(takka_stada == LOW && sidasta_takka_stada == HIGH) {
    int tala = random(1, 6);
    sevseg.setNumber(tala);
    sevseg.refreshDisplay();
    delay(1000);
  }
  sidasta_takka_stada = takka_stada;
}
```

## 3. 7-Segment fjórir tölustafir

![7-Segment](https://user-images.githubusercontent.com/117899282/225647983-35553049-f585-434e-a41d-b053a8ad54a9.jpg "7-Segment display")

![7-Segment](C:\Users\kriss\Downloads\20230315_091022.jpg "7-Segment display")

```cpp
#include "SevSeg.h"
SevSeg sevseg; 

void setup(){
  byte numDigits = 4;
  byte digitPins[] = {10, 11, 12, 13};
  byte segmentPins[] = {9, 2, 3, 5, 6, 8, 7, 4};

  bool resistorsOnSegments = true; 
  bool updateWithDelaysIn = true;
  byte hardwareConfig = COMMON_CATHODE; 
  sevseg.begin(hardwareConfig, numDigits, digitPins, segmentPins, resistorsOnSegments);
  sevseg.setBrightness(90);
}

void loop(){
    sevseg.setNumber(1234, 3);
    sevseg.refreshDisplay(); 
}
```

## 4. BOBA

```cpp
#include "SevSeg.h"
SevSeg sevseg;

float displayTimeSecs = 1;
float displayTime = (displayTimeSecs * 5000);
long buzzerFrequency = 500;
float buzzerDuration = (displayTimeSecs * 100);
long startNumber = 60;
long endNumber = 0;

int fani = 1;
int endahljod = 1;

const int buzzerPin = A4;

void setup() {
  Serial.begin(9600);

  pinMode(buzzerPin,OUTPUT);
  byte numDigits = 4;
  byte digitPins[] = {10, 11, 12, 13};
  byte segmentPins[] = {9, 2, 3, 5, 6, 8, 7, 4};
  
  bool resistorsOnSegments = false;
  byte hardwareConfig = COMMON_CATHODE;
  bool updateWithDelays = false;
  bool leadingZeros = true;
  bool disableDecPoint = true;
  
  sevseg.begin(hardwareConfig, numDigits, digitPins, segmentPins, resistorsOnSegments,
  updateWithDelays, leadingZeros, disableDecPoint);
  sevseg.setBrightness(90);

  pinMode(A2, INPUT_PULLUP);
  pinMode(A3, INPUT_PULLUP);
}

void loop() {

  if (startNumber >= endNumber) {
    for (long i = 0; i <= displayTime; i++){
      sevseg.setNumber(startNumber,0);
      sevseg.refreshDisplay();
    }

    if (digitalRead(A3) == HIGH) {
      startNumber = endNumber;
    } else if (digitalRead(A2) == HIGH && fani == 1) {
      displayTime = displayTime / 2;
      fani -= 1;
    }
  }

sevseg.setNumber(1234,0);
sevseg.refreshDisplay();

if (startNumber > endNumber) {
  tone(buzzerPin,buzzerFrequency,buzzerDuration);
  startNumber --;
} else if (endahljod == 1) {
    for (int i = 0; i < 6; i++) {
      tone(buzzerPin,buzzerFrequency + i * 100,buzzerDuration);
      sevseg.setNumber(startNumber,0);
      sevseg.refreshDisplay();
      delay(buzzerDuration);
    }
    startNumber --;
    endahljod = 0;
  }
}
```