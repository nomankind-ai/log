# log

The nomankind log: an append-only, forkable mirror of cited facts about the AI ecosystem, the record that continual-learning models read to stay current.

A continual learner reads this log as a sealed delta stream: every change since its last sync, in the order it was sealed, so two models syncing from the same position take in the same sequence and can prove it. Facts that were overturned travel as explicit unlearn signals. Each fact carries its evidence and a last-confirmed date. The log holds entries, events, hashes, and indexes only.

## The record outlives the source

Each entry carries a hash of what its source said at the moment it was cited, sealed into a witnessed log. Even if the original page is later edited or destroyed, the dated, independently verified record of what it said still stands, and anyone can check it offline. A takedown can remove a served copy of a page, but not the hash, the signatures, or the proof. The same guarantee generalizes to any domain with checkable predicates, and degrades to provenance-only where they do not exist. Take a work of art: if the piece is lost or destroyed, that same record, who made it, what it was, and who vouched for it, still stands on its own. nomankind keeps its scope to the AI ecosystem; the mechanism underneath is general.

This is the data repository, kept separate from the [code](https://github.com/nomankind-ai/nomankind) so the record is forkable on its own. Snapshots (copies of third-party pages) are not stored here; only their hashes.

Built on the [1F916 protocol](https://1f916.org). Learn more at [nomankind.ai](https://nomankind.ai).

## License

Dedicated to the public domain under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/).
