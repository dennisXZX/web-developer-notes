## Laravel

#### Homestead development set-up

1. Install `vagrant` and `virtualbox`
2. Install the `Homestead` vagrant box

```
vagrant box add laravel/homestead
```

3. Clone the Homestead repository to your local machine and checkout out the latest release branch.

```
git clone https://github.com/laravel/homestead.git Homestead
```

4. Run `bash init.sh` to create a `Homestead.yaml` configuration file

5. Configure the shared folders

```
folders:
  - map: ~/code/project1
    to: /home/vagrant/code/project1

  - map: ~/code/project2
    to: /home/vagrant/code/project2

sites:
  - map: dennisxiao.dev
    to: /home/vagrant/code/project1/public
  - map: dx-blog.dev
    to: /home/vagrant/code/project2/public
```

Everytime the sites property is changes you need to run `vagrant reload --provision`.

6. Change the hosts file

Get into your hosts file by `sudo vi /etc/hosts` and add `192.168.10.10  dennisxiao.test`.

7. Visit your development domain `dennisxiao.dev` on your machine

8. Connect to your local database by the following credentials

```
Name: Homestead
Host: 127.0.0.1
Username: homestead
Password: secret
Database: (optional, whatever you want if you want to directly connect)
Port: 33060
```

Important: in the `.env` config file of your Laravel project, you should use `DB_HOST=localhost`.

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

`php artisan make:model modelName -m` to create a model as well as a database table associated with it. (the -m flag means migration)

`php artisan make:controller controllerName --resource` to create a resource controller.

`php artisan migrate` to migrate tables to database.

`php artisan route:list` to list all the routes.

#### Routes

```
Route::get('/zombie/{id}', 'ZombieController@show');

Route::resource('posts', 'PostController');
```

#### Blade template

Blade `{{ }}` statements are automatically sent through PHP's `htmlentities` function to prevent XSS attacks, so if you want to display HTML content in Blade, you would need to use `{!! !!}`.

You can pass variables from controller to view by using `with()`.

```
// now you can access the variable from the view using {{ $data }}
return view('pages.about')->withData($todos);
```
Extend from a layout using `@extend('templateName')`.

Inject dynamic content into a layout using `@yield('sectionName')`;

Specify dynamic content using `@section()`.

Include partial template using `@include('partials._message')`.

```
@section('title', ' | Homepage')

@section('sectionName')
  // dynamic content
@endsection
```
