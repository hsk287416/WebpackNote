# 1. 引入CSS

```text
npm i style-loader css-loader -D
```

## 1.1 style-loader与css-loader的区别

style-loader用于创建标签，也就是说它负责将css样式绑定到HTML标签上。而css-loader是负责将css样式import到JavaScript文件中。

# 1.2 style-loader
```javascript
let path = require ('path');

module.exports = {
  mode: 'none',
  entry: {/*省略*/},
  output: {/*省略*/},
  resolve: {
    extensions: ['.ts', '.css', '.js'],
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        exclude: path.resolve(__dirname, './node_modules'),
        use: ['style-loader', 'css-loader']
      }
    ],
  }
};
```
这样一来，会直接将css样式嵌入HTML页面中。

# 1.3 style-loader/url（不常用）
如果不想在HTML中直接嵌入css样式，而是想采用link方式导入外部css的话，可以这样来做：
```javascript
let path = require ('path');

module.exports = {
  mode: 'none',
  entry: {/*省略*/},
  output: {
    /*省略*/
    publicPath: './dist/' // 必须
  },
  resolve: {
    extensions: ['.ts', '.css', '.js'],
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        exclude: path.resolve(__dirname, './node_modules'),
        use: ['style-loader/url', 'file-loader']
      }
    ],
  }
};
```

# 1.4 通过transform，加载不同的css样式

```javascript
let path = require ('path');

module.exports = {
  mode: 'none',
  entry: {/*省略*/},
  output: {/*省略*/},
  resolve: {
    extensions: ['.ts', '.css', '.js'],
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        exclude: path.resolve(__dirname, './node_modules'),
        use: [
          {
            loader: 'style-loader',
            options: {
              transform: path.resolve(__dirname, './css.transform.js')
            }
          },
          {
            loader: 'css-loader'
          }
        ]
      }
    ],
  }
};
```
css.transform.js:
```javascript
module.exports = function(css) {
  // 根据不同的页面宽度，显示不同的背景色
  if (window.innerWidth > 700) {
    return css.replace('#0094ff', '#284456');
  } else {
    return css.replace('#0094ff', '#cccccc');
  }
}
```

# 2. scss-loader

```text
npm i -D sass node-sass sass-loader
```

配置如下：
```javascript
let path = require ('path');

module.exports = {
  mode: 'none',
  entry: {/*省略*/},
  output: {/*省略*/},
  resolve: {/*省略*/},
  module: {
    rules: [
      {
        test: /\.scss$/,
        exclude: path.resolve(__dirname, './node_modules'),
        use: [
          {
            loader: 'style-loader'
          },
          {
            loader: 'css-loader',
            options: {
              minimize: true
            }
          },
          {
            loader: 'sass-loader' // 放在最后
          }
        ]
      }
    ],
  }
};
```

# 3. PostCSS

```text
npm i -D postcss
npm i -D postcss-loader
npm i -D autoprefixer
npm i -D postcss-cssnano
npm i -D postcss-cssnext
```

## 3.1 autoprefixer

它可以为我们写的css代码加上前缀，使之适应于其他浏览器，例如：

它可以将以下代码：
```css
a {
  display: flex;
}
```

改变成这样：
```css
a {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
}
```

## 3.2 cssnano

它可以帮助我们优化和压缩css，css-loader在内部就使用了cssnano。

## 3.3 cssnext

cssnext可以是程序支持下一个css3的版本中的语法。由于cssnext中已经使用了autoprefixer，所以如果使用cssnext的话，就不需要引入autoprefixer了。