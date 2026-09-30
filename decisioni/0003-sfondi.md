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
