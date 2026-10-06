# Decisione 0014: il marketplace senza commissione

- **Data**: 2026-10-06
- **Stato**: decisa dal fondatore; fase 1 fatta nel codice (ramo `dev` del sito, non pubblicata)
- **Sostituisce in parte** la [decisione 0013](0013-marketplace-a-pagamento-senza-account.md): commissione, affiliazione e anti-dirottamento non ci sono più. Restano tutto il resto (Notaio, licenze, plugin ritirati, firme).

## Perché

Il modello con il 10% di affiliazione richiedeva un parere legale e fiscale a pagamento (persona fisica senza partita IVA, commissioni da un fornitore estero, clausole anti-dirottamento, obblighi di identificazione). Il fondatore vuole **costo zero** e accetta il rischio residuo: i ricavi attesi sono di poche centinaia di euro l'anno e, se un domani crescono, aprirà la partita IVA forfettaria. Senza commissione l'argomento fiscale sul marketplace sparisce e la clausola anti-dirottamento non ha più ragione di esistere.

## Cosa si decide

1. **Pubblicare è gratuito**, per plugin gratuiti e a pagamento. Nessuna commissione, affiliazione o esclusiva. L'autore vende dove vuole.
2. **Il guadagno del fondatore** non viene dal marketplace: viene da plugin suoi a pagamento (come qualsiasi autore, tramite Lemon Squeezy) e dalle donazioni. Attenzione: vendere un prodotto proprio in modo continuativo avvicina all'«abitualità» fiscale più di una commissione sporadica: da valutare con un commercialista quando le vendite diventano regolari.
3. **La porta resta aperta**: le condizioni (art. 2) dicono che in futuro possono esserci commissioni, solo per i plugin proposti dopo la modifica e con 15 giorni di preavviso.
4. **Il software è un contenitore con connettori**: il protocollo, l'SDK, i pannelli e un processo per plugin sono già l'architettura. Va stabilizzata con regole scritte (compatibilità del protocollo 1.x solo in aggiunta, deprecazioni con preavviso) e con una suite di controlli (fase 2).
5. **Regole di condotta** (art. 4 delle condizioni): diritti, niente malware, dati e permessi onesti, compatibilità, trasparenza sul prezzo e su dove si compra, contenuti leciti, sicurezza, supporto dell'autore.
6. **Verifica che l'autore sia attivo**: l'email dell'autore serve anche per questo. Controlli oggettivi automatici (pacchetto raggiungibile e invariato) e una conferma periodica per email con un clic. Dopo tre richiami senza risposta il plugin diventa **dormiente** e l'autore riceve un **quarto messaggio, definitivo**, che spiega cosa è successo e come risolvere. Il copyright resta dell'autore: non si «perdono i diritti», il plugin si toglie dal catalogo.
7. **Chi risponde di un problema**: l'**autore** risponde del proprio plugin (contenuto, supporto, malware, diritti). Il **Gestore** risponde di far funzionare il catalogo: revisione, ascolto delle segnalazioni, rimozione tempestiva. Superare i controlli **non** è una garanzia (art. 5): si dice «ha superato i controlli tecnici di compatibilità», mai «sicuro». Il Gestore non risponde dei danni causati da plugin di terzi nei limiti dell'art. 1229 c.c.

## Stati di un plugin

| Stato | Quando | Effetto | Comunicazione |
| --- | --- | --- | --- |
| attivo | normale | installabile, acquistabile | — |
| dormiente | l'autore non risponde a tre richiami | non installabile né acquistabile; chi lo ha, lo usa; le licenze si rinnovano | quarto messaggio definitivo; riattivabile con una conferma |
| non conforme | violazione delle condizioni | 14 giorni per correggere, poi tolto dal catalogo (subito per malware) | avviso con i fatti, poi decisione motivata |
| obsoleto | incompatibile con la versione attuale del programma (controllo automatico) | segnalato; non proposto per quella versione | all'autore, con cosa correggere |
| ritirato | lo toglie l'autore, o il Gestore dopo una violazione | esce dagli elenchi; le licenze già vendute continuano (rinnovo e spostamento) | motivata se decisa dal Gestore |

La rimozione dal catalogo **non spegne** il plugin già installato sul computer di un utente: oggi non esiste un blocco da remoto. Va detto agli utenti; se serve, si costruisce (lista di blocco), ma è una scelta da pesare.

## Cosa è già fatto (sito, `dev`, 113 prove)

- Condizioni **v2** in 12 articoli, italiano e inglese, in `/marketplace/condizioni/` (le verifiche periodiche sono scritte come **facoltà**, perché ancora non esistono: nessuna funzione finta).
- **Informativa** `/privacy/` (8 punti), con l'indirizzo di contatto letto da `CONTACT_EMAIL` al momento della costruzione (non sta nel codice, che è pubblico; con `--production` la costruzione si ferma se manca).
- **Due caselle** nel modulo: accettazione e approvazione specifica degli artt. 2, 5, 6, 8 e 11 (ex artt. 1341-1342 c.c.).
- **Traccia dell'accettazione** (plugin, email, versione, data; **senza IP**) in KV, senza scadenza automatica; «Dimentica» segna la data di uscita da cui contano i 10 anni.
- Niente più affiliazione né percentuale nel modulo, nella scheda, nel pannello; la scheda dice «Prodotto da [Autore]. Venduto da Lemon Squeezy (Merchant of Record)».

## Piano a costo zero

1. **Fatto nel codice**: condizioni v2, informativa, caselle, traccia (qui sopra). Manca: creare l'indirizzo per segnalazioni (Email Routing), impostare `CONTACT_EMAIL`, pubblicare.
2. **Suite di compatibilità** (controlli nel registro con GitHub Actions, gratuito per i repository pubblici): statica (manifest, versioni, permessi, firma, icona, pannelli senza risorse esterne), poi **dinamica** (il plugin si installa e parte con il motore vero, risponde entro 5 secondi, nessuna rete non dichiarata, risorse misurate) e comando `pnpm check` per gli autori.
3. **Matrice a ogni release** del programma: tutti i plugin del catalogo riprovati; marcato «compatibile con X» o «obsoleto».
4. **Verifica dell'autore**: controlli settimanali di pacchetto e impronta, conferma per email con un clic (o risposta firmata con la chiave d'autore), stato dormiente. Serve un invio di email gratuito (limiti da verificare) e una pianificazione gratuita (Cloudflare o GitHub).
5. **Kit per gli autori e connettori**: modello di plugin, `pnpm check`, GitHub Action pronta, elenco dei connettori con livello (sperimentale o stabile), politica di deprecazione di 12 mesi.
6. **Se serve**: soglie per aprire la partita IVA; in futuro, la commissione (versione 3 delle condizioni).

## Forum per gli utenti

Un forum si può aggiungere **senza costi** con **GitHub Discussions** (sull'organizzazione, categorie Supporto, Plugin, Idee): niente server, e il codice di conduzione vale anche lì. Un forum proprio (Discourse, NodeBB) richiede un server e un budget; un server Discord è gratuito ma è una piattaforma esterna e senza archivio pubblico. Un forum è anche un **canale di segnalazione** in più, quindi va moderato. Prima del forum: l'email per le segnalazioni (già prevista) e il campo «supporto» obbligatorio per ogni plugin di terzi (l'autore dice dove si chiede aiuto).

## Rischi accettati dal fondatore

Nessun parere legale: plugin a pagamento di terzi (consumatori, segnalazioni, responsabilità), obbligo di identificazione (art. 7 D.Lgs. 70/2003: non si indica nome né domicilio; l'obbligo vale per i servizi «normalmente prestati dietro retribuzione», dubbio da chiarire), informativa non rivista, nessuna struttura giuridica. Il pacchetto per un avvocato (nota, bozza delle condizioni) è in `documenti-riservati/` (fuori dai repository). Va **aggiornato** al modello senza commissione prima dell'invio.

## Cosa non cambia

Notaio, licenze firmate (3 posti, 30/90 giorni), plugin ritirati, chiavi d'autore, procedura di approvazione con un solo clic, mai fermare una diretta.
