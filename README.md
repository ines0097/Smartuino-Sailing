# Smartuino-Sailing
![Polytech](http://www.polytechnice.fr/jahia/jsp/jahia/templates/inc/img/polytech_nice-sophia.png)

Ce projet est réalisé dans le cadre de la formation de prépa intégrée de Polytech'Nice Sophia

lien des images: <https://postimg.cc/files>

# Smartuino-Sailing




# Description du projet

Le Smartuino-Sailing sera un outil qui permettra aux sportifs de haut niveau en voile de faire un bilan et une évaluation de leurs courses, et sera d'une aide afin de prendre les meilleurs décisions et limiter les erreurs tactiques liées aux oscillations du vent sur le plan d'eau.
       
Le projet consiste à créer un outil qui permette aux compétiteurs de minimiser le nombres d'erreurs pendant une course et de leurs fournir un résumé des manches.

Afin de comprendre l'utilité du Smartuino-Sailing il faut comprendre comment se déroulent les manches pendant une régate.

Les explications sont sur la présentation pdf .


# Materiel utilisé

Notre projet est basé sur

une carte Arduino Xplained d'Atmel,

un GPS: <https://www.ebay.fr/itm/NEO-7M-GPS-Module-Built-in-Data-Memory-with-Antenna-and-USB2TTL-Replace-NEO-6M/222564891312?hash=item33d1e6bab0:g:7ukAAOSwbopZVlxj>

![Gps.jpg](https://s20.postimg.cc/uxypoz2x9/Gps.jpg)

un anémomètre: <https://www.lextronic.fr/temperature-meteo/19999-capteur-anenometre.html>"L'anémomètre génère la femeture cyclique d'un contact lors de sa rotation. Ce dernier est câblé entre la masse et l'entrée 0 du "CB210" (avec une résistance de tirage au +5V afin de pouvoir récupérer son signal). Le programme va détecter le front descendant du signal, puis décrémenter un compteur dont la valeur sera affichée sous la forme d'un bargraph lors de la détection du front montant du signal. Plus la vitesse de rotation sera important, moins le compteur aura le temps de diminuer et plus sa valeur sera importante (donc plus le bargraph sera "long")." (LEXTRONIX idée de base) nous au lieu du bargraph on veut afficher la vittesse du vent en noeuds.

![image.jpg](https://s20.postimg.cc/4s7glxu1p/image.jpg)

une girouettte: <https://www.lextronic.fr/temperature-meteo/27643-capteur-girouette.html> :"La girouette génère une variation de la valeur de sa résistance en fonction de son orientation. Cette variation n'est pas proportionelle à la direction (consultez la notice de la girouette pour plus d'infos). La résistance de la girouette est utilisée en pont diviseur avec une résistance de 10 K ohms afin de pouvoir générer une tension appliquée sur l'entrée du convertisseur "A/N" A0 du "CB210". Nous avons relevé les valeurs des tensions des 16 positions de la girouette et nous les avons stockée dans une variable de type "tableau". Le programme va ensuite récupérer la valeur de la tension analogique de la girouette et soustraire cette valeur à celles (des tensions des 16 positions) que nous avions stocké. Si la différence de cette soustraction est faible (nous avons choisi une tolérance de 6 unités pour couvrir la tolérance de la conversion "A/N"),le programme en déduit alors la position de la girouette et l'affiche via un pavé en dessous d'une >échelle (N E S O)." LEXTRONIX 

![gir.jpg](https://s20.postimg.cc/i94f4wz99/gir.jpg)

un encodeur rotatif:  <https://www.ebay.fr/itm/5218-Module-encodeur-rotatif-Arduino-rotary-encoder/142256956846?hash=item211f2ce5ae:m:mNvEJYsxPSWNcwDr5aozQKQ>

![encod.jpg](https://s20.postimg.cc/griyti7f1/encod.jpg)

# Concept physique
Comme vu sur les diapo on travaillera sur 3 vecteurs: 
* Vecteur vent vitesse : VV (en bleu)
* Vecteur vent apparent :VA (en noir)
* Vecteur vent réel:VR (en rouge)

![photoVentApp](https://s20.postimg.cc/kekxu3yjh/Vent_App.png)

Sur ce lien vous trouverez un document pdf qu'on a écrit qui résume les étapes qu'on a suivi pour aboutir à la valeur de la direction du vent réel. [cliquer pour la page 1](https://s20.postimg.cc/vfl587inh/Projet-arduino-_V2-page-001.jpg)

[cliquer pour la page 2](https://s20.postimg.cc/jdpre21p9/Projet-arduino-_V2-page-002.jpg)

![calcul_VR.jpg](https://s20.postimg.cc/5mpeuruod/calcul_VR.jpg)

Avec les formules de l'image au dessus: On trouve la la vitesse et la direction du vent réel.

<ul>On compare la direction du VR au <b>Cap de l'Axe du Parcours</b>(AxeParcours): 
       <li>Si la direction du VR > AxeParcours: on est sur le bord rapprochant en TRI donc refusant en babord </li>
       <li>Si la direction du VR < AxeParcours: on est sur le bord refusant en TRI donc rapprochant en babord</li>


# Code

le code utilisé pour essayer l'anémomètre:
<pre><code>
int impulsion_anemometre = 3;           //pin pour compter le nombre d'impulsion 
int compt = 0;  //fonction pour compter le nombre d'impulsio
float vitesse = 0;  //vitesse du vent
float valeur = 2.4;
void setup()
{
  Serial.begin(1200);
  attachInterrupt(1,compteur,RISING); //fonction pour compter le nombre d'interruption
}
void loop()
{
   delay(1000);
   compt = 0;
   Serial.println("vitesse du vent en km/h: 0");
}
void compteur()
{
    compt++;
    Serial.println(compt);
    vitesse = valeur*compt; //calcul de la vitesse du vent
    delay(1000);
    Serial.print("vitesse du vent en km/h:");
    Serial.println(vitesse);
    delay(1000);
}
</pre></code>

le code utilisé pour essayer la girouette:
<pre><code>
void setup() {
Serial.begin(9600);
}
void loop() {
int sensorValue = analogRead(A1);
Serial.println(sensorValue);
delay(100); 
}
</pre></code>

Le code pour le GPS:
<pre><code>
#include <TinyGPS++.h>
#include <SoftwareSerial.h>
/*
   This sample code demonstrates the normal use of a TinyGPS++ (TinyGPSPlus) object.
   It requires the use of SoftwareSerial, and assumes that you have a
   4800-baud serial GPS device hooked up on pins 4(rx) and 3(tx).
*/
static const int RXPin = 2, TXPin = 3;
static const uint32_t GPSBaud = 9600;
// The TinyGPS++ object
TinyGPSPlus gps;
// The serial connection to the GPS device
SoftwareSerial ss(RXPin, TXPin);
void setup()
{
  Serial.begin(9600);
  ss.begin(GPSBaud);
  Serial.println(F("FullExample.ino"));
  Serial.println(F("An extensive example of many interesting TinyGPS++ features"));
  Serial.print(F("Testing TinyGPS++ library v. ")); Serial.println(TinyGPSPlus::libraryVersion());
  Serial.println(F("by Mikal Hart"));
  Serial.println();
  Serial.println(F("Sats HDOP Latitude   Longitude   Fix  Date       Time     Date Alt    Course Speed Card  Distance Course Card  Chars Sentences Checksum"));
  Serial.println(F("          (deg)      (deg)       Age                      Age  (m)    --- from GPS ----  ---- to London  ----  RX    RX        Fail"));
  Serial.println(F("---------------------------------------------------------------------------------------------------------------------------------------"));
}
void loop()
{
  static const double LONDON_LAT = 51.508131, LONDON_LON = -0.128002;
  printInt(gps.satellites.value(), gps.satellites.isValid(), 5);
  printInt(gps.hdop.value(), gps.hdop.isValid(), 5);
  printFloat(gps.location.lat(), gps.location.isValid(), 11, 6);
  printFloat(gps.location.lng(), gps.location.isValid(), 12, 6);
  printInt(gps.location.age(), gps.location.isValid(), 5);
  printDateTime(gps.date, gps.time);
  printFloat(gps.altitude.meters(), gps.altitude.isValid(), 7, 2);
  printFloat(gps.course.deg(), gps.course.isValid(), 7, 2);
  printFloat(gps.speed.kmph(), gps.speed.isValid(), 6, 2);
  printStr(gps.course.isValid() ? TinyGPSPlus::cardinal(gps.course.value()) : "*** ", 6);
  unsigned long distanceKmToLondon =
    (unsigned long)TinyGPSPlus::distanceBetween(
      gps.location.lat(),
      gps.location.lng(),
      LONDON_LAT,
      LONDON_LON) / 1000;
  printInt(distanceKmToLondon, gps.location.isValid(), 9);
  double courseToLondon =
    TinyGPSPlus::courseTo(
      gps.location.lat(),
      gps.location.lng(),
      LONDON_LAT,
      LONDON_LON);
  printFloat(courseToLondon, gps.location.isValid(), 7, 2);
  const char *cardinalToLondon = TinyGPSPlus::cardinal(courseToLondon);
  printStr(gps.location.isValid() ? cardinalToLondon : "*** ", 6);
  printInt(gps.charsProcessed(), true, 6);
  printInt(gps.sentencesWithFix(), true, 10);
  printInt(gps.failedChecksum(), true, 9);
  Serial.println();
  
  smartDelay(1000);
  if (millis() > 5000 && gps.charsProcessed() < 10)
    Serial.println(F("No GPS data received: check wiring"));
}
// This custom version of delay() ensures that the gps object
// is being "fed".
static void smartDelay(unsigned long ms)
{
  unsigned long start = millis();
  do
  {
    while (ss.available())
      gps.encode(ss.read());
  } while (millis() - start < ms);
}
static void printFloat(float val, bool valid, int len, int prec)
{
  if (!valid)
  {
    while (len-- > 1)
      Serial.print('*');
    Serial.print(' ');
  }
  else
  {
    Serial.print(val, prec);
    int vi = abs((int)val);
    int flen = prec + (val < 0.0 ? 2 : 1); // . and -
    flen += vi >= 1000 ? 4 : vi >= 100 ? 3 : vi >= 10 ? 2 : 1;
    for (int i=flen; i<len; ++i)
      Serial.print(' ');
  }
  smartDelay(0);
}
static void printInt(unsigned long val, bool valid, int len)
{
  char sz[32] = "*****************";
  if (valid)
    sprintf(sz, "%ld", val);
  sz[len] = 0;
  for (int i=strlen(sz); i<len; ++i)
    sz[i] = ' ';
  if (len > 0)
    sz[len-1] = ' ';
  Serial.print(sz);
  smartDelay(0);
}
static void printDateTime(TinyGPSDate &d, TinyGPSTime &t)
{
  if (!d.isValid())
  {
    Serial.print(F("********** "));
  }
  else
  {
    char sz[32];
    sprintf(sz, "%02d/%02d/%02d ", d.month(), d.day(), d.year());
    Serial.print(sz);
  }
  
  if (!t.isValid())
  {
    Serial.print(F("******** "));
  }
  else
  {
    char sz[32];
    sprintf(sz, "%02d:%02d:%02d ", t.hour(), t.minute(), t.second());
    Serial.print(sz);
  }
  printInt(d.age(), d.isValid(), 5);
  smartDelay(0);
}
static void printStr(const char *str, int len)
{
  int slen = strlen(str);
  for (int i=0; i<len; ++i)
    Serial.print(i<slen ? str[i] : ' ');
  smartDelay(0);
}
</pre></code>


