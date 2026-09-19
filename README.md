# nomankind log

The append-only log of [nomankind](https://nomankind.ai), exported once per UTC
day. Entries, events, hashes, seals, anchors and indexes: the sealed record and
nothing else.

nomankind is a free, tamper-evident record of attested facts about AI,
independent of every lab, for models that keep learning. What attested means is
defined on the thesis page, <https://nomankind.ai/docs/thesis>. This repository
is the copy of the record that anybody can check without asking nomankind for
anything.

**This data is dedicated to the public domain under [CC0-1.0](LICENSE).** Fork
it, mirror it, train on it, build on it. No attribution is required and none is
asked for.

The export exists so that nomankind going away is an inconvenience rather than
an ending. Nothing here needs our servers, our uptime or our permission to be
useful. The exit is not a promise, it is a copy.

## What is in here

Two directories, one per deployment.

- **`production/`**: [app.nomankind.ai](https://app.nomankind.ai), the real log.
- **`demo/`**: [demo.nomankind.ai](https://demo.nomankind.ai), the demonstration
  environment. Its witnesses are a published mock pair whose keys are in the code
  repository: it proves the plumbing runs, and it is not a record of anything
  about the world.

Each directory is a complete, self-contained export of that log's sealed state.

| Path | What it holds |
| --- | --- |
| `mirror.json` | The manifest: `format` (`nomankind-mirror-v3`), the environment, `exported_at`, `as_of` and `head` from the newest seal, `seal_seq`, the counts, the schema and norm versions, the domains, `captures_base`, and the verify command. |
| `events/<seal seq, 8 digits>.jsonl` | The events one seal covers, in seq order, hash chain and all, in full. A seal's range never moves, so a seal's file is written once and never changes. |
| `seals.jsonl` | Every seal in seq order, with its Merkle root, its chain link, its countersignatures and its registry receipt. |
| `anchors.jsonl` | Every daily anchor in date order, with its external timestamp receipt. |
| `operators.json` | Every operator, with the domains it is attested in and the keys bound to it, and the map from each key to its operator. |
| `entries/<entry id>.json` | One entry as it stood at the sealed head: the derived entry with its seal object, its sidecar, and its core hash. |
| `index.json` | One row per entry in submission order, for finding things without opening every file. Every column is proof. |

Three further files sit beside those, recomputed from the sealed events rather
than copied off any table: the offline check below rebuilds each of them and
diffs it line by line, and `docs/FORK.md` in the code repository describes them
in detail.

A clone pulled before any of this is still good: a directory whose manifest says
`nomankind-mirror-v1` or `nomankind-mirror-v2` is checked and replayed as what
that format was, because a copy in somebody's hands is their exit and taking it
back is not on offer.

Every JSON document is two-space indented with a trailing newline; every
`.jsonl` file is one compact document per line. The identity is per file, not
per directory: for the same sealed content at the same clock, a seal's events
file, an entry's file, a seal row and an anchor row come out byte for byte the
same in every export, so a diff in one of those files is a change in the log and
never a change in formatting. "At the same clock" is the whole of the caveat: an
entry's file carries the derived entry, and `stale` is derived against the day it
is read, so an entry whose freshness window runs out flips that one field on its
expiry date with nothing sealed having changed. The directory itself moves every
day, for two reasons that say nothing about any record: `mirror.json` carries the
`exported_at` of the run that wrote it, and, once the log has ever issued a read
receipt, the sweep appends one `read_count` event for every finished UTC day
after that, even when it counts nothing, so there is a new event, a new seal and
a new head each day.

The proof and the content travel together, under CC0, from the first export
either can appear in: every hash, every seal, every anchor, every operator
record, every event payload, and every entry's id, domain, subject, category,
status, tier, entry hash, seal and signers. There is nothing here a key or a
signature reaches that a stranger does not.

Nothing unsealed is ever here. An entry whose submission no seal covers is not
exported, and neither are the events after the head.

## What the statuses mean

An entry's status is folded from the sealed events and is never set by hand.
There are six.

- **draft**: submitted and signed, not yet decided.
- **attested**: every element of the entry's check was attested by a party
  nomankind does not control, and the decision is reproducible by anyone. The
  thesis page names the five witnesses that make it so: input, archive,
  execution, time and decision.
- **verified**: attested, and in addition confirmed by a party outside
  nomankind through the one outsider layer. nomankind's own runs never reach it
  alone.
- **rejected**: decided against, with the reasons attached.
- **superseded**: a later entry changed the same fact again. The old entry was
  right for its dates and stays readable.
- **stale**: past its freshness window and awaiting a fresh check.

## What is not in here

The **snapshots**. The mirror holds the hash of every captured page and never
the page itself: the bytes are a third party's, they are held as evidence of
what a source said at a moment, and they live outside this repository as an
evidentiary archive. A takedown can remove a served copy of a page; it cannot
remove the hash, the signatures, the seal or the inclusion proof. The captures
are served from each environment's own `/captures/{hash}`, which `mirror.json`
names in `captures_base`.

## Verify it

Clone this repository and the code, and check the whole thing offline. Node 22,
no accounts, no network except for the captures.

```sh
git clone https://github.com/nomankind-ai/log
git clone https://github.com/nomankind-ai/nomankind
cd nomankind
npm install
npm run verify-mirror -- ../log/production
```

It checks `mirror.json` against the files that are there, the event chain over
every events file, every seal against the events it names and the seal before it,
every anchor against that day's roots, and then every entry file: its `entry` and
`sidecar` re-derived from these very events and diffed field by field, its
`entry_hash`, its row in `index.json`, its core against the core the log sealed,
and the author's signature over that core. The same offline verifier the two
files and one command rest on runs on top of that. Then come the three files
nothing was read for, each recomputed at the sealed head and diffed. One line per
item, one summary line, and the exit code is the answer: 0 when nothing failed,
1 when something did.

Every file is checked. Nothing here is a hash line, so nothing is out of reach of
a fold, and every entry is re-derived from the events beside it.

A record sealed under the older schema v0.6 is reported as `legacy` rather than
`ok`: everything above is checked over it, and only the rules that came with
v0.7 are left off, because they cannot be applied to bytes that never claimed
them. An edited legacy record still fails, and still exits 1.

`--entry <id>` checks one entry. `--captures <url-or-dir>` reads the captures
from somewhere other than the live archive, including your own copy of it.

You can also rebuild this directory yourself, from the public API, and diff it:

```sh
npm run mirror -- https://app.nomankind.ai ./my-mirror
diff -r ./my-mirror/production ../log/production
```

## Reading it without cloning anything

The record is free to read and needs no key, no account and no header. The
reader kit in the code repository is the short way in:

```sh
npm run kit -- read https://app.nomankind.ai <entry id>
npm run kit -- sync https://app.nomankind.ai --from 0
npm run kit -- export https://app.nomankind.ai <entry id> ./bundle
npm run kit -- verify ./bundle/entry.json ./bundle/log.json
```

`npm run mcp -- https://app.nomankind.ai` serves the same tools to an agent over
MCP. [`docs/READER-KIT.md`](https://github.com/nomankind-ai/nomankind/blob/main/docs/READER-KIT.md)
is the whole of it.

## Where this comes from

- **Code, schema and whitepaper:**
  [nomankind-ai/nomankind](https://github.com/nomankind-ai/nomankind),
  Apache-2.0.
- **The definition:** the thesis page, <https://nomankind.ai/docs/thesis>.
- **The live log:** [nomankind.ai](https://nomankind.ai).
- **The newest export:** `GET /mirror/latest` on either environment names the
  commit it landed in, the sealed head it covers, and the raw `mirror.json`
  beside it.

Issues and pull requests against this repository are not the way to correct the
record. The log is append-only and everything in it is derived from sealed
events: a fact that is wrong is corrected by disputing the entry on the live
instance. Editing a file here would only break the proofs.

## License

[CC0-1.0](LICENSE). The code that produced these files is Apache-2.0 and lives
in the [nomankind](https://github.com/nomankind-ai/nomankind) repository.
