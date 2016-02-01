# Eloquent ORM #

* The Eloquent ORM (object-relational mapping) is a simple ActiveRecord implementation for working with your database. Each database table has a corresponding "Model" which is used to interact with that table.
* ActiveRecord - one class represents the associated row from database table
* Eloquent model name should be singular and tables plural 
    * Eg: model name is `User` and table name is `users`

```
	$php artisan make:model NameOfModel
```
* This creates the class which extends model
* The class assumes the table is the plural of NameOfModel

```
	php artisan tinker
```

### Add new Data ###
```
$article = new App\Article
$article->title = 'My First Article'
$article->save()
```
### Add new Data Fast ###
First you have to specify which attributes are you ok with being mass assigned
```
class Article extends Model {
	protected $fillable = [
		'title',
		'body',
		'published_at'
	];
}
```
To make all fillable except some use `ignore or something`

**Mass assign data**
create([]) creates and then persists the attributes all at once (so you don't need to call save())
```
$article = App\Article::create([
	'title' => 'New Article', 
	'published_at' => Carbon\Cabon::now()
	]);
```

### Update Data ###
**First Way**
```
$article->title = 'My Updated First Article';
$article->save();
```
**Second Way**
This way does save() automatically
```
$article->update(['body' => 'This is the updated body'])
```
##Accessing data##
Example: `$article=App\Article::find(1); $article->toArray;`now we can echo out the our data like so `$article->title;`

MySQL    | Eloquent
-------- | ---
`select * from table`  | `App\Article::all();` This retrieves all the table's records
`select * from table where id = 1;`  | `App\Article::find(1);`
`select * from articles where body = 'blob';`  | `App\Article::where('body', 'blah')->get();` This returns a collection
First record that matches the where clause     | `App\Article::where('body', 'blah')->first();` This returns the first result. We can echo the body now with `$article->body`

###Useful Methods in Model###
 - `save()` -  persists record to the database after you fetch a record from the database and modify it
 - `update()`
 - `find()`
 - `create()`

###Other Useful Methods###

 - `$article->published_at = Carbon\Carbon::now();`
 - `$article->toArray();`