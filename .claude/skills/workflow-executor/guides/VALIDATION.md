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

## Rapport de Validation

### Structure du Rapport

```json
{
  "status": "needs_validation",
  "validation_report": {
    "task_name": "Création Module Effectifs Complet",
    "classification": "CHANGEMENT MAJEUR",
    "impact_summary": {
      "estimated_time": "8-10h",
      "files_affected": 15,
      "files_new": 12,
      "files_modified": 3,
      "modules_impacted": ["Database", "Components", "Pages", "Tests"]
    },
    "risks": [
      {
        "level": "ÉLEVÉ",
        "description": "Migration BDD avec 3 nouvelles tables",
        "mitigation": "Tests avant/après migration + backup BDD"
      },
      {
        "level": "MODÉRÉ",
        "description": "Ajout de 12 nouveaux fichiers",
        "mitigation": "Respect du design system existant"
      }
    ],
    "benefits": [
      "Module complet de gestion des employés",
      "CRUD complet avec validation",
      "Interface UI cohérente avec Budget",
      "Tests unitaires inclus"
    ],
    "alternatives": [
      "Option 1 : Module complet en une fois (8-10h)",
      "Option 2 : Module minimal puis itérations (4h + 2h + 2h)"
    ]
  }
}
```

## Format d'Affichage pour Claude

Claude doit afficher à l'utilisateur :

```
⚠️ CHANGEMENT MAJEUR DÉTECTÉ

Cette action aura un impact important sur le projet.

📋 Tâche : Création Module Effectifs Complet
⏱️  Durée estimée : 8-10h
📂 Fichiers : 15 fichiers (12 nouveaux, 3 modifiés)
🏗️ Modules : Database, Components, Pages, Tests

⚠️ Risques identifiés :
• ÉLEVÉ : Migration BDD avec 3 nouvelles tables
  → Mitigation : Tests + backup BDD
• MODÉRÉ : Ajout de 12 nouveaux fichiers
  → Mitigation : Respect du design system

✨ Bénéfices :
• Module complet de gestion des employés
• CRUD complet avec validation
• Interface UI cohérente avec Budget
• Tests unitaires inclus

🎯 Alternatives :
1. Module complet en une fois (8-10h) - Recommandé
2. Module minimal puis itérations (4h + 2h + 2h)

Es-tu sûr de vouloir continuer ?

Options :
1. ✅ Oui, continuer
2. ❌ Non, annuler
3. 📝 Voir plus de détails
```

## Gestion de la Réponse

### Si "Oui, continuer"
```json
{
  "validation_received": true,
  "user_choice": "continue",
  "action": "Continuer vers PLANNING.md"
}
```

### Si "Non, annuler"
```json
{
  "validation_received": true,
  "user_choice": "cancel",
  "action": "Arrêter le workflow, ne rien modifier"
}
```

### Si "Voir plus de détails"
```json
{
  "validation_received": false,
  "user_choice": "details",
  "action": "Fournir détails supplémentaires (liste fichiers, architecture, etc.)"
}
```

## Notes Importantes

- La validation DOIT passer par Claude (interface)
- Ne JAMAIS continuer sans validation si changement majeur
- Fournir TOUTES les informations nécessaires pour décision éclairée
- Proposer des alternatives si possible
