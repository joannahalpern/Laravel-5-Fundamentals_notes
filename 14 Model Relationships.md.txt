#Eloquent Model Relationships#


##One to Many Relationship Example
Creating a one-to-many relationship between users and user **has many** articles and each article **belongs to** a user (which we call writer in this example) 

###1. Update the Models###

 In the User model (in app/User) we add:
```
    /**
     * A user can have many articles
     * 
     * We're writing this functoin so that when we create and article, 
     * we can associate that article with a user in our system
     * 
     * This allows us to do $user->articles later to see all articles belonging to you $user
     */
    public function articles()
    {
        return $this->hasMany('App\Article');
    }

```
- In the Article Model we add:
```
    /**
     * An article is owned by a user
     * This allows us to do $article->writer
     */
    public function writer()
    {
        return  $this->belongsTo('App\User');
    }
}
```

###2. Update the Migrations###
- We also need to add another column to the articles table. This column should be a foreign key.
- Either in the original migration `create_articles_table` or as a new migration which will update the table which we can call `add_foreign_keys_articles_table` we add the following:

```
$table->integer('user_id')->unsigned();

$table->foreign('user_id')
	->reference('id')
	->on('users')
	->onDelete('cascade') ;
```

- `onDelete('cascade')` would cause all articles with user_id to be deleted when that user_id is deleted

###3. How Relations are helpful###
- Now as a user you can access all it's articles by typing `$user->articles->toArray();`
- This is equivalent to `$user->articles()->get();` except you can add more constraints to the second one


###Extra: Encode a Password###
- 2 easy ways to encode passwords
	1. `$user->password = hash::make('password');`
	2. `$user->password = bcrypt('password');`



**excercise**: make it so that when I type /users.JonhDoe/articles it list all of the articles for John Doe