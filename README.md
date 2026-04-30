# ESP32 – Serveur Web – Machine Virtuelle Ubuntu

## 1. Présentation du projet

L'objectif de ce projet est de créer un **objet connecté** capable de communiquer avec un serveur web pour envoyer et afficher des informations à distance. Le système repose sur une carte ESP32 connectée au réseau WiFi, un serveur web installé sur une machine virtuelle Ubuntu et une interface web pour visualiser et contrôler le matériel.

## 2. Matériel et technologies utilisés

### Matériel

* Une carte ESP32

* Une LED

* Un buzzer

* Un ordinateur hôte

### Logiciels et technologies

* Arduino IDE

* Ubuntu (Machine virtuelle)

* Apache2

* PHP

* HTML / CSS

* Réseau WiFi

## 3. Architecture générale du système

Voici comment fonctionne le projet :

1. L’ESP32 exécute un programme et se connecte au réseau WiFi.

2. Il envoie une donnée vers un serveur web.

3. Le serveur Ubuntu reçoit l’information.

4. Un script PHP enregistre la donnée.

5. Le site web affiche la valeur reçue.

6. L’utilisateur peut contrôler les composants électroniques depuis la page web.

## 4. Utilisation d’une machine virtuelle Ubuntu

Une machine virtuelle permet de créer un serveur web sans modifier le système principal de l’ordinateur. Cela offre plusieurs avantages :

* Sécurité pour le système principal

* Possibilité de recommencer facilement l’installation

* Environnement similaire aux serveurs professionnels

Des logiciels de virtualisation tels que VirtualBox ou VMware peuvent être utilisés.

## 5. Installation du serveur Web

Pour installer le serveur web, les commandes suivantes sont exécutées dans le terminal Ubuntu :

```bash

sudo apt update

sudo apt install apache2 php libapache2-mod-php

```

## 6. Méthode de développement

Le projet est réalisé progressivement pour faciliter la détection des erreurs :

1. Test de la LED

2. Test du buzzer

3. Connexion WiFi

4. Communication avec le serveur

5. Intégration complète du système

## 7. Test de la LED

L’objectif est de vérifier le bon fonctionnement de l’ESP32 et de valider l’envoi du programme.

```cpp

int led = 2;

void setup() {

pinMode(led, OUTPUT);

}

void loop() {

digitalWrite(led, HIGH);

delay(1000);

digitalWrite(led, LOW);

delay(1000);

}

```

## 8. Test du buzzer

L’objectif est de vérifier les sorties numériques et de contrôler la génération d’un signal électrique.

```cpp

int buzzer = 15;

void setup() {

pinMode(buzzer, OUTPUT);

}

void loop() {

digitalWrite(buzzer, HIGH);

delay(1000);

digitalWrite(buzzer, LOW);

delay(1000);

}

```

## 9. Communication entre ESP32 et serveur Web

L’ESP32 envoie une requête HTTP vers le serveur Ubuntu. Un script PHP reçoit la donnée, l’enregistre dans un fichier et affiche la dernière valeur reçue.

### Script PHP

```php

<?php

error_reporting(E_ALL);

ini_set('display_errors', 1);

$file = __DIR__. “/valeur.txt";

if ($_SERVER['REQUEST_METHOD'] === 'POST') {

if (isset($_POST['valeur'])) {

$valeur = $_POST['valeur'];

file_put_contents($file, $valeur);

echo “Valeur enregistrée : “. $valeur;

}

exit;

}

if (file_exists($file)) {

echo file_get_contents($file);

} else {

echo “Aucune valeur reçue.";

}

?>

```

## 10. Interface Web

La page web permet d’afficher automatiquement la dernière valeur reçue et de contrôler à distance la LED et le buzzer.

```html

<button onclick="fetch('http://IP ESP32/led')">

Allumer LED

</button>

<button onclick="fetch('http://IP ESP32/son')">

Allumer Buzzer

</button>

```

## 11. Fonctionnement complet du système

1. Démarrage de l’ESP32

2. Connexion au réseau WiFi

3. Envoi d’une donnée vers le serveur

4. Réception par Ubuntu

5. Traitement et enregistrement par PHP

6. Affichage sur le site web

7. Commande distante des composants électroniques

## 12. Résultat obtenu

Le projet aboutit à la création d’un objet connecté capable de communiquer avec un serveur web, de transmettre des données via Internet, d’afficher les informations en temps réel et de contrôler des composants électroniques à distance.

## 13. conclusion

Ce projet permet de mettre en pratique plusieurs notions essentielles comme la programmation embarquée, la communication réseau, l’administration serveur Linux, le développement web et le principe de fonctionnement des objets connectés (IoT). Il constitue une introduction complète à la conception d’un système IoT fonctionnel.
