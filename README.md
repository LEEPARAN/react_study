### 이 프로젝트는 React를 공부하면서 알아둬야될 것들을 순서에 상관없이 작성한 문서이다.

#### - ES6 Module

모듈이란 애플리케이션을 구성하는 개별적 요쇼로서 재사용 가능한 코드 조각을 말한다.
모듈은 세뷰 사항을 캡슐화하고 공개가 필요한 API만을 외부에 노출한다.

모듈은 독립적인 파일 스코프를 갖기 때문에 모듈 안에 선언한 모든 것들은 기본적으로 해당 모듈 내부에서만
참조할 수 있다. 만약 모듈 안에 선언한 항목을 외부에 공개하여 다른 모듈들이 사용할 수 있게 하고 싶다면
export해야 한다. 선언된 변수, 함수, 클래스 모두 export할 수 있다.

export한 모듈을 로드하려면 export한 이름으로 import한다.

###### EXAMPLE
```
// my-module.js
function cube(x) {
  return x * x * x;
}

const foo = Math.PI + Math.SQRT2;

var graph = {
  options: {
    color: 'white',
    thickness: '2px'
  },
  draw: function() {
    console.log('From graph drow function');
  }
}

// 공개하고자 하는 모듈을 export 중괄호 안에 넣어둔다.
export { cube, foo, graph } 

// demo.js
// import 하고자 하는 모듈을 import의 중괄호 안에 넣고 불러오고자 하는 모듈을 담고 있는 파일을 from 뒤에 확장자 명을 제외한 이름만 작성한다.
import { cube, foo, graph } from 'my-module';

graph.options = { // my-module.js의 graph 객체를 변경할 수 있다.
  color: 'blue',
  thinkness: '3px'
};

graph.draw(); // my-module.js의 draw 메소드를 호출할 수 있다.

console.log(cube(3)); // my-module.js의 cube 함수를 실행할 수 있다.
console.log(foo); // my-module.js의 foo 변수를 호출할 수 있다.
```
#### - JSX의 기본 문법 & 규칙

##### JSX란?

JSX라고 불리는 이 구문은 string도 아니고 HTML도 아니다.
React 라이브러리에서 UI를 구성할 때 사용하는 구문으로 Javascript Extension이라고 할 수 있다.
타 프레임워크에서 사용했던 템플릿 엔진이라고 불리는 것들과 유사한 문법을 취하며(생김새만) Javascript의 모든 기능을 제공한다.

형태를 보았을 때 HTML 코드와 유사해보이나 Babel을 돌려보면 Javascript로 변환되는 것을 확인할 수 있다.

##### JSX의 기본 규칙

1. 태그는 무조건 닫아줘야 한다.
HTML5에서 Self-Closing Tags(예시: <input type="text">)같은 경우 뒤에 닫아주지 않더라도
코드가 정상적으로 동작하는 것을 확인할 수 있는데 JSX에서는 이러한 형태의 Single-Closing Tags도
꼭 닫아주어야만 정상적으로 동작을 한다.

###### EXAMPLE
```
<input type="text" />
```

2. 감사져 있는 엘리먼트
두 개 이상의 엘리먼트는 무조건 하나의 엘리먼트로 감싸져있어야 한다.

아래는 실패 예제이다.
아래와 같이 두 개 이상의 엘리먼트를 하나로 감싸지 않을 경우 오류가 발생한다.

###### EXAMPLE
```
// src/App.js
import React, { Component } from 'react';

class App extends Compoent {
  render() {
    return {
      <div>Hello</div>
      <div>Bye</div>
    }
  }
}
```

위의 에러의 대처방안 예제이다.

###### EXAMPLE
```
// src/App.js
import React, { Component } from 'react';

class App extends Compoent {
  render() {
    return {
      <div>
        <div>Hello</div>
        <div>Bye</div>
      </div>
    }
  }
}
```

하지만 위 두 개의 div를 감싸기 위해 하나의 불필요한 Extra div를 생성한다는 것이 별로 달가운 일은 아니다.
이러한 상황에 대처하기 위해 React v16.2에서 Fragment라는 것들 도입하였는데 이를 활용하면 변환하였을 시
Extra div가 존재하지 않는 것을 알 수 있다.

###### EXAMPLE
```
import React, { Component, Fragment } from 'react';

class App extends Compoent {
  render() {
    return {
      <Fragment>
        <div>Hello</div>
        <div>Bye</div>
      </Fragment>
    }
  }
}
```

##### JSX 안에 자바스크립트 값 사용하기

별 다른 설명은 필요하지 않은 것 같아 예제들만 작성하고 넘어가도록 한다.

###### EXAMPLE
```
import React, { Component } from 'react';

class App extends Compoent {
  render() {
    const name = 'react';
    return {
      <div>Hello {name}!</div>
    }
  }
}
```

###### EXAMPLE
```
import React, { Component } from 'react';

class App extends Compoent {
  render() {
    return {
      <div>
      {
        1 + 1 === 2 ? (<div>맞아요!</div>): (<div>틀려요!</div>)
      }
      </div>
    }
  }
}
```

###### EXAMPLE
```
import React, { Component } from 'react';

class App extends Compoent {
  render() {
    const name = 'velopert';
    return {
      <div>
      {
        name === 'velopert!' && <div>밸로퍼트다!</div>
      }
      </div>
    }
  }
}
```

###### EXAMPLE
```
import React, { Component } from 'react';

class App extends Compoent {
  render() {
    const value = 1;
    return {
      <div>
      {
        (function() {
          if(value === 1) return <div>1이다!</div>
          if(value === 1) return <div>2다!</div>
          if(value === 1) return <div>3이다!</div>
          return <div>없다</div>
        })() // 여기 세미콜론 작성하면 에러
      }
      </div>
    }
  }
}
```






