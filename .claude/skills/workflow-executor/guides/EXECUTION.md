# Execution - Exécution des Tâches

## Objectif

Exécuter le plan méthodiquement, sous-tâche par sous-tâche, EN SILENCE.

---

## ✅ CHECKLIST (Pour chaque sous-tâche)

- [ ] 1. Marquer sous-tâche "en cours"
- [ ] 2. Exécuter (créer/modifier fichiers)
- [ ] 3. Valider (tests, syntaxe, fichiers créés)
- [ ] 4. SI ERREUR → Appliquer gestion d'erreurs (max 3 tentatives)
- [ ] 5. Marquer sous-tâche "complétée"
- [ ] 6. Passer à la suivante (vérifier dépendances)

---

## 📋 Workflow d'Exécution Séquentielle

### Pour chaque sous-tâche :

**1. Marquer comme "en cours"**
- Noter ID + nom + heure début

**2. Exécuter la sous-tâche**
- Créer/modifier fichiers nécessaires
- Respecter conventions projet
- Suivre design system
- Ajouter tests si applicable

**3. Valider la sous-tâche**
- Lancer tests (si présents)
- Vérifier syntaxe (`python -m py_compile`)
- Vérifier fichiers créés

**4. SI ERREUR**
- Voir section "Gestion d'Erreurs" ci-dessous
- Tenter correction (max 3 fois)
- Si échec définitif → RETOURNER erreur à Claude

**5. Marquer comme "complétée"**
- Noter durée réelle + fichiers créés/modifiés

**6. Passer à la suivante**
- Vérifier dépendances satisfaites
- Continuer avec prochaine sous-tâche

---

## 🎯 Ordre d'Exécution Recommandé

1. **Models** (fondation)
2. **Queries** (logique métier)
3. **Migration** (BDD)
4. **Composants** (UI)
5. **Pages** (intégration)
6. **Tests** (validation)
7. **Documentation** (finalisation)

---

## 📐 Conventions à Respecter

### Imports
```python
from nicegui import ui, app
from datetime import date, datetime, timedelta  # Séparément
from database.initialisation import SessionLocal
```

### Design System
```python
from components.design_system import create_kpi_card
from style.theme import init_page
```

### Tests
```python
import pytest
from database.models.xxx import YYY
```

---

## ✅ Vérifications à Chaque Sous-Tâche

- [ ] Fichiers créés/modifiés existent
- [ ] Syntaxe Python valide (`py_compile`)
- [ ] Imports corrects
- [ ] Respect design system
- [ ] Tests passent (si applicable)

---

## ⚠️ Gestion d'Erreurs

### 🔍 Diagnostic d'Erreur

**a) Identifier type erreur**
- ImportError → ERR-001
- AttributeError → ERR-002
- NameError → ERR-003
- SyntaxError → ERR-004
- Alembic Migration → ERR-005
- Autre → Diagnostic manuel

**b) Vérifier pattern connu**
- Lire `.claude/context/error-patterns.md`
- Chercher pattern ID correspondant
- Appliquer solution documentée

**c) Si pattern inconnu**
- Analyser message erreur
- Identifier cause racine
- Formuler solution

### 🔧 Processus de Correction

**Pour chaque tentative (max 3)** :
1. Appliquer correction
2. Vérifier syntaxe : `python -m py_compile fichier.py`
3. Lancer tests (si applicable)
4. Si succès → Continuer
5. Si échec → Réessayer avec nouvelle approche

### 🛠️ Patterns d'Erreurs Communs

**ERR-001 : ImportError**
- Symptôme : `ImportError: cannot import name 'X' from 'module'`
- Causes : Classe non définie, typo, import circulaire
- Solution : Vérifier `grep -n "class X" fichier.py`, vérifier orthographe

**ERR-002 : AttributeError datetime**
- Symptôme : `AttributeError: 'datetime' has no attribute 'timedelta'`
- Solution : `from datetime import datetime, timedelta` (import séparé)

**ERR-003 : NameError**
- Symptôme : `NameError: name 'X' is not defined`
- Solution : Ajouter import manquant

**ERR-004 : SyntaxError**
- Symptôme : `SyntaxError: invalid syntax at line X`
- Causes : Parenthèse non fermée, indentation, virgule manquante
- Diagnostic : `python -m py_compile fichier.py`

**ERR-005 : Alembic Migration**
- Symptôme : `Target database is not up to date`
- Solution : `alembic upgrade head`

### ❌ Si Échec Définitif (3 tentatives)

**a) Enregistrer pattern** dans `.claude/context/error-patterns.md` :
```yaml
- id: ERR-XXX
  type: [ErrorType]
  symptom: "[message]"
  context: "[contexte]"
  root_cause: "[cause]"
  solution: "[tentées]"
  status: unresolved
  reported_date: YYYY-MM-DD
```

**b) RETOURNER à Claude**
- Message d'erreur complet
- 3 fixes tentés
- Pattern enregistré

### Après exécution (succès ou échec) :
→ Passer à ÉTAPE 7 (ARCHIVING.md)

---

## ⚠️ AVANT DE PASSER À L'ÉTAPE 7

**Vérifie que toutes les sous-tâches sont complétées** ✅

- Chaque sous-tâche exécutée et validée
- Fichiers créés/modifiés vérifiés
- Tests passent (si applicable)
- Erreurs résolues ou enregistrées

---

## ❌ ANTI-PATTERNS (NE PAS FAIRE)

### ❌ Anti-pattern #1 : "Sauter validation"
**Symptôme** : Marquer sous-tâche "complétée" sans tester
**Conséquence** : Erreurs découvertes trop tard
**Solution** : TOUJOURS valider (py_compile + tests)

### ❌ Anti-pattern #2 : "4ème tentative erreur"
**Symptôme** : Réessayer indéfiniment même erreur
**Conséquence** : Boucle infinie, perte de temps
**Solution** : Max 3 tentatives → RETOURNER à Claude

### ❌ Anti-pattern #3 : "Ignorer dépendances"
**Symptôme** : Exécuter sous-tâche avant ses dépendances
**Conséquence** : Erreurs import, fichier manquant
**Solution** : Vérifier dépendances AVANT exécution

### ❌ Anti-pattern #4 : "Ne pas enregistrer erreur"
**Symptôme** : Échec définitif sans MAJ error-patterns.md
**Conséquence** : Système répète même erreur
**Solution** : TOUJOURS enregistrer pattern si échec

### ❌ Anti-pattern #5 : "Tests ignorés"
**Symptôme** : "Tests cassés mais feature marche"
**Conséquence** : Régression future, code fragile
**Solution** : Tests DOIVENT passer ou être corrigés
