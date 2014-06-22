# gulp-svg-symbols

[![NPM version](https://badge.fury.io/js/gulp-svg-symbols.svg)](http://badge.fury.io/js/gulp-svg-symbols) [![Build Status](https://travis-ci.org/Hiswe/gulp-svg-symbols.svg?branch=master)](https://travis-ci.org/Hiswe/gulp-svg-symbols)

*gulp-svg-symbols* is a minimal plugin for [gulp](http://gulpjs.com).  
It converts a bunch of svg files to a single svg file containing each one as a symbol.  
See [css-trick](http://css-tricks.com/svg-symbol-good-choice-icons/) for more details.

The plugin produce 2 files:

- *svg-symbols.svg* containing all SVG symbols
- *svg-symbols.css* containing all classes with the right svg sizes

## install

```
npm install --save-dev gulp-svg-symbols
```

## example

in your gulpfile.js

```js
var gulp = require('gulp');
var svgSymbols = require('gulp-svg-symbols');

gulp.task('sprites', function () {
  return gulp.src('assets/svg/*.svg')
    .pipe(svgSymbols())
    .pipe(gulp.dest('assets'));
});
```

in your HTML, you first have to [reference the SVG](http://css-tricks.com/svg-sprites-use-better-icon-fonts/)  
then:

```html
<svg role="img" class="github"> 
  <use xlink:href="#github"></use> 
</svg>
```

- **class** is the one generated in the CSS file
- **xlink:href** is the symbol id in the SVG file

## options

You can overload the default options by passing an object as an argument to ```svgSymbols()```  
```%f``` is the file name. 

Default options can be find [here](https://github.com/Hiswe/gulp-svg-symbols/blob/master/lib/default-config.js)

Some observations

- The `fontSize` options le you define a base font.  
  If it's superior to 0, then the sizes in your CSS file will be in **em**.  
  Otherwise it sticks to **pixels** (default behavior).
- If `css` option is set to `false`, the plugin won't ouput the CSS file.   
- If you want to change the file name use [gulp-rename](https://www.npmjs.org/package/gulp-rename)  
- If you want to change the generated files name, again use [gulp-rename](https://www.npmjs.org/package/gulp-rename)
- If you want different destination for the files, use [gulp-if](https://www.npmjs.org/package/gulp-if)
- Unlike [gulp-svg-sprites](https://www.npmjs.org/package/gulp-svg-sprites) it doesn't generate preview files.
- Unlike [gulp-svg-sprites](https://www.npmjs.org/package/gulp-svg-sprites) there is no way to add padding to svg files.   
 It should be the purpose of another plugin, no?

So put together, you will have something like that:

```js
var gulp 		= require('gulp');
var if 			= require('gulp-if');
var rename 		= require('gulp-rename');
var svgSymbols 	= require('gulp-svg-symbols');

gulp.task('sprites', function () {
  return gulp.src('assets/svg/*.svg')
   	.pipe(rename(renameFunction))
    .pipe(svgSymbols({
      svgId:     'icon-%f',
	  className: '.icon-%f',
	  fontSize:   16,
	  css: false
    }))
    .pipe(rename(outputFilesRenameFunction))
    .pipe(gulp.dest('views/svg'));
});
```

## Release History

- **0.1.2** — Config for optional CSS output 
- **0.1.1** — Add travis build
- **0.1.0** — Publish to NPM and fix [watch issue](https://github.com/Hiswe/gulp-svg-symbols/issues/2)
- **0.0.2** — Css can be generated with *em* size
- **0.0.1** — First release

## All credits goes to

- [Chris Coyier](http://css-tricks.com/) for the trick
- [Shaky Shane](https://www.npmjs.org/~shakyshane) for the [gulp-svg-sprites](https://www.npmjs.org/package/gulp-svg-sprites) plugin
