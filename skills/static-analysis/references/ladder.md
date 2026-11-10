# The analysis ladder

Each rung reads the code a different way and catches a different kind of problem. Lower rungs are cheaper and run earlier. A first gate is usually the bottom three rungs plus the tests; climb from there once the gate is green.

| Rung | Catches | How it reads the code |
|---|---|---|
| **Compiler and type checker** | Syntax and type errors, unreachable code | Parses and type-checks |
| **Formatter** | Inconsistent layout, and the arguments about it | Reprints the syntax tree in one style |
| **Linter (bug patterns)** | Known mistakes and risky idioms: empty catch, dead stores, `==` on strings, unused code | Matches patterns in the syntax tree or the compiled code |
| **Null types** | Null dereferences, at compile time | A type system that tracks which values may be null |
| **Dataflow and taint** | Untrusted input reaching a dangerous call (SQL, HTML, shell, file paths) | Follows values across methods and files |
| **Dependencies** | Libraries with known vulnerabilities | Matches the lockfile against a vulnerability database |
| **Containers and infrastructure** | A container that runs as root, an unpinned base image, an open bucket | Reads the Dockerfile or infrastructure files, or scans the built image |
| **Secrets** | A key or password committed to the repository | Matches known key formats in every commit or push |
| **CI workflows** | Unpinned actions, injectable expressions, overbroad permissions | Reads the workflow files |

## Candidate tools by stack

**These are candidates, not answers.** Before proposing one, check its latest release yourself; a tool can be the common answer and still be dead (ESLint 8 reached end of life in October 2024). Prefer one tool per rung.

| Stack | Formatter (check mode) | Linter | Type checker / null types |
|---|---|---|---|
| Python | `ruff format --check` | `ruff check` (Ruff's `S` rules port Bandit's security checks) | `mypy` or `pyright` |
| JavaScript / TypeScript (React, Vue, Node) | `prettier --check` | ESLint 9 or later, flat config (`eslint.config.js`), with the framework's plugin; or Biome for both | `tsc --noEmit` (`vue-tsc --noEmit` for Vue); `strictNullChecks` |
| Java | Spotless (google-java-format or Palantir) | SpotBugs (bytecode) and PMD (source); they barely overlap, so both earn their place | JSpecify annotations checked by NullAway (an Error Prone plugin) |
| Kotlin | ktlint | detekt | The compiler's null safety |
| C# / .NET | `dotnet format --verify-no-changes` | The built-in Roslyn analyzers (`AnalysisLevel`, warnings as errors) | Nullable reference types (`<Nullable>enable</Nullable>`) |
| Go | `gofmt -l` (fails if it lists a file) | `go vet`, staticcheck, or golangci-lint to run several | The compiler |
| Rust | `cargo fmt --check` | `cargo clippy -- -D warnings` | The compiler |
| Dart / Flutter | `dart format --set-exit-if-changed .` | `flutter analyze` (or `dart analyze`) | Sound null safety, built in |
| C / C++ | `clang-format --dry-run --Werror` | clang-tidy; compiler warnings as errors (`-Wall -Wextra -Werror`) | — |
| SQL, dbt | SQLFluff (`sqlfluff lint`, `sqlfluff format` locally) | SQLFluff; `dbt parse` or `dbt compile` catches broken references | — |
| Notebooks | Ruff reads `.ipynb` directly | Ruff; nbstripout keeps output out of commits | mypy through nbQA |
| Shell | shfmt (`-d`) | ShellCheck | — |

**Beyond your own code**, one per rung:

| Rung | Candidates | Notes |
|---|---|---|
| Dataflow and taint | CodeQL (GitHub code scanning), Semgrep Community Edition | CodeQL's default setup is free on public repositories; on a private one, check the plan |
| Dependencies | Dependabot (alerts and update pull requests), OSV-Scanner, `pip-audit`, `npm audit` | Turn on update pull requests only once CI would catch an update that breaks the build |
| Containers and infrastructure | Hadolint (Dockerfile), Trivy (built image, infrastructure files), `terraform fmt -check`, `terraform validate`, tflint | A linter reads the instructions written; an image scanner sees what is missing, such as a root user |
| Secrets | GitHub secret scanning with push protection; gitleaks locally or in CI | Free on public repositories; on a private one, check the plan. A secret that reached a public repository is compromised even after the commit is deleted: rotate it |
| CI workflows | actionlint; zizmor for workflow security | Pin every action to a full commit SHA, and let Dependabot's `github-actions` ecosystem propose updates |
| Architecture rules | ArchUnit (Java), import-linter (Python), dependency-cruiser (JavaScript) | Rules such as "no controller calls a repository", written as tests |

**Data and ML projects** add no new rung; they have more kinds of code. Lint and format the SQL and the notebooks, type-check the Python, and keep committed notebooks free of output (it can hold data, personal data, or secrets). Data-quality checks (row counts, nulls, freshness) and model evaluation are tests named in the data or model contract, not static analysis.

## The first run, without touching the build

Run the tool on its own before it goes into the build, so the team sees its opinion before anything changes:

- **Java (Maven):** compile, then run a plugin by its coordinates, for example `./mvnw com.github.spotbugs:spotbugs-maven-plugin:<version>:spotbugs` and `./mvnw org.apache.maven.plugins:maven-pmd-plugin:<version>:pmd -Dformat=xml`. Reports land in `target/`.
- **JavaScript:** `npm ci`, then `npx eslint .`, `npx prettier --check .`, and the type check, all without `--fix` or `--write`.
- **Python:** `pipx run ruff check .` or the project's virtual environment.

## Counting by rule

Every finding names the rule that fired it. Group by rule, sorted by count:

```bash
# Any tool with SARIF output (CodeQL, Semgrep, Ruff, ESLint with a SARIF formatter, and others)
jq -r '.runs[].results[].ruleId' report.sarif | sort | uniq -c | sort -rn

# Ruff
ruff check . --statistics

# ESLint (rule "null" is a parse error)
npx eslint . -f json | jq -r '.[].messages[].ruleId' | sort | uniq -c | sort -rn

# mypy
mypy . | grep -oE '\[[a-z-]+\]$' | sort | uniq -c | sort -rn

# SpotBugs XML report
grep -oE '<BugInstance [^>]*' target/spotbugsXml.xml | grep -oE ' type="[A-Z0-9_]+"' | sort | uniq -c | sort -rn

# PMD XML report
grep -oE ' rule="[A-Za-z0-9]+"' target/pmd.xml | sort | uniq -c | sort -rn
```

Then read one example of each rule before deciding anything about it.

## Choosing: the four tests

1. **Free, and runs in the project's own build with no account**, so the gate works on every runner and every fork.
2. **Maintained:** a release in the last year, checked today.
3. **Has a checking mode** that reports and changes nothing, for CI.
4. **One per rung:** two linters with overlapping rules double the findings and the arguments. The exception is two tools that read different things (SpotBugs reads bytecode, PMD reads source) and barely overlap.

Run each tool yourself before it goes in the workflow. The agent's description of a tool is a claim; the first run's counts by rule are the evidence.
