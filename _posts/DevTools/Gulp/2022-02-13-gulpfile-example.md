---
title: gulpfile.js 代码示例
date: 2022-02-13 11:53:33 +0800
categories: [DevTools, Gulp]
tags: [js-devtools, gulp]
---

## gulpfile.js 代码示例

```javascript
var gulp = require('gulp');
var uglify = require('gulp-uglify');
var csso = require('gulp-csso');
var del = require('del');
var paths = {
    input: {
        css: 'src/css/*.css',
        js: 'src/js/*.js'
    },
    output: {
        css: 'dist/css/',
        js: 'dist/js/'
    }
};

function getTime(){
    return new Date().getTime();
}

// 清理已编译文件
gulp.task('del', function () {
    del([paths.output.css, paths.output.js]);
});
// 压缩CSS文件
gulp.task('minify_css', function () {
    console.log('task:minify_css ' + getTime() + ' is running...');
    return gulp.src(paths.input.css)
        .pipe(csso())
        .pipe(gulp.dest(paths.output.css));
});
// 压缩JS文件
gulp.task('minify_js', function () {
    console.log('task:minify_js ' + getTime() + ' is running...');
    return gulp.src(paths.input.js)
        .pipe(uglify())
        .pipe(gulp.dest(paths.output.js));
});

// v4.*.* 版本示例
// 串行化任务
gulp.task('series', gulp.series('minify_css', 'minify_js', function () {
    console.log('task:series ' + getTime() + ' is running...');
}));
// 并行化任务
gulp.task('parallel', gulp.parallel('minify_css', 'minify_js', function () {
    console.log('task:parallel ' + getTime() + ' is running...');
}));

// --------------------

// v3.*.* 版本示例
gulp.task('default', ['minify_css', 'minify_js'], function () {
    console.log('task:default ' + getTime() + ' is running...');
});
```
