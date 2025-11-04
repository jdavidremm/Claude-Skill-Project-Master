# Execution - Exécution des Tâches

## Objectif

Exécuter le plan de manière méthodique, sous-tâche par sous-tâche, avec suivi de progression.

## Workflow d'Exécution

### 1. Initialisation

```json
{
  "status": "execution_started",
  "plan": {},
  "current_subtask": null,
  "completed_subtasks": [],
  "pending_subtasks": [1, 2, 3, 4, 5, 6, 7]
}
```

### 2. Exécution Séquentielle

Pour chaque sous-tâche :

**Étape 1 : Marquer comme "en cours"**
```json
{
  "status": "in_progress",
  "current_subtask": {
    "id": 1,
    "name": "Créer models BDD",
    "started_at": "2025-11-04T15:00:00"
  }
}
```

**Étape 2 : Exécuter la sous-tâche**
- Créer/modifier les fichiers nécessaires
- Respecter les conventions du projet
- Suivre le design system
- Ajouter les tests si applicable

**Étape 3 : Valider la sous-tâche**
- Lancer les tests si présents
- Vérifier la syntaxe (python -m py_compile)
- Vérifier que les fichiers sont créés

**Étape 4 : SI ERREUR → Lire ERROR-HANDLING.md**
- Tenter correction (max 3 fois)
- Si échec définitif → RETOURNER erreur à Claude

**Étape 5 : Marquer comme "complétée"**
```json
{
  "subtask_completed": {
    "id": 1,
    "name": "Créer models BDD",
    "duration": "52min",
    "files_created": ["database/models/effectifs.py"],
    "tests_passed": true
  }
}
```

**Étape 6 : Passer à la suivante**
- Vérifier les dépendances
- Continuer avec la prochaine sous-tâche sans dépendances non satisfaites

### 3. Progression en Temps Réel

Si durée totale > 1h, envoyer des updates de progression :

```json
{
  "status": "progress_update",
  "progress": {
    "completed": [
      {"id": 1, "name": "Créer models", "duration": "52min"}
    ],
    "current": {
      "id": 2,
      "name": "Créer queries",
      "started_at": "2025-11-04T15:52:00",
      "elapsed": "15min"
    },
    "pending": [
      {"id": 3, "name": "Migration"},
      {"id": 4, "name": "Composants UI"}
    ]
  },
  "time_elapsed": "1h07",
  "time_remaining_estimate": "~6h50"
}
```

## Bonnes Pratiques d'Exécution

### Ordre d'Exécution

1. **Models** (fondation)
2. **Queries** (logique métier)
3. **Migration** (BDD)
4. **Composants** (UI)
5. **Pages** (intégration)
6. **Tests** (validation)
7. **Documentation** (finalisation)

### Conventions à Respecter

**Imports** :
```python
from nicegui import ui, app
from datetime import date, datetime, timedelta  # Importer séparément
from database.initialisation import SessionLocal
```

**Design System** :
```python
from components.design_system import create_kpi_card
from style.theme import init_page
```

**Tests** :
```python
import pytest
from database.models.xxx import YYY
```

### Vérifications à Chaque Sous-Tâche

- [ ] Fichiers créés/modifiés existent
- [ ] Syntaxe Python valide (py_compile)
- [ ] Imports corrects
- [ ] Respect du design system
- [ ] Tests passent (si applicable)

## Gestion d'Erreurs Pendant Exécution

### Erreur Détectée

```json
{
  "status": "error_detected",
  "subtask": {
    "id": 2,
    "name": "Créer queries"
  },
  "error": {
    "type": "ImportError",
    "message": "cannot import name 'Employe' from 'database.models.effectifs'",
    "file": "database/queries/effectifs_queries.py",
    "line": 3
  },
  "attempt": 1
}
```

**Action** : Lire ERROR-HANDLING.md et tenter correction

### Correction Réussie

```json
{
  "status": "error_fixed",
  "subtask": {
    "id": 2,
    "name": "Créer queries"
  },
  "fix_applied": "Correction de l'import : from database.models.effectifs import Employe",
  "attempt": 1,
  "tests_passed": true
}
```

### Échec Définitif (3 tentatives)

```json
{
  "status": "execution_failed",
  "subtask": {
    "id": 2,
    "name": "Créer queries"
  },
  "error": {
    "type": "ImportError",
    "message": "...",
    "attempts": 3,
    "last_fix_tried": "..."
  },
  "completed_subtasks": [1],
  "action": "Lire ERROR-HANDLING.md pour pattern, RETOURNER à Claude"
}
```

## Format de Retour Final

### Succès

```json
{
  "status": "execution_completed",
  "summary": {
    "task_name": "Création Module Effectifs",
    "duration": "8h15min",
    "subtasks_completed": 7,
    "files_created": [
      "database/models/effectifs.py",
      "database/queries/effectifs_queries.py"
    ],
    "files_modified": [
      "main.py"
    ],
    "tests_passed": true,
    "tests_count": 18
  },
  "next_action": "Lire ARCHIVING.md pour archiver"
}
```

### Échec

```json
{
  "status": "execution_failed",
  "summary": {
    "task_name": "Création Module Effectifs",
    "duration": "2h30min",
    "subtasks_completed": 2,
    "subtasks_failed": 1,
    "error": {
      "subtask": "Créer queries",
      "attempts": 3,
      "last_error": "..."
    }
  },
  "next_action": "Lire ERROR-HANDLING.md, puis ARCHIVING.md"
}
```
