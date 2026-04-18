# RSpec Testing Patterns

## Feature Tests

Feature tests use Capybara and check for end-to-end functionality.

```ruby
# frozen_string_literal: true

# spec/features/groups/join_spec.rb
RSpec.describe "Group joining" do
  include Deps["repos.user_repo"]

  let(:user) { user_repo.create(email: "test@example.com") }
  
  before do
    login_as(user)
  end

  it "allows a user to request to join a group" do
    visit "/groups"
    click_button "Join Group"
    expect(page).to have_content("Join request has been sent")
  end
end
```

## Action Tests

Unit test actions by calling them with request parameters.

```ruby
# frozen_string_literal: true

# spec/unit/actions/groups/show_spec.rb
RSpec.describe HanProject::Actions::Groups::Show do
  subject(:action) { described_class.new }
  let(:params) { {id: 1} }

  it "is successful" do
    response = action.call(params)
    expect(response.status).to eq(200)
    expect(response.body).to include("Group Name")
  end
end
```

## View Tests

Unit test views by calling them with locals.

```ruby
# frozen_string_literal: true

# spec/unit/views/groups/show_spec.rb
RSpec.describe HanProject::Views::Groups::Show do
  subject(:view) { described_class.new }
  let(:group) { double("group", name: "Engineering") }

  it "renders the group name" do
    output = view.call(group: group).to_s
    expect(output).to include("Engineering Group")
  end
end
```

## Repository & Operation Tests

Unit tests check for logic and data access.

```ruby
# frozen_string_literal: true

# spec/unit/repos/group_repo_spec.rb
RSpec.describe HanProject::Repos::GroupRepo do
  let(:repo) { described_class.new }

  it "creates a group" do
    group = repo.create(name: "Test Group")
    expect(group.name).to eq("Test Group")
  end
end
```

## Authentication in Tests

Use `Warden` helpers to login a user in feature tests.

```ruby
# frozen_string_literal: true

before do
  login_as(user)
end
```
