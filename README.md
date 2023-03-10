# Tímaverkefni 2

## 1. Teljari

### 1.

### 2.

```cpp
#include "SevSeg.h"
SevSeg sevseg;

int i = 0

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
  sevseg.setNumber(i);
  sevseg.refreshDisplay(); 
  delay(2000);
  i++
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