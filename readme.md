# Шпаргалка по Typescript

*   [Преимущества TS](#Преимущества-TS)
*   [Базовые типы](#Базовые-типы)
*   [Объявление переменных](#Объявление-переменных)
*   [Функции](#Функции)
*   [Классы](#Классы)
*   [Интерфейсы](#Интерфейсы)
*   [Дженерики](#Дженерики)


### Преимущества TS

- Static typing (статическая типизация)
- Type inference (получение типа из значения, которое присвоили в переменную)
- Support IDE (через точку доступные варианты методы и т.д.)
- Strict null checking (строгая проверка на null)
- Interoperability (совместимость с js)

### Базовые типы

- Boolean 

``` typescript
let isDone: boolean = false; 
```
- Number

``` typescript
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```
- String

``` typescript
let color: string = "blue";

// неявное преобразование типов
var fullName = "Bob Bobbington";
var age = 37;
var sentence = "Hello, my name is " + fullName + ".\nI'll be " + (age + 1) + " years old next month.";
```
- Array

``` typescript
let list: number[] = [1, 2, 3,];


let anotherList: Array<number> = [1, 2, 3];
```
- Tuple - кортежи

``` typescript
let data: [string, number];
data = ['hello', 10];


let collection: [string, number];
collection = ['hello', 10];
console.log(collection[0].substr(1));
```
- Enum - перечисления

``` typescript
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
console.log(c); // 1


enum ColorList {Red = 1, Green, Blue}
let myColor: ColorList = ColorList.Green;
console.log(myColor); // 2


enum ColorsData {Red = 1, Green, Blue}
let colorName: string = ColorsData[2];
console.log(colorName); // Green
```
- Any - произвольный тип

``` typescript
let n: any = 1;
n = 'string';
n = false; 
console.log(false); // false
```
- Void - отсутствие конкретного значения, используется в основном в качестве возвращаемого типа функций

``` typescript
function initFunction(): void {
  console.log('Hello');
}


let unusable: void = void 0;
console.log(unusable); // undefined
```
- Undefined - по аналогии с js, указывает, что значение не установлено

``` typescript
let n: undefined = undefined;
```
- Null - по аналогии с js, указывает на неопределенное значение

``` typescript
let n: null = null;
```
- Never - отсутствие значения и используется в качестве возвращаемого типа функций, которые генерируют или возвращают ошибку

``` typescript
let core: never = (() => {
    console.log(true);
    throw new Error('Some Error');
    // return 1;
})();
```
- Object

``` typescript
let obj: object | null;

obj = null;
obj = {
  n:1
};

console.log(obj); // {n:1}
```
- Type assertions - приведение типов (кастинг)

``` typescript
let someString: any = 'Some string';
let n: number = (<string>someString).length;
console.log(n); // 11


let value: any = 'value';
let size: number = (value as string).length;
console.log(size);  // 5
```

[назад](#Шпаргалка-по-Typescript)
