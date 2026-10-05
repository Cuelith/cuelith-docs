# 0011 · Contributi esterni e tutela del progetto

Stato: **Deciso** (2026-10-02). Tutto gratuito. Restano al fondatore: verifica in due passaggi e deposito del marchio.

## Come contribuisce chi è fuori

1. Fa un **fork** del repo (è già permesso: i repo sono pubblici) e lavora lì.
2. Apre una **pull request verso `dev`**, che ora è il ramo predefinito di ogni repo.
3. Alla prima proposta accetta l'**accordo di contribuzione** con un commento.
4. Partono i controlli automatici.
5. Il responsabile approva o chiede correzioni. Senza la sua approvazione non entra nulla.

Per la maggior parte delle persone la strada giusta resta un **modulo**: non serve permesso e il codice resta dell'autore.

## Cosa c'è

| Cosa | Dove |
| --- | --- |
| Come contribuire, guida di compatibilità per chi sviluppa, sicurezza, comportamento, nome e logo, accordo di contribuzione, modelli per segnalazioni e proposte | repo `Cuelith/.github`: vale per tutti i repo dell'organizzazione |
| Approvazione obbligatoria del responsabile | `.github/CODEOWNERS` in ogni repo |
| Accordo di contribuzione con firma automatica | `.github/workflows/cla.yml` in ogni repo; le firme stanno nel ramo `cla-signatures` |
| Avviso su licenza, nome e logo | `NOTICE` in ogni repo |
| Protezione di `main` e `dev` | niente riscrittura della storia, niente cancellazione; per chi non è amministratore servono pull request, un'approvazione e i controlli verdi |
| Segnalazioni di sicurezza riservate | attive su ogni repo (scheda Security → Report a vulnerability) |

Il responsabile, da amministratore, può ancora inviare direttamente su `dev` e portare `dev` su `main` per i rilasci: la procedura di rilascio non cambia. `main` continua a ricevere solo versioni con tag.

## Perché l'accordo di contribuzione

Con la sola licenza Apache ogni contributore resta l'unico a poter decidere del suo pezzo. L'accordo lascia a ciascuno il diritto d'autore e dà al titolare del progetto il diritto di distribuire i contributi anche con altre condizioni: serve per poter offrire in futuro moduli o edizioni a pagamento (decisioni 0004 e 0008) senza dover chiedere il consenso a tutti. In cambio il progetto si impegna a tenere i contributi accettati disponibili con una licenza open source (aggiornato il 2026-10-05, decisione 0012: ogni contributo è offerto con la licenza del repo e il titolare ha il diritto di rilicenziare). Il testo è una base ragionevole, non un parere legale: prima di vendere qualcosa va fatto rivedere da un avvocato, indicando il titolare per nome.

## Nome e logo

La licenza copre il codice, non il nome. `TRADEMARK.md` dice cosa si può fare (dire «per Cuelith», parlarne, ridistribuire gli installatori ufficiali) e cosa no (versioni modificate col nome o il logo, nomi che si confondono, «Cuelith Qualcosa»). La regola vale di più a marchio depositato: vedi sotto.

## Moduli a pagamento di altri sviluppatori

- **Oggi**: il marketplace elenca solo moduli gratuiti. Un autore può vendere il suo modulo per conto suo, con la sua licenza; si installa da file.
- **Più avanti** (decisione 0008): moduli a pagamento nel marketplace con account autore, prezzo, condizioni dell'autore e licenze legate ai computer. Ripartizione dei ricavi e date non sono decise.

## Resta da fare (fondatore)

- **Verifica in due passaggi**: attivarla sul proprio account GitHub e poi renderla obbligatoria nell'organizzazione.
- **Marchio**: ricerca di anteriorità e deposito (nazionale o europeo), classi 9 e 42.
- **Un indirizzo email del progetto** per le segnalazioni, al posto del modulo riservato di GitHub usato oggi anche per il comportamento.
- Le prove dell'accordo di contribuzione con una proposta vera: il flusso è configurato ma non ancora visto in azione, perché serve una proposta da un account esterno.
