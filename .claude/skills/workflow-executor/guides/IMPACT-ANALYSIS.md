# Impact Analysis - Analyse d'Impact

## Objectif

Évaluer complexité, impact et risques d'une tâche AVANT de commencer.

---

## ✅ CHECKLIST

- [ ] 1. Estimer durée (Complexité Temporelle)
- [ ] 2. Compter fichiers impactés (Portée)
- [ ] 3. Identifier modules touchés
- [ ] 4. Évaluer risques (CRITIQUE/ÉLEVÉ/MODÉRÉ/FAIBLE)
- [ ] 5. Classifier (MINEUR/MODÉRÉ/MAJEUR)
- [ ] 6. Décider si validation nécessaire

---

## 📊 Grilles d'Évaluation

### 1. Complexité Temporelle

| Durée Estimée | Classification |
|---------------|----------------|
| < 30 min | TRIVIALE |
| 30min - 2h | SIMPLE |
| 2h - 8h | COMPLEXE |
| > 8h | MAJEURE |

### 2. Portée des Changements

| Fichiers | Impact |
|----------|--------|
| 1-2 | MINEUR |
| 3-5 | MODÉRÉ |
| 6-10 | IMPORTANT |
| > 10 | MAJEUR |

### 3. Modules Impactés

| Modules Touchés | Impact |
|-----------------|--------|
| 1 module existant | MINEUR |
| 2-3 modules existants | MODÉRÉ |
| Nouveau module complet | MAJEUR |
| Refactoring multi-modules | MAJEUR |

### 4. Risques

**CRITIQUE** (au moins un) :
- Migration base de données
- Breaking changes API
- Modification authentification
- Refactoring core

**ÉLEVÉ** (au moins un) :
- Modification modèles existants
- Changement structure fichiers
- Ajout dépendances externes

**MODÉRÉ** :
- Nouvelle fonctionnalité isolée
- Modification composants UI existants

**FAIBLE** :
- Fonction simple
- Style/CSS
- Bug ponctuel

---

## 🎯 Classification Finale

### CHANGEMENT MINEUR
- Durée < 2h
- 1-2 fichiers
- 1 module
- Risque FAIBLE

→ **PAS de validation nécessaire**

### CHANGEMENT MODÉRÉ
- Durée 2-4h
- 3-5 fichiers
- 2 modules
- Risque MODÉRÉ

→ **Validation optionnelle**

### CHANGEMENT MAJEUR
- Durée ≥ 4h
- > 5 fichiers
- Nouveau module ou refactoring
- Risque ÉLEVÉ/CRITIQUE

→ **VALIDATION OBLIGATOIRE** (lire VALIDATION.md)

---

## ⚠️ AVANT DE PASSER À L'ÉTAPE 3

**Vérifie que tous les items de la CHECKLIST sont cochés** ✅

- Classification claire (MINEUR/MODÉRÉ/MAJEUR)
- Risques identifiés
- Décision validation prise
