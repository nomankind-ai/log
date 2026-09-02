# log

The nomankind log: an append-only, forkable mirror of cited facts about the AI ecosystem, the record that continual-learning models read to stay current.

A continual learner reads this log as a sealed delta stream: every change since its last sync, in the order it was sealed, so two models syncing from the same position take in the same sequence and can prove it. Facts that were overturned travel as explicit unlearn signals. Each fact carries its evidence and a last-confirmed date. The log holds entries, events, hashes, and indexes only.

This is the data repository, kept separate from the [code](https://github.com/nomankind-ai/nomankind) so the record is forkable on its own. Snapshots (copies of third-party pages) are not stored here; only their hashes.

## License

Dedicated to the public domain under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/).
