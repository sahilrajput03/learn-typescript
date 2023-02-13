### Extra Topic Require to understand Protected (simulated) and `private` in javascript classes
			   
Protected Behaviour (simulated protected with `get` and `set` methods as JS doesn't have real protected feature) for the class proerties:

Source of below example - JavascriptInfo: [Click here](https://javascript.info/private-protected-properties-methods)
			   
```ts
class CoffeeMachine {
  _waterAmount = 0;

  // Note whenever you'll try to set property `waterAmount` of the instace this method will be called with assigning values as the parameter value
  set waterAmount(value) {
    if (value < 0) {
      value = 0;
    }
    this._waterAmount = value;
  }

  // Note whenever you'll try to get property `waterAmount` of the instace this method will be called and the return value will act as the property value
  get waterAmount() {
    return this._waterAmount;
  }

  constructor(power) {
    this._power = power;
  }

}

// create the coffee machine
let coffeeMachine = new CoffeeMachine(100);

// Add water
coffeeMachine.waterAmount = -10; // _waterAmount will become 0, not -10
```

***Private Behaviour of the class properties:***
			   
Source of below example - JavascriptInfo: [Click here](https://javascript.info/private-protected-properties-methods)

```ts
class CoffeeMachine {
  #waterLimit = 200;

  #fixWaterAmount(value) {
    if (value < 0) return 0;
    if (value > this.#waterLimit) return this.#waterLimit;
  }

  setWaterAmount(value) {
    this.#waterLimit = this.#fixWaterAmount(value);
  }

}

let coffeeMachine = new CoffeeMachine();

// can't access privates from outside of the class
coffeeMachine.#fixWaterAmount(123); // Error
coffeeMachine.#waterLimit = 1000; // Error			   
```

## No BS TS by Jack Herrington

**#1**

Regular Expression type and defining Array types

```ts
let k: RegExp = /car/

// Array types
let a: string[]
let b: Array<string> // defining type with generic type
```

**#3**

Types around functions

```ts
export const format = (title: string, param: string | number): string =>
  `${title} ${param}`;

// for a funciton not returning anything we can use `void` as return type
export const printFormat = (title: string, param: string | number): void => {
  console.log(format(title, param));
};

function introduce(salutation: string, ...names: string[]): string {
  return `${salutation} ${names.join(" ")}`;
}

export function getName(user: { first: string; last: string }): string {
  return `${user?.first ?? "first"} ${user?.last ?? "last"}`;
}

// explicit function type
export type MutationFunction = (v: number) => number;
const myNewMutateFunc: MutationFunction = (v: number) => v * 100;
```

**#4**

Function Overloading
	
- **What is `unknown`?** Ans.  It is basically any but where you have to cast it before you use it. So, its kind of like a safe any.
	
```ts
interface Coordinate {
  x: number;
  y: number;
}

// Function overloading (type definitions)
function parseCoordinate(str: string): Coordinate;
function parseCoordinate(obj: Coordinate): Coordinate;
function parseCoordinate(x: number, y: number): Coordinate;

// Function Definition
function parseCoordinate(arg1: unknown, arg2?: unknown): Coordinate {
  let coord: Coordinate = {
    x: 0,
    y: 0,
  };

  if (typeof arg1 === "string") {
    (arg1 as string).split(",").forEach((str) => {
      const [key, value] = str.split(":");
      coord[key as "x" | "y"] = parseInt(value, 10);
    });
  } else if (typeof arg1 === "object") {
    coord = {
      ...(arg1 as Coordinate),
    };
  } else {
    coord = {
      x: arg1 as number,
      y: arg2 as number,
    };
  }

  return coord;
}

console.log(parseCoordinate(10, 20));
console.log(parseCoordinate({ x: 52, y: 35 }));
console.log(parseCoordinate("x:12,y:22"));
```

**Challenge #1:**
- Learn usage of indexOf in javascript:
	
![image](https://user-images.githubusercontent.com/31458531/206897640-6b62a14d-ea5d-439b-af6a-dcb705df5935.png)

```ts
const houses = [
    { "name": "Atreides", "planets": "Calladan" },
    { "name": "Corrino", "planets": ["Kaitan", "Salusa Secundus"] },
    { "name": "Harkonnen", "planets": ["Giedi Prime", "Arrakis"] }
]

interface House {
    name: string;
    planets: string | string[];
}

// Using existing type to create more type:
// Way1: Sahil
// type HouseWithID = House & { id: number }

// Way2: JackHerrington
interface HouseWithID extends House {
    id: number
}

// function findHouses(houses: string): HouseWithID[];
// function findHouses(houses: string, filter: (house: House) => boolean): HouseWithID[];
// function findHouses(houses: House[]): HouseWithID[];
// function findHouses(houses: House[], filter: (house: House) => boolean): HouseWithID[];

// Above function overloading type definitions are optional to have in code in case you want to have those. But below composite way is much more flexible approach IMO Sahil.

function findHouses(input: string | House[], filter?: (house: House) => boolean): HouseWithID[] {
    const houses = typeof input === 'string' ? JSON.parse(input) : input
        
    return (filter ? houses.filter(filter): houses).map((house: House) => ({...house, id: houses.indexOf(house)}))
}

console.log(
    findHouses(JSON.stringify(houses), ({ name }) => name === "Atreides")
);

console.log(findHouses(houses, ({ name }) => name === "Harkonnen"));
```

**#5**
Optional fields, optional parameteres and optional calls
	
Learn: A required param cannot be placed after an optional param. I.e.,
	
![image](https://user-images.githubusercontent.com/31458531/206898661-16011d6e-c559-4be2-b617-c26aca1ba63a.png)

```ts
function printIngredient(quantity: string, ingredient: string, extra?: string) {
  console.log(`${quantity} ${ingredient} ${extra ? ` ${extra}` : ""}`);
}

printIngredient("1C", "Flour");
printIngredient("1C", "Sugar", "something more");

interface User {
  id: string;
  info?: {
    email?: string;
  };
}

function getEmail(user: User): string {
  if (user.info) {
    return user.info.email!; // Note the usage of ! here to tell typescript I know what I'm doing.
  }
  return "";
}

function getEmailEasy(user: User): string {
  return user?.info?.email ?? ""; // Using Nullish coalescing operator
}

function addWithCallback(x: number, y: number, callback?: () => void) {
  console.log([x, y]);
  callback?.();
}	
```

**#6**

Tuples
	
```ts
type ThreeDCoordinate = [x: number, y: number, z: number];

function add3DCoordinate(
  c1: ThreeDCoordinate,
  c2: ThreeDCoordinate
): ThreeDCoordinate {
  return [c1[0] + c2[0], c1[1] + c2[1], c1[2] + c2[2]];
}

console.log(add3DCoordinate([0, 100, 0], [10, 20, 30]));

type ReturnTuple = [() => string, (v: string) => void]

function simpleStringState(
  initial: string
): ReturnTuple {
  let str: string = initial;
  return [
    () => str,
    (v: string) => {
      str = v;
    },
  ];
}

const [str1getter, str1setter] = simpleStringState("hello");
const [str2getter, str2setter] = simpleStringState("jack");
console.log(str2getter());
console.log(str1getter());
str1setter("goodbye");
console.log(str1getter());
console.log(str2getter());	
```

**#7**

Generics: Interesting thing about typescript is that you can make anything generic, i.e, function, class, type, interface, etc.
	
```ts
function simpleState<T>(initial: T): [() => T, (v: T) => void] {
  let val: T = initial;
  return [
    () => val,
    (v: T) => {
      val = v;
    },
  ];
}

const [st1getter, st1setter] = simpleState(10);
console.log(st1getter());
st1setter(62);
console.log(st1getter());

const [st2getter, st2setter] = simpleState<string | null>(null);
console.log(st2getter());
st2setter("str");
console.log(st2getter());

interface Rank<RankItem> {
  item: RankItem;
  rank: number;
}

function ranker<RankItem>(
  items: RankItem[],
  rank: (v: RankItem) => number
): RankItem[] {
  const ranks: Rank<RankItem>[] = items.map((item) => ({
    item,
    rank: rank(item),
  }));

  ranks.sort((a, b) => a.rank - b.rank);

  return ranks.map((rank) => rank.item);
}

interface Pokemon {
  name: string;
  hp: number;
}

const pokemon: Pokemon[] = [
  {
    name: "Bulbasaur",
    hp: 20,
  },
  {
    name: "Megaasaur",
    hp: 5,
  },
];

const ranks = ranker(pokemon, ({ hp }) => hp);
console.log(ranks);
```
	
**#2 Challenge**
	
Make `forEach`, `map` and `filter` javascript methods using reduce method.
	
```ts
// CHALLENGE #2: Make `forEach`, `filter` and `map` using reduce function only

function myForEach<T>(items: T[], forEachFunc: (item: T) => void) : void{
    items.reduce((_, v: T) => {
        forEachFunc(v)
        return undefined
    }, undefined)
}
// myForEach([1,3,6], (value) => console.log(value))

function myFilter<T>(items: T[], filterFunc: (item: T) => boolean) : boolean[] {
    return items.reduce((a, v: T) => [...a, filterFunc(v)], [] as boolean[])
}
// console.log(myFilter([1,3,6], (value) => value > 2))


function myMap<T, K>(items: T[], mapFunc: (item: T) => K) : K[] {
    return items.reduce((a, v) => [...a, mapFunc(v)], [] as K[])
}
let newValues = myMap([1,3,6], (value) => String(value * 2))
// You can check in editor that typescript has assigned type as string[] to newValues.
// console.log(newValues)
```

**#8**
	
Generic Types and `extends keyof`
	
```ts
function pluck<DataType, KeyType extends keyof DataType>(
  items: DataType[],
  key: KeyType
): DataType[KeyType][] {
  return items.map((item) => item[key]);
}

const dogs = [
  { name: "Mimi", age: 12 },
  { name: "LG", age: 13 },
];

console.log(pluck(dogs, "age"));
console.log(pluck(dogs, "name"));

interface BaseEvent {
  time: number;
  user: string;
}
interface EventMap {
  addToCart: BaseEvent & { quantity: number; productID: string };
  checkout: BaseEvent;
}

function sendEvent<Name extends keyof EventMap>(
  name: Name,
  data: EventMap[Name]
): void {
  console.log([name, data]);
}

sendEvent("addToCart", {
  productID: "foo",
  user: "baz",
  quantity: 1,
  time: 10,
});
sendEvent("checkout", { time: 20, user: "bob" });
```

**#9**

UTILITY TYPES: `Partial`, `Requried`, `Record`, `Pick`, `Omit` ♥💕❤😘 (**TIP: Record is used to type a object type amazingly. #Object type #type object** 

**Typescript Docs - Utility Types:** [Click here](https://www.typescriptlang.org/docs/handbook/utility-types.html)

**`Partial`, `Required`**

```ts
interface Dog {
  name: string;
  age: number;
  breed: string;
  dietary?: string;
}

type AnyDog = Partial<Dog>; // All fields of `AnyDog` type are optional
type RequiredDog = Required<Dog>; // All fields of `RequiredDog` type are required (including optional fields). 
//LEARN: `Required` is opposite of `Partial` Utility Type
```

**`Record`, `Pick`, `Omit`**, getting particular type from existing `type`

```ts
let list: Record<string, number> = {
  'foo': 1,
  'bar': 2,
}

type personType = {
  id: string,
  name: string,
  age?: number
}

// getting particular type from existing `type`
type idType = personType["id"]
type nameType = personType["name"]
type ageType = personType["age"] // *not required* i.e., type ageType = number | undefined

type JustEmailAndNameType = Pick<personType, "name" | "age"> // Pick picks up the name and age keys (keeps optional property type e.g., age).
type exceptNameAndId = Pick<personType, "name" | "id"> // Omit omits name and id keys (keeps optional property type e.g., age).

type personListByIdType = Record<string, personType>

let personListById: personListByIdType = {
  'vjhth4fsg': {
    id: 'vjhth4fsg',
    name: 'sahli',
    age: 10
  },
  'if3h5': {
    id: 'if3h5',
    name: 'John',
    age: 35
  }
}
```

**#10**

- `readonly` allows you to make the properties of a interface immutable.	
- `const` allows you to make the values of the tuple immutable (i.e, array's elements) 

```ts
interface carType {
    name: string
    readonly engine: string
}

let lambo: carType = {
    name: 'lamborgini',
    engine: 'bs6'
}

lambo.name = 'lamborgini sports'
// lambo.engine = 'bs4' // Cannot assign to 'engine' because it is a read-only property.	
```

- From BS TS:

```ts
interface Cat {
  name: string;
  breed: string;
}

function makeCat(name: string, breed: string): Readonly<Cat> {
  return {
    name,
    breed,
  };
}

const usul = makeCat("Usul", "Tabby");
// usul.name = "Piter";

function makeCoordinate(
  x: number,
  y: number,
  z: number
): readonly [number, number, number] {
  return [x, y, z];
}

const c1 = makeCoordinate(10, 20, 30);
c1[0] = 50;

const reallyConst = [1, 2, 3] as const;
reallyConst[0] = 50;
```

**#11**
	
Enums and Literal Types
	
```ts
// const beforeLoad = "beforeLoad";
// const loading = "loading";
// const loaded = "loaded";

enum LoadingState {
  beforeLoad = "beforeLoad",
  loading = "loading",
  loaded = "loaded",
}

const englishLoadingStates = {
  [LoadingState.beforeLoad]: "Before Load",
};

// definiing enum type for the function input
const isLoading = (state: LoadingState) => state === LoadingState.loading;

console.log(isLoading(LoadingState.beforeLoad));

// Using Literal types (`dice` below as function argument)
function rollDice(dice: 1 | 2 | 3): number {
  let pip = 0;
  for (let i = 0; i < dice; i++) {
    pip += Math.floor(Math.random() * 5) + 1;
  }
  return pip;
}
console.log(rollDice(3));

function sendEvent(name: "addToCart", data: { productId: number }): void;
function sendEvent(name: "checkout", data: { cartCount: number }): void;
function sendEvent(name: string, data: unknown): void {
  console.log(`${name}: ${JSON.stringify(data)}`);
}

sendEvent("addToCart", { productId: 123123 });
```

**#12**
		
Typescript Classes; Member Visibility and Implements
			   
- FYI: Member types in class: private, protected and publi
- Default visibility is public.


```ts
interface Database {
  get(id: string): string;
  set(id: string, value: string): void;
}

interface Persistable {
  saveToString(): string;
  restoreFromString(storedState: string): void;
}

class InMemoryDatabase implements Database {
  protected db: Record<string, string> = {};
  get(id: string): string {
    return this.db[id];
  }
  set(id: string, value: string): void {
    this.db[id] = value;
  }
}

class PersistentMemoryDB extends InMemoryDatabase implements Persistable {
  saveToString(): string {
    return JSON.stringify(this.db);
  }
  restoreFromString(storedState: string): void {
    this.db = JSON.parse(storedState);
  }
}

const myDB = new PersistentMemoryDB();
myDB.set("foo", "bar");
// myDB.db["foo"] = "baz";
console.log(myDB.get("foo"));
const saved = myDB.saveToString();
myDB.set("foo", "db1 - bar");

const myDB2 = new PersistentMemoryDB();
myDB2.restoreFromString(saved);
console.log(myDB2.get("foo"));			   
```

	

**TODO: Continue from video 13: [Click here](https://www.youtube.com/watch?v=_ZnNZWlyw7M&list=PLNqp92_EXZBJYFrpEzdO2EapvU0GOJ09n&index=15)**
