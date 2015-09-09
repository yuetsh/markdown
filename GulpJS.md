# Gulp.Js
## 简介
### 自动化构建工具
```javascript
var gulp =require('gulp');
gulp.task('default',function(){
    //whatever
});
```

### 参见 **[Gulp开发教程：Building With Gulp](http://www.tuicool.com/articles/AzI3Ib)**

## 插件
### 安装插件
网上的：
```
npm install gulp-ruby-sass gulp-autoprefixer gulp-minify-css gulp-jshint gulp-concat gulp-uglify gulp-imagemin gulp-notify gulp-rename gulp-livereload gulp-cache del --save-dev  
```

目前项目中的：
```
npm install del gulp gulp-autoprefixer gulp-cache gulp-connect gulp-htmlmin gulp-imagemin gulp-minify-css gulp-jshint gulp-sass gulp-sourcemaps gulp-usemin gulp-watch gulp-uglify --save-dev 
```

## gulpfile.js
### 项目中的：
```javascript
var gulp = require('gulp');
var connect = require('gulp-connect');
var jshint = require('gulp-jshint');
var watch = require('gulp-watch');

var del = require('del');
var usemin = require('gulp-usemin');

var uglify = require('gulp-uglify');
var sourcemaps = require('gulp-sourcemaps');

var minifyCss = require('gulp-minify-css');
var sass = require('gulp-sass');
var autoprefixer = require('gulp-autoprefixer');

var imagemin = require('gulp-imagemin');
var cache = require('gulp-cache');

var htmlmin = require('gulp-htmlmin');


gulp.task('clean', function (cb) {
    del(['dist/**'], cb);
});

gulp.task('lint', function () {
    return gulp.src(['src/scripts/**/*.js'])
        .pipe(jshint())
        .pipe(jshint.reporter('default'))
        .pipe(jshint.reporter('fail'));
});

gulp.task('watch-sass', function () {
    return gulp.src('src/assets/styles/*.scss')
        .pipe(sourcemaps.init())
        .pipe(sass({errLogToConsole: true}))
        .pipe(autoprefixer({ browsers: ['last 2 versions', 'last 4 Android versions'] }))
        .pipe(sourcemaps.write('.'))
        .pipe(gulp.dest('.tmp/assets/styles'))
        .pipe(connect.reload());
});

gulp.task('watch', function () {
    return gulp.watch('src/assets/styles/**/*.scss', ['watch-sass']);
});

gulp.task('connect', function () {
    connect.server({
        port: 8000,
        livereload: true,
        root: ['src/', '.tmp/', 'shim/']
    });
});

// default task
gulp.task('default',
    ['lint', 'connect', 'watch-sass', 'watch']
);


gulp.task('usemin', ['clean'], function () {
    return gulp.src('src/index.html')
        .pipe(usemin({
            js: [sourcemaps.init(), uglify(), sourcemaps.write('')],
            js1: [sourcemaps.init(), uglify(), sourcemaps.write('')],
            js2: [sourcemaps.init(), uglify(), sourcemaps.write('')],
            //scss: [sourcemaps.init(), sass({errLogToConsole: true}), autoprefixer({ browsers: ['last 2 version'] }), minifyCss({comments: true, spare: true}), sourcemaps.write('')],
            css: [sourcemaps.init(), autoprefixer({ browsers: ['last 2 version'] }), minifyCss({comments: true, spare: true}), sourcemaps.write('')],
            //html: [htmlmin({collapseWhitespace: true})]
        }))
        .pipe(gulp.dest('dist'));
});

gulp.task('minify-scss', ['clean'], function () {
    var opts = {comments: true, spare: true};
    return gulp.src(['src/assets/styles/*.scss'], {base: 'src/assets/styles'})
        .pipe(sourcemaps.init())
        .pipe(sass({errLogToConsole: true}))
        .pipe(autoprefixer({ browsers: ['last 2 versions', 'last 4 Android versions'] }))
        .pipe(minifyCss(opts))
        .pipe(sourcemaps.write(''))
        .pipe(gulp.dest('dist/assets/styles'));
});

gulp.task('minify-img', ['clean'], function () {
    return gulp.src('src/assets/images/**', {base: 'src/assets/images'})
        .pipe(cache(imagemin({
            optimizationLevel: 3,
            progressive: true,
            interlaced: true
        })))
        .pipe(gulp.dest('dist/assets/images'));
});

gulp.task('copy-assets', ['clean'], function () {
    return gulp.src(['src/assets/**', '!src/assets/styles', '!src/assets/styles/**', '!src/assets/images', '!src/assets/images/**'], {base: 'src/assets'})
        .pipe(gulp.dest('dist/assets/'));
});

gulp.task('copy-html-files', ['clean'], function () {
    return gulp.src(['src/templates/**/*.html', 'src/*.html', '!src/index.html'], {base: 'src'})
        //.pipe(htmlmin({collapseWhitespace: true}))
        .pipe(gulp.dest('dist/'));
});

// build task
gulp.task('build',
    ['clean', 'usemin', 'minify-img', 'minify-scss', 'copy-html-files', 'copy-assets']
);

gulp.task('connectDist', function () {
    connect.server({
        root: ['dist/', 'shim/'],
        port: 9000
    });
});
```
### 具体语法：
以livereload为例子 // 7月8日
```
//这个需要浏览器插件 **livereload**
```
```
npm install gulp-livereload --save-dev
```
```
cd gulpfile.js
```
```javscript
var gulp = require('gulp'),
    sass = require('gulp-ruby-sass'),
    uglify = require('gulp-uglify'),
    livereload = require('gulp-livereload');

function errorLog(error){
    console.error.bind(error);
    this.emit('end');
}

//Scripts Task
//Uglifies
gulp.task('script',function(){
    gulp.src('js/*.js')
        .pipe(uglify())
	.on('error',errorLog)
	.pipe(gulp.dest('bulid/js'));
});

//Style Task
//Uglifies
gulp.task('style',function(){
    gulp.src('scss/**/*.scss')
        .pipe(sass({
	    style:'compressed'
	}))
	.on('error',errorLog)
	.pipe(gulp.dest('css/'))
	.pipe(livreload());
});

//Watch Task
gulp.task('watch',function(){

    var server = livereload();

    gulp.watch('js/*.js',['scripts']);
    gulp.watch('scss/**/*.scss',['style']);
})
```

以imagemin为例子 // 7月8日
```javascript
var imagemin = require('gulp-imagemin');

//Image Task
//Compress
gulp.task('image',function(){
    gulp.src('img/*')
        .pipe(imagemin())
        .pipe(gulp.dest('build/img'));
});

```
以autoprefixer为例子 // 7月8日
```javascrpit

```