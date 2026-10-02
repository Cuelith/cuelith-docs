# 0010 · Lingua inglese e scelta della lingua

Stato: **Deciso** (2026-10-02, confermato dal fondatore). Protocollo 1.13.0. Rilasciato con Cuelith 0.2.0.

## Perché

Il sito di Cuelith è in italiano e in inglese. Una pagina inglese con schermate in italiano non è una pagina inglese, e le schermate del sito sono sempre il programma vero, mai ritoccate. Per fotografare Cuelith in inglese serviva ciò che finora mancava: la lingua inglese e un modo per sceglierla.

## Come funziona

1. **La lingua si sceglie** in Impostazioni → Generale, tra quelle installate. Vale per tutte le postazioni, si applica subito senza riavviare e resta tra un avvio e l'altro (`settings.json` nella cartella dati). Solo la postazione sul computer del motore può cambiarla.
2. **Italiano e inglese sono inclusi nel programma** (moduli `cuelith.locale.it` e `cuelith.locale.en`, repo `plugin-locale-it` e `plugin-locale-en`). Al primo avvio Cuelith segue la lingua del sistema, se è tra quelle installate; altrimenti l'italiano. Ogni altra lingua resta un modulo del marketplace, come prima.
3. **Un modulo non ancora tradotto** in una lingua mostra i suoi testi nella lingua che ha, non le chiavi.
4. **Ciò che Cuelith crea da solo** (nome di un nuovo show, look «Sala» e «Palco») nasce nella lingua in uso in quel momento e poi è dell'utente: cambiando lingua non cambia nome.
5. Restano uguali in ogni lingua le parole fisse (Verse, Chorus, Bridge…: decisione 0005).

## Protocollo 1.13

- `locale.set { lang }` → `{ active }` (permesso `admin`); `NotFound` se la lingua non è installata.
- `live.lang`: la lingua in uso. Quando cambia, le postazioni rileggono i testi (`locale.catalog`) e li passano ai pannelli dei moduli.

## Prove

- Motore: la lingua si sceglie, arriva nello stato, resta dopo un riavvio e dopo un nuovo show; il modulo non tradotto mostra i suoi testi.
- Lingue: l'inglese ha le stesse chiavi e gli stessi segnaposto dell'italiano (nucleo e modulo Canti).
- Schermate del sito: una serie in italiano e una in inglese, dal programma vero (`e2e/site/shots.spec.ts`); le prove avviano sempre Cuelith in italiano (`CUELITH_LANG`), qualunque sia la lingua del sistema.

## Note di versione in due lingue

Nel testo di ogni versione pubblicata una riga di separazione (`---`) divide l'italiano, che viene prima, dall'inglese. Il sito mostra a ogni pagina la sua lingua; se l'inglese manca, mostra l'italiano.

## Rilascio

Uscito con Cuelith 0.2.0 (2026-10-02): protocollo 1.13 (SDK v0.6.0), italiano 0.2.0, inglese 0.1.0 (repo `plugin-locale-en`, incluso nell'installatore), Canti 0.5.0.

Nella stessa versione i «moduli» diventano **plugin** in tutti i testi del programma, nelle due lingue, come sul sito. Nel codice, nel protocollo e in questi documenti il nome tecnico resta modulo.

I plugin dichiarano la versione del programma con un intervallo esplicito (`>=0.1.0 <1.0.0`): per le versioni 0.x l'accento circonflesso (`^0.1.0`) esclude la 0.2.0. Chi aveva Canti 0.4 lo aggiorna dal marketplace.
