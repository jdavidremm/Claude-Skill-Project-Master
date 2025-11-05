# Requirements Clarifier - Clarification des Exigences

## Objectif

Identifier les ambiguïtés dans la demande utilisateur et poser les bonnes questions pour clarifier.

## Déclencheurs d'Ambiguïté

### Demandes Vagues

❌ "Améliore le module X"
❌ "Optimise les performances"
❌ "Rends le code meilleur"
❌ "Ajoute des fonctionnalités"

**→ CLARIFICATION NÉCESSAIRE**

### Demandes Précises

✅ "Créer le module Effectifs avec CRUD complet"
✅ "Corriger le bug d'import dans kpi_cards.py ligne 45"
✅ "Ajouter dark mode avec binding sur app.storage.user"

**→ PAS de clarification nécessaire**

## Checklist de Clarification

### 1. Objectif Clair ?

- [ ] Quel est le résultat attendu exactement ?
- [ ] Quels sont les critères de succès ?
- [ ] Y a-t-il des contraintes spécifiques ?

### 2. Portée Définie ?

- [ ] Quels fichiers/modules sont concernés ?
- [ ] Quelles fonctionnalités exactes doivent être ajoutées ?
- [ ] Y a-t-il des fonctionnalités à NE PAS toucher ?

### 3. Approche Technique ?

- [ ] Y a-t-il une approche technique préférée ?
- [ ] Faut-il utiliser une bibliothèque spécifique ?
- [ ] Y a-t-il des patterns à respecter ?

### 4. Priorités ?

- [ ] Quelle est la priorité (performance vs lisibilité vs rapidité) ?
- [ ] Y a-t-il un ordre spécifique pour les sous-tâches ?

## Templates de Questions

### Demande : "Améliore le module de facturation"

**Questions à poser** :

```
❓ J'ai besoin de précisions pour t'aider au mieux.

1️⃣ Que veux-tu améliorer exactement ?
   A. Performance (vitesse d'exécution)
   B. Fonctionnalités (nouvelles features)
   C. Interface utilisateur (UX/UI)
   D. Code (refactoring, maintenabilité)

2️⃣ Y a-t-il un problème spécifique à résoudre ?

3️⃣ Quel est l'objectif principal de cette amélioration ?

4️⃣ Y a-t-il des contraintes (temps, compatibilité, etc.) ?
```

### Demande : "Optimise les performances"

**Questions à poser** :

```
❓ Pour optimiser efficacement, j'ai besoin de détails :

1️⃣ Quelle partie de l'application est lente ?
   A. Chargement initial de la page
   B. Requêtes de base de données
   C. Calculs / traitement de données
   D. Rendu des composants UI

2️⃣ As-tu des métriques actuelles (temps de chargement, etc.) ?

3️⃣ Quel est l'objectif de performance visé ?

4️⃣ Y a-t-il des fonctionnalités prioritaires à optimiser ?
```

## 📤 Format de Retour des Questions

⚠️ **TOUJOURS utiliser ce format avec le marqueur 🔄**

### Si clarification nécessaire

```markdown
🔄 **Clarifications nécessaires**

❓ **Questions** :

1. **[Catégorie]** : [Question claire et précise ?]
   - Option A : [description avec contexte]
   - Option B : [description avec contexte]
   - Option C : [description avec contexte]
   [- Option D : optionnel]

2. **[Catégorie]** : [Question claire et précise ?]
   - Option A : [description avec contexte]
   - Option B : [description avec contexte]

[Ajouter autant de questions que nécessaire]

---
**Demande initiale** : [répéter exactement la demande utilisateur]
```

### Si clarification non nécessaire

Ne rien retourner, passer directement à ÉTAPE 4 en silence.

## 📋 Exemples de Questions par Type

### Choix technologique

```markdown
1. **Base de données** : Quel type de base de données souhaitez-vous utiliser ?
   - SQLite : Local, pas de serveur, simple (idéal pour prototypes/petits projets)
   - PostgreSQL : Robuste, production-ready, relations complexes
   - MongoDB : NoSQL, flexible, bon pour données non structurées
   - MySQL : Classique, bien documenté, largement supporté
```

### Architecture

```markdown
2. **Architecture** : Quelle structure d'application préférez-vous ?
   - Monolithe : Tout dans un seul projet (simple, démarrage rapide)
   - Microservices : Services séparés (scalable, mais plus complexe)
   - Serverless : Functions cloud (pay-as-you-go, maintenance minimale)
```

### Authentification

```markdown
3. **Authentification** : Les données sont-elles par utilisateur ?
   - Oui : Chaque utilisateur a ses propres données (besoin système d'auth)
   - Non : Données globales partagées par tous
```

### Fonctionnalités ambiguës

```markdown
4. **Amélioration du module** : Que voulez-vous améliorer exactement ?
   - Performance : Optimiser vitesse d'exécution et requêtes
   - Fonctionnalités : Ajouter de nouvelles features
   - Interface : Améliorer UX/UI
   - Code : Refactoring pour maintenabilité
```

### Portée du travail

```markdown
5. **Portée** : Quel niveau de changement envisagez-vous ?
   - Minimal : Correction/ajout simple sans impact architectural
   - Modéré : Modification avec quelques fichiers impactés
   - Complet : Refonte majeure du module
```

## 🎯 Exemple Complet

### Demande : "Créé une app todo avec BDD"

**Retour du skill** :

```markdown
🔄 **Clarifications nécessaires**

❓ **Questions** :

1. **Base de données** : Quel type de base de données souhaitez-vous utiliser ?
   - SQLite : Local, fichier unique, parfait pour débuter et petits projets
   - PostgreSQL : Production-ready, robuste, supporte relations complexes
   - MongoDB : NoSQL, flexible, idéal pour structure de données évolutive

2. **Authentification** : Les todos sont-ils par utilisateur ?
   - Oui : Chaque utilisateur a ses propres todos (nécessite système d'auth complet)
   - Non : Todos globaux partagés par tous les utilisateurs

3. **Interface** : Quel type d'interface souhaitez-vous ?
   - Web (React/Vue) : Application web moderne dans le navigateur
   - Desktop (NiceGUI/Electron) : Application bureau native
   - CLI : Interface en ligne de commande
   - API uniquement : Pas d'interface, juste l'API backend

---
**Demande initiale** : Créé une app todo avec BDD
```

### Demande : "Optimise les performances"

**Retour du skill** :

```markdown
🔄 **Clarifications nécessaires**

❓ **Questions** :

1. **Quelle partie est lente** : Où constatez-vous des problèmes de performance ?
   - Chargement initial : Page met du temps à s'afficher au démarrage
   - Requêtes BD : Opérations de lecture/écriture lentes
   - Calculs : Traitement de données ou algorithmes lents
   - Rendu UI : Interface qui lag lors des interactions

2. **Objectif** : Quel niveau d'optimisation visez-vous ?
   - Rapide : Gains "quick wins" faciles à implémenter (< 2h)
   - Complet : Optimisation profonde avec refactoring si nécessaire (> 4h)

3. **Métriques** : Avez-vous des métriques actuelles ?
   - Oui : [Précisez temps de chargement, requêtes/sec, etc.]
   - Non : Je constate juste que c'est lent

---
**Demande initiale** : Optimise les performances
```

## ⚠️ Règles Critiques

### ✅ TOUJOURS
- ✅ Utiliser le marqueur **🔄 Clarifications nécessaires**
- ✅ Poser des questions **claires et précises**
- ✅ Fournir **options avec contexte** (pourquoi choisir A vs B ?)
- ✅ Répéter la **demande initiale** à la fin
- ✅ Limiter à **3-5 questions max** (éviter overload)

### ❌ JAMAIS
- ❌ Retourner du JSON brut (utiliser markdown lisible)
- ❌ Poser des questions vagues ("Que veux-tu faire ?")
- ❌ Donner trop d'options (max 4-5 par question)
- ❌ Oublier le contexte des options
- ❌ Oublier de répéter la demande initiale
