# Système de Capacités - Apprentissage Dynamique

## 🎯 Philosophie

Le système démarre **VIDE** (ou quasi-vide) et s'enrichit **dynamiquement** au fur et à mesure du projet, selon **TES besoins spécifiques**.

### ❌ Ce qu'on NE veut PAS
```
Système pré-chargé avec :
- React, Vue, Angular, Svelte...
- PostgreSQL, MongoDB, MySQL...
- Express, Fastify, NestJS...
→ Lourd, inutile, générique, pas adapté
```

### ✅ Ce qu'on VEUT
```
Le système démarre vide

User : "Voici la doc de React 18 pour notre projet"
→ Le workflow-executor l'intègre

User : "Va chercher la doc de l'API Stripe"
→ Le workflow-executor la récupère et l'ajoute

User : "On utilise toujours cette structure de fichiers"
→ Le workflow-executor le mémorise

→ Le système devient expert de TON projet spécifiquement
```

## 📁 Structure

```
capabilities/
├── _registry.json                    # Registre (VIDE au départ)
├── README.md                         # Ce fichier
├── frameworks/                       # Frameworks (React, NiceGUI, FastAPI, etc.)
├── libraries/                        # Libraries (SQLAlchemy, Stripe, requests, etc.)
├── patterns/                         # Design patterns, architecture patterns
├── tools/                            # Outils (Docker, Git workflows, etc.)
├── languages/                        # Langages (Python conventions, TypeScript, etc.)
└── project-guidelines/               # Guidelines spécifiques au projet
```

## 🔄 Comment Enrichir le Système

### Méthode 1 : Fournir un Fichier

**Commande utilisateur** :
```
"Utilise ce fichier comme documentation React pour le projet"
[Fournit react-guidelines.md]
```

**Ce que fait le workflow-executor** :
1. Lit le fichier `.md` fourni
2. Extrait les informations pertinentes
3. Convertit en format JSON de capacité
4. Stocke dans `capabilities/frameworks/react-guidelines.json`
5. Met à jour `_registry.json`

**Résultat** :
```json
// capabilities/frameworks/react-guidelines.json
{
  "id": "react-project-guidelines",
  "name": "React - Guidelines Projet",
  "source": "user_provided_file",
  "added_date": "2025-11-04",
  "triggers": ["react", "hooks", "component"],
  "knowledge": {
    "best_practices": [...],
    "project_specific_rules": [...]
  }
}
```

### Méthode 2 : Fournir un Lien Web

**Commande utilisateur** :
```
"Va chercher la documentation de Stripe API sur stripe.com/docs/api"
```

**Ce que fait le workflow-executor** :
1. Fetch le contenu du lien
2. Extrait les informations clés
3. Crée `capabilities/libraries/stripe-api.json`
4. Met à jour `_registry.json`

**Résultat** :
```json
// capabilities/libraries/stripe-api.json
{
  "id": "stripe-api",
  "name": "Stripe API Documentation",
  "source": "https://stripe.com/docs/api",
  "fetched_date": "2025-11-04",
  "triggers": ["stripe", "payment", "checkout"],
  "knowledge": {
    "endpoints": [...],
    "common_errors": [...]
  }
}
```

### Méthode 3 : Dicter des Conventions

**Commande utilisateur** :
```
"Pour ce projet, on utilise TOUJOURS des interfaces TypeScript plutôt que des types, et tous les composants doivent avoir un fichier .test.tsx"
```

**Ce que fait le workflow-executor** :
1. Crée `capabilities/project-guidelines/typescript-conventions.json`
2. Ajoute ces règles dans `best_practices`
3. Met à jour `_registry.json`

**Résultat** :
```json
// capabilities/project-guidelines/typescript-conventions.json
{
  "id": "project-typescript-conventions",
  "name": "Conventions TypeScript - Projet",
  "source": "user_dictated",
  "added_date": "2025-11-04",
  "triggers": ["typescript", "interface", "type"],
  "knowledge": {
    "best_practices": [
      "Toujours utiliser interface plutôt que type",
      "Chaque composant doit avoir un fichier .test.tsx"
    ]
  }
}
```

### Méthode 4 : Apprentissage Automatique (Proposition du workflow-executor)

**Situation** :
Le workflow-executor détecte que tu utilises toujours le même pattern de structure de dossiers.

**Ce que fait le workflow-executor** :
```json
// Retour à Claude
{
  "status": "learning_suggestion",
  "pattern_detected": "Structure de module avec index.ts, types.ts, utils.ts",
  "occurrences": 5,
  "suggestion": "Veux-tu que je mémorise ce pattern de structure ?"
}
```

**Si l'utilisateur accepte** :
1. Crée `capabilities/patterns/module-structure-pattern.json`
2. Appliquera automatiquement ce pattern désormais

## 📊 Cycle de Vie d'un Projet

### 🌱 Jour 1 - Démarrage
```
capabilities/ : VIDE (ou juste _registry.json vide)

Le système : Connaissances générales uniquement
```

### 🌿 Jour 2 - Premiers ajouts
```
User: "Voici notre stack : React 18 + TypeScript + Tailwind"
User: [Fournit fichier conventions.md]

capabilities/ :
└── project-guidelines/
    └── conventions.json ✅

Le système connaît maintenant :
- Conventions de code
- Stack technique
```

### 🌳 Jour 5 - Enrichissement
```
User: "Va chercher la doc Stripe et Supabase"

capabilities/ :
├── project-guidelines/
│   └── conventions.json
└── libraries/
    ├── stripe-api.json ✅
    └── supabase-api.json ✅

Le système connaît maintenant :
- Conventions de code
- API Stripe
- API Supabase
```

### 🌲 Jour 10 - Apprentissage
```
Le workflow-executor détecte pattern et propose mémorisation

capabilities/ :
├── project-guidelines/
│   └── conventions.json
├── libraries/
│   ├── stripe-api.json
│   └── supabase-api.json
└── patterns/
    └── error-handling-pattern.json ✅

Le système applique maintenant :
- Conventions
- Connaît les APIs
- Applique les patterns détectés
```

### 🏆 Jour 30 - Expert
```
capabilities/ contient :
- 3 project-guidelines/
- 5 libraries/
- 2 frameworks/
- 4 patterns/

→ Le système est devenu EXPERT de TON projet
→ Génère du code PARFAITEMENT aligné avec TES conventions
→ Connaît TOUTES les APIs que TU utilises
```

## 🎨 Exemples de Capacités

### Exemple 1 : Convention de Nommage

**User dit** : "On nomme toujours les composants avec PascalCase et les hooks avec use[Action]"

**Capacité créée** :
```json
{
  "id": "naming-conventions",
  "name": "Conventions de Nommage - Projet",
  "source": "user_dictated",
  "triggers": ["naming", "convention", "nom"],
  "knowledge": {
    "best_practices": [
      "Composants : PascalCase (UserProfile, TodoList)",
      "Hooks : use[Action] (useAuth, useFetch)",
      "Fichiers : kebab-case (user-profile.tsx, todo-list.tsx)"
    ]
  }
}
```

### Exemple 2 : Structure de Projet

**User dit** : "Chaque feature doit avoir : components/, hooks/, utils/, types.ts"

**Capacité créée** :
```json
{
  "id": "feature-structure",
  "name": "Structure Feature - Projet",
  "source": "user_dictated",
  "triggers": ["feature", "structure", "dossier"],
  "knowledge": {
    "file_structure": {
      "feature/": "Dossier racine de la feature",
      "feature/components/": "Composants React",
      "feature/hooks/": "Custom hooks",
      "feature/utils/": "Fonctions utilitaires",
      "feature/types.ts": "Types TypeScript"
    },
    "execution_hints": {
      "planning": [
        "Créer la structure complète dès le début",
        "Créer types.ts en premier"
      ]
    }
  }
}
```

### Exemple 3 : Documentation Externe

**User dit** : "Va chercher la doc de l'API interne sur http://api.company.com/docs"

**Capacité créée** :
```json
{
  "id": "internal-api",
  "name": "API Interne - Company",
  "source": "http://api.company.com/docs",
  "fetched_date": "2025-11-04",
  "triggers": ["api", "endpoint", "fetch"],
  "knowledge": {
    "base_url": "https://api.company.com",
    "authentication": "Bearer token dans headers",
    "endpoints": [
      {
        "method": "GET",
        "path": "/users",
        "description": "Liste des utilisateurs"
      }
    ],
    "common_errors": [
      {
        "code": 401,
        "error": "Unauthorized",
        "solution": "Vérifier le token d'authentification"
      }
    ]
  }
}
```

## 🚀 Commandes Utilisateur

L'utilisateur peut dire à Claude :

### Ajouter des Capacités
```
"Ajoute cette doc comme référence"
"Voici notre fichier de conventions de code"
"Va chercher la doc de [technologie] sur [lien]"
"Utilise ce fichier .md comme guidelines"
"Pour ce projet, on fait toujours [règle]"
```

### Apprentissage
```
"Mémorise ce pattern pour le projet"
"Ajoute cette convention à nos règles"
"Applique toujours cette structure"
```

### Gestion
```
"Oublie cette capacité"
"Montre-moi les capacités actuelles"
"Quelles capacités as-tu chargées ?"
"Mets à jour la capacité [nom]"
```

## ⚙️ Workflow du Système

### ÉTAPE 1 : Charger Contexte + Capacités

```
1. Lire _registry.json
2. Analyser la demande utilisateur
3. Identifier les triggers qui matchent
4. Charger UNIQUEMENT les capacités pertinentes
```

**Exemple** :
```
User: "Créé un composant de paiement Stripe"

Le workflow-executor :
1. Lit _registry.json
2. Détecte "stripe" dans la demande
3. Charge capabilities/libraries/stripe-api.json
4. Charge capabilities/project-guidelines/conventions.json (toujours chargé)

→ Connaît l'API Stripe + conventions projet
```

### ÉTAPE 2-6 : Utiliser les Capacités

Durant tout le workflow, le workflow-executor utilise :
- `best_practices` pour l'analyse d'impact
- `file_structure` pour la planification
- `common_errors` pour la résolution d'erreurs
- `execution_hints` pour l'exécution

### ÉTAPE 7 : Apprentissage

À la fin, le workflow-executor peut proposer :
```json
{
  "status": "success",
  "archived": true,
  "learning_suggestion": {
    "pattern_detected": "Error handling avec try/catch + toast",
    "occurrences": 4,
    "suggest_memorize": true
  }
}
```

## 🎯 Avantages

✅ **Léger** : Démarre vide, pas de surcharge
✅ **Pertinent** : Uniquement ce dont TU as besoin
✅ **Évolutif** : S'enrichit au fil du temps
✅ **Spécifique** : TON projet, TES conventions, TES APIs
✅ **Pas de pollution** : Pas de React si projet Python
✅ **Apprentissage** : Mémorise TES patterns

## 📝 Format d'une Capacité

Structure standard (JSON) :

```json
{
  "id": "identifiant-unique",
  "name": "Nom Lisible",
  "version": "1.0.0",
  "category": "frameworks|libraries|patterns|tools|languages|project-guidelines",
  "source": "user_provided|user_dictated|url|auto_learned",
  "added_date": "2025-11-04",
  "last_updated": "2025-11-04",

  "triggers": ["mot-clé-1", "mot-clé-2", "..."],

  "knowledge": {
    "best_practices": ["règle 1", "règle 2"],
    "common_patterns": [
      {
        "name": "Pattern X",
        "description": "...",
        "code_example": "..."
      }
    ],
    "common_errors": [
      {
        "error": "...",
        "solution": "..."
      }
    ],
    "file_structure": {
      "dossier/": "description"
    }
  },

  "execution_hints": {
    "planning": ["conseil 1"],
    "validation": ["conseil 1"],
    "execution": ["étape 1"]
  }
}
```

## 🔄 Maintenance des Capacités

### Mettre à Jour
```
User: "Mets à jour la capacité React, on utilise maintenant React 19"

Le workflow-executor :
1. Trouve capabilities/frameworks/react-guidelines.json
2. Met à jour le contenu
3. Incrémente la version
4. Met à jour last_updated
```

### Supprimer
```
User: "On n'utilise plus Stripe, supprime cette capacité"

Le workflow-executor :
1. Supprime capabilities/libraries/stripe-api.json
2. Enlève l'entrée de _registry.json
```

### Consulter
```
User: "Montre-moi toutes les capacités actuelles"

Le workflow-executor :
1. Lit _registry.json
2. Liste toutes les capacités avec :
   - Nom
   - Source
   - Date d'ajout
   - Nombre d'utilisations
```

## 💡 Conseils d'Utilisation

1. **Ajouter progressivement** : Pas tout d'un coup au début
2. **Ajouter au besoin** : Quand tu en as vraiment besoin
3. **Être spécifique** : Plus c'est précis, mieux c'est
4. **Maintenir à jour** : Mettre à jour si conventions changent
5. **Nettoyer** : Supprimer ce qui devient obsolète

## 🎓 Exemple Complet : Projet E-commerce

### Jour 1
```bash
# État initial
capabilities/_registry.json : {"capabilities": []}
```

### Jour 2
```bash
User: "Voici nos conventions de code [fichier .md]"

# Ajouté :
capabilities/project-guidelines/coding-standards.json
```

### Jour 5
```bash
User: "Va chercher la doc Stripe et NextAuth"

# Ajouté :
capabilities/libraries/stripe-api.json
capabilities/libraries/nextauth-docs.json
```

### Jour 10
```bash
User: "On structure toujours les features comme ça [explique]"

# Ajouté :
capabilities/patterns/feature-structure.json
```

### Jour 15
```bash
Le workflow-executor détecte : "Error handling pattern répété 5 fois"
Le workflow-executor propose : "Mémoriser ce pattern ?"
User : "Oui"

# Ajouté :
capabilities/patterns/error-handling.json
```

### Jour 30
```bash
# État final
capabilities/
├── project-guidelines/
│   ├── coding-standards.json
│   └── naming-conventions.json
├── libraries/
│   ├── stripe-api.json
│   └── nextauth-docs.json
└── patterns/
    ├── feature-structure.json
    └── error-handling.json

→ Le système connaît PARFAITEMENT ton projet
```

---

## 🚦 Pour Commencer

1. **Démarre avec rien** : `_registry.json` est vide
2. **Ajoute au fur et à mesure** : Quand tu en as besoin
3. **Laisse apprendre** : Le workflow-executor proposera de mémoriser
4. **Maintiens à jour** : Mets à jour si ça change

**Le système s'enrichit AVEC toi, POUR toi** 🎯
