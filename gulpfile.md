
```javascript

// 获取 gulp
var gulp = require('gulp');

// 获取 uglify 模块（用于压缩 JS）
var uglify = require('gulp-uglify');

// js文件集合
var jsFiles = ['src/**/*.js'];
// 图片资源
var imgFiles = ['src/**/*.png', 'src/**/*.jpeg', 'src/**/*.jpg', 'src/**/*.gif'];
// css文件组合
var cssFiles = ['src/**/*.css'];
// 复制文件
var copyFiles = ['src/**/*.html', 'src/**/*.php', 'src/**/*.json'];

// 获取 minify-css 模块（用于压缩 CSS）
var minifyCSS = require('gulp-minify-css');
// 获取 gulp-imagemin 模块
var imagemin = require('gulp-imagemin');
var gutil = require('gulp-util');
var watchPath = require('gulp-watch-path');
var sourcemaps = require('gulp-sourcemaps');

// 压缩 js 文件
// 在命令行使用 gulp script 启动此任务
gulp.task('script', function() {
    // 1. 找到文件
    gulp.src(jsFiles)
    // 2. 压缩文件
       .pipe(uglify())
    // 3. 另存压缩后的文件
       .pipe(gulp.dest('dist'));
});

// 压缩 css 文件
// 在命令行使用 gulp css 启动此任务
gulp.task('style', function () {
    // 1. 找到文件
    gulp.src(cssFiles)
    // 2. 压缩文件
        .pipe(minifyCSS())
    // 3. 另存为压缩文件
        .pipe(gulp.dest('dist'));
});


// 复制压缩图片资源
gulp.task('images', function() {

    // 找到图片
    gulp.src(imgFiles)
        // 2. 压缩图片
        .pipe(imagemin({
            progressive: true
            // optimizationLevel: 5, //类型：Number  默认：3  取值范围：0-7（优化等级）
            // progressive: true, //类型：Boolean 默认：false 无损压缩jpg图片
            // interlaced: true, //类型：Boolean 默认：false 隔行扫描gif进行渲染
            // multipass: true //类型：Boolean 默认：false 多次优化svg直到完全优化
        }))
        // 另存为
        .pipe(gulp.dest('dist'));

});

// 复制文件
gulp.task('copy', function () {
    gulp.src(copyFiles)
        .pipe(gulp.dest('dist'));
});

// 监测脚本变化
var watchScript = function(){
    gulp.watch(jsFiles, function (event) {
        var paths = watchPath(event, 'src/', 'dist/');
        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath);
        gutil.log('Dist ' + paths.distPath);

        gulp.src(paths.srcPath)
            .pipe(uglify())
            .pipe(gulp.dest(paths.distDir));
    });
}

// 监测样式文件变化
var watchStyle = function(){
     gulp.watch(cssFiles, function (event) {
        var paths = watchPath(event, 'src/', 'dist/');

        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath);
        gutil.log('Dist ' + paths.distPath);

        gulp.src(paths.srcPath)
            .pipe(sourcemaps.init())
            .pipe(minifyCSS())
            .pipe(sourcemaps.write('./'))
            .pipe(gulp.dest(paths.distDir))
    });
}

// 监测图片资源变化
var watchImages = function(){
     gulp.watch(imgFiles, function (event) {
        var paths = watchPath(event,'src/','dist/');

        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath);
        gutil.log('Dist ' + paths.distPath);

        gulp.src(paths.srcPath)
            .pipe(imagemin({
                progressive: true
            }))
            .pipe(gulp.dest(paths.distDir))
    });
}

// 监听复制文件
var watchCopy = function(){
     gulp.watch(copyFiles, function (event) {
        var paths = watchPath(event,'src/','dist/');

        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath);
        gutil.log('Dist ' + paths.distPath);

        gulp.src(paths.srcPath)
            .pipe(gulp.dest(paths.distDir))
    });
}

// 监听任务
gulp.task('listening', function () {
    watchScript();
    watchStyle();
    watchImages();
    watchCopy();
});

gulp.task('run', ['script', 'style', 'images', 'copy', 'listening']);

gulp.task('default', ['run']);

// "devDependencies": {
//     "colors": "^1.1.2",
//     "gulp": "^3.9.0",
//     "gulp-autoprefixer": "^3.1.0",
//     "gulp-imagemin": "^2.3.0",
//     "gulp-less": "^3.0.3",
//     "gulp-minify-css": "^1.2.1",
//     "gulp-ruby-sass": "^2.0.4",
//     "gulp-sourcemaps": "^1.6.0",
//     "gulp-uglify": "^1.4.2",
//     "gulp-util": "^3.0.6",
//     "gulp-watch-path": "^0.1.0",
//     "stream-combiner2": "^1.0.2"
// }

// 安装相关依赖
// npm install colors gulp-autoprefixer gulp-imagemin gulp-less gulp-minify-css gulp-ruby-sass gulp-sourcemaps gulp-uglify gulp-util gulp-watch-path stream-combiner2 --save-dev
// 安装相关依赖 除去Less、Sass、autoprefixer
// npm install colors  gulp-imagemin gulp-minify-css gulp-sourcemaps gulp-uglify gulp-util gulp-watch-path stream-combiner2 --save-dev

// autoprefixer 解析 CSS 文件并且添加浏览器前缀到CSS规则里。 通过示例帮助理解

// gulp-watch-path
// 检测变换路径
// 此配置有个性能问题，当 gulp.watch 检测到 src/js/ 目录下的js文件有修改时会将所有文件全部编译。实际上我们只需要重新编译被修改的文件。

// gulp-sourcemaps
// 压缩后的代码不存在换行符和空白符，导致出错后很难调试，好在我们可以使用 sourcemap 帮助调试

// 详细介绍: https://github.com/nimojs/gulp-book/blob/master/chapter7.md

// gulp-autoprefixer
// autoprefixer 解析 CSS 文件并且添加浏览器前缀到CSS规则里。 会加大css文件大小，慎用

```

[介绍文档](https://github.com/nimojs/gulp-book/blob/master/chapter7.md)

[GoBack](README.md)