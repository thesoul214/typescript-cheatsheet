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

> 번들링이란 종속성이 존재하는 여러 개의 파일을 하나의 파일로 묶어주는 것

webpack은 의존성을 가진 수십, 수백개의 파일(에셋)을 브라우저에 넣을 수 있는 작은 단위로 번들링 해주는 도구.

다양한 에셋(js, sass, png, svg등)을 모아 정적 에셋으로 번들링한다. 

### 의존성 설치 
ts-loader는 typescript와 webpack사이의 중간자 역할을 한다. 

typescript를 호출해서 javascript로 컴파일링 한 후, 이를 모두 번들링하게 될 webpack으로 전달하는 역할

### 설정파일 예제(webpack.config.js)

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