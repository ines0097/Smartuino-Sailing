# Smartuino-Sailing
![Polytech](http://www.polytechnice.fr/jahia/jsp/jahia/templates/inc/img/polytech_nice-sophia.png)

Ce projet est réalisé dans le cadre de la formation de prépa intégrée de Polytech'Nice Sophia

# Smartuino-Sailing




# Description du projet

Le Smartuino-Sailing sera un outils qui permettra aux voileux de résumer leurs courses, et sera d'une aide afin de prendre            les meilleurs décisions et limiter les erreurs tactiques liées aux oscillations du vent sur le plan d'eau.
       
Le projet consiste à créer un outils qui permet aux compétiteurs de minimiser le nombres d'erreurs pendant une course et de leurs fournir un résumé des manches.

Afin de comprendre l'utilité du Smartuino-Sailing il faut comprendre le fonctionnement des manches pendant une régate.

Les explications sont sur la présentation pdf .

# Materiel utilisé

Notre projet est basé sur

une carte Arduino Xplained d'Atmel,

un GPS: <https://www.ebay.fr/itm/NEO-7M-GPS-Module-Built-in-Data-Memory-with-Antenna-and-USB2TTL-Replace-NEO-6M/222564891312?hash=item33d1e6bab0:g:7ukAAOSwbopZVlxj>

![Gps.jpg](https://s20.postimg.org/uxypoz2x9/Gps.jpg)

un anémomètre: <https://www.lextronic.fr/temperature-meteo/19999-capteur-anenometre.html>

![image.jpg](https://s20.postimg.org/4s7glxu1p/image.jpg)

une girouettte: <https://www.lextronic.fr/temperature-meteo/27643-capteur-girouette.html> :"La girouette génère une variation de la valeur de sa résistance en fonction de son orientation. Cette variation n'est pas proportionelle à la direction (consultez la notice de la girouette pour plus d'infos). La résistance de la girouette est utilisée en pont diviseur avec une résistance de 10 K ohms afin de pouvoir générer une tension appliquée sur l'entrée du convertisseur "A/N" A0 du "CB210". Nous avons relevé les valeurs des tensions des 16 positions de la girouette et nous les avons stockée dans une variable de type "tableau". Le programme va ensuite récupérer la valeur de la tension analogique de la girouette et soustraire cette valeur à celles (des tensions des 16 positions) que nous avions stocké. Si la différence de cette soustraction est faible (nous avons choisi une tolérance de 6 unités pour couvrir la tolérance de la conversion "A/N"),le programme en déduit alors la position de la girouette et l'affiche via un pavé en dessous d'une >échelle (N E S O)." LEXTRONIX 
![gir.jpg](https://s20.postimg.org/i94f4wz99/gir.jpg)

un encodeur rotatif:  <https://www.ebay.fr/itm/5218-Module-encodeur-rotatif-Arduino-rotary-encoder/142256956846?hash=item211f2ce5ae:m:mNvEJYsxPSWNcwDr5aozQKQ>

![encod.jpg](https://s20.postimg.org/griyti7f1/encod.jpg)

# Concept physique
Comme montré sur le diapo on travaillera sur 3 vecteurs: 
* Vecteur vent vitesse : VV (en blue)
* Vecteur vent apparent :VA (en noir)
* Vecteur vent réel:VR (en rouge)

![photoVentApp](https://s20.postimg.org/kekxu3yjh/Vent_App.png)

![calcul_VR.jpg](https://s20.postimg.org/71qzjhvrf/calcul_VR.jpg)


