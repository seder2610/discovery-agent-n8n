# Stack prospection SEA — 3 workflows n8n

> Base Notion CRM : `ad2b7af7-d0a6-4cb9-a431-360b4c304bf6`  
> **Ton job** : générer les messages (Claude à la demande) → envoyer LinkedIn → statut **Envoyé**.  
> **Automatique** : dates (Planning) + files relance J+2/J+5 (CRON).

---

## Les 3 workflows

| Fichier | Quand | Rôle |
|---------|-------|------|
| `discovery-agent-v2.json` **v2.5** | 1×/jour ouvré (manuel) | Serper + ICP Claude → fiche Notion **Qualifié** (sans messages) |
| `planning-semaine.json` | Dimanche soir | 10 prospects/jour ouvré + `Date envoi` / `Date J+2` / `Date J+5` |
| `relance-cron.json` | CRON lun–ven 8h | Statut → **J+2 — À relancer** / **J+5 — À relancer** |

**Messages LinkedIn** : hors n8n → voir `prompt_messages_batch.md` (1 appel Claude pour 10 prospects).

---

## Chaîne complète

```
Discovery (matin)     →  Statut Qualifié (nom, agence, douleur, LinkedIn)
Toi (avant envoi)     →  Claude batch → champ Message → Prêt à envoyer
Planning (dimanche)   →  Dates lun→ven (10/j)
Toi (lun→ven)         →  Envoie M1 → Envoyé
CRON                  →  J+2 / J+5 — À relancer
Toi                   →  Relances → Relance J+2 envoyée / Relance J+5 envoyée
```

---

## Coûts estimés (50 prospects / semaine)

| Poste | Avant (messages auto) | Maintenant |
|-------|----------------------|------------|
| **Serper** | ~40 crédits/sem | ~40 crédits/sem |
| **Claude ICP** | ~5 runs × 1 batch | ~5 × ~0,05–0,15 € |
| **Claude messages** | ~50 × ~0,03 € ≈ **1,50 €/sem** | **~0,20 €/sem** (1 batch de 10, 5×/sem) ou moins si 1 batch pour la semaine |
| **Total Claude** | ~**2–5 €/sem** | ~**0,30–1 €/sem** |

Tu ne paies les messages **que pour les listes que tu rédiges**.

---

## Statuts Notion

| Statut | Qui |
|--------|-----|
| **Qualifié** | Discovery (ICP OK, pas encore de copy) |
| **Prêt à envoyer** | Toi après collage des messages |
| **À relire** | Toi si copy à retravailler |
| **Envoyé** | Toi après M1 LinkedIn |
| **J+2 — À relancer** | CRON |
| **Relance J+2 envoyée** | Toi |
| **J+5 — À relancer** | CRON |
| **Relance J+5 envoyée** | Toi |

**Statut** = colonne Notion type **Select** (pas Status). Option **Qualifié** en violet en 1ère position.

---

## Setup

1. **Notion API** → 3 workflows  
2. **Serper** + **Claude** → Discovery uniquement (ICP seulement)  
3. Remplacer `REMPLACER_ID_NOTION_CREDENTIAL`  
4. Activer **relance-cron** une fois  

---

## Vues Notion

| Vue | Filtre |
|-----|--------|
| **À rédiger** | Statut = Qualifié |
| **Aujourd'hui — M1** | Date envoi = aujourd'hui · Prêt à envoyer |
| **Relances J+2** | J+2 — À relancer |
| **Relances J+5** | J+5 — À relancer |

---

## v2.5 (mai 2026)

- Suppression génération messages dans n8n (économie Claude)
- Création directe en **Qualifié** (`Statut|select`)
- API Notion : `select` / `selectValue` partout (Discovery, Planning, Relance)
- Prompt batch : `prompt_messages_batch.md`
- Planning inclut les prospects **Qualifié**
