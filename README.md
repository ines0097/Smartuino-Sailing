# Smartuino-Sailing
![Polytech](http://www.polytechnice.fr/jahia/jsp/jahia/templates/inc/img/polytech_nice-sophia.png)

Ce projet est réalisé dans le cadre de la formation de prépa intégrée de Polytech'Nice Sophia

# Smartuino-Sailing




# Description du projet
Pendant la 1ère séance on a commercer à mettre en place le projet.
    
Le Smartuino-Sailing sera un outils qui permettra aux voileux de résumer leurs courses, et sera d'une aide afin de prendre             
les meilleurs décisions et limiter les erreurs tactiques liées aux oscillations du vent sur le plan d'eau.
       
Le projet consiste à créer un outils qui permet aux compétiteurs de minimiser le nombres d'erreurs pendant une course.

Afin de comprendre l'utilité du Smartuino-Sailing il faut comprendre le fonctionnement des manches pendant une régate.

Les explications sont sur le diapo.

# Materiel utilisé

Notre projet est basé sur

une carte Arduino Xplained d'Atmel,

un GPS: <https://www.ebay.fr/itm/NEO-7M-GPS-Module-Built-in-Data-Memory-with-Antenna-and-USB2TTL-Replace-NEO-6M/222564891312?hash=item33d1e6bab0:g:7ukAAOSwbopZVlxj>

un anémomètre: <https://www.lextronic.fr/temperature-meteo/19999-capteur-anenometre.html>

une girouettte: <https://www.lextronic.fr/temperature-meteo/27643-capteur-girouette.html>

un encodeur rotatif:  <https://www.ebay.fr/itm/5218-Module-encodeur-rotatif-Arduino-rotary-encoder/142256956846?hash=item211f2ce5ae:m:mNvEJYsxPSWNcwDr5aozQKQ>

# Concept physique
Comme montré sur le diapo on travaillera sur 3 vecteurs: 
* Vecteur vent vitesse : VV
* Vecteur vent apparent :VA
* Vecteur vent réel:VR

on considére l'angle alpha = l'angle entre VR et VV

on considére l'angle beta = l'angle entre VA et VV

on a VR = sqrt(VA^2+VV^2+(2*VA*VV*cos(beta)))

et a partir de cela on déduit l'angle alpha

alpha = arccos((VA*cos(beta)-VV)/VR)



