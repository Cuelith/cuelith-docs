# Firma gratuita dell'installatore Windows: domanda a SignPath Foundation

Per chi: il fondatore, che invia la domanda. Le risposte in inglese sono pronte da incollare; chi le legge è il personale di SignPath Foundation.

## Perché

L'installatore di Windows non è firmato: chi lo apre vede «PC protetto da Windows» con autore sconosciuto. SignPath Foundation firma gratis i progetti open source; l'autore mostrato sarà «SignPath Foundation».

## Cosa fare

1. Apri <https://signpath.org/apply> e compila il modulo con le risposte qui sotto.
2. Usa il tuo account GitHub (quello che possiede l'organizzazione Cuelith, con la verifica in due passaggi già attiva).
3. Aspetta la risposta per email: possono volerci giorni o settimane, e possono chiedere chiarimenti. Giramela e preparo la risposta.
4. Quando accettano, ti danno accesso a SignPath: da lì in poi collego io la firma alla procedura di rilascio. Ogni rilascio andrà approvato a mano da te con un clic.

## Risposte pronte (campi del modulo, visti il 2026-10-02)

| Campo | Risposta |
| --- | --- |
| Project Name | Cuelith |
| Repository URL | https://github.com/Cuelith/cuelith-core |
| Homepage URL | https://cuelith.lzrhive.it/en/ |
| Download URL (facoltativo) | https://cuelith.lzrhive.it/en/ |
| Privacy Policy URL | https://github.com/Cuelith/cuelith-core#privacy |
| Wikipedia URL | vuoto |
| Build System | GitHub Actions |
| Company Name | vuoto |
| Caselle in fondo | la prima e la terza (obbligatorie); la seconda, le comunicazioni commerciali, no |

**Tagline**

> Free, open-source live projection software with a small core and a plugin system.

**Description**

> Cuelith is free, open-source software for live events: it shows texts, lyrics and announcements on a projector, a stage monitor and other screens, and lets the operator control what is on air from one desk. It has a small core and a plugin system; plugins run in separate processes with declared permissions, so a faulty plugin never stops the projection. It runs on Windows and Linux, works offline, and is available in English and Italian.

**Reputation**

> Cuelith is a new project: its first public version was released on 1 October 2026, so it has no download statistics or press coverage yet. What can be verified today:
>
> - Development is fully public in the GitHub organisation https://github.com/Cuelith (core, SDK, plugins, registry, docs, website).
> - Two releases so far (0.1.0 and 0.2.0), built entirely by GitHub Actions from tagged source: https://github.com/Cuelith/cuelith-core/releases
> - Every commit runs automated checks, including end-to-end tests that drive the packaged app.
> - Protected branches with mandatory review, two-factor authentication enforced on the organisation, private vulnerability reporting enabled.
> - Public contributor guide, developer guide, security policy and trademark policy: https://github.com/Cuelith/.github
> - Code signing policy and privacy statement: https://github.com/Cuelith/cuelith-core#code-signing-policy

## Due punti deboli, detti chiaramente

- **Reputazione**: chiedono prove che il progetto sia largamente usato o fidato. Cuelith è pubblico dal 1° ottobre 2026 e non ha ancora numeri: il testo dice solo ciò che è vero e verificabile. Possono rispondere di ripresentarsi più avanti; in quel caso si rifà la domanda con i download veri.
- **Download URL**: il modulo dice che quella pagina deve menzionare SignPath Foundation. Non si può scrivere prima dell'accettazione; il campo non è obbligatorio. Se accettano, la frase va aggiunta subito alla sezione Download del sito e al README.

## Se chiedono chiarimenti

**Why we need code signing**

> The Windows installer is downloaded by event operators and volunteers who are not technical. Without a signature they see the "Windows protected your PC" screen with an unknown publisher, and many stop there. The app also updates itself with electron-updater, and a signature lets Windows and the updater verify that an update really comes from our release workflow.

**How releases are built**

> Every release is built from a version tag on the `main` branch by the GitHub Actions workflow in the repository, using only the source code of our public repositories (`cuelith-core`, `cuelith-sdk`, `plugin-locale-it`, `plugin-locale-en`) and their open-source dependencies. `main` and `dev` are protected branches: changes from contributors need a pull request, a passing check and the maintainer's approval. The release is created as a draft and is published only when all installers have been built.

**Artifact to sign**

> The Windows installer `Cuelith-Setup-<version>.exe` (NSIS, per-user, no administrator rights) and the executables inside it. Licence: Apache-2.0 for the core and for every component in the installer.

## Cosa ho già preparato

- La sezione **Code signing policy** e la sezione **Privacy** nel README del nucleo.
- Rami protetti, approvazione obbligatoria, verifica in due passaggi sull'organizzazione.
- La procedura di rilascio costruisce tutto dal codice pubblico, senza passaggi a mano.

## Cosa manca finché non accettano

- La frase obbligatoria «Free code signing provided by SignPath.io, certificate by SignPath Foundation» va aggiunta al README e alla pagina di download **solo dopo** l'accettazione: prima sarebbe falsa.
- Il collegamento della firma alla procedura di rilascio.

## Condizioni da ricordare

- Nell'installatore solo codice open source: i futuri plugin a pagamento restano fuori dall'installatore.
- Si firmano solo file costruiti dalla procedura automatica di questo repo.
- Ogni rilascio va approvato a mano prima della firma.
