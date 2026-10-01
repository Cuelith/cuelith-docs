# 0003 · Sfondi dei testi

Stato: **Deciso** (fondatore, 2026-09-30).

## Richiesta del fondatore

Poter mettere sfondi ai testi: PNG, JPEG, GIF e simili.

## Decisioni

- Gli sfondi sono **del nucleo**, non un modulo: il cap. 04 mette "slide di testo, immagini, video" nel nucleo, e il modello dati li prevede già (`slide.background`, layer `background` del look Sala).
- **Formati**: PNG, JPEG, WebP, GIF (anche animate), SVG solo come immagine statica; i video di sfondo (MP4/WebM, in loop e senza audio) arrivano con i media del nucleo in Fase 1.
- **Dove si sceglie**: per singola slide, per tutto l'elemento (ogni slide senza sfondo proprio usa quello dell'elemento) e come sfondo predefinito del look (es. il colore o l'immagine della Sala). Ordine: slide → elemento → look.
- **File**: importati nell'archivio media (decisione 0001), con anteprima in miniatura; lo show li porta con sé quando si esporta/condivide.
- **Leggibilità**: il look può scurire lo sfondo (velo regolabile) perché il testo resti leggibile.
- **Uscite**: le immagini si caricano prima di andare in onda (mai un fotogramma vuoto), la dissolvenza vale anche per lo sfondo; il look Palco per definizione non mostra sfondi.

## Ordine

Dopo il passo 6b (archivio media): sfondi immagine nel nucleo. GIF animate e video di sfondo con i media del nucleo (Fase 1).

## Come è fatto (passo 6c, 2026-10-01, protocollo 1.11.0)

- **Dove si sceglie**: nel pannello Slide, menu **Sfondo**.
  - Per la slide scelta (quella in anteprima, altrimenti quella in onda).
  - Per tutto l'elemento.
  - Immagine predefinita del look Sala.
  - Velo: nessuno, leggero, medio, forte.

  Ogni voce ha il suo "Togli".
- **Ordine**: slide → elemento → look. Il look Palco non mostra sfondi, e nemmeno un look senza il layer `background`.
- **File**: l'immagine si sceglie dal computer del motore e finisce nell'archivio media (`media.import`). Slide ed elementi la ricordano come `media:<impronta>`. Il motore rifiuta sfondi che non sono immagini dell'archivio.
- **Uscite**:
  - l'immagine si scarica e si decodifica **prima** del cambio: finché non è pronta resta ciò che c'è, poi slide e sfondo cambiano insieme, con la dissolvenza del look;
  - lo sfondo della slide in anteprima si carica in anticipo;
  - l'immagine riempie l'uscita senza deformarsi;
  - se un'immagine non si carica entro 4 secondi si va avanti senza: mai uno schermo bloccato.
- **Postazione**: miniature, anteprima e programma mostrano lo sfondo e il velo come le uscite.
- **Protocollo**: `Item.background`, `item.create/update` con `background`, `slideBackground`, `mediaUrl`; stile del look Sala con `background.image` e `background.dim`; `look.update` ora esiste nel motore e valida lo stile.

### Rimandato

- GIF animate e video di sfondo (oggi una GIF mostra il primo fotogramma): Fase 1, coi media del nucleo.
- Scegliere uno sfondo tra le immagini già nell'archivio, con le miniature (oggi si sceglie il file ogni volta; un file già presente non si duplica).
- Portare le immagini insieme allo show quando lo si copia su un altro computer: oggi il file `.cuelith` ricorda le immagini ma non le contiene.
- Scegliere sfondi da una postazione in rete (decisione 0009).
