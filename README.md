# kensho-skills

Curated custom skills for OpenAI Codex, Claude Code, and other Agent Skills compatible tools.

## Why "kensho"

`kensho` comes from the Japanese term `見性`, often rendered as "seeing one's nature" or "seeing into one's true nature" in Zen contexts.

The name fits this repository for a simple reason: a good skill should not feel like decoration. It should help an agent see a problem more clearly, act with better judgment, and arrive at a cleaner understanding of what matters in the task at hand.

This repository uses the name in that reflective sense:
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
```

## Local discovery

List skills in this repository:

```bash
npx skills add . --list
```

Install a specific local skill into an agent:

```bash
npx skills add . --skill compose-actions-contract -a codex
```

## Publishing

After pushing `kensho-skills` to GitHub, others can list its skills with:

```bash
npx skills add <owner>/<repo> --list
```

And install this skill with:

```bash
npx skills add <owner>/<repo> --skill compose-actions-contract -a codex
```

For this repository:

```bash
npx skills add DevPicon/kensho-skills --list
```

## Adding future skills

1. Create a new directory at `<skill-name>/`
2. Add `SKILL.md`
3. Ensure frontmatter `name` matches the directory name
4. Verify discovery with `npx skills add . --list`
