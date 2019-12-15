##자바스크립트 es6 개발환경 구축
> 사용법
~~~
git clone https://github.com/kdeveloper87/es6-dev-environment.git folderName
~~~
~~~
npm install
~~~
~~~
npm run dev
~~~


> 설치모듈

    "@babel/cli": "^7.7.5",
    "@babel/core": "^7.7.5",
    "@babel/polyfill": "^7.7.0",
    "@babel/preset-env": "^7.7.6",
    "babel-loader": "^8.0.6",
    "webpack": "^4.41.2",
    "webpack-cli": "^3.3.10",
    "webpack-dev-server": "^3.9.0"

목표

ES6 코드 작성→babel, webpack→ 한개 js

> babel

최신 js 코드→es5코드 변환

> Node.js 설치

> 패키지 초기화

    npm i init

> babel 설치

    npm i -dev @babel/core @babel/preset-env @babel/core @babel/cli

> .babelrc 설정

    {
      "presets": ["@babel/preset-env"]
    }

> 빌드 스크립트 작성

    {
      "name": "es6-config",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
        "build": "babel src/js -w -d dist/js"
      },
      "author": "",
      "license": "MIT",
      "dependencies": {
        "@babel/core": "^7.7.5",
        "@babel/preset-env": "^7.7.6",
        "babel-loader": "^8.0.6"
      }
    }

src/js 모든 파일을 트랜스파일링 후 dist/js에 저장

-w : 타겟폴더 파일을 감지하여 트랜스파일

-d : 저장폴더 지정

> build 테스트

    npm run build

dist/js 파일에 es5로 변환된 코드들이 저장된다.

 export, import 구문은 require로 변환되는데, 브라우저에서 지원하지 않는다.

모듈기능은 node 환경에서 동작하기 때문에 webpack 설정이 필요하다.

> Webpack

모듈들을 하나의 js파일로 번들링 하는 모듈번들러이다.

의존 모듈이 하나의 파일로 번들링 된다.

하나의 파일로 번들링 되므로 html script 태그에서 하나의 js파일만 로드하면 된다.

> webpack 설치

    npm i -dev webpack webpack-cli

> babel-loader 설치

webpack이 모듈을 번들링할때 babel을 사용하여 es6코드를 es5코드로

트랜스 파일링 하도록 설치한다.

    npm i -dev babel-loader

> webpack.config 설정

웹팩 실행시 참조하는 설정파일

    const path = require('path');
    
    module.exports = {
      entry: './src/js/index.js',
      output: {
        path: path.resolve(__dirname, 'dist/js'),
        filename: 'bundle.js'
      },
      module: {
        rules: [
          {
            test: /\.js$/,
            include: [
              path.resolve(__dirname, 'src/js')
            ],
            exclude: /node_modules/,
            use: {
              loader: 'babel-loader',
              options: {
                presets: ['@babel/preset-env'],
              }
            }
          }
        ]
      },
      devtool: 'source-map',
      // https://webpack.js.org/concepts/mode/#mode-development
      mode: 'development'
    };

> 빌드 스크립트 변경

빌드 스크립트를 babel에서 webpack으로 변경

    "scripts": {
        "build": "webpack -w"
      }

npm run build 시 dist 폴더로 bundle.js 파일이 생성된다.

> html에서 script 변경

    <script src="dist/js/bundle.js"></script>

> babel-polyfill

es6→es5변환시 es5표현할수 없는 es6코드가 있다.

promise 등.. 따라서 polyfill이 그런것들을 해결해준다.

> 설치

    npm i @babel/polyfill

*개발환경뿐만 아니라 실제서비스에서도 쓰이기 때문에 -dev 옵션은 제외

webpack 설정

    module.exports = {
      entry: ['@babel/polyfill', './src/js/index.js'],
    ...
    }

빌드 해본다.

npm run build

빌드된파일 실행시 크롬,ie등에서 polyfill을 통해 잘 동작하는것을 확인 할 수 있다.

> webpack-dev-server

소스코드 작성 후 빌드를 수동으로 해야하는데

자동으로 코드 변경을 감지해 반영해준다

    npm i webpack-dev-server

스크립트 코드 수정

    "dev": "webpack-dev-server --open --progress"

스크립트 실행

npm run dev 

이제 소스코드 변경시 자동으로 반영되고 reload 된다.

> package.json

    {
      "name": "es6-config",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
        "build": "webpack -w",
        "dev": "webpack-dev-server --open --progress"
      },
      "author": "",
      "license": "MIT",
      "dependencies": {
        "@babel/cli": "^7.7.5",
        "@babel/core": "^7.7.5",
        "@babel/polyfill": "^7.7.0",
        "@babel/preset-env": "^7.7.6",
        "babel-loader": "^8.0.6",
        "webpack": "^4.41.2",
        "webpack-cli": "^3.3.10",
        "webpack-dev-server": "^3.9.0"
      }
    }

> webpack.config.js
~~~
   const path = require('path');
   
   module.exports = {
     entry: ['@babel/polyfill', './src/js/index.js'],
     output: {
       path: path.resolve(__dirname, 'dist/js'),
       publicPath: '/dist/js',
       filename: 'bundle.js'
     },
     module: {
       rules: [
         {
           test: /\.js$/,
           include: [
             path.resolve(__dirname, 'src/js')
           ],
           exclude: /node_modules/,
           use: {
             loader: 'babel-loader',
             options: {
               presets: ['@babel/preset-env'],
             }
           }
         }
       ]
     },
     devtool: 'source-map',
     // https://webpack.js.org/concepts/mode/#mode-development
     mode: 'development'
   };
~~~
