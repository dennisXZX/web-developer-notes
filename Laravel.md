## Laravel

### Laravel Warm-up

The installation should be straighforward, but does have a little trip.

1. Install composer, which is a package manager for Laravel (composer --version)
2. Install Laravel using composer (php artisan --version)
3. Run `vi ~/.bash_profile` to add Laravel to the PATH
```
export PATH="vendor/bin:$PATH"
export PATH="~/.composer/vendor/bin:$PATH"
```
4. Run `source ~/.bash_profile` to reload the config file
5. Create a Laravel project by running `laravel new projectName`
6. Use `php artisan serve` to launch your project
