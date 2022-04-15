# Typescript 기본 & 기본 타입
## 타입사용하기

```js
// case 1. 타입을 제대로 잘 적었을 때
function add(num1, num2) {
  return num1 + num2;
}

const number1 = 5;
const number2 = 10;

const result = add(number1, number2);
console.log(result); // 15

// case 2. 타입을 잘 못 입력했을 때 
function add(num1, num2) {
  return num1 + num2;
}

const number1 = '5';
const number2 = 10;

const result = add(number1, number2) // 15
console.log(result) // '510'
```
우리는 위에 있는 코드를 짜면서 첫번째 경우처럼 당연히 number1이 숫자일 것이라고 생각한다.
하지만 코드를 짜는 우리가 실수 할 수 있고, 사용자의 입력에서 잘못된 타입이 입력될 수 도 있고, 
코드를 타고타고 짜다 타입을 변환했다는 사실을 잊어버릴수도 있고, 몇가지 스크립트를 공동 작업하는 도중 다른 개발자에 의해 일어날 수 도 있다.

그래서 위에 있는 에러를 미리 방지하기 위해 function add의 num1, num2에 type을 정하게 해줌으로써 에러를 막아줄 수 있다. 

```ts
function add(num1: number, num2: number) {
  return num1 + num2;
}

const number1 = 5;
const number2 = 10;

const result = add(number1, number2);
console.log(result); // 15

```
만약 위 코드에서 number1 이 '5'으로 입력해게 된다면 typescript는 컴파일되기 전에 에러메세지를 보여주게 된다. 
기본적으로 타입스크립트는 컴파일을 차단하지 않고 실수에 대해 알려주며 지적한다. 


## typescript vs javascript 
그렇다면 자바스크립트에서 함수안에 타입을 체크해 주도록 하면 되지 않나 라고 생각할 수 있다. 예를들어 typeof 연산자를 사용해서 아래와 같이 체크할 수 있다. 
```js 
function add(num1, num2) {
  if (typeof num1 !== 'number' || typeof num2 !== 'number') {
    throw new Error('Incorrect input!');
  }
  return num1 + num2;
}

const number1 = '5';
const number2 = 10;

const result = add(number1, number2) // 15
console.log(result) // error
```

여기서 타입과 관련하여 자바스크립트와 타입스크립트의 차이점을 확인할 수 있다. 

  **자바스크립트는 동적 타입이다.** 이는 즉, 나중에 문자열을 할당할 때 처음에 숫자형을 잡아둘 수 있는 변수가 있더라도 전혀 문제 없다는 것을 의미한다. 그래서 특정 타입에 의존하는 코드가 있는 경우, 런타임에서 무언가의 현재의 타입을 확인할 수 있게 해주는 typeof 연산자를 사용하는 거다. 런타임시 에러를 확인 할 수 있다.

  **반면 타입스크립트는 정적 타입으로,** 이는 즉 개발 도중에 끝나는 변수와 매개변수의 타입을 정의한다는 것을 의미한다. 런타임 중에 갑자기 변경되지는 않는다. 물론 타입스크립트는 자바스크립트로 컴파일 되기 때문에 이론적으로는 그럴 수 있지만 타입스크립트를 사용하여 갑자기 새로운 유형의 데이터를 변수(예를 들어, 우리가 앞서 숫자형이어야 한다고 설정했던 변수)에 할당하는 코드를 작성하고 문자열을 할당하면 개발 도중에 에러가 발생하므로 어떤 타입을 보유할 수 있는지 여부를 명확히 할 수 밖에 없다.

## 숫자 및 불리언 타입 

```ts
function(num1: number, num2: number, showResult: boolean, phase: string) {
  const result = num1 + num2;
  if(showResult) {
    console.log(showResult + result);
  } else {
    return result;
  }
}
```
함수안의 매개변수에서 ':'를 사용해 boolean과 string을 지정할 수 있다. 

## 타입할당과 타입추론

  위의 예시에서 우리는 변수할당할때가 아닌 함수안의 매개변수에 타입을 지정해 주었는데, 그렇다면 타입할당할때에는 아래와 같이 하면 된다. 
```ts
const num: number = 1;
const text: string = 'tsts';
const bool: boolean = true; // or false
```
  위와 같이 타입을 할당해 줄 수 있다. 하지만 이렇게 뒤에 *:타입*을 지정해 줄 필요가 없다. 타입추론(type inference)이라는 내장 기능을 활용하기 때문이다.
```ts
const num = 1;
```
  예를들어 위와같이 num에 1이라는 number type를 할당하면 number type도 같이 num이라는 변수에 할당되는 것을 알 수 있다. 
```ts
let num: number;
num = 1;

let num2: number;
num2 = '1';
```
 위와같은 경우에는 첫번째에는 잘 돌아가지만 두번째 예시에서는 타입 경고 메세지를 띄운다. 왜냐하면 num2에는 number type이 할당되었기 때문에 number 타입으로만 사용해야한다. 
