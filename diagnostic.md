# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    I join table allows the developer to keep their database tables dry. In our ingredient-recipe examample, it allows the ingredient table, for example, not to have duplicate items, even if multiple receipes use the same ingredient.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    profile:  id, name, email
    movie: id, title, cast, director
    favorite: movie_id, profile_id
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :movies, through :favorites
    has_many :favorities, dependent: :destroy
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    has_many :profiles, through favorites
    has_many :favorities, dependent: :destroy
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :profile, inverse_of: :favorites
    belongs_to :movie, inverse_of: : favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    A serializer filters and formats the database response to provide only the attributes needed by the developer.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :name, :email
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    bin/rails generate scaffold favorites profile:references movie:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    You'd put 'dependent: :destroy' on movie and profile, like so:
    'has_many :favorities, dependent: :destroy'

    This would make it so that if a profile or a movie is destroyed,
    the favorite that references that profile or movie will also be
    deleted.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    In our patient, doctor, appointment example it makes sense for there to be a one to many relationship between doctor and patient, so that a doctor can see all patients regardless of whether they have an upcoming appointment. It also makes sense for there to be a many to many relationship through appointments, so that a patient can have more than one doctor, depending on the type of appointment.
  ```
