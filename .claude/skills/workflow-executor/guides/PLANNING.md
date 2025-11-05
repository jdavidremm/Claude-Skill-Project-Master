# Planning - Planification des Tâches

## Objectif

Créer plan d'exécution avec sous-tâches, estimations et dépendances.

---

## ✅ CHECKLIST

- [ ] 1. Décomposer en sous-tâches atomiques
- [ ] 2. Estimer durée de chaque sous-tâche
- [ ] 3. Identifier dépendances entre tâches
- [ ] 4. Identifier fichiers impactés par tâche
- [ ] 5. **Identifier nouveaux dossiers créés** (pour tracking archivage)
- [ ] 6. Calculer durée totale + marge 20%

---

## 📋 Template de Plan

```yaml
plan:
  task_name: "Nom de la tâche principale"
  estimated_total_duration: "8h"
  new_folders: ["/models", "/api", "/ui"]  # Dossiers créés par workflow
  subtasks:
    - id: 1
      name: "Sous-tâche 1"
      duration: "1h30"
      dependencies: []
      files: ["fichier1.py", "fichier2.py"]
      creates_folder: "/models"  # Si cette tâche crée un dossier

    - id: 2
      name: "Sous-tâche 2"
      duration: "45min"
      dependencies: [1]  # Dépend de sous-tâche 1
      files: ["fichier3.py"]
      creates_folder: null
```

**Note** : Les dossiers listés dans `new_folders` seront automatiquement enrichis dans `project-registry.json` pendant ÉTAPE 7 (Archivage).

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

---

## ❌ ANTI-PATTERNS (NE PAS FAIRE)

### ❌ Anti-pattern #1 : "Plan trop vague"
**Symptôme** : Sous-tâche "Créer backend", "Faire frontend"
**Conséquence** : Sous-tâche non exécutable, ambiguë
**Solution** : Sous-tâches ATOMIQUES (1 fichier, 1 action précise)

### ❌ Anti-pattern #2 : "Oublier durées"
**Symptôme** : Plan sans estimations temporelles
**Conséquence** : Impossible de suivre progression, risque dépassement
**Solution** : TOUJOURS estimer + marge 20%

### ❌ Anti-pattern #3 : "Ignorer dépendances"
**Symptôme** : Toutes sous-tâches marquées "parallèle"
**Conséquence** : Erreurs exécution (fichier inexistant, import manquant)
**Solution** : Identifier dépendances techniques ET logiques

### ❌ Anti-pattern #4 : "Oublier fichiers impactés"
**Symptôme** : Sous-tâche sans liste fichiers
**Conséquence** : Impossible de MAJ registres correctement
**Solution** : Lister TOUS fichiers créés/modifiés par sous-tâche

### ❌ Anti-pattern #5 : "Plan sans marge"
**Symptôme** : Estimer 2h pile sans buffer
**Conséquence** : Dépassement systématique
**Solution** : +20% marge (2h → 2h24min)
