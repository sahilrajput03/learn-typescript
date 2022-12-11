# Learn Typescript

**Utility Types:**

Docs: [Click here](https://www.typescriptlang.org/docs/handbook/utility-types.html)

> Awaited<Type>, Partial<Type>, Required<Type>, Readonly<Type>, Record<Keys, Type>, Pick<Type, Keys>, Omit<Type, Keys>, Exclude<UnionType, ExcludedMembers>, Extract<Type, Union>, NonNullable<Type>, Parameters<Type>, ConstructorParameters<Type>, ReturnType<Type>, InstanceType<Type>, ThisParameterType<Type>, OmitThisParameter<Type>, ThisType<Type>, Intrinsic String Manipulation Types, Uppercase<StringType>, Lowercase<StringType>, Capitalize<StringType>, Uncapitalize<StringType>

**Best Typescript Courses:**
- From Jack Herrington: [No BS TS](https://www.youtube.com/playlist?list=PLNqp92_EXZBJYFrpEzdO2EapvU0GOJ09n)

**Tips:**
- From Jack Herrington: You can use `ctrl+k ctrl+i` to get the typescript popup in vscode.


- **TODO:** Typescript fundamentals on egghead (7$/month): [Click here](https://egghead.io/lessons/typescript-use-the-optional-chaining-operator-in-typescript).
	- Blog & Articles: [Click here](https://mariusschulz.com/blog)
	- Typescript Evolution Blog Series **Marius Schulz**: [Click here](https://mariusschulz.com/blog/series/typescript-evolution)
- **TODO:** [Read this book EffectiveTypescript-62 Specific Ways to Improve Your Typescript.pdf](./EffectiveTypescript-62%20Specific%20Ways%20to%20Improve%20Your%20Typescript.pdf)
- Optionla chaining:

```js
const k = a?.b?.c // k will be undefined if anything is undefined in this chain
const m = a?.['some-property'] // another syntax helpful to get properties with hyphens becoz hyphen property doesn't work with `?.some-property`
const n = a?.() // call function if it's not undefined
```

- Generating types from existing function of libraries:

```js
// Example: 1
let contractBase = new web3Base.eth.Contract([])
export type contractType = typeof contractBase
type getContractsReturnType = {
	[key: string]: contractType
}
const getContracts = async (web3: web3Type): Promise<getContractsReturnType> => {	return new Promise(res => {...})	}
////////

// Example: 2
let web3Base = new Web3(null) // This is to extract type ~Sahil
export type web3Type = typeof web3Base
const getWeb3 = (): Promise<web3Type> => {	new Promise(res => {...}	)
}
```

- Using .d.ts files in typescript

![image](https://user-images.githubusercontent.com/31458531/190871516-05d00936-fcfb-411d-94b9-b808ed2b9a13.png)

- Getting type via *magic* from a zod validator: 

Source: See source of next point in this content.

**Why this is game change?** Because say in nextjs (typescript project) I can simply import get the type from the zod validator we created say for an api and then we can simply ipmort that `type` in frontend ui code to make use of types simply bcoz of the validator's inference's magic, yikes. This make building consuming type-safe apis in frontend like a piece of cake, amazing!

![image](https://user-images.githubusercontent.com/31458531/189499387-c4f9bce2-feed-4b2f-b6f1-e5e58c9aab57.png)

- Making use of validator i.e., zod for writing type safe code

Source: [Click here](https://youtu.be/RmGHnYUqQ4k?t=738)

![image](https://user-images.githubusercontent.com/31458531/189499296-ada048d0-3d9c-43a5-9f31-b48edd016aaa.png)


- Generics in typescrpt:

```ts
type Container<T> = { value: T };

let ss: Container<string>
// ss = "coal" // throws error: Type 'string' is not assignable to type 'Container<string>' 
// ss.value = 23 // throws error: Type 'number' is not assignable to type string.
ss = {value: "my text"}  // works

console.log(ss)
```

- Thats how enums work in typescript:

source: https://www.w3schools.com/typescript/typescript_enums.php

```ts
enum Tabs {
	Notifications = 'notifications',
	ChangePassword = 'change_password',
	Help = 'help',
}

console.log(Tabs.Notifications) // notifications
console.log(Tabs.ChangePassword) // change_password
console.log(Tabs.Help) // help

// for react you can use:
const [tab, setTab] = useState<Tab>(Tab.Notifications)
```

- A popular and trusted validation library for ts: https://github.com/colinhacks/zod (9k*)
- Move tiny things of typescript from learn-react, learn-nextjs, learn-express repos to here.
- Exporting Types is same exporting anything else: [src](https://www.typescriptlang.org/docs/handbook/modules.html)

```ts
// a.ts
export interface UserList {
	id: number;
	name: string;
	email: string;
	phone: string;
}

// b.ts
import {UserList} from './a';

let users: UserList[]

users.map(u => {
	u.<SHOULD AUTOCOMPLETE WITH TYPES>
})
```

![image](https://user-images.githubusercontent.com/31458531/187341148-b5598470-a918-4662-b890-c28973376d65.png)

- Reexports: [src](https://www.typescriptlang.org/docs/handbook/modules.html)

![image](https://user-images.githubusercontent.com/31458531/187341404-be15210c-e8ea-4d96-9e29-a01529987805.png)

- Learn more about exports: ![image](https://user-images.githubusercontent.com/31458531/187341599-5da4ccde-e814-4624-992d-d76a06971114.png)

- Solution to assigning variable to window object without any error in TYPESCRIPT:

![image](https://user-images.githubusercontent.com/31458531/187341945-42ebbfe6-aecc-4f29-9ad6-e3208b819a3d.png)

I used it like: 

```ts
declare global {
	interface Window {
		userLoggedIn: any;
	}
}
window.userLoggedIn = {name: 'Sahil'};
```

- Learn typescript types in in javascript with jsdocs: [Click here](https://github.com/sahilrajput03/sahilrajput03/blob/master/@ts-check.md)
- Enable typescript check in js projects:

	You can do either a workspace level or user settings level or both.

	Source: ttps://code.visualstudio.com/docs/nodejs/working-with-javascript#_type-checking-javascript

	*Tldr: Search for `ts-check` in VSCode settings, and enable `js/ts.implicitProjectConfig.checkJs` setting @ workspace level. Using this settings user level is also encouraged as it will show all the warnings for you at all times but since you want all users of the project to have this feature enabled by defautl you must enable this workspace level too(even if you have it enabled @ user level).*

## Enumbs vs. objects?

**Enums are good but I am considering advantage of 1. sharing the options(label,value) pairs directly from the backend, 2. we can control label values from the backend, 3. Removing a key-value pair is very easy as it won't result in dangling pointer values(coz enums are by default 0,1,2,etc indices only). Actually I think objects are better fit, for e.g.,**

Check below code in ts playgorund: [Click here](https://www.typescriptlang.org/play#code/PTAEBECcHsAcBNoHcB2oDysAuBLaKBnAWACgRQAhAUxSoDMcsCAuU8gRgDpQB1K0AMYBDNAGEAEgEEAcgHEAoqAA2QgEZUlBUPByQqArEoCeoaHVB0YKLDXigkjABbQArllCq9QgNY4UAc1ARIyxHP382MAAmbklwcGAAJXkAWXQANUVaJFBvKiMAWgA3ISUXflghXS0dPQNjCxgAWw8hATyUOxE7S3wbTsERQXwCFyb+RiCBAWhIHQCGyN5JROkASTlmUGkqIqpIQUcRf35Q-jyTFCFxrTMPDWRTVQArfXchLUnhND1RpXc-KAmjgCE0hFgBI4qPBgPBjkpwqASmUqMQSDNCO5VNB4EYACpGWD8AC8oAA3qRQKA8eINlsAER4sIoekAGkpoEkmUSkgUDMke0gQhObI5ADE1niAPopACqAGVRLKADIrBliyZgFIuAgCFwqSCikhU5WSvHK+RS+QADTxPP5ykYhn4VAAHlghUaTSsFFKAArKhVS+VrABa8gZyqEkBOoDAfrKWnlOAAXlQjQBfUikDEEdzwGCwcDIFCYXAjUCk9AvN6cGgenCogAU2NxBKJAEpOGDYE2mwBtZHlVnKNQaAC6HcrAD5QE2ySp1EoR0OqBmOx3s+iRtAlFROEpoP4mwByODlwgAfhPI4LcGLqDLeEIm5ISzFzSCsBw9n431ABC2KY2DPlo3SNH0QH-pUkCAaAkxtDMczhMYSy-JwgGdE2ABS8roNIGENgEOB0EYTZ3kWJZPiMG6kEAA)

```ts
// Dropdown Options
// Benefits:
// 1. We can CHANGE labels directly of frontend without breaking anything
// 2. ADD/REMOVE new key-value pairs directly from backend and frontend can consume it accordingly 
// WARNING: Never change the key names of below object as it can result in mismatched/dangling values
const bodyType = {
  THIN: "Thin",
  AVERAGE: "Average",
  FIT_MUSCULAR: "Fit / Muscular",
  LITTLE_EXTRA: "A little extra",
  LARGE_PLUS_SIZE: "Large / Plus Size",
}

const dropDownOptions = Object.entries(bodyType).map(([value, label]) => ({label, value}))

console.log('options?', dropDownOptions)

type bodyTypeValue = keyof typeof bodyType;
let person1: bodyTypeValue = 'LARGE_PLUS_SIZE' // gives autocomplete as well
// Below throws error
let person2: bodyTypeValue = 'something else?'

// From api we can send options and frontend can parse it accordingly
// res.send(JSON.stringify(dropDownOptions))
```
## Enums? Bad/Good?

Please see **Enumbs vs. objects?** section above for more advanced levaraging power of enum like structure from the just plain objects.**

**Enum Patterns, advantages and disadvantages:**

```txt
# Why Enums?

Why?
- More reliable when we change synonyms alternate for labels (Robust)
- No more case sensitive problems for labels
- More readable code in frontend and backend for labels
- More flexible with multilingual words for labels
Trade Offs (for index based enums)?:
- Less readable in database values in database
- If enum backing number value is changed in code, all values in database need to be updated
```

**UPDATE: 7 Dec, 2022: Actually above discussed trade off of number based enum can be solved simply by using string based enums and we can specify more meaning enum values for each enum option. Yikes** For using string enums refer below links.
- TypeScript string enums, and when and how to use them: [Click here](https://blog.logrocket.com/typescript-string-enums-guide/)
- How To Convert A TypeScript Enum To A JavaScript Array Or String: [Click here](https://hasnode.byrayray.dev/how-to-convert-a-typescript-enum-to-a-javascript-array-or-string)

Typescriptlang Playground of below code: [Click here](https://www.typescriptlang.org/play?&q=301#code/PTAEGUBcCcEsDsDmoCm8CuBbAzgWAFBpagDiAhpigApkAmAkvAA7qSgDeBooAqk6AF5QAIh5VhAGi6gAIgHsA7vEEiZAeQDqAOUnSAMigBmbIcL0BRAGIAVXfm4AlWIgAWJkQ-okAEran4AXwICAGM5eGw5ABsUADoouUQACnJKGgZmVli+AEpQ8MiY+MSUimo6RhZIWPklPPwCEFBENBRoMijUDBwCIkxQAGE5TCYybGwZWGgUEMhYcI5pLTloSBd-bnMxyA2IOVZ16Q0UbB2CIIb8MIjouITkoZGxiamZufDY5dWXeuvCu5Kj1G40m01m83gsXA+zW9SAA)


![image](https://user-images.githubusercontent.com/31458531/206011636-b87f2e38-e70d-4d5f-943f-d6692cfb9abb.png)


## UTILITY TYPES: By jack Herrington `Partial`, `Requried`, `Record`, `Pick`, `Omit` ♥💕❤😘

[Leran from Jack Herrington](https://www.youtube.com/watch?v=tD7DM99nH30)

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

**challenge:**
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
	
**#8**
	
