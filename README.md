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

