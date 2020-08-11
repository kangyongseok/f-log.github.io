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

### BannerPlugin
결과물에 빌드 정보나 커밋 버전같은걸 추가 할 수 있다.

### DefinePlugin
같은소스를 두 환경에서 배포하기위해 환경의존적인 정보를 소스가 아닌곳에서 관리할 수 있도록 해주는 플러그인

### HtmlWebpackPlugin
Html 파일을 후처리하는데 사용한다. 빌드 타임의 값을 넣거나 코드를 입력할 수 있다.

### CleanWebpackPlugin
빌드 이전 결과물을 제거하는 플러그인. 빌드결과물은 아웃풋경로에 모이는데 과거 파일이 남아있을수있어 전체 제거후 다시 빌드한 결과물을 가질 수 있도록 하는 플러그인

### MiniCssExtraPlugin
스타일시트가 많아지면서 하나의 자바스크립트 코드로 만드는것은 부담일 수 있다. 변들결과에서 스타일시트의 코드만 뽑아서 별도의 CSS 파일로 만들어 역할에 따라 분리하는게 좋다. 브라우저에서 큰 파일 하나보다는 작은파일 여러개를 동시에 다운받는게 더 낫기 떄문에 개발환경은 상관없어도 프로덕션환경에서는 분리해서 빌드되도록 하는것이 효과적이다. 이 플러그인은 CSS 를 별도로 뽑아내는 플러그인이다.