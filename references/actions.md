# Hanami 2.3 Action Patterns

Guide: https://github.com/hanami/guides/tree/main/content/v2.3/actions

## Basic Action Structure

Actions should inherit from `HanProject::Action` or `HanProject::AuthenticatedAction`.

```ruby
# frozen_string_literal: true

module HanProject
  module Actions
    module Groups
      class Show < AuthenticatedAction
        include Deps["repos.group_repo"]

        def handle(request, response)
          group = group_repo.get(request.params[:id])
          response.render(view, group: group)
        end
      end
    end
  end
end
```

## Parameter Validation

Use `params` to define and validate input. Access nested params safely with `dig`.

```ruby
# frozen_string_literal: true

params do
  required(:id).filled(:integer)
  optional(:filters).hash do
    optional(:query).maybe(:string)
  end
end

def handle(request, response)
  # halt returns a response immediately
  halt 422, {errors: request.params.errors}.to_json unless request.params.valid?
  
  id = request.params[:id]
  query = request.params.dig(:filters, :query)
  # ...
end
```

For complex params, use a separate class in `params/`:

```ruby
# frozen_string_literal: true

# app/actions/groups/params/create.rb
module HanProject
  module Actions
    module Groups
      module Params
        class Create < Hanami::Action::Params
          params do
            required(:group).hash do
              required(:name).filled(:string)
            end
          end
        end
      end
    end
  end
end

# In the action:
params Params::Create
```

## Exception Handling

Handle exceptions at the action or base action level.

```ruby
# frozen_string_literal: true

class Show < AuthenticatedAction
  handle_exception RecordNotFound => 404
  handle_exception StandardError => :handle_error

  private

  def handle_error(request, response, exception)
    response.status = 500
    response.body = {error: "Internal Server Error"}.to_json
  end
end
```

## Request & Response

- **Status**: `response.status = 201`
- **Body**: `response.body = "..."`
- **Format**: `response.format = :json`
- **Headers**: `response.headers["X-Custom"] = "..."`
- **Redirect**: `response.redirect_to routes.path(:root)`
- **Flash**: `response.flash[:notice] = "Done"`
- **Cookies**: `response.cookies["key"] = "val"`
- **Session**: `request.session[:user_id] = 1`

## Body Parsers

Register custom parsers in `config/app.rb`:

```ruby
# frozen_string_literal: true

config.middleware.use :body_parser, :json # Built-in
config.middleware.use :body_parser, [json: ["application/scim+json"]]
```

## Dependency Injection

Use `Deps` to inject repositories, operations, and other components.

```ruby
# frozen_string_literal: true

include Deps[
  "repos.group_repo",
  "operations.create_group"
]
```
