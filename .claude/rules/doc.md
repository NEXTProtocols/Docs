# Documentation Integration Rules

Cette règle explique comment préparer et envoyer de la documentation vers le repo central NEXTProtocol Docs.

## Détection automatique du type de projet

Quand tu analyses un repo source, identifie son type selon ces critères :

### 1. SDK / Librairie Python
**Critères de détection :**
- Présence de `setup.py`, `pyproject.toml`, ou `setup.cfg`
- Dossier avec `__init__.py`
- Fichier `requirements.txt` ou dépendances poetry/pip

**Structure de doc à générer :**
```
docs/
├── installation.mdx      # pip install, requirements
├── quickstart.mdx        # Premier exemple fonctionnel
├── reference.mdx         # API reference (classes, fonctions)
└── examples/             # Exemples avancés (optionnel)
    └── {example}.mdx
```

**Template installation.mdx :**
```mdx
---
title: 'Installation'
description: 'Comment installer {lib-name}'
---

## Installation

<CodeGroup>
```bash pip
pip install {package-name}
```

```bash poetry
poetry add {package-name}
```
</CodeGroup>

## Requirements

- Python >= {version}
- {dependencies}
```

**Template quickstart.mdx :**
```mdx
---
title: 'Quickstart'
description: 'Premiers pas avec {lib-name}'
---

## Exemple de base

```python
from {package} import {main_class}

# Exemple minimal fonctionnel
{example_code}
```
```

**Template reference.mdx :**
```mdx
---
title: 'API Reference'
description: 'Documentation complète de {lib-name}'
---

## Classes

### `{ClassName}`

{docstring}

**Paramètres :**
| Nom | Type | Description |
|-----|------|-------------|
| {param} | `{type}` | {description} |

**Méthodes :**

#### `{method_name}()`

{method_docstring}
```

---

### 2. API Reference (Backend/Server)
**Critères de détection :**
- Présence de fichiers OpenAPI/Swagger (`openapi.json`, `swagger.yaml`)
- Framework API (FastAPI, Flask, Express, etc.)
- Routes/endpoints définis

**Structure de doc à générer :**
```
docs/
├── openapi.json          # Spec OpenAPI générée/mise à jour
└── endpoints/
    └── {endpoint}.mdx    # Un fichier par endpoint (optionnel)
```

**Template endpoint.mdx :**
```mdx
---
title: '{Endpoint Name}'
openapi: '{METHOD} {/path}'
---

<Badge icon="clock" stroke color="green">{date}</Badge>
<Badge icon="check" stroke color="green">Testé</Badge>

## Description

{description}

## Exemple

<CodeGroup>
```bash curl
curl -X {METHOD} {base_url}{path} \
  -H "Authorization: Bearer $TOKEN" \
  -d '{body}'
```

```python python
import requests

response = requests.{method}(
    "{base_url}{path}",
    headers={"Authorization": f"Bearer {token}"},
    json={body}
)
```
</CodeGroup>
```

---

### 3. Database (Supabase/PostgreSQL)
**Critères de détection :**
- Fichiers SQL de migration
- Dossier `supabase/` ou config Supabase
- Fichiers de schéma (`schema.sql`, `migrations/`)

**Structure de doc à générer :**
```
docs/
├── schema.mdx            # Structure des tables
├── triggers.mdx          # Triggers et leurs actions
├── functions.mdx         # Fonctions PostgreSQL
└── policies.mdx          # RLS policies (si applicable)
```

**Template schema.mdx :**
```mdx
---
title: 'Schema'
description: 'Structure de la base de données {project-name}'
---

## Tables

### `{table_name}`

{description}

| Colonne | Type | Nullable | Default | Description |
|---------|------|----------|---------|-------------|
| {col} | `{type}` | {nullable} | {default} | {desc} |

**Relations :**
- `{column}` → `{other_table}.{other_column}`

**Index :**
- `{index_name}` on `({columns})`
```

**Template triggers.mdx :**
```mdx
---
title: 'Triggers'
description: 'Actions automatiques sur {project-name}'
---

## Triggers

### `{trigger_name}`

**Table :** `{table_name}`
**Event :** `{BEFORE|AFTER} {INSERT|UPDATE|DELETE}`

**Action :**
{description de ce que fait le trigger}

**Code :**
```sql
{trigger_code}
```

**Service appelé :**
- Endpoint: `{url}`
- Payload: `{payload_structure}`
```

---

### 4. Application Web (Frontend)
**Critères de détection :**
- `package.json` avec framework frontend (Next.js, React, Vue, etc.)
- Dossier `pages/` ou `app/` (Next.js)
- Fichiers `.tsx`, `.jsx`, `.vue`

**Structure de doc à générer :**
```
docs/
├── overview.mdx          # Description générale
├── features.mdx          # Fonctionnalités principales
├── architecture.mdx      # Architecture technique (optionnel)
└── deployment.mdx        # Déploiement
```

**Template overview.mdx :**
```mdx
---
title: '{App Name}'
description: '{Short description}'
---

## Présentation

{description}

<CardGroup cols={2}>
  <Card title="Live" icon="globe" href="{production_url}">
    Version production
  </Card>
  <Card title="Repository" icon="github" href="{repo_url}">
    Code source
  </Card>
</CardGroup>

## Stack technique

- **Framework** : {framework}
- **Hosting** : {hosting}
- **UI** : {ui_library}
```

---

### 5. Service/Worker (Backend non-API)
**Critères de détection :**
- Dockerfile ou docker-compose.yml
- Scripts de worker/listener
- Pas de routes HTTP exposées (ou très peu)
- Consomme des events (webhooks, queues, triggers)

**Structure de doc à générer :**
```
docs/
└── {service-name}.mdx
```

**Template service.mdx :**
```mdx
---
title: '{Service Name}'
description: '{Short description}'
---

## Description

{what the service does}

## Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| `{VAR}` | {desc} | `{default}` |

## Events écoutés

| Source | Event | Action |
|--------|-------|--------|
| {source} | {event} | {action} |

## Déploiement

```bash
docker-compose up -d {service}
```
```

---

## Format d'envoi au repo central

Quand tu envoies la doc au repo central, structure le payload ainsi :

```json
{
  "source_repo": "{org}/{repo}",
  "source_type": "sdk|api|database|app|service",
  "source_name": "{nom-kebab-case}",
  "docs_content": "{base64 encoded tar.gz of docs/}",
  "changed_files": ["file1.mdx", "file2.mdx"],
  "metadata": {
    "version": "{version if applicable}",
    "language": "python|typescript|sql|etc"
  }
}
```

## Conventions obligatoires

1. **Nommage fichiers** : kebab-case (`quick-start.mdx`, pas `quickStart.mdx`)
2. **Frontmatter** : Toujours `title` et `description`
3. **Code blocks** : Toujours spécifier le langage
4. **Dates** : Format `DD Month YYYY` (ex: `23 January 2026`)
5. **Langue** : Français avec vouvoiement
6. **Liens** : Relatifs pour les pages internes

## Exemple de workflow complet

```
1. Repo source push sur main
2. GitHub Action détecte changements dans le code
3. Claude analyse le repo et identifie le type
4. Claude génère/met à jour la doc dans docs/
5. Claude envoie au repo central via repository_dispatch
6. Repo central reçoit et intègre la doc
```
