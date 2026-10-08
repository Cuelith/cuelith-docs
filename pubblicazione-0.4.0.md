# Pubblicazione di Cuelith 0.4.0: piano passo per passo

Preparato il **2026-10-08**. Niente di questo è stato eseguito: è l'elenco di cosa fare, nell'ordine giusto, con le condizioni per passare al passo dopo. Si va avanti solo se il passo prima è verde. Le regole generali stanno in [STATO.md](STATO.md) («Come si rilascia»).

## Cosa si pubblica

| Cosa | Da | A | Dove |
| --- | --- | --- | --- |
| Protocollo | 1.15.0 (npm) | **1.19.0** | `cuelith-sdk`, pacchetto npm `@cuelith/protocol` |
| SDK, pannelli, interfaccia | 0.7.0 | **0.8.0** | `cuelith-sdk`, npm `@cuelith/sdk`, `panel`, `ui` |
| Lingue | it 0.2.5 / en 0.1.5 pubblicate | **it 0.2.6 / en 0.1.6** | `plugin-locale-it`, `plugin-locale-en` |
| Brani | 0.5.2 | **0.6.0** | `plugin-songs` + voce nel registro |
| Registro | | voce 0.6.0; nasce `extras.json` | `cuelith-registry` |
| Programma | 0.3.3 | **0.4.0** | `cuelith-core` (installatori Windows e Linux) |
| Sito | 0.5.1 | **0.6.0** | `cuelith-site` → Cloudflare Pages |
| Guide e documenti | | | `cuelith-community` (`DEVELOPERS`, sezioni 10 e 11), `cuelith-docs` |

**Perché in quest'ordine**: la release del programma costruisce l'installatore prendendo SDK e lingue dal ramo `main` dei loro repo (`release.yml`): vanno prima su `main`. Il registro va dopo Brani 0.6.0, perché ne scrive l'impronta del pacchetto **pubblicato**. Il sito va per ultimo, perché descrive funzioni che prima non si possono scaricare.

## Prima di cominciare (a mano)

- [ ] Provare `installatori-prova/Cuelith-Setup-0.4.0.exe` sul computer dove Cuelith è già installato: scelta della lingua e prima pagina «Aggiornamento di Cuelith» (la grafica non l'ho potuta vedere).
- [ ] Provare il programma: Brani dal marketplace non c'è ancora (si installa da file), colonna destra, Ctrl+K, impostazioni dei plugin, stili del testo con bordo e ombra.
- [ ] Decidere se pubblicare adesso o provare ancora (lo show salvato con la 0.4.0 si apre con errore in una 0.3.x: è nelle note di versione).

Per ogni repo, prima di ogni passo: `git status` (cosa cambia), controlli verdi (`pnpm check`, e per il nucleo anche `pnpm e2e`), `pnpm exec prettier --check .`.

## 1. `cuelith-sdk` (protocollo 1.19, pacchetti 0.8.0)

1. Alzare le versioni di `packages/sdk`, `panel`, `ui` a **0.8.0** (il protocollo è già 1.19.0 in `packages/protocol`).
2. `pnpm install && pnpm build && pnpm exec turbo run typecheck lint test --force`.
3. Commit su `dev`, poi `git push origin dev:main`, tag annotato `v0.8.0` su `dev`, `git push origin v0.8.0`.
4. **npm** (vedi «Come si fa npm» qui sotto): `pnpm -r publish --access public --no-git-checks`.
5. Verifica: `npm view @cuelith/protocol version` deve dire `1.19.0`; in una cartella pulita `npm install @cuelith/protocol@1.19.0 @cuelith/sdk@0.8.0` riesce.
6. `plugin-template` e la CI dei plugin usano npm: ricostruire il modello (`pnpm install`, `pnpm build`, `pnpm check`) e, se serve, rilasciare un suo 0.3.0.

## 2. Lingue

- `plugin-locale-it`: commit, `git push origin dev:main`, tag `v0.2.6`.
- `plugin-locale-en`: commit, `git push origin dev:main`, tag `v0.1.6`.
- Servono su `main` **prima** del nucleo (l'installatore le include da lì).

## 3. Brani 0.6.0

1. In `plugin-songs`: `pnpm check` e `pnpm build` (stampa impronta e dimensione del pacchetto locale: `cuelith.songs-0.6.0.cpkg`, ~175 KB).
2. Commit, `git push origin dev:main`, tag `v0.6.0`, `git push origin v0.6.0`: la CI pubblica il pacchetto nella release del repo.
3. **Scaricare il pacchetto pubblicato** e calcolarne `sha256` e dimensione: sono quelli che vanno nel registro (non quelli del pacchetto locale: possono differire).

## 4. Registro

1. `plugins/cuelith.songs.json`: aggiungere in **cima** a `versions` la voce 0.6.0 (`version`, `engines` come 0.5.2, `url` della release `v0.6.0`, `sha256`, `size` del pacchetto pubblicato, `permissions: []`, `published` = ora). Facoltativo: `guide` della voce e una copertina `plugins/cuelith.songs.png` (+ campo `image` nel manifest di Brani, e quindi un'altra versione) per mostrare Brani con immagine e «Come si usa».
2. `pnpm validate` (con la rete: scarica e controlla il pacchetto).
3. Commit, `git push origin dev:main`: la CI pubblica `index.json`, `index-2.json`, `support.json`, **`extras.json`** e `licenses.json`.
4. Verifica: aprire `https://cuelith.github.io/cuelith-registry/index-2.json` (c'è Brani 0.6.0) e `extras.json`.
5. Matrice di compatibilità: `gh workflow run conformance.yml -R Cuelith/cuelith-registry -f core=0.4.0` (dopo il passo 7).

## 5. Guide e documenti

- `cuelith-community` (repo `.github`, ramo `main`): commit e push delle due guide per chi sviluppa.
- `cuelith-docs`: commit, `git push origin dev:main`.

## 6. Nucleo 0.4.0

1. `versioni/0.4.0.md` è già scritto (italiano, riga `---`, inglese); la versione in `apps/desktop/package.json` è 0.4.0.
2. `pnpm install && pnpm build && pnpm check && pnpm exec prettier --check . && pnpm e2e` (tutte verdi: l'ultima corsa 39 prove su 39).
3. Commit su `dev`, `git push origin dev:main`, tag annotato `v0.4.0`, `git push origin v0.4.0`.
4. `release.yml` crea la release **in bozza** con le note, costruisce gli installatori (Windows `Cuelith-Setup-0.4.0.exe`, Linux AppImage) e la rende pubblica solo quando ci sono tutti. Se qualcosa non va resta in bozza e nessuno la vede.
5. Verifica dopo il rilascio: scaricare l'installatore dal sito, confrontarlo con `latest.yml`, installarlo in silenzio (`/S`) **in una cartella di prova**, far girare le prove principali con `CUELITH_E2E_EXECUTABLE`, controllare gli aggiornamenti, disinstallare.
6. **Non** installarlo sopra il Cuelith del fondatore per le prove: per quello c'è la prova a mano (sezione «Prima di cominciare»).

## 7. Sito 0.6.0

1. Alzare `cuelith-site/package.json` a 0.6.0; `pnpm test`; `node build.mjs`.
2. Le schermate sono già rifatte; se la grafica del programma cambia ancora: `pnpm shots` (dopo `pnpm build` e `pnpm -C e2e run site:shots` nel nucleo).
3. Commit, `git push origin dev:main`, tag `v0.6.0`, poi `pnpm exec wrangler pages deploy dist --project-name cuelith --branch main`.
4. Verifica: `https://cuelith.lzrhive.it` (italiano e inglese), pagina Download con la 0.4.0, una scheda di plugin, il telefono in verticale (400 px).

## Dopo

- Aggiornare `STATO.md` (versioni pubbliche, cosa è fatto) e la decisione 0018/0017 se serve.
- Mail a SignPath (firma dell'installatore): ancora in sospeso, non dipende da questa release.
- Se qualcosa va storto: la release del nucleo resta in bozza (non si pubblica); il sito si riporta alla distribuzione precedente da Cloudflare; **un pacchetto npm pubblicato non si ritira dopo 72 ore**, quindi il passo 1 si fa solo quando il protocollo è davvero quello giusto (adesso lo è: 120 prove verdi).

## Come si fa npm (cosa serve da te)

Su questo computer **non c'è un accesso a npm** (`npm whoami` risponde «401 Unauthorized»), e pubblicare richiede l'account `cuelith` con la verifica in due passaggi. Due modi, il primo è consigliato perché nessun codice passa dalla chat:

1. **Accedi tu, una volta.** Nel terminale di questo computer: `npm login` (si apre il browser; entri con l'account `cuelith` e confermi con l'app di autenticazione). Le credenziali restano nel file di npm di questo computer: io non vedo la password.
2. **Pubblica tu, con il comando che ti do io.** Quando arriviamo al passo 1.4 ti scrivo il comando esatto (`pnpm -r publish --access public --no-git-checks`) e lo lanci tu nel terminale (se la tua finestra lo permette, anche scrivendo `!` davanti nel messaggio): npm ti chiede il codice a sei cifre **nel tuo terminale**. Il codice non entra mai nella chat.

In alternativa, se preferisci che lo faccia io: dopo `npm login`, al momento della pubblicazione mi scrivi il codice a sei cifre dell'app (vale 30 secondi e una sola volta) e io lancio subito il comando. Va bene anche questo, perché scade da solo; non mandarmi mai la password né un token di accesso.
