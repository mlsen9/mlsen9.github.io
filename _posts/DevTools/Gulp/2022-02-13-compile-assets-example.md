---
title: Gulp 前端资源编译代码示例
date: 2022-02-13 11:53:33 +0800
categories: [DevTools, Gulp]
tags: [js-devtools, gulp]
---

## 压缩/合并/重命名等任务代码示例

### 用到的模块
-   [del](https://npm.taobao.org/package/del) — 删除文件(替代gulp-clean)
-   [gulp-uglify](https://npm.taobao.org/package/gulp-uglify)  — 压缩JavaScript
-   [gulp-clean-css](https://npm.taobao.org/package/gulp-clean-css)  — 压缩CSS
-   [gulp-imagemin](https://npm.taobao.org/package/gulp-imagemin)  — 压缩图片
-   [gulp-rename](https://npm.taobao.org/package/gulp-rename)  — 重命名
-   [gulp-concat](https://npm.taobao.org/package/gulp-concat)  — 合并
-   [gulp-filter](https://npm.taobao.org/package/gulp-filter)  — 过滤文件

### gulpfile.js 代码示例
```javascript

var gulp = require('gulp');
var uglify = require('gulp-uglify');
var clean = require('gulp-clean-css');
var imagemin = require('gulp-imagemin');
var rename = require('gulp-rename');
var concat = require('gulp-concat');

// 压缩js
gulp.task('jscompress', function () {
  return gulp.src(['./javascripts/common.js', './javascripts/index.js'])
    .pipe(uglify())
    .pipe(rename({
      suffix: '.min'
    }))
    .pipe(gulp.dest('./javascripts'))
})

// 压缩css
gulp.task('csscompress', function () {
  return gulp.src(['./stylesheets/common.css', './stylesheets/index.css'])
    .pipe(clean())
    .pipe(rename({
      suffix: '.min'
    }))
    .pipe(gulp.dest('./stylesheets'))
})

// 压缩image
gulp.task('imagecompress', function () {
  return gulp.src(['./images/*.jpg', './images/*.png'])
    .pipe(imagemin())
    .pipe(gulp.dest('./images'))
})

// 合并js
gulp.task('concatJs', function () {
  return gulp.src(['/javascripts/index.js', './javascripts/common.js'])
    .pipe(concat('concat.js'))
    .pipe(gulp.dest('./javascripts'))
})

// 监听js和css的改动
gulp.task('auto', function () {
  gulp.watch('./javascripts/*.js', ['jscompress']);
  gulp.watch('./stylesheets/*.css', ['csscompress']);
})

// 默认任务
gulp.task('default', ['jscompress', 'csscompress', 'imagecompress', 'concatJs']);


```
