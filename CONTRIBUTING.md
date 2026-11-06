# Contributing to KEEL

Thank you for your help. This file tells you how to give feedback and how to send a change.

## Feedback, questions, and bugs

This repository uses GitHub Discussions, not issues.

- For general feedback and bug reports, use the pinned Discussion [Feedback on KEEL 2.0](https://github.com/rickflashman/Keel/discussions/9).
- For a question, open a Discussion in the **Q&A** category.
- For a new idea, open a Discussion in the **Ideas** category.

A useful report names the AI tool and model, the KEEL version, and what you expected to happen.

## How the repository works

`KEEL.md` is the single source of truth. All other KEEL content comes from it.

- `build/build.py` generates `skills/` and `.claude-plugin/` from `KEEL.md`. Do not edit those folders by hand.
- The GitHub Actions workflow runs `python3 build/build.py --check` on every push and pull request. The check fails when the generated files do not match `KEEL.md`.

## Send a change

1. Discuss a large change in a Discussion first. A small fix, such as a typo or a broken link, needs no discussion.
2. Fork the repository and make a branch from `main`.
3. Edit `KEEL.md`. To change the README, edit `README.md`.
4. Run the build: `python3 build/build.py`. On Windows, use `py build/build.py`.
5. Run the check: `python3 build/build.py --check`. It must pass.
6. Commit `KEEL.md` and all the generated files together.
7. Open a pull request. Tell what you changed and why.

Note: on Windows with `core.autocrlf=true`, the build can show many line-ending-only changes. Run `git add --renormalize .` before you commit.

## Writing style

KEEL is written for developers and AI tools that read English as a second language. Use the same style in your change:

- Use short sentences, with one idea in each sentence.
- Use the active voice. Write an instruction as a command.
- Use the same word for the same thing every time.
- Write numbers as digits.

## Versions

The maintainer sets the version number and writes the changelog entry in `KEEL.md`. Do not change the version line in a pull request.

## License

KEEL uses the MIT License. When you send a change, you agree to license it under the same terms. See [`LICENSE`](LICENSE).
