# Decisione 0020: editor degli stili completo, con caratteri veri

- **Data**: 2026-10-10
- **Stato**: **decisa e fatta su `dev`** (richiesta del fondatore dopo la 0.4.0: «gli stili mi risultano incompleti, pochi font e senza caratterizzazione, l'esempio "Aa" non rende»); da pubblicare con la 0.5.0

## Cosa si decide

1. **Ventisette caratteri inclusi nel programma** (pacchetto `show-fonts`, file locali: funzionano senza internet), in cinque famiglie con una frase che dice a cosa servono (con grazie, senza grazie, titoli, a mano, spaziatura fissa). Ognuno si presenta scritto col suo stesso carattere. Nessun limite di peso dell'installatore («l'importante è che funzioni e rimanga fluido»): i file si caricano solo quando un carattere serve davvero (uscite e anteprima chiedono quello della slide, l'elenco dei caratteri carica solo la famiglia aperta).
2. **Spessore a sei gradini e corsivo, ma solo veri**: l'elenco (`core-looks/src/fonts.ts`) dice per ogni carattere quali spessori ha e se ha un corsivo vero; l'editor offre solo quelli. Niente grassetto o corsivo inventati dal browser, perché sulle uscite (canvas) e nell'anteprima (CSS) verrebbero diversi. Un carattere con un solo spessore o senza corsivo lo dice.
3. **Spaziatura tra le lettere e posizione** (in alto, al centro, in basso; in basso sta sopra la fascia dei crediti). Non ci sono, per scelta, sottolineato e spaziatura tra le parole: il motore di disegno delle uscite non li disegna come l'anteprima, e la regola è che uscite e anteprima dicono la stessa cosa.
4. **Il controllo dello spazio misura con le stesse cose che si disegnano**: la misura riceve carattere, spessore, corsivo e spaziatura (`MeasureText` ora prende lo stile del carattere). Le righe continuano a non spezzarsi mai.
5. **L'esempio nell'editor** è il testo della slide **in anteprima**; senza una slide è un testo di prova **nella lingua in uso** (`core.textstyles.sample`: quattro righe di lunghezze molto diverse, ispirate alle immagini del sito). L'«Aa» scompare. La lingua in uso è sempre una sola (il programma lo garantisce già: la scheda Lingue ha un solo «In uso» e non si toglie l'ultima lingua), quindi il testo di prova segue quella.
6. **Editor più largo**, con l'anteprima sempre in vista accanto ai controlli, e controlli raggruppati (Carattere, Aspetto, Misure e posizione, Contorno/ombra/adattamento). I nomi «Titolo / Testo / Spaziato» che confondevano non ci sono più: si vede il nome vero del carattere e la famiglia dice a cosa serve.
7. **Lo stile del singolo testo** (scheda **Stile** dell'editor di un testo o brano) usa lo stesso elenco di caratteri e in più spessore e corsivo. **Regola che non cambia** (decisione 0015): uno stile globale scelto a destra vince su quello del singolo testo, che resta salvato e torna quando lo si toglie.

## Com'è fatto

- **Protocollo 1.20** (`cuelith-sdk`, ancora da pubblicare su npm): `TEXT_FONT_IDS`, `TextFontSchema`, `TEXT_WEIGHTS`, `TextWeightSchema`; le modifiche del singolo testo (`TextOverride`) ricevono `font` esteso, `weight` a sei valori, `italic`.
- **Nucleo** (`cuelith-core`): `core-looks/src/fonts.ts` (elenco, spessori veri, `cssFont`), `styles.ts` (`italic`, `letterSpacing`, `vAlign`, `weight` esteso), `text.ts` (misura), pacchetto `packages/show-fonts` (costruisce `fonts.css` e copia i file; controlla che ogni carattere dell'elenco sia dichiarato), `renderer/painter.ts` (carica il carattere prima di disegnare e di misurare, `letterSpacing`, `fontStyle`, posizione), `client/ui/SlideText.tsx` (stessa resa in CSS, `font-synthesis: none`), `client/panels/StyleEditor.tsx` (nuovo), `TextStylesPanel.tsx`, `shell/ItemEditorDialog.tsx`, `station/textStyles.ts` (carica i caratteri quando servono e rifà le misure appena arrivano: `useFontsVersion`).
- **Lingue**: 33 chiavi nuove in italiano e in inglese (`core.textstyles.*`).
- **Prove**: `text.test.ts` (elenco, spessori veri, corsivo solo se vero, spaziatura nella misura, compatibilità con gli stili vecchi), `textstyles.spec.ts` (famiglie e caratteri, un solo spessore per Bebas Neue, Lora con corsivo e spaziatura che arrivano all'uscita e il carattere davvero caricato, esempio dalla slide e testo di prova).

## Da sapere

- Uno show che usa i caratteri o gli spessori nuovi, aperto in una versione 0.4.x, può dare errore (come era già per gli stili del testo della 0.4).
- **L'editor «stile Word» per lettera** della colonna di sinistra (formattazione di singole parole o lettere nei testi, nei brani e nei plugin futuri) **non c'è in questa decisione**: per ora il singolo testo ha lo stile del testo intero. Va progettato a parte, perché cambia il modo in cui le slide conservano il testo (oggi è testo semplice) e deve restare d'accordo con le uscite e con lo stile globale.
