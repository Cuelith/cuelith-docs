# 0005 · Nomi delle sezioni, tasti delle sezioni, elementi fuori scaletta

Stato: **Deciso** (fondatore, 2026-09-30).

## Nomi delle sezioni: fissi, in ogni lingua

Le sezioni di un canto si chiamano sempre **Verse, Chorus, Pre-Chorus, Bridge, Intro, Ending, Others**, in qualunque lingua dell'interfaccia. Non passano dai cataloghi delle traduzioni. Le lettere restano V C P B I E O (anche nei nomi brevi: V1, C1, P1…). L'importazione riconosce comunque le etichette in italiano e in inglese ("Strofa 2", "Rit.", "[Chorus]"…).

## Tasti delle sezioni in diretta

- Si ragiona **per sezioni nell'ordine di proiezione**, non per slide: una sezione di piu' slide si salta tutta.
- Ogni lettera porta all'**inizio della prossima sezione di quel tipo**; dopo l'ultima si riparte dalla prima (**in cerchio**: il tasto non si blocca mai).
- Se nulla e' in onda si parte dall'elemento in anteprima, dall'inizio (anche il tasto premuto subito dopo Invio non va perso).
- Niente "lettera + numero": la lettera salterebbe subito e il numero dopo, con un lampo della sezione sbagliata in onda. Per una sezione precisa si clicca la sua slide.
- I tasti della regia premuti in un pannello di modulo (fuori dai campi di testo) arrivano alla postazione (`host.key`).

## Elementi fuori scaletta (protocollo 1.5)

Un elemento della libreria (canto, testo; in futuro immagini e video) si puo' mandare **in anteprima o subito in onda senza metterlo in scaletta** (`cue.send`). La copia vive solo in memoria (`live.direct`), non modifica lo show e non viene salvata; il motore la toglie quando non e' piu' ne' in programma ne' in anteprima. Programma e anteprima possono quindi puntare a una voce della scaletta (`entryId`) oppure a un elemento fuori scaletta (`itemId`). Dalla colonna Slide lo si puo' poi mettere in scaletta.

## Riferimenti ad altri programmi

Nel prodotto non si nominano programmi concorrenti; i formati aperti (OpenLyrics, ChordPro) si.
