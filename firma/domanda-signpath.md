# Firma gratuita dell'installatore Windows: domanda a SignPath Foundation

Per chi: il fondatore, che invia la domanda. Le risposte in inglese sono pronte da incollare; chi le legge è il personale di SignPath Foundation.

## Perché

L'installatore di Windows non è firmato: chi lo apre vede «PC protetto da Windows» con autore sconosciuto. SignPath Foundation firma gratis i progetti open source; l'autore mostrato sarà «SignPath Foundation».

## Cosa fare

1. Apri <https://signpath.org/apply> e compila il modulo con le risposte qui sotto.
2. Usa il tuo account GitHub (quello che possiede l'organizzazione Cuelith, con la verifica in due passaggi già attiva).
3. Aspetta la risposta per email: possono volerci giorni o settimane, e possono chiedere chiarimenti. Giramela e preparo la risposta.
4. Quando accettano, ti danno accesso a SignPath: da lì in poi collego io la firma alla procedura di rilascio. Ogni rilascio andrà approvato a mano da te con un clic.

## Risposte pronte

| Domanda | Risposta |
| --- | --- |
| Project name | Cuelith |
| Repository URL | https://github.com/Cuelith/cuelith-core |
| Homepage / download page | https://cuelith.lzrhive.it/en/ |
| License | Apache-2.0 (OSI approved), for the core and for every component in the installer |
| Programming language / build | TypeScript, Electron; built with electron-builder by GitHub Actions |
| Artifact to sign | Windows installer `Cuelith-Setup-<version>.exe` (NSIS, per-user) and the executables inside it |
| Build system | GitHub Actions, workflow `.github/workflows/release.yml`, started by a version tag on `main` |
| Release frequency | A few releases per month while in preview |
| Maintainer | The owner of the Cuelith organisation on GitHub (@MattiaLazzari) |

**Project description (short)**

> Cuelith is free, open-source live projection software for events: it shows texts, lyrics and announcements on a projector, a stage monitor and other screens. It has a small core and a plugin system; plugins run in separate processes with declared permissions.

**Why we need code signing**

> The Windows installer is downloaded by event operators and volunteers who are not technical. Without a signature they see the "Windows protected your PC" screen with an unknown publisher, and many stop there. The app also updates itself with electron-updater, and a signature lets Windows and the updater verify that an update really comes from our release workflow.

**How releases are built**

> Every release is built from a version tag on the `main` branch by the GitHub Actions workflow in the repository, using only the source code of our public repositories (`cuelith-core`, `cuelith-sdk`, `plugin-locale-it`, `plugin-locale-en`) and their open-source dependencies. `main` and `dev` are protected branches: changes from contributors need a pull request, a passing check and the maintainer's approval. The release is created as a draft and is published only when all installers have been built.

**Privacy**

> Cuelith has no accounts and collects no data. It contacts the internet only to check for app updates and to read the public list of plugins. The automatic update check can be turned off in the settings. The code signing policy and the privacy statement are in the repository README.

**Code signing policy**

> https://github.com/Cuelith/cuelith-core#code-signing-policy

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
