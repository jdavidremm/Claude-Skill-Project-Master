# 🏗️ Architecture 3-Tiers : Interface + Agent + Skill

## 🎯 Vue d'Ensemble

Cette architecture résout le problème d'**explosion de contexte** tout en préservant l'**auto-apprentissage**.

```
┌─────────────────────────────────────────────────────────────┐
│                         UTILISATEUR                          │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  NIVEAU 1 : CLAUDE (.claude/claude.md)                      │
│  Rôle : Interface & Détection de Documentation              │
│  Contexte : Conversation principale                         │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  NIVEAU 2 : AGENT (.claude/agents/project-master.md)       │
│  Rôle : Orchestrateur léger                                 │
│  Contexte : Isolé (ne pollue pas conversation principale)   │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  NIVEAU 3 : SKILL (.claude/skills/workflow-executor/)       │
│  Rôle : Exécuteur du workflow complet                       │
│  Contexte : Invocation de skill (auto-apprentissage)        │
└─────────────────────────────────────────────────────────────┘
```

## 📁 Structure Actuelle

```
.claude/
├── claude.md                                # 🎯 NIVEAU 1 : Interface
├── agents/
│   └── project-master.md                   # 🎯 NIVEAU 2 : Orchestrateur
├── skills/
│   └── workflow-executor/                  # 🎯 NIVEAU 3 : Exécuteur
│       ├── SKILL.md                        # ⭐ Définition du Skill
│       ├── guides/                         # ✅ 8 guides de workflow
│       │   ├── CONTEXT-LOADING.md
│       │   ├── IMPACT-ANALYSIS.md
│       │   ├── REQUIREMENTS-CLARIFIER.md
│       │   ├── VALIDATION.md
│       │   ├── PLANNING.md
│       │   ├── EXECUTION.md
│       │   ├── ERROR-HANDLING.md
│       │   └── ARCHIVING.md
│       └── capabilities/                   # ✅ Auto-apprentissage
│           ├── _registry.json
│           ├── README.md
│           ├── frameworks/
│           ├── libraries/
│           ├── patterns/
│           ├── architectures/
│           ├── tools/
│           ├── languages/
│           └── project-guidelines/
└── context/                                # 📊 État du projet
    ├── tasks.md
    ├── system-state.md
    ├── error-patterns.md
    ├── decisions-log.md
    └── improvements-log.md
```

## 🎯 Responsabilités de Chaque Niveau

### 🔵 NIVEAU 1 : Claude (`.claude/claude.md`)

**Rôle** : Interface entre utilisateur et système

**Responsabilités** :
1. ⭐ **AVANT de déléguer** : Détecter et extraire documentation
   - Liens web → WebFetch
   - Fichiers .md/.txt/.json → Read
   - Règles dictées oralement → Extraction
2. Structurer les données d'apprentissage
3. Demander à l'agent project-master d'exécuter
4. Afficher le résultat retourné (déjà formaté)

**Ce qu'il NE fait PAS** :
- ❌ Coder directement
- ❌ Exécuter le workflow
- ❌ Transformer du JSON brut

**Exemple d'invocation** :
```
Utilise l'agent project-master pour :

DEMANDE UTILISATEUR :
Créé une todo app avec NiceGUI

APPRENTISSAGE REQUIS :
- Framework: NiceGUI
- Category: frameworks
- Source: url
- Triggers: [nicegui, ui.table, ui.button]
- Knowledge: {...}
- Documentation: [contenu complet]
```

### 🟢 NIVEAU 2 : Agent project-master (`.claude/agents/project-master.md`)

**Rôle** : Orchestrateur léger qui délègue au skill

**Responsabilités** :
1. Recevoir la demande de Claude (avec ou sans apprentissage)
2. **Invoquer immédiatement le skill workflow-executor**
3. Transmettre TOUT au skill (demande + données d'apprentissage)
4. Attendre le résultat du skill
5. Retourner le résultat tel quel à Claude

**Ce qu'il NE fait PAS** :
- ❌ Exécuter le workflow lui-même
- ❌ Lire les guides directement
- ❌ Modifier le résultat du skill
- ❌ Accéder aux capacités directement

**Avantage CRITIQUE** : Contexte isolé
- L'agent a son propre contexte séparé de la conversation principale
- Peut invoquer des skills sans polluer le contexte utilisateur
- Les guides/capacités sont chargés dans le contexte du skill, pas de l'agent

**Format d'invocation du skill** :
```markdown
Utilise le skill workflow-executor pour exécuter cette tâche :

[demande utilisateur complète]

[Si apprentissage: APPRENTISSAGE REQUIS: ...]
```

### 🟣 NIVEAU 3 : Skill workflow-executor (`.claude/skills/workflow-executor/SKILL.md`)

**Rôle** : Exécuteur du workflow complet (ÉTAPES 0-7)

**Responsabilités** :
- **ÉTAPE 0** : Apprentissage (si données fournies)
  - Lire `_registry.json`
  - Créer/mettre à jour capacités dans `capabilities/[category]/[id].json`
  - ⚠️ Travaille en silence

- **ÉTAPE 1** : Charger contexte + capacités existantes
  - Lire guides (CONTEXT-LOADING.md)
  - Charger capacités pertinentes depuis _registry.json
  - ⚠️ Travaille en silence

- **ÉTAPE 2** : Analyser impact
  - Lire guides (IMPACT-ANALYSIS.md)
  - Évaluer complexité, fichiers impactés, risques
  - Utiliser capacités chargées pour enrichir analyse
  - ⚠️ Travaille en silence

- **ÉTAPE 3** : Clarifier requirements (si nécessaire)
  - Lire guides (REQUIREMENTS-CLARIFIER.md)
  - Identifier ambiguïtés
  - ✅ **SI ambiguïtés** : Retourner questions en langage naturel
  - **SINON** : Passer à ÉTAPE 4 en silence

- **ÉTAPE 4** : Validation utilisateur (si changement majeur)
  - Lire guides (VALIDATION.md)
  - ✅ **SI impact MAJEUR** : Retourner rapport pour validation
  - **SINON** : Passer à ÉTAPE 5 en silence

- **ÉTAPE 5** : Planifier
  - Lire guides (PLANNING.md)
  - Créer plan détaillé avec sous-tâches
  - Utiliser execution_hints des capacités
  - ⚠️ Travaille en silence

- **ÉTAPE 6** : Exécuter
  - Lire guides (EXECUTION.md)
  - Exécuter tâche par tâche
  - Utiliser common_errors des capacités
  - ⚠️ Travaille en silence

- **ÉTAPE 7** : Archiver (OBLIGATOIRE)
  - Lire guides (ARCHIVING.md)
  - Mettre à jour tous les fichiers de contexte
  - ✅ **Retourner message structuré en langage naturel**

**Avantage CRITIQUE** : Auto-apprentissage préservé
- Le skill gère le système de capacités
- Peut créer/mettre à jour des fichiers JSON dans capabilities/
- Mémoire persistante entre projets

## 🔄 Flux Complet : Exemple Concret

### Exemple : "Créé une todo app avec NiceGUI (https://nicegui.io/documentation/)"

```
┌─────────────────────────────────────────────────────────────┐
│ 1. UTILISATEUR                                              │
│    "Créé une todo app avec NiceGUI (lien doc)"             │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│ 2. CLAUDE (.claude/claude.md)                              │
│    ✅ Détecte le lien                                       │
│    ✅ WebFetch https://nicegui.io/documentation/            │
│    ✅ Extrait : composants, patterns, best practices        │
│    ✅ Structure les données :                               │
│       - Framework: NiceGUI                                  │
│       - Category: frameworks                                │
│       - Triggers: [nicegui, ui.table, ui.button, ui.run]   │
│       - Knowledge: {...}                                    │
│       - Documentation: [contenu complet]                    │
│    ✅ Demande à l'agent project-master                      │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│ 3. AGENT project-master (.claude/agents/project-master.md) │
│    [CONTEXTE ISOLÉ - ne pollue pas conversation]           │
│                                                             │
│    ✅ Reçoit :                                              │
│       - Demande utilisateur                                 │
│       - Données d'apprentissage structurées                 │
│                                                             │
│    ✅ Invoque immédiatement :                               │
│       "Utilise le skill workflow-executor pour exécuter :   │
│        DEMANDE: Créé une todo app avec NiceGUI              │
│        APPRENTISSAGE: [toutes les données]"                 │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│ 4. SKILL workflow-executor                                  │
│    (.claude/skills/workflow-executor/SKILL.md)              │
│    [CONTEXTE D'INVOCATION DE SKILL]                         │
│                                                             │
│    ⚠️ ÉTAPE 0 : Apprentissage (EN SILENCE)                  │
│       • Lit _registry.json                                  │
│       • Vérifie si "nicegui" existe → Non                   │
│       • Crée capabilities/frameworks/nicegui.json           │
│       • Met à jour _registry.json                           │
│       • ✅ Capacité apprise !                               │
│                                                             │
│    ⚠️ ÉTAPE 1 : Contexte (EN SILENCE)                       │
│       • Lit guides/CONTEXT-LOADING.md                       │
│       • Charge _registry.json                               │
│       • Détecte trigger "nicegui" → Charge nicegui.json     │
│       • Lit context/tasks.md                                │
│                                                             │
│    ⚠️ ÉTAPE 2 : Impact (EN SILENCE)                         │
│       • Lit guides/IMPACT-ANALYSIS.md                       │
│       • Évalue : Simple (< 2h)                              │
│       • Fichiers : 2 nouveaux (main.py, requirements.txt)   │
│                                                             │
│    ⚠️ ÉTAPE 3-4 : Clarification/Validation (EN SILENCE)     │
│       • Pas nécessaire (tâche simple)                       │
│                                                             │
│    ⚠️ ÉTAPE 5 : Planning (EN SILENCE)                       │
│       • Lit guides/PLANNING.md                              │
│       • Utilise execution_hints de nicegui.json             │
│       • Plan : 5 sous-tâches                                │
│                                                             │
│    ⚠️ ÉTAPE 6 : Exécution (EN SILENCE)                      │
│       • Lit guides/EXECUTION.md                             │
│       • Crée main.py avec ui.table, ui.button, etc.         │
│       • Crée requirements.txt                               │
│       • Utilise best_practices de nicegui.json              │
│                                                             │
│    ⚠️ ÉTAPE 7 : Archivage (EN SILENCE)                      │
│       • Lit guides/ARCHIVING.md                             │
│       • Met à jour context/tasks.md                         │
│       • Met à jour context/system-state.md                  │
│                                                             │
│    ✅ RETOURNE (message structuré en langage naturel) :     │
│                                                             │
│       ✅ **Application Todo NiceGUI créée avec succès !**   │
│          (45min)                                            │
│                                                             │
│       📂 **Fichiers créés** :                               │
│       • main.py - Application principale avec NiceGUI       │
│       • requirements.txt - Dépendances Python               │
│                                                             │
│       ✨ **Fonctionnalités** :                              │
│       • Ajout de tâches via input + bouton                  │
│       • Suppression de tâches avec bouton par ligne         │
│       • Toggle statut (Complété ↔ En cours)                 │
│       • Statistiques en temps réel                          │
│       • Interface moderne avec table interactive            │
│                                                             │
│       🚀 **Comment utiliser** :                             │
│       1. pip install -r requirements.txt                    │
│       2. python main.py                                     │
│       3. Ouvre ton navigateur sur http://localhost:8080     │
│                                                             │
│       L'application est prête à être utilisée !             │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│ 5. AGENT project-master                                     │
│    ✅ Reçoit le résultat du skill                           │
│    ✅ Retourne tel quel à Claude                            │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│ 6. CLAUDE                                                   │
│    ✅ Affiche le résultat à l'utilisateur                   │
│       (déjà formaté en langage naturel par le skill)        │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│ 7. UTILISATEUR                                              │
│    ✅ Voit le message structuré avec émojis                 │
│    ✅ Instructions claires pour utiliser l'app              │
└─────────────────────────────────────────────────────────────┘
```

## ⚡ Pourquoi 3 Niveaux ?

### 🔴 Problème Initial (Skill seul)

**Avant** :
```
.claude/
├── claude.md (interface + transformation JSON)
└── skills/
    └── project-master/
        ├── SKILL.md ← Tout le workflow ici
        ├── guides/ (8 fichiers, ~3500 lignes)
        └── capabilities/ (auto-apprentissage)
```

**Problème** :
- SKILL.md chargeait **tous les guides** dans le **contexte principal**
- Conversation principale saturée : ~3500-4500 lignes
- ❌ Explosion de contexte dans les grands projets

### 🟢 Solution (Architecture 3-Tiers)

**Maintenant** :
```
.claude/
├── claude.md (interface simplifiée)
├── agents/
│   └── project-master.md ← Orchestrateur léger
└── skills/
    └── workflow-executor/
        ├── SKILL.md ← Exécuteur avec auto-apprentissage
        ├── guides/ (chargés dans contexte du skill)
        └── capabilities/ (auto-apprentissage préservé)
```

**Avantages** :

1. **🎯 Contexte Isolé**
   - Agent travaille dans son propre contexte
   - Skill invoqué charge guides dans SON contexte
   - Conversation principale reste légère
   - ✅ Plus d'explosion de contexte

2. **🎯 Auto-Apprentissage Préservé**
   - CRITIQUE : Système de capacités intact
   - Skill peut créer/mettre à jour capacités
   - Mémoire persistante entre projets
   - S'enrichit au fil du temps

3. **🎯 Progressive Disclosure Efficace**
   - Skill charge guides UNIQUEMENT quand nécessaire
   - Charge capacités UNIQUEMENT si triggers matchent
   - Pas de limite de contexte dans le skill

4. **🎯 Séparation des Responsabilités**
   - Claude : Interface + Détection doc
   - Agent : Orchestration simple
   - Skill : Exécution workflow complète
   - Guides : Instructions détaillées
   - Capacités : Connaissances extensibles

5. **🎯 Réutilisabilité**
   - Guides réutilisables par d'autres skills futurs
   - Capacités accessibles par tous
   - Architecture modulaire

## 🤔 Pourquoi pas Agent seul (2 niveaux) ?

**Si on supprimait le Skill** :
```
.claude/
├── claude.md
└── agents/
    └── project-master.md ← Tout ici (guides + capacités)
```

**Problème** :
- ❌ On perd le concept de **Skill** (fichier SKILL.md requis)
- ❌ On perd le système d'**auto-apprentissage** (Skill écrit dans capabilities/)
- ❌ Agent doit gérer les capacités lui-même (plus complexe)

**User a dit** :
> "bah nan, on perd COMPLETEMENT le skill la, puisque c'est ce qui définit un skill le SKILL.md. [...] une des parties ULTRA interessante, c'est la capacité d'auto apprentissage du skill project master de base"

**Donc** : 3 niveaux = Meilleur équilibre

## 📊 Tableau Comparatif

| Critère | Skill seul | Agent seul | **3-Tiers (actuel)** |
|---------|-----------|-----------|---------------------|
| Contexte isolé | ❌ Non | ✅ Oui | ✅ Oui |
| Auto-apprentissage | ✅ Oui | ⚠️ Complexe | ✅ Oui (préservé) |
| Explosion contexte | ❌ Oui (~3500 lignes) | ✅ Non | ✅ Non |
| Séparation responsabilités | ⚠️ Moyenne | ⚠️ Moyenne | ✅ Excellente |
| Réutilisabilité | ⚠️ Moyenne | ⚠️ Moyenne | ✅ Excellente |
| **VERDICT** | 🔴 Problème | 🟠 OK mais perd Skill | 🟢 **OPTIMAL** |

## 🔍 Questions Fréquentes

### Q1 : Pourquoi l'agent ne fait-il pas tout le travail lui-même ?

**R** : Parce qu'on perdrait le concept de Skill et son système d'auto-apprentissage. Le Skill est une entité qui peut :
- Être invoqué par le modèle automatiquement
- Gérer un système de capacités extensibles
- Écrire dans `capabilities/` pour apprendre
- Être réutilisé par d'autres agents futurs

### Q2 : L'agent peut-il utiliser un Skill ?

**R** : **OUI !** Les Skills sont "invoked by the model", et l'agent EST une instance de Claude avec accès aux Skills. C'est l'architecture que nous utilisons.

### Q3 : Pourquoi 3 niveaux au lieu de 2 ?

**R** :
- 2 niveaux (Interface + Agent) = Perd le Skill et l'auto-apprentissage
- 3 niveaux (Interface + Agent + Skill) = Préserve tout + contexte isolé

### Q4 : Le skill charge-t-il tous les guides d'un coup ?

**R** : **NON !** Le skill utilise la **progressive disclosure** : il charge les guides UN PAR UN selon les besoins, dans SON propre contexte d'invocation.

### Q5 : Comment l'auto-apprentissage fonctionne-t-il ?

**R** :
1. User fournit doc → Claude extrait → Passe à Agent
2. Agent passe au Skill
3. **Skill (ÉTAPE 0)** crée `capabilities/frameworks/[nom].json`
4. Skill continue workflow en utilisant la nouvelle capacité
5. Projet futur : Capacité déjà disponible !

### Q6 : L'utilisateur doit-il changer ses habitudes ?

**R** : **NON !** L'utilisation est identique :
- Demandes naturelles
- Agent s'active automatiquement (description PROACTIVE)
- Auto-apprentissage fonctionne pareil (fournir doc/liens/règles)
- Résultats formatés automatiquement

## 🚀 Prochaines Évolutions Possibles

### Phase 2 : Serveur MCP (Optionnel)

Pour alléger encore plus :

```
.claude/
├── agents/
│   └── project-master.md (utilise MCP tools)
├── skills/workflow-executor/
│   ├── SKILL.md
│   ├── guides/
│   └── capabilities/
└── .mcp.json
    └── "project-context": {
          "command": "./servers/project-context",
          "tools": [
            "get_tasks",
            "update_task",
            "archive_decision"
          ]
        }
```

**Avantage** : Contexte projet accessible via tools MCP au lieu de Read/Write

### Phase 3 : Plugin (Distribution)

Pour distribution :

```
project-master-plugin/
├── .claude-plugin/plugin.json
├── agents/project-master.md
├── skills/workflow-executor/
├── .mcp.json (si Phase 2)
└── README.md
```

**Avantage** : Installation simple via marketplace

## 📝 Notes pour Contributeurs

### ⚠️ IMPORTANT : Ne pas supprimer SKILL.md

- `.claude/skills/workflow-executor/SKILL.md` est **CRITIQUE**
- C'est ce qui définit le Skill (le modèle le détecte via ce fichier)
- Sans SKILL.md, on perd le Skill et l'auto-apprentissage

### Modifications de Workflow

- **Orchestration** : éditer `.claude/agents/project-master.md`
- **Exécution** : éditer `.claude/skills/workflow-executor/SKILL.md`
- **Guides** : créer/modifier dans `.claude/skills/workflow-executor/guides/`
- **Capacités** : créer dans `.claude/skills/workflow-executor/capabilities/[category]/`

### Tests

Pour tester l'architecture :
1. Demande simple : "Créé une fonction hello_world"
2. Demande avec apprentissage : "Créé une app avec [Framework] (lien doc)"
3. Demande avec clarification : "Créé une app web" (ambiguë)
4. Demande majeure : "Migrer vers React 19" (validation requise)

## 🎓 Glossaire

- **Skill** : Fichier SKILL.md invoqué par le modèle, peut gérer auto-apprentissage
- **Agent** : Instance de Claude avec contexte isolé et system prompt custom
- **Progressive Disclosure** : Charger fichiers un par un selon besoins
- **Auto-apprentissage** : Système de capacités extensibles dans `capabilities/`
- **Contexte isolé** : Contexte séparé de la conversation principale
- **Orchestrateur** : Composant qui délègue sans faire le travail lui-même
- **Exécuteur** : Composant qui fait le vrai travail (ÉTAPES 0-7)

---

**Date** : 2025-11-05
**Version** : 3.0 (Architecture 3-Tiers)
**Architecture** : Interface (Claude) → Orchestrateur (Agent) → Exécuteur (Skill)
**Problème résolu** : Explosion de contexte (~3500 lignes) → Contexte isolé
**Fonctionnalité préservée** : Auto-apprentissage (ULTRA importante selon user)
