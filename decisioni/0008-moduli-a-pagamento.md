# 0008 · Moduli a pagamento: architettura (da costruire più avanti)

Stato: **Proposta di architettura** (2026-10-01). Oggi nulla è a pagamento (decisione 0004). Questo documento fissa la logica in anticipo, così le parti costruite adesso (ID di installazione, registry, pacchetti verificati, processi dei moduli) non vanno rifatte. Prima di vendere qualsiasi cosa serve il parere di un avvocato.

## Il punto di partenza: il nucleo è open source

Il nucleo è Apache 2.0: chiunque può leggerlo, modificarlo e ridistribuirlo, compreso togliere un controllo di licenza. Ne seguono tre regole:

1. **La sicurezza sta nelle chiavi, non nel segreto del codice.** Il formato delle licenze e il codice che le verifica sono pubblici. Senza la chiave privata del server nessuno può fabbricare una licenza valida.
2. **Il controllo non vive solo nel nucleo.** Un nucleo modificato può saltare i controlli, quindi la protezione sta in tre posti:
   - nel **server**, che consegna il pacchetto solo a chi ha la licenza;
   - nel **modulo stesso**, che verifica la propria licenza con l'SDK;
   - nei **servizi online**, quando il modulo ne usa.
3. **Nessuna protezione è assoluta.** Un modulo in JavaScript si può leggere e modificare. Lo scopo realistico è triplice:
   - comprare deve essere più comodo che copiare;
   - chi regala un modulo non deve poterlo fare con un clic;
   - una copia diffusa deve essere riconoscibile.

   Il codice dei moduli a pagamento **non** è Apache: ha la licenza del suo autore (EULA).

## Le parti

| Parte | Dove | Cosa fa |
| --- | --- | --- |
| **Account** | server licenze | identità di chi compra (email con link di accesso, nessuna password da custodire) |
| **Negozio** | servizio di pagamento esterno con ruolo di "venditore registrato" (es. Lemon Squeezy, Paddle) | incassa, gestisce l'IVA di ogni paese UE, emette ricevute, gestisce i rimborsi; avvisa il server licenze a pagamento avvenuto |
| **Server licenze** | Cloudflare Workers + D1 (database) + R2 (file), piano gratuito, su un sottodominio di lzrhive | diritti d'uso (chi possiede cosa), posti per computer, firma delle licenze, consegna dei pacchetti |
| **Registry pubblico** | GitHub Pages (come oggi) | elenca anche i moduli a pagamento (descrizione, prezzo, permessi, risorse dichiarate, autore) ma **senza** indirizzo pubblico del pacchetto |
| **Nucleo** | app | acquisto, attivazione, disattivazione, verifica, avvisi; mai blocchi durante una diretta |
| **SDK** | dentro il modulo | `ctx.license`: il modulo verifica da sé la propria licenza |

## Il computer: un "posto" che non si copia

- Al primo avvio (oggi già fatto) Cuelith crea l'**ID di installazione**. Per i moduli a pagamento si aggiunge una **coppia di chiavi del dispositivo** (Ed25519), generata sul computer.
- La chiave privata è cifrata con la protezione del sistema operativo (`safeStorage` di Electron: DPAPI su Windows, Portachiavi su macOS, libsecret su Linux).
- **Copiare la cartella dell'app su un altro PC non porta con sé il posto**: là la chiave non si decifra, quindi quel computer deve attivarsi da sé.

## La licenza: un permesso firmato, verificabile senza internet

Il server firma con la sua chiave privata (Ed25519) un permesso che contiene:

- account (un identificativo, non l'email);
- modulo;
- versioni coperte (es. "aggiornamenti fino al 2027-10-01");
- **impronta della chiave del dispositivo**;
- data di emissione;
- "rinnova dopo" (30 giorni);
- "tolleranza fino a" (90 giorni).

Come si verifica:

- **Il nucleo** verifica il permesso con le chiavi pubbliche del server, incluse nell'app, con un elenco che ne permette la rotazione. In più chiede al dispositivo di firmare una sfida con la sua chiave privata: un permesso copiato su un altro PC non basta.
- **Il modulo** fa lo stesso controllo con `ctx.license.verify()` dell'SDK. Così un nucleo modificato che salta il controllo non basta a usare il modulo; servirebbe modificare anche il modulo (vedi "Cosa non si può impedire").
- **Senza internet funziona**: si va in rete solo per rinnovare, in background, quando c'è connessione.

## Regola d'oro: mai fermare una diretta

- La licenza si controlla **all'attivazione del modulo**, mai mentre lavora.
- Se il permesso scade durante uno show, il modulo continua fino alla fine; l'operatore vede solo un avviso discreto.
- Dopo la tolleranza (90 giorni senza mai collegarsi) il modulo all'avvio successivo resta in stato "licenza da rinnovare". Se in quel momento si è in onda (`isOnAir`), il controllo si rimanda.
- Server spento o irraggiungibile: valgono i permessi già scaricati. Il servizio non è mai un punto unico di guasto per lo spettacolo.

## Comprare, cambiare PC, riavere i posti

- **Acquisto**:
  1. dal marketplace, "Acquista" apre il negozio nel browser;
  2. pagato, il server registra il diritto d'uso;
  3. l'app lo vede (accesso con l'account) e scarica il pacchetto con un indirizzo temporaneo firmato.
- **Posti**: ogni licenza vale per un numero di computer (proposta: **2**, regia e portatile).
- **Cambio PC**:
  - dal vecchio: Impostazioni → Licenze → "Disattiva questo computer": il posto si libera subito;
  - PC rotto o rubato: dalla pagina dell'account si libera il posto **da remoto**, in autonomia, fino a 3 volte l'anno; oltre, si chiede all'assistenza. Così si cambia computer senza ricomprare, ma non si gira la licenza a decine di persone.
- **Rimborsi e revoche**: il server smette di rinnovare. I permessi già emessi scadono da soli: niente "interruttore a distanza" che spenga un'installazione in diretta.

## Contro chi regala il modulo

- **Niente pacchetto pubblico**: il pacchetto si scarica solo con un account che ha il diritto d'uso.
- **Copia marcata**: ogni pacchetto scaricato contiene un file firmato con l'identificativo della licenza. Se gira in rete, si sa da quale licenza viene e si può revocare.
- **Posto legato al dispositivo**: anche con il pacchetto, senza un permesso per il proprio dispositivo il modulo non parte, sia nel nucleo ufficiale sia per il controllo interno al modulo.
- **Valore online**: dove ha senso (sincronizzazione, contenuti aggiornati, servizi), una parte del valore sta sul server, che è al sicuro per natura.

## Cosa non si può impedire (e va bene così)

Chi modifica sia il nucleo sia il codice di un modulo JavaScript può eliminare i controlli. Lo si rende scomodo, non impossibile:

- i moduli nativi sono più difficili da modificare;
- un modulo modificato non riceve aggiornamenti;
- il marchio **Cuelith** (da registrare) impedisce a una versione modificata di presentarsi come quella ufficiale.

Chi compra paga soprattutto aggiornamenti, assistenza e comodità.

## Moduli di terzi a pagamento (più avanti)

- Un autore esterno pubblica col suo account autore: chiave di firma dei pacchetti registrata nel registry, prezzo, EULA.
- Il negozio paga la quota all'autore (ripartizione decisa allora).
- La revisione del registry resta: permessi, risorse dichiarate, icona unica, impronta.

## Privacy

- Sul server solo l'indispensabile: email, acquisti, impronte dei dispositivi (non i nomi dei computer).
- L'ID di installazione va al server **solo** quando l'utente attiva un modulo a pagamento, con l'informativa aggiornata.
- Account cancellabile in autonomia.

## Cosa esiste già e cosa si aggiungerà

| Già fatto | Si aggiunge quando si vende |
| --- | --- |
| ID di installazione locale (0004) | chiavi del dispositivo cifrate con `safeStorage` |
| Pacchetti `.cpkg` con impronta SHA-256 verificata (registry) | firma dei pacchetti con la chiave dell'autore; pacchetti marcati per licenza |
| Registry con permessi e compatibilità | campi `pricing` e `access: "licensed"` (senza indirizzo pubblico) |
| Processi dei moduli e SDK (0007) | `ctx.license` nell'SDK, stato "licenza da rinnovare" nel ciclo di vita |
| `isOnAir` (niente interruzioni in onda) | riuso per rimandare ogni controllo di licenza |
| Impostazioni → Informazioni (edizione, ID, informativa) | Impostazioni → Licenze (account, posti, disattiva questo computer) |

I campi nuovi del protocollo e del registry si aggiungono insieme a queste funzioni, non prima: nessuna parte finta nel programma.
