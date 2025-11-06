# Context Loading - Chargement de l'État du Projet

## Objectif

Charger l'état actuel du projet avant de commencer une nouvelle tâche.

---

## ✅ CHECKLIST DE CHARGEMENT

### Obligatoire (État du Projet)

- [ ] `.claude/context/tasks.md` → Tâches terminées/en cours/en attente
- [ ] `.claude/context/system-state.md` → État actuel, modules, technologies
- [ ] `.claude/context/error-patterns.md` → Erreurs connues à éviter

### ⭐ OBLIGATOIRE (Registres Codebase - ULTRA LÉGERS)

- [ ] `.claude/context/codebase/structure.md` → Arborescence + dossiers clés
- [ ] `.claude/context/codebase/database.md` → Models/tables + relations
- [ ] `.claude/context/codebase/api.md` → Routes API + endpoints
- [ ] `.claude/context/codebase/components.md` → Composants UI + purpose
- [ ] `.claude/context/codebase/dependencies.md` → Dépendances + versions

### ⭐ OBLIGATOIRE (Capacités Apprises)

- [ ] `capabilities/_registry.json` → Lire registre des capacités
- [ ] Détecter triggers dans demande utilisateur
- [ ] Charger capacités pertinentes depuis `capabilities/[category]/[id].json`

**Workflow de chargement** :
1. Lire `capabilities/_registry.json`
2. Parser demande utilisateur pour identifier mots-clés
3. Matcher avec triggers de chaque capacité
4. Charger UNIQUEMENT les capacités qui matchent (Progressive Disclosure)

**Exemple - Chargement unique** :
```
Demande: "Ajoute un bouton avec NiceGUI"

1. Lit _registry.json → 1 capacité disponible : nicegui
2. Triggers nicegui: ["nicegui", "nice gui", "ui.button", "@ui.page"]
3. Détecte "NiceGUI" et "bouton" → Match ! ✅
4. Charge capabilities/frameworks/nicegui.json
5. Capacité NiceGUI disponible pour toutes les étapes
```

**Exemple - Chargement multiple** :
```
Demande: "Crée une API REST avec FastAPI qui utilise SQLAlchemy et suit nos conventions de code"

1. Lit _registry.json → 3 capacités disponibles
2. Matching :
   - "FastAPI" → Match "fastapi" triggers ✅
   - "SQLAlchemy" → Match "sqlalchemy" triggers ✅
   - "conventions" → Match "project-guidelines" triggers ✅
3. Charge 3 fichiers :
   - capabilities/frameworks/fastapi.json
   - capabilities/libraries/sqlalchemy.json
   - capabilities/project-guidelines/coding-standards.json
4. Les 3 capacités disponibles pour toutes les étapes
```

**Si ÉTAPE 0 a créé une nouvelle capacité** :
→ Elle est déjà persistée dans `_registry.json` + fichier JSON
→ Elle sera détectée et chargée ici automatiquement

**Si workflow relancé plus tard (sans apprentissage)** :
→ ÉTAPE 0 skip (pas de "APPRENTISSAGE REQUIS :")
→ Capacités existantes chargées ici quand même ✅

**Progressive Disclosure** :
- Capacités = Version légère (best_practices, common_patterns, execution_hints)
- SI besoin détails exhaustifs → Read `documentation` field du JSON

### 📁 Détection Structure Projet

**Objectif** : Détecter nouveaux dossiers/fichiers créés par workflow OU utilisateur.

#### Workflow de détection

- [ ] Lire `.claude/context/project-registry.json`
- [ ] Lire `ignored_patterns` depuis registry
- [ ] Scanner filesystem (filtré) :
  ```bash
  find . -maxdepth 2 -type d \
    -not -path "*/\.*" \
    -not -path "*/node_modules*" \
    -not -path "*/venv*" \
    -not -path "*/__pycache__*" \
    -not -path "*/dist*" \
    -not -path "*/build*"
  ```
- [ ] Filtrer selon `ignored_patterns` additionnels du registry
- [ ] Diff : `nouveaux = actuels - registry.folders`
- [ ] **SI nouveaux dossiers détectés** → Mode découverte
- [ ] **SINON** → Charger selon `load_priority` et triggers

#### Mode découverte (nouveaux dossiers)

**Pour chaque nouveau dossier** :

1. **Chercher README.md** dans le dossier
   - Si présent → Parser première ligne/paragraphe pour `purpose`
   - Extraire mots-clés pour `triggers`
   - Deviner `load_priority` (migrations→high, api/workers/models→medium, docs/scripts/utils→low)
   - Ajouter temporairement au contexte
   - Marquer pour archivage ÉTAPE 7

2. **Si aucun README** :
   - **STOP workflow**
   - Retourner **📁 Enrichissement registry nécessaire**
   - Afficher template structuré pour TOUS les dossiers sans README
   - Workflow reprendra quand user fournit infos

**Template de réponse** (format YAML-like) :
```
/[dossier]
  purpose: [description ou "ignore"]
  priority: [high/medium/low]
```

**Notes** :
- `purpose: ignore` → Dossier ignoré définitivement (ex: temp, .vscode)
- Triggers auto-générés depuis purpose par workflow
- Priority ignorée si purpose: ignore

#### Chargement contexte connu

**Pour dossiers dans registry** :

- [ ] Matcher `triggers` avec demande utilisateur
- [ ] Charger selon `load_priority` :
  - `high` : Toujours charger
  - `medium` : Si triggers matchent
  - `low` : Seulement si mention explicite

**Exemple - Chargement sélectif** :
```
Demande: "Ajoute une migration pour table users"

Registry :
- /migrations (triggers: ["database", "migration"]) → Match ✅
- /api/routes (triggers: ["api", "endpoint"]) → Pas de match ❌
- /tests (load_priority: "high") → Toujours chargé ✅

Résultat : Charge /migrations + /tests seulement
```

#### Format d'affichage

**Si README présent** :
```
Structure projet :
✅ 12 dossiers connus chargés
✅ Nouveau dossier : /workers
→ Détecté automatiquement : "Background job processing"
→ Ajouté temporairement au contexte
```

**Si aucun README (STOP workflow)** :
```
---
## ÉTAPE 1 : Context
---

Contexte projet :
✅ tasks.md : 3 tâches complétées
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

### Optionnel (Si Pertinent)

- [ ] `.claude/context/improvements-log.md` → Améliorations récentes
- [ ] `.claude/context/decisions-log.md` → Décisions techniques passées
- [ ] `.claude/context/design-system.md` → Conventions de design

---

## 🔍 Progressive Disclosure

Les registres sont **ULTRA LÉGERS** (références + info clé seulement).

**SI besoin de détails complets** :
1. Parser demande utilisateur
2. Identifier fichiers pertinents dans registres
3. Read fichiers spécifiques pour détails

**Exemple** :
```
Demande: "Ajoute endpoint pour archiver un todo"
→ api.md indique routes dans `api/todos.py`
→ SI besoin: Read `api/todos.py` pour pattern exact
→ database.md indique model `models/Todo.py`
→ SI besoin: Read `models/Todo.py` pour champs
```

---

## 🎯 Pourquoi Charger la Codebase ?

✅ **Éviter doublons** : Ne pas recréer ce qui existe
✅ **Cohérence** : Respecter patterns et structure existants
✅ **Réutilisation** : Identifier composants/models réutilisables
✅ **Performance** : Registres légers, Read seulement si besoin

---

## 💾 Gestion Mémoire des Capacités

### Chargement (ÉTAPE 1)

1. Lire `capabilities/_registry.json`
2. Matcher triggers avec demande utilisateur
3. Lire fichiers JSON des capacités matchées
4. **Stocker dans variable `loaded_capabilities`** (contexte du skill en mémoire)

### Utilisation (ÉTAPES 2-6)

Les capacités sont accessibles via `loaded_capabilities` en mémoire:
- **ÉTAPE 2 (Impact)**: Utiliser `best_practices` et `common_errors` pour l'analyse
- **ÉTAPE 5 (Planning)**: Utiliser `execution_hints.planning` et `file_structure`
- **ÉTAPE 6 (Execution)**: Utiliser `common_patterns`, `common_errors.solution`, `execution_hints.execution`

Pas besoin de relire les fichiers JSON à chaque étape.

### Progressive Disclosure - Détails Complets

**Par défaut**: Charger structure légère depuis JSON
- `id`, `name`, `category`, `triggers`
- `best_practices` (liste courte)
- `common_patterns` (noms + descriptions courtes)
- `common_errors` (types + solutions courtes)
- `execution_hints` (listes courtes)

**Si besoin détails exhaustifs** (optionnel):
- ÉTAPE 2: Si `common_errors` matching → Lire détails erreur depuis champ `documentation`
- ÉTAPE 5: Si `file_structure` complexe → Lire documentation complète
- ÉTAPE 6: Si erreur rencontrée → Lire solution détaillée depuis `common_errors.documentation`
- Toute ÉTAPE: Si `execution_hints` mentionne "Voir documentation" → Read champ `documentation` du JSON

**Principe**: Charger léger par défaut, approfondir seulement si nécessaire.

### Après Archivage (ÉTAPE 7)

- Capacités restent **persistées** dans fichiers JSON sur disque (`.claude/skills/workflow-executor/capabilities/[category]/[id].json`)
- Contexte du skill est **libéré** après retour du résultat à Claude
- Prochain workflow rechargera les capacités pertinentes si triggers matchent la demande

**Analogie**: Comme charger des librairies Python avec `import` au début, utilisables partout ensuite.

---

## ⏸️ Détection d'Interruption

Vérifie dans `tasks.md` si tâche marquée "⏸️ En cours" :
- Si interruption → Charger détails + Retourner à Claude pour proposition reprise

---

## ⚠️ AVANT DE PASSER À L'ÉTAPE 2

**Vérifie que tous les items de la CHECKLIST sont cochés** ✅

- État du Projet : tasks.md, system-state.md, error-patterns.md chargés
- Registres Codebase (⭐) : 5 registres chargés
- Capacités Apprises (⭐) : _registry.json lu + capacités pertinentes chargées

Si un fichier obligatoire (⭐) manque → Impossible de continuer avec contexte incomplet.

**Note sur les capacités** :
- Si _registry.json vide (nouveau projet) → OK, continuer
- Si capacités existent MAIS aucune ne match → OK, continuer
- L'essentiel : AVOIR VÉRIFIÉ et tenté le matching

---

## 📊 Note sur VERBOSITY

Le niveau de détail affiché s'adapte selon `VERBOSITY` (voir SKILL.md) :

- **silent** : Pas d'affichage ÉTAPE 1 (chargement silencieux)
- **normal** : Résumé factuel (nombre de fichiers, capacités matchées) - défaut
- **verbose** : Détails complets :
  - Chemins absolus de tous les fichiers lus
  - Nombre de lignes par fichier
  - Triggers matchés pour les capacités
  - Knowledge et execution hints chargés
  - Décisions prises (nouveaux dossiers ignorés, capacités skippées, etc.)
