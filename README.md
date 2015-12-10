
# [gulp](https://github.com/wearefractal/gulp)-rev-collector-r base on [gulp-rev-collector](https://www.npmjs.com/package/gulp-rev-collector)

> Static asset revision data collector from manifests, generated from different streams, and replace their links in html template `and require module in js`.

## Install

```sh
$ npm install --save gulp-rev-collector-r
```

## Usage

```js
 // adjust manifest.json
 var replace = require('gulp-replace')
 gulp.task('adjust_manifest', function(){
    return gulp.src(["webapp/rev_config/js/*.json"])
        .pipe(replace(/"(.*)\.js":/g, '"pages/$1":'))
        .pipe(replace(/:\s"(.*)\.js"/g, ': "pages/$1"'))
        .pipe(gulp.dest('webapp/rev_config/js'));
});
...

var revCollector = require('gulp-rev-collector-r');

gulp.task('rev', function () {
    return gulp.src(['rev/**/*.json', 'templates/**/*.html'])
        .pipe( revCollector({
            replaceReved: true,
            isRequirejs: true
        }) )
        .pipe( minifyHTML({
                empty:true,
                spare:true
            }) )
        .pipe( gulp.dest('dist') );
});
```

### Options

#### replaceReved

Type : `Boolean`

You set a flag, replaceReved, which will replace alredy replaced links in template's files. Default value is `false`.

#### dirReplacements

Specifies a directories replacement set. [gulp-rev](https://github.com/sindresorhus/gulp-rev) creates manifest files without any info about directories. E.c. if you use dirReplacements param from [Usage](#usage) example, you get next replacement:

```
"/css/style.css" => "/dist/css/style-1d87bebe.css"
"/js/script1.js" => "/dist/script1-61e0be79.js"
"cdn/image.gif"  => "//cdn8.example.dot/img/image-35c3af8134.gif"
```

#### revSuffix

Type : `String`

It is pattern for define reved files suffixes. Default value is '-[0-9a-f]{8,10}-?'. This is necessary in case of e.c. [gulp-rename](https://github.com/hparra/gulp-rename) usage. If reved filenames had different from default mask.


### Works with gulp-rev-collector

- [gulp-rev](https://github.com/sindresorhus/gulp-rev)

## License

[MIT](http://opensource.org/licenses/MIT) © [Oleksandr Ovcharenko](mailto:shonny.ua@gmail.com)
