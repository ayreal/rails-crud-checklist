## Simple CRUD With Rails Checklist!!!

- Create project: `rails new project_name`
- Create table/model: `rails g model Model_name attribute:datatype --no-test-framework`
- Create relationships in model
- Run migrations
- Seed DB
- Check objects/relationships in `console rails c`
- Draw routes with `resources: routes`
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
    @post = post.new(post_params(:title,:description))
    if @post.valid?
      @post.save
      redirect_to post_path(@post)
    else
      render :new
    end
  end

def update
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
