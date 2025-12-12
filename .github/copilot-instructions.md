## Quick context

This is a small, single-package Java CLI quiz app implemented under `src/`.
Core model classes: `Category`, `Answer`, `Question`, and the runner `Quiz` (contains `main`).

Keep changes minimal and Java-idiomatic: the project currently does not use Maven/Gradle — compilation is done with `javac` and run with `java`.

## Architecture & data flow (short)

- `Quiz` constructs `Category` objects and `Question` instances, each `Question` contains an `Answer[4]`.
- `Question.ask(Scanner)` prints choices and returns the `Category` associated with the chosen `Answer`.
- `Quiz` increments `Category.points` per answer and uses `getMostPopularCatIndex` to pick the winner. Tie-breaker: the first highest-count category in the provided array.

Files to look at for examples:
- `src/Quiz.java` — app orchestration, user input loop, and game intro.
- `src/Question.java` — how answers are printed and chosen (1-based display, 0-based array indexing internally).
- `src/Answer.java` — simple tuple of `label` and `Category`.
- `src/Category.java` — label, description, and `points` counter.

## Project-specific patterns & gotchas (do not change without tests)

- Answers are stored in a fixed-size `Answer[4]`. When adding questions, always populate all 4 entries to avoid `NullPointerException` in `ask()`.
- `Question.ask()` shows choices labeled `1`..`4` and then reads an `int` — code expects valid input (no validation beyond array bounds). When improving input handling, keep the UI behavior consistent.
- `Category.points` is incremented directly by `Quiz` after each `ask()`; the final scoring expects the same `Category[]` order used in `getMostPopularCatIndex` — don't reorder that array without adjusting callers.
- Current code has a few incomplete/commented lines in `src/Quiz.java` (syntax errors and undefined variables like `monopoly`, `catan`, etc.). When fixing, prefer reinstating consistent category variable names (e.g., `fantasy`, `horror`, `mystery`, `thriller`, `dystopian`) and ensure the `Category[]` passed to `getMostPopularCatIndex` matches those used in scoring.

## How to build & run locally (examples)

1) Compile all sources into `out/`:

```bash
javac -d out src/*.java
```

2) Run the app (class name `Quiz`, default package):

```bash
java -cp out Quiz
```

If you edit package declarations, update compilation and run commands accordingly.

## Typical edits an AI agent may be asked to do

- Fix input validation in `Question.ask()` (guard against non-int and out-of-range values).
- Repair and complete `src/Quiz.java` (populate `Question.possibleAnswers[]`, remove or replace commented placeholders, and fix variable names used in final scoring array).
- Add a small test runner or smoke test (a script that compiles and runs with a simulated input stream). If adding tests, use a lightweight approach (shell script or JUnit in a small `test/` folder) and document how to run them.

## Examples from codebase (copy/paste safe snippets)

- How to add an answer to a question (pattern used in comments):

```java
q1.possibleAnswers[0] = new Answer("Outside playing catch at the pool.", fantasy);
```

- How `ask()` expects to be called and used:

```java
Category c = q.ask(sc);
c.points++;
```

## If you need to change behavior

- Keep edits focused and runnable: after changes, compile with `javac -d out src/*.java` and run `java -cp out Quiz` to smoke-test. Mention any assumptions you make in your PR description.

---
If anything above is unclear or you need more examples (for instance, a canonical set of categories/answers to restore `Quiz.java`), tell me which parts you'd like me to expand or fix and I will update this file.
