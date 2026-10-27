# Skills

## Install

Install all skills locally for the current project:

```sh
npx skills add primeapple/skills
```

Install all skills globally for all projects:

```sh
npx skills add primeapple/skills -g
```

## `easy-wins`
Find and rank evidence-backed, high-leverage codebase improvements, then implement the chosen fix.

```sh
npx skills add primeapple/skills -g --skill easy-wins
```

## `browser`
Browser testing/debugging with Chrome DevTools MCP. Manual-only; in Pi, invoke `/skill:browser`.

```sh
npx skills add primeapple/skills -g --skill browser
```

The skill stays site-agnostic. A caller or wrapper can supply matching site/repository instructions when it invokes the skill. This repository contains no machine-specific or organization-specific browser context. Console checks compare against a baseline; pre-existing noise is non-blocking unless it prevents testing.

## `code-review`
Review code changes with evidence-backed, actionable findings and a bias toward simpler designs.

```sh
npx skills add primeapple/skills -g --skill code-review
```
