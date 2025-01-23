# webpack.config

---

<b style="color:blue">webpack을 사용하기 위한 설정 파일</b>

```js
const path = require("path"); // core nodejs 모듈 중 하나, 파일 경로 설정할 때 사용
const HtmlWebpackPlugin = require("html-webpack-plugin"); // index.html 파일을 dist 폴더에 index_bundle.js 파일과 함께 자동으로 생성, 우리는 그냥 시작만 하고싶지 귀찮게 index.html 파일까지 만들고 싶지 않다.!!

module.exports = {
  // moduel export (옛날 방식..)
  entry: "./src/index.js", // 리액트 파일이 시작하는 곳
  output: {
    // bundled compiled 파일
    path: path.join(__dirname, "/dist"), //__dirname : 현재 디렉토리, dist 폴더에 모든 컴파일된 하나의 번들파일을 넣을 예정
    filename: "index_bundle.js",
  },
  module: {
    // javascript 모듈을 생성할 규칙을 지정 (node_module을 제외한.js 파일을 babel-loader로 불러와 모듈을 생성
    rules: [
      {
        test: /\.js$/, // .js, .jsx로 끝나는 babel이 컴파일하게 할 모든 파일
        exclude: /node_module/, // node module 폴더는 babel 컴파일에서 제외
        use: {
          loader: "babel-loader", // babel loader가 파이프를 통해 js 코드를 불러옴
        },
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html", // 생성한 템플릿 파일
    }),
  ],
};
```

## path

- 파일의 경로를 지정
- \_\_dirname : 노드 변수로 현재 모듈의 디렉토리를 리턴합니다.

## HtmlWebpackPlugin

- 컴파일 이후 index.html 파일을 생성
- template에 지정된 index.html에 모든 static 파일들을 긁어모은 index_bundle.js 파일을 `<script src='index_bundle.js'></script>` 형식으로 연결해줍니다.

## module.export

- 출력할 모듈

## entry

- 컴파일 할 파일
- index.js

## output

- 컴파일 이후 파일
- \_\_dirname/dist/index_bundle.js

## module

- 모듈의 컴파일 형식
- es6 문법을 es5으로 바꾸기 위해 webpack이 js, jsx를 포함한 모든 파일을 babel을 통하여 컴파일 시킵니다.

## plugin

- 사용할 plugins
- 여기서는 htmlwebpackPlugin을 사용하여 index.html과 index_bundle.js에 연결해줍니다.

  ### html-webpack-plugin

  - html-webpack-plugin은 script 태그안에 넣은 webpack 번들 파일과 함께 html5 파일을 생성합니다.
  - HTMLWebpackPlugin이 index.html의 script 태그안에 컴파일된 bundle 파일을 심어줄 것입니다.
  - new HtmlWebpackPlugin()을 위의 예시처럼 그냥 사용하는 것도 좋지만 이렇게 템플릿을 만들어 놓으면 커스터마이징 하기 편합니다.

# babelrc

---

- babel-preset-env와 babel-preset-react와 같이 preset을 사용하고 싶으면 root폴더에 .babelrc을 생성하여 사용하고자할 preset을 설정하면 됩니다.
- plugin들을 각각의 npm dependency를 가지고 있습니다. 하지만 설치시 매번 .bablrc에 설정을 해야하므로 그 두가지를 모두 해결해줄 preset을 사용하면됩니다. preset을 설치하고 설정하므로서 preset이 가진 plugin들을 설정할 필요없이 사용할 수 있게 됩니다.

# package.json

---

```json
"scripts": {
    "start":"webpack-dev-server --mode development --open --hot",                    // webpack-dev-server, --open : 자동으로 브라우저 열어줌, --hot : hot realod 저장했을 때 자동적으로 reload 해줌
    "build":"webpack --mode production"                                              // dist 폴더에 컴파일된 파일 다 넣어줌
  },
```
