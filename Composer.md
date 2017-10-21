## Composer

#### autoload 

In your `composer.json`, you can specify the path for the autoload.

```
// set the autoload path to be the project root folder
"autoload": {
  "classmap": [
    "./"
  ]
}
```

Then you can run `composer dump-autoload` to look for all of the classes need to include again.
