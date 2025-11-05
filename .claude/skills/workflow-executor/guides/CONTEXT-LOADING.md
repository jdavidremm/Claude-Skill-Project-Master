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
