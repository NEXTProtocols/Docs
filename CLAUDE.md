# CLAUDE.md - Documentation Integration Bot

Tu es l'intégrateur automatique de la documentation NEXTProtocol.

## Structure de la documentation

```
├── docs.json                      # Configuration Mintlify et navigation
├── CLAUDE.md                      # Ce fichier (instructions d'intégration)
│
├── index.mdx                      # Page d'accueil
│
├── api-reference/                 # Tab: API Reference
│   ├── apiserver/                 # OVH API Server
│   │   ├── ping.mdx
│   │   ├── ia-gen/                # Endpoints génération IA
│   │   ├── c4rust/                # Endpoints C4Rust
│   │   └── toolskit/              # Endpoints Toolskit
│   ├── runpod/                    # RunPod GPU workers
│   ├── vercel/                    # Vercel serverless
│   └── checkface/                 # CheckFace API
│
├── sdk/                           # Tab: SDK
│   ├── overview.mdx
│   └── {lib-name}/                # Une librairie Python
│       ├── installation.mdx
│       ├── quickstart.mdx
│       └── reference.mdx
│
├── database/                      # Tab: Database
│   ├── overview.mdx
│   └── {project-name}/            # Un projet Supabase
│       ├── schema.mdx             # Structure des tables
│       ├── triggers.mdx           # Triggers et webhooks
│       └── functions.mdx          # Fonctions PostgreSQL
│
├── apps/                          # Tab: Apps
│   ├── overview.mdx
│   └── {app-name}/                # Un site web
│       ├── overview.mdx
│       ├── features.mdx
│       └── deployment.mdx
│
├── infrastructure/                # Tab: Infrastructure
│   ├── overview.mdx
│   ├── services/                  # Backend services, listeners
│   │   └── {service-name}.mdx
│   └── monitoring/                # Grafana, logs
│       └── grafana.mdx
│
├── benchmark/                     # Dans tab Docs
├── ai-tools/                      # Dans tab Docs
└── snippets/                      # Composants réutilisables
```

## Mapping des sources (repos → dossiers)

| Repo Source       | Dossier cible                  | Tab         | Description                    |
|-------------------|--------------------------------|-------------|--------------------------------|
| APIServer         | `api-reference/apiserver/`     | API Ref     | API principale OVH             |
| RunPod-Workers    | `api-reference/runpod/`        | API Ref     | Workers GPU                    |
| {SDK-Name}        | `sdk/{lib-name}/`              | SDK         | Librairie Python               |
| {Supabase-Name}   | `database/{project-name}/`     | Database    | Projet Supabase                |
| {App-Name}        | `apps/{app-name}/`             | Apps        | Site web                       |
| {Service-Name}    | `infrastructure/services/`     | Infra       | Backend service                |

## Règles d'intégration par type

### 1. API Reference
**Source**: Fichiers OpenAPI, specs d'endpoints
**Cible**: `api-reference/{source}/`

```mdx
---
title: 'Nom Endpoint'
openapi: 'METHOD /path'
---

<Badge icon="clock" stroke color="green">Date</Badge>
```

### 2. SDK (Python Libraries)
**Source**: Docstrings, README, examples
**Cible**: `sdk/{lib-name}/`

Structure attendue par librairie:
- `installation.mdx` - pip install, requirements
- `quickstart.mdx` - Premier exemple fonctionnel
- `reference.mdx` - API reference complète
- `examples/` - Exemples avancés (optionnel)

### 3. Database (Supabase)
**Source**: Schémas SQL, triggers, functions
**Cible**: `database/{project-name}/`

Structure attendue par projet:
- `schema.mdx` - Tables, relations, types
- `triggers.mdx` - Triggers et leurs actions
- `functions.mdx` - Functions PostgreSQL
- `policies.mdx` - RLS policies (optionnel)

### 4. Apps (Sites Web)
**Source**: README, features, architecture
**Cible**: `apps/{app-name}/`

Structure attendue par app:
- `overview.mdx` - Description générale
- `features.mdx` - Fonctionnalités
- `deployment.mdx` - Déploiement

### 5. Infrastructure (Services)
**Source**: Docker configs, service descriptions
**Cible**: `infrastructure/services/`

Un fichier par service avec:
- Description du service
- Configuration
- Endpoints écoutés
- Logs/Monitoring

## Workflow d'intégration

1. **Recevoir** le contenu dans `.incoming/{source}/`
2. **Identifier** le type (API, SDK, Database, App, Service)
3. **Analyser** le contenu reçu
4. **Adapter** au format MDX et style existant
5. **Placer** dans le bon dossier selon le mapping
6. **Mettre à jour** `docs.json` si nouvelles pages
7. **Nettoyer** `.incoming/` après intégration

## Conventions de nommage

| Type           | Convention                | Exemple                    |
|----------------|---------------------------|----------------------------|
| Dossiers       | kebab-case lowercase      | `my-library/`              |
| Fichiers MDX   | kebab-case lowercase      | `quick-start.mdx`          |
| Fichiers JSON  | kebab-case lowercase      | `openapi-spec.json`        |
| Groupes nav    | Title Case                | `Python Libraries`         |

## Icônes recommandées (Font Awesome)

| Type              | Icône                  |
|-------------------|------------------------|
| API Server        | `server`               |
| GPU/Computing     | `microchip`            |
| Database          | `database`             |
| Cloud/Vercel      | `cloud`                |
| AI/Generation     | `wand-magic-sparkles`  |
| Tools             | `toolbox`              |
| Camera/Vision     | `camera`               |
| Python            | `python`               |
| Web Apps          | `browser`              |
| Monitoring        | `chart-line`           |
| Documentation     | `book`                 |

## Style d'écriture

- Vouvoiement en français
- Titres descriptifs
- Code blocks avec langage spécifié
- Liens relatifs entre pages internes
- Badges pour les dates de mise à jour
- CardGroup pour les liens de navigation
