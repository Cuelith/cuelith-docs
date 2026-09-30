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

Si prevedono alcune disposizioni fisse e studiate (es. Presenta, Band, Conferenza, Regia, Compatta), scelte dall'operatore; cambiarle non tocca mai uscite ne' diretta. Bozzetti da approvare prima di costruirle.
