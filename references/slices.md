# Hanami 2.3 Slices

Guide: https://github.com/hanami/guides/tree/main/content/v2.3/app/slices

Slices are distinct modules of your application, separating business domains or technical concerns.

## Creating a Slice

```shell
bundle exec hanami generate slice api
```

## Slice Imports & Exports

Slices can share components by importing from other slices.

### Exporting (in the source slice)

```ruby
# frozen_string_literal: true

# config/slices/cdn.rb
module CDN
  class Slice < Hanami::Slice
    export ["book_covers.purge"]
  end
end
```

### Importing (in the destination slice)

```ruby
# frozen_string_literal: true

# config/slices/admin.rb
module Admin
  class Slice < Hanami::Slice
    import from: :cdn
  end
end
```

Using the imported component:

```ruby
# frozen_string_literal: true

include Deps["cdn.book_covers.purge"]
```

## Slice Settings

Slices can have their own settings in `slices/[slice_name]/config/settings.rb`.

```ruby
# frozen_string_literal: true

module CDN
  class Settings < Hanami::Settings
    setting :cdn_api_key, constructor: Types::String
  end
end
```

Access via `include Deps["settings"]` within the slice.

## Slice Routing

Slices can be mounted in `config/routes.rb`:

```ruby
# frozen_string_literal: true

slice :admin, at: "/admin" do
  get "/users", to: "users.index"
end
```

Or have their own routes in `slices/[slice_name]/config/routes.rb`.
