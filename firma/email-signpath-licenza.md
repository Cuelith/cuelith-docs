# Mail a SignPath: cambio di licenza a GPLv3

Per chi: il fondatore, che invia la mail al supporto di SignPath Foundation; chi la legge è il personale che valuta la domanda. Il testo è in inglese, pronto da incollare.

## Come inviarla

1. Scrivi dall'indirizzo email con cui hai presentato la domanda (SignPath lega la richiesta a quell'indirizzo).
2. Indirizzo: quello di supporto che compare nella loro mail di conferma, oppure `support@signpath.org` (controlla sul sito <https://signpath.org> prima di inviare).
3. **Invia dopo** che i repo con la GPL sono pubblicati su GitHub (il ramo `main` di `cuelith-core` mostra la licenza GPL-3.0 solo dopo il rilascio successivo alla 0.2.0; se vuoi scrivere prima, lascia la frase tra parentesi quadre). Il link alla pagina della licenza sul ramo `dev` va bene fin da subito.

## Testo

**Subject:** Cuelith (SignPath Foundation application): licence changed from Apache-2.0 to GPL-3.0-or-later

Hello,

I submitted an application for free code signing for **Cuelith** on 2 October 2026 (repository: https://github.com/Cuelith/cuelith-core). I am writing to let you know that I have changed the project's licence since then, so that your review uses the correct information.

- **Core (`cuelith-core`) and the bundled language plugins:** now **GPL-3.0-or-later** (OSI-approved). The `LICENSE` file is the unmodified GNU GPL v3 text, and GitHub detects it as such. Releases up to 0.2.0 stay available under Apache-2.0; the GPL applies from the next release, which is the first one we would like to sign.
- **Additional permission:** the core has a documented plugin exception under section 7 of the GPL (`PLUGIN-EXCEPTION.md`), which lets third parties license separate plugins as they wish. Plugins are separate programs, in separate repositories and packages, installed by users from the plugin list; **they are never part of the installer you would sign**. The installer contains only the GPL core, the two GPL language plugins, and open-source dependencies (MIT, ISC, BSD, Apache-2.0, MPL-2.0).
- **SDK (`cuelith-sdk`):** a separate library, Apache-2.0, included in the build of the core as a dependency.
- **No proprietary component** is or will be in the signed artifact, and the project remains free of charge.
- The name and logo are trademarks of the project owner (see `TRADEMARK.md` in https://github.com/Cuelith/.github); this does not restrict use of the source code under the GPL.

Everything else in my application is unchanged. Please let me know if you need anything further, for example a different wording in the README or in the code signing policy.

Thank you,
[Your first and last name]
Maintainer of Cuelith
[email]

---

## Note per il fondatore (non incollare)

- La domanda originale diceva «Apache-2.0 for the core and for every component in the installer». Qui diciamo ciò che è vero ora, senza nascondere il cambio.
- Se SignPath chiede chiarimenti sull'eccezione per i plugin: i plugin di terzi non finiscono mai nell'installatore, e l'eccezione è un permesso aggiuntivo GPL standard (sezione 7), non una licenza proprietaria sul nucleo.
- Quando accettano, la frase obbligatoria nel README e nel sito resta: «Free code signing provided by SignPath.io, certificate by SignPath Foundation».
