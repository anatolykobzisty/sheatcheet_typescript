# Шпаргалка по Typescript

- [Преимущества TS](#Преимущества-TS)
- [Базовые типы](#Базовые-типы)
- [Литеральные типы](#Литеральные-типы)
- [Объявление переменных](#Объявление-переменных)
- [Функции](#Функции)
- [Дженерики](#Дженерики)
- [Классы](#Классы)
- [Интерфейсы](#Интерфейсы)
- [Namespace](#Namespace)
- [Декораторы](#Декораторы)
- [Сложные типы](#Сложные-типы)

### Преимущества TS

- Static typing (статическая типизация)
- Type inference (получение типа из значения, которое присвоили в переменную)
- Support IDE (через точку доступные варианты методы и т.д.)
- Strict null checking (строгая проверка на null)
- Interoperability (совместимость с js)

### Базовые типы

- Boolean

```typescript
let isDone: boolean = false;
```

- Number

```typescript
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

- String

```typescript
let color: string = "blue";

// неявное преобразование типов
var fullName = "Bob Bobbington";
var age = 37;
var sentence =
  "Hello, my name is " +
  fullName +
  ".\nI'll be " +
  (age + 1) +
  " years old next month.";
```

- Array

```typescript
let list: number[] = [1, 2, 3];

let anotherList: Array<number> = [1, 2, 3];
```

- Tuple - кортежи. Используются для хранения значений разных типов в некотором массиве.

```typescript
let data: [string, number];
data = ["hello", 10];

let collection: [string, number];
collection = ["hello", 10];
console.log(collection[0].substr(1));
```

- Enum - перечисления. Полезно использовать, когда нужно ограничить область значений, которая сможет принимать переменная, являющееся типом данного enum-а.

```typescript
enum Color {
  Red,
  Green,
  Blue,
}
let c: Color = Color.Green;
console.log(c); // 1

// Числовой enum
enum ColorList {
  Red = 1,
  Green,
  Blue,
}
let myColor: ColorList = ColorList.Green;
console.log(myColor); // 2

enum ColorsData {
  Red = 1,
  Green,
  Blue,
}
let colorName: string = ColorsData[2];
console.log(colorName); // Green

// Строковый enum
enum ColorList {
  Red = 'RED,
  Green = GREEN',
  Blue = 'BLUE',
}

// Гетерогенный enum
enum Decision {
  Yes = 1,
  No = 'NO',
}

// Обратный мапинг - получение строкового значения какого-то из enum-ом
enum Test {
  A
}

let test = Test.A;
let nameA = Test[test] // A

// Констовый enum - экономия ресурсов при компиляции
const enum ConstEnum {
  A,
  B
}

let a = ConstEnum.A; // при компиляции в Js получаем var a = 0;
```

- Any - произвольный тип
- Unknown - тип также неизвестен, как any, но он запрещает любые операции без проверки валидности этой операции

```typescript
let n: any = 1;
n = "string";
n = false;
console.log(false); // false
```

- Void - отсутствие конкретного значения, используется в основном в качестве возвращаемого типа функций

```typescript
function initFunction(): void {
  console.log("Hello");
}

let unusable: void = void 0;
console.log(unusable); // undefined
```

- Undefined - по аналогии с js, указывает, что значение не установлено

```typescript
let n: undefined = undefined;
```

- Null - по аналогии с js, указывает на неопределенное значение

```typescript
let n: null = null;
```

- Never - отсутствие значения и используется в качестве возвращаемого типа функций, которые генерируют или возвращают ошибку

```typescript
let core: never = (() => {
  console.log(true);
  throw new Error("Some Error");
  // return 1;
})();
```

- Object

```typescript
let obj: object | null;

obj = null;
obj = {
  n: 1,
};

console.log(obj); // {n:1}
```

- Type assertions - приведение типов (кастинг)

```typescript
let someString: any = "Some string";
let n: number = (<string>someString).length;
console.log(n); // 11

let value: any = "value";
let size: number = (value as string).length;
console.log(size); // 5
```

### Литеральные типы

```typescript
type actionType = "up" | "down";

function performAction(action: actionType): 1 | -1 {
  switch (action) {
    case "up": // строковый литеральный тип
      return 1; // нумирический литеральный тип
    case "down": // строковый литеральный тип
      return -1; // нумирический литеральный тип
  }
}
```

[назад](#Шпаргалка-по-Typescript)

### Объявление переменных

```typescript
let n = 1;

const n = 1;
```

```typescript
const show = (msg: string) => {
  console.log(msg);
};
```

```typescript
function f([first, second]: [number, number]) {
  console.log(first);
  console.log(second);
}

f([1, 2]);
```

```typescript
// Деструктуризация объекта
{
  const resource = {
    a: "a",
    b: 1,
  };

  let { a, b }: { a: string; b: number } = resource;

  console.log(a);
  console.log(b);
}
```

```typescript
// Вынесение типа с сигнатурой
type C = { a: string; b?: number }; // ? - необязательный тип

function showProperties({ a, b }: C): void {
  console.log(a);
  console.log(b);
}

showProperties({ a: "a" });
```

```typescript
// Вынесение типа с сигнатурой и значением по умолчанию - Schema = { a: "" }
type Schema = { a: string; b?: number };

function init({ a, b = 0 }: Schema = { a: "" }): void {
  console.log(a);
  console.log(b);
}

init({ a: "str" });
```

[назад](#Шпаргалка-по-Typescript)

### Функции

```typescript
// Всегда типизируем вывод функции
function add(x: number, y: number): number {
  return x + y;
}

const sum: number = add(1, 2);
```

```typescript
let myAdd = function (x: number, y: number): number {
  return x + y;
};

const total: number = myAdd(1, 2);
```

```typescript
// Типовая сигнатура для функции (контракт)
type f = (baseValue: number, increment: number) => number;

let increase = <f>function increase(x: number, y: number): number {
  return x + y;
};

const updatedValue: number = increase(3, 1);
```

```typescript
// Всегда проверяем необязательные значения
function buildName(firstName: string, lastName?: string) {
  if (lastName) return firstName + " " + lastName;
  else return firstName;
}

let result1 = buildName("Oliver");
let result2 = buildName("Oliver", "Black");
```

```typescript
// Типизация массива из rest оператора кортежем
function buildLetters(
  firstLetter: string,
  ...restOfLetters: [string, string, string]
) {
  return firstLetter + " " + restOfLetters.join(" ");
}

let letters = buildLetters("a", "b", "c", "d");
```

```typescript
// Запрет this
const run: (this: void, n: number) => void = function (n) {
  // this.n = n;
  console.log(n);
};
```

```typescript
// Применение any
function show(n: number): any {
  if (n < 5) {
    return "Good";
  } else {
    return 100;
  }
}

const myValue = show(5);
```

```typescript
// Присвоение контракта функции
const f = (a: number, b: number): number => a + b;

type FType = (a: number, b: number) => number;

const sum: FType = f;
```

[назад](#Шпаргалка-по-Typescript)

### Дженерики

```typescript
// Сигнатура функции
const returnValueByGeneric = function <T>(arg: T): T {
  return arg;
};

// Имплементация функции
const text: string = returnValueByGeneric<string>("str");
const n: number = returnValueByGeneric<number>(1);
```

```typescript
const returnValueByGeneric = function <T>(arg: T): T {
  return arg;
};
// благодаря типизации из значения, можно не указывать тип дженерика
// тип void при неявной типизации пропускается
const text = returnValueByGeneric("str");
const n: number = returnValueByGeneric(1);
```

```typescript
const readLength = function <T>(arg: T): T {
  // console.log(arg.length);  // Error: T doesn't have .length
  return arg;
};
```

```typescript
//Всегда делаем проверку (guard) при использовании значения определенного типа
const readLength = function <T>(arg: T[]): T[] {
  const n = arg[1];
  // Для полной проверки нужен сложный guard
  if (n instanceof Number) {
    console.log(n.toFixed());
  }
  return arg;
};

readLength<number>([1, 2, 3]);
```

```typescript
//Аргумент функции по типу является дженериком
const readLength = function <T>(arg: Array<T>): Array<T> {
  console.log(arg.length);
  return arg;
};

readLength([1, 2, 3]);
```

```typescript
const getArgument = function <T>(arg: T): T {
  return arg;
};

const core = function <T>(arg: T): void {
  console.log(1);
};

// Функция getArgument валидна по сигнатуре функции anotherFunction
const anotherFunction: <T>(arg: T) => T = getArgument;
// Функция core не валидна по сигнатуре функции anotherFunction
const anotherFunction2: <T>(arg: T) => T = core;

const n: number = <number>anotherFunction(1);
const text: string = <string>anotherFunction("str");
```

```typescript
const fn () : number => 1;
// При сравнении типов двух сигнатур void может взаимозаменяться на другой тип
const anotherFn : <T, U> (n1 : T, n2 : T, t : U) => void = fn;

anotherFn<number, string>(1,2, 'str')
```

```typescript
// Здесь callback всегда возвращает undefined
// Тип с - тоже undefined
function doSomething(callback: () => void) {
  let c = callback();
}

// Эта функция возвращает числа
function aNumberCallback(): number {
  return 2;
}

// работает; обеспечивается типобезопасность в doSomething
doSomething(aNumberCallback);
```

```typescript
// Применение несколько дженериков
const mix = function <T, U>(number1: T, number2: T, text: U): void {
  const str = `${text}: ${number1}, ${number2}`;

  console.log(str);
};
// Контракт
const anotherFunction: <T, U>(number1: T, number2: T, text: U) => void = mix;

anotherFunction<number, string>(1, 2, "List");
```

```typescript
// Типизация функции
const getArgument = function <T>(arg: T): T {
  return arg;
};

// Другой вид типизации функции
const readArgument: { <T>(arg: T): T } = getArgument;
```

```typescript
// В отдельном интерфейсе описывается типизация функции
interface GenericInterface {
  <T>(arg: T): T;
}

const getArgument = function <T>(arg: T): T {
  return arg;
};

const readArgument: GenericInterface = getArgument;

const argument = readArgument(1);
```

```typescript
// В интерфейс можно передавать дженерики
interface GenericInterface<T> {
  (arg: T): T;
}

const getArgument = function <T>(arg: T): T {
  return arg;
};

const readArgument: GenericInterface<number> = getArgument;

const argument = readArgument(1);
```

```typescript
// Внутри класса можно использовать дженерик
class GenericNumber<T> {
  n: T;
  sum: (a: T, b: T) => T;
}

// Передаем дженерик при вызове инстанса класса
let myGenericNumber = new GenericNumber<number>();
myGenericNumber.n = 0;
myGenericNumber.sum = function (n1, n2) {
  return n1 + n2;
};
```

```typescript
interface Core {
  length: number;
}

// Ограничение дженерика семейством интерфеса
const getArgument = function <T extends Core>(arg: T): T {
  console.log(arg.length); // у аргумента есть свойство length, ошибки не будет
  return arg;
};

getArgument(3); // у аргумента нет свойство length, будет ошибка

getArgument({ length: 10, value: 3 });
```

```typescript
// Ограничение типа дженерика K по ключу дженерика T
function getSomeData<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}

const properties = { a: 1, b: 2, c: 3, d: 4 };

getSomeData(properties, "a"); // работает
getSomeData(properties, "m"); // ошибка
```

```typescript
// Типизация функции вызванной, как конструктор
const create = function <T>(Entity: { new (): T }): T {
  return new Entity();
};

class Person {}

create(Person); // работает
create(() => 1); // ошибка, так как у стрелочной функции нет конструктора
```

```typescript
class BeeKeeper {
  hasMask: boolean;
}

class ZooKeeper {
  tag: string;
}

class Animal {
  numLegs: number;
}

class Bee extends Animal {
  keeper: BeeKeeper;
}

class Lion extends Animal {
  keeper: ZooKeeper;
}

// Типизации функции - фабрики с ограничением дженерика типами,
// которые расширяются  классом Animal
const createInstance = function <A extends Animal>(c: new () => A): A {
  return new c();
};

createInstance(Lion).keeper.tag; // работает
createInstance(Bee).keeper.hasMask; // работает
createInstance(BeeKeeper).keeper.hasMask; // ошибка
```

[назад](#Шпаргалка-по-Typescript)

### Классы

```typescript
class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }
  greet() {
    return "Hello, " + this.greeting;
  }
}

let greeter = new Greeter("world");

console.log(greeter.greet());
```

```typescript
class Animal {
  move(distanceInMeters: number = 0) {
    console.log(`Animal moved ${distanceInMeters}m.`);
  }
}

class Dog extends Animal {
  bark() {
    console.log("Woof! Woof!");
  }
}

const dog = new Dog();
dog.bark();
dog.move(10);
dog.bark();
```

```typescript
class AnimalCore {
  name: string;
  constructor(theName: string) {
    this.name = theName;
  }
  move(distanceInMeters: number = 0) {
    console.log(`${this.name} moved ${distanceInMeters}m.`);
  }
}

class Snake extends AnimalCore {
  constructor(name: string) {
    super(name);
  }
  move(distanceInMeters = 5) {
    console.log("Slithering...");
    super.move(distanceInMeters);
  }
}

class Horse extends AnimalCore {
  constructor(name: string) {
    super(name);
  }
  move(distanceInMeters = 45) {
    console.log("Galloping...");
    super.move(distanceInMeters);
  }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```

```typescript
// Применение  модификатора доступа - private
class AnimalBase {
  private name: string; // ограничение доступа к полю за пределами класса
  constructor(theName: string) {
    this.name = theName;
  }
}

new AnimalBase("Cat").name; // ошибка
```

```typescript
class AnotherAnimal {
  private name: string;
  constructor(theName: string) {
    this.name = theName;
  }
}

class Rhino extends AnotherAnimal {
  constructor() {
    super("Rhino");
  }
}

class Employee {
  private name: string;
  constructor(theName: string) {
    this.name = theName;
  }
}

let animal = new AnotherAnimal("Goat");
let rhino = new Rhino();
let employee = new Employee("Bob");

animal = rhino;
animal = employee; // Ошибка: классы 'AnotherAnimal' and 'Employee' не взаимозаменяемы
```

```typescript
class Person {
  protected name: string; // доступно только в тех классах, которые являются расширением Person
  constructor(name: string) {
    this.name = name;
  }
}

class Customer extends Person {
  private department: string;

  constructor(name: string, department: string) {
    super(name);
    this.department = department;
  }

  public getElevatorPitch() {
    return `Hello, my name is ${this.name} and I work in ${this.department}.`;
  }
}

let howard = new Customer("Howard", "Sales");
console.log(howard.getElevatorPitch());
console.log(howard.name); // ошибка
```

```typescript
// Ограничение вызова конструктора за пределами класса
class Base {
  protected name: string;
  protected constructor(theName: string) {
    this.name = theName;
  }
}

// Класс Entity может расширять Base
class Entity extends Base {
  private department: string;

  constructor(name: string, department: string) {
    super(name);
    this.department = department;
  }

  public getElevatorPitch() {
    return `Hello, my name is ${this.name} and I work in ${this.department}.`;
  }
}

let item1 = new Entity("Howard", "Sales");
let item2 = new Base("John"); // Ошибка: конструктор Person - protected
```

```typescript
class Octopus {
  readonly name: string;
  readonly numberOfLegs: number = 8;
  constructor(theName: string) {
    this.name = theName;
  }
}
let dad = new Octopus("Man with the 8 strong legs");
dad.name = "Man with the 3-piece suit"; // ошибка! name только для чтения.
```

```typescript
class Agent {
  private _fullName: string;
  private _secret: string;
  private _passcode: string;

  get fullName(): string {
    return this._fullName;
  }

  set fullName(newName: string) {
    if (this._passcode == this._secret) {
      this._fullName = newName;
    } else {
      console.log("Error: Unauthorized update of employee!");
    }
  }

  constructor(_passcode: string) {
    this._passcode = _passcode;
    this._secret = "secret passcode";
  }
}

let agent = new Agent("secret passcode");
agent.fullName = "Bob Smith";

if (agent.fullName) {
  console.log(agent.fullName);
}
```

```typescript
// Статическое свойство класса
class Builder {
  static capacity: number = 45;
}

console.log(Builder.capacity);
```

```typescript
// Абстрактный класс - применяется в дочерних классах
abstract class Department {
  constructor(public name: string) {}

  getId(): string {
    return this.name + Math.random();
  }

  abstract printId(): void; // имеет абстрактный метод
}

class Item extends Department {
  constructor(public name: string) {
    super(name);
  }

  printId(): void {
    const id: string = super.getId();
    console.log(id);
  }
}

const item: Item = new Item("Oliver");

item.printId();
```

```typescript
// Расширение интерфейса Point3d классом Point
class Point {
  x: number;
  y: number;
}

interface Point3d extends Point {
  z: number;
}

let point3d: Point3d = { x: 1, y: 2, z: 3 };

console.log(point3d);
```

[назад](#Шпаргалка-по-Typescript)

### Интерфейсы

```typescript
interface LabeledValue {
  label: string;
}

const printLabel = function (labeledObj: LabeledValue) {
  console.log(labeledObj.label);
};

let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);
```

```typescript
// Если поля интерфейса не обязательны, нужно делать проверку
interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
  let newSquare = { color: "white", area: 100 };
  if (config.color) {
    newSquare.color = config.color;
  }
  if (config.width) {
    newSquare.area = config.width * config.width;
  }
  return newSquare;
}

let mySquare = createSquare({ color: "black" });
```

```typescript
// Ограничение полей интерфеса - только чтение
interface Point {
  readonly x: number;
  readonly y: number;
  set: (a: number) => void;
}

let p1: Point = {
  x: 10,
  y: 20,
  set(a) {
    this.x = a;
  },
}; // ошибка присвоения x через this
p1.x = 5; // ошибка!
p1.set(1);
```

```typescript
//
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a; // запрет записи в массив
ro[0] = 12; // ошибка!
ro.push(5); // ошибка!
ro.length = 100; // ошибка!
a = ro; // ошибка!
```

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}

const createSquare = function (config: SquareConfig): {
  color: string;
  area: number;
} {
  return { color: "s", area: 1 };
};

let mySquare = createSquare({ colour: "red", width: 100 }); // ошибка
```

```typescript
// Описание интерфейса функции
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;

mySearch = function (source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
};
```

```typescript
// Описание интерейса массива (объекта)
interface StringArray {
  [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];

let myStr: string = myArray[0];
```

```typescript
// У obj, описанного интерфейсом NumberDictionary, нет методов массива
type NumberDictionary = {
  [index: number]: number;
  length: number;
};

const t: Array<string> = ["sd"];
t.map((value) => value);
let obj: NumberDictionary = [1, 2];
obj.length; // работает
obj.map((value) => value); // ошибка
```

```typescript
// Имплементация интерфеса ClockInterface в классе Clock
interface ClockInterface {
  currentTime: Date;
}

class Clock implements ClockInterface {
  currentTime: Date = new Date();
  constructor(h: number, m: number) {}
}
```

```typescript
// Имплементация интерфеса ClockInterface в классе Clock, с методом
interface ClockInterface {
  currentTime: Date;
  setTime(d: Date): void; // метод возвращает что-угодно
}

class Clock implements ClockInterface {
  currentTime: Date = new Date();
  setTime(d: Date) {
    this.currentTime = d;
    return 1; // ошибки не будет
  }
  constructor(h: number, m: number) {}
}
```

```typescript
// Имплементация интерфеса ClockConstructor в классе Clock, с конструктором
interface ClockConstructor {
  new (hour: number, minute: number);
}

class Clock implements ClockConstructor {
  currentTime: Date;
  constructor(h: number, m: number) {}
}
```

```typescript
// Типизация входного конструктора
interface ClockConstructor {
  new (hour: number, minute: number): ClockInterface;
}
interface ClockInterface {
  tick(): void;
}

// Типизация фабрики
function createClock(
  ctor: ClockConstructor,
  hour: number,
  minute: number
): ClockInterface {
  return new ctor(hour, minute);
}

class DigitalClock implements ClockInterface {
  constructor(h: number, m: number) {}
  tick() {
    console.log("beep beep");
  }
}
class AnalogClock implements ClockInterface {
  constructor(h: number, m: number) {}
  tick() {
    console.log("tick tock");
  }
}

let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
```

```typescript
// Присвоение переменной Clock класса-конструктора ClockConstructor, с имплементацией интерфеса ClockInterface
interface ClockConstructor {
  new (hour: number, minute: number): void;
}

interface ClockInterface {
  tick(): void;
}

const Clock: ClockConstructor = class Clock implements ClockInterface {
  constructor(h: number, m: number) {}
  tick() {
    console.log("beep beep");
  }
};
```

```typescript
// Расширение интерфейса Square другим интерфейсом Shape
interface Shape {
  color: string;
}

interface Square extends Shape {
  sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
```

```typescript
// Расширение интерфейса от нескольких интерфейсов
interface Shape {
  color: string;
}

interface PenStroke {
  penWidth: number;
}

interface Square extends Shape, PenStroke {
  sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```

```typescript
interface Counter {
  (start: number): string;
  interval: number;
  reset(): void;
}

function getCounter(): Counter {
  let counter = <Counter>function (start: number) {};
  counter.interval = 123;
  counter.reset = function () {};
  return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

```typescript
class Control {
  private state: any;
}

interface SelectableControl extends Control {
  select(): void;
}

class Button extends Control implements SelectableControl {
  select() {} // обязательный метод
}

class TextBox extends Control {
  select() {} // не обязательный метод
}

// Ошибка: В классе Image нельзя имплеминтировать интерфейс SelectableControl, без расширения Control
class Image implements SelectableControl {
  private state: any;
  select() {}
}
```

[назад](#Шпаргалка-по-Typescript)

### Namespace

```typescript
// Экспорт переменной
namespace Teachers {
  export const category: string = "a";
}
console.log(Teachers.category);
```

```typescript
// Внутри namespace можно создавать другой namespace
namespace Core {
  namespace inner {
    export const n = 1;
  }
}

console.log(Core.inner.n);
```

```typescript
namespace A {
  export namespace B {
    export const C: string = "C";
  }
}

import C = A.B.C;

console.log(C);
```

```typescript
// экспортируем file.js
export namespace Source {
  export const name: string = "Source";
}

// импортируем namespace
import * as Utility from "./file.js";

console.log(Utility.Source.name);
```

[назад](#Шпаргалка-по-Typescript)

### Декораторы

```typescript
// Декоратор
{
  const sealed = function (target: Function) {
    // что-то делать с 'target' ...
  };
}
```

```typescript
// Декоратор Фабрика
function color(value: string) {
  //  это декоратор
  return function (target: Function) {
    // что-то делат с'target' и значением...
  };
}
```

```typescript
// Декорирование класса
const sealed = function (constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
  console.log("Hello from Decorator");
};

@sealed
class Greeter {
  greeting: string;

  constructor(message: string) {
    this.greeting = message;
    console.log(`Hello ${this.greeting} from Constructor`);
  }
}

new Greeter("User");
```

```typescript
// Декорирование функции
const enumerable = function (value: boolean) {
  return function (
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
  ) {
    descriptor.enumerable = value;
  };
};

class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }

  @enumerable(true)
  greet() {
    return "Hello, " + this.greeting;
  }
}

for (const property in Greeter.prototype) {
  console.log(property);
}
```

```typescript
// Декорирование свойства
const format = function () {
  return function (target: any, propertyKey: string): any {
    return {
      value: "default",
      writable: true,
    };
  };
};

class Greeter {
  @format()
  greeting: string;

  constructor(message: string) {
    console.log(this.greeting);
    this.greeting = message;
  }
}

console.log(new Greeter("test"));
```

```typescript
// Вынос валидации с помощью декоратора
function validate(
  target: any,
  propertyName: string,
  descriptor: TypedPropertyDescriptor<Function>
) {
  let method = descriptor.value;
  descriptor.value = function () {
    const name = arguments[0];

    if (name.length < 1) {
      throw new Error("Length should be greater than 0");
    }

    return method.apply(this, arguments);
  };
}

class Greeter {
  greeting: string;

  constructor(message: string) {
    this.greeting = message;
  }

  @validate
  greet(name: string) {
    console.log("Hello " + name + ", " + this.greeting);
  }
}

new Greeter("Hello").greet("S");
```

[назад](#Шпаргалка-по-Typescript)

### Сложные типы

```typescript
// Применение нескольких интерфейсов оператором &

interface teacher {
  email: string;
}

interface pupil {
  group: number;
}

const n: teacher & pupil = {
  email: "",
  group: 1,
};
```

```typescript
// Применение нескольких интерфейсов оператором |

interface teacher {
  email: string;
}

interface pupil {
  group: number;
}

const n: teacher | pupil = {
  email: "",
};
```

```typescript
// Типы для конкатенации
const sum = (a: string, b: string | number) => {
  return a + b;
};
```

```typescript
// Применение Guard - функции проверки
interface Bird {
  fly(): void;
  name: string;
}

interface Fish {
  swim(): void;
  name: string;
}

const getSmallPet = function (): Fish | Bird {
  return {
    name: "oliver",
    swim() {
      console.log("swim");
    },
  };
};

// Guard (проверяет есть ли поле pet в Fish)
const isFish = function (pet: Fish | Bird): pet is Fish {
  // реализация проверки
  return (<Fish>pet).swim !== void 0;
};

const pet = getSmallPet();

if (isFish(pet)) {
  pet.swim();
}
```

```typescript
//  Тип guard-а (проверка того, что x - число)
const isNumber = function (x: any): x is number {
  return typeof x === "number";
};

const f = function (a: string, b: number | string): number {
  const length = a.length;
  const value = isNumber(b) ? b : b.length;

  return length + value;
};
```

```typescript
// Интерфейсы с одинаковым именем (плохая практика)
type name = string;

interface Teacher {
  name: name;
}

interface Teacher {
  age: number;
}

const teacher: Teacher = {
  name: "Oliver",
  age: 21,
};
```

```typescript
// Типизированная сигнатура
type Container<T> = { value: T };

const data: Container<number> = {
  value: 1,
};
```

```typescript
// Типизированная сигнатура с оператором |
type LinkedList<T> = T | { meta: string };

interface Teacher {
  name: string;
}

interface Pupil {
  group: number;
}

const people: LinkedList<Teacher & Pupil> = {
  name: "Oliver",
  group: 21,
  meta: "Some text",
};
```

```typescript
// Ораничение дженерика

type Easing = "ease-in" | "ease-out" | "ease-in-out";
type AnimationNumber = number;

// Конракт для функции animate
// Расширение дженерика от типа AnimationNumber (ограничение)
interface Animation {
  <T extends AnimationNumber>(dx: T, dy: T, easing: Easing): {
    dx: T;
    dy: T;
    easing: Easing;
  };
}

const animate: Animation = function (dx, dy, easing) {
  return {
    dx,
    dy,
    easing,
  };
};

animate<number>(1, 2, "ease-in");
```

```typescript
// Оганичение параметра code типом ErrorCodes
type ErrorCodes = 404 | 503;

const showMessage = function (code: ErrorCodes): void {
  if (code === 404) {
    console.log("Client Error");
  } else if (code === 503) {
    console.log("Server Error");
  }
};

showMessage(404);
showMessage(503);
// showMessage(200);
```

```typescript
// Проверка на,то что все интерфейсы указаны в функции area

interface Square {
  kind: "square";
  size: number;
}

interface Rectangle {
  kind: "rectangle";
  width: number;
  height: number;
}

interface Circle {
  kind: "circle";
  radius: number;
}

interface myShape {
  kind: "myShape";
  size: number;
}

// Генерация ошибки
const generateError = function (x: never): never {
  throw new Error("Unexpected object: " + x);
};

type allowedShapes = Square | Rectangle | Circle | myShape;

const area = function (s: allowedShapes): number {
  let result;

  switch (s.kind) {
    case "square":
      result = s.size * s.size;
      break;
    case "rectangle":
      result = s.height * s.width;
      break;
    case "circle":
      result = Math.PI * s.radius ** 2;
      break;
    // case 'myShape':
    //     result = s.size;
    //     break;
    default:
      generateError(s);
      result = NaN;
  }

  return result;
};

area({
  kind: "myShape",
  size: 0,
});
```

```typescript
// Решение проблемы, когда переменная неинициализированна

class Library {
  titles: string[]; // ошибка
  titles: string[] | undefined; // такое решение не пойдет
  titles: string[] = []; // такое решение не пойдет

  constructor() {}
}

const library = new Library();

const shortTitles = library.titles.filter((title) => title.length < 5);

// Правильное решение
class Library {
  titles: string[];

  constructor(underRenovation: boolean) {
    if (underRenovation) {
      this.titles = ["Some text"];
    } else {
      this.titles = []; // инициализация переменной
    }
  }
}

const library = new Library(false);

const shortTitles = library.titles.filter((title) => title.length < 5);
```

```typescript
// Проверка на существования переменной role c помощью Guard-а

interface Admin {
  id: string;
  role: string;
}

interface User {
  email: string;
}

function redirect(usr: Admin | User) {
  if (isAdmin(usr)) {
    console.log(usr.id);
  } else {
    console.log(usr.email);
  }
}

// Guard
function isAdmin(usr: Admin | User): usr is Admin {
  return (<Admin>usr).role !== undefined;
}
```

```typescript
// Проверка на существования переменной role c помощью оператора in

interface Admin {
  id: string;
  role: string;
}

interface User {
  email: string;
}

function redirect(usr: Admin | User) {
  // проверка
  if ("role" in usr) {
    console.log(usr.role);
  } else {
    console.log(usr.email);
  }
}
```

```typescript
// Запрет мутации

interface IPet {
  name: string;
  age: number;
}

// Описываем объект только для чтения
type ReadonlyPet = {
  readonly [K in keyof IPet]: IPet[K];
};

// Mutable State
const pet: IPet = { name: "Buttons", age: 5 };

// Immutable State
const readonlyPet: ReadonlyPet = { name: "Buttons", age: 1 };

// State mutations
pet.age = 6;

// State mutations
readonlyPet.age = 6;
```

```typescript
// Запрет мутации объекта с необязательным свойством

interface IPet {
  name: string;
  age: number;
  favoritePark?: string;
}

// Описываем объект только для чтения
type ReadonlyPet = {
  readonly [K in keyof IPet]-?: IPet[K]; // исключаем необязательные свойства
};

// Mutable State
const pet: IPet = { name: "Buttons", age: 5 };

// Immutable State
const readonlyPet: ReadonlyPet = { name: "Buttons", age: 5 };
```

```typescript
// Присвоение одной переменной другой возможно с одинаковой сигнатурой

interface IAnimal {
  age: number;
  eat(): void;
  speak(): string;
}

type AnimalTypeAlias = {
  age: number;
  eat(): void;
  speak(): string;
};

let animalInterface: IAnimal = {
  age: 1,
  eat() {},
  speak() {
    return "speak";
  },
};

let animalTypeAlias: AnimalTypeAlias = {
  age: 1,
  eat() {},
  speak() {
    return "speak";
  },
};

animalInterface = animalTypeAlias;
```

```typescript
// Расширение интерфейса

interface IPet {
  pose(): void;
}

interface IFeline {
  nightvision: boolean;
}

interface ICat extends IPet, IFeline {
  color: string;
}

let cat: ICat = {
  color: "red",
  pose(): void {},
  nightvision: true,
};
```

```typescript
// Интерфейс может расширять тип

type Pet = {
  pose(): void;
};

interface IFeline {
  nightvision: boolean;
}

interface Cat extends Pet, IFeline {
  color: string;
}

let cat: Cat = {
  color: "red",
  pose(): void {},
  nightvision: true,
};
```

```typescript
// Применение интерфейса и типа

type Pet = {
  pose(): void;
};

interface IFeline {
  nightvision: boolean;
}

type Cat = IFeline & Pet;

let cat: Cat = {
  pose(): void {},
  nightvision: true,
};
```

```typescript
// Нельзя применять 2 разных типа в одной сущности
type Person = {
  age: number;
};
type Person = {
  name: string;
};

// ошибка
const person: Person = {
  name: "Oliver",
  age: 21,
};
```

```typescript
// Использование unknown (вместо any)

interface IComment {
  date: Date;
  message: string;
}

interface IDataService {
  getData(): unknown;
}

let service: IDataService = {
  getData() {
    return "some text";
  },
};

const response = service.getData();

if (typeof response === "string") {
  console.log(response.toUpperCase());
} else if (isComment(response)) {
  console.log(response.date);
  console.log(response.message);
}

// Guard
function isComment(type: unknown): type is IComment {
  return (
    (<IComment>type).message !== undefined &&
    (<IComment>type).date !== undefined
  );
}
```

```typescript
// Применение дженерика с тернарным оператором

interface StringContainer {
  split(): string[];
}

interface NumberContainer {
  round(): number;
}

type Item<T> = {
  id: T;
  container: T extends string ? StringContainer : NumberContainer;
};

const item: Item<number> = {
  id: 1,
  container: {
    round() {
      return 1;
    },
  },
};
```

[назад](#Шпаргалка-по-Typescript)
