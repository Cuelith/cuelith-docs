# 0006 · Zona centrale, selezione e azioni fisse, moduli attivi e passivi, icone, impostazioni

Stato: **Deciso** (fondatore, 2026-10-01).

## La zona centrale e' dell'operatore

- Nessun modulo prende la zona centrale da solo. Gli editor dei moduli (pannelli `center` nel manifest) si aprono **su richiesta in una finestra propria**, spostabile su un altro schermo; programma e anteprima restano sempre visibili. Da postazione browser: nuova scheda.
- La zona centrale cambia solo per scelta dell'operatore (disposizioni, vedi sotto).

## Elenchi: selezione e barra fissa

- Negli elenchi (Canti, Librerie): **clic = seleziona**, Ctrl+clic e Maiusc+clic = piu' elementi, **doppio clic = in anteprima** (sicuro, non va in onda). Un clic non apre mai un editor.
- Sotto il filtro delle librerie, una **barra fissa**: **Anteprima · In onda · In scaletta · Editor**. Con piu' elementi selezionati vale solo «In scaletta» (nell'ordine di selezione).

## Moduli attivi e passivi, icone

- **Attivo** = ha uno strumento (pannello `side`): la sua icona sta nella colonna di sinistra; un clic apre la sua scheda e basta.
- **Passivo** = lingue, servizi: lavora in background, si vede e si configura in Moduli → Installati. Il tipo si ricava dal manifest (`isActivePlugin`), non da un campo scritto a mano.
- Ogni modulo ha un'**icona SVG** propria (`icon` nel manifest, protocollo 1.6). Il registry la richiede, controlla che sia un SVG semplice (niente script o risorse esterne), identica a quella del pacchetto e **diversa da tutte le altre**; l'indice la incorpora per il marketplace.

## Impostazioni e niente scritte di spiegazione

- Ingranaggio in alto e **Ctrl+,**: Generale, **Scorciatoie** (legenda di tutti i tasti), Uscite, Moduli, Informazioni e licenza. Solo voci che funzionano davvero.
- Le scritte che spiegano cosa fare spariscono dall'interfaccia: la legenda sta in Scorciatoie; le spiegazioni brevi restano come suggerimento al passaggio del mouse.

## Disposizioni fisse per tipo di servizio

Cinque disposizioni fisse, scelte dall'operatore dalla barra in alto o con **Ctrl+1…5**. Cambiarle non tocca mai uscite ne' diretta; in ognuna il **programma resta sempre visibile** (mai in una scheda) e c'e' sempre la scaletta, dove si aggiungono le schede dei moduli.

Ricerca (2026-10-01, documentazione pubblica dei programmi del settore — presentazione per chiese ed eventi, mixer video, timer da palco):

- il flusso universale va da sinistra a destra: libreria/scaletta → slide → anteprima/programma (e' Presenta);
- chi segue i canti dal vivo deve anticipare e saltare tra sezioni ripetute o improvvisate: tasti per sezione e pulsanti grandi, anche da tablet;
- nei mixer video l'anteprima sta a sinistra, il programma a destra, i comandi di passaggio in mezzo, le sorgenti sotto;
- i timer da palco usano colori fissi (verde; ambra sotto i 2 minuti; rosso sotto i 30 secondi e oltre il tempo) e messaggi al relatore che il pubblico non vede;
- i volontari alle prime armi lavorano meglio con un'interfaccia ridotta, adatta anche agli schermi piccoli.

Le disposizioni:

1. **Presenta** — Scaletta | Slide | Programma sopra l'Anteprima.
2. **Band** — Scaletta | **Sezioni** come grossi pulsanti (tocco = inizio della sezione in onda; rosso in onda, ciano la prossima; Pulisci) con la **striscia dell'ordine** sotto | Programma, Anteprima, **Palco** (messaggio ai musicisti).
3. **Conferenza** — Scaletta | Programma largo, sotto Anteprima e **Note** del relatore | **Timer** (durate pronte, avvia/pausa/azzera, colori, oltre il tempo in negativo; anche sul monitor del palco) e **Palco** (messaggio al relatore).
4. **Regia** — Scaletta stretta | Anteprima e Programma **grandi uguali** con i **comandi** in mezzo (MANDA IN ONDA, avanti, indietro, pulisci, nero su tutte le uscite) | Slide sotto. Sottopancia, media e scene arriveranno quando esisteranno le funzioni (niente schede finte).
5. **Compatta** — Scaletta, Slide e Librerie a schede (scegliendo una voce si passa alle sue slide) | Programma e Anteprima.

Protocollo 1.7 per Conferenza e Band: timer della regia condiviso (`live.timer`, `timer.set/start/pause/reset/clear`), messaggio per singola uscita (`message.send` → `live.outputs[id].message`), mostrati dal look Palco.
