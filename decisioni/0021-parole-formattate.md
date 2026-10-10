# Decisione 0021: parole formattate dentro un testo

- **Data**: 2026-10-10
- **Stato**: **decisa e fatta su `dev`** (il fondatore ha lasciato la scelta a Claude, con un solo vincolo: deve restare possibile esportare sia come testo semplice sia come testo personalizzato); **non ancora pubblicata**

## Cosa si decide

1. **Un solo dato, non due file.** Il testo di una slide resta una stringa; accanto, un elenco facoltativo di intervalli (`spans`) dice lo stile di ogni pezzo: dimensione (multiplo di quella dello stile, da 0,5 a 3), grassetto, corsivo, colore. Due file per ogni testo si sarebbero disallineati alla prima modifica fatta con un altro strumento; un campo solo, con le posizioni accanto al testo, si sposta insieme a lui.
2. **Esportazione, per costruzione.**
   - **Testo semplice**: è `value`, e basta. Chi non conosce gli intervalli (vecchie versioni, importatori, plugin che leggono il testo) non si accorge di niente.
   - **Testo personalizzato**: è `value` più `spans`, tali e quali, e `segmentsOf(testo, spans)` (nel protocollo) li spezza in pezzi con il loro stile: da lì qualunque formato (HTML, ChordPro con direttive, OpenLyrics, documento) si scrive senza altro lavoro. Le funzioni `plainText` e `segmentsOf` sono la parte pubblica pensata per questo.
3. **Cosa può cambiare una parola**: dimensione, grassetto, corsivo, colore. Non il carattere né il contorno: ogni cosa in più sarebbe un'altra cosa da far disegnare uguale su uscite, anteprima e controllo dello spazio. Il corsivo c'è solo se il carattere ne ha uno vero (come per gli stili del testo, decisione 0020).
4. **Lo stile scelto a destra non cancella le parole formattate.** Le dimensioni sono relative a quella dello stile e il colore/grassetto/corsivo restano; cambiare stile cambia il resto. (La regola «lo stile globale vince sull'editor del singolo testo» è per le modifiche del testo intero, decisione 0015, e non cambia.)
5. **Il testo non si spezza mai**: la disposizione (`layoutRich`, codice puro in `core-looks`) è la stessa per il controllo dello spazio e per il disegno delle uscite; a capo agli spazi, riga alta quanto il pezzo più grande, parola fatta di pezzi con stili diversi che resta una parola, parola più lunga della riga spezzata a caratteri. Con «Adatta se non entra» le righe restano intere e la parola grande conta nella larghezza.
6. **Editor** (quello dei testi del nucleo): barra con dimensione, **G**rassetto, **C**orsivo, colore e «Togli formattazione», che agisce sul tratto selezionato (anche Ctrl+B e Ctrl+I); sotto, «Come lo vede il pubblico». La casella resta testo semplice; scrivendo, le parole formattate si spostano con il testo (il testo scritto in mezzo a un tratto formattato prende quella formattazione, in fondo a una parola la continua, dopo uno spazio la chiude).

## Com'è fatto

- **Protocollo 1.21** (`cuelith-sdk`): `rich.ts` (`SpanSchema`, `cleanSpans`, `segmentsOf`, `styleRange`, `shiftSpans`, `sliceRich`, `joinRich`, `plainText`), campo `spans` solo nel campo `text` (massimo 300 intervalli, dentro il testo; messaggio `protocol.field.spansOutside`).
- **Nucleo**: `core-looks/src/rich.ts` (disposizione), `text.ts` (`textFits` e il resto accettano testo semplice o con parole formattate), `renderer/frame.ts` (`spans`, `fitTexts` con le parole formattate) e `painter.ts` (le uscite disegnano il testo formattato riga per riga su una tela, una sola immagine per slide, con contorno e ombra), `client/ui/SlideText.tsx` (stessa resa in CSS), `client/shell/FormatBar.tsx` e `ItemEditorDialog.tsx` (barra, spostamento delle parole, salvataggio), `station/show.ts` (`splitSlidesRich`, `joinSlidesRich`).
- **Prove**: `rich.test.ts` (protocollo), `text.test.ts` (disposizione e controllo dello spazio), `richslides.test.ts` (le slide sono sempre le stesse), `frame.test.ts`, `richtext.spec.ts` (dall'editor all'uscita vera, con immagine).

## Da sapere

- **Il palco non mostra le parole formattate** (solo il testo): il monitor del relatore resta semplice.
- **Brani** (il plugin) non ha ancora la formattazione nel suo editor: le slide che produce possono già portare `spans` (il protocollo lo permette a qualunque plugin), ma il suo modello di brano e l'importazione/esportazione ChordPro e OpenLyrics vanno estesi a parte, nel plugin. Per ora vale per i testi e per ciò che si scrive nell'editor del nucleo.
- Uno show con parole formattate, aperto in una versione precedente (0.5.x o prima), può dare errore (campo sconosciuto).
- Non ci sono ancora: sottolineato e un carattere (famiglia) diverso per una parola.
