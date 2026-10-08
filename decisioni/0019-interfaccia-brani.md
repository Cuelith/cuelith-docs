# Decisione 0019: interfaccia di Brani (0.6.0)

- **Data**: 2026-10-08
- **Stato**: **decisa e fatta su `dev`** (il fondatore ha chiesto un'interfaccia «molto migliorata», non più «grossolana», e ha lasciato la scelta a Claude); da provare nell'app del fondatore

## Cosa cambia

**Pannello «Brani»** (la scheda a sinistra)
- Ricerca e **+ Nuovo brano** sulla stessa riga; sotto, il numero dei brani, il filtro per libreria (solo se ci sono librerie), e i due comandi poco usati, **Importa…** ed **Esporta tutti…**, come collegamenti discreti invece di tre pulsanti grandi.
- Le azioni sulla selezione (**Anteprima, In onda, In scaletta, Editor**) stanno in una barra **fissa in fondo al pannello**, sempre nello stesso posto.
- Elenco più compatto, il numero del brano come etichetta.

**Editor del brano** (la finestra a parte)
- **Titolo grande** e autori subito sotto; **titoli alternativi, crediti e altri dati** nel fianco, in sezioni che si aprono solo se servono.
- **Sezioni** come schede con un colore per tipo (strofa, ritornello, ponte…), numero delle slide, **salto rapido** (V1 C1 V2…) sempre in vista mentre si scorre.
- **«Cosa vede il pubblico»**: nel fianco, le slide nell'ordine di proiezione, senza accordi, aggiornate mentre si scrive. Si capisce subito dove cade ogni `[---]` e come suona l'ordine «V1 C1 V2 C1».
- Barra in alto fissa (Salva, Salva e metti in scaletta, Chiudi, Esporta).

**Colonna centrale (le slide)**, per qualsiasi elemento
- **Dimensione delle miniature** S / M / L (si ricorda): piccole per vedere tutto un brano, grandi per leggere il testo.
- **Stato a parole**: la slide in onda dice «in onda» e la prossima «prossima», non solo il bordo rosso o ciano.
- **Numero delle slide** sotto il titolo; l'etichetta della sezione (V, C, B…) ha un colore per tipo; la slide in onda o in anteprima **si porta da sola in vista** nei brani lunghi.

## Cosa non cambia

Il formato dei brani, l'importazione e l'esportazione (OpenLyrics e ChordPro), i tasti delle sezioni, i nomi accessibili dei campi e dei pulsanti (le prove sono le stesse, più quelle nuove per anteprima, salto rapido e conteggio).

## Prove

`songs.spec.ts` (anteprima delle slide, salto rapido, conteggio, sezioni chiuse), le altre prove di Brani invariate, `pnpm check` del plugin.

## Da fare ancora (non chiesto in questa decisione)

Trascinare le sezioni per riordinarle; scelta di una slide dall'editor che la porta in anteprima. **Gli accordi** (anteprima con gli accordi, trasposizione, schede per i musicisti) non si toccano: il plugin Accordi, collegato a Brani, **non è ancora stato progettato** (decisione del fondatore, 2026-10-08).
