建置配合wio執行可用的環境
===

# 依下列的步驟執行(配合VS Code)

## 初步建置
### 首先新增目錄, 並初始npm
``` bash
mkdir react-hello-world 
cd react-hello-world 
npm init
```

## 由Git clone下來需修改的部份
### clone時請修改成將要命名的專案(目錄)
修改package.json
```
{
  "name": "helloworld", //修改成新的專案名稱
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Vincent",
  ...
  ...
  }
}
```
再重新執行下列指令
``` bash
npm init
npm install
```

### 安裝webpack
```
npm i webpack -S
```

### 新增 webpack.config.js

貼入下列代碼
``` javascript
var webpack = require('webpack'); 
var path = require('path'); 
var BUILD_DIR = path.resolve(__dirname, './public'); 
var APP_DIR = path.resolve(__dirname, './app'); 
var config = { 
		entry: APP_DIR + '/index.jsx', 
		output: { path: BUILD_DIR, filename: 'bundle.js' 
	} 
}; 
module.exports = config;
```

### 在專案目錄下新增兩個子目錄
一個為 public, 一個為 app

在專案目錄下, 新增檔案 index.html

``` html
<html>
  <head>
    <meta charset="utf-8">
    <title>React.js using NPM, Babel6 and Webpack</title>
  </head>
  <body>
    <div id="app" />
    <script src="public/bundle.js" type="text/javascript"></script>
  </body>
</html>
```

### 接著安裝相關的 node.js 模組
```
npm i babel-loader babel-preset-es2015 babel-preset-react babel-core -S
``` 

### 接著在 webpack.config.js 加入下列內容
```
// Existing Code ....
var config = {
  // Existing Code ....
  module : {
    loaders : [
      {
        test : /\.jsx?/,
        include : APP_DIR,
        loader : 'babel',
        query:
          {
            presets:['es2015','react']
          }
      }
    ]
  }
}
```

### 再安裝react相關模組
```
npm i react react-dom -S
```

### 在app子目錄下,新增一個index.jsx的檔案
``` jsx
import React from 'react';
import {render} from 'react-dom';

class App extends React.Component {
  render () {
    return <p> Hello React!</p>;
  }
}

render(<App/>, document.getElementById('app'));
```

### 執行 webpack 打包
```
webpack -d
```

### 試著新增另一個 react元件
在 app子目錄下, 新增一個檔案 AwesomeComponent.jsx
``` 
import React from 'react';

class AwesomeComponent extends React.Component {

  constructor(props) {
    super(props);
    this.state = {likesCount : 0};
    this.onLike = this.onLike.bind(this);
  }

  onLike () {
    let newLikesCount = this.state.likesCount + 1;
    this.setState({likesCount: newLikesCount});
  }

  render() {
    return (
      <div>
        Likes : <span>{this.state.likesCount}</span>
        <div><button onClick={this.onLike}>Like Me</button></div>
      </div>
    );
  }

}

export default AwesomeComponent;
```

### 接著修改 index.jsx 內容
```
// ...
import AwesomeComponent from './AwesomeComponent.jsx';
// ...
class App extends React.Component {
  render () {
    return (
      <div>
        <p> Hello React!</p>
        <AwesomeComponent />
      </div>
    );
  }
}

// ...
```

### 重新執行打包,並refresh index.html

