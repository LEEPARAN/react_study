### 이 프로젝트는 React를 공부하면서 알아둬야될 것들을 순서에 상관없이 작성한 문서이다.

#### ES6 Module

모듈이란 애플리케이션을 구성하는 개별적 요쇼로서 재사용 가능한 코드 조각을 말한다.
모듈은 세부사항을 캡슐화하고 공개가 필요한 API만을 외부에 노출한다.

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

<hr/>

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
    return (
      <div>Hello</div>
      <div>Bye</div>
    )
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
    return (
      <div>
        <div>Hello</div>
        <div>Bye</div>
      </div>
    )
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
    return (
      <Fragment>
        <div>Hello</div>
        <div>Bye</div>
      </Fragment>
    )
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
    return (
      <div>Hello {name}!</div>
    )
  }
}
```

###### EXAMPLE
```
import React, { Component } from 'react';

class App extends Compoent {
  render() {
    return (
      <div>
      {
        1 + 1 === 2 ? (<div>맞아요!</div>): (<div>틀려요!</div>)
      }
      </div>
    )
  }
}
```

###### EXAMPLE
```
import React, { Component } from 'react';

class App extends Compoent {
  render() {
    const name = 'velopert';
    return (
      <div>
      {
        name === 'velopert!' && <div>밸로퍼트다!</div>
      }
      </div>
    )
  }
}
```

###### EXAMPLE
```
import React, { Component } from 'react';

class App extends Compoent {
  render() {
    const value = 1;
    return (
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
    )
  }
}
```

##### JSX에서 스타일 사용하기

JSX에서 style을 사용하고자 한다면 객체로 만들어서 사용해야한다.
스타일 속성명은 기존의 스타일 작성법인 단어마다 -(dash)를 사용하는 것과 달리
camelCase(낙타 표기법)를 사용하며 속성값은 String으로 작성하며 수치까지 전부 표기한다.

###### EXAMPLE
```
import React, { Component } from 'react';

class App extends Compoent {
  render() {
    const style = {
      backgroundColor: 'black',
      padding: '16px',
      color: 'white',
      fontSize: '36px'
    }
    return <div style={style}>리액트가 최고야</div>
  }
}
```

##### JSX에서 클래스 사용하기

JSX에서 클래스명을 작성할 경우 속성명에 기존 HTML과 같이 class가 아닌 className으로 작성을 해야한다.
이전에는 class라고 작성했을 때 동작하지 않았으나 버젼업이 진행 된 현재는 class로 작성해도 동작하지만
className으로 작성하는 것이 올바른 컨벤션이니 className으로 작성해주도록 하자.

css는 import로 불러오면되며 속성값은 {} 안에 넣어주면 된다.

###### EXAMPLE
```
// App.css
.App {
  background: black;
  color: aqua;
  font-size: 36px;
  padding: 1rem;
  font-weight: 600;
}

// App.js
import React, { Component } from 'react';
import './App.css';

class App extends Compoent {
  render() {
    return <div className={App}>리액트가 최고야</div>
  }
}
```

##### JSX에서 주석 사용하기

예제를 보면 이해하기 쉽게 작성되었다.
JSX 외부에서의 주석은 기존 자바스크립트와 동일하니
JSX 내부에서 태그 밖과 태그 안에 작성하고자 할때의 차이를 이해하면 편할 것이다.

###### EXAMPLE
```
import React, { Component } from 'react';
import './App.css';

class App extends Compoent {
  render() {
    // JSX 밖에선 이렇게 편하게 작성하면 된다.
    return (
      // 여기까진 JSX 밖이다.
      <div className="App">
         {/* JSX안에선 주석은 멀티라인으로 작성하되 중괄호로 감싸줘야 한다. */}
        <h1
          example="reactBest" // 태그 내부에 주석을 작성하고 싶다면 이렇게 작성해야 한다.
        >
          리액트가 최고야!
        </h1>
      </div>
    )
  }
}
```

#### ES6 Class

ES6에서 새롭게 Class라는 것이 나왔는데 자바나 C++ 등 프로그래밍 언어에서 클래스는 이미 많이 사용되고 있었다
괜시리 사용해보지 않은 것이라 일단 겁부터 먹었는데 MDN의 예제코드를 따라서 작성해보니 기존 Javascript의
prototype과 사용 방식이 같았다. 물론 완전히 같다고 볼 수는 없지만 매우 비슷한 형태를 띄었다.

###### EXAMPLE
```
// ES6 Class
class Number {
	constructor(a, b) {
		this.a = a;
		this.b = b;
	}

	sum() {
		console.log(this.a + this.b);
	}
}

var p = new Number(1, 2);
p.sum(); // 3
```

##### Extends

React 강의를 보다보면 자꾸 class sum extends Number { ... }와 같은 형태의 코드가 나온다.
도대체 이놈의 정체란 무엇인가? 솔직히 이 부분이 궁금해서 Github에 Class 관련 글을 작성하였다.
extends 키워드는 클래스 선언이나 클래스 표현식에서 다른 클래스의 자식 클래스를 생성하기 위해 사용된다고 한다.

예제를 본 후 글을 이어가도록하자.

###### EXAMPLE
```
import React, { Component } from 'react';
import MyName from './MyName';

class App extends Component {
  render() {
    return <MyName name="리액트" />;
  }
}
```

위 코드를 보면 불러온 react에서 Component 클래스를 불러왔고
아래엔 직접 선언한 App 클래스를 extends하여 Component 라는 상위 클래스에 연장했다고 보면 될 것 같다.

솔직히 아직 활용방법이나 이런 부분에 대해선 잘 모르겠고 그냥 이렇구나 하고 넘어가도록 해야겠다.

##### Static

Class Extends와 같이 많이 나오는 것중 하나가 Class static이다.
용도는 Class의 정적 메소드를 사용할 때 static 키워드를 사용하며, 정적 메소드는 클래스의 인스턴스가 아닌
클래스의 이름으로 호출한다. 따라서 클래스의 인스턴스를 생성하지 않아도 호출할 수 있다.

```
Class Foo {
  constructor(prop) {
    this.prop = prop;
  }
  
  static staticMethod() {
    return 'staticMethod';
  }

  prototypeMethod() {
    return this.prop;
  }
}

console.log(Foo.staticMethod()); // staticMethod

const foo = new Foo(123);

console.log(foo.staticMethod()); // Uncaught TypeError: foo.static...
```

위 결과와 같이 클래스의 정적 메소드는 인스턴스로 호출할 수 없으며 
이것은 정적 메소드는 this를 사용할 수 없다는 것을 의미한다.

왜 정적 메소드는 클래스의 이름으로 호출하며 왜 static이 필요한 지에 대해서는 후에 알아보도록 하자.

#### Props

velopert 강사님은 Props가 너무너무너무 중요하다고 하였다. 그만큼 중요한 강의인만큼 열심히 듣고 최대한 쉽게 이해하려고 하였다.

```
// App.js
import React, { Component } from 'react';
import MyName from './MyName'

class App extends Component {
  render() {
    return <MyName name="REACT" />
  }
}

export default App;

// MyName.js

import React, { Component } from 'react';

class MyName extends Component {
  static defaultProps = {
    name: '기본이름'
  }
  render() {
    return <div>안녕 내 이름은 <b>{this.props.name}</b> 라고해!</div>
  }
}

export default MyName;

// 결과: 안녕 내 이름은 REACT 라고해!
```

Props라는 것은 부모가 자식에게 값을 일방적으로 전달해주는 읽기형식이다.
부모 컴포넌트에서 자식 컴포넌트로 값을 전달해주고자 할 때 App 클래스의 return 값처럼 ???="???" 다와 같은 형태로 전달을 한다.
여기서 props라고 하는 것이 name 객체의 value "REACT"를 가져와서 넣어준다.
라고 생각하면 기능적인면에서는 끝내면 될 것 같다.

추가로 static defaultProps라는 것이 있는데 이부분은 부모 컴포넌트에서 props 값 전달을 까먹어 <MyName /> 같은 현상이 생길 경우
해당 값에 default로 defaultProps의 값이 전달되어 들어간다.

#### State

State 또한 매우 중요하다 Props와 다른 차이점이 있다면 Props가 부모 컴포넌트에서 자식 컴포넌트로의 일방적인 읽기 형태의 데이터를
전달만 하였다면 State는 한 컴포넌트에서 데이터를 가지고 있으며 수정을 하고자 하면 수정 또한 가능하다는 점이다.

어떻게 돌아가는지와 수정 방법에 대해서는 코드를 통해 확인하도록 하자.

```
// App.js
import React, { Component } from 'react';
import Counter from './Counter'

class App extends Component {
  render() {
    return <Counter />
  }
}

export default App;

// Counter.js
import React, { Component } from 'react';

class Counter extends Component {
  state = {
    number: 0
  }
  
  handleIncrease = () => {
    this.setState({
	number: this.state.number + 1
    });
  }
  
  handleDecrease = () => {
    this.setState({
	number: this.state.number - 1
    });
  }
  
  render() {
    return (
      <div>
        <h1>카운터</h1>
        <p>숫자: {this.state}</p>
        <button onClick={this.handleIncrease}>+</button>
        <button onClick={this.handleDecrease}>-</button>
      </div>
    )
  }
}

export default Counter;
```

아까전에 State는 한 컴포넌트 안에서 데이터를 보내고 수정하는 작업들을 진행한다고 하였다.
App 클래스를 확인해보면 별 다른 내용이 존재하지는 않는다. 단지 render 메소드에서 return에서 Counter를 Self-Closing-Tag로 생성하였을 뿐이다.
나머지는 전부 Counter 클래스에서 전부 해결한다.

상단의 state에는 문자열, 숫자열이 들어가면 안되고 무조건 객체로 값이 들어가야한다.
그리고 this.setState({ ... })를 활용해서 값을 저장해주어야한다. 만약 이 과정을 거치지않고 단독적으로 this.state.number + 1 이런식으로
진행할 경우 Component에서 값을 기억하지 못해서 setState를 활용해주는 것이다.

부르고자 하는 것들은 같은 클래스 안에있는 this로 부르면 될 것 같고 나머지 onClick 같은 것들은 배우면 또 금세 알게 될 것 같다.






