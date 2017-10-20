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

```ruby
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
    @post = Post.new(post_params(:title,:description))
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
    if @post.update(post_params(:title,:description))
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

  def post_params(*args)    # strong params method, required for form_for
    params.require(:post).permit(*args)
  end

  ```

  ## Basic Error Messages for Validations

  Printing out a list of error messages at the top of a page

  ```ruby
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

  ```ruby
  <div class="field<%= ' field_with_errors' if @song.errors[:title].any? %>">
    <%= f.label :title %>
    <%= f.text_field :title %>
  </div>
  ```
