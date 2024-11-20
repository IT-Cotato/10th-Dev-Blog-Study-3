리액트 프로젝트를 처음 시작하면 거의 `npx create-react-app`을 통해서 프로젝트를 만들었다. 이렇게 프로젝트를 세팅하면 명령어 한 줄로만 리액트 프로젝트를 세팅 가능하기에 처음 리액트를 배울때 부터 이 방법을 사용했지만, 이렇게 프로젝트를 세팅하다보니 **실제 리액트가 어떻게 동작하는지 전혀 이해를 하지 못하고 있었다.** babel, webpack을 리액트 하면서 분명 보기는 했지만 이게 전혀 어떤 기능을 하는지 모르고 사용해왔다.

작년에 'Slack 클론 코딩'강의를 들으면서 1장에 있던 내용이 CRA 없이 리액트 프로젝트를 세팅하는거였다. 당시에는 리액트를 시작한지 얼마 되지 않아 크게 감흥을 느끼지 못했지만, 지금 시점에서 다시 보니 프로젝트가 동작하는 방식을 이해하는데 도움이 될 수 있을거 같아 정리를 해보겠다!!

참고로 강의는 타입스크립트를 포함해서 세팅하고, 강의가 21년에 제작되었다 보니 버전이 현재와는 다른점이 있다.

## CRA (Creat React App)

CRA는 리액트 프로젝트 초기 환경을 직접 설정하지 않고 프로젝트를 시작할 수 있게 하는 CLI 도구이다. 이를 통해서 쉽게 리액트 프로젝트를 설정할 수 있다.

여담으로 리액트 공식문서에서는 프로젝트를 처음 세팅할때 **리액트를 사용하는것을 권장하고 있지 않다.** 리액트는 프레임워크가 아닌 UI 구축을 위한 라이브러리이다. 따라서 리액트를 이용해서 프론트엔드 개발을 진행한다고 해도 필수적인 라이브러리 설치들이 필요하다. (라우팅을 위한 react-router-dom 라이브러리 추가 설치) 이렇듯 리액트 공식문서에서는 결국 리액트로 프로젝트를 시작해도 필수적으로 라이브러리들을 계속 설치하다 보니 차라리 리액트를 기반으로 하는 프레임워크를 설치하라고 한다. 대표적으로 Next.js는 리액트를 기반으로 페이지 라우팅 기능과 SSR을 제공하는 프레임워크이다. 리액트로 프로젝트를 시작해서 라우팅, SSR 라이브러리들을 추가적으로 설치하는 대신에 처음부터 기능이 포함되어 있는 프레임워크를 설치하라고 권장하고 있다.

하지만 Next.js 강의도 들어봤지만...... 아직 리액트 자체에서도 부족함이 많다고 생각하고 있기 때문에 나는 프레임워크 없이 리액트만 이용해서 프로젝트를 세팅해 보겠다.

## 프론트엔드 세팅하기

### npm init

`npm init`명령어를 통해서 package.json 파일을 생성

```json
{
  "name": "react-without-cra",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "WONYOUNG-HC",
  "license": "ISC",
  "description": ""
}
```

유의사항 : name을 설정할 때 npm에 있는 패키지와 겹치는 이름인 경우 오류가 발생 가능 (설치하려는 패키지와 이름이 중복되는 경우)

### react, react-dom 설치

`npm install react react-dom`명령어를 통해 react와 react-dom 라이브러리 설치  
react-dom 라이브러리는 리액트 라이브러리가 DOM과 상호작용을 하기 위한 라이브러리이다.

```ts
ReactDOM.render(<App />, document.getElementById('root'));
```

리액트에서 index.js를 보면 다음과 유사하게 App 컴포넌트를 렌더링 한다. 리액트로 작성된 App 컴포넌트를 실제 HTML DOM에 렌더링이 가능하게 해주는 기능을 react-dom 라이브러리에서 제공한다.

### 타입스크립트 설치

`npm install typescript`명령어를 통해 타입스크립트 설치  
`npm install @types/react @types/react-dom`명령어를 통해  
타입스크립트를 사용하는 경우 라이브러리 설치시 @types/~을 통해 라이브러리의 타입들을 같이 설치해주어야 한다.

현재까지 package.json

```json
{
  "name": "react-without-cra",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "WONYOUNG-HC",
  "license": "ISC",
  "description": "",
  "dependencies": {
    "@types/react": "^18.3.12",
    "@types/react-dom": "^18.3.1",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "typescript": "^5.6.3"
  }
}
```

#### package.json과 pacak-lock.json은?

package.json은 프로젝트의 메타데이터와 의존성 정보를 담고있다. package.json에서 담당하고 있는 의존성 관리는 설치된 라이브러리의 버전을 명시한다. 라이브러리를 설치하면 node_modules에 설치가 되지만, 협업을 하면서 코드를 공유할 때는 이 node_modules 폴더는 공유하지 않는다. 설치된 라이브러리들이 모두 모여있는 폴더는 용량이 크기때문에 라이브러리 파일 전체를 공유해는 대신에 라이브러리의 버전을 명시한 package.json 파일만 공유해 각자 설치하는 방법을 통해 팀원들 모두가 동이한 라이브러리를 사용할 수 있게 한다.  
package-lock.json은 라이브러리들의 의존성 트리 대한 정보를 담고있다. A 라이브러리는 모든 기능이 직접 구현되어 있는게 아니라 K 라이브러리에 의존해있다 가정하자. 그럼 A 라이브러리를 설치하면 K 라이브러리가 같이 설치될것이다. 이때 A 라이브러리가 의존하고 있는 K 라이브러리의 버전이 있을것이다. 그 버전을 명시하고 있는 파일이 package-lock.json 파일이다.

### ESLint, Prettier 설치

`npm install -D eslint`명령어를 통해 ESLnit 설치  
`npm install -D prettier eslit-plugin-prettier eslint-config-prettier`명령어를 통해 prettier와 관련 라이브러리 설치  
**ESLint**는 자바스크립트 코드의 문법과 스타일을 검사해 틀린 부분이 있으면 에러를 발생시키는 **정적 코드 분석 도구**이다.  
**Prettier**는 코드의 스타일로 **자동 포맷팅해주는 도구**이다. 공백, 세미코론 여부, 작은 따옴표 사용 여부등 다양한 스타일 포맷을 통일시켜주는 도구이다.  
eslint-plugin-prettier와 eslint-config-prettier는 ESLint와 Prettier를 함께 사용할 때 발생할 수 있는 충돌을 방지해주는 라이브러리이다. eslint-plugin-prettier는 Prettier를 ESLint 규칙으로 추가하여 Prettier와 맞지 않게 코드를 작성한 경우 정적 코드 분석으로 에러를 발생시키고, eslint-config-prettier는 Prettier와 충돌하는 ESLint 규칙을 비활성화하여 두 도구가 충돌 없이 함께 작동하도록 설정한다.

.eslintrc

```json
{
  "extends": ["plugin:prettier/recommended"]
}
```

간단하게 세팅한 방법으로, prettier에서 제안한 방법을 따르겠다고 설정.

.prettierrc

```json
{
  "printWidth": 120,
  "tabWidth": 2,
  "singleQuote": true,
  "trailingComma": "all",
  "semi": true
}
```

코드 스타일을 규정

### 타입스크립트 설정

.tsconfig.json

```json
{
  "compilerOptions": {
    "esModuleInterop": true,
    "sourceMap": true,
    "lib": ["ES2020", "DOM"],
    "jsx": "react",
    "module": "esnext",
    "moduleResolution": "Node",
    "target": "es5",
    "strict": true,
    "resolveJsonModule": true,
    "baseUrl": ".",
    "paths": {
      "@hooks/*": ["hooks/*"],
      "@components/*": ["components/*"],
      "@layouts/*": ["layouts/*"],
      "@pages/*": ["pages/*"],
      "@utils/*": ["utils/*"],
      "@typings/*": ["typings/*"]
    }
  },
  "ts-node": {
    "compilerOptions": {
      "module": "commonjs",
      "moduleResolution": "Node",
      "target": "es5",
      "esModuleInterop": true
    }
  }
}
```

타입스크립트가 자바스크립트로 컴파일되는 과정을 설정.

`esModuleInterop: true` -> CommonJS 모듈과 ES6 모듈 간의 호환성을 위한 설정 (import 구문 사용 가능)  
`sourceMap: true` -> 컴파일된 자바스크립트 코드에 대한 소스 맵 파일 생성 (타입스크립트 코드와 컴파일된 자바스크립트 코드가 연결되어 디버깅 용이)  
`lib: ["ES2020", "DOM"]` -> 컴파일 시 사용할 기본 라이브러리의 버전과 환경을 지정 (ES2020은 최신 ECMAScript 기능을 사용, DOM은 브라우저에서 사용할 수 있는 DOM API를 사용)  
`jsx: react` -> jsx 문법을 사용 가능하도록 설정  
`module: esnext` -> 최신 자바스크립트 모듈 시스템(ESM)을 사용하도록 설정  
`target: es5` -> 타입스크립트가 컴파일된 자바스크립트 코드의 버전을 ES5로 설정  
`strict: true` -> 엄격한 타입 검사를 활성화 (any 남용을 방지)  
`resolveJsonModule: true` -> 타입스크립트에서 JSON 파일을 import 가능하게 함  
`baseUrl: "."` -> 모듈 해석 시 기준이 되는 경로를 현재 폴더로 설정  
`pahts` -> 경로의 alias 부여 (import시 @hooks로 경로를 사용하면 baseUrl/hooks로 인식)  
`ts-node` -> 타입스크립트를 직접 Node.js에서 실행하때 적용

## babel과 webpack 설정하기

**Webpac**k은 자바스크립트 모듈 번들러로, 프로젝트에 필요한 여러 파일을 하나의 파일 또는 여러 파일로 번들링하여 브라우저가 효율적으로 로드할 수 있게 해주는 기능을 제공한다. 프로젝트를 진행하면서 단위별로 파일들을 나누어 개발을 하지만, 빌드시 분리된 파일들을 하나로 모으는 작업을 해주어 브라우저에서 해석하는 속도를 빠르게 해준다.

**Babel**은 자바스크립트 컴파일러로, 최신 자바스크립트 코드를 구 버전의 자바스크립트로 변환을 해주어 구형 브라우저에서도 동작이 가능하게 한다.  
컴파일러로, 최신 JavaScript 코드를 오래된 환경에서도 실행될 수 있도록 구 버전 JavaScript로 변환해줍니다.

tsx 파일을 타입스크립트가 변환을 해주고, 그거를 webpack이 받아서 babel로 처리해서 자바스크립트 파일을 만든다.

---

`npm install -D webpack @types/webpack @types/node` 명령어를 통해 webpack과 관련된 라이브러리 설치.  
`npm install -D css-loader style-loader @babel/core babel-loader @babel/preset-env @babel/preset-react @babel/preset-typescript`명령어를 통해 babel과 css loader를 설치

webpack.config.ts

```ts
import path from 'path';
// import ReactRefreshWebpackPlugin from '@pmmmwh/react-refresh-webpack-plugin';
import webpack, { Configuration as WebpackConfiguration } from 'webpack';
// import { Configuration as WebpackDevServerConfiguration } from 'webpack-dev-server';

// interface Configuration extends WebpackConfiguration {
//   devServer?: WebpackDevServerConfiguration;
// }

// import ForkTsCheckerWebpackPlugin from 'fork-ts-checker-webpack-plugin';

const isDevelopment = process.env.NODE_ENV !== 'production';

// const config: Configuration = {
const config: WebpackConfiguration = {
  name: 'react-without-cra',
  mode: isDevelopment ? 'development' : 'production',
  devtool: !isDevelopment ? 'hidden-source-map' : 'eval',
  resolve: {
    extensions: ['.js', '.jsx', '.ts', '.tsx', '.json'],
    alias: {
      '@hooks': path.resolve(__dirname, 'hooks'),
      '@components': path.resolve(__dirname, 'components'),
      '@layouts': path.resolve(__dirname, 'layouts'),
      '@pages': path.resolve(__dirname, 'pages'),
      '@utils': path.resolve(__dirname, 'utils'),
      '@typings': path.resolve(__dirname, 'typings'),
    },
  },
  entry: {
    app: './src/index.tsx',
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        loader: 'babel-loader',
        options: {
          presets: [
            [
              '@babel/preset-env',
              {
                targets: { browsers: ['IE 10'] },
                debug: isDevelopment,
              },
            ],
            '@babel/preset-react',
            '@babel/preset-typescript',
          ],
          //   env: {
          //     development: {
          //       plugins: [require.resolve('react-refresh/babel')],
          //     },
          //   },
        },
        exclude: path.join(__dirname, 'node_modules'),
      },
      {
        test: /\.css?$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
  // plugins: [
  //   new ForkTsCheckerWebpackPlugin({
  //     async: false,
  //     // eslint: {
  //     //   files: "./src/**/*",
  //     // },
  //   }),
  //   new webpack.EnvironmentPlugin({ NODE_ENV: isDevelopment ? 'development' : 'production' }),
  // ],
  output: {
    path: path.join(__dirname, 'dist'),
    filename: '[name].js',
    publicPath: '/dist/',
  },
  // devServer: {
  //   historyApiFallback: true, // react router
  //   port: 3090,
  //   devMiddleware: { publicPath: '/dist/' },
  //   static: { directory: path.resolve(__dirname) },
  // },
};

if (isDevelopment && config.plugins) {
  config.plugins.push(new webpack.HotModuleReplacementPlugin());
  // config.plugins.push(new ReactRefreshWebpackPlugin());
}
if (!isDevelopment && config.plugins) {
}

export default config;
```

간단한 실행을 위해 아직 설치하지 않은 라이블리는 주석으로 실행에서 제외시켜 놓았다.

### Configration 인터페이스

```ts
interface Configuration extends WebpackConfiguration {
  devServer?: WebpackDevServerConfiguration;
}
```

기본 `WebpackConfiguration` 인터페이스를 확장하여 `devServer` 속성을 추가할 수 있도록 함.

### isDevelopment

```ts
const isDevelopment = process.env.NODE_ENV !== 'production';
```

현재 환경이 `production`이 아닌 경우 `development` 모드로 간주하여, 개발과 배포 설정을 분리하는 데 사용.

### name

```ts
name: 'react-without-cra',
```

프로젝트의 이름을 나타내며, 웹팩에서 해당 설정을 참조할 때 사용.

### mode

```ts
mode: isDevelopment ? 'development' : 'production',
```

빌드 모드를 설정.

- `development`: 디버깅이 용이하도록 빠르게 빌드하고 최적화가 덜된 설정을 적용.
- `production`: 배포를 위해 최적화된 설정을 적용.

### devtool

```ts
devtool: !isDevelopment ? 'hidden-source-map' : 'eval',
```

소스 맵 생성 방식을 지정.

- `hidden-source-map`: `production` 모드에서 소스 맵을 숨기면서 오류 추적에 사용된다.
- `eval`: `development` 모드에서 빠르게 소스 맵을 생성하여 디버깅에 사용된다.

### resolve

```ts
resolve: {
  extensions: ['.js', '.jsx', '.ts', '.tsx', '.json'],
    alias: { ... },
},
```

`extensions`: 확장자를 생략하고 `import`할 수 있도록 허용. `.js`, `.jsx`, `.ts`, `.tsx`, `.json` 파일을 확장자 없이 사용할 수 있도록 함.  
`alias`: .tsconfig.json에서 정의한 경로 별칭을 사용

### entry

```ts
entry: {
  app: './src/index.tsx',
},
```

애플리케이션의 진입점 파일을 지정. `app`이라는 이름으로 `./src/index.tsx` 파일을 설정.

### module

```ts
module: {
  rules: [ ... ],
},
```

웹팩이 각 파일 유형을 처리하는 방법을 지정하는 규칙들을 포함.

#### 타입스크립트 및 자바스크크리트 파일 처리

```ts
{
  test: /\.tsx?$/,
  loader: 'babel-loader',
  options: { ... },
  exclude: path.join(__dirname, 'node_modules'),
},
```

- `test`: `.ts`와 `.tsx` 확장자를 가진 파일에 적용.
- `loader`: `babel-loader`를 사용하여 최신 자바스크립트 및 타입스크립트 코드를 트랜스파일링.
- `options`:
- \-`presets`: 바벨 프리셋을 설정.
- \--`@babel/preset-env`: 최신 자바스크립트 기능을 구 버전의 브라우저에서 호환되도록 트랜스파일링.
- \--`@babel/preset-react`: JSX 문법을 트랜스파일링.
- \--`@babel/preset-typescript`: 타입스크립트 코드를 자바스크립트로 변환.
- \-`exclude`: `node_modules` 폴더를 제외하여 외부 라이브러리는 트랜스파일링하지 않도록 함.

#### CSS 파일 처리

```ts
{
  test: /\.css?$/,
  use: ['style-loader', 'css-loader'],
},
```

- `test`: `.css` 파일을 대상으로 함.
- `use`: `style-loader`와 `css-loader`를 사용하여 CSS 파일을 번들링.
- `css-loader`: CSS 파일을 읽고 자바스크립트 모듈로 변환.
- `style-loader`: 변환된 CSS를 `<style>` 태그로 HTML에 삽입.

### plugins

```ts
plugins: [ ... ],
```

빌드 과정에서 사용할 플러그인을 지정.

#### ForkTsCheckerWebpackPlugin

```ts
new ForkTsCheckerWebpackPlugin({
  async: false,
}),
```

타입스크립트의 타입 검사를 별도로 수행하여 빌드 속도를 높이고, 타입 오류 발생 시 빌드를 중단하지 않도록 한다.  
`async: false`: 비동기 방식으로 검사를 수행하지 않아, 즉시 오류를 확인할 수 있다.

#### webpack.EnvironmentPlugin

```ts
new webpack.EnvironmentPlugin({ NODE_ENV: isDevelopment ? 'development' : 'production' }),
```

`NODE_ENV` 환경 변수를 설정하여 현재 빌드 환경(`development` 또는 `production`)에 따라 애플리케이션이 동작하도록 한다.

### output

```ts
output: {
  path: path.join(__dirname, 'dist'),
  filename: '[name].js',
  publicPath: '/dist/',
},
```

빌드된 파일의 출력을 설정.  
\-`path`: 빌드된 파일이 저장될 디렉토리로 `dist` 폴더에 저장.  
\-`filename`: 빌드된 파일의 이름 형식을 지정합. `[name].js`는 `entry`에서 설정한 이름에 따라 파일 이름을 지정.  
\-`publicPath`: webpack DevServer가 제공하는 파일의 기본 경로를 지정하여 `/dist/` 폴더의 파일을 참조하도록 설정.

### devServer

```ts
devServer: {
  historyApiFallback: true,
  port: 3090,
  devMiddleware: { publicPath: '/dist/' },
  static: { directory: path.resolve(__dirname) },
},
```

개발 서버 설정으로, 로컬에서 애플리케이션을 테스트할 수 있게 함.  
`historyApiFallback`: SPA에서 클라이언트 측 라우팅을 지원하여 경로에 대응하는 파일이 없더라도 `index.html`을 반환하게 한다. (react-router-dom 사용을 위해)
`port`: 개발 서버가 열리는 포트를 지정.  
`devMiddleware.publicPath`: DevServer가 `/dist/` 경로에 빌드 파일을 제공하도록 설정.  
`static.directory`: 정적 파일을 제공할 디렉토리를 지정. `path.resolve(__dirname)`로 현재 디렉토리를 기준으로 한다.

### 개발 모드 플러그인 추가

```ts
if (isDevelopment && config.plugins) {
  config.plugins.push(new webpack.HotModuleReplacementPlugin());
  config.plugins.push(new ReactRefreshWebpackPlugin());
}
```

개발 모드일 때 `Hot Module Replacement` `React Refresh` 플러그인을 추가.  
`HotModuleReplacementPlugin`: 변경된 모듈만 새로 로드하여 전체 애플리케이션을 새로고침하지 않고도 실시간으로 코드 업데이트가 가능하게 함.  
`ReactRefreshWebpackPlugin`: React 컴포넌트의 상태를 유지한 채로 빠르게 업데이트할 수 있게 함.

### index.html

```html
<html>
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0"
    />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>REACT-WITHOUT-CRA</title>
    <style></style>
  </head>
  <body>
    <div id="app"></div>
    <script src="./dist/app.js"></script>
  </body>
</html>
```

브라우저가 처음으로 받는 html 파일. `<script src="/dist/app.js"></script>`코드가 실행되면 리액트로 작성된 코드드이 불러와져 위의 `div`태그에서 렌더링된다.

### index.tsx와 app.tsx

/src/index.tsx

```ts
import React from 'react';
import { createRoot } from 'react-dom/client';

import App from './App';

const app = createRoot(document.getElementById('app') as HTMLElement);
app.render(<App />);
```

App 컴포넌트를 불러와 아이디가 app인 태그에 렌더링.

/src/app.tsx

```ts
import React from 'react';

const App = () => {
  return <div>CRA없이 만드는 리액트</div>;
};

export default App;
```

## webpack 실행해보기

위의 과정에서 설치가 되지 않은 라이브러리들을 마저 설치.
`npm install -D ts-node react-refreah`

이후 package.json의 `script`에 빌드 명령을 추가하여 `npm run build`를 수행.

```json
"scripts": {
"build": "NODE_ENV=production webpack",
},
```

<br>
빌드 결과로 아래와 같이 dist 폴더에 app.js파일이 생성된다!!

![alt text](<스크린샷 2024-11-06 오후 5.50.39.png>)

그리고 이제 index.html 파일을 실행해보면

![alt text](<스크린샷 2024-11-06 오후 6.19.24.png>)
App.tsx에서 작성한 문구가 출력이 된다!!

## 웹팩 데브 서버 세팅하기

현재 상태에서 개발을 한다고 하면 코드를 적을때마다 저장을 하고, 저장한 파일을 build를 하는 과정으로 브라우저에서 확인을 해야한다. 하지만 CRA에서 리액트 프로젝트를 세팅하면 `npm run dev`를 통해 개발 서버를 실행시키고 파일을 저장할 때 마다 브라우저에 코드가 반영된다. 이 기능을 해주는게 **Hot Reloading**이다.

`npm install -D webpack-dev-server @types/webpack-dev-server`

`npm install -D @pmmmwh/react-refresh-webpack-plugin`

`npm install -D react-refresh`

`npm install -D fork-ts-checker-webpack-plugin`

위 명령어들로 webpack.config.ts 파일에서 사용중이지만 아직 설치하지 않은 라이브러리들을 설치해주고 주석을 제거해준다.

그리고 데브 서버 실행을 위한 명령어를 package.json의 `script`에 추가해준다.

```json
"scripts": {
    "build": "NODE_ENV=production webpack",
    "dev": "NODE_ENV=development webpack server --env development",
    "test": "echo \"Error: no test specified\" && exit 1"
},
```

그리고 이제 `npm run dev`명령어를 실행시키면

![alt text](<스크린샷 2024-11-06 오후 6.19.39.png>)

**CRA를 통해서만 사용했던 개발 서버가 이제 실행 가능해진다!!**!

## 그래서 CRA 없이 리액트 프로젝트 세팅???

**리액트 프로젝트 세팅할때는 무조건 CRA를 사용할거다!!!**

CRA 없이 리액트 프로젝트를 세팅하면서 더 크게 느낀점은 "CRA를 계속 잘 써야겠다"이다. 특히 webpack.config.ts 파일을 일일히 직접 세팅하는 작업이 상당히 어려웠고, 정리는 했지만 각 설정이 어떤 기능을 하지는 아직 정확히 파악하지는 못했다.

하지만 이전에는 리액트만 사용할 줄 알았지 webpack, babel, 타입스크립트의 변환 과정은 전혀 모르는 상태에서 이제는 어느정도 리액트로 코드를 짜면 어떻게 프로젝트가 실행되고, 이 과정에서 어던 도구들이 대충 무슨일을 하겠구나~ 정도 생각할 수 있게 되었다. 특히 리액트에서 개발하면서 코드를 저장만하면 새로고침 없이 브라우저에 수정 사항이 반영되는 원리가 웹팩 데브 서버때문에 가능한 기능인것도 처음 알게 되었다.

프로젝트를 세팅하면서 가장 어려운 부분은 webpack이였고, 이는 나중에 여유가 되면 추가로 정리를 더 해보고 싶다!
