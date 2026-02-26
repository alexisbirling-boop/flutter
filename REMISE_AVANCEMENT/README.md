# Remise d’avancement

## Instructions d’installation et d’utilisation

Voici un guide pour l’installation des différents répertoires permettant à wger de fonctionner.

---

## Installer le projet backend (Python)

### Prérequis

- Avoir l’application Docker Desktop installée
- Avoir un IDE de votre choix (j’utilise PyCharm)
- Avoir Git installé
- Avoir un interpréteur Python installé

### Préparation

- Cloner le projet Docker (https://github.com/wger-project/docker) sur votre ordinateur
- Cloner le projet Wger (https://github.com/alexisbirling-boop/wger) sur votre ordinateur à partir de mon fork

### Étapes

1. Ouvrir le projet Docker
2. Aller dans le dossier `dev`

```
cd docker/dev
```

3. Copier le fichier `.env.example` dans un nouveau fichier `.env`

```
cp .env.example .env
```

4. Inscrire le chemin du projet Wger cloné précédemment dans la variable `WGER_CODEPATH`

```
WGER_CODEPATH=C:\A_Documentation_Natif\wger
```

(Votre chemin variera selon l’emplacement de sauvegarde. Vous pouvez copier le chemin depuis l’Explorateur de fichiers.)

5. Ouvrir Docker Desktop
6. Lancer Docker à partir du dossier `dev`

```
docker compose up --watch
```

7. Exécuter les commandes suivantes (cela peut prendre un certain temps)

```
docker compose exec web /bin/bash
wger bootstrap
python3 manage.py sync-exercises
wger load-online-fixtures
```

8. Démarrer le serveur de développement

```
python3 manage.py runserver 0.0.0.0:8000
```

9. Accéder au serveur local : http://localhost:8000
10. Se connecter avec un compte administrateur

```
username: admin
mot de passe: adminadmin
```

---

## Important

Pour démarrer le projet lors des prochaines utilisations, exécuter uniquement les commandes suivantes depuis le dossier `dev`, avec Docker Desktop ouvert :

```
docker compose up --watch
docker compose exec web /bin/bash
python3 manage.py runserver 0.0.0.0:8000
```

Lors de ma première installation du projet, j’ai dû modifier le `Dockerfile` dans le projet Wger pour que le tout fonctionne. Lors de ma deuxième installation avec un nouveau clone, je n’ai pas eu à effectuer cette modification. Il est donc possible qu’elle ne soit pas obligatoire. Je l’inscris tout de même ici au cas où une erreur surviendrait lors de l’installation.

1. Aller dans le dossier `extras/docker/development`
2. Ouvrir le fichier `Dockerfile`
3. Remplacer les lignes suivantes :

```
RUN wget -O- https://deb.nodesource.com/setup_22.x | bash - \
    && apt update \
    && apt install --no-install-recommends -y \
```

par :

```
RUN wget -O- https://deb.nodesource.com/setup_22.x | bash && \
    apt update && \
    apt install --no-install-recommends -y \
```

---

## Utiliser le projet backend (Python)

### Fonctionnalités

Les fonctionnalités sont les mêmes que celles décrites plus loin dans la section Android. Je ne les répéterai donc pas ici.  
L’intérêt principal du backend est d’avoir accès à l’API.

### API

Pour visualiser et tester l’API, rendez-vous à l’adresse suivante :  
http://localhost:8000/fr/software/api

Vous aurez accès à quatre options :
- Trois pour visualiser l’API
- Une pour télécharger le schéma

J’utilise généralement Swagger, car il est simple et intuitif. À partir de la page Swagger, il est possible de consulter les différentes tables de la base de données et d’effectuer des requêtes via l’API, ce qui est très utile lors du développement backend.

---

# Installer le projet Android (Flutter)

### Prérequis

- Avoir un IDE de votre choix (j’utilise Android Studio)
- Avoir Git installé
- Avoir le Flutter SDK installé : https://docs.flutter.dev/get-started/install

### Préparation

- Cloner le projet Flutter (https://github.com/alexisbirling-boop/flutter.git) sur votre ordinateur à partir de mon fork

### Étapes

1. Ajouter un émulateur Android dans votre IDE (simple avec Android Studio)
2. Sélectionner le SDK téléchargé précédemment pour ouvrir le projet
3. Attendre que toutes les tâches en arrière-plan (Background tasks) se terminent
4. Attendre la fin de l’indexation (Indexing)
5. Démarrer l’émulateur Android
6. Exécuter la commande suivante dans le terminal :

```
flutter run
```

7. Se connecter avec un compte utilisateur :

```
username: user
mot de passe: flutteruser
```

---

## Important

Si vous rencontrez des problèmes lors du démarrage de l’application, vous pouvez utiliser la commande suivante pour redémarrer l’application :

```
r
```

---

# Utiliser le projet Android (Flutter)

## Fonctionnalités

L’application mobile possède une barre de navigation en bas de l’écran permettant d’accéder aux différents modules. Voici un résumé des principales fonctionnalités.

---

### Icône maison

Cette page offre un aperçu regroupé des différentes sections. Elle permet une visualisation rapide des éléments, mais offre moins d’options de personnalisation.  
Les trophées y sont également affichés afin de visualiser vos réalisations.

---

### Icône poids

Cette page permet de consulter les routines d’entraînement.  
Avec le bouton « + », vous pouvez ajouter une nouvelle routine.  
Lors de la création ou de la modification d’une routine, il est possible d’ajouter différentes journées d’entraînement contenant divers exercices.

---

### Icône ustensile

Cette page permet de consulter les plans de nutrition.  
Avec le bouton « + », vous pouvez ajouter un nouveau plan.

À l’intérieur d’un plan :
- Le bouton « carotte » permet d’ajouter des ingrédients
- Le bouton « barcode » permet d’ouvrir la caméra pour scanner un produit
- Le bouton « assiette » permet d’ajouter un repas composé de plusieurs ingrédients

---

### Icône calepin

Cette page permet de suivre vos progrès en inscrivant régulièrement des entrées (logs).

Par défaut, le bouton « + » permet d’enregistrer votre poids, qui sera ensuite affiché dans les graphiques pertinents.

Au bas de la page, il est également possible d’ajouter des graphiques personnalisés pour mesurer d’autres valeurs (par exemple : le tour de biceps en cm).

---

### Icône image

Cette page sert de galerie photo afin de visualiser votre progression.  
Elle permet de téléverser des photos depuis l’appareil mobile.

---

### Contenu additionnel

Depuis le menu principal, vous pouvez accéder aux paramètres à l’aide du bouton « engrenage » afin de modifier votre profil et certains réglages de l’application.

---

# Documentation utile

Voici la documentation en ligne de wger pour vous aider en cas de problème :

- Installation Docker dev :  
  https://wger.readthedocs.io/en/latest/development/docker.html#development-docker

- Installation du projet Flutter :  
  https://wger.readthedocs.io/en/latest/development/mobile_app.html

---

# Description des issues

(Section à compléter)