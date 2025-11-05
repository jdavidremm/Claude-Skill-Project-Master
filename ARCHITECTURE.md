# 🏗️ Architecture Finale : project-master (Agent)

## 📁 Structure Actuelle

```
.claude/
├── claude.md                                # Interface utilisateur (Claude)
├── agents/
│   └── project-master.md                   # 🎯 Agent principal (nouveau)
├── skills/
│   └── project-master/                     # 📚 Ressources partagées
│       ├── guides/                         # ✅ 8 guides de workflow
│       │   ├── CONTEXT-LOADING.md
│       │   ├── IMPACT-ANALYSIS.md
│       │   ├── REQUIREMENTS-CLARIFIER.md
│       │   ├── VALIDATION.md
│       │   ├── PLANNING.md
│       │   ├── EXECUTION.md
│       │   ├── ERROR-HANDLING.md
│       │   └── ARCHIVING.md
│       └── capabilities/                   # ✅ Système d'auto-apprentissage
│           ├── _registry.json              # Registre des capacités
│           ├── README.md                   # Documentation
│           ├── frameworks/                 # React, Vue, NiceGUI, etc.
│           ├── libraries/                  # Stripe, Supabase, etc.
│           ├── patterns/                   # Patterns de code
│           ├── architectures/              # Clean Architecture, etc.
│           ├── tools/                      # Git, Docker, etc.
│           ├── languages/                  # Python, TypeScript, etc.
│           └── project-guidelines/         # Conventions du projet
└── context/                                # 📊 État du projet
    ├── tasks.md
    ├── system-state.md
    ├── error-patterns.md
    ├── decisions-log.md
    └── improvements-log.md
```

## 🎯 Rôles et Responsabilités

### `.claude/claude.md` - Interface Utilisateur
**Rôle** : Interface entre utilisateur et agent
**Responsabilités** :
- Détecter et extraire documentation (liens, fichiers, règles)
- Déléguer toute demande de développement à l'agent
- Afficher les résultats retournés par l'agent

**Ce qu'elle NE fait PAS** :
- ❌ Coder directement
- ❌ Accéder aux fichiers de contexte
- ❌ Transformer du JSON

### `.claude/agents/project-master.md` - Agent Autonome
**Rôle** : Chef de projet exécutant le workflow complet
**Responsabilités** :
- Workflow 0-7 (apprentissage, contexte, impact, clarification, validation, planning, exécution, archivage)
- Travail en silence pendant ÉTAPES 0-6
- Retour message structuré en langage naturel (ÉTAPE 7)
- Progressive disclosure (charge guides un par un)
- **Contexte isolé** : Ne pollue pas la conversation principale

### `.claude/skills/project-master/guides/` - Guides de Workflow
**Rôle** : Instructions détaillées pour chaque étape
**Utilisé par** : L'agent project-master (chargés progressivement selon besoin)
**Contenu** :
- 8 fichiers markdown détaillant chaque étape du workflow
- Chargés **dans le contexte de l'agent**, pas dans la conversation principale

### `.claude/skills/project-master/capabilities/` - Auto-Apprentissage
**Rôle** : Système de connaissances extensible
**Utilisé par** : L'agent project-master
**Fonctionnement** :
1. Claude détecte documentation fournie par l'utilisateur
2. Claude extrait et structure les données
3. Claude passe à l'agent avec format APPRENTISSAGE REQUIS
4. **Agent écrit dans capabilities/** (nouveau fichier JSON)
5. Agent continue workflow en utilisant la nouvelle capacité

**Avantage CRITIQUE** :
- 🎯 **Auto-apprentissage préservé** : L'agent peut créer/mettre à jour des capacités
- 🎯 **Mémoire persistante** : Les capacités sont stockées et réutilisées
- 🎯 **Évolutif** : Le système s'enrichit au fil des projets

### `.claude/context/` - État du Projet
**Rôle** : Mémoire du projet
**Utilisé par** : L'agent project-master (lecture/écriture)
**Contenu** :
- Tâches en cours et terminées
- Patterns d'erreurs détectés
- Décisions techniques
- État du système
- Journal d'améliorations

## 🔄 Flux de Travail Complet

### Exemple : "Créé une todo app avec NiceGUI (https://nicegui.io/documentation/)"

```
1. User envoie demande avec lien doc
   ↓
2. Claude (claude.md) :
   - Détecte le lien
   - WebFetch https://nicegui.io/documentation/
   - Extrait : composants, patterns, best practices, erreurs
   - Structure les données d'apprentissage
   ↓
3. Claude demande à l'agent project-master avec données
   ↓
4. Agent project-master [contexte isolé] :

   ÉTAPE 0 : Apprentissage
   - Lit .claude/skills/project-master/capabilities/_registry.json
   - Vérifie si "nicegui" existe → Non
   - Crée .claude/skills/project-master/capabilities/frameworks/nicegui.json
   - Met à jour _registry.json
   - ✅ Capacité apprise !

   ÉTAPE 1 : Contexte
   - Lit .claude/skills/project-master/guides/CONTEXT-LOADING.md
   - Charge _registry.json
   - Détecte trigger "nicegui" → Charge nicegui.json
   - Lit .claude/context/tasks.md

   ÉTAPE 2 : Impact
   - Lit .claude/skills/project-master/guides/IMPACT-ANALYSIS.md
   - Évalue : Simple (< 2h)
   - Fichiers : 2 nouveaux (main.py, requirements.txt)

   ÉTAPE 3-4 : Clarification/Validation
   - Pas nécessaire (tâche simple)

   ÉTAPE 5 : Planning
   - Lit .claude/skills/project-master/guides/PLANNING.md
   - Utilise execution_hints de nicegui.json
   - Plan : 5 sous-tâches

   ÉTAPE 6 : Exécution
   - Lit .claude/skills/project-master/guides/EXECUTION.md
   - Crée main.py avec ui.table, ui.button, etc.
   - Crée requirements.txt
   - Utilise best_practices de nicegui.json

   ÉTAPE 7 : Archivage
   - Lit .claude/skills/project-master/guides/ARCHIVING.md
   - Met à jour .claude/context/tasks.md
   - Met à jour .claude/context/system-state.md
   - Retourne message structuré
   ↓
5. Claude affiche le résultat à l'utilisateur :

✅ **Application Todo NiceGUI créée avec succès !** (45min)

📂 **Fichiers créés** :
• main.py - Application principale avec interface NiceGUI
• requirements.txt - Dépendances Python

✨ **Fonctionnalités** :
• Ajout de tâches via input + bouton
• Suppression de tâches avec bouton par ligne
• Toggle statut (Complété ↔ En cours)
• Statistiques en temps réel
• Interface moderne avec table interactive

🚀 **Comment utiliser** :
1. pip install -r requirements.txt
2. python main.py
3. Ouvre ton navigateur sur http://localhost:8080

L'application est prête à être utilisée !
```

## 🎯 Avantages de cette Architecture

### ✅ Contexte Isolé
- Agent travaille dans son propre contexte
- Peut charger autant de guides qu'il veut
- Ne pollue pas la conversation principale

### ✅ Auto-Apprentissage Préservé
- **CRITIQUE** : Système de capacités intact
- Agent peut créer/mettre à jour capacités
- Mémoire persistante entre projets
- S'enrichit au fil du temps

### ✅ Progressive Disclosure Efficace
- Agent charge guides uniquement quand nécessaire
- Charge capacités uniquement si triggers matchent
- Pas de limite de contexte

### ✅ Séparation des Responsabilités
- Claude : Interface + Détection doc
- Agent : Exécution workflow complète
- Guides : Instructions détaillées
- Capacités : Connaissances extensibles
- Contexte : État du projet

### ✅ Réutilisabilité
- Guides réutilisables par d'autres agents futurs
- Capacités accessibles par tous
- Architecture modulaire

## 🔍 Comparaison Avant/Après

### Avant (Skill)
```
.claude/
├── claude.md (interface + transformation JSON)
└── skills/
    └── project-master/
        ├── SKILL.md ← Tout le workflow ici (contexte principal!)
        ├── guides/ (chargés dans contexte principal)
        └── capabilities/ (auto-apprentissage)
```

**Problème** : SKILL.md chargeait guides dans contexte principal → explosion

### Après (Agent)
```
.claude/
├── claude.md (interface simplifiée)
├── agents/
│   └── project-master.md ← Workflow ici (contexte isolé!)
└── skills/
    └── project-master/
        ├── guides/ (chargés dans contexte agent)
        └── capabilities/ (auto-apprentissage préservé)
```

**Solution** : Agent avec contexte isolé, guides chargés là-bas

## 🚀 Prochaines Évolutions Possibles

### Phase 2 : Serveur MCP (Optionnel)
Pour alléger encore plus le contexte :

```
.claude/
├── agents/
│   └── project-master.md (utilise MCP tools)
├── skills/project-master/
│   ├── guides/
│   └── capabilities/
└── .mcp.json
    └── "project-context": {
          "command": "./servers/project-context",
          "tools": [
            "get_tasks",
            "update_task",
            "archive_decision"
          ],
          "resources": [
            "@project:tasks",
            "@project:errors"
          ]
        }
```

**Avantage** : Contexte projet accessible via tools MCP au lieu de Read/Write

### Phase 3 : Plugin (Optionnel)
Pour distribution :

```
project-master-plugin/
├── .claude-plugin/plugin.json
├── agents/project-master.md
├── .mcp.json (si Phase 2)
├── servers/project-context/ (si Phase 2)
└── README.md
```

**Avantage** : Installation simple via marketplace

## 📝 Notes Importantes

### Pour les Contributeurs
- **Ne pas recréer** `.claude/skills/project-master/SKILL.md` (supprimé volontairement)
- Modifications de workflow : éditer `.claude/agents/project-master.md`
- Ajout de guides : créer dans `.claude/skills/project-master/guides/`
- Ajout de capacités : créer dans `.claude/skills/project-master/capabilities/[category]/`

### Pour les Utilisateurs
- Utilisation identique : demandes naturelles
- Agent s'active automatiquement (description PROACTIVE)
- Auto-apprentissage fonctionne pareil (fournir doc/liens/règles)
- Résultats formatés automatiquement

### Auto-Apprentissage : Comment ça marche ?

1. **Utilisateur fournit doc** : "Créé une app avec [Framework] (lien)"
2. **Claude extrait** : WebFetch + structuration
3. **Claude passe à l'agent** : Format APPRENTISSAGE REQUIS
4. **Agent apprend (ÉTAPE 0)** :
   - Crée `.claude/skills/project-master/capabilities/frameworks/[framework].json`
   - Met à jour `_registry.json`
5. **Agent continue** : Utilise la nouvelle capacité immédiatement
6. **Projet futur** : Capacité déjà disponible, pas besoin de réapprendre

**Exemple concret** :
- Jour 1 : User fournit doc NiceGUI → Agent apprend
- Jour 5 : User demande "todo app NiceGUI" → Agent sait déjà !
- Jour 30 : 10+ capacités apprises → Agent expert du projet

---

**Date** : 2025-11-05
**Version** : 2.0 (Agent avec auto-apprentissage)
**Architecture** : Interface + Agent + Guides + Capacités + Contexte
