# Decisione 0018: finestra Plugin, lingue a parte, avvio guidato, installatore e vetrina

- **Data**: 2026-10-08
- **Stato**: **decisa e fatta su `dev`** (il fondatore ha lasciato la scelta a Claude: «pensa di essere l'utente»); da provare nell'app del fondatore

## Cosa si decide

1. **Finestra «Plugin» in tre schede**: *Marketplace*, *Installati*, *Lingue*. Le lingue (famiglia `locale`) non stanno più in mezzo ai plugin: la scheda **Lingue** mostra quelle installate, con «In uso» o «Usa questa lingua» (cambia subito, come nelle Impostazioni), e sotto le altre dal marketplace. In *Marketplace* e *Installati* ci sono solo plugin.
2. **Marketplace come vetrina**: ricerca, filtro **Tutti / Gratuiti / A pagamento** (solo se ci sono plugin a pagamento), i già installati in fondo, **immagine di copertina** sulla scheda e **«Come si usa»** (i passi della guida) prima di installare.
3. **Avvio guidato** («Benvenuto in Cuelith»): la prima volta, se non c'è nessuno strumento installato, propone i plugin più utili (Brani per primo) con cosa fanno e che permessi chiedono; installa con un clic, mostra la guida del plugin e apre lo strumento. «Più tardi» o «Fatto» lo ricordano **sul computer** (`preferences.json`, campo `welcomeSeen`: vale solo per l'app desktop, mai da una postazione in browser). Si ritrova sempre con Ctrl+K → «benvenuto». Riaperto, sa cosa è già installato.
4. **Brani non è nell'installatore**: l'installatore porta solo le lingue italiana e inglese (già così); Brani si installa dal marketplace, e l'avvio guidato lo propone per primo.
5. **Immagine e guida dagli autori, dal pacchetto** (protocollo 1.19): il manifest ha `image` (PNG, JPEG o WebP, 150 KB al massimo); la guida d'uso è l'`onboarding` che c'è già, letto con i file di lingua. Il form di proposta non ha campi in più: quando si approva, la pull request nel registro aggiunge `plugins/<id>.<ext>` e il campo `guide` della voce. Il registro li pubblica in **`extras.json`**, non negli indici (le app già installate rifiutano campi nuovi); le app 0.4 lo leggono a parte e, se manca, funzionano lo stesso. Il sito mostra immagine e guida nella pagina del plugin.
6. **Installatore**: scelta della lingua (italiano o inglese) all'inizio; l'app installata parte in quella lingua al primo avvio (`resources/install-lang.txt`, poi vale la scelta nelle Impostazioni); se Cuelith è già installato la prima pagina dice che è un **aggiornamento** («Aggiornamento di Cuelith… i tuoi show, le impostazioni e i plugin restano dove sono»).

## Com'è fatto

- Client: `ModulesWindow.tsx` (schede, filtri, `Cover`, `HowToUse`, `LocaleUse`), `WelcomeDialog.tsx`, `Shell.tsx` (condizione dell'avvio guidato), `Palette.tsx` (voce «Benvenuto»).
- Desktop: `installation.ts` (`welcomeSeen`), `installLang.ts`, `build/installer.nsh`, `electron-builder.yml` (installatore a due lingue).
- Motore: `modules/marketplace.ts` legge `extras.json` e lo unisce alle voci (anche nella copia salvata).
- SDK 1.19: `image` nel manifest, `CoverImageSchema`, `GuideSchema`, `RegistryExtrasSchema`, `REGISTRY_EXTRAS_URL`.
- Registro: `validate.mjs` (immagine vera, una sola, 150 KB, uguale a quella del pacchetto), `build-index.mjs` (`extras.json`).
- Sito: `package.js` (immagine e guida dal pacchetto), `entry.js` e `review.js` (nella pull request), `sources.js` (`loadExtras`), pagina del plugin.
- Prove: `modules.spec.ts` (schede, Lingue, avvio guidato, copertina e «Come si usa»), `registry.test.mjs`, `review.test.mjs`, `store.test.mjs`, `modules.test.ts`, `installLang.test.ts`, `registry.test.ts` e `manifest.test.ts` dell'SDK.

## Da sapere

- **Riconoscimento dell'aggiornamento** (corretto il 2026-10-08 dopo la prova del fondatore: la lingua funzionava ma l'aggiornamento non veniva riconosciuto). Ora si controlla all'avvio dell'installatore, in entrambe le sezioni del registro (utente e computer), e l'installatore scrive `resources/install-kind.txt` («new» o «update»): due installazioni di fila con una copia di prova (altro identificatore) danno «new» e poi «update». La pagina di benvenuto con il testo «Aggiornamento di Cuelith» usa lo stesso controllo; **la sua grafica va comunque guardata a mano** (non posso vederla da qui).

- **Il testo della procedura guidata dell'installatore non l'ho potuto vedere** (solo compilato e provato in silenzio: installa, scrive la lingua, l'app parte nella lingua scelta; disinstalla). Va guardato una volta a mano con `installatori-prova/Cuelith-Setup-0.4.0.exe`: la lingua all'inizio e, su un computer dove Cuelith c'è già, la prima pagina «Aggiornamento di Cuelith».
- Per togliere l'immagine a un plugin che l'aveva, serve togliere anche il file dal registro: la CI confronta con l'ultimo pacchetto e blocca la pull request finché non coincidono.
- Un autore che non mette `image` o `onboarding` non perde nulla: la scheda è come prima.

## Ancora da fare (dalla lista del fondatore)

- Interfaccia di **Brani** rifatta (decisione a parte).
- «La parte centrale si può riguardare» (colonna delle slide).
