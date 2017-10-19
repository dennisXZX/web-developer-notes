## Laravel

#### Homestead development set-up

#### Laravel installation

The installation should be straighforward, but does have a little trip.

1. Install composer, which is a package manager for Laravel (composer --version)
2. Install Laravel using composer
```
composer global require "laravel/installer"
```
3. Run `vi ~/.bash_profile` to add Laravel to the PATH
```
export PATH="vendor/bin:$PATH"
export PATH="~/.composer/vendor/bin:$PATH"
```
4. Run `source ~/.bash_profile` to reload the config file
5. Create a Laravel project by running `laravel new projectName`
6. Go into a Laravel project directory and use `php artisan --version` to check Laravel version and `php artisan serve` to launch your project

#### Artisan Command

`php artisan make:model modelName -m` to create a model as well as a database table associated with it.

`php artisan make:controller controllerName --resource` to create a resource controller.

`php artisan migrate` to create tables in database.

`php artisan route:list` to list all the routes.

#### Routes

```
Route::get('/zombie/{id}', function($id) {
  echo 'This zombie has an id of ' . $id;
});
```
