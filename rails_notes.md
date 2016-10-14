


---------CREATE PROJECT------------------------------------------------------------------------------------------------
 rails new (name) --> create new project
-----------------------------------------------------------------------------------------------------------------------

---------ADD BOOTSTRAP to rails project--------------------------------------------------------------------------------
/Gemfile.rb

	gem 'bootstrap-sass', '3.3.6'
                 or
        gem 'bootstrap-sass'  # will install the latest version

then run:

        bundle install


Create custom.scss file at: app/assets/stylesheets/custom.scss

custom.scss:

	@import "bootstrap-sprockets";
	@import "bootstrap";

-The two lines include the entire Bootstrap CSS framework

-----------------------------------------------------------------------------------------------------------------------





------ASSET PIPELINE---------------------------------------------------------------------------------------------------
-From the perspective of a typical Rails developer, 
 there are three main features to understand about the asset pipeline: 
      ( asset directories, manifest files, and preprocessor engines. )

-----------------------------------------------------------------------------------------------------------------------



------Rails environments-----------------------------------------------------------------------------------------------

Rails comes equipped with three environments:  TEST, DEVELOPMENT, and PRODUCTION.

 The default environment for the Rails console is DEVELOPMENT

-----------------------------------------------------------------------------------------------------------------------


------ ternary operator------------------------------------------------------------------------------------------------

	if wow == '1'
  		wow = 'wow'
	else
  		wow = 'not_wow'
	end


one line using the ternary operator:

	wow == '1' ? wow = 'wow' : wow = 'not_wow'


-----------------------------------------------------------------------------------------------------------------------



------SOME RAILS SHORTCUTS---------------------------------------------------------------------------------------------

 rails server	      -->     rails s
 rails console	      -->     rails c
 rails generate       -->     rails g
 rails test	      -->     rails t
 bundle install       -->     bundle
-----------------------------------------------------------------------------------------------------------------------


---------GENERATE CONTROLLER (AND VIEW)--------------------------------------------------------------------------------
 rails generate controller StaticPages home help

   -creates    app/controllers/static_pages_controller.rb
   -creates    app/views/static_pages/home.html.erb	
   -creates    app/views/static_pages/help.html.erb

-------DESTROY
 rails destroy  controller StaticPages home help

   -removes    app/controllers/static_pages_controller.rb
   -removes    app/views/static_pages/home.html.erb	
   -removes    app/views/static_pages/help.html.erb

--
app/controllers/application_controller.rb

	Application controller - base class of all controllers

-----------------------------------------------------------------------------------------------------------------------

---------GENERATE MODEL------------------------------------------------------------------------------------------------

 rails generate model User name:string email:string

 -By passing the optional parameters name:string and email:string, 
  we tell Rails about the two attributes we want, along with which types those attributes should be(in this case, string)


(Note that, in contrast to the plural convention for controller names, model names are singular: 
 a Users controller, but a User model.)


-----
  
 rails generate model Micropost content:text user:references

 -This migration leads to the creation of the Micropost model.

 -In addition to inheriting from ApplicationRecord as usual, 
  the generated model includes a line indicating that a micropost belongs_to a user, 
  which is included as a result of the Åuuser:referencesÅv

The generated Micropost model:
 --app/models/micropost.rb


   class Micropost < ApplicationRecord
     belongs_to :user
   end

-----------------------------------------------------------------------------------------------------------------------

---------GENERATE MAILER-----------------------------------------------------------------------------------------------

As with models and controllers, we can generate a mailer using rails generate:

 rails generate mailer UserMailer account_activation


-The command also generates two view templates for each mailer, 
 one for plain-text email and one for HTML email



--app/views/user_mailer/account_activation.text.erb         (The generated account activation text view)

UserMailer#account_activation

<%= @greeting %>, find me in app/views/user_mailer/account_activation.text.erb


--app/views/user_mailer/account_activation.html.erb         (The generated account activation HTML view)

<h1>UserMailer#account_activation</h1>

<p>
  <%= @greeting %>, find me in app/views/user_mailer/account_activation.html.erb
</p>


--app/mailers/application_mailer.rb                         (The generated application mailer)

class ApplicationMailer < ActionMailer::Base
  default from: "from@example.com"
  layout 'mailer'
end



-----------------------------------------------------------------------------------------------------------------------



---------SCAFFOLD------------------------------------------------------------------------------------------------------
 rails generate scaffold User name:string email:string

 -Creates CRUD

 -The argument of the scaffold command is the singular version of the resource name (in this case, User), 
  together with optional parameters for the data modelÅfs attributes
-----------------------------------------------------------------------------------------------------------------------

----------DATABASE-----------------------------------------------------------------------------------------------------

 -In the Unix tradition, the Make utility has played an important role in building executable programs from source code. 
  Rake is Ruby make, a Make-like language written in Ruby.

 -itÅfs important to ensure that the command uses the version of Rake corresponding to the Rails applicationÅfs Gemfile, 
  which is accomplished using the Bundler command "bundler exec"


rake ->
ex:  rake db:migate  
          OR 
     bundle exec rake db:migrate


db:create            creates the database for the current env
db:create:all        creates the databases for all envs
db:drop              drops the database for the current env
db:drop:all          drops the databases for all envs
db:migrate           runs migrations for the current env that have not run yet
db:migrate:up        runs one specific migration
db:migrate:down      rolls back one specific migration
db:migrate:status    shows current migration status
db:rollback          rolls back the last migration
db:forward           advances the current schema version to the next one
db:seed (only)       runs the db/seed.rb file
db:schema:load       loads the schema into the current env's database
db:schema:dump       dumps the current env's schema (and seems to create the db as well)
db:setup             runs db:schema:load, db:seed
db:reset             runs db:drop db:setup
db:migrate:redo      runs (db:migrate:down db:migrate:up) or (db:rollback db:migrate) depending on the specified migration
db:migrate:reset     runs db:drop db:create db:migrate

-----------------------------------------------------------------------------------------------------------------------



--------------RAILS SERVER---------------------------------------------------------------------------------------------

 rails server -b $IP -p $PORT    # Use `rails server` if running locally.
  
   ex: rails -b 0.0.0.0 -p 3001   #will run on your_ip:3001 (in my case: http://192.168.2.124:3001)

-----------------------------------------------------------------------------------------------------------------------


-----ExecJS::ProgramError   FIX----------------------------------------------------------------------------------------

There is a problem with coffee-script-source 1.9.0 running on windows.

It seems you have to add this to your gemfile:

gem 'coffee-script-source', '1.8.0'
then do

bundle update coffee-script-source

#stackoverflow
-----------------------------------------------------------------------------------------------------------------------


----RETRIEVING RECORDS-------------------------------------------------------------------------------------------------


app/controllers/users_controller.rb

	@users = User.all

     -asks the User model to retrieve a list of all the users from the database
     -and then places them in the variable @users (pronounced Ågat-usersÅh)


app/views/users/index.html.erb

	<% @users.each do |user| %>
		<tr>
			<td><%= user.name %></td>
		</tr>
	<% end %>

     - Variables that start with the @ sign, called instance variables, are automatically available in the views; in this case
       the index.html.erb view iterates through the @users list and outputs a line of HTML for each one.
     

------------------------------------------------------------------------------------------------------------------------


----------------MODEL---------------------------------------------------------------------------------------------------

app/models/___.rb

----STRING AND TEXT--------------------------------------------------------------------------------

 rails generate User name:string email:string comment:text


As a general rule of thumb, use :string for short text input (username, email, password, etc.) 
and use :text for longer expected input such as descriptions, comment content, etc.


---------------------------------------------------------------------------------------------------

---Associations between different data models-------------------------------------------------------

	app/models/user.rb

		class User < ApplicationRecord
  			has_many :microposts
		end

	app/models/micropost.rb

		class Micropost < ApplicationRecord
  			belongs_to :user
		end


	# A user has many microposts.
	# A micropost belongs to a user.

		MICROPOSTS		                        	USERS
  	id       content       user_id			   id         name          email
 
  	1          wow            1                        1         nice_guy       niceguy@wow.com
  	2          wow_2          1                        2         good_guy       goodguy@wow.com	
  	3          wow_3          2                         


-----Using the belongs_to/has_many

instead of:

	Micropost.create
	Micropost.create!
	Micropost.new

use:
        user = User.find(1)
	user.microposts.create
	user.microposts.create!
	user.microposts.build


 -These latter methods constitute the idiomatically correct way to make a micropost, namely, 
  through its association with a user. When a new micropost is made in this way, 
  its user_id is automatically set to the right value.


------METHODS

METHOD				PURPOSE

micropost.user			Returns the User object associated with the micropost
user.microposts			Returns a collection of the userÅfs microposts
user.microposts.create(arg)	Creates a micropost associated with user
user.microposts.create!(arg)	Creates a micropost associated with user (exception on failure)
user.microposts.build(arg)	Returns a new Micropost object associated with user
user.microposts.find_by(id: 1)	Finds the micropost with id 1 and user_id equal to user.id


To get code like @user.microposts.build to work:

	class Micropost < ApplicationRecord
		belongs_to :user
	end


	class User < ApplicationRecord
  	has_many :microposts
	end

------Dependent: destroy

if a user is destroyed, the userÅfs microposts should be destroyed as well.
We can arrange for this behavior by passing an option to the has_many association method:

	class User < ApplicationRecord
  		has_many :microposts, dependent: :destroy
	end

 -Here the option dependent: :destroy arranges for the dependent microposts to be destroyed when 
  the user itself is destroyed. This prevents userless microposts from being stranded in the 
  database when admins choose to remove users from the system.



-------class_name & foreign_key

app/models/user.rb

class User < ApplicationRecord
   has_many :active_relationships, class_name:  "Relationship",
                                  foreign_key: "follower_id",
                                  dependent:   :destroy

end


----------------------------------------------------------------------------------------------------


------REFERENCES-----------------------------------------------------------------------------------


 rails generate model Micropost content:text user:references

 -the generated model includes a line indicating that a micropost belongs_to a user, 
  which is included as a result of the user:references argument.
 -automatically adds a user_id column (along with an index and a foreign key reference) 
  for use in the user/micropost association.

app/models/micropost.rb

class Micropost < ApplicationRecord
   belongs_to :user
end


---------------------------------------------------------------------------------------------------


-------default_scope-------------------------------------------------------------------------------

class Micropost < ApplicationRecord
	default_scope -> { order(created_at: :desc) }
end


 -arrange the order of records



-----TAKE command

	User.order(:created_at).take(6)

 -select just the first six users



---------------------------------------------------------------------------------------------------




----attr_accessor----------------------------------------------------------------------------------

app/models/example_user.rb

attr_accessor :name, :email


-creates attribute accessors corresponding to a userÅfs name and email address. 
 This creates ÅggetterÅh and ÅgsetterÅh methods 
 that allow us to retrieve (get) and assign (set) @name and @email instance variables

-In Rails, the principal importance of instance variables is that they are 
 automatically available in the views, but in general they are used for variables 
 that need to be available throughout a Ruby class.

-Instance variables always begin with an @ sign, and are nil when undefined.

----------------------------------------------------------------------------------------------------

---hashed password----------------------------------------------------------------------------------

	has_secure_password

When included in a model as above, this one method adds the following functionality:

  -The ability to save a securely hashed password_digest attribute to the database.

  -A pair of virtual attributes18 (password and password_confirmation), 
   including presence validations upon object creation and a validation requiring that they match

  -An authenticate method that returns the user when the password is correct (and false otherwise)

----------------------------------------------------------------------------------------------------

---add attribute(column_table) to model

	rails generate migration add_password_digest_to_users password_digest:string

        - add_password_digest_to_users - is the name of migration
        - password_digest              - attribute(column) to be added
        - string                       -  type of attribute




---change attribute:

model:

  def delete_name
    update_attribute(:name, nil)
  end


controller:

    user1 = User.find(1)
    user1.delete_name

----------------------------------------------------------------------------------------------------

---initialize---------------------------------------------------------------------------------------

app/models/example_user.rb

-initialize, is special in Ruby: itÅfs the method called when we execute Model.new .

  def initialize(attributes = {})
    @name  = attributes[:name]
    @email = attributes[:email]
  end


-Here the attributes variable has a default value equal to the empty hash, 
 so that we can define a user with no name or email address.


----------------------------------------------------------------------------------------------------

---Add validation-----------------------------------------------------------------------------------

                          
		validates :content, length: { maximum: 140 }, presence: true

	# :content is table_column
        # presence: true = cannot be null


--Uniqueness validation
               
                validates :email, uniqueness: true
				or
                validates :email, uniqueness: { case_sensitive: false }
                      


--custom validation

-app/models/micropost.rb
  
  validate  :picture_size

  private

    # Validates the size of an uploaded picture.
    def picture_size
      if picture.size > 5.megabytes
        errors.add(:picture, "should be less than 5MB")
      end
    end


 -This custom validation arranges to call the method corresponding to the given symbol (:picture_size). 
  In picture_size itself, we add a custom error message to the errors collection,
  in this case a limit of 5 megabytes
 
----------------------------------------------------------------------------------------------------





---Finding user objects----------------------------------------------------------------------------

  User.find(1)                                        --> find by id

  User.find_by(email: "mhartl@example.com")           --> find by attribute (email)
  
  User.first                                          --> returns the first user in the database   

  User.all                                            --> returns all the users in the database

  User.count	                                      --> returns the number of records


--WHERE

  Micropost.where("user_id = ?", id)

  - The question mark (?) ensures that id is properly escaped before being included in the 
    underlying SQL query, thereby avoiding a serious security hole called SQL injection.

----------------------------------------------------------------------------------------------------


---Updating user objects----------------------------------------------------------------------------

 user1 = User.first

 user1.email = 'wow@example.com'

 user1.save


-- by update_attributes

 user1 = User.first

 user1.update_attributes(name: "The Dude", email: "dude@abides.org")

-The update_attributes method accepts a hash of attributes, 
 and on success performs both the update and the save in one step 
 (returning true to indicate that the save went through).

----------------------------------------------------------------------------------------------------


---user with boolean attribute----------------------------------------------------------------------

schema:

tablename: user    columns: id, name(string),password(string), admin(boolean)


>> user = User.first

>> user.admin?
=> false

>> user.toggle!(:admin)
=> true

>> user.admin?
=> true


-Rails figures out the boolean nature of the admin attribute and 
 automatically adds the question-mark method admin?

-Here weÅfve used the toggle! method to flip the admin attribute from false to true.

----------------------------------------------------------------------------------------------------


----before_save AND before_create-------------------------------------------------------------------


A before_save callback is automatically called before the object is saved, 
which includes both object creation and updates

before_create callback - same as before_save but only on object creation.

ex:

app/models/user.rb
  attr_accessor :remember_token, :activation_token
  before_save   :downcase_email
  before_create :create_activation_digest


    # Converts email to all lower-case.
    def downcase_email
      self.email = email.downcase
    end

    # Creates and assigns the activation token and digest.
    def create_activation_digest
      self.activation_token  = User.new_token
      self.activation_digest = User.digest(activation_token)
    end

  def User.new_token
    SecureRandom.urlsafe_base64
  end


  def User.digest(string)
    cost = ActiveModel::SecurePassword.min_cost ? BCrypt::Engine::MIN_COST :
                                                  BCrypt::Engine.cost
    BCrypt::Password.create(string, cost: cost)
  end

----------------------------------------------------------------------------------------------------

-------MODEL.ERRORS---------------------------------------------------------------------------------

controller:

 
  @errors = @user.errors

view:

  <% if @errors %>
    <h2>
        <%# pluralize(@errors.count, "åèÇÃÉGÉâÅ[Ç™Ç†ÇËÇ‹Ç∑ÅB") %>
        <%= @errors.count.to_s + "åèÇÃÉGÉâÅ[Ç™Ç†ÇËÇ‹Ç∑ÅB" %>
    </h2>
    <% @errors.full_messages.each do |message| %>
        <div class="div_error_message">ÅE<%= message %></div>
    <% end %>

  <% end %>


----------------------------------------------------------------------------------------------------


------------------------------------------------------------------------------------------------------------------------



----------------CONTROLLER----------------------------------------------------------------------------------------------
app/controllers/users_controller.rb

   URL: example.com/users/1

	@user = User.find(params[:id])

   -Here weÅfve used params to retrieve the user id. When we make the appropriate request to the Users controller, 
    params[:id] will be the user id 1,so the effect is the same as the find method User.find(1)



--strong parameters

allows us to specify which parameters are required and which ones are permitted.

We want to require the params hash to have a :user attribute, and 
we want to permit the name, email, password, and password confirmation attributes (but no others). 
We can accomplish this as follows:

	params.require(:user).permit(:name, :email, :password, :password_confirmation)

   -This code returns a version of the params hash with only the permitted attributes 
    (while raising an error if the :user attribute is missing).

To facilitate the use of these parameters, itÅfs conventional to introduce an auxiliary method called user_params 
 (which returns an appropriate initialization hash):


    def user_params
      params.require(:user).permit(:name, :email, :password, :password_confirmation)
    end



--create session

controller:

	session[:user_id] = user.id

      -This places a temporary cookie on the userÅfs browser containing an encrypted version of the userÅfs id, 
       which allows us to retrieve the id on subsequent pages using session[:user_id].

      -the temporary cookie created by the session method expires immediately when the browser is closed.



--create cookie

controller:
	cookies[:user_id] = { value:   user.id,expires: 20.years.from_now.utc }
                                     OR
        cookies.permanent[:user_id] = user.id
        

Because it places the id as plain text, this method exposes the form of the applicationÅfs cookies and 
makes it easier for an attacker to compromise user accounts. To avoid this problem, 
weÅfll use a signed cookie, which securely encrypts the cookie before placing it on the browser:


	cookies.signed[:user_id] = user.id

make it permanent:

        cookies.permanent.signed[:user_id] = user.id


to retrieve tha cookies:
     
        cookies.signed[:user_id]

delete:
 
        cookies.delete(:user_id)



--BEFORE_ACTION


app/controllers/users_controller.rb


  before_action :logged_in_user, only: [:edit, :update]

  def edit
    #
  end

  def update
    #
  end

  private

    def logged_in_user
      unless logged_in?
        flash[:danger] = "Please log in."
        redirect_to login_url
      end
    end


By default, before filters apply to every action in a controller, 
so here we restrict the filter to act only on the :edit and :update actions by passing the 
appropriate only: options hash.



--request.referrer

redirect_to request.referrer || root_url
  
  -request.referrer - is just the previous URL

------------------------------------------------------------------------------------------------------------------------






----------------VIEW---------------------------------------------------------------------------------------------------

----link_to

-Rails helper link_to to create links

	<%= link_to "back", '#', id: "back_button" %>

-the first argument to link_to is the link text, while the second is the URL
-The third argument is an options hash, in this case adding the CSS id "back_button" to the "back" link.



----link_to image_tag

	link_to image_tag("rails.png", alt: "Rails logo"),'http://rubyonrails.org/' %>

-image_tag helper, which takes as arguments the path to an image and an optional options hash, 
 in this case setting the alt attribute of the image tag using symbols

-For this to work, there needs to be an image called rails.png in the app/assets/images/ directory.




----yield

app/views/layouts/application.html.erb

 <%= yield %>

-the yield method inserts the contents of each page into the site layout




----Display debug info

	<%= debug(params) if Rails.env.development? %>

  -restrict the debug information to the development environment



----The flash

- The Rails way to display a temporary message(disappears upon visiting a second page or on page reload)
  is to use a special method called the flash, which we can treat like a hash.
   
controllers:

	flash[:success] = "Welcome to the Sample App!"

views:

	<% flash.each do |message_type, message| %>
  		<div class="alert alert-<%= message_type %>"><%= message %></div>
	<% end %>


Pag flash.now[:success] - isang beses lang niya gagawin   -masuta ervs


----redirect_to

	redirect_to @user

equivalent to:

	redirect_to user_url(@user)



----add javascript 

<script type="text/javascript">
  #codes
</script>



----form_for

The heart of the signup page is a form for submitting the relevant signup information (name, email, password,).
We can accomplish this in Rails with the form_for helper method, which takes in an Active Record object and 
builds a form using the objectÅfs attributes.
 
app/controllers/users_controller.rb

  def new
    @user = User.new
  end


app/views/users/new.html.erb

	<%= form_for(@user) do |f| %>

	<% end %>

-The presence of the ÅudoÅv keyword indicates that form_for takes a block with one variable, 
 which weÅfve called ÅufÅv (for ÅgformÅh).



	<%= form_for(@user) do |f| %>

		<%= f.label :name %>
		<%= f.text_field :name %>

	<% end %>


-produces the HTML

  	<label for="user_name">Name</label>
	<input id="user_name" name="user[name]" type="text" />

-while

	<%= form_for(@user) do |f| %>

		<%= f.label :email %>
		<%= f.email_field :email %>

	<% end %>

-produces the HTML

	<label for="user_email">Email</label>
	<input id="user_email" name="user[email]" type="email" />

-and

	<%= form_for(@user) do |f| %>

		<%= f.label :password %>
		<%= f.password_field :password %>

	<% end %>

-produces the HTML

	<label for="user_password">Password</label>
	<input id="user_password" name="user[password]" type="password" />



-Text and email fields (type="text" and type="email") simply display their contents, 
 whereas password fields (type="password") obscure the input for security purposes
 (The benefit of using an email field is that some systems treat it differently from a text field; 
  for example, the code type="email" will cause some mobile devices to display a special keyboard 
  optimized for entering email addresses.)


The key to creating a user is the special ÅunameÅv attribute in each input:

	<input id="user_name" name="user[name]" - - - />
	.
	.
	.
	<input id="user_password" name="user[password]" - - - />

These ÅunameÅv values allow Rails to construct an initialization hash (via the ÅuparamsÅv variable) 
for creating users using the values entered by the user

The second important element is the form tag itself. Rails creates the form tag using the @user object: 
because every Ruby object knows its own class, Rails figures out that @user is of class User; moreover, 
since @user is a new user, Rails knows to construct a form with the post method, 
which is the proper verb for creating a new object

       <form action="/users" class="new_user" id="new_user" method="post">



---login form (form_for)

	<%= form_for(:session, url: login_path) do |f| %>

      		<%= f.label :email %>
      		<%= f.email_field :email, class: 'form-control' %>

      		<%= f.label :password %>
      		<%= f.password_field :password, class: 'form-control' %>

      		<%= f.submit "Log in", class: "btn btn-primary" %>

	<% end %>

        --getting the submitted information:
            
            params[:session]  --> is itself a hash:
            
		params[:session][:email]        --> is the submitted email
                params[:session][:password]     --> is the submitted password




---checkbox(form_for)

    <%= form_for(:session, url: login_path) do |f| %>
      <%= f.check_box :remember_me %>

      <%= f.submit "Log in", class: "btn btn-primary" %>
    <% end %>


the value of params[:session][:remember_me] is Åf1Åf if the box is checked, and Åf0Åf if it isnÅft.



---f.object

  -In ( form_for(@user) do |f| ),         f.object is @user
  -In ( form_for(@micropost) do |f| )     f.object is @micropost


-app/views/shared/_error_messages.html.erb

 <% if object.errors.any? %>

 <% end %>

-app/views/users/new.html.erb

 <%= form_for(@user) do |f| %>
 	<%= render 'shared/error_messages', object: f.object %>

 <% end %>



---AJAX

 -add remote: true to form_for 
  ex: 

       <%= form_for(current_user.active_relationships.find_by(followed_id: @user.id),
             html: { method: :delete }, remote: true) do |f| %>

       <% end %>


 -we now need to arrange for the controller to respond to Ajax requests.
  
 something_controller.rb:
    
   def destroy
	respond_to do |format|
  		format.html { redirect_to @user }
  		format.js
	end
   end

 -The syntax is potentially confusing, and itÅfs important to understand that in the code above 
  only one of the lines gets executed. (In this sense, respond_to is more like an if-then-else statement 
                                        than a series of sequential lines.)



 -In the case of an Ajax request, Rails automatically calls a JavaScript embedded Ruby (.js.erb) file
  with the same name as the action, i.e., create.js.erb or destroy.js.erb.

destroy.js.erb

	$("#follow_form").html("<%= escape_javascript(render('users/destroy')) %>");



 - j or escape_javascript method: Some evil user may submit a comment containing (malicious) javascript 
   which would be executed on your page unless you make use of the j method which:

   Escapes carriage returns and single and double quotes for JavaScript segments.

   and therefore prevents the execution of the code in the browser.




-----------------------------------------------------------------------------------------------------------------





-----------------ROUTES-------------------------------------------------------------------------------------------------

GET is the most common HTTP operation, used for reading data on the web; it just means Ågget a pageÅh

POST is the next most common operation; it is the request sent by your browser when you submit a form. 
     In Rails applications, POST requests are typically used for creating things 
     (although HTTP also allows POST to perform updates)
     For example, the POST request sent when you submit a registration form creates a new user on the remote site.
 
PATCH and DELETE, are designed for updating and destroying things on the remote server.

-------RESOURCES-------------------------------------------------------------------------------------

config/routes.rb

    resources :users   #    ---> controller:    app/controllers/users_controller.rb

    HTTP request	URL			Action		Purpose

	GET	    	/users			index		page to list all users
	GET		/users/1		show		page to show users with id 1
	GET		/users/new		new		page to make a new users
	POST		/users			create		create a new users
	GET		/users/1/edit		edit		page to edit users with id 1
	PATCH		/users/1		update		update users with id 1
	DELETE		/users/1		destroy		delete users with id 1


-----
 
    resources :users, only: [:edit]	

    HTTP request	URL			Action		Purpose

	GET		/users/1/edit		edit		page to edit users with id 1

----------------------------------------------------------------------------------------------------
get  'static_pages/home'

  -maps requests for the URL /static_pages/home to the home action in the Static Pages controller.
   Moreover, by using get we arrange for the route to respond to a GET request

root 'static_pages#home'
  - This arranges for requests for / (localhost:3000/) to be routed to the home action in the Static Pages controller.

----------------------------------------------------------------------------------------------------
root  'static_pages/home' 

get  '/help', to: 'static_pages#help'


This pattern routes a GET request for the URL /help to the help action in the Static Pages controller.
this creates two named routes, help_path and help_url:

	help_path -> '/help'
	help_url  -> 'http://www.example.com/help'




----------------------------------------------------------------------------------------------------
get '/patients/:id', to: 'patients#show', as: 'patient'



	view:
 		patient_path(17)

-the router will generate the path /patients/17


------------------------------------------------------------------------------------------------------------------------






---------RAILS CONSOLE-------------------------------------------------------------------------------------------------

 rails console

 - enter  rails console

-------------ON CONSOLE

>> "foobar".length        # Passing the "length" message to a string
=> 6

>> "foobar".empty?
=> false

>> "".empty?
=> true

----split

>>  "foo bar     baz".split     # Split a string into a three-element array.
=> ["foo", "bar", "baz"]

 -The result of this operation is an array of three strings. 
  By default, split divides a string into an array by splitting on whitespace, 
  but you can split on nearly anything else as well:

>> "fooxbarxbazx".split('x')
=> ["foo", "bar", "baz"]


--------ARRAY

-As is conventional in most computer languages, Ruby arrays are zero-offset, 
 which means that the first element in the array has index 0, 
 the second has index 1, and so on:

>> a = [42, 8, 17]
=> [42, 8, 17]

>> a[0]               # Ruby uses square brackets for array access.
=> 42

>> a[1]
=> 8

>> a[2]
=> 17

>> a[-1]              # Indices can even be negative!
=> 17

-We see here that Ruby uses square brackets to access array elements. 
 In addition to this bracket notation, Ruby offers synonyms for some commonly accessed elements:

>> a                  # Just a reminder of what 'a' is
=> [42, 8, 17]

>> a.first
=> 42

>> a.second
=> 8

>> a.last
=> 17

>> a.last == a[-1]    # Comparison using ==
=> true

-arrays respond to a wealth of other methods:

>> x = a.length       # Like strings, arrays respond to the 'length' method.
=> 3

>> a
=> [42, 8, 17]

>> a.empty?
=> false

>> a.include?(42)
=> true

>> a.sort
=> [8, 17, 42]

>> a.reverse
=> [17, 8, 42]

>> a.shuffle
=> [17, 42, 8]

>> a
=> [42, 8, 17]


-Note that none of the methods above changes 'a' itself. To mutate the array, 
 use the corresponding ÅgbangÅh methods 
 (so-called because the exclamation point is usually pronounced ÅgbangÅh in this context):

>> a
=> [42, 8, 17]

>> a.sort!
=> [8, 17, 42]

>> a
=> [8, 17, 42]

-You can also add to arrays with the push method or its equivalent operator, <<:

>> a.push(6)                  # Pushing 6 onto an array
=> [42, 8, 17, 6]

>> a << 7                     # Pushing 7 onto an array
=> [42, 8, 17, 6, 7]

>> a << "foo" << "bar"        # Chaining array pushes
=> [42, 8, 17, 6, 7, "foo", "bar"]

----join = array ÅÀ string

>> a
=> [42, 8, 17, 6, 7, "foo", "bar"]

>> a.join                       # Join on nothing.
=> "4281767foobar"

>> a.join(', ')                 # Join on comma-space.
=> "42, 8, 17, 6, 7, foo, bar"


--------RANGES
>> 0..9
=> 0..9

>> 0..9.to_a              # Oops, call to_a on 9.
NoMethodError: undefined method `to_a' for 9:Fixnum

>> (0..9).to_a            # Use parentheses to call to_a on the range.
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

-Though 0..9 is a valid range, the second expression above shows that we need to add parentheses 
 to call a method on it.


Ranges are useful for pulling out array elements:

>> a = %w[foo bar baz quux]         # Use %w to make a string array.
=> ["foo", "bar", "baz", "quux"]

>> a[0..2]
=> ["foo", "bar", "baz"]

A particularly useful trick is to use the index -1 at the end of the range to select every element 
from the starting point to the end of the array without explicitly having to use the arrayÅfs length:

>> a = (0..9).to_a
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

>> a[2..(a.length-1)]               # Explicitly use the array's length.
=> [2, 3, 4, 5, 6, 7, 8, 9]

>> a[2..-1]                         # Use the index -1 trick.
=> [2, 3, 4, 5, 6, 7, 8, 9]

Ranges also work with characters:

>> ('a'..'e').to_a
=> ["a", "b", "c", "d", "e"]


--------BLOCKS

Both arrays and ranges respond to a host of methods that accept blocks, 
which are simultaneously one of RubyÅfs most powerful and most confusing features:

>> (1..5).each { |i| puts 2 * i }
2
4
6
8
10
=> 1..5

This code calls the each method on the range (1..5) and passes it the block { |i| puts 2 * i }. 
The vertical bars around the variable name in |i| are Ruby syntax for a block variable, 
and itÅfs up to the method to know what to do with the block.



MAP method:

>> 3.times { puts "Betelgeuse!" }   # 3.times takes a block with no variables.
"Betelgeuse!"
"Betelgeuse!"
"Betelgeuse!"
=> 3

>> (1..5).map { |i| i ** 2 }          # The ** notation is for 'power'.
=> [1, 4, 9, 16, 25]

>> %w[a b c]                        # Recall that %w makes string arrays.
=> ["a", "b", "c"]

>> %w[a b c].map { |char| char.upcase }
=> ["A", "B", "C"]

>> %w[A B C].map { |char| char.downcase }
=> ["a", "b", "c"]

As you can see, the map method returns the result of applying the given block to each element 
in the array or range. In the final two examples, the block inside map involves calling a 
particular method on the block variable, and in this case thereÅfs a commonly 
used shorthand called Ågsymbol-to-procÅh:

>> %w[A B C].map { |char| char.downcase }
=> ["a", "b", "c"]

>> %w[A B C].map(&:downcase)
=> ["a", "b", "c"]



--------HASHES and SYMBOLS

Hashes are essentially arrays that arenÅft limited to integer indices.  
Instead, hash indices, or keys, can be almost any object. For example, we can use strings as keys:

>> user = {}                         			  # {} is an empty hash.
=> {}

>> user["first_name"] = "Michael"   			  # Key "first_name", value "Michael"
=> "Michael"

>> user["last_name"] = "Hartl"      			  # Key "last_name", value "Hartl"
=> "Hartl"

>> user["first_name"]                			  # Element access is like arrays.
=> "Michael"

>> user                              			  # A literal representation of the hash
=> {"last_name"=>"Hartl", "first_name"=>"Michael"}


Instead of defining hashes one item at a time using square brackets, 
itÅfs easy to use a literal representation with keys and values separated by =>, called a ÅghashrocketÅh:

>> user = { "first_name" => "Michael", "last_name" => "Hartl" }
=> {"last_name"=>"Hartl", "first_name"=>"Michael"}


So far weÅfve used strings as hash keys, but in Rails it is much more common to use symbols instead. 
Symbols look kind of like strings, but prefixed with a colon instead of surrounded by quotes. 
For example, :name is a symbol. You can think of symbols as basically strings without all the extra baggage:

>> "name".split('')
=> ["n", "a", "m", "e"]

>> :name.split('')
NoMethodError: undefined method `split' for :name:Symbol

>> "foobar".reverse
=> "raboof"

>> :foobar.reverse
NoMethodError: undefined method `reverse' for :foobar:Symbol




Symbols are a special Ruby data type shared with very few other languages, so they may seem weird at first, 
but Rails uses them a lot, so youÅfll get used to them fast. Unlike strings, not all characters are valid:

>> :foo-bar
NameError: undefined local variable or method `bar' for main:Object

>> :2foo
SyntaxError

As long as you start your symbols with a letter and stick to normal word characters, you should be fine.




In terms of symbols as hash keys, we can define a user hash as follows:

>> user = { :name => "Michael Hartl", :email => "michael@example.com" }
=> {:name=>"Michael Hartl", :email=>"michael@example.com"}

>> user[:name]              # Access the value corresponding to :name.
=> "Michael Hartl"

>> user[:password]          # Access the value of an undefined key.
=> nil

We see here from the last example that the hash value for an undefined key is simply nil.



Because itÅfs so common for hashes to use symbols as keys, 
as of version 1.9 Ruby supports a new syntax just for this special case:

>> h1 = { :name => "Michael Hartl", :email => "michael@example.com" }
=> {:name=>"Michael Hartl", :email=>"michael@example.com"}

>> h2 = { name: "Michael Hartl", email: "michael@example.com" }
=> {:name=>"Michael Hartl", :email=>"michael@example.com"}

>> h1 == h2
=> true




{ :name => "Michael Hartl" }

and

{ name: "Michael Hartl" }

are equivalent, but otherwise you need to use :name (with the colon coming first) to denote a symbol



---Nested hashes

>> params = {}        # Define a hash called 'params' (short for 'parameters').
=> {}

>> params[:user] = { name: "Michael Hartl", email: "mhartl@example.com" }
=> {:name=>"Michael Hartl", :email=>"mhartl@example.com"}

>> params
=> {:user=>{:name=>"Michael Hartl", :email=>"mhartl@example.com"}}

>>  params[:user][:email]
=> "mhartl@example.com"


As with arrays and ranges, hashes respond to the each method:

>> flash = { success: "It worked!", danger: "It failed." }
=> {:success=>"It worked!", :danger=>"It failed."}

>> flash.each do |key, value|
?>   puts "Key #{key.inspect} has value #{value.inspect}"
>> end

Key :success has value "It worked!"
Key :danger has value "It failed."





inspect method returns a string with a literal representation of the object itÅfs called on:

>> puts :name, :name.inspect
name
:name

>> puts "It worked!", "It worked!".inspect
It worked!
"It worked!"


using inspect to print an object is common enough that thereÅfs a shortcut for it, the p function:

>> p :name             # Same output as 'puts :name.inspect'
:name


------------RUBY CLASSES


---Constructors

WeÅfve seen lots of examples of using classes to instantiate objects, 
but we have yet to do so explicitly. 
For example, we instantiated a string using the double quote characters, 
which is a literal constructor for strings:

>> s = "foobar"       # A literal constructor for strings using double quotes
=> "foobar"

>> s.class
=> String

We see here that strings respond to the method class, 
and simply return the class they belong to.



Instead of using a literal constructor, we can use the equivalent named constructor, 
which involves calling the new method on the class name:

>> s = String.new("foobar")   # A named constructor for a string
=> "foobar"

>> s.class
=> String

>> s == "foobar"
=> true

This is equivalent to the literal constructor, but itÅfs more explicit about what weÅfre doing.



Arrays work the same way as strings:

>> a = Array.new([1, 3, 2])
=> [1, 3, 2]




Hashes, in contrast, are different. While the array constructor Array.new takes an initial value for the array, 
Hash.new takes a default value for the hash, which is the value of the hash for a nonexistent key:

>> h = Hash.new
=> {}

>> h[:foo]            # Try to access the value for the nonexistent key :foo.
=> nil

>> h = Hash.new(0)    # Arrange for nonexistent keys to return 0 instead of nil.
=> {}

>> h[:foo]
=> 0


When a method gets called on the class itself, as in the case of "new", itÅfs called a CLASS METHOD.
The result of calling "new" on a class is an object of that class, also called an INSTANCE of the class.
A method called on an instance, such as "length", is called an INSTANCE METHOD.



---- operator (  ||=  )

The ||= (Ågor equalsÅh) assignment operator is a common Ruby idiom and is thus important for aspiring Rails 
developers to recognize. 
Although at first it may seem mysterious, or equals is easy to understand by analogy.

  >> @foo
  => nil
  >> @foo = @foo || "bar"
  => "bar"
  >> @foo = @foo || "baz"
  => "bar"

Since nil is false in a boolean context, the first assignment to @foo is nil || "bar", 
which evaluates to "bar". Similarly, the second assignment is @foo || "baz", i.e., "bar" || "baz", 
which also evaluates to "bar". 
This is because anything other than nil or false is true in a boolean context, 
and the series of || expressions terminates after 
the first true expression is evaluated.






-----------------------------------------------------------------------------------------------------------------------







---------A working form-------------------------------------------------------------------------------------------------

-app/controllers/users_controller.rb


 class UsersController < ApplicationController

  	def show
    		@user = User.find(params[:id])
  	end

  	def new
    		@user = User.new
  	end

  	def create
    		@user = User.new(user_params)
    		if @user.save
      			redirect_to @user
    		else
      			render 'new'
    		end
  	end

  	private

    		def user_params
      			params.require(:user).permit(:name, :email, :password, :password_confirmation)
    		end

 end


-app/views/users/new.html.erb


    <%= form_for(@user) do |f| %>
      <%= render 'shared/error_messages' %>

      <%= f.label :name %>
      <%= f.text_field :name, class: 'form-control' %>

      <%= f.label :email %>
      <%= f.email_field :email, class: 'form-control' %>

      <%= f.label :password %>
      <%= f.password_field :password, class: 'form-control' %>

      <%= f.label :password_confirmation, "Confirmation" %>
      <%= f.password_field :password_confirmation, class: 'form-control' %>

      <%= f.submit "Create my account", class: "btn btn-primary" %>
    <% end %>


-app/views/shared/_error_messages.html.erb

<% if @user.errors.any? %>
  <div id="error_explanation">
    <div class="alert alert-danger">
      The form contains <%= pluralize(@user.errors.count, "error") %>.
    </div>
    <ul>
    <% @user.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
<% end %>




-----------------------------------------------------------------------------------------------------------------------


---------working login(simple) (session)-------------------------------------------------------------------------------

-app/controllers/application_controller.rb

	include SessionsHelper





-app/controllers/sessions_controller.rb


	class SessionsController < ApplicationController

  		def new
  		end

  		def create
    			user = User.find_by(email: params[:session][:email].downcase)
    			if user && user.authenticate(params[:session][:password])
				#log_in is a method of sessions_helper.rb
      				log_in user
      				redirect_to user
    			else
      				flash.now[:danger] = 'Invalid email/password combination'
      				render 'new'
    			end
  		end

  		def destroy
    			log_out
    			redirect_to root_url
  		end

	end


-app/views/sessions/new.html.erb


<% provide(:title, "Log in") %>
<h1>Log in</h1>

<div class="row">
  <div class="col-md-6 col-md-offset-3">
    <%= form_for(:session, url: login_path) do |f| %>

      <%= f.label :email %>
      <%= f.email_field :email, class: 'form-control' %>

      <%= f.label :password %>
      <%= f.password_field :password, class: 'form-control' %>

      <%= f.submit "Log in", class: "btn btn-primary" %>
    <% end %>

    <p>New user? <%= link_to "Sign up now!", signup_path %></p>
  </div>
</div>



-app/helpers/sessions_helper.rb


module SessionsHelper

  # Logs in the given user.
  def log_in(user)
    session[:user_id] = user.id
  end

  # Returns the current logged-in user (if any).
  def current_user
    @current_user ||= User.find_by(id: session[:user_id])
  end

  # Returns true if the user is logged in, false otherwise.
  def logged_in?
    !current_user.nil?
  end

  # Logs out the current user.
  def log_out
    session.delete(:user_id)
    @current_user = nil
  end

end


---------Logging in the user upon signup.


-app/controllers/users_controller.rb


class UsersController < ApplicationController

  def show
    @user = User.find(params[:id])
  end

  def new
    @user = User.new
  end

  def create
    @user = User.new(user_params)
    if @user.save
      log_in @user
      flash[:success] = "Welcome to the Sample App!"
      redirect_to @user
    else
      render 'new'
    end
  end

  private

    def user_params
      params.require(:user).permit(:name, :email, :password,
                                   :password_confirmation)
    end
end


-----------------------------------------------------------------------------------------------------------------------




---UPDATE-SHOW-DELETE USER---------------------------------------------------------------------------------------------





-app/views/users/edit.html.erb  (UPDATE)


<% provide(:title, "Edit user") %>
<h1>Update your profile</h1>

<div class="row">
  <div class="col-md-6 col-md-offset-3">
    <%= form_for(@user) do |f| %>
      <%= render 'shared/error_messages' %>

      <%= f.label :name %>
      <%= f.text_field :name, class: 'form-control' %>

      <%= f.label :email %>
      <%= f.email_field :email, class: 'form-control' %>

      <%= f.label :password %>
      <%= f.password_field :password, class: 'form-control' %>

      <%= f.label :password_confirmation, "Confirmation" %>
      <%= f.password_field :password_confirmation, class: 'form-control' %>

      <%= f.submit "Save changes", class: "btn btn-primary" %>
    <% end %>

    <div class="gravatar_edit">
      <%= gravatar_for @user %>
      <a href="http://gravatar.com/emails" target="_blank">change</a>
    </div>
  </div>
</div>




-app/views/users/index.html.erb    (SHOW)

<% provide(:title, 'All users') %>
<h1>All users</h1>

<ul class="users">
  <% @users.each do |user| %>
    <li>
      <%= gravatar_for user, size: 50 %>
      <%= link_to user.name, user %>
    </li>
  <% end %>
</ul>




-app/views/users/index.html.erb  (DELETE)


<% provide(:title, 'All users') %>
<h1>All users</h1>

<%= will_paginate %>

<ul class="users">
  <% @users.each do |user| %>
    <li>
      <%= gravatar_for user, size: 50 %>
      <%= link_to user.name, user %>
  <% if current_user.admin? && !current_user?(user) %>
    | <%= link_to "delete", user, method: :delete,
                                  data: { confirm: "You sure?" } %>
  <% end %>
    </li>
  <% end %>
</ul>

<%= will_paginate %>



-app/controllers/users_controller.rb  (CONTROLLER)


class UsersController < ApplicationController
  before_action :logged_in_user, only: [:index, :edit, :update, :destroy]
  before_action :correct_user,   only: [:edit, :update]

  def index
    @users = User.paginate(page: params[:page], per_page: 3)
  end


  def show
    @user = User.find(params[:id])
  end

  def new
  	@user = User.new
  end

  def create

    @user = User.new(user_params)
    if @user.save
    	log_in @user
    	flash[:success] = "Welcome to the Sample App!"
      	redirect_to @user
    else
      	render 'new'
    end

  end

  def edit
    @user = User.find(params[:id])
  end

  def update
    @user = User.find(params[:id])
    if @user.update_attributes(user_params)
      flash[:success] = "Profile updated"
      redirect_to @user
    else
      render 'edit'
    end
  end

  def destroy
    User.find(params[:id]).destroy
    flash[:success] = "User deleted"
    redirect_to users_url
  end


  private

    def user_params
      	params.require(:user).permit(:name, :email, :password, :password_confirmation)
    end

    # Before filters

    # Confirms a logged-in user.
    def logged_in_user
      unless logged_in?
        flash[:danger] = "Please log in."
        redirect_to login_url
      end
    end

    def correct_user
      @user = User.find(params[:id])
      redirect_to(root_url) unless current_user?(@user)
    end

end





-----------------------------------------------------------------------------------------------------------------------








9.4.1 What we learned in this chapter

Rails can maintain state from one page to the next using persistent cookies via the cookies method.
We associate to each user a remember token and a corresponding remember digest for use in persistent sessions.
Using the cookies method, we create a persistent session by placing a permanent remember token cookie on the browser.
Login status is determined by the presence of a current user based on the temporary sessionÅfs user id or the permanent sessionÅfs unique remember token.
The application signs users out by deleting the sessionÅfs user id and removing the permanent cookie from the browser.
The ternary operator is a compact way to write simple if-then statements.



10.5.1 What we learned in this chapter

Users can be updated using an edit form, which sends a PATCH request to the update action.
Safe updating through the web is enforced using strong parameters.
Before filters give a standard way to run methods before particular controller actions.
We implement an authorization using before filters.
Authorization tests use both low-level commands to submit particular HTTP requests directly to controller actions and high-level integration tests.
Friendly forwarding redirects users where they wanted to go after logging in.
The users index page shows all users, one page at a time.
Rails uses the standard file db/seeds.rb to seed the database with sample data using rails db:seed.
Running render @users automatically calls the _user.html.erb partial on each user in the collection.
A boolean attribute called admin on the User model automatically creates an admin? boolean method on user objects.
Admins can delete users through the web by clicking on delete links that issue DELETE requests to the Users controller destroy action.
We can create a large number of test users using embedded Ruby inside fixtures.



11.5.1 What we learned in this chapter

Like sessions, account activations can be modeled as a resource despite not being Active Record objects.
Rails can generate Action Mailer actions and views to send email.
Action Mailer supports both plain-text and HTML mail.
As with ordinary actions and views, instance variables defined in mailer actions are available in mailer views.
Account activations use a generated token to create a unique URL for activating users.
Account activations use a hashed activation digest to securely identify valid activation requests.
Both mailer tests and integration tests are useful for verifying the behavior of the User mailer.
We can send email in production using SendGrid.



12.5.1 What we learned in this chapter

Like sessions and account activations, password resets can be modeled as a resource despite not being Active Record objects.
Rails can generate Action Mailer actions and views to send email.
Action Mailer supports both plain-text and HTML mail.
As with ordinary actions and views, instance variables defined in mailer actions are available in mailer views.
Password resets use a generated token to create a unique URL for resetting passwords.
Password resets use a hashed reset digest to securely identify valid reset requests.
Both mailer tests and integration tests are useful for verifying the behavior of the User mailer.
We can send email in production using SendGrid.



13.5.1 What we learned in this chapter

Microposts, like Users, are modeled as a resource backed by an Active Record model.
Rails supports multiple-key indices.
We can model a user having many microposts using the has_many and belongs_to methods in the User and Micropost models, respectively.
The has_many/belongs_to combination gives rise to methods that work through the association.
The code user.microposts.build(...) returns a new Micropost object automatically associated with the given user.
Rails supports default ordering via default_scope.
Scopes take anonymous functions as arguments.
The dependent: :destroy option causes objects to be destroyed at the same time as associated objects.
Pagination and object counts can both be performed through associations, leading to automatically efficient code.
Fixtures support the creation of associations.
It is possible to pass variables to Rails partials.
The where method can be used to perform Active Record selections.
We can enforce secure operations by always creating and destroying dependent objects through their association.
We can upload and resize images using CarrierWave.



14.4.2 What we learned in this chapter

RailsÅf has_many :through allows the modeling of complicated data relationships.
The has_many method takes several optional arguments, including the object class name and the foreign key.
Using has_many and has_many :through with properly chosen class names and foreign keys, we can model both active (following) and passive (being followed) relationships.
Rails routing supports nested routes.
The where method is a flexible and powerful way to create database queries.
Rails supports issuing lower-level SQL queries if needed.
By putting together everything weÅfve learned in this book, weÅfve successfully implemented user following with a status feed of microposts from followed users.









