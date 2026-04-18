---
name: ruby-hanami-expert
description: Expert-level Ruby development using Hanami 2.3, Dry-rb, and ROM-rb. Use for adding features, fixing bugs, and refactoring business logic in a Hanami project.
---

# Ruby Hanami Expert

This skill provides expert guidance for building high-quality, maintainable, and efficient Ruby code using Hanami 2.3, Dry-rb, and ROM-rb.

## Core Development Lifecycle

1.  **Define Params**: Validate and coerce incoming request data in actions using `params` blocks or specialized classes.
2.  **Encapsulate Business Logic**: Extract logic into `Dry::Operation` objects in `app/operations/`.
3.  **Data Access via Repositories**: Use Repositories for data modification and high-level querying.
4.  **Schema & Relations**: Define database structure, associations, and chainable scopes in Relations.
5.  **Handle Results in Actions**: Match on `Success` and `Failure` from operations to determine response.
6.  **Rich Views & Templates**: Use Exposures to source data and Parts to encapsulate view-specific logic.
7.  **Organize via Slices**: Partition large applications into Slices for better isolation and modularity.
8.  **Verified Delivery**: Comprehensive testing using RSpec, including feature, action, view, and unit tests.

## Key Architectural Patterns

- **Dependency Injection**: Use `include Deps["..."]` to inject components from the container.
- **Monads for Control Flow**: Use `Success` and `Failure` for expected business outcomes.
- **Functional Querying**: Leverage ROM's expression-based query syntax for complex database interactions.
- **Clean Actions**: Delegate almost all logic to operations; handle only request/response concerns.

## Quick References

- **Actions and Params**: [actions.md](references/actions.md) - Request handling, validation, and status codes.
- **Business Operations**: [operations.md](references/operations.md) - Domain logic and result matching.
- **Data Repos & Relations**: [repos.md](references/repos.md) - ROM, schemas, associations, and queries.
- **Views, Parts & Context**: [views.md](references/views.md) - Rendering, exposures, and view logic.
- **Application Slices**: [slices.md](references/slices.md) - Modularity, imports, and exports.
- **Test Strategy**: [testing.md](references/testing.md) - RSpec patterns for all layers.

## Project Context

- **Base Classes**: `HanProject::Action`, `HanProject::AuthenticatedAction`, `HanProject::Operation`, `HanProject::DB::Repo`, `HanProject::DB::Relation`.
- **Session Helper**: Use `user_id_from_session(request)` in actions.
- **Authentication**: Powered by `Warden`. Requires `AuthenticatedAction` for protected routes.
- **Database**: MySQL managed by `ROM-rb` and `Sequel`.

## Coding Standards

- **Frozen String Literals**: Always include `# frozen_string_literal: true`.
- **Module Nesting**: Follow Hanami 2.x standard directory structure for autoloading.
- **Naming**: Descriptive and idiomatic Ruby naming conventions.
