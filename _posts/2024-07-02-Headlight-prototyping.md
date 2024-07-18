---
title: Miata Headlight Prototyping
author: Hunter
date: 2024-07-02 10:57:00 +0800
categories: [Miata, Mods]
tags: [headlight, drl, prototyping, planning]
---

# Headlight Prototyping... Again

Alright, so I'm looking to redo my foggy headlights. The last time I worked on them, they didn't seal properly and ruined the headlight lens. I am considering ordering brand new headlights and swapping the DRLs and MLED into the new housing. The best pricing I've found so far is:

- [Driver Side](https://www.mazdapartsconnect.com/oem-parts/mazda-headlight-lens-and-housing-nc10510l0e?c=Zz1lbGVjdHJpY2FsJnM9aGVhZGxhbXAtY29tcG9uZW50cyZsPTQmbj1Bc3NlbWJsaWVzIFBhZ2UmYT1tYXpkYSZvPW1pYXRhJnk9MTk5OSZ0PWJhc2UmZT0xLThsLWw0LWdhcw%3D%3D) - $256.11
- [Passenger Side](https://www.mazdapartsconnect.com/oem-parts/mazda-lens-and-housing-passenger-side-rh-nc10510k0e?c=Zz1lbGVjdHJpY2FsJnM9aGVhZGxhbXAtY29tcG9uZW50cyZsPTUmbj1Bc3NlbWJsaWVzIFBhZ2UmYT1tYXpkYSZvPW1pYXRhJnk9MTk5OSZ0PWJhc2UmZT0xLThsLWw0LWdhcw%3D%3D) - $256.11

For looks, I was talking to Matt over at [Left Lane Designs](https://leftlanedesigns.net), and he gave me this image. I wasn't a huge fan of it at first, but when I considered this setup with custom DRLs, ooh damn, will it look good.

![Matt's headlights with silver paint](assets/img/headlight/image.png)

So what is this custom DRL you ask? Well, it's basically making my own version of the BMW angel eyes.

![BMW Devil eyes](assets/img/headlight/IMG_5743.jpeg)

But how do we make these? Well, 3D printing of course, and some custom electronics.

Alright, after some planning and other shenanigans happening, I have decided to use:

## LED Parts List

- XIAO ESP32C3 as my microcontroller because it has built-in Bluetooth and is small and compact. My main worry for the ESP32C3 is heat, but this will be monitored in testing.
- A total of 7 TLC59401RHBR as the LED drivers.
  > I need to connect a 41.2Ω resistor to one of the IREF pins of the LED driver, but I will double check this.
  > At the time of writing this section, it is planned to use 101 LEDs: 55 per color of Yellow and Cool White. I do not have accurate measurements at this time.
- LEDs will be the KingBright APTB1612SYKQWDF, some really fancy Bi-LED SMD LEDs with low power usage as well.

## Power Draw Calculations

Each LED channel current: 20mA (set by the IREF resistor).  
Total LEDs: 108.  
Total current for LEDs: 
108 × 0.02A = 2.16A 

TLC59401RHBR drivers:  
Operating current (for logic and internal operations): typically a few milliamps per driver. We'll estimate 10mA per driver for seven drivers: 
7 × 0.01A = 0.07A

XIAO ESP32C3:  
Operating current: typically around 50mA when active.

Total Current Draw = 2.16A + 0.07A + 0.05A = 2.28A  
Total Current = 2.28A

So, knowing the whole system will draw around 2.28A, I will need to find a ~12V to 5V 3A buck converter. 
> Keep in mind when a car is running, voltage will fluctuate because of the alternator to as high as 15V.

## Coding Stuff
Current setup on a test bench setup with an Ardiuno Uno and 11 Single color LEDS. I orginally used delays for the lights but after a post on youtube someone suggested I use Millis for faster animations, used it in a simulator on tinkercard found [here](https://www.tinkercad.com/things/kmN0x3k3XpJ-headlight-animation-test)
```
const int startLEDPin = 3; // Start of LED pins
const int endLEDPin = 13; // End of LED pins
const unsigned long interval = 100; // Interval between LEDs in milliseconds

int currentLEDPin = startLEDPin;
int direction = 1; // 1 for moving right, -1 for moving left
bool animationComplete = false;
unsigned long previousMillis = 0;

void setup() {
  // Initialize all the LED pins as outputs
  for (int pin = startLEDPin; pin <= endLEDPin; pin++) {
    pinMode(pin, OUTPUT);
    digitalWrite(pin, LOW); // Ensure all LEDs are off initially
  }
}

void loop() {
  if (!animationComplete) {
    unsigned long currentMillis = millis();

    // Check if it's time to move to the next LED
    if (currentMillis - previousMillis >= interval) {
      // Move to the next LED
      digitalWrite(currentLEDPin, LOW); // Turn off the current LED
      currentLEDPin += direction;
      
      // Check for direction change
      if (currentLEDPin > endLEDPin) {
        direction = -1; // Change direction to left
        currentLEDPin = endLEDPin - 1;
      } else if (currentLEDPin < startLEDPin) {
        direction = 1; // Change direction to right
        currentLEDPin = startLEDPin + 1;
        animationComplete = true; // Animation is complete
      }

      digitalWrite(currentLEDPin, HIGH); // Turn on the next LED
      previousMillis = currentMillis; // Save the time of the last update
    }
  } else {
    // Turn on all LEDs after animation is complete
    for (int pin = startLEDPin; pin <= endLEDPin; pin++) {
      digitalWrite(pin, HIGH);
    }
  }
}

```
