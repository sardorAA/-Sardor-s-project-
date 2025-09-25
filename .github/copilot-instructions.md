## Repository quick summary

This repository currently contains a small console C++ "mathematical calculator" example embedded in `README.md`.
Primary runtime behavior: read lines from stdin, parse one or more double values per line, compute three math expressions for each value and print formatted results. The program uses Russian UI strings and emoji.

## Key symbols and places to look
- `README.md` — contains the entire program source as a code block. Key symbols:
  - `void calculate(double x)` — performs the math for a single input value.
  - `int main()` — read-loop, parsing and "exit" sentinel handling.
- There is no `src/`, no build system (Makefile/CMake/CI) in the repository at the time of writing.

## Big-picture architecture and data flow
- Single small binary, single-threaded, console-only.
- Data flow: stdin line -> `getline` -> check for literal `"exit"` -> `stringstream ss(input)` -> while `ss >> x` call `calculate(x)` -> formatted `cout` output.
- Error handling: `calculate` uses a catch-all `catch(...)` and prints `❌ Ошибка вычислений` on failure. Non-numeric tokens are detected in main and print `❌ Ошибка: найдено некорректное значение.`

## Project-specific conventions and patterns
- Preserve Russian messages and emojis when editing UI text. Examples to keep exactly or update consistently: `"Введите x:"`, `"exit"`, `"Для x = "`, `"✅ Программа завершена."`.
- Keep `calculate(double)` signature and its single-responsibility: compute and print results for one x. Refactors should keep the same observable CLI behavior.
- Formatting is applied with `cout << fixed << setprecision(4);` — maintain this approach for human-readable numeric output.
- Uses `M_PI` and <cmath> math functions (tan, exp, sin, pow). On macOS/clang this compiles without extra defines; ensure math constants remain available if moving to different toolchains.

## Build / run / debug (discoverable from code)
Since there's no build configuration, use system compiler on macOS (zsh):

  - Compile (example):
    clang++ -std=c++17 -O2 -o calc main.cpp

  - Run:
    ./calc

  - Smoke test (stdin example):
    Type: 1 2 3 <Enter>
    Type: exit <Enter>

If the source remains in `README.md`, first move the code into `main.cpp` (suggested path `src/main.cpp` or project root `main.cpp`) before compiling.

## Tests and CI
- No tests or CI present. For quick verification, prefer manual smoke runs described above. If adding unit tests, keep them minimal and platform-independent; the code is pure computation + IO.

## When making changes, follow these rules
- Do not change the CLI semantics: input parsing (multiple numbers per line), the `exit` sentinel, or the error messages without explicit reason.
- Preserve Russian localization and emoji usage in user-visible strings unless you intentionally internationalize the project (document this in the PR).
- Keep `calculate(double)` behavior visible via the console (it prints three results: y1, y2, y3).

## Useful refs for editors/agents
- Look at `README.md` code block for the full source; any edits should either modify that block (if updating README) or move the code into a real source file and update README to show how to build/run.

If anything here is incomplete or you want more detail (preferred build layout, tests, or CI), tell me which areas to expand and I will iterate.
