## Simple CRUD With Rails Checklist!!!

- Create project: `rails new project_name`
- Create table/model: `rails g model Model_name_singular attribute:datatype --no-test-framework` (or `rails g resources Model_name_singular attribute:datatype --no-test-framework` for tables, models, views, and routes)
- Run migrations
- Create relationships in model
- Seed DB (optional)
- Check objects/relationships in `console rails c`
- Draw routes with `resources: routes`, remove unnecessary routes for simplicity
- Check routes with `rake:routes`
- Run server `rails s` (refresh any time you make changes to routes)
- Create controllers (name plural), inherit from ApplicationController
- Make show pages in views, under their respective model names (plural)
- Create controller actions for CRUD/other routes as necessary (see below)
- If you need to delete, check that your assets > javascript > application.js has the following code and that `gem 'jquery-rails'` and `gem 'jquery-ui-rails'` are in your gemfile. Run bundle install.
`//= require jquery`
`//= require jquery_ujs`

Check that layouts/application.html.erb contains:

```HTML
<%= stylesheet_link_tag    'application', media: 'all' %>
<%= javascript_include_tag :application %>
<%= csrf_meta_tags %>
```

- Add validations to models



## CRUD Controller Actions With Validations

```ruby

  def index
    @posts = Post.all
  end

 def show
    @post = Post.find(params[:id])
  end

  def new
    @post = Post.new    # required for form_for
  end

 def create
    @post = Post.new(post_params)
    if @post.save       # same as checking if post.valid? first
      redirect_to post_path(@post)
    else
      render :new
    end
  end

  def edit
    @post = Post.find(params[:id])
  end

  def update
    @post = Post.find(params[:id])
    if @post.update(post_params)
      redirect_to post_path(@post)
    else
      render :edit
    end
  end

  def destroy
    @post = Post.find(params[:id]).destroy
    redirect_to posts_path
  end

  private

  def post_params    # strong params method, required for form_for
    params.require(:post).permit(:title, :description)
  end

  ```

  ## Basic Error Messages for Validations

  Printing out a list of error messages at the top of a page

  ```HTML
  <% if @song.errors.any? %>
  <div id="error_explanation">
    <h2>There were some errors:</h2>
    <ul>
      <% @song.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
    </ul>
  </div>
<% end %>
```

  Example form field to highlight erroneous field.

  ```HTML
  <div class="field<%= ' field_with_errors' if @song.errors[:title].any? %>">
    <%= f.label :title %>
    <%= f.text_field :title %>
  </div>
  ```

## Associating Models with Basic Forms

```HTML
Character Info: <br/>
<%= form_for @character do |f| %>
  <%= f.label :name %>
  <%= f.text_field :name %> <br />
Show Info: <br/>
  <%= f.fields_for :make_show do |j| %>
    <%= j.label :title %>
    <%= j.text_field :title %><br />
  <% end %>
  <%= f.submit %>
<% end %>
```    

```ruby
# in the character controller, start with

def create
  @character = Character.new(character_params)
  # the association will be made once you receive hydrated object back from #make_show= and call save in the create method
end

private

def character_params
  params.require(:character).permit(:name,make_show:[:title])
end
```

```ruby
# In the Character model

def make_show=(arg)
  build_show(args)  #OR notes.build (based on relationship)
  # show is not created, but an instance is created and associated with the character
  # object is passed back to the controller, which will call #save and persist it

end

```
