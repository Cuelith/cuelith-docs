# 0012 · Licenza: nucleo GPLv3, SDK Apache 2.0, eccezione per i plugin

Stato: **Deciso** (fondatore, 2026-10-05). Sostituisce la parte sulla licenza della [decisione 0004](0004-licenza-marketplace-aggiornamenti.md) («nucleo Apache 2.0 per sempre»). Non è un parere legale: prima del primo plugin a pagamento serve un avvocato (vedi 0008).

## Il problema

Con Apache 2.0 chiunque può prendere il nucleo, migliorarlo in segreto e rivenderlo come programma chiuso. Il fondatore vuole che il nucleo resti libero per sempre, e insieme vuole che chiunque possa scrivere plugin, anche a pagamento e a sorgente chiuso.

## La scelta

| Parte | Licenza | Perché |
| --- | --- | --- |
| `cuelith-core` (programma) | **GPL 3.0 o successiva** + eccezione per i plugin | Chi distribuisce il programma, anche modificato, deve dare il sorgente sotto GPL. Un fork chiuso non è più possibile |
| `cuelith-sdk` (protocollo, schemi, tipi, libreria dei pannelli) | **Apache 2.0** | Un plugin incorpora l'SDK. Se l'SDK fosse GPL, ogni plugin lo diventerebbe e il marketplace commerciale sparirebbe |
| `plugin-template` | Apache 2.0 | Si parte da lì per qualsiasi plugin, anche chiuso |
| Plugin ufficiali: `plugin-songs`, `plugin-locale-it`, `plugin-locale-en` | **GPL 3.0 o successiva** | Sono parte della distribuzione ufficiale: restano libere come il nucleo |
| `cuelith-registry`, `cuelith-docs`, `cuelith-site` | Apache 2.0 (invariata) | Dati, testi, sito: non sono il cuore |

Per le versioni già pubblicate (nucleo 0.2.0, Canti 0.5.0, lingue 0.2.0 e 0.1.0) la licenza Apache **non si può ritirare**: restano disponibili con quella licenza. La GPL vale dalla versione successiva di ciascun repo.

## L'eccezione per i plugin (`PLUGIN-EXCEPTION.md` nel nucleo)

È un **permesso aggiuntivo** ai sensi della sezione 7 della GPLv3, quindi non è una licenza diversa: il nucleo resta GPL, il file di licenza resta il testo GPL puro (così GitHub lo riconosce) e l'eccezione è dichiarata in `NOTICE`, nel README e nel suo file. Dice che un plugin che dialoga col nucleo **solo** tramite l'interfaccia pubblica è un'opera indipendente, con la licenza che l'autore sceglie:

1. il protocollo (messaggi JSON tra processi separati),
2. l'SDK,
3. i pannelli (interfaccia del plugin, mostrata dal nucleo in un iframe isolato, con sandbox e origine opaca, che parla solo con messaggi),
4. i file del plugin (manifest, pacchetto, icone, lingue).

Non copre un plugin che contiene o carica codice del nucleo. Se qualcuno modifica il nucleo può togliere l'eccezione dalla sua versione (è un diritto GPL).

**Perché regge:** è l'architettura già decisa in 0007 (plugin in processi separati) e nel cap. 11/24 del documento (pannelli in iframe con sandbox). Il confine non è una promessa: è nel modo in cui il programma è fatto. L'eccezione lo rende esplicito, così nessuno deve indovinare dove sta il «collegamento» ai sensi della GPL.

## Che cosa la GPL non fa (da dire senza giri di parole)

- Obbliga a dare il sorgente **chi distribuisce** una versione modificata, ai destinatari. Chi modifica e non distribuisce non deve nulla a nessuno.
- Non obbliga a restituire i miglioramenti **al progetto**: lo si chiede nelle regole di contribuzione, non lo impone la licenza.
- Non protegge il nome: serve il marchio (sotto).

## Contributi esterni

L'accordo di contribuzione (`CLA.md`, repo `.github`) è stato riscritto: ogni contributo è offerto **con la licenza del repo a cui si contribuisce** e il titolare del progetto riceve il **diritto di cambiarla in futuro** (rilicenziare). Il fondatore ha il 100% del diritto d'autore attuale (nessun contributo esterno accettato al 2026-10-05), quindi il cambio non ha richiesto il consenso di nessuno.

## Il nome «Cuelith»

Il README del nucleo e `TRADEMARK.md` dicono che il nome e il logo non fanno parte della licenza (GPL 7(e)): un fork deve cambiare nome e logo, sostituire `brand/` e usare i propri indirizzi per aggiornamenti e marketplace. Può scrivere «basato su Cuelith». Il deposito del marchio resta rimandato (vedi STATO.md); la regola scritta vale intanto come avviso.

## Cosa cambia nel codice e nei repo

- `cuelith-core`: `LICENSE` (testo GPLv3), `PLUGIN-EXCEPTION.md`, `NOTICE`, `REUSE.toml` (licenza di ogni file), campo `license` di ogni `package.json`, README, Impostazioni → Informazioni («GPL 3.0»), e il testo della licenza e l'eccezione dentro l'installatore (`resources/legal/`), perché la GPL vuole la licenza insieme al programma.
- Plugin ufficiali: `LICENSE`, `NOTICE`, README, `license` nel manifest. Il registry elenca ancora «Apache-2.0» per Canti 0.5.0, ed è vero per quel pacchetto; passerà a «GPL-3.0-or-later» con il prossimo rilascio del plugin.
- `cuelith-site`: piè di pagina, dati strutturati (`license` di `SoftwareApplication`), domande frequenti, test.
- `.github`: accordo di contribuzione, `CONTRIBUTING`, `DEVELOPERS` (sezione «La licenza del tuo plugin»), `TRADEMARK`.
- Firma: la domanda a SignPath citava Apache 2.0; la mail di aggiornamento è in [firma/email-signpath-licenza.md](../firma/email-signpath-licenza.md).

## Dipendenze

Controllate il 2026-10-05 con `pnpm licenses list` sul nucleo: MIT, ISC, BSD, Apache 2.0 e MPL 2.0 (`lightningcss`, solo in fase di build). Tutte compatibili con la GPLv3. L'unica CC-BY (`caniuse-lite`) è una dipendenza di sviluppo e non entra nell'installatore. Da ripetere quando si aggiungono dipendenze.

## Marketplace a pagamento

La [decisione 0008](0008-moduli-a-pagamento.md) prevedeva account e un server licenze con database. Il fondatore vuole nessun account, nessun database e nessun pagamento gestito da lui: l'architettura va riprogettata (rivenditore registrato esterno, licenza come file firmato legato al computer, catalogo su Git, proposta dal sito con approvazione in un angolo protetto). Sarà la **decisione 0013**, dopo che policy e tutela del fondatore sono a posto. Nel frattempo niente di quanto sopra cambia il comportamento del programma.
