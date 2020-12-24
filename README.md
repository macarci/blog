# README

This is the implementation of the **Getting Started (Rails)** tutorial provided by the **MongoDB** official documentation at https://docs.mongodb.com/mongoid/current/tutorials/getting-started-rails/

# How to use

```shell
git clone https://github.com/macarci/blog.git
cd blog
bundle install
…
yarn install
…
rails s
```

# Learn more

If you want to learn more about **Rails** and **Mongoid** then continue reading the next sections.
You can also checkout and fallow the links at the presentation [MongoDB at a glance](https://docs.google.com/presentation/d/1ba9CEbY21ruBxDpaLdGiCOMrr_owdaPvJ34EeH8zBIQ/edit?usp=sharing) to learn some basics about **MongoDB**.

## Add the `rails_admin` gem

Fallow the instructions at https://github.com/sferik/rails_admin#installation

Basically

1. Add to the Gemfile: `gem 'rails_admin', '~> 2.0'`
2. Run `bundle install`
3. Run `rails g rails_admin:install`
4. Provide a namespace for the routes when asked

## Fix the `kaminari` issue

1. Add to the Gemfile: `gem 'kaminari-mongoid'`
2. Run `bundle install`

## Check **RailsAdmin**

Start a server `rails s` and administer your data at [/admin](http://localhost:3000/admin) (if you chose default namespace: /admin).

## Add a `Person` model

Create a file `app/models/person.rb`

````ruby
class Person
 include Mongoid::Document

 field :name, type: String
 field :email, type: String
 
end
````

Restart the server and try to add some `Person` records by using the **RailsAdmin UI**.

## Extends the `Person` model

Create a file `app/models/student.rb`

````ruby
class Student < Person

 field :ave_grade, type: Float
 field :year, type: Integer

end
````

Create a file `app/models/professor.rb`

````ruby
class Professor < Person

field :subject, type: String
field :salary, type: Float

end
````

Restart the server and try to add some `Student` and `Professor` records by using the **RailsAdmin UI**.

## Try composition by adding an `Address` model

Create a file `app/models/address.rb`

````ruby
class Address
 include Mongoid::Document

 field :street, type: String
 field :number, type: String
 field :city, type: String

 embedded_in :person

end
````

Update the `Person` model file `app/models/person.rb` to add the `embedded` association.

````ruby
class Person
 include Mongoid::Document

 field :name, type: String
 field :email, type: String
 
 embeds_one :address
  
 # For rails_admin
 accepts_nested_attributes_for :address

end
````

Restart the server and try to add an address by editing an already existing `Person` record.

## Add a second address

To try a _many composition association_ by updating the `Person` model file `app/models/person.rb` and adding a `second_address`.

````ruby
class Person
 include Mongoid::Document

 field :name, type: String
 field :email, type: String
 
 embeds_one :address
 embeds_one :second_address, class_name: 'Address'
  
 # For rails_admin
 accepts_nested_attributes_for :address, :second_address

end
````

Restart the server and try to add a second address.

## Aggregation

Explore the aggregation relations between the already existing models `Post` and `Comment`.
## More Mongoid associations

Explore adding more associations by following the official Mongoid documentation at https://docs.mongodb.com/mongoid/current/tutorials/mongoid-relations/.

