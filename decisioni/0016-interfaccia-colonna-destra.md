# Decisione 0016: colonna di destra della regia

- **Data**: 2026-10-07
- **Stato**: **decisa dal fondatore e fatta su `dev`** (2026-10-07); da provare nell'app del fondatore

## Il problema

La colonna di destra (programma, anteprima, sfondi, stili del testo) ha scorrimenti verticali annidati (gli sfondi scorrono dentro la colonna, che scorre a sua volta), l'etichetta «ANTEPRIMA» copre la prima riga del testo e manca un modo per mostrare solo lo sfondo.

## Cosa si decide (proposta)

1. **Niente scorrimento verticale** nella colonna. Ogni parte ha un'altezza che si ricava da quella disponibile; il programma e l'anteprima si rimpiccioliscono, non scorrono.
2. **Solo scorrimento orizzontale**, dove serve: la riga degli sfondi e la riga degli stili sono file di miniature che scorrono di lato, con frecce e sfumatura ai bordi (come le schede della colonna sinistra della 0.3.3).
3. **Ordine dall'alto**: Programma → comandi di regia (Indietro, Avanti, Pulisci) → Anteprima con «Manda in onda» → Sfondi → Stili del testo.
4. **Etichette fuori dal testo**: «PROGRAMMA» e «ANTEPRIMA» vanno in una riga sopra il riquadro, non sopra l'immagine.
5. **Pulsante «Solo sfondo»**: mostra in onda lo sfondo senza testo (e lo toglie con un secondo clic). Non cambia la slide corrente: il testo torna subito. Vale per le uscite dell'aspetto Sala; è un'azione di regia come «Pulisci».
6. **«Dove mettere lo sfondo»** (Slide N / Tutto l'elemento / Predefinito) diventa un controllo a segmenti compatto accanto al titolo «Sfondi», con il «Velo».
7. Telefono in verticale: stessa regola (niente scorrimento verticale nella colonna; le due righe scorrono di lato).

## Cosa non cambia

Il contenuto e il comportamento di sfondi e stili (decisioni 0003 e 0015); solo la disposizione.

## Risposte del fondatore

- «Solo sfondo» è un interruttore che nasconde il contenuto proiettato lasciando lo sfondo, e lavora sullo stato in onda, quindi vale per ogni slide finché non lo si toglie. Il Programma lo specchia; l'Anteprima no (serve a vedere cosa viene dopo); il Palco tiene il testo.
- Le proporzioni tra programma e anteprima le sceglie chi implementa; l'anteprima deve essere la più grande possibile (fatto: 2fr contro 3fr).

## Com'è fatto

- Protocollo 1.17: campo facoltativo `live.textHidden` e metodo `live.textHidden` (ambito regia).
- Presenta: righe `minmax(0px, 2fr)`, `minmax(0px, 3fr)`, `auto`, `auto`; `Screen` con `fill`; `HScroll` per le righe che scorrono di lato (la rotella le sposta).
- **Scambio delle dimensioni** (richiesta del fondatore, 2026-10-08): un pulsante con due frecce circolari, a destra dei comandi del programma, fa diventare grande il Programma e piccola l'Anteprima (e viceversa). Si ricorda su questa postazione (`station/screenSwap.ts`); vale solo in «Presenta».
- Prova: `e2e/tests/column.spec.ts`.
