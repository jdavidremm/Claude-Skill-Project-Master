# Decisions Log - Journal des Décisions

## 2025-11-04 : Architecture du Skill project-master

### Décision
Utiliser une architecture modulaire avec progressive disclosure plutôt qu'un seul fichier monolithique.

### Contexte
Besoin de créer un skill qui gère le cycle complet des tâches de développement sans surcharger le contexte.

### Alternatives Considérées
1. ❌ Un seul SKILL.md avec toutes les instructions (risque de contexte surchargé)
2. ✅ Architecture modulaire avec 9 fichiers spécialisés + progressive disclosure
3. ❌ Sous-skills séparés pour chaque étape (trop complexe à coordonner)

### Justification
- Progressive disclosure : chargement on-demand des fichiers selon les besoins
- Séparation des responsabilités : chaque fichier a un rôle précis
- Maintenabilité : facile d'améliorer ou d'ajouter des patterns
- Workflow séquentiel clair : les 7 étapes obligatoires sont explicites

### Impact
- ✅ Utilisation optimale du contexte
- ✅ Workflow séquentiel garanti
- ✅ Facile à étendre et maintenir
- ⚠️ Légèrement plus de fichiers à gérer (9 fichiers de guidance)

### Résultat
Architecture adoptée avec succès, structure claire et extensible.

---

## 2025-11-04 : Format de Communication JSON

### Décision
Utiliser des structures JSON standardisées pour la communication entre le skill et Claude (interface).

### Contexte
Besoin d'un format de communication clair et structuré pour les retours du skill.

### Alternatives Considérées
1. ❌ Texte libre (ambiguïté, difficile à parser)
2. ✅ JSON structuré avec schemas définis
3. ❌ YAML (moins adapté pour communication programmatique)

### Justification
- Format standard et universel
- Facile à parser et valider
- Permet de structurer clairement les données
- Compatible avec tous les outils

### Impact
- ✅ Communication claire et sans ambiguïté
- ✅ Facile à étendre avec de nouveaux champs
- ✅ Permet validation automatique des données

### Résultat
Format JSON adopté pour tous les retours du skill.
