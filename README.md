
# SQLAlchemy Association Object Many to Many Lab

## Objectives

1.  Understand how an Association Object join table differs from a basic "has many through" join table
2.  Write a "has many through" relationship using an Association Object as the join table
3.  Become comfortable with the syntax required for setting up an Association Object

## Introduction

In the previous lab, we created a "has many through" relationship between `Actor`s and `Role`s using a join table consisting of only foreign keys, the `actor_id` and `role_id`.  In this lab, we will write another "has many through" relationship but this time with a join table that contains other information on it.

We will work with the following classes: `Artist`, `Genre`, and `Song`.  In this case, the `Song` class will be mapped to and serve as our "join table".  It will contain its own `id` (primary key), the `artist_id` and `genre_id` foreign keys, and a `name`.

* An Artist has many Songs and has many Genres through Songs
* A Genre has many Songs and has many Artists through Songs
* Every Song belongs to an Artist and belongs to a Genre

When properly setup, we should be able to see every Artist's songs with `Artist.songs` and genres with `Artist.genres`.  Likewise, we should be able to see all of a Genre's songs with `Genre.songs` and artists with `Genre.artists`.

## Instructions

For the purposes of this lab, we will write all of our model classes in the `models.py` file without worrying about using Alembic migrations to alter any of our database schema.

Our Artist and Genre models will be set up just like a normal "has many through" relationship; however, we will need to provide further information to our Song Association Object.

#### `Artist`

Every Artist will have:
* an `id` (primary key)
* a `name`
* a `genres` parameter to configure the has many relationship to genres in the format below
> ```python
 genres = relationship('Genre', secondary='songs')
  ```

#### `Genre`

Similarly, every Genre will have:
- an `id` (primary key)
- a `name`
- an `artists` parameter to configure the has many relationship to artists

#### `Song`

Similar to the normal "has many through" relationship, our Song join table will have foreign keys for `artist_id` and `genre_id` that use the ForeignKey function to point to their respective matching remote table id values.  But **do not include** the `primary_key=True` parameter for `artist_id` and `genre_id` since the Song's own `id` will serve as the table's primary key.

Since each Song is its own object with its own properties, we need to add the following:
- an `id` (primary key) column
- a `name`
- parameters to configure the Song's "belongs to" relationship to Artist and Genre.  Use the format below.
> ```python
artist = relationship(Artist, backref=backref("songs", cascade="all, delete-orphan"))
genre = relationship(Genre, backref=backref("songs", cascade="all, delete-orphan"))
```

#### That's It!

Now we can make associate Songs to their Artists, and subsequently associate the Genres back to these Artists through the Songs.

Although there are no tests for queries, feel free to experiment with the data inserted into the database you just created in the `queries.py` file.  The data inserted into the tables can be viewed in the `seed.py` file.  To experiment, uncomment `import pdb; pdb.set_trace()` and remove the `pass` command from the function in `queries.py`.  Run the tests and you will be able to play with a `session` object in your terminal!
