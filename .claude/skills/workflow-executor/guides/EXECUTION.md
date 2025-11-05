# Execution - Exécution des Tâches

## Objectif

Exécuter le plan méthodiquement, sous-tâche par sous-tâche, EN SILENCE.

---

## ✅ CHECKLIST (Pour chaque sous-tâche)

- [ ] 1. Marquer sous-tâche "en cours"
- [ ] 2. Exécuter (créer/modifier fichiers)
- [ ] 3. Valider (tests, syntaxe, fichiers créés)
- [ ] 4. SI ERREUR → Lire ERROR-HANDLING.md (max 3 tentatives)
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
- Lire ERROR-HANDLING.md
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

### Si erreur détectée :
1. Lire ERROR-HANDLING.md
2. Identifier type erreur + solution
3. Appliquer correction (tentative 1/3)
4. Valider correction
5. Si échec → Réessayer (max 3 fois)
6. Si échec définitif → RETOURNER erreur à Claude

### Après exécution (succès ou échec) :
→ Passer à ÉTAPE 7 (ARCHIVING.md)
