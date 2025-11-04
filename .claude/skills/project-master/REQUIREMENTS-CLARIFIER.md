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

## Format de Retour

Si clarification nécessaire :

```json
{
  "status": "needs_clarification",
  "reason": "Demande trop vague : 'Améliore le module de facturation'",
  "questions": [
    "Que veux-tu améliorer exactement ? (Performance / Fonctionnalités / UI / Code)",
    "Y a-t-il un problème spécifique à résoudre ?",
    "Quel est l'objectif principal ?",
    "Y a-t-il des contraintes ?"
  ]
}
```

Si clarification non nécessaire :

```json
{
  "status": "requirements_clear",
  "summary": "Créer module Effectifs complet avec CRUD, models, UI, tests"
}
```
