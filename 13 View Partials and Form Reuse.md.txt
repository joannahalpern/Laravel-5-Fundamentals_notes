#13 View Partials and Form Reuse#

```
Route::get('articles', 'ArticlesController@index);
Route::get('articles/create', 'ArticlesController@create);
Route::get('articles/{id}', 'ArticlesController@show);
Route::post('articles', 'ArticlesController@store);
Route::get('articles/{id}/edit', 'ArticlesController@edit);

//OR

Route:resource('articles', 'ArticlesController');
```
`Route:resource` will automatically create the RESTful routes which are index, create, show, store, edit, update, and destroy. These can all be viewed with the command `php artisan route:list`


##Editing the article example##
 - When we want to submit an edit, the convention is to submit a patch request to articles/{id}
 - this way, we limit the number of URIs we have to create
```
@extends('app')
@section('content')
	//using url
	{!! Form::open(['method' => 'PATCH', 'url' => 'articles/'.$article->id])!!}
	
	//OR by route if you have a named route (aka alias)
	{!! Form::open(['method' => 'PATCH', 'route' => 'articles/'.$article->id])!!}
	
	//OR by action
	{!! Form::open(['method' => 'PATCH', 'action' => ['ArticlesController@update', $article->id]])!!}
```
**Form Model Binding** - bind an eloquent model to a form
```
Form::model($article, ['method' => 'PATCH', 'action' => ['ArticlesController@update', $article->id]]
```
 **Requests** are the form authorization
- type `php artisan make:request ArticleRequest` to make new request. It can then be found in app/Http/Requests
-  to allow anyone to submit for change the `authorize()` method to `return true` and then add your validations in `rules()`
For example:
```
	public function rules()
	{
		return [
			'title' => 'required\min:3',
            'body' => 'required',
            'published_at' => 'required|date'
		];
	}
```