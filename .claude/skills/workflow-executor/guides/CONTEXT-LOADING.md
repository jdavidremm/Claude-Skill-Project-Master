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

Si un fichier obligatoire (⭐) manque ou est vide → Impossible de continuer avec contexte incomplet.
