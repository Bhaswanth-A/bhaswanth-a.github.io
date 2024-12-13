---
title: Smart Agricultural Seeding Robot
date: 2022-08-12 00:00:00 +0800
categories: [Projects, Robotics]
tags: [arduino, electronics]     # TAG names should always be lowercase
subsection: "Academic Projects at BITS Pilani"
author: <author_id>
mermaid: true
pin: false
image: /assets/images/Thumbnail/agribot.png
---

# Electronics

<img src="/assets/images/AgriBot/D3S1_A2.png" alt="drawing" width="700"/>



# Arduino Code

```c++
#include <Servo.h>
Servo myservo;

int botrightpin = 5;
int botleftpin = 6;
int toprightpin = 9;
int topleftpin = 10;
int servopin = 11;
int val=0;
int buzzPin = 3;

void setup()
{
  pinMode(botleftpin, OUTPUT);
  pinMode(botrightpin, OUTPUT);
  pinMode(topleftpin, OUTPUT);
  pinMode(toprightpin, OUTPUT);

    pinMode(buzzPin, OUTPUT);
  myservo.attach(servopin);
  Serial.begin(9600);
}

void loop()
{
  digitalWrite(botleftpin, HIGH);
  digitalWrite(botrightpin, HIGH);
  digitalWrite(topleftpin, HIGH);
  digitalWrite(toprightpin, HIGH);
  
  delay(7000);

  digitalWrite(botleftpin, LOW);
  digitalWrite(botrightpin, LOW);
  digitalWrite(topleftpin, LOW);
  
  digitalWrite(toprightpin, LOW);
  
  myservo.write(90);
  digitalWrite(buzzPin, HIGH);
  delay(200);
  myservo.write(0);
  digitalWrite(buzzPin, LOW);
  delay(2000);
}

```

# Funnel

<img src="/assets/images/AgriBot/funnel1.png" alt="drawing" width="300"/>

<img src="/assets/images/AgriBot/funnel2.png" alt="drawing" width="300"/>

<img src="/assets/images/AgriBot/funnel3.png" alt="drawing" width="300"/>

# Chassis and Bot

<img src="/assets/images/AgriBot/bot1.png" alt="drawing" width="500"/>

<img src="/assets/images/AgriBot/bot2.png" alt="drawing" width="500"/>

<img src="/assets/images/AgriBot/bot3.png" alt="drawing" width="500"/>


<iframe width="800" height="480" src="https://youtube.com/embed/cTRTf_YRMfY" frameborder="0" allowfullscreen></iframe>

