# Capabilities - Système d'Apprentissage

## Structure

```
capabilities/
  _registry.json          # Index de toutes les capacités
  frameworks/             # Frameworks (React, NiceGUI, FastAPI, etc.)
  libraries/              # Libraries (lodash, requests, etc.)
  patterns/               # Design patterns, architecture patterns
  tools/                  # Outils (Docker, Git workflows, etc.)
  languages/              # Langages (Python conventions, etc.)
  project-guidelines/     # Guidelines spécifiques au projet
```

## Format Capacité

Chaque capacité est un fichier JSON :

```json
{
  "id": "framework-name",
  "name": "Framework Name",
  "category": "frameworks",
  "source": "url|file|user_dictated",
  "source_url": "https://...",
  "learned_date": "YYYY-MM-DD",
  "triggers": ["keyword1", "keyword2"],
  "knowledge": {
    "best_practices": [],
    "common_patterns": [],
    "common_errors": [],
    "file_structure": "",
    "key_components": []
  },
  "execution_hints": {
    "planning": "",
    "validation": "",
    "execution": ""
  },
  "documentation": "..."
}
```

## Usage

1. Claude.md détecte documentation (lien, fichier, règles)
2. Claude.md extrait avec WebFetch/Read
3. Claude.md prépare "APPRENTISSAGE REQUIS :"
4. workflow-executor ÉTAPE 0 crée/enrichit capacité
5. workflow-executor charge capacité dans ÉTAPE 1 (Context)
6. Capacité disponible pour Planning, Execution, Validation
