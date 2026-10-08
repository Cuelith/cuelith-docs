# Decisione 0015: stili del testo, globali e dell'editor

- **Data**: 2026-10-07
- **Stato**: decisa dal fondatore; **fasi 1–3 fatte nel ramo `dev`** (core 0.4.0, protocollo 1.16: campi facoltativi, calcolo e controllo dello spazio, riga degli stili sotto gli sfondi, stile dell'editor, avviso mentre si scrive); prova nell'app del fondatore da fare; fase 4 (formattazione dentro il testo) da fare

## Il problema

Oggi esiste un solo stile di testo, dentro l'aspetto «Sala»: carattere, dimensione, colore, allineamento, margine. Serve (1) poter cambiare in un attimo come appare il testo, per tutto ciò che si proietta, e (2) poter personalizzare un singolo testo mentre lo si scrive. Il testo proiettato deve **sempre** essere ben visibile, su qualunque schermo.

## Cosa si decide

1. **Stili globali, creati dall'utente.** Non ci sono stili preimpostati: la riga parte vuota, con «+» per crearne uno dallo stato attuale. Stanno nell'archivio del computer, come gli sfondi, e si scelgono **in qualunque momento**, in anteprima e in diretta, in una riga **subito sotto gli sfondi**. Valgono per l'aspetto selezionato (Sala, Streaming, Palco), come gli sfondi.
2. **Stile dell'editor, per un solo testo.** Chi scrive personalizza il testo di quell'elemento, per qualsiasi tipo di elemento, non solo i brani. La dimensione dell'editor è una **scala** (es. 120%) e non un numero assoluto; colore, grassetto, allineamento sono valori.
3. **Lo stile globale, quando è selezionato, sostituisce del tutto quello dell'editor.** Se lo si toglie, torna lo stile dell'editor (che era rimasto salvato, solo sospeso). Due soli stati: nessuna miscela da indovinare. L'editor lo dice («stile globale attivo: le tue modifiche sono sospese») e l'anteprima mostra il risultato vero.
4. **Controllo dello spazio, vincolante.** Prima di applicare uno stile si misura ogni slide come la vedrebbero le uscite (righe a capo, altezza e larghezza, margini). Deve entrare su **tutte le uscite attive** (vale il caso peggiore, con il motivo detto). Per ciò che è **in onda, in anteprima o nell'elemento corrente** il blocco è vero: lo stile non si può scegliere e non cambia mai ciò che il pubblico guarda. Per il resto della scaletta c'è solo un avviso ⚠ con l'elenco delle slide a rischio.
5. **Righe intere** (aggiunta del 2026-10-07): con l'adattamento acceso le righe del testo non vanno mai a capo da sole; si rimpicciolisce fino a far entrare la riga più lunga. Se neppure al minimo dello stile entra, il disegno scende ancora (fino al 10%): in diretta è meglio un testo piccolo che una riga spezzata, mentre il controllo continua a segnalare «non entra». Senza adattamento si va a capo come prima.
6. **Bordo e ombra** (2026-10-07): campi facoltativi `outline` (spessore, colore) e `shadow` (distanza, sfumatura, colore) negli stili globali, in pixel su un'uscita alta 1080.
7. **Adattamento automatico**, opzione di ogni stile, **acceso per i nuovi stili**: se una slide non entra, il testo si rimpicciolisce fino a un minimo (60% della dimensione dello stile). Il rimpicciolimento è **uguale per tutte le slide dello stesso elemento** (la più lunga decide), così le strofe non cambiano grandezza una dopo l'altra. Lo stile si blocca solo se anche al minimo non entra.
8. **Formattazione dentro il testo (fase 2, «come Word»)**: grassetto e corsivo di singole parole sono contenuto e restano anche con uno stile globale; dimensione e colore di singole parole cedono allo stile globale. Tocca importazione ed esportazione (OpenLyrics, ChordPro): va fatta a parte.

## Come si realizza senza rompere nulla

- **Solo campi opzionali.** Lo stile del testo (`text` negli aspetti) guadagna campi facoltativi: interlinea, peso, ombra o contorno, maiuscolo, adattamento. Senza valore tutto resta identico a oggi. Ogni elemento guadagna un campo facoltativo `textStyle` (parziale: solo ciò che l'utente ha toccato). Il protocollo cresce in modo additivo (1.16).
- **Compatibilità**: gli show salvati oggi si aprono uguali. Uno show salvato con la nuova versione e aperto in una vecchia (0.3.x) può dare errore sul campo nuovo: lo si scrive nelle note di versione.
- **Lo show porta con sé una copia dello stile globale usato**, così su un altro computer il testo si vede identico anche se lì quello stile non esiste.
- **Un solo punto di calcolo**: lo «stile effettivo» (globale se attivo, altrimenti dell'editor, altrimenti l'aspetto) e la misura dello spazio vivono nello stesso codice del disegno delle uscite, così quello che l'anteprima promette è ciò che l'uscita fa.
- **Prove**: l'effettivo per ogni combinazione (nessuno, solo editor, solo globale, entrambi); la misura su schermi 16:9, 4:3, ultra-larghi e verticali; un testo lungo che non entra; il passaggio di stile mentre una slide è in onda (non deve mai cambiare se non entra); gli show vecchi che si aprono uguali.

## Fasi

1. **Protocollo e calcolo**: campi opzionali, stile effettivo, misura dello spazio, adattamento; prove.
2. **Riga degli stili sotto gli sfondi**: crea, scegli, modifica, elimina; blocco e avvisi.
3. **Stile dell'editor** per elemento (finestra di modifica) con l'indicazione «sospeso».
4. **Formattazione dentro il testo** (fase 2 del testo, decisione a parte quando si arriva).

## Da decidere più avanti

- Il nome e i controlli esatti degli stili (cosa si può impostare) e dove stanno nella finestra di modifica.
- Se uno stile può essere condiviso tra più computer (esportazione), come le impostazioni degli sfondi.
