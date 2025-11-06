# Initialisation Projet Existant - Remplissage Registres

## Objectif

Remplir les 5 registres codebase ULTRA-LÉGERS pour un projet existant, permettant au workflow-executor de comprendre rapidement la structure du projet sans lire tous les fichiers.

---

## Quand Utiliser Ce Guide

- **Nouveau workflow sur projet existant** : Les registres sont vides mais le code existe
- **Migration vers ce système** : Projet déjà développé, besoin d'initialiser les registres
- **Après interruption longue** : Registres périmés, besoin de refresh

---

## ✅ CHECKLIST D'INITIALISATION

### Prérequis
- [ ] Projet existe déjà avec du code
- [ ] Les 5 fichiers registres existent (templates vides dans `.claude/context/codebase/`)

### Étapes
- [ ] 1. Scanner et remplir `structure.md`
- [ ] 2. Scanner et remplir `database.md` (si BDD)
- [ ] 3. Scanner et remplir `api.md` (si API)
- [ ] 4. Scanner et remplir `components.md` (si UI)
- [ ] 5. Scanner et remplir `dependencies.md`

---

## 📋 REGISTRE 1 : structure.md

### Objectif
Arborescence + dossiers clés (1 ligne par dossier)

### Commande de scan
```bash
# Scanner l'arborescence (max 3 niveaux)
find . -maxdepth 3 -type d \
  -not -path "*/\.*" \
  -not -path "*/node_modules*" \
  -not -path "*/venv*" \
  -not -path "*/__pycache__*" \
  -not -path "*/dist*" \
  -not -path "*/build*" | sort
```

### Format ULTRA-LÉGER
```markdown
# Project Structure

Last updated: 2025-11-06

---

## 📁 Dossiers Clés

project/
├── src/                 # Code source principal
├── api/                 # Routes API REST
├── models/              # Models SQLAlchemy
├── components/          # Composants NiceGUI
├── migrations/          # Migrations Alembic
├── tests/               # Tests unitaires
├── scripts/             # Scripts utilitaires
└── docs/                # Documentation

## 📄 Fichiers Racine

- main.py                # Point d'entrée application
- requirements.txt       # Dépendances Python
- .env.example           # Variables d'environnement
- README.md              # Documentation projet
```

**Règles** :
- 1 ligne par dossier : `├── nom/ # Description courte`
- Fichiers racine importants seulement
- PAS de contenu détaillé, juste la structure

---

## 📋 REGISTRE 2 : database.md

### Objectif
Models/tables + relations (1 ligne par model)

### Commandes de scan

**Python (SQLAlchemy)** :
```bash
# Trouver tous les models
grep -r "class.*Base" models/ --include="*.py" | grep -v "__pycache__"
```

**TypeScript (Prisma)** :
```bash
# Lire le schema Prisma
cat prisma/schema.prisma | grep "^model"
```

**Django** :
```bash
# Trouver les models Django
grep -r "class.*models.Model" . --include="*.py" | grep -v "migrations"
```

### Format ULTRA-LÉGER
```markdown
# Database Schema

Last updated: 2025-11-06

---

## 📊 Models

### User
- id, email, password_hash, created_at
- Relations: → todos (1-N)

### Todo
- id, title, completed, user_id, created_at
- Relations: → user (N-1)

### Category
- id, name, color
- Relations: → todos (N-N via todo_categories)

---

**Total** : 3 tables, 2 relations
```

**Règles** :
- 1 model = 1 section (nom + champs principaux + relations)
- Champs : Liste séparée par virgules (PAS de types détaillés)
- Relations : Flèches simples (→ N-1, ← 1-N, ↔ N-N)

---

## 📋 REGISTRE 3 : api.md

### Objectif
Routes API + endpoints (1 ligne par route)

### Commandes de scan

**FastAPI** :
```bash
# Trouver les decorators de route
grep -r "@app\.\(get\|post\|put\|delete\|patch\)" api/ --include="*.py"
```

**Flask** :
```bash
grep -r "@app\.route\|@blueprint\.route" . --include="*.py"
```

**Express.js** :
```bash
grep -r "router\.\(get\|post\|put\|delete\)" routes/ --include="*.js" --include="*.ts"
```

### Format ULTRA-LÉGER
```markdown
# API Endpoints

Last updated: 2025-11-06

---

## 🔌 Routes

### Users
- GET    /api/users              # Liste users
- GET    /api/users/:id          # Détails user
- POST   /api/users              # Créer user
- PUT    /api/users/:id          # Modifier user
- DELETE /api/users/:id          # Supprimer user

### Todos
- GET    /api/todos              # Liste todos (filtre user)
- POST   /api/todos              # Créer todo
- PATCH  /api/todos/:id          # Toggle completed
- DELETE /api/todos/:id          # Supprimer todo

### Auth
- POST   /api/auth/login         # Connexion
- POST   /api/auth/logout        # Déconnexion
- POST   /api/auth/refresh       # Refresh token

---

**Total** : 13 endpoints
```

**Règles** :
- 1 ligne par route : `METHOD /path # Description courte`
- Grouper par ressource (Users, Todos, etc.)
- PAS de détails sur params/body/response

---

## 📋 REGISTRE 4 : components.md

### Objectif
Composants UI + purpose (1 ligne par composant)

### Commandes de scan

**React/Next.js** :
```bash
# Trouver les composants
find src/components -name "*.tsx" -o -name "*.jsx" | grep -v ".test"
```

**Vue** :
```bash
find src/components -name "*.vue"
```

**NiceGUI (Python)** :
```bash
grep -r "@ui\.page\|def.*page" . --include="*.py"
```

### Format ULTRA-LÉGER
```markdown
# UI Components

Last updated: 2025-11-06

---

## 🎨 Pages

- TodoListPage             # Page principale avec liste todos
- LoginPage                # Page authentification
- ProfilePage              # Page profil utilisateur

## 🧩 Components

- TodoItem                 # Item todo individuel (checkbox + texte)
- TodoForm                 # Formulaire ajout/édition todo
- Header                   # Header avec nav + user menu
- Sidebar                  # Sidebar navigation
- Modal                    # Modal générique réutilisable

---

**Total** : 3 pages, 5 composants
```

**Règles** :
- 1 ligne par composant : `NomComposant # Description courte`
- Séparer Pages vs Composants
- PAS de détails sur props/state

---

## 📋 REGISTRE 5 : dependencies.md

### Objectif
Packages + versions + purpose (1 ligne par dépendance majeure)

### Commandes de scan

**Python** :
```bash
cat requirements.txt
# OU
pip list --format=freeze
```

**Node.js** :
```bash
cat package.json | jq '.dependencies, .devDependencies'
```

**Rust** :
```bash
cat Cargo.toml | grep "^\[dependencies\]" -A 50
```

### Format ULTRA-LÉGER
```markdown
# Dependencies

Last updated: 2025-11-06

---

## 📦 Core

- nicegui==1.4.0           # Framework UI
- fastapi==0.104.0         # Framework API
- sqlalchemy==2.0.23       # ORM base de données
- pydantic==2.5.0          # Validation données

## 🔧 Dev Tools

- pytest==7.4.3            # Tests unitaires
- black==23.11.0           # Formatage code
- ruff==0.1.6              # Linter

## 🚀 Deployment

- uvicorn==0.24.0          # Serveur ASGI
- alembic==1.12.1          # Migrations BDD

---

**Total** : 9 packages
```

**Règles** :
- 1 ligne par package : `nom==version # Purpose court`
- Grouper par catégorie (Core, Dev Tools, Deployment, etc.)
- Seulement dépendances MAJEURES (pas toutes les 50 deps)

---

## 🚀 Workflow d'Initialisation Rapide

### Option 1 : Manuel (Recommandé pour première fois)

1. Ouvrir chaque registre `.md` dans `.claude/context/codebase/`
2. Scanner le projet avec les commandes ci-dessus
3. Remplir ULTRA-LÉGER (1 ligne par item)
4. Mettre à jour "Last updated"

**Temps estimé** : 10-15 minutes pour projet moyen

---

### Option 2 : Script d'Initialisation (À créer)

Créer un script `scripts/init-registries.sh` :

```bash
#!/bin/bash

echo "🔍 Scanning project structure..."
# Scanner + générer structure.md

echo "🗄️ Scanning database models..."
# Scanner + générer database.md

echo "🔌 Scanning API routes..."
# Scanner + générer api.md

echo "🎨 Scanning UI components..."
# Scanner + générer components.md

echo "📦 Scanning dependencies..."
# Scanner + générer dependencies.md

echo "✅ All registries initialized!"
```

**Temps estimé** : 30 secondes (après création du script)

---

## 📊 Exemple Complet - Projet Todo App

### structure.md
```markdown
todo-app/
├── src/               # Code source
├── api/               # Routes FastAPI
├── models/            # Models SQLAlchemy
├── components/        # Composants NiceGUI
├── migrations/        # Migrations Alembic
└── tests/             # Tests

- main.py              # Point d'entrée
- requirements.txt     # Dépendances
```

### database.md
```markdown
### User
- id, email, password_hash
- Relations: → todos (1-N)

### Todo
- id, title, completed, user_id
- Relations: → user (N-1)
```

### api.md
```markdown
### Todos
- GET    /api/todos              # Liste
- POST   /api/todos              # Créer
- PATCH  /api/todos/:id          # Toggle
- DELETE /api/todos/:id          # Supprimer

### Auth
- POST   /api/auth/login         # Connexion
```

### components.md
```markdown
## Pages
- TodoListPage             # Liste todos

## Components
- TodoItem                 # Item individuel
- TodoForm                 # Formulaire
```

### dependencies.md
```markdown
## Core
- nicegui==1.4.0           # UI
- fastapi==0.104.0         # API
- sqlalchemy==2.0.23       # ORM

## Dev
- pytest==7.4.3            # Tests
```

---

## ⚠️ Bonnes Pratiques

### ✅ FAIRE

- **Rester ULTRA-LÉGER** : 1 ligne par item
- **Mettre à jour "Last updated"** : Important pour savoir si périmé
- **Grouper logiquement** : Par ressource, par catégorie
- **Descriptions courtes** : 2-5 mots max

### ❌ NE PAS FAIRE

- **Copier tout le code** : C'est un REGISTRE, pas une copie
- **Détails exhaustifs** : Pas de types, signatures complètes, etc.
- **Oublier les relations** : Critique pour database.md
- **Ignorer les mises à jour** : Registre périmé = inutile

---

## 🔄 Maintenance Continue

### Quand Mettre à Jour ?

Les registres sont automatiquement mis à jour par ÉTAPE 7 (Archivage) lors de chaque workflow.

**Mais initialisation manuelle nécessaire si** :
- Premier workflow sur projet existant
- Registres très périmés (> 1 mois sans workflow)
- Changements massifs faits hors workflow

### Comment Vérifier Si Périmé ?

```bash
# Vérifier dernière mise à jour
head -n 5 .claude/context/codebase/*.md | grep "Last updated"
```

Si date > 1 mois → Considérer re-scan

---

## 💡 Conseils

1. **Commencer simple** : Remplir minimalement au début, enrichir au fil des workflows
2. **Progressive disclosure** : Le workflow-executor lit les détails (fichiers) si besoin
3. **Automatiser si possible** : Créer script d'init pour projets similaires
4. **Ne pas sur-documenter** : Les registres sont des INDEX, pas de la doc

---

## 🎯 Résultat Final

Après initialisation, le workflow-executor peut :
- ✅ Comprendre la structure projet en 5 secondes (pas 5 minutes)
- ✅ Éviter doublons (détecte ce qui existe déjà)
- ✅ Respecter patterns existants (voit la cohérence)
- ✅ Progressive disclosure (lit détails seulement si nécessaire)

**Temps investi** : 10-15 minutes
**Gain de temps** : 5-10 minutes par workflow futur

---

## 📚 Voir Aussi

- `ARCHIVING.md` - Comment maintenir les registres à jour
- `REGISTRES.md` - Détails complets sur les 5 registres
- `CONTEXT-LOADING.md` - Comment le workflow-executor charge les registres
