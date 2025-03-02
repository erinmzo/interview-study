# 11/12

## 리액트에서 성능 최적화를 위한 방법을 소개해주세요.

https://www.maeil-mail.kr/question/18

첫 번째로, react의 memo가 있습니다. 컴포넌트를 메모이제이션(동일한 계산을 반복해야 할 때, 이전에 계산한 값을 메모리에 저장함으로써 동일한 계산으 반복 수행을 제거)이 존재합니다.

또한, `useCallBack`, `useMemo`가 존재합니다. useCallback이란, 함수를 메모이제이션하여 불필요한 함수 재생성을 방지합니다. useMemo는 값의 재계산을 방지하여 성능을 최적화합니다. 만약 함수나 값이 변경되지 않을 경우, 리렌더링을 방지할 수 있습니다.

코드 스플리팅 방법 또한 존재합니다. 큰 애플리케이션을 작은 청크로 나누는 방법입니다. react.lazy, Suspense를 사용하여 동적으로 컴포넌트를 로드할 수 있습니다.

### 코드 스플리팅은 어떤 경우에 사용해야 하나요?

초기 로딩 시간이 길어지는 경우에 사용해야 합니다. 초기 로드 시 필요한 핵심 코드만 로드할 수 있도록 하여 초기 로딩 속도를 크게 개선시킬 수 있습니다.

또한, 라우트별 코드 분할이 필요한 경우에도 사용합니다. SPA에서는 각 페이지가 별도의 기능과 UI를 가지므로, 필요한 코드만 분리하여 로드할 수 있습니다. lazy, Suspense를 사용하여 컴포넌트를 동적으로 불러올 때 유용합니다.

---

# 11/13

## 브라우저 렌더링 파이프라인에 대해 설명해주세요.

https://www.maeil-mail.kr/question/19

렌더링 파이프라인이란, 브라우저가 웹 페이지를 화면에 표시하기 위해 거치는 과정입니다. 크게 5가지 과정으로 나누어집니다.

DOM 생성
브라우저가 HTML 파일을 받을 시, byte 단위로 읽기 시작합니다. HTML Parser는 byte들을 문자로 변환하고, 문자를 다시 HTML 토큰으로 변환합니다.

CSSOM 생성
CSS 파일을 파싱하여, CSS 규칙을 기반으로 CSSOM 트리를 생성합니다.

렌더 트리 생성
DOM과 CSSOM을 결합하여 렌더 트리를 생성합니다.

레이아웃
렌더 트리를 사용해 각 요소의 정확한 위치와 크기를 계산합니다.

페인팅
각 요소를 실제로 화면에 그립니다. 성능에 영향을 많이 끼칠 수 있습니다.

컴포지팅
화면에 그려질 요소를 각각의 레이어로 분리, 결합하여 최종 화면을 구성합니다.

`transform`, `opacity`와 같은 속성은 레이아웃이나 페인트 과정을 거치지 않고, 컴포지팅 단계에서만 처리됩니다. 컴포지팅 단계는 GPU 가속을 활용하여 성능을 최적화하고, 최종적으로 표시되는 결과를 빠르게 생성하는데 중요한 역할을 합니다.

---

# 11/14

## 인터넷 창에서 www.google.com을 입력하면 무슨 일이 일어나는지 설명해주세요.

https://www.maeil-mail.kr/question/20

첫 번째로, DNS 조회가 일어납니다.

브라우저는 해당 도메인 이름을 IP주소로 변환합니다. 이 과정을 DNS 조회라고 합니다. 캐시된 DNS 기록을 확인 후, 없으면 DNS 서버에 요청하여 google에 해당하는 IP 주소를 얻습니다.

두 번째로, TCP 연결 수립입니다. IP 주소가 확인되면, 브라우저는 서버와 TCP 연결을 수립합니다.

세 번째로, HTTP 요청입니다. TCP 연결이 수립되면, 브라우저는 HTTP 또는 HTTPS 요청을 보냅니다. 이 과정에서 브라우저와 서버가 암호화된 연결을 설정하기 위해 보안 인증서를 교환, 암호화 키를 협상합니다.

네 번째로, 서버의 응답이 옵니다. HTTP 응답 코드와 같이 전달됩니다.

마지막으로 받은 리소스를 활용해 렌더링 파이프라인을 진행합니다.

---

# 11/18

## React의 render phase와 commit phase에 대해서 설명해주세요.

https://www.maeil-mail.kr/question/30

리액트의 렌더링 과정은 크게 `render phase`와 `commit phase`로 나눌 수 있습니다.

`render phase`란, 리액트가 변화된 상태나 props에 따라 어떤 UI가 변경되어야 할지를 결정하는 단계입니다. 이 과정에서는 실제로 DOM을 업데이트 하지 않고, 변경사항을 가상 DOM에서 계산하여 비교합니다. 순수한 계산과정이므로, 성능에 영향을 주지 않도록 중단 혹은 다시 실행될 수 있습니다. React 18에서 도입된 Concurrent Mode를 통해 비동기적으로 처리가 가능합니다.

`commit phase`는 실제로 변화된 UI를 DOM에 반영하는 단계입니다. 가상 DOM에서 계산된 결과를 실제 DOM에 적용하고, 변화된 UI를 브라우저에 렌더링합니다. 이 과정에서 `useEffect`와 같은 사이드 이펙트가 발생하는 hook들이 실행됩니다.

**`render phase`는 변화된 UI를 결정하는 과정, `commit phase`는 결정된 결과를 실제로 반영하는 단계입니다.**

### render phase와 commit phase는 어떻게 동기화되나요?

**단계적 진행**과 **병목 관리**로 동기화됩니다.

첫 번째로, `render phase`가 완료되면 리액트는 즉시 `commit phase`를 실행하지 않고, 우선순위에 따라 처리합니다. 이를 통해 동기화가 필요한 작업을 효율적으로 관리합니다.

두 번째는 병목 관리입니다. `render phase`의 모든 변경 사항은 Fiber Tree에 준비된 상태에서 넘어갑니다. 그러므로 render와 commit의 일관성이 유지됩니다.

## 예상 꼬리 질문 - Concurrent mode란?

### 동시성이란?

자바스크립트는 싱글 스레드 언어이다.

각 작업은 실행될 때마다 스레드를 블록하고, 해당 작업을 끝마칠 때까지 다른 작업은 실행되지 않는다.

여기서 동시성이란, 독립적으로 실행될 수 있는 여러 조각들로 나누어서 프로그램을 구조화하는 방식이다. 싱글 스레드의 한계를 벗어나, 어플리케이션을 효율적으로 만들 수 있는 방법이다.

주요 특징으로는, 인터럽트가 가능한 렌더링과, 우선순위 기반 스케줄링, 시간 분할, Suspense와 통합이 있다.

인터럽트가 가능한 렌더링이란, 렌더링을 중간에 중단할 수 있어 새로운 업데이트가 필요할 때 즉시 반응이 가능하다.

우선순위 기반 스케줄링이란, 사용자 입력, 애니메이션 등 중요한 작업을 높은 우선순위로 처리하고, 비동기 업데이트 등 덜 중요한 작업은 낮은 우선순위로 처리하여 전체 애플리케이션의 응답성을 유지하는 것이다.

시간 분할이란, 큰 렌더링 작업을 여러 개의 작은 작업으로 나누어 처리하는 것으로, 브라우저가 중간에 사용자 입력 처리가 가능하므로 UI가 더 부드럽게 유지, 사용자 경험을 향상시킬 수 있다.

Suspense와의 통합은, 데이터 패칭이나 비동기 작업의 로딩 상태를 관리하는데 도움을 준다.

---

# 11/19

## 자바스크립트 호이스팅에 대해서 설명해주세요.

https://www.maeil-mail.kr/question/31

`호이스팅 Hoisting`은 자바스크립트가 코드를 실행하기 전에 변수, 함수 선언을 코드의 최상단으로 끌어올리는 것처럼 동작하는 특징입니다. 이를 통해 코드의 선언된 위치와 관계 없이 변수를 사용하는 것처럼 보입니다.

선언의 호이스팅이지, 실제로 변수 값 할당까지 끌어올리지는 않습니다.

```
console.log(myVar);
var myVar = 10;
console.log(myVar);
```

위의 코드에서, 맨 처음 console.log는 undefined가 나옵니다. myVar의 선언은 호이스팅 되었지만, 값이 할당되지는 않습니다. 밑의 console.log에서는 정상적으로 10이 출력됩니다.

함수 선언의 경우, 전체가 호이스팅되므로 함수 호출을 이전에 해도 문제가 되지 않습니다.

`let`, `const`의 경우는 호이스팅이 되지만, 선언하기 전에 접근하려고 하면 `ReferenceError`가 발생합니다. TDZ가 존재하여, 해당 구간에서는 변수에 접근할 수 없기 떄문입니다.

TDZ란, `Temporal Dead Zone`으로, 변수가 선언되었지만 초기화되기 전까지의 구간을 뜻합니다.

```
console.log(myLet);

let myLet = 10;
```

이 경우, 위의 console.log에서는 `ReferenceError`가 발생합니다.
변수 선언은 호이스팅되었지만 초기화는 변수 선언이 실제로 실행될 떄에 일어나기 때문입니다.

결론적으로, var는 선언만 호이스팅되고(초기화 전에 undefined), let과 const의 경우 TDZ의 존재로 인해 초기화 전에 접근하면 ReferenceError가 발생합니다.

---

# 11/20

## 쿠키와 세션, 웹 스토리지의 차이를 설명해주세요.

쿠키와 웹 스토리지는 클라이언트에 저장되고, 세션은 서버에 저장됩니다.

데이터 수명에 따른 차이점 또한 존재합니다. 쿠키는 만료 기간을 설정할 수 있습니다. 세션의 경우 브라우저를 종료 시 만료기간에 상관 없이 종료됩니다.
웹 스토리지는 로컬 스토리지와 세션 스토리지가 있습니다. 로컬 스토리지는 사용자가 데이터를 지우지 않는 이상, 브라우저나 OS를 조욜해도 계속 브라우저에 남아있습니다. 동일한 브라우저를 사용할 때만 해당됩니다. 세션 스토리지의 경우 오리진 뿐만 아닌 브라우저 탭에도 데이터가 종속되어 있기 때문에 윈도우, 브라우저 탭을 닫을 경우 제거된다.

데이터 용량에 따른 차이점도 존재합니다. 쿠키는 약 4KB, 웹 스토리지는 약 5~10MB, 세션은 서버 용량에 따라 다릅니다.

보안적인 측면의 차이점은 다음과 같습니다. 쿠키와 웹 스토리지는 클라이언트 측 저장으로 보안에 취약합니다. 세션의 경우 서버 측 저장으로 비교적 안전하지만, 세션 관리에 주의가 필요합니다.

---

# 11/21

## 자바스크립트 함수에 대해 설명햊쉐요.

자바스크립트 함수는 일급 객체로 취급됩니다.

이를 통해 자바스크립트는 매우 유연하고, 고차 함수를 포함한 다양한 패턴을 구현할 수 있습니다.

자바스크립트 함수의 주요 특징은 다음과 같습니다.

### 일급 객체

자바스크립트에서 함수는 값으로 취급될 수 있습니다. 변수에 할당되거나, 다른 함수의 인자로 전달하거나, 함수의 변환값으로 사용할 수 있습니다.

```
const sayHello = function() { return 'Hello'; };
console.log(sayHello()); // 'Hello'

const executeFunction = function(fn) {
  return fn();
};
console.log(executeFunction(sayHello)); // 'Hello'
```

### 익명 함수, 함수 표현식

자바스크립트에서 이름 없는 함수를 정의할 수 있습니다. 익명 함수는 표현식에서 주로 사용됩니다.

```
const add = function(a, b) {
  return a + b;
};
console.log(add(2, 3)); // 5
```

### 호이스팅

함수 선언은 코드가 실행되기 전 호이스팅됩니다. 함수 선언 이전에 호출이 가능합니다.

함수 표현식의 경우 변수에 할당된 후에 사용이 가능합니다.

```
console.log(declaredFunction()); // 'Declared Function'
function declaredFunction() {
  return 'Declared Function';
}

// 함수 표현식은 할당 후에만 사용할 수 있음
const expressedFunction = function() {
  return 'Expressed Function';
};
console.log(expressedFunction()); // 'Expressed Function'
```

### 클로저

클로저는 함수가 자신이 선언된 환경을 기억하고, 그 외부 스코프에 접근할 수 있는 기능입니다. 이를 통해 함수는 자신이 선언된 스코프 내의 변수를 참조하고 유지할 수 있습니다.

```
function outer() {
  const outerVar = 'I am outer!';

  return function inner() {
    return outerVar; // 외부 변수에 접근 가능
  };
}
const innerFunction = outer();
console.log(innerFunction()); // 'I am outer!'
```

### 고차 함수

함수가 일급 객체이므로, 고차 함수를 정의할 수 있습니다.

고차 함수란, 다른 함수를 인자로 받거나 반환하는 함수입니다.

```
function multiplyBy(factor) {
  return function(num) {
    return num * factor;
  };
}
const double = multiplyBy(2);
console.log(double(5)); // 10
```

### 화살표 함수

더 간결한 문법을 제공하고, this 바인딩에서 기존 함수와 다른 동작을 합니다. 화살표 함수는 선언된 위치의 this 값을 유지하므로 일반 함수와 달리 별도로 this 바인딩을 할 필요가 없습니다.

```
const obj = {
  value: 42,
  method: function() {
    setTimeout(() => {
      console.log(this.value); // 42 (Arrow 함수는 obj의 this를 유지)
    }, 1000);
  }
};
obj.method();
```

---

# 11/25

## CommonJS와 ES Module의 차이점에 대해서 설명해주세요.

`CommonJS`와 `ES Module(ESM)`은 자바스크립트에서 모듈을 관리하고 불러오는 두 가지의 주요 방식입니다.

`CommonJS`은 Node.js 환경에서 사용되며, 모듈을 동기적으로 불러옵니다. 모듈이 로드될 때까지 다음 코드가 실행되지 않습니다. `require` 키워드를 사용해 모듈을 가져오고, `module.exports`를 통해 내보냅니다. 주로 서버측에서 사용됩니다.

`ESM`은 자바스크립트의 공식 표준 모듈 시스템입니다. ES6부터 도입되었으며, 브라우저와 node.js 환경에서 모두 사용이 가능합니다.

모듈을 비동기적으로 로드하며, 모듈을 가져올 때는 import, 내보낼 때는 export를 사용합니다. 또한 정적 분석이 가능하여 트리 쉐이킹과 같은 최적화 작업에도 유리합니다.

`CommonJS`는 동기적이며 주로 서버측에서 사용, `ESM`은 비동기적이고 브라우저, 서버 모두에서 사용이 가능하다는 차이점이 있습니다.

최근에는 `ESM`의 사용이 증가하고 있는 추세입니다. 브라우저와 서버 간의 모듈 호환성, 비동기적 로딩과 트리 쉐이킹 같은 최적화 작업에 유리하다는 점에서 선호됩니다.

---

# 11/26

## 이벤트 전파(event propagation)에 대해서 설명해주세요.

이벤트 전파는 DOM에서 이벤트가 발생하였을 때, 이벤트가 어떤 방식으로 전파되는지를 설명하는 개념입니다.

캡처링, 타겟, 버블링의 3가지 단계로 나눌 수 있습니다.

캡처링의 경우, 이벤트가 DOM 트리 최상위 요소(주로 document, window)에서 시작하며, 이벤트가 발생한 요소를 향해 내려가는 단계입니다. 이 과정에서 상위 요소들에 이벤트 리스너가 있으면 그 순서대로 실행될 수 있습니다.

타깃 단계의 경우, 이벤트가 실제로 발생한 타깃 요소에 도달하는 단계입니다. 타깃 요소에 등록된 이벤트 리스너가 이 시점에 실행됩니다.

버블링 단계의 경우, 타깃 요소에서 이벤트가 발생한 후 다시 상위 요소로 이벤트가 전파되어 올라가는 단계입니다. 상위 요소에 등록된 이벤트 리스너가 실행될 수 있습니다.

대부분의 이벤트는 버블링을 통해 전파되지만, addEventListener의 세 번째 인자로 {capture:ture}를 전달하면 캡처링 단계에서도 이벤트를 처리할 수 있습니다.

웹 페이지에서 요소 간의 상호작용을 하는 데 아주 중요한 역할을 합니다.

---

# 11/27

## 웹 애플리케이션의 성능을 최적화할 수 있는 방법들에 대해서 아는대로 설명해주세요.

첫 번째로, 코드 스플리팅입니다. 자바스크립트 파일을 필요한 부분만 나누어 로드할 수 있습니다. 페이지 로딩 속도를 개선하는데 도움을 줍니다.

두 번쨰로, 레이지 로딩 기법입니다. 페이지에 있는 무거운 리소스(이미지, 비디오 등)을 사용자가 실제로 볼 때만 로드하는 방식입니다. 불필요한 리소스 로딩을 줄여 성능이 높아집니다.

이미지에 대한 최적화도 존재합니다. 물리적 크기를 줄이거나, WebP와 같은 가벼운 포맷으로 변환하는 방법입니다.

캐싱을 활용하면, 로딩된 리소스를 다시 다운로드 하지 않고 브라우저가 캐시된 데이터를 재사용하여 성능을 크게 향상시킬수 있습니다.

자바스크립트 로딩 시에는 비동기 로딩 혹은 지연 로딩을 사용하여 DOM을 차단하지 않도록 할 수 있습니다. 페이지가 로딩되는 동안에도 자바스크립트 파일을 병렬로 불러오거나, 적절한 타이밍에 로드하게 되어 사용자 경험이 쾌적해질 수 있습니다.

---

# 11/28

## 디바운스와 쓰로틀에 대해서 각각 설명해주세요.

**디바운스**와 **쓰로틀**은 이벤트 핸들러가 너무 자주 실행되지 않도록 조절하는 기법입니다.

### 디바운스

이벤트가 연속적으로 발생 시, 마지막 이벤트가 발생한 후 일정 시간이 지나야 이벤트 핸들러가 실행되는 방식입니다.

검색창에 사용자가 키를 입력할 때마다 검색하는 것이 아닌, 입력을 멈춘 후 몇 초 뒤에 요청을 보내는 예시가 존재합니다.

### 쓰로틀

일정 시간 간격 동안 발생한 이벤트 중 첫 번째, 혹은 마지막 이벤트만 처리하는 방식입니다. 이벤트가 계속해서 발생하더라도 설정한 시간 동안은 한 번만 이벤트 핸들러가 발생합니다.

디바운스는 마지막 이벤트 이후에 딜레이를 두고 처리, 쓰로틀은 일정 시간 간격을 두고 이벤트를 처리하는 방식입니다.

## 디바운스와 쓰로틀 중에서 무한 스크롤 구현 시 어떤 방식을 선택하시겠습니까? 그 이유는 무엇인가요?

무한 스크롤 구현 시에는 쓰로틀을 사용할 것 같습니다.

스크롤은 연속적인 동작이며, 사용자가 페이지 하단에 도착할 시 바로 요청을 하여야 합니다.

쓰로틀은 하단에 위치하게 된 즉시 추가 데이터 요청을 수행할 수 있습니다.

디바운스의 경우 스크롤이 멈춰야 데이터를 불러올 수 있습니다. 지연이 발생할 확률이 높습니다.

데이터를 선제적으로 로드하는게 좋으므로, 쓰로틀을 선택할 것 같습니다.

---

# 11/29

## 리액트에서 index를 key값으로 사용하면 안되는 이유에 대해서 설명해주세요.

배열의 요소들이 추가되거나 삭제될 때, 배열의 순서가 바뀌는 경우 문제가 발생할 수 있기 때문입니다.

리액트는 key를 통해 리스트에서 어떤 요소가 변경, 추가, 삭제되었는지를 추적합니다. index를 키로 사용할 시, 배열의 순서가 변경될 때 리액트가 요소를 잘못 인식할 가능성이 높습니다.

예를 들어, 배열에 새로운 요소가 추가되면 그 뒤에 있는 요소들의 인덱스가 모두 바뀌게 됩니다. 리액트는 이를 새로운 요소로 인식해 불필요하게 재렌더링을 하거나, 요소의 상태를 잘못 처리할 수 있습니다.

배열의 순서나 요소 변경에 영향을 받지 않는 고유한 값을 key로 사용하는 것이 좋습니다.

## key로 사용되는 고유한 값의 생성 방법에는 어떤 것들이 있나요?

데이터의 유일성을 보장, 변하지 않는 값을 선정해야 합니다.

데이터베이스에서 제공하는 고유 ID를 사용하는 것이 일반적이고, `UUID`와 같은 고유한 id를 생성할 수 있는 라이브러리를 사용할 수도 있습니다.

---

# 12/2

## async/await에 대해 설명해보세요.

async/await는 JavaScript에서 비동기 작업을 보다 간결하고 직관적으로 처리하기 위한 ES2017(ECMAScript 2017) 문법입니다.

async 키워드는, 함수 앞에 async를 붙이면 해당 함수는 항상 Promise 객체를 반환하는 비동기 함수가 됩니다.
await 키워드는, async 함수 내부에서만 사용 가능하며, Promise가 처리될 때까지 함수의 실행을 일시 정지합니다. Promise가 해결되면 결과 값을 반환하고 다음 코드를 실행합니다.

async/await를 사용하면 복잡한 비동기 처리 로직을 마치 동기식 코드처럼 읽기 쉽게 작성할 수 있습니다. 코드의 가독성을 높이고, Promise의 then() 체인으로 인한 콜백 지옥을 방지하는 데 도움이 됩니다.

## 비동기(async)란 무엇인가요?

업이 순차적으로 처리되지 않고, 다른 작업이 완료되기를 기다리지 않으면서 동시에 여러 작업을 처리하는 방식입니다.

프로그램이 하나의 작업이 완료될 때까지 멈추지 않고 다음 작업을 계속 수행할 수 있게 하여 효율성과 성능을 향상시킵니다.

---

# 12/3

## useEffect와 useLayoutEffect의 차이점에 대해서 설명해주세요.

`useEffect`와 `useLayoutEffect`는 모두 렌더링된 후에 특정 작업을 수행하기 위해 사용됩니다.

실행되는 타이밍과 용도가 다릅니다.

### useEffect

렌더링이 완료되는 시점에 비동기적으로 실행됩니다. 화면이 실제로 사용자에게 그려진 후에 실행되는 방식입니다.

보통 데이터를 가져오는 작업, 이벤트 리스너 추가 등 렌더링 후에 화면에 직접적인 영향을 주지 않는 작업에 주로 사용됩니다.

### useLayoutEffect

렌더링 후 DOM이 업데이트되기 직전의 시점에 동기적으로 실행됩니다.

동기적이란, 화면에 내용이 그려지기 전에 모든 레이아웃 관련 작업이 완료된다는 의미입니다.

DOM의 크기를 측정하거나, 위치를 조정해야 할때 사용할 수 있습니다.

렌더링 후 실행되는 비동기 작업에는 useEffect가 적합하고, 레이아웃 작업이나 DOM 조작과 같이 화면이 그려지기 전에 완료되어야 하는 작업에는 useLayoutEffect가 적합합니다.

동기적으로 실행되므로, 많은 작업이 실행되면 렌더링이 느려질 수 있습니다. 화면에 영향을 주는 작업만 처리하는 것이 좋습니다.

### 예시

useEffect는 사용자 데이터를 API로부터 가져오는 상황에 자주 사용합니다. 데이터가 렌더링 후에 설정되면 화면이 자연스럽게 업데이트되는 것입니다.

```
useEffect(() => {
  fetchData().then(data => setData(data));
}, []);
```

useLayoutEffect는 DOM의 크기를 측정해서, 다른 요소의 위치를 조정해야 할 때 유용합니다. 예를 들어, 어떤 요소의 높이를 측정해 그 높이에 맞춰 레이아웃을 맞추고 싶을 때 사용합니다.

```
useLayoutEffect(() => {
  const height = ref.current.offsetHeight;
  setHeight(height);
}, []);
```

---

# 12/4

## SSR(Server Side Rendering)에 대해 설명해주세요.

`SSR`이란, 초기 화면을 클라이언트가 아닌 서버에서 렌더링하여 완성된 HTML을 클라이언트에 내려주는 방식입니다.

`CSR(Client Side Rendering)` 방식에서는 브라우저가 서버로부터 비어있는 뼈대 HTML을 받아온 후, 필요한 자바스크립트 번들을 다운로드하고 번들을 싫애하여 동적으로 컨텐츠를 채웁니다.

### SSR의 장점은 무엇인가요?

SEO(검색 엔진 최적화) 측면에서 유리합니다. 화면이 동적으로 그려지는 CSR에 비해 검색 엔진이 크롤링할 때 더 쉽게 컨텐츠를 인식할 수 있기 때문입니다.

이런 점에서 SSR은 블로그, 커머스 등 SEO가 중요한 웹 어플리케이션에 특히 적합합니다.

또한, 빠른 로딩 속도를 경험할 수 있습니다. CSR과 달리 SSR에서는 번들을 다운로드할 필요가 없고, 번들을 실행하여 동적으로 화면을 그려야 할 필요도 없습니다.

### SSR의 단점은 무엇인가요?

상호작용 초기화가 느립니다. 페이지가 표시되기까지 걸리는 시간(TTV)과 상호작용까지 걸리는 시간(TTI) 사이에 격차가 발생한다는 의미입니다.

상대적으로 구현 난이도가 높습니다. 자바스크립트 번들로 대부분의 작업이 수행되는 CSR에 비해서 SSR은 과정이 복잡하기 때문입니다.

서버 비용이 증가합니다. 정적인 파일을 내려주기만 하면 되는 CSR과 달리, 동적으로 HTML을 생성하기 위해 WAS 서버를 구동해야 하기 때문입니다.

---

# 12/6

## CSS Flexbox와 Grid의 차이점에 대해서 설명해주세요.

Flexbox와 Grid는 페이지에서 레이아웃을 구성할 때 자주 사용되는 CSS 속성입니다.

Flexbox는 1차원 레이아웃이지만, Grid는 2차원 레이아웃입니다.

Flexbox는 1차원 레이아웃 속성으로, row 또는 column 중 하나를 기준으로 요소를 정렬하고 배치하는 데 최적화되어 있습니다. 행이나 열 중 하나의 방향으로 정렬해야 할 때 유용합니다.

Grid는 2차원 레이아웃 속성으로, 행과 열을 모두 사용해 요소를 배치할 수 있습니다. 따라서 복잡한 레이아웃을 구성하거나, 웹페이지의 전체적인 구조를 잡는 데 적합합니다.

Flexbox는 콘텐츠 중심으로, 콘텐츠가 추가되거나 줄어들 때 유연하게 대처하기 좋습니다.

Grid는 레이아웃 중심으로 페이지 구조를 구성하는 데 최적화되어 있습니다. 예를 들어, 카드 레이아웃, 갤러리 형식 등 명확하게 구분된 영역을 기반으로 레이아웃을 구성할 때 Grid가 효과적입니다.

Flexbox에서는 주로 요소가 컨테이너의 크기나 위치에 맞춰 자동으로 정렬됩니다. 에시로 justify-content, align-items 등이 있습니다.

Grid는 행과 열을 사전에 정의하고 그 격자(grid cell)에 요소를 배치하는 방식입니다. 예시로는 grid-template-rows, grid-template-columns 등이 있습니다.

---

# 12/9

## 프론트엔드 E2E 테스트에 대해서 설명해주세요.

프론트엔드 E2E(End-to-End) 테스트는 애플리케이션의 사용자 경험을 처음부터 끝까지 시뮬레이션하여 테스트하는 방식입니다. E2E 테스트는 사용자 관점에서 전체 애플리케이션이 의도한 대로 작동하는지 검증합니다.

E2E 테스트는 Cypress, Playwright과 같은 도구를 이용해 작성합니다.

E2E 테스트를 진행하면 사용자와 동일한 방식으로 애플리케이션을 테스트하므로, 사용자에게 직접적인 영향을 미치는 오류를 조기에 발견할 수 있습니다. 프로덕트의 안정성을 높이고, 실제 배포 후 발생할 수 있는 리스크를 줄이는 데 도움이 됩니다.

따라서 E2E 테스트는 중요한 사용자 흐름이나 비즈니스 로직이 포함된 페이지에 적용하면 효과적입니다.

### 유닛 테스트로도 충분히 안정성을 높이고, 리스크를 줄일 수 있지 않나요?

유닛 테스트는 개별적인 코드 조각이 제대로 작동하는지는 확인하지만, 전체 시스템의 흐름과 사용자가 실제로 겪는 경험을 확인하지는 않습니다.

예를 들어, 로그인 기능의 유닛 테스트는 올바른 사용자 정보를 입력했을 때 인증이 성공하는지를 확인할 수 있지만, 로그인 이후의 페이지 이동, 세션 유지, 렌더링 등은 확인하지 못합니다.

E2E 테스트는 애플리케이션을 사용자 관점에서 처음부터 끝까지 검사하여, 모든 시스템이 통합적으로 잘 작동하는지 확인합니다.

유닛테스트와 E2E 테스트를 상호 보완적으로 함께 활용하는 것이 좋습니다. 유닛 테스트는 개별 컴포넌트를 신속하고 정확하게 검사하여 디버깅 시간을 줄이고, 코드의 작은 변화가 의도한 대로 작동하는지 확인합니다. 반면, E2E 테스트는 애플리케이션의 중요한 사용자 흐름을 점검하여 배포 후 발생할 수 있는 치명적인 문제를 예방합니다. 이처럼 두 테스트를 함께 활용하면 애플리케이션의 안정성과 신뢰성을 극대화할 수 있습니다.

---

# 12/10

## 이미지 크기가 클 경우 렌더링 속도가 느려질 텐데, 이를 개선하기 위한 방법들을 설명해주세요.

첫째, 이미지 포맷 최적화입니다.

전통적인 JPEG나 PNG 대신, 압축 효율이 높은 WebP 또는 AVIF와 같은 최신 포맷으로 변환할 수 있습니다.

이미지 품질을 유지하면서도 파일 크기를 크게 줄여줍니다. 하지만, 일부 구버전 브라우저에서는 지원하지 않을 수도 있습니다.

둘째, 이미지 사이즈 조정입니다.

화면에 노출되는 크기에 비해 이미지가 과도하게 큰 경우 이미지를 작게 리사이징할 수 있습니다.

셋째, 지연 로딩(Lazy Loading)입니다.

사용자가 화면에 스크롤할 때 해당 위치에 도달하는 이미지가 로드되도록 설정하는 방법입니다.

CDN(Content Delivery Network) 방법 또한 존재합니다.

CDN을 적용하면 사용자가 지리적으로 가까운 서버에서 이미지를 다운로드하게 되어 로딩 속도를 단축시킬 수 있습니다.

### WebP나 AVIF는 모든 브라우저에서 지원하지 않는다고 하셨는데, 호환성 문제를 어떻게 해결할 수 있을까요?

HTML의 <picture> 요소를 통해 fallback 이미지를 적용할 수 있습니다.

```typescript
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Image description">
</picture>
```

---

# 12/12

## 자바스크립트는 싱글 스레드 언어인데, 어떻게 동시에 여러 작업들을 수행하나요?

자바스크립트는 브라우저의 Web API나 Node의 libuv, 이벤트 루프, 태스크 큐를 이용하여 비동기 작업을 동시에 처리합니다.

비동기 작업이 발생하면, 해당 작업(타이머, 네트워크 요청 등)은 브라우저의 Web API에 위임됩니다. 예를 들어, setTimeout이나 fetch와 같은 작업이 수행되면 자바스크립트 엔진은 이 작업들을 Web API에 넘기고 다른 코드 실행을 이어갑니다. Web API에서 비동기 작업이 완료되면, 그 작업은 태스크 큐에 들어가 대기합니다.

이후 이벤트 루프가 콜 스택이 비어있는지 확인한 뒤 태스크 큐에서 대기 중인 작업을 콜 스택으로 가져와 실행합니다. 이러한 구조 덕분에 자바스크립트는 싱글 스레드임에도 비동기적으로 작업을 처리하여 다양한 작업을 효율적으로 관리할 수 있습니다. 이 메커니즘 덕분에 UI 인터랙션이 끊기지 않으며, 대기 시간이 필요한 작업도 동시에 실행되는 것과 같이 동작하게 됩니다.

### 태스크 큐의 종류에는 어떤 게 있나요?

매크로태스크 큐와 마이크로태스크 큐로 나뉩니다.

매크로태스크 큐는 일반적인 비동기 작업의 콜백이 저장되는 큐입니다.

setTimeout, setInterval, I/O 작업, 이벤트 핸들러 등은 작업 완료 후 매크로태스크 큐에 콜백을 대기시킵니다. 매크로태스크 큐는 이벤트 루프의 한 번의 반복마다 하나의 태스크만 처리되므로, UI 업데이트나 다른 작업과 균형 있게 진행됩니다.

마이크로태스크 큐는 더 높은 우선순위가 필요한 비동기 작업들이 대기하는 큐입니다.

Promise.then, MutationObserver 등의 비동기 콜백이 여기에 저장됩니다. 이벤트 루프는 매크로태스크를 실행하기 전에 항상 마이크로태스크 큐를 먼저 확인하고, 모든 마이크로태스크를 처리한 후 매크로태스크로 넘어갑니다. 이 방식으로 마이크로태스크 큐의 작업은 높은 우선순위로 처리됩니다.

---

# 12/13

## 타입스크립트의 타입과 인터페이스의 차이점을 설명해주세요.

interface는 객체의 형태를 확장하는 데 용이합니다.

type은 튜플, 인터섹션, 유니온 등을 이용하여 더 복잡한 타입 정의 및 조합을 표현하는 데 용이합니다.

### interface

interface는 선언 병합을 지원해 여러 번 선언할 수 있어, 주로 객체 타입을 확장할 때 유리합니다.

동일한 이름을 가진 interface를 여러 번 선언하면, 이 속성들이 자동으로 합쳐집니다.

```typescript
interface Person {
  age: number;
  name: string;
  isBirthday: boolean;
}

interface Person {
  address: string;
}

const person1: Person = {
  age: 1,
  name: "abcd",
  isBirthday: false,
  address: "1010",
};
```

### type

type으로 선언한 경우에는 동일한 이름을 중복 선언하면 에러가 발생합니다.

type은 튜플과 같은 복잡한 타입 표현이 가능하며, 복잡한 타입 조합을 위해 인터섹션(&)과 유니온(|) 연산자를 지원합니다.

```typescript
type BasicInfo = {
  name: string;
  age: number;
};

type ContactInfo = {
  email: string;
  phone: string;
};

// 인터섹션 타입 (&)을 사용해 두 타입을 결합하여 하나의 타입으로 생성
type PersonInfo = BasicInfo & ContactInfo;

const person2: PersonInfo = {
  name: "John",
  age: 30,
  email: "john@example.com",
  phone: "123-456-7890",
};
```

**interface는 선언 병합을 통해 여러 번 선언이 가능하여 주로 객체 타입을 확장하는 데 유리하며, type은 튜플 등 복잡한 타입을 사용하고 유연한 연산자를 통해 복잡한 타입 조합을 표현하는 데 적합합니다.**

---

# 12/16

## 시맨틱 마크업이란 무엇이며, 왜 중요한가요?

시맨틱 마크업은 HTML 요소를 사용하는 방식으로, 단순히 시각적 목적이 아닌 요소의 의미를 잘 나타내도록 작성하는 방식을 말합니다.

<div>와 <span> 같은 비시맨틱 태그가 아닌, <header>, <footer>, <article>, <section> 같은 시맨틱 태그를 사용하여 문서 구조와 콘텐츠의 역할을 명확하게 하는 것입니다.

첫째, 접근성을 개선하기 위함입니다. 시맨틱 요소들은 스크린 리더와 같은 접근성 도구에서 콘텐츠의 구조를 더욱 잘 해석할 수 있게 해 주어 시각장애인이나 노인 등 다양한 사용자층이 사이트를 효과적으로 탐색할 수 있게 합니다. 이러한 요소를 올바르게 사용하면, 더 많은 사람들에게 접근 가능한 웹 환경을 제공할 수 있습니다.

둘째, SEO(검색 엔진 최적화)에 유리합니다. 검색 엔진은 HTML의 시맨틱 구조를 통해 페이지의 구성을 파악합니다. 그렇기에 시맨틱 마크업을 적절히 적용하면, 검색 엔진이 페이지를 올바르게 파악할 수 있고, 그에 따라 검색 결과에서 페이지가 더 잘 노출될 가능성이 높아집니다.

웹 접근성와 SEO에 대해 영향을 미칩니다.

### CSR(Client Side Rendering)에서도 시맨틱 마크업이 SEO에 영향을 미친다고 보시나요? 만약 그렇다면, 왜 그렇다고 생각하시나요?

CSR에서는 대부분의 콘텐츠가 클라이언트 측에서 렌더링되기 때문에, 검색 엔진이 페이지를 크롤링할 때 페이지의 초기 콘텐츠만 인식할 가능성이 큽니다. 그렇더라도 최근 검색 엔진들은 JavaScript 렌더링을 지원하는 방향으로 진화하고 있으며, 페이지의 시맨틱 구조를 어느 정도 파악할 수 있습니다.

따라서 시맨틱 마크업을 제대로 적용하면 CSR에서도 검색 엔진이 콘텐츠의 중요한 부분을 더 쉽게 인식하게 되어 검색 결과에 긍정적인 영향을 미칠 수 있습니다.

---

# 12/18

## useEffect가 호출되는 시점에 대해 설명해 주세요.

컴포넌트가 마운트, 업데이트, 언마운트 되는 시점에 호출됩니다.

useEffect는 컴포넌트가 마운트될 때, 즉, 처음 렌더링되고 나서 호출됩니다. 이때 데이터 초기화나 외부 API 호출, 구독 설정 등의 작업을 실행할 수 있습니다. 이때 useEffect는 컴포넌트가 처음 마운트될 때 필요한 초기 작업을 수행할 수 있도록 해줍니다.

useEffect는 의존성 배열에 지정된 값이 변경될 때마다 다시 호출됩니다. 두 번째 인자로 주어지는 의존성 배열은 useEffect가 어떤 상태나 props의 변화에 반응할지를 결정하는데, 예를 들어 useEffect(() => {...}, [count])처럼 count 상태가 의존성 배열에 있을 경우, count 값이 변경될 때마다 useEffect가 호출됩니다.

이를 통해 특정 상태나 props가 변경될 때마다 필요한 동작을 수행하도록 할 수 있으며, 컴포넌트의 변화에 따라 동적으로 실행되는 로직을 설정할 수 있습니다.

컴포넌트가 언마운트될 때 useEffect의 return 값으로 지정된 클린업 함수가 호출됩니다. 이 정리 함수는 이벤트 리스너 제거, 타이머 해제, 구독 취소 등의 작업을 수행하여 컴포넌트가 사라질 때 필요한 정리 작업을 처리할 수 있습니다.

이를 통해 컴포넌트에서 발생한 부수효과를 정리하는 작업을 수행할 수 있습니다.

**컴포넌트가 처음 렌더링된 후, 의존성 배열의 값이 변경될 때, 그리고 컴포넌트가 언마운트될 때 호출됩니다.**

---

# 12/19

## 자바스크립트 Promise에 대해서 아는 대로 설명해주세요.

Promise는 비동기 작업을 관리하고, 해당 작업의 성공 또는 실패 결과를 나중에 사용할 수 있도록 하는 객체입니다.

Promise는 비동기 작업이 아직 완료되지 않은 초기 상태를 나타내는 Pending, 비동기 작업이 성공적으로 완료되어 값을 반환한 상태인 Fulfilled, 비동기 작업이 실패하여 오류를 반환한 상태인 Rejected 총 3가지 상태를 가집니다.

세 가지 상태 중 하나로 전환되면, 이후에는 다른 상태로 전환되지 않으며, Fulfilled나 Rejected 상태가 되면 결과 값을 통해 해당 작업의 성공 여부를 알 수 있습니다.

Promise 객체는 비동기 작업을 수행할 함수를 인자로 받아서 실행하며, 이 함수는 resolve와 reject라는 두 가지 콜백을 받습니다.

resolve는 비동기 작업이 성공했을 때 값을 전달하여 Promise를 fulfilled 상태로 전환하고, reject는 비동기 작업이 실패했을 때 오류를 전달하여 Promise를 rejected 상태로 전환합니다.

또한 try-catch-finally의 구조로 비동기 작업의 실패와 에러, 또 마지막 부분에 대한 처리를 명시적으로 나타내줄 수 있습니다.

**Promise는 코드의 가독성을 높이고, 비동기 작업의 흐름을 제어하는 데에 매우 유용합니다. 특히 여러 개의 Promise를 순차적으로 연결할 수도 있고 Promise.all 이나 allSettled 같은 메서드를 통해 병렬로 비동기 작업을 처리해볼 수도 있습니다.**

### 그럼 Promise의 단점은 없나요?

복잡한 에러 처리, 완전히 해결하지 못하는 콜백 지옥이 단점입니다.

여러 Promise가 중첩되거나 서로 다른 비동기 흐름에서 에러가 발생할 경우 복잡도가 증가할 수 있습니다.

비동기 작업이 복잡하게 중첩되면 여전히 콜백과 유사하게 여러 then 메서드가 연속해서 사용되며 가독성이 떨어질 수 있습니다. 여러 Promise를 순차적으로 실행해야 할 때 then 체인을 계속 사용하면 코드의 들여쓰기 구조가 복잡해지고 이해하기 어려워지는 문제점이 있습니다. 이는 async/await을 통해 개선할 수 있습니다.

---

# 12/20

## ES6에 대해서 아는 대로 설명해 주세요.

ES6(ECMAScript 2015)는 자바스크립트의 최신 버전으로, 2015년에 공식 발표되었습니다.

첫째, let과 const 키워드가 추가됐습니다. let은 변수 선언, const는 상수 선언에 사용됩니다. var와 달리 let과 const는 블록 스코프를 가지므로 코드의 안정성이 더 높습니다. 또한, 변수 선언 이전에 접근했을 때 undefined가 할당되지 않고, ReferenceError가 발생한다는 점에서도 차이가 있습니다.

둘째, 화살표 함수(Arrow Function)가 도입되었습니다. 기존의 함수 정의 방식보다 간결하고 가독성이 좋습니다. 또한 this의 바인딩을 호출 문맥과 일치시키기 때문에 함수 내부에서의 혼란이 줄었습니다.

셋째, 클래스 문법이 추가되었습니다. 이를 통해 객체 지향 프로그래밍의 핵심 개념인 생성자, 상속, 메서드 오버라이딩 등을 자바스크립트에서 활용할 수 있게 되었습니다.

넷째, 템플릿 리터럴이 추가되었습니다. 문자열 내에 변수를 손쉽게 삽입할 수 있어, 기존의 문자열 연결 방식보다 가독성과 유연성이 향상되었습니다.

그 외에도, 구조 분해 할당, Spread Operator와 Rest Parameter, Promise 등 중요한 기능들이 ES6를 기점으로 추가되었습니다.

### 이제 ES6 이전 버전의 문법은 몰라도 괜찮을까요?

여전히 중요합니다.

기존 코드베이스와의 호환성 때문입니다. 기존 자바스크립트 프로젝트, 라이브러리는 es5 이전의 문법을 사용하고 있습니다.

또한 대규모 프로젝트의 경우 점진적 마이그레이션이 필요해, 이 과정에서 문법이 혼재될 수 있습니다.

ES6 기능을 구형 브라우저에서 사용하려면 폴리필, 트랜스파일러를 활용해야 하는데, 이때 ES5 이전 문법에 대한 기본적 이해가 필요합니다.

---

# 12/23

## JWT(JSON Web Token)에 대해 설명해주세요.

JWT는 JSON 형태의 데이터를 안전하게 전송하기 위해 사용되는 토큰 기반 인증 방식입니다. 서버와 클라이언트가 통신할 때, 사용자의 인증 상태 정보를 토큰 안에 담아 보냄으로써, 추가적인 서버 세션이나 DB 조회 없이도 사용자를 인증할 수 있게 해줍니다.

JWT는 크게 세 부분으로 구성됩니다.

헤더(Header), 토큰의 타입(JWT)과 해싱 알고리즘(HS256 등) 정보를 담습니다.

페이로드(Payload), 사용자 정보나 권한 등 필요한 데이터를 JSON 형태로 담습니다.

시그니처(Signature), 헤더와 페이로드를 합친 뒤 비밀키로 서명하여, 토큰이 변조되지 않았음을 보장합니다.

사용자가 로그인하면 서버는 사용자 정보를 바탕으로 JWT를 생성하고, 클라이언트에 전달합니다. 이후 클라이언트는 API 요청 시 헤더(Authorization)에 JWT를 포함하여 서버로 보냅니다.

서버는 토큰의 시그니처를 확인하여 유효성(변조 여부, 만료 여부 등)을 검증하고, 정상일 경우 페이로드 안의 사용자 정보를 통해 접근을 허용합니다. 이 과정에서 추가 세션 저장이나 DB 조회가 필요 없는 것이 특징입니다.

JWT의 장점은 세션 상태를 서버에 저장할 필요가 없어 서버의 확장성이 좋고, 모바일 애플리케이션에서도 쉽게 사용할 수 있다는 것입니다.

하지만 토큰 자체에 정보가 포함되어 있어 크기가 커질 수 있고, 한번 발급된 토큰은 만료되기 전까지 취소가 어렵다는 단점이 있습니다.

---

## TDD란 무엇인지 설명해주세요.

TDD(Test-Driven Development)는 소프트웨어 개발 방법론 중 하나로, 테스트를 먼저 작성한 후 실제 코드를 작성하는 방법론입니다.

일반적으로 ‘Red-Green-Refactor’ 사이클을 따릅니다.

첫 번째 단계는 Red로, 실패하는 테스트를 작성하는 것입니다.

아직 구현되지 않은 기능에 대한 테스트로, 코드가 이를 통과하지 못하는 상태에서 시작합니다.

두 번째 단계는 Green입니다.

테스트를 통과할 수 있는 최소한의 코드를 작성합니다. 테스트를 통과시키는 것만 목표로 하여 코드를 간결하게 작성합니다.

마지막 단계는 Refactor로, 작성한 코드를 리팩토링하여 가독성이나 성능을 개선합니다.

이때 테스트는 여전히 통과해야 하므로, 리팩토링이 기능에 영향을 미치지 않도록 합니다.

### TDD에는 어떤 장점이 있나요?

1. 디버깅 시간이 단축됩니다. 자동화된 테스트를 통해 오작동하는 영역을 좁혀나갈 수 있습니다.

2. 리팩토링이 용이해집니다. 코드가 올바르게 동작하는지 확인해 줍니다.

3. 좋은 설계가 유도됩니다. 테스트를 통해 요구사항을 명확하게 이해하며, 이를 바탕으로 정교한 설게가 가능합니다.

---

## Node와 Element의 차이에 대해 설명해주세요.

Node는 DOM을 구성하는 가장 기본적인 구성 단위입니다.

"Document Node"는 HTML 문서 전체를 나타내는 루트 노드이며, "Element Node"는 HTML 태그를 나타내고, "Text Node"는 텍스트 내용을, "Comment Node"는 주석을 나타냅니다.

Node는 DOM 트리의 모든 구성 요소를 포함하는 포괄적인 개념입니다.

Element는 Node의 특정 타입 중 하나로, HTML이나 XML 태그로 표현되는 객체를 의미합니다.

모든 Element는 Node이지만, 모든 Node가 Element인 것은 아닙니다.

Element는 id, class, style과 같은 HTML 속성을 가질 수 있으며, querySelector()나 getElementsByClassName()과 같은 메서드를 사용할 수 있다는 특징이 있습니다.

<div>Hello<!--주석-->World</div>라는 HTML이 있다면, div 태그는 Element Node이면서 동시에 Node입니다. 반면 'Hello'와 'World'라는 텍스트는 Text Node이며, 주석은 Comment Node입니다. 이들은 모두 Node이지만 Element는 아닙니다.

### Node와 Element의 차이와 관련된 구체적인 예시를 들어주세요.

textContent라는 속성은 Node의 속성이므로 모든 종류의 Node에서 사용할 수 있지만, innerHTML은 Element의 속성이므로 Element에서만 사용할 수 있습니다.

childNodes와 children 속성에도 중요한 차이가 있습니다. Node의 속성인 childNodes는 주어진 요소의 모든 자식 Node를 포함하는 NodeList를 반환합니다. 여기에는 Element뿐만 아니라 모든 종류의 Node가 포함됩니다. 따라서 HTML 태그뿐 아니라 텍스트, 주석도 childNodes에 포함됩니다. 반면 Element의 속성인 children은, Element 타입의 자식 노드만을 포함하는 HTMLCollection을 반환합니다. 여기에는 텍스트 노드나 주석 노드는 제외되고 HTML 요소 노드만 포함됩니다.

---

## Redux와 Context API의 차이점과 각각의 장단점을 설명해주세요.

Context API는 React에 내장된 기능으로, 주로 컴포넌트 트리 안에서 데이터를 공유하기 위한 목적으로 설계되었습니다.

props drilling 문제를 해결하기 위한 간단한 솔루션을 제공하는 것이 주 목적입니다.
Provider로 감싼 하위 컴포넌트들은 Consumer를 통해 해당 데이터에 직접 접근할 수 있으며, 별도의 상태 관리 패턴이나 규칙을 따르지 않아도 됩니다.

Redux는 전체 애플리케이션의 상태를 관리하기 위한 독립적인 상태 관리 라이브러리입니다.

Redux는 단방향 데이터 흐름과 불변성을 강조하며, 액션(Action)을 디스패치하고 리듀서(Reducer)를 통해 상태를 변경하는 엄격한 패턴을 따릅니다.
모든 상태 변화는 예측 가능하고 추적 가능해야 한다는 철학을 가지고 있으며, 이를 위해 미들웨어와 같은 추가적인 기능들을 제공합니다.

Context API는 간단한 상태 공유에는 적합하지만, 복잡한 상태 로직이나 빈번한 업데이트가 필요한 경우에는 성능 이슈가 발생할 수 있습니다.
상태 변화를 추적하거나 디버깅하기 위한 도구가 기본적으로 제공되지 않습니다.

Redux는 강력한 개발자 도구와 미들웨어 시스템을 통해 복잡한 상태 관리와 디버깅을 용이하게 만들어주지만, 이를 위해 더 많은 보일러플레이트 코드와 학습이 필요합니다.

Context API는 간단한 전역 상태 관리나 정적인 데이터 공유에 적합하고, Redux는 복잡한 상태 로직과 빈번한 업데이트가 필요한 대규모 애플리케이션에 더 적합합니다.

---

## HTML 데이터 속성(data-)은 무엇인가요?

데이터 속성은 사용자 정의 데이터를 HTML 요소에 저장하기 위해 사용되는 속성입니다.

선언 방법은 data-로 시작하는 속성을 HTML 태그에 추가하면 됩니다.

<div data-user-id="12345" data-role="admin"></div>와 같이 사용할 수 있습니다.

data-user-id와 data-role이 데이터 속성에 해당합니다.

자바스크립트를 통해 데이터 속성에 접근하려면 dataset 객체를 활용합니다.

중요한 점은, HTML의 데이터 속성 이름이 JS의 camelCase 형식으로 매핑된다는 것입니다.

data-user-id는 dataset.userId로, data-role은 dataset.role로 접근할 수 있습니다.

CSS에서도 데이터 속성을 활용할 수 있습니다. attr() 함수나 속성 선택자를 통해 데이터 속성의 값을 기반으로 스타일을 적용할 수 있습니다.

### 데이터 속성은 언제 활용하나요?

DOM 요소에 특정 데이터를 바인딩하고, 자바스크립트 로직에서 해당 데이터를 활용하기 위해 사용됩니다.

버튼 클릭 이벤트에서 특정 데이터를 전달하거나, 데이터를 기반으로 UI를 동적으로 변경해야 할 때 유용합니다.

HTML과 자바스크립트 간 데이터 상호작용을 간단하게 구현할 수 있습니다.

---

## 네트워크 통신에서 Body(Payload)와 Header의 차이는 무엇인가요?

Body와 Header의 가장 큰 차이는 정보(데이터)의 역할입니다.

Header는 데이터의 메타 정보를 담습니다.
데이터 자체가 아니라 데이터에 대한 컨텍스트 정보를 포함합니다. 이로써 수신자가 데이터를 어떻게 처리해야 할지 지침을 제공하는 역할을 합니다.
예를 들어, HTTP 요청이나 응답에서 Header에는 Content-Type, Authorization, Cache-Control과 같은 정보가 포함됩니다. 이는 정보의 유형, 인증 정보, 캐시 설정 등 컨텍스트 정보를 전달합니다.

Body는 전송하려는 실제 데이터를 의미합니다.
HTTP 요청에서 서버로 전달하는 JSON 데이터나 폼 데이터가 이에 해당됩니다. 일반적으로 헤더에 비해 복잡하고 용량이 큰 데이터를 포함합니다.

### Header 크기에 제한이 있나요?

Header의 명시적인 크기 제한은 정해져 있지 않습니다.

다만, Apache, Nginx와 같은 웹서버 단에서 Header의 크기를 제한하고 있는 경우가 많습니다.

일반적으로 8 ~ 16KB입니다.

제한값을 초과할 경우, 일반적으로 응답코드 413(Content Too Large)를 응답합니다.

---

## Storybook을 사용할 때 기대할 수 있는 장점에 대해 설명해 주세요.

Storybook은 비즈니스 로직과 맥락으로부터 분리된 UI 컴포넌트를 만들 수 있도록 도와주는 툴입니다.

컴포넌트는 사용자 인터페이스 코드를 구성, 재사용 및 테스트할 수 있으므로 인터페이스를 쉽게 구축할 수 있는 방법을 제공하지만 대규모 프로젝트에서 작업할 때는 애플리케이션 내의 모든 컴포넌트를 관리하기가 어려울 수 있습니다.

더 빠르고 효율적인 개발이 가능합니다.
개발자가 UI 구성 요소를 개별적으로 빌드하고 테스트할 수 있으므로 애플리케이션의 나머지 부분에 영향을 주지 않고 각 구성 요소에 대해 개별적으로 작업할 수 있습니다.
개발 속도가 빨라질 뿐만 아니라 각 컴포넌트가 의도한 대로 작동하여 문제를 쉽게 식별하고 수정할 수 있습니다.

컴포넌트 관리에 용이합니다.
개발자는 각 컴포넌트의 시각적 표현과 관련 코드, 관련 문서 또는 주석을 볼 수 있습니다.
각 컴포넌트의 목적과 기능을 쉽게 이해하고 필요에 따라 수정할 수 있습니다.

디자이너와 개발자 간의 협업 촉진도가 높아집니다.
각 컴포넌트를 시각적으로 표현하여 디자이너와 개발자 간의 격차를 해소하는 데 도움이 될 수 있습니다.
디자인 사양을 충족하고 최종 제품이 의도한 대로 보이고 작동하는지 확인할 수 있으며, 개발자는 디자인이 제대로 구현되었는지 확인하고 일관성을 유지하며 오류의 위험을 줄일 수 있습니다.

---

## 테스트 코드를 작성하면서 개발할 때 장점과 단점에 대해 설명해 주세요.

테스트 코드는 소프트웨어의 기능과 동작을 테스트하는 데 사용되는 코드입니다.
일반적으로 개발자는 단위 테스트, 통합 테스트와 관련된 테스트 코드를 주로 다룹니다.

### 단위 테스트란?

단위 테스트는 소프트웨어 개발에서 일반적으로 사용되는 테스트 중 하나로, 개별적인 코드 단위가 의도한 대로 작동하는지 확인하는 과정입니다. 단위 테스트의 주요 목표는 개발 초기 단계에서 제품의 각 단위 또는 모듈을 테스트하여 오류가 다음 단계로 이전되는 것을 방지하는 것입니다.

“단위”는 제품의 성능을 개선하기 위해 테스트할 수 있는 가장 작고 간단한 부분입니다. 메서드, 프로시저, 객체 또는 모듈이 될 수 있습니다.

소프트웨어 개발 주기(SDLC)의 초기 단계에서 잠재적인 결함을 발견하고 수정하는 것은 필수적입니다. 따라서 모든 애자일 소프트웨어 개발 프로세스에서 필수적인 부분입니다.

### 통합 테스트란?

통합 테스트는 서로 다른 모듈들 간의 상호작용을 테스트 하는 과정입니다.

통합 테스트는 보통 모듈 간 인터페이스 테스트, 시스템 레벨 테스트 등의 방법으로 수행됩니다.

모듈 간의 상호작용을 테스트하기 때문에 각기 다른 모듈의 설정 방법을 알아야 하고, 테스트에 영향을 끼치는 요인이 데이터와 로직뿐만 아니라 통신구간이나 해당 모듈의 환경설정 정보 등이 더 많습니다.

통합 테스트를 수행할 때는 더 많은 리소스와 시간이 필요하며, 오류를 발견하고 수정하는데 보다 많은 노력이 필요합니다. 그러나 통합 테스트를 수행함으로써, 전체적인 소프트웨어 시스템의 신뢰성과 안정성을 높일 수 있습니다.

### 테스트 코드의 장점

1. 소프트웨어 제품에서 발생할 수 있는 모든 결함을 초기 단계에서 노출하여 다음 단계로 넘어가기 전에 수정 및 제거할 수 있습니다.

2. 코드 가독성 및 품질 향상

3. 문서화 간소화 및 코드 재사용 가능

4. 이후 프로젝트 단계로 넘어갈 수 있는 오류를 신속하게 발견하여 시간과 비용 절약

5. 배포 속도를 개선하여 프로젝트 완료 및 디버깅 시간 단축

### 테스트 코드의 단점

1. 새로운 기능을 도입하고 구현할 때 발생하는 구현 비용

2. 기본 코드가 빠르게 변경되는 프로토타입 개발에 방해가 됩니다.

3. 테스트에서 해당 기능을 사용하므로 IDE에서 해당 기능이 사용 중인지 확인하기가 더 어려워짐

4. 상호 의존성이 있는 테스트는 개별 코드베이스가 변경될 때 결과에 영향을 미칠 수 있습니다.

---

## HTTP란 무엇인지 설명해 주세요.

HTTP(Hypertext Transfer Protocol) 는 웹 상에서 클라이언트와 서버 간 데이터를 주고받는 데 사용되는 통신 규약입니다.

클라이언트가 서버에 요청을 보내고, 서버가 이에 대한 응답을 반환하는 방식으로 동작합니다.
HTTP는 비연결성(stateless) 을 특징으로 하여 한 번의 요청-응답이 끝나면 연결이 종료됩니다. 또한, 통신이 안전하게 연결될 수 있도록 TCP 연결을 사용합니다.

HTTP는 HTML, JSON 등 다양한 데이터 포맷을 전달할 수 있습니다. 요청과 응답에는 URL 경로, 각종 메서드, 상태 코드와 헤더 등 정해진 몇 가지 정보를 포함합니다.

HTTP의 보안을 강화한 버전인 HTTPS(Hypertext Transfer Protocol Secure) 는 HTTP에 TLS/SSL 프로토콜에 따라 데이터를 암호화하여 전송합니다. 이를 통해 보안 상 중요한 정보들을 안전하게 보호하여 통신을 주고 받습니다.

### HTTP와 함께 자주 언급되는 RESTFul API

RESTful API는 REST(Representational State Transfer) 스타일을 준수하여 설계된 API를 의미합니다. 여기서 REST는 웹의 리소스를 클라이언트와 서버가 일관된 방식으로 처리할 수 있도록 하는 설계 원칙입니다.

기본적으로 REST에서는 리소스를 고유한 URI로 표현하고, HTTP 메서드(GET, POST, PUT, DELETE 등)를 사용해 행위를 표현합니다. 예를 들어, /users URI에 GET 요청을 보내면 사용자 목록을 가져오는 API로 동작할 수 있습니다.

### REST 핵심 규칙

클라이언트-서버 분리: 클라이언트와 서버 간 역할을 명확히 분리합니다.
무상태성(Stateless): 서버는 클라이언트의 상태를 저장하지 않으며, 각 요청은 독립적으로 처리합니다.
일관된 인터페이스(Uniform Interface): 고유한 URI로 리소스를 식별하고 일관된 인터페이스를 통해 클라이언트와 서버가 간단하고 예측 가능하게 통신할 수 있게 합니다.
캐시 가능성: 가능하다면, 서버의 응답 시간을 개선하기 위해 리소스 캐싱을 지원합니다.

---

## 네트워크 요청 최적화 방법에 대해 설명해주세요.

네트워크 요청을 최적화하는 것은 웹 애플리케이션의 성능을 향상시키는 핵심 요소입니다.

기본적으로 캐싱 전략을 활용해 동일한 데이터에 대한 중복 요청을 방지하고, 브라우저의 캐시 메커니즘을 최대한 활용합니다.

HTTP 캐시 헤더를 적절히 설정하여 브라우저가 캐시된 리소스를 효율적으로 사용할 수 있게 하며, Service Worker를 통해 오프라인 상태에서도 애플리케이션이 동작할 수 있도록 구현할 수 있습니다.

데이터 전송량을 최소화하기 위해 gzip이나 Brotli 같은 압축 알고리즘을 사용하며, CDN을 활용하여 사용자와 가까운 서버에서 리소스를 제공함으로써 지연 시간을 줄입니다.

API 요청의 경우, GraphQL을 사용하여 필요한 데이터만 선택적으로 가져오거나, REST API에서 필요한 필드만 지정하여 요청하는 방식을 사용할 수 있습니다.

### 구체적인 네트워크 요청 최적화 전략들을 설명해주세요.

첫째, 데이터 캐싱 전략입니다.

브라우저의 HTTP 캐시를 활용하여 정적 리소스를 효율적으로 관리하고, 메모리 캐시를 구현하여 자주 사용되는 데이터를 빠르게 접근할 수 있게 합니다.

Service Worker를 사용하면 오프라인 상태에서도 애플리케이션이 동작할 수 있으며, localStorage나 sessionStorage를 활용하여 클라이언트 측에서 데이터를 저장하고 관리할 수 있습니다.

둘째, 데이터 전송을 최적화합니다.

HTTP/2를 활용하여 여러 요청을 동시에 처리하고, 데이터 압축을 통해 전송량을 줄입니다.
CDN을 사용하여 사용자와 가까운 위치에서 리소스를 제공하며, 이미지 최적화를 통해 불필요한 데이터 전송을 방지합니다.
특히 이미지의 경우 WebP 형식을 사용하거나 적절한 크기로 리사이징하여 제공합니다.

셋째, 요청 제어 기법을 구현합니다.

사용자 입력에 따른 연속적인 API 요청을 제어하기 위해 디바운싱이나 쓰로틀링을 적용하고, AbortController를 사용하여 불필요한 요청을 취소합니다.
여러 개의 작은 요청을 하나로 병합하여 네트워크 부하를 줄이고, 필요한 경우 요청의 우선순위를 조정하여 중요한 데이터를 먼저 로드합니다.

---

## CORS(Cross-Origin Resource Sharing)에 대해 설명해주세요.

CORS는 다른 출처의 리소스를 공유하는 것을 제어하는 보안 메커니즘입니다.

여기서 '다른 출처'란 프로토콜, 도메인, 포트 중 하나라도 다른 경우를 말하는데요.
예를 들어, 프론트엔드는 localhost:3000에서 실행되고 백엔드 API는 localhost:8080에서 실행될 때, 이 둘은 서로 다른 출처가 됩니다.
이때 브라우저는 기본적으로 보안상의 이유로 이러한 요청을 차단합니다.

### CORS가 왜 필요한가요?

CORS는 웹 보안을 위해 필요합니다.

악의적인 사이트가 사용자의 브라우저를 통해 다른 사이트의 리소스에 마음대로 접근하는 것을 방지하기 위해서죠.

이는 사용자 데이터를 보호하고 웹의 안전성을 유지하는 데 중요한 역할을 합니다.

### CORS 관련 이슈는 어떻게 해결하나요?

CORS 이슈를 해결하는 방법은 크게 서버 측과 클라이언트 측 해결방법이 있습니다.

서버 측에서는, Access-Control-Allow-Origin 헤더를 설정하여 특정 출처를 허용할 수 있습니다.
필요한 경우 Access-Control-Allow-Methods, Access-Control-Allow-Headers 등도 설정합니다.
인증이 필요한 요청의 경우 Access-Control-Allow-Credentials도 true로 설정해야 합니다.

클라이언트 측에서는, 개발 환경에서는 프록시 서버를 설정하여 우회할 수 있습니다.
axios나 fetch 사용 시 credentials 옵션을 적절히 설정합니다.

물론 이러한 설정은 보안을 고려하여 필요한 범위 내에서만 제한적으로 이루어져야 합니다. 무분별한 CORS 허용은 보안 취약점이 될 수 있기 때문입니다.

---

## 프로토타입 상속의 동작 방식에 대해 설명해주세요.

프로토타입은 자바스크립트에서 객체 간의 상속을 구현하는 메커니즘입니다.

자바스크립트의 모든 객체는 기본적으로 [[Prototype]]이라는 숨김 프로퍼티를 가지고 있으며, 이 프로퍼티는 다른 객체를 참조하거나 null 값을 가집니다.
프로토타입 연결은 Object.create()나 함수 생성자의 prototype 프로퍼티를 통해 이루어집니다.

프로토타입 상속이 동작하는 방식은 프로토타입 체인을 기반으로 합니다.

객체에서 어떤 프로퍼티를 접근하려고 할 때, 자바스크립트 엔진은 해당 객체에서 프로퍼티를 찾습니다.
만약 찾을 수 없다면, 객체의 [[Prototype]]이 가리키는 프로토타입 객체에서 프로퍼티를 탐색합니다.

만약 프로토타입 객체에서도 해당 프로퍼티를 찾지 못하면, 그 다음에는 프로토타입의 프로토타입을 탐색합니다.
탐색 과정을 계속 반복하면서 결국 원하는 프로퍼티를 찾거나, 프로토타입이 null이 되는 단계에 도달할 때까지 프로토타입 체인을 타고 올라가는 방식으로 탐색합니다.

프로토타입이 꼬리에 꼬리를 물고 연결된 형태를 두고 프로토타입 체인이라고 부르는 것입니다.

### 예시

```
// 1) Object.create()를 이용한 방식
const dog = {
  greet() {
    console.log('Hello from dog!');
  }
};

const maru = Object.create(dog); // maru의 프로토타입이 dog로 설정됨
maru.greet(); // "Hello from dog!" 출력
```

```
// 2) prototype 프로퍼티를 이용한 방식
function Dog() {}
Dog.prototype.greet = function () {
  console.log('Hello from Dog!');
};

const maru = new Dog(); // maru의 프로토타입이 dog로 설정됨
maru.greet(); // "Hello from Dog!" 출력
```

객체 maru가 dog를 프로토타입으로 갖는다고 가정해봅시다.

만약 maru.greet()을 호출했을 시 maru에 greet()이 없으면 프로토타입인 dog에 greet()이 존재하는지 탐색합니다.
이때 dog에 greet()이 존재하면 탐색을 멈추고 해당 메서드를 호출합니다.
만약 dog에도 존재하지 않는다면 프로토타입 체인의 끝에 도달할 때까지 상위 프로토타입을 계속 탐색해 나갑니다.

---

## 알고 있는 이미지 포맷과 각 포맷별 특징을 알려주세요.

JPEG, PNG, WebP, SVG

JPEG는 Joint Photographic Experts Group의 줄임말로, 손실 압축 방식을 사용하는 이미지 포맷입니다.

파일 크기를 효율적으로 줄일 수 있다는 장점이 있습니다.
이미지를 저장할 때마다 데이터가 손실되어 화질이 점점 나빠지는 현상이 발생한다는 단점이 있습니다.
이미지를 여러 번 편집하고 저장하는 과정을 반복하면 이미지 품질이 현저히 저하될 수 있습니다.
사진과 같이 복잡한 색상과 그라데이션이 있는 이미지에 적합한 이미지 포맷입니다.

PNG는 Portable Network Graphics의 줄임말로, 무손실 압축 방식을 사용하는 이미지 포맷입니다. PNG는 투명도를 지원하며, JPEG보다 파일 크기가 큰 편입니다.
회사 로고나 아이콘과 같이 선명한 텍스트나 투명 색상이 있는 이미지에 적합합니다.

WebP는 Google이 웹 환경을 위해 개발한 최신 이미지 포맷입니다.
손실/무손실 압축을 모두 지원하며 투명도도 지원합니다. JPEG나 PNG보다 더 나은 압축률을 제공하면서도 화질을 유지할 수 있어 웹 최적화에 적합합니다.
다만, WebP는 비교적 최신 포맷이기 때문에 지원하지 않는 브라우저가 존재합니다.
따라서 호환성 문제에 유의해야 합니다.

SVG는 Scalable Vector Graphics의 줄임말로, 벡터 기반의 이미지 포맷입니다.
SVG는 XML 형식으로 이루어져 있습니다.
크기를 조절해도 이미지 품질 저하가 없고 파일 크기가 작다는 장점이 있습니다.
이미지의 형태가 복잡할 경우 XML 기반의 코드가 매우 길어져 파일 크기가 커지고 연산이 무거워진다는 단점이 있습니다. 주로 로고, 아이콘, 단순한 일러스트레이션에 사용됩니다.
