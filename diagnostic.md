# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
  So we can connect information from multiple tables!
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
  'Profile' ('given_name', 'surname', 'email')
  'Movies' ('title', 'release_date', 'length')
  'Favorites' (profile: references, movies: references)
In this we have Profiles and Movies as unique items with connections through the action of adding movies to favorites. This allows an association between a movie and many Profiles and between Profiles and many movies.
```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites
end
```

```rb
class Movie < ActiveRecord::Base
  has_many :profiles, through: :favorites
  has_many :favorites
end
```

```rb
class Favorite < ActiveRecord::Base
  belongs_to :movie, inverse_of: :favorites
  belongs_to :profile, inverse_of: :favorites
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
  The purpose of the Serializer is to control the display and behavior of
  data through the model.
```

```rb
class ProfileSerializer < ActiveModel::Serializer
  has_many :movies
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
  bundle exec rails g scaffold ProfileMovies movie_id:integer profile_id:integer
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
  Dependent destroy allows us to remove all data through it\s associations
  we use it to clear out information that will no longer be relevant when
  the connected data is removed.
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
  With a app like Yelp you have a one user to many reviews relationship and you also have a one restaurant to many reviews. Through this you also have an one restaurant to many users(reviewers) relationship through reviews and
  a one user to many restaurants relationship through reviews.
```
