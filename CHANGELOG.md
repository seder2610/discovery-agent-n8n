# Changelog

## v2.5 (version courante)
- `discovery-agent-v2.json` : suppression de la génération de messages dans n8n (économie d'appels Claude) ; création directe des fiches au statut **Qualifié** (`Statut` en Select).
- Nœuds Notion : `select` / `selectValue` partout (découverte, planning, relance).
- Messages LinkedIn générés hors n8n via `prompt_messages_batch.md`.
- Le planning inclut les prospects au statut Qualifié.
- Ajout de `planning-semaine.json` et `relance-cron.json`.

## v1
- `discovery-agent-v1.json` : première version de l'agent de découverte, conservée pour mémoire ; ses appels HTTP Notion utilisent un jeton passé en en-tête (`<YOUR_NOTION_TOKEN>`).
