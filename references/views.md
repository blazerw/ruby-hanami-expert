# Hanami 2.3 View Patterns

Guide: https://github.com/hanami/guides/tree/main/content/v2.3/views

## Views & Exposures

Views source data for templates via exposures.

```ruby
# frozen_string_literal: true

module HanProject
  module Views
    module Groups
      class Show < HanProject::View
        include Deps["repos.group_repo"]

        # Input 'id' passed from action: response.render(view, id: 1)
        expose :group do |id:|
          group_repo.get(id)
        end

        # Depends on another exposure
        expose :members do |group|
          group_repo.members_for(group.id)
        end
        
        # Private exposure (not available in template)
        private_expose :config do
          {theme: "dark"}
        end
      end
    end
  end
end
```

### Exposure Options

- `layout: true`: Make exposure available to the layout.
- `decorate: false`: Opt out of part decoration.
- `as: :other_name`: Use a different part class for decoration.

## Parts

Parts wrap domain objects to provide view-specific logic.

```ruby
# frozen_string_literal: true

# app/views/parts/group.rb
module HanProject
  module Views
    module Parts
      class Group < HanProject::Views::Part
        # Delegate to the wrapped value (group)
        def display_name
          "#{name} Group"
        end

        # Access routes/helpers via context
        def path
          context.routes.path(:group, id: id)
        end
        
        # Render a partial from a part
        def summary
          render("groups/summary")
        end
        
        # Decorate nested attributes
        decorate :organization
      end
    end
  end
end
```

## Context

The context provides shared facilities (routes, flash, session, inflector) to all view components.

```ruby
# frozen_string_literal: true

# app/views/context.rb
module HanProject
  module Views
    class Context < Hanami::View::Context
      include Deps["repos.user_repo"]

      def current_user
        @current_user ||= user_repo.get(session[:user_id]) if session[:user_id]
      end
      
      # Decorate context methods
      decorate :current_user, as: :user
    end
  end
end
```

## Templates

Templates are usually ERB files in `app/templates/`.

```erb
<h1><%= group.display_name %></h1>
<ul>
  <% members.each do |member| %>
    <li><%= member.name %></li>
  <% end %>
</ul>
```
