# Conventional Commits

- Structured commit messages to convey the type of change.

- Message Syntax template:

```txt
<type>[optional scope]: <description>
[optional body]
[optional footer] 
```

## Structural Components:

- **Type**: Noun describing *what* kind of change is introduced (e.g., `feat`, `fix`).
- **Scope (Optional)**: A noun describing the specific section of the codebase affected, enclosed in parentheses (e.g.,
  `(parser)`, `(auth)`).
- **Description**: A short summary of the code changes.
    * Separate from type/scope by a colon and a space (`: `).
- **Body (Optional)**: Detailed explanatory text after a blank line. Used to explain the *why* and *what* of the change.
- **Footer (Optional)**: One or more blocks of text separated by a blank line, used for referencing issues or breaking
  changes. Format is `Token: value` or `Token #value`.

## Type

You MUST use one of the following structural tokens as the `<type>`:

| Type       | Purpose                                                       | Semantic Version Impact |
|:-----------|:--------------------------------------------------------------|:------------------------|
| `feat`     | Introduces a new feature to the codebase                      | **MINOR**               |
| `fix`      | Patches a bug or resolves an issue                            | **PATCH**               |
| `docs`     | Changes to documentation only                                 | None                    |
| `style`    | Formatting, missing semi-colons (no production code change)   | None                    |
| `refactor` | Code change that neither fixes a bug nor adds a feature       | None                    |
| `perf`     | Code change that improves performance                         | None                    |
| `test`     | Adding missing tests or correcting existing tests             | None                    |
| `build`    | Changes that affect the build system or external dependencies | None                    |
| `ci`       | Changes to CI configuration files and scripts                 | None                    |
| `chore`    | Other changes that don't modify src or test files             | None                    |
| `revert`   | Reverts a previous commit                                     | None                    |

## Message description

- Concise summary of the change.
- Sacrifice grammar to fit 50 characters.
- Add issue/ticket references if applicable.

## Body and Footer

- Body: detailed explanation of the change.
- Footer: references to issues or breaking changes.
- Sacrifice grammar to fit 72 characters.

## Breaking Changes

- A breaking change indicates an API or behavioral change that requires a **MAJOR** version bump.
- You MUST signal a breaking change using exclamation mark. Place an `!` immediately after the type or scope, right
  before the colon.

* *Example (No scope):* `feat!: remove deprecated api endpoints`
* *Example (With scope):* `refactor(auth)!: switch to stateless JWT validation`

## Message Constraints

* **Case Sensitivity**: `<type>` MUST be strictly lowercase.
* **Spaces**: A space MUST follow the colon after the type/scope (`feat: change`, NOT `feat:change`).
* **Description**: Must be non-empty, imperative mood, present tense (e.g., "add feature" instead of "added
  feature"). Do NOT end the description with a period.
* **Footers**:
* Token names must use hyphens instead of spaces (e.g., `Reviewed-by:`).
* The only exception is `BREAKING CHANGE`, which is a token on its own without hyphens.
