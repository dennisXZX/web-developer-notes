## Gulp

Install Gulp globally so you can run `gulp` command anywhere and also locally in your project. 

#### Gulp task

Create a basic Gulp task like the following.

```js
var gulp = require('gulp');
// Requires the gulp-sass plugin
var sass = require('gulp-sass');

// task to spin up a server using Browser Sync
gulp.task('browserSync', function() {
  browserSync.init({
    server: {
      baseDir: 'app'
    },
  })
})

// task to convert SASS into CSS
gulp.task('sass', function () {
  return gulp.src('app/scss/**/*.scss') // Get source files with gulp.src with globbing
    .pipe(sass()) // Sends it through a Gulp plugin
    .pipe(gulp.dest('app/css')) // Outputs the file in the destination folder
    .pipe(browserSync.reload({ // inject new CSS styles into the browser
      stream: true
    }))
})

// 'browserSync' and 'sass' tasks will run before 'watch'
gulp.task('watch', ['browserSync', 'sass'], function (){
  gulp.watch('app/scss/**/*.scss', ['sass']); 
  // Reloads the browser whenever HTML or JS files change
  gulp.watch('app/*.html', browserSync.reload); 
  gulp.watch('app/js/**/*.js', browserSync.reload);
});
```

You can test by running `gulp watch` now.

Gulp 4.0 brings in new task execution system. Now you can control whether to run tasks in sequence or in parallel.

```js
gulp.task('default', gulp.series('clean', 'webpack', 'sprites', gulp.parallel('scripts', 'stylesheets', 'pdfs'), 'shell'));
```
