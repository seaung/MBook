##### React项目中使用craco配置路径别名

1. 安装craco

```bash
yarn add craco -D
```

2. 在项目根目录下创建craco.config.js文件，并键入如下配置项

```js
const path = require('path')

module.exports = {
    webpack: {
        alias: {
            '@': path.resolve(__dirname, 'src')
        }
    }
}
```

3. 修改package.json配置项

```json
"scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject"
  },
```

##### React项目中配置vscode提示

1. 在项目根目录下创建jsconfig.json

```json
{
    "compilerOptions": {
        "baseUrl": "./",
        "paths": {
            "@/*": ["src/*"]
        }
    }
}
```



---

that's all