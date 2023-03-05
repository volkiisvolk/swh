---
title: "Microcomputer mit Arudino´s"
date: 2022-12-05T14:04:47+01:00
featured_image: "../images_micro/sound.jpeg"
description: "Arudino Mechanik"
draft: false
---

## Mein erstesmal mit einem Microcontroller
Am Mittwoch den 23.11.2022 hatte ich die Möglichkeit, meinen ersten Microcontroller in der Hand zu halten. Anfangs hatte ich Angst, irgendwelche Fehler zu machen und so etwas zu beschädigen, doch durch ein wenig Probieren und der Vorlesung wurde mir die Angst genommen.


## Versuch 1: Es Werde Licht

Nachdem wir unsere ersten Arduinos erhalten haben, wurden auch schon breadboards und verkablungen verteilt. Unser erste Aufgabe war es LED´s zum leuchten zu bringen. Dies war sehr einfach, denn es genügte den Ground Port vom Arduino mit dem - pol vom Breadboard zu verbinden und einen GPIO port mit dem + Pol. Dazu musste man natürlich auch noch zwischenverkablungen zur LED legen und in der Arduino IDE den Port programmieren.




{{< figure src="../images_micro/led.jpeg"  width="60%" height="60%">}}


Gleich nachdem wir die erste LED eigebaut haben, habe ich versucht, eine zweite dazu zu schalten. Disese haben dann abwechselnd geleuchtet.

## Versuch 2:Es wird Lauter

Hier hab ich gleich mal mit Sensoren herumgespielt. Der erste war einer der Geräausche macht. Die Lautstärke ließ sich letztendlich auch einstellen.


{{< figure src="../images_micro/sound.jpeg"  width="60%" height="60%">}}



## Versuch 3: Displays sind schwerer als gedacht.

So selbstbewusst wie ich nach meinen ersten erfolgen war, habe ich mich gleich mal an so ein Display gewagt. Nach langem hin und her musste ich dann leider der Wahrheit ins Auge sehen, und akzeptieren, dass das wohl doch zu voreilig war. Alleine das zusammenstecken war nicht leicht, das Programmieren jedoch war für meinen jetzigen Standpunkt, noch zu schwer.

{{< figure src="../images_micro/dis.jpeg"  width="60%" height="60%">}}


## Versuch 4: Serielle Kommunikation
In der Seriellen Kommunikation habe ich mich an einer LED Matrix dran gesetzt. Das hier ist mein Code


{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}

#include "LedControl.h"

LedControl lc=LedControl(12,11,10,1);

void setup() {
  lc.shutdown(0,false);

  lc.setIntensity(0,8);

  lc.clearDisplay(0);
}

void writeArduinoOnMatrix() {
 
  byte a[5]={B01111110,B10001000,B10001000,B10001000,B01111110};

  lc.setRow(0,0,a[0]);
  lc.setRow(0,1,a[1]);
  lc.setRow(0,2,a[2]);
  lc.setRow(0,3,a[3]);
  lc.setRow(0,4,a[4]);
}


{{< / highlight >}}

Dadurch hab ich es geschafft, dass meine LED Matrix ein A Anzeigt

## Was genau ist I2C Serielle Kommunikation

{{< figure src="../images/matrix.jpeg"  width="60%" height="60%">}}

Dieser basiert auf dem Master-Slave-Prinzip und besteht aus 2 Leitunden, einer der Leitungen ist die Daten- und die andere die Taktleitung. Diese sind im Idle-Zustand und werden durch Pullupwiderstände auf hohem Pegel gehalten. Der Takt wird durch den Master genertiert, aber kann vom Slave solange auf 0 gehalten werden, bis der Slave weider bereit ist.

Das besondere eben ist, dass es durch 2 Leitungen möglich ist bis zu 128 Teilnehmen zu lassen. Also es kann mit 128 Teilnehmern kommuniziert werden. Dies funktioniert durch serielle Datenübertragung. Der I2C-Bus definiert den Ablauf.
