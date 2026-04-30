Dans ce projet, nous allons créer notre propre objet connecté en utilisant :

une carte ESP32 (microcontrôleur avec WiFi intégré),
un site web créé par nous,
une machine virtuelle Ubuntu servant de serveur,
des composants électroniques simples :
une LED,
un buzzer.

étape 1:
L’ESP32 exécute un programme.
Il se connecte au réseau WiFi.
Il envoie une information vers un serveur web.
Le serveur reçoit la donnée.
Le site web affiche la valeur reçue.
Donc :
ESP32 → Internet → Serveur Ubuntu → Site Web

3️⃣ Pourquoi utiliser une machine virtuelle Ubuntu ?
Un site web a besoin d’un serveur pour fonctionner. Au lieu d’utiliser un serveur physique, vous pouvez créer une machine virtuelle.
C'est simplement un ordinateur installé à l’intérieur de votre ordinateur.
Les avantages sont nombreux :
* aucun risque pour votre PC principal,
* facile à recommencer en cas d’erreur,
* très utilisé dans le monde professionnel.
Installation d’Ubuntu
Pour installer Ubuntu, vous aurez besoin d’un logiciel tel que :
* VirtualBox
* VMware
La machine virtuelle Ubuntu servira de serveur web local.
4.Installation du serveur Web
Un serveur web permet :

d’héberger un site,
de recevoir les données envoyées par l’ESP32.
Dans Ubuntu, on ouvre le terminal et on installe :
sudo apt update
sudo apt install apache2 php libapache2-mod-php

Apache servira à un afficher notre site web 
Le PHP servira a traiter les information envoyé
libapache2-mod-php on installe ceci dans le la machine virtuelle (Ubuntu) pur pouvoir relié Apache et PHP

5️⃣ Création du site web
Tous les fichiers du site sont stockés dans:
/var/www/html
sur le dossier principale sur le serveur
Pour le dossier PHP
ce fichier sera très important pour notre page Web 
il fera office d'un réceptionniste
* il recevra les donnée envoyé par l'ESP32
* Le PHP reçoit et enregistre la donnée

6️⃣ Pourquoi tester le matériel étape par étape ?
il sera mieux d'avancer progressivement carse serait plus facile à trouver l'erreur 
1. Tester la LED.
2. Tester le buzzer.
3. Ajouter Internet.
4. Fusionner les programmes.
7️⃣ Première étape : Câblage et test de la LED
Rôle de la LED
On utilise d'abord la LED c'est pour vérifier que l'ESP32 fonctionne
que le programme soit bien envoyé 
et enfin que les sortie GPIO fonctionne
utilise se programme après avoir câbler:

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
* HIGH → LED allumée
* LOW → LED éteinte
* delay(1000) → 1 seconde

8️⃣ Deuxième étape : Test du buzzer
Rôle du buzzer
*Le buzzer sert à produire un son.

Il permet de vérifier :
les signaux électriques,
les sorties numériques.
une fois câbler il faut utiliser se programme
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
si un son est entendu c'est qu'il fonctionne

9️⃣ Connexion WiFi de l’ESP32

l'avantage principale de l'ESP32 est son Wifi intégrer 
il peut donc communiquer a distance
programme 
<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

$file = __DIR__ . "/valeur.txt";

// Si c'est une requête POST, on enregistre la valeur
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    if (isset($_POST['valeur'])) {
        $valeur = $_POST['valeur'];
        $result = file_put_contents($file, $valeur);
        if ($result === false) {
            echo "Erreur écriture fichier";
        } else {
            echo "Valeur enregistrée : " . $valeur;
        }
    } else {
        echo "POST reçu mais pas de valeur";
    }
    exit; // On arrête ici pour ne pas afficher le GET aussi
}

// Si c'est une requête GET, on lit et affiche la dernière valeur
if (file_exists($file)) {
    $valeur = file_get_contents($file);
    echo "Dernière valeur reçue : " . htmlspecialchars($valeur);
} else {
    echo "Aucune valeur reçue pour l'instant.";
}
?>

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Valeur Arduino</title>
    <!-- Rafraîchissement automatique toutes les 5 secondes -->
    <meta http-equiv="refresh" content="5">
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin-top: 50px; }
        .valeur { font-size: 2em; color: #007BFF; }
    </style>
</head>
<body>
    <h1>Dernière valeur reçue de l'Arduino</h1>
    <div class="valeur"><?php echo $valeurAffiche; ?></div>
    <p>La page se rafraîchit automatiquement toutes les 5 secondes.</p>
    <button onclick="fetch('http://IP ESP32/led')">
    Allumer Led
    </button>

    <button onclick="fetch('http://IP ESP32/son')">
     Allumer Buzzer
    </button>

</body>
</html>

cela envoyé sur le site les valeur de l'ESP32 il y aura aussi deux bouton "Allumer LED" ou "Allumer le Buzzer"

🔟 Programme final combiné
Le programme va allumer la LED et active le buzzer une fois que le serveur va recevoir la 
donnée 
il faudra appuyé sur les boutons "Allumer LED" ou "Allumer le Buzzer".

1️⃣1️⃣ Fonctionnement complet du système

Voici ce qui se passe réellement :

1. * L’ESP32 démarre.
2. * Il rejoint le WiFi.
3. * Il allume la LED et le buzzer.
4. * Il envoie une donnée.
5. * Ubuntu reçoit l’information.
6. * PHP l’enregistre.
7. * Le site web affiche la valeur


# ESP32-Wifi-serveur-WEB-machine-virtuelle
