# Ponts open source Telegram ↔ Claude (Claude Code CLI, Agent SDK, API) : état au 28 septembre 2026

Méthode (pour le rédacteur) : toutes les pages ont été consultées le **2026-09-28**. Les compteurs (étoiles, forks, issues, PR, commits) viennent des pages GitHub publiques lues via WebFetch. GitHub arrondit ces nombres (« 2.8k »), et l'API GitHub était bloquée dans cette session, donc aucun compte exact n'est disponible. Les dates de dernier commit viennent des flux Atom `…/commits.atom` de GitHub. Les dates de release viennent de PyPI et du registre npm. Les fonctionnalités viennent des README et des docs lus en version brute (`raw.githubusercontent.com`, branche par défaut au 2026-09-28). Pour les petits projets, la date « Updated » vient de la recherche GitHub. Elle indique une activité récente, mais je ne l'ai pas vérifiée commit par commit. Le seuil d'abandon retenu est « aucun commit depuis 3 mois », soit avant le 2026-06-28. Hors périmètre, à la demande du coordinateur : OpenClaw, NanoClaw, Nanobot, ZeroClaw et le plugin officiel Channels d'Anthropic.

---

## 1. Panorama et métadonnées : popularité, licence, langage, activité des mainteneurs

### Takeaway
Trois ponts sont activement maintenus au 2026-09-28 et comptent une vraie communauté : **chenhg5/cc-connect** (15,7k★, Go, commits le jour même), **overwirehq/claude-code-telegram** (ex-RichardAtCT, 2,8k★, Python, release v1.8.0 du 2026-09-22) et **tiann/hapi** (5,1k★). HAPI n'est toutefois pas un bot de conversation Telegram. Plusieurs projets connus sont **abandonnés selon le critère des 3 mois** : JessyTsui/Claude-Code-Remote, linuz90/claude-telegram-bot, godagoo/claude-telegram-relay, banteg/takopi, hanxiao/claudecode-telegram et op7418/Claude-to-IM-skill. PleasePrompto/ductor et six-ddc/ccbot restent sous ce seuil, mais ralentissent (dernier commit en juillet 2026).

### Cited Findings
**Projets actifs (dernier commit de moins de 3 mois)**
- **overwirehq/claude-code-telegram (anciennement RichardAtCT/claude-code-telegram)** : 2.8k★, 425 forks, 20 issues ouvertes, 18 PR ouvertes, licence MIT, 258 commits (au 2026-09-28). — [Page GitHub](https://github.com/RichardAtCT/claude-code-telegram)
  - Langage principal Python ; le dépôt apparaît désormais sous le nom « overwirehq/claude-code-telegram ». — [Recherche GitHub « claude code telegram »](https://github.com/search?q=claude+code+telegram&type=repositories&s=stars&o=desc)
  - Dernier commit le 2026-09-22T17:59Z. La release « v1.8.0 » a été commitée le 2026-09-22 par RichardAtCT. Un commit du 2026-09-11 s'intitule « repoint URLs at the overwirehq org ». Parmi les autres auteurs récents : FrundlesTian et un compte « claude ». — [Flux commits](https://github.com/RichardAtCT/claude-code-telegram/commits/main.atom)
  - Le README renvoie vers `github.com/overwirehq/claude-code-telegram` et mentionne des fichiers MAINTAINERS.md, CONTRIBUTING.md et ROADMAP-v2. — [README](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/README.md)
  - Le projet a été présenté sur le blog Adafruit le 2026-05-20 (date tirée de l'URL). — [Adafruit blog](https://blog.adafruit.com/2026/05/20/a-telegram-bot-that-provides-remote-access-to-claude-code/)
- **chenhg5/cc-connect** : 15.7k★, 1.6k forks, 239 issues ouvertes, 338 PR ouvertes, MIT, 1 274 commits, dernière release affichée v1.5.1-beta.1 (au 2026-09-28). — [Page GitHub](https://github.com/chenhg5/cc-connect)
  - Langage Go. — [Recherche GitHub](https://github.com/search?q=claude+telegram&type=repositories&s=stars&o=desc)
  - Dernière version npm stable : 1.5.0, publiée le 2026-08-16. Bêta 1.5.1-beta.1 publiée le 2026-08-28. — [Registre npm cc-connect](https://registry.npmjs.org/cc-connect)
  - Cinq commits le 2026-09-28 (entre 07:28 et 08:38 UTC), par cinq contributeurs différents. — [Flux commits](https://github.com/chenhg5/cc-connect/commits.atom)
- **tiann/hapi** : 5.1k★, 580 forks, 89 issues, 177 PR, licence **AGPL-3.0**, 1 530 commits. — [Page GitHub](https://github.com/tiann/hapi)
  - Mis à jour « 12 hours ago » selon la recherche GitHub. — [Recherche multi-dépôts](https://github.com/search?q=repo%3Agodagoo%2Fclaude-telegram-relay+repo%3Asix-ddc%2Fccmux+repo%3Asix-ddc%2Fccbot+repo%3Atiann%2Fhapi+repo%3ANachoSEO%2Fclaudegram+repo%3Asuhocki%2Ftelegram-claude-bridge+repo%3Adolfrin%2FClaude-voice-bridge+repo%3Aterranc%2Fclaude-telegram-bot-bridge+repo%3ANickqiaoo%2Fchatcode+repo%3Aseedprod%2Fclaude-code-telegram&type=repositories&s=stars&o=desc)
  - Paquet npm @twsxtd/hapi en version 0.30.7, publiée le 2026-09-15. — [npm](https://registry.npmjs.org/@twsxtd%2Fhapi)
- **PleasePrompto/ductor** : 458★, 89 forks, 19 issues, 41 PR, MIT, 480 commits. — [Page GitHub](https://github.com/PleasePrompto/ductor)
  - Dernier commit le 2026-07-23. Les cinq derniers commits sont tous de PleasePrompto. — [Flux commits](https://github.com/PleasePrompto/ductor/commits.atom)
  - PyPI 0.20.1 publiée le 2026-07-23 (45 versions au total). — [PyPI ductor](https://pypi.org/project/ductor/)
- **smixs/agent-second-brain** : 388★, 219 forks, 0 issue, 2 PR, MIT, 191 commits. — [Page GitHub](https://github.com/smixs/agent-second-brain)
  - Dernier commit le 2026-08-05, par un contributeur externe (Osamaali313). Dernier commit de l'auteur smixs : v3.0.3, le 2026-06-20. — [Flux commits](https://github.com/smixs/agent-second-brain/commits.atom)
- **six-ddc/ccbot** : 274★, 107 forks, 8 issues, 19 PR, MIT, 90 commits. — [Page GitHub](https://github.com/six-ddc/ccbot)
  - Dernier commit le 2026-07-08, par six-ddc. — [Flux commits](https://github.com/six-ddc/ccbot/commits.atom)
  - Le README pointe désormais les commandes d'installation vers `six-ddc/ccmux`, ce qui suggère un renommage en cours. — [README](https://github.com/six-ddc/ccbot/blob/HEAD/README.md)

**Projets abandonnés (aucun commit depuis plus de 3 mois)**
- **op7418/Claude-to-IM-skill** : 2.9k★, 317 forks, 60 issues, 40 PR, MIT, 46 commits. — [Page GitHub](https://github.com/op7418/Claude-to-IM-skill)
  - Dernier commit le 2026-03-23, soit environ 6 mois. — [Flux commits](https://github.com/op7418/Claude-to-IM-skill/commits.atom)
- **JessyTsui/Claude-Code-Remote** : 1.3k★, 136 forks, 9 issues, 1 PR, MIT, 40 commits. — [Page GitHub](https://github.com/JessyTsui/Claude-Code-Remote)
  - Dernier commit le 2025-12-06, soit environ 10 mois. — [Flux commits](https://github.com/JessyTsui/Claude-Code-Remote/commits.atom)
- **banteg/takopi** : 1.1k★, 137 forks, 16 issues, 33 PR, MIT, 382 commits. — [Page GitHub](https://github.com/banteg/takopi)
  - Dernier commit et dernière release v0.23.4 le 2026-05-25, soit environ 4 mois. — [Flux commits](https://github.com/banteg/takopi/commits.atom) ; [PyPI takopi](https://pypi.org/project/takopi/)
- **hanxiao/claudecode-telegram** : 609★, Python, « Updated Jan 25 » (2026), soit environ 8 mois. — [Recherche GitHub p.2](https://github.com/search?q=claude+telegram&type=repositories&s=stars&o=desc&p=2)
- **linuz90/claude-telegram-bot** : 449★, 115 forks, 5 issues, 8 PR, MIT, 52 commits. — [Page GitHub](https://github.com/linuz90/claude-telegram-bot)
  - TypeScript. — [Recherche GitHub p.3](https://github.com/search?q=claude+telegram&type=repositories&s=stars&o=desc&p=3)
  - Dernier commit le 2026-05-01, soit environ 5 mois. — [Flux commits](https://github.com/linuz90/claude-telegram-bot/commits.atom)
- **coleam00/remote-agentic-coding-system** : 351★, TypeScript, « Updated Nov 29, 2025 ». — [Recherche GitHub p.3](https://github.com/search?q=claude+telegram&type=repositories&s=stars&o=desc&p=3)
- **godagoo/claude-telegram-relay** : 326★, 164 forks, 1 issue, 1 PR, MIT, 18 commits. — [Page GitHub](https://github.com/godagoo/claude-telegram-relay)
  - Dernier commit le 2026-02-15, soit environ 7,5 mois. — [Flux commits](https://github.com/godagoo/claude-telegram-relay/commits.atom)
- Autres petits projets, avec la date « Updated » de la recherche GitHub. — [Recherche multi-dépôts](https://github.com/search?q=repo%3Agodagoo%2Fclaude-telegram-relay+repo%3Asix-ddc%2Fccmux+repo%3Asix-ddc%2Fccbot+repo%3Atiann%2Fhapi+repo%3ANachoSEO%2Fclaudegram+repo%3Asuhocki%2Ftelegram-claude-bridge+repo%3Adolfrin%2FClaude-voice-bridge+repo%3Aterranc%2Fclaude-telegram-bot-bridge+repo%3ANickqiaoo%2Fchatcode+repo%3Aseedprod%2Fclaude-code-telegram&type=repositories&s=stars&o=desc)
  - **NachoSEO/claudegram** : 152★, TS, Mar 24 (abandonné).
  - **terranc/claude-telegram-bot-bridge** : 133★, Python, May 30 (abandonné).
  - **Nickqiaoo/chatcode** : 76★, TS, Jan 29 (abandonné).
  - **seedprod/claude-code-telegram** : 20★, Python, Feb 2 (abandonné).
  - **suhocki/telegram-claude-bridge** : 2★, mis à jour la veille (trop jeune pour juger).
  - **dolfrin/Claude-voice-bridge** : 0★, mis à jour la veille (trop jeune pour juger).

**Hors périmètre, cités pour mémoire (frameworks, applis de bureau ou orchestrateurs, pas des ponts dédiés)** — [Recherche GitHub p.1–3](https://github.com/search?q=claude+telegram&type=repositories&s=stars&o=desc)
- NanmiCoder/cc-haha (14.7k★, espace de travail de bureau).
- composio-community/secure-openclaw (1.2k★, clone d'OpenClaw).
- KroMiose/nekro-agent (1.1k★).
- xvirobotics/metabot (988★).
- firstintent/ccteam (623★).
- Lifecycle-Innovations-Limited/claude-ops (528★, « Business operating system for Claude Code »).
- CoWork-OS (459★).

### Inferences
- Deux projets combinent popularité et maintenance continue : cc-connect (très nombreux contributeurs) et claude-code-telegram, qui dispose d'un processus de release, de CI et d'une organisation GitHub dédiée depuis septembre 2026. Ce sont les seuls choix sûrs pour un fork à long terme.
- ductor reste actif, mais avec un seul mainteneur et 41 PR en attente. Il faut surveiller sa cadence : le dernier commit date de plus de deux mois.
- Le nombre de forks élevé d'agent-second-brain (219 pour 388★) correspond à son modèle d'usage. Le README demande explicitement de forker le dépôt en privé.

### Gaps
- Nombres exacts d'étoiles, de forks et de contributeurs : non disponibles, car l'API GitHub est bloquée dans cette session et les pages n'affichent que des valeurs arrondies. Non vérifié.
- Sens exact de la date « Updated » dans la recherche GitHub (dernier push ou dernière mise à jour des métadonnées) : non vérifié. Pour les projets clés, j'ai recoupé avec les flux de commits.
- Nombre de contributeurs par projet : non affiché dans le rendu WebFetch. Non vérifié.

---

## 2. Comment chaque projet parle à Claude, abonnement ou clé API, et conditions d'Anthropic

### Takeaway
Aucun projet sérieux n'utilise l'API brute. Tous pilotent **Claude Code**, de trois manières :
- **Agent SDK** : claude-code-telegram, linuz90, claudegram, Claude-to-IM-skill, chatcode.
- **CLI en sous-processus headless** (`claude -p` / stream-json) : cc-connect, ductor, godagoo, seedprod, takopi.
- **Session interactive dans tmux** : ccbot, hanxiao, JessyTsui, agent-second-brain.

Tous acceptent la **connexion à l'abonnement Claude** (login du CLI) comme alternative à une clé API. Selon le centre d'aide d'Anthropic (mise à jour du 2026-06-16), l'Agent SDK et `claude -p` **consomment toujours les limites de l'abonnement**, car la bascule vers un crédit séparé prévue au 15 juin 2026 a été **mise en pause**. Les conditions interdisent en revanche aux développeurs tiers de faire passer les identifiants d'abonnement *pour le compte d'autres utilisateurs*.

### Cited Findings
**Agent SDK**
- claude-code-telegram : « Full Claude Code integration with SDK (primary) and CLI (fallback) ». — [README](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/README.md)
  - Option A, recommandée : « Uses the SDK with your existing Claude CLI credentials… No ANTHROPIC_API_KEY needed ». Option B : clé API directe. — [docs/setup.md](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/docs/setup.md)
  - Le 2026-09-22, le projet a quitté la version retirée `claude-agent-sdk 0.1.39` pour la 0.2.157. — [Flux commits](https://github.com/RichardAtCT/claude-code-telegram/commits/main.atom)
- linuz90/claude-telegram-bot : utilise `@anthropic-ai/claude-agent-sdk`. La « CLI Auth (recommended) » utilise l'abonnement Claude Code (« much more cost-effective for heavy usage »). L'alternative est `ANTHROPIC_API_KEY`. — [README](https://github.com/linuz90/claude-telegram-bot/blob/HEAD/README.md)
- NachoSEO/claudegram : Agent SDK, avec `ANTHROPIC_API_KEY` « optional with Claude Max subscription ». — [README](https://github.com/NachoSEO/claudegram/blob/HEAD/README.md)
- op7418/Claude-to-IM-skill : « Claude Agent SDK or Codex SDK (configurable via CTI_RUNTIME) ». — [README](https://github.com/op7418/Claude-to-IM-skill/blob/HEAD/README.md)
- Nickqiaoo/chatcode : « Claude Code SDK ». — [README](https://github.com/Nickqiaoo/chatcode/blob/HEAD/README.md)

**CLI en sous-processus headless**
- cc-connect : le CLI de l'agent doit être installé et authentifié avant cc-connect. — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md)
  - Claude Code est lancé avec `--permission-prompt-tool stdio`. — [docs/usage.md](https://github.com/chenhg5/cc-connect/blob/HEAD/docs/usage.md)
  - Les deux modes d'authentification sont documentés : identifiants OAuth claude.ai (`~/.claude/.credentials.json`) ou `ANTHROPIC_API_KEY`. — [docs/usage.md](https://github.com/chenhg5/cc-connect/blob/HEAD/docs/usage.md)
  - Le README est sponsorisé par de nombreux services « relais d'API » tiers. Exemples : « Claude Code exclusive models at 66% off », et la revente de comptes « Claude Max 200 ». — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md)
- ductor : exécute les CLI officiels en sous-processus, « so you can use your active subscriptions (Claude Max…) directly. No API proxying, no SDK patching ». — [README](https://github.com/PleasePrompto/ductor/blob/HEAD/README.md)
  - Claude est appelé en `--output-format json` ou `stream-json`, avec `--max-turns` et `--max-budget-usd`. — [docs/modules/cli.md](https://github.com/PleasePrompto/ductor/blob/HEAD/docs/modules/cli.md)
- godagoo/claude-telegram-relay : lance `claude -p <prompt> --resume <session> --output-format text`. — [src/relay.ts](https://github.com/godagoo/claude-telegram-relay/blob/HEAD/src/relay.ts)
- seedprod/claude-code-telegram : `Telegram → telegram-bot.py → claude -p "message"`. — [README](https://github.com/seedprod/claude-code-telegram/blob/HEAD/README.md)
- takopi : « works with existing anthropic and openai subscriptions ». Moteurs pris en charge : codex, claude, opencode, pi. — [readme](https://github.com/banteg/takopi/blob/HEAD/readme.md)

**Session interactive dans tmux**
- ccbot : « operates on tmux, not the Claude Code SDK ». Le terminal reste la source de vérité. — [README](https://github.com/six-ddc/ccbot/blob/HEAD/README.md)
- hanxiao : injection par `tmux send-keys`, avec un hook Stop qui renvoie la réponse. — [README](https://github.com/hanxiao/claudecode-telegram/blob/HEAD/README.md)
- JessyTsui : hooks Claude Code et injection par tmux ou PTY. — [README](https://github.com/JessyTsui/Claude-Code-Remote/blob/HEAD/README.md)
- agent-second-brain : « one long-lived interactive Claude Code session » dans tmux, sans `claude -p` dans le chemin critique (un garde-fou en CI l'interdit). — [README](https://github.com/smixs/agent-second-brain/blob/HEAD/README.md)

**Autre cas**
- HAPI enveloppe les sessions officielles Claude Code, Codex et autres, et les pilote à distance depuis des applis natives iOS/Android, une appli Web/PWA ou une Mini App Telegram. — [README](https://github.com/tiann/hapi/blob/HEAD/README.md)

**Conditions et facturation Anthropic**
- Le centre d'aide dit : « Claude Agent SDK usage, the `claude -p` command, and third-party apps built on the Agent SDK » « still draw from your subscription's usage limits ». Il ajoute : « We're pausing the changes to Claude Agent SDK usage described below ». Le crédit mensuel annoncé « isn't available ». Article daté du 2026-06-16, avec une note « Update June 15 ». — [Anthropic Help Center](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan)
- **Contradiction** : le README d'agent-second-brain affirme « Since June 15, 2026, headless `claude -p` runs bill against a separate paid Agent SDK credit ». Le centre d'aide officiel ci-dessus dit l'inverse (changement mis en pause). — [README agent-second-brain](https://github.com/smixs/agent-second-brain/blob/HEAD/README.md) ; [Help Center](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan)
- Page juridique de Claude Code, consultée le 2026-09-28. — [Legal and compliance](https://code.claude.com/docs/en/legal-and-compliance)
  - « Advertised usage limits for Pro and Max plans assume ordinary, individual usage of Claude Code and the Agent SDK. »
  - « OAuth authentication is intended exclusively for purchasers of Claude Free, Pro, Max, Team, and Enterprise subscription plans and is designed to support ordinary use of Claude Code and other native Anthropic applications. »
  - « Developers building products or services… including those using the Agent SDK, should use API key authentication… Anthropic does not permit third-party developers to offer Claude.ai login into their own applications, or to route requests through Free, Pro, or Max plan credentials on behalf of their users. »
  - « Nor does it prevent an end user from signing in to the unmodified Claude Code binary with their own Claude subscription. »
  - « Anthropic reserves the right to take measures to enforce these restrictions and may do so without prior notice. »
- Note de la doc Agent SDK : « Unless previously approved, Anthropic does not allow third party developers to offer claude.ai login or rate limits for their products, including agents built on the Claude Agent SDK. Use the API key authentication methods… instead. » — [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview)

### Inferences
- Le cas visé est un dirigeant qui héberge lui-même un bot personnel sur sa machine ou son VPS, connecté à son propre abonnement, avec le binaire Claude Code non modifié. Il semble correspondre à l'« ordinary, individual usage » toléré. Ce n'est pas un avis juridique.
- Le risque de non-conformité apparaît dans trois situations : partager le bot avec des salariés ou clients sur son abonnement personnel, distribuer le bot comme produit, ou passer par les « relais d'API » à prix cassés promus dans les sponsors de cc-connect. Pour un usage multi-utilisateurs, une clé API Console est la voie documentée.
- Les ponts « tmux / session interactive » (ccbot, agent-second-brain) sont les plus robustes si Anthropic réactive un jour la facturation séparée du SDK et de `claude -p`. Les ponts SDK et headless (claude-code-telegram, cc-connect, ductor, linuz90) seraient alors concernés. Aujourd'hui, la différence de coût est nulle d'après le centre d'aide.

### Gaps
- Évolution de la politique « Agent SDK credit » entre le 2026-06-16 et le 2026-09-28 : je n'ai pas trouvé de source plus récente, car le budget de recherche web de la session était épuisé. Non vérifié.
- Position officielle d'Anthropic sur les bots personnels pilotés via Telegram avec un abonnement Pro/Max : aucune mention explicite trouvée. Je ne sais pas.

---

## 3. Messages vocaux : reconnaissance vocale, français, réponses vocales

### Takeaway
La transcription des notes vocales est **native** dans la plupart des ponts, mais les moteurs diffèrent :
- **claude-code-telegram** : Mistral Voxtral par défaut, OpenAI Whisper ou whisper.cpp **local hors ligne**.
- **cc-connect** : OpenAI ou Groq Whisper, plus **réponses vocales TTS**.
- **linuz90** : OpenAI gpt-4o-transcribe.
- **takopi** : gpt-4o-mini-transcribe, ou un serveur local compatible OpenAI.
- **godagoo** : Groq ou whisper.cpp.
- **ccbot** : OpenAI.
- **ductor** : outil appelé par l'agent, avec une cascade OpenAI Whisper → whisper local.
- **agent-second-brain** : Deepgram, **codé en dur en russe**.

Seuls cc-connect et claudegram (abandonné) documentent des **réponses vocales** (TTS). HAPI, JessyTsui, hanxiao et Claude-to-IM-skill ne transcrivent pas les vocaux Telegram.

### Cited Findings
**claude-code-telegram**
- `VOICE_PROVIDER=mistral|openai|local`. Modèles par défaut : `voxtral-mini-latest` (Mistral), `whisper-1` (OpenAI), `base` (whisper.cpp). Taille maximale `VOICE_MAX_FILE_SIZE_MB=20`. — [docs/configuration.md](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/docs/configuration.md)
- `.env.example` règle `VOICE_PROVIDER=mistral` par défaut. — [.env.example](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/.env.example)
- La doc décrit l'option locale ainsi : « Local whisper.cpp (offline, no API key needed) ». — [docs/setup.md](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/docs/setup.md)
- Le README ne mentionne pas de réponse vocale. — [README](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/README.md)

**cc-connect**
- Section `[speech]` : `provider = "openai"` (`whisper-1`) ou `"groq"` (`whisper-large-v3-turbo`), `language = ""` avec la valeur « "zh", "en", or auto-detect ». Nécessite `ffmpeg`. — [docs/usage.md](https://github.com/chenhg5/cc-connect/blob/HEAD/docs/usage.md)
- Section `[tts]` : fournisseurs `qwen | openai | minimax | mimo | espeak | pico | edge`, modes `voice_only` (répond en vocal si l'utilisateur a envoyé un vocal) ou `always`, commande `cc-connect send --tts`. — [docs/usage.md](https://github.com/chenhg5/cc-connect/blob/HEAD/docs/usage.md)
- La matrice des plateformes indique « Voice / STT / TTS » ✅ pour Telegram. — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md)
- L'interface n'existe qu'en 5 langues : anglais, chinois simplifié et traditionnel, japonais, espagnol. Pas de français. — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md)

**ductor**
- Les fichiers audio et vocaux sont orientés vers `tools/media_tools/transcribe_audio.py`. — [docs/modules/files.md](https://github.com/PleasePrompto/ductor/blob/HEAD/docs/modules/files.md)
- Cascade de transcription : « external hook -> OpenAI Whisper API -> local `whisper` CLI -> `whisper.cpp` ». Un extra Docker « whisper » (Faster Whisper, ~500 Mo) est disponible. — [docs/config.md](https://github.com/PleasePrompto/ductor/blob/HEAD/docs/config.md)

**linuz90**
- Modèle `gpt-4o-transcribe` avec un prompt « The speaker may use multiple languages (English, and possibly others) ». Le contexte est personnalisable via `TRANSCRIPTION_CONTEXT_FILE`. — [src/utils.ts](https://github.com/linuz90/claude-telegram-bot/blob/HEAD/src/utils.ts) ; [src/config.ts](https://github.com/linuz90/claude-telegram-bot/blob/HEAD/src/config.ts)
- Les fichiers audio (mp3, m4a, ogg, wav) sont aussi transcrits. — [README](https://github.com/linuz90/claude-telegram-bot/blob/HEAD/README.md)

**Autres projets**
- godagoo : Groq `whisper-large-v3-turbo` ou whisper.cpp local, sans paramètre de langue dans le code. — [src/transcribe.ts](https://github.com/godagoo/claude-telegram-relay/blob/HEAD/src/transcribe.ts) ; [README](https://github.com/godagoo/claude-telegram-relay/blob/HEAD/README.md)
- takopi : `voice_transcription` désactivé par défaut. Modèle `gpt-4o-mini-transcribe`, ou un serveur Whisper local compatible OpenAI via `voice_transcription_base_url`. — [docs/how-to/voice-notes.md](https://github.com/banteg/takopi/blob/HEAD/docs/how-to/voice-notes.md) ; [docs/reference/config.md](https://github.com/banteg/takopi/blob/HEAD/docs/reference/config.md)
- ccbot : « Voice messages are transcribed via OpenAI and forwarded as text » (`OPENAI_BASE_URL` configurable). — [README](https://github.com/six-ddc/ccbot/blob/HEAD/README.md)
- agent-second-brain : `DeepgramTranscriber` utilise `model="nova-3"` et **`language="ru"`**, codé en dur. — [src/d_brain/services/transcription.py](https://github.com/smixs/agent-second-brain/blob/HEAD/src/d_brain/services/transcription.py)
  - Le README précise : « voice audio goes to Deepgram ». — [README](https://github.com/smixs/agent-second-brain/blob/HEAD/README.md)
- claudegram : transcription par Groq Whisper, et `/tts` qui renvoie la réponse en note vocale via OpenAI `gpt-4o-mini-tts` (13 voix). — [README](https://github.com/NachoSEO/claudegram/blob/HEAD/README.md)
- seedprod : `mlx-whisper` local, réservé à Apple Silicon. — [README](https://github.com/seedprod/claude-code-telegram/blob/HEAD/README.md)
- chatcode : service ASR auto-hébergé Fun-ASR-Nano (~2 Go). — [README](https://github.com/Nickqiaoo/chatcode/blob/HEAD/README.md)
- HAPI : Telegram ne sert qu'aux « Notifications and Mini App integration ». La voix passe par un assistant web (ElevenLabs, Gemini Live, Qwen Realtime). — [docs/guide/how-it-works.md](https://github.com/tiann/hapi/blob/HEAD/docs/guide/how-it-works.md) ; [docs/guide/installation.md](https://github.com/tiann/hapi/blob/HEAD/docs/guide/installation.md)
- Claude-to-IM-skill : aucune transcription vocale Telegram. Seul WeChat est couvert, via sa propre transcription intégrée. — [README](https://github.com/op7418/Claude-to-IM-skill/blob/HEAD/README.md)

**Français dans les modèles Whisper**
- La liste des langues de Whisper contient `"fr": "french"`. C'est la famille de modèles utilisée par whisper-1, Groq whisper-large-v3-turbo, whisper.cpp et faster-whisper. — [openai/whisper tokenizer.py](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py)

### Inferences
- **Transcription de vocaux en français** : tous les ponts basés sur Whisper devraient fonctionner, que ce soit via OpenAI, Groq ou whisper.cpp local, en détection automatique ou avec une langue forcée si l'option existe.
  - agent-second-brain **transcrirait le français comme du russe** sans modification du code. Le correctif est trivial (`language="fr"` ou un mode multilingue), mais c'est un fork obligatoire.
  - Mistral Voxtral, choix par défaut de claude-code-telegram, est un fournisseur européen. C'est potentiellement pertinent pour le RGPD, mais sa qualité en français n'est pas vérifiée ici.
- **Confidentialité** : l'option whisper.cpp locale de claude-code-telegram est la seule, parmi les ponts actifs, à garantir que l'audio ne quitte pas la machine sans travail supplémentaire. takopi peut viser un serveur local, mais le projet est abandonné.
- **Réponses vocales (TTS)** : si c'est un critère, cc-connect est le seul pont actif qui le fait en natif. Les autres demanderaient une adaptation, par exemple ajouter un appel TTS dans le fork.

### Gaps
- Prise en charge et qualité du français par Mistral Voxtral : non vérifié (mistral.ai et huggingface.co bloqués dans cette session).
- Français dans Deepgram Nova-3 : non vérifié (developers.deepgram.com bloqué). De toute façon, le code force « ru ».
- Disponibilité de voix françaises dans les fournisseurs TTS de cc-connect (Edge, OpenAI…) : non vérifié.
- cc-connect accepte-t-il `language="fr"`, alors que la doc ne cite que « zh », « en » ou l'auto-détection ? Non vérifié.

---

## 4. Autres médias (photos, PDF, documents), tâches longues et suivi en temps réel

### Takeaway
Pour les médias entrants, linuz90 (photos, PDF, archives, audio, vidéo) et agent-second-brain (photos, documents, vidéos, posts transférés) sont les plus complets. claude-code-telegram et cc-connect gèrent images et fichiers. cc-connect et linuz90 savent **renvoyer des fichiers** dans le chat. Pour les tâches longues, ductor (tâches de fond, délai de 30 min par défaut) et cc-connect sont les mieux outillés. claude-code-telegram a un **délai par défaut de 300 s** à relever. La plupart affichent la progression en direct (outils utilisés, réflexion).

### Cited Findings
**claude-code-telegram**
- Fonctions listées dans le README :
  - « File upload handling with archive extraction » et « Image/screenshot upload with analysis » ;
  - export de session en Markdown, HTML ou JSON ;
  - `/verbose 0|1|2` pour afficher en temps réel les outils et la réflexion ;
  - indicateur « en train d'écrire » persistant ;
  - `CLAUDE_TIMEOUT_SECONDS=300 # Operation timeout`.
  — [README](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/README.md)

**linuz90**
- Fonctions listées dans le README :
  - photos, PDF, fichiers texte, archives ZIP/TAR, audio, vidéo ;
  - file d'attente des messages pendant que Claude travaille (`/stop` ou le préfixe `!` pour interrompre) ;
  - usage des outils visible en temps réel ;
  - MCP intégré `send_file` pour renvoyer des fichiers et `ask_user` pour afficher des boutons.
  — [README](https://github.com/linuz90/claude-telegram-bot/blob/HEAD/README.md)

**cc-connect**
- Pour Telegram : « Images & files » ✅ et « Streaming / chunked replies » ✅. — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md)
- L'agent peut renvoyer images, PDF et fichiers via `cc-connect send --image/--file` (`attachment_send = "on"` par défaut). — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md) ; [docs/usage.md](https://github.com/chenhg5/cc-connect/blob/HEAD/docs/usage.md)

**ductor**
- Streaming par édition du message en direct. — [README](https://github.com/PleasePrompto/ductor/blob/HEAD/README.md)
- Tâches de fond avec leur propre `TASKMEMORY.md` et un résultat renvoyé dans la conversation (exemple : « Research the top 5 competitors and write a summary »). — [README](https://github.com/PleasePrompto/ductor/blob/HEAD/README.md)
- Sessions nommées, images redimensionnées et converties en WebP. — [README](https://github.com/PleasePrompto/ductor/blob/HEAD/README.md)
- `cli_timeout` par défaut de 1 800 s. — [docs/config.md](https://github.com/PleasePrompto/ductor/blob/HEAD/docs/config.md)

**Autres projets**
- takopi : suivi de progression (commandes, outils, fichiers modifiés, temps écoulé), transfert de fichiers dans les deux sens. — [readme](https://github.com/banteg/takopi/blob/HEAD/readme.md)
- ccbot : notifications pour les réponses, la réflexion et chaque outil utilisé, plus `/screenshot` du terminal. — [README](https://github.com/six-ddc/ccbot/blob/HEAD/README.md)
- agent-second-brain : « Voice…, text, photos, documents, videos, forwarded posts, whole albums — the agent reads files itself ». — [README](https://github.com/smixs/agent-second-brain/blob/HEAD/README.md)
- claudegram : réponses en streaming, et « agent-watchdog.ts » qui surveille les tâches longues. — [README](https://github.com/NachoSEO/claudegram/blob/HEAD/README.md)

### Inferences
- Pour « lancer une recherche, partir, et récupérer le résultat plus tard », les tâches de fond de ductor et les notifications ou tâches planifiées de claude-code-telegram et cc-connect sont les modèles les plus adaptés.
- Avec claude-code-telegram, il faudra augmenter `CLAUDE_TIMEOUT_SECONDS` pour les recherches longues.

### Gaps
- Prise en charge explicite des PDF dans claude-code-telegram et cc-connect : le README parle de « files » sans détailler. Non vérifié.
- Limites de taille des fichiers Telegram gérées par chaque pont : non vérifié en dehors du plafond vocal de 20 Mo de claude-code-telegram et de 10 Mo de takopi.

---

## 5. Sessions et mémoire, multi-projets, MCP (Gmail, Agenda, Notion), tâches planifiées

### Takeaway
**Aucun pont n'intègre nativement Gmail ou Google Agenda.** Tous délèguent à Claude Code, via ses serveurs MCP, ses skills et son CLAUDE.md dans le dossier de travail. Côté planification :
- claude-code-telegram, cc-connect et ductor ont un **planificateur cron intégré** ;
- ductor ajoute un « heartbeat » proactif ;
- agent-second-brain et godagoo (abandonné) proposent briefings et rappels.

Côté mémoire : fichiers Markdown (ductor, agent-second-brain), SQLite (claude-code-telegram) ou Supabase avec recherche sémantique (godagoo).

### Cited Findings
**claude-code-telegram**
- Persistance automatique des sessions par utilisateur et par dossier projet, stockage SQLite avec migrations. — [README](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/README.md)
- Planificateur cron (`ENABLE_SCHEDULER`), serveur webhook (`ENABLE_API_SERVER`), notifications (`NOTIFICATION_CHAT_IDS`). — [README](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/README.md)
- Mode « Project Threads » : un sujet (topic) Telegram par projet. — [README](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/README.md)
- MCP activable via `ENABLE_MCP=false` et `MCP_CONFIG_PATH=/path/to/mcp/config.json`. — [docs/configuration.md](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/docs/configuration.md)

**cc-connect**
- Commandes de session `/new`, `/list`, `/switch`. Rotation vers une nouvelle session après **30 min d'inactivité par défaut** (`reset_on_idle_mins`, 0 pour désactiver). — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md)
- `/memory` pour lire et écrire les fichiers d'instructions de l'agent. — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md)
- `/cron add 0 6 * * * …` et création de tâches cron en langage naturel (« Claude Code auto-creates the cron job »). — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md) ; [docs/usage.md](https://github.com/chenhg5/cc-connect/blob/HEAD/docs/usage.md)
- Multi-projets dans un seul processus. — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md)

**ductor**
- Mémoire persistante en Markdown (`MAINMEMORY.md`, `SHAREDMEMORY.md`) avec maintenance et compaction. — [README](https://github.com/PleasePrompto/ductor/blob/HEAD/README.md)
- Cron avec fuseau horaire, webhooks, « Heartbeat — proactive checks ». — [README](https://github.com/PleasePrompto/ductor/blob/HEAD/README.md)
- Topics, sessions nommées, sous-agents, `project_roots` par topic. — [README](https://github.com/PleasePrompto/ductor/blob/HEAD/README.md)

**linuz90**
- Persistance des sessions, `/resume` parmi les 5 dernières. — [README](https://github.com/linuz90/claude-telegram-bot/blob/HEAD/README.md)
- Fichier MCP `mcp-config.ts` : « Add your own MCP servers (Things, Notion, Typefully, etc.) ». — [README](https://github.com/linuz90/claude-telegram-bot/blob/HEAD/README.md)
- Le guide d'assistant personnel s'appuie sur plusieurs briques. — [Personal Assistant Guide](https://github.com/linuz90/claude-telegram-bot/blob/HEAD/docs/personal-assistant-guide.md)
  - un CLAUDE.md personnel ;
  - des skills `things-todo`, `gmail`, `research` ;
  - un script Google Calendar (`calendar.ts today|tomorrow|week`) ;
  - une intégration Notion ;
  - une « file-based memory » à base de notes synchronisées par iCloud.

**Autres projets**
- godagoo : mémoire Supabase avec embeddings OpenAI et recherche sémantique, faits et objectifs, « smart check-ins » proactifs et briefing matinal. — [README](https://github.com/godagoo/claude-telegram-relay/blob/HEAD/README.md)
- agent-second-brain : mémoire en graphe de connaissances (« autograph »), rappels cron créés en langage naturel, traitement nocturne à 21 h avec rapport quotidien, MCP via `mcp-config.json`, skills. — [README](https://github.com/smixs/agent-second-brain/blob/HEAD/README.md)
- seedprod : skill `daily-brief` planifiée via launchd, exemples de skills Google Calendar et Gmail (lecture seule). — [README](https://github.com/seedprod/claude-code-telegram/blob/HEAD/README.md)
- takopi : projets et worktrees, reprise sans état, tâches planifiées. — [readme](https://github.com/banteg/takopi/blob/HEAD/readme.md) ; [docs/index.md](https://github.com/banteg/takopi/blob/HEAD/docs/index.md)
- ccbot : « 1 Topic = 1 Window = 1 Session », état persistant. — [README](https://github.com/six-ddc/ccbot/blob/HEAD/README.md)

### Inferences
- Les emails, l'agenda et les notes demanderont dans tous les cas d'**ajouter des serveurs MCP ou des skills** (Gmail, Google Calendar, Notion) dans la configuration de Claude Code du dossier de travail. Le guide de linuz90 est le meilleur modèle documenté pour cet usage « assistant personnel », même si le projet est abandonné.
- cc-connect et ductor lancent le CLI `claude` non modifié dans un dossier de travail. Ils devraient donc hériter des MCP configurés côté Claude Code, mais leur documentation ne le dit pas explicitement.
- La rotation de session de cc-connect après 30 min d'inactivité convient à des tâches ponctuelles. Pour un « compagnon » qui se souvient, la mémoire doit vivre dans des fichiers (CLAUDE.md, `/memory`).

### Gaps
- Comportement exact des MCP dans cc-connect et ductor (héritage de `~/.claude.json` ou `.mcp.json`) : non documenté dans les pages lues. Non vérifié.
- Qualité et robustesse des planificateurs (reprise après redémarrage, fuseau Europe/Paris) au-delà de la doc : non vérifié.

---

## 6. Sécurité : allowlist, sandbox, gestion des permissions, secrets, journaux, vulnérabilités

### Takeaway
claude-code-telegram a le modèle de sécurité le plus mûr et le mieux documenté :
- allowlist obligatoire ;
- dossier confiné ;
- limites de débit et de coût ;
- journal d'audit ;
- validation des appels d'outils ;
- **approbation interactive optionnelle** des actions risquées depuis septembre 2026 ;
- signalement privé des vulnérabilités.

Un bug notable (#219) a toutefois été corrigé : les contrôles n'étaient pas consultés dans la configuration par défaut. Plusieurs projets **contournent les permissions par défaut** : linuz90 (non désactivable), ductor (`bypassPermissions` par défaut), hanxiao (skip-permissions sans allowlist). D'autres sont **ouverts à tous par défaut** si l'allowlist est vide : cc-connect, takopi, godagoo, seedprod, chatcode.

### Cited Findings
**claude-code-telegram**
- Protections documentées : allowlist `ALLOWED_USERS`, confinement à `APPROVED_DIRECTORY`, limitation de débit, plafond de coût par utilisateur, journal d'audit, signalement privé via GitHub Security Advisories. — [SECURITY.md](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/SECURITY.md)
- Bug corrigé : avant correctif, les contrôles de périmètre « were wired up but never consulted on a default configuration » (#219). — [SECURITY.md](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/SECURITY.md)
- Limite connue : l'authentification par jeton n'est pas utilisable de bout en bout (#58), d'où la consigne « Use `ALLOWED_USERS` as the access control ». — [SECURITY.md](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/SECURITY.md)
- Approbation interactive, désactivée par défaut : `INTERACTIVE_TOOL_APPROVAL=false`, outils concernés `Bash,Write,Edit`, délai de 60 s, puis `deny` par défaut. — [.env.example](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/.env.example)
  - La fonctionnalité a été ajoutée par la PR #217 le 2026-09-11. — [Flux commits](https://github.com/RichardAtCT/claude-code-telegram/commits/main.atom)
- La validation des commandes Bash dangereuses n'existe qu'en mode classique. Le mode agentique « relies on OS-level sandboxing instead ». — [docs/tools.md](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/docs/tools.md)
- Page Security : « There aren't any published security advisories » (au 2026-09-28). — [Security](https://github.com/overwirehq/claude-code-telegram/security)

**linuz90**
- Contournement total des permissions : « permissionMode: "bypassPermissions" », « allowDangerouslySkipPermissions: true », et « This is not configurable ». — [SECURITY.md](https://github.com/linuz90/claude-telegram-bot/blob/HEAD/SECURITY.md)
- Couches de protection annoncées : allowlist, classification d'intention par IA, validation des chemins, blocage de commandes destructrices, limitation de débit, journal d'audit dans `/tmp/claude-telegram-audit.log`. — [README](https://github.com/linuz90/claude-telegram-bot/blob/HEAD/README.md)

**cc-connect**
- Pour `allow_from` : « Not set (default) → all users are permitted (a WARN will be logged) ». `admin_from` protège `/shell`, `/restart` et `/upgrade`. — [docs/telegram.md](https://github.com/chenhg5/cc-connect/blob/HEAD/docs/telegram.md)
- Modes de permission Claude Code : `default` (« Every tool call requires approval »), `acceptEdits`, `auto`, `plan`, `yolo`. — [docs/usage.md](https://github.com/chenhg5/cc-connect/blob/HEAD/docs/usage.md)
- Isolation `run_as_user` (l'agent tourne sous un autre utilisateur Unix) et `cc-connect doctor user-isolation`. — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md)
- Ni SECURITY.md ni avis publié (« No security policy detected »). — [Security](https://github.com/chenhg5/cc-connect/security)
- Correctif du 2026-09-28 : « stop replying to unauthorized users » (côté Feishu). — [Flux commits](https://github.com/chenhg5/cc-connect/commits.atom)

**ductor**
- Double allowlist. `allowed_user_ids` : « At least one required ». Groupes fermés par défaut. — [README](https://github.com/PleasePrompto/ductor/blob/HEAD/README.md)
- `permission_mode` vaut `"bypassPermissions"` par défaut, et `file_access` vaut `"all"`. — [docs/config.md](https://github.com/PleasePrompto/ductor/blob/HEAD/docs/config.md)
- La détection de prompts suspects ne fait que journaliser (« log warning only »). — [docs/modules/security.md](https://github.com/PleasePrompto/ductor/blob/HEAD/docs/modules/security.md)
- Bac à sable Docker optionnel. — [README](https://github.com/PleasePrompto/ductor/blob/HEAD/README.md)

**Autres projets**
- takopi : « Empty disables sender filtering » pour `allowed_user_ids`. Par défaut `allowed_tools` auto-approuve `["Bash", "Read", "Edit", "Write"]`, avec `dangerously_skip_permissions` à false. — [docs/reference/config.md](https://github.com/banteg/takopi/blob/HEAD/docs/reference/config.md)
- ccbot : `ALLOWED_USERS` obligatoire. Les demandes de permission s'affichent en boutons Telegram. Sur VPS, le README suggère `IS_SANDBOX=1 claude --dangerously-skip-permissions`. — [README](https://github.com/six-ddc/ccbot/blob/HEAD/README.md)
- godagoo : sans `TELEGRAM_USER_ID`, le code affiche « ANY (not recommended) ». — [src/relay.ts](https://github.com/godagoo/claude-telegram-relay/blob/HEAD/src/relay.ts)
- hanxiao : le README fait lancer `claude --dangerously-skip-permissions` derrière un tunnel Cloudflare public. — [README](https://github.com/hanxiao/claudecode-telegram/blob/HEAD/README.md)
  - `bridge.py` accepte le chat_id de tout message entrant (aucune allowlist trouvée par recherche dans le code, au 2026-09-28). — [bridge.py](https://github.com/hanxiao/claudecode-telegram/blob/HEAD/bridge.py)
- seedprod : « If left empty, anyone who discovers your bot's username can… run Claude with full tool access ». — [README](https://github.com/seedprod/claude-code-telegram/blob/HEAD/README.md)
- chatcode : « By default, anyone who finds your bot can use it ». Mode `/bypass` disponible. — [README](https://github.com/Nickqiaoo/chatcode/blob/HEAD/README.md)
- claudegram : `acceptEdits` par défaut, `DANGEROUS_MODE` sur demande explicite, protection SSRF. — [README](https://github.com/NachoSEO/claudegram/blob/HEAD/README.md)
- Claude-to-IM-skill : approbation des outils par boutons Allow/Deny, logs expurgés des secrets. — [README](https://github.com/op7418/Claude-to-IM-skill/blob/HEAD/README.md)
- agent-second-brain : `ALLOWED_USER_IDS`, `.env` en chmod 600, « permission hardening » à l'installation. — [README](https://github.com/smixs/agent-second-brain/blob/HEAD/README.md)

### Inferences
- Un compagnon qui lit des **emails** est exposé à l'injection de prompt : un email piégé peut donner des instructions à l'agent. Les ponts en contournement total (linuz90, ductor par défaut, hanxiao) sont risqués s'ils ont aussi accès à l'envoi d'emails ou au shell.
- Trois ponts permettent une approbation humaine depuis Telegram :
  - claude-code-telegram avec `INTERACTIVE_TOOL_APPROVAL=true` ;
  - cc-connect en mode `default` ;
  - ccbot et Claude-to-IM-skill, par boutons.
- Point de configuration à ne pas oublier dans un fork : **renseigner l'allowlist**. Sinon, cc-connect, takopi, godagoo, seedprod et chatcode sont ouverts à quiconque trouve le nom du bot.
- La correction rapide de #219 et la politique de signalement privé montrent un mainteneur réactif sur la sécurité chez claude-code-telegram. Elles montrent aussi qu'une configuration « par défaut » peut être moins sûre que ce qu'annonce le README.

### Gaps
- Audit de sécurité indépendant ou CVE pour l'un de ces projets : aucun trouvé (recherche limitée, budget de recherche épuisé). Je ne sais pas.
- Revue des issues ouvertes à la recherche de failles non publiées : non réalisée.

---

## 7. Déploiement, qualité de la documentation, difficulté d'installation

### Takeaway
cc-connect et ductor sont les plus simples pour un non-développeur :
- cc-connect : binaire npm, brew ou téléchargement direct, puis **interface web d'administration**, et même une installation « par un agent IA » ;
- ductor : `pipx install` puis **assistant d'installation**, avec service système Linux, macOS ou Windows.

claude-code-telegram demande Python 3.11, un `.env` et un guide systemd : c'est faisable, avec une documentation abondante. agent-second-brain s'installe en **une commande sur un VPS Ubuntu**. godagoo et seedprod se font installer « par Claude Code » en lisant leur CLAUDE.md, mais ils sont abandonnés.

### Cited Findings
**claude-code-telegram**
- Installation depuis un tag de release avec `uv tool install` ou `pip`, Python 3.11+, lancement par `make run`. — [README](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/README.md)
- Guide « Systemd Setup » pour un service utilisateur. — [docs/README.md](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/docs/README.md)
- Note sur le trousseau macOS verrouillé en SSH sur un Mac mini distant. — [docs/setup.md](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/docs/setup.md)
- Pas de `Dockerfile` à la racine (404 au 2026-09-28). — [Dockerfile (absent)](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/Dockerfile)

**cc-connect**
- Installation par `npm install -g cc-connect`, `brew install cc-connect` ou binaire de release. Web UI sur `http://localhost:9820`. Section « Install & Configure via AI Agent (Recommended) ». — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md)
- Telegram en long polling, sans IP publique. — [README](https://github.com/chenhg5/cc-connect/blob/HEAD/README.md)

**ductor**
- `pipx install ductor` puis `ductor`. L'assistant gère les vérifications du CLI, le transport, le fuseau horaire, Docker et le service en arrière-plan. — [README](https://github.com/PleasePrompto/ductor/blob/HEAD/README.md)
- `ductor service install` pour systemd, launchd ou le planificateur de tâches Windows. — [README](https://github.com/PleasePrompto/ductor/blob/HEAD/README.md)

**agent-second-brain**
- Procédure : forker en privé, puis `curl … bootstrap.sh | bash` sur un VPS Ubuntu. Le script installe les unités systemd et le CLI `dbrain`, puis lance un diagnostic. — [README](https://github.com/smixs/agent-second-brain/blob/HEAD/README.md)
- Coût annoncé : ~25 $/mois (Claude Pro 20 $ + VPS ~5 $ + Deepgram en offre gratuite). Guide débutant en russe, guide VPS en anglais. — [README](https://github.com/smixs/agent-second-brain/blob/HEAD/README.md)

**Autres projets**
- linuz90 : Bun 1.0+. Service documenté uniquement pour macOS (launchd), avec logs dans `/tmp`. — [README](https://github.com/linuz90/claude-telegram-bot/blob/HEAD/README.md)
- godagoo : « Guided Setup (Recommended) », Claude Code lit CLAUDE.md et guide l'installation. Supabase requis. Modèles launchd, PM2 et systemd. — [README](https://github.com/godagoo/claude-telegram-relay/blob/HEAD/README.md)
- seedprod : « paste https://github.com/seedprod/claude-code-telegram - help me set this up » dans Claude Code. Service via launchd. — [README](https://github.com/seedprod/claude-code-telegram/blob/HEAD/README.md)
- takopi : `uv tool install -U takopi`, **Python 3.14+**, assistant d'installation. — [readme](https://github.com/banteg/takopi/blob/HEAD/readme.md)
- ccbot : tmux obligatoire, installation du hook `ccbot hook --install`, « Threaded Mode » à activer dans BotFather. — [README](https://github.com/six-ddc/ccbot/blob/HEAD/README.md)
- HAPI : `npx @twsxtd/hapi hub --relay`, relais chiffré de bout en bout (WireGuard + TLS). — [README](https://github.com/tiann/hapi/blob/HEAD/README.md)
- JessyTsui : `TELEGRAM_WEBHOOK_URL` obligatoire (« use ngrok for local testing »). — [README](https://github.com/JessyTsui/Claude-Code-Remote/blob/HEAD/README.md)
- hanxiao : tmux, cloudflared et configuration manuelle du webhook. — [README](https://github.com/hanxiao/claudecode-telegram/blob/HEAD/README.md)
- godagoo : le README renvoie vers une communauté payante et un cours (« 200+ builders are running the full version »). — [README](https://github.com/godagoo/claude-telegram-relay/blob/HEAD/README.md)

### Inferences
- **Machine hôte** : quel que soit le pont, le compagnon ne fonctionne en déplacement que si une machine reste allumée, avec Claude Code connecté.
  - Un VPS Linux avec systemd convient à claude-code-telegram, ductor, cc-connect et agent-second-brain.
  - Un Mac à domicile est une alternative (attention à la mise en veille et au trousseau).
- Les solutions en long polling (cc-connect, claude-code-telegram, ductor, linuz90…) évitent d'exposer un port. C'est préférable aux solutions à webhook public (hanxiao, JessyTsui).
- Pour un fork avec l'aide de Claude Code, un code Python ou TypeScript compact et testé est plus facile à adapter qu'un gros projet Go multi-plateformes comme cc-connect. En contrepartie, cc-connect peut s'utiliser sans fork, par configuration seule.

### Gaps
- Temps réel d'installation pour un non-développeur : aucune source indépendante. Non vérifié.
- Images Docker officielles pour cc-connect ou claude-code-telegram : aucune trouvée dans les pages lues. Non vérifié.

---

## 8. Tableau comparatif et top 3 classé (pour un dirigeant non-développeur en France)

### Takeaway
Classement recommandé au 2026-09-28 :
1. **overwirehq/claude-code-telegram** : le meilleur équilibre entre activité, sécurité, voix (dont Mistral et whisper.cpp local), planificateur et MCP.
2. **chenhg5/cc-connect** : le plus populaire et le plus actif, installation sans code, voix entrante et sortante, cron en langage naturel. Codebase lourde, allowlist à configurer impérativement.
3. **PleasePrompto/ductor** : interface en français, assistant d'installation, cron, heartbeat, mémoire, service système. Contournement des permissions par défaut et mainteneur unique à surveiller.

Alternatives selon le profil :
- **smixs/agent-second-brain** pour des « notes vocales vers Obsidian » sur VPS, à condition de corriger la langue codée en dur (`ru`).
- **linuz90** comme modèle d'assistant personnel, mais il est abandonné et en contournement total des permissions.

### Cited Findings
Tableau de synthèse, valeurs au 2026-09-28. Chaque ligne s'appuie sur les sources détaillées dans les sections 1 à 7.

| Projet | ★ / forks | Licence · langage | Dernier commit (release) | Issues ouv. | Moteur Claude · auth | Voix entrante → FR | Réponse vocale | Planif. · MCP | Sécurité clé | Statut | Sources |
|---|---|---|---|---|---|---|---|---|---|---|---|
| **overwirehq/claude-code-telegram** (ex-RichardAtCT) | 2.8k / 425 | MIT · Python | 2026-09-22 (v1.8.0) | 20 | Agent SDK (+ CLI) · abonnement ou clé API | Mistral Voxtral (défaut), OpenAI Whisper, whisper.cpp local | Non mentionnée | Cron + webhooks · MCP (`MCP_CONFIG_PATH`) | Allowlist, dossier confiné, audit, limites de coût, approbation interactive (désactivée par défaut), bug #219 corrigé | **Actif** | [GitHub](https://github.com/RichardAtCT/claude-code-telegram), [commits](https://github.com/RichardAtCT/claude-code-telegram/commits/main.atom), [config](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/docs/configuration.md), [SECURITY](https://github.com/RichardAtCT/claude-code-telegram/blob/HEAD/SECURITY.md) |
| **chenhg5/cc-connect** | 15.7k / 1.6k | MIT · Go | 2026-09-28 (npm 1.5.0 du 2026-08-16) | 239 | CLI headless (`--permission-prompt-tool stdio`) · OAuth ou clé | OpenAI / Groq Whisper, langue auto | **Oui** (OpenAI, Edge, Qwen, MiniMax…) | Cron en langage naturel · via Claude Code (inféré) | `allow_from` ouvert par défaut, mode `default` avec approbation par outil, `run_as_user`, pas de SECURITY.md | **Très actif** | [GitHub](https://github.com/chenhg5/cc-connect), [usage](https://github.com/chenhg5/cc-connect/blob/HEAD/docs/usage.md), [telegram](https://github.com/chenhg5/cc-connect/blob/HEAD/docs/telegram.md) |
| **PleasePrompto/ductor** | 458 / 89 | MIT · Python | 2026-07-23 (0.20.1) | 19 | CLI officiel (stream-json) · abonnement | Outil agent : OpenAI Whisper → whisper local | Non trouvée | Cron, webhooks, heartbeat · via Claude Code (inféré) | Double allowlist, `bypassPermissions` par défaut, Docker optionnel | Actif (mono-mainteneur) | [GitHub](https://github.com/PleasePrompto/ductor), [config](https://github.com/PleasePrompto/ductor/blob/HEAD/docs/config.md) |
| **smixs/agent-second-brain** | 388 / 219 | MIT · Python | 2026-08-05 (v3.0.3, 2026-06-20) | 0 | Session interactive tmux · abonnement | Deepgram Nova-3, **`ru` codé en dur** | Non | Rappels cron, rapport nocturne · `mcp-config.json` | Allowlist, `.env` en 600 | Actif (ralenti) | [GitHub](https://github.com/smixs/agent-second-brain), [transcription.py](https://github.com/smixs/agent-second-brain/blob/HEAD/src/d_brain/services/transcription.py) |
| **six-ddc/ccbot** | 274 / 107 | MIT · Python | 2026-07-08 | 8 | tmux (session interactive) | OpenAI | Non | – · via Claude Code | Allowlist, permissions par boutons | Limite (≈2,7 mois) | [GitHub](https://github.com/six-ddc/ccbot), [README](https://github.com/six-ddc/ccbot/blob/HEAD/README.md) |
| **tiann/hapi** | 5.1k / 580 | **AGPL-3.0** · TS | Actif (npm 0.30.7 du 2026-09-15) | 89 | Enveloppe les sessions officielles | Assistant vocal web (ElevenLabs, Gemini, Qwen), **pas de vocaux Telegram** | Assistant vocal web | – | Relais chiffré de bout en bout | Actif, **hors cible** (Mini App + notifications) | [GitHub](https://github.com/tiann/hapi), [how-it-works](https://github.com/tiann/hapi/blob/HEAD/docs/guide/how-it-works.md) |
| **linuz90/claude-telegram-bot** | 449 / 115 | MIT · TS (Bun) | 2026-05-01 | 5 | Agent SDK · abonnement ou clé | OpenAI gpt-4o-transcribe | Non | – · MCP (`mcp-config.ts`) + guide assistant personnel | **Contournement total non désactivable**, allowlist, audit | **Abandonné** (≈5 mois) | [GitHub](https://github.com/linuz90/claude-telegram-bot), [SECURITY](https://github.com/linuz90/claude-telegram-bot/blob/HEAD/SECURITY.md) |
| **banteg/takopi** | 1.1k / 137 | MIT · Python 3.14 | 2026-05-25 (0.23.4) | 16 | CLI multi-moteurs · abonnements | OpenAI gpt-4o-mini-transcribe ou serveur local | Non | Messages planifiés | Allowlist vide = ouvert | **Abandonné** (≈4 mois) | [GitHub](https://github.com/banteg/takopi), [config](https://github.com/banteg/takopi/blob/HEAD/docs/reference/config.md) |
| **godagoo/claude-telegram-relay** | 326 / 164 | MIT · TS (Bun) | 2026-02-15 | 1 | `claude -p` · abonnement | Groq Whisper ou whisper.cpp | Non | Check-ins, briefing · via Claude Code | Allowlist optionnelle | **Abandonné** (≈7,5 mois) | [GitHub](https://github.com/godagoo/claude-telegram-relay), [relay.ts](https://github.com/godagoo/claude-telegram-relay/blob/HEAD/src/relay.ts) |
| **op7418/Claude-to-IM-skill** | 2.9k / 317 | MIT · TS | 2026-03-23 | 60 | Agent SDK ou Codex SDK | **Pas de vocaux Telegram** | Non | – | Permissions par boutons | **Abandonné** (≈6 mois) | [GitHub](https://github.com/op7418/Claude-to-IM-skill) |
| **JessyTsui/Claude-Code-Remote** | 1.3k / 136 | MIT · JS | 2025-12-06 | 9 | Hooks + tmux/PTY | Non | Non | – | Allowlist par ID, webhook public (ngrok) | **Abandonné** (≈10 mois) | [GitHub](https://github.com/JessyTsui/Claude-Code-Remote) |
| **hanxiao/claudecode-telegram** | 609 / non vérifié | non vérifié · Python | « Updated Jan 25 » | non vérifié | tmux + hook Stop | Non | Non | – | **Pas d'allowlist trouvée + skip-permissions** | **Abandonné** | [README](https://github.com/hanxiao/claudecode-telegram/blob/HEAD/README.md), [bridge.py](https://github.com/hanxiao/claudecode-telegram/blob/HEAD/bridge.py) |
| NachoSEO/claudegram | 152 / non vérifié | MIT · TS | « Updated Mar 24 » | non vérifié | Agent SDK | Groq Whisper | **Oui** (OpenAI TTS) | MCP intégré | `acceptEdits` par défaut | Abandonné | [README](https://github.com/NachoSEO/claudegram/blob/HEAD/README.md) |
| seedprod/claude-code-telegram | 20 / non vérifié | MIT · Python | « Updated Feb 2 » | non vérifié | `claude -p` | mlx-whisper local (Mac Apple Silicon) | Non | Brief quotidien (launchd) | Allowlist vide = ouvert | Abandonné, gabarit minimal | [README](https://github.com/seedprod/claude-code-telegram/blob/HEAD/README.md) |

### Inferences
**Top 3 classé, avec justification**

**1. overwirehq/claude-code-telegram (ex-RichardAtCT)** : recommandé en premier.
- *Pour* :
  - c'est le pont **dédié à Telegram** le plus populaire (2.8k★) ;
  - il est **activement maintenu** (release du 2026-09-22, CI, organisation GitHub dédiée, gouvernance documentée) ;
  - sa **sécurité** est la plus sérieuse : allowlist, confinement, audit, plafonds de coût, approbation interactive des actions risquées, politique de signalement ;
  - la **voix** couvre trois options : Mistral (européen), OpenAI, ou **whisper.cpp local** (l'audio ne quitte pas la machine) ;
  - il intègre **planificateur, webhooks, notifications et MCP**, ce qu'il faut pour briefings, emails et agenda ;
  - le code Python est testé (`make test`) et facile à faire adapter par Claude Code.
- *Contre* :
  - le vocabulaire et l'interface sont orientés « code » (projets, dépôts) ;
  - pas de réponse vocale ;
  - délai par défaut de 300 s à relever ;
  - pas de Docker officiel ;
  - activer `INTERACTIVE_TOOL_APPROVAL=true` est fortement conseillé dès qu'on branche un accès email.

**2. chenhg5/cc-connect** : le plus facile à installer, et le plus riche en voix.
- *Pour* :
  - la plus grande communauté (15.7k★, commits quotidiens) ;
  - installation sans compilation, avec **interface web d'administration** ;
  - **voix entrante et réponses vocales** (TTS) ;
  - envoi de fichiers et PDF dans le chat ;
  - **cron en langage naturel** ;
  - le mode par défaut demande l'approbation de chaque outil dans le chat ;
  - isolation par utilisateur Unix.
- *Contre* :
  - **`allow_from` ouvert par défaut**, à renseigner impérativement ;
  - pas de SECURITY.md ;
  - codebase Go très large (13 plateformes, plus de 10 agents), donc difficile à forker en profondeur. Mieux vaut l'utiliser par configuration ;
  - interface non francisée ;
  - sessions réinitialisées après 30 min d'inactivité ;
  - le README fait la promotion de « relais d'API » tiers à éviter, pour des raisons de conformité avec les conditions d'Anthropic (voir section 2) ;
  - 239 issues et 338 PR ouvertes : évolution rapide, donc risque de régressions.

**3. PleasePrompto/ductor** : le plus orienté assistant proactif, avec interface en français.
- *Pour* :
  - **interface en français** (`"language": "fr"`) ;
  - assistant d'installation et service système sur les trois OS ;
  - CLI officiel, donc usage de l'abonnement ;
  - **cron avec fuseau horaire, heartbeat proactif, webhooks** ;
  - mémoire Markdown persistante ;
  - **tâches de fond** (exemple : recherche sur des concurrents) ;
  - bac à sable Docker optionnel ;
  - allowlist obligatoire.
- *Contre* :
  - **`bypassPermissions` par défaut**, à changer ou à compenser par le bac à sable Docker ;
  - la voix passe par un outil appelé par l'agent, pas par un pipeline natif ;
  - mainteneur quasi unique, dernier commit le 2026-07-23 : à surveiller, car le seuil d'abandon serait franchi fin octobre 2026.

**Mentions selon le profil**
- **smixs/agent-second-brain** : conception la plus aboutie pour la « capture vocale vers notes organisées (Obsidian) », avec rappels et rapport quotidien, sur un VPS installé en une commande. Il utilise une session interactive, la plus prudente vis-à-vis de la facturation. Il faut **modifier `language="ru"`** pour le français, accepter Deepgram (cloud) et une documentation surtout en russe. Il est moins adapté aux emails et à l'agenda sans ajout de MCP.
- **linuz90/claude-telegram-bot** : son guide « Personal Assistant » (Gmail, Calendar, Notion, todo) est **le meilleur modèle d'usage** pour un dirigeant, à reprendre comme inspiration dans le pont choisi. Le projet lui-même est déconseillé : abandonné depuis mai 2026 et en contournement total des permissions non désactivable.

**À écarter pour ce besoin**
- HAPI : pas un bot de conversation Telegram, licence AGPL.
- JessyTsui/Claude-Code-Remote et hanxiao : abandonnés ; hanxiao n'a pas d'allowlist trouvée et contourne les permissions.
- takopi : abandonné, orienté worktrees de code, ouvert par défaut.
- ccbot : bon outil de pilotage de sessions de code, pas un assistant.
- Claude-to-IM-skill : abandonné, pas de vocaux Telegram.
- claudegram, chatcode, seedprod, coleam00 : abandonnés.

### Gaps
- Aucun test pratique n'a été fait. Le classement repose sur la documentation et les métadonnées, pas sur un essai réel, notamment la qualité de la transcription en français.
- Les retours d'utilisateurs non-développeurs (tutoriels datés, avis) n'ont pas pu être recherchés, faute de budget de recherche web. La seule couverture presse repérée est le billet Adafruit du 2026-05-20 sur claude-code-telegram.
- La pérennité de ductor (mainteneur unique) et la réactivation éventuelle de la facturation séparée Agent SDK / `claude -p` par Anthropic sont des incertitudes à réévaluer avant de choisir. Je ne sais pas.
