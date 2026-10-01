# 0007 · Processi dei moduli, permessi, moduli di terzi

Stato: **Deciso** per la parte costruita (passi 9b e 10 della Fase 0, 2026-10-01); **Proposta** per i contratti futuri (flussi video e audio, sfondi), da confermare prima della Fase 1.

## Cosa c'è (protocollo 1.8.0, SDK `@cuelith/sdk` 0.1.0)

- **Un processo per modulo con codice** (cap. 21). Il motore avvia ogni modulo attivo con `runtime: node` o `native` nel suo processo e gli parla con JSON-RPC su stdio, una riga JSON per messaggio. Il modulo usa gli stessi metodi delle postazioni, con il ruolo `plugin:<id>`: regia e librerie sì, uscite, configurazione e amministrazione no. Eventi, storage e comandi tra moduli sono metodi in più.
- **Ciclo di vita** (cap. 24):
  - stati `activating → active → deactivating`;
  - `plugin.activate` riceve il contesto: impostazioni, permessi, cartella privata;
  - il modulo risponde entro **5 s**, e il motore lo controlla ogni 10 s con `plugin.ping`;
  - un modulo bloccato viene terminato;
  - un crash porta a un riavvio automatico, **al massimo 3 in 60 s**; poi `crashed` con errore leggibile, finché l'utente non lo spegne e lo riaccende.
- **Le uscite non cadono** (cap. 27): nessun codice dei moduli gira nel motore o nelle finestre di uscita. La prova `e2e/tests/processes.spec.ts` uccide il processo durante la proiezione e misura i fotogrammi: nessuna pausa, poi il riavvio con i dati conservati. La stessa prova spegne e riaccende il modulo a caldo.
- **Eventi**:
  - un modulo emette solo gli eventi dichiarati in `contributes.events`, ricevuti dagli altri come `<id>.<nome>`;
  - riceve gli eventi del nucleo (`core.cue.changed`, `core.output.blackout`, `core.show.opened`, …) a cui si iscrive;
  - gli eventi del nucleo nascono dal confronto dello stato prima e dopo ogni cambiamento, quindi valgono anche per i comandi aggiunti in futuro.
- **Comandi**:
  - una postazione o un pannello chiama `plugin.command`, e il comando viene eseguito nel processo del modulo;
  - un pannello può chiamare solo i comandi del proprio modulo;
  - un processo può chiamare i comandi propri e quelli dei moduli da cui dipende ("Unione", cap. 10).
- **SDK**:
  - `export default definePlugin({ activate(ctx) { … } })`;
  - `ctx` offre `engine.call`, `commands.register`, `events.on/emit`, `state.watch`, `storage`, `log` e `dataDir`;
  - `console.log` finisce nel log del motore, perché stdout è del protocollo.
- **Esempio**: `plugin-template` è il modulo **Ciao** (hello-panel): un pannello, un comando, lo spazio dati e un evento. Si installa da **Moduli → Installa da cartella…**, una voce nuova per chi sviluppa.

## Permessi: cosa è davvero imposto

Prova fatta su Electron 44.5, che include Node 24.21.

| Permesso                       | Modulo Node                                                                                                                                                                    | Come                             |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------- |
| (nessuno)                      | legge solo la propria cartella; scrive solo nella cartella privata `plugin-data/<id>`                                                                                         | modello dei permessi di Node     |
| `fs:read` · `fs:write`         | tutto il disco                                                                                                                                                                 | `--allow-fs-read/write=*`        |
| `process`                      | avviare programmi (es. FFmpeg)                                                                                                                                                 | `--allow-child-process`          |
| `addons`                       | codice nativo nel processo                                                                                                                                                     | `--allow-addons`                 |
| `network` · `network:<host>`   | connessioni solo verso gli host dichiarati, sottodomini compresi. Server e UDP solo con `network`                                                                               | controllo caricato prima del modulo |
| `storage`                      | spazio chiave/valore nel motore (10 MB)                                                                                                                                        | il motore lo rifiuta senza       |
| `native`                       | obbligatorio per `runtime: native`: un programma nativo non si può chiudere in un recinto                                                                                      | dichiarato                       |

- **La rete.** Node 24 non limita la rete, quindi la limita il controllo caricato con `--require` prima del modulo. Sostituisce in modo non annullabile le connessioni TCP (e con loro http, https, fetch e WebSocket), i server e l'UDP. I binding interni restano chiusi: `process.binding` è negato dal modello dei permessi, e questo è stato provato.
- **Accesso completo.** `fs:*`, `process`, `addons` e `native` danno accesso completo al computer. Il marketplace lo scrive chiaramente prima di installare.
- **Memoria.** Ogni modulo Node ha un massimo di 1 GB, così uno che perde memoria non ferma il computer.
- **Ambiente.** Il processo riceve solo le variabili di sistema indispensabili, niente dell'ambiente del motore.
- **Pacchetti dell'app.** Il motore avvia i moduli Node con l'Electron incluso (`ELECTRON_RUN_AS_NODE=1`), quindi il "fuse" RunAsNode deve restare acceso negli installatori.

## Moduli di terzi: il contratto è generico

Il nucleo non conosce nessun modulo. Un modulo nuovo, anche mai immaginato, ha a disposizione:

- **codice** in qualunque linguaggio: Node con l'SDK, oppure un eseguibile nativo che parla lo stesso JSON-RPC su stdio. È il caso di C/C++/Rust per SDK di protocolli video e audio;
- **interfaccia**:
  - pannelli laterali (strumenti);
  - editor in finestra propria;
  - disposizioni della postazione (`contributes.modes`), costruite con pannelli del nucleo, propri e dei moduli da cui dipende;
- **contributi dichiarati**: `itemTypes`, `sourceTypes`, `outputKinds`, `lookTemplates`, `commands`, `events`, `settings`, `extensionPoints`, `provides` (servizi sostituibili);
- **dati** senza codice (`runtime: none`): lingue, temi, look, layout, raccolte.

Le famiglie del cap. 12 cadono così:

| Famiglia                                                    | Come entra                                                                                                                                                                                                                                                           | Oggi                               |
| ----------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| Rete e controllo (OSC, MIDI, Art-Net/sACN, API REST, Stream Deck, Ableton Link) | processo del modulo + eventi del nucleo + comandi; `network` per UDP e server; MIDI/seriale con `addons` o `native`                                                                                                                                       | **funziona già**                   |
| Funzioni, integrazioni (Bibbia, timer, Planning Center, CCLI) | processo + pannelli + `storage` + `network:<host>`                                                                                                                                                                                                                 | **funziona già**                   |
| Interfaccia (strumenti, editor, disposizioni)               | pannelli `side`/`center`, `contributes.modes`                                                                                                                                                                                                                         | **funziona già**                   |
| Protocolli video (camera, NDI, Spout/Syphon, SRT, RTMP)     | `sourceTypes` (ingressi) e `outputKinds` (uscite) di un modulo nativo o Node con `process`; i fotogrammi **non** passano dal JSON (cap. 21)                                                                                                                            | contratto dei fotogrammi: **da decidere (sotto)** |
| Audio (Dante, ASIO/CoreAudio, FFT, LTC)                     | come il video, più il modello audio che manca (sotto)                                                                                                                                                                                                                | **da decidere (sotto)**            |
| Sfondi                                                      | vedi sotto                                                                                                                                                                                                                                                           | immagini del nucleo: passo 6c; sfondi dei moduli: **da decidere** |

## Il modello "come un mixer video": verificato nel documento

Il documento (cap. 06, 07, 08, 22) prevede quello che chiedi. Lo **stesso elemento usato insieme da più ingressi e più uscite** è proprio la separazione tra:

- **Sorgente**: cosa esiste;
- **Scena**: come si combinano le sorgenti;
- **Uscita**: dove va il risultato.

Nel dettaglio:

- Una **scena** è un elenco di `SceneElement` che **puntano** a una sorgente (`sourceId`), con posizione libera (`rect` 0..1), ritaglio, opacità, ordine e visibilità. La sorgente non viene copiata, quindi la stessa sorgente sta in più scene nello stesso momento. Esempio: la camera nella scena "Camera + testo" e nella scena "Camera piena".
- Un'**uscita** riceve un `Feed`, che può essere:
  - una scena;
  - una sorgente con un look;
  - lo specchio di un'altra uscita.

  Più uscite possono ricevere la stessa scena o la stessa sorgente nello stesso momento, ognuna col suo look. Per esempio la presentazione va al proiettore col look Sala, al palco col look Palco e alla diretta come fascia dentro una scena (cap. 08).
- Ogni uscita ha la sua scena attiva (`activeScene: outputId → sceneId`). Cambiare scena sulla diretta non tocca il proiettore.

Stato del codice:

- i tipi esistono nel protocollo (`Source`, `Scene`, `SceneElement`, `Feed`);
- oggi il renderer disegna i feed `source` (presentazione) e `mirror`;
- **il compositing delle scene è della Fase 1** ("Scene nel nucleo", cap. 28).

## Proposte da confermare prima della Fase 1

1. **Canale dei fotogrammi.** Un modulo che produce o riceve video (camera, NDI, sfondi animati) non manda i fotogrammi nel JSON. La proposta è un canale a parte, definito nel protocollo, che il motore apre per ogni sorgente o uscita del modulo:
   - **memoria condivisa** con un buffer circolare a più fotogrammi, sullo stesso PC;
   - nelle finestre di uscita la sorgente diventa una texture. Se il modulo cade, resta l'ultimo fotogramma valido, poi il nero (cap. 24).

   Prima di decidere serve una prova tecnica: su Electron 44, letta della memoria condivisa nel renderer contro altre strade (socket locale con fotogrammi grezzi).
2. **Modello audio.** Il documento descrive le sorgenti solo come immagine. Senza audio non ci sono né la diretta (Fase 1) né i moduli Dante/ASIO. La proposta:
   - ogni sorgente può avere anche una traccia audio;
   - ogni uscita che lo richiede (diretta, registrazione, NDI) ha un **mix audio** (volume per sorgente, muto, monitor);
   - i moduli audio portano ingressi e uscite audio con lo stesso canale a parte.

   La stessa sorgente audio può stare in più mix, come per il video.
3. **Sfondi.** La proposta:
   - uno sfondo (di slide, di elemento, predefinito del look) può puntare, oltre che a un'immagine dell'archivio (passo 6c), a **una sorgente qualsiasi**: video, camera, NDI, o una sorgente di un modulo come gli sfondi animati generati;
   - un **pacchetto di sfondi** è un modulo di soli dati (`runtime: none`) che porta immagini e video nell'archivio;
   - uno sfondo **generato** è una sorgente del suo modulo e arriva come fotogrammi (punto 1).

   Mai codice dei moduli dentro le finestre di uscita.
4. **Look dei moduli** (`lookTemplates`): restano **dichiarativi** (campi, disposizione, stile), disegnati dal renderer del nucleo. Per la stessa regola, un modello di look non può essere codice.

## Contatore delle risorse (protocollo 1.9, 2026-10-01)

- **Cosa misura.** Quanto usano piattaforma e moduli attivi rispetto al computer.
  - Il motore misura ogni 10 s: processo principale, scheda video, postazione, ogni uscita, "altro" (pannelli e servizi), dalle metriche di Electron. Misurare costa pochissimo.
  - I moduli mandano il loro consumo (memoria, CPU) nella risposta a `plugin.ping`. L'SDK lo fa da solo.
- **Minimo, attuale, massimo.**
  - Ogni modulo può dichiarare nel manifest `resources` (memoria e CPU a riposo e al massimo).
  - Cuelith ricorda i minimi e i massimi osservati davvero; i massimi restano tra un avvio e l'altro (`resources.json` nella cartella dati).
  - Il "massimo" usa il più alto tra dichiarato e osservato.
- **Semaforo** (`summarizeResources`):
  - **giallo**: il massimo stimato supera il 70% della memoria, la CPU adesso supera il 70%, oppure resta poca memoria libera;
  - **rosso**: il massimo stimato supera il 90% della memoria, oppure un'uscita perde fotogrammi (pause oltre 50 ms).
- **Interfaccia.**
  - Indicatore colorato nella barra in alto.
  - Impostazioni → Risorse: barre per memoria e processore (a riposo, adesso, al massimo, su quanto ha il PC), fotogrammi al secondo di ogni uscita, tabella per parte.
- **Misure di riferimento** (PC del fondatore, Ryzen 7 5700U, 16 GB):
  - Cuelith con una slide in onda su un'uscita: **circa 600 MB** di memoria e **meno dell'1%** del processore, uscita a 60 fotogrammi al secondo senza ritardi;
  - un modulo Node a riposo (Ciao): **circa 43 MB**.
