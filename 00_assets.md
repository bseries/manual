# Assets

## Side-Autoloading of JavaScript Assets

For autoloading of JavaScript it is important, that
files are named using camelCase:

BAD:
  - `assets/js/foo-bar.js`
  - `assets/js/foo.bar.js`

GOOD:
  - `assets/js/fooBar.js`

The following JavaScript scripts will always be loaded and bootstrap the application's JavaScript, thus must always be present:

  - `assets/js/require.js`
  - `assets/js/base.js`

Often you've got JavaScript that is specific to a single view and action. Place files using the following path schema to autoload them once a view is requested. When using AMD in these files they must use `require()` instead of `define()`.

  - `assets/js/views/layouts/<layout>.js`
  - `assets/js/views/<controller>/<action>.js`

Such an autoloadable files would look like this:
```javascript
require(['jquery', 'domready!'], function($) {
  // ...
});
```

## Side-Autoloading of CSS Assets

Autoloading of CSS files is very similar to autoloading of JavaScript files. These Stylesheets are always loaded and must be present:

  - `assets/css/base.css`

View specific CSS can be placed into the "views" subdirectory structure similar as described for JavaScript above.

  - `assets/css/views/layouts/<layout>.css`
  - `assets/css/views/<controller>/<action>.css`

All `views` CSS files, except those in `elements`, are loaded automatically when needed. However, its good practice to
scope them to their page (here: `views/pages/home.css`):

```css
.home .banner {
	/* ... */
}
```
