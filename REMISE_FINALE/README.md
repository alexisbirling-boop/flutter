# Remise finale

## Instructions d’installation et d’utilisation

Voici un guide pour l’installation des différents répertoires permettant à wger de fonctionner.

***

## Installer le projet backend (Python)

### Prérequis

* Avoir l’application Docker Desktop installée
* Avoir un IDE de votre choix (j’utilise PyCharm)
* Avoir Git installé
* Avoir un interpréteur Python installé

### Préparation

* Cloner le projet Docker (<https://github.com/wger-project/docker>) sur votre ordinateur
* Cloner le projet Wger (<https://github.com/alexisbirling-boop/wger>) sur votre ordinateur à partir
  de mon fork

### Étapes

1. Ouvrir le projet Docker
2. Aller dans le dossier `dev`

```
cd docker/dev
```

1. Copier le fichier `.env.example` dans un nouveau fichier `.env`

```
cp .env.example .env
```

1. Inscrire le chemin du projet Wger cloné précédemment dans la variable `WGER_CODEPATH`

```
WGER_CODEPATH=C:\A_Documentation_Natif\wger
```

(Votre chemin variera selon l’emplacement de sauvegarde. Vous pouvez copier le chemin depuis
l’Explorateur de fichiers.)

1. Ouvrir Docker Desktop
2. Lancer Docker à partir du dossier `dev`

```
docker compose up --watch
```

1. Exécuter les commandes suivantes (cela peut prendre un certain temps)

```
docker compose exec web /bin/bash
wger bootstrap
python3 manage.py sync-exercises
wger load-online-fixtures
```

1. Démarrer le serveur de développement

```
python3 manage.py runserver 0.0.0.0:8000
```

1. Accéder au serveur local : <http://localhost:8000>
2. Se connecter avec un compte administrateur

```
username: admin
mot de passe: adminadmin
```

***

## Important

Pour démarrer le projet lors des prochaines utilisations, exécuter uniquement les commandes
suivantes depuis le dossier `dev`, avec Docker Desktop ouvert :

```
docker compose up --watch
docker compose exec web /bin/bash
python3 manage.py runserver 0.0.0.0:8000
```

Lors de ma première installation du projet, j’ai dû modifier le `Dockerfile` dans le projet Wger
pour que le tout fonctionne. Lors de ma deuxième installation avec un nouveau clone, je n’ai pas eu
à effectuer cette modification. Il est donc possible qu’elle ne soit pas obligatoire. Je l’inscris
tout de même ici au cas où une erreur surviendrait lors de l’installation.

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

***

## Utiliser le projet backend (Python)

### Fonctionnalités

Les fonctionnalités sont les mêmes que celles décrites plus loin dans la section Android. Je ne les
répéterai donc pas ici.\
L’intérêt principal du backend est d’avoir accès à l’API.

### API

Pour visualiser et tester l’API, rendez-vous à l’adresse suivante :\
<http://localhost:8000/fr/software/api>

Vous aurez accès à quatre options :

* Trois pour visualiser l’API
* Une pour télécharger le schéma

J’utilise généralement Swagger, car il est simple et intuitif. À partir de la page Swagger, il est
possible de consulter les différentes tables de la base de données et d’effectuer des requêtes via
l’API, ce qui est très utile lors du développement backend.

***

# Installer le projet Android (Flutter)

### Prérequis

* Avoir un IDE de votre choix (j'utilise Android Studio)
* Avoir Git installé
* Avoir le Flutter SDK installé : <https://docs.flutter.dev/get-started/install>

### Préparation

* Cloner le projet Flutter (<https://github.com/alexisbirling-boop/flutter.git>) sur votre ordinateur à partir de mon fork

### Étapes

#### 1. Installer et configurer le Flutter SDK

1. Télécharger le Flutter SDK depuis <https://docs.flutter.dev/get-started/install>
2. Extraire le fichier ZIP dans un dossier (par exemple `C:\flutter` sur Windows ou `~/flutter` sur Mac/Linux)
3. Ajouter Flutter au PATH de votre système :
    * **Windows** : Aller dans "Paramètres système avancés" → "Variables d'environnement" → Dans "Path", ajouter `C:\flutter\bin`
    * **Mac/Linux** : Ajouter `export PATH="$PATH:$HOME/flutter/bin"` dans votre fichier `~/.bashrc` ou `~/.zshrc`
4. Vérifier l'installation dans un terminal :

```bash
flutter doctor
```

Cette commande affiche l'état de votre installation Flutter. Suivez les instructions pour installer les composants manquants (Android SDK, IDE, etc.).

#### 2. Configurer Android Studio

1. Ouvrir Android Studio
2. Aller dans **File** → **Settings** (ou **Ctrl + Alt + S** sur Windows)
3. Aller dans **Languages & Frameworks** → **Flutter**
4. Cliquer sur le champ **Flutter SDK path** et naviguer vers le dossier où vous avez extrait Flutter (par exemple `C:\flutter`)
5. Cliquer sur **Apply** puis **OK**

#### 3. Créer un émulateur Android

1. Dans Android Studio, cliquer sur l'icône **Device Manager** (téléphone avec Android) dans la barre d'outils
    * Ou aller dans **Tools** → **Device Manager**
2. Cliquer sur **Create Device** (ou **+** en haut)
3. Sélectionner un appareil (par exemple **Pixel 7**)
4. Cliquer sur **Next**
5. Sélectionner une image système (par exemple **Tiramisu** ou **UpsideDownCake**)
    * Si l'image n'est pas téléchargée, cliquer sur **Download** à côté du nom de l'image
6. Cliquer sur **Next** puis **Finish**
7. Votre émulateur est maintenant créé et apparaît dans la liste des appareils disponibles

#### 4. Ouvrir le projet Flutter

1. Dans Android Studio, aller dans **File** → **Open**
2. Naviguer vers le dossier où vous avez cloné le projet Flutter
3. Sélectionner le dossier racine du projet (celui qui contient `pubspec.yaml`)
4. Cliquer sur **OK**
5. Attendre que toutes les tâches en arrière-plan (Background tasks) se terminent
6. Attendre la fin de l'indexation (Indexing) — cela peut prendre quelques minutes

#### 5. Récupérer les dépendances

1. Ouvrir un terminal dans Android Studio (en bas de l'écran)
2. Exécuter la commande suivante :

```bash
flutter pub get
```

Cette commande télécharge toutes les dépendances du projet.

#### 6. Lancer l'application

1. Démarrer l'émulateur Android :
    * Cliquer sur l'icône **Play** (▶) à côté du nom de l'émulateur dans la barre d'outils
    * Ou ouvrir le **Device Manager** et cliquer sur **Play** à côté de l'émulateur
2. Attendre que l'émulateur démarre complètement (écran d'accueil Android visible)
3. Dans le terminal, exécuter la commande suivante :

```bash
flutter run
```

4. L'application se lance sur l'émulateur
5. Se connecter avec un compte utilisateur :

```
username: user
mot de passe: flutteruser
```
***

## Important

### Commandes utiles pour les prochaines utilisations

Pour démarrer le projet lors des prochaines utilisations, exécuter uniquement les commandes suivantes :

```bash
# Lancer l'application (l'émulateur doit être démarré)
flutter run
```

### Hot Reload (rechargement à chaud)

Lorsque l'application est en cours d'exécution, vous pouvez appliquer vos modifications sans redémarrer complètement l'app :

* **Hot Reload** : Appuyer sur `r` dans le terminal (recharge l'interface, garde l'état)
* **Hot Restart** : Appuyer sur `R` dans le terminal (redémarre l'app complètement)
* **Quitter** : Appuyer sur `q` dans le terminal

### Résolution de problèmes

#### Problème : Erreurs de dépendances lors du `flutter run`

Si vous rencontrez des erreurs comme `version solving failed` ou `package not found` :

1. Mettre à jour Flutter :

```bash
flutter upgrade
```

2. Nettoyer le cache du projet :

```bash
flutter clean
```

3. Récupérer à nouveau les dépendances :

```bash
flutter pub get
```

4. Relancer l'application :

```bash
flutter run
```

#### Problème : "Flutter SDK not found" dans Android Studio

1. Aller dans **File** → **Settings** → **Languages & Frameworks** → **Flutter**
2. Vérifier que le **Flutter SDK path** pointe vers le bon dossier (par exemple `C:\flutter`)
3. Si le chemin est incorrect, cliquer sur le dossier et naviguer vers l'emplacement correct
4. Cliquer sur **Apply** puis **OK**
5. Redémarrer Android Studio

#### Problème : L'émulateur ne démarre pas

1. Essayer de créer un nouvel émulateur avec une image système différente
2. Redémarrer Android Studio

#### Problème : "No devices found"

1. S'assurer que l'émulateur est bien démarré et complètement chargé
2. Dans le terminal, vérifier les appareils disponibles :

```bash
flutter devices
```

3. Si l'émulateur n'apparaît pas, redémarrer l'émulateur

***

# Utiliser le projet Android (Flutter)

## Fonctionnalités

L’application mobile possède une barre de navigation en bas de l’écran permettant d’accéder aux
différents modules. Voici un résumé des principales fonctionnalités.

***

### Icône maison

Cette page offre un aperçu regroupé des différentes sections. Elle permet une visualisation rapide
des éléments, mais offre moins d’options de personnalisation.\
Les trophées y sont également affichés afin de visualiser vos réalisations.

***

### Icône poids

Cette page permet de consulter les routines d’entraînement.\
Avec le bouton « + », vous pouvez ajouter une nouvelle routine.\
Lors de la création ou de la modification d’une routine, il est possible d’ajouter différentes
journées d’entraînement contenant divers exercices.

***

### Icône ustensile

Cette page permet de consulter les plans de nutrition.\
Avec le bouton « + », vous pouvez ajouter un nouveau plan.

À l’intérieur d’un plan :

* Le bouton « carotte » permet d’ajouter des ingrédients
* Le bouton « barcode » permet d’ouvrir la caméra pour scanner un produit
* Le bouton « assiette » permet d’ajouter un repas composé de plusieurs ingrédients

***

### Icône calepin

Cette page permet de suivre vos progrès en inscrivant régulièrement des entrées (logs).

Par défaut, le bouton « + » permet d’enregistrer votre poids, qui sera ensuite affiché dans les
graphiques pertinents.

Au bas de la page, il est également possible d’ajouter des graphiques personnalisés pour mesurer
d’autres valeurs (par exemple : le tour de biceps en cm).

***

### Icône image

Cette page sert de galerie photo afin de visualiser votre progression.\
Elle permet de téléverser des photos depuis l’appareil mobile.

***

### Contenu additionnel

Depuis le menu principal, vous pouvez accéder aux paramètres à l’aide du bouton « engrenage » afin
de modifier votre profil et certains réglages de l’application.

***

# Documentation utile

Voici la documentation en ligne de wger pour vous aider en cas de problème :

* Installation Docker dev :\
  <https://wger.readthedocs.io/en/latest/development/docker.html#development-docker>

* Installation du projet Flutter :\
  <https://wger.readthedocs.io/en/latest/development/mobile_app.html>

***

# Description des issues

Au moment de la remise d'avancement, j'ai travaillé sur 3 issues. 2 des issues ont été dans le
projet Flutter en dart et l'autre dans le projet backend en python.

## Issue 1: Améliorer le dialogue de produit non trouvé

Voir la [pull request](https://github.com/wger-project/flutter/pull/1122)

Pour cette issue, je devais améliorer le message affiché lorsque le scan d'un code-barres ne donne
aucun résultat. Le pop-up initial indiquait seulement que rien n'était trouvé. Je devais donc
rajouter un message expliquant que l'utilisateur peut aller sur Open Food Facts (une base de données
Open Source) pour rajouter le produit eux-mêmes ainsi que mettre le lien nécessaire.

### Démarche

* Pour commencer, j'ai dû trouver le widget responsable d'afficher le pop-up de résultat et trouver
  l'endroit pour le résultat non trouvé. (`lib/widgets/nutrition/ingredient_details.dart`)
* J'ai fait ma première tentative de `Text` à rajouter dans le pop-up et ça marchait.
* J'ai donc recherché et trouvé comment mettre un bouton menant à un URL extérieur en Flutter pour
  le mettre au bas du widget.
* Puisque l'application est traduite, j'ai dû mettre des labels pour mes strings et trouver comment
  faire permettre la traduction de ces labels plus tard. En regardant le dossier `generated` dans
  `l10n`, j'ai vu qu'ils utilisaient Weblate pour permettre à des contributeurs de faire la
  traduction. Je suis allé voir sur Weblate mais je n'ai pas trouvé comment ajouter de nouveaux
  labels. J'ai ensuite vu qu'il y avait des fichiers `app_XX.arb` avec un format de langue à la
  place de XX (exemple, `app_fr.arb`). C'est là que les labels étaient déclarés, donc j'ai modifié
  le fichier en anglais pour mettre les tous premiers labels.
* Je voulais ensuite faire mes tests. J'ai regardé la structure des tests déjà existants et je l'ai
  reprise pour les miens. J'ai créé le fichier de test sous `test/widgets/nutrition` puisqu'il n'y
  avait pas encore de tests pour mon widget. J'ai rajouté un test pour vérifier que le bouton de
  lien Open Food Facts est présent quand il n'y a pas de produit trouvé, et un autre pour vérifier
  que le bouton fonctionne comme attendu.
* J'ai utilisé la commande `dart format .` pour appliquer le bon format à mon code comme demandé
  dans le guide de contribution.

**Première PR**\
J'ai eu 2 commentaires. Le premier pour changer le style du texte afin qu'il soit identique au
reste (je l'avais mis plus pâle puisque c'était une description optionnelle et que l'utilisateur n'
est pas obligé d'aller sur Open Food Facts et contribuer). Le deuxième était de mettre mes textes
dans une même `Column` pour être sûr de ne pas afficher le message de contribution Open Food Facts
lorsqu'il y a un produit qui est trouvé.

* J'ai effectué les 2 changements et j'ai envoyé mes modifications.

**Deuxième PR**\
Elle a été merge dans le projet principal.

### Problèmes rencontrés

* Ce n'est pas vraiment un problème, mais au début je ne savais pas que je pouvais reload rapidement
  l'application avec `r` donc je fermais tout pour tester mes changements à chaque fois.
* Au début de ma tentative de faire la traduction, j'essayais de modifier les fichiers generated
  avec les labels, ce qui causait des problèmes puisque je devais modifier les fichiers `.arb`.
* Je ne savais pas comment lancer mes tests pour voir s'ils fonctionnaient. En cliquant sur la
  flèche verte à côté du `void main()` dans Android Studio, j'obtenais l'erreur
  `Because wger depends on integration_test from sdk which doesn't exist (the Flutter SDK is not available), version solving failed`.
  Le problème était qu'Android Studio essayait d'utiliser `dart pub` au lieu de `flutter test`. J'ai
  fini par trouver que je devais lancer les tests via le terminal avec la commande
  `flutter test test/widgets/nutrition/ingredient_details_test.dart`.

***

## Issue 2: Ajout d'un flag "AI Generated" aux images d'exercices

Voir la [pull request](https://github.com/wger-project/wger/pull/2222)

Cette issue consistait à ajouter un champ booléen dans la base de données pour identifier si une
image d'exercice a été générée par IA. Le mainteneur du projet souhaitait pouvoir tracer ces images
pour des raisons de transparence et de futures fonctionnalités.

### Démarche

* J'ai d'abord dû comprendre l'architecture du projet backend Django qui est séparé en 2 dépôts
  distincts : `docker` (configuration Docker), `wger` (code backend Python).
* J'ai modifié le modèle `ExerciseImage` dans `wger/exercises/models/image.py` pour ajouter le champ
  `is_ai_generated` de type `BooleanField` avec une valeur par défaut de `False`.
* J'ai tenté de créer la migration automatiquement avec
  `docker compose exec web python3 manage.py makemigrations exercises`, mais Django détectait plein
  de changements non liés à mon feature et générait une migration massive qui échouait lors de
  l'application.
* Après plusieurs tentatives (fake de migrations, reset de la base de données), j'ai décidé de créer
  la migration manuellement. J'ai créé le fichier `0035_exerciseimage_is_ai_generated.py` avec
  uniquement l'ajout de mon champ sur les deux modèles `exerciseimage` et `historicalexerciseimage`.
* J'ai appliqué la migration avec `docker compose exec web python3 manage.py migrate exercises`.
* J'ai modifié le serializer dans `wger/exercises/api/serializers.py` pour exposer le nouveau champ
  via l'API REST.
* J'ai testé l'API à `http://localhost:8000/api/v2/exerciseimage/` pour vérifier que le champ
  `is_ai_generated` apparaissait bien dans les réponses JSON.

**Première PR**\
J'ai eu un commentaire disant que je devais rajouter le flag dans un autre fichier en plus afin que
les autres programmeurs avec des serveurs plus anciens puissent tout de même avoir accès à la
nouvelle propriété, mais le PO l'a corrigé avant moi.\
Elle a été merge dans le projet principal ensuite.

### Problèmes rencontrés

* Après un reset complet de la base de données, j'ai dû recréer les données initiales avec
  `wger bootstrap`. Cependant, au début je ne savais pas que la BD avait été reset donc j'ai perdu
  du temps à trouver pourquoi rien ne marchait.
* Le code backend est monté en volume depuis un dépôt séparé, donc toutes les commandes Django
  devaient être exécutées DANS le conteneur avec `docker compose exec web python3 ...`. Au début,
  j'essayais d'exécuter les commandes directement depuis le terminal Windows, ce qui ne fonctionnait
  pas.
* Django générait automatiquement une migration `0035_rename_exercises...` avec énormément de
  modifications non liées à mon feature. Cette migration échouait avec une erreur SQLite
  `index exercises_historicalexercise_id_7853acff already exists`. Après avoir essayé de fake la
  migration, de supprimer et recréer la base de données plusieurs fois, j'ai finalement opté pour
  créer la migration manuellement, ce qui a résolu tous les problèmes.
* Pour les tests, je devais à nouveau trouver comment les lancer puisque c'est projet différent de
  celui en Flutter. J'ai utilisé les commandes Django dans le conteneur Docker :
  `docker compose exec web python3 manage.py test exercises` pour lancer les tests automatisés dans
  mon projet wger.

***

## Issue 3: Vérification automatique de la configuration serveur (Sanity Check)

Voir la [pull request](https://github.com/wger-project/flutter/pull/1136)

Cette issue visait à détecter automatiquement les problèmes de configuration de reverse proxy pour
les serveurs self-hosted. Le problème survient quand les headers HTTP ne sont pas correctement
transmis,ce qui causant des liens de pagination cassés dans l'API (par exemple
`http://localhost:8000/...` au lieu de `https://wger.mydomain.com/...`).

### Démarche

* J'ai créé une fonction helper `checkServerPaginationUrls()` dans `lib/helpers/sanity_checks.dart`
  qui fait une requête test à l'endpoint `/api/v2/exercise/?limit=1` et compare le domaine de l'URL
  de base avec celui retourné dans le lien de pagination `next`.
* J'ai intégré cette vérification dans le flow de login (`lib/providers/auth.dart`) pour qu'elle s'
  exécute automatiquement après une connexion réussie. J'ai ajouté une variable
  `_serverConfigWarning` dans le provider pour stocker le message d'avertissement.
* J'ai créé un widget de dialogue `showServerConfigWarning()` dans
  `lib/widgets/core/server_config_warning_dialog.dart` pour afficher l'avertissement à l'utilisateur
  avec un message clair.
* J'ai modifié `lib/screens/auth_screen.dart` pour afficher le dialogue après le login si un
  problème est détecté.
* J'ai ajouté des nouveaux labels dans `lib/l10n/app_en.arb` pour l'internationalisation.
* J'ai créé des tests unitaires complets dans `test/helpers/sanity_checks_test.dart` couvrant
  différents scénarios : configuration valide, mismatch de domaine (localhost vs domaine réel),
  mismatch de protocole (HTTP vs HTTPS), erreurs serveur, erreurs réseau, et validation des headers
  HTTP.

**Première PR**\
J'ai eu un commentaire disant de rendre le message d'erreur plus court et de mettre un lien vers une
certaine page de la doc en ligne. J'ai donc fait les changements au niveau de l'affichage et changé
un peu mon helper puisque le message d'erreur n'avait pas besoin d'être personnalisé.

**Deuxième PR**  
J'ai reçu des commentaires demandant d'améliorer la cohérence du code avec le reste du projet :
* Déplacer la fonction `checkServerConfiguration()` du fichier helper vers `AuthProvider` pour regrouper la logique liée à l'authentification.
* Utiliser `AuthState.serverConfigIssue` au lieu d'une variable booléenne séparée pour être cohérent avec les autres états comme `AuthState.updateRequired`.
* Remplacer la classe `SanityCheckResult` par un simple booléen puisqu'on n'a besoin que de savoir si la configuration est valide ou non.
* Utiliser `Theme.of(context).colorScheme.error` au lieu de `Colors.orange` pour l'icône d'avertissement, ce qui permet au thème de gérer automatiquement les couleurs.

J'ai effectué toutes ces modifications. Cependant, mon projet Flutter local était en retard de plusieurs centaines de commits sur le master. En tentant de faire un rebase pour mettre à jour ma branche, j'ai rencontré des conflits de dépendances qui m'ont empêché de lancer `flutter run`. Après avoir résolu ces problèmes et complété le rebase, j'ai poussé mes changements, mais toutes les modifications du rebase (300+ fichiers) se sont retrouvées dans ma PR originale #1136, la rendant très désorganisée et difficile à review.

L'admin du projet a alors ouvert une nouvelle PR [1188](https://github.com/wger-project/flutter/pull/1188) pour cherry-pick uniquement mes changements pertinents (sanity check) sans tout l'historique du rebase. Cette PR a été reviewée, approuvée, et merge dans le projet principal.


### Problèmes rencontrés

* Au début, le dialogue n'apparaissait pas, même si je rajoutais manuellement une erreur qui devait
  être affichée lors de la connexion. J'ai fini par réaliser que j'appelais mon widget au mauvais
  moment dans le flow de la connexion, et que la page changeait avant que mon widget affiche son
  message.
* Après le rebase, tous les changements du master (300+ fichiers) se sont retrouvés dans ma PR sans vouloir les envoyer, la rendant impossible à review proprement. L'admin a dû créer une nouvelle PR pour cherry-pick seulement mes changements pertinents.

***

_Version 1.1_\
_Dernière mise à jour: 04 mai 2026_\
_Auteur: Alexis Birling_