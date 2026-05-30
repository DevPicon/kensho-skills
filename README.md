# kensho-skills

Curated custom skills for OpenAI Codex, Claude Code, and other Agent Skills compatible tools.

## License

MIT. See [LICENSE](/Users/devpicon/Documents/custom%20skills/LICENSE).

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

## Adding future skills

1. Create a new directory at `<skill-name>/`
2. Add `SKILL.md`
3. Ensure frontmatter `name` matches the directory name
4. Verify discovery with `npx skills add . --list`
