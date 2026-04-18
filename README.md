# Ruby Hanami Expert Skill

Expert-level (aspirational) Ruby development guidance for building high-quality, maintainable, and efficient applications using **Hanami 2.3**, **Dry-rb**, and **ROM-rb**.

This skill is designed to assist developers in adding features, fixing bugs, and refactoring business logic within a modern Ruby architectures, specifically optimized for projects based on Hanami.

## 🚀 Key Features

- **Hanami 2.3 Excellence**: Deep understanding of Hanami's slice-based architecture, actions, and views.
- **Dry-rb Integration**: Leverage `dry-validation`, `dry-operation`, and `dry-monads` for robust business logic and control flow.
- **ROM-rb & Data Persistence**: Expert guidance on using ROM (Ruby Object Mapper) for database relations, repositories, and complex querying.
- **Architectural Best Practices**: Implementation of Dependency Injection (`dry-system`), Functional Querying, and clean separation of concerns.
- **Comprehensive Testing**: Patterns for RSpec across all layers, from unit tests to full feature specs.

## 🛠 Architectural Patterns

This skill promotes a clean, decoupled architecture:

- **Slices**: Partitioning applications into isolated, modular components.
- **Operations**: Encapsulating domain logic into reusable, result-oriented objects.
- **Repositories & Relations**: Decoupling data access from business logic using ROM.
- **Actions**: Thin, request-handling layers that delegate to operations.
- **Views & Parts**: Rich, logic-aware view components for complex UI rendering.

## 📚 Reference Guides

The skill includes detailed documentation on core development areas:

- [Actions and Params](references/actions.md) - Request handling and validation.
- [Business Operations](references/operations.md) - Domain logic and `Success`/`Failure` matching.
- [Data Repos & Relations](references/repos.md) - Database schemas and querying.
- [Views, Parts & Context](references/views.md) - Advanced rendering techniques.
- [Application Slices](references/slices.md) - Modularity and isolation.
- [Test Strategy](references/testing.md) - RSpec patterns for robust verification.

## 💻 Usage

Gemini wrote this readme, mostly, and felt the need to give Gemini specific instructions. Should work nicely with any agent.

### Gemini

To activate this skill in your Gemini CLI session, use the `activate_skill` tool:

```bash
activate_skill(name: "ruby-hanami-expert")
```

Once activated, Gemini CLI will follow the expert guidance and architectural standards defined in this skill to assist with your Ruby development tasks.

## 📝 Coding Standards

- **Modern Ruby**: Always uses `# frozen_string_literal: true`.
- **Autoloading**: Follows Hanami 2.x standard directory and naming conventions.
- **Type Safety & Validation**: Prioritizes explicit validation and coerced parameters.
- **Result-Oriented Flow**: Uses `dry-monads` for clear success and error handling.

