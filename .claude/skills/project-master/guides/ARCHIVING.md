# Archiving - Archivage Post-Tâche

## Objectif

Mettre à jour TOUS les fichiers de contexte après CHAQUE tâche complétée. Cette étape est **OBLIGATOIRE** et **NON NÉGOCIABLE**.

## Fichiers à Mettre à Jour

### 1. tasks.md (OBLIGATOIRE)

**Emplacement** : `.claude/context/tasks.md`

**Actions** :
1. Ajouter la tâche dans la section "✅ Terminées"
2. Mettre à jour les statistiques
3. Retirer la tâche de "En cours" si présente

**Format** :
```yaml
## ✅ Terminées

### Création Module Effectifs
- **Date** : 2025-11-04
- **Durée** : 8h15min
- **Fichiers créés** : 12
- **Fichiers modifiés** : 3
- **Tests** : 18 tests (100% pass)
- **Description** : Module complet de gestion des employés avec CRUD, UI et tests

---

## 📊 Statistiques

- **Total tâches terminées** : 15 → 16
- **Temps total investi** : 42h → 50h15min
- **Modules créés** : 2 → 3 (Authentification, Budget, Effectifs)
```

---

### 2. error-patterns.md (SI ERREUR RENCONTRÉE)

**Emplacement** : `.claude/context/error-patterns.md`

**Quand mettre à jour** :
- Si erreur rencontrée pendant l'exécution
- Si nouveau pattern identifié

**Actions** :
1. Ajouter le pattern avec ID unique
2. Documenter la solution
3. Mettre à jour les statistiques

**Format** :
```yaml
- id: ERR-006
  type: ImportError
  symptom: "cannot import name 'Presence' from 'database.models.effectifs'"
  context: "Création module Effectifs"
  root_cause: "Classe Presence non définie dans le module"
  solution: "Ajouter la définition de la classe Presence dans database/models/effectifs.py"
  status: resolved
  attempts: 1
  reported_date: 2025-11-04
  resolved_date: 2025-11-04

---

## 📊 Statistiques

- **Total patterns** : 5 → 6
- **Résolus** : 4 → 5
- **Non résolus** : 1
```

---

### 3. system-state.md (OBLIGATOIRE)

**Emplacement** : `.claude/context/system-state.md`

**Actions** :
1. Mettre à jour l'état de l'application
2. Ajouter nouveaux modules/skills créés
3. Mettre à jour les métriques de performance

**Format** :
```yaml
## État de l'Application

### Modules Disponibles

- ✅ **Authentification** (Complété le 2025-10-28)
- ✅ **Budget** (Complété le 2025-11-03)
- ✅ **Effectifs** (Complété le 2025-11-04) ← NOUVEAU
- ⏸️ **Véhicules** (Non démarré)
- ⏸️ **Stock** (Non démarré)
- ⏸️ **Événements** (Non démarré)

### Technologies Utilisées

- NiceGUI 2.5.0
- SQLAlchemy 2.0
- Alembic 1.13
- Pytest 8.0
- Python 3.12

### Base de Données

- **Tables** : 8 → 11 (User, Categorie, Depense, Employe, Contrat, Presence) ← NOUVEAU
- **Migrations** : 2 → 3

---

## 📊 Métriques Performance

- **Pages totales** : 3 → 4
- **Composants UI** : 15 → 21
- **Tests unitaires** : 20 → 38
- **Couverture tests** : 85% → 87%
```

---

### 4. improvements-log.md (SI AMÉLIORATION)

**Emplacement** : `.claude/context/improvements-log.md`

**Quand mettre à jour** :
- Si amélioration significative apportée
- Si nouveau pattern/composant réutilisable créé

**Format** :
```yaml
## 2025-11-04 : Module Effectifs

### Amélioration
Création d'un module complet de gestion des employés avec CRUD, UI et tests.

### Impact
- ✅ Gestion centralisée des employés
- ✅ Suivi des contrats et présences
- ✅ KPI cards pour visualisation rapide
- ✅ Réutilisation du design system Budget

### Composants Réutilisables Créés
- `components/effectifs/form_employe.py` - Formulaire générique employé
- `components/effectifs/table_employes.py` - Tableau AG Grid employés
- `components/effectifs/kpi_cards.py` - KPI cards effectifs

### Métriques
- **Temps de développement** : 8h15min
- **Lignes de code** : ~850 lignes
- **Tests créés** : 18 tests
- **Réutilisabilité** : Élevée (design system)
```

---

### 5. decisions-log.md (SI DÉCISION TECHNIQUE)

**Emplacement** : `.claude/context/decisions-log.md`

**Quand mettre à jour** :
- Si décision technique importante prise
- Si choix d'architecture/technologie

**Format** :
```yaml
## 2025-11-04 : Structure du Module Effectifs

### Décision
Créer 3 models séparés (Employe, Contrat, Presence) plutôt qu'un seul model monolithique.

### Contexte
Besoin de gérer les employés, leurs contrats et leurs présences de manière flexible.

### Alternatives Considérées
1. ❌ Un seul model Employe avec tous les champs
2. ✅ 3 models séparés avec relations SQLAlchemy
3. ❌ 2 models (Employe + Contrat, présences en JSON)

### Justification
- Séparation des responsabilités
- Flexibilité pour ajouter des types de contrats
- Historique des présences facilement requêtable
- Relations SQLAlchemy claires

### Impact
- ✅ Code plus maintenable
- ✅ Requêtes BDD plus performantes
- ⚠️ Légèrement plus complexe (3 tables au lieu de 1)

### Résultat
Structure adoptée avec succès, tests passent, UI cohérente.
```

---

## Checklist d'Archivage

Avant de retourner le résultat final à Claude, **VÉRIFIER** :

- [ ] `tasks.md` mis à jour avec la tâche terminée
- [ ] `tasks.md` statistiques mises à jour
- [ ] `error-patterns.md` mis à jour (si erreur rencontrée)
- [ ] `system-state.md` mis à jour avec nouveaux modules
- [ ] `system-state.md` métriques mises à jour
- [ ] `improvements-log.md` mis à jour (si amélioration)
- [ ] `decisions-log.md` mis à jour (si décision technique)

**SI UN SEUL ITEM N'EST PAS COCHÉ → ARCHIVAGE INCOMPLET → À REFAIRE**

## Format de Retour

### Archivage Complet

```json
{
  "status": "archived",
  "files_updated": [
    ".claude/context/tasks.md",
    ".claude/context/system-state.md",
    ".claude/context/improvements-log.md"
  ],
  "archived_at": "2025-11-04T23:15:00",
  "summary": {
    "task_name": "Création Module Effectifs",
    "duration": "8h15min",
    "files_created": 12,
    "files_modified": 3,
    "tests_passed": true
  }
}
```

### Archivage Incomplet (ERREUR)

```json
{
  "status": "archiving_incomplete",
  "error": "error-patterns.md non mis à jour",
  "files_updated": [
    ".claude/context/tasks.md",
    ".claude/context/system-state.md"
  ],
  "files_missing": [
    ".claude/context/error-patterns.md"
  ],
  "action": "Mettre à jour les fichiers manquants AVANT de retourner"
}
```

## Commandes Utiles

```bash
# Vérifier que tous les fichiers existent
ls -la .claude/context/

# Ajouter une tâche à tasks.md
echo "..." >> .claude/context/tasks.md

# Vérifier la dernière ligne de tasks.md
tail -10 .claude/context/tasks.md

# Compter le nombre de tâches terminées
grep -c "^### " .claude/context/tasks.md
```

## Notes Importantes

- L'archivage est la **dernière étape obligatoire** du workflow
- **Aucune exception** : même les petites tâches doivent être archivées
- Si archivage oublié → Claude (interface) DOIT demander archivage forcé
- L'archivage permet l'apprentissage et l'amélioration continue du système
