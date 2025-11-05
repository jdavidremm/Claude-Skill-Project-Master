# Requirements Clarifier - Clarification des Exigences

## Objectif

Identifier les ambiguïtés dans la demande utilisateur et poser des questions claires.

---

## ✅ CHECKLIST

- [ ] 1. Objectif clair ? (résultat attendu, critères succès)
- [ ] 2. Portée définie ? (fichiers/modules concernés, fonctionnalités exactes)
- [ ] 3. Approche technique ? (bibliothèque, patterns à respecter)
- [ ] 4. Priorités ? (performance vs lisibilité vs rapidité)

---

## 🔍 Déclencheurs d'Ambiguïté

### Demandes Vagues (→ CLARIFICATION)

❌ "Améliore le module X"
❌ "Optimise les performances"
❌ "Rends le code meilleur"
❌ "Ajoute des fonctionnalités"

### Demandes Précises (→ PAS de clarification)

✅ "Créer module Effectifs avec CRUD complet"
✅ "Corriger bug d'import dans kpi_cards.py ligne 45"
✅ "Ajouter dark mode avec binding sur app.storage.user"

---

## 📤 FORMAT DE RETOUR

### Si clarification nécessaire

```markdown
🔄 **Clarifications nécessaires**

❓ **Questions** :

1. **[Catégorie]** : [Question claire et précise ?]
   - Option A : [description avec contexte]
   - Option B : [description avec contexte]
   - Option C : [description avec contexte]

2. **[Catégorie]** : [Question claire et précise ?]
   - Option A : [description avec contexte]
   - Option B : [description avec contexte]

---
**Demande initiale** : [répéter exactement la demande utilisateur]
```

### Si clarification non nécessaire

→ Passer directement à ÉTAPE 4 en silence

---

## ⚠️ RÈGLES

### ✅ TOUJOURS

- ✅ Utiliser marqueur **🔄 Clarifications nécessaires**
- ✅ Poser questions **claires et précises**
- ✅ Fournir **options avec contexte** (pourquoi A vs B ?)
- ✅ Répéter **demande initiale** à la fin
- ✅ Limiter à **3-5 questions max**

### ❌ JAMAIS

- ❌ Questions vagues ("Que veux-tu faire ?")
- ❌ Trop d'options (max 4-5 par question)
- ❌ Oublier le contexte des options
- ❌ Oublier de répéter la demande initiale
