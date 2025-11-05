# Archiving - Archivage Post-Tâche

## Objectif

Mettre à jour TOUS les fichiers de contexte après CHAQUE tâche. **OBLIGATOIRE** et **NON NÉGOCIABLE**.

---

## ✅ CHECKLIST D'ARCHIVAGE

### Obligatoire (État du Projet)

- [ ] `.claude/context/tasks.md` → Section "✅ Terminées" + statistiques
- [ ] `.claude/context/system-state.md` → État + modules + métriques

### ⭐ CRITIQUE (Registres Codebase - selon modifications)

- [ ] `.claude/context/codebase/structure.md` + "Last updated" (si nouveaux dossiers)
- [ ] `.claude/context/codebase/database.md` + "Last updated" (si nouveaux models)
- [ ] `.claude/context/codebase/api.md` + "Last updated" (si nouvelles routes)
- [ ] `.claude/context/codebase/components.md` + "Last updated" (si nouveaux composants)
- [ ] `.claude/context/codebase/dependencies.md` + "Last updated" (si nouvelles deps)

### Si Applicable

- [ ] `.claude/context/error-patterns.md` (si erreur rencontrée)
- [ ] `.claude/context/improvements-log.md` (si amélioration significative)
- [ ] `.claude/context/decisions-log.md` (si décision technique)

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

## ⚠️ VÉRIFICATION FINALE

**SI UN SEUL ITEM OBLIGATOIRE NON COCHÉ → ARCHIVAGE INCOMPLET → À REFAIRE**

**⚠️ REGISTRES CODEBASE SONT CRITIQUES** : Sans eux, perte de mémoire !
