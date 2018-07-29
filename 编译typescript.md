# 1. 安装Loader
```text
npm i typescript ts-loader -D
```

# 2. 配置webpack.config.js

```typescript
let path = require('path');

module.exports = {
  mode: 'none',
  entry: {
    'app': path.resolve(__dirname, './src/app.ts')
  },
  output: {
    filename: 'index.js'
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: ['ts-loader']
      }
    ]
  }
}
```

# 3. 配置tsconfig.json

在 **根目录** 上新建tsconfig.json文件，然后如下配置：
```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es5",
    "allowJs": true
  },
  "include": [
    "./src/*"
  ],
  "exclude": [
    "./node_modules/*"
  ]
}
```