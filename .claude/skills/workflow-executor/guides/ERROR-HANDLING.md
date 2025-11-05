# Error Handling - Gestion des Erreurs

## Objectif

Diagnostiquer et corriger erreurs systématiquement. Max 3 tentatives. Enregistrer patterns.

---

## ✅ CHECKLIST

- [ ] 1. Identifier type erreur
- [ ] 2. Vérifier pattern connu dans error-patterns.md
- [ ] 3. Appliquer solution (tentative 1/3)
- [ ] 4. Valider correction (`py_compile` + tests)
- [ ] 5. Si échec → Réessayer (max 3 tentatives)
- [ ] 6. Si échec définitif → Enregistrer pattern + RETOURNER à Claude

---

## 🔍 Patterns d'Erreurs Communs

### ERR-001 : ImportError - Module/Attribute Non Trouvé

**Symptôme** : `ImportError: cannot import name 'X' from 'module'`

**Causes** :
1. Classe non définie dans module
2. Typo dans nom classe
3. Import circulaire

**Solutions** :
1. Vérifier classe existe : `grep -n "class X" fichier.py`
2. Vérifier orthographe
3. Réorganiser imports

---

### ERR-002 : AttributeError - datetime.timedelta

**Symptôme** : `AttributeError: 'datetime' has no attribute 'timedelta'`

**Cause** : Import incorrect

**Solution** :
```python
# ✅ Correct
from datetime import datetime, timedelta
delta = timedelta(days=1)
```

---

### ERR-003 : NameError - Variable Non Définie

**Symptôme** : `NameError: name 'X' is not defined`

**Cause** : Import manquant

**Solution** : Ajouter import manquant

---

### ERR-004 : SyntaxError

**Symptôme** : `SyntaxError: invalid syntax at line X`

**Causes** :
1. Parenthèse/crochet non fermé
2. Indentation incorrecte
3. Virgule manquante

**Diagnostic** : `python -m py_compile fichier.py`

---

### ERR-005 : Alembic Migration Failed

**Symptôme** : `Target database is not up to date`

**Solutions** :
```bash
alembic current          # Vérifier état
alembic upgrade head     # Appliquer migrations
```

---

## 🔧 Processus de Correction

### 1. Diagnostic

**a) Identifier type erreur**
- ImportError → ERR-001
- AttributeError → ERR-002
- NameError → ERR-003
- SyntaxError → ERR-004
- Autre → Diagnostic manuel

**b) Vérifier pattern connu**
- Lire `.claude/context/error-patterns.md`
- Chercher pattern ID correspondant
- Appliquer solution documentée

**c) Si pattern inconnu**
- Analyser message erreur
- Identifier cause racine
- Formuler solution

### 2. Application de la Correction

**Pour chaque tentative (max 3)** :
1. Appliquer correction
2. Vérifier syntaxe : `python -m py_compile fichier.py`
3. Lancer tests (si applicable)
4. Si succès → Continuer
5. Si échec → Réessayer avec nouvelle approche

### 3. Si Échec Définitif (3 tentatives)

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

---

## 🛠️ Commandes Diagnostic

```bash
# Vérifier syntaxe
python -m py_compile fichier.py

# Tester imports
python -c "from module import Class"

# Lancer tests
pytest tests/test_xxx.py -v

# Vérifier BDD
alembic current
```

---

## ⚠️ RÈGLES

- ✅ Max 3 tentatives par erreur
- ✅ Toujours enregistrer pattern si échec définitif
- ✅ Valider correction avec `py_compile` + tests
- ❌ Ne pas continuer si 3 échecs (RETOURNER à Claude)
