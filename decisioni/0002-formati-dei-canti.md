# 0002 · Formati universali dei canti

Stato: **Deciso** (fondatore, 2026-09-30). Completa la decisione 0001 (modulo Canti).

## Richiesta del fondatore

I testi dei canti devono essere scritti in un **formato universale**: Cuelith legge il formato, lo sistema nel proprio formato di lettura e fa anche il contrario. Titolo, testo e artista/i sono **obbligatori**, tutto il resto è facoltativo.

## Decisioni

- **Formato di scambio principale: [OpenLyrics](https://docs.openlyrics.org/)** (XML aperto, usato da OpenLP e altri). È l'unico che contiene senza perdite tutto il nostro modello: sezioni con nome (`v1`, `c1`, `b1`, `p1`, `i`, `e`…), ordine di proiezione (`verseOrder`), autori con ruolo, copyright, CCLI, raccolte e numeri (`songbooks`), temi (= tag), accordi.
- **Secondo formato: [ChordPro](https://www.chordpro.org/)** (testo con accordi, diffuso tra i musicisti): lettura e scrittura. Direttive `{title}`, `{artist}`, `{start_of_verse}`/`{start_of_chorus}`/`{start_of_bridge}`…, accordi in linea `[Sol]`.
- **Testo semplice**: sola lettura (incolla; righe vuote = slide, eventuali etichette "Strofa 1", "Ritornello", "Verse", "Chorus" riconosciute come sezioni).
- **Formato interno** = elemento del modello dati (cap. 22): una slide per sezione (o più slide per sezione lunga) con `group`, `arrangement` per l'ordine, campo `text` senza accordi e campo `chords` in ChordPro, crediti e tag (decisione 0001). L'importazione converte nel formato interno, l'esportazione fa il contrario.
- **Campi obbligatori**: titolo, testo (almeno una sezione non vuota), almeno un autore/artista. Un canto senza questi campi non si salva; un'importazione che li manca segnala cosa completare invece di fallire in silenzio. Tutto il resto è facoltativo.
- **Dove sta il codice**: i convertitori sono funzioni pure in un pacchetto a sé del modulo Canti (riusabile da altri moduli e strumenti), provate con test di **andata e ritorno** (importa → esporta → reimporta = stesso canto) e con file reali esportati da OpenLP.
- Gli accordi (decisione 0001, modulo Accordi) viaggiano negli stessi formati: OpenLyrics `<chord>` e ChordPro in linea.
