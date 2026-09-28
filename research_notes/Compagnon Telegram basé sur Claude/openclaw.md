# OpenClaw (ex-Clawdbot / Moltbot) comme compagnon Telegram basé sur Claude — notes de recherche (état au 28/09/2026)

Notes compilées le 28/09/2026. **Légende de fiabilité** : **[lu]** = page consultée en entier le 28/09/2026 (surtout le dépôt GitHub officiel, dont le dossier `docs/` qui alimente docs.openclaw.ai, ainsi que les pages officielles d'Anthropic) ; **[résumé]** = information obtenue seulement par le résumé du moteur de recherche. Dans ce cas, l'article lui-même était inaccessible depuis cet environnement, car le proxy réseau bloquait notamment docs.openclaw.ai, openclaw.ai, cert.ssi.gouv.fr, techcrunch.com, theregister.com, venturebeat.com, thenewstack.io, nvd.nist.gov, osv.dev, wikipedia.org, censys.com et koi.ai. Ces informations sont à reconfirmer avant toute citation définitive. Les sources commerciales sont signalées (hébergeurs d'OpenClaw ou éditeurs de produits concurrents), car leurs chiffres peuvent être biaisés.

---

## 1. Historique, gouvernance, version actuelle et niveau d'activité

### Takeaway
OpenClaw est un projet open source sous licence MIT, très populaire et très actif : 391 k étoiles et environ 101,5 k commits au 28/09/2026, avec plusieurs versions par mois. Il est né fin 2025 sous le nom Clawdbot, puis a été renommé deux fois fin janvier 2026, en Moltbot le 27/01 puis en OpenClaw le 30/01, après une plainte d'Anthropic sur la marque. Peter Steinberger a rejoint OpenAI à la mi-février 2026. Le projet est depuis porté par l'OpenClaw Foundation, une association 501(c)(3) de droit américain constituée le 08/07/2026, qui dispose d'une petite équipe salariée et de sponsors industriels. Dernière version : 2026.9.6 (23/09/2026), avec en parallèle une branche « extended-stable » (2026.7.35, 21/09/2026).

### Cited Findings
- Nom d'origine « Clawdbot », publié en novembre 2025 par Peter Steinberger — [Lightning AI, « What Is OpenClaw (Formerly Clawdbot and Moltbot)? », date n.c.](https://lightning.ai/blog/what-is-openclaw-clawdbot-moltbot) [résumé]
- 27/01/2026 : renommage en « Moltbot » après des plaintes d'Anthropic sur la marque (risque de confusion avec « Claude ») — [Trending Topics, « Clawdbot Becomes Moltbot After Anthropic Trademark Issue », janv. 2026](https://www.trendingtopics.eu/clawdbot-moltbot-anthropic/) ; [AlternativeTo News, janv. 2026](https://alternativeto.net/news/2026/1/trending-open-source-ai-agent-clawdbot-rebrands-to-moltbot-after-pressure-from-anthropic/) [résumé]
- 30/01/2026 : nouveau renommage en « OpenClaw », car le nom « Moltbot » était jugé confus et difficile à rechercher — [Lightning AI](https://lightning.ai/blog/what-is-openclaw-clawdbot-moltbot) [résumé]
- L'ancien nom subsiste dans les avis de sécurité : la CVE-2026-25253 vise le paquet npm « clawdbot <= 2026.1.28 » et s'intitule « OpenClaw/Clawdbot has 1-Click RCE… » — [GitHub Advisory Database, GHSA-g8p2-7wf7-98mq, publié le 31/01/2026](https://github.com/advisories/GHSA-g8p2-7wf7-98mq) [lu]
- Le 30/01/2026, TechCrunch titre que les assistants OpenClaw « construisent leur propre réseau social » (Moltbook) — [TechCrunch, 30/01/2026](https://techcrunch.com/2026/01/30/openclaws-ai-assistants-are-now-building-their-own-social-network) [résumé, titre seul]
- Mi-février 2026 : OpenAI recrute Peter Steinberger et OpenClaw doit passer sous une fondation — [Forbes, Ron Schmelzer, 16/02/2026](https://www.forbes.com/sites/ronschmelzer/2026/02/16/openai-hires-openclaw-creator-peter-steinberger-and-sets-up-foundation/) ; [SEN-X « OpenClaw Daily », 16/02/2026](https://senx.ai/openclaw-news/2026-02-16-steinberger-openai-foundation-future). Billet de l'intéressé, non consulté car bloqué : [Peter Steinberger, « OpenClaw, OpenAI and the future », steipete.me, 2026](https://steipete.me/posts/2026/openclaw) [résumé]
- 08/07/2026 : constitution de l'« OpenClaw Foundation », organisation à but non lucratif 501(c)(3). Elle est cofondée par Steinberger et Dave Morin, qui la préside et parle de « Switzerland of AI ». Sponsors cités : OpenAI, GitHub, Nvidia, Microsoft et Tencent. L'Université du Michigan en serait le plus gros donateur (point non recoupé) — [explainx.ai, juillet 2026](https://explainx.ai/blog/openclaw-foundation-501c3-nonprofit-july-2026) ; [Trending Topics, « OpenClaw Foundation Launches With OpenAI, NVIDIA, Microsoft, and Tencent as Sponsors », ~juillet 2026](https://www.trendingtopics.eu/opwnclaw-foundation-launches-with-openai-nvidia-microsoft-and-tencent-as-sponsors/) ; [openclaw.org/donors](https://www.openclaw.org/donors) (non consulté) [résumé]
- Le README officiel indique : « OpenClaw is stewarded by the OpenClaw Foundation, an independent 501(c)(3) ». Il cite parmi les donateurs Amazon, OpenAI, Red Hat et des sponsors d'infrastructure. Licence : « MIT License © OpenClaw Foundation » — [GitHub openclaw/openclaw, README, consulté le 28/09/2026](https://github.com/openclaw/openclaw) [lu]
- Équipe : le projet dispose pour la première fois d'une équipe à plein temps issue des mainteneurs communautaires. Côté ingénierie : Vincent Koc (Chief Architect), Josh Avant, Patrick Erichsen, Dallin Romney, Jason Sy et Gideon Adegbesan. Côté opérations : Jen Vescio (partenariats), Matt Jasie (finances), Hannes Rudolph (communauté) et Kelly Pike (recrutement). Des mainteneurs bénévoles complètent l'équipe — [The New Stack, « "The Switzerland of AI": OpenClaw becomes a non-profit foundation », ~juillet 2026](https://thenewstack.io/openclaw-foundation-nonprofit-status/) [résumé]
- Rôle actuel de Steinberger : il « continue de prendre les décisions, surtout techniques ». Il dirige aussi chez OpenAI une équipe baptisée « Claw Labs », chargée d'améliorations utiles à la fois à OpenClaw et aux produits OpenAI — [The New Stack](https://thenewstack.io/openclaw-foundation-nonprofit-status/) [résumé]
- Activité GitHub au 28/09/2026 : 391 k étoiles, 82,2 k forks et 101 490 commits sur main — [GitHub openclaw/openclaw](https://github.com/openclaw/openclaw) [lu]
- Versions récentes, consultées le 28/09/2026 [lu] — [GitHub Releases](https://github.com/openclaw/openclaw/releases) :
  - v2026.9.6 (23/09/2026). La build macOS plantait au lancement et a été remplacée le 24/09 par une build recompilée et notarisée.
  - v2026.7.35 (21/09/2026), branche « extended-stable ».
  - v2026.9.5 (19/09/2026).
  - Auparavant : 2026.9.4, 2026.6.35, 2026.9.3, 2026.9.2, 2026.9.1 et 2026.8.2.
  - Selon un hébergeur, la 2026.9.2 est sortie le 05/09/2026 — [BetterClaw (hébergeur), blog](https://www.betterclaw.io/blog/openclaw-2026-9-2-update) [résumé]
- Notes de version de septembre 2026 : durcissement du parsing de commandes, des contrôles d'origine du navigateur, des installations de plugins via Git, des identifiants de service et des logs de webhooks. Elles corrigent aussi des cas limites Telegram, WhatsApp, Slack, Discord, etc., avec la mention « preventing silent message loss » — [GitHub Releases](https://github.com/openclaw/openclaw/releases) [lu]
- Plateformes : applications natives « for macOS, iOS, Android, Windows, and Linux » ; canaux « Discord, iMessage, Slack, Teams, Telegram, WhatsApp, and 20+ more » — [README](https://github.com/openclaw/openclaw/blob/main/README.md) [lu]
- Le dossier `docs/maturity` du dépôt contient `scorecard.md` et `taxonomy.md`, soit une grille de maturité par fonctionnalité — [GitHub docs/maturity](https://github.com/openclaw/openclaw/tree/main/docs/maturity) [lu, contenu non lu]

### Inferences
- Les versions sont calendaires (AAAA.M.n). On compte environ six versions « stables » et deux « extended-stable » en septembre 2026, soit une cadence au moins hebdomadaire. Pour un non-développeur, la branche extended-stable paraît plus adaptée, mais les correctifs de sécurité restent à appliquer vite (voir §7).
- La fondation, l'équipe salariée et les sponsors ont professionnalisé la gouvernance, ce qui réduit le risque d'abandon du projet. En contrepartie, le créateur est salarié d'OpenAI, qui est aussi sponsor. Je n'ai trouvé aucun indice d'un traitement défavorable de Claude, mais la dépendance aux sponsors mérite d'être surveillée (spéculatif).
- Le retrait de la build macOS du 23/09 et les corrections de « silent message loss » montrent que des régressions arrivent encore en production (maturité perfectible).

### Gaps
- Date exacte (14 ou 15/02/2026) et texte de l'annonce de Steinberger : billet bloqué, non vérifié.
- Nombre exact de contributeurs, composition du conseil d'administration et budget de la fondation : non vérifiés.
- Contenu de la grille de maturité `docs/maturity` : non lu.
- La métrique « 4,5 millions de nouveaux claws par semaine » n'apparaît que dans un titre SEN-X du 11/07/2026 ([lien](https://senx.ai/openclaw-news/2026-07-11-openclaw-news)) : non vérifiée.

---

## 2. Canal Telegram : mise en place, types de messages, notes vocales (STT) et réponses vocales (TTS)

### Takeaway
Telegram est un canal de premier plan. On crée le bot avec BotFather, et par défaut chaque inconnu doit être approuvé en message privé (« pairing »). Les notes vocales, les fichiers audio, les photos, les vidéos, les localisations et les stickers sont pris en charge. Les notes vocales sont transcrites automatiquement, soit par un service cloud (OpenAI, Deepgram, Groq, Mistral Voxtral, Google, ElevenLabs, xAI, SenseAudio), soit **en local** (whisper-cli / whisper.cpp, sherpa-onnx, parakeet-mlx sur Mac Apple Silicon). La langue est détectée automatiquement ou peut être imposée. Les réponses peuvent être lues à voix haute (TTS) et arrivent comme de vraies bulles vocales Telegram. Microsoft Edge TTS fonctionne sans clé API ; ElevenLabs, OpenAI, Azure, Gemini et des moteurs locaux sont aussi proposés.

### Cited Findings
**Mise en place et contrôle d'accès**
- Première étape documentée : « Create the bot token in BotFather ».
  - Clés de configuration : `enabled`, `botToken`, `tokenFile` (fichiers ordinaires uniquement, les liens symboliques sont refusés), `accounts.*`.
  - Contrôle d'accès : `dmPolicy`, `allowFrom`, `groupPolicy`, `groupAllowFrom`, `groups`.
  - Streaming : `off | partial | block | progress`.
  - Source : [docs/channels/telegram.md (dépôt officiel), consulté le 28/09/2026](https://github.com/openclaw/openclaw/blob/main/docs/channels/telegram.md) [lu]
- « Default DM policy for Telegram is pairing ». Trois politiques existent : pairing, allowlist et open. Les groupes sont en allowlist par défaut et demandent de régler le « privacy mode » dans BotFather. Les sujets de forum peuvent être liés chacun à un agent — [docs/channels/telegram.md](https://github.com/openclaw/openclaw/blob/main/docs/channels/telegram.md) [lu]
- Le README précise : « DM-capable channels pair unknown senders by default; approve a pairing with `openclaw pairing approve` » et « Treat inbound messages as untrusted input. » — [README](https://github.com/openclaw/openclaw/blob/main/README.md) [lu]
- Types de messages entrants pris en charge : texte, albums photo, notes vocales, messages audio, messages vidéo, localisations, lieux (venues) et stickers — [docs/channels/telegram.md](https://github.com/openclaw/openclaw/blob/main/docs/channels/telegram.md) [lu]

**Transcription des notes vocales (STT)**
- Fournisseurs cloud : OpenAI (gpt-4o-transcribe, gpt-4o-mini-transcribe), Deepgram (nova-3), Groq, Mistral (voxtral-mini-latest), Google, ElevenLabs, xAI et SenseAudio. Outils locaux en ligne de commande : whisper-cli, whisper.cpp, sherpa-onnx-offline et parakeet-mlx (Apple Silicon) — [docs/nodes/audio.md](https://github.com/openclaw/openclaw/blob/main/docs/nodes/audio.md) [lu]
- Ordre d'auto-détection — [docs/nodes/audio.md](https://github.com/openclaw/openclaw/blob/main/docs/nodes/audio.md) [lu] :
  1. le modèle de réponse actif, s'il accepte l'audio ;
  2. les fournisseurs pour lesquels des identifiants sont configurés, dans l'ordre Groq, OpenAI, xAI, Deepgram, Google, SenseAudio, ElevenLabs, Mistral ;
  3. les outils locaux en ligne de commande.
- Réglages — [docs/nodes/audio.md](https://github.com/openclaw/openclaw/blob/main/docs/nodes/audio.md) [lu] :
  - `tools.media.audio.language` : non renseigné = détection automatique ; on peut forcer une langue.
  - `echoTranscript` : affiche la transcription dans le chat (désactivé par défaut).
  - Plafond de 20 Mo par fichier ; les audios de moins de 1024 octets sont ignorés ; timeout de 60 s pour les outils locaux.
- La transcription remplace le corps du message. Les commandes slash peuvent donc être dictées, puisque `CommandBody` reçoit le texte transcrit — [docs/nodes/audio.md](https://github.com/openclaw/openclaw/blob/main/docs/nodes/audio.md) [lu]
- Point utile pour la confidentialité : « Once a provider attempts transcription, upload or HTTP failures are reported without automatically sending the recording to another provider or switching credential classes. » — [docs/nodes/audio.md](https://github.com/openclaw/openclaw/blob/main/docs/nodes/audio.md) [lu]
- Les skills fournies avec OpenClaw incluent `openai-whisper` (local), `openai-whisper-api` et `sherpa-onnx-tts` — [dossier skills/ du dépôt, consulté le 28/09/2026](https://github.com/openclaw/openclaw/tree/main/skills) [lu]

**Réponses vocales (TTS)**
- « OpenClaw converts outbound replies into native voice messages on Feishu, Matrix, Telegram, and WhatsApp. Every other channel receives an audio attachment. » La documentation liste plus de 15 fournisseurs, dont Azure Speech, ElevenLabs, Google Gemini, OpenAI, un fournisseur « Microsoft (no key) » et des moteurs locaux — [docs/tools/tts.md](https://github.com/openclaw/openclaw/blob/main/docs/tools/tts.md) [lu, page d'index]
- Edge TTS passe par la bibliothèque node-edge-tts (service neuronal en ligne de Microsoft) et ne demande aucune clé API. Il est utilisé par défaut quand aucune clé OpenAI ou ElevenLabs n'est configurée. Le réglage `messages.tts.auto: "inbound"` limite la réponse vocale aux messages reçus en vocal. Sur Telegram, la sortie est en Opus 48 kHz / 64 kbps, format requis pour la bulle vocale ronde — [Stack Junkie, « OpenClaw Voice Mode: Telegram Voice Notes and TTS Setup »](https://www.stack-junkie.com/blog/openclaw-voice-mode-telegram) ; [miroir coréen de la doc, docs.openclaw.kr/tools/tts](https://docs.openclaw.kr/tools/tts) [résumé]
- Bug #65951, en version 2026.4.11 : les notes vocales reçues en privé sur Telegram étaient bien transcrites, mais la réponse vocale automatique en mode « inbound » ne se déclenchait pas — [GitHub issue #65951, avril 2026](https://github.com/openclaw/openclaw/issues/65951) [résumé ; statut de correction non vérifié]

**Français**
- La documentation a une localisation française : le dossier `docs/.i18n` contient `fr-navigation.json` et `glossary.fr.json`, aux côtés de de, es, it, ja, ko, zh, etc. — [GitHub docs/.i18n](https://github.com/openclaw/openclaw/tree/main/docs/.i18n) [lu]

### Inferences
- Pour un utilisateur francophone, il semble prudent de fixer `tools.media.audio.language` sur le français pour éviter une mauvaise détection. Ce réglage existe, mais son effet sur la qualité n'a pas été testé.
- Seule la transcription locale (whisper.cpp ou parakeet-mlx sur un Mac mini Apple Silicon) évite d'envoyer la voix à un tiers. Edge TTS, bien que « gratuit », envoie le texte des réponses à un service en ligne de Microsoft.
- Le flux voix vers Telegram est donc réalisable : notes vocales transcrites en entrée, bulles vocales en sortie. Il a toutefois connu des régressions (bug #65951).

### Gaps
- Qualité et prise en charge du français par chaque fournisseur STT (OpenAI, Deepgram, Groq, Voxtral, parakeet-mlx…) et existence de voix françaises dans Edge TTS ou ElevenLabs : non vérifiées ici (« Non vérifié / je ne sais pas »).
- Coût à la minute de la transcription selon le fournisseur : non recherché.
- Correction du bug #65951 dans les versions actuelles : non vérifiée.

---

## 3. Autres fonctionnalités utiles : mémoire, skills/ClawHub, tâches planifiées/heartbeat, Gmail/Agenda/Notion, navigateur, multi-modèles

### Takeaway
- **Mémoire** : de simples fichiers Markdown dans l'espace de travail (USER.md, MEMORY.md, notes quotidiennes, DREAMS.md), avec une recherche hybride sémantique et par mots-clés.
- **Proactivité** : un « heartbeat » (toutes les 30 min par défaut, 1 h avec l'authentification OAuth/jeton d'Anthropic) et des automatisations planifiées livrables sur Telegram.
- **Google** : Gmail, Agenda, Drive, Contacts, Sheets et Docs passent par la skill `gog` fournie avec OpenClaw, qui demande un client OAuth Google.
- **Autres skills fournies** : Notion et Himalaya (e-mail).
- **Navigateur** : un profil isolé piloté par l'agent.
- **Modèles** : tous interchangeables (Claude, Codex/OpenAI, modèles locaux).

### Cited Findings
**Mémoire**
- Espace de travail par défaut `~/.openclaw/workspace`, avec quatre fichiers : `USER.md` (préférences), `MEMORY.md` (faits durables), `memory/YYYY-MM-DD.md` (notes du jour) et `DREAMS.md` (consolidation). La documentation précise : « The model only remembers what gets saved to disk; there is no hidden state. » — [docs/concepts/memory.md](https://github.com/openclaw/openclaw/blob/main/docs/concepts/memory.md) [lu]
- Recherche hybride (similarité vectorielle et mots-clés). Fournisseurs d'embeddings : OpenAI (par défaut), Gemini, Voyage, Mistral, Bedrock, DeepInfra, Ollama, LM Studio et GitHub Copilot. Une option locale (GGUF, Ollama, LM Studio) existe. Avant chaque compaction, un « silent turn » rappelle à l'agent de sauvegarder l'essentiel — [docs/concepts/memory.md](https://github.com/openclaw/openclaw/blob/main/docs/concepts/memory.md) [lu]

**Skills et ClawHub**
- Le README présente les skills ainsi : « Tools, skills, and plugins extend what an assistant can do ». Elles se partagent sur ClawHub (clawhub.ai) — [README](https://github.com/openclaw/openclaw/blob/main/README.md) [lu]
- Plus de 60 skills sont fournies avec OpenClaw, dont `notion`, `gog` (Google Workspace), `himalaya` (e-mail), `weather`, `summarize`, `openai-whisper` et `sherpa-onnx-tts`. Aucune skill « calendar » dédiée n'apparaît : l'agenda passe par `gog` — [dossier skills/](https://github.com/openclaw/openclaw/tree/main/skills) [lu]
- ClawHub est passé d'environ 2 857 skills (audit Koi, février 2026) à plus de 10 700 — [The Hacker News, févr. 2026](https://thehackernews.com/2026/02/researchers-find-341-malicious-clawhub.html) [résumé] (voir §7 pour les skills malveillantes)

**Heartbeat et tâches planifiées**
- Définition : « a system-owned automation that runs periodic agent turns in the main session so the model can surface anything that needs attention without spamming you ». Réglages — [docs/gateway/heartbeat.md](https://github.com/openclaw/openclaw/blob/main/docs/gateway/heartbeat.md) [lu] :
  - Cadence par défaut de 30 min, portée à **1 h avec l'authentification OAuth/jeton d'Anthropic** ; `agents.defaults.heartbeat.every` à `"0m"` désactive le heartbeat.
  - Plages horaires, par exemple `activeHours: { start: "09:00", end: "22:00" }`.
  - Livraison possible sur Telegram : `target: "telegram"`, `to: "<chat id>"`.
- Coûts : `isolatedSession: true` fait passer chaque exécution d'environ 100K tokens à environ 2–5K ; `lightContext: true` ne charge pas les fichiers d'amorçage — [docs/gateway/heartbeat.md](https://github.com/openclaw/openclaw/blob/main/docs/gateway/heartbeat.md) [lu]
- Automatisations planifiées : rappels ponctuels, par exemple `openclaw automations create "2027-02-01T16:00:00Z" --name "Reminder" --delete-after-run`. Exécution en session isolée ou dans la session principale, livraison vers les canaux de chat ou des webhooks, avec un « Unattended run contract » — [docs/automation/cron-jobs.md](https://github.com/openclaw/openclaw/blob/main/docs/automation/cron-jobs.md) [lu]

**Gmail, Agenda et Notion**
- La skill `gog` est un outil en ligne de commande Google Workspace (Gmail, Calendar, Drive, Contacts, Sheets, Docs). Installation : `gog auth credentials /path/to/client_secret.json` puis `gog auth add you@gmail.com --services gmail,calendar,drive,contacts,docs,sheets`. Les identifiants sont stockés dans le trousseau du système — [skills/gog/SKILL.md](https://github.com/openclaw/openclaw/blob/main/skills/gog/SKILL.md) ; [Qualcomm Developer Blog, mai 2026](https://www.qualcomm.com/developer/blog/2026/05/connect-gmail-openclaw-gog-skill) [résumé]
- Mise en garde, venant d'un éditeur concurrent : la détection d'abus de Gmail peut signaler le comportement d'un agent comme suspect — [AgentMail (vend une alternative), blog 2026](https://www.agentmail.to/blog/connect-openclaw-to-gmail) [résumé, source intéressée]

**Navigateur**
- Le profil dédié « `openclaw` profile never touches your personal browser profile ». Le service de contrôle tourne dans la Gateway, en loopback uniquement. Un profil `user` optionnel réutilise les sessions Chrome connectées via Chrome DevTools MCP. — [docs/tools/browser.md](https://github.com/openclaw/openclaw/blob/main/docs/tools/browser.md) [lu]
- L'agent peut gérer les onglets, cliquer, taper, glisser et sélectionner, prendre des captures ou des snapshots et produire des PDF. Mise en garde de la doc : « This browser is **not** your daily driver ». Une politique anti-SSRF est configurable — [docs/tools/browser.md](https://github.com/openclaw/openclaw/blob/main/docs/tools/browser.md) [lu]

**Multi-modèles**
- Le README l'annonce ainsi : « Models and agent harnesses (Claude, Codex, local models) are plugins you can swap without changing anything else. » — [README](https://github.com/openclaw/openclaw/blob/main/README.md) [lu]
- Modèle recommandé par la doc du fournisseur Anthropic : **Claude Opus 5.5** (contexte d'entrée de 1 M tokens, sortie de 128 K, « adaptive thinking » en effort « medium » par défaut, non désactivable). Cache de prompt `short` (5 min, par défaut avec une clé API) ou `long` (1 h) — [docs/providers/anthropic.md](https://github.com/openclaw/openclaw/blob/main/docs/providers/anthropic.md) [lu]

### Inferences
- Toutes les briques du cas d'usage existent : e-mails, agenda, notes, recherche et relances proactives. En revanche, la connexion Google demande de créer un client OAuth dans Google Cloud, ce qui n'est pas trivial pour un non-développeur.
- La mémoire en Markdown est transparente et modifiable à la main, ce qui est bon pour le contrôle. Mais c'est aussi un vecteur de persistance en cas d'injection de prompt (voir §7).

### Gaps
- Capacités exactes de la skill Notion et prise en charge d'Outlook/Microsoft 365 : non vérifiées.
- Outil de recherche web utilisé par défaut, API et coût : non vérifiés.

---

## 4. Installation et hébergement : prérequis, lieux d'exécution, offres gérées, difficulté pour un non-développeur

### Takeaway
OpenClaw exige Node.js 24.16+ ou 26.1+ (Node 26 recommandé). L'installation se fait par une commande en une ligne (macOS, Linux, WSL2, Windows PowerShell), par npm ou avec Docker. Il tourne sur un ordinateur personnel (le Mac mini allumé en permanence est courant), sur n'importe quel VPS Linux ou sur un Raspberry Pi. Je n'ai trouvé **aucune offre d'hébergement géré officielle** de la fondation, seulement des offres tierces « en un clic » ou gérées (Hostinger, xCloud, MyClawHost, ClawHosters). Pour un non-développeur, démarrer est faisable via ces offres, mais sécuriser correctement l'installation reste technique : le CERT-FR juge la configuration par défaut « extrêmement fragile ».

### Cited Findings
- « OpenClaw requires **Node.js 24.16+ or Node.js 26.1+**. Node 26 is recommended » — [SECURITY.md, consulté le 28/09/2026](https://github.com/openclaw/openclaw/blob/main/SECURITY.md) ; [README](https://github.com/openclaw/openclaw) [lu]
- Méthodes d'installation : `curl -fsSL https://openclaw.ai/install.sh | bash` (macOS/Linux/WSL2), `iwr -useb https://openclaw.ai/install.ps1 | iex` (Windows) et `npm install -g openclaw@latest --allow-scripts=openclaw`. Docker est aussi mentionné — [README](https://github.com/openclaw/openclaw) [lu]
- Pour configurer Claude avec une clé API : `openclaw onboard --anthropic-api-key "$ANTHROPIC_API_KEY"` — [docs/providers/anthropic.md](https://github.com/openclaw/openclaw/blob/main/docs/providers/anthropic.md) [lu]
- Guide VPS, qui ne donne aucune exigence minimale de RAM ou de CPU — [docs/vps.md](https://github.com/openclaw/openclaw/blob/main/docs/vps.md) [lu] :
  - « Run the OpenClaw Gateway on any Linux server or cloud VPS ».
  - Guides disponibles pour DigitalOcean, Fly.io, Hetzner (Docker), AWS, Google Compute Engine, Render et Railway, plus Kubernetes et Ansible ; une page dédiée au Raspberry Pi ; des VM macOS en option.
  - Cache de compilation Node recommandé sur ARM et machines peu puissantes.
  - Règle par défaut : « keep the Gateway on loopback and access it via SSH tunnel or Tailscale Serve ».
- La campagne ClawHavoc visait les utilisateurs qui font tourner OpenClaw en continu, « often on dedicated, always-on machines such as Mac minis » — [The Hacker News, févr. 2026](https://thehackernews.com/2026/02/researchers-find-341-malicious-clawhub.html) ; [Koi Security](https://www.koi.ai/blog/clawhavoc-341-malicious-clawedbot-skills-found-by-the-bot-they-were-targeting) [résumé]
- Pour utiliser un abonnement Claude via Claude Code, OpenClaw doit tourner « on the same host as the Claude CLI login ». Docker peut conserver le dossier home et la connexion ; Podman et les autres conteneurs qui ne peuvent pas monter `~/.claude` exigent une clé API — [docs/providers/anthropic.md](https://github.com/openclaw/openclaw/blob/main/docs/providers/anthropic.md) [lu]
- Offres tierces [résumé ; toutes commerciales] :
  - Hostinger : déploiement en un clic depuis hPanel, avec « pre-configured security and AI credits » — [TechRadar](https://www.techradar.com/pro/our-favourite-web-hosting-company-is-giving-you-access-to-ais-latest-superstar-for-free-one-click-gets-you-openclaw-on-hostingers-shared-hosting) ; [Hostinger tutorials](https://www.hostinger.com/tutorials/best-openclaw-hosting/)
  - xCloud : service géré, en ligne en « ~5 minutes » — [xCloud](https://xcloud.host/openclaw-hosting/)
  - Autres : [MyClawHost](https://www.myclawhost.com/) ; [ClawHosters](https://clawhosters.com/)
- Le CERT-FR parle d'une « configuration par défaut extrêmement fragile », avec des privilèges élevés : accès au système de fichiers, lecture des variables d'environnement, installation de plugins — [CERT-FR, CERTFR-2026-ACT-016, 13/04/2026](https://www.cert.ssi.gouv.fr/actualite/CERTFR-2026-ACT-016/) ; [Clubic, avr. 2026](https://www.clubic.com/actualite-608878-l-anssi-alerte-openclaw-est-bien-trop-risque-sur-votre-poste-de-travail.html) ; [Silicon.fr, avr. 2026](https://www.silicon.fr/cybersecurite-1371/anssi-openclaw-226895) [résumé]

### Inferences
- Un chef d'entreprise non développeur a deux voies réalistes :
  - un hébergeur géré, ce qui revient à confier à un tiers tous les accès (boîte mail, agenda, clés API) ;
  - une machine dédiée (Mac mini ou VPS), installée et durcie par un prestataire : Gateway en loopback, accès par Tailscale, mises à jour régulières.
- Le chemin « abonnement Claude via Claude CLI » ajoute une contrainte d'hébergement : OpenClaw doit tourner sur la même machine que la connexion Claude Code.

### Gaps
- Existence d'une offre gérée officielle de la fondation : aucune trouvée, mais l'absence n'est pas prouvée.
- Configuration matérielle minimale recommandée, prix des VPS, localisation des données (UE ou non) chez les hébergeurs gérés : non recherchés.

---

## 5. Coûts d'exploitation : consommation de tokens, fourchettes mensuelles, modèles recommandés

### Takeaway
Le logiciel est gratuit (MIT). Les coûts viennent des tokens du modèle, du STT/TTS et de l'hébergement. Tarifs officiels de l'API Claude au 28/09/2026 : Opus 5.5 à 4 $ / 20 $ par million de tokens (entrée/sortie), Sonnet 5 à 2 $ / 10 $, Haiku 4.5 à 1 $ / 5 $. La consommation dépend surtout des tâches de fond (heartbeat) et de la taille du contexte. Les chiffres publiés vont d'environ 7 $/mois à plusieurs centaines de dollars, avec des dérapages de plusieurs milliers. **Ces fourchettes viennent presque toutes de blogs commerciaux, pas d'études indépendantes.**

### Cited Findings
- Tarifs API Anthropic au 28/09/2026 — [Anthropic, page Pricing, consultée le 28/09/2026](https://platform.claude.com/docs/en/about-claude/pricing) [lu] :
  - **Opus 5.5** : 4 $/MTok en entrée et 20 $/MTok en sortie ; lecture en cache à 0,20 $/MTok (0,05x) ; écriture en cache 5 min à 5 $, 1 h à 8 $.
  - **Sonnet 5** : 2 $ / 10 $. « The previously scheduled increase to $3/$15 … on September 1, 2026 will not occur. »
  - **Haiku 4.5** : 1 $ / 5 $.
  - Recherche web à 10 $ pour 1 000 recherches.
  - Le nouveau tokenizer (modèles Claude 4.7 et suivants) produit « approximately 30% more tokens for the same text ».
- OpenClaw recommande Claude Opus 5.5 pour Anthropic — [docs/providers/anthropic.md](https://github.com/openclaw/openclaw/blob/main/docs/providers/anthropic.md) [lu]
- Heartbeat : environ 100K tokens par exécution dans la session principale, contre 2–5K avec `isolatedSession` — [docs/gateway/heartbeat.md](https://github.com/openclaw/openclaw/blob/main/docs/gateway/heartbeat.md) [lu]
- Suivi des coûts : avec une clé Admin API (`sk-ant-admin…`), l'interface de contrôle affiche « 30 days of provider-reported organization cost » — [docs/providers/anthropic.md](https://github.com/openclaw/openclaw/blob/main/docs/providers/anthropic.md) [lu]
- Estimations de blogs commerciaux, toutes [résumé] — [Kilo](https://kilo.ai/openclaw/how-much-does-it-cost) ; [fast.io](https://fast.io/resources/openclaw-pricing/) ; [haimaker.ai](https://haimaker.ai/blog/openclaw-api-costs-pricing/) :
  - de ~7 $/mois pour un usage personnel à plus de 500 $/mois pour une équipe ;
  - « power user » sous Claude Haiku 4.5 sur un VPS Contabo : 25 à 80 $/mois ;
  - heartbeat par défaut toutes les 30 min, 8 000 à 15 000 tokens par appel, soit environ 85 $/mois sous Opus 4.6 ; les heartbeats représenteraient souvent 60–80 % des tokens. Ce chiffre diverge de la doc officielle, qui indique ~100K tokens en session principale ; l'écart dépend sans doute de la taille de l'historique ;
  - une instance inactive sous Opus brûlerait environ 5 $/jour ;
  - gros utilisateur (plus de 100 M tokens/mois) : environ 400–800 $/mois avec une clé API facturée à l'usage.
- Un cas de 180 M tokens en un mois, soit environ 3 600 $, est relayé par plusieurs blogs, dont [OpenClaw Pulse](https://openclawpulse.com/openclaw-api-cost-deep-dive/) [résumé]. Source d'origine non identifiée.
- Notebookcheck titre qu'OpenClaw « can burn through hundreds of Dollars per day » — [Notebookcheck, 2026](https://www.notebookcheck.net/Free-to-use-AI-tool-can-burn-through-hundreds-of-Dollars-per-day-OpenClaw-has-absurdly-high-token-use.1219925.0.html) [résumé, titre]
- Cas extrême, non représentatif : Steinberger aurait dépensé 1 305 088,81 $ d'API OpenAI en 30 jours (603 milliards de tokens, 7,6 M de requêtes, environ 100 agents Codex, en « Fast Mode ») — [Tom's Hardware, 2026](https://www.tomshardware.com/tech-industry/artificial-intelligence/openclaw-creator-burns-through-1-3-million-in-openai-api-tokens-in-a-single-month) [résumé]

### Inferences
Calculs personnels à partir des chiffres sourcés ci-dessus, hors tokens de sortie :
- **Heartbeat dans la session principale** (~100K tokens, toutes les 30 min, 24 h/24) : 48 × 100K = 4,8 M tokens d'entrée par jour.
  - Sous Opus 5.5 sans cache : environ 19 $/jour, soit ~580 $/mois.
  - Le cache « short » par défaut (5 min) expire entre deux heartbeats espacés de 30 min. Il ne profite donc pas à ce scénario et ajouterait même le surcoût d'écriture (1,25x).
  - Avec le cache « long » (1 h), en lectures de cache à 0,20 $/MTok : environ 1 $/jour, soit ~29 $/mois.
- **Heartbeat isolé** (~5K tokens) : 240K tokens/jour, soit environ 0,96 $/jour (~29 $/mois) sous Opus 5.5, ~14 $/mois sous Sonnet 5 et ~7 $/mois sous Haiku 4.5.
- Ce sont donc surtout les réglages qui déterminent la facture, plus que le nombre de messages envoyés : fréquence du heartbeat, `isolatedSession`, `activeHours`, choix du modèle et du cache.
- Avec un abonnement Claude via Claude CLI, le heartbeat passe par défaut à 1 h ([doc heartbeat](https://github.com/openclaw/openclaw/blob/main/docs/gateway/heartbeat.md)), ce qui réduit mécaniquement la consommation.

### Gaps
- Aucune étude indépendante sur un profil « assistant personnel » comparable (quelques dizaines de messages par jour, e-mails et agenda).
- Coût du STT à la minute, prix des VPS et existence d'un plafond de dépenses dans la Claude Console : non vérifiés ici.

---

## 6. Compatibilité Claude et conditions d'Anthropic (abonnement Pro/Max, OAuth/setup-token, changements de 2026, état actuel)

### Takeaway
OpenClaw propose trois façons d'utiliser Claude :
1. une **clé API Anthropic**, facturée à l'usage et recommandée « for production » ;
2. le mode **« Claude CLI »** : OpenClaw exécute le binaire officiel Claude Code, connecté avec l'abonnement Pro/Max de l'utilisateur ;
3. un **setup-token** généré par `claude setup-token`.

La politique d'Anthropic a changé cinq fois en 2026 :
- **janvier** : blocages techniques côté serveur ;
- **19–20/02** : les CGU interdisent l'usage de jetons OAuth grand public dans des outils tiers ;
- **04/04** : l'abonnement ne couvre plus les « third-party harnesses including OpenClaw », il faut activer l'« extra usage » payant ;
- **13/05** : l'usage tiers est rétabli, avec un crédit « Agent SDK » séparé prévu au 15/06 ;
- **15/06** : ce crédit est mis en pause.

**État au 28/09/2026** : l'aide Anthropic, mise à jour le 16/06/2026, indique que l'usage de l'Agent SDK, de `claude -p` et des « third-party app » décompte des limites de l'abonnement. La page juridique rappelle toutefois que les développeurs tiers ne peuvent pas faire transiter des requêtes par les identifiants Pro/Max, tout en autorisant un utilisateur à se connecter au binaire Claude Code non modifié. **Seule la clé API ne présente aucune ambiguïté.**

### Cited Findings
**Chronologie**
- **Janvier 2026** : blocage côté serveur (à partir du 09/01) ; les jetons OAuth des offres grand public renvoient des erreurs en dehors de Claude Code et de Claude.ai — [aihackers.net, févr. 2026](https://aihackers.net/posts/anthropic-claude-code-oauth-policy-feb-2026/) ; [Shareuhack](https://www.shareuhack.com/en/posts/opencode-anthropic-legal-controversy-2026) [résumé, sources secondaires]
- **19–20/02/2026** : nouvelle formulation des conditions, « Using OAuth tokens obtained through Claude Free, Pro, or Max accounts in any other product, tool, or service, including the Agent SDK, is not permitted and constitutes a violation of the Consumer Terms of Service. » — [The Register, 20/02/2026](https://www.theregister.com/2026/02/20/anthropic_clarifies_ban_third_party_claude_access/) ; [GIGAZINE, 20/02/2026](https://gigazine.net/gsc_news/en/20260220-anthropic-third-party-block/) [résumé]
- **04/04/2026, 12 h (heure du Pacifique)**. Formulations rapportées :
  - « no longer be able to use your Claude subscription limits for third-party harnesses including OpenClaw » ;
  - il faut « extra usage, a pay-as-you-go option billed separately from your subscription » ;
  - la mesure « applies to all third-party harnesses and will be rolled out to more shortly » ;
  - un « one-time credit for extra usage equal to your monthly subscription price » était utilisable jusqu'au 17/04.
  - Sources : [TechCrunch, 04/04/2026](https://techcrunch.com/2026/04/04/anthropic-says-claude-code-subscribers-will-need-to-pay-extra-for-openclaw-support/) ; [The Register, 06/04/2026](https://www.theregister.com/2026/04/06/anthropic_closes_door_on_subscription/) ; [TechRadar, avr. 2026](https://www.techradar.com/pro/bad-news-claude-users-anthropic-says-youll-need-to-pay-to-use-openclaw-now) ; [Hacker News](https://news.ycombinator.com/item?id=47633396) [résumé]
- Motif avancé : des abonnés payant 20 à 200 $/mois consommaient l'équivalent de centaines, voire de milliers de dollars d'API — [VentureBeat, avr. 2026](https://venturebeat.com/technology/anthropic-cuts-off-the-ability-to-use-claude-subscriptions-with-openclaw-and) [résumé]
- **13/05/2026** : Anthropic rétablit l'usage des agents tiers comme OpenClaw sur les abonnements, sous la forme d'un crédit mensuel « Agent SDK » fixe, non reportable, de 20 à 200 $ selon l'offre, prévu pour le 15/06. C'est « the catch » — [VentureBeat, 13/05/2026](https://venturebeat.com/technology/anthropic-reinstates-openclaw-and-third-party-agent-usage-on-claude-subscriptions-with-a-catch) ; [The New Stack](https://thenewstack.io/anthropic-agent-sdk-credits/) [résumé]
- **15/06/2026** : pause le jour même de l'entrée en vigueur. Anthropic dit retravailler le plan « to better support how users build with Claude subscriptions » et promet un préavis avant tout futur changement — [The New Stack, ~15/06/2026](https://thenewstack.io/anthropic-pauses-claude-agent-sdk-subscription-change/) ; [The Decoder, juin 2026](https://the-decoder.com/anthropic-backs-off-unpopular-billing-overhaul-as-price-war-with-openai-looms/) ; [wmedia.es](https://wmedia.es/en/tips/claude-code-agent-sdk-credit) [résumé]

**Textes officiels en vigueur**
- **Aide officielle Anthropic, mise à jour le 16/06/2026** : « We're pausing the changes to Claude Agent SDK usage described below. For now, nothing has changed: Claude Agent SDK, `claude -p`, and third-party app usage still draw from your subscription's usage limits. » — [Claude Help Center, « Use the Claude Agent SDK with your Claude plan », consulté le 28/09/2026](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan) [lu]
  - Crédits qui étaient prévus : Pro 20 $/mois, Max 5x 100 $, Max 20x 200 $, Team Standard 20 $, Team Premium 100 $, Enterprise Premium 200 $.
  - Ils devaient couvrir les « third-party apps that authenticate with your Claude subscription through the Agent SDK ».
- **Page juridique officielle, consultée le 28/09/2026** — [Anthropic, Claude Code Docs, « Legal and compliance »](https://code.claude.com/docs/en/legal-and-compliance) [lu] :
  - « **OAuth authentication** is intended exclusively for purchasers of Claude Free, Pro, Max, Team, and Enterprise subscription plans and is designed to support ordinary use of Claude Code and other native Anthropic applications. »
  - « Anthropic does not permit third-party developers to offer Claude.ai login into their own applications, or to route requests through Free, Pro, or Max plan credentials on behalf of their users. Moreover, developers may not collect, store, or intermediate Claude.ai credentials or session tokens — sign-in to a Claude account must complete through Anthropic's own flow. »
  - « Nor does it prevent an end user from signing in to the unmodified Claude Code binary with their own Claude subscription »
  - « Advertised usage limits for Pro and Max plans assume ordinary, individual usage of Claude Code and the Agent SDK. »
  - « Anthropic reserves the right to take measures to enforce these restrictions and may do so without prior notice. »
  - Les Consumer Terms s'appliquent aux offres Free/Pro/Max, les Commercial Terms aux offres Team, Enterprise et à l'API.

**Côté OpenClaw**
- Documentation du fournisseur Anthropic — [docs/providers/anthropic.md, consulté le 28/09/2026](https://github.com/openclaw/openclaw/blob/main/docs/providers/anthropic.md) [lu] :
  - clé API : « usage-based billing » ;
  - réutilisation de Claude CLI : OpenClaw « never reads, persists, refreshes, selects, or forwards the native login tokens » ;
  - setup-token (`sk-ant-oat01-…`) : `openclaw models auth login --provider anthropic --method setup-token` ;
  - la doc cite l'aide Anthropic : « Subscription-plan Claude Agent SDK, `claude -p`, and third-party app usage still draw from the signed-in subscription's usage limits » (mise à jour du 15/06/2026), et précise que le crédit Agent SDK séparé est « not available while Anthropic revises that plan » ;
  - Anthropic peut modifier la facturation de Claude Code « without an OpenClaw release » ;
  - consigne : ne pas « copy an OAuth token into OpenClaw ».
- Avec l'authentification OAuth/jeton d'Anthropic, le heartbeat passe par défaut à 1 h au lieu de 30 min — [docs/gateway/heartbeat.md](https://github.com/openclaw/openclaw/blob/main/docs/gateway/heartbeat.md) [lu]

### Inferences
- Pour un usage professionnel, la clé API (Commercial Terms) est la voie conforme et stable. La facturation à l'usage est prévisible par unité, mais pas plafonnée par défaut.
- L'abonnement Pro/Max via le binaire Claude Code non modifié est **actuellement toléré** selon l'aide Anthropic. La règle a toutefois changé cinq fois en 2026, et Anthropic se réserve d'agir « without prior notice ».
- Un agent qui tourne 24 h/24 (heartbeat, automatisations) s'éloigne de l'« ordinary, individual usage » prévu par les CGU. Il y a donc un risque de limitation ou de sanction du compte, sans que j'aie de cas documenté.
- L'usage d'une entreprise sous Consumer Terms (Pro/Max) pose en outre une question de cadre contractuel, à vérifier.

### Gaps
- Je n'ai trouvé aucune source primaire indiquant si la règle du 04/04 (« extra usage » obligatoire pour les « third-party harnesses ») s'applique encore aux accès tiers qui ne passent **pas** par l'Agent SDK ou le binaire Claude Code. L'aide Anthropic ne vise que les « third-party apps that authenticate… through the Agent SDK » (« Non vérifié / je ne sais pas »).
- Textes primaires d'Anthropic du 04/04 et du 13/05/2026 (e-mail aux abonnés, billets officiels) : non lus, seule la presse l'a été.

---

## 7. Sécurité : CVE, instances exposées, skills malveillantes, injection de prompt, recommandations officielles, avis d'agences, interdictions ; problèmes corrigés vs structurels

### Takeaway
Le bilan de sécurité est très chargé :
- des centaines d'avis de sécurité GitHub : environ 722 affichés au 28/09/2026, dont plusieurs « High » publiés le 11/09/2026 ;
- des CVE critiques, corrigées rapidement ;
- des dizaines de milliers d'instances exposées sur Internet ;
- des campagnes de skills malveillantes sur ClawHub (de 341 à plus de 1 184 selon les éditeurs) ;
- des démonstrations d'injection de prompt, dont une exfiltration via les aperçus de liens Telegram.

Le CERT-FR/ANSSI (13/04/2026) recommande de ne pas déployer ces assistants sur des postes de travail. La Chine a restreint leur usage dans les administrations, et Meta ainsi que d'autres entreprises les ont interdits. Les failles ponctuelles sont corrigées vite. Les risques **structurels** demeurent : injection de prompt, modèle mono-opérateur « de confiance », chaîne d'approvisionnement des skills et privileges étendus. SECURITY.md exclut même explicitement de son périmètre l'injection de prompt seule et les plugins malveillants installés par l'opérateur.

### Cited Findings
**CVE et avis majeurs (corrigés)**
- **CVE-2026-25253** (GHSA-g8p2-7wf7-98mq), « 1-Click RCE via Authentication Token Exfiltration From gatewayUrl » — [GitHub Advisory](https://github.com/advisories/GHSA-g8p2-7wf7-98mq) [lu] :
  - gravité High, CVSS 3.1 de 8,8 (AV:N/AC:L/PR:N/UI:R) ;
  - versions touchées : `clawdbot` <= 2026.1.28, **corrigée en 2026.1.29** ;
  - publiée le 31/01/2026, mise à jour le 02/02/2026 ;
  - mécanisme : « The Control UI trusts `gatewayUrl` from the query string without validation and auto-connects on load, sending the stored gateway token ».
- CVE-2026-25253 « actively exploited in the wild », surnommée « ClawBleed » — affirmation d'un hébergeur, [BetterClaw, sept. 2026](https://www.betterclaw.io/blog/openclaw-security-2026) [résumé]. **Non vérifié** : présence au catalogue CISA KEV non contrôlée.
- **CVE-2026-32922** (GHSA-x8qx-w8w2-g4rx) — [GitHub Advisory](https://github.com/advisories/GHSA-x8qx-w8w2-g4rx) [lu] :
  - gravité **Critical**, CVSS v4 de 9,4 selon GitHub ; 9,9 en CVSS 3.1 selon d'autres bases ([Strix](https://www.strix.ai/cve/CVE-2026-32922) [résumé]) ;
  - publiée le 29/03/2026 ;
  - `device.token.rotate` permet à un détenteur de la portée `operator.pairing` de « mint tokens with broader scopes », jusqu'à l'administration et une RCE ;
  - corrigée en **2026.3.11**. La « community baseline » minimale recommandée serait la v2026.3.21 — [Blink](https://blink.new/blog/cve-2026-32922-openclaw-privilege-escalation-fix-guide) [résumé].
- **« Claw Chain »** (découverte par Cyera), quatre failles **corrigées en 2026.4.22** — [The Hacker News, mai 2026](https://thehackernews.com/2026/05/four-openclaw-flaws-enable-data-theft.html) [résumé] :
  - CVE-2026-44112 : TOCTOU d'écriture hors du bac à sable OpenShell (CVSS 9,6/6,3) ;
  - CVE-2026-44113 : TOCTOU de lecture (7,7/6,3) ;
  - CVE-2026-44115 : contournement de l'allowlist par expansion shell dans un heredoc (8,8) ;
  - CVE-2026-44118 : contrôle d'accès défaillant, où des clients loopback non propriétaires se font passer pour le propriétaire.
- CVE-2026-35665 : déni de service avant 2026.3.24, dû à un correctif incomplet de CVE-2026-32011 — [SentinelOne](https://www.sentinelone.com/vulnerability-database/cve-2026-35665/) [résumé]
- Flux continu d'avis : le 11/09/2026 ont été publiés notamment GHSA-3mq7-q27j-mq7q (**High**, approbations d'exécution survivant au répertoire revu), GHSA-9m4p-cqp4-jppq (**High**, outil de login WhatsApp accessible à des tours non propriétaires) et GHSA-5rx7-34fw-64qg (Moderate, échappement des racines `workspaceOnly` via Unicode). D'autres avis concernaient l'exposition d'identifiants iOS, l'authentification du relais navigateur et les autorisations Slack — [page Security du dépôt, consultée le 28/09/2026](https://github.com/openclaw/openclaw/security) [lu]
- Volume total, avec des chiffres qui divergent :
  - la page Security affichait environ **722** avis au 28/09/2026 [lu ; libellé exact du compteur non confirmé] ;
  - un hébergeur parle de « 543 CVEs » ([BetterClaw](https://www.betterclaw.io/blog/openclaw-security-2026)) ;
  - un autre site de « 138 CVEs » ([cvefind/Bexxo](https://www.cvefind.com/en/blog/openclaw-compromise-ai-agents.html)) [résumé]. Les méthodes de comptage diffèrent.

**Instances exposées sur Internet** (chiffres très dépendants de la méthode)
- Censys : croissance de 1 000 à plus de 21 000 instances en une semaine, **plus de 21 000 au 31/01/2026**, repérées via les titres HTML « Moltbot Control » / « clawdbot Control » — [Censys, blog, ~févr. 2026](https://censys.com/blog/openclaw-in-the-wild-mapping-the-public-exposure-of-a-viral-ai-assistant/) ; [Cyberpress](https://cyberpress.org/over-21000-openclaw-ai-instances-found-exposing-personal-configuration-data/) [résumé]
- Censys aurait confirmé **63 070** instances actives par empreinte applicative au 31/03/2026 — [CyberDesserts](https://blog.cyberdesserts.com/openclaw-exposure-numbers-explained/) [résumé]
- Autres décomptes : SecurityScorecard environ 135 000, Penligent plus de 220 000. Un chercheur indépendant a compté à la mi-février 42 665 instances exposées, dont 5 194 vérifiées vulnérables, et 93,4 % présentant des conditions de contournement d'authentification. Répartition : d'abord les États-Unis, puis la Chine et Singapour — [CyberDesserts](https://blog.cyberdesserts.com/openclaw-exposure-numbers-explained/) ; [Hive Security](https://hivesecurity.gitlab.io/blog/openclaw-ai-agent-security-crisis-2026/) [résumé]

**Skills malveillantes (ClawHub)**
- Koi Security (« ClawHavoc ») a audité 2 857 skills : 341 malveillantes, dont 335 issues d'une même campagne — [Koi Security, févr. 2026](https://www.koi.ai/blog/clawhavoc-341-malicious-clawedbot-skills-found-by-the-bot-they-were-targeting) ; [The Hacker News, févr. 2026](https://thehackernews.com/2026/02/researchers-find-341-malicious-clawhub.html) ; [eSecurity Planet](https://www.esecurityplanet.com/threats/hundreds-of-malicious-skills-found-in-openclaws-clawhub/) [résumé]
  - Charges : voleur d'informations macOS de la famille AMOS ; keylogger Windows livré dans un ZIP protégé par mot de passe et hébergé sur GitHub.
  - Déguisements : portefeuilles crypto, bots Polymarket, utilitaires YouTube, auto-updaters et **intégrations Google Workspace**, avec du typosquatting.
  - Mise à jour : plus de 10 700 skills sur la place de marché et **824** malveillantes.
- Antiy Labs recense **1 184** skills malveillantes — [Cyberpress](https://cyberpress.org/clawhavoc-poisons-openclaws-clawhub-with-1184-malicious-skills/) ; [Antiy](https://www.antiy.net/p/clawhavoc-analysis-of-large-scale-poisoning-campaign-targeting-the-openclaw-skill-market-for-ai-agents/) [résumé]. Un hébergeur avance « 1,400+ » ([BetterClaw](https://www.betterclaw.io/blog/openclaw-security-2026), non vérifié). Voir aussi l'analyse de [Palo Alto Networks Unit 42](https://unit42.paloaltonetworks.com/openclaw-ai-supply-chain-risk/) et celle de [SlowMist](https://slowmist.medium.com/threat-intelligence-analysis-of-clawhub-malicious-skills-poisoning-0448ffd49c80) [résumé, titres].
- Réponse du projet en février 2026 : partenariat avec VirusTotal (Google) — [The Hacker News, févr. 2026](https://thehackernews.com/2026/02/openclaw-integrates-virustotal-scanning.html) ; [eSecurity Planet](https://www.esecurityplanet.com/threats/openclaw-adds-virustotal-scanning-to-ai-agent-marketplace/) ; [billet officiel](https://openclaw.ai/blog/virustotal-partnership) (non consulté) [résumé] :
  - chaque skill est vérifiée par empreinte SHA-256 et analysée avec Code Insight (analyse par LLM) ;
  - « benign » : approbation automatique ; « suspicious » : avertissement ; « malicious » : téléchargement bloqué ; nouvelle analyse quotidienne ;
  - contexte : au moins 230 skills malveillantes avaient été publiées depuis le 27/01/2026 ;
  - les mainteneurs préviennent que ce n'est « not a silver bullet », une injection de prompt bien dissimulée pouvant passer.

**Injection de prompt (risque structurel)**
- SECURITY.md exclut de son périmètre — [SECURITY.md](https://github.com/openclaw/openclaw/blob/main/SECURITY.md) [lu] :
  - « Prompt injection without a policy, auth, approval, sandbox, or tool-boundary bypass » ;
  - « A malicious plugin after a trusted operator installs or enables it » ;
  - « Public internet exposure or risky deployment choices that the docs already recommend against ».
  - Il pose aussi le modèle de confiance : « OpenClaw is local-first agent infrastructure for trusted operators; it is not designed as a shared multi-tenant boundary between adversarial users ».
- PromptArmor : l'aperçu de liens de Telegram ou Discord peut servir de canal d'exfiltration via une injection de prompt indirecte. L'agent glisse des données dans une URL de sa réponse et le client charge automatiquement la ressource — [The Hacker News, mars 2026](https://thehackernews.com/2026/03/openclaw-ai-agent-flaws-could-enable.html) [résumé]
- Scénarios de recherche relayés sans attribution vérifiée — sources candidates : [HiddenLayer](https://www.hiddenlayer.com/research/exploring-the-security-risks-of-ai-assistants-like-openclaw), [Giskard](https://www.giskard.ai/knowledge/openclaw-security-vulnerabilities-include-data-leakage-and-prompt-injection-risks), [arXiv 2603.11619 « Taming OpenClaw », mars 2026](https://arxiv.org/pdf/2603.11619), [arXiv 2605.10763 « MATRA », mai 2026](https://arxiv.org/pdf/2605.10763) [résumé] :
  - un e-mail piégé peut demander à l'agent de résumer un message, puis d'exfiltrer discrètement les 5 derniers e-mails ;
  - un attaquant peut modifier le fichier de comportement persistant (SOUL.md) pour créer une tâche planifiée qui réinjecte ses instructions et survit aux redémarrages.
- Demande de fonctionnalité ouverte : « Prompt injection defense at tool result and message boundaries » — [GitHub issue #62939](https://github.com/openclaw/openclaw/issues/62939) [résumé]

**Recommandations officielles de durcissement**
- « Recommended: keep the Gateway loopback-only (`127.0.0.1` / `::1`). Config: `gateway.bind="loopback"` ». Pour l'accès à distance, SSH tunnel ou « Tailscale serve/funnel ». Audit via « `openclaw security audit --deep` and `--fix` » — [SECURITY.md](https://github.com/openclaw/openclaw/blob/main/SECURITY.md) [lu]
- Pairing par défaut, messages entrants traités comme non fiables, guide de sandboxing (docs.openclaw.ai/gateway/sandboxing) — [README](https://github.com/openclaw/openclaw/blob/main/README.md) [lu]

**Avis d'agences nationales**
- **France — CERT-FR/ANSSI, CERTFR-2026-ACT-016 du 13/04/2026**, « Vulnérabilités et risques des produits d'automatisation par IA agentique sur les postes de travail » — [CERT-FR](https://www.cert.ssi.gouv.fr/actualite/CERTFR-2026-ACT-016/) ; [Clubic](https://www.clubic.com/actualite-608878-l-anssi-alerte-openclaw-est-bien-trop-risque-sur-votre-poste-de-travail.html) ; [Silicon.fr](https://www.silicon.fr/cybersecurite-1371/anssi-openclaw-226895) ; [Leto](https://www.leto.legal/news/openclaw-claude-cowork-cert-fr-cert-chinois-2026) [résumé ; le libellé français exact de la recommandation n'a pas pu être lu, site bloqué] :
  - le bulletin vise surtout OpenClaw et mentionne aussi Claude Cowork ;
  - risques : compromission du système, fuite de données sensibles, actions destructrices ; « configuration par défaut extrêmement fragile » ;
  - recommandation, en substance : en l'état, ces assistants ne doivent pas être déployés sur les postes de travail et leur usage doit être interdit tant que le produit n'est pas stabilisé et éprouvé ; usage limité à des environnements de test isolés, sans données sensibles.
- **Chine** :
  - le 11/03/2026, la NVDB rattachée au MIIT publie des lignes directrices : mises à jour officielles, exposition Internet limitée, moindre privilège, prudence sur les places de marché de skills, vigilance face à l'ingénierie sociale et au détournement du navigateur — [Caixin, 12/03/2026](https://www.caixinglobal.com/2026-03-12/tech-brief-march-12-miit-warns-of-security-risks-in-openclaw-102422191.html) ;
  - administrations, entreprises d'État et grandes banques sont invitées à ne pas installer OpenClaw sur leurs postes — [Bloomberg, 11/03/2026](https://www.bloomberg.com/news/articles/2026-03-11/china-moves-to-limit-use-of-openclaw-ai-at-banks-government-agencies) ; [Tom's Hardware](https://www.tomshardware.com/tech-industry/artificial-intelligence/china-bans-openclaw-from-government-computers-and-issues-security-guidelines-amid-adoption-frenzy) ;
  - avertissement pour le secteur de la finance en ligne — [Global Times, mars 2026](https://www.globaltimes.cn/page/202603/1356997.shtml) [résumé].
- **Hong Kong** : les fonctionnaires sont avertis de ne pas installer OpenClaw — [SCMP](https://www.scmp.com/news/hong-kong/society/article/3346552/hong-kong-government-workers-warned-not-install-openclaw-due-security-risks) [résumé]
- **Pays-Bas** : l'Autoriteit Persoonsgegevens (AP) met en garde contre des risques de sécurité majeurs des agents IA comme OpenClaw — [OpenClaws.io, « Governments Are Watching »](https://openclaws.io/blog/openclaw-global-security-regulation/) ; [liste joylarkin/openclaw-security-news](https://github.com/joylarkin/openclaw-security-news) [résumé ; date non vérifiée]
- **Établissements** : l'Université de Toronto a publié un avis — [U of T Information Security](https://security.utoronto.ca/advisories/openclaw-vulnerability-notification/) [résumé]

**Interdictions en entreprise**
- Meta interdit OpenClaw sur les appareils professionnels, avec menace de licenciement. D'autres entreprises tech restreignent aussi son usage (Wired, repris par Slashdot le 19/02/2026). Parmi les autres noms cités : Naver, Massive et Valere ; Google n'est cité que par une source secondaire, non vérifié — [Slashdot, 19/02/2026](https://it.slashdot.org/story/26/02/19/223226/openclaw-security-fears-lead-meta-other-ai-firms-to-restrict-its-use) ; [Trending Topics](https://www.trendingtopics.eu/meta-and-others-restrict-openclaw-while-some-startups-embrace-the-controversial-ai-tool/) ; [Mezha](https://mezha.ua/en/news/companies-ban-employees-from-using-openclaw-308753/) [résumé]

### Inferences
- **Corrigé** : les CVE ponctuelles, dont la version corrective est connue (2026.1.29, 2026.3.11, 2026.3.24, 2026.4.22, puis les correctifs de septembre). Une version récente et mise à jour chaque semaine n'est plus touchée par ces failles publiées.
- **Structurel** (non corrigeable par un patch) :
  - l'injection de prompt, exclue par design du périmètre des vulnérabilités ;
  - le modèle « un opérateur de confiance » ;
  - la chaîne d'approvisionnement des skills tierces (VirusTotal n'est « not a silver bullet ») ;
  - la persistance possible via la mémoire, SOUL.md et les tâches planifiées ;
  - l'exposition réseau due à une mauvaise configuration ;
  - le rythme soutenu de nouveaux avis, dont des « High » en septembre 2026.
- Le cas d'usage visé réunit trois ingrédients :
  - l'accès à des données privées : e-mails, agenda, notes ;
  - l'ingestion de contenus non fiables : e-mails entrants, pages web ;
  - la capacité d'envoyer à l'extérieur : envoi d'e-mails, Telegram et ses aperçus de liens.

  C'est exactement le scénario d'exfiltration démontré par les chercheurs.
- Mesures de réduction du risque qui découlent des sources citées :
  - n'utiliser que les skills fournies avec OpenClaw, jamais celles de ClawHub (les intégrations « Google Workspace » ont justement servi de leurre) ;
  - garder la Gateway en loopback et passer par Tailscale ;
  - activer le pairing ou une allowlist réduite à son propre compte Telegram ;
  - envisager des comptes ou périmètres dédiés ;
  - exiger une approbation avant tout envoi ;
  - mettre à jour rapidement.
- La position du CERT-FR (pas de déploiement sur un poste de travail contenant des données sensibles) pèse fortement pour une entreprise française : au minimum, une machine dédiée et isolée.

### Gaps
- Présence de la CVE-2026-25253 au catalogue CISA KEV et réalité d'une exploitation active : non vérifiées, NVD et CISA étant inaccessibles.
- **BSI (Allemagne) et CISA (États-Unis)** : aucun avis spécifique à OpenClaw trouvé (« Non vérifié / je ne sais pas »).
- Libellé exact en français de la recommandation CERT-FR et date de l'avis de l'AP néerlandaise : non lus.
- Nombre exact de CVE formellement attribuées, par opposition aux GHSA : les chiffres des sources divergent (138 / 543 / ~722 avis).
- Aucun cas documenté d'exploitation visant spécifiquement un particulier ou une TPE via Telegram n'a été trouvé.
