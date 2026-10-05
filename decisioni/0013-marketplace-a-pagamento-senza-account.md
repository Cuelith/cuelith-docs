# 0013 · Marketplace con plugin a pagamento: senza account, senza database, senza denaro

Stato: **Deciso** (fondatore, 2026-10-05) nelle linee generali; **in costruzione**: fatte le fasi 1 (catalogo, schema, controlli) e 2 (pagine del sito). Sostituisce le parti «Account», «Server licenze» e «Negozio» della [decisione 0008](0008-moduli-a-pagamento.md); restano valide le sue regole di fondo (licenza firmata legata al computer, verifica senza internet, mai fermare una diretta, limiti onesti). La licenza del nucleo è la [0012](0012-licenza-gpl-e-eccezione-plugin.md). Non è un parere legale né fiscale.

## Vincoli del fondatore

1. **Costo zero**: niente server né database tradizionali; solo GitHub Pages, Cloudflare (Pages Functions/Workers, KV, Access) nei piani gratuiti.
2. **Nessun account**: né per chi usa Cuelith né per chi pubblica. Meno dati, meno GDPR.
3. **Automazione**: l'autore propone il plugin da un modulo sul sito senza vedere GitHub; il fondatore apre una pagina protetta e preme **Approva** o **Rifiuta**; il resto è automatico.
4. **Fisco**: il fondatore non ha Partita IVA e non incassa le vendite. Gli autori vendono con il **proprio negozio presso un rivenditore registrato** (oggi Lemon Squeezy), che incassa, versa l'IVA e gestisce i rimborsi.
5. **Certezza e anti-pirateria**: chi compra usa il plugin anche se cambia computer, con la sola chiave ricevuta; una chiave non si può condividere con tutti; un rimborso invalida la licenza.

## Che cosa si è deciso (e che cosa si è scartato)

| Idea iniziale | Decisione | Perché |
| --- | --- | --- |
| Nuovo file `plugins.json` nel repo del sito | **Si estende il registry esistente** (`cuelith-registry`) | Ha già impronte SHA-256, controlli in CI e pubblicazione automatica, e il nucleo lo legge già. Un secondo file duplicherebbe tutto; un commit sul repo del sito, poi, non aggiorna il sito senza un nuovo deploy |
| Pagina admin con password in una variabile d'ambiente | **Cloudflare Access** (codice via email, già in uso per l'hub), con verifica del token dentro la funzione | Una password su una pagina pubblica va difesa da tentativi ripetuti; Access è più sicuro e gratuito |
| `license_api_url` scelto dallo sviluppatore | Campo `licensing` con **fornitore ammesso** + `storeId` + `productId`; un **notaio** nostro verifica la chiave presso il fornitore e firma un permesso | Un indirizzo libero riceverebbe chiave e identificativo del computer dell'utente; e un file solo cifrato in locale si può fabbricare a mano: serve una firma che solo il notaio sa produrre |
| ID hardware dai componenti del PC | **Coppia di chiavi Ed25519 generata sul computer**, protetta da `safeStorage` | Gli ID hardware cambiano con un componente e bloccano chi ha pagato; non aggiungono sicurezza e si avvicinano ai dati personali |
| Il rivenditore registrato «divide» la vendita e manda il 10% al fondatore | **Commissione di affiliazione volontaria**, vedi sotto | Non esiste uno split imposto dal catalogo |
| Posti per licenza | **3 computer** insieme | Fisso, portatile, un terzo; protegge dalla condivisione di massa |

## Commissione ed affiliazione: che cosa è vero

- Il programma di affiliazione di Lemon Squeezy lo **gestisce il negozio dello sviluppatore**: lui lo attiva e lui iscrive il fondatore come affiliato. Se lo disattiva o cambia la percentuale, il progetto non può impedirlo.
- Il fondatore, come affiliato, riceverebbe **dollari su banca o PayPal** (attesa di 30 giorni, 2% di spese del servizio e spese di PayPal). Verificato sulla documentazione ufficiale il 2026-10-05. Le pagine consultate non dicono come si trattano le commissioni in caso di rimborso: va chiesto al supporto o provato.
- Il campo `checkoutUrl` del catalogo contiene il link di acquisto, **può** contenere il riferimento di affiliazione. Gli utenti vengono avvisati nel marketplace che il link può contenerlo.
- **Fisco: da chiarire con un commercialista prima di incassare il primo centesimo.** Il fondatore ha confermato che lo farà. Le provvigioni non sono diritti d'autore, e una provvigione continuativa su un marketplace rischia di essere considerata attività abituale e non «prestazione occasionale».
- Finché questo non è chiarito, nel codice e nei testi **nulla promette incassi** al fondatore.

## Architettura

```
Autore ──modulo sul sito──▶ Pages Function ─▶ KV (richiesta «pending», chiave pubblica Ed25519)
                                                    │
Fondatore ─pagina protetta da Access─▶ Approva ─────┘
                                                    ▼
                        GitHub (token solo sul repo del registry): pull request su cuelith-registry
                                                    ▼  controlli verdi (pacchetto, impronta, permessi, firma, negozio)
                        main ─▶ GitHub Pages: index.json (solo gratuiti) + index-2.json (tutti)
                                                    ▼
              programma (index-2) e sito (/api/modules) mostrano il plugin: «Acquista» apre il negozio

Utente ─ chiave via email dal negozio ─▶ programma ─▶ notaio (Pages Function, senza stato)
                                                      │  chiede al fornitore di attivare il posto (API pubblica)
                                                      │  verifica store/prodotto e limite di 3 posti
                                                      ▼
                                         permesso firmato Ed25519 legato alla chiave del computer
```

- **Catalogo**: il registry su Git è il «database». Gli autori non hanno account: **possiedono la chiave** con cui firmano i pacchetti; gli aggiornamenti firmati con la stessa chiave possono passare senza nuova approvazione manuale.
- **Backend**: Pages Functions dentro `cuelith-site` (un solo deploy, stesso dominio, KV collegato). Stato delle richieste solo in KV, cancellato dopo l'approvazione o il rifiuto. I dati personali si limitano a quanto serve per contattare l'autore e non finiscono mai nel repo pubblico.
- **Notaio**: non memorizza nulla. Riceve chiave di licenza e chiave pubblica del computer, interroga il fornitore e restituisce un permesso con «rinnova dopo 30 giorni» e «tolleranza 90 giorni».
- **Nucleo**: verifica il permesso offline, rinnova in silenzio ogni 15-30 giorni, non blocca mai una diretta (`isOnAir`). Dopo un rimborso il fornitore disattiva la chiave, il notaio non rinnova e il permesso scade da solo.
- **Garanzia del limite**: il notaio rifiuta una chiave il cui limite di attivazioni non sia **al massimo 3**. L'autore lo imposta nel suo negozio; è una condizione per essere approvati.

## Regola di compatibilità degli indici

I programmi già installati (fino alla 0.2.5) rifiutano un indice che contiene campi che non conoscono. Perciò:

- `index.json` (schema 1) resta com'è: **solo plugin gratuiti**, **solo campi di sempre**;
- `index-2.json` (schema 2) porta tutto, anche i plugin a pagamento e i campi nuovi.

Il nucleo che introduce il marketplace a pagamento leggerà l'indice 2. Fino ad allora un plugin a pagamento approvato non compare nei programmi vecchi (e non li rompe).

## Fasi

| Fase | Contenuto | Stato |
| --- | --- | --- |
| **1** | Schema del registry e dell'SDK (protocollo 1.14: `access`, `price`, `checkoutUrl`, `licensing`, `authorKey`, firma dei pacchetti), controlli del registry, due indici, decisione 0013 | **Fatto** (su `dev`, non rilasciato) |
| 2 | Pagine `/marketplace` e `/marketplace/submit` nel sito (italiano e inglese, telefono compreso), con l'avviso sull'affiliazione; `/marketplace/buy/<id>`; controllo condiviso delle proposte; `pnpm keys` e `pnpm sign` nel modello di plugin | **Fatto** (su `dev`, non pubblicato; il modulo risponde «non disponibile» finché non c'è la fase 3) |
| 3 | Pages Functions: proposta con KV e Turnstile, pannello con Access, approvazione che apre la pull request, notaio dei permessi | **Codice fatto e provato** (94 test del sito, anche nel runtime vero di Cloudflare); **da collegare** all'infrastruttura (KV, Turnstile, Access, token, chiavi: `cuelith-site/MARKETPLACE_SETUP.md`) e da provare dal vero con un negozio di prova |
| 4 | Nucleo: stato «a pagamento», pulsante Acquista, campo licenza, chiavi del computer con `safeStorage`, permesso cifrato, rinnovo silenzioso, `ctx.license` per i plugin; prova con un fornitore simulato | **Fatto** su `dev` (protocollo 1.15; non rilasciato): motore, interfaccia, desktop, SDK; prove con un Notaio finto, con il Notaio vero del sito e nell'app vera |
| 5 | `MARKETPLACE_SETUP.md`, aggiornamento di `CLAUDE.md` e `STATO.md` | Da fare, in parte già a ogni fase |

## Fase 3: scelte fatte nel costruire

- **Approvazione = pull request, non commit.** Il sito apre una pull request nel registry; la CI `validate` (schema vero, pacchetto, impronta, permessi, icona unica, firma) è il cancello, e a controlli verdi GitHub la unisce da sola (auto-merge). Un commit diretto su `main` con una voce sbagliata bloccherebbe la pubblicazione di tutti gli indici. Perché funzioni il repo del registry deve **permettere l'auto-merge** e la protezione di `main` non deve chiedere approvazioni umane (il «Approva» del pannello lo è): da confermare, vedi `MARKETPLACE_SETUP.md` §4.
- **Il pannello verifica il token di Access da sé** (firma RS256 con le chiavi del team, scadenza, emittente, AUD, email) e chiede, per ogni modifica, un'intestazione propria e l'origine del sito. Senza configurazione risponde 503: mai un accesso libero. Niente password.
- **La voce del registry si costruisce dal pacchetto, non dalla proposta**: manifest e icona si leggono dallo zip (solo quelle due voci si decomprimono, con limiti di dimensione), impronta e dimensione si calcolano sul file scaricato; la proposta fornisce solo ciò che il pacchetto non dice (tipo, prezzo, negozio, chiave d'autore, firma). Un plugin di terzi non è mai «verified». Un aggiornamento è accettato solo con la stessa chiave d'autore, lo stesso tipo e una versione nuova.
- **Il link di acquisto pubblicato lo sceglie il fondatore** nel pannello (quello con il suo riferimento di affiliazione), ricontrollato contro i negozi ammessi.
- **La firma del pacchetto è un campo del modulo**: con la chiave d'autore ogni versione ha la sua firma (`pnpm sign`), altrimenti il registry la rifiuterebbe.
- **Dati personali**: l'email di chi propone sta solo in KV (scade in 60 giorni e si cancella alla decisione) e non finisce mai in GitHub; l'indirizzo di chi invia non si conserva, solo un'impronta con scadenza di un giorno.
- **Notaio**: rifiuta le chiavi il cui limite di attivazioni non sia da 1 a 3, e libera subito il posto preso se poi rifiuta; un permesso è legato al computer (rinnovo: il nome del posto presso il fornitore deve essere quello derivato dalla chiave del computer); chiavi di prova (`test_mode`) solo con `ALLOW_TEST_MODE`.

### Verificato dal vero il 2026-10-06 (negozio di prova `lzrhive-test` in test mode)

Il Notaio vero del sito, contro l'API pubblica vera di Lemon Squeezy (store 490522, prodotto 1413803, chiave di prova con limite 3 e scadenza a 1 anno): la forma delle risposte coincide con quella attesa (`meta.store_id`, `meta.product_id`, `license_key.status|activation_limit|activation_usage|test_mode|expires_at`, `instance.id|name`); una chiave di prova è rifiutata senza `ALLOW_TEST_MODE` e il posto preso si libera subito; tre computer entrano, il quarto riceve `limit`; il rinnovo funziona per il computer giusto e dà `revoked` per il posto di un altro computer, per un posto inventato e per un posto già disattivato; «disattiva» libera il posto e il quarto computer entra; una chiave inventata dà `invalidKey`; prodotto o negozio diversi danno `wrongProduct` e il posto si libera; alla fine 0 posti usati. Due osservazioni: (1) la risposta del fornitore contiene `customer_name` e `customer_email` in `meta`: il Notaio le riceve ma non le conserva né le inoltra (risponde solo con il permesso); (2) una chiave con scadenza propria (`expires_at`, qui 1 anno) fa scadere anche i permessi: **il permesso non supera mai la scadenza della chiave**. Di conseguenza, per un plugin venduto una volta sola, l'autore deve impostare la chiave **senza scadenza** (o accettare che dopo un anno smetta di funzionare); va scritto nelle condizioni per gli autori.

**Chiave disattivata dal venditore** (provata il 2026-10-06 con «Disable» nel pannello del negozio di prova, equivalente a ciò che fa un rimborso): il fornitore risponde **HTTP 400** con `valid: false` / `activated: false`, `error: "This license key is disabled."` e `status: "disabled"`; il Notaio risponde `invalidKey` all'attivazione e `revoked` al rinnovo, e il programma segna la licenza come revocata. Il rimborso vero dal lato cliente non è provabile in un negozio non attivato («contatta il supporto»); il portale cliente (`/billing`) risponde «store not activated» in test mode.

### Ancora da verificare (non provabile senza un negozio)

I test usano un fornitore simulato che segue la documentazione ufficiale. Vanno confermati con un prodotto in **test mode** di Lemon Squeezy: la forma esatta delle risposte (`meta.store_id`, `license_key.activation_limit`, stato dopo un **rimborso** — la documentazione consultata non lo dice), se il cliente può **liberare un posto** quando il computer è rotto (portale del cliente) o serve un nostro percorso, e il comportamento di `validate` con `instance_id`.

## Fase 4: come funziona nel programma

**Chiavi e permessi sul computer.** Un solo file cifrato, `licenses/vault.bin` nella cartella dati, scritto in modo atomico. Il motore non conosce Electron: riceve una custodia (`SecretStore`: cifra, decifra, «è disponibile?») che il desktop realizza con **`safeStorage`** (DPAPI su Windows, Portachiavi su macOS, libsecret su Linux; su Linux senza portachiavi, quando Electron ripiega su un testo non protetto, la custodia dice «non disponibile» e le licenze non si attivano: mai finta sicurezza). Dentro il file, cifrato: la **coppia di chiavi Ed25519 del computer** (casuale, nata al primo bisogno, mai uscita da lì) e, per plugin, chiave di licenza, posto presso il fornitore e permesso firmato. Nessun identificativo hardware, nessun nome del computer. Se il file non si decifra (cartella copiata su un altro utente o computer) si riparte vuoti senza cancellarlo: le licenze si riattivano.

**Cosa va al Notaio.** Solo la chiave di licenza, l'identificativo del plugin, la chiave *pubblica* casuale del computer (e il posto, per rinnovare). Il fornitore vede un nome anonimo del computer derivato da quella chiave (`cuelith-` e 12 caratteri). Il permesso che torna si controlla qui con le chiavi pubbliche del Notaio (`NOTARY_PUBLIC_KEYS` del protocollo): se non è firmato dal Notaio, o non è per quel plugin e quel computer, si scarta anche se il Notaio dicesse «ok».

**Giorni.** Il permesso dice «rinnova dopo 30 giorni» e «scade dopo 90». Dal trentesimo giorno il motore prova a rinnovare in silenzio (primo giro 30 secondi dopo l'avvio, poi ogni 6 ore; il plugin continua a funzionare). Se il Notaio risponde con un rifiuto definitivo (rimborso, chiave cambiata, prodotto fuori regola) la licenza si segna **revocata** e non si riprova; un guasto (rete, servizio giù, risposta strana) non toglie nulla. Passati i 90 giorni senza rinnovo la licenza è **scaduta**.

**Mai fermare una diretta.** I plugin ammessi sono «agganciati»: concedere è immediato, **togliere** (scadenza, revoca, «Disattiva questo computer») si applica solo fuori onda (`isOnAir`); in onda si rimanda e si riprova ogni minuto. Il controllo non avviene mentre un plugin lavora, ma solo quando il registro dei moduli decide chi parte (avvio, installazione, abilitazione, cambio di licenza).

**Dove sta nel nucleo.** Un modulo autonomo (`apps/engine/src/licenses/`: `secrets`, `vault`, `notary`, `service`), un filtro in `ModuleRegistry.active()` e `statuses()` (un plugin a pagamento senza licenza valida è «disattivato» con il motivo `core.license.needed|expired|revoked`), il campo `licensed` nell'elenco dei plugin installati (vale per i plugin installati dal marketplace come «a pagamento» e **non si toglie reinstallandoli da file**), e i metodi `license.list|activate|refresh|deactivate` (ambito «plugins», cioè la regia). Un plugin a pagamento non si installa né si aggiorna senza licenza valida. Il marketplace legge l'indice 2 e, se non risponde, l'indice 1 (così funziona anche prima che l'indice 2 sia pubblicato).

**Perché il nucleo non basta (e cosa fa il plugin).** Il nucleo è GPL: chi lo modifica toglie il suo controllo. Perciò il plugin si verifica da solo con **`ctx.license.verify()`** dell'SDK: chiede al nucleo (`license.prove`) il permesso e la **firma del computer su una sfida casuale**, e li controlla con le chiavi pubbliche del Notaio. Un permesso copiato da un altro computer non ha la firma giusta; una risposta registrata non vale per una sfida nuova; un nucleo che «dice di sì» non può inventare né il permesso né la firma. Il plugin la chiama all'avvio, mai in diretta.

**Interfaccia.** Marketplace: la scheda di un plugin a pagamento mostra prezzo, **Acquista** (apre nel browser il `checkoutUrl` del catalogo, con l'avviso sull'affiliazione), il campo della chiave di licenza e «Attiva»; l'installazione si sblocca con la licenza valida. Installati: stato della licenza (`attiva fino al…`, `da rinnovare`, `scaduta`, `non più valida`), «Verifica ora» e «Disattiva questo computer».

**Chiavi del Notaio.** Generate il 2026-10-06: la **pubblica** (`n1`) è nel protocollo; la **privata** è in un file fuori da ogni repository, da incollare in `NOTARY_PRIVATE_KEY` di Cloudflare e poi cancellare (vedi `MARKETPLACE_SETUP.md` §6). Cambiare chiave: nuovo `NOTARY_KEY_ID` e nuova voce in `NOTARY_PUBLIC_KEYS` (i permessi vecchi, firmati con `n1`, restano validi finché `n1` resta nell'elenco).

**Prove.** Motore (153 test) con un Notaio finto e con il **Notaio vero del sito** (formato dei messaggi, codici d'errore, firme, limite di 3 computer); protocollo e SDK (permessi, prove di possesso, replay, permessi copiati); app vera con Playwright (acquisto, chiave sbagliata e giusta, installazione, verifica, disattivazione).

## Cosa non si può impedire

Come in 0008: chi modifica sia il nucleo (GPL) sia il codice di un plugin può togliere i controlli. Il controllo del nucleo non basta da solo: il plugin verifica la propria licenza. Lo scopo realistico è rendere condividere un codice più scomodo che comprare, e far «bruciare» una chiave pubblicata (i primi 3 computer la consumano). Il valore dei plugin a pagamento sta anche in aggiornamenti e assistenza.

## Da fare prima di aprire ai primi autori

- **Commercialista** per la commissione (vedi sopra).
- **Parere legale** su termini del marketplace, eccezione per i plugin (0012) e obblighi da piattaforma online (DSA): con acquisto presso un rivenditore esterno il progetto dovrebbe restare solo vetrina, ma va confermato.
- **Condizioni per gli autori** scritte nella pagina di proposta: niente malware, uso obbligatorio di un rivenditore registrato, limite di 3 posti, EULA a carico dell'autore.
- Un negozio vero (anche di prova, in modalità test) presso il fornitore, per provare il notaio dal vero.
