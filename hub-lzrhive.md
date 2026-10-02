# Cuelith sull'hub lzrhive.it

Come aggiungere Cuelith ai progetti dell'hub personale del fondatore (`https://lzrhive.it`). L'hub è anonimo: nei testi non compare il nome della persona, e questa scheda non lo nomina.

## Cosa serve

- Accesso alla console: `https://lzrhive.it/admin/`. Si entra in due tempi: prima Cloudflare Access (arriva un codice via email), poi la password della console e il codice a 6 cifre della verifica in due passaggi.
- L'immagine della scheda: `cuelith-core/brand/lzrhive-card.jpg` (16:9, logo su fondo scuro, senza testo; si rifà con `node scripts/scheda-hub.mjs` in `cuelith-site`).

## I campi della scheda (console → Progetti → Nuovo progetto)

| Campo | Italiano | English |
| --- | --- | --- |
| Titolo | Cuelith | Cuelith |
| Frase breve | Software di proiezione live, gratuito e open source. | Free, open-source live projection software. |
| Descrizione | Porta testi, canzoni e annunci su proiettore, monitor del palco e altri schermi. Un programma leggero che cresce con i plugin che ti servono. Gratuito, open source, in italiano e inglese, per Windows e Linux. | Puts texts, lyrics and announcements on the projector, the stage monitor and other screens. A lightweight app that grows with the plugins you need. Free, open source, in English and Italian, for Windows and Linux. |

I testi si scrivono con le schede delle lingue in cima al pannello (Italiano, English). Campi uguali per tutte le lingue:

| Campo | Valore |
| --- | --- |
| Stato | Online |
| Indirizzo breve | `cuelith` |
| Link del progetto | `https://cuelith.lzrhive.it` |
| Immagine | `lzrhive-card.jpg` (si carica dal selettore, che la ridimensiona e la salva nella libreria Media) |

## Dopo il salvataggio

Aprire `https://lzrhive.it` (e `https://lzrhive.it/en`): la scheda deve comparire nell'elenco, con l'immagine, e il clic deve portare a `cuelith.lzrhive.it`. Il testo «Presto nuovi progetti» sparisce da solo quando c'è almeno un progetto.

## Collegamenti nei due sensi

- Dal sito di Cuelith a lzrhive: piè di pagina («Altri progetti su lzrhive»). Dal sito di Cuelith a Ko-fi: blocco «Sostienici» e piè di pagina. L'indirizzo Ko-fi è `https://ko-fi.com/mlhive`, lo stesso già impostato sull'hub (console → Impostazioni sito → Link donazioni).
- Da lzrhive a Cuelith: la scheda sopra.
