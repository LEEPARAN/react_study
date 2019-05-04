### 이 프로젝트는 React를 공부하면서 알아둬야될 것들을 순서에 상관없이 작성한 문서이다.

#### - ES6 Module

모듈이란 애플리케이션을 구성하는 개별적 요쇼로서 재사용 가능한 코드 조각을 말한다.
모듈은 세뷰 사항을 캡슐화하고 공개가 필요한 API만을 외부에 노출한다.

모듈은 독립적인 파일 스코프를 갖기 때문에 모듈 안에 선언한 모든 것들은 기본적으로 해당 모듈 내부에서만
참조할 수 있다. 만약 모듈 안에 선언한 항목을 외부에 공개하여 다른 모듈들이 사용할 수 있게 하고 싶다면
export해야 한다. 선언된 변수, 함수, 클래스 모두 export할 수 있다.

export한 모듈을 로드하려면 export한 이름으로 import한다.

##### EXAMPLE

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




