# Planning - Planification des Tâches

## Objectif

Créer plan d'exécution avec sous-tâches, estimations et dépendances.

---

## ✅ CHECKLIST

- [ ] 1. Décomposer en sous-tâches atomiques
- [ ] 2. Estimer durée de chaque sous-tâche
- [ ] 3. Identifier dépendances entre tâches
- [ ] 4. Identifier fichiers impactés par tâche
- [ ] 5. Calculer durée totale + marge 20%

---

## 📋 Template de Plan

```yaml
plan:
  task_name: "Nom de la tâche principale"
  estimated_total_duration: "8h"
  subtasks:
    - id: 1
      name: "Sous-tâche 1"
      duration: "1h30"
      dependencies: []
      files: ["fichier1.py", "fichier2.py"]

    - id: 2
      name: "Sous-tâche 2"
      duration: "45min"
      dependencies: [1]  # Dépend de sous-tâche 1
      files: ["fichier3.py"]
```

---

## ⏱️ Référence d'Estimation

| Type de Tâche | Durée Estimée |
|---------------|---------------|
| Model simple (1-2 classes) | 30-45min |
| Model complexe (3+ classes) | 1h-1h30 |
| Queries CRUD | 45min-1h |
| Migration BDD | 20-30min |
| Composant UI simple | 30-45min |
| Composant UI complexe | 1h-1h30 |
| Page complète | 1h-2h |
| Tests unitaires | 1h-1h30 |
| Documentation | 30-45min |

**Règles** :
- Ajouter 20% marge imprévus
- Doubler si techno inconnue
- Ajouter 30min pour 1ère occurrence d'un pattern

---

## 🔗 Dépendances

**Technique** : Tâche B nécessite résultat de Tâche A
```yaml
- id: 2
  dependencies: [1]  # Queries nécessitent models de tâche 1
```

**Logique** : Tâche B doit être après A pour cohérence
```yaml
- id: 7
  dependencies: [5, 6]  # Doc après page et tests
```

**Parallélisable** : Tâches sans dépendances communes
```yaml
- id: 3
  dependencies: [1]
- id: 6
  dependencies: [2]
# 3 et 6 peuvent être en parallèle
```

---

## ⚠️ AVANT DE PASSER À L'ÉTAPE 6

**Vérifie que tous les items de la CHECKLIST sont cochés** ✅

- Plan complet avec sous-tâches
- Durées estimées + marge 20%
- Dépendances identifiées
- Fichiers impactés listés par tâche
