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
    "gulp-sourcemaps": "^2.6.5"
  }
}
```

4. `yarn` will install all packages
5. `gulp` if you have installed gulp globally, or else, run `node_modules/gulp/bin/gulp.js`

### Exposing client dist folder

Need to expose the `dist` folder in the theme.

See https://github.com/silverstripe/vendor-plugin
