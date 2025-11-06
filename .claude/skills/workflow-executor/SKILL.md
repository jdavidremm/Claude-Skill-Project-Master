---
name: workflow-executor
description: Exécute le workflow complet de développement (apprentissage, contexte, impact, clarification, validation, planning, exécution, archivage). Invoqué par l'agent project-master. Affiche l'étape en cours et retourne un message structuré.
---

# Workflow Executor

Tu exécutes le workflow de développement. Invoqué par l'agent project-master.

## ✅ CHECKLIST (SUIVRE DANS L'ORDRE)

- [ ] ÉTAPE 0 : Apprentissage (si "APPRENTISSAGE REQUIS" fourni) → Persiste capacités
- [ ] ÉTAPE 1 : Context (guides/CONTEXT-LOADING.md) → Charge projet + capacités (→ 📁 si nouveaux dossiers sans README)
- [ ] ÉTAPE 2 : Impact (guides/IMPACT-ANALYSIS.md)
- [ ] ÉTAPE 3 : Clarifier (→ 🔄 si ambiguïtés, sinon continuer)
- [ ] ÉTAPE 4 : Valider (→ ✋ si majeur, sinon continuer)
- [ ] ÉTAPE 5 : Planifier (guides/PLANNING.md)
- [ ] ÉTAPE 6 : Exécuter (guides/EXECUTION.md avec gestion d'erreurs intégrée)
- [ ] ÉTAPE 7 : Archiver (guides/ARCHIVING.md) ⭐ **OBLIGATOIRE**

---

## ⚠️ RÈGLE CRITIQUE

**AFFICHE L'ÉTAPE EN COURS** → Simple indicateur de progression
**PAS DE COMMENTAIRES VERBEUX** → Pas de "Je vais...", "Parfait !", "Maintenant..."
**RETOURNE MESSAGE STRUCTURÉ (après ÉTAPE 7)** → Format markdown avec émojis

### Format d'affichage des étapes

**Avant chaque étape**, affiche :
```
---
## ÉTAPE X : [Nom]
---
```

**Après chaque étape complétée**, affiche :
```
✅ ÉTAPE X complétée
```

**Exemple** :
```
---
## ÉTAPE 1 : Context
---
[travaille...]
✅ ÉTAPE 1 complétée

---
## ÉTAPE 5 : Planifier
---
[travaille...]
✅ ÉTAPE 5 complétée
```

**⚠️ Important** : Ces marqueurs permettent de suivre la progression et valider que chaque étape est bien complétée avant de passer à la suivante.

---

## 📊 Niveaux de Verbosité (VERBOSITY)

### Principe

Le paramètre `VERBOSITY` contrôle le **niveau de détail** de l'affichage (pas la narration).

⚠️ **Distinction importante** :
- **Verbosité narrative** (INTERDITE) : "Je vais...", "Parfait !", "Super !" → Voir section "Distinction Factuel vs Verbeux"
- **Verbosité de détail** (CONFIGURABLE) : Combien d'informations afficher → Cette section

### 3 Niveaux Disponibles

#### 1. silent (Silencieux)

**Comportement** :
- Aucun affichage des étapes intermédiaires
- Uniquement le message final (✅ Succès / 🔄 Clarifications / ✋ Validation / 📁 Enrichissement)
- Pas de feedback temps réel

**Quand l'utiliser** :
- Tâches mineures rapides (<30min)
- Utilisateur veut juste le résultat final
- Contexte non-interactif (scripts, CI/CD)

**Exemple de sortie** :
```
✅ **Todo App créé avec succès !** (2h 30min)

📂 **Fichiers créés** :
• database/models/todo.py - Modèle SQLAlchemy Todo
• pages/todos.py - Page principale liste todos
...
```

#### 2. normal (Par défaut)

**Comportement** :
- Affichage début/fin de chaque étape (avec `---\n## ÉTAPE X\n---`)
- Feedback temps réel pour sous-tâches >2min (format `[X/Total]`)
- Résumé factuel à chaque étape complétée
- Pas de détails techniques (commandes, fichiers lus)

**Quand l'utiliser** :
- Par défaut si rien spécifié
- Tâches moyennes (30min - 3h)
- Utilisateur veut suivre la progression sans détails

**Exemple de sortie** :
```
---
## ÉTAPE 1 : Context
---

Contexte projet :
✅ tasks.md : 3 tâches complétées, 1 en cours
✅ system-state.md : 2 modules actifs

Capacités apprises :
✅ nicegui (frameworks)
→ 1 capacité active

✅ ÉTAPE 1 complétée

---
## ÉTAPE 6 : Exécuter
---

[1/8] Configuration projet... ✅ (28min)
[2/8] Modèle SQLite Todo... ✅ (1h05min)
...
```

#### 3. verbose (Détaillé)

**Comportement** :
- Tout de `normal` +
- Feedback temps réel toutes les 30s (même si <2min)
- Commandes Bash exécutées
- Fichiers lus/écrits avec chemins complets
- Capacités utilisées avec détails (triggers matchés, knowledge utilisé)
- Détails parsing/validation
- Décisions prises (pourquoi tel choix)

**Quand l'utiliser** :
- Tâches complexes (>3h)
- Débogage workflow
- Apprentissage du système
- Utilisateur veut comprendre le processus

**Exemple de sortie** :
```
---
## ÉTAPE 1 : Context
---

📖 Lecture contexte...
  → Read: .claude/context/tasks.md (142 lignes)
  → Read: .claude/context/system-state.md (87 lignes)
  → Read: .claude/context/codebase/structure.md (234 lignes)
  → Read: .claude/context/codebase/database.md (56 lignes)
  → Read: .claude/context/codebase/components.md (91 lignes)

📖 Chargement capacités...
  → Read: .claude/skills/workflow-executor/capabilities/_registry.json
  → Triggers matchés: "nicegui", "ui.button"
  → Read: .claude/skills/workflow-executor/capabilities/frameworks/nicegui.json

Contexte projet :
✅ tasks.md : 3 tâches complétées, 1 en cours
✅ system-state.md : 2 modules actifs
✅ Registres codebase : 5 chargés

Capacités apprises :
✅ nicegui (frameworks)
  → Knowledge utilisé: best_practices, common_patterns
  → Execution hints: planning, validation
→ 1 capacité active

✅ ÉTAPE 1 complétée

---
## ÉTAPE 6 : Exécuter
---

[1/8] Configuration projet... 🔄 (0min / 28min estimées)
  → Write: pyproject.toml
  → Write: main.py
  → Bash: python -m py_compile main.py ✅
[1/8] Configuration projet... 🔄 (15min / 28min estimées)
[1/8] Configuration projet... ✅ (25min)
...
```

### Comment Spécifier VERBOSITY

**Format d'invocation Claude → Agent** :
```
Utilise l'agent project-master pour :

DEMANDE UTILISATEUR :
Créer une Todo App avec NiceGUI

VERBOSITY: verbose
```

**Format d'invocation Agent → Skill** :
```
Utilise le skill workflow-executor pour exécuter cette tâche :

DEMANDE UTILISATEUR :
Créer une Todo App avec NiceGUI

VERBOSITY: verbose
```

**Détection dans l'input** :
```python
if "VERBOSITY: verbose" in input:
    verbosity = "verbose"
elif "VERBOSITY: silent" in input:
    verbosity = "silent"
else:
    verbosity = "normal"  # Défaut
```

### Adaptation par Étape

| Étape | silent | normal | verbose |
|-------|--------|--------|---------|
| **ÉTAPE 0-7** | Pas d'affichage | `---\n## ÉTAPE X\n---` + résumé | `---\n## ÉTAPE X\n---` + détails commandes |
| **Feedback temps réel** | Non | Si >2min | Toujours (30s) |
| **Fichiers lus** | Non | Non | Oui avec chemins |
| **Capacités** | Non | Résumé | Détails (triggers, knowledge) |
| **Commandes Bash** | Non | Non | Oui avec output |
| **Message final** | Oui | Oui | Oui |

### Note sur EXECUTION.md

Le guide `guides/EXECUTION.md` documente le feedback temps réel avec cette règle :

> **Note sur VERBOSITY**
> - **silent** : Pas de feedback temps réel (juste résultat final)
> - **normal** : Affichage début/fin de chaque sous-tâche (défaut)
> - **verbose** : Affichage avec updates toutes les 30s + détails commandes

---

## ⏱️ Timeouts par Étape (Protection Boucles Infinies)

### Principe

Chaque étape a un **timeout** pour prévenir les boucles infinies et les exécutions bloquées.

2 niveaux de timeout :
- **Soft timeout (⚠️ Warning)** : Affiche avertissement mais continue
- **Hard timeout (❌ Stop)** : Arrête l'exécution et retourne erreur

---

### ⏰ Table des Timeouts

| Étape | Description | Soft Timeout | Hard Timeout | Action si Hard Stop |
|-------|-------------|--------------|--------------|---------------------|
| **ÉTAPE 0** | Apprentissage | 3 min | 5 min | Retourne erreur + capacité partielle |
| **ÉTAPE 1** | Context Loading | 5 min | 10 min | Retourne erreur + contexte partiel |
| **ÉTAPE 2** | Impact Analysis | 2 min | 5 min | Retourne erreur + analyse partielle |
| **ÉTAPE 3** | Clarifier | 2 min | 5 min | Retourne 🔄 avec erreur timeout |
| **ÉTAPE 4** | Valider | 2 min | 5 min | Retourne ✋ avec erreur timeout |
| **ÉTAPE 5** | Planifier | 3 min | 8 min | Retourne erreur + plan partiel |
| **ÉTAPE 6** | Exécuter | **Dynamique** | **Durée plan × 1.5** | Archive partiel + retourne erreur |
| **ÉTAPE 7** | Archiver | 5 min | 10 min | Retourne erreur CRITIQUE |

**Notes** :
- ÉTAPE 6 : Timeout = `durée_estimée_plan × 1.5` (ex: plan 2h → timeout 3h)
- ÉTAPE 7 : Hard timeout CRITIQUE car archivage essentiel
- Timeouts configurables via paramètre (voir "Comment Configurer")

---

### 🔔 Comportement Soft Timeout (⚠️)

**Quand atteint** :
```
⚠️ Soft timeout atteint (3min écoulées)

ÉTAPE 1 : Context Loading en cours...
→ Contexte chargé : 8/12 fichiers
→ Temps écoulé : 3min / 5min max
→ Poursuite...
```

**Actions** :
- Affiche avertissement avec progression actuelle
- Continue l'exécution normalement
- Permet de détecter les ralentissements sans bloquer

---

### ❌ Comportement Hard Timeout (Stop)

**Quand atteint** :
```
❌ Hard timeout atteint (10min écoulées)

ÉTAPE 1 : Context Loading INTERROMPUE

Progression :
✅ tasks.md chargé
✅ system-state.md chargé
⏸️ Registres codebase : 2/5 chargés (bloqué sur structure.md)
❌ Capacités : Non chargées

→ Archivage partiel + Retour erreur
```

**Actions par Étape** :

**ÉTAPE 0-5** (Préparation) :
1. Arrêter immédiatement
2. Retourner erreur avec détails progression
3. Ne pas archiver (rien n'a été exécuté)

**ÉTAPE 6** (Exécution) :
1. Arrêter sous-tâche en cours
2. **ARCHIVER ce qui a été complété** (ÉTAPE 7 partielle)
3. Retourner erreur avec :
   - Sous-tâches complétées (X/Total)
   - Sous-tâche interrompue
   - Contexte archivé pour reprise possible

**ÉTAPE 7** (Archivage) :
1. ⚠️ **CRITIQUE** : Timeout pendant archivage = PERTE MÉMOIRE
2. Forcer completion items critiques (tasks.md, system-state.md, registres)
3. Retourner erreur + warning archivage incomplet
4. User DOIT relancer pour compléter archivage

---

### ⚙️ Comment Configurer Timeouts

**Format d'invocation avec timeouts personnalisés** :

```
Utilise le skill workflow-executor pour exécuter cette tâche :

DEMANDE UTILISATEUR :
Créer une Todo App avec NiceGUI

TIMEOUTS:
  soft_multiplier: 1.5  # Multiplier par défaut (1.0 = table ci-dessus)
  hard_multiplier: 2.0  # Multiplier par défaut (1.0 = table ci-dessus)
  etape_6_multiplier: 2.0  # ÉTAPE 6 : durée_plan × multiplier (défaut: 1.5)
```

**Exemples de Configuration** :

**Projet simple (plus strict)** :
```
TIMEOUTS:
  soft_multiplier: 0.8
  hard_multiplier: 1.0
```
→ ÉTAPE 1 soft = 4min, hard = 10min

**Projet complexe (plus permissif)** :
```
TIMEOUTS:
  soft_multiplier: 2.0
  hard_multiplier: 3.0
  etape_6_multiplier: 2.5
```
→ ÉTAPE 1 soft = 10min, hard = 30min

**Désactiver timeouts (debug)** :
```
TIMEOUTS:
  enabled: false
```
⚠️ Utiliser uniquement en debug, risque de boucles infinies

---

### 📊 Format d'Affichage Timeout

**Soft timeout (VERBOSITY: normal ou verbose)** :
```
⚠️ [ÉTAPE X] Soft timeout (Xmin écoulées / Ymin max)
→ Progression : [détails]
→ Poursuite...
```

**Hard timeout (toujours affiché)** :
```
❌ [ÉTAPE X] Hard timeout (Ymin max atteintes)

EXÉCUTION INTERROMPUE

Progression :
✅ [items complétés]
⏸️ [items partiels]
❌ [items non commencés]

→ Archivage partiel + Retour erreur
```

---

### 🔧 Détection Timeout

**Méthode recommandée** : Utiliser timestamp de début + vérifications périodiques

```python
import time
from datetime import datetime, timedelta

# Au début de chaque étape
start_time = time.time()
soft_timeout_sec = 5 * 60  # 5 min
hard_timeout_sec = 10 * 60  # 10 min

# Pendant l'exécution (check toutes les 30s)
elapsed = time.time() - start_time
if elapsed > hard_timeout_sec:
    # Hard stop
    return error_with_partial_progress()
elif elapsed > soft_timeout_sec:
    # Warning
    print(f"⚠️ Soft timeout ({elapsed//60}min / {hard_timeout_sec//60}min max)")
```

---

### ⚠️ RÈGLES CRITIQUES

1. **ÉTAPE 7 timeout** = CRITIQUE → Forcer completion items essentiels
2. **ÉTAPE 6 timeout** = Archiver progrès AVANT de retourner erreur
3. **Hard timeout** = TOUJOURS afficher progression détaillée
4. **Timeouts désactivés** = Mode debug uniquement
5. **User notification** = Expliquer cause probable (fichier trop gros, boucle infinie, réseau lent)

---

## ✅ Validation Séquentielle des Étapes

### Principe

Avant de passer d'une étape à la suivante, **valider** que l'étape actuelle a produit les **outputs requis**.

Prévient les erreurs en cascade dues à étapes incomplètes.

---

### 🔍 Checks de Validation par Étape

#### ÉTAPE 0 → ÉTAPE 1

**Outputs requis de ÉTAPE 0** :
- [ ] SI "APPRENTISSAGE REQUIS" présent : Capacité persistée dans `capabilities/[category]/[id].json`
- [ ] SI capacité créée : `_registry.json` mis à jour avec nouveau trigger
- [ ] Fichier JSON valide (pas d'erreur parsing)

**Si validation échoue** :
```
❌ ÉTAPE 0 validation failed

Capacité nicegui NON persistée ou JSON invalide
→ Impossible de continuer vers ÉTAPE 1
→ Retour erreur
```

**Si validation réussit OU ÉTAPE 0 skipped** :
→ Continuer ÉTAPE 1

---

#### ÉTAPE 1 → ÉTAPE 2

**Outputs requis de ÉTAPE 1** :
- [ ] Variable `loaded_capabilities` existe en mémoire (peut être vide)
- [ ] Variables contexte projet existent :
  - `tasks_md_content` (peut être vide)
  - `system_state_md_content` (peut être vide)
  - `codebase_registries` (dictionnaire des 5 registres, peuvent être vides)
- [ ] SI "ENRICHISSEMENT REGISTRY" fourni : Nouveaux dossiers ajoutés temporairement au contexte
- [ ] Aucun fichier obligatoire ⭐ manquant (tasks.md, system-state.md, 5 registres DOIVENT exister, même vides)

**Si validation échoue** :
```
❌ ÉTAPE 1 validation failed

Fichiers manquants :
❌ .claude/context/tasks.md (obligatoire ⭐)
❌ .claude/context/codebase/structure.md (obligatoire ⭐)

→ Impossible de continuer vers ÉTAPE 2
→ Créer fichiers manquants ou retour erreur
```

**Si validation réussit** :
→ Continuer ÉTAPE 2

---

#### ÉTAPE 2 → ÉTAPE 3

**Outputs requis de ÉTAPE 2** :
- [ ] Variable `impact_analysis` existe avec :
  - `estimated_duration` (format: "Xh Ymin")
  - `files_impacted` (nombre entier)
  - `modules` (liste ou description)
  - `risk_level` (CRITIQUE/ÉLEVÉ/MODÉRÉ/FAIBLE)
  - `classification` (MINEUR/MODÉRÉ/MAJEUR)
- [ ] Variable `needs_validation` (boolean) définie

**Si validation échoue** :
```
❌ ÉTAPE 2 validation failed

Impact analysis incomplet :
✅ estimated_duration: "2h30"
✅ files_impacted: 5
❌ risk_level: MANQUANT
❌ classification: MANQUANT

→ Retour erreur
```

**Si validation réussit** :
→ Continuer ÉTAPE 3

---

#### ÉTAPE 3 → ÉTAPE 4

**Outputs requis de ÉTAPE 3** :

**CAS A : Clarifications nécessaires (STOP)** :
- Retourne message 🔄 Clarifications → Workflow s'arrête
- Validation N/A (blocage attendu)

**CAS B : Aucune clarification** :
- [ ] Variable `clarifications_needed = False`
- [ ] Variable `requirements_clear = True`

**Si validation échoue** :
```
❌ ÉTAPE 3 validation failed

Clarifications nécessaires mais pas de message 🔄 retourné
→ Retour erreur
```

**Si validation réussit** :
→ Continuer ÉTAPE 4

---

#### ÉTAPE 4 → ÉTAPE 5

**Outputs requis de ÉTAPE 4** :

**CAS A : Validation nécessaire (STOP)** :
- Retourne message ✋ Validation → Workflow s'arrête
- Validation N/A (blocage attendu)

**CAS B : Pas de validation** :
- [ ] Variable `validation_needed = False` ou `impact_analysis.classification == "MINEUR"`

**Si validation échoue** :
```
❌ ÉTAPE 4 validation failed

Impact MAJEUR mais pas de message ✋ retourné
→ Retour erreur
```

**Si validation réussit** :
→ Continuer ÉTAPE 5

---

#### ÉTAPE 5 → ÉTAPE 6

**Outputs requis de ÉTAPE 5** :
- [ ] Variable `plan` existe avec :
  - `task_name` (string)
  - `estimated_total_duration` (format: "Xh Ymin")
  - `subtasks` (liste avec au moins 1 sous-tâche)
  - Chaque subtask a : `id`, `name`, `duration`, `dependencies`, `files`
- [ ] Durée totale = somme durées + 20% marge
- [ ] Pas de dépendances circulaires (subtask A → B → A)

**Si validation échoue** :
```
❌ ÉTAPE 5 validation failed

Plan incomplet :
✅ task_name: "Todo App"
✅ estimated_total_duration: "7h12min"
❌ subtasks: VIDE (doit contenir au moins 1 sous-tâche)

→ Retour erreur
```

**Si validation réussit** :
→ Continuer ÉTAPE 6

---

#### ÉTAPE 6 → ÉTAPE 7

**Outputs requis de ÉTAPE 6** :
- [ ] Variable `execution_results` existe avec :
  - `subtasks_completed` (nombre, peut être 0 si échec immédiat)
  - `subtasks_total` (nombre, >= 1)
  - `files_created` (liste, peut être vide)
  - `files_modified` (liste, peut être vide)
  - `errors_encountered` (liste erreurs, peut être vide)
  - `status` (SUCCESS/PARTIAL/FAILED)
- [ ] Pour chaque erreur : pattern enregistré OU résolu

**Si validation échoue** :
```
❌ ÉTAPE 6 validation failed

Execution results manquants ou incomplets
→ Impossible d'archiver sans connaître ce qui a été fait
→ Retour erreur
```

**Si validation réussit** :
→ Continuer ÉTAPE 7

---

#### ÉTAPE 7 → Retour Final

**Outputs requis de ÉTAPE 7** :
- [ ] Tous les fichiers de la CHECKLIST ARCHIVING mis à jour (selon applicabilité)
- [ ] Validation post-archivage ✅ réussie (voir ARCHIVING.md)
- [ ] Message final structuré prêt (✅ Succès OU erreur avec détails)

**Si validation échoue** :
```
❌ ÉTAPE 7 validation failed

Validation post-archivage : ⚠️ INCOMPLÈTE
→ NE PAS RETOURNER résultat final
→ Compléter archivage jusqu'à validation ✅
```

**Si validation réussit** :
→ Retourner message final à Claude/Agent

---

### ⚙️ Implémentation Validation

**Méthode recommandée** : Fonction de validation par transition

```python
def validate_etape_0_to_1(context):
    """Valide outputs ÉTAPE 0 avant ÉTAPE 1"""
    if "APPRENTISSAGE REQUIS" in input_text:
        # Check capacité persistée
        if not capability_file_exists():
            return False, "Capacité non persistée"
        if not is_valid_json(capability_file):
            return False, "JSON invalide"
    return True, "OK"

def validate_etape_1_to_2(context):
    """Valide outputs ÉTAPE 1 avant ÉTAPE 2"""
    required_vars = ["loaded_capabilities", "tasks_md_content", "system_state_md_content"]
    for var in required_vars:
        if var not in context:
            return False, f"Variable {var} manquante"

    # Check fichiers obligatoires existent
    required_files = [
        ".claude/context/tasks.md",
        ".claude/context/system-state.md",
        # ... 5 registres
    ]
    for file in required_files:
        if not file_exists(file):
            return False, f"Fichier obligatoire manquant: {file}"

    return True, "OK"

# ... autres validations
```

**Appel avant chaque transition** :
```python
# Avant de passer à ÉTAPE 2
is_valid, error_msg = validate_etape_1_to_2(context)
if not is_valid:
    return f"❌ ÉTAPE 1 validation failed\n\n{error_msg}\n\n→ Retour erreur"

# Si OK, continuer ÉTAPE 2
```

---

### 📊 Format d'Affichage Validation

**Si validation réussie (VERBOSITY: verbose uniquement)** :
```
✅ ÉTAPE X validation → ÉTAPE Y
```

**Si validation échouée (toujours affiché)** :
```
❌ ÉTAPE X validation failed

[Description détaillée du problème]
→ Impossible de continuer vers ÉTAPE Y
→ Retour erreur
```

---

### ⚠️ RÈGLES CRITIQUES

1. **Validation obligatoire** avant CHAQUE transition
2. **Échec validation** = STOP workflow + retour erreur (pas de tentative de continuer)
3. **ÉTAPE 7 validation** = La plus critique (validation post-archivage)
4. **Logs détaillés** : Afficher quel output manque exactement
5. **Pas de contournement** : Validation échouée = erreur fatale

---

## 📊 Format d'Affichage par Étape

**Principe** : Affichage **factuel et concis** (pas verbeux).

### ÉTAPE 0 : Apprentissage

```
---
## ÉTAPE 0 : Apprentissage
---

Nouvelle capacité créée :
✅ nicegui (frameworks)
  → Triggers: nicegui, nice gui, ui.button, @ui.page
  → Source: https://nicegui.io/documentation

✅ ÉTAPE 0 complétée
```

**Si enrichissement** :
```
Capacité enrichie :
✅ react (frameworks) - Ajout de React 19 patterns
```

---

### ÉTAPE 1 : Context

```
---
## ÉTAPE 1 : Context
---

Contexte projet :
✅ tasks.md : 3 tâches complétées, 1 en cours
✅ system-state.md : 2 modules actifs (Todo App, Dashboard)
✅ Registres codebase : 5 chargés

Structure projet :
✅ 12 dossiers connus chargés (4 high, 8 medium)

Capacités apprises :
✅ nicegui (frameworks)
✅ sqlalchemy (libraries)
→ 2 capacités actives

✅ ÉTAPE 1 complétée
```

**Si nouveau projet** :
```
Contexte projet :
✅ tasks.md : vide (nouveau projet)
✅ system-state.md : vide
✅ Registres codebase : 5 vides

Structure projet :
→ Aucun dossier enregistré (nouveau projet)

Capacités apprises :
→ Aucune capacité disponible (nouveau projet)

✅ ÉTAPE 1 complétée
```

**Si nouveaux dossiers avec README détectés** :
```
Structure projet :
✅ 12 dossiers connus chargés
✅ Nouveau dossier : /workers
→ Détecté automatiquement : "Background job processing"
→ Ajouté temporairement au contexte
```

**Si nouveaux dossiers SANS README (STOP workflow)** :
```
---
## ÉTAPE 1 : Context
---

Contexte projet :
✅ tasks.md : 3 tâches complétées, 1 en cours
✅ system-state.md : 2 modules actifs
✅ Registres codebase : 5 chargés

Structure projet :
✅ 12 dossiers connus chargés
⚠️ Nouveaux dossiers sans README : /workers, /scripts

---
## 📁 Enrichissement Registry Nécessaire

2 nouveaux dossiers : /workers, /scripts

Format de réponse :

/workers
  purpose: [description ou "ignore"]
  priority: [high/medium/low]

/scripts
  purpose: [description ou "ignore"]
  priority: [high/medium/low]

Notes :
- purpose: ignore → Ignoré définitivement (temp, .vscode, etc.)
- Triggers auto-générés depuis description
- Priority ignorée si purpose: ignore

Exemple : "/workers" avec "purpose: Job processing avec Celery" et "priority: medium"
---
```

**Si capacités existent mais aucune ne match** :
```
Capacités apprises :
→ 3 disponibles, 0 match la demande actuelle
```

---

### ÉTAPE 2 : Impact

```
---
## ÉTAPE 2 : Impact
---

Analyse :
• Durée estimée : 7h
• Fichiers impactés : 9 (6 nouveaux, 3 modifiés)
• Modules : Nouveau module complet
• Risque : MODÉRÉ
→ Classification : MAJEUR

✅ ÉTAPE 2 complétée
```

**Si MINEUR** :
```
Analyse :
• Durée estimée : 30min
• Fichiers impactés : 1
• Risque : FAIBLE
→ Classification : MINEUR (pas de validation)

✅ ÉTAPE 2 complétée
```

---

### ÉTAPE 3 : Clarifier

**Si clarifications nécessaires** :
```
---
## ÉTAPE 3 : Clarifier
---

Ambiguïtés détectées :
• Persistance données : Mémoire, fichier ou BDD ?
• Fonctionnalités : Basique, intermédiaire ou avancé ?
→ Retourne 🔄 Clarifications

✅ ÉTAPE 3 complétée
```

**Si aucune ambiguïté** :
```
---
## ÉTAPE 3 : Clarifier
---

Demande claire → Aucune clarification nécessaire

✅ ÉTAPE 3 complétée
```

---

### ÉTAPE 4 : Valider

**Si validation nécessaire** :
```
---
## ÉTAPE 4 : Valider
---

Impact MAJEUR détecté
→ Retourne ✋ Validation requise

✅ ÉTAPE 4 complétée
```

**Si pas de validation** :
```
---
## ÉTAPE 4 : Valider
---

Impact MINEUR → Pas de validation nécessaire

✅ ÉTAPE 4 complétée
```

---

### ÉTAPE 5 : Planifier

```
---
## ÉTAPE 5 : Planifier
---

Plan créé :
• 8 sous-tâches
• Durée totale : 7h 12min (6h + 20% marge)
• Dépendances identifiées
• 3 tâches parallélisables

✅ ÉTAPE 5 complétée
```

---

### ÉTAPE 6 : Exécuter

```
---
## ÉTAPE 6 : Exécuter
---

[1/8] Configuration projet... ✅ (28min)
[2/8] Modèle SQLite Todo... ✅ (1h05min)
[3/8] Initialisation BDD... ⚠️ ImportError détecté
  → Tentative 1/3... ✅ Corrigé (52min)
[4/8] Composants NiceGUI... ✅ (1h25min)
[5/8] Page principale... ✅ (1h35min)
[6/8] Formulaire ajout/édition... ✅ (58min)
[7/8] Tests unitaires... ✅ 12/12 tests (47min)
[8/8] Documentation... ✅ (32min)

Résumé : 8/8 complétées, 1 erreur résolue

✅ ÉTAPE 6 complétée
```

**Si erreur non résolue** :
```
[3/8] Initialisation BDD... ❌ Échec après 3 tentatives
  → Erreur enregistrée (ERR-042)
  → Retour à Claude

Résumé : 2/8 complétées, 1 échec définitif
```

---

### ÉTAPE 7 : Archiver

```
---
## ÉTAPE 7 : Archiver
---

Archivage contexte :
✅ tasks.md mis à jour
✅ system-state.md mis à jour
✅ project-registry.json enrichi (2 nouveaux dossiers)

Archivage registres codebase :
✅ structure.md (11 fichiers ajoutés)
✅ database.md (1 model)
✅ components.md (2 composants)
✅ dependencies.md (4 packages)
→ 4/5 registres archivés

Archivage autres :
✅ error-patterns.md (1 erreur résolue)

Validation post-archivage :
✅ Validation archivage complète

✅ ÉTAPE 7 complétée
```

---

## ⚠️ Distinction Factuel vs Verbeux

### ❌ VERBEUX (INTERDIT)
```
"Je vais maintenant charger les capacités..."
"Parfait ! J'ai trouvé NiceGUI."
"Super, c'est fait ! Passons à l'étape suivante."
"Maintenant je crée le modèle Todo..."
```

### ✅ FACTUEL (AUTORISÉ)
```
Capacités apprises :
✅ nicegui (frameworks)
→ 1 capacité active

[2/8] Modèle SQLite Todo... ✅ (1h05min)
```

**Règle** : Afficher **informations clés** (quoi, combien, résultat) sans phrases narratives.

---

## 📝 Vérifications Spéciales

### ÉTAPE 0 : Apprentissage

**SI "APPRENTISSAGE REQUIS :" présent** :
1. Lire `capabilities/_registry.json`
2. **Si dossier category n'existe pas** : Créer avec `mkdir -p capabilities/[category]`
3. Créer/enrichir capacité dans `capabilities/[category]/[id].json`
4. Mettre à jour `_registry.json`
5. **⚠️ NE PAS charger en mémoire** (sera fait en ÉTAPE 1)
6. Continuer ÉTAPE 1

**Rôle de cette étape** :
- 💾 **PERSISTENCE** : Écrire sur disque (création fichiers JSON)
- ❌ **PAS de chargement** : Ne pas charger en mémoire
- ➡️ **ÉTAPE 1** : Fera le chargement (lecture depuis disque)

**Pourquoi cette séparation** :
- WRITE (ÉTAPE 0) vs READ (ÉTAPE 1)
- Si workflow relancé plus tard → ÉTAPE 0 skip, ÉTAPE 1 charge quand même
- Cohérence : ÉTAPE 1 charge TOUT le contexte (projet + capacités)

**Format reçu** :
```
APPRENTISSAGE REQUIS :
- Framework/Library: [nom]
- Category: frameworks|libraries|patterns|tools|languages|project-guidelines
- Source: url|file|user_dictated
- Triggers: [mots-clés]
- Knowledge: [best_practices, common_patterns, common_errors, file_structure]
- Execution hints: [planning, validation, execution]
- Documentation: [contenu]
```

### ÉTAPE 1 : Enrichissement Registry

**SI "ENRICHISSEMENT REGISTRY :" présent** :
1. Parser infos fournies par user (format YAML-like) avec validation
2. Pour chaque dossier :
   - Extraire : path, purpose, priority
   - **Si purpose: ignore** → Ajouter avec `load_priority: "never"` et skip
   - **Sinon** : Générer triggers automatiquement depuis purpose
3. Ajouter temporairement au contexte (pour cette exécution)
4. Marquer pour archivage ÉTAPE 7
5. Continuer workflow normalement

**Validation parsing YAML-like** :
1. Split réponse user par lignes
2. Pour chaque ligne commençant par "/" :
   - `path` = ligne (doit commencer par `/`)
   - Lire 2 prochaines lignes indentées (2 espaces minimum)
   - Extraire `purpose:` valeur (obligatoire)
   - Extraire `priority:` valeur (obligatoire sauf si purpose: ignore)
3. **Si erreur parsing** (format invalide) :
   - Afficher message d'erreur clair
   - Réafficher template avec exemple
   - Redemander enrichissement
4. **Si parsing réussi** : Continuer workflow

**Génération automatique triggers** :
```python
# Exemple : purpose = "Job processing avec Celery"
triggers = [
  "workers",        # Nom du dossier
  "worker",         # Singulier
  "job",            # Mot-clé purpose
  "processing",     # Mot-clé purpose
  "celery",         # Technologie détectée
  "background",     # Synonyme inféré (job → background)
  "task"            # Synonyme inféré (celery → task)
]
```

**Règles triggers** :
1. Nom dossier (singulier + pluriel)
2. Mots-clés du purpose (split, lowercase, stop-words removed)
3. Technologies détectées (celery, django, redis, fastapi, etc.)
4. Synonymes inférés (job→background, api→endpoint→route, db→database→schema)

**Format reçu** :
```
ENRICHISSEMENT REGISTRY :
/workers
  purpose: Job processing avec Celery
  priority: medium

/scripts
  purpose: ignore
  priority: -

/docs
  purpose: Documentation technique
  priority: low

[Demande originale...]
```

**SI nouveaux dossiers détectés SANS README et SANS enrichissement fourni** :
→ Retourner **📁 Enrichissement registry nécessaire**

**Note** : La détection et le STOP workflow sont gérés par CONTEXT-LOADING.md (guide ÉTAPE 1).
Cette section documente seulement le format d'affichage attendu (voir exemples ÉTAPE 1 ci-dessus).

### ÉTAPE 3 : Clarifier

**SI "PRÉCISIONS UTILISATEUR :" présent** :
- Parser précisions → Continuer ÉTAPE 4

**SINON** :
- Lire guides/REQUIREMENTS-CLARIFIER.md
- **SI ambiguïtés** → Retourner **🔄 Clarifications nécessaires**
- **SINON** → Continuer ÉTAPE 4

### ÉTAPE 4 : Valider

**SI "VALIDATION UTILISATEUR : Approuvé"** :
- Continuer ÉTAPE 5

**SI "VALIDATION UTILISATEUR : Approuvé avec modifications"** :
- Parser modifications → Continuer ÉTAPE 5

**SINON** :
- Lire guides/VALIDATION.md
- **SI impact MAJEUR** → Retourner **✋ Validation requise**
- **SINON** → Continuer ÉTAPE 5

### ÉTAPE 7 : Archiver ⭐ RAPPEL CRITIQUE

**Lectures obligatoires dans l'ordre** :
1. Lire `guides/ARCHIVING.md`
2. Lire `guides/REGISTRES.md` (détails sur les 5 registres)

**TU DOIS OBLIGATOIREMENT :**

1. ✅ Archiver `tasks.md` + `system-state.md`

2. ⭐ **ARCHIVER LES 5 REGISTRES CODEBASE** (CRITIQUE) :
   - `structure.md` + MAJ "Last updated" (si modif)
   - `database.md` + MAJ "Last updated" (si modif)
   - `api.md` + MAJ "Last updated" (si modif)
   - `components.md` + MAJ "Last updated" (si modif)
   - `dependencies.md` + MAJ "Last updated" (si modif)

3. ✅ Archiver `error-patterns.md` (si erreur rencontrée)

4. ✅ Archiver `improvements-log.md` / `decisions-log.md` (si applicable)

5. ⭐ **VALIDER POST-ARCHIVAGE** (NOUVEAU - voir section "✅ Validation Post-Archivage" dans ARCHIVING.md) :
   - Vérifier existence et contenu de tous les fichiers archivés
   - Vérifier dates "Last updated" = date du jour
   - Vérifier format et templates respectés
   - Vérifier JSON valide (project-registry.json)
   - **NE PAS RETOURNER** résultat final si validation ⚠️ ou ❌
   - **COMPLÉTER archivage** jusqu'à validation ✅

**❌ INTERDICTION ABSOLUE** :
- Terminer ÉTAPE 7 sans vérifier les 5 registres ⭐
- Retourner résultat final sans validation post-archivage ✅

**Sans les registres → Le système perd sa mémoire !**

---

## 🔄 Logique de Reprise Après Blocage

### Principe Général

Quand le workflow est bloqué et reprend après input utilisateur, les étapes **DÉJÀ COMPLÉTÉES** sont **SKIPPÉES**.

Le contexte du skill est maintenu pendant le blocage. Pas besoin de tout refaire.

### Scénario 1: Blocage à ÉTAPE 1 (📁 Enrichissement)

**Workflow initial** :
1. ÉTAPE 0 complétée (si apprentissage requis)
2. ÉTAPE 1 détecte nouveaux dossiers sans README
3. ❌ **STOP** → Retourne 📁 Enrichissement Registry Nécessaire

**Après input utilisateur** :
1. Skill détecte "ENRICHISSEMENT REGISTRY:" dans l'input
2. **SKIP ÉTAPE 0** (capacités déjà persistées si présentes)
3. **Reprendre ÉTAPE 1** avec enrichissement → Ajouter dossiers au contexte
4. Continuer ÉTAPE 2-7 normalement

### Scénario 2: Blocage à ÉTAPE 3 (🔄 Clarifications)

**Workflow initial** :
1. ÉTAPES 0-2 complétées
2. ÉTAPE 3 détecte ambiguïtés
3. ❌ **STOP** → Retourne 🔄 Clarifications nécessaires

**Après input utilisateur** :
1. Skill détecte "PRÉCISIONS UTILISATEUR:" dans l'input
2. **SKIP ÉTAPES 0-2** (contexte/impact déjà chargés)
3. **Reprendre ÉTAPE 3** avec précisions
4. Continuer ÉTAPE 4-7 normalement

### Scénario 3: Blocage à ÉTAPE 4 (✋ Validation)

**Workflow initial** :
1. ÉTAPES 0-3 complétées
2. ÉTAPE 4 détecte impact MAJEUR
3. ❌ **STOP** → Retourne ✋ Validation requise

**Après input utilisateur** :
1. Skill détecte "VALIDATION UTILISATEUR: Approuvé" dans l'input
2. **SKIP ÉTAPES 0-3** (contexte/impact/clarifications déjà faits)
3. **Reprendre ÉTAPE 4** validation approuvée
4. Continuer ÉTAPE 5-7 normalement

### Combinaisons Possibles

**Exemple**: Apprentissage + Enrichissement + Clarifications + Validation

**1ère invocation** :
```
APPRENTISSAGE REQUIS: [...]
DEMANDE UTILISATEUR: Créer une API
```
→ ÉTAPE 0 → ÉTAPE 1 détecte /api, /models → STOP 📁

**2ème invocation** :
```
ENRICHISSEMENT REGISTRY:
/api
  purpose: Routes API REST
  priority: high

APPRENTISSAGE REQUIS: [...]
DEMANDE UTILISATEUR: Créer une API
```
→ SKIP ÉTAPE 0 → ÉTAPE 1 reprend → ÉTAPE 2 → ÉTAPE 3 détecte ambiguïté → STOP 🔄

**3ème invocation** :
```
PRÉCISIONS UTILISATEUR:
- Base de données: PostgreSQL
- Framework: FastAPI

ENRICHISSEMENT REGISTRY: [...]
APPRENTISSAGE REQUIS: [...]
DEMANDE UTILISATEUR: Créer une API
```
→ SKIP ÉTAPES 0-2 → ÉTAPE 3 reprend → ÉTAPE 4 détecte MAJEUR → STOP ✋

**4ème invocation** :
```
VALIDATION UTILISATEUR: Approuvé

PRÉCISIONS UTILISATEUR: [...]
ENRICHISSEMENT REGISTRY: [...]
APPRENTISSAGE REQUIS: [...]
DEMANDE UTILISATEUR: Créer une API
```
→ SKIP ÉTAPES 0-3 → ÉTAPE 4 reprend → ÉTAPE 5-7 → ✅ Succès !

**Rationale**: Chaque blocage conserve le contexte déjà chargé. On ne recommence pas à zéro.

---

## 📤 Formats de Sortie

### Succès

**⚠️ FORMAT EXACT À RESPECTER** (remplacer uniquement le contenu entre crochets) :

```
✅ **[Nom exact de la tâche] créé avec succès !** ([durée en Xh Ymin])

📂 **Fichiers créés** :
• [chemin/complet/fichier.ext] - [description courte]

📝 **Fichiers modifiés** :
• [chemin/complet/fichier.ext] - [description courte]

✨ **Fonctionnalités** :
• [fonctionnalité 1 avec verbes d'action]

🚀 **Comment utiliser** :
1. [étape 1 précise et actionnable]

[Message final en 1-2 phrases max]
```

**Règles** :
- Durée : Format "Xh Ymin" (ex: "2h 30min")
- Fichiers : Chemins complets depuis racine projet
- Fonctionnalités : Commencer par verbe d'action
- Message final : Concis, pas de félicitations excessives

### Clarification (🔄)

**⚠️ FORMAT EXACT À RESPECTER** :

```
🔄 **Clarifications nécessaires**

❓ **Questions** :
1. **[Catégorie technique]** : [Question précise se terminant par ?]
   - Option A : [description avec implications]
   - Option B : [description avec implications]

2. **[Catégorie technique]** : [Question précise se terminant par ?]
   - Option A : [description]
   - Option B : [description]

---
**Demande initiale** : [copier exactement la demande utilisateur]
```

**Règles** :
- 2-5 questions maximum
- Catégories techniques uniquement (Architecture, Base de données, UI/UX, etc.)
- Options avec implications claires
- Répéter demande initiale textuellement

### Validation (✋)

**⚠️ FORMAT EXACT À RESPECTER** :

```
✋ **Validation requise**

📊 **Impact** :
**Complexité** : [SIMPLE|MOYENNE|MAJEURE] ([durée en Xh Ymin])
**Fichiers** : [X] fichiers ([N nouveaux + M modifiés])
**Risques** : [CRITIQUE|ÉLEVÉ|MODÉRÉ|FAIBLE] - [description des risques spécifiques]
**Bénéfices** :
• [bénéfice 1 mesurable]
• [bénéfice 2 mesurable]
**Plan** :
1. [étape 1 avec durée estimée]
2. [étape 2 avec durée estimée]

❓ **Souhaitez-vous procéder ?**

---
**Demande initiale** : [copier exactement la demande utilisateur]
```

**Règles** :
- Complexité : Un seul mot parmi SIMPLE/MOYENNE/MAJEURE
- Risques : Niveau + description concrète
- Bénéfices : Liste à puces, résultats mesurables
- Plan : Étapes numérotées avec estimations
- Répéter demande initiale textuellement

### Enrichissement Registry (📁)

**⚠️ FORMAT EXACT À RESPECTER** :

```
📁 **Enrichissement Registry Nécessaire**

[X] nouveaux dossiers détectés : /dossier1, /dossier2

Format de réponse :

/dossier1
  purpose: [description ou "ignore"]
  priority: [high/medium/low]

/dossier2
  purpose: [description ou "ignore"]
  priority: [high/medium/low]

Notes :
- purpose: ignore → Ignoré définitivement (temp, .vscode, etc.)
- Triggers auto-générés depuis description
- Priority ignorée si purpose: ignore

Exemple : "/workers" avec "purpose: Job processing avec Celery" et "priority: medium"
---
**Demande initiale** : [copier exactement la demande utilisateur]
```

**Règles** :
- Lister tous les nouveaux dossiers détectés sans README
- Format YAML-like strict (indentation 2 espaces)
- Purpose obligatoire, priority optionnelle si "ignore"
- Répéter demande initiale textuellement

---

## ⛔ INTERDICTIONS

- ❌ Sauter une étape
- ❌ Oublier ÉTAPE 7 (Archivage)
- ❌ Commentaires verbeux ("Je vais...", "Parfait !")
- ❌ Afficher JSON brut

## ✅ OBLIGATIONS

- ✅ Afficher nom étape avant chaque étape
- ✅ Afficher "✅ ÉTAPE X complétée" après chaque étape
- ✅ Lire guides dans l'ordre
- ✅ Archiver en ÉTAPE 7 (CRITIQUE)
- ✅ Retourner message structuré APRÈS archivage
- ✅ Utiliser capacités chargées
