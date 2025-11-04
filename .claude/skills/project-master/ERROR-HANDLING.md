# Error Handling - Gestion des Erreurs

## Objectif

Diagnostiquer et corriger les erreurs de manière systématique, avec enregistrement des patterns pour apprentissage futur.

## Patterns d'Erreurs Connus

### ERR-001 : ImportError - Module/Attribute Non Trouvé

**Symptôme** :
```
ImportError: cannot import name 'Employe' from 'database.models.effectifs'
```

**Causes Communes** :
1. Classe non définie dans le module
2. Typo dans le nom de la classe
3. Import circulaire

**Solutions** :
1. Vérifier que la classe existe dans le fichier
2. Vérifier l'orthographe exacte
3. Vérifier l'ordre des imports

**Commandes de Diagnostic** :
```bash
# Vérifier le contenu du module
grep -n "class Employe" database/models/effectifs.py

# Vérifier les imports du fichier
head -20 database/models/effectifs.py
```

---

### ERR-002 : AttributeError - datetime.timedelta

**Symptôme** :
```
AttributeError: type object 'datetime.datetime' has no attribute 'timedelta'
```

**Cause** :
Import incorrect : `from datetime import datetime` puis utilisation de `datetime.timedelta`

**Solution** :
```python
# ❌ Incorrect
from datetime import datetime
delta = datetime.timedelta(days=1)

# ✅ Correct
from datetime import datetime, timedelta
delta = timedelta(days=1)
```

---

### ERR-003 : NameError - Variable Non Définie

**Symptôme** :
```
NameError: name 'SessionLocal' is not defined
```

**Cause** :
Import manquant

**Solution** :
```python
# Ajouter l'import
from database.initialisation import SessionLocal
```

---

### ERR-004 : SyntaxError

**Symptôme** :
```
SyntaxError: invalid syntax at line X
```

**Causes Communes** :
1. Parenthèse/crochet non fermé
2. Indentation incorrecte
3. Virgule manquante

**Diagnostic** :
```bash
python -m py_compile fichier.py
```

---

### ERR-005 : Alembic Migration Failed

**Symptôme** :
```
alembic.util.exc.CommandError: Target database is not up to date
```

**Causes** :
1. Migration non appliquée
2. Conflit de versions

**Solutions** :
```bash
# Vérifier l'état actuel
alembic current

# Appliquer les migrations
alembic upgrade head

# Si conflit, créer une nouvelle migration
alembic revision --autogenerate -m "fix_migration_conflict"
```

---

## Processus de Correction

### 1. Diagnostic

**Étape 1 : Identifier le type d'erreur**
- ImportError → ERR-001
- AttributeError → ERR-002
- NameError → ERR-003
- SyntaxError → ERR-004
- Autre → Diagnostic manuel

**Étape 2 : Vérifier le pattern connu**
- Lire `.claude/context/error-patterns.md`
- Chercher le pattern ID correspondant
- Appliquer la solution documentée

**Étape 3 : Si pattern inconnu**
- Analyser le message d'erreur
- Identifier la cause racine
- Formuler une solution

### 2. Application de la Correction

**Tentative 1** :
```json
{
  "attempt": 1,
  "error": "ImportError: cannot import name 'Employe'",
  "diagnosis": "Classe non définie dans le module",
  "fix_to_apply": "Vérifier et ajouter la classe Employe dans database/models/effectifs.py"
}
```

Appliquer la correction, puis vérifier :
```bash
python -m py_compile fichier.py
```

**Tentative 2** (si échec) :
```json
{
  "attempt": 2,
  "error": "Même erreur",
  "diagnosis": "Import circulaire possible",
  "fix_to_apply": "Réorganiser les imports, déplacer import à la fin"
}
```

**Tentative 3** (si échec) :
```json
{
  "attempt": 3,
  "error": "Même erreur",
  "diagnosis": "Problème structurel",
  "fix_to_apply": "Refactoring de la structure du module"
}
```

### 3. Si Échec Définitif (3 tentatives)

```json
{
  "status": "error_unresolved",
  "error": {
    "type": "ImportError",
    "message": "cannot import name 'Employe' from 'database.models.effectifs'",
    "attempts": 3,
    "fixes_tried": [
      "Ajout de la classe Employe",
      "Réorganisation des imports",
      "Refactoring de la structure"
    ]
  },
  "action": "Enregistrer nouveau pattern, RETOURNER à Claude"
}
```

**Enregistrer le pattern** dans `.claude/context/error-patterns.md` :

```yaml
- id: ERR-XXX
  type: ImportError
  symptom: "cannot import name 'Employe' from 'database.models.effectifs'"
  context: "Création de nouveau module avec modèles SQLAlchemy"
  root_cause: "..."
  solution: "..."
  status: unresolved
  reported_date: 2025-11-04
```

## Commandes de Diagnostic Utiles

```bash
# Vérifier syntaxe Python
python -m py_compile fichier.py

# Lancer les tests
pytest tests/test_xxx.py -v

# Vérifier imports
python -c "from module import Class"

# Vérifier structure BDD
alembic current
alembic history

# Chercher un pattern
grep -r "pattern" .claude/context/error-patterns.md
```

## Format de Retour

### Correction Réussie

```json
{
  "status": "error_fixed",
  "error": {
    "type": "ImportError",
    "original_message": "..."
  },
  "fix_applied": "Ajout de 'from datetime import timedelta'",
  "pattern_id": "ERR-002",
  "attempt": 1,
  "tests_passed": true
}
```

### Échec Définitif

```json
{
  "status": "error_unresolved",
  "error": {
    "type": "ImportError",
    "message": "...",
    "attempts": 3,
    "pattern_id": "ERR-XXX (nouveau)",
    "registered": true
  },
  "action": "RETOURNER à Claude avec rapport complet"
}
```
