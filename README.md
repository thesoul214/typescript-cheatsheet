<p align="center">
  <img src="https://github.com/thesoul214/typescript-cheatsheet/blob/master/lib/attached_images/TypeScript.png" alt="TypeScript"/>
</p>

# Table of contents

- [Types](#types)
- [Dom](#dom)
- [Claases](#classes)
- [Generics](#generics)
- [Type Narrowing(타입 좁히기)](#narrowing)
- [Declarations(타입 선언)](#declarations)
- [Modules](#modules)
- [webpack](#webpack)

## Types

### String, Number, Boolean

```ts
const myName: string = "Robert";
const myAge: number = 24;
const bHasHobbies: boolean = true;
```

### Array

> 타입을 직접적으로 지정하는 걸 권장

```ts
const hobbies: string[] = ["Programming", "Cooking"];
const numbers: number[] = [1, 3.22, 6, -1]
// const numbers: Array<number> 배열의 다른 선언 방식

// 다차원 배열
const board: string[][] = [
  ["X", "O", "X"],
  ["X", "O", "X"],
  ["X", "O", "X"],
];

const numbers = [1, 3.22, 6, -1] // 자동으로 number[] 타입으로 인식된다. 
```

### Tuples

> Tuple은 길이와 타입이 고정된 배열

```ts
const address: [string, number] = ["Street", 99];
```

### Any

```ts
let myCar: any = "BMW";
console.log(myCar); // Prints: BMW

myCar = { brand: "BMW", series: 3 };
console.log(myCar) // Prints: { brand: "BMW", series: 3 }
```

### Object

> key, value로 구성된다.

기본 구성
```ts
const dog = {
  name: "Elton",
  breed: "Australian Shepehrd",
  age: 0.5
}
```

흔히 type과 같이 사용한다.
```ts
type Point = {
  x: number;
  y: number;
  z?: number; // optional key
  readonly dimension: number; // read only key
};

const myPoint1: Point = { x: 1, y: 3, dimension: 2 };
const myPoint2: Point = { x: 1, y: 3, z: 6, dimension: 3 };
const myPoint3: Point = { x: 1, y: 3, dimension: 2 };

console.log(myPoint3.dimension): // OK
myPoint3.dimension = 3; // Error
```

nested object
```ts
type Song = {
  title: string;
  artist: string;
  numStreams: number;
  credits: { producer: string; writer: string };
};

const mySong: Song = {
  title: "Unchained Melody",
  artist: "Righteous Brothers",
  numStreams: 12873321,
  credits: {
    producer: "Phil Spector",
    writer: "Alex North",
  },
};
```

교차 타입(intersection)
```ts
type Circle = {
  radius: number;
};

type Colorful = {
  color: string;
};

type ColorfulCircle = Circle & Colorful;

const happyFace: ColorfulCircle = {
  radius: 4, // Circle type
  color: "yellow", // Colorful type
};

// 새로운 타입을 인라인으로 정의(age)
type Cat = {
  numLives: number;
};

type Dog = {
  breed: string;
};

type CatDog = Cat &
  Dog & {
    age: number;
  };

const christy: CatDog = {
  numLives: 7,
  breed: "Husky",
  age: 9,
};
```

### Union

> 하나의 변수에 복수의 타입을 선언할 수 있다.

```ts
let age: number | string = 21;
age = 23;
age = "24";

// 타입과 함께 쓰는 것도 가능
type Point = {
  x: number;
  y: number;
};

type Loc = {
  lat: number;
  long: number;
};

let coordinates: Point | Loc = { x: 1, y: 34 };
coordinates = { lat: 321.213, long: 23.334 };

// 리터럴 타입과 함께 쓰는 것도 가능
type DayOfWeek =
  | "Monday"
  | "Tuesday"
  | "Wednesday"
  | "Thursday"
  | "Friday"
  | "Saturday"
  | "Sunday";

let today: DayOfWeek = "Sunday";
today = "Everyday"; // Error
```

### Enums

```ts
enum Color {
  Gray, // 0
  Red, // 1
  Green = 100, // 100
  Blue, // 101
  Yellow = 2 // 2
}

const myColor: Color = Color.Green
console.log(myColor); // Prints: 100

// String Enum:
enum ArrowKeys {
  UP = "up",
  DOWN = "down",
  LEFT = "left",
  RIGHT = "right",
}
```

### Functions

```ts
// 파라미터 type annotation
function greet(person: string, age: number) {
  return `Hello, ${person}! I am ${age} years old`;
}
greet("Kim", 35)

// 파라미터에 기본값 설정
function greet(person: string = "kim", age: number = 35) {
  return `Hello, ${person}! I am ${age} years old`;
}
greet();
greet("Kim");
greet("Kim", 40);
```

다른 선언 방식
```ts
// Arrow function:
const add = (x: number, y: number): number => {
  return x + y;
};

// Contextual Type Clues
const colors = ["red", "orange", "yellow"];
let new_color = colors.map((color) => {
  return color.toUpperCase();
});

console.log(new_color) // prints ["RED", "ORANGE", "YELLOW"] 
```

리턴 타입

```ts
// 리턴 타입 설정
function greet(person: string = "stranger"): string {
  return `Hi there, ${person}!`;
}

// Void 리턴 타입 : 함수의 리턴값이 없는 경우 사용
function printTwice(msg: string): void {
  console.log(msg);
  console.log(msg);
}

// Never 리턴 타입 : 함수를 호출한 곳에 리턴값을 돌려주지 않는 경우 사용
function makeError(msg: string): never {
  throw new Error(msg);
}
```

### Interfaces

> Type과 거의 비슷한 문법으로 구성된다.

```ts
// type Point = {
//   x: number,
//   y: number
// }
// 위의 type과 거의 비슷한 문법(=을 사용하지 않음)
interface Point {
  x: number;
  y: number;
}
const pt: Point = { x: 123, y: 1234 };
```

인터페이스내에 메소드 정의하기
```ts
// 파라미터는 받지 않고 string을 리턴하는 메소드(sayHi)
interface Person {
  readonly id: number;
  first: string;
  last: string;
  nickname?: string;
  sayHi(): string;
  // 메소드는 아래의 문법으로도 정의 가능
  // sayHi: () => string;
}

const thomas: Person = {
  first: "Thomas",
  last: "Hardy",
  nickname: "Tom",
  id: 21837,
  sayHi: () => {
    return "Hello!";
  },
};

// number를 파라미터로 받고 number를 리턴하는 메소드(applyDiscount)
interface Product {
  name: string;
  price: number;
  applyDiscount(discount: number): number;
}

const shoes: Product = {
  name: "Blue Suede Shoes",
  price: 100,
  // 파라미터명을 discount로 하지 않아도 된다.
  applyDiscount(amount: number) {
    // this.price는 위에 정의한 price: 100을 가리킨다.
    const newPrice = this.price * (1 - amount);
    this.price = newPrice;
    return this.price;
  },
};

console.log(shoes.applyDiscount(0.4)); // prints 60
```

> 인터페이스내 특징 1 : 다시 열기
* type의 경우 같은 기능을 교차 타입(intersection) 으로 구현가능하다
```ts
// Re-open an interface
interface Dog {
  name: string;
  age: number;
}

// 위에서 선언한 인터페이스를 다시 선언해도 새로 정의되거나 덮어 씌워지지 않는다.
interface Dog {
  breed: string;
  bark(): string;
}

const elton: Dog = {
  name: "Elton",
  age: 0.5,
  breed: "Australian Shepherd",
  bark() {
    return "WOOF WOOF!";
  },
};
```

> 인터페이스내 특징 2 : 확장(extends)
```ts
// 기존 dog의 속성 + 새로운 속성(job)을 정의
interface ServiceDog extends Dog {
  job: "drug sniffer" | "bomb" | "guide dog";
}

const chewy: ServiceDog = {
  name: "Chewy",
  age: 4.5,
  breed: "Lab",
  bark() {
    return "Bark!";
  },
  job: "guide dog"
};

// 다중 확장도 가능하다
interface Human {
  name: string;
}

interface Employee {
  readonly id: number;
  email: string;
}

// Extending multiple interfaces
interface Engineer extends Human, Employee {
  level: string;
  languages: string[];
}

const pierre: Engineer = {
  name: "Pierre",
  id: 123897,
  email: "pierre@gmail.com",
  level: "senior",
  languages: ["JS", "Python"]
};
```

## Dom

### null 처리
```ts
// getElementById은 HtmlElement | null을 반환한다.
const btn = document.getElementById("btn");

// btn이 null이 될 수 있으므로 ts에서 밑의 코드는 에러가 발생한다.
btn.addEventListener("click", function () {
  console.log("clicked!!!");
});

// ?를 추가하여 btn이 존재하지 않는 경우 실행되지 않도록 처리
btn?.addEventListener("click", function () {
  console.log("clicked!!!");
});
```

### non-null 단언 연산자(!)
> ts의 기능으로, 값이 확실하게 존재하고 null이 아닌 경우에만 사용해야 함
```ts
// !를 추가하여 절대 null이 되지 않는다고 ts에 약속한다.
const btn = document.getElementById("btn")!;

btn.addEventListener("click", function () {
  console.log("clicked!!!");
});
```

### type assertion 타입 단언
> 어떤 타입인지 구체적으로 ts에게 알려주고 싶을 때 사용한다.
```ts
// HtmlElement는 value라는 프로퍼티를 가지고 있지 않으므로 에러가 발생한다.
const input1 = document.getElementById("myInput")!;
input1.value = "aaaa";

// 단순한 HtmlElement이 아닌 HTMLInputElement 이라고 선언한다. 
// HTMLInputElement는 value라는 프로퍼티를 가지므로 에러가 발생하지 않는다.
const input = document.getElementById("myInput")! as HTMLInputElement;
input.value = "aaa";
```

## Classes

### public, private 제어자

public 제어자 : 클래스 내부, 외부에서 접근할 수 있는 프로퍼티나 메소드를 선언할 때 사용한다.

private 제어자 : 정의된 클래스 내부에서만 접근할 수 있는 프로퍼티나 메소드를 선언할 때 사용한다.

```ts
class Player {
  public readonly first: string;
  public last: string;
  private score: number = 0;

  constructor(first: string, last: string){
    this.first = first;
    this.last = last;
  }

  private secretMethod(): void {
    console.log("secretMethod");
  }
}

const player1 = new Player("Elton", "Steele");

console.log(player1.first); // prints Elton
player1.first = "kim"; // readonly이므로 에러!!

console.log(player1.last); // prints Steele
player1.last = "kim"; // 에러 발생하지 않음

player1.score; // score는 private 프로퍼티로 클래스 외부에서는 접근 불가하므로 에러!!
player1.secretMethod(); // secretMethod는 private 메소드로 클래스 외부에서는 접근 불가하므로 에러!!
```

> 위의 예제를 단축 구문을 이용하여 간단하게 구현하는 것이 가능하다.
```ts
class Player {
  constructor(
    public readonly first: string,
    public last: string,
    private score: number
  ){}

  private secretMethod(): void {
    console.log("secretMethod");
  }
}

// public, private 프로퍼티를 초기화
const player1 = new Player("Elton", "Steele", 0);
```

### getter, setter
```ts
class Player {
  constructor(
    public readonly first: string,
    public last: string,
    private _score: number
  ){}

  get fullName(): string {
    return `${this.first} ${this.last}`;
  }

  get score(): number {
    return this._score;
  }

  // setter에는 반환 타입을 지정할 필요가 없다.
  set score(newScore: number) {
    if (newScore < 0) {
      throw new Error("Score cannot be negative!");
    }
    this._score = newScore;
  }
}

const player1 = new Player("Elton", "Steele", 100);
console.log(player1.fullName); // prints Elton Steele

// setter를 이용하여 private 프로퍼티인 _socre값을 갱신
player1.score = 99;
player1.score = "99"; // 타입 에러
```

### protected 제어자

정의된 클래스와 그로부터 상속한 모든 클래스에서 사용할 수 있는 프로퍼티나 메소드를 선언할 때 사용한다.
```ts
class Player {
  constructor(
    public first: string,
    public last: string,
    private _private_score: number,
    protected _protected_score: number
  ) {}
}

class SuperPlayer extends Player {
  public isAdmin: boolean = true;
  maxScore() {
    // private 프로퍼티는 자식 클래스에서 사용할 수 없으므로 에러 발생!
    this._private_score = 99999999;

    // protected 프로퍼티는 자식 클래스에서 사용가능
    this._protected_score = 99999999;
  }
}
```

### Static 제어자

프로퍼티나 메소드를 개별 인스턴스가 아닌 해당 클래스 자체에 존재하게 하는 방식

```ts
class Helpers {
  static PI: number = 3.14;
  static calcCircunferance(diameter: number): number {
    return this.PI * diameter;
  }
}
console.log(Helpers.PI);
console.log(2 * Helpers.PI);
console.log(Helpers.calcCircunferance(10));

// 인스턴스 h로는 static 프로퍼티에 접근할 수 없다.
const h = new Helpers();
h.PI
```

### Interfaces, Abstract Class

Interface
```ts
interface Colorful {
  color: string;
}

interface Printable {
  print(): void;
}

// interface를 이용해서 class를 확장
class Bike implements Colorful {
  // Colorful Interface의 프로퍼티를 선언해야 한다.
  constructor(public color: string) {}
}

class Jacket implements Colorful, Printable {
  constructor(public brand: string, public color: string) {}

  // Printable Interface에 정의된 print 메소드를 무조건 시행해야 한다.
  print() {
    console.log(`${this.color} ${this.brand} jacket`);
  }
}

const bike1 = new Bike("red");
const jacket1 = new Jacket("Prada", "black");
```

Abstract Class : 직접적으로 인스턴스화는 하지 못하지만, abstract 클래스는 패턴을 정의하고 상속하는 모든 자식 클래스에서 시행해야 하는 메소드를 정의하는 데 사용한다.
```ts
abstract class Employee {
  constructor(public first: string, public last: string) {}
  abstract getPay(): number;
  greet() {
    console.log("HELLO!");
  }
}

class FullTimeEmployee extends Employee {
  constructor(first: string, last: string, private salary: number) {
    // Employee 클래스의 프로퍼티를 지정한다.
    super(first, last);
  }

  // getPay 메소드는 Abstract이므로 자식 클래스에서 무조건 시행되어야 한다.
  getPay(): number {
    return this.salary;
  }
}

class PartTimeEmployee extends Employee {
  constructor(
    first: string,
    last: string,
    private hourlyRate: number,
    private hoursWorked: number
  ) {
    super(first, last);
  }

  // getPay 메소드는 Abstract이므로 자식 클래스에서 무조건 시행되어야 한다.
  getPay(): number {
    return this.hourlyRate * this.hoursWorked;
  }
}

const betty = new FullTimeEmployee("Betty", "White", 95000);
console.log(betty.getPay());

const bill = new PartTimeEmployee("Bill", "Billerson", 24, 1100);
console.log(bill.getPay());
```

## Generics

> 여러 타입에서 사용할 수 있는 함수나 클래스를 정의할 수 있게 해주는 특수 기능

```ts
// 제네릭이 없는 경우, 다양한 타입의 파라미터를 지원하는 함수를 복수개 정의해야 한다.
function numberIdentity(item: number): number {
  return item;
}
function stringIdentity(item: string): string {
  return item;
}
function booleanIdentity(item: boolean): boolean {
  return item;
}

// 제네릭을 이용하면 하나의 함수만 정의해도 된다.
function identity<T>(item: T): T {
  return item;
}

console.log(identity<number>(7)); // prints 7
console.log(identity<string>("hello")); // prints hello

// 또 다른 예제
function getRandomElement<T>(list: T[]): T {
  const randIdx = Math.floor(Math.random() * list.length);
  return list[randIdx];
}

getRandomElement<string>(["a", "b", "c"]);
getRandomElement<number>([5, 6, 21, 354, 567, 234, 654]);

// <number>라고 적어주지 않아도 ts가 자동으로 T가 number인 것을 추론할 수 있다.
getRandomElement([1, 2, 3, 4]);
```

### 타입 제한하기 (extends 키워드)

> extends 키워드를 이용하여 제네릭의 타입을 제한해준다. 

```ts
// T와 U가 항상 객체이어야 한다.
function merge<T extends object, U extends object>(object1: T, object2: U) {
  return {
    ...object1,
    ...object2,
  };
}

// 또 다른 예제
interface Lengthy {
  length: number;
}

function printDoubleLength<T extends Lengthy>(thing: T): number {
  return thing.length * 2;
}

printDoubleLength("asdasd");
printDoubleLength(234); // 숫자 234에는 length함수나 프로퍼티가 존재하지 않으므로 에러 발생
```

### 파라미터의 기본 타입 지정
```ts
function makeEmptyArray<T = number>(): T[] {
  return [];
}

const nums = makeEmptyArray(); // 아무것도 지정하지 않았으므로 T는 number가 된다.
const bools = makeEmptyArray<boolean>();
```

### 제네릭 클래스
```ts
interface Song {
  title: string;
  artist: string;
}

interface Video {
  title: string;
  creator: string;
  resolution: string;
}

class Playlist<T> {
  public queue: T[] = [];
  add(el: T) {
    this.queue.push(el);
  }
}

const songs = new Playlist<Song>();
const videos = new Playlist<Video>();

// songs의 queue에는 Song타입의 객체가 추가되어야 하므로 에러 발생
songs.add({title: "title", creator: "kim", resolution: "1280"});

// videos queue에 Video타입의 객체가 추가되었으므로 정상동작
videos.add({title: "title", creator: "kim", resolution: "1280"});
```

## Narrowing

> 명확하지 않은 타입(주로 유니온 타입)을 보다 명확하게 타입을 지정하기 위해(좁이기 위해) 사용


### in 연산자 사용
```ts
interface Movie {
  title: string;
  duration: number;
}

interface TVShow {
  title: string;
  numEpisodes: number;
  episodeDuration: number;
}

function getRuntime(media: Movie | TVShow) {
  if ("numEpisodes" in media) {
    // media는 numEpisodes 프로퍼티를 가지는 TVShow 타입으로 인식된다.
    return media.numEpisodes * media.episodeDuration;
  }

  // media는 Movie타입으로 인식된다.
  return media.duration;
}

console.log(getRuntime({ title: "Amadeus", duration: 140 })); // prints 140
console.log(getRuntime({ title: "Spongebob", numEpisodes: 80, episodeDuration: 30 })); // prints 2400
```

### instanceof 연산자 사용

> instanceof 연산자란, 객체가 특정 클래스(또는 해당 클래스를 상속하고 있는 클래스)가 인스턴스화 된 것인지 확인하는 자바스크립트 연산자

> {객체} instanceof {클래스}

```ts
function printFullDate(date: string | Date) {
  if (date instanceof Date) {
    console.log(date.toUTCString());
  } else {
    console.log(new Date(date).toUTCString());
  }
}

// 또 다른 예제
class User {
  constructor(public username: string) {}
}
class Company {
  constructor(public name: string) {}
}

function printName(entity: User | Company) {
  if (entity instanceof User) {
    return "user";
  }

  return "company";
}

const user1 = new User("kim")
// user1은 User 클래스가 인스턴스화 된 것이므로 user를 출력한다.
printName(user1)
```

### 타입 명제(Type Predicates)

> 타입을 판단하는 함수, {parameter name} is {type}의 형태

```ts
interface Cat {
  name: string;
  numLives: number;
}
interface Dog {
  name: string;
  breed: string;
}

// animal is Cat -> 만약 이 함수가 true를 반환한다면 파라미터 animal은 cat타입이다. 라는 의미
function isCat(animal: Cat | Dog): animal is Cat {
  return (animal as Cat).numLives !== undefined;
}

function makeNoise(animal: Cat | Dog): string {
  if (isCat(animal)) {
    // 타입스크립트는 animal을 Cat타입이라고 간주한다.
    animal;
    return "Meow";
  } else {
    // 타입스크립트는 animal을 Dog타입이라고 간주한다.
    animal;
    return "Woof!";
  }
}
```

### 소진검사(exhaustiveness check)와 never

> case문 등에서 모든 경우를 제대로 처리하지 않았음을 타입스크립트가 알려주도록 하는 방법

```ts
interface Rooster {
  name: string;
  weight: number;
  age: number;
  kind: "rooster";
}

interface Cow {
  name: string;
  weight: number;
  age: number;
  kind: "cow";
}

interface Pig {
  name: string;
  weight: number;
  age: number;
  kind: "pig";
}

interface Sheep {
  name: string;
  weight: number;
  age: number;
  kind: "sheep";
}

type FarmAnimal = Pig | Rooster | Cow | Sheep;

function getFarmAnimalSound(animal: FarmAnimal) {
  switch (animal.kind) {
    case "pig":
      return "Oink!";
    case "cow":
      return "Moooo!";
    case "rooster":
      return "Cockadoodledoo!";
    default:
      // never 타입에는 어떤 값도 할당할 수 없으므로 에러가 발생한다.
      // 해당 예제에서는 case문으로 Sheep타입을 제대로 처리하지 않았으므로 에러 발생
      const _exhaustiveCheck: never = animal;
      return _exhaustiveCheck;
  }
}
```

## Declarations

> 타입 선언 파일은 TypeScript 생태계에서 제3자 JavaScript 라이브러리를 사용하기 위해 필요한 파일이다.

> *.d.ts의 파일 형식을 지니며, 오직 타입 정보만을 포함하고 있다.

### 제3자 라이브러리가 타입 선언 파일을 기본적으로 제공하는 경우(예: axios)

`npm install axios`로 라이브러리를 설치하면 `import axios from "axios";` 만으로 바로 사용하는 것이 가능하다.

axios의 타입 선언 파일은 `node_modules/axios/index.d.ts`에 존재한다.

### 제3자 라이브러리가 타입 선언 파일을 제공하지 않는 경우(예: lodash)

`npm install lodash`로 라이브러리를 설치해도 타입 선언 파일이 존재하지 않기 때문에 바로 사용할 수 없다.

이런 경우에는 `npm install --save-dev @types/lodash` 명령어를 실행하여 lodash라이브러리의 타입 선언 파일을 다운로드 해야 한다.

`@types/lodash`란, 타입 선언 파일을 모아둔 레포지토리 (https://github.com/DefinitelyTyped/DefinitelyTyped) 에서 lodash 라이브러리의 타입 선언 파일을 다운로드 한다는 의미이다.

다운로드 한 타입 선언 파일은 `node_modules/@types/lodash/common/collection.d.ts`에 존재한다.

## Modules

> 파일간 코드를 공유하는 방법(export, import)

### export

export를 추가함으로써 다른 파일에서 사용 가능하도록 간단하게 내보낼 수 있다.

```ts
// 파일 A
export function add(x: number, y: number) {
  return x + y;
}

export function sample<T>(arr: T[]): T {
  const idx = Math.floor(Math.random() * arr.length);
  return arr[idx];
}

export const pi = 3.14;
```

```ts
// 파일 B
// {} 안에 변수를 입력하는 방법을 기명 가져오기 라고 한다.
import { add, sample as randomSample, pi } from "./파일 A";
```

### export default

> 기본 내보내기 기능으로, 파일이나 모듈마다 하나만 정의할 수 있다.

> 기명 가져오기를 할 필요가 없다. 

```ts
// 파일 A
export default class User implements Person {
  constructor(public username: string, public email: string) {}
  logout() {
    console.log(`${this.username} logs out!!`);
  }
}

export function userHelper() {
  console.log("USER HELPER!");
}
```

```ts
// 파일 B
// class User는 export default로 선언되었으므로 기명 가져오기를 하지 않아도 된다.
import User, { userHelper } from "./파일 A";
```

## Webpack

<p align="center">
  <img src="https://github.com/thesoul214/typescript-cheatsheet/blob/master/lib/attached_images/webpack.png" alt="webpack"/>
</p>

webpack은 의존성을 가진 수십, 수백개의 파일(에셋)을 브라우저에 넣을 수 있는 작은 단위로 번들링 해주는 도구.

다양한 에셋(js, sass, png, svg등)을 모아 정적 에셋으로 번들링한다. 

> 번들링이란 종속성이 존재하는 여러 개의 파일을 하나의 파일로 묶어주는 것

### 의존성 설치 
ts-loader는 typescript와 webpack사이의 중간자 역할을 한다. 

typescript를 호출해서 javascript로 컴파일링 한 후, 이를 모두 번들링하게 될 webpack으로 전달하는 역할


### 설정파일(webpack.config.js)

```js
module.exports = {
  entry:"./src/app.ts",
  output: {
      filename:"./bundle.js"
  },
  resolve: {
      extensions: ["*",".ts",".tsx",".js"]
  },
  module:{
      rules: [
          {test:/\.tsx?$/, loader:"ts-loader"}
      ]
  }
};
```

entry: webpack에게 번들링을 시작할 어플리케이션의 시작점을 지정

module: .ts 혹은 .tsx로 끝나는 파일이 있을 시, ts-loader를 사용하라는 규칙을 지정

resolve: 확장자 생략 설정

output: webpack으로 생성하려는 출력에 관한 설정

### 소스맵
압축된 번들 코드를 디버깅하고 이해하는데 도움을 준다.

컴파일 하기 전의 ts파일을 구조 그대로 브라우저에서 디버깅 할 수 있도록 해준다.

- tsconfig.json에서 sourceMap을 true로 설정
- webpack.config.js에서 devtool을 inline-source-map으로 설정
  - 해당 소스 맵을 추출해 최종 번들에 포함하도록 설정

### 개발 서버 설정 예제 (webpack.dev.js)

- webpack.config.js에 mode를 development로 설정
  - webpack으로 번들링된 js파일을 경량화 하지 않는다.
- webpack-dev-server 설치 (npm install --save-dev webpack-dev-server)
  - 개발 과정 동안 번들링을 백그라운드에서 처리하여 매번 별도의 번들 파일로 만들어 dist에 출력하지 않아도 된다.
  - webpack.config.js에 output내에 publicPath를 /dist로 추가

```js
const path = require("path");
module.exports = {
  mode: "development",
  entry: "./src/index.ts",
  devtool: "inline-source-map",
  devServer: {
    static: {
      directory: path.join(__dirname, "./"),
    },
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: "ts-loader",
        exclude: /node_modules/,
      },
    ],
  },
  resolve: {
    extensions: [".tsx", ".ts", ".js"],
  },
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
    publicPath: "/dist",
  },
};
```

### 프로덕션 서버 설정 예제 (webpack.prod.js)

- webpack.config.js에 mode를 production으로 설정
- npm install --save-dev clean-webpack-plugin
  - 빌드할 때마다 출력 폴더가 자동으로 비워지도록 설정

```js
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const path = require("path");
module.exports = {
  mode: "production",
  entry: "./src/index.ts",
  devtool: "inline-source-map",
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: "ts-loader",
        exclude: /node_modules/,
      },
    ],
  },
  resolve: {
    extensions: [".tsx", ".ts", ".js"],
  },
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
    publicPath: "/dist",
  },
  plugins: [new CleanWebpackPlugin()],
};
```













------upto here


## Decorators

### Prologue

Before getting into what decorators are, here is some important information taken from the [TypeScript official documentation about decorators](https://www.typescriptlang.org/docs/handbook/decorators.html):

> With the introduction of Classes in TypeScript and ES6, there now exist certain scenarios that require additional features to support annotating or modifying classes and class members. Decorators provide a way to add both annotations and a meta-programming syntax for class declarations and members. Decorators are a stage 2 proposal for JavaScript and are available as an experimental feature of TypeScript.
> **NOTE:  Decorators are an experimental feature that may change in future releases.**
> To enable experimental support for decorators, you must enable the `experimentalDecorators` compiler option either on the command line or in your `tsconfig.json`:

***Command Line:***

```cli
tsc --target ES5 --experimentalDecorators
```

***tsconfig.json:***

```json
{
    "compilerOptions": {
        "target": "ES5",
        "experimentalDecorators": true
    }
}
```

[⬆️ Back to top](#table-of-contents)<br>

<a id="decorators-introduction"></a>

### Decorators Introduction

> A Decorator is a special kind of declaration that can be attached to a class declaration, method, accessor, property, or parameter. Decorators use the form `@expression`, where **`expression`** must evaluate to a function that will be called at runtime with information about the decorated declaration.

[⬆️ Back to top](#table-of-contents)<br>

### Basic Decorator

Another way to explain a decorator would be to say that they are a way to enhance the functionality of a `class`. We can use decorators to add properties to a decorated class when its constructor is executed, for example. This is of course among many other things that are possible, more of this below.

```ts
  function logged(constructorFn: Function) {
    console.log(constructorFn); // Will log the class Person constructor.
  }

  @logged
  class Person {
    constructor() {
      console.log("Hi!");
    }
  }
```

Every time a new instance of `Person` is built, the **constructor function** will be logged into the console.

[⬆️ Back to top](#table-of-contents)<br>

### Factory

> If we want to customize how a decorator is applied to a declaration, we can write a decorator factory. A Decorator Factory is simply a function that returns the expression that will be called by the decorator at runtime.

In other words, a factory is a functions that returns a decorator.

```ts
  function logged(constructorFn: Function) {
    console.log(constructorFn); // Will log the class Person constructor.
  }

  // Factory
  function logging(value: boolean) {
    return value ? logged : () => {};
  }

  @logging(true) // If true, it will return the `logged` decorator, if false, it will execute an empty function.
  class Car {

  }
```

[⬆️ Back to top](#table-of-contents)<br>

### Advanced Decorator

Since decorators have access to a class constructor function, through the decorators we can access the class `prototype` and add properties to said class. In the example below we will add the `print` method to any `class` decorated by `printable` (to `Plant` in this case). Also notice in the next example how the argument passed to `logging` (same `factory` function as above) is `false`. This means the constructor won"t be printed to the console through the `logged` decorator **(because the `factory` won"t return it)**.

```ts
  function printable(constructorFn: Function) {
    constructorFn.prototype.print = function() {
      console.log(this);
      this.name = "Red Plant"
    }
  }

  @logging(false)
  @printable
  class Plant {
    private name = "Green Plant";
  }

  const plant = new Plant();
  (<any>plant).print(); // Would print the constructed Plant object: { name: "Green Plant" }, then change the name property.
  console.log("After print() is executed", plant); // Prints: the constructed Plant object: { name: "Red Plant" }.
```

[⬆️ Back to top](#table-of-contents)<br>

### Method Decorator

> A Method Decorator is declared just before a method declaration. The decorator is applied to the Property Descriptor for the method, and can be used to observe, modify, or replace a method definition. A method decorator cannot be used in a declaration file, on an overload, or in any other ambient context (such as in a declare class).

Besides adding a decorator to the class, we can also add decorators to a class member method (and properties, parameters - more on this below). It is done in the exact same way.

```ts
  function editable(value: boolean) {
    return function(target:any, propName: string, descriptor: PropertyDescriptor = {}) {
      descriptor.writable = value;
      Object.defineProperty(target, propName, descriptor);
    }
  }
  
  class Project {
    constructor(name: string) {
      this.projectName = name;
    }

    @editable(false)
    calcBudget() {
      console.log(1000);
    }
  }
  
  const project = new Project("Super Project");
  project.calcBudget(); // Prints: 1000
  // project.calcBudget = () => { // Throws warning in the console IF editable is false. Property won"t ever be changed.
  //   console.log(2000);
  // };
```

In the above example, we are modifying the `calcBudget` object"s writable property to avoid it from being changed in the future. If changed then a warning will be thrown in the console. [More information about PropertyDescriptors can be found here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty).

[⬆️ Back to top](#table-of-contents)<br>

### Property Decorator

The good thing about decorators is that they are exactly as their name imply. With the property decorator, we can also change its property during runtime, among other things. Here"s what the official documentation have to say about **property decorators**:

> A Property Decorator is declared just before a property declaration. A property decorator cannot be used in a declaration file, or in any other ambient context (such as in a declare class).

The expression for the property decorator will be called as a function at runtime, with the following two arguments:

1. Either the constructor function of the class for a static member, or the prototype of the class for an instance member.
2. The name of the member.

> **NOTE  A Property Descriptor is not provided as an argument to a property decorator due to how property decorators are initialized in TypeScript. This is because there is currently no mechanism to describe an instance property when defining members of a prototype, and no way to observe or modify the initializer for a property. The return value is ignored too. As such, a property decorator can only be used to observe that a property of a specific name has been declared for a class.**

**That is not to say that a property"s PropertyDescriptor object cannot  be modified.** We may define a new object descriptor and return it, here"s an example of how to make a property with `writable` false:

```ts
  // Method Decorator
  function editable(value: boolean) {
    return function(target:any, propName: string, descriptor: PropertyDescriptor = {}) {
      descriptor.writable = value;
      Object.defineProperty(target, propName, descriptor);
    }
  }

  // Property Decorator
  function overwritable(value: boolean): any {
    return function(target: any, propName: string): any {
      const newDescriptor: PropertyDescriptor = { // New PropertyDescriptor object.
        writable: value
      };
      return newDescriptor;
    }
  }

  class Project {
    @overwritable(false)
    private projectName: string;

    constructor(name: string) {
      this.projectName = name;
    }

    @editable(true)
    calcBudget() {
      console.log(1000);
    }
  }
  
  const project = new Project("Super Project"); // The name won"t be modified because the @overwritable decorator"s argument is false.
  project.calcBudget(); // Prints: 1000
  project.calcBudget = () => { // Throws error in console IF the @editable decorator"s argument is false, cannot assign new property to this.
    console.log(2000);
  };
  project.calcBudget(); // Prints: 2000
  console.log(project) // IF overwritable is false, then projectName will be equal to undefined.
```

[⬆️ Back to top](#table-of-contents)<br>

### Parameter Decorator

Let"s quote the official documentation again:

> A Parameter Decorator is declared just before a parameter declaration. The parameter decorator is applied to the function for a class constructor or method declaration. A parameter decorator cannot be used in a declaration file, an overload, or in any other ambient context (such as in a declare class).

The expression for the parameter decorator will be called as a function at runtime, with the following three arguments:

Either the constructor function of the class for a static member, or the prototype of the class for an instance member.

1. The name of the member.
2. The ordinal index of the parameter in the function’s parameter list.

> **NOTE  A parameter decorator can only be used to observe that a parameter has been declared on a method.**

Here is an example of how we can put that into practice:

```ts
  function printInfo(target: any, methodName: string, paramIndex: number) {
    console.log("Target: ", target);
    console.log("Method name: ", methodName);
    console.log("Param index: ", paramIndex);
  }

  class Course {
    name: string;

    constructor(name: string) {
      this.name = name;
    }

    // @printInfo behind printAll
    printStudentNumbers(mode: string, @printInfo printAll: boolean) {
      if (printAll) {
        console.log(10000);
      } else {
        console.log(2000);
      }
    }
  }

  const course = new Course("TypeScript");
  course.printStudentNumbers("anything", true);
  course.printStudentNumbers("anything", false);

```

So, `@printInfo` will only be called at runtime whenever `printStudentNumbers` is executed. Here is what `@printInfo` would print for both instances:

```js
  Target: > Object {}
  methodName: printStudentNumbers
  paramIndex: 1 // Starts counting at 0. It"s 1 because `printAll` is the second argument of `printStudentNumbers`.
```

[⬆️ Back to top](#table-of-contents)<br>

---

<a id="webpack-workflow"></a>

## Webpack Workflow (v^4.X)

[Here is the official documentation about TypeScript made by the folks at Webpack](https://webpack.js.org/guides/typescript/).

Here"s the TLDR:

1. Install the TypeScript compiler with `npm install --save-dev typescript ts-loader`.

> We use ts-loader in this guide as it makes enabling additional webpack features, such as importing other web assets, a bit easier.

2. Run `tsc --init`, then modify the `tsconfig.json` file to look like this or similar:

```ts
  {
    "compilerOptions": {
      "outDir": "./dist/",
      "noImplicitAny": true,
      "module": "es6",
      "target": "es5",
      "jsx": "react",
      "allowJs": true
    }
  }
```

This will make sure the compiler can parse TypeScript down to ES5.

3. Webpack `webpack.config.js` file configuration to handle TypeScript:

```js
  const path = require("path");

  module.exports = {
    entry:"./src/app.ts",
    output: {
      filename: "bundle.js",
      path: path.resolve(__dirname, "dist")
    },
    devtool:"source-map",
    resolve: {
      extensions: ["*",".ts",".tsx",".js"]
    },
    module:{
      rules: [
        { test:/\.tsx?$/, loader:"ts-loader" }
      ]
    }
  };
```

> This will direct webpack to enter through ./index.ts, load all .ts and .tsx files through the ts-loader, and output a bundle.js file in our current directory.

[⬆️ Back to top](#table-of-contents)<br>

---

## TypeScript with React.js

[Here is the official documentation about using TypeScript along with React.js](https://www.typescriptlang.org/docs/handbook/react-&-webpack.html).

In this section though, I"ll focus on how to use TypeScript while using `create-react-app`, and how to set general Prop types like `className`, `ref`, `style`, among others.

[⬆️ Back to top](#table-of-contents)<br>

### TypeScript while using `create-react-app`

1. Create the React.js application by running `npx create-react-app project-name --scripts-version=react-scripts-ts`.

This will now also create the react app with all the configuration already set up and ready to go.

[Here is an example React.js app created by running the previous command.](https://github.com/rmolinamir/typescript-cheatsheet/tree/master/lib/react.js)

When using an interface to declare the PropTypes, by convention we must write a capital I when defining an interface. Let"s see an example about how we can use PropTypes together with interfaces, enums, and how to apply them:

***App.tsx***

```tsx
  // ...

  interface IAppProps {[propName]: any}
  interface IAppState {
    counterValue: number;
  }

  class App extends React.Component<IAppProps, IAppState> {
    public state = { counterValue: 0 }; // State is required to be public.

    public render() {
      return (
        <div>
          <p>
            <CounterOutput
              counter={this.state.counterValue}
              onClick={this.counterHandler} />
          </p>
        </div>
      );
    }

    private counterHandler = (mode: ECounterHandlers) => {
      this.setState(prevState => {
        switch(mode) {
          case ECounterHandlers.Dec:
            return { counterValue: prevState.counterValue - 1 }
          case ECounterHandlers.Inc:
            return { counterValue: prevState.counterValue + 1 }
        }
      });
    }
  }

  export default App;
```

***CounterOutput.tsx***

```tsx
  export enum ECounterHandlers {
    Inc,
    Dec
  }

  interface ICounterOutputProps {
    counter: number;
    onClick: (mode: ECounterHandlers) => void;
  }
  const counterOutput = (props: ICounterOutputProps) => {
    return (
      <div>
        <p>{props.counter}</p>
        <button onClick={() => props.onClick(ECounterHandlers.Dec)}>Decrement</button>
        <button onClick={() => props.onClick(ECounterHandlers.Inc)}>Increment</button>
      </div>
    );
  }
```

Notice the following:

1. There"s a capital E when defining the enum. That is a personal thing I picked up while using the Unreal Engine software that is based in `c++` which is similar in many ways to TypeScript, and it also makes sense since it"s inline with the interface naming convention.
2. Passing variables, defining and declaring components and everything else is exactly as you"d expect - with the extra features of TypeScript added on top of it.
3. Remember that class members are `public` by default while using TypeScript, **but, a the state of a class component must always be public**.

It is also possible to modify the `tsconfig.json` or the `tslint.json` to handle highlights in your IDE:

```json
  {
    "extends": ["tslint:recommended", "tslint-react", "tslint-config-prettier"],
    "linterOptions": {
      "exclude": [
        "config/**/*.js",
        "node_modules/**/*.ts",
        "coverage/lcov-report/*.js"
      ]
    },
    "rules": {
      "jsx-no-lambda": false
    }
  }
```

TypeScript by default doesn"t like when lambda functions are used and the IDE will hightlight them as an error. To disable this, add the following rule into your `tslint.json` file: `"jsx-no-lambda": false`.

[⬆️ Back to top](#table-of-contents)<br>

<a id="setting-up-prop-types"></a>

## Setting up general React prop types

Like shown above, prop types are very encouraged in React.js. Some prop types however, may not be as obvious as others, so here"s a small list of the most common prop types you might end up defining while programming your application:

[⬆️ Back to top](#table-of-contents)<br>

<a id="reference-prop-types"></a>

### References (`React.createRef()` or `useRef` API)

Passing down references is a bit tricky, because it **depends on which HTML element type you are referring to**.

React provides the reference objects types out of the box, imported from the **React** default export, from the `react` package (or as a named export as well). The name of the object type is `RefObject`, and it is essentially a [generic interface](#generic-types). Here"s the definition:

```ts
interface React.RefObject<T>
```

As I said earlier, the **RefObject** must be accompanied by a **generic type declaration** (remember the angle brackets typing in the Generics section? e.g. `<T>`), that specifies **which type of HTML element tag the reference belongs to**.

The methodology is the same for every HTML element, but the generic type is respective to which HTML element we are referencing. Here"s an example on how to apply the prop type to an SVG and to a button:

***SVG Element:***

```tsx
  interface ISVGProps {
    reference?: React.RefObject<SVGSVGElement>
    ...
  }

  const SVG = (props: ISVGProps) => {
    return (
      <svg
        ref={props.reference}
        className={classes.Triangle}
        {...props} />
        <path d="M0,0 10,10 20,0" />
      </svg>
    )
  }

  // ...

  <SVG reference={myReference} />
```

***Button Element:***

```tsx
  interface IButtonProps {
    reference?: React.RefObject<HTMLButtonElement>
  }

  const button = (props: IButtonProps) => {
    return (
      <button ref={props.reference} />
    )
  }

  // ...

  <Button reference={props.reference} />
```

[⬆️ Back to top](#table-of-contents)<br>

<a id="children-prop-types"></a>

### React Children

Declaring the children prop types might be a bit confusing as well. The children could be a JSX element, a string, numbers, etc. Fortunately and just like with references, the **React default export object** includes the children prop type declaration. It"s called **`ReactNode`**. The `ReactNode` is [a type alias](#alias), defined as:

```ts
type React.ReactNode = string | number | boolean | {} | React.ReactElement<any, string | ((props: any) => React.ReactElement<any, string | ... | (new (props: any) => React.Component<any, any, any>)> | null) | (new (props: any) => React.Component<...>)> | React.ReactNodeArray | React.ReactPortal | null | undefined
```

Pretty long, right? Good thing it exists. Here is an example of how we can use it, along with the reference prop type:

```tsx
  interface IButtonProps {
    reference?: React.RefObject<HTMLButtonElement>
    children?: React.ReactNode
  }

  const button = (props: IButtonProps) => {
    return (
      <button ref={props.reference}>{props.children}</button>
    )
  }

  // ...

  <Button reference={props.reference}>
    {props.children}
  </Button>
```

[⬆️ Back to top](#table-of-contents)<br>

<a id="css-prop-types"></a>

### CSS Properties and Classes Prop Types

Now, let"s say we want to pass CSS classes and CSS style properties as React props down to our button component. We know that CSS classes, classNames in react, are of type `string`. However, CSS style properties are made of an entirely different type definition.

The type definition can be imported from the **React default export object** just like the previous type declarations. It"s called **`CSSProperties`**, and it is defined as:

```ts
  interface CSSProperties extends CSS.Properties<string | number>
```

Here"s how we can pass CSS classes and style properties down to our component:

```tsx
  interface IButtonProps {
    reference?: React.RefObject<HTMLButtonElement>
    children?: React.ReactNode
    // CSS
    style?: React.CSSProperties
    className?: string
  }

  const button = (props: IButtonProps) => {
    return (
      <button
        style={props.style}
        className={props.className}
        ref={props.reference}>
        {props.children}
      </button>
    )
  }

  // ...

  <Button
      className={classes.Button}
      style={{ backgroundColor: "gold", color: "#FFF" }}
      reference={props.reference}>
    {props.children}
  </Button>
```

[⬆️ Back to top](#table-of-contents)<br>

<a id="general-prop-types"></a>

### General Prop Types (e.g. HTML 5 props, event listeners etc.)

Let"s finish our button. We"ll now create an interface declaring most of the possible HTML-5 properties a button can accept, such as mouse in and out and click event listeners, CSS classes, CSS styles, form properties (form, formaction, formenctype, etc.), value, among others.

***Button.tsx***

```tsx
  interface IButtonProps {
    reference?: React.RefObject<HTMLButtonElement>
    children?: React.ReactNode
    data: IButtonData
  }

  interface IButtonData {
    // HTML Props
    type?: string
    name?: string
    value?: string | number | string[] | undefined
    onClick?: ((event: React.MouseEvent<HTMLButtonElement, MouseEvent>) => void) | undefined
    onMouseEnter?: ((event: React.MouseEvent<HTMLButtonElement, MouseEvent>) => void)
    onMouseLeave?: ((event: React.MouseEvent<HTMLButtonElement, MouseEvent>) => void)
    tabIndex?: number | undefined
    disabled?: boolean
    // HTML 5 Props
    autoFocus?: boolean
    form?: string
    formAction?: string
    formEncType?: string
    formMethod?: string
    formNoValidate?: boolean
    formTarget?: string
    // CSS
    style?: React.CSSProperties
    className?: string
  }

  const data: IButtonData = {...props.data}

  export const button = (props: IButtonProps) => {
    return (
      <button ref={props.reference} {...data}>
        {props.children}
      </button>
    )
  }
```

Let"s now call our button component inside an **App.js** file then pass some props:

***App.js***

```js
  /// ...

  <Button reference={myButton}
    data={
      type: "submit",
      className: classes.Button,
      style: { backgroundColor: "gold", color: "#FFF" },
      tabIndex: -1,
      autoFocus,
      formEncType: "multipart/form-data",
      onClick: onSubmitHandler
      // ...
    }>
    {props.children}
  </Button>
```

For more information about button HTML properties, [give this a read](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button).

The methodology is essentially the same for other components. When in doubt about other HTML properties for any kind of element, you can always consult the [MDN docs](https://developer.mozilla.org/en-US/docs/Web/).

[⬆️ Back to top](#table-of-contents)<br>

<a id="setting-up-react-references"></a>

## Setting up React References (React.createRef() or useRef API)

Setting up a React reference is similar to how we pass down references as props. Let"s set up the reference using the `useRef` hook and the old `React.createRef()`.

[⬆️ Back to top](#table-of-contents)<br>

Let"s start with the `useRef` API first:

<a id="typescript-useref"></a>

### TypeScript & useRef

The general idea is to define the reference with a generic HTML type while initializing them as `null`. This is to let TypeScript know what kind of variable will be stored. Here"s an example using an input element:

```tsx
  const input = () => {
    // Initialize the reference as null while letting TypeScript know it"s an input element.
    const myInput = useRef<HTMLInputElement>(null)

    return (
      <input ref={myInput} type="text" />
    );
  }
```

[⬆️ Back to top](#table-of-contents)<br>

<a id="typescript-reactcreateref"></a>

### TypeScript & React.createRef

Similarly to how we just created a reference using the `useRef` API, let"s now use the `React.createRef` API to set up a reference by initializing it as null and declaring the HTML type:

```tsx
  /// ...

  class Input extends Component {
    constructor() {
      super()
      // Initialize the reference as null while letting TypeScript know it"s an input element.
      this.myInput = React.createRef<HTMLInputElement>(null)
    }

    render() {
      return (
        <input ref={this.myInput} type="text" />
      )
    }
  }
```

[⬆️ Back to top](#table-of-contents)<br>

<a id="setting-up-event-references"></a>

## Setting up Event Handlers

> There are many types of events, some of which use other interfaces based on the main Event interface. Event itself contains the properties and methods which are common to all events.

There are many types of events. Because of this, when accessing an event, we must let TypeScript know what kind of event we are referring to. [Here is a list of all the possible events according to the official MDN docs](https://developer.mozilla.org/en-US/docs/Web/API/Event#Introduction).

Let"s create two event handlers, a `MouseEvent` handler that will prevent the default event, trigger an alert displaying our value, then reset our input"s value on upon clicking a button, and finally, an `InputEvent` handler that will listen to our input element value changes. Let"s expand our previous example:

```tsx
  /// ...
  
  const input = () => {
    const [inputValue, setInputValue] = React.useState("")

    // Initialize the reference as null while letting TypeScript know it"s an input element.
    const myInput = useRef<HTMLInputElement>(null)

    /**
     * Our InputEvent listener.
     */
    const onChangeHandler = (event: React.InputEvent) => {
      const value = event.target.value
      console.log(myInput.current.value)
      setInputValue(value)
    }

    /**
     * Our MouseEvent listener.
     */
    const onClickHandler = (event: React.MouseEvent) => {
      event.preventDefault()
      alert(inputValue)
      setInputValue("")
    }

    return (
      <form>
        <input
          ref={myInput}
          value={inputValue}
          onChange={onChangeHandler}
          type="text" />
        <button onClick={onClickHandler}>
          Click me to reset the input field!
        </button>
      </form>
    );
  }
```

[⬆️ Back to top](#table-of-contents)<br>

## TypeScript and React Hooks

In this section, we will cover how to use the React Hooks implemented in v16.8 in TypeScript. It will be assumed that the reader has at least basic knowledge of Hooks and generics. Generics are **heavily** used alongside Hooks, mostly to set the type(s) of the variables that will be returned from the hooks, e.g., a state variable returned from `useState`. In general, it is only necessary to type a hook if it returns a value, here"s an example with `useState`:

Given `useState` , we can specify the type of the returning value by declaring it in TypeScript as follows:

```tsx
  const [state, setState] = useHook<T>;
```

The variable `state`, will be of type `T`. If we defined `T` as:

```tsx
  type T = number | null;
```

Then `state` and `setState` will be defined as:

```tsx
  const state: number | null;
  const setState: (value: number | null) => void;
```

Or, as the React does it by applying generics for scalability:

```tsx
  type Dispatch<A> = (value: A) => void;
  type SetStateAction<S> = S | ((prevState: S) => S);

  const state: number | null;
  const setState: Dispatch<SetStateAction<number | null>>;
```

If the above seems too complicated, don"t worry about it too much. The reason they declare type aliases such as `Dispatch` is because they are used in *many* places, and as we have covered already, generics are great for that reason.

If your variable **does not** returns a hook, chances are you will still need to type its parameters, e.g. `useEffect`, or even *custom hooks*. We will talk about custom hooks at the end of this section since it is the most complicated topic due to the literal infinite hooks anyone could make, nevertheless, we will try to showcase the typing of custom hooks similar to how React types their own "default" hooks by taking a look at some of their own hooks as examples, with the intention of making them as general and broad as possible.

[⬆️ Back to top](#table-of-contents)<br>

### useState

Arguably the most basic hook. Because of this, let"s discuss one more point about the basics of TypeScript and React Hooks. There are two ways to type a hook function that returns a value, assuming the hook is similar to `useState` where it accepts generic types:

1. By typing the returned value.
2. By typing the hook.

As shown above, `useState` will return an array with two values. The first value of the array will be our  that can be defined as:

By typing the returned value:

```tsx
const [state, setState]: [S: Dispatch<SetStateAction<S>>] = useState(initialState);
```

By typing the hook:

```tsx
const [state, setState] = useState<S>(initialState);
```

Both:

```tsx
const [state, setState]: [S: Dispatch<SetStateAction<S>>] = useState<S>(initialState);
```

Obviously, option number 3 is borderline overkill. Option number 2 is easily the most readable of the three and will be the most commonly typed expression you might come across. Let"s have a look a more practical example of a hero slider component by building up its initial settings:

```tsx
  interface ISettings {
    slidingDuration: number
    isSmartSliding: boolean
    shouldAutoplay: boolean
    width: WidthProperty<string | number>
    height: HeightProperty<string | number>
  }

  /**
   * Initial settings for the slider.
   */
  const initialSettings: ISettings = {
    slidingDuration: 500,
    isSmartSliding: true,
    shouldAutoplay: true,
    autoplayDuration: 8000,
    width: "100%",
    height: "100%",
    ...props.settings
  }

  const [settings, setSettings] = React.useState<ISettings>(initialSettings)

  
  /**
   * Subscribes to any changes made to the settings, then re-sets them through `setSettings`.
   */
  React.useEffect(() => {
    setSettings({
      ...settings,
      ...props.settings as ISettings
    })
  }, [props.settings])
```

The example above is a snippet of [this component](https://github.com/rmolinamir/hero-slider/blob/master/src/Slider/HeroSlider.tsx). The slider is being configured with its initial default settings, or any other settings the developer may have passed down to it. Then, by setting up a subscription to any changes made to the `settings` property, we use the `useEffects` hook that will re-set the settings by executing the dispatched returned from `useState`.

Finally, as you may have already been able to tell; `useState` is defined by the React team as:

```tsx
  function useState<S>(initialState: S | (() => S)): [S, Dispatch<SetStateAction<S>>];
```

[⬆️ Back to top](#table-of-contents)<br>

### useEffect

As stated above, the `useEffects` hook will execute whenever any changes are made to any variable(s) we subscribe to. Although are no typings in `useEffect`, it is worth nothing that TypeScript will function normally from within the scope of an `useEffect` hook, as regular. It is also worth noting that [there is quite a discussion going on about `useEffects` and how actions and dispatchers are handled, I recommend giving it a read, especially the linked comment](https://github.com/facebook/create-react-app/issues/6880#issuecomment-488158024).

In any case, let"s take a look at how `useEffect` is defined:

```tsx
  function useEffect(effect: EffectCallback, deps?: DependencyList): void;

  type EffectCallback = () => (void | (() => void | undefined));
  type DependencyList = ReadonlyArray<any>;

  interface ReadonlyArray<T> {
    readonly length: number;
    toString(): string;
    toLocaleString(): string;
    indexOf(searchElement: T, fromIndex?: number): number;
    ...
  }
```

The point of showcasing how the React team defines their official hooks, is to give the reader an idea of how should custom hooks be typed and defined, and to be aware of the best coding practices of today.

It"s worth nothing that `ReadonlyArray<T>` is actually defined by TypeScript, [and to quote the official documentation about this type definition](https://www.typescriptlang.org/docs/handbook/interfaces.html), **TypeScript comes with a `ReadonlyArray<T>` type that is the same as `Array<T>` with all mutating methods removed**.

[⬆️ Back to top](#table-of-contents)<br>

### useContext

Probably the most powerful hook when it comes to practicality alongside `useReducer`, `useContext` accepts a context returned from `React.createContext` and then returns the current value of that context. Having said that, you might already have guessed how `useContext` is defined, but let"s give it a go anyways:

```tsx
  ...

  type Provider<T> = ProviderExoticComponent<ProviderProps<T>>;
  type Consumer<T> = ExoticComponent<ConsumerProps<T>>;

  interface Context<T> {
      Provider: Provider<T>;
      Consumer: Consumer<T>;
      displayName?: string;
  }

  function useContext<T>(context: Context<T>): T;
```

Generics all over the place! They provide us with a way to build any number of reusable, consistent, and well defined API.

Here is a basic example of `useContext`, which provides CSS styling to buttons that are within the context:

Let"s start by defining the context created by React:

```tsx
  interface IContextProps {
    // CSS Classes
    className?: string
    classNames: IClassNamesByButton
    setGlobalClassName?: (className: string) => void
    setCustomClassname?: (classNameKey:string, className: string) => void
    // CSS Properties
    style?: React.CSSProperties
    setStyle?: (CSSstyles: React.CSSProperties) => void
    // React Children
    children?: React.ReactNode
  }

  /**
   * Button functionality and context.
   */
  const initialContext: IContextProps = {
    className: [classes.Button, classes.Disabled].join(" "),
    classNames: classNamesByButton,
    style: undefined,
  }

  const Context: React.Context<IContextProps> = React.createContext(initialContext)
```

Imported the context and other necessary dependencies, then using `useContext`:

```tsx
// ...
const useContext = { React }
import { Context, capitalizeString } from "./context"
import classes from "./Button.css"

interface IButtonProps extends IButtonData {
  button: string,
  reference?: React.RefObject<HTMLButtonElement>
  children?: React.ReactNode
  blockButton?: boolean
}

const ButtonComponent = (props: IButtonProps) => {
  const context = useContext(Context)
  const { className, classNames } = context

  const buttonClasses: (string | undefined)[] = [className]

  // Classes are picked depending on the `button` prop of type string
  const button: string | undefined = props.button ? capitalizeString(props.button) : undefined

  // Boolean to keep track of matched CSS classes, sort of like a hash.
  let bIsMatch = false

  // Selects a className from the imported context.
  if (button) {
    if (classNames[button]) {
      bIsMatch = true
      buttonClasses.push(classNames[button])
    }
  }

  // In case there were no matches, then set the button class equal to the Default class.
  if (!bIsMatch) {
    buttonClasses.push(classes.Default)
  }

  /**
   * We are omitting the blockButton, children, and reference
   * props from the data button properties.
   */
  const { blockButton, reference, children, ...data } = props

  /**
   * If blockButton is true, then apply the BlockButton class.
   */
  if (blockButton) {
    buttonClasses.push(classes.BlockButton)
  }

  /**
   * Build the button data object that will be passed down to the Button component.
   */
  const buttonData: IButtonData = ButtonData.setData( { ...data }, buttonClasses)

  /**
   * CSS Properties.
   */
  buttonData.style = context.style || props.style

  return (
    <Button
      reference={reference}
      data={buttonData}
    >
      {children}
    </Button>
  )
}

export default ButtonComponent

```

Lets have a look at a more advanced example of `useContext` typing, that composes the multiple contexts argument of a higher-order component, which then passes down the composed contexts as prop to the wrapped component:

```tsx
  interface IContexts {
    [propName: string]: React.Context<any>
  }

  const withContexts = (
      WrappedComponent: React.JSXElementConstructor<any>,
      Contexts: IContexts,
    ) => {
    return (props: any) => {
      const contextKeys: string[] = Object.keys(Contexts)

      /**
       * Reducing the array of context keys, based on the `Contexts`
       * object, into another object with the returned context values
       * from `useContext`.
       */
      const usedContexts = contextKeys.reduce(
        (
          contexts: { [propName: string]: any },
          key: string
        ) => {
          return Object.assign(
            contexts,
            { [key]: useContext<any>(Contexts[key]) }
          )
        },
        {} // Initial value
      )

      const newProps: { [propName: string]: any } = {
        usedContexts,
        ...props
      }

      return (
        <WrappedComponent {...newProps} />
      );
    }
  }
```

[⬆️ Back to top](#table-of-contents)<br>

### useReducer

Perhaps the "primordial" hook, many hooks are based on how `useReducer` works, including some of the oficial React hooks such as `useState`. This, of course, thanks to Redux and its popularity. As you might know, `useReducer` is used mainly whenever you need to tackle at least one of the two following scenarios:

1. You"re handling complex state logic, meaning multiple state values for example.
2. You find yourself in need of multiple action creators for a single state.

Or both! Whatever may be the case, using TypeScript with `useReducer` is not too hard, but it"s certainly a bit tedious because there are many variables involved, assuming that at least the basics of `useReducer` are known. The good news is that every reducer will follow the same steps whenever typing it. Before going into details about how to define types for the reducer, let"s have a look at how the React team defined `useReducer`:

```ts
  function useReducer<R extends Reducer<any, any>>(
    reducer: R,
    initialState: ReducerState<R>,
    initializer?: undefined
  ): [ReducerState<R>, Dispatch<ReducerAction<R>>];
```

The first thing you might notice, is that it is very similar to `useState`. As mentioned above, `useState` is actually a very simplified version of `useReducer`, where you have a dispatcher and a state variable. The types `ReducerState` and `ReducerAction` are also defined as:

```ts
  type Reducer<S, A> = (prevState: S, action: A) => S;
  type ReducerState<R extends Reducer<any, any>> = R extends Reducer<infer S, any> ? S : never;
  type ReducerAction<R extends Reducer<any, any>> = R extends Reducer<any, infer A> ? A : never;
```

The main take of this, is that reducers are functions that take a previous state as a first argument, and an action **(which includes the payload)** as a second argument. This action will let the programmer decide what to do in the reducer. Let"s now compare how your typical `useReducer` looks like in JavaScript to a typical `useReducer` in TypeScript:

**JavaScript**:

```jsx
  const [state, dispatch] = React.useReducer(reducer, initialState)
```

**TypeScript**:

```tsx
  const [state, dispatch] = React.useReducer<React.Reducer<IInputState, IReducerAction>>(reducer, initialState)
```

That"s it! That"s all there is to typing an `useReducer`. However, let"s have a look at a real `useReducer` example that handles an input"s value, validity, and a touched state that becomes `true` when interacted with. This example will cover:

1. TypeScript `interface` definitions for the `state` (and `initialState`), `dispatch` (actions), and `reducer`.
2. A TypeScript `enum` definition to handle a switch statement and its cases inside the `reducer`, similar to reducer stores that are commonly used in Redux and their action types.

Having said all that, to sum up, any and every `useReducer` can be built by following these steps:

1. Define and declare all of the necessary `interface` and `enum` types for our reducer and its variables.
2. Define a reducer, and apply our defined types to it, the reducer will typically look like this:

```tsx
  const reducer = (state: IState, action: IReducerAction) => {
    const { handler, ...newState } = action
    switch (handler) {
      case EActionHandler.TYPE: // Enum member
        return {
          ...state,
          ...newState
        }
      default:
        throw new Error()
    }
  }
```

3. Lastly, define a `initialState` variable if needed, and a `useReducer` hook inside the React component, then it"s good to go. The `useReducer` declaration will almost always look like this:

```tsx
  const [state, dispatch] = React.useReducer<React.Reducer<IState, IReducerAction>>(reducer, initialState)
```

Let"s apply these steps to our example. Let"s begin by typing the necessary `interface` structures for the following variables, and the reducer `enum` for the action types:

```tsx
  /**
   * REDUCER TYPE DEFINITIONS:
   */

  // `reducer` action types.
  enum EOnChangeHandler {
    VALUE,
    VALID,
    TOUCHED,
    STATE
  }

  // `state` and `initialState`.
  interface IInputState {
    identifier?: string
    value?: value
    validationMessage: string
    className?: string
    valueType?: string
    validation?: IInputValidation
    style?: React.CSSProperties
    placeholder?: string
    elementConfig?: IInputConfig
    required?: boolean
    valid?: boolean
    shouldValidate?: boolean
    touched?: boolean
  }

  // Interface for the `validation` property of `IInputState`.
  interface IInputValidation {
    customRules?: ICustomRulesValidation
    required?: boolean
    email?: boolean
    number?: boolean
    minLength?: number
    maxLength?: number
  }

  // For the `dispatch` actions/arguments.
  interface IReducerAction extends IInputState {
    handler: EOnChangeHandler
    valid: boolean
  }
```

Next, let"s apply define a `reducer` function and apply typing. Here"s where the `interface` and `enum` types come in:

```tsx
  const reducer = (state: IInputState, action: IReducerAction) => {
    const { handler, ...newState } = action
    switch (handler) {
      case EOnChangeHandler.STATE:
        return {
          ...state,
          ...newState
        }
      case EOnChangeHandler.TOUCHED:
        state.touched = newState.touched
        return state
      case EOnChangeHandler.VALID:
        state.valid = newState.valid
        return state
      case EOnChangeHandler.VALUE:
        state.value = newState.value
        return state
      default:
        throw new Error()
    }
  }
```

Let"s now define our `initialState` variable, and our `useReducer` hook inside our input component, and then we"ll plug everything up:

```tsx
  const MyInput = withContext(React.memo((props: IInputProps) => {
    /**
     * Input initial state, which dictates how it will behave (validation, validity, required, etc.).
     */
    const initialState: IInputState = {
      identifier: props.identifier || (`${displayName}_${props.placeholder || props.type || "default"}`),
      value: props.value || "",
      validationMessage: "",
      valueType: props.valueType || props.placeholder && props.placeholder.toLowerCase(),
      placeholder: props.placeholder,
      validation: {
        required: props.required || false,
        email: props.type === "email" && true,
        ...props.validation
      },
      required: props.required || false,
      shouldValidate: Boolean(props.validation) || false,
      valid: props.valid || false,
      touched: props.touched || false
    }
    const [state, dispatch] = React.useReducer<React.Reducer<IInputState, IReducerAction>>(reducer, initialState)
  }
```

Finally, let"s plug everything up and see our reducer in action:

```tsx
  /**
   * REDUCER TYPE DEFINITIONS:
   */
  interface IInputState {
    identifier?: string
    value?: value
    validationMessage: string
    className?: string
    valueType?: string
    validation?: IInputValidation
    style?: React.CSSProperties
    placeholder?: string
    elementConfig?: IInputConfig
    required?: boolean
    valid?: boolean
    shouldValidate?: boolean
    touched?: boolean
  }

  // Interface for the `validation` property of `IInputState`
  interface IInputValidation {
    customRules?: ICustomRulesValidation
    required?: boolean
    email?: boolean
    number?: boolean
    minLength?: number
    maxLength?: number
  }

  interface IReducerAction extends IInputState {
    handler: EOnChangeHandler
    valid: boolean
  }

  enum EOnChangeHandler {
    VALUE,
    VALID,
    TOUCHED,
    STATE
  }

  const reducer = (state: IInputState, action: IReducerAction) => {
    const { handler, ...newState } = action
    switch (handler) {
      case EOnChangeHandler.STATE:
        return {
          ...state,
          ...newState
        }
      case EOnChangeHandler.TOUCHED:
        state.touched = newState.touched
        return state
      case EOnChangeHandler.VALID:
        state.valid = newState.valid
        return state
      case EOnChangeHandler.VALUE:
        state.value = newState.value
        return state
      default:
        throw new Error()
    }
  }

  const MyInput = React.memo((props: IInputProps) => {
    /**
     * Input initial state.
     */
    const initialState: IInputState = {
      identifier: props.identifier,
      value: props.value || "",
      validationMessage: "",
      valueType: props.valueType,
      placeholder: props.placeholder,
      validation: {
        required: props.required || false,
        email: props.type === "email" && true,
        ...props.validation
      },
      required: props.required || false,
      shouldValidate: Boolean(props.validation) || false,
      valid: props.valid || false,
      touched: props.touched || false
    }

    const [state, dispatch] = React.useReducer<React.Reducer<IInputState, IReducerAction>>(reducer, initialState)
  
    /**
     * -------------------------------------
     * ----------REDUCER DISPATCHERS--------
     * -------------------------------------
     * `onChangeHandler` handles the input `onChange` event.
     * Evaluates the validity of the value respective to how it"s set up through `checkValidity`.
     * Executes the `props.onChange` callback if it exists after evaluating the value and saving it in the state.
     */
    const onChangeHandler = (event: React.ChangeEvent<HTMLInputElement>) => {
      const value = event.target.value
      const validation = checkValidity(value, state.validation, state.valueType)
      const action: IReducerAction = {
        valid: validation.status,
        validationMessage: validation.message || state.validationMessage,
        handler: EOnChangeHandler.STATE,
        value: value,
        touched: value && value !== "" ? true : false
      }
      dispatch({ ...action })
    }

    /**
     * Subscribe to any changes to the `state.value` and `state.valid` properties.
     * Execute `onChange` if any of them change, which will be handled
     * by the `reducer`.
     */
    React.useEffect(() => {
      if (props.onChange) {
        props.onChange(state.identifier, state.value, state.valid)
      }
    }, [state.value, state.valid])

    /**
     * The input element configuration props.
     */
    const inputProps: IInputElementProps = {
      ...
      className: inputClasses.join(" "),
      elementConfig: props.elementConfig,
      required: props.required || false,
      value: state.value,
      valid: state.valid,
      touched: state.touched,
      shouldValidate: state.shouldValidate,
      onChangeHandler: onChangeHandler,
      style: props.disabled ? { opacity: 0.5, pointerEvents: "none" } : undefined
    }

    // A lot more code needed, but let"s focus on the reducer.

    return (
      <Password {...inputProps} />
    )
  })
```

[⬆️ Back to top](#table-of-contents)<br>

### useCallback

Both `useCallback` and `useMemo` are commonly compared to `shouldComponentUpdate`. This is because both of the previously mentioned hooks return [*memoized*](https://en.wikipedia.org/wiki/Memoization) callbacks and values respectively. Memoization is basically caching the results of expensive functions in case they are executed again, and if so, then the cached values will be returned instead of executing the functions, which in turn will increase performance.

To summarize, they are commonly used to increase performance, which is the reason they"re compared to `shouldComponentUpdate`, and, `useCallback` will be used for functions whereas `useMemo` will be used for values.

`useCallback` is defined as:

```ts
  function useCallback<T extends (...args: any[]) => any>(callback: T, deps: DependencyList): T;
```

As you can see, it"s very simply. Notice the following:

1. It is defined by `T`, which extends to a function that accepts any number of arguments of `any` type, and can return values of `any` types. Basically, any function.
2. As a second argument, it accepts a dependency list. This dependency list is nothing more than a read only array that the hook will subscribe to. If any of them change, the hook will execute. `DependencyList` is defined as:

```ts
  type DependencyList = ReadonlyArray<any>;
```

Remember that `ReadonlyArray` is a native TypeScript type. Moving on, using `useCallback` in TypeScript is very easy since it"s configured to accept `any` types. Here"s a simple yet shallow example of a real-life application of `useCallback`.

The following `useCallback` application will return a function that will be *throttled* in a later hook (which is actually featured in the custom hooks section). This function will execute whenever the user scrolls through the page, and will mix two colors depending on where the user is and depending on which RBG channels colors should be mixed. Finally, the new mixed color is set through the `setBackgroundColor` dispatcher.

```ts
  const onThrottledScrollHandler = useCallback(() => {
    const currentScrollHeight = window.pageYOffset;
    const [colorOne, colorTwo] = colors;
    const mixRatio = currentScrollHeight / scrollHeight;
    const mixedColor = mixColors(
      [colorOne, colorTwo],
      mixRatioNumberToTriple(mixRatio, mixRatioChannels)
    );
    setBackgroundColor(mixedColor);
  }, [backgroundColor, colors, scrollHeigh, mixRatioChannels]);

  const throttled = useThrottle(onThrottledScrollHandler, throttleLimit);
  ...
```

[⬆️ Back to top](#table-of-contents)<br>

### useMemo

Very similar to `useCallback`. As mentioned previously, `useMemo` is commonly compared to `shouldComponentUpdate`.

> This is because both of the previously mentioned hook return [*memoized*](https://en.wikipedia.org/wiki/Memoization) callbacks and values respectively. Memoization is basically caching the results of expensive functions in case they are executed again, and if so, then the cached values will be returned instead of executing the functions, which in turn will increase performance.

Instead of returning a function as `useCallback` does, `useMemo` will return a value. The React team defines `useMemo` as:

```ts
  function useMemo<T>(factory: () => T, deps: DependencyList | undefined): T;
```

Same as every single hook shown before, generics are dominant. `useMemo` will take a *factory* or *create* function that returns a value of type `T` (think of React components, for example), and a dependency list to subscribe to. If any of the values in the list change, the hook will execute again. Otherwise it"ll simply return the cached result!

`useMemo` is a very, very useful hook. It can be implemented in many, many places within any application to improve performances. Here are some very simple examples of `useMemo` used inside [this hero-slider component](https://github.com/rmolinamir/hero-slider/blob/master/src/Slider/HeroSlider.tsx).

There are two worker functions declared as `getChildren` and `setSlides` that can potentially be performance expensive. These functions will filter the `React.Children` props, and filter them to set up the slides of the hero slider according to the previously set settings. By using `useMemo`, we can memoize the initial returned values, so that every time these functions are executed in the future, the memoized values will be returned instead, and thus increasing performance.

```tsx
  const heroSlider = React.memo((props: ISliderProps) => {
    ...

    /**
     * `getChildren` will categorize the `props.children` elements array into the `children` object.
     */
    const getChildren = (): IChildren => {
      const children: IChildren = {
        slidesArray: [],
        navbarsArray: [],
        autoplayButtonsArray: [],
        othersArray: [],
        navDescriptions: []
      }
      React.Children.toArray(props.children).forEach(child => {
        if (typeof child.type === "function" && React.isValidElement(child)) {
          // tslint:disable-next-line:variable-name
          const RFC_Child: React.FunctionComponent = child.type as React.FunctionComponent
          const displayName = RFC_Child.displayName
          switch (displayName) {
            case "hero-slider/slide":
              const props = child.props as ISlideProps
              children.navDescriptions.push(props.navDescription)
              return children.slidesArray.push(child)
            case "hero-slider/nav":
            case "hero-slider/menu-nav":
              return children.navbarsArray.push(child)
            case "hero-slider/autoplay-button":
              return children.autoplayButtonsArray.push(child)
            default:
              return children.othersArray.push(child)
          }
        }
        return children.othersArray.push(child)
      })
      return children
    }

    /**
     * `setSlides` clones the necessary properties for each slide to work.
     */
    const setSlides = () => {
      return React.Children.map(slidesArray, (child, index) => {
        const currentSlide = index + 1
        return (
          React.cloneElement(
            child as React.ReactElement<ISlideProps>,
            {
              isActive: activeSlide === currentSlide,
              isDoneSliding: isDoneSliding,
              slidingAnimation: settings.slidingAnimation
            }
          )
        )
      })
    }

    /**
     * Sets up initial slides array, `useMemo` is used for performance optimization since a loop is
     * ran inside `getChildren`.
     */
    const children: IChildren = React.useMemo(() => {
      return getChildren()
    }, [])

    /**
     * Performance optimization to avoid re-rendering after mouse over captures.
     * Only updates if `activeSlide` or `isDoneSliding` change.
     */
    const slides = React.useMemo(() => {
      return children.slidesArray && setSlides()
    }, [activeSlide, isDoneSliding])

    return (
      <div
        {...}
      >
        {slides}
        {...}
      </div>
    );
  }
```

[⬆️ Back to top](#table-of-contents)<br>

### useRef

`useRef` is a hook commonly used to set up a reference to a DOM node. Unfortunately, most articles about this hook touch upon using `useRef` *only* to setup references to DOM nodes, as just previously mentioned. In reality, `useRef` sets a reference to a mutable object which is then paired to a DOM node, but, there are *a lot* of equally or even more useful applications.

Before talking about different `useRef` applications other than DOM nodes, it is worth mentioning that `useRef` behaves very similarly to pointers. [In computer science, a *pointer* is defined as](https://en.wikipedia.org/wiki/Pointer_(computer_programming)):

> A pointer references a location in memory, and obtaining the value stored at that location is known as dereferencing the pointer. As an analogy, a page number in a book"s index could be considered a pointer to the corresponding page; dereferencing such a pointer would be done by flipping to the page with the given page number and reading the text found on that page.

The most obvious comparison would be C++ pointers. With the references returned by `useRef` we can achieve very similar results, and instead of setting up references for DOM nodes, we can set up references to handlers within a component, for example.

`useRef` is defined by the React team as:

```ts
  function useRef<T>(initialValue: T): MutableRefObject<T>;
```

Very, very straightforward, but what"s important here is to take a look at the `MutableRefObject` interface:

```tsx
  interface MutableRefObject<T> {
    current: T;
  }
```

It is quite literally just an object defined as `{ current: T }`. Where `T`, can be anything. The React team has a very good explanation as to how `useRef` works:

> `useRef` returns a mutable ref object whose `.current` property is initialized to the passed argument (`initialValue`). The returned object will persist for the full lifetime of the component. Note that `useRef()` is useful for more than the `ref` attribute. It’s handy for keeping any mutable value around similar to how you’d use instance fields in classes.

As mentioned above at the introduction of this section, and to emphasize what the React team has to say about `useRef`, we can use this hook to set mutable values that will persist until the component is unmounted.

Instead of providing a generic and simple example about using `useRef` to set a reference to a DOM node that you could find anywhere, let"s look at relatively advanced yet simple applications of `useRef`, by using the previously referenced [hero-slider component](https://github.com/rmolinamir/hero-slider/blob/master/src/Slider/HeroSlider.tsx) in the `useMemo` section as a guide.

In the `hero-slider` component, a couple features are available that use `useRef` to their advantage:

The first feature are watchers that help both the autoplay instance of the hero-slider and the next, previous or specific slide setters to dictate if they should change the slide, or wait until the slider is actually done sliding, and to also make them self-aware of which slide is currently active.

We will use two references to achieve this. `activeSlideWatcher` and `isDoneSlidingWatcher` are a mutable objects that will persist for the full lifetime of the hero-slider component.

- `isDoneSlidingWatcher` will serve as a pointer in case a `nextSlide` or any other event is called from an upper scope, like the autoplay instance.
- `activeSlideWatcher` serves as a pointer to the `activeSlide` so that the auto play instance won"t be out of sync with the current active slide. It is updated during the `useEffect` hooks subscribed to the `activeSlide` state variable whenever the user changes slide:

```tsx
  ...

  const isDoneSlidingWatcher = React.useRef<boolean>(true)
  const activeSlideWatcher = React.useRef<number>(activeSlide)

  const autoplayInstanceRef = React.useRef((React.useMemo(() => {
    return new IntervalTimer(autoplay, settings.autoplayDuration + slidingTimeoutDuration)
  }, [])))
  const autoplayInstance = autoplayInstanceRef.current

  /**
   * Update the respective watchers" current values.
   */
  React.useEffect(() => {
    activeSlideWatcher.current = activeSlide
  }, [activeSlide])
  React.useEffect(() => {
    isDoneSlidingWatcher.current = isDoneSliding
  }, [isDoneSliding])

  ...

  /**
   * `autoplay` is the callback sent to the autoplay instance.
   */
  const autoplay = (): void => {
    const nextSlide = getNextSlide(activeSlideWatcher.current)
    if (settings.isSmartSliding) {
      smartAnimations(nextSlide)
    }
    changeSlide(getNextSlide(activeSlideWatcher.current))
  }

  /**
   * `changeSlide` sets a new slide then executes `onSlidingHandler` to handle the smooth transition and
   * set `isDoneSlidingWatcher.current` (a pointer) as true. While `isDoneSliding` is true, the
   * slides won"t change.
   * The `onBeforeChange` event is executed here.
   */
  const changeSlide = (nextSlide: number): void => {
    if (isDoneSlidingWatcher.current) {
      if (props.onBeforeChange) {
        props.onBeforeChange(activeSlideWatcher.current, nextSlide)
      }
      setActiveSlide(nextSlide)
      onSlidingHandler(nextSlide)
    }
  }

  ...
```

There is another incredible use-case scenario for `useRef`. By using a `useEffect` hook to work similarly to the `componentDidMount` lifecycle method by passing an empty dependencies list, we can run an initial setup for the hero-slider.

In this initial setup, by passing unset `useRef` mutable objects as props down to the hero-slider component, we can set handlers from **within** our component so that these handlers can be used from an upper scope, as next or previous buttons for example.

```tsx
  ...

  /**
   * After mounting, akin to `componentDidMount`.
   */
  React.useEffect(() => {
    activeSlideWatcher.current = activeSlide
    /**
     * Turn on autoplay if `props.shouldAutoplay` is true.
     */
    if (settings.shouldAutoplay) {
      autoplayInstance.start()
    }
    /**
     * Sets up the `nextSlide` and `previousSlide` reference object if they exist.
     */
    if (props.nextSlide) {
      props.nextSlide.current = setNextSlide
    }
    if (props.previousSlide) {
      props.previousSlide.current = setPreviousSlide
    }
    return () => {
      ...
      autoplayInstance.stop()
    }
  }, [])

  ...
```

[Here"s a link showcasing this particular example](https://www.robertmolina.dev/codelab/hero-slider#buttons). Notice how the initial `activeSlideWatcher.current` variable is also set up in here, which will change in the future whenever a slide changes, and the autoplay instance is started here as well (which is yet another reference).

[⬆️ Back to top](#table-of-contents)<br>

### useImperativeHandle

`useImperativeHandle` is **very rarely** used. This hook exposes the instantiated custom value of a React reference to parent components ([more details in `useRef`](#useref)). This means that you can setup a React reference object, customize it in any way possible, e.g. adding methods, then forward it with `React.forwardRef`. If a parent component then creates a reference then "hooks" it to the child component, the parent component will have access to the reference.

Here"s what the React team has to say about this hook:

> `useImperativeHandle` customizes the instance value that is exposed to parent components when using:
    - `ref`. As always, imperative code using refs should be avoided in most cases.
    - `useImperativeHandle` should be used with `React.forwardRef`.

`useImperativeHandle` is defined by the React team as:

```tsx
  function useImperativeHandle<T, R extends T>(
    ref: Ref<T>|undefined,
    init: () => R,
    deps?: DependencyList
  ): void;
```

Where:

1. `ref` is the React `RefObject<T>`.
2. `init` the common hook initialize function as used in other hooks as well.
3. `deps` is the dependency list of variables used in `init` that the hook will subscribe to.

[Here"s an example that uses the react hooks example about `useImperativeHandle` as a guideline](https://reactjs.org/docs/hooks-reference.html#useimperativehandle).

First, let"s define a component that uses `useImperativeHandle` then apply `forwardRef` on it to expose the `ref` object:

```tsx
  interface IInputRefObject {
    exposedFocusMethod (): void
  }

  interface IInputProps {
    [propName: string]: any;
  }

  const Input: React.RefForwardingComponent<IInputRefObject, IInputProps> = (props, ref) => {
    const inputRef = useRef<HTMLInputElement>(null);
    useImperativeHandle(ref, () => ({
      exposedFocusMethod: () => {
        if (inputRef.current) {
          inputRef.current.focus();
          /// Possibly more logic
        };
      }
    }));
    return <input ref={inputRef} {...props} />;
  }
  
  const ImperativeInput = forwardRef(Input);
  export default ImperativeInput;
```

Now let"s use it in a parent component, and access `ref`:

```tsx
  interface IAutofocusProps {
    shouldFocusOnMount: boolean;
    [propName: string]: any;
  }

  const AutofocusedInput = (props: IAutofocusProps) => {
    const { shouldFocusOnMount = true, ...rest } = props;
    const myInputRef = useRef<IInputRefObject>(null);

    useEffect(() => {
      if (shouldFocusOnMount && myInputRef.current) {
        myInputRef.current.exposedFocusMethod();
      }
    }, [shouldFocusOnMount]);

    return <ImperativeInput ref={myInputRef} {...rest} />
  }
```

Notice how the `AutofocusedInput` component focuses on functionality with almost no declarations. The `focus` propety of the `ImperativeInput` component"s DOM node is not directly executed inside `AutofocusedInput`, instead we are executing the exposed method declared as `exposedFocusMethod`, which then executes `focus`. This is why the React team named this hook as `useImperativeHandle`.

> In computer science, imperative programming [is a programming paradigm that uses statements that change a program"s state](https://en.wikipedia.org/wiki/Imperative_programming).

[⬆️ Back to top](#table-of-contents)<br>

### useLayoutEffect

`useLayoutEffect` is very similar to `useEffect`, the difference is that the initial execution of `useLayoutEffect` is a bit delayed, specifically fired in [the same phase as componentDidMount and componentDidUpdate](https://reactjs.org/docs/hooks-reference.html#useimperativehandle).

It is worth noting that this hook is very rarely used. It is necessary for very specific use-case scenarios where the program needs to wait for the initial rendering.

One scenario where the program might need to wait for the initial rendering, is when there are DOM calculations that must be ran before displaying the components. For example, think of a self-aware popover tooltip that renders towards the center of the page depending on where it"s placed at.

Here"s how the React team defines `useLayoutEffect` (which is literally identical to the definition of `useEffect`), and what do they have to say about it:

```tsx
  function useLayoutEffect(effect: EffectCallback, deps?: DependencyList): void;
```

> The signature is identical to `useEffect`, but it fires synchronously after all DOM mutations. **Use this to read layout from the DOM and synchronously re-render. Updates scheduled inside `useLayoutEffect` will be flushed synchronously, before the browser has a chance to paint.**
Prefer the standard `useEffect` when possible to avoid blocking visual updates.
If you’re migrating code from a class component, `useLayoutEffect` fires in the same phase as `componentDidMount` and `componentDidUpdate`.

[⬆️ Back to top](#table-of-contents)<br>

### useDebugValue

[`useDebugValue` can be used to display a label for custom hooks in React DevTools.](https://reactjs.org/docs/hooks-reference.html#useimperativehandle)

As this is exclusively used for custom hooks, it"s recommended by the React team to only use it for hooks that are part of shared libraries.

`useDebugValue` is defined as:

```tsx
  function useDebugValue<T>(value: T, format?: (value: T) => any): void;
```

The `value` argument is what will be displayed next to its respective label in the *Elements* tab of the React DevTools.

Sometimes though, formatting values might be an expensive operation as noted in the official documentation. For this reason the second parameter `format` exists, where it essentially allows you to access formatting methods, such as:

```tsx
useDebugValue(date, date => date.toDateString());
```

Here"s an example showcased in the official documentation about `useDebugValue`:

```tsx
function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  // ...

  // Show a label in DevTools next to this Hook
  // e.g. "FriendStatus: Online"
  useDebugValue(isOnline ? "Online" : "Offline");

  return isOnline;
}
```

The label shown in the React DevTools will be the name of the function without the "use" prefix of hooks. The example above would result in: **"FriendStatus: Online"**.

[⬆️ Back to top](#table-of-contents)<br>

### Custom Hooks

As shown in `useDebugValue`, hooks are expected to be declared with variable names prefixed by "use". In this section, the most important factors to keep in mind when creating a custom hook will be emphasized, however, [definitely check the official documentation for more general information](https://reactjs.org/docs/hooks-custom.html).

> Custom Hooks offer the flexibility of sharing logic that wasn’t possible in React components before. You can write custom Hooks that cover a wide range of use cases like form handling, animation, declarative subscriptions, timers, and probably many more we haven’t considered. What’s more, you can build Hooks that are just as easy to use as React’s built-in features.

There is literally an infinite amount of different custom hooks we can create. For this reason, it is extremely important to be consistent with them. Any custom hooks should be developed in a way that is consistent with the new functional paradigm imposed by the React team as shown in the prepacked React hooks such as `useState`, and `useEffect`.

Here are three pointers worth quoting from the React official documentation about custom hooks:

> **Do I have to name my custom Hooks starting with “use”?** Please do. This convention is very important. Without it, we wouldn’t be able to automatically check for violations of rules of Hooks because we couldn’t tell if a certain function contains calls to Hooks inside of it.
> **Do two components using the same Hook share state?** No. Custom Hooks are a mechanism to reuse stateful logic (such as setting up a subscription and remembering the current value), but every time you use a custom Hook, all state and effects inside of it are fully isolated.

And finaly, paraphrasing the third point:

> **How does a custom Hook get isolated state?** Each call to a Hook gets isolated state. Because we call a custom hook directly, from React’s point of view our component just calls any hooks that might be used inside the custom hook, such as `useState` and `useEffect`. And as we learned earlier, we can call `useState` and `useEffect` many times in one component, and they will be completely independent.

To summarize, be consistent. Do you want to declare a state that involves additional functionality not offered by `useState`? Make sure to return a value, and possibly a dispatcher too. Do you want to handle side-effects based on `useEffect`? Pass a dependency list, or even just a single dispatcher [to pass a minimal amount of dependencies as mentioned here by Dan Abramov](https://github.com/facebook/create-react-app/issues/6880#issuecomment-488158024).

Let"s now do our first two custom hooks!

First, let"s begin by developing a hook that can throttle **any** callback, within a specified and limited (minimum) amount of time between function calls. Basically, a custom hook used to throttle function callbacks. It will be declared as `useThrottled` and it will accept a `callback` as its first parameter, and a `limit` as its second parameter.

- `callback` can and will be *any* callback that is passed to the hook, and it will **only be executed if the amount of time between the current and last function call is higher than `limit`**.
- `limit` will be the minimum time required to wait between callbacks.

```tsx
  import React from "react";
  const { useState } = React;

  export const useThrottle = (
    callback: () => void,
    limit: number
  ): (() => void) => {
    const [callbackTimeoutId, setCallbackTimeoutId] = useState<number>();
    const [lastCallbackRunDate, setLastCallbackRunDate] = useState<number>();
    return () => {
      if (!lastCallbackRunDate) {
        setLastCallbackRunDate(Date.now());
      } else {
        clearTimeout(Number(callbackTimeoutId));
        setCallbackTimeoutId(
          setTimeout(() => {
            if (Date.now() - lastCallbackRunDate >= limit) {
              callback();
              setLastCallbackRunDate(Date.now());
            }
          }, limit - (Date.now() - lastCallbackRunDate))
        );
      }
    };
  };
```

The `callback` parameter will **only** execute if the difference between the current date and the last time the callback fire is higher than the throttle limit.

Next, let"s develop a hook that handles the scroll event callbacks while also throttling them. We will call this hook `useScrollCallback`:

```tsx
  import React from "react";

  // Dependencies
  import { useThrottle } from "../useThrottle";
  const { useState, useCallback, useEffect } = React;

  export const useScrollCallback = (
    minimumOffset: number = 50, // Minimum height offset to actually execute the callback
    throttleLimit: number = 500, // Minimum time between calls in milliseconds
    callback: (scrollPosition: number) => void
  ) => {
    const [scrollPosition, setScrollPosition] = useState<number>(
      window.pageYOffset
    );

    const onThrottledScrollHandler = useCallback(() => {
      const currentScrollHeight = window.pageYOffset;
      setScrollPosition(currentScrollHeight);
      if (currentScrollHeight > minimumOffset) callback(scrollPosition);
    }, [minimumOffset, callback, scrollPosition]);

    const throttled = useThrottle(onThrottledScrollHandler, throttleLimit);

    useEffect(() => {
      window.addEventListener("scroll", throttled);

      // Return clause.
      return () => window.removeEventListener("scroll", throttled);
    }, [throttled]);
  };
```

Throttling is a very important concept to keep in mind. Even more so in this case when using the scroll event listener because of how often it fires. By throttling it, we are increasing performance significantly. Let"s now create our `App` container to put everything together:

```tsx
  // Libraries
  import * as React from "react";
  import { render } from "react-dom";

  // Global styles
  import "./styles.css";

  // Hooks
  import { useScrollCallback } from "./hooks/useOnScrollCallback";

  // Dependencies
  import randomColor from "randomcolor";
  const minimumOffset: number = 0; // Minimum height offset to actually execute the callback
  const throttleLimit: number = 500; // Minimum time between calls in milliseconds

  function App() {
    /**
    * We"re randomly changing the color of the body when scrolling!
    */
    useScrollCallback(minimumOffset, throttleLimit, (scrollPosition: number) => {
      console.log(`I'm currently scrolling at ${scrollPosition}!`);
      document.body.style.backgroundColor = randomColor({
        luminosity: "light",
        format: "hsla" // e.g. "hsla(27, 88.99%, 81.83%, 0.6450211517512798)"
      });
    });
    return (
      <div className="App">
        <div className="fixed">
          <h1>
            Open the console and scroll the page to see the throttling in action!
          </h1>
          <h1>
            Feel free to experiment by changing the throttle limit to see a change in the frequency of the callbacks.
          </h1>
        </div>
        <div className="empty-space" />
      </div>
    );
  }

  const rootElement = document.getElementById("root");
  render(<App />, rootElement);
```

And that"s it! A couple of things are worth pointing out:

- Did you notice how `useThrottled` returns a function similar to `useCallback`, but with extra functionality, yet still following the new paradigm?
- Did you also notice how `useScrollCallback` handles side-effects only, similar to `useEffect` without a dependency list?

The key to creating custom hooks is to keep them as consistent as possible with the new functional mindset the React team is pushing, keep that in mind!

Here"s the example hosted in CodeSandbox of the hooks shown above, with a few more functionalities to play around with them. Feel free to fork it as well.

[![Custom Hooks example](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/custom-hooks-example-68n7l?fontsize=14)  

[⬆️ Back to top](#table-of-contents)<br>

---

## Feedback

This repository. Submit a PR with any change or addition you"d like to see!

And thank you very much for taking the time to do so 💖

[⬆️ Back to top](#table-of-contents)<br>

---

## Collaborators

This is a list of all the awesome collaborators (present or past) and contributors of the TypeScript Cheatsheet:

- [evdama](https://github.com/evdama)
- [KevinKelbie](https://www.reddit.com/user/KevinKelbie)
- [Ical89](https://github.com/Ical89)
- [disco0](https://github.com/disco0)
- [kberg](https://github.com/kberg)
- [Svish](https://github.com/Svish)
- [MaxmaxmaximusAWS](https://github.com/MaxmaxmaximusAWS)
- [nfedyashev](https://github.com/nfedyashev)
- [jakangah](https://github.com/jakangah)
- [tobiasbueschel](https://github.com/tobiasbueschel)
- [ValentinTapiaTorti](https://github.com/ValentinTapiaTorti)
- [thatseanren](https://github.com/thatseanren)

Make sure to buy them a beer if you ever meet one of them 😊

[⬆️ Back to top](#table-of-contents)<br>

---

## Contribute

Contributions are always welcome! Just like before, all you would have to do is submit a PR and I will be reviewing it shortly. If you have any doubts you can let me know at:

- [u/rmolinamir](https://www.reddit.com/user/rmolinamir)
- [rmolinamir@gmail.com](mailto:rmolinamir@gmail.com)

[⬆️ Back to top](#table-of-contents)<br>
