# cuelith-docs

Documentazione di Cuelith: il documento di progetto e le decisioni prese durante il lavoro. È la fonte di verità per chi sviluppa il nucleo, l'SDK e i moduli.

- [`documento/index.html`](documento/index.html): il **documento di progetto**. Visione, funzionamento, ecosistema, interfaccia e specifica tecnica (capitoli 1–30). Si apre in un browser, anche senza internet.
- [`decisioni/`](decisioni): le **decisioni numerate** che completano o cambiano il documento. In caso di conflitto vale la decisione più recente.

| N. | Decisione |
| --- | --- |
| [0001](decisioni/0001-librerie-canti-accordi.md) | Librerie multiple, crediti, basi musicali, moduli Canti e Accordi |
| [0002](decisioni/0002-formati-dei-canti.md) | Formati dei canti: OpenLyrics e ChordPro |
| [0003](decisioni/0003-sfondi.md) | Sfondi dei testi |
| [0004](decisioni/0004-licenza-marketplace-aggiornamenti.md) | Licenza, marketplace, installatori e aggiornamenti, librerie organizzate |
| [0005](decisioni/0005-sezioni-e-fuori-scaletta.md) | Sezioni dei canti ed elementi fuori scaletta |
| [0006](decisioni/0006-interfaccia-strumenti-impostazioni.md) | Interfaccia: zona centrale, strumenti, impostazioni, disposizioni |
| [0007](decisioni/0007-processi-dei-moduli.md) | Processi e permessi dei moduli, moduli di terzi, contatore delle risorse |
| [0008](decisioni/0008-moduli-a-pagamento.md) | Moduli a pagamento: architettura (da costruire più avanti) |
| [0009](decisioni/0009-postazioni-in-rete.md) | Postazioni in rete locale |

## Stato

**Fase 0 (fondamenta) completata il 2026-10-01**: gli otto criteri del capitolo 28 sono soddisfatti, ognuno con una prova automatica (vedi il capitolo 28 del documento). Prossima: Fase 1, culto con diretta.

## Dove sta il codice

Organizzazione GitHub [Cuelith](https://github.com/Cuelith): `cuelith-core` (motore, app, postazione, uscite), `cuelith-sdk` (protocollo e strumenti per i moduli), `cuelith-registry` (elenco dei moduli), `plugin-template` (modulo d'esempio), `plugin-locale-it` (lingua italiana), `plugin-songs` (modulo Canti).

Licenza Apache 2.0.
