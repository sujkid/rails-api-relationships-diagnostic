# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
  Join tables hold references to two or more tables.
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
  profiles table will have columns - given_name, surname, email.
  movies table will have columns - title, release_date, length.
  favorites will be a join table that will have columns -
  movie_id:integer(reference to movies table),
  profile_id:integer(reference to profiles table)
```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites, dependent: :destroy
end
```

```rb
class Movie < ActiveRecord::Base
  has_many :profiles, through: :favorites
  has_many :favorites, dependent: :destroy
end
```

```rb
class Favorite < ActiveRecord::Base
  belongs_to :profile, inverse_of: :favorites
  belongs_to :movie, inverse_of: :favorites
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
  Serializers determine the attributes that are visible in the database.
  http://localhost:3000/profiles/1 will list out the profile with id=1.
  {
    "profile":
      "given_name": "",
      "surname": "",
      "email": "",
      "favorites":
        "movie_id":
        "profile_id": 1
  }
```

```rb
class ProfileSerializer < ActiveModel::Serializer
  attributes :id, :given_name, :surname, :email, :favorites
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
  rails g scaffold favorite profile:string movie:string
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
  dependent: :destroy is used in the model class to enable to delete a column in
  the join table.
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
  The ingredient-recipe example can have a one-to-one as well as many-to-many
  relationships.
  A recipe can have many ingredients, while an ingredient could be used in
  a single recipe - one-to-many.
  It is also possible for ingredients to be used in many recipes - many-to-many.
```
