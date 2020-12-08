# TypeScript
# 객체와 인터페이스

#### object 타입

타입스크립트의 `object` 타입은 `interface`와 `class`의 상위 타입이다. `object`타입으로 선언된 변수는 `number`, `boolean`, `string`타입의 값을 가질 수는 없지만, 다음처럼 속성 이름이 다른 객체를 모두 자유롭게 담아서 마치 any 타입처럼 동작.

```typescript
let o : object = {name: 'KeunSoo', age:27}
o = {first: 1, second: 2}
```



#### interface 타입

하지만 `interface`구문은 변수 o에는 항상 name과 age 속성으로 구성된 객체만 가질 수 있게함. 구성된 객체 중 <u>누락된 객체가 있거나</u>, <u>이상한 객체가 있으면</u> 오류. 또한 어떤 속성은 있어도 되고 없어도 되는 형태도 만들 수 있는데 이러한 속성을 **선택속성**이라함. 물음표로 씀

```typescript
interface Iperson{
    name : string	//필수 속성
    age : number	//필수 속성
    init? : boolean	//선택 속성
}
let good : Iperson = {name: 'KeunSoo', age:27}

let bad1 : Iperson = {name: 'KeunSoo'}	//age속성이 없으므로 오류
let bad2 : Iperson = {age : 27}			//name속성이 없으므로 오류
let bad3 : Iperson = {name: 'KeunSoo', etc: true}	// etc속성이 있으므로 오류
```



#### 익명 interface 타입

이러한 `interface`는 익명으로도 만들 수 있음. 보통 boolean으로 확인으로 하고 함수를 구현할때 사용함

```typescript
let ai:{
    name : string
    age: number
    init? : boolean
} = {name : 'KeunSoo', age: 27}

function printMe(me : {name: string, age: number, init?:boolean}){
    console.log(
    	me.init?
        `${me.name} ${me.age} ${me.etc}` :
        `${me.name} ${me.age}`
    )
}
printMe(ai)
```



# 객체와 클래스

`class`,`private`,`public`, `protected`, `implements`, `extends` 키워드 모두 제공. `class`의 이름 앞에 접근제한자(`public`, `private` 같은거)가 이름 앞에 붙일 수 잇꼬 없으면 모두  public으로 간주

```typescript
class Person1{
    name: string
    age?: number
}

let keunsoo : Person1 = new Person1()
keunsoo.name = 'KeunSoo'; keunsoo.age = 26
console.log(keunsoo)		// Person1 { name: 'KeunSoo', age: 27}
```



#### 생성자(constructor)

`class`는 `constructor`라는 특별한 메서드 포함. 다른 언어와는 다르게 2행 가능. 생성자의 매게변수에 public을 붙이면 해당 매게 변수의 이름을 가진 속성, 즉 여기선 Person2타입이 `class`에 선언된 것처럼 동작함.

```typescript
class Person2{
    constructor(public name, public age?: number){}
}
let keunsoo : Person2 = new Person2('KeunSoo', 27)
console.log(keunsoo)		// Person2 { name: 'KeunSoo', age: 27}
```



#### 인터페이스의 구현

`class`가 `interface`를 구현할 때는 `implements`를 사용한다. `interface`는 이러이러한 속성이 있어야 한다는 규약에 불과할 뿐 물리적으로 해당 속성을 만들지 않아서 `class`몸통에는 반드시 `interface`가 정의하고 있는 속성을 멤버 속성으로 포함해야한다.

```typescript
interface Person3{
    name : string
    age? : number
}
class Person4 implements Person3{
    constructor(public name : string, public age?: number){}
}
let keunsoo : Person4 = new Person4('KeunSoo', 27)
console.log(keunsoo)		// Person2 { name: 'KeunSoo', age: 27}
```



#### 객체의 구조화

```typescript
export interface Iperson{
    name : string
    age : number
}
export interface Icompany{
    name : string
    age : number
}
```

```typescript
import {Iperson, Icompany} from './Iperson_Icompany'

let keunsoo:Iperson = {name: 'Keunsoo', age : 27},
    justin:Iperson = {name: 'Justin', age : 28}

let  apple:Icompany = {name: 'Apple Computer, Inc', age: 43},
    ms : Icompany = {name: 'Microsoft', age: 44}
```





## Typescript 명령어 정리



**npm init --y     package.json**



package.json에 scripts안에
"dev" : "ts-node src", 
"build" : "tsc && node dist"



**npm i -D typescript ts-node**



1. package.json에 devDependencies 항목등록  
2. node_modules 디렉토리 설치
   npm i -D @types/node  node_modules에 @types\node 설치
   tsc --init    tsconfig.json (ts compiler set file) [복사해서 넣으면 쉬움]

터미널-빌드작업실행-tsc:감시



**ts -> js -> 각각 방식 다름**
Web : AMD(asynchronous module definition)
Nodejs : CommonJS



**tsconfig.ts - compilerOptions**

1. module 키
   Web : amd , Nodejs : commonjs
2. moduleResolution 키
   amd : classic, commonjs : node
3. target
   transfile할 js의 version, 대부분 es5
4. baseUrl
   현재 디렉터리 : . 
5. OutDir
   base에서 디렉터리 이름 (ex ./dist)
6. paths
   import문에서 from 부분을 해석할때 찾아야하는 디렉터리 설정
   외부패키지면 node_modules에 찾아야하므로 키값에 node_modules/* 도 포함
7. esModuleInterop
   AMD방식으로만 동작한다는 가정인 라이브러리 존재때문에 (ex Chance) ture로 설정
8. sourceMap
   true이면 .js 말고 .js.map 파일도 만들어짐
   map파일은 변환된 js코드가 ts코드 어디에 해당하는지 알려줌. 주로 디버깅할때 사용
9. downlevelteration
   generator 구문을 동작하려면 반드시 true로
10. nolmplicitAny 
    f(a, b)일 경우 f(a: any, b:any)처럼 암시적으로 any타입 간주. 
    true 일경우 f(a,b)오류 
    false 일경우 f(a,b) 오류아님 



**키워드 let, const**
let 변수이름  // 값이 수시로 변결될수 있음
const 변수이름 = 초깃값 // 절대 변하지않음



let a : any = 1/ture/...
let num : number = 1
let bool : boolean = true
let str : string = 'Hello'
let obj : object = {}
obj = {name: 'Jack', age: 32} // any처럼 작동, name과 age 의 속성이름만 가짐
let u : undefined = undefined



타입추론
let n = 1
let b = true



템플릿 문자열
`${변수 이름}`



let count = 10, message = 'Your count'
let result = `${message} is ${count}`
console.log(result) // Your count is 10



interface Iperson{
  name : string
  age : number
  etc? : boolean
}



interface안쓰고 쓰는 anonymous interface



let ai : { 

 name : string
  age : number
  etc? : boolean
} = {name: 'Jack', age:32}



function printMe(me : {name : string, age : number, etc?: boolean}){
cosole.log(
  me.etc?
      `${me.name} ${me.age} ${me.etc}` :
      `${me.name} ${me.age}
)
}
printMe(ai)





생성자(constructor)
class Person{
  constructor (public name : string, public age? : number) {}
}
let jack : Person = new Person('Jack', 32)
console.log(jack2)     //Person{name: Jack, age: 32}