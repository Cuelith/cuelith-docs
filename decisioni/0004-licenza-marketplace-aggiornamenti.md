# 0004 · Licenza, marketplace, aggiornamenti, librerie organizzate

Stato: **Deciso** (fondatore, 2026-09-30).

## Licenza e sostenibilità: strada A

- Il **nucleo resta open source (Apache 2.0) per sempre**. Il codice pubblicato con una licenza libera non si puo' "ritirare": chiunque abbia una versione puo' usarla e ridistribuirla. Un blocco a distanza di installazioni esistenti non si fa (fiducia della community, tutela dei consumatori in UE).
- Un'eventuale sostenibilita' economica futura passera' da **servizi e moduli** (es. sincronizzazione cloud, moduli professionali, assistenza) controllati dal server al momento del download o dell'attivazione: cio' che e' a pagamento lo e' per tutti.
- **Oggi nulla e' a pagamento**, moduli compresi (conferma della decisione del cap. 29: solo moduli gratuiti).
- **CLA (accordo per i contributori)** da introdurre prima di accettare codice esterno, per non precludere scelte future.
- **Si prepara subito** (senza lock-in, rispettando la privacy): ID di installazione anonimo generato in locale, sezione "Licenza" nell'app con "Edizione community, gratuita", canale verso il server per funzioni future. Tutto dichiarato in un'informativa e disattivabile. Nota: non e' un parere legale; prima di qualsiasi scelta a pagamento serve un avvocato.

## Marketplace dei moduli (cap. 13, 24, 26)

- Ogni modulo nel suo repository GitHub; versioni come file `.cpkg` nelle GitHub Releases.
- `cuelith-registry` = indice pubblicato su GitHub Pages (costo zero): id, versioni, URL, impronta SHA-256, permessi, compatibilita'.
- L'app legge l'indice in background: **Marketplace** (descrizione, versione, permessi) → **Scarica** → verifica dell'impronta → installazione nella cartella dati (mai dentro l'app).
- **Moduli installati**: attiva/disattiva senza riavvio, aggiorna, disinstalla, tutorial iniziale e documentazione.
- Offline: funzionano i moduli gia' installati.

## Aggiornamenti dell'app

- Controllo delle nuove versioni su GitHub Releases (tag SemVer su `main`), download in background.
- **Mai installazione automatica e mai durante una diretta** (uscite in onda): si chiede all'operatore.
- Windows senza certificato di firma (avviso SmartScreen al primo avvio, costo zero); macOS rimandato (richiede Apple Developer, 99 $/anno).

### Come è fatto (2026-10-01)

- Installatori con electron-builder: Windows NSIS per l'utente (niente amministratore, cartella a scelta), Linux AppImage; app di circa 11 MB, senza `node_modules`.
- Release: un tag `vX.Y.Z` su `main` di `cuelith-core` crea gli installatori e li pubblica nella release di GitHub.
- Aggiornamenti con electron-updater:
  - controllo 30 s dopo l'avvio e poi ogni 6 ore; il controllo automatico si può spegnere;
  - download in background, con un avviso e un puntino sull'ingranaggio quando l'aggiornamento è pronto;
  - **Installa e riavvia** in Impostazioni → Informazioni: rifiutato se si è **in onda** (una slide in programma e almeno un'uscita non in nero, `isOnAir` nel protocollo);
  - prima di installare si chiede se salvare.
- ID di installazione anonimo (UUID) generato sul computer, mostrato e rigenerabile in Informazioni, **mai inviato**. Accanto c'è l'informativa: Cuelith va in rete solo per aggiornamenti e indice dei moduli.
- Il "canale verso il server per funzioni future" non esiste ancora: si farà quando ci sarà un servizio vero, con consenso esplicito.

## Librerie organizzate (tante librerie, tanti innari)

- Ogni libreria ha una **categoria** (Canti, Innari, Letture, Avvisi… scelte dall'utente), una **sigla** facoltativa e unica (es. `INN`), un colore, e puo' essere **preferita**.
- **Selettore con ricerca** al posto della tendina: Preferite, Recenti (per postazione), poi per categoria.
- Ricerca rapida per sigla e numero: `INN 245` va al brano 245 di quell'innario; `INN luce` cerca solo in quell'innario.
- Nella ricerca in tutto l'archivio ogni risultato mostra in quali librerie si trova (con sigla e numero).

## Ordine dei lavori

1. Librerie organizzate. 2. Gestore moduli con marketplace e registry. 3. Modulo Canti. 4. Installatori, aggiornamenti, ID di installazione, sezione Licenza. 5. Resto della Fase 0: sfondi, test lingue, postazioni in rete.
