# Compagnon Telegram voix/texte : Bot API, STT/TTS en français et hébergement en Europe (état au 28/09/2026)

> **Note méthodologique (à lire avant d'utiliser ces notes).** Recherche effectuée le 28/09/2026. Le proxy réseau de l'environnement bloquait le chargement direct (WebFetch) de la plupart des sources primaires : core.telegram.org, telegram.org, t.me, grammy.dev, docs.python-telegram-bot.org, openai.com (et platform./developers.), groq.com (et console.), mistral.ai (et docs.), deepgram.com, elevenlabs.io, huggingface.co, arxiv.org, en.wikipedia.org, decodeur-ia.com. En conséquence :
> 1. Les limites de la Bot API proviennent du **code source de python-telegram-bot (PTB)**, qui recopie les docstrings et constantes officielles, lu via git (commit `93968d8` du 27/09/2026).
> 2. Les prix des fournisseurs proviennent d'**extraits du moteur de recherche restreint au domaine officiel** (marqués « extrait de recherche » : la page n'a pas été chargée, le texte est un résumé du moteur, la date de publication est souvent invisible) et/ou de la **base de prix communautaire LiteLLM** (GitHub, commit `90e4962` du 28/09/2026 ; chaque entrée y cite la page officielle), marquée « base LiteLLM ». Ce sont des sources secondaires : **à revérifier sur les pages officielles avant toute décision**. Quand les deux concordent, je le signale.
> 3. Le quota de recherches web de la session a été épuisé en fin de travail ; certains points sont donc restés en « Gaps ».
> 4. Prix en **USD hors taxes** pour les API, en **EUR** pour les hébergeurs (HT sauf mention). Aucune conversion USD→EUR n'a été faite (pas de taux de change sourcé).

## 1. Telegram Bot API : version 2026, création avec BotFather, format des notes vocales, limites (fichiers, messages, débit), webhooks vs long polling, réponses vocales, restriction à un seul utilisateur

### Takeaway
La version la plus récente trouvée est la Bot API 10.3 (fin août 2026). Pour un bot mono-utilisateur, les limites standard suffisent largement (téléchargement 20 Mo, envoi 50 Mo, 4 096 caractères par message, ~1 message/s par chat). Le long polling est la solution la plus simple (ni IP publique ni certificat) ; le webhook ne se justifie qu'en serverless ou à fort trafic.

### Cited Findings
**Version actuelle et nouveautés 2026**
- aiogram v3.31.0 (publiée sur PyPI le 26/08/2026) : « Added full support for the Bot API 10.3 ». Versions précédentes : v3.30.0 (17/07/2026) pour la 10.2, v3.29.0 (14/06/2026) pour la 10.1, v3.28.0 (≈ 08/05/2026) pour la 10.0, v3.27.0 pour la 9.6, v3.26.0 pour la 9.5, v3.25.0 pour la 9.4 — [aiogram, page « Releases » sur GitHub, consultée le 28/09/2026](https://github.com/aiogram/aiogram/releases). Dates confirmées par l'API JSON de PyPI (3.31.0 : 2026-08-26T00:00Z) — [PyPI, JSON aiogram, consulté le 28/09/2026](https://pypi.org/pypi/aiogram/json). *Attention : l'outil de lecture de GitHub a affiché « 2024 » pour ces releases, ce que contredit PyPI.*
- grammY v1.46.0 (npm, 26/08/2026) prend en charge la Bot API 10.3. Versions précédentes : v1.45.x (16–17/07/2026) pour la 10.2, v1.44.0 (14/06/2026) pour la 10.1, v1.43.0 (16/05/2026) pour la 10.0, v1.42.0 (03/04/2026) pour la 9.6 — [grammY, « Releases » sur GitHub](https://github.com/grammyjs/grammY/releases) ; dates tirées du [registre npm « grammy », consulté le 28/09/2026](https://registry.npmjs.org/grammy).
- Selon l'extrait de recherche, la Bot API 10.3 a été « released on August 24, 2026 ». Elle apporte les « Rich Messages » (espace de noms `InputRichBlock`, 24 blocs concrets) et les messages éphémères (`editEphemeralMessageText`, `deleteEphemeralMessage`, etc.). Sources agrégées par le moteur : [PR python-telegram-bot #5359](https://github.com/python-telegram-bot/python-telegram-bot/pull/5359), [release rubenlagus/TelegramBots v10.3.0](https://github.com/rubenlagus/TelegramBots/releases/tag/v10.3.0) et [release pengrad/java-telegram-bot-api 10.3.0](https://github.com/pengrad/java-telegram-bot-api/releases/tag/10.3.0) (pages non chargées ; date à confirmer sur core.telegram.org/bots/api-changelog).
- La Bot API 9.5 (mars 2026) ouvre `sendMessageDraft` à tous les bots, ce qui permet le « streaming » natif des réponses générées par une IA — [AIBase, « Bot API 9.5 Major Update… », 2026, extrait de recherche](https://news.aibase.com/news/25881). La Bot API 9.6 est datée du 3 avril 2026 — [go-telegram/bot, issue #279, GitHub](https://github.com/go-telegram/bot/issues/279).
- Une recherche dédiée n'a trouvé aucune mention d'une « Bot API 10.4 » au 28/09/2026 — [résultats de recherche incluant core.telegram.org/bots/api-changelog, 28/09/2026](https://core.telegram.org/bots/api-changelog).

**Création du bot (BotFather)**
- « Obtaining a token is as simple as contacting @BotFather, issuing the /newbot command and following the steps ». BotFather demande un nom puis un username : 5 à 32 caractères, lettres latines, chiffres ou underscores, terminé obligatoirement par « bot ». Il génère ensuite un token de la forme `110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw`. Ce token doit être gardé comme un mot de passe, car « it can be used by anyone to control your bot » — [Telegram, « From BotFather to 'Hello World' », core.telegram.org/bots/tutorial, extrait de recherche](https://core.telegram.org/bots/tutorial).

**Notes vocales reçues (objet `Voice`) et téléchargement**
- Champs de l'objet `Voice` : `file_id`, `file_unique_id`, `duration` (en secondes, « as defined by the sender »), `mime_type` (optionnel, défini par l'expéditeur) et `file_size` (optionnel, en octets) — [python-telegram-bot, `src/telegram/_files/voice.py`, commit 93968d8 du 27/09/2026](https://github.com/python-telegram-bot/python-telegram-bot/blob/93968d89d7896c72664cfde0b45526795e58cb98/src/telegram/_files/voice.py).
- `getFile` : « For the moment, bots can download files of up to 20MB in size… It is guaranteed that the link will be valid for at least 1 hour. When the link expires, a new one can be requested by calling get_file again. » Le nom et le type MIME d'origine peuvent ne pas être conservés — [python-telegram-bot, `src/telegram/_bot.py` (docstring de get_file), commit 93968d8](https://github.com/python-telegram-bot/python-telegram-bot/blob/93968d89d7896c72664cfde0b45526795e58cb98/src/telegram/_bot.py).
- Constantes correspondantes : `FILESIZE_DOWNLOAD = 20e6` (« Bots can download files of up to 20MB »), `FILESIZE_UPLOAD = 50e6` (« non-photo files of up to 50MB »), `PHOTOSIZE_UPLOAD = 10e6` — [python-telegram-bot, `src/telegram/constants.py`, commit 93968d8](https://github.com/python-telegram-bot/python-telegram-bot/blob/93968d89d7896c72664cfde0b45526795e58cb98/src/telegram/constants.py).

**Serveur Bot API local (Local Bot API Server)**
- Lancé avec `--local`, le serveur autorise : téléchargements « without size restrictions », envois jusqu'à 2 000 Mo, chemins locaux pour les envois, webhooks en HTTP simple (sans HTTPS), IP locale et n'importe quel port pour le webhook, jusqu'à 100 000 connexions webhook, et `getFile` qui renvoie un chemin local absolu. « The only mandatory options are `--api-id` and `--api-hash` » (identifiants obtenus via core.telegram.org/api/obtaining_api_id) — [tdlib/telegram-bot-api, README GitHub, consulté le 28/09/2026](https://github.com/tdlib/telegram-bot-api).
- Constantes miroir côté PTB : `FILESIZE_UPLOAD_LOCAL_MODE = 2e9` et `FILESIZE_DOWNLOAD_LOCAL_MODE = sys.maxsize` — [PTB, constants.py](https://github.com/python-telegram-bot/python-telegram-bot/blob/93968d89d7896c72664cfde0b45526795e58cb98/src/telegram/constants.py).

**Réponses vocales (`sendVoice`)**
- `sendVoice` : « your audio must be in an .ogg file encoded with OPUS, or in .MP3 format, or in .M4A format (other formats may be sent as Audio or Document). Bots can currently send voice messages of up to [FILESIZE_UPLOAD = 50 MB] in size » — [PTB, `_bot.py`, docstring de send_voice](https://github.com/python-telegram-bot/python-telegram-bot/blob/93968d89d7896c72664cfde0b45526795e58cb98/src/telegram/_bot.py).
- Note propre à PTB (constante `VOICE_NOTE_FILE_SIZE = 1e6`) : « Bots can send audio/ogg files of up to 1MB in size as a voice note. Larger voice notes (up to 20MB) will be sent as files » — [PTB, constants.py](https://github.com/python-telegram-bot/python-telegram-bot/blob/93968d89d7896c72664cfde0b45526795e58cb98/src/telegram/constants.py).

**Limites de longueur et de débit**
- `MessageLimit.MAX_TEXT_LENGTH = 4096` et `MessageLimit.CAPTION_LENGTH = 1024` — [PTB, constants.py](https://github.com/python-telegram-bot/python-telegram-bot/blob/93968d89d7896c72664cfde0b45526795e58cb98/src/telegram/constants.py).
- `FloodLimit` : 1 message/s dans un chat donné (« Telegram may allow short bursts that go over this limit, but eventually you'll begin receiving 429 errors »), environ 30 messages/s tous chats confondus, environ 20 messages/min par groupe, et 1 000 messages/s en diffusion payée en Telegram Stars (`allow_paid_broadcast`) — [PTB, constants.py](https://github.com/python-telegram-bot/python-telegram-bot/blob/93968d89d7896c72664cfde0b45526795e58cb98/src/telegram/constants.py).

**Webhooks vs long polling**
- `setWebhook` : Telegram envoie un HTTPS POST par update. « In case of an unsuccessful request (a request with response HTTP status code different from 2XY), Telegram will repeat the request and give up after a reasonable amount of attempts. » Si un `secret_token` est défini, il est envoyé dans l'en-tête `X-Telegram-Bot-Api-Secret-Token`. « You will not be able to receive updates using get_updates for long as an outgoing webhook is set up. » Un certificat auto-signé est accepté s'il est téléversé — [PTB, `_bot.py`, docstring de set_webhook](https://github.com/python-telegram-bot/python-telegram-bot/blob/93968d89d7896c72664cfde0b45526795e58cb98/src/telegram/_bot.py).
- Constantes associées : ports supportés `SUPPORTED_WEBHOOK_PORTS = [443, 80, 88, 8443]`, `MAX_CONNECTIONS_LIMIT = 100`, `secret_token` de 1 à 256 caractères — [PTB, constants.py](https://github.com/python-telegram-bot/python-telegram-bot/blob/93968d89d7896c72664cfde0b45526795e58cb98/src/telegram/constants.py).
- Wiki PTB : le polling via getUpdates « is fine for smaller to medium-sized bots and for testing » ; « You should have a good reason to switch from polling to a webhook. Don't do it simply because it sounds cool. » Un webhook exige une IP publique ou un domaine joignable et du HTTPS (« With polling, this is taken care of by the Telegram Servers »). La limite à 4 ports se contourne avec un reverse proxy (nginx, haproxy) qui termine le TLS. Il est recommandé de définir `secret_token` « so no one can send fake updates to your bot » — [python-telegram-bot wiki, « Webhooks », révision du 04/08/2026](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Webhooks).

**Restreindre le bot à un seul utilisateur**
- PTB fournit `filters.User(user_id=...)` : « Filters messages to allow only those which are from specified user ID(s) or username(s) ». Exemple : `MessageHandler(filters.User(1234), callback_method)` — [PTB, `src/telegram/ext/filters.py`, commit 93968d8](https://github.com/python-telegram-bot/python-telegram-bot/blob/93968d89d7896c72664cfde0b45526795e58cb98/src/telegram/ext/filters.py).
- Le username d'un bot est « used in search, mentions and t.me links » — [core.telegram.org/bots/tutorial, extrait de recherche](https://core.telegram.org/bots/tutorial).

### Inferences
- **Filtrage par ID** : un bot est public, n'importe qui peut lui écrire s'il trouve son username. Il faut donc filtrer sur l'ID numérique (stable) plutôt que sur le username (modifiable), ignorer silencieusement les autres expéditeurs, et appliquer le même filtre aux commandes et aux callbacks.
- **Local Bot API Server inutile ici** : une note vocale d'1 min reste très loin de la limite de 20 Mo. Même avec un débit hypothétique de 64 kbit/s, 60 s ≈ 480 Ko. Ce serveur ajouterait un service à maintenir et des identifiants api_id/api_hash sans bénéfice.
- **Polling vs webhook** : le long polling convient à un bot personnel sur VPS ou Raspberry Pi. Il n'utilise que des connexions sortantes (il fonctionne derrière une box/NAT) et ne demande ni certificat ni reverse proxy. Le webhook devient nécessaire en serverless, faute de processus permanent. Avec un webhook, il faut répondre 200 immédiatement puis faire tourner l'agent en tâche de fond ; sinon Telegram peut re-livrer l'update.
- **Voix de réponse** : produire de l'OGG/Opus (directement si le moteur TTS le permet, sinon par conversion ffmpeg) et rester sous ~1 Mo pour obtenir une bulle vocale selon la note de PTB. Calcul indicatif : cela fait ~2 min à 64 kbit/s ou ~4 min à 32 kbit/s.
- **Réponses longues de l'agent** : les découper à 4 096 caractères. `sendMessageDraft` (API 9.5) peut afficher la réponse en streaming.

### Gaps
- Les pages officielles core.telegram.org (bots/api, bots/faq, api-changelog) n'ont pas pu être chargées (bloquées). La date du 24/08/2026 pour la 10.3 et l'absence de 10.4 reposent sur des extraits et sur les changelogs des bibliothèques. Non vérifié / je ne sais pas si une version plus récente est sortie en septembre 2026.
- Débit et codec exacts des notes vocales enregistrées par les clients Telegram (bitrate Opus, fréquence d'échantillonnage) : non vérifié / je ne sais pas.
- Délai au-delà duquel Telegram considère qu'un webhook a échoué, et nombre exact de ré-essais : non documenté dans les sources lues (« reasonable amount of attempts »).
- Une page « Telegram Serverless » (core.telegram.org/bots/serverless) apparaît dans une liste de résultats. Son contenu n'a pas été vérifié ; je ne sais pas ce qu'elle propose (hébergement de bots par Telegram ?).
- Les limites de `sendAudio` n'ont pas été lues en détail. Seule la mention de PTB « other formats may be sent as Audio or Document » est vérifiée.

## 2. Confidentialité et sécurité Telegram pour un bot (chiffrement, stockage, politique pour les bots, implications pour des informations d'affaires sensibles)

### Takeaway
Les échanges avec un bot sont des « cloud chats » chiffrés client-serveur, pas de bout en bout. Plusieurs acteurs voient donc le contenu : Telegram (données stockées aux Pays-Bas pour un compte créé dans l'EEE ou au Royaume-Uni), le serveur du bot et chaque API tierce (STT, LLM, TTS). Par ailleurs, depuis septembre 2024, Telegram peut transmettre l'adresse IP et le numéro de téléphone d'un utilisateur aux autorités sur demande légale valide.

### Cited Findings
- FAQ de Telegram : « Messages in Secret Chats use client-client encryption, while Cloud Chats use client-server/server-client encryption and are stored encrypted in the Telegram Cloud ». Les secret chats sont « device-specific and are not part of the Telegram cloud » — [Telegram, FAQ, telegram.org/faq, extrait de recherche (date non visible)](https://telegram.org/faq).
- Les données des cloud chats sont réparties dans plusieurs centres de données, contrôlés par des entités juridiques différentes dans plusieurs juridictions. Les clés de déchiffrement sont découpées et jamais stockées au même endroit que les données qu'elles protègent — [Telegram, FAQ, extrait de recherche](https://telegram.org/faq).
- Politique de confidentialité : pour un compte créé depuis le Royaume-Uni ou l'EEE, « your data is stored in data centers in the Netherlands », sur des serveurs appartenant à Telegram. Les données sont chiffrées et les clés conservées dans d'autres centres de données situés dans d'autres juridictions. Seuls les secret chats, le contenu des appels et Telegram Passport sont chiffrés avec une clé connue uniquement de l'utilisateur et de son destinataire — [Telegram, « Telegram Privacy Policy », telegram.org/privacy, extrait de recherche (date de mise à jour non visible)](https://telegram.org/privacy).
- Bots tiers : « if you use AI through a third-party bot or mini app, you are using a third-party service, not Telegram ». Telegram encourage chaque développeur à publier sa propre politique de confidentialité — [Telegram Privacy Policy, extrait de recherche](https://telegram.org/privacy).
- Telegram publie aussi une « Standard Bot Privacy Policy » et des « Telegram Bot Platform Developer Terms of Service ». Un extrait indique que les développeurs ne doivent jamais partager les données des utilisateurs avec des tiers sans autorisation explicite ou obligation légale — [Telegram, Standard Bot Privacy Policy](https://telegram.org/privacy-tpa) ; [Telegram, Bot Platform Developer ToS](https://telegram.org/tos/bot-developers). Ce sont des extraits de recherche, et je n'ai pas pu vérifier laquelle des deux pages contient cette phrase.
- Septembre 2024 : Pavel Durov annonce que Telegram remettra « the IP addresses and phone numbers » des utilisateurs qui violent les CGU « to relevant authorities in response to valid legal requests ». Auparavant, cela se limitait aux suspects de terrorisme. Telegram promet un rapport de transparence trimestriel. Le changement intervient un mois après l'arrestation de Durov en France — [Help Net Security, 24/09/2024](https://www.helpnetsecurity.com/2024/09/24/telegram-legal-requests/) ; [The Hacker News, septembre 2024](https://thehackernews.com/2024/09/telegram-agrees-to-share-user-data-with.html).
- Le token du bot est à protéger comme un mot de passe : « it can be used by anyone to control your bot » — [core.telegram.org/bots/tutorial, extrait de recherche](https://core.telegram.org/bots/tutorial). Pour un webhook, un `secret_token` est recommandé contre les fausses updates — [PTB wiki, « Webhooks »](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Webhooks).

### Inferences
- **Pas de secret chat pour un bot** : la Bot API ne fonctionne que par getUpdates ou webhook, des serveurs Telegram vers le serveur du bot, en HTTPS. Le contenu (texte et fichiers audio) est donc accessible à Telegram, selon le modèle de chiffrement client-serveur. Un bot ne bénéficie pas des secret chats : la Bot API n'a pas de méthode correspondante. Ce point n'est pas confirmé par une page officielle lue.
- **Chaîne de sous-traitants pour une note vocale** : Telegram, puis le serveur du bot (VPS), le fournisseur STT, le fournisseur LLM, éventuellement le fournisseur TTS, puis de nouveau Telegram. Pour des informations d'affaires sensibles :
  - préférer des fournisseurs UE et/ou à zéro rétention (voir §4) ;
  - ne pas faire transiter de secrets (mots de passe, données de santé, données clients nominatives) ;
  - purger fichiers audio et journaux sur le VPS ;
  - filtrer par ID utilisateur et garder le token hors de Git (variable d'environnement) ;
  - envisager une STT locale pour retirer un maillon de la chaîne.
- **Usage professionnel** : je n'ai trouvé aucune mention d'un contrat de sous-traitance RGPD (DPA) proposé par Telegram aux développeurs de bots. C'est un point juridique à valider (non vérifié).

### Gaps
- Le texte intégral de telegram.org/privacy (section sur les bots, durées de conservation, date de dernière mise à jour) n'a pas pu être chargé (bloqué). Non vérifié / je ne sais pas combien de temps Telegram conserve les messages et fichiers envoyés à un bot.
- Existence d'un DPA Telegram pour les développeurs de bots : non vérifié / je ne sais pas.
- Volumes de données transmis aux autorités en 2025–2026 (rapports de transparence) : non vérifié.

## 3. Bibliothèques Telegram (python-telegram-bot, aiogram, grammY, Telegraf) : maintenance et dernières versions en 2026

### Takeaway
En Python, aiogram (3.31.0, Bot API 10.3, août 2026) et python-telegram-bot (22.8, Bot API 10.0, juin 2026 ; dépôt actif fin septembre) sont maintenues. En TypeScript, grammY (1.46.0, Bot API 10.3) est à jour. Telegraf n'a plus rien publié depuis février 2024 (Bot API 7.1).

### Cited Findings
- **python-telegram-bot**
  - Dernière version 22.8 (12/06/2026) ; précédentes : 22.7 (16/03/2026), 22.6 (24/01/2026), 22.5 (27/09/2025), 22.4 (13/09/2025).
  - « All types and methods of the Telegram Bot API 10.0 are natively supported by this library » ; Python 3.10 à 3.15 ; licence LGPL-3.0-only — [PyPI, python-telegram-bot, consulté le 28/09/2026](https://pypi.org/project/python-telegram-bot/) ; dates via [PyPI JSON](https://pypi.org/pypi/python-telegram-bot/json).
  - Branche principale active : dernier commit le 27/09/2026, `BOT_API_VERSION_INFO` toujours à 10.0 — [PTB, constants.py, commit 93968d8](https://github.com/python-telegram-bot/python-telegram-bot/blob/93968d89d7896c72664cfde0b45526795e58cb98/src/telegram/constants.py).
  - Une PR ajoute la Bot API 10.3 ; son statut (fusionnée ou non) n'est pas vérifié — [PTB, PR #5359](https://github.com/python-telegram-bot/python-telegram-bot/pull/5359).
- **aiogram**
  - 3.31.0 du 26/08/2026, « Supports Telegram Bot API 10.3 » ; Python 3.10 à 3.14 (et PyPy) ; licence MIT.
  - Rythme d'environ une version par mois : 3.28.2 (10/05), 3.29.0 (14/06) et 3.29.1 (01/07/2026), toutes deux marquées « yanked », puis 3.30.0 (17/07/2026) — [PyPI, aiogram](https://pypi.org/project/aiogram/) ; [PyPI JSON](https://pypi.org/pypi/aiogram/json).
- **grammY** (TypeScript, Node et Deno) : 1.46.0 publiée le 26/08/2026 (étiquette « latest »), puis la série déjà décrite au §1 (10.3, 10.2, 10.1, 10.0, 9.6) — [registre npm grammy](https://registry.npmjs.org/grammy) ; [grammY, Releases GitHub](https://github.com/grammyjs/grammY/releases).
- **Telegraf**
  - Étiquette « latest » = 4.16.3, publiée le 29/02/2024 ; aucune version plus récente au 28/09/2026. Elle couvre la Bot API 7.0–7.1 — [registre npm telegraf, consulté le 28/09/2026](https://registry.npmjs.org/telegraf).
  - L'annonce de la 4.16.0 indiquait « This will be the last major update for Telegraf v4 », avec correctifs jusqu'en février 2025 et une v5 annoncée — [Telegraf, Releases GitHub](https://github.com/telegraf/telegraf/releases).

### Inferences
- Pour un projet neuf : en Python, aiogram (asynchrone natif, suit la Bot API au plus près) ou python-telegram-bot (wiki et exemples riches, `filters.User`) ; en TypeScript, grammY. Éviter Telegraf : environ 2 ans et demi sans version, et bloqué à l'API 7.1 alors que l'API est en 10.3.
- Le retard de PTB (10.0 contre 10.3) ne gêne pas un compagnon vocal : les nouveautés 10.x (rich messages, messages éphémères, gestion de bots) ne sont pas nécessaires.

### Gaps
- Statut de Telegraf v5 (sortie éventuelle sous un autre nom de paquet) : non vérifié / je ne sais pas.
- Dates officielles de publication des Bot API 10.0 à 10.2 : non vérifiées (seules les dates des bibliothèques le sont).

## 4. Speech-to-text en français : prix 2026, qualité, résidence UE / RGPD / zéro rétention, options locales

### Takeaway
Pour des notes d'environ 1 min, tous les services cloud coûtent moins de 0,02 $/min. Les moins chers sont Groq Whisper large-v3-turbo (0,04 $/h), puis Mistral Voxtral Mini Transcribe 2 et OpenAI gpt-4o-mini-transcribe (0,003 $/min). Mistral a l'avantage d'être un acteur français proposant la zéro rétention (ZDR) en paiement à l'usage. Je n'ai pas pu vérifier de benchmark primaire chiffré sur le français.

### Cited Findings
**Prix (API, USD HT, à l'usage)**

*OpenAI*
- **whisper-1** : 0,006 $/min — [OpenAI, extrait de recherche](https://openai.com/api/pricing/) ; concordant avec la base LiteLLM (0,0001 $/s) — [BerriAI/LiteLLM, model_prices_and_context_window.json, commit 90e4962 du 28/09/2026](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json).
- **gpt-4o-transcribe** et **gpt-4o-transcribe-diarize** : 0,0001 $/s, soit 0,006 $/min. **gpt-4o-mini-transcribe** : 0,00005 $/s, soit 0,003 $/min — [base LiteLLM, source citée : developers.openai.com/api/docs/pricing](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json).
- **gpt-transcribe** (nouveau modèle 2026) : 0,000075 $/s, soit 0,0045 $/min. **gpt-realtime-whisper** (streaming) : 0,000283 $/s, soit 0,017 $/min — [base LiteLLM](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json).
- Fonctionnalités de GPT-Transcribe : « speech-to-text model for completed audio files, streamed file transcripts, and committed turns in Realtime sessions ». Il accepte « unstructured context, keyword hints, and multiple language hints » ; il existe une variante « GPT-Live-Transcribe » et un guide de migration depuis Whisper — [OpenAI, page modèle gpt-transcribe, extrait](https://developers.openai.com/api/docs/models/gpt-transcribe) ; [OpenAI Cookbook, « Migrate from Whisper to GPT-Transcribe and GPT-Live-Transcribe »](https://developers.openai.com/cookbook/examples/migrating_from_whisper_to_gpt_transcribe) (extraits).

*Groq*
- **whisper-large-v3-turbo** : 0,04 $/h — [Groq, docs modèles, extrait de recherche](https://console.groq.com/docs/model/whisper-large-v3-turbo) ; concordant avec la base LiteLLM (1,111e-5 $/s).
- **whisper-large-v3** : 3,083e-5 $/s, soit ≈ 0,111 $/h — [base LiteLLM](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json).
- Vitesse revendiquée par Groq : Whisper Large V3 à « 164x Speed Factor » selon un benchmark d'Artificial Analysis — [Groq, communiqué, extrait (affirmation du fournisseur)](https://wow.groq.com/groq-runs-whisper-large-v3-at-a-164x-speed-factor-according-to-new-artificial-analysis-benchmark/).

*Mistral*
- **Voxtral Mini Transcribe 2** (`voxtral-mini-2602`) : 0,003 $/min — [Mistral, « Voxtral transcribes at the speed of sound », extrait](https://mistral.ai/news/voxtral-transcribe-2/) ; concordant avec la base LiteLLM (5e-5 $/s ; source citée : docs.mistral.ai, fiche voxtral-mini-transcribe-26-02).
- **Voxtral Mini Transcribe Realtime** (2602) : 0,0001 $/s, soit 0,006 $/min — [base LiteLLM](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json).

*Deepgram*
- **Nova-3** : 0,0043 $/min en pré-enregistré — [base LiteLLM, 7,167e-5 $/s, source citée deepgram.com/pricing](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json).
- Un extrait du site Deepgram donne des prix différents selon le mode : 0,0077 $/min (streaming monolingue) et 0,0092 $/min (multilingue), le 0,0043 $/min correspondant à « a specific batch tier ». Facturation à la seconde — [Deepgram, pricing et blog, extrait de recherche](https://deepgram.com/pricing).

*ElevenLabs*
- **Scribe v2** : 0,22 $/h, et 0,39 $/h en temps réel. Options : détection d'entités (+0,07 $/h), « keyterm prompting » (+0,05 $/h) — [ElevenLabs, API pricing, extrait](https://elevenlabs.io/pricing/api).
- Le paiement à l'usage (PAYG) a été introduit le 07/05/2026 ; Scribe v2 est passé de 0,40 $ à 0,22 $/h — [ElevenLabs, « We've lowered API & Agents pricing and introduced PAYG », 07/05/2026, extrait](https://elevenlabs.io/blog/weve-lowered-api-agents-pricing-and-introduced-pay-as-you-go) ; concordant avec la base LiteLLM (6,11e-5 $/s).

*Google Cloud*
- **Speech-to-Text V2** : prix ramené de 0,024 à 0,016 $/min ; paliers de volume jusqu'à 0,004 $/min ; tarif « Dynamic Batch » à −75 % pour une latence acceptée jusqu'à 24 h. Chirp 3 n'est disponible que dans l'API V2 — [Google Cloud Blog, « Speech-to-Text V2 API », extrait](https://cloud.google.com/blog/products/ai-machine-learning/google-cloud-speech-to-text-v2-api) ; [Google Cloud docs, Chirp 3, extrait](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3).
- LiteLLM donne chirp_3 à 0,00026667 $/s, soit 0,016 $/min — [base LiteLLM](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json).

*AssemblyAI*
- **Universal-3.5 Pro** : 0,21 $/h en asynchrone. **Universal-2** : 0,15 $/h. Temps réel : Universal-3.5 Pro Realtime 0,45 $/h, Universal Streaming 0,15 $/h.
- Facturation à la seconde, sans minimum. Offre gratuite : 185 h de pré-enregistré et 333 h de streaming — [AssemblyAI, pricing, extrait](https://www.assemblyai.com/pricing).
- Divergence : la base LiteLLM affiche des tarifs manifestement obsolètes (« assemblyai/best » ≈ 0,12 $/h).

**Formats et limites de fichier**
- OpenAI : fichiers jusqu'à 25 Mo. Deux listes de formats contradictoires selon les pages : « mp3, mp4, mpeg, mpga, m4a, wav, webm » d'un côté, « flac, mp3, mp4, mpeg, mpga, m4a, ogg, wav, webm » de l'autre — [OpenAI, « File transcription », extrait](https://platform.openai.com/docs/guides/speech-to-text).

**Qualité en français (niveau de preuve faible)**
- Voxtral Transcribe 2 afficherait 5,9 % de WER moyen, contre 7,4 % pour Whisper, sur FLEURS. C'est une moyenne multilingue, pas un chiffre propre au français. Voxtral couvre 13 langues dont le français, Whisper plus de 99 — [Weesper Neon Flow (blog tiers), 31/03/2026, extrait de recherche](https://weesperneonflow.ai/en/blog/2026-03-31-voxtral-whisper-open-source-speech-models-comparison-2026/).
- OpenAI revendique pour gpt-4o-transcribe et sa version mini un WER inférieur aux modèles Whisper sur FLEURS (plus de 100 langues). Le graphique par langue n'a pas été lu — [OpenAI, « Introducing next-generation audio models in the API », extrait](https://openai.com/index/introducing-our-next-generation-audio-models/).
- Un article en français, publié par un éditeur de service de transcription (donc partial), avance : Whisper large-v3 entre 4 et 6 % d'erreur sur un audio professionnel propre, et un service basé sur « Gemini 3.8 Flash » sous 2 % — [Le Scribe Audio, 2026, extrait de recherche](https://lescribeaudio.com/transcription-automatique-entretien-whisper-speechmatics). À traiter comme du marketing.
- Le même extrait agrégé signale le risque d'« hallucinations » de Whisper (phrases inventées et répétées) sur les silences et le bruit — [extrait de recherche, lescribeaudio.com](https://lescribeaudio.com/transcription-ia-audio). À vérifier.

**Données : résidence UE, RGPD, rétention**
- **OpenAI** : résidence des données en Europe pour l'API, réservée aux clients éligibles (nouveau projet avec la région Europe). Les requêtes sont alors traitées dans la région « with zero data retention ». Par défaut, les journaux de surveillance des abus sont conservés jusqu'à 30 jours ; ZDR et « Modified Abuse Monitoring » sont soumis à approbation — [OpenAI, « Introducing data residency in Europe »](https://openai.com/index/introducing-data-residency-in-europe/) ; [OpenAI, « Data controls in the OpenAI platform »](https://developers.openai.com/api/docs/guides/your-data) (extraits).
- **Mistral**
  - Entrées et sorties conservées le temps de la génération, puis « thirty (30) rolling days to monitor abuse » sauf si la ZDR est activée. La ZDR n'est disponible qu'en paiement à l'usage et pour les appels API sans état — [Mistral Help Center, « How long do you store my data? »](https://help.mistral.ai/en/articles/347628-how-long-do-you-store-my-data) ; [« Can I activate Zero Data Retention (ZDR)? »](https://help.mistral.ai/en/articles/347612-can-i-activate-zero-data-retention-zdr) (extraits).
  - Priorité donnée à des sous-traitants dans l'UE ; transferts hors UE encadrés par l'article 46 du RGPD — [Mistral, Privacy Policy, extrait](https://legal.mistral.ai/terms/privacy-policy/).
- **Groq** : « Customer data is not retained by default ». Des journaux temporaires peuvent être conservés jusqu'à 30 jours (fiabilité, abus). Tous les clients peuvent activer la ZDR dans « Data Controls ». Groq n'entraîne pas de modèles sur les données des clients — [Groq, « Your Data in GroqCloud », extrait](https://console.groq.com/docs/your-data).
- **Deepgram** : endpoint UE `api.eu.deepgram.com` en disponibilité générale, hébergé dans des régions AWS UE, pour STT, TTS, Voice Agent et Text Intelligence, avec les mêmes clés API — [Deepgram, « Deepgram EU Endpoint Is Now Generally Available », extrait (date non visible)](https://deepgram.com/learn/deepgram-eu-endpoint-now-generally-available).
- **ElevenLabs** : résidence des données dans l'UE réservée aux clients Enterprise ; le « Zero Retention Mode » est aussi Enterprise et ne couvre que l'API — [ElevenLabs docs, « Data residency »](https://elevenlabs.io/docs/overview/administration/data-residency) ; [« Zero Retention Mode (Enterprise) »](https://elevenlabs.io/docs/eleven-api/resources/zero-retention-mode) (extraits).
- **AssemblyAI** : endpoints UE (`api.eu.assemblyai.com` en asynchrone, `streaming.eu.assemblyai.com` en streaming) au même prix que les endpoints US, sans sortie des données de l'UE. Universal-3.5 Pro Realtime prend en charge le français — [AssemblyAI docs, « Select the EU region »](https://www.assemblyai.com/docs/guides/how_to_use_the_eu_endpoint) ; [« Cloud Endpoints and Data Residency »](https://www.assemblyai.com/docs/pre-recorded-audio/select-the-region) (extraits).

**Options locales (CPU)**
- Benchmark de faster-whisper : 13 minutes d'audio, modèle small, CPU Intel Core i7-12700K avec 8 threads, beam 5 — [SYSTRAN/faster-whisper, README, commit ed9a06c du 19/11/2025](https://github.com/SYSTRAN/faster-whisper/blob/ed9a06cd89a93e47838f564998a6c09b655d7f43/README.md).

  | Implémentation | Précision | Temps | RAM |
  |---|---|---|---|
  | openai/whisper | fp32 | 6 min 58 s | 2 335 Mo |
  | whisper.cpp | fp32 | 2 min 05 s | 1 049 Mo |
  | faster-whisper | fp32 | 2 min 37 s | 2 257 Mo |
  | faster-whisper | int8 | 1 min 42 s | 1 477 Mo |
  | faster-whisper, batch_size=8 | int8 | 51 s | 3 608 Mo |

  Le README ajoute : « up to 4 times faster than openai/whisper for the same accuracy ».
- Raspberry Pi 5 : whisper.cpp avec le modèle small tournerait à ≈ 0,4–0,6× le temps réel (10 min d'audio en 17 à 25 min) ; tiny et base iraient au temps réel ou plus vite avec `-t 4`. Le fichier GGML small pèse 466 Mio pour ≈ 852 Mo de RAM — [extrait de recherche agrégé : promptquorum.com « Whisper.cpp vs faster-whisper 2026 », openwhispr.com, discussion whisper.cpp #166](https://www.promptquorum.com/power-local-llm/local-whisper-stt-comparison-2026). Confiance faible.

### Inferences
- **Vitesse locale estimée** : 102 s / 13 min ≈ 7,8 s par minute d'audio avec faster-whisper small int8 sur 8 threads d'un i7-12700K. Sur un VPS à 2 vCPU partagés (4 fois moins de threads, cœurs plus lents), compter plutôt ~30 à 60 s par minute d'audio ; c'est une extrapolation, pas une mesure. Les modèles medium et large-v3 seraient plusieurs fois plus lents. Une STT locale en français de bonne qualité sur CPU implique donc ~2 à 4 Go de RAM et une latence de l'ordre de la minute par note.
- **Format d'entrée** : Telegram envoie de l'OGG/Opus. Les listes de formats OpenAI sont contradictoires, et Gemini mentionne « OGG Vorbis ». Transcoder en MP3, WAV ou FLAC avec ffmpeg avant l'envoi est plus sûr, pour un coût quasi nul.
- **Options « souveraines » / RGPD** : Mistral Voxtral (société française, ZDR en paiement à l'usage), Deepgram ou AssemblyAI via leur endpoint UE (sans surcoût chez AssemblyAI), ou une STT locale. OpenAI suppose d'être éligible à la résidence des données en Europe ; ElevenLabs n'offre ces garanties qu'en Enterprise.
- **Qualité en français** : les grands modèles récents (gpt-4o-transcribe et gpt-transcribe, Voxtral, Scribe, Nova-3, Universal-3.5) visent tous un français « haute ressource ». Faute de données primaires, le plus sûr est un test A/B sur 20 à 30 notes réelles de l'utilisateur, en comparant noms propres et jargon métier.

### Gaps
- Benchmarks primaires de WER en français (tableaux FLEURS-fr des papiers Voxtral sur arXiv, graphique par langue d'OpenAI, Open ASR Leaderboard multilingue de Hugging Face, Artificial Analysis) : non chargés, domaines bloqués. Non vérifié / je ne sais pas quel service est le meilleur en français chiffres à l'appui.
- Aucune page de prix officielle n'a été chargée directement. Divergences non résolues : Deepgram (0,0043 ou 0,0077/0,0092 $/min selon le mode), AssemblyAI (valeur obsolète dans LiteLLM), ElevenLabs TTS (voir §6).
- Facturation minimale par requête chez Groq (souvenir non sourcé d'un minimum de 10 s) : non vérifié / je ne sais pas. Sans effet pour des notes d'1 min.
- Localisation des serveurs Groq (US ou UE) et disponibilité de Chirp 3 dans les régions UE de Google : non vérifiées.
- Vitesse réelle de faster-whisper ou whisper.cpp sur un VPS Hetzner ou OVH à 2 vCPU : pas de mesure sourcée.
- Licence et disponibilité des poids ouverts de Voxtral Transcribe 2 : non vérifiées.

## 5. Des LLM acceptent-ils directement l'audio (transcription intégrée) ? OpenAI, Google Gemini, et Mistral

### Takeaway
Oui. Gemini (tous les modèles Flash récents) accepte l'audio à environ 32 tokens/s, soit ~1 920 tokens/min (~0,002 $/min à 1 $ le million de tokens). OpenAI propose gpt-audio, gpt-audio-mini et gpt-realtime (audio en entrée à 10–32 $ le million de tokens). Mistral Voxtral Small est un modèle de chat à entrée audio (~0,004 $/min). Pour un agent principal Claude (couvert par un autre chercheur), cela ajoute toutefois un second modèle dans la chaîne.

### Cited Findings
- **Gemini, caractéristiques audio** : « 32 tokens per second (1 minute = 1,920 tokens) ». Formats acceptés : WAV, MP3, AIFF, AAC, OGG Vorbis (audio/ogg) et FLAC. Jusqu'à 9,5 h d'audio par prompt ; l'audio est ramené à 16 Kbps et les canaux fusionnés en un seul — [Google, « Audio understanding | Gemini API », ai.google.dev, extrait de recherche](https://ai.google.dev/gemini-api/docs/audio).
- **Contradiction** : un extrait de la page de prix Gemini parle de « 25 audio tokens per second » et d'un tarif effectif « ~$0.005 per minute for Transcribe » — [Google, « Gemini Developer API pricing », extrait](https://ai.google.dev/gemini-api/docs/pricing).
- **Gemini, prix de l'audio en entrée** (base LiteLLM, source citée : ai.google.dev/gemini-api/docs/pricing) — [BerriAI/LiteLLM, commit 90e4962 du 28/09/2026](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json) :

  | Modèle | Audio en entrée ($/M tokens) | Texte entrée / sortie ($/M tokens) |
  |---|---|---|
  | gemini-2.5-flash | 1,00 | 0,30 / 2,50 |
  | gemini-2.5-flash-lite | 0,30 | 0,10 / 0,40 |
  | gemini-3-flash-preview | 1,00 | 0,50 / 3,00 |
  | gemini-3.1-flash-lite | 0,50 | 0,25 / 1,50 |
  | gemini-3.5-flash | 1,50 | 1,50 / 9,00 |

- **Gemini 3.8 Flash** est présenté comme le dernier modèle (page « What's new in Gemini 3.8 Flash ») — [ai.google.dev/gemini-api/docs/latest-model, titre vu en résultat de recherche](https://ai.google.dev/gemini-api/docs/latest-model). LiteLLM lui attribue 0,75 $/M tokens en entrée et 3,75 $/M en sortie, sans prix audio distinct — [base LiteLLM](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json).
- **Gemini, usage des données** : les données du niveau gratuit servent « to improve Google products » (oui), pas celles du niveau payant (non) — [Gemini API pricing, extrait](https://ai.google.dev/gemini-api/docs/pricing).
- **OpenAI, modèles à entrée audio** (base LiteLLM, source citée : developers.openai.com/api/docs/pricing) — [base LiteLLM](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json) :
  - gpt-audio et gpt-audio-1.5 : audio en entrée 32 $/M tokens, audio en sortie 64 $/M, texte 2,50 $ (entrée) et 10 $ (sortie) ;
  - gpt-audio-mini : audio 10 $/M en entrée et 20 $/M en sortie, texte 0,60 $ et 2,40 $ ;
  - gpt-realtime, gpt-realtime-2 et gpt-realtime-2.1 : audio en entrée 32 $/M ;
  - gpt-realtime-mini et gpt-realtime-2.1-mini : audio en entrée 10 $/M.
- **Mistral Voxtral Small** (`voxtral-small-2507`, modèle de chat) : audio à 6,67e-5 $/s (≈ 0,004 $/min), texte à 0,10 $/M tokens en entrée et 0,40 $/M en sortie — [base LiteLLM, source citée docs.mistral.ai/models/voxtral-small-25-07](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json).

### Inferences
- **Coût de Gemini utilisé comme transcripteur** : 600 min/mois × 1 920 tokens = 1,152 M tokens, soit 0,35 $ (2.5 Flash-Lite) à 1,73 $ (3.5 Flash). Il faut y ajouter la sortie texte, sur une hypothèse non sourcée de 200 à 250 tokens par minute de parole. C'est du même ordre que les API STT dédiées. L'intérêt est d'obtenir en un seul appel la transcription et un résumé ou une intention.
- **Deux architectures** : STT dédiée → texte → agent (la plus simple, tout passe par le texte), ou LLM audio natif. Avec un agent Claude, la STT dédiée reste l'intégration la plus directe.
- **Format** : Gemini liste « OGG Vorbis » alors que les notes Telegram sont en OGG Opus. Par prudence, convertir en MP3, FLAC ou WAV (non testé).

### Gaps
- Nombre de tokens audio par minute chez OpenAI (gpt-audio et gpt-realtime) : non vérifié, donc coût par minute non calculable ici.
- Prix de l'audio pour Gemini 3.8 Flash : non vérifié / je ne sais pas.
- Contradiction entre 32 et 25 tokens/s chez Gemini : non résolue (pages non chargées).
- Prise en charge de l'OGG/Opus par Gemini et par gpt-audio : non vérifiée.

## 6. Text-to-speech en français pour les réponses vocales : options et prix

### Takeaway
Pour ~300 000 caractères par mois, compter :
- ~4,5 à 5 $ avec OpenAI tts-1, gpt-4o-mini-tts ou Mistral Voxtral TTS ;
- ~9 $ avec OpenAI tts-1-hd ou Google Chirp 3 HD ;
- 15 à 30 $ avec ElevenLabs ;
- 0 € en local avec Piper, qui a des voix fr_FR mais une qualité moindre, et dont le projet cherche des mainteneurs.

### Cited Findings
- **OpenAI** : tts-1 à 15 $/M caractères, tts-1-hd à 30 $/M. gpt-4o-mini-tts : 0,60 $/M caractères en entrée et 12 $/M tokens audio en sortie, « Estimated cost: $0.015 / minute » — [OpenAI, API pricing, extrait](https://openai.com/api/pricing/). Concordant avec la base LiteLLM (tts-1 à 1,5e-5 $/car., tts-1-hd à 3e-5 $/car., gpt-4o-mini-tts à 0,00025 $/s d'audio) — [base LiteLLM](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json).
- **ElevenLabs** : le paiement à l'usage est annoncé le 07/05/2026, avec un TTS « up to 55% lower cost » et un STT « up to 45% lower ». Prix : 0,10 $ pour 1 000 caractères avec les modèles multilingues (Multilingual v2, v3), 0,05 $ avec Flash/Turbo v2.5 — [ElevenLabs, « We've lowered API & Agents pricing and introduced PAYG », 07/05/2026, extrait](https://elevenlabs.io/blog/weve-lowered-api-agents-pricing-and-introduced-pay-as-you-go) ; [ElevenLabs, API pricing, extrait](https://elevenlabs.io/pricing/api). Divergence : LiteLLM affiche encore 0,18 $ pour 1 000 caractères pour eleven_multilingual_v2 et eleven_v3, probablement avant la baisse — [base LiteLLM](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json).
- **Mistral Voxtral TTS** (`voxtral-mini-tts-2603`) : 0,016 $ pour 1 000 caractères. 9 langues dont le français, avec des voix de dialectes « American, British, and French ». Poids ouverts publiés sur Hugging Face sous licence CC BY-NC 4.0 — [Mistral, « Speaking of Voxtral », extrait](https://mistral.ai/news/voxtral-tts/) ; [Mistral docs, fiche Voxtral TTS, extrait](https://docs.mistral.ai/models/model-cards/voxtral-tts-26-03). Prix concordant avec la base LiteLLM (1,6e-5 $/car.).
- **Google Cloud TTS** : voix Chirp (HD) à 30 $/M caractères — [base LiteLLM, source citée cloud.google.com/text-to-speech/pricing](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json). Gratuité mensuelle : 1 M de caractères en voix WaveNet et 4 M en voix Standard — [Google Cloud, TTS pricing, extrait](https://cloud.google.com/text-to-speech/pricing).
- **Gemini TTS** (tarification en tokens) : gemini-2.5-flash-preview-tts à 0,50 $/M en entrée et 10 $/M d'audio en sortie ; gemini-3.1-flash-tts-preview à 1 $ et 20 $ ; gemini-3.8-flash-tts à 0,50 $ et 9 $ — [base LiteLLM](https://github.com/BerriAI/litellm/blob/90e4962c816695a87f357e0a1774a6369ca4520a/model_prices_and_context_window.json).
- **Piper (local)**
  - Présentation : « A fast and local neural text-to-speech engine that embeds espeak-ng for phonemization ». Installation : `pip install piper-tts`. Licence GPL-3.0.
  - Maintenance : « The Open Home Foundation is looking for maintainers for Piper! » ; dernier commit le 17/09/2026.
  - Voix : entraînées avec VITS et exportées pour onnxruntime ; le français (fr_FR) figure dans la liste des langues.
  - Licences : « Piper is intended for personal use and text to speech research only » ; certaines voix ont des licences restrictives, précisées dans leur fichier MODEL_CARD.
  - Sources : [OHF-Voice/piper1-gpl, docs/VOICES.md, commit 5b355b1](https://github.com/OHF-Voice/piper1-gpl/blob/5b355b110aecf3de8f4e000ede1ce06831acff35/docs/VOICES.md) ; [README](https://github.com/OHF-Voice/piper1-gpl).

### Inferences
- **Coûts pour 300 000 caractères par mois** (hypothèse : 20 réponses de 500 caractères par jour) :
  - Voxtral TTS : 300 × 0,016 = 4,80 $ ;
  - tts-1 : 0,3 × 15 = 4,50 $ ;
  - gpt-4o-mini-tts : ≈ 5,2 $. Hypothèse d'environ 15 caractères/s en français : 500 caractères ≈ 33 s, soit ≈ 333 min/mois × 0,015 $ = 5,00 $, plus 0,3 × 0,60 = 0,18 $ d'entrée ;
  - tts-1-hd : 9 $ ;
  - Chirp HD : 9 $ si la gratuité ne s'applique pas ;
  - ElevenLabs : 15 $ en Flash, 30 $ en Multilingual ou v3 ;
  - Piper : 0 €.
- **Voix optionnelle** : réserver la réponse vocale à une demande explicite (commande `/voix`) ou aux cas où l'utilisateur a lui-même envoyé une note vocale. Lire à voix haute un long texte d'agent coûte cher et prend du temps.
- **Licence Voxtral TTS** : la licence CC BY-NC des poids interdit un usage commercial en auto-hébergement ; l'API reste utilisable.

### Gaps
- Qualité comparée des voix françaises (MOS, tests d'écoute) : non vérifié / je ne sais pas.
- Sortie native en OGG/Opus pour chaque TTS (format « opus » chez OpenAI, ElevenLabs, Mistral) : non vérifié dans les sources chargées. La conversion ffmpeg reste possible.
- Prix de Deepgram Aura-2 et prix Google Neural2, WaveNet et Standard par million de caractères : non obtenus.
- Gratuité éventuelle des voix Chirp 3 HD : non vérifiée.
- Vitesse de synthèse de Piper sur Raspberry Pi 5 ou sur un VPS : non vérifiée.

## 7. Hébergement 24/7 en Europe : VPS (Hetzner, OVHcloud, Scaleway, Hostinger, IONOS), Raspberry Pi 5 / mini-PC, Docker/systemd, serverless

### Takeaway
En septembre 2026, un petit VPS européen (2 vCPU, 4 Go) coûte de 3,81 € HT/mois (OVHcloud VPS-1, gamme « VPS 2027 ») à 5,49–5,99 € HT/mois (Hetzner CX23 ou CAX11, après la hausse du 15/06/2026). Le Raspberry Pi 5 est devenu cher (110 $ en 4 Go), mais il ne consomme que ~2,7 W au repos, soit ~5 € d'électricité par an. Le serverless (Cloudflare Workers, Vercel) se prête mal aux tâches d'agent longues.

### Cited Findings
**VPS**
- **Hetzner**
  - Ajustement de prix au 15/06/2026 (8 h CEST) pour les nouvelles commandes et les redimensionnements ; les serveurs existants ne sont pas touchés tant qu'ils ne sont pas redimensionnés. « All prices are excluding VAT » — [Hetzner Docs, « Price Adjustment 15 June 2026 », extrait](https://docs.hetzner.com/general/infrastructure-and-availability/price-adjustment/).
  - Nouveaux prix : CX23 de 3,99 € à 5,49 € ; CAX11 de 4,49 € à 5,99 € ; CPX22 de 7,99 € à 19,49 €. Hausse de +30 à +38 % pour les vCPU partagés et de +113 à +175 % pour les vCPU dédiés — [webhosting.today, 18/06/2026](https://webhosting.today/2026/06/18/hetzners-price-increases-reached-209-the-30-headline-applied-to-a-different-tier/) ; [byteiota](https://byteiota.com/hetzner-june-2026-price-shock/) ; [WZ-IT](https://wz-it.com/en/blog/hetzner-price-increase-june-2026-cpx-ccx-alternatives/) (extraits de sources secondaires concordantes).
  - Deux hausses en 10 semaines — [bex.co, 16/08/2026, extrait](https://bex.co/blog/2026/08/16/hetzner-double-price-hike-cheap-box-assumption-cost-model).
  - CX23 : 2 vCPU, 4 Go de RAM, 40 Go de stockage, 20 To de trafic inclus ; prix de lancement affiché « €2.99/3.49 », sans que l'extrait précise à quoi correspondent les deux montants — [Hetzner, « new CX plans », extrait](https://www.hetzner.com/pressroom/new-cx-plans/).
- **OVHcloud, gamme « VPS 2027 »**
  - VPS-1 : 2 vCores, 4 Go de RAM, 40 Go NVMe, 500 Mbit/s, sauvegarde quotidienne (dernières 24 h), trafic illimité, IPv4 et IPv6, SLA 99,9 %, « à partir de 3,81 €/mois HT ».
  - Gamme au-dessus : VPS-2 (4 vCores, 8 Go, 75 Go, 1 Gbit/s), VPS-3 (6 vCores, 12 Go, 100 Go, 2 Gbit/s), VPS-4 (8 vCores, 24 Go, 200 Go, 3 Gbit/s) — [OVHcloud Blog, « VPS 2027 : une nouvelle gamme conçue pour des projets tournés vers l'avenir », extrait](https://blog.ovhcloud.com/vps-2027-fr/).
  - Hausse de prix à partir du 01/04/2026, de +9 à +11 % en moyenne selon l'extrait — [OVHcloud Blog, « Évolutions tarifaires de Public Cloud, Bare Metal et VPS », extrait](https://blog.ovhcloud.com/evolutions-tarifaires-de-public-cloud-bare-metal-et-vps-chez-ovhcloud/).
  - Un fil de la communauté s'intitule « Gamme VPS 2027 attention nouveaux prix » (contenu non lu) — [OVHcloud Community](https://community.ovhcloud.com/t/gamme-vps-2027-attention-nouveaux-prix/53407).
- **Scaleway** : hausses ciblées au 01/06/2026, attribuées à l'inflation et à une « severe ongoing hardware crisis » (RAM, stockage) — [Scaleway, « A transparent update on Scaleway pricing », extrait](https://www.scaleway.com/en/blog/a-transparent-update-on-scaleway-pricing/) ; [AgentXCloud, extrait](https://agentxcloud.com/news/scaleway-june-2026-pricing-update). Les prix STARDUST1-S et DEV1-S donnés par l'extrait sont incohérents ; je ne les retiens pas.
- **Hostinger KVM 1** : 1 vCPU, 4 Go de RAM, 50 Go NVMe, 4 To de bande passante. 6,49 $/mois avec −67 % et un engagement de 24 mois, puis renouvellement à 11,99 $/mois (en USD, site .com) — [Hostinger, VPS hosting, extrait](https://www.hostinger.com/vps-hosting).
- **IONOS** (site US) : VPS XS à 2 $/mois pendant 3 mois avec engagement d'un an (1 vCore, 2 Go, 60 Go NVMe) ; S à 5 $ (2 vCores, 4 Go, 120 Go) ; trafic illimité — [IONOS, VPS Hosting, extrait](https://www.ionos.com/servers/vps).
- Le wiki PTB cite comme hébergeurs possibles Hetzner, OVH, Scaleway, le Raspberry Pi, Oracle Cloud « AlwaysFree » et les fonctions serverless Vercel — [PTB wiki, « Where to host Telegram Bots »](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Where-to-host-Telegram-Bots).

**Raspberry Pi 5 et auto-hébergement**
- Hausses de prix dues au coût de la mémoire LPDDR4 (« competition for memory fab capacity from the AI infrastructure rollout »). En avril 2026 : Pi 5 4 Go à 110 $ (60 $ au lancement), 8 Go à 175 $ (80 $ au lancement) ; le Pi 5 1 Go reste à 45 $ — [Raspberry Pi, « More memory-driven price rises », extrait](https://www.raspberrypi.com/news/more-memory-driven-price-rises/).
- Consommation au repos d'environ 2,7 W, sans refroidissement (≈ 50,5 °C) — [Tom's Hardware, « Raspberry Pi 5 Review », extrait](https://www.tomshardware.com/reviews/raspberry-pi-5). D'autres mesures donnent 2,1 à 3,0 W — [raspberry.tips, 2026, extrait](https://raspberry.tips/en/raspberrypi-tutorials/raspberry-pi-power-consumption-update-2026-all-models-compared).
- **Électricité (tarif bleu EDF, option Base, compteur 3–6 kVA)** :
  - 0,1940 € TTC/kWh au 01/02/2026 ;
  - 0,2001 € TTC/kWh au 01/08/2026, soit +2,6 % sur l'option Base ;
  - abonnement 6 kVA à 188,24 € TTC/an au 01/02/2026.

  Sources (extraits) : [Kelwatt, « Voici le Tarif Bleu EDF réglementé officiel du 1er août 2026 »](https://www.kelwatt.fr/actu/tarif-edf-reglemente-1er-aout-2026) ; [fournisseurs-electricite.com](https://www.fournisseurs-electricite.com/fournisseurs/edf/tarifs/bleu-reglemente) ; [CRE, proposition TRVE au 01/02/2026](https://www.cre.fr/actualites/toute-lactualite/la-cre-propose-de-maintenir-les-tarifs-reglementes-de-vente-de-lelectricite-ttc-stables-en-moyenne-au-1er-fevrier-2026-pour-les-consommateurs-souscrivant-une-puissance-inferieure-a-36-kva.html).

**Exploitation (systemd, Docker)**
- Wiki PTB « Hosting your bot » : pour que le bot survive à la déconnexion SSH, soit un multiplexeur (`screen` ou `tmux`), soit « Run your bot as a systemd service » en pointant vers le Python du virtualenv (`which python`). L'authentification SSH par clé est « strongly recommended » plutôt que par mot de passe — [PTB wiki, « Hosting your bot », révision du 04/08/2026](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Hosting-your-bot).

**Serverless**
- **Cloudflare Workers** (documentation officielle lue sur GitHub, commit du 28/09/2026) — [cloudflare-docs, limits.mdx](https://github.com/cloudflare/cloudflare-docs/blob/fbe0b92bb27210886b3d2ff8881103ea2ea50b7c/src/content/docs/workers/platform/limits.mdx) ; [pricing.mdx](https://github.com/cloudflare/cloudflare-docs/blob/fbe0b92bb27210886b3d2ff8881103ea2ea50b7c/src/content/docs/workers/platform/pricing.mdx) :
  - offre gratuite : 100 000 requêtes/jour et 10 ms de CPU par invocation ;
  - offre payante : minimum 5 $/mois, 10 M de requêtes et 30 M de ms CPU inclus, jusqu'à 5 min de CPU par requête HTTP (30 s par défaut) ;
  - durée réelle : « No limit » pour une requête HTTP tant que le client reste connecté ; `ctx.waitUntil()` prolonge l'exécution de 30 s au plus après la réponse ;
  - Cron Trigger, consommateur de Queue et alarme Durable Object : 15 min maximum.
- **Vercel Functions** (Fluid compute), selon des extraits aux informations contradictoires — [Vercel docs, « Configuring Maximum Duration »](https://vercel.com/docs/functions/configuring-functions/duration) ; [changelog « Higher defaults and limits… Fluid compute »](https://vercel.com/changelog/higher-defaults-and-limits-for-vercel-functions-running-fluid-compute) ; [changelog « Hobby … up to 60 seconds »](https://vercel.com/changelog/vercel-functions-for-hobby-can-now-run-up-to-60-seconds) :
  - durée par défaut de 300 s ;
  - Pro et Enterprise : 800 s maximum (disponibilité générale), et 1 800 s en bêta pour Node.js et Python ;
  - Hobby : un changelog annonce « up to 60 seconds », ce qui contredit « default 300 s on all plans ».

### Inferences
- **Consommation du Pi 5** : 2,7 W × 8 760 h = 23,7 kWh/an × 0,2001 € = 4,73 €/an (≈ 0,39 €/mois). Avec 5 W en moyenne (hypothèse incluant des pics de charge) : 43,8 kWh, soit 8,76 €/an. L'achat (110 à 175 $ plus alimentation, boîtier et SSD, non chiffrés) représente 2 à 3 ans d'un VPS-1 OVH.
- **Fiabilité à domicile** : elle dépend de la box, de la connexion Internet et du courant. Le long polling évite d'ouvrir des ports. Prévoir le redémarrage automatique (`Restart=always` dans systemd, ou `restart: unless-stopped` avec Docker) et des sauvegardes (non sourcé).
- **TVA** : pour un particulier en France, ajouter probablement 20 % : OVH VPS-1 3,81 € HT ≈ 4,57 € TTC ; Hetzner CX23 5,49 € HT ≈ 6,59 € TTC. Le taux réellement appliqué par l'hébergeur est à vérifier.
- **Serverless** : un agent qui travaille plusieurs minutes (appels d'outils, éventuels sous-processus) convient mal à Workers (10 ms de CPU en gratuit, pas de processus persistant, travail après réponse limité à 30 s). Il convient mal aussi à Vercel Hobby. Il faudrait des files d'attente (Queues, Workflows, Durable Objects), une complexité injustifiée face à un VPS à ~5 €/mois. L'impossibilité de lancer un sous-processus (par exemple une CLI d'agent) dans le runtime Workers reste à vérifier.
- **Dimensionnement** : 4 Go de RAM si la STT est locale (faster-whisper small int8 ≈ 1,5 Go). Sinon, 1 à 2 Go suffisent pour un bot qui appelle des API.

### Gaps
- Prix officiels en euros TTC vérifiés sur les pages Hetzner, OVH, Scaleway, IONOS France et Hostinger France : non chargés. Coût de l'IPv4 chez Hetzner et conditions d'engagement du VPS-1 OVH (« à partir de 3,81 € ») : non vérifiés.
- Prix Scaleway actuels (Stardust, DEV1-S, PLAY2) après le 01/06/2026 : non vérifié / je ne sais pas.
- Mini-PC (Intel N100, etc.) : prix et consommation non recherchés, faute de quota.
- Bonnes pratiques Docker (image, politique de redémarrage, gestion des secrets) : aucune source primaire consultée.
- Vercel Hobby (60 s ou 300 s) : contradiction non résolue.

## 8. Scénario de coût : ~20 notes vocales d'1 minute par jour, un seul utilisateur en France (hors coût du LLM, couvert ailleurs)

### Takeaway
Hors LLM, voici les ordres de grandeur :
- **Transcription** de 600 min/mois : ~0,40 $ (Groq turbo) à ~3,60 $ (OpenAI whisper-1 ou gpt-4o-transcribe) ; Google Chirp 3 à 9,60 $ fait exception.
- **Hébergement** : ~4,6 à 6,6 € TTC/mois pour un VPS.
- **Voix de réponse optionnelle** : ~5 $ (Mistral, OpenAI) à 30 $ (ElevenLabs Multilingual).

Un total de 5 à 15 €/mois hors LLM est réaliste.

### Cited Findings
Les prix unitaires utilisés, tous sourcés dans les sections précédentes, sont les suivants.

- **Transcription (STT)**
  - Groq : whisper-large-v3-turbo à 0,04 $/h ; whisper-large-v3 à ≈ 0,111 $/h (§4).
  - Mistral Voxtral Mini Transcribe 2 : 0,003 $/min (§4).
  - OpenAI : gpt-4o-mini-transcribe 0,003 $/min ; gpt-transcribe 0,0045 $/min ; whisper-1 et gpt-4o-transcribe 0,006 $/min (§4).
  - AssemblyAI : Universal-2 à 0,15 $/h ; Universal-3.5 Pro à 0,21 $/h (§4).
  - ElevenLabs Scribe v2 : 0,22 $/h (§4).
  - Deepgram Nova-3 : 0,0043 $/min en pré-enregistré (§4).
  - Google STT V2 / Chirp 3 : 0,016 $/min (§4).
- **Gemini utilisé comme transcripteur** : 1 920 tokens par minute d'audio ; audio à 0,30 $/M tokens (2.5 Flash-Lite) ou 1,00 $/M (2.5 Flash, 3 Flash preview) (§5).
- **Voix de réponse (TTS)**
  - Mistral Voxtral TTS : 0,016 $ pour 1 000 caractères (§6).
  - OpenAI : tts-1 à 15 $/M caractères ; tts-1-hd à 30 $/M ; gpt-4o-mini-tts à 0,015 $/min + 0,60 $/M caractères (§6).
  - ElevenLabs : 0,05 $ pour 1 000 caractères (Flash) ou 0,10 $ (Multilingual/v3) (§6).
  - Google Chirp HD : 30 $/M caractères (§6).
- **Hébergement**
  - OVHcloud VPS-1 : 3,81 € HT/mois ; Hetzner : CX23 à 5,49 € HT, CAX11 à 5,99 € HT (§7).
  - Hostinger KVM 1 : 6,49 $ puis 11,99 $/mois (§7).
  - Raspberry Pi 5 4 Go : 110 $ ; électricité à 0,2001 €/kWh et consommation de ~2,7 W au repos (§7).
  - Cloudflare Workers en offre payante : 5 $/mois minimum (§7).

### Inferences
**Hypothèses de volume**
- Par jour : 20 notes × 1 min = 20 min.
- Par mois (30 jours) : 600 min = 10 h.
- Par an (365 jours) : 7 300 min ≈ 121,7 h.
- Réponses vocales : 20 réponses × 500 caractères = 10 000 caractères/jour, soit 300 000 par mois et 3,65 M par an.

**A. Transcription (STT), en USD HT**

| Option | Prix unitaire | Calcul (mois) | Coût/mois | Coût/an |
|---|---|---|---|---|
| Groq whisper-large-v3-turbo | 0,04 $/h | 10 h × 0,04 | **0,40 $** | 121,7 × 0,04 = 4,87 $ |
| Gemini 2.5 Flash-Lite (audio natif) | 0,30 $/M tok + sortie 0,40 $/M | 600 × 1 920 = 1,152 M tok × 0,30 = 0,35 $ ; + 0,12–0,15 M tok × 0,40 ≈ 0,05 $ | ≈ 0,40 $ | ≈ 4,8 $ |
| Groq whisper-large-v3 | ≈ 0,111 $/h | 10 × 0,111 | 1,11 $ | 13,5 $ |
| Gemini 2.5 Flash (audio natif) | 1,00 $/M + sortie 2,50 $/M | 1,152 × 1,00 + (0,12–0,15) × 2,50 | ≈ 1,45–1,53 $ | ≈ 17,7–18,6 $ |
| AssemblyAI Universal-2 | 0,15 $/h | 10 × 0,15 | 1,50 $ | 18,3 $ |
| Mistral Voxtral Mini Transcribe 2 | 0,003 $/min | 600 × 0,003 | **1,80 $** | 21,90 $ |
| OpenAI gpt-4o-mini-transcribe | 0,003 $/min | 600 × 0,003 | 1,80 $ | 21,90 $ |
| AssemblyAI Universal-3.5 Pro | 0,21 $/h | 10 × 0,21 | 2,10 $ | 25,6 $ |
| ElevenLabs Scribe v2 | 0,22 $/h | 10 × 0,22 | 2,20 $ | 26,8 $ |
| Deepgram Nova-3 (pré-enregistré) | 0,0043 $/min | 600 × 0,0043 | 2,58 $ | 31,4 $ |
| OpenAI gpt-transcribe | 0,0045 $/min | 600 × 0,0045 | 2,70 $ | 32,9 $ |
| OpenAI whisper-1 / gpt-4o-transcribe | 0,006 $/min | 600 × 0,006 | 3,60 $ | 43,8 $ |
| Google STT V2 Chirp 3 | 0,016 $/min | 600 × 0,016 | 9,60 $ | 116,8 $ |
| faster-whisper local | 0 (CPU du VPS/Pi) | — | 0 $ | 0 $ |

Remarques :
- La sortie texte de Gemini suppose 200 à 250 tokens par minute de parole (hypothèse non sourcée).
- L'offre gratuite d'AssemblyAI (185 h de pré-enregistré, selon l'extrait) couvrirait environ 18 mois de ce scénario, si elle s'applique et sous réserve de ses conditions.

**B. Réponses vocales (TTS), si activées, en USD HT**

| Option | Calcul (mois) | Coût/mois | Coût/an |
|---|---|---|---|
| OpenAI tts-1 | 0,3 M car. × 15 $ | 4,50 $ | 54,75 $ |
| Mistral Voxtral TTS | 300 × 0,016 $ | 4,80 $ | 58,40 $ |
| OpenAI gpt-4o-mini-tts | ≈ 333 min × 0,015 + 0,3 × 0,60 | ≈ 5,18 $ | ≈ 63 $ |
| OpenAI tts-1-hd | 0,3 × 30 $ | 9,00 $ | 109,50 $ |
| Google Chirp 3 HD | 0,3 × 30 $ (hors gratuité éventuelle) | 9,00 $ | 109,50 $ |
| ElevenLabs Flash v2.5 | 300 × 0,05 $ | 15,00 $ | 182,50 $ |
| ElevenLabs Multilingual v2 / v3 | 300 × 0,10 $ | 30,00 $ | 365,00 $ |
| Piper local | — | 0 $ | 0 $ |

Pour gpt-4o-mini-tts, la durée repose sur une hypothèse d'environ 15 caractères/s : 500 caractères ≈ 33 s, soit 20 × 33 s ≈ 11,1 min/jour ≈ 333 min/mois.

**C. Hébergement 24/7**

| Option | Mensuel | Annuel |
|---|---|---|
| OVHcloud VPS-1 | 3,81 € HT ≈ 4,57 € TTC | 45,72 € HT ≈ 54,86 € TTC |
| Hetzner CX23 | 5,49 € HT ≈ 6,59 € TTC | 65,88 € HT ≈ 79,06 € TTC |
| Hetzner CAX11 (ARM) | 5,99 € HT ≈ 7,19 € TTC | 71,88 € HT ≈ 86,26 € TTC |
| Hostinger KVM 1 | 6,49 $ (promo 24 mois, soit 155,76 $ sur 2 ans) | puis 11,99 $/mois |
| Raspberry Pi 5 4 Go | électricité ≈ 0,39 à 0,73 €/mois | 4,73 à 8,76 € + achat 110 $ hors accessoires |
| Cloudflare Workers (offre payante) | 5 $ minimum | — (peu adapté à un agent long, voir §7) |

La TVA à 20 % est supposée pour les montants TTC.

**D. Exemples de configurations complètes (hors LLM)**
- **Minimal** : OVH VPS-1 (4,57 € TTC) + Groq turbo (0,40 $) + réponses en texte seulement ≈ 4,57 € + 0,40 $ par mois, soit ≈ 54,86 € TTC + 4,87 $ par an.
- **Européen / RGPD** : OVH VPS-1 + Voxtral Transcribe 2 (1,80 $) + Voxtral TTS (4,80 $) ≈ 4,57 € + 6,60 $ par mois.
- **Voix premium** : Hetzner CX23 (6,59 € TTC) + Scribe v2 (2,20 $) + ElevenLabs Multilingual (30 $) ≈ 6,59 € + 32,20 $ par mois.
- **Tout local** : Pi 5 + faster-whisper/whisper.cpp + Piper ≈ 0,4 à 0,7 €/mois d'électricité, plus le matériel. La latence de transcription sera élevée : ≈ 2 min pour une note d'1 min avec whisper small sur Pi 5, d'après l'extrait à faible confiance du §4.

**Autres ordres de grandeur**
- Le poste STT est marginal (< 4 $/mois hors Google). La voix de réponse et le LLM dominent le budget.
- Passer de 20 à 60 notes par jour multiplie la STT par 3 et laisse l'hébergement inchangé.

### Gaps
- Coût du LLM (Claude) : exclu, couvert par un autre chercheur.
- Taux de change USD→EUR et TVA appliquée par les fournisseurs d'API étrangers à un particulier français : non vérifiés, donc aucune conversion effectuée.
- Conditions exactes des offres gratuites (AssemblyAI, Google TTS, crédits Deepgram, niveau gratuit Gemini, qui sert à l'amélioration des produits Google) : non vérifiées en détail.
- Nombre moyen de caractères par réponse vocale et débit de parole en français : hypothèses de travail (500 caractères, ~15 caractères/s), non sourcées.
