# Archiving - Archivage Post-Tâche

## Objectif

Mettre à jour TOUS les fichiers de contexte après CHAQUE tâche. **OBLIGATOIRE** et **NON NÉGOCIABLE**.

---

## ✅ CHECKLIST D'ARCHIVAGE OBLIGATOIRE

⚠️ **AVANT DE RETOURNER LE RÉSULTAT FINAL, VÉRIFIE CETTE CHECKLIST** ⚠️

### 📋 Obligatoire (État du Projet)

- [ ] **1.** `.claude/context/tasks.md` MIS À JOUR
  - Section "✅ Terminées" + statistiques MAJ

- [ ] **2.** `.claude/context/system-state.md` MIS À JOUR
  - État + modules + métriques MAJ

- [ ] **2.5.** `.claude/skills/workflow-executor/project-registry.json` MIS À JOUR (SI nouveaux dossiers)
  - Enrichi avec dossiers créés/détectés
  - Timestamp `last_scan` MAJ

### ⭐ CRITIQUE (Registres Codebase - OBLIGATOIRE selon modifications)

**⚠️ CES 5 REGISTRES SONT LE CŒUR DE LA MÉMOIRE DU SYSTÈME ⚠️**

- [ ] **3.** `.claude/context/codebase/structure.md` + "Last updated" *(si nouveaux dossiers)*
- [ ] **4.** `.claude/context/codebase/database.md` + "Last updated" *(si nouveaux models)*
- [ ] **5.** `.claude/context/codebase/api.md` + "Last updated" *(si nouvelles routes)*
- [ ] **6.** `.claude/context/codebase/components.md` + "Last updated" *(si nouveaux composants)*
- [ ] **7.** `.claude/context/codebase/dependencies.md` + "Last updated" *(si nouvelles deps)*

### 📝 Si Applicable

- [ ] **8.** `.claude/context/error-patterns.md` *(si erreur rencontrée)*
- [ ] **9.** `.claude/context/improvements-log.md` *(si amélioration significative)*
- [ ] **10.** `.claude/context/decisions-log.md` *(si décision technique)*

---

**⚠️ SI UN SEUL ⭐ NON COCHÉ → ARCHIVAGE INCOMPLET → NE PAS RETOURNER LE RÉSULTAT**

---

## 📋 Fichiers de Contexte

### 1. tasks.md (OBLIGATOIRE)

**Emplacement** : `.claude/context/tasks.md`

**Actions** :
1. Ajouter tâche dans "✅ Terminées"
2. MAJ statistiques (total tâches, temps investi)
3. Retirer de "En cours" si présent

**Template** :
```markdown
### [Nom de la Tâche]
- **Date** : YYYY-MM-DD
- **Durée** : Xh
- **Fichiers créés** : X
- **Fichiers modifiés** : X
- **Description** : [description courte]

---

## 📊 Statistiques
- **Total tâches terminées** : X → X+1
- **Temps total investi** : Xh → Yh
```

---

### 2. system-state.md (OBLIGATOIRE)

**Emplacement** : `.claude/context/system-state.md`

**Actions** :
1. MAJ modules disponibles
2. MAJ technologies/base de données
3. MAJ métriques performance

---

### 2.5. project-registry.json (SI NOUVEAUX DOSSIERS)

**Emplacement** : `.claude/skills/workflow-executor/project-registry.json`

**Objectif** : Persister les nouveaux dossiers/fichiers dans le registry pour chargement automatique futurs workflows.

#### Workflow d'enrichissement

- [ ] **Si workflow a créé nouveaux dossiers/fichiers** :
  - Récupérer liste depuis plan (`new_folders` créé en ÉTAPE 5)
  - Pour chaque dossier dans `new_folders` :
    - Déduire `purpose` depuis contexte/nom dossier
    - Générer `triggers` automatiquement (voir algorithme ci-dessous)
    - Déterminer `load_priority` (voir règles ci-dessous)
    - Ajouter à `project-registry.json` avec `created_by: "workflow"`, `created_at`, `files_pattern`

- [ ] **Si ÉTAPE 1 a reçu enrichissement user (format YAML-like)** :
  - Pour chaque dossier fourni :
    - **Si purpose: ignore** → Ajouter avec `purpose: "ignored"`, `load_priority: "never"`, `triggers: []`
    - **Sinon** → Générer triggers depuis purpose, ajouter avec métadonnées complètes
  - Marquer `created_by: "user"`

- [ ] **Si ÉTAPE 1 a détecté dossier avec README** :
  - Confirmer métadonnées parsées
  - Générer triggers depuis purpose
  - Ajouter avec métadonnées complètes

- [ ] Mettre à jour `last_scan` timestamp

#### Déterminer load_priority

**Règles automatiques** :

```
high → Dossiers critiques : migrations, config, env, models, database
medium → Dossiers fonctionnels : api, workers, services, ui, components
low → Dossiers auxiliaires : docs, scripts, utils, tests, static
```

**OU deviner selon triggers** :
- `database, migration, schema, model` → high
- `api, service, worker, ui, component` → medium
- `doc, script, util, test, static` → low

#### Algorithme génération triggers

**1. Extraction basique** :
- Nom dossier (singulier + pluriel) : `workers` → `["workers", "worker"]`
- Split purpose par espaces/virgules
- Lowercase + suppression stop-words (le, la, de, pour, avec, et, dans, etc.)

**2. Détection technologies** (pattern matching dans purpose) :
- `celery` → ajouter: `["celery", "task", "worker", "background"]`
- `django` → ajouter: `["django", "orm", "model"]`
- `fastapi` → ajouter: `["fastapi", "api", "endpoint", "route"]`
- `sqlalchemy` → ajouter: `["sqlalchemy", "orm", "database", "db"]`
- `redis` → ajouter: `["redis", "cache", "queue"]`
- `nicegui` → ajouter: `["nicegui", "ui", "interface", "page"]`
- `react` → ajouter: `["react", "component", "jsx"]`

**3. Synonymes contextuels** :
- `job` → ajouter: `["background", "task", "worker"]`
- `api` → ajouter: `["endpoint", "route", "service"]`
- `database` → ajouter: `["db", "schema", "migration"]`
- `ui` → ajouter: `["interface", "page", "component"]`

**4. Déduplication** : Supprimer doublons de la liste finale

**Exemple complet** :
```
Dossier : workers
Purpose : "Job processing avec Celery"

1. Basique : workers → ["workers", "worker"]
2. Split : "Job processing avec Celery" → ["job", "processing", "celery"]
3. Stop-words : avec → supprimé
4. Technologies : celery → ["celery", "task", "worker", "background"]
5. Synonymes : job → ["background", "task", "worker"]
6. Fusion : ["workers", "worker", "job", "processing", "celery", "task", "background"]
7. Déduplication : ["workers", "worker", "job", "processing", "celery", "task", "background"]
```

#### Exemple - Workflow crée /workers

```json
{
  "path": "workers",
  "purpose": "Background job processing with Celery",
  "created_by": "workflow",
  "created_at": "2025-11-05",
  "triggers": ["worker", "job", "background", "celery", "task"],
  "load_priority": "medium",
  "files_pattern": "*.py"
}
```

#### Exemple - User ignore /temp

```json
{
  "path": "temp",
  "purpose": "ignored",
  "created_by": "user",
  "created_at": "2025-11-05",
  "triggers": [],
  "load_priority": "never",
  "files_pattern": null
}
```

**Note** : Dossiers `purpose: "ignored"` ne seront JAMAIS chargés ni redemandés dans futurs workflows.

#### Format d'affichage

```
Archivage :
✅ tasks.md mis à jour
✅ system-state.md mis à jour
✅ project-registry.json enrichi (2 nouveaux dossiers)
```

---

### 3. error-patterns.md (SI ERREUR)

**Emplacement** : `.claude/context/error-patterns.md`

**Quand** : Si erreur rencontrée pendant exécution

**Template** :
```markdown
- id: ERR-XXX
  type: [ErrorType]
  symptom: "[message erreur]"
  context: "[contexte]"
  solution: "[solution]"
  status: resolved|unresolved
  reported_date: YYYY-MM-DD
```

---

### 4. improvements-log.md (SI AMÉLIORATION)

**Quand** : Si amélioration significative ou composant réutilisable créé

---

### 5. decisions-log.md (SI DÉCISION TECHNIQUE)

**Quand** : Si décision technique importante prise

---

## 🔥 REGISTRES CODEBASE (CRITIQUE)

**⚠️ LE CŒUR DE LA MÉMOIRE DU PROJET ⚠️**

**📖 LIRE D'ABORD** : `guides/REGISTRES.md` pour comprendre quand et comment MAJ chaque registre.

Sans ces registres, le système perd sa mémoire et refait les mêmes erreurs.

### 6.1. structure.md

**Quand** : Nouveaux dossiers/changement arborescence

**Template strict** :
```markdown
## Key Directories
- `dir/` - Description courte
```

**Actions** :
1. MAJ section "## Root" avec arborescence
2. MAJ section "## Key Directories"
3. MAJ "Last updated: YYYY-MM-DD"

---

### 6.2. database.md

**Quand** : Nouveaux models/tables/relations

**Template strict** :
```markdown
### ModelName
File: `path/to/file`
Table: `table_name`
Relations: → OtherModel (foreign_key)
Key fields: field1, field2, field3
```

**Actions** :
1. Ajouter nouveau model avec template
2. MAJ "Last updated: YYYY-MM-DD"

---

### 6.3. api.md

**Quand** : Nouvelles routes API

**Template strict** :
```markdown
## ResourceName
File: `path/to/file`
- METHOD /path - Description courte
```

**Actions** :
1. Ajouter routes avec template
2. MAJ "Last updated: YYYY-MM-DD"

---

### 6.4. components.md

**Quand** : Nouveaux composants UI

**Template strict** :
```markdown
## CategoryName
File: `path/to/file`
Purpose: Description courte
```

**Actions** :
1. Ajouter composant avec template
2. Organiser par catégorie
3. MAJ "Last updated: YYYY-MM-DD"

---

### 6.5. dependencies.md

**Quand** : Nouvelles dépendances installées

**Template strict** :
```markdown
## Stack Name (Language)
File: `path/to/file`
- package version - Purpose courte
```

**Actions** :
1. Ajouter package avec template
2. Organiser par stack (Backend, Frontend, etc.)
3. MAJ "Last updated: YYYY-MM-DD"

---

## ⚠️ RÈGLES CRITIQUES REGISTRES

1. **TOUJOURS respecter template strict** (visible en haut de chaque fichier)
2. **TOUJOURS MAJ "Last updated: YYYY-MM-DD"**
3. **Rester ULTRA LÉGER** (nom + fichier + info clé, pas détails exhaustifs)
4. **Pas de doublons** (registres = références, pas doc complète)
5. **Si erreur MAJ registre** → Logger dans error-patterns.md, continuer

---

## ⚠️ VÉRIFICATION FINALE AVANT RETOUR

**CHECKLIST COMPLÈTE ?**

✅ **OUI** → Items 1-2 cochés + Items 3-7 cochés (si modifications) → Retourner résultat final

❌ **NON** → Un item ⭐ manquant → **NE PAS RETOURNER** → Compléter archivage

---

**⚠️ RAPPEL CRITIQUE ⚠️**

Sans les 5 registres codebase (items 3-7), le système **PERD SA MÉMOIRE** et refera les mêmes erreurs !

C'est LA partie la plus importante de l'archivage.

---

## ❌ ANTI-PATTERNS (NE PAS FAIRE)

### ❌ Anti-pattern #1 : "J'ai terminé sans archiver"
**Symptôme** : Retourner résultat final sans MAJ contexte
**Conséquence** : Système perd toute mémoire de la tâche
**Solution** : TOUJOURS archiver AVANT de retourner

### ❌ Anti-pattern #2 : "Les registres n'ont pas changé"
**Symptôme** : Dire "pas de changement" alors que nouveaux fichiers créés
**Conséquence** : Progressive Disclosure casse, doublons créés
**Solution** : Vérifier CHAQUE registre avec questions guide (REGISTRES.md)

### ❌ Anti-pattern #3 : "Oublier Last updated"
**Symptôme** : MAJ registre mais oublier date
**Conséquence** : Impossible de savoir si info à jour
**Solution** : TOUJOURS MAJ "Last updated: YYYY-MM-DD"

### ❌ Anti-pattern #4 : "Archiver tasks.md seulement"
**Symptôme** : MAJ tasks.md mais ignorer system-state.md et registres
**Conséquence** : Archivage incomplet = perte mémoire partielle
**Solution** : Suivre CHECKLIST complète (items 1-10)

### ❌ Anti-pattern #5 : "Registres verbeux"
**Symptôme** : Copier code complet dans registres
**Conséquence** : Prompt fatigue, registres illisibles
**Solution** : Format ultra-léger (1 ligne max, voir REGISTRES.md)
