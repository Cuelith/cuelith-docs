# Decisione 0022: la formattazione delle parole è un plugin annesso

- **Data**: 2026-10-10
- **Stato**: **decisa e fatta su `dev`** (il fondatore: «per Brani e qualsiasi altro plugin, presente e futuro, sia un plugin annesso come i futuri accordi; chi non lo vuole non lo usa; manteniamo la modularità»; la scelta su cosa resta nel nucleo l'ha lasciata a Claude); **non ancora pubblicata**

## Cosa si decide

1. **Nel nucleo resta ciò che nessun plugin può fare** (decisione 0021): il campo `spans` nel testo di una slide, il disegno sulle uscite e sull'anteprima, il controllo che il testo non si spezzi. Senza parole formattate non pesa e non si vede: gli show di prima non cambiano.
2. **La barra della formattazione per i testi del nucleo** (tasto «+ Testo», decisione 0021) **resta nel nucleo**: è piccola, non esegue codice di terzi ed è il caso più comune. Per questo i testi semplici si formattano senza installare niente.
3. **Per Brani e per ogni altro plugin la formattazione è un plugin annesso**: `cuelith.richtext` («Formattazione», repo `plugin-richtext`, GPL 3). Chi non lo vuole non lo installa; l'editor del plugin funziona come prima.
4. **Come parlano l'editor e il plugin annesso** (protocollo **1.22**, nessun canale nuovo: usano quello che c'è, cioè lo stato condiviso e i metodi con l'ambito «edit», che hanno già tutti i pannelli dei plugin):
   - l'editor descrive il testo in modifica con `richtext.session` (testo, intervalli, tratto selezionato, carattere) e lo chiude con `richtext.end`; il motore lo tiene in `live.richText` (non entra nello show e non lo segna da salvare);
   - il plugin annesso legge `live.richText` e, quando si preme un pulsante, chiede `richtext.apply` (dimensione, grassetto, corsivo, colore del tratto selezionato);
   - il motore calcola i nuovi intervalli con la stessa funzione dell'editor del nucleo (`styleRange`) e li riporta in `live.richText.spans`, aumentando `applied`; l'editor, quando `applied` cambia, adotta gli intervalli nuovi.
   Il testo lo scrive sempre e solo l'editor.
5. **La barra sta dove si scrive** (protocollo **1.23**, sistemazione del 2026-10-10): un plugin annesso può dichiarare un pannello con `placement: "editor"`, cioè una barra di strumenti che la postazione mostra **in cima alla finestra di ogni editor di plugin**, solo mentre l'editor ha un testo che la usa (`live.richText.owner` = il plugin di quella finestra). Non ha icona, non è una scheda e non si trova nella colonna di sinistra. Così non si passa più da una finestra all'altra: si seleziona nell'editor e si formatta dalla barra sopra. Il plugin Formattazione è questo.
6. **Brani usa l'aggancio** (Brani 0.7.0): ogni sezione ha le sue parole formattate; con gli accordi tra quadre le posizioni dell'editor e quelle della slide proiettata sono diverse, quindi le parole formattate si portano da una all'altra togliendo i caratteri che spariscono (`remapSpans`: spazi, accordi, separatori di slide). Nell'elemento stanno due volte: sulla slide proiettata (campo `text.spans`, quello che disegnano le uscite) e, nelle posizioni dell'editor, nei dati del brano (`meta.spans`), per ritrovarle quando si riapre. «Cosa vede il pubblico» nell'editor le mostra. Un brano senza formattazione produce esattamente lo stesso elemento di prima.
7. **Per chi scrive un plugin**: `@cuelith/panel` ha `bindRichText(panel, campo, onSpans)` (per qualunque editor) e `bindTextarea(...)` (per una casella di testo, con lo spostamento delle parole formattate mentre si scrive). Se il plugin annesso non c'è, non succede niente di visibile. Il corsivo si offre solo se il carattere ne ha uno vero: l'elenco (`TEXT_FONTS_WITH_ITALIC`) sta nel protocollo, con una prova che lo tiene uguale a quello del nucleo.

## Com'è fatto

- **Protocollo 1.22 e 1.23** (`cuelith-sdk`; 1.23: `placement: "editor"` e `remapSpans`): `RichSessionSchema`, `RichChangeSchema`, `live.richText`, metodi `richtext.session`, `richtext.end`, `richtext.apply`, `TEXT_FONTS_WITH_ITALIC`; `@cuelith/panel` 0.9.0 (`bindRichText`, `bindTextarea`).
- **Motore**: tre gestori in `rpc/handlers/cue.ts`; errori `core.error.richTextNoSession` e `core.error.richTextNoSelection`.
- **Nucleo**: `PanelWindow` mostra le barre `editor` in cima alla finestra dell'editor (`data-editor-strips`), accese solo mentre c'è un testo in modifica di quel plugin.
- **Plugin** `plugin-richtext` 0.2.0 (cartella affiancata, **senza repository su GitHub ancora**): barra con dimensione, G, C, colore, «Togli formattazione», accesa/spenta secondo la selezione; avvisa se il carattere non ha il corsivo.
- **Brani 0.7.0** (`plugin-songs`): `model/rich.ts`, `RichTextarea`, `meta.spans`, anteprima dell'editor con le parole formattate.
- **Prove**: Brani (`rich.test.ts`: slide sempre uguali, parole al posto giusto senza accordi, andata e ritorno, slide vuote), e2e con Brani vero e Formattazione vero (`richtext-plugin.spec.ts`), motore (`richtext.test.ts`: sessione, applicazione, conto delle modifiche, selezione fuori testo, chi chiude, show non sporcato), pannello (`richtext.test.ts`), plugin (`logic.test.ts`), e2e (`richtext-plugin.spec.ts`: un plugin di prova con una casella in una finestra propria e il plugin Formattazione vero; il tratto selezionato diventa grassetto, poi 150%, poi senza formattazione, e l'editor lo adotta).

## Da sapere

- **Esportazione dei brani**: ChordPro e OpenLyrics escono come testo semplice (senza formattazione), come prima; le parole formattate restano nei dati di Cuelith (elemento e copie di sicurezza). Scriverle anche in OpenLyrics (che ha dei tag per la formattazione) è un passo a parte.
- **Il plugin annesso non ha ancora un repository** e quindi nemmeno un pacchetto pubblicato né una voce nel registro. La CI e la release sono già copiate da Brani.
- **Una barra per più plugin annessi**: se se ne installano due con `placement: "editor"`, le barre si impilano sopra l'editor (ognuna alta quanto serve al suo lavoro, oggi 72 px).
- Un plugin annesso nuovo (accordi, ecc.) può usare lo stesso schema: la conversazione tra editor e plugin passa dallo stato, e basta definire un nome e una forma.
