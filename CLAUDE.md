# CLAUDE.md - Documentation Integration

Tu es l'intégrateur de documentation pour NEXTProtocol.

## Workflow d'intégration

```
1. L'utilisateur copie un fichier doc.{repo-name} dans _sources/
2. L'utilisateur te signale les modifications
3. Tu compares avec git diff pour voir les changements
4. Tu adaptes la documentation globale en conséquence
```

## Commandes utiles

```bash
# Voir les fichiers sources modifiés
git diff --name-only _sources/

# Voir le détail des modifications d'un fichier source
git diff _sources/doc.{repo-name}

# Voir les modifications non commitées
git status
```

## Structure de la documentation

```
├── docs.json                      # Configuration Mintlify et navigation
├── CLAUDE.md                      # Ce fichier
│
├── _sources/                      # Fichiers doc bruts des repos
│   ├── doc.apiserver              # → api-reference/apiserver/
│   ├── doc.{sdk-name}             # → sdk/{sdk-name}/
│   ├── doc.{db-name}              # → database/{db-name}/
│   └── ...
│
├── api-reference/                 # Tab: API Reference
│   ├── apiserver/
│   ├── runpod/
│   ├── vercel/
│   └── checkface/
│
├── sdk/                           # Tab: SDK
│   └── {lib-name}/
│
├── database/                      # Tab: Database
│   └── {project-name}/
│
├── apps/                          # Tab: Apps
│   └── {app-name}/
│
├── infrastructure/                # Tab: Infrastructure
│   ├── services/
│   └── monitoring/
│
├── benchmark/
├── ai-tools/
└── snippets/
```

## Mapping sources → destinations

| Fichier source | Type détecté | Destination |
|----------------|--------------|-------------|
| `doc.apiserver` | api | `api-reference/apiserver/` |
| `doc.{name}-sdk` | sdk | `sdk/{name}/` |
| `doc.{name}-db` | database | `database/{name}/` |
| `doc.{name}-app` | app | `apps/{name}/` |
| `doc.{name}-service` | service | `infrastructure/services/` |

## Processus d'intégration

### 1. Réception d'un nouveau fichier source

Quand l'utilisateur ajoute/modifie un fichier dans `_sources/` :

```bash
# Voir ce qui a changé
git diff _sources/doc.{name}
```

### 2. Analyse du contenu

Identifier le type de documentation :
- **SDK** : Classes, fonctions, installation pip
- **API** : Endpoints HTTP, OpenAPI
- **Database** : Tables, triggers, functions SQL
- **App** : Features, deployment, stack
- **Service** : Config, events, workers

### 3. Adaptation

- Créer/mettre à jour les fichiers MDX dans le bon dossier
- Adapter au format Mintlify (frontmatter, components)
- Mettre à jour `docs.json` si nouvelles pages

### 4. Mise à jour navigation

Si nouvelles pages ajoutées, modifier `docs.json` :
```json
{
  "group": "...",
  "pages": [
    "path/to/new-page"
  ]
}
```

## Format des fichiers MDX

```mdx
---
title: 'Titre de la page'
description: 'Description courte'
---

## Section

Contenu...

<CodeGroup>
```python Python
code example
```
</CodeGroup>
```

## Conventions

- Fichiers : kebab-case (`quick-start.mdx`)
- Dossiers : kebab-case lowercase
- Langue : Français avec vouvoiement
- Code blocks : Toujours spécifier le langage
- Liens : Relatifs pour les pages internes
