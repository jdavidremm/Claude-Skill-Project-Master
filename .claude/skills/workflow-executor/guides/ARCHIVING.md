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

## ⚠️ VÉRIFICATION FINALE AVANT RETOUR

**CHECKLIST COMPLÈTE ?**

✅ **OUI** → Items 1-2 cochés + Items 3-7 cochés (si modifications) → Retourner résultat final

❌ **NON** → Un item ⭐ manquant → **NE PAS RETOURNER** → Compléter archivage

---

**⚠️ RAPPEL CRITIQUE ⚠️**

Sans les 5 registres codebase (items 3-7), le système **PERD SA MÉMOIRE** et refera les mêmes erreurs !

C'est LA partie la plus importante de l'archivage.
