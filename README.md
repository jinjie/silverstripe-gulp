## SilverStripe 4 Gulp Workflow

### Setting up

1. Clone repository into `themes`
2. `rm -rf .git`
3. Add `package.json` to the root of your project, with the following (change accordingly):

```json
{
  "name": "app",
  "version": "1.0.0",
  "description": "SilverStripe 4 Gulp Workflow",
  "dependencies": {
    "gulp": "^4.0.2",
    "gulp-browserify": "^0.5.1",
    "gulp-sass": "^4.0.2",
    "gulp-sourcemaps": "^2.6.5",
    "browser-sync": "^2.26.7"
  }
}
```

4. Add `gulpfile.js` to the root of your project with the following (change accordingly):

```js
'use strict';

var
    // Path to the theme directory (no trailing slash)
    THEME_DIR = 'themes/mysite',
    PROXY_URL = 'localhost:8080/mysite',

    gulp        = require('gulp'),

    sass        = require('gulp-sass'),
    sourcemaps  = require('gulp-sourcemaps'),

    browserify  = require('gulp-browserify'),

    browserSync = require('browser-sync').create();
;

gulp.task('sass', function(cb) {
    gulp.src(THEME_DIR + '/client/src/sass/*.scss')
        .pipe(sourcemaps.init())
        .pipe(sass({
            outputStyle: 'compressed',
            includePaths: [
                // to import other sass something like
                // @import "bootstrap/scss/bootstrap.scss";
                './node_modules'
            ]
        })
        .on('error', sass.logError))
        .pipe(gulp.dest(THEME_DIR + '/client/dist/css'))
        .pipe(browserSync.stream());

    cb();
});

gulp.task('browserify', function(cb) {
    gulp.src(THEME_DIR + '/client/src/js/*.js')
        .pipe(browserify())
        .pipe(gulp.dest(THEME_DIR + '/client/dist/js'))
        .pipe(browserSync.stream());

    cb();
});

gulp.task('watch', function(cb) {
    browserSync.init({
        proxy: PROXY_URL,
        files: [
            THEME_DIR + '/templates/**/*.ss'
        ],
        notify: false
    });

    gulp.watch(THEME_DIR + '/client/src/sass/**/*.scss', gulp.series(['sass']));
    gulp.watch(THEME_DIR + '/client/src/js/*.js', gulp.series(['browserify']));

    cb();
});

gulp.task('default', gulp.series('sass', 'browserify'));
```

4. `yarn` will install all packages
5. `gulp` if you have installed gulp globally, or else, run `node_modules/gulp/bin/gulp.js`

### Exposing client dist folder

Need to expose the `dist` folder in the theme.

See https://github.com/silverstripe/vendor-plugin

### Update .gitignore

Add `/node_modules` to .gitignore in the root

`echo "/node_modules" >> .gitignore
