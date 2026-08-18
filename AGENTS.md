# Agent Instructions

## Response format

- Must Begin every response with exactly: `Hey Yz!`
- Keep responses concise, clear, and focused on the requested work.
- State assumptions, blockers, and verification results when they matter.

## Before making changes

- Read the relevant files and surrounding code before editing.
- Check existing project documentation, scripts, tests, and conventions.
- Inspect the current Git status and preserve unrelated user changes.
- Prefer the smallest change that fully satisfies the request.

## Implementation guidelines

- Follow the repository's existing architecture, naming, formatting, and dependency conventions.
- Keep modules and functions focused; avoid unrelated refactors.
- Reuse existing utilities and patterns before introducing new ones.
- Update documentation and tests when behavior or public interfaces change.
- Never hard-code secrets, credentials, tokens, or private data.
- Do not modify generated files unless the project workflow requires it.

## Testing and verification

- Run the most relevant available tests, linters, formatters, or type checks after making changes.
- If a check cannot be run, explain why and identify what remains unverified.
- Review the final diff for accidental edits, debug output, secrets, and unnecessary changes.

## Git and safety

- Do not discard, overwrite, or revert existing user work.
- Do not run destructive commands such as `git reset --hard`, `git clean`, or broad file deletion.
- Do not commit, push, create branches, or open pull requests unless explicitly requested.
- Do not change dependencies or configuration unless required by the task.
- Ask for clarification when an ambiguity could materially change the implementation.

## Completion report

When finishing a task, summarize:

1. What changed.
2. Which files were affected.
3. What verification was run and its result.
4. Any remaining limitations or follow-up work.
