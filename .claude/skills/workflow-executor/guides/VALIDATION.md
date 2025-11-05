# Validation - Validation Utilisateur

## Objectif

Préparer un rapport d'impact complet et RETOURNER à Claude pour qu'il demande validation explicite à l'utilisateur.

## Quand Demander Validation ?

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

## 📤 Format de Retour pour Validation

⚠️ **TOUJOURS utiliser ce format avec le marqueur ✋**

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

Ne rien retourner, passer directement à ÉTAPE 5 en silence.

## 📋 Exemples par Niveau de Complexité

### Exemple SIMPLE (< 2h)

Pas de validation nécessaire → Passer directement à ÉTAPE 5

### Exemple MOYENNE (2-4h)

Validation optionnelle. Si demandée :

```markdown
✋ **Validation requise**

📊 **Impact de la modification** :

**Complexité** : MOYENNE (3h)

**Fichiers impactés** : 5 fichiers
- Components (2 fichiers - nouveaux)
- Utils (1 fichier - modifié)
- Tests (2 fichiers - nouveaux)

**Risques identifiés** :
- FAIBLE : Ajout de dépendances (react-query)
  → Mitigation : Dépendance bien maintenue et documentée

**Bénéfices** :
- Cache automatique des requêtes API
- Gestion des états loading/error simplifiée
- Performance améliorée

**Plan proposé** :
1. Installer react-query
2. Créer hook useEffectifs
3. Refactorer composant EffectifsList
4. Ajouter tests

❓ **Souhaitez-vous procéder ?**
- Répondez "Oui vas-y" pour confirmer
- Ou précisez modifications : "Oui mais fais X au lieu de Y"

---
**Demande initiale** : Ajoute react-query pour gérer les effectifs
```

### Exemple MAJEURE (> 4h)

Validation OBLIGATOIRE :

```markdown
✋ **Validation requise**

📊 **Impact de la modification** :

**Complexité** : MAJEURE (8-10h)

**Fichiers impactés** : 15 fichiers
- Database (3 fichiers - nouveaux : migrations, models)
- Backend API (4 fichiers - nouveaux : routes, controllers)
- Components (5 fichiers - nouveaux : CRUD UI)
- Pages (1 fichier - nouveau : page principale)
- Tests (2 fichiers - nouveaux : unit + integration)

**Risques identifiés** :
- ÉLEVÉ : Migration base de données avec 3 nouvelles tables
  → Mitigation : Backup BDD + tests migration + rollback plan
- MODÉRÉ : Ajout de 12 nouveaux fichiers (risque cohérence)
  → Mitigation : Respect strict du design system existant
- FAIBLE : Nouvelles dépendances (zod pour validation)
  → Mitigation : Dépendances standards du projet

**Bénéfices** :
- Module complet de gestion des employés
- CRUD complet avec validation robuste (zod)
- Interface UI cohérente avec module Budget existant
- Tests unitaires et intégration inclus
- Réutilisable pour futurs modules similaires

**Plan proposé** :
1. Phase 1 : Database (migrations + models) - 2h
2. Phase 2 : Backend API (routes + controllers + validation) - 3h
3. Phase 3 : Frontend UI (composants CRUD) - 3h
4. Phase 4 : Tests + intégration - 2h

**Alternatives** :
- Option 1 : Module complet en une fois (8-10h) - Recommandé pour cohérence
- Option 2 : Approche incrémentale (Phase 1→2→3→4 avec validations intermédiaires) - Plus sécurisé mais plus long

❓ **Souhaitez-vous procéder ?**
- Répondez "Oui vas-y" pour confirmer
- Ou précisez modifications : "Oui mais commence par X seulement"
- Ou "Non" pour annuler

---
**Demande initiale** : Créé module Effectifs complet avec CRUD
```

### Exemple avec Breaking Changes

```markdown
✋ **Validation requise**

📊 **Impact de la modification** :

**Complexité** : MAJEURE (12-15h)

**Fichiers impactés** : 45 fichiers
- Components React (25 fichiers - modifiés)
- Configuration (5 fichiers - modifiés)
- Tests (15 fichiers - modifiés)

**Risques identifiés** :
- CRITIQUE : Breaking changes React 19 (Concurrent rendering, Suspense changes)
  → Mitigation : Tests complets avant/après + migration progressive par module
- ÉLEVÉ : Compatibilité librairies tierces (react-router, redux)
  → Mitigation : Vérifier compatibilité + MAJ si nécessaire
- MODÉRÉ : Performance temporairement dégradée pendant migration
  → Mitigation : Migration module par module pour limiter impact

**Bénéfices** :
- Performance améliorée (Concurrent rendering)
- Nouvelles fonctionnalités React 19 (Actions, useOptimistic)
- Support long-terme assuré
- Corrections de bugs React 18

**Plan proposé** :
1. Backup complet + nouvelle branche - 1h
2. MAJ dépendances (React, React-DOM, librairies) - 2h
3. Migration composants module par module (5 modules) - 6h
4. Adaptation tests - 2h
5. Tests régression complets - 2h

**Alternatives** :
- Option 1 : Migration complète immédiate (12-15h) - Rapide mais risqué
- Option 2 : Migration module par module avec validations (15-20h) - Plus long mais sécurisé
- Option 3 : Rester sur React 18 pour l'instant - Pas de risque mais pas de bénéfices

❓ **Souhaitez-vous procéder ?**
- Répondez "Oui vas-y" pour confirmer (indiquez quelle option)
- Ou précisez modifications : "Oui mais option 2 seulement"
- Ou "Non, restons sur React 18"

---
**Demande initiale** : Migre vers React 19
```

## ⚠️ Règles Critiques

### ✅ TOUJOURS
- ✅ Utiliser le marqueur **✋ Validation requise**
- ✅ Être **transparent sur les risques** (ne pas cacher)
- ✅ Fournir **plan détaillé** avec étapes claires
- ✅ Proposer **alternatives** si pertinent
- ✅ Inclure **mitigations** pour chaque risque
- ✅ Répéter la **demande initiale** à la fin

### ❌ JAMAIS
- ❌ Retourner du JSON brut (utiliser markdown lisible)
- ❌ Minimiser les risques pour "rassurer"
- ❌ Proposer validation pour changements simples (< 2h)
- ❌ Oublier les bénéfices (focus équilibré risques/bénéfices)
- ❌ Oublier de répéter la demande initiale

## 📊 Grille de Décision Validation

| Critère | SIMPLE | MOYENNE | MAJEURE |
|---------|--------|---------|---------|
| Durée | < 2h | 2-4h | > 4h |
| Fichiers | 1-2 | 3-5 | > 5 |
| Risque max | FAIBLE | MODÉRÉ | ÉLEVÉ/CRITIQUE |
| Migration BD | Non | Non | Oui |
| Breaking changes | Non | Non | Possible |
| **Validation** | ❌ Non | ⚠️ Optionnel | ✅ **OBLIGATOIRE** |
