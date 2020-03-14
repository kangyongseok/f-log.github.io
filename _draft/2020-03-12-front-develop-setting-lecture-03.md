---
title: "인프런 - 프론트엔드 개발환경의 이해와 실습-03"
categories: 
  - study
tags: 
    - lecture
    - online
    - inflearn
    - front
    - webpack
    - babel
    - lint
    - npm
    - study
    - plugin
toc: true
toc_sticky: true
comments:  true
---

## 플러그인
로더가 각 파일을 처리하는 반면 플러그인은 번들된 결과물을 처리
- 자바스크립트 난독화
- 특정 텍스트 추출
  

``` javascript
// my-webpack-plugin
class MyWebpackPlugin {
    apply(compiler) {
        compiler.plugin('emit', (compilation, callback) => {
            const source = compilation.assets['main.js'].source(); // 번들링된 main.js 소스코드 가져오는 코드
            compilation.assets['main.js'].source = () => {
                const banner = [
                    '/**',
                    ' * 이것은 BannerPlugin이 처리한 결과입니다.',
                    ' * Build Date: 2020-01-01',
                    '*/'
                ].join('\n');
                return banner + '\n\n' + source; // 원본소스코드에 주석문 추가
            }

            callback();
        })
    }
}

module.exports = MyWebpackPlugin;
```

``` javascript
// webpack.config.js
const MyWebpackPlugin = require('./my-webpack-pluigin');

module.exports = {
    ...
    plugins: [
        new MyWebpackPlugin(),
    ]
}
```

``` javascript
$ npm run build
```

``` javascript
// dist/main.js

/**,
* 이것은 BannerPlugin이 처리한 결과입니다.,
* Build Date: 2020-01-01,
*/

...
```


## 자주사용하는 플러그인

- BannerPlugin
- DefinePlugin
- HtmlWebpackPlugin
- CleanWebpackPlugin
- MiniCssExtraPlugin