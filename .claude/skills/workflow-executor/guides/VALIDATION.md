# Validation - Validation Utilisateur

## Objectif

Préparer rapport d'impact et RETOURNER à Claude pour demander validation explicite.

---

## 🎯 Quand Demander Validation ?

### OBLIGATOIRE si :

- Classification "CHANGEMENT MAJEUR" par impact-analyzer
- Durée estimée ≥ 4h
- Plus de 5 fichiers impactés
- Nouveau module complet
- Risque CRITIQUE ou ÉLEVÉ

### OPTIONNEL si :

- Classification "CHANGEMENT MODÉRÉ"
- Durée 2-4h
- 3-5 fichiers

### PAS NÉCESSAIRE si :

- Classification "CHANGEMENT MINEUR"
- Durée < 2h
- 1-2 fichiers
- Risque FAIBLE

---

## 📤 FORMAT DE RETOUR

### Si validation nécessaire

```markdown
✋ **Validation requise**

📊 **Impact de la modification** :

**Complexité** : [SIMPLE|MOYENNE|MAJEURE] ([estimation heures])

**Fichiers impactés** : [nombre] fichiers
- [catégorie 1] ([nombre] fichiers - [nouveaux/modifiés])
- [catégorie 2] ([nombre] fichiers - [nouveaux/modifiés])

**Risques identifiés** :
- [NIVEAU] : [Description du risque]
  → Mitigation : [Comment on réduit ce risque]
- [NIVEAU] : [Description du risque]
  → Mitigation : [Comment on réduit ce risque]

**Bénéfices** :
- [Bénéfice 1]
- [Bénéfice 2]
- [Bénéfice 3]

**Plan proposé** :
1. [Étape 1 claire]
2. [Étape 2 claire]
3. [Étape 3 claire]

**Alternatives** (optionnel) :
- Option 1 : [Description avec durée]
- Option 2 : [Description avec durée]

❓ **Souhaitez-vous procéder ?**
- Répondez "Oui vas-y" pour confirmer
- Ou précisez modifications : "Oui mais fais X au lieu de Y"
- Ou "Non" pour annuler

---
**Demande initiale** : [répéter exactement la demande utilisateur]
```

### Si validation non nécessaire

→ Passer directement à ÉTAPE 5 en silence

---

## 📊 Grille de Décision

| Critère | SIMPLE | MOYENNE | MAJEURE |
|---------|--------|---------|---------|
| Durée | < 2h | 2-4h | > 4h |
| Fichiers | 1-2 | 3-5 | > 5 |
| Risque max | FAIBLE | MODÉRÉ | ÉLEVÉ/CRITIQUE |
| Migration BD | Non | Non | Oui |
| Breaking changes | Non | Non | Possible |
| **Validation** | ❌ Non | ⚠️ Optionnel | ✅ **OBLIGATOIRE** |

---

## ⚠️ RÈGLES

### ✅ TOUJOURS

- ✅ Utiliser marqueur **✋ Validation requise**
- ✅ Être **transparent sur les risques** (ne pas cacher)
- ✅ Fournir **plan détaillé** avec étapes claires
- ✅ Proposer **alternatives** si pertinent
- ✅ Inclure **mitigations** pour chaque risque
- ✅ Répéter **demande initiale** à la fin

### ❌ JAMAIS

- ❌ Minimiser les risques pour "rassurer"
- ❌ Proposer validation pour changements simples (< 2h)
- ❌ Oublier les bénéfices (focus équilibré risques/bénéfices)
- ❌ Oublier de répéter la demande initiale
