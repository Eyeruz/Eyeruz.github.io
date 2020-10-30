---
layout: post
title:      "Rails Project"
date:       2020-10-30 18:02:04 +0000
permalink:  rails_project
---



My rails project is just an upgraded version of my sinatra project.. I kept the same theme of an online store and added a couple more tables to it, i originally had a customers table, items table, and a customer items table i used as a join table and also a bag (i also added a :quantity to my customer items table so that i could keep track of how many items were being added to the bag and got the total).. and this time i added a category table and used a category items table to join the category and item tables together.
[extraction from app/models/category.rb]
class Category < ApplicationRecord
has_many :category_items
has_many :items, through: :category_items
end
— — — — — — — — — — — — — — —
[extraction from app/models/category_item.rb]
class CategoryItem < ApplicationRecord
belongs_to :category
belongs_to :item
end
— — — — — — — — — — — — —
[extraction from app/models/customer.rb]
class Customer < ApplicationRecord
has_secure_password
has_many :customer_items
end
— — — — — — — — — — -
[extraction from app/models/item.rb]
class Item < ApplicationRecord
has_many :category_items
has_many :categories, through: :category_items
has_many :customer_items
has_many :customers, through: :customer_items
end
— — — — — — — — -
[extraction from app/models/customer_item.rb]
class CustomerItem < ApplicationRecord
belongs_to :item
belongs_to :customer
end
— — — — — — — — — —
i also used omniauth for the first time and used facebook as an external outlet to let users login without signing up directly to the site (which was very cool)
[extraction from app/controllers/sessions_controller.rb]
def facebook
@customer = Customer.find_or_create_by(uid: auth[‘uid’]) do |u|
u.username = auth[‘info’][‘name’]
u.email = auth[‘info’][‘name’] + ‘@email.com’
u.image = auth[‘info’][‘image’]
u.password = SecureRandom.hex
end
session[:user_id] = @customer.id
redirect_to items_path
end
scope methods were a requirement so i did an :order_by_name in the item model
[extraction from my app/models/item.rb]
scope :order_by_name, -> { order(name: :asc)}
and used that in my index page to order all my items alphabetically
[extraction from my app/controllers/items_controller.rb]
def index
@items = Item.order_by_name
end
validations were the simplest part of the project i validated the user name email and used bcrypt gem to secure the password
[extraction from my app/models/customer.rb]
has_secure_password → (bcrypt method)
has_many :customer_items
has_many :items, through: :customer_items
validates :username, presence: true, uniqueness: true
validates :email, presence: true, uniqueness: true
byebug was my bff throughout this project got me out of a lot of errors
[extraction from my app/Gemfile]
gem ‘byebug’, platforms: [:mri, :mingw, :x64_mingw]
the before_action method helped me not only keep my code DRY but also saved me time from writing the same code over and over again.
[extraction from my app/controllers/items_controller.rb]
before_action :find_item, only: [:edit, :update, :show, :destroy]
before_action :find_category, only: [:edit, :update]
before_action :must_be_logged_in
skip_before_action :must_be_logged_in, only: [:index]
helper_methods did exactly that, helped me save plenty of time and definitely kept my code very DRY.
[extraction from my app/controllers/application_controller.rb]
helper_method :logged_in?, :flash_error, :current_customer
It was a requirement in this project to have a nested new form and either a show or index form. i decided to do all three..
[extraction from my app/config/routes.rb]
resources :categories do
resources :items
end
redirect_to category_item_path(@category, @item) → (redirection to a nested items from inside of a categories resource)
i created my own routes for the signup, and signin process
[extraction from my app/config/routes.rb]
root ‘categories#index’
get ‘/logout’, to: ‘sessions#destroy’
get ‘/signup’, to: ‘customers#new’
post ‘/signup’, to: ‘customers#create’
get ‘/signin’, to: ‘sessions#new’
post ‘/signin’, to: ‘sessions#create’
post’/edit’, to: ‘items#edit’
post ‘/delete’, to: ‘customer_items#destroy’
get ‘/checkout’, to: ‘customer_items#checkout’
get ‘/auth/facebook/callback’ => ‘sessions#facebook’
get ‘/userinfo’, to: ‘sessions#userinfo’
it was also very simple to do since i grasped the idea of get and post in the sinatra era (lol)
also side note i descoverd that link_to and button_to are complete opposites (funny i thought i messed up my whole entire and couldnt figure out excatly what i was doing wrong and was stuck for hours try to figure out what i did wrong)
link_to → get method
button_to → post method
[extraction from my app/views/application.html.erb file]
<h6><p>Dont have an account? <%= link_to “Sign Up”,’/signup’ %></p>
<p>Already have an account? <%= link_to “Sign In”, ‘/signin’ %></p>
<p> View all items <%= link_to “All items”, items_path %> </p>
<p> <%= button_to(‘Log in with Facebook!’, ‘/auth/facebook’) %> </p></h6>
the shuffle method
@category = Category.find(params[:customer][:items][0][:category_id])
@item = Item.create(item_params)
@item.creator = current_customer.id
@item.quantity = params[:customer][:items][0][:quantity].to_i
if @item.save
@item.categories << @category → (creating an array of items that belong to a specific category.)
i used materialize for my css and added cute background i found on tumblr after searching up vintage mirrors, after i found the perfect picture i used the colors of the background match the colors of the buttons to give better a appearance
[extraction from app/views/layouts/application.html.erb]
<! — Compiled and minified CSS →
<link rel=”stylesheet” href=”https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/css/materialize.min.css">
<%= stylesheet_link_tag ‘application’, media: ‘all’, ‘data-turbolinks-track’: ‘reload’ %>
<! — Compiled and minified JavaScript →
<%= javascript_pack_tag ‘application’, ‘data-turbolinks-track’: ‘reload’ %>
<script src=”https://code.jquery.com/jquery-3.5.1.min.js" integrity=”sha256–9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0=” crossorigin=”anonymous”></script>
<script src=”https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js"></script>
thanks for reading :)
