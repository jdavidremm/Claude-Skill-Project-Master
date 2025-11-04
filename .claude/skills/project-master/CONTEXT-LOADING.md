# Context Loading - Chargement de l'État du Projet

## Objectif

Charger l'état actuel du projet pour comprendre où on en est avant de commencer une nouvelle tâche.

## Fichiers à Charger

### Obligatoires

1. **context/tasks.md**
   - Liste des tâches terminées, en cours, en attente
   - Dernière tâche effectuée
   - Statistiques globales

2. **context/system-state.md**
   - État actuel de l'application
   - Modules disponibles
   - Skills/agents créés
   - Technologies utilisées

3. **context/error-patterns.md**
   - Patterns d'erreurs connus
   - Solutions documentées
   - Erreurs récurrentes à éviter

### Optionnels (selon besoin)

4. **context/improvements-log.md**
   - Améliorations récentes
   - Impact des changements

5. **context/decisions-log.md**
   - Décisions techniques passées
   - Justifications

6. **context/design-system.md**
   - Conventions de design
   - Composants disponibles

## Détection d'Interruption

Vérifie dans `tasks.md` si une tâche est marquée comme "⏸️ En cours" :

```yaml
status: in_progress
started_at: 2025-11-04T14:30:00
subtasks:
  - name: "Créer models"
    status: completed
  - name: "Créer UI"
    status: in_progress  # ← INTERRUPTION DÉTECTÉE
  - name: "Tests"
    status: pending
```

Si interruption détectée :
- Charger les détails de la tâche interrompue
- Identifier ce qui a été complété
- Identifier ce qui reste à faire
- RETOURNER à Claude pour proposer reprise à l'utilisateur

## Format de Retour

```json
{
  "context_loaded": true,
  "interruption_detected": false,
  "project_state": {
    "last_task": "Création Module Budget",
    "last_task_date": "2025-11-04",
    "modules": {
      "authentification": "completed",
      "budget": "completed",
      "effectifs": "not_started",
      "vehicules": "not_started"
    },
    "skills_available": ["nicegui-doc"],
    "recent_errors": []
  }
}
```

Si interruption :

```json
{
  "context_loaded": true,
  "interruption_detected": true,
  "interrupted_task": {
    "name": "Création Module Stock",
    "started_at": "2025-11-04T14:30:00",
    "progress": "2/4 sous-tâches (50%)",
    "completed": ["Créer models", "Créer queries"],
    "in_progress": "Créer UI (fichier créé mais pas validé)",
    "pending": ["Tests"]
  }
}
```

## Commandes Bash Utiles

```bash
# Lister tous les fichiers context/
ls .claude/context/

# Lire tasks.md
cat .claude/context/tasks.md

# Chercher tâches en cours
grep -A 10 "status: in_progress" .claude/context/tasks.md
```
