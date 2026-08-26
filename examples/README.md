# Examples

> A filled in version of every document in the second list, all describing the
> same invented project.

Filled in examples of the documents described in
[Documents worth having](../02-documents.md). Copy the shape and throw the
content away.

They all describe the same invented project, so they fit together. Meadow is a
diary and health record app for parents of young children. It runs on iOS and
Android, keeps everything on the phone, sells a subscription through both
stores, and ships a few hundred articles that a person edits in a spreadsheet.

In the same order as the second document, which is rough order of how much each
one earns its keep. None of them is required.

| Example | Its name in a real project | What it holds |
| --- | --- | --- |
| [spec.md](./spec.md) | `spec.md` | The project in four lines, every feature, and the one being built now |
| [to-be-continued.md](./to-be-continued.md) | `to-be-continued.md` | What looks finished and is not |
| [change-log.md](./change-log.md) | `change-log.md` | What changed, when, and why |
| [rules-file.md](./rules-file.md) | `CLAUDE.md` or `AGENTS.md` | What an agent may do without asking |
| [runbook.md](./runbook.md) | `runbook.md` | How work is done here, and when to update each document |
| [architecture.md](./architecture.md) | `architecture.md` | One page and one diagram |
| [decisions.md](./decisions.md) | `decisions.md` | The decisions that took real thought |
| [test-notes.md](./test-notes.md) | `test-notes.md` | What is tested and what is not |
| [data-map.md](./data-map.md) | `data-map.md` | What personal data you hold |
| [domain-source.md](./domain-source.md) | `content/` | Domain facts kept outside the code |

Every example opens with the one line description that the document should
carry, which is the convention the second document asks for.

The rules file example is named `rules-file.md` rather than `CLAUDE.md` on
purpose, so that an agent working in this repository does not read an example as
its own instructions. Rename it when you copy it.

Every example is short. A document nobody finishes reading is a document nobody
follows, and a document that repeats the code goes stale in a month.
