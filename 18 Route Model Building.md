## 18 Route Model Building ##

Our `RouteServiceProvider` looks like this:
```
<?php namespace App\Providers;

use Illuminate\Routing\Router;
use Illuminate\Foundation\Support\Providers\RouteServiceProvider as ServiceProvider;

class RouteServiceProvider extends ServiceProvider {

	/**
	 * This namespace is applied to the controller routes in your routes file.
	 *
	 * In addition, it is set as the URL generator's root namespace.
	 *
	 * @var string
	 */
	protected $namespace = 'App\Http\Controllers';

	/**
	 * Define your route model bindings, pattern filters, etc.
	 *
	 * @param  \Illuminate\Routing\Router  $router
	 * @return void
	 */
	public function boot(Router $router)
	{
		parent::boot($router);

		$router->model
		
	}

	/**
	 * Define the routes for the application.
	 *
	 * @param  \Illuminate\Routing\Router  $router
	 * @return void
	 */
	public function map(Router $router)
	{
		$router->group(['namespace' => $this->namespace], function($router)
		{
			require app_path('Http/routes.php');
		});
	}

}
```
**Notes**
	 - the first line makes it so that this namespace is applied to the controller routes in  the `routes` file.
	 - in `map()` it shows us where we load our routes from
	 - **`boot()`** is called on each service provider after everything has been registered
	 - `php artisan route:list` will display each route

All we need to do is add this to the `boot()` method:
```
	public function boot(Router $router)	{
		parent::boot($router);

		$router->model('articles', 'App\Atricle');
	}
```

and in the `ArticlesController` we an now use the `show_better_way()` instead:
```
    public function show_old_way($id)
    {
        $article = Article::findOrFail($id);
        return view('articles.show', compact('article'));
    }
    public function show_better_way(Article $article)
    {
        return view('articles.show', compact('article'));
    }
```
and in the `routes` file we have:
```
Route::get('articles/{id}', 'ArticlesController@show_better_way');
```


**Note**

 - if you need to override the default logic for fetching a record, then in `RouteServiceProvider` use `$router->bind()`. For example:

 ```
 $router->bind('articles', function($id)){
	 return \App\Article::published()->findOrFail($id);
}
 ```

