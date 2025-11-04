# Planning - Planification des Tâches

## Objectif

Créer un plan d'exécution détaillé avec sous-tâches, estimations et dépendances.

## Structure d'un Plan

### Template de Plan

```yaml
plan:
  task_name: "Nom de la tâche principale"
  estimated_total_duration: "8h"
  subtasks:
    - id: 1
      name: "Sous-tâche 1"
      duration: "1h30"
      dependencies: []
      files:
        - "fichier1.py"
        - "fichier2.py"

    - id: 2
      name: "Sous-tâche 2"
      duration: "45min"
      dependencies: [1]  # Dépend de la sous-tâche 1
      files:
        - "fichier3.py"

    - id: 3
      name: "Sous-tâche 3"
      duration: "2h"
      dependencies: [1, 2]
      files:
        - "fichier4.py"
```

## Exemple : Module Effectifs

```yaml
plan:
  task_name: "Création Module Effectifs Complet"
  estimated_total_duration: "8-10h"
  subtasks:
    - id: 1
      name: "Créer models BDD (Employe, Contrat, Presence)"
      duration: "1h30"
      dependencies: []
      files:
        - "database/models/effectifs.py"

    - id: 2
      name: "Créer queries CRUD"
      duration: "1h"
      dependencies: [1]
      files:
        - "database/queries/effectifs_queries.py"

    - id: 3
      name: "Créer migration Alembic"
      duration: "30min"
      dependencies: [1]
      files:
        - "alembic/versions/xxxxx_create_effectifs_tables.py"

    - id: 4
      name: "Créer composants UI (formulaire, tableau, KPI)"
      duration: "2h"
      dependencies: [2]
      files:
        - "components/effectifs/form_employe.py"
        - "components/effectifs/table_employes.py"
        - "components/effectifs/kpi_cards.py"

    - id: 5
      name: "Créer page principale /effectifs"
      duration: "1h30"
      dependencies: [4]
      files:
        - "pages/page_effectifs.py"
        - "main.py"  # Import de la page

    - id: 6
      name: "Créer tests unitaires"
      duration: "1h30"
      dependencies: [2]
      files:
        - "tests/test_effectifs.py"

    - id: 7
      name: "Documenter le module"
      duration: "45min"
      dependencies: [5, 6]
      files:
        - ".claude/documentation/module-effectifs.md"
```

## Estimation des Durées

### Référence pour Estimer

| Type de Tâche                    | Durée Estimée |
|----------------------------------|---------------|
| Model simple (1-2 classes)       | 30-45min      |
| Model complexe (3+ classes)      | 1h-1h30       |
| Queries CRUD complètes           | 45min-1h      |
| Migration Alembic                | 20-30min      |
| Composant UI simple              | 30-45min      |
| Composant UI complexe            | 1h-1h30       |
| Page complète                    | 1h-2h         |
| Tests unitaires (module complet) | 1h-1h30       |
| Documentation                    | 30-45min      |

### Règles d'Estimation

- Ajouter 20% de marge pour les imprévus
- Doubler la durée si technologies inconnues
- Ajouter 30min pour la première occurrence d'un pattern

## Dépendances

### Types de Dépendances

**Dépendance Technique** : Une tâche nécessite le résultat d'une autre
```yaml
- id: 2
  name: "Créer queries"
  dependencies: [1]  # Nécessite les models créés en tâche 1
```

**Dépendance Logique** : Une tâche doit être faite avant pour des raisons de cohérence
```yaml
- id: 7
  name: "Documentation"
  dependencies: [5, 6]  # Documentation après page et tests
```

**Parallélisable** : Tâches sans dépendances peuvent être faites en parallèle
```yaml
- id: 3
  name: "Migration"
  dependencies: [1]

- id: 6
  name: "Tests"
  dependencies: [2]

# 3 et 6 peuvent être faites en parallèle
```

## Format de Retour

```json
{
  "status": "plan_ready",
  "plan": {
    "task_name": "Création Module Effectifs",
    "estimated_total_duration": "8-10h",
    "subtasks": [
      {
        "id": 1,
        "name": "Créer models BDD",
        "duration": "1h30",
        "dependencies": [],
        "files": ["database/models/effectifs.py"]
      }
    ]
  }
}
```
