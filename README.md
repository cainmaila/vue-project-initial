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
npm i --save-dev babel-core babel-loader babel-preset-es2015 css-loader del ejs file-loader gulp gulp-concat gulp-ejs gulp-flatten gulp-less gulp-livereload gulp-nodemon gulp-uglify less less-loader raw-loader should style-loader url-loader vue webpack webpack-stream babel-preset-stage-0 vue-loader
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
