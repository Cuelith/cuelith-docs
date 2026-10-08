# Decisione 0017: trovare, aprire e configurare i plugin (colonna di sinistra)

- **Data**: 2026-10-07
- **Stato**: **decisa e fatta su `dev`** (2026-10-07; il fondatore ha lasciato la scelta a Claude, «pensa di essere l'utente»): punti 1–4 fatti (ricerca, preferiti, schede, impostazioni uniformi con protocollo 1.18); finestra «Plugin» in tre schede e avvio guidato (punti 5–6) da fare con la 0018

## Il problema

Oggi ogni plugin attivo con uno strumento ha un'icona nella colonna stretta a sinistra e una scheda; le icone scorrono e le schede scorrono su una riga (0.3.3). Con decine di plugin questo non basta: bisogna sapere quale icona è quale, non c'è una ricerca, e la configurazione di un plugin sta in un'altra finestra («Moduli») che non si capisce dove trovare. L'obiettivo del fondatore: **trovare un plugin e metterci mano in pochi gesti, senza ritardi**.

## Cosa si propone

1. **Cerca e vai (Ctrl+K).** Una finestrella con un campo di ricerca che trova, mentre si scrive, plugin, loro strumenti e impostazioni, brani/elementi della scaletta e i comandi principali (Avanti, Nero, Solo sfondo…). Frecce e Invio; Esc chiude. È il modo più veloce con tanti plugin, e funziona con la sola tastiera. Un pulsante con la lente, nella colonna delle icone, la apre anche con il mouse.
2. **Preferiti nella colonna delle icone.** L'utente fissa (☆) i plugin che usa di più: solo quelli stanno nella colonna. Gli altri si raggiungono da un pulsante «Tutti i plugin» (griglia con ricerca, ordinata per ultimo uso) e dalla ricerca. Alla prima installazione di un plugin si propone di fissarlo. Meno icone, sempre quelle giuste.
3. **Schede: restano come sono** (una riga che scorre, elenco ▾). In più, il tasto Ctrl+Tab passa alla scheda usata prima.
4. **Impostazioni dello stesso posto per tutti.** Ogni plugin ha un ingranaggio ⚙ nella sua scheda (e nella griglia «Tutti i plugin»). Si apre un pannello che il nucleo disegna da una descrizione delle impostazioni scritta dal plugin (campi di testo, numeri, scelte, interruttori, colori, file), con le stesse regole di aspetto e di lingua per tutti. Un plugin con esigenze particolari può ancora offrire un proprio pannello, ma il percorso per arrivarci è uguale. **Da verificare** nel protocollo se il manifest ha già questa descrizione; se no, serve un campo nuovo (protocollo 1.18) e va scritto nella guida degli autori.
5. **Gestione (installa, aggiorna, disattiva, disinstalla)** nella finestra «Plugin»: tre schede, *Installati*, *Marketplace*, *Lingue* (le lingue separate dai plugin, come chiesto). Il ridisegno della finestra e del marketplace si decide a parte (0018).
6. **Primo avvio guidato.** Poiché Brani non sarà più nell'installatore, la prima volta (e finché non c'è nessuno strumento) la colonna mostra un avvio guidato: «Scegli cosa ti serve» con i plugin del marketplace in evidenza (Brani per primo), installazione in un clic e apertura dello strumento. Si decide nel dettaglio in 0018.

## Cosa non cambia

I plugin continuano a girare in processi separati, con gli stessi permessi (decisione 0007); il manifest e il modo di dichiarare strumenti e pannelli non cambiano, salvo l'eventuale descrizione delle impostazioni (punto 4), che è un campo facoltativo.

## Ordine di lavoro proposto

1. Ricerca Ctrl+K (la parte che dà più vantaggio, nessuna modifica al protocollo).
2. Preferiti e «Tutti i plugin».
3. Ingranaggio e pannello di impostazioni uniforme (con l'eventuale protocollo 1.18).
4. Finestra «Plugin» con le tre schede e l'avvio guidato (0018).

## Com'è fatto (2026-10-07)

- **Ricerca**: `Palette.tsx` (Ctrl+K, di nuovo Ctrl+K la chiude; frecce e Invio; filtri Tutto/Plugin/Comandi/Scaletta); logica pura in `station/palette.ts` (ogni parola scritta deve trovarsi, senza accenti né maiuscole; l'inizio del titolo vince). Trova plugin, comandi della regia, disposizioni, impostazioni (anche «Impostazioni di <plugin>») ed elementi della scaletta (che vanno in anteprima, mai in onda).
- **Preferiti**: `station/pins.ts`, nel browser della postazione. Finché non si sceglie vale «tutti»; alla prima scelta l'elenco diventa esplicito e i plugin nuovi si fissano da soli. Le **schede** in alto mostrano i fissati più quelli aperti da poco dalla ricerca.
- **Impostazioni**: scelta presa dal fondatore-utente: **il nucleo disegna la finestra** (`PluginSettingsDialog`), con salvataggio automatico e «Ripristina»; il plugin riceve i valori all'avvio e l'evento `core.plugin.settingsChanged` quando cambiano. Nessun pannello proprio per le impostazioni: meno cose da costruire per chi sviluppa e aspetto uguale per tutti. Valori in `plugin-settings.json` (cartella dati), controllati contro il manifest (tipo, limiti, scelte).
- **Protocollo 1.18**: `contributes.settings` con `description`, `min`, `max`, `choices`; metodi `pluginsettings.get` (lettura) e `pluginsettings.set` (regia); evento `core.plugin.settingsChanged`. Guida per chi sviluppa: `DEVELOPERS(.it).md`, sezione 10.
- **Prove**: `palette.test.ts`, `plugin-settings.test.ts`, `manifest.test.ts`, e2e `palette.spec.ts` e `pluginsettings.spec.ts`.

## Cosa è stato aggiunto dopo (2026-10-08)

- **Ctrl+Tab** torna alla scheda usata prima; **Ctrl+Maiusc+Tab** passa alla successiva. Nelle Impostazioni → Scorciatoie ci sono anche Ctrl+K e queste.
- **Le schede aperte dalla ricerca si chiudono** (×): il plugin non è fissato, quindi la scheda sparisce e si torna alla precedente.

## Prova di carico (2026-10-08, `e2e/tests/load.spec.ts`, solo Windows)

12 plugin veri (copie del modello, ciascuno con il suo processo e il suo pannello) attivi insieme mentre un'uscita proietta:

| Cosa | Risultato |
| --- | --- |
| Memoria privata di un plugin | **circa 27 MB** (12 insieme: ~320 MB) |
| Memoria privata di tutto Cuelith | 499 MB senza plugin → **791 MB** con 12 plugin (18 processi) |
| CPU dei 12 plugin fermi | **0%** |
| Cambio scheda (pannello caricato) | mediana **~150 ms**, al massimo ~300 ms |
| Aprire la ricerca / trovare un plugin | **~85 ms / ~100 ms** |
| Pausa massima tra due fotogrammi dell'uscita | cambiando scheda e a riposo **17–23 ms**; **installando** i plugin fino a 150 ms |

Letture: il «peso di lavoro» che Windows mostra per ogni processo (~107 MB) include pagine condivise tra processi e non è quello che si somma; quella che conta è la memoria privata. La CPU alta nei test è l'uscita che disegna (scheda grafica e finestra di uscita), non i plugin. **Un limite vero**: installare un plugin mentre si proietta può dare una pausa fino a 150 ms nell'uscita; rientra nella soglia che il progetto usa per «le uscite non cadono», ma meglio non installare nulla in diretta.

## Protezioni: «non deve mai cadere ed essere reattivo» (2026-10-08)

- **Priorità più bassa per i plugin Node**: la postazione e le uscite hanno la priorità normale e passano per prime se un plugin si mette a consumare (`SpawnSpec.lowPriority`, prova con processi veri).
- **Freno della memoria** (`MEMORY_GUARD`, `resources.ts`): se la memoria libera del computer scende sotto il maggiore tra 400 MB e il 4% del totale, si ferma il plugin più pesante (almeno 150 MB riportati). Resta fermo, senza riavvio automatico, con l'errore «Fermato: la memoria del computer stava per finire»; lo riaccendi dalla finestra Plugin. Limite: conta il consumo che il plugin riporta (SDK); uno che non lo riporta non si può scegliere.
- **Avvisi** (`ResourceWatch`): quando il computer passa a «vicino al limite» o «al limite», e quando il freno ferma un plugin.
- **Già c'erano**: ogni plugin nel suo processo con permessi ristretti e tetto di memoria (1 GB), controllo periodico (un plugin che non risponde viene terminato e riavviato fino a 3 volte in 60 s), uscite indipendenti dai plugin.
- **Difetto trovato dalla prova di resistenza e corretto**: reinstallare **la stessa versione** di un plugin acceso (da file o cartella) dava «errore interno» su Windows (cartella bloccata dal processo). Ora il plugin si ferma, si sostituisce e riparte.

**Prova di resistenza** (`e2e/tests/stress.spec.ts`, Windows): 16 plugin che consumano tutto il processore (uno per core) e 100 MB ciascuno, 1 plugin che si blocca del tutto, 6 plugin normali con pannello; 29 processi.

| Cosa | Risultato |
| --- | --- |
| Cambio scheda con tutti i core occupati | mediana ~140 ms, al massimo ~330 ms |
| Aprire la ricerca | ~45 ms (al massimo ~150 ms) |
| Da «Avanti» all'uscita | ~65 ms |
| Pausa massima tra due fotogrammi dell'uscita | **~20 ms** sotto carico e dopo il blocco |
| Plugin bloccato | fermato dal controllo periodico, il resto non se ne accorge |
| Installare con tutto acceso | pausa dell'uscita fino a ~150 ms (limite noto) |

Non ho confrontato con e senza priorità ridotta: so che con la priorità ridotta regge, non quanto cambi senza.

## Domande per il fondatore (risolte: scelte di Claude, rivedibili)

Colonna delle icone con i preferiti; finestra di impostazioni disegnata dal nucleo; la ricerca trova anche la scaletta.
