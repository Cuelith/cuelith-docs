# Stato del progetto e come riprendere

Aggiornato il **2026-10-06** (sera). Questo è il primo file da leggere per riprendere il lavoro su Cuelith senza la conversazione precedente. Dice dove siamo, cosa è in sospeso e cosa viene dopo. I dettagli stanno nel [documento di progetto](documento/index.html) e nelle [decisioni](decisioni).

## In una riga

Cuelith 0.2.0 è pubblico (Windows e Linux, italiano e inglese), il sito è online su `cuelith.lzrhive.it`, i repo sono pronti per contributi esterni. **Dal 2026-10-05 il nucleo è GPL 3.0 con eccezione per i plugin** (decisione 0012): la prima release con la nuova licenza è la **0.2.5**, pubblica. Prossimo lavoro: **Fase 1** (due prove tecniche) e la decisione 0013 sul marketplace.

## Cosa è pubblico oggi

| Cosa | Versione | Dove |
| --- | --- | --- |
| Programma (nucleo) | **0.3.3** (pubblica il 2026-10-07: schede della colonna di sinistra che scorrono su una riga con frecce, sfumature e elenco ▾, colonna delle icone dei plugin che scorre col «+» sempre raggiungibile; prova e2e `tabs.spec.ts` che con 7 plugin veri fallisce senza la correzione; la 0.3.2: disinstallare un plugin in funzione non dà più «Errore interno», e installare/disinstallare mostra «in corso»; la 0.3.1, del 2026-10-06: «Prodotto da… Venduto da…», senza affiliazione; la 0.3.0 portò il marketplace a pagamento, la 0.2.5 fu la prima con la GPL) | repo `cuelith-core`, release con installatore Windows e AppImage Linux. Le release 0.1.0 e 0.2.0 sono state **eliminate** il 2026-10-05 (restano i tag Git) |
| Protocollo / SDK | 1.15.0 / tag v0.7.0 | `cuelith-sdk` |
| Lingua italiana | 0.2.5 | `plugin-locale-it` (inclusa nell'installatore) |
| Lingua inglese | 0.1.5 | `plugin-locale-en` (inclusa nell'installatore) |
| Plugin **Brani** (prima «Canti») | 0.5.2 (GPL; le 0.5.0 e 0.4.2 Apache sono ancora scaricabili dal repo del plugin) | `plugin-songs`, nel marketplace |
| Plugin d'esempio «Ciao» | 0.2.0 | `plugin-template` |
| Marketplace | Brani 0.5.2, 0.5.1, 0.5.0 e 0.4.2 | `cuelith-registry` → `https://cuelith.github.io/cuelith-registry/index.json` |
| Sito | v0.1.3 (con Ko-fi, link a lzrhive e dati strutturati per i motori di ricerca) | `cuelith-site` → Cloudflare Pages, progetto `cuelith`, <https://cuelith.lzrhive.it> |
| Regole per chi contribuisce | — | repo `Cuelith/.github` (cartella locale `cuelith-community`) |
| Documentazione | v0.2.0 | questo repo |

Tutti i repo stanno affiancati in `C:\1.Materiali\Cuelith\`. Il 2026-10-03 erano tutti puliti e inviati. Anche l'hub personale `lzrhive.it` (cartella `C:\1.Personale\lzrhive.it`, repo `ML-lzrhive/lzrhive.it`) è collegato a questo lavoro: vedi sotto.

## Licenze (decisione 0012, dal 2026-10-05)

| Repo | Licenza |
| --- | --- |
| `cuelith-core`, `plugin-songs`, `plugin-locale-it`, `plugin-locale-en` | **GPL 3.0 o successiva** (nucleo con `PLUGIN-EXCEPTION.md`: i plugin che usano solo protocollo, SDK, pannelli e file di dati hanno la licenza che vogliono) |
| `cuelith-sdk`, `plugin-template` | **Apache 2.0**, di proposito: i plugin li incorporano |
| `cuelith-registry`, `cuelith-docs`, `cuelith-site` | Apache 2.0 |

Le versioni già pubblicate (nucleo 0.2.0, Canti 0.5.0, lingue) restano Apache: non si ritira. Mai copiare codice del nucleo nell'SDK o in un plugin; mai dipendenze incompatibili con la GPLv3 (`pnpm licenses list`). Il nome «Cuelith» non è nella licenza: README del nucleo e `TRADEMARK.md`.

## Regole di lavoro che valgono sempre

- **Rami**: il lavoro va su `dev` (ramo predefinito di ogni repo); `main` riceve solo versioni con tag SemVer. `main` e `dev` sono protetti: chi non è amministratore passa da pull request, approvazione e controlli verdi. Il fondatore (e chi lavora con le sue credenziali `gh`) può inviare direttamente: GitHub stampa «Changes must be made through a pull request» ma l'invio passa.
- **Costi zero.** Nessun segreto in chat. Il fondatore va guidato passo per passo nelle cose che fa lui.
- **Si risponde in italiano.**
- **Mai nominare altri programmi** del settore in prodotto, sito, documenti, prove, commenti, note di versione. Formati aperti e protocolli sì.
- **Nessuna funzione finta.** Ogni passo si prova nel programma vero prima del successivo.
- **«Plugin»** nei testi che vede l'utente (programma, sito, documenti per chi contribuisce); **«modulo»** resta il nome tecnico in codice, protocollo e decisioni.
- **Ogni testo nuovo in due lingue**: italiano e inglese, stesse chiavi e stessi segnaposto.
- **Telefono in verticale**: ogni cosa del sito va guardata anche a 400 px.
- I plugin dichiarano `engines.cuelith` come `>=0.1.0 <1.0.0` (per le versioni 0.x `^0.1.0` esclude la 0.2.0).

## In sospeso: cose che aspettano qualcuno

| Cosa | Chi | Cosa fare quando si sblocca |
| --- | --- | --- |
| **Mail a SignPath per il cambio di licenza: ora la release 0.2.5 esiste** | fondatore | Aggiornare il testo in [firma/email-signpath-licenza.md](firma/email-signpath-licenza.md) citando la 0.2.5 come prima release da firmare, poi inviarlo. |
| **Parere legale** su eccezione per i plugin e su obblighi da «piattaforma online» (DSA) | fondatore | Prima di qualsiasi plugin a pagamento nel marketplace, non prima. |
| **Marketplace senza commissione (decisione 0014, 2026-10-06)**: **pubblicato**. Il fondatore ha tolto commissione, affiliazione e anti-dirottamento (costo zero, nessun avvocato a pagamento, rischio accettato). In produzione (sito 0.4.0, 126 prove): condizioni v2 (12 articoli, regole di condotta, verifica dell'autore come facoltà), informativa `/privacy/`, due caselle di accettazione, traccia dell'accettazione senza IP, scheda «Prodotto da… Venduto da Lemon Squeezy», indirizzo `contact@lzrhive.it` (Cloudflare Email Routing → Gmail del progetto dentro la Cartella protetta del telefono; «Invia come» via SMTP di Gmail con password per le app; SPF unico con Cloudflare e Google). **Il sito è ora a pagine**: home breve (hero, numeri, tre scelte, quattro porte, download, Ko-fi) e pagine Funzioni, Plugin, Download (con tutte le versioni), Domande (a gruppi), Contatti; menu con Home; menu a pulsante e layout compatto sul telefono; i vecchi `/#download`, `/#funzioni`... portano alla pagina giusta. GitHub `.github`: guida autori, sicurezza e condotta aggiornate. **Da fare**: il pacchetto per un avvocato in `documenti-riservati/` descrive ancora il modello con commissione (rifarlo prima dell'invio); altri indirizzi (`security@`, `privacy@`, `authors@`) solo se servono; piano a costo zero (suite di compatibilità, matrice a ogni release, verifica dell'autore, kit autori) nella decisione 0014; nell'app, togliere la nota sul riferimento di affiliazione e aggiungere «Prodotto da… Venduto da…» (versione 0.3.1). | fondatore + Claude | — |
| **Store, conformità, verifica degli autori, guida (2026-10-06, costo zero)**: **fatto** — (1) sito 0.5.0: il marketplace è uno *store* (riquadri con nome, cosa fa, autore, prezzo; ricerca; scheda per plugin disegnata al momento su `/marketplace/<id>/`; sul telefono elenco compatto stile app, nessun download perché i plugin si installano solo sul computer); «canti» → **«brani»** ovunque. (2) **Suite di conformità** `cuelith-core/apps/conformance` (ramo `dev`): controlli sul pacchetto (manifest, intervalli, file dichiarati, permessi, segreti e chiavi private, file indesiderati, pannelli contro la CSP, euristiche sui permessi) e prove con il **motore vero** (installa, attiva, resta su, risorge dopo SIGKILL, si ferma pulito, riaccende); 17 prove; è su `main` e allegata alla release **0.3.1** come `cuelith-conformance.mjs` (un solo file); le release future la allegano da sole (`release.yml`). `pnpm conformance` nel modello la scarica e la lancia: superata sul modello «Ciao», Windows, prove complete compreso l'arresto brusco. (3) **Matrice**: `cuelith-registry/.github/workflows/conformance.yml` + `scripts/conformance.mjs` provano l'ultima versione di ogni plugin a ogni proposta, ogni lunedì e a mano a ogni release (`gh workflow run conformance.yml -R Cuelith/cuelith-registry -f core=X.Y.Z`); verificata in CI. (4) **Verifica dell'autore**: Worker `cuelith-site/liveness/` (cron giornaliero, 9 prove): ogni 180 giorni email di conferma con un pulsante, due promemoria a 14 giorni, poi **dormiente** + quarta email; conferma anche dopo; `/status.json` elenca i dormienti. **Acceso il 2026-10-06** su `liveness-cuelith.lzrhive.it` (Resend, dominio verificato; prova dal vero: invio e conferma riusciti, dati di prova tolti). (5) **Guida autori** `AUTHOR-GUIDE(.it).md` in `.github` (strada con AI, testo guida da incollare, modello di esecuzione, rotture note, verifica, pubblicazione, dopo la pubblicazione). **Da fare**: dal fondatore, account Resend (piano gratuito) con dominio `lzrhive.it` verificato e `npx wrangler secret put RESEND_API_KEY` nel Worker, poi `wrangler deploy` in `cuelith-site/liveness` e impostare `SITE_ORIGIN`; da me, collegare i dormienti al registro (oggi `/status.json` non è letto da nessuno: indice e app non li nascondono ancora), «supporto» nelle voci del registro (richiede un campo nuovo nel protocollo, quindi nuovo `@cuelith/protocol`). **`@cuelith/*` su npm: fatto il 2026-10-06** (account `cuelith`, `protocol` 1.15.0, `sdk`/`panel`/`ui` 0.7.0; installazione pulita provata); il modello `plugin-template` e la CI lo usano da npm, niente cartelle affiancate; per ripubblicare serve il codice 2FA del fondatore (`pnpm -r publish --access public --no-git-checks` in `cuelith-sdk`, dopo aver alzato le versioni). **Prova generale da autore esterno (2026-10-07)**: modello clonato da zero, plugin `acme.countdown` creato, 8 errori tipici provati (font web nel pannello, rete senza permesso, `activate` lento, `process.exit`, id con maiuscole, chiave con trattino, chiave privata nel pacchetto, versione non ricostruita): tutti individuati; corretti i messaggi del manifest (campo + spiegazione), il `CLAUDE.md` del modello (era dei manutentori) e aggiunto `pnpm registry` (voce del registro già pronta, accettata dal validatore). **Prova nell'app (2026-10-07, fondatore)**: installare da cartella funziona ma non mostrava nulla (aggiunto indicatore); disinstallare un plugin acceso dava «Errore interno» e poi «non installato» ma l'elenco lo mostrava (causa: su Windows i file erano ancora aperti dal processo; ora si aspetta la chiusura e si ritenta, e le cartelle rimaste si tolgono all'avvio; prova nuova nel motore che falliva col vecchio codice). Il tempo di installazione nel motore è ~0,14 s: la lentezza vista nell'app va riverificata con la 0.3.2. **Assistenza**: campo `support` (https o mailto) nelle voci del registro, fuori da `index*.json` (le app 0.3.x rifiutano campi nuovi) e in `support.json`; scheda del plugin sul sito (0.5.1) e `pnpm registry --support`. **Plugin dormienti**: il registro li toglie da `index.json`/`index-2.json` (non da `licenses.json`) leggendo `/status.json` e rifà l'indice ogni giorno alle 05:45 UTC. | fondatore + Claude | — |
| **Marketplace a pagamento, versione con commissione (decisione 0013, superata in parte dalla 0014)**: **pubblicato il 2026-10-06** con nucleo 0.3.0, SDK 0.7.0 (protocollo 1.15), lingue 0.2.2/0.1.2, plugin-template 0.2.0, sito 0.2.0, registry su `main` senza la voce di prova | fondatore + Claude | Architettura e verifiche dal vero nella [decisione 0013](decisioni/0013-marketplace-a-pagamento-senza-account.md). Il sito vero ha pagine, modulo con Turnstile (informativa aggiornata), pannello dietro Access e Notaio; `index.json` (schema 1) è intatto per le app vecchie. Condizioni per gli autori: pagina `/marketplace/submit/` e `DEVELOPERS*.md` (chiave senza scadenza, 3 computer, computer rotto = l'autore libera il posto dal suo negozio, rimborso = licenza spenta). **Da fare dal fondatore**: cancellare `C:UsersMattiaLazzaricuelith-notary-chiave-privata.txt` (o conservarla in luogo sicuro) ora che è su Cloudflare; togliere `ALLOW_TEST_MODE` da Anteprima a fine prove; Search Console. **Ancora da verificare dal vero**: un rimborso vero, la scadenza `expired`, l'aggiornamento 0.2.0→0.3.0 su un computer vero. Prima di accettare il primo plugin a pagamento di terzi: parere del commercialista sul 10% (confermato dal fondatore il 2026-10-06 di procedere). |
| **Firma dell'installatore Windows**: domanda inviata a SignPath Foundation il 2026-10-02 | risposta di SignPath via email al fondatore | Se chiedono chiarimenti: risposte pronte in [firma/domanda-signpath.md](firma/domanda-signpath.md). Se accettano: aggiungere al README di `cuelith-core` e alla sezione Download del sito la frase «Free code signing provided by SignPath.io, certificate by SignPath Foundation»; collegare la firma a `release.yml`; ogni rilascio va poi approvato a mano. Se rifiutano per mancanza di utenti: ripresentare tra qualche mese con i download veri. |
| **Marchio «Cuelith»**: rimandato per il costo (149–183 € in Italia) | fondatore | Depositare quando il progetto ha utenti o prima del primo incasso. Marchio denominativo, classi 9 e 42, portale UIBM. Ricerca fatta: nessun «Cuelith»; unico simile in UE «CueLight» (classi 11, 16, 41, non software). Il fondatore ha detto che il nome «al massimo può essere ripensato in futuro». |
| **Email del progetto** per le segnalazioni | fondatore | Oggi sicurezza e comportamento rimandano alla pagina riservata di GitHub. Con un indirizzo vero, aggiornare `SECURITY.md` e `CODE_OF_CONDUCT.md` nel repo `.github`. |
| **Accordo di contribuzione** (firma automatica alla prima pull request) | prima proposta da un account esterno | Mai visto in azione: controllare che il commento dell'automatismo compaia e che la firma finisca nel ramo `cla-signatures`. |
| **Animazioni del sito** | giudizio del fondatore | Verificate su fotogrammi fermi a tre larghezze, non in movimento. Se qualcosa non convince, si regola in `cuelith-site/src/app.js` (zone d'ingresso e d'uscita) e `styles.css`. |
| **Scheda di Cuelith sull'hub lzrhive.it** | fondatore, dalla console `https://lzrhive.it/admin/` | Il sito di Cuelith ha già il link a lzrhive e a Ko-fi. La scheda da creare (testi italiano e inglese, immagine `cuelith-core/brand/lzrhive-card.jpg`) è in [hub-lzrhive.md](hub-lzrhive.md). Il fondatore ha segnalato l'errore «file immagine non valido» al caricamento: era un difetto della console (le immagini `blob:` erano bloccate dalla sua politica di sicurezza), corretto e pubblicato su `main` dell'hub il 2026-10-03, provato in un browser vero. Da confermare che ora il caricamento riesca (ricaricare con Ctrl+F5). Il fondatore ha poi detto che la scheda è fatta ("il punto 2 è già fatto"). |
| **Google Search Console** per `lzrhive.it` | fondatore | Il sito non compare ancora cercando «Cuelith» (online da due giorni, nessun link entrante). Guida data al fondatore: proprietà di tipo Dominio `lzrhive.it`, record TXT `google-site-verification=…` su Cloudflare (DNS), poi invio di `https://cuelith.lzrhive.it/sitemap.xml` e «Richiedi indicizzazione» per `/` e `/en/`. Controllo: la ricerca `site:cuelith.lzrhive.it`. Il sito è tecnicamente a posto (nessun noindex, mappa del sito, canonical, dati strutturati `SoftwareApplication`); ciò che manca è tempo e link. |

## Cose fatte ma non ancora provate dal vero

- **Release 0.2.5 verificata dal vero** (2026-10-05): impronta dell'installatore uguale a `latest.yml`, pacchetto estratto con 7-Zip (non installato: sul computer del fondatore c'è la 0.2.0, non si tocca) con `resources/legal/` (LICENSE, NOTICE, PLUGIN-EXCEPTION.md), lingue 0.2.1 e 0.1.1 in GPL, 26 prove e2e verdi con `CUELITH_E2E_EXECUTABLE`. Non provato: aggiornamento da 0.2.0 a 0.2.5 su un computer vero.
- **Nuova procedura di rilascio** (`cuelith-core/.github/workflows/release.yml`): la release nasce in bozza con le note di `versioni/X.Y.Z.md` e diventa pubblica solo con tutti gli installatori. Si vedrà al prossimo rilascio. Se qualcosa non va, la versione resta in bozza e nessuno la vede.
- **Aggiornamento da 0.1.0 a 0.2.0 su un computer vero**: provato solo il controllo («Cuelith è aggiornato» dalla 0.2.0), non il passaggio di versione.

## Come si rilascia

1. Ordine: `cuelith-sdk` → lingue → plugin (e `cuelith-registry` se cambia un plugin) → `cuelith-docs` → `cuelith-core` → sito.
2. In ogni repo: controlli verdi su `dev`, poi `git push origin dev:main`, tag annotato `vX.Y.Z` su `dev`, invio del tag.
3. Nucleo: prima scrivere `versioni/X.Y.Z.md` (italiano, una riga `---`, inglese); la versione in `apps/desktop/package.json` deve essere uguale al tag.
4. Plugin nel marketplace: dopo la release del plugin, aggiornare `plugins/<id>.json` nel registry con l'impronta SHA-256 del pacchetto **pubblicato** (non di quello locale), poi `dev` → `main`.
5. Verifica dopo il rilascio: scaricare l'installatore dal sito, confrontarlo con `latest.yml`, installarlo in silenzio (`/S`), far girare le prove principali con `CUELITH_E2E_EXECUTABLE`, controllare gli aggiornamenti, disinstallare.
6. Sito: `pnpm shots` (schermate) se l'interfaccia è cambiata, `node build.mjs`, `dev` → `main`, tag, `pnpm exec wrangler pages deploy dist --project-name cuelith --branch main`. `wrangler` su quel computer è già collegato all'account.

## Prossimo lavoro: Fase 1

La Fase 1 del documento (cap. 30) porta camera, scene, sottopancia e streaming. Prima di costruirla vanno chiuse le due proposte lasciate aperte nella [decisione 0007](decisioni/0007-processi-dei-moduli.md), perché tutto il resto ci si appoggia:

1. **Canale dei fotogrammi.** Come un plugin che produce o riceve video passa i fotogrammi al programma senza metterli nei messaggi JSON. Proposta: memoria condivisa con buffer circolare. **Serve una prova tecnica** su Electron 44: lettura della memoria condivisa nella finestra di uscita, a confronto con un socket locale a fotogrammi grezzi. Criterio: 1080p a 60 fotogrammi al secondo senza perderne, misurati come nella prova «le uscite non cadono».
2. **Modello audio.** Il documento descrive le sorgenti solo come immagine: mancano traccia audio per sorgente e mix audio per uscita. Va scritta la proposta e fatta approvare al fondatore.

Poi, nell'ordine della [decisione 0001](decisioni/0001-librerie-canti-accordi.md): media del nucleo (immagini, video, audio con scelta dell'uscita) → Accordi → Bibbia → disposizione Culto → compositing delle scene.

Ogni decisione nuova va scritta in `decisioni/` con il numero successivo (la prossima è la **0014**) e confermata dal fondatore se cambia ciò che vede l'utente.

## Dove guardare per ogni cosa

| Argomento | Dove |
| --- | --- |
| Visione, interfaccia, specifica | [documento/index.html](documento/index.html) |
| Lingue, scelta della lingua, «plugin», note in due lingue | [decisione 0010](decisioni/0010-lingua-inglese-e-scelta-della-lingua.md) |
| Contributi esterni, protezioni, accordo di contribuzione, nome e logo | [decisione 0011](decisioni/0011-contributi-e-tutela.md) |
| Licenza GPL, SDK Apache, eccezione per i plugin, marchio | [decisione 0012](decisioni/0012-licenza-gpl-e-eccezione-plugin.md) |
| Marketplace a pagamento: architettura, fasi, commissione, cosa è vero e cosa no | [decisione 0013](decisioni/0013-marketplace-a-pagamento-senza-account.md) |
| Stili del testo (globali sotto gli sfondi, dell'editor, controllo dello spazio): decisi; fase 1 (calcolo e controllo) fatta su `dev`, non ancora in una release | [decisione 0015](decisioni/0015-stili-del-testo.md) |
| Marketplace senza commissione: cosa cambia, stati dei plugin, chi risponde, piano a costo zero | [decisione 0014](decisioni/0014-marketplace-senza-commissione.md) |
| Plugin a pagamento, prima proposta (le parti Account, Server licenze, Negozio sono **superate dalla 0013**) | [decisione 0008](decisioni/0008-moduli-a-pagamento.md) |
| Regole del sito (tono, schermate, animazioni, inglese tutto in inglese) | `cuelith-site/CLAUDE.md` e `README.md` |
| Regole per chi sviluppa plugin | repo `.github`: `DEVELOPERS.md`, `CONTRIBUTING.md` |
| Regole di ogni repo | il suo `CLAUDE.md` |

## Hub lzrhive.it: cosa serve sapere

- L'hub è anonimo: mai il nome del fondatore nei testi, nell'identità Git (`lzrhive` / `ML-lzrhive@users.noreply.github.com`) né nei campi pubblici.
- Lavoro sul ramo `dev` dell'hub; `main` (pubblicazione automatica su Cloudflare) solo quando il fondatore dà il via. Il 2026-10-03 il via è stato dato per la correzione del caricamento immagini.
- La console sta dietro Cloudflare Access (codice via email) e poi password + verifica in due passaggi: non è raggiungibile da qui.
- Il Ko-fi del fondatore è `https://ko-fi.com/mlhive`, già impostato nell'hub e usato dal sito di Cuelith. Gli unici indirizzi esterni ammessi nel sito di Cuelith sono Ko-fi e `lzrhive.it` (controllo in `cuelith-site/test/site.test.mjs`).

## Trappole scoperte di recente

- **Chi apre l'app su Linux senza portachiavi non può attivare licenze**: `safeStorage` ripiega su un testo non protetto (`basic_text`); `electronSecrets` lo rifiuta di proposito. Le prove usano `CUELITH_TEST_SECRETS=memory`, un Notaio di prova (`CUELITH_TEST_NOTARY_URL`, `CUELITH_TEST_NOTARY_KEYS`) e `CUELITH_TEST_REGISTRY_DIR` con `index-2.json`: nomi che iniziano per `CUELITH_TEST_` e **mai** da usare altrove.
- **Il Notaio vero firma con l'ora del sistema**: nelle prove che viaggiano nel tempo si porta anche `Date` (`vi.useFakeTimers({ toFake: ["Date"] })`) al tempo simulato, altrimenti il rinnovo sembra non riuscire.
- **Il codice che passa `URLSearchParams` come corpo di `fetch`** non si legge con `typeof body === "string"`: i finti fornitori delle prove usano `instanceof URLSearchParams`.
- **I test che sostituiscono `notary.fetch` dopo aver creato il servizio** non hanno effetto se il servizio ha tenuto il riferimento: si collega la rete in modo «tardivo» (`(input, init) => notary.fetch(input, init)`).
- **L'endpoint dell'API delle licenze di Lemon Squeezy è `https://api.lemonsqueezy.com/v1/licenses/…`** (non `lemonsqueezy.com`): non esiste un host «sandbox», la modalità di prova è un interruttore del negozio (chiavi con `test_mode: true`).
- **`await reader.cancel()` su una risposta duplicata (`Response.clone()`) non si risolve mai** finché l'altro ramo non è letto: nel codice si usa `reader.cancel().catch(...)` senza attendere.
- **`_headers` di Cloudflare Pages somma le intestazioni di più regole**: per allargare la CSP solo su una pagina si toglie prima la generale con `! Content-Security-Policy` (provato in locale con `wrangler pages dev`).
- **Le Pages Functions free hanno 10 ms di CPU a richiesta**: l'analisi di un pacchetto decomprime solo manifest e icona (con `filter` e limiti di `originalSize`); un pacchetto grande va bene perché impronta e download non pesano sulla CPU.
- **La CSP del sito (`form-action 'none'`, `script-src 'self'`, `connect-src 'self'`) vale anche per le pagine nuove**: il modulo di proposta non si invia da solo, lo fa lo script con `fetch` allo stesso sito. Turnstile (fase 3) ha bisogno di script e cornici da `challenges.cloudflare.com`: la regola va aggiunta in `_headers` solo per `/marketplace/submit*`.
- **Il feed `releases.atom` e i test del sito**: i test di `market.test.mjs` cercano gli indirizzi esterni con un nome di sito vero; la frase «inizia con https://.» nei messaggi non è un indirizzo.
- **I programmi installati rifiutano un indice del registry con campi che non conoscono** (schema stretto): `index.json` (schema 1) non deve mai ricevere campi nuovi né plugin a pagamento; per quelli c'è `index-2.json`. In `build-index.mjs` si scrive l'oggetto originale, non quello restituito da `parse()` (che aggiunge `access`).
- **La cache di Turbo non si accorge dei cambi dell'SDK** (sta fuori dal repo del nucleo, collegato con `link:`): dopo ogni modifica all'SDK si controlla il nucleo con `pnpm exec turbo run typecheck lint test --force`. Il 2026-10-05 la cache nascondeva un errore di tipi.
- **Negli script scritti al volo (heredoc con Node) i `\n` e le barre inverse delle regex si perdono o diventano a capo veri**: per testi con escape si usa l'editor, o uno script salvato su file, e si rilegge il risultato.
- **`z.url({ hostname })` non guarda il nome utente**: `https://utente:pw@negozio.example/` passa; l'indirizzo di acquisto si controlla anche con una regola sul `@` prima del primo `/`.
- **Il feed `releases.atom` di GitHub elenca anche i tag senza release** (titolo = nome del tag). Dopo aver eliminato una release il suo tag resta e la voce resta nel feed: il sito (`functions/_lib/sources.js`) le scarta guardando il titolo, che nelle nostre release è «Cuelith X.Y.Z». Se cambia il modo di titolare le release, cambia anche quel controllo.
- **Eliminare una release non ritira la licenza data**: chi ha già scaricato 0.1.0 o 0.2.0 le ha con Apache. Vale solo per la distribuzione nostra.
- **Le vecchie distribuzioni di Cloudflare Pages** (4 di produzione e 1 di anteprima, con la licenza Apache, raggiungibili da `<id>.cuelith.pages.dev`) sono state lasciate: il fondatore ha detto che non sono attive. La cancellazione con `wrangler pages deployment delete` fu bloccata dai permessi; si può fare dalla dashboard.
- **Su questo computer c'è un Cuelith 0.2.0 installato dal fondatore** (`%LOCALAPPDATA%\Programs\Cuelith`): per le prove sull'installatore non lanciarlo (sovrascrive e registra la disinstallazione), ma estrarlo con 7-Zip.
- **Cambiare licenza di un plugin cambia il pacchetto**: il manifest `cuelith-plugin.json` e il `LICENSE` stanno dentro il `.cpkg`, quindi l'impronta SHA-256 cambia. Per questo i plugin non si ritoccano senza un nuovo rilascio, e il registry va aggiornato solo con l'impronta del pacchetto pubblicato.
- **`LICENSE` deve restare il testo GPL puro**: GitHub lo riconosce così. L'eccezione per i plugin sta in `PLUGIN-EXCEPTION.md`, `NOTICE` e README, non dentro `LICENSE`.
- In bash un comando con virgolette annidate e apostrofi può rompersi a metà: per le modifiche lunghe ai testi meglio uno script Node su file.
- **La politica di sicurezza della console dell'hub ammette immagini solo da sé e da `data:`**: niente `blob:`. Per leggere un file scelto dall'utente si usa `createImageBitmap` o `data:`.
- Nelle prove, un comando concatenato in PowerShell non si ferma da solo se un passo fallisce: controllare l'esito dei test prima di pubblicare.

- **Pannello vuoto per 10 secondi** alla prima apertura di un plugin appena installato: era la prima lettura dei file nuovi trattenuta dai controlli del sistema, non la poca memoria. Risolto con la lettura anticipata (`apps/engine/src/modules/warm.ts`).
- **Chi riceve dati dal motore non li valida in modo rigido**: un campo nuovo nello stato (com'è stato `live.lang`) non deve rompere pannelli e plugin costruiti prima.
- Le prove dell'app partono sempre con `CUELITH_LANG=it` e `CUELITH_LANGS=it`; chi prova le lingue passa `CUELITH_LANGS: "it,en"`.
- In Playwright `toHaveValue` su un elemento che non è un campo fallisce subito, senza riprovare.
- Le schermate del sito non si usano se il semaforo delle risorse non è verde (`pnpm shots` lo controlla): sul computer del fondatore capita quando resta poca memoria libera, e si riprova.
- Una prova legata ai tempi può far fallire il rilascio su una macchina lenta: le prove guardano l'esito, non l'istante.
