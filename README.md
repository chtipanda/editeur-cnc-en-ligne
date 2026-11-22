# Contrôle CNC GRBL en WiFi (ESP3D)

Une page web unique, sans installation, pour piloter une CNC sous **GRBL / grblHAL / FluidNC** à travers un module **ESP3D** en WiFi. L’interface s’inspire d’OpenBuilds CONTROL, dont le développement est arrêté. Le code est écrit indépendamment.

Gratuit, pensé pour les hobbyistes : on ouvre le fichier dans le navigateur, on entre l’adresse IP de l’ESP3D, et on usine.

> **Version de test.** Elle n’a pas encore été validée sur toutes les cartes et tous les firmwares. Fais les premiers essais sans fraise ni pièce, et garde un arrêt d’urgence physique à portée de main. Le bouton Reset de la page ne le remplace pas.

## Fonctions

- **Connexion WiFi** par WebSocket à l’ESP3D (IP, port et chemin réglables), avec mémorisation de l’adresse.
- **Position pièce et position machine** affichées en direct pour X, Y et Z, avec l’état de la machine (Idle, Run, Alarm…).
- **setzero par axe** : fixe le zéro pièce de l’axe et mémorise sa valeur, par système de coordonnées (G54 à G59).
- **Homing individuel par axe** (`$HX`, `$HY`, `$HZ`) et homing complet (`$H`).
- **Boutons G28 et G30** : Z monte d’abord à une hauteur machine réglable, puis seuls X et Y rejoignent la position prédéfinie, pour éviter les brides et les obstacles.
- **Déplacement manuel** : pas fixes (0.1, 1, 10, 100 mm) ou mode continu, vitesse réglable, flèches du clavier et PgUp/PgDn.
- **Surcharge d’avance** (curseur Feed) en temps réel.
- **Console** de dialogue avec le contrôleur, historique des commandes (↑ ↓) et raccourcis `$$`, `$#`, `$G`, `$I`.
- **Fichiers G-code** : ouverture depuis l’ordinateur, ou depuis la **mémoire de l’ESP** et la **carte SD** via le serveur web de l’ESP3D.
- **Éditeur G-code** intégré, **envoi du programme** avec pause, reprise et arrêt, et barre de progression.
- **Vue 3D** du parcours avec la position de l’outil (rotation à la souris).
- **Macros** enregistrées dans le navigateur, et mode sombre.

## Utilisation

1. Télécharge `esp3d-grbl-controle-v3.html`.
2. Ouvre-le **en local** dans Chrome ou Edge (double-clic sur le fichier).
3. Entre l’adresse IP de ton ESP3D, puis clique sur **Connecter**.
4. Si la connexion échoue, ouvre « Avancé » et essaie le port WebSocket `81` ou `8282`, le chemin `/` ou `/ws`, et coche ou décoche le sous-protocole « arduino » selon ton firmware.

### À savoir

- **N’héberge pas la page en https** (GitHub Pages, par exemple) : le navigateur bloque alors les connexions `ws://` et `http://` vers ton réseau local. Utilise le fichier en local.
- Le homing par axe (`$HX`, `$HY`, `$HZ`) existe sur grblHAL et FluidNC. Grbl 1.1 d’origine ne connaît que `$H`.
- Les boutons G28 et G30 vont à une position enregistrée dans le contrôleur avec `G28.1` ou `G30.1`. Règle aussi le champ **Z haut (machine)** : après un homing, c’est la hauteur de dégagement, en coordonnées machine.
- L’onglet **Fichiers SD / mémoire** suppose les adresses `/files` et `/sdfiles` de l’ESP3D. Elles varient selon la version du firmware ; en cas d’échec, la page affiche l’adresse essayée. Le navigateur peut aussi refuser la requête si l’ESP3D n’autorise pas l’accès depuis une page locale (CORS).
- Le programme est envoyé ligne par ligne depuis le navigateur, qui doit donc rester ouvert pendant l’usinage.
- Les zéros mémorisés et les macros sont gardés dans le navigateur, sur cet ordinateur seulement.

## Limites actuelles

- La vue 3D trace les arcs (G2/G3) en lignes droites.
- Pas encore de sonde Z, de nivellement ni de gestion des outils.
- Pas de connexion USB directe : seule la liaison WiFi par ESP3D est prise en charge.

## Contribuer

Les retours sont les bienvenus, surtout les tests sur du matériel réel : indique ton contrôleur, ta version d’ESP3D et le firmware, avec le message d’erreur affiché dans la console de la page.

## Licence

À compléter (par exemple MIT).
