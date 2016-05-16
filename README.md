# Vue專案包備忘
vue專案備忘紀錄

## npm 佈建

### 全域套件
```
npm i -g gulp-cli
npm i -g nodemon
```

### 開發套件
```bash
npm i --save-dev babel-core babel-loader babel-preset-es2015 css-loader del ejs file-loader gulp gulp-concat gulp-ejs gulp-flatten gulp-less gulp-livereload gulp-nodemon gulp-uglify less less-loader raw-loader should style-loader url-loader vue webpack webpack-stream babel-preset-stage-0 vue-loader gulp-util
```

內容
```json
"devDependencies": {
        "babel-core": *,
        "babel-loader": *,
        "babel-preset-es2015": *,
        "css-loader": *,
        "del": *,
        "ejs": *,
        "file-loader": *,
        "gulp": *,
        "gulp-util": *,
        "gulp-concat": *,
        "gulp-ejs": *,
        "gulp-flatten": *,
        "gulp-less": *,
        "gulp-livereload": *,
        "gulp-nodemon": *,
        "gulp-uglify": *,
        "less": *,
        "less-loader": *,
        "raw-loader": *,
        "should": *,
        "style-loader": *,
        "url-loader": *,
        "vue": *,
        "webpack": *,
        "webpack-stream": *,
        "babel-preset-stage-0": *,
        "vue-loader": *
    }
```

### Node服務
```bash
npm i --save body-parser compression cookie-parser cors express method-override express-ipv4
```

內容
```
body-parser
compression
cookie-parser
cors
express
method-override
express-ipv4
```

### webpack.config.js
```js
var path = require('path');
var webpack = require('webpack');
module.exports = {
    devtool: "source-map",
    entry: {
        'index': ['./src/index.js']
    },
    output: {
        filename: '[name].js',
        publicPath: 'js/',
        path: path.join(__dirname, 'www', 'js')
    },
    module: {
        noParse: [],
        loaders: [{
            loader: "babel-loader",
            test: /\.js$/,
            query: {
                presets: ['es2015']
            }
        }, {
            test: /\.less$/,
            loader: 'style!css!less'
        }, {
            test: /\.html$/,
            loader: 'raw'
        }, {
            test: /\.(png|jpg)$/,
            loader: 'url?limit=25000'
        },{
            test: /\.vue$/,
            loader: 'vue'
        }]
    },
    plugins: [
        new webpack.optimize.UglifyJsPlugin({
            compress: {
                warnings: false
            },
        }),
        new webpack.optimize.CommonsChunkPlugin({
            name: 'vendors',
            filename: "vendors.js",
            minChunks: Infinity
        }),
        // new webpack.ProvidePlugin({
        //     $: "jquery",
        //     jQuery: "jquery",
        //     d3: "d3"
        // })
    ]
};

```

### gulpfile.js

```js
var gulp = require('gulp');
var webpack = require('webpack-stream');
var gulpLivereload = require('gulp-livereload');
var gulpUglify = require('gulp-uglify');
var flatten = require('gulp-flatten');
var concat = require('gulp-concat');
var nodemon = require('gulp-nodemon');
var del = require('del');
var ejs = require("gulp-ejs");
var less = require('gulp-less');
var path = require('path');
var gutil = require('gulp-util');


gulp.task('vendors', function() {
    gulp.src([
            //'bower_components/vue/dist/vue.min.js',
            //'bower_components/js-cookie/src/js.cookie.js',
            //'bower_components/js-md5/js/md5.min.js',
        ])
        .pipe(gulpUglify())
        .pipe(flatten())
        .pipe(concat('vendors.js'))
        .pipe(gulp.dest('www/vendors'));
});

gulp.task('scripts', function() {
    return gulp.src('./xxx.js') //隨便打
        .pipe(webpack(require('./webpack.config.js')))
        .pipe(gulp.dest('www/js'))
        .pipe(gulpLivereload());
});

gulp.task('ejs', function() {
    return gulp.src([
            "./ejs/index.html",
        ])
        .pipe(gulp.dest("./view"))
        .pipe(gulpLivereload());
});

gulp.task('less', function() {
    return gulp.src([
            './less/index.less',
        ])
        .pipe(less())
        .pipe(gulp.dest('./www/css'))
        .pipe(gulpLivereload());
});

gulp.task('clear_vendors', function() {
    return del([
        'www/js/vendors.*',
    ]);
});

gulp.task('clear', function() {
    return del([
        'www/css/*.css',
        'www/js/*.js',
        'www/vendors/*.js',
        'view/*.html'
    ]);
});

/**
 * watch 的配置
 */
gulp.task('watch', function() {
    gulpLivereload.listen();
    gulp.watch(['ejs/*.html'], ['ejs']);
    gulp.watch(['less/*.less'], ['less']);
    gulp.watch(['src/**'], ['scripts']);
});

/**
 * nodemon 的配置
 */
var nodemonConfig = {
    script: 'app/app.js',
    ignore: [
        "bower_components/**",
        "less/**",
        "www/**",
        "view/**",
        "ejs/**",
        "src/**"
    ],
    env: {
        "NODE_ENV": "development",
        "PORT": 80
    }
};

/**
 *  nodemon 重啟 app
 */
gulp.task('app_dev', function() {
    nodemon(nodemonConfig)
        .on('restart', function() {
            console.log('web server restarted!');
        })
})

gulp.task('default', ['app_dev', 'scripts', 'vendors', 'ejs', 'less', 'watch']);

gulp.task('build', ['clear', 'scripts', 'vendors', 'ejs', 'less']);

```
