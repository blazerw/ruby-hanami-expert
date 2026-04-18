# ROM Repositories and Relations

Guide: https://github.com/hanami/guides/blob/main/content/v2.3/database/overview.md

## Repositories

Repositories are the primary interface for data access.

```ruby
# frozen_string_literal: true

module HanProject
  module Repos
    class GroupRepo < HanProject::DB::Repo
      def get(id)
        groups.by_pk(id).one!
      end

      def find_by_name(name)
        groups.where(name: name).one
      end
      
      def create(attributes)
        groups.changeset(:create, attributes).commit
      end

      def update(id, attributes)
        groups.by_pk(id).changeset(:update, attributes).commit
      end
    end
  end
end
```

## Relations

Relations define the schema, associations, and low-level queries.

```ruby
# frozen_string_literal: true

module HanProject
  module Relations
    class Groups < HanProject::DB::Relation
      schema(:groups, infer: true) do
        associations do
          has_many :users_groups
          has_many :users, through: :users_groups
          belongs_to :organization
        end
      end

      # Scopes are chainable query methods
      def recent
        where { created_at > Date.today - 30 }
      end
      
      def active
        where(status: "active")
      end
    end
  end
end
```

### Schema Customization

Override inferred types or define complex coercions:

```ruby
# frozen_string_literal: true

schema :books, infer: true do
  attribute :status, Types::String, read: Types::Coercible::Symbol
  attribute :metadata, Types::PG::JSONB
end
```

### Associations

- `has_many :books`
- `belongs_to :author`
- `has_many :tags, through: :book_tags`

### Query Building

ROM supports Hash-based and expression-based syntax:

```ruby
# frozen_string_literal: true

# Hash-based (simple)
groups.where(name: "Engineering")

# Expression-based (complex)
groups.where { (created_at > Date.today) & (name.ilike("E%")) }

# Projection (Selection)
groups.select(:id, :name)

# Ordering
groups.order { [created_at.desc, name.asc] }
```

## SQL and Custom Logic

Use `Sequel.lit` for raw SQL when needed:

```ruby
# frozen_string_literal: true

def search(query:)
  sql = "(SELECT * FROM groups WHERE name LIKE :query)"
  groups.read(Sequel.lit(sql, query: "%#{query}%")).to_a
end
```

## Migrations

Guide: https://github.com/hanami/guides/blob/main/content/v2.3/database/migrations.md

Create migrations using `hanami db create_migration`.

