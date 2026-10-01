# 0009 · Postazioni in rete locale

Stato: **Deciso** (passo 8 della Fase 0, 2026-10-01). Realizza i capitoli 9, 23 e 27 del documento. Protocollo 1.10.0.

## Come funziona

- **Di base il motore ascolta solo su questo computer** (`127.0.0.1`). L'ascolto in rete si accende da Impostazioni → **Rete e postazioni**, e solo sull'indirizzo di rete scelto, mai su tutti. La scelta resta tra un avvio e l'altro; se quella rete non c'è, Cuelith riprova da solo ogni 10 secondi.
- **Le altre postazioni usano il browser**: tablet, telefoni e computer sulla stessa rete aprono l'indirizzo mostrato (anche come codice QR). Non si installa nulla e non serve internet. La porta in rete è fissa, `7420`.
- **Abbinamento con codice a 6 cifre**:
  1. sul computer del motore si sceglie il ruolo e si preme «Mostra codice»;
  2. sull'altra postazione si scrivono codice e nome;
  3. la postazione riceve un token suo, che resta in quel browser.

  Il codice vale 5 minuti, una volta sola, e si brucia dopo 5 tentativi sbagliati.
- **Token revocabili**: l'elenco delle postazioni abbinate mostra chi è collegato. «Revoca» scollega subito la postazione e il suo token smette di valere. Sul disco del motore resta solo l'impronta (SHA-256) dei token.
- **Annuncio in rete**: il motore si annuncia come `_cuelith._tcp` (mDNS), per i programmi che in futuro vorranno trovarlo da soli. Ai browser serve comunque l'indirizzo.

## Ruoli

| Ruolo | Cosa vede e fa |
| --- | --- |
| **Operatore** | la postazione completa: scaletta, slide, librerie, nero e blocco delle uscite. Non gestisce moduli né la configurazione delle uscite: quei pulsanti non compaiono |
| **Telecomando** | una schermata semplice, pensata per il telefono: slide in onda, prossima slide, «Indietro» e «Avanti» |
| **Visualizzatore** | la stessa schermata, senza pulsanti |
| **Regia** | come la postazione del motore, tranne ciò che è riservato a quel computer |

**Riservato al computer del motore**, qualunque sia il ruolo:

- aprire o chiudere la rete;
- abbinare e revocare postazioni;
- tutto ciò che tocca i file di quel computer (aprire e salvare show, importare media, installare moduli da file).

## Sicurezza

- Senza abbinamento una postazione può solo leggere i testi dell'interfaccia (servono alla schermata del codice): niente show, niente comandi.
- L'ascolto in rete accetta solo richieste rivolte al proprio indirizzo (controllo dell'intestazione `Host`). Così una pagina web qualunque non può far puntare il browser di qualcuno al motore con un altro nome.
- Spegnere la rete scollega subito tutte le postazioni collegate da lì.
- **Limite dichiarato**: il collegamento è `http`/`ws`, non cifrato. Va usato su una rete di cui ci si fida, e l'interfaccia lo dice. Il cifrato (`https`) richiede certificati e si valuterà più avanti.

## Rimandato

- **File dalle postazioni in rete**: caricare immagini e basi musicali dal tablet (oggi si importano solo dal computer del motore).
- **Proprietà di uscite e layer per ruolo** (errore 4090 del cap. 23): chi fa la diretta non può oscurare il proiettore. Arriva con la regia della diretta (Fase 1).
- **Ruoli personalizzati.**
