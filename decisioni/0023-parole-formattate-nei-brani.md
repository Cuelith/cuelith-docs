# Decisione 0023: personalizzare le parole dei brani, e leggibili da altri programmi

- **Data**: 2026-10-10
- **Stato**: **decisa e fatta su `dev`** (richiesta del fondatore: «personalizzare certe parole del brano, ingrandirle, colorarle, cambiare i bordi, per una proiezione più personale; funziona solo su Cuelith; il brano deve restare leggibile da OpenLP e dagli altri programmi, e viceversa; la grandezza responsive, le frasi mai tagliate»); **non ancora pubblicata**

## Cosa si decide

1. **Cosa si può cambiare in una parola**: dimensione, grassetto, corsivo, colore (decisione 0021) e, in più, **bordo** (sottile, medio, spesso, con un colore) e **ombra** (leggera, forte). Sono scelte pronte, e nei dati ogni valore intermedio è valido (protocollo **1.24**: `outline {width, color}` e `shadow {offset, blur, color}` nell'intervallo, misure in pixel su un'uscita alta 1080 come lo stile del testo).
2. **Responsive e frasi mai tagliate**: già così (decisione 0021): le misure sono relative a quelle dello stile, che scalano con lo schermo; la disposizione è la stessa per il controllo dello spazio e per le uscite, e «Adatta se non entra» rimpicciolisce tutto insieme. Bordo e ombra non cambiano la larghezza delle parole, quindi non spostano le righe: si scalano con lo schermo come il resto.
3. **Il brano resta leggibile da tutti**. Il testo, gli accordi tra quadre, le sezioni e l'ordine sono scritti come li legge qualunque programma. La formattazione viaggia a parte:
   - **«Esporta» normale = pulito**: nessun dato di Cuelith in più; lo stesso file di prima. Se il brano ha parole formattate, un avviso dice che non partono.
   - **«Con la formattazione di Cuelith»** (casella nell'editor, compare solo se il brano ha parole formattate): lo stesso file con in più un blocco che gli altri programmi ignorano. **ChordPro**: una riga `{meta: cuelith_format …}`, come gli altri dati che ChordPro non prevede (ordine, etichette). **OpenLyrics**: un elemento `<cuelith:richtext>` con un nome tutto nostro (spazio dei nomi `https://cuelith.lzrhive.it/ns/richtext/1`).
   - **La copia di sicurezza** di tutti i brani tiene sempre la formattazione.
4. **Importazione ("viceversa")**:
   - un file di Cuelith col blocco riporta le parole formattate; il blocco porta la lunghezza di ogni slide e, se un altro programma ha cambiato il testo nel frattempo, quella slide resta senza formattazione (mai al posto sbagliato); un blocco rovinato o di una versione che non conosciamo si ignora;
   - un file OpenLyrics di altri programmi con tag di formattazione (grassetto, corsivo, colori: `b`/`bold`/`st`, `i`/`it`/`italic`, `r`, `y`, `g`, `bl`, `o`, `pk`, `p`, `w` e i nomi per esteso) diventa parole formattate; i tag che non conosciamo lasciano il testo com'è;
   - un file senza formattazione si importa come prima.
5. **Chi scrive i brani**: dall'editor di Brani con il plugin Formattazione (barra in cima, decisione 0022), che ora ha anche Bordo e Ombra.

## Com'è fatto

- **Protocollo 1.24** (`cuelith-sdk`): `SpanOutlineSchema`, `SpanShadowSchema`, `OUTLINE_PRESETS`, `SHADOW_PRESETS`, `outlinePresetOf`, `shadowPresetOf`; `styleRange` e `richtext.apply` accettano `outline` e `shadow` (o `null` per toglierli).
- **Nucleo** (`cuelith-core`): `layoutRich` porta bordo e ombra di ogni pezzo; le uscite li disegnano per pezzo al posto di quelli dello stile; l'anteprima li applica in CSS; la barra dei testi del nucleo ha Bordo e Ombra.
- **Formattazione 0.3.0**: Bordo (spessore e colore) e Ombra.
- **Brani 0.8.0**: `formats/extension.ts` (il blocco), lettura e scrittura in `chordpro.ts` e `openlyrics.ts`, tag di altri programmi in `openlyrics.ts`, casella «Con la formattazione di Cuelith» e avviso nell'editor.
- **Prove**: protocollo (`rich.test.ts`), Brani (`formatting.test.ts`: esportazione pulita identica a prima, ChordPro e OpenLyrics con andata e ritorno, blocco rovinato / sconosciuto / testo cambiato, copia di sicurezza, tag di altri programmi), e2e (`richtext-plugin.spec.ts`: bordo e ombra dal plugin all'editor e alla slide proiettata).

## Da sapere

- **Non ho OpenLP qui**: ho seguito lo standard (un elemento di uno spazio dei nomi sconosciuto va ignorato; ChordPro ignora i `{meta:}` che non conosce), ma non ho potuto provare come reagisce davvero. Per questo l'«Esporta» normale resta sempre pulito. **Da provare una volta a mano**: esportare un brano «Con la formattazione di Cuelith» in OpenLyrics e aprirlo in OpenLP.
- **I nomi dei tag di OpenLP** (`st`, `it`, `r`, `y`, ...) li ho messi per quel che so della loro convenzione; un nome sbagliato non rovina il testo (si ignora). Se un file reale mostra altri nomi, si aggiungono a una tabella (`FORMAT_TAGS` in `openlyrics.ts`).
- Dentro i file le posizioni delle parole formattate sono quelle del testo scritto nel file (con gli accordi tra quadre); sulla slide proiettata sono ricalcolate senza gli accordi.
- Non c'è ancora la lettura di formattazione dentro il testo ChordPro (`<b>`, `<i>`): ChordPro non ne definisce una standard.
