# Mail a SignPath: cambio di licenza a GPLv3

Per chi: il fondatore, che invia la mail al supporto di SignPath Foundation; chi la legge è il personale che valuta la domanda. Il testo è in inglese, pronto da incollare.

## Come inviarla

1. Scrivi dall'indirizzo email con cui hai presentato la domanda (SignPath lega la richiesta a quell'indirizzo).
2. Indirizzo: quello di supporto che compare nella loro mail di conferma, oppure `support@signpath.org` (controlla sul sito <https://signpath.org> prima di inviare).
3. **Puoi inviarla subito**: la release 0.2.5 con la GPL è pubblica, GitHub mostra «GPL-3.0» su `cuelith-core` (ramo `main` compreso) e il sito è aggiornato.

## Testo

**Subject:** Cuelith (application of 2 October 2026): project licence is now GPL-3.0-or-later, first GPL release 0.2.5

Hello,

On 2 October 2026 I applied for free code signing for **Cuelith**, a free live-projection program for churches and events (https://github.com/Cuelith/cuelith-core, site: https://cuelith.lzrhive.it/en/). In my application I wrote that the project was licensed under Apache-2.0. **That is no longer accurate, so I am writing to update it before you review the application.**

**What changed**

On 5 October 2026 I changed the licence of the core program from Apache-2.0 to **GPL-3.0-or-later**, to make sure the program stays open source for good: anyone who distributes it, modified or not, must share the source code under the same licence. I own all the copyright, because no outside contribution had been accepted yet, so no one else's consent was needed.

- **First GPL release: 0.2.5**, published on 5 October 2026 (https://github.com/Cuelith/cuelith-core/releases/tag/v0.2.5). This is the release we would like to have signed.
- **Earlier releases withdrawn:** 0.1.0 and 0.2.0 (Apache-2.0) have been removed from GitHub Releases and from our download page. Nobody had downloaded the application yet, so there is no installed base on those versions.
- The `LICENSE` file is the unmodified GNU GPL v3 text, and GitHub detects the repository as GPL-3.0.

**What the signed installer contains**

The Windows installer `Cuelith-Setup-<version>.exe` contains only open-source code:

- the Cuelith core, GPL-3.0-or-later;
- two language plugins (Italian and English), GPL-3.0-or-later;
- the Cuelith SDK packages, Apache-2.0 (a separate library that plugin authors need to be able to use under any licence), and third-party dependencies under MIT, ISC, BSD, Apache-2.0 and MPL-2.0 licences, all compatible with the GPL. I checked them with `pnpm licenses list` on 5 October 2026.
- the licence texts themselves, in the installer's `resources/legal` folder.

**The plugin exception (and why it does not affect signing)**

The core has a written additional permission under section 7 of the GPL (`PLUGIN-EXCEPTION.md`): separate plugins that talk to Cuelith only through its public interface may be licensed as their authors wish, including as paid, closed-source plugins. This is a standard GPL additional permission, not a proprietary licence on the core. Plugins are separate programs, in separate repositories and packages, installed by users from a plugin list inside the app, and run in separate processes. **No third-party or proprietary plugin is, or will be, part of the installer you would sign**, and the core itself has no proprietary component. The project remains free of charge.

**Unchanged from my application**

The project name, the repository, the build process (installers are built by GitHub Actions from the tagged source, releases are approved manually), the code signing policy and the privacy section of the README. The name and logo are trademarks of the project owner; this only concerns the identity of the project and does not restrict anyone's use of the source code under the GPL.

If you need anything else, for example a different wording in the README or in the code signing policy, a link to a specific file, or a confirmation of ownership of the repository, please tell me and I will provide it right away.

Thank you for your time,
[Your first and last name]
Maintainer of Cuelith
[email]

---

## Note per il fondatore (non incollare)

- La domanda originale diceva «Apache-2.0 for the core and for every component in the installer». Qui diciamo ciò che è vero ora, senza nascondere il cambio, e prima che qualcuno trovi l'incoerenza.
- «Nobody had downloaded the application yet» riflette quanto detto dal fondatore il 2026-10-05: se nel frattempo qualcuno l'ha scaricata, togliere quella frase.
- Se SignPath chiede chiarimenti sull'eccezione per i plugin: i plugin di terzi non finiscono mai nell'installatore, e l'eccezione è un permesso aggiuntivo GPL standard (sezione 7), non una licenza proprietaria sul nucleo.
- Quando accettano, la frase obbligatoria nel README e nel sito resta: «Free code signing provided by SignPath.io, certificate by SignPath Foundation».
