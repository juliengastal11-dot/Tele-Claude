# Côté Claude/Anthropic d'un compagnon Telegram personnel : API et Agent SDK (capacités, prix), connecteurs Gmail/Agenda/Drive/Notion, CGU des abonnements, données personnelles (RGPD). État au 28 septembre 2026

> **Méthode et fiabilité.** Date de référence : 28/09/2026. Sauf mention contraire, les prix sont en **USD hors taxes**. Les pages officielles ont été lues **en entier** via WebFetch le 28/09/2026 : platform.claude.com, code.claude.com, claude.com, support.claude.com, anthropic.com/legal, github.com.
>
> Le proxy de l'environnement **bloquait** plusieurs domaines : techcrunch.com, venturebeat.com, theregister.com, thenewstack.io, gigazine.net, winbuzzer.com, alternativeto.net, cnil.fr, developers.google.com, support.google.com, notion.com, developers.notion.com, privacy.claude.com et news.ycombinator.com. Pour ces sources, je n'ai pu lire que les **extraits ou résumés du moteur de recherche**. Elles sont marquées « *(extrait de recherche)* » et sont moins fiables : l'attribution d'une phrase à un article précis peut être approximative.
>
> Les pages de doc Anthropic ne sont pas datées. Elles sont citées « consultée le 28/09/2026 ». Les articles d'aide marqués « Updated this week » ont été mis à jour la semaine du 21 au 28/09/2026.

## 1. Gamme de modèles Claude et prix API (entrée/sortie, cache, batch), modèle adapté à un assistant, audio natif, outils web

### Takeaway
Au 28/09/2026, la gamme « actuelle » de l'API compte quatre modèles :
- **Claude Fable 5.1** : 10 $ en entrée / 50 $ en sortie par million de tokens.
- **Claude Opus 5.5** : 4 $ / 20 $.
- **Claude Sonnet 5** : 2 $ / 10 $. Ce tarif est désormais permanent.
- **Claude Haiku 4.5** : 1 $ / 5 $.

Le Batch API donne −50 %. La lecture de cache coûte 0,1× le prix d'entrée (0,05× sur Opus 5.5, 0,025× sur Fable 5.1).

**Aucun modèle n'accepte l'audio en entrée** : texte et image seulement, en sortie du texte. La recherche web coûte 10 $ pour 1 000 recherches ; le web fetch n'a pas de surcoût au-delà des tokens.

Pour un assistant conversationnel qui appelle des outils, **Sonnet 5 en effort « low » ou « medium »** est le compromis coût/qualité le plus défendable. Haiku 4.5 est le moins cher, mais son retrait est possible dès le **15/10/2026**.

### Cited Findings
**Gamme actuelle et caractéristiques**
- Anthropic recommande : « If you're unsure which model to use, start with Claude Opus 5.5 for most workloads. Use Claude Fable 5.1 for demanding reasoning and long-horizon agentic work » — [Anthropic, docs « Models overview » (consultée le 28/09/2026)](https://platform.claude.com/docs/en/about-claude/models/overview)
- Identifiants API : `claude-fable-5-1`, `claude-opus-5-5`, `claude-sonnet-5`, `claude-haiku-4-5-20251001` (alias `claude-haiku-4-5`). — [Anthropic, « Models overview »](https://platform.claude.com/docs/en/about-claude/models/overview)
- Descriptions officielles :
  - Fable 5.1 : « demanding reasoning and long-horizon agentic work » ; latence « Slower ».
  - Opus 5.5 : « long-running agentic coding and knowledge work » ; latence « Moderate ».
  - Sonnet 5 : « The best combination of speed and intelligence » ; latence « Fast ».
  - Haiku 4.5 : « The fastest model with near-frontier intelligence » ; latence « Fastest ».

  — [Anthropic, « Models overview »](https://platform.claude.com/docs/en/about-claude/models/overview)
- Caractéristiques par modèle :

  | | Fable 5.1 | Opus 5.5 | Sonnet 5 | Haiku 4.5 |
  |---|---|---|---|---|
  | Contexte | 1M | 1M | 1M | 200K |
  | Sortie max | 128K | 128K | 128K | 64K |
  | Coupure de connaissances fiable | juin 2026 | juin 2026 | janv. 2026 | févr. 2025 |
  | Réflexion | adaptive, toujours active | adaptive, toujours active | adaptive | « Extended » |
  | Effort par défaut | `high` | `medium` | `high` | non supporté |
  | Retrait « not sooner than » | 01/09/2027 | 22/09/2027 | 30/06/2027 | **15/10/2026** |

  — [Anthropic, « Models overview »](https://platform.claude.com/docs/en/about-claude/models/overview)
- Modèles « legacy » encore disponibles : Fable 5, Opus 5, Opus 4.8, 4.7, 4.6, 4.5, Sonnet 4.6, 4.5. — [Anthropic, « Models overview »](https://platform.claude.com/docs/en/about-claude/models/overview)
- **Audio** : « All current models support text and image input, text output, multilingual capabilities, vision, and tool use. » Aucune modalité audio n'est listée. — [Anthropic, « Models overview »](https://platform.claude.com/docs/en/about-claude/models/overview)
  - Source secondaire concordante : « no Claude model accepts audio files… there is no audio input type ». — [ParseJet, « Can Claude Transcribe Audio? What Works in 2026 » *(extrait de recherche, date non vérifiée)*](https://parsejet.com/guides/can-claude-transcribe-audio/)

**Prix par million de tokens (MTok), en USD** — [Anthropic, docs « Pricing » (consultée le 28/09/2026 ; « All prices are in USD »)](https://platform.claude.com/docs/en/about-claude/pricing)

| Modèle | Entrée | Écriture cache 5 min | Écriture cache 1 h | Lecture cache | Sortie | Batch entrée/sortie |
|---|---|---|---|---|---|---|
| Fable 5.1 | 10 $ | 12,50 $ | 20 $ | 0,25 $ | 50 $ | 5 $ / 25 $ |
| Opus 5.5 | 4 $ | 5 $ | 8 $ | 0,20 $ | 20 $ | 2 $ / 10 $ |
| Opus 5 / 4.8 / 4.7 / 4.6 / 4.5 | 5 $ | 6,25 $ | 10 $ | 0,50 $ | 25 $ | 2,50 $ / 12,50 $ |
| **Sonnet 5** | **2 $** | 2,50 $ | 4 $ | 0,20 $ | **10 $** | 1 $ / 5 $ |
| Sonnet 4.6 / 4.5 | 3 $ | 3,75 $ | 6 $ | 0,30 $ | 15 $ | 1,50 $ / 7,50 $ |
| **Haiku 4.5** | **1 $** | 1,25 $ | 2 $ | 0,10 $ | **5 $** | 0,50 $ / 2,50 $ |

- **Sonnet 5, changement 2026** : « The $2/$10 per million input/output token pricing for Claude Sonnet 5, announced at launch as introductory pricing through August 31, 2026, is now the standard price. The previously scheduled increase to $3/$15 … on September 1, 2026 will not occur. » — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Cache** :
  - Écriture 5 min = 1,25× l'entrée ; écriture 1 h = 2× ; lecture = 0,1× (0,05× sur Opus 5.5, 0,025× sur Fable 5.1 et Mythos 5.1).
  - « caching pays off after one cache read for the 5-minute duration … or after two cache reads for the 1-hour duration ».
  - Ces multiplicateurs se cumulent avec le Batch et la résidence des données.
  - Il existe un mode de « cache automatique » : un seul champ `cache_control` au niveau de la requête.

  — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Batch** : « 50% discount on both input and output tokens ». Le traitement est asynchrone. — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Tokenizer** : « Claude 4.7 and later models … use a newer tokenizer … approximately 30% more tokens for the same text … Claude Sonnet 4.6 and earlier models use the previous tokenizer. » — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Contexte long** : les modèles 4.6 et suivants incluent le contexte de 1M tokens au tarif standard. — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Surcoût « tool use »** : prompt système ajouté automatiquement dès qu'un outil est fourni.
  - Opus 5.5 : 286 tokens.
  - Sonnet 5 : 354 tokens (`auto`/`none`) ou 474 (`any`/`tool`).
  - Haiku 4.5 : 496 / 588.

  — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Recherche web** : « $10 per 1,000 searches, plus standard token costs for search-generated content ». Une recherche en erreur n'est pas facturée. — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Web fetch** : « no additional cost », seuls les tokens du contenu récupéré sont payés. Ordres de grandeur :
  - page web moyenne (10 kB) ≈ 2 500 tokens ;
  - PDF de 500 kB ≈ 125 000 tokens ;
  - le paramètre `max_content_tokens` permet de plafonner.

  — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Exécution de code** : gratuite si utilisée avec `web_search_20260209` ou `web_fetch_20260209` (ou versions ultérieures). Sinon, 1 550 heures gratuites par mois et par organisation, puis 0,05 $ par heure et par conteneur. — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Fast mode** (research preview) sur Opus 5.5 : 8 $ / 40 $. — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Facturation API** :
  - « All payments are in USD » ; facturation à l'usage mensuel.
  - « New users receive a small amount of free credits ».
  - Paliers de limites de débit : Start, Build, Scale.

  — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Conseil officiel** : « Choose Haiku for simple tasks, Sonnet for most production workloads, and Opus for the most complex reasoning ». — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Effort sur Sonnet 5** :
  - « Low effort: For high-volume or latency-sensitive workloads. Suitable for chat and non-coding use cases where faster turnaround is prioritized. »
  - « Medium effort: Cost-saving step-down … Comparable to Claude Sonnet 4.6 at high effort. »
  - L'effort s'applique à tous les tokens de sortie, y compris la réflexion (thinking).
  - `max_tokens` est « a hard limit on total output (thinking plus response text) ».

  — [Anthropic, docs « Effort » (consultée le 28/09/2026)](https://platform.claude.com/docs/en/build-with-claude/effort)
- **Opus 5.5** : « Adaptive thinking is always on and can't be turned off, so effort is the primary control for how much the model reasons and what a request costs ». Effort par défaut : `medium`. — [Anthropic, « Effort »](https://platform.claude.com/docs/en/build-with-claude/effort)
- **Haiku 4.5 et `inference_geo`** : Haiku 4.5 ne supporte pas `inference_geo` (erreur 400). Seuls les modèles 4.6 et suivants le supportent. — [Anthropic, docs « Data residency »](https://platform.claude.com/docs/en/manage-claude/data-residency)
- **Covered Models** : Fable 5.1, Mythos 5.1, Fable 5 et Mythos 5 sont des « Covered Models ». Ils exigent une rétention de 30 jours et sont incompatibles avec le zéro-rétention (ZDR) sauf autorisation expresse. — [Anthropic, docs « API and data retention »](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention)
- **Claude Managed Agents** (harnais hébergé par Anthropic) : tokens au tarif standard + « $0.08 per session-hour » de temps d'exécution. — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)

### Inferences
- **Choix du modèle pour l'assistant** : des échanges courts en français, avec 1 à 3 appels d'outils (mails, agenda, notes).
  - Sonnet 5 à effort `low` ou `medium` est le choix par défaut raisonnable : prix moitié moindre qu'Opus 5.5, contexte 1M, retrait pas avant 06/2027.
  - Opus 5.5 est à réserver aux tâches complexes : rédaction délicate, planification multi-étapes. Il coûte 2× Sonnet 5 en entrée et en sortie, mais son cache est proportionnellement moins cher (0,05×).
  - Haiku 4.5 convient à des sous-tâches (tri, classification, résumé). Il n'est pas recommandé comme modèle principal : retrait possible après le 15/10/2026, connaissances arrêtées à févr. 2025, pas d'`inference_geo` ni d'`effort`.
  - Fable 5.1 est surdimensionné (10 $/50 $) et impose une rétention de 30 jours.
- **Comparaison entre modèles** : comparer les coûts « au token » est trompeur. Sonnet 5 et Opus 5.5 utilisent le nouveau tokenizer, qui produit environ +30 % de tokens pour un même texte ; Haiku 4.5 utilise l'ancien. Il faut mesurer avec l'endpoint de comptage de tokens sur des échanges réels en français.
- **Messages vocaux** : les messages vocaux Telegram doivent être **transcrits avant l'envoi à Claude**. C'est une brique STT hors Claude, couverte par l'autre chercheur.
- **Coût des outils web** : web fetch et recherche web sont négligeables à l'échelle d'un seul utilisateur (1 centime par recherche). Le vrai coût est dans les tokens des pages injectées dans le contexte.

### Gaps
- Aucune source officielle ne compare les modèles sur des tâches d'assistant personnel en français. Seules les descriptions qualitatives sont disponibles.
- Il n'existe pas de chiffre officiel « tokens par mot » pour le français (la page ne donne que l'anglais : ~4 caractères ou 0,75 mot par token).
- Le rattachement de Sonnet 5 au « nouveau tokenizer » est déduit de la formule « Claude 4.7 and later models ». Sonnet 5 n'est pas nommé explicitement.
- Non vérifié / je ne sais pas : l'existence d'une bêta privée d'entrée audio. Rien trouvé.

## 2. Prix des abonnements grand public (Pro, Max 5x, Max 20x) en France/EUR et « extra usage »

### Takeaway
Les prix officiels vérifiables sont en **USD hors taxes** :
- Pro : 20 $/mois, ou 17 $/mois en annuel (200 $/an).
- Max 5x : 100 $/mois.
- Max 20x : 200 $/mois.
- Team : minimum 2 membres, siège Standard à 25 $/mois (20 $ en annuel).

**Je n'ai pas pu vérifier de prix en euros sur une page officielle.** La page française consultée depuis ce proxy affichait des dollars.

L'« extra usage », renommé **« usage credits »**, permet de continuer au-delà des limites du forfait. C'est du **paiement à l'usage prépayé, aux tarifs API standard**.

### Cited Findings
- **Page tarifs (vue depuis une sortie réseau US)** :
  - Pro « $20/month » ou « $17/month ($200 billed upfront) » en annuel.
  - Max 5x et Max 20x affichés « From $100/month » (le palier 20x n'est pas détaillé dans l'extrait).
  - Team Standard « $20/seat/month » annuel (« $25 monthly ») ; Team Premium « $100 » annuel (« $125 monthly »).
  - Enterprise « $20/seat/month, billed annually. Usage cost scales with model and task ».
  - « Prices shown don't include applicable tax ».

  — [Anthropic, claude.com/pricing (consultée le 28/09/2026)](https://claude.com/pricing)
- **Page en français** : claude.com/fr-fr/pricing affichait en dollars (« 20 $ », « 17 $ », « À partir de $100 ») avec la mention « Les prix affichés n'incluent pas les taxes applicables. » — [Anthropic, claude.com/fr-fr/pricing (consultée le 28/09/2026)](https://claude.com/fr-fr/pricing)
- **Offre Pro** :
  - « $20 per month (US), with pricing in your local currency where supported » ;
  - « Monthly pricing varies by region, and some regions include applicable taxes in the displayed price while others add tax at checkout » ;
  - Pro inclut « Claude Code access », des limites par session qui « reset every five hours » et une « Weekly usage limit » ;
  - « The Pro plan does not include API usage through the Claude Console ».

  — [Claude Help Center, « What is the Pro plan? » (MAJ semaine du 28/09/2026)](https://support.claude.com/en/articles/8325606-what-is-the-pro-plan)
- **Offre Max** :
  - « Max 5x: $100 per month » et « Max 20x: $200 per month ».
  - 5× ou 20× l'allocation par session du Pro, reset toutes les 5 h, plus des limites hebdomadaires. Accès à Claude Code inclus.
  - « Price and plans are subject to change at Anthropic's discretion. »

  — [Claude Help Center, « What is the Max plan? » (MAJ semaine du 28/09/2026)](https://support.claude.com/en/articles/11049741-what-is-the-max-plan)
- **Offre Team** :
  - « Team plans require a minimum of two members ».
  - Standard : 25 $ par membre et par mois en mensuel, 20 $ en annuel. Premium : 125 $ / 100 $.
  - « Prices shown are for US customers and exclude applicable taxes ». Claude Code inclus.

  — [Claude Help Center, « What is the Team plan? » (MAJ semaine du 28/09/2026)](https://support.claude.com/en/articles/9266767-what-is-the-team-plan)
- **Usage credits (ex-« extra usage »)** : l'article s'intitule désormais « Manage usage credits for paid Claude plans ».
  - Définition : « Usage credits allow individuals subscribed to paid Claude plans (Pro, Max 5x, and Max 20x) to continue using Claude seamlessly after reaching their included usage limits », « billed at standard API rates ».
  - Mode de fonctionnement : prépaiement (« Add funds »), plafond mensuel, rechargement automatique, limite de consommation de 2 000 $ par jour.
  - Périmètre : « Usage credits apply to both Claude conversations and Claude Code terminal usage ». Si l'abonnement a été pris via une app mobile, l'activation se fait sur le web.

  — [Claude Help Center, « Manage usage credits for paid Claude plans » (MAJ semaine du 28/09/2026)](https://support.claude.com/en/articles/12429409-manage-extra-usage-for-paid-claude-plans)
- **Avril 2026, extra usage et harnais tiers** : l'usage des harnais tiers a basculé sur l'extra usage (voir Q3). Anthropic a offert « a month of extra usage credit based on their monthly plan » et des lots d'extra usage « at 30 percent off ». — [The Register, « Anthropic closes door on subscription use of OpenClaw », 06/04/2026 *(extrait de recherche)*](https://www.theregister.com/2026/04/06/anthropic_closes_door_on_subscription/)
- **Prix en euros (non officiel, non vérifié)** : un blog affirme que « depuis août 2026, la page de tarifs d'Anthropic affiche directement des prix en euros pour la France : 18 €/mois pour Claude Pro (15 € en annuel) ». Il ajoute qu'« au 20/09/2026, la page française s'arrête à "À partir de 90 € par mois" pour Max ». — [geotoolbox.ai, « Prix de Claude en 2026 : formules, tarifs en euros et API » *(extrait de recherche)*](https://geotoolbox.ai/blog/claude-pricing). **Contredit** par ma lecture de [claude.com/fr-fr/pricing](https://claude.com/fr-fr/pricing), qui affichait des USD. L'affichage est probablement géolocalisé ; non tranché.
- **TVA (non officiel)** : des sources tierces indiquent que saisir un numéro de TVA intracommunautaire valide dans le portail de facturation Stripe fait passer les factures à 0 % avec la mention d'autoliquidation. — [Fazm, « Anthropic VAT: Tax Charges on Claude Pro, Team, and API Billing » *(extrait de recherche)*](https://fazm.ai/blog/anthropic-vat)
  - Un ticket GitHub signale un numéro de TVA européen qui ne s'enregistre pas et une TVA de 20 % facturée à tort. — [GitHub anthropics/claude-code, issue #34561 *(titre vu en recherche)*](https://github.com/anthropics/claude-code/issues/34561)

### Inferences
- **Particulier ou entreprise sans numéro de TVA** : il faut s'attendre au prix de liste (USD, ou EUR local « where supported ») **+ TVA française de 20 %** selon la région. Pour une entreprise assujettie qui renseigne son numéro de TVA, l'autoliquidation est plausible mais **non confirmée par une source Anthropic**.
- **Au-delà des limites du forfait**, l'abonnement ne procure plus d'avantage de prix : les usage credits sont facturés au tarif API. Il faut comparer le forfait au coût API du scénario (voir Q7).
- **Team pour un seul dirigeant** : 2 sièges minimum, soit 40 à 50 $ HT par mois. En contrepartie, on bénéficie des **Commercial Terms** (voir Q6), ce que Pro et Max n'offrent pas.

### Gaps
- **Non vérifié / je ne sais pas** : prix officiels en EUR pour la France, TVA incluse ou non dans l'affichage français, et existence d'un prix Max 20x distinct en EUR.
- Les limites d'usage Pro et Max ne sont pas publiées en tokens ni en messages. Impossible de dire si 30 échanges outillés par jour tiennent dans un Pro.
- Non vérifié : traitement TVA officiel (autoliquidation B2B) sur les factures Anthropic.

## 3. Politique d'Anthropic sur l'usage des abonnements Free/Pro/Max (OAuth / login claude.ai) dans des outils tiers, l'Agent SDK, `claude -p` et Channels : chronologie 2026 et état actuel

### Takeaway
Au 28/09/2026, la documentation officielle **autorise** un abonné à utiliser **son propre abonnement** avec le **binaire Claude Code non modifié** : en interactif, en headless (`claude -p`), avec **Channels** (dont le plugin **Telegram officiel**) et avec un jeton `claude setup-token` « for CI pipelines, scripts ». L'**Agent SDK** pour son propre usage est aussi admis. Cet usage **est décompté des limites de l'abonnement** : la séparation annoncée pour le 15/06/2026 a été suspendue.

Restent **interdits** pour un développeur tiers :
- proposer le login claude.ai dans son application ;
- faire passer les requêtes d'autres personnes par des identifiants Free/Pro/Max ;
- collecter, stocker ou relayer des identifiants claude.ai ;
- revendre le service ;
- tout accès automatisé qu'Anthropic n'a pas « explicitement permis ».

La politique a changé au moins **5 fois en 2026** (janvier, février, 4 avril, 13 mai, 15 juin). Le risque de revirement est donc élevé.

### Cited Findings
**Base contractuelle (antérieure à 2026, toujours en vigueur)**
- Les Consumer Terms (en vigueur le 08/10/2025) interdisent : « Except when you are accessing our Services via an Anthropic API Key or where we otherwise explicitly permit it, to access the Services through automated or non-human means, whether through a bot, script, or otherwise ». C'est l'item 7 de la section 3. — [Anthropic, Consumer Terms of Service (effective 08/10/2025)](https://www.anthropic.com/legal/consumer-terms)
- The Register indique que cette clause (« Section 3.7 ») interdit les harnais tiers non autorisés « since at least February 2024 », texte inchangé. — [The Register, « Anthropic clarifies ban on third-party tool access to Claude », 20/02/2026 *(extrait de recherche)*](https://www.theregister.com/2026/02/20/anthropic_clarifies_ban_third_party_claude_access/)

**Chronologie 2026**
- **Janvier 2026, blocage technique** :
  - Anthropic « confirmed the implementation of strict new technical safeguards preventing third-party applications from spoofing its official coding client, Claude Code ». Cela a perturbé notamment OpenCode, qui « spoof[s] the client identity ».
  - Thariq Shihipar (Anthropic) « cited technical instability as the primary driver ».
  - Réaction de DHH : « Seems very customer hostile ».

  — [VentureBeat, « Anthropic cracks down on unauthorized Claude usage by third-party harnesses and rivals », janvier 2026 *(extrait de recherche, date exacte non vérifiée)*](https://venturebeat.com/technology/anthropic-cracks-down-on-unauthorized-claude-usage-by-third-party-harnesses)
  - La date du « 9 janvier » (vérifications côté serveur contre OpenCode, Cline, RooCode) et des bannissements automatiques de comptes ensuite annulés proviennent d'un résumé de recherche agrégeant des sources secondaires. **Non vérifié.** — [paddo.dev, « Anthropic's Walled Garden: The Claude Code Crackdown » *(extrait, attribution approximative)*](https://paddo.dev/blog/anthropic-walled-garden-crackdown/)
- **19–20 février 2026, doc révisée** : Anthropic a précisé par écrit que les jetons OAuth Free, Pro et Max sont réservés à Claude Code et Claude.ai.
  - GIGAZINE rapporte : « use of these tokens with any other products, tools, or services, including the Agent SDK, is unauthorized and a violation of its consumer terms of service ». — [GIGAZINE, « Anthropic officially bans third-party subscription authentication », 20/02/2026 *(extrait de recherche)*](https://gigazine.net/gsc_news/en/20260220-anthropic-third-party-block/)
  - The Register confirme : « including the Agent SDK — is not permitted and constitutes a violation of the Consumer Terms of Service ». OpenCode a retiré le support Pro/Max en citant « anthropic legal requests ». — [The Register, 20/02/2026 *(extrait)*](https://www.theregister.com/2026/02/20/anthropic_clarifies_ban_third_party_claude_access/)
  - Voir aussi [Winbuzzer, « Anthropic Bans Claude Subscription OAuth in Third-Party Apps », 19/02/2026 *(extrait)*](https://winbuzzer.com/2026/02/19/anthropic-bans-claude-subscription-oauth-in-third-party-apps-xcxwbn/).
- **Mars 2026 (date non vérifiée), lancement de Claude Code Channels** : VentureBeat titre « Anthropic just shipped an OpenClaw killer called Claude Code Channels, letting you message it over Telegram and Discord ». — [VentureBeat *(titre vu en recherche ; date non vérifiée)*](https://venturebeat.com/orchestration/anthropic-just-shipped-an-openclaw-killer-called-claude-code-channels)
- **4 avril 2026, midi (heure du Pacifique)** : les abonnés ne peuvent plus « use your Claude subscription limits for third-party harnesses including OpenClaw ». Ils doivent payer l'extra usage, « a pay-as-you-go option billed separately from your subscription ».
  - Boris Cherny (responsable de Claude Code) : « subscriptions weren't built for the usage patterns of these third-party tools ».

  — [TechCrunch, « Anthropic says Claude Code subscribers will need to pay extra for OpenClaw usage », 04/04/2026 *(extrait de recherche)*](https://techcrunch.com/2026/04/04/anthropic-says-claude-code-subscribers-will-need-to-pay-extra-for-openclaw-support/)
  - Position d'Anthropic rapportée : « Using Claude subscriptions with third-party tools isn't permitted under our Terms of Service, and they put an outsized strain on our systems ».
  - Les outils tiers restent utilisables via l'extra usage ou une clé API. Compensation : un mois de crédit, lots à −30 %.

  — [The Register, 06/04/2026 *(extrait)*](https://www.theregister.com/2026/04/06/anthropic_closes_door_on_subscription/)
- **13 mai 2026, annonce d'un crédit mensuel « programmatique »** à partir du 15/06.
  - Le crédit devait couvrir « Claude Agent SDK – claude -p – Claude Code GitHub Actions – Third-party apps built on the Agent SDK ». — [ClaudeDevs sur X, ~13/05/2026 *(titre du post vu en recherche)*](https://x.com/ClaudeDevs/status/2054610152817619388)
  - VentureBeat y voit une « major reversal » : les harnais tiers comme OpenClaw redeviennent utilisables, mais via un crédit fixe de 20 à 200 $ par mois, « non-rollover », facturé aux tarifs API. Au-delà, l'usage s'arrête sauf si l'extra usage est activé. — [VentureBeat, « Anthropic reinstates OpenClaw and third-party agent usage on Claude subscriptions — with a catch », 13/05/2026 *(extrait de recherche)*](https://venturebeat.com/technology/anthropic-reinstates-openclaw-and-third-party-agent-usage-on-claude-subscriptions-with-a-catch)
  - Voir aussi [The Register, « Anthropic tosses agents into the API billing pool », 14/05/2026 *(extrait)*](https://www.theregister.com/ai-ml/2026/05/14/anthropic-tosses-agents-into-the-api-billing-pool/5240748).
- **Montants prévus (non accordés)** :
  - Pro : 20 $ ; Max 5x : 100 $ ; Max 20x : 200 $.
  - Team Standard : 20 $ ; Team Premium : 100 $.
  - Enterprise « usage-based » : 20 $ ; Enterprise Premium : 200 $.

  Le crédit aurait couvert les « Third-party apps that authenticate with your Claude subscription through the Agent SDK ». Il excluait « Interactive Claude Code in the terminal or IDE » et les conversations web, desktop et mobile. Il était individuel et non mutualisable. — [Claude Help Center, « Use the Claude Agent SDK with your Claude plan » (MAJ 16/06/2026)](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan)
- **15 juin 2026, suspension le jour même** : « We're pausing the changes to Claude Agent SDK usage described below. For now, nothing has changed: Claude Agent SDK, `claude -p`, and third-party app usage still draw from your subscription's usage limits. » Et : « The previously announced monthly credit … isn't available. » — [Claude Help Center (MAJ 16/06/2026)](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan)
  - Voir aussi [The New Stack, « Anthropic pauses Claude Agent SDK subscription change on day it was due to take effect », ~15/06/2026 *(titre vu en recherche)*](https://thenewstack.io/anthropic-pauses-claude-agent-sdk-subscription-change/).

**Texte officiel actuel (page « Legal and compliance » de Claude Code, consultée le 28/09/2026)** — [Anthropic, Claude Code docs « Legal and compliance »](https://code.claude.com/docs/en/legal-and-compliance)
- « **OAuth authentication** is intended exclusively for purchasers of Claude Free, Pro, Max, Team, and Enterprise subscription plans and is designed to support ordinary use of Claude Code and other native Anthropic applications. »
- « **Developers** building products or services that interact with Claude's capabilities, including those using the Agent SDK, should use API key authentication … Anthropic does not permit third-party developers to offer Claude.ai login into their own applications, or to route requests through Free, Pro, or Max plan credentials on behalf of their users. Moreover, developers may not collect, store, or intermediate Claude.ai credentials or session tokens — sign-in to a Claude account must complete through Anthropic's own flow. »
- « Nor does it prevent an end user from signing in to the unmodified Claude Code binary with their own Claude subscription … »
- « Advertised usage limits for Pro and Max plans assume **ordinary, individual usage of Claude Code and the Agent SDK**. »
- « Anthropic reserves the right to take measures to enforce these restrictions and may do so without prior notice. »
- Licence : Commercial Terms pour Team, Enterprise et API ; Consumer Terms pour Free, Pro et Max.

**Documentation d'exploitation**
- Note de l'Agent SDK : « Unless previously approved, Anthropic does not allow third party developers to offer claude.ai login or rate limits for their products, including agents built on the Claude Agent SDK. Use the API key authentication methods … instead. » — [Anthropic, « Agent SDK overview » (consultée le 28/09/2026)](https://code.claude.com/docs/en/agent-sdk/overview)
- **Scripts et CI avec un abonnement** : « For CI pipelines, scripts, or other environments where interactive browser login isn't available, generate a one-year OAuth token with `claude setup-token` … This token authenticates with your Claude subscription and requires a Pro, Max, Team, or Enterprise plan. It can only make model requests, so it can't establish Remote Control sessions or fetch claude.ai connectors. MCP servers you configure locally still work. » — [Anthropic, Claude Code docs « Authentication » (consultée le 28/09/2026)](https://code.claude.com/docs/en/authentication)
- **Sessions sans surveillance** : une session en arrière-plan ou Remote Control « that outlives the login stops making progress once the credential expires ». Claude Code avertit 3 jours avant l'expiration du login `/login`. En mode `-p`, une variable `ANTHROPIC_API_KEY` présente est toujours utilisée en priorité. — [Anthropic, « Authentication »](https://code.claude.com/docs/en/authentication)
- **Channels** :
  - « Channels are in research preview. They require Anthropic authentication through claude.ai or a Console API key, and are not available on Amazon Bedrock, Google Cloud's Agent Platform, or Microsoft Foundry. »
  - Le plugin Telegram officiel est publié dans `anthropics/claude-plugins-official`.
  - Pour Pro et Max : « Pro and Max users without an organization skip these checks entirely: channels are available and users opt in per session with `--channels` ». En Team ou Enterprise, les Channels sont bloqués tant qu'un Owner ne les a pas activés.
  - « Events only arrive while the session is open, so for an always-on setup you run Claude in a background process or persistent terminal. »
  - Pour un usage sans surveillance, `--dangerously-skip-permissions` est à utiliser « only … in environments you trust ».
  - Channels fonctionne aussi en mode non interactif `-p`.

  — [Anthropic, Claude Code docs « Channels » (consultée le 28/09/2026)](https://code.claude.com/docs/en/channels)
- **Alternative officielle sans Telegram** : « Remote Control: You drive your local session from claude.ai or the Claude mobile app ». — [Anthropic, « Channels » (tableau comparatif)](https://code.claude.com/docs/en/channels)

### Inferences
- **Clairement dans les clous** (docs officielles de septembre 2026) :
  - Claude Code non modifié, connecté à *son propre* abonnement Pro ou Max, lancé avec `--channels plugin:telegram@claude-plugins-official`, qu'on pilote seul depuis son Telegram (liste blanche des expéditeurs).
  - Un script personnel qui appelle `claude -p` ou l'Agent SDK avec `claude setup-token`.

  Ces usages sont documentés (« scripts ») et **décomptés des limites de l'abonnement**. Ils correspondent à l'exception « where we otherwise explicitly permit it » des Consumer Terms. C'est mon interprétation, pas une déclaration juridique d'Anthropic.
- **Clairement interdit** :
  - un bot qui sert **d'autres personnes** (clients, employés) via *votre* Pro ou Max ;
  - une appli tierce qui fait saisir le login claude.ai ailleurs que dans le flux Anthropic, ou qui stocke les jetons ;
  - tout client qui usurpe l'identité de Claude Code ;
  - la revente.
- **Zone grise**
  - **Intensité d'usage.** Un bot maison (hors Channels) qui pilote `claude -p` 24 h/24 reste « personnel ». Mais l'« ordinary, individual usage » n'est pas défini. Anthropic peut agir « without prior notice ».
  - **Facturation des harnais tiers (type OpenClaw), ambiguë.** La règle du 4 avril (harnais tiers → extra usage) n'a jamais été explicitement abrogée dans une source que j'ai pu lire. La notice du 15 juin dit pourtant que « third-party app usage still draw[s] from your subscription's usage limits ». Cela viserait au moins les applications qui s'authentifient « through the Agent SDK ».
- **Recommandation d'architecture** :
  - Garder un chemin de repli par **clé API** (Commercial Terms), qui ne dépend d'aucune de ces politiques.
  - Éviter de bâtir sur une authentification par abonnement hors du binaire officiel.
- **Détail d'exploitation** :
  - Le login claude.ai (`/login`) est nécessaire pour les connecteurs claude.ai (voir Q5), mais il expire et doit être renouvelé.
  - Le jeton `setup-token` dure un an mais ne charge pas ces connecteurs.

### Gaps
- Je n'ai pas pu lire le texte complet des articles de presse (sites bloqués), ni le post X de ClaudeDevs ni l'e-mail d'Anthropic aux abonnés du 3-4 avril. Les citations proviennent d'extraits de recherche.
- **Non vérifié / je ne sais pas** : la date exacte du blocage de janvier (le « 9 janvier » vient d'une source secondaire), la date exacte du lancement de Channels, et le statut de facturation actuel des harnais tiers qui ne passent pas par l'Agent SDK.
- Aucun seuil quantitatif officiel pour « ordinary, individual usage ».
- La mention de février (« including the Agent SDK — is not permitted ») n'apparaît plus dans la version actuelle de la page juridique. Je n'ai pas trouvé la date exacte de cette reformulation.

## 4. Claude Agent SDK : nature, langages, licence, outils intégrés, MCP, sessions/mémoire, authentification

### Takeaway
L'Agent SDK est **« Claude Code as a library »** : une bibliothèque **Python et TypeScript** qui **exécute le binaire Claude Code**. Elle en reprend la boucle d'agent, les outils intégrés, les permissions, les hooks, les sous-agents, le MCP, les sessions (reprise et fork), ainsi que les skills et la mémoire `.claude/`.

Son usage est régi par les **Commercial Terms**. L'authentification prévue pour les produits se fait par **clé API** (Console) ou par un fournisseur cloud : Bedrock, Google Cloud, Foundry. L'usage personnel avec son abonnement est aujourd'hui décompté des limites du forfait. Un développeur ne peut pas proposer le login claude.ai dans son produit.

### Cited Findings
- **Nature** : « The Agent SDK gives you the same tools, agent loop, and context management that power Claude Code, programmable in Python and TypeScript. » C'est « A library that runs the Claude Code binary, with Claude Code's capabilities, such as built-in tools, permissions, sessions, and hooks. » — [Anthropic, « Agent SDK overview » (consultée le 28/09/2026)](https://code.claude.com/docs/en/agent-sdk/overview)
- **Autres langages** : « run the CLI as a subprocess with the `-p` flag and `--output-format json` ». — [Anthropic, « Agent SDK overview »](https://code.claude.com/docs/en/agent-sdk/overview)
- **Capacités** :
  - outils intégrés (« Read, write, edit files, run commands, and search the web ») ;
  - Hooks, Subagents ;
  - MCP (« Connect external tools and data sources via the Model Context Protocol ») ;
  - Permissions ;
  - Sessions (« Maintain context across exchanges, resume or fork later ») ;
  - « Skills, commands, and memory: Load automatically from your project's `.claude/` and from `~/.claude/`, same as Claude Code » ;
  - Plugins.

  — [Anthropic, « Agent SDK overview »](https://code.claude.com/docs/en/agent-sdk/overview)
- **Dépôts** : `anthropics/claude-agent-sdk-typescript` et `anthropics/claude-agent-sdk-python` (CHANGELOG, issues). — [Anthropic, « Agent SDK overview »](https://code.claude.com/docs/en/agent-sdk/overview)
- **Licence** : « Use of the Claude Agent SDK is governed by Anthropic's Commercial Terms of Service, including when you use it to power products and services … except to the extent a specific component or dependency is covered by a different license as indicated in that component's LICENSE file. » — [Anthropic, « Agent SDK overview »](https://code.claude.com/docs/en/agent-sdk/overview)
- **Marque** : interdit de nommer son produit « Claude Code » ou « Claude Code Agent ». Formules autorisées : « Claude Agent », « {YourAgentName} Powered by Claude ». — [Anthropic, « Agent SDK overview »](https://code.claude.com/docs/en/agent-sdk/overview)
- **Authentification**
  - Note produit : « Unless previously approved, Anthropic does not allow third party developers to offer claude.ai login or rate limits for their products, including agents built on the Claude Agent SDK. Use the API key authentication methods described in the Quickstart instead. » — [Anthropic, « Agent SDK overview »](https://code.claude.com/docs/en/agent-sdk/overview)
  - Variables d'environnement : `apiKeyHelper`, `ANTHROPIC_API_KEY` et `ANTHROPIC_AUTH_TOKEN` s'appliquent « to the CLI and the surfaces that wrap it, including the VS Code extension, the Agent SDK, and GitHub Actions ».
  - Fournisseurs cloud : sélectionnés via `CLAUDE_CODE_USE_BEDROCK`, `CLAUDE_CODE_USE_VERTEX` ou `CLAUDE_CODE_USE_FOUNDRY`.
  - L'Agent SDK figure parmi les chemins de login claude.ai (« the terminal /login flow, the VS Code extension, the Agent SDK, `claude setup-token` … »).

  — [Anthropic, « Authentication »](https://code.claude.com/docs/en/authentication)
- **Usage avec abonnement** : « Claude Agent SDK, `claude -p`, and third-party app usage still draw from your subscription's usage limits » (situation suspendue au 15/06/2026). — [Claude Help Center (MAJ 16/06/2026)](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan)
- **Données locales** : Claude Code stocke les transcriptions de session « locally in plaintext under `~/.claude/projects/` for 30 days by default ». Ce délai se règle avec `cleanupPeriodDays`. — [Anthropic, Claude Code docs « Data usage » (consultée le 28/09/2026)](https://code.claude.com/docs/en/data-usage)
- **Alternatives hébergées par Anthropic**
  - **Managed Agents** : « A hosted agent harness that runs the agent loop, with sessions in an Anthropic-managed cloud sandbox or a self-hosted sandbox ». — [Anthropic, « Agent SDK overview »](https://code.claude.com/docs/en/agent-sdk/overview)
  - Tarif : tokens + 0,08 $ par heure de session. — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
  - Les sessions Managed Agents ne sont **pas** éligibles au zéro-rétention (ZDR) : « transcripts persist until you delete them ». — [Anthropic, « API and data retention »](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention)
- **Mémoire et contexte avec l'API brute (Messages API)** :
  - l'outil Memory est « Client-side memory storage where you control data retention » et éligible au ZDR ;
  - la compaction serveur (« Context management ») est éligible au ZDR.

  — [Anthropic, « API and data retention » (tableau d'éligibilité)](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention)

### Inferences
- **Pour un compagnon mono-utilisateur**, trois architectures côté Claude :
  - **(a)** Claude Code + Channels : le moins de code, avec le plugin Telegram officiel.
  - **(b)** Agent SDK (Python ou TypeScript) + une bibliothèque de bot Telegram : contrôle fin des permissions, des hooks, du modèle et des sessions.
  - **(c)** Messages API brute + MCP connector ou outils maison : pas de binaire Claude Code, contrôle total, coût au token.
- Les options **(a) et (b)** héritent de la mémoire fichier (`CLAUDE.md`) et des sessions reprenables. Elles laissent des **transcriptions en clair sur le serveur**, point à sécuriser au titre du RGPD.
- **Authentification de l'option (b)** : clé API. Cela évite toute dépendance aux politiques d'abonnement vues en Q3 et place le traitement sous DPA (voir Q6).

### Gaps
- Noms exacts des paquets npm et PyPI, dernières versions et licence des dépôts GitHub : **non vérifiés** dans cette session (pages non lues).
- Non vérifié : les connecteurs claude.ai se chargent-ils dans l'Agent SDK quand il est authentifié par login claude.ai ? Documenté pour la CLI seulement.

## 5. Connecteurs MCP : MCP connector de l'API, serveurs officiels Gmail/Agenda/Drive/Notion et pièges OAuth Google

### Takeaway
La Messages API sait appeler des **serveurs MCP distants** (bêta `mcp-client-2025-11-20`). Contraintes : outils uniquement, serveur exposé publiquement en HTTP, **jeton OAuth fourni et rafraîchi par vous**, pas d'éligibilité au zéro-rétention.

Côté serveurs officiels, trois options :
- **Google** propose des serveurs MCP Workspace distants (Gmail, Agenda, Drive, etc.), encore en **Developer Preview** réservée aux membres d'un programme. Ils exigent un projet Cloud et un client OAuth.
- **Anthropic** fournit dans claude.ai des **connecteurs Gmail, Agenda et Drive** gérés, disponibles pour tous les forfaits. Ils sont utilisables dans Claude Code **si on est connecté par login claude.ai**.
- **Notion** a un MCP hébergé **OAuth uniquement** ; son serveur local open source pourrait être abandonné.

Sur la voie « application OAuth Google personnelle » :
- en statut « Testing », les refresh tokens **expirent au bout de 7 jours** ;
- les scopes Gmail sont « restricted », avec vérification et évaluation de sécurité CASA annuelle, sauf exception d'usage personnel (moins de 100 utilisateurs).

### Cited Findings
**A. MCP connector de la Messages API** — [Anthropic, docs « MCP connector » (consultée le 28/09/2026)](https://platform.claude.com/docs/en/agents-and-tools/mcp-connector)
- Statut : bêta, en-tête `mcp-client-2025-11-20` (la version `mcp-client-2025-04-04` est dépréciée).
- Plateformes : Claude API, Claude Platform on AWS et Microsoft Foundry. « not available » sur Amazon Bedrock et Google Cloud.
- « Of the feature set of the MCP specification, only tool calls are currently supported. »
- « The server must be publicly exposed through HTTP (supports both Streamable HTTP and SSE transports). Local STDIO servers cannot be connected directly. »
- « API consumers are expected to handle the OAuth flow and obtain the access token prior to making the API call, and to refresh the token as needed. » Le jeton est passé dans `authorization_token`. Pour les tests, la doc suggère d'obtenir un jeton via le MCP Inspector.
- Liste d'autorisation ou d'exclusion d'outils possible ; plusieurs serveurs par requête ; fonctionne dans le Batch API.
- « The MCP connector is not covered by ZDR arrangements. … retained according to Anthropic's standard data retention policy. »

**B. MCP dans Claude Code et l'Agent SDK (côté client)** — [Anthropic, Claude Code docs « MCP » (consultée le 28/09/2026)](https://code.claude.com/docs/en/mcp)
- **Transports** : HTTP (« recommended »), SSE (« deprecated »), stdio local et WebSocket.
- **OAuth 2.0** : pris en charge via `/mcp` ou `claude mcp login <name>`, avec rafraîchissement automatique du jeton sur une erreur 401.
- **Connecteurs claude.ai** : « If you've logged into Claude Code with a claude.ai account, MCP servers you've added in claude.ai, known as connectors, are automatically available in Claude Code ».
  - Ils ne sont **pas** chargés quand la session est authentifiée par `ANTHROPIC_API_KEY`, `ANTHROPIC_AUTH_TOKEN`, `apiKeyHelper`, un fournisseur cloud, ou `CLAUDE_CODE_OAUTH_TOKEN` (`setup-token`).

**C. Serveurs et connecteurs Gmail, Google Agenda et Google Drive**
- **Connecteurs gérés par Anthropic (claude.ai)** — [Claude Help Center, « Use Google Workspace connectors » (MAJ semaine du 28/09/2026)](https://support.claude.com/en/articles/10166901-use-google-workspace-connectors)
  - Disponibilité : « Google Workspace connectors (Gmail, Google Calendar, and Google Drive) are available for all users on Claude and Claude Desktop ».
  - Gmail : « Search and read emails », « Draft emails », « Send, reply to, and forward emails from Gmail », avec approbation requise par défaut avant chaque action.
  - Agenda : création, modification et suppression d'événements. Drive : recherche, lecture, dépôt de fichiers.
  - Une organisation Google Workspace peut devoir « allow Claude as a trusted application ».
  - « We do not train our models on your Gmail, Drive, or Calendar connector data. »
- **Serveurs MCP distants officiels de Google (Workspace)**
  - Des pages de configuration existent pour Gmail, Drive, Calendar, People, Docs et Chat. — [Google for Developers, « Configure the Google Workspace MCP servers » *(extrait de recherche)*](https://developers.google.com/workspace/guides/configure-mcp-servers)
  - Endpoints : `gmailmcp.googleapis.com` et `calendarmcp.googleapis.com`. — [Google, « MCP Reference: gmailmcp.googleapis.com » *(titre vu en recherche)*](https://developers.google.com/workspace/gmail/api/reference/mcp) ; [Google, « MCP Reference: calendarmcp.googleapis.com » *(titre)*](https://developers.google.com/workspace/calendar/api/v3/reference/mcp)
  - Prérequis : « membership in the Google Workspace Developer Preview Program and a Google Cloud project ».
  - Configuration OAuth : « Under Audience, select Internal. If you can't select Internal, select External. »
  - Usage visé : permettre à « AI applications like Google Antigravity and Claude to perform actions in Gmail ».

  — [Google for Developers, « Configure the Gmail MCP server » *(extrait de recherche)*](https://developers.google.com/workspace/gmail/api/guides/configure-mcp-server)
  - **Date de lancement, sources discordantes** : annoncé à Cloud Next '26 et « opened to public developer preview in May 2026 » selon le blog Workspace Updates. — [Google Workspace Updates, « New: Agent tools and security updates for Google Workspace developers », mai 2026 *(extrait)*](https://workspaceupdates.googleblog.com/2026/05/agent-tools-and-security-updates-for-workspace-developers.html). Un blog tiers donne « launched April 22, 2026 in Developer Preview ». — [Drag, « Gmail MCP Server: Google's Official Preview… » *(extrait, date de l'article non vérifiée)*](https://www.dragapp.com/blog/gmail-mcp-server/)
  - **Détails du serveur Gmail selon un blog tiers** :
    - URL `https://gmailmcp.googleapis.com/mcp/v1` ;
    - scopes `gmail.readonly` et `gmail.compose` ;
    - 10 outils (recherche et lecture de fils, brouillons, libellés) : « no send tool … no delete tool ».

    — [Drag *(extrait)*](https://www.dragapp.com/blog/gmail-mcp-server/). Voir aussi le titre [Vorp Labs, « Gmail MCP server: no send tool, but a scope that can send »](https://vorplabs.com/agent-tools/gmail-mcp).
- **Serveurs communautaires**, par exemple « taylorwilsdon/google_workspace_mcp » (Gmail, Agenda, Drive, Docs, etc.). — [GitHub *(titre vu en recherche)*](https://github.com/taylorwilsdon/google_workspace_mcp)

**D. Notion**
- **Serveur local open source** : « We are prioritizing, and only providing active support for, Remote Notion MCP. As a result: We may sunset this local MCP server repository in the future. »
  - Authentification : `NOTION_TOKEN` (jeton d'intégration).
  - Transports : STDIO, ou Streamable HTTP avec bearer token.
  - Licence MIT ; version 2.0.0 (Notion API 2025-09-03) ; 22 outils.

  — [GitHub makenotion/notion-mcp-server, README (consulté le 28/09/2026)](https://github.com/makenotion/notion-mcp-server)
- **MCP hébergé par Notion** : flux OAuth « one-click » par utilisateur ; Streamable HTTP et SSE ; jetons stockés dans AWS Secrets Manager. — [Notion, blog « Notion's hosted MCP server: an inside look » *(extrait de recherche, date non vérifiée)*](https://www.notion.com/blog/notions-hosted-mcp-server-an-inside-look) ; [Notion Docs, « Notion MCP » *(extrait)*](https://developers.notion.com/guides/mcp/overview)
- **Limites du MCP hébergé selon des sources tierces** : « MCP auth is OAuth-only for Notion's hosted remote server ». Il ne serait « not designed for cloud-based agentic workflows that run without human interaction and does not support bearer token authentication ». — [Scalekit *(extrait)*](https://www.scalekit.com/blog/notion-mcp-vs-api) ; [StackOne *(extrait)*](https://www.stackone.com/blog/notion-mcp-deep-dive/)
- **Connecteur claude.ai** : Notion figure dans le répertoire de connecteurs de Claude. — [Anthropic, « Notion connector » *(extrait)*](https://claude.com/connectors/notion)
  - Selon un extrait du centre d'aide, les connecteurs distants sont réservés aux forfaits payants (Pro, Max, Team, Enterprise). — [Claude Help Center, « Use the Connectors Directory » *(extrait)*](https://support.claude.com/en/articles/11724452-use-the-connectors-directory-to-extend-claude-s-capabilities)

**E. Pièges OAuth Google pour une application personnelle** (pages Google non accessibles, extraits de recherche)
- **Refresh tokens en « Testing »** : un projet dont l'écran de consentement est en « External » et « Testing » reçoit des refresh tokens qui **expirent au bout de 7 jours**. En « In production », pas d'expiration à 7 jours, mais révocation possible, notamment après environ 6 mois d'inutilisation. — [Google for Developers, « Using OAuth 2.0 to Access Google APIs » *(extrait)*](https://developers.google.com/identity/protocols/oauth2) ; [DEV Community, « Google's OAuth 'Testing' mode expires refresh tokens in 7 days… » *(extrait)*](https://dev.to/ko-hi/googles-oauth-testing-mode-expires-refresh-tokens-in-7-days-publish-the-consent-screen-before-24hm)
- **Type « Internal »** : réservé aux organisations Google Workspace ou Cloud Identity. Il n'a ni expiration à 7 jours ni plafond de 100 utilisateurs de test. — [DEV Community *(extrait)*](https://dev.to/ko-hi/googles-oauth-testing-mode-expires-refresh-tokens-in-7-days-publish-the-consent-screen-before-24hm) ; [Google Cloud Help, « When is verification not needed » *(extrait)*](https://support.google.com/cloud/answer/13464323?hl=en)
- **Scopes restreints** : « Restricted scopes … require you to go through a scope verification process ». Les scopes Gmail (`gmail.readonly`, `gmail.modify`, `gmail.compose`, `https://mail.google.com/`, etc.) relèvent de ce niveau : vérification + évaluation de sécurité **CASA**, à renouveler au moins tous les 12 mois. — [Google for Developers, « Restricted scope verification » *(extrait)*](https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification)
- **Exceptions à la vérification** : « personal use », environnements de développement ou de test, usage interne à une organisation. Une application non vérifiée affiche l'écran « unverified app » et reste limitée à **100 nouveaux utilisateurs sur toute la vie du projet**. — [Google Cloud Help, « When is verification not needed » *(extrait)*](https://support.google.com/cloud/answer/13464323?hl=en) ; [Google Cloud Help, « Unverified apps » *(extrait)*](https://support.google.com/cloud/answer/7454865?hl=en)

### Inferences
- **Voie la plus simple côté connecteurs** : Claude Code connecté par **login claude.ai** (Pro ou Max), avec les connecteurs Anthropic Gmail, Agenda et Drive et le connecteur Notion activés dans claude.ai. Aucune application OAuth Google à créer ni à vérifier.
  - Revers 1 : ces connecteurs **ne se chargent pas** avec `setup-token` ni avec une clé API.
  - Revers 2 : le login claude.ai expire et doit être renouvelé pour une session permanente.
  - Revers 3 : les données passent par un compte **grand public** (voir Q6).
- **Voie API avec son propre OAuth** : un compte @gmail.com personnel ne peut pas choisir « Internal ». Deux possibilités :
  - rester en « Testing » et **se réauthentifier tous les 7 jours** ;
  - passer « In production » sans vérification au titre de l'exception d'usage personnel, avec l'écran d'avertissement « unverified app ».

  Avec un domaine Google Workspace d'entreprise, le type « Internal » évite ces deux écueils.
- **MCP connector de l'API** : il exige un serveur **public**. Un serveur Gmail communautaire auto-hébergé devrait donc être exposé sur Internet, ce qui augmente la surface d'attaque. Mieux vaut exécuter les serveurs MCP **côté client** (Agent SDK ou Claude Code, stdio local) ou utiliser les serveurs distants de Google et Notion.
- **Serveur Gmail MCP de Google** : sans outil d'envoi selon un blog tiers. Un assistant qui doit **envoyer** des e-mails devra passer par le connecteur Anthropic (envoi avec approbation), un serveur communautaire ou l'API Gmail directe.
- **Notion** : le MCP hébergé (OAuth) convient à Claude Code et claude.ai. Pour un bot headless via la Messages API, deux options : gérer soi-même le flux OAuth et le rafraîchissement, ou auto-héberger le serveur open source avec un jeton d'intégration. Ce dernier est menacé d'abandon.

### Gaps
- Pages Google (developers.google.com, support.google.com), Notion et CNIL non accessibles : les règles OAuth ci-dessus viennent d'extraits. **À revérifier** sur les pages officielles avant décision.
- **Non vérifié / je ne sais pas** :
  - les comptes Gmail grand public peuvent-ils rejoindre le Developer Preview Program ?
  - liste exacte des scopes et outils des serveurs MCP Calendar et Drive de Google ;
  - statut « sensitive » ou « restricted » des scopes Agenda et Drive.
- URL exacte du MCP hébergé Notion (`mcp.notion.com` n'a été vu que dans des requêtes, pas dans une page lue) et limites de débit.
- Non vérifié : fonctionnement des connecteurs Google d'Anthropic dans Claude Code via Channels. La doc dit que les connecteurs claude.ai sont disponibles dans Claude Code avec un login claude.ai, sans détailler Google Workspace.

## 6. Données personnelles : conditions grand public vs commerciales, rétention, ZDR, DPA, résidence des données ; implications RGPD pour un dirigeant en France (CNIL)

### Takeaway
**Abonnements grand public (Free, Pro, Max)** :
- l'entraînement des modèles sur les conversations est un **choix de l'utilisateur**, en vigueur depuis le 28/08/2025 avec une échéance au 08/10/2025 ;
- rétention de **5 ans** si l'entraînement est activé, **30 jours** sinon ;
- pas de zéro-rétention (ZDR) ;
- Anthropic Ireland est **responsable de traitement** pour les utilisateurs de l'EEE ;
- **pas de contrat de sous-traitance (DPA)**.

**API, Team et Enterprise (Commercial Terms)** :
- **pas d'entraînement** sur le contenu client ;
- **DPA** avec clauses contractuelles types (CCT) : Anthropic y est **sous-traitant** ;
- rétention standard de 30 jours ou moins, avec une discordance entre pages officielles ;
- ZDR possible sur demande commerciale, mais **pas pour le MCP connector** ;
- les contenus signalés peuvent être conservés jusqu'à 2 ans.

**Pas de résidence de données dans l'UE** sur l'API Anthropic en direct : l'inférence est « global » ou « us », le stockage « us » uniquement.

Pour traiter des e-mails et des données de clients, seule la voie **commerciale** (clé API, ou Team avec 2 sièges minimum) offre un cadre de sous-traitance conforme à l'article 28 du RGPD. Les avis CNIL de 2024-2026 invitent à analyser au cas par cas la réutilisation des données par le fournisseur et les risques propres aux agents.

### Cited Findings
**Grand public (Free, Pro, Max)**
- **Mise à jour du 28/08/2025** :
  - « We will train new models using data from Free, Pro, and Max accounts when this setting is on » ;
  - choix à faire avant le 08/10/2025 ;
  - rétention de 5 ans en cas d'opt-in, 30 jours sinon ;
  - ne s'applique **pas** à Claude for Work (Team, Enterprise), à l'API, à Bedrock, à Vertex, à Claude Gov ni à Education.

  — [Anthropic, « Updates to Consumer Terms and Privacy Policy », 28/08/2025](https://www.anthropic.com/news/updates-to-our-consumer-terms)
- **Claude Code sur compte grand public** :
  - « We will train new models using data from Free, Pro, and Max accounts when this setting is on (including when you use Claude Code from these accounts) » ;
  - rétention : « 5-year retention period » si autorisé, « 30-day retention period » sinon ;
  - réglage modifiable sur claude.ai/settings/data-privacy-controls ;
  - les transcriptions `/feedback`, `/bug` et `/share` sont « retained for 5 years ».

  — [Anthropic, Claude Code docs « Data usage » (consultée le 28/09/2026)](https://code.claude.com/docs/en/data-usage)
- **Consumer Terms** : les entrées et sorties peuvent servir à l'entraînement sauf désactivation. Les contenus signalés pour revue de sécurité ou envoyés en feedback peuvent être utilisés même en cas d'opt-out. — [Anthropic, Consumer Terms (effective 08/10/2025)](https://www.anthropic.com/legal/consumer-terms)
- **Politique de confidentialité (en vigueur le 10/09/2026)** :
  - responsable de traitement pour l'EEE, le Royaume-Uni et la Suisse : « Anthropic Ireland, Limited … 6th Floor, South Bank House, Barrow Street, Dublin 4 » ;
  - DPO : dpo@anthropic.com ;
  - transferts fondés sur les décisions d'adéquation, les CCT (article 46 RGPD) ou des dérogations ;
  - pour les offres commerciales, Anthropic traite les données selon les contrats clients.

  — [Anthropic, Privacy Policy (effective 10/09/2026)](https://www.anthropic.com/legal/privacy)
- **Exclusion du ZDR** : « Claude consumer products: Claude Free, Pro, and Max plans, including when customers on those plans use Claude's web, desktop, or mobile apps or Claude Code ». — [Anthropic, docs « API and data retention » (consultée le 28/09/2026)](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention)
- **Connecteurs Google** : « We do not train our models on your Gmail, Drive, or Calendar connector data ». En revanche, si l'utilisateur grand public a accepté l'entraînement, le contenu copié-collé ou les réponses de Claude contenant ces données peuvent servir à l'entraînement. — [Claude Help Center, « Use Google Workspace connectors »](https://support.claude.com/en/articles/10166901-use-google-workspace-connectors)

**Commercial (API, Team, Enterprise)**
- **Commercial Terms** (en vigueur le 17/06/2025) :
  - « Anthropic may not train models on Customer Content from Services » ;
  - le client « retains all rights to its Inputs, and … owns its Outputs » ;
  - DPA incorporé par référence ;
  - « Services under these Terms are not for consumer use » ;
  - revente interdite sauf accord.

  Selon le résumé de la page, le droit applicable aux clients de l'EEE, de la Suisse et du Royaume-Uni est le droit irlandais. — [Anthropic, Commercial Terms of Service (effective 17/06/2025)](https://www.anthropic.com/legal/commercial-terms)
- **DPA** (en vigueur le 24/02/2025) :
  - « Customer is the controller and Anthropic is Customer's processor » ;
  - CCT de l'UE (modules 2 et 3), addendum britannique et dispositions suisses ;
  - sous-traitants ultérieurs listés sur anthropic.com/subprocessors ;
  - restitution ou suppression des données dans les 30 jours suivant la fin du contrat ;
  - chiffrement « AES-256 for data at rest, and TLS1.2+ for data in transit » ;
  - droit d'audit.

  — [Anthropic, Data Processing Addendum (effective 24/02/2025)](https://www.anthropic.com/legal/data-processing-addendum)
- **Claude Code sous Commercial Terms** : « Commercial users (Team, Enterprise, and API) — Standard: 30-day retention period ». Pas d'entraînement « unless the customer has chosen to provide their data » (Development Partner Program). — [Anthropic, « Data usage »](https://code.claude.com/docs/en/data-usage)
- **Rétention sur l'API, discordance** : la page de doc API indique « Conversation content (your prompts and Claude's outputs) is not retained by default; the exception is Covered Models, which require 30-day retention » et « Retained data is never used for model training without your express permission ». — [Anthropic, « API and data retention »](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention). **Contredit en partie** par :
  - la page Claude Code, qui indique « Standard: 30-day retention period » pour l'API — [Anthropic, « Data usage »](https://code.claude.com/docs/en/data-usage) ;
  - un résumé du Privacy Center : « inputs and outputs are automatically deleted from the backend within 30 days » — [Anthropic Privacy Center, « How long do you store my organization's data? » *(extrait de recherche ; page bloquée)*](https://privacy.claude.com/en/articles/7996866-how-long-do-you-store-my-organization-s-data).
- **Contenus signalés** : « if a chat or session is flagged, Anthropic may retain inputs and outputs for up to 2 years », même sous ZDR. — [Anthropic, « API and data retention »](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention)
- **ZDR** : « contact the Anthropic sales team », activation par organisation. — [Anthropic, « API and data retention »](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention)
  - Éligibles : Messages API, cache de prompts, web search, web fetch, outil Memory, compaction.
  - **Non éligibles** : **MCP connector** (« Data retained per standard policy »), Batch (« 29-day retention »), Files API (jusqu'à suppression), exécution de code (« Container data retained up to 30 days »), Managed Agents, Agent Skills, Console, Covered Models.

**Résidence des données**
- `inference_geo` : « Only "us" and "global" are available ». Workspace geo : « Currently, "us" is the only available workspace geo ». L'inférence US seule coûte 1,1× sur les modèles 4.6 et suivants. — [Anthropic, docs « Data residency » (consultée le 28/09/2026)](https://platform.claude.com/docs/en/manage-claude/data-residency)
- Sur Bedrock et Google Cloud, les « Regional and multi-region endpoints include a 10% premium over global endpoints ». — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
  - « On Amazon Bedrock and Google Cloud's Agent Platform, the cloud provider is the data processor ». — [Anthropic, « API and data retention »](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention)

**CNIL** (site cnil.fr bloqué, extraits de recherche)
- **Questions-réponses sur l'utilisation d'un système d'IA générative** :
  - modes de déploiement « sur site », cloud ou « via des API » ;
  - « Lorsque les données sont susceptibles d'être réutilisées par le fournisseur (conformément à ses conditions générales d'utilisation), l'organisme utilisateur devra mener une analyse au cas par cas pour déterminer s'il doit ou non interdire la fourniture de toute donnée personnelle, ou seulement certaines catégories » ;
  - recommandation de partir d'un besoin concret et de définir des usages autorisés et interdits.

  — [CNIL, « Les questions-réponses de la CNIL sur l'utilisation d'un système d'IA générative » *(extrait ; date non vérifiée)*](https://www.cnil.fr/fr/les-questions-reponses-de-la-cnil-sur-lutilisation-dun-systeme-dia-generative)
- **Note exploratoire CNIL et CIANum sur l'IA agentique** (publiée le 20/07/2026 selon l'extrait) :
  - les agents « capables d'accéder et de traiter de grands volumes de données issues de sources multiples » entraînent des flux de données difficiles à comprendre et un « risque réel de perte de contrôle » ;
  - l'historique et les mémoires persistantes favorisent des profils « hyper-personnalisés » ;
  - le RGPD s'applique, mais sa mise en œuvre doit s'adapter à l'autonomie, à la mémoire persistante et aux actions multi-services.

  — [CNIL, « IA agentique et données personnelles : la CNIL et le Conseil de l'IA et du Numérique publient une note exploratoire » *(extrait)*](https://www.cnil.fr/fr/ia-agentique-cnil-cianum-note) ; [PDF de la note (juillet 2026)](https://www.cnil.fr/sites/default/files/2026-07/ia-cianum-cnil.pdf)

### Inferences
- **Compte Pro ou Max grand public** : faire transiter des **e-mails de clients** ou des données de tiers par ce compte revient à les confier à Anthropic Ireland agissant comme **responsable de traitement autonome**, sous Consumer Terms. Il n'y a pas de contrat de sous-traitance (article 28), et un opt-in d'entraînement éventuel porterait la rétention à 5 ans. Le dirigeant (lui-même responsable de traitement vis-à-vis de ses clients) aurait du mal à le justifier.
  - Le minimum serait de **désactiver l'entraînement**, minimiser les données et documenter l'usage.
  - La CNIL demande précisément une analyse au cas par cas quand le fournisseur peut réutiliser les données selon ses CGU.
- **Cadre plus défendable** : **clé API** (Commercial Terms + DPA avec CCT, pas d'entraînement, ZDR possible) ou **Team** (Commercial Terms, 2 sièges minimum). Il faut en plus :
  - inscrire le traitement au **registre** ;
  - mettre à jour l'information des personnes (traitement par IA, transfert vers les États-Unis via CCT) ;
  - fixer des durées de conservation, y compris pour les **transcriptions locales en clair** de Claude Code (`~/.claude/projects/`, 30 jours par défaut) ;
  - envisager une AIPD si le volume ou la sensibilité le justifie. C'est une analyse juridique à confirmer par un DPO ou un avocat.
- **Transferts et ZDR** : pas d'inférence UE sur l'API Anthropic en direct, donc transfert hors UE encadré par les CCT du DPA. Pour une localisation UE, il faut passer par Bedrock ou Google Cloud (endpoints régionaux +10 %, le fournisseur cloud étant sous-traitant), mais le MCP connector n'y est pas disponible. En cas de ZDR, le MCP connector en sort : exécuter les outils côté client (Agent SDK ou outils maison) garde le trafic dans le périmètre ZDR.
- **Choix du modèle** : éviter les « Covered Models » (Fable et Mythos), qui imposent 30 jours de rétention, si l'on vise une rétention minimale.

### Gaps
- **Non vérifié / je ne sais pas** : certification d'Anthropic au Data Privacy Framework UE–États-Unis (le DPA s'appuie sur les CCT) ; possibilité de signer le DPA pour un compte Pro ou Max (a priori non, puisque le DPA est rattaché aux Commercial Terms).
- Date et texte intégral de la Q&R CNIL sur l'IA générative, et texte intégral de la note sur l'IA agentique (site bloqué).
- La discordance « non conservé par défaut » (doc API) contre « 30 jours » (doc Claude Code et Privacy Center) n'est pas résolue.
- Disponibilité des modèles Claude récents dans les régions UE de Bedrock ou Vertex (Paris, Francfort) : non vérifiée.
- Les Consumer Terms visent les « individuals » (« These Terms of Service govern your use of Claude.ai, Claude Pro, and other products and services that we may offer for individuals ») — [Anthropic, Consumer Terms](https://www.anthropic.com/legal/consumer-terms). Je n'y ai pas vu d'interdiction explicite d'usage professionnel, mais je n'ai lu qu'un résumé de la page, pas le texte ligne à ligne. **Non vérifié.**

## 7. Estimation du coût mensuel API pour environ 30 échanges par jour avec un agent qui appelle des outils (hypothèses et calcul)

### Takeaway
Hypothèses : 900 échanges par mois, environ 50 000 tokens d'entrée et 1 400 de sortie par échange, 3 appels API par échange. Avec le **cache de prompts**, on obtient environ **31 $/mois (Haiku 4.5)**, **57 à 63 $/mois (Sonnet 5)** et **107 à 121 $/mois (Opus 5.5)**, en USD hors taxes. Sans cache, on passe à 51 $, 102 $ et 204 $.

Si mes hypothèses sous-estiment les tokens du nouveau tokenizer (+30 % environ pour Sonnet 5 et Opus 5.5), Sonnet 5 avec cache approche **82 $/mois**. La recherche web ajoute environ 2 $/mois. La transcription vocale n'est pas incluse.

### Cited Findings
- **Tarifs utilisés** (USD par MTok) :
  - Haiku 4.5 : entrée 1 $, écriture de cache 5 min 1,25 $, écriture 1 h 2 $, lecture 0,10 $, sortie 5 $ ;
  - Sonnet 5 : 2 $, 2,50 $, 4 $, 0,20 $, 10 $ ;
  - Opus 5.5 : 4 $, 5 $, 8 $, 0,20 $, 20 $ ;
  - recherche web 10 $ pour 1 000.

  — [Anthropic, « Pricing » (consultée le 28/09/2026)](https://platform.claude.com/docs/en/about-claude/pricing)
- **Surcoût système « tool use »** : 286 à 496 tokens selon le modèle, inclus dans mon préfixe fixe. — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Cache** :
  - « 5-minute cache write 1.25x … 1-hour cache write 2x … Cache read (hit) 0.1x (… 0.05x on Claude Opus 5.5) » ;
  - « Reading a prompt prefix from the prompt cache … also refreshes it » ;
  - le mode « Automatic caching » gère lui-même les points de rupture du cache à mesure que la conversation s'allonge.

  — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Tokenizer** : « approximately 30% more tokens for the same text » pour les modèles 4.7 et suivants. — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Réflexion** : les tokens de réflexion comptent dans la sortie (« total output (thinking plus response text) »). Sonnet 5 est à effort `high` par défaut ; l'effort `low` est recommandé pour le « chat and non-coding ». — [Anthropic, « Effort »](https://platform.claude.com/docs/en/build-with-claude/effort)

**Hypothèses** (de moi, non sourcées ; à recalibrer avec l'endpoint de comptage de tokens)
- 30 échanges par jour × 30 jours = **900 échanges par mois**.
- 1 échange = **3 appels API** en moyenne : appel 1, qui demande un outil ; appel 2 après le résultat, qui demande un second outil ; appel 3, qui produit la réponse finale.
- **Préfixe fixe = 10 000 tokens** : prompt système, définitions d'outils Gmail, Agenda, Drive et Notion, mémoire.
- **Historique = 3 000 tokens** : fenêtre glissante ou résumé.
- **Message utilisateur = 200 tokens** : texte déjà transcrit.
- **Résultat d'outil = 3 000 tokens** : par exemple 10 e-mails résumés, ou l'agenda de la semaine.
- **Sortie** : 400 tokens aux appels 1 et 2, 600 à l'appel 3, soit **1 400 tokens par échange**, réflexion comprise, effort `low` ou `medium`.
- **Entrée par appel** : 13 200 ; puis 13 200 + 400 + 3 000 = 16 600 ; puis 16 600 + 400 + 3 000 = 20 000. Soit **49 800 tokens par échange**.
- **Volume mensuel** : **44,82 MTok** en entrée (49 800 × 900) et **1,26 MTok** en sortie (1 400 × 900).

**Scénario A, sans cache** : coût = 44,82 × prix d'entrée + 1,26 × prix de sortie.
- Haiku 4.5 : 44,82 × 1 + 1,26 × 5 = 44,82 + 6,30 = **51,12 $**
- Sonnet 5 : 44,82 × 2 + 1,26 × 10 = 89,64 + 12,60 = **102,24 $**
- Opus 5.5 : 44,82 × 4 + 1,26 × 20 = 179,28 + 25,20 = **204,48 $**

**Scénario B, cache de 5 minutes, froid au début de chaque échange** (échanges espacés de plus de 5 min)
- Par échange :
  - écritures en cache = 13 200 (appel 1) + 3 400 + 3 400 = 20 000 tokens ;
  - lectures = 13 200 + 16 600 = 29 800 tokens.
- Par mois : 18,0 MTok écrites et 26,82 MTok lues.
- Coûts :
  - Haiku 4.5 : 18,0 × 1,25 + 26,82 × 0,10 + 6,30 = 22,50 + 2,68 + 6,30 = **31,48 $**
  - Sonnet 5 : 18,0 × 2,50 + 26,82 × 0,20 + 12,60 = 45,00 + 5,36 + 12,60 = **62,96 $**
  - Opus 5.5 : 18,0 × 5 + 26,82 × 0,20 + 25,20 = 90,00 + 5,36 + 25,20 = **120,56 $**

**Scénario C, cache d'1 heure maintenu chaud** (environ un échange toutes les 24 min sur 12 h ; chaque lecture renouvelle le TTL)
- Le préfixe fixe de 10 000 tokens est relu à chaque échange, sauf le premier de la journée.
- Écritures, toutes comptées au tarif 1 h (hypothèse prudente) : (3 200 + 3 400 + 3 400) × 900 + 10 000 × 30 = **9,3 MTok**.
- Lectures : (10 000 + 13 200 + 16 600) × 900 − 300 000 = **35,52 MTok**.
- Coûts :
  - Haiku 4.5 : 9,3 × 2 + 35,52 × 0,10 + 6,30 = **28,45 $**
  - Sonnet 5 : 9,3 × 4 + 35,52 × 0,20 + 12,60 = 37,20 + 7,10 + 12,60 = **56,90 $**
  - Opus 5.5 : 9,3 × 8 + 35,52 × 0,20 + 25,20 = 74,40 + 7,10 + 25,20 = **106,70 $**

**Compléments**
- **Recherche web** : 1 échange sur 5 déclenche une recherche, soit 180 recherches par mois × 10 $/1 000 = **1,80 $**, plus les tokens des résultats (non chiffrés).
- **Sensibilité au tokenizer** : si les volumes ci-dessus correspondent à un comptage « ancien tokenizer », il faut multiplier par 1,3 environ pour Sonnet 5 et Opus 5.5. Scénario B : Sonnet 5 ≈ **81,85 $**, Opus 5.5 ≈ **156,73 $**.
- **Inférence limitée aux États-Unis** (`inference_geo: "us"`) : ×1,1 sur Sonnet 5 et Opus 5.5 ; non supporté par Haiku 4.5.
- **Taxes** : montants en USD hors taxes (« All payments are in USD »). — [Anthropic, « Pricing »](https://platform.claude.com/docs/en/about-claude/pricing)
- **Abonnements, pour comparaison** : Pro 20 $/mois hors taxes ; Max 5x 100 $ ; Max 20x 200 $. — [Claude Help Center, Max](https://support.claude.com/en/articles/11049741-what-is-the-max-plan) ; [Pro](https://support.claude.com/en/articles/8325606-what-is-the-pro-plan). Team : 2 sièges minimum à 20–25 $ l'un. — [Team](https://support.claude.com/en/articles/9266767-what-is-the-team-plan)

### Inferences
- À ce volume, **Sonnet 5 avec cache coûte environ 55 à 85 $/mois HT** selon le tokenizer et le TTL : 3 à 4 fois un Pro, un peu moins qu'un Max 5x. Haiku 4.5 descend vers 30 à 40 $, mais son retrait est possible dès le 15/10/2026.
- **Le poste principal est l'entrée**, gonflée par les résultats d'outils et la ré-injection de l'historique à chaque appel de la boucle. Les leviers de réduction, par ordre d'impact :
  - filtrer et résumer les résultats d'outils ;
  - limiter l'historique (compaction) ;
  - activer le cache automatique, avec un TTL d'1 h si les échanges sont rapprochés ;
  - effort `low` ;
  - Haiku pour les sous-tâches ;
  - Batch (−50 %) pour les tâches non urgentes comme un résumé quotidien.
- **Échanges sans outil** (question simple, environ 13 000 tokens d'entrée et 300 de sortie) : ils coûtent 3 à 4 fois moins cher. Le coût réel dépend fortement de la part d'échanges outillés.
- Un **abonnement Pro ou Max** peut sembler moins cher, mais :
  - les limites d'usage ne sont pas publiées ;
  - on dépend des politiques d'usage vues en Q3 ;
  - le cadre grand public est inadapté à des données de clients (Q6) ;
  - au-delà des limites, les usage credits sont facturés au tarif API.

### Gaps
- Toutes les volumétries (tokens par appel, nombre d'appels, part d'échanges outillés, tokens de réflexion) sont des **hypothèses de modélisation**. Aucune source ne donne de profil de consommation typique pour un assistant personnel.
- Coût de la transcription audio (autre chercheur) et de l'hébergement : non inclus.
- Conversion en EUR et traitement de la TVA : non faits faute de source officielle (voir Q2).
