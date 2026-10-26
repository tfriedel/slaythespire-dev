---
name: spire-mentor
url: "https://github.com/tfriedel/spire-mentor"
repo: tfriedel/spire-mentor
author: tfriedel
games: [sts2]
category: bot
approaches: [search, rl, imitation, tooling]
language: "Rust, Python, TypeScript, C#"
license: MIT
status: active
summary: "Rust port of Slay the Spire 2's combat and run rules, parity-tested against the game's own code, with MCTS combat and a PPO run policy. Includes a browser workbench and an in-game coaching mod."
---

The engine covers all five characters on the v0.107.1 build. After every action it is checked in lockstep against the game's C# code, run headless, plus a fuzz tier. The README reports Ironclad at 6,104/6,104 fuzzed fights clean and the other characters at 99.4–99.8%. On one core it runs about 59k random-play fights per second, which the project puts at about 1,500× the game's own logic run headless.

Combat uses MCTS with chance nodes, so it does not see upcoming draws or other hidden RNG. Run-level choices such as cards, paths, shops and events come from a PPO policy per character. The README reports these win rates at 8,000 sims per decision (October 2026, 200 runs per cell at A0–A8 and 500 at A9–A10):

| Character | A0 | A9 | A10 |
| --- | --- | --- | --- |
| Defect | 93.0% | 39.2% | 22.0% |
| Ironclad | 89.5% | 39.0% | 26.8%* |
| Silent | 78.5% | 20.0% | 9.0% |
| Regent | 67.5% | 15.6% | 6.2% |
| Necrobinder | 66.5% | 13.8% | 5.6% |

\* Ironclad A10 is newer: 107/400 runs (95% CI 22.6–31.3%), measured 26 October 2026 at 4,000 sims (16,000 at bosses) after an A10-focused PPO continuation and a behavior-cloned move prior for elite and boss fights. Its other cells predate those changes.

The gaps between characters mostly reflect training effort, not a ceiling. Development currently pushes one character, Ironclad, as far as possible before returning to the others, and the lower rows haven't been retrained since.

The browser workbench compiles the engine to WebAssembly. You can rewind and branch any decision, ask the AI for a hint, browse its search tree, let it play the fights, or review a finished run for its biggest drops in win chance. The Spire Mentor mod on the Steam Workshop brings much of this into the real game. You need your own copy of the game: art and sound are extracted locally from your install and aren't in the repository.
