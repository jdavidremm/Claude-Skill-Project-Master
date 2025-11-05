# Registres Codebase - MÉMOIRE SYSTÈME

## Objectif

⭐ **LES 5 REGISTRES SONT LA MÉMOIRE DU SYSTÈME** ⭐

Sans eux, le système oublie tout et refait les mêmes erreurs.

---

## ⭐ LES 5 REGISTRES OBLIGATOIRES

### 1. structure.md
**Quand MAJ** : Nouveau dossier/fichier créé, réorganisation
**Contenu** :
```markdown
Last updated: YYYY-MM-DD

## Arborescence
/dossier
  /sous-dossier
    - fichier.py - [description courte]

## Dossiers Clés
- `/dossier` - [purpose]
```

### 2. database.md
**Quand MAJ** : Nouveau model, nouveau champ, nouvelle relation
**Contenu** :
```markdown
Last updated: YYYY-MM-DD

## Models
### ModelName (fichier: path/to/model.py)
- champ1: Type - [description]
- champ2: Type - [description]

**Relations** :
- relation_name → AutreModel
```

### 3. api.md
**Quand MAJ** : Nouvelle route, modification endpoint
**Contenu** :
```markdown
Last updated: YYYY-MM-DD

## Routes
### /api/endpoint
- **Fichier** : path/to/file.py
- **Méthode** : GET|POST|PUT|DELETE
- **Purpose** : [description courte]
- **Params** : param1, param2
```

### 4. components.md
**Quand MAJ** : Nouveau composant UI, modification majeure
**Contenu** :
```markdown
Last updated: YYYY-MM-DD

## Composants
### ComponentName (fichier: path/to/component.py)
- **Purpose** : [description courte]
- **Props/Params** : param1, param2
- **Usage** : [exemple 1 ligne]
```

### 5. dependencies.md
**Quand MAJ** : Nouvelle dépendance ajoutée (pip install, npm install)
**Contenu** :
```markdown
Last updated: YYYY-MM-DD

## Dépendances
- package-name==version - [purpose]
- autre-package==version - [purpose]
```

---

## ⚠️ RÈGLES CRITIQUES

### 1. ULTRA-LÉGERS
- **1 ligne par item** maximum
- **Pas de code complet**, juste références
- **Si détails nécessaires** → Read fichier spécifique

### 2. TOUJOURS MAJ "Last updated"
```markdown
Last updated: 2025-01-15  ✅ CORRECT
Last updated: 2025-01-10  ❌ OUBLIÉ DE MAJ
```

### 3. ARCHIVAGE CONDITIONNEL
- **SI aucune modification dans ce registre** → Pas de MAJ nécessaire
- **SI modification** → MAJ obligatoire

**Exemples** :
- Tâche ajoute nouveau endpoint → MAJ `api.md` obligatoire
- Tâche modifie seulement CSS → Aucun registre à MAJ

### 4. VÉRIFICATION AVANT ARCHIVAGE
**Question à se poser pour chaque registre** :
- `structure.md` : Ai-je créé/déplacé des dossiers/fichiers ?
- `database.md` : Ai-je créé/modifié des models ?
- `api.md` : Ai-je créé/modifié des routes/endpoints ?
- `components.md` : Ai-je créé/modifié des composants UI ?
- `dependencies.md` : Ai-je installé de nouvelles dépendances ?

**Si OUI** → MAJ registre + "Last updated"
**Si NON** → Laisser tel quel

---

## 📋 Workflow d'Archivage Registres

```
Pour chaque registre des 5 :

1. ❓ Ce registre est-il impacté par la tâche ?

   NON → Passer au suivant
   OUI → Continuer étape 2

2. ✏️ MAJ contenu du registre
   - Ajouter nouveaux items
   - Format ultra-léger (1 ligne max)

3. 📅 MAJ "Last updated: YYYY-MM-DD"

4. ✅ Passer au registre suivant
```

---

## ⚠️ CONSÉQUENCES SI OUBLI

**Sans les registres à jour** :
- ❌ Système recrée composants existants (doublons)
- ❌ Système ignore patterns établis (incohérence)
- ❌ Système répète erreurs résolues
- ❌ Progressive Disclosure impossible

**Avec les registres à jour** :
- ✅ Système réutilise l'existant
- ✅ Système maintient cohérence
- ✅ Système apprend des erreurs
- ✅ Charge uniquement ce qui est nécessaire
