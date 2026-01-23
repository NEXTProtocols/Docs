# Sources Documentation

Ce dossier contient les fichiers de documentation bruts provenant des différents repos.

## Convention de nommage

```
doc.{repo-name}
```

Exemples :
- `doc.apiserver` - Documentation du repo APIServer
- `doc.my-sdk` - Documentation d'une librairie Python
- `doc.supabase-prod` - Documentation d'un projet Supabase

## Workflow

1. Copier/coller le contenu du fichier doc depuis le repo source
2. Demander à Claude d'intégrer les modifications
3. Claude compare avec git et adapte la doc globale

## Format attendu

Chaque fichier `doc.*` doit suivre les conventions définies dans `.claude/rules/doc.md`
