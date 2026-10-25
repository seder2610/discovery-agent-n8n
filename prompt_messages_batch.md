# Prompt batch — Messages LinkedIn (10 prospects)

> 1 appel Claude.ai pour 10 prospects (~0,10–0,20 €) au lieu de 10 appels n8n séparés.

---

## 1. Exporter depuis Notion

Vue filtrée : **Statut = Qualifié**.

Copie pour chaque prospect : Nom, Agence, Rôle, Douleur, (optionnel : snippet Notes)

```
1. Jean Dupont | Agence X | Fondateur | P-MON — Monitoring
2. ...
```

---

## 2. Prompt à coller dans Claude.ai

```
Tu génères des messages LinkedIn de prospection pour Sedera, freelance automation n8n spécialisé agences SEA françaises.

POSITIONNEMENT SEDERA
"Je construis le système que t'as pas le temps de construire toi-même."
Services : reporting Google Ads automatique, alertes budget, onboarding client, monitoring comptes.
Prix de départ : 800€ le workflow. Résultat concret : les agences récupèrent 5-10h/semaine.

RÈGLES IMPÉRATIVES
- Première phrase = accroche sur leur réalité (jamais "je me permets" ou "j'espère que vous allez bien")
- Phrases courtes 5-12 mots, registre oral : tu/t'as/ça/du coup/franchement
- Zéro liste à puces dans les messages
- Zéro mots IA : synergies, optimiser, pertinent, valeur ajoutée, n'hésitez pas, cordialement
- 1 seul CTA court : "T'as 5 min ?" / "Ça t'intéresse ?" / "Je peux te montrer ça."
- Signature : — Sedera

LIMITES STRICTES (compter les caractères)
- Objet InMail : 6 mots max, accrocheur, pas corporate
- M1 : ≤ 500 caractères
- M2 (relance J+2) : ≤ 280 caractères, angle différent de M1, jamais répéter
- M3 (relance J+5) : ≤ 200 caractères, fermeture douce, porte ouverte

FORMAT DE RÉPONSE — JSON strict
[
  {
    "nom": "Jean Dupont",
    "agence": "Agence X",
    "objet_inmail": "",
    "m1": "",
    "m2": "",
    "m3": ""
  }
]

PROSPECTS :
[COLLER TA LISTE ICI]
```

---

## 3. Après génération

Pour chaque fiche Notion :

1. Coller **objet_inmail** → champ `Objet InMail`
2. Coller **m1** → champ `M1`
3. Coller **m2** → champ `M2`
4. Coller **m3** → champ `M3`
5. Passer statut → **Prêt à envoyer**
6. Lancer le workflow **Planning Semaine** (n8n) → dates assignées
7. Envoyer M1 sur LinkedIn le jour J → statut → **Envoyé**
8. Le CRON gère le reste (J+2 / J+5)

---

## Douleurs → angles messages

| Douleur | Ce que tu règles | Accroche typique |
|---------|-----------------|------------------|
| **P-MON — Monitoring** | Budget Ads qui brûle le week-end sans alerte | "Tes comptes Google Ads — qui les surveille le samedi soir ?" |
| **P08 — Reporting** | 5h/mois sur des rapports que personne ne lit | "Tes rapports clients, tu les fais encore à la main ?" |
| **P-ONB — Onboarding** | L'arrivée d'un nouveau client = chaos manuel | "L'onboarding de ton dernier client, ça a pris combien de jours ?" |
| **P11 — Coordination** | Emails en boucle entre chef de projet, client, équipe | "Combien d'emails pour synchroniser ton équipe sur un seul client ?" |
