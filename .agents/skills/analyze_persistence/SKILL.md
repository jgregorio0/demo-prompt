---
name: Analyze persistence
description: "Analyze the provided [class] and [method] to identify all Data Repositories and Entities invoked and generate a
documentation file that maps out exactly which database tables and columns are accessed."
---

# CONTEXT

- **Entry Point:** The analysis starts at the Class/Method provided by the user at the end of this prompt.
- **Persistence Layer Location:** All repositories and entity mappings are located within the
  `src/main/java/com/vitaly/gef/infrastructure/persistence` directory.
- **Naming Conventions:** Database entities strictly follow the suffix `Entity` (e.g., `UserEntity`). They contain JPA
  annotations (`@Table`, `@Column`, `@JoinColumn`, `@ManyToOne`, etc.) that define the database schema.

# STEPS TO FOLLOW

1. **Static Analysis & Entity Discovery:**
    - Trace the execution flow of the provided method.
    - Identify all Spring Data Repositories invoked (directly or indirectly).
    - Locate the corresponding `Entity` classes within `src/main/java/com/vitaly/gef/infrastructure/persistence`.

2. **Schema Mapping:**
    - Extract the exact database **Table Names** and **Column Names** from the JPA annotations in those entities.
    - Pay special attention to implicit column names (if `@Column(name=...)` is missing, derive it from the field name
      using standard Spring Boot snake_case naming strategies).
    - Identify foreign keys or join tables involved in relationships (`@ManyToMany`, `@OneToMany`).

3. **Behavioral Analysis ("The Why"):**
    - Analyze the business logic inside the method.
    - Explain *why* each piece of data is being read, inserted, or updated. (e.g., "The column `status` is read to
      validate if the account is active before processing the payment").

# DOCUMENTATION TEMPLATE

Your response must be wrapped in a single, clean Markdown code block representing a file to be saved under the `docs/`
directory. Use the following structure: [template.md](template.md)

## Constraints

- Do not run tests.
- Do not modify any existing file.
- Write the output file only.

# OUTPUT CHECKLIST

- [ ] The output should be a md file `docs/[feature]/persistence.md`

