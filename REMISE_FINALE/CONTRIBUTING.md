# Contributing to wger Workout Manager

Merci de votre intérêt pour contribuer à wger! Ce document explique comment installer le projet, faire des modifications, et soumettre vos contributions.

---

## Table des matières

* [Code de conduite](#code-de-conduite)
* [Comment puis-je contribuer?](#comment-puis-je-contribuer)
* [Installation du projet](#installation-du-projet)
* [Workflow de développement](#workflow-de-développement)
* [Standards de code](#standards-de-code)
* [Tests](#tests)
* [Soumettre une Pull Request](#soumettre-une-pull-request)
* [Ressources utiles](#ressources-utiles)

---

## Code de conduite

En participant à ce projet, vous acceptez de respecter notre [Code de Conduite](https://wger.readthedocs.io/en/latest/contributing.html#code). Soyez respectueux, inclusif et constructif dans vos interactions avec la communauté.

---

## Comment puis-je contribuer?

Il existe plusieurs façons de contribuer au projet wger :

* **🐛 Signaler des bugs** : Ouvrez une [issue](https://github.com/wger-project/wger/issues) en décrivant le problème et comment le reproduire
* **💡 Proposer des fonctionnalités** : Discutez d'abord de votre idée sur [Discord](https://discord.gg/rPWFv6W) ou dans une issue
* **🔧 Corriger des bugs** : Consultez les [issues](https://github.com/wger-project/wger/issues) marquées comme `good first issue`
* **✨ Ajouter des fonctionnalités** : Assurez-vous de discuter de la fonctionnalité avant de commencer le développement
* **📝 Améliorer la documentation** : Corrigez les fautes, clarifiez les instructions, ou ajoutez des exemples
* **🌍 Traduire l'application** : Contribuez via [Weblate](https://hosted.weblate.org/engage/wger/)
* **🏋️ Ajouter des exercices** : Enrichissez la base de données d'exercices via l'interface web

---

## Installation du projet

Pour installer et configurer votre environnement de développement, consultez le fichier **[README.md](README.md)** qui contient des instructions détaillées pour :

* **Backend (Python/Django)** : Configuration Docker, initialisation de la base de données, et lancement du serveur
* **Frontend (Flutter)** : Installation du Flutter SDK, configuration d'Android Studio, et création d'un émulateur

Les commandes principales sont également listées dans le README pour faciliter le démarrage rapide.

---

## Workflow de développement

### 1. Créer une branche

Créez toujours une nouvelle branche pour vos modifications :

```bash
# Se positionner sur master
git checkout master

# Mettre à jour master
git pull origin master

# Créer une nouvelle branche
git checkout -b feature/nom-de-votre-feature
```

### 2. Faire vos modifications

* Suivez les [standards de code](#standards-de-code)
* Ajoutez des [tests](#tests) pour vos modifications
* Mettez à jour la documentation si nécessaire

### 3. Commiter vos changements

```bash
# Ajouter les fichiers modifiés
git add .

# Commiter avec un message clair
git commit -m "Add feature X to improve Y"
```

**Format des messages de commit :**
* Utiliser l'impératif présent ("Add feature" pas "Added feature")
* Première ligne : résumé court (<50 caractères)
* Ligne vide si description détaillée nécessaire
* Description détaillée du changement

**Exemple :**
````
Add server configuration sanity check

Check pagination URLs after login
Warn users if reverse proxy is misconfigured
Add internationalization support for dialog messages
````
### 4. Pousser votre branche

```bash
# Première fois
git push -u origin feature/nom-de-votre-feature

# Pushs suivants
git push
```

---

## Standards de code

### Backend (Python/Django)

* **Style** : Suivre [PEP 8](https://pep8.org/)
* **Formatage** : Utiliser `ruff format` et `isort`
* **Linting** : Le code doit passer `ruff check`
* **Docstrings** : Documenter les fonctions/classes complexes
* **Type hints** : Utiliser les annotations de type Python quand approprié

**Commandes de formatage :**

```bash
# Formater le code
docker compose exec web ruff format /home/wger/src

# Trier les imports
docker compose exec web isort /home/wger/src
```

**Exemple de code Python :**

```python
def calculate_bmi(weight: float, height: float) -> float:
    """
    Calculate Body Mass Index.
    
    Args:
        weight: Weight in kilograms
        height: Height in meters
    
    Returns:
        BMI value as a float
    """
    return weight / (height ** 2)
```

### Frontend (Flutter/Dart)

* **Style** : Suivre les [Effective Dart guidelines](https://dart.dev/guides/language/effective-dart)
* **Formatage** : Utiliser `dart format .`
* **Linting** : Le code doit passer `flutter analyze`
* **Header AGPL** : Inclure le header de licence dans chaque nouveau fichier

**Commandes de formatage :**

```bash
# Formater le code
dart format .

# Analyser le code
flutter analyze
```

**Exemple de header AGPL :**

```dart
/*
 * This file is part of wger Workout Manager.
 * Copyright (C) 2025 wger Team
 *
 * wger Workout Manager is free software: you can redistribute it and/or modify
 * it under the terms of the GNU Affero General Public License as published by
 * the Free Software Foundation, either version 3 of the License, or
 * (at your option) any later version.
 */
```

---

## Tests

### Backend (Django)

**Les tests doivent couvrir :**
* Les models (validations, méthodes)
* Les vues et API endpoints
* Les formulaires
* La logique métier complexe

**Lancer les tests :**

```bash
# Tous les tests
docker compose exec web python3 manage.py test

# Tests d'une app spécifique
docker compose exec web python3 manage.py test exercises

# Tests d'un fichier spécifique
docker compose exec web python3 manage.py test exercises.tests.test_models
```

**Exemple de test :**

```python
from django.test import TestCase
from wger.exercises.models import ExerciseImage

class ExerciseImageTestCase(TestCase):
    def test_ai_generated_default_false(self):
        """Test that is_ai_generated defaults to False"""
        image = ExerciseImage.objects.create(exercise=self.exercise)
        self.assertFalse(image.is_ai_generated)
```

### Frontend (Flutter)

**Les tests doivent couvrir :**
* Les fonctions helper
* Les widgets
* Les providers/state management
* Les cas limites et erreurs

**Lancer les tests :**

```bash
# Tous les tests
flutter test

# Tests d'un fichier spécifique
flutter test test/helpers/sanity_checks_test.dart

# Avec rapport détaillé
flutter test --reporter expanded
```

**Exemple de test :**

```dart
import 'package:flutter_test/flutter_test.dart';

void main() {
  group('AuthProvider - checkServerConfiguration', () {
    test('returns true when pagination URL matches base URL', () async {
      final result = await checkServerConfiguration(
        baseUrl: 'https://example.com',
        token: 'test-token',
      );
      
      expect(result, isTrue);
    });
  });
}
```

---

## Soumettre une Pull Request

### Avant de soumettre

Assurez-vous que votre contribution respecte les critères suivants :

* ✅ Le code suit les [standards de code](#standards-de-code)
* ✅ Tous les tests passent (`flutter test` ou `python3 manage.py test`)
* ✅ Le code est formaté (`dart format .` ou `ruff format`)
* ✅ Vous avez ajouté des tests pour vos modifications
* ✅ La documentation est mise à jour si nécessaire
* ✅ Votre branche est à jour avec `master`

### Créer la Pull Request

1. **Allez sur GitHub** : [wger backend](https://github.com/wger-project/wger) ou [wger flutter](https://github.com/wger-project/flutter)
2. Cliquez sur **Pull Requests** → **New Pull Request**
3. Sélectionnez votre branche
4. **Remplissez le template de PR** :
    * **Titre** : Résumé clair et concis de vos modifications
    * **Description** : Expliquez ce que vous avez changé et pourquoi
    * **Related Issue** : Liez l'issue correspondante si applicable (ex: `Closes #123`)
    * **Screenshots** : Ajoutez des captures d'écran pour les changements UI

**Exemple de description de PR :**

```markdown
# Proposed Changes

* Add automatic server configuration check after user login
* Detect reverse proxy misconfigurations by comparing pagination URLs
* Display warning dialog when misconfiguration is detected
* Add internationalization support for dialog messages

## Related Issue(s)

Addresses #456

## Testing

Added comprehensive unit tests in `test/providers/auth_test.dart` covering:
* Valid configuration scenarios
* Host mismatch detection
* Protocol mismatch detection
* Error handling
```

5. **Attendez la review** des mainteneurs
6. **Effectuez les modifications** demandées si nécessaire
7. **Soyez patient** — les mainteneurs sont bénévoles!

### Pendant la review

* Soyez ouvert aux commentaires et suggestions
* Répondez aux questions des reviewers de manière constructive
* Poussez les corrections demandées rapidement
* Remerciez les reviewers pour leur temps et leurs suggestions

---

## Ressources utiles

### Documentation

* **Documentation générale** : <https://wger.readthedocs.io>
* **API Documentation** : <https://wger.de/api/v2>
* **Guide de contribution officiel** : <https://wger.readthedocs.io/en/latest/contributing.html>
* **Django Documentation** : <https://docs.djangoproject.com>
* **Flutter Documentation** : <https://docs.flutter.dev>

### Communauté

* **Discord** : <https://discord.gg/rPWFv6W> (pour discuter et poser des questions)
* **Mastodon** : <https://fosstodon.org/@wger>
* **Issue Tracker** : <https://github.com/wger-project/wger/issues>
* **Weblate (Traductions)** : <https://hosted.weblate.org/engage/wger/>

### Dépôts GitHub

* **Backend** : <https://github.com/wger-project/wger>
* **Mobile** : <https://github.com/wger-project/flutter>
* **Docker** : <https://github.com/wger-project/docker>

---

## Questions fréquentes

### Comment choisir une issue sur laquelle travailler?

Consultez les issues marquées `good first issue` pour débuter. Si une issue vous intéresse, commentez-la pour indiquer que vous travaillez dessus.

### Dois-je discuter de ma fonctionnalité avant de commencer?

Oui! Toujours discuter des nouvelles fonctionnalités sur Discord ou dans une issue avant de commencer le développement. Cela évite de perdre du temps sur une fonctionnalité qui pourrait ne pas être acceptée.

### Combien de temps prend la review d'une PR?

Les mainteneurs sont bénévoles, donc le temps de review peut varier. Soyez patient et n'hésitez pas à faire un ping poli après une semaine si aucune réponse.

### Mon environnement ne fonctionne pas, que faire?

Consultez d'abord le [README.md](README.md) pour les instructions d'installation. Si le problème persiste, demandez de l'aide sur [Discord](https://discord.gg/rPWFv6W).

---

## Besoin d'aide?

Si vous avez des questions ou besoin d'aide, n'hésitez pas à :

* **Rejoindre notre Discord** : <https://discord.gg/rPWFv6W>
* **Ouvrir une issue** : <https://github.com/wger-project/wger/issues>
* **Consulter la documentation** : <https://wger.readthedocs.io>

Merci de contribuer à wger! 🎉