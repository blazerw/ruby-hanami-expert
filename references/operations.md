# Dry-Operation Business Logic

Guide: https://github.com/hanami/guides/blob/main/content/v2.3/operations/overview.md

Operations encapsulate business logic and multiple repository calls, returning a `Dry::Monads::Result`.

## Base Operation

All operations should inherit from `HanProject::Operation`.

```ruby
# frozen_string_literal: true

module HanProject
  class Operation < Dry::Operation
  end
end
```

## Example Operation

```ruby
# frozen_string_literal: true

module HanProject
  module Operations
    module Groups
      class Join < Operation
        include Deps[
          "repos.request_repo",
          "mailers.admin_request_mailer"
        ]

        def call(user_id:, group_id:)
          # Check for existing pending request
          if request_repo.pending_for_user_and_group(user_id, group_id)
            return Failure(:already_requested)
          end

          # Create request
          req = request_repo.create_request(
            user_id: user_id,
            group_id: group_id
          )

          # Send email
          admin_request_mailer.deliver(req.id)

          Success(req)
        end
      end
    end
  end
end
```

## Usage in Action

Use `match` to handle success and failure cases cleanly.

```ruby
# frozen_string_literal: true

def handle(request, response)
  join_group.call(user_id: user_id, group_id: group_id).match do |m|
    m.success { |req| 
      response.flash[:notice] = "Request sent!"
      response.redirect_to routes.path(:group, id: group_id)
    }
    m.failure(:already_requested) {
      response.flash[:error] = "Request already exists."
      response.redirect_to routes.path(:group, id: group_id)
    }
    m.failure { |error|
      # Handle other failures
      response.status = 422
      response.body = {error: error}.to_json
    }
  end
end
```

## Error Handling Patterns

- **Symbolic Errors**: `Failure(:not_found)`
- **Validation Errors**: `Failure(validation_result.errors)`
- **Detailed Error Objects**: `Failure(error_code: 403, message: "Unauthorized")`
