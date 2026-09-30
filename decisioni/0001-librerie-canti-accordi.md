# 0001 · Librerie, canti, accordi, copyright e basi musicali

Stato: **Deciso** (fondatore, 2026-09-30). Completa i capitoli 04, 12, 22 e 30 del documento di progetto.

## Richiesta del fondatore

- Scrivere canti divisi in sezioni (intro, strofa, pre-ritornello, ritornello, bridge, finale…), ognuna numerata, e proiettarle nell'ordine che si vuole, come in OpenLP.
- Un modulo dedicato ai canti e uno complementare per testi + accordi per i musicisti.
- Non una libreria sola: **quante librerie si vuole**, come playlist. Una libreria può contenere **più copie dello stesso brano** (autore, arrangiamento diversi) e **tag**.
- Una **pagina per il copyright** e la possibilità di **allegare una base musicale** (mp3 e simili).

## Decisioni

### 1. Librerie nel nucleo (cap. 04: "Scaletta e libreria di elementi")

- Una **libreria** è un elenco ordinato con nome, descrizione e colore. Se ne creano quante si vuole.
- Gli elementi stanno in un **archivio** unico; le librerie contengono riferimenti. Lo stesso elemento può stare in più librerie.
- **Copie e versioni**: "Duplica" crea un elemento indipendente (modificarlo non tocca l'originale) che ricorda l'origine (`derivedFrom`), così si trovano tutte le versioni di un brano.
- **Tag** liberi e **ricerca** su titolo, testo, autori, tag, numero (innario).
- Valgono per ogni tipo di elemento (testo, canto, passo biblico, media), non solo per i canti.
- **Rapporto con lo show**: mettendo un elemento in scaletta, lo show ne tiene una **copia** con il riferimento all'originale (`libraryRef`), così il file `.cuelith` si apre ovunque. Se l'originale cambia, la postazione propone "aggiorna dalla libreria".
- **Archiviazione**: nella cartella dati di Cuelith. Proposta: `node:sqlite` con ricerca FTS5 (da verificare con uno spike sul Node di Electron 44); in alternativa file JSON. Esportazione/importazione di una libreria come file unico per condividerla.

### 2. Crediti e copyright nel nucleo

Una scheda **Crediti** per ogni elemento: titolo e titoli alternativi, autori con ruolo (testo, musica, traduzione, arrangiamento), riga di copyright, editore, anno, numero CCLI, note di licenza, dove mostrarli (prima slide, ultima slide, mai). I moduli possono aggiungere campi propri (es. Canti).

### 3. Allegati e basi musicali

- Un elemento può avere **allegati** audio (mp3, wav, m4a/aac, flac, ogg) con un ruolo: base, traccia guida, click.
- I file vengono **copiati** nell'archivio media della cartella dati (nome = hash del contenuto): spostare l'originale non rompe nulla, due copie uguali occupano spazio una volta.
- La riproduzione (scelta dell'uscita audio, play/pausa/volume) arriva con i **media del nucleo** (Fase 1). Sincronizzare le slide ai tempi della base è una funzione del modulo Canti.

### 4. Modulo Canti (`plugin-songs`, famiglia funzioni)

Separato dalla modalità Culto perché serve anche a concerti ed eventi. Culto dipende da Canti.

- Sezioni tipizzate e numerate, ognuna con un colore: Intro, Strofa 1…n, Pre-ritornello, Ritornello, Bridge, Strumentale, Tag, Finale (`slide.group`).
- **Ordine di proiezione** (`item.arrangement`) trascinando le sezioni o scrivendolo in breve come in OpenLP (`v1 c1 v2 c1 b1 c1`); ripetizioni libere.
- In onda: tasti per saltare a una sezione (es. C = ritornello, B = bridge).
- Importazione da OpenLP (OpenLyrics), ChordPro, testo semplice.
- Pagina copyright con i campi dei canti; base musicale sincronizzabile.

### 5. Modulo Accordi (`plugin-chords`, estende Canti)

- Accordi sopra le sillabe (campo `chords`, ChordPro), trasposizione, capotasto, tonalità.
- Vista musicisti sul monitor palco (look Palco) e sui tablet in rete (ruolo visualizzatore): il pubblico vede solo il testo, stesso comando "avanti".

## Impatto sul protocollo

`@cuelith/protocol` 1.1.0 (compatibile): `Item` guadagna `credits`, `tags`, `attachments`, `libraryRef`, `derivedFrom` opzionali; nuovi metodi `library.*` e `media.import` con un ambito `library`. Il modello di slide (gruppi, arrangiamento, campo accordi) c'era già.

## Ordine dei lavori

**Fase 0** (nucleo): 5 uscite e look → 6 file `.cuelith` → **6b librerie, crediti, archivio media e allegati** → 7 lingue → 8 postazioni in rete → 9 gestore moduli → 10 modulo d'esempio e prova "le uscite non cadono" → 11 documentazione.

**Fase 1**: media del nucleo (immagini, video, audio con scelta dell'uscita) → Canti → Accordi → Bibbia → modalità Culto → Timer.

Perché: le uscite sono il cuore del prodotto e vanno provate per prime; le librerie sono del nucleo e servono ai moduli, quindi vengono prima del gestore moduli; Canti e Accordi sono moduli e richiedono il gestore moduli (passo 9) e la riproduzione audio (media del nucleo).
