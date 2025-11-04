# Impact Analysis - Analyse d'Impact

## Objectif

Évaluer la complexité, l'impact et les risques d'une tâche AVANT de commencer.

## Grille d'Évaluation

### 1. Complexité Temporelle

| Durée Estimée | Classification |
|---------------|----------------|
| < 30 min      | TRIVIALE       |
| 30min - 2h    | SIMPLE         |
| 2h - 8h       | COMPLEXE       |
| > 8h          | MAJEURE        |

### 2. Portée des Changements

| Nombre de Fichiers | Impact        |
|-------------------|---------------|
| 1-2 fichiers      | MINEUR        |
| 3-5 fichiers      | MODÉRÉ        |
| 6-10 fichiers     | IMPORTANT     |
| > 10 fichiers     | MAJEUR        |

### 3. Modules Impactés

| Modules Touchés              | Impact    |
|-----------------------------|-----------|
| 1 module existant           | MINEUR    |
| 2-3 modules existants       | MODÉRÉ    |
| Nouveau module complet      | MAJEUR    |
| Refactoring multi-modules   | MAJEUR    |

### 4. Risques Identifiés

**Risque CRITIQUE** si au moins un des critères :
- Migration de base de données
- Breaking changes dans l'API
- Modification du système d'authentification
- Refactoring majeur du core

**Risque ÉLEVÉ** si au moins un des critères :
- Modification de modèles existants
- Changement de structure de fichiers
- Ajout de dépendances externes

**Risque MODÉRÉ** si :
- Nouvelle fonctionnalité isolée
- Modification de composants UI existants

**Risque FAIBLE** si :
- Ajout de fonction simple
- Modification de style/CSS
- Correction de bug ponctuel

## Classification Finale

### CHANGEMENT MINEUR
- Durée < 2h
- 1-2 fichiers
- 1 module
- Risque faible

**→ PAS de validation utilisateur nécessaire**

### CHANGEMENT MODÉRÉ
- Durée 2-4h
- 3-5 fichiers
- 2 modules
- Risque modéré

**→ Validation recommandée mais optionnelle**

### CHANGEMENT MAJEUR
- Durée ≥ 4h
- > 5 fichiers
- Nouveau module ou refactoring
- Risque élevé ou critique

**→ VALIDATION UTILISATEUR OBLIGATOIRE**

## Exemple d'Analyse

### Demande : "Créer le module Effectifs complet"

**Analyse** :
- ⏱️ Durée estimée : 8-10h → **MAJEURE**
- 📂 Fichiers : ~15 fichiers → **MAJEUR**
- 🏗️ Modules : Nouveau module complet → **MAJEUR**
- ⚠️ Risques :
  - Migration BDD (nouveau tables) → **ÉLEVÉ**
  - Breaking changes potentiels → **ÉLEVÉ**

**Classification Finale** : **CHANGEMENT MAJEUR**

**Action** : Lire VALIDATION.md et RETOURNER à Claude avec `needs_validation: true`

---

### Demande : "Ajoute une fonction pour calculer la TVA"

**Analyse** :
- ⏱️ Durée estimée : 30min → **SIMPLE**
- 📂 Fichiers : 2 fichiers (utils/finance.py + test) → **MINEUR**
- 🏗️ Modules : 1 module existant → **MINEUR**
- ⚠️ Risques : Aucun → **FAIBLE**

**Classification Finale** : **CHANGEMENT MINEUR**

**Action** : Continuer directement (pas de validation nécessaire)

## Format de Retour

```json
{
  "impact_analysis": {
    "classification": "MAJEUR",
    "estimated_time": "8-10h",
    "files_affected": 15,
    "modules_impacted": ["Database", "Components", "Pages", "Tests"],
    "risks": [
      {
        "type": "ÉLEVÉ",
        "description": "Migration BDD avec nouvelles tables"
      },
      {
        "type": "ÉLEVÉ",
        "description": "Breaking changes potentiels sur l'API"
      }
    ],
    "validation_required": true
  }
}
```
