# kensho-skills

Curated custom skills for OpenAI Codex, Claude Code, and other Agent Skills compatible tools.

## Why "kensho"

`kensho` comes from the Japanese term `見性`, often rendered as "seeing one's nature" or "seeing into one's true nature" in Zen contexts.

When I chose the name, the intention was close to this idea: insight, or a deeper kind of understanding that becomes useful in practice.

In that sense, `kensho-skills` is not about sounding mystical or ornamental. It is about the moment when a problem becomes clearer, when structure emerges from noise, and when better judgment follows from better understanding.

That is the spirit behind this repository:
- clarity before ceremony
- insight before repetition
- better decisions through better framing

## License

This project is released under the MIT License. In practice, that means the skills can be used, modified, redistributed, and used commercially, as long as the copyright notice and license text are preserved.

The license is intentionally permissive: the goal of `kensho-skills` is to make useful agent skills easy to reuse and adapt.

See [LICENSE](/Users/devpicon/Documents/custom%20skills/LICENSE).

## Repository layout

`kensho-skills` stores multiple skills in a single repository using a flat, distribution-oriented layout so each skill lives in its own top-level directory.

```text
compose-actions-contract/
  SKILL.md
kmp-mvp-task-executor/
  SKILL.md
kmp-task-independent-review/
  SKILL.md
mobile-litert-validation/
  SKILL.md
```

Published skills:
- `compose-actions-contract`: Compose and Compose Multiplatform callback-contract refactoring guidance.
- `kmp-mvp-task-executor`: Scoped implementation workflow for Kotlin Multiplatform MVP tasks.
- `kmp-task-independent-review`: Independent engineering review workflow for completed KMP tasks.
- `mobile-litert-validation`: Android and iOS LiteRT-LM validation workflow for on-device AI integrations.

## Discover skills

To see the skills published in this repository:

```bash
npx skills add DevPicon/kensho-skills --list
```

## How to contribute

Contributions are welcome.

If you want to add a skill or improve an existing one:
1. Fork the repository
2. Create or update the skill in its own top-level directory
3. Open a pull request with a clear description of the change
