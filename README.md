# Learn Typescript

**Utility Types:**

Docs: [Click here](https://www.typescriptlang.org/docs/handbook/utility-types.html)

> Awaited<Type>, Partial<Type>, Required<Type>, Readonly<Type>, Record<Keys, Type>, Pick<Type, Keys>, Omit<Type, Keys>, Exclude<UnionType, ExcludedMembers>, Extract<Type, Union>, NonNullable<Type>, Parameters<Type>, ConstructorParameters<Type>, ReturnType<Type>, InstanceType<Type>, ThisParameterType<Type>, OmitThisParameter<Type>, ThisType<Type>, Intrinsic String Manipulation Types, Uppercase<StringType>, Lowercase<StringType>, Capitalize<StringType>, Uncapitalize<StringType>

**Best Typescript Courses:**
- From Jack Herrington: [No BS TS](https://www.youtube.com/playlist?list=PLNqp92_EXZBJYFrpEzdO2EapvU0GOJ09n) | My Notes: [Click here](no-bs-ts.md)

**Tips:**
- From Jack Herrington: You can use `ctrl+k ctrl+i` to get the typescript popup in vscode.


- **TODO:** Typescript fundamentals on egghead (7$/month): [Click here](https://egghead.io/lessons/typescript-use-the-optional-chaining-operator-in-typescript).
	- Blog & Articles: [Click here](https://mariusschulz.com/blog)
	- Typescript Evolution Blog Series **Marius Schulz**: [Click here](https://mariusschulz.com/blog/series/typescript-evolution)
- **TODO:** [Read this book EffectiveTypescript-62 Specific Ways to Improve Your Typescript.pdf](./EffectiveTypescript-62%20Specific%20Ways%20to%20Improve%20Your%20Typescript.pdf)

	## we can type req.body in express like that

	Source: [Click here](https://stackoverflow.com/a/55413670/10012446)
	
	![image](https://user-images.githubusercontent.com/31458531/215592538-b7941e08-38b3-4a87-8c70-102e5ab21185.png)

	
## TODO: To merge gists
	
- https://gist.github.com/sahilrajput03/abaac0d120f6e693a768dfd8ff7f7335
- TypeOf and ReturnType.: [Docs](https://www.typescriptlang.org/docs/handbook/2/typeof-types.html)	
- [Click here](https://gist.github.com/sahilrajput03/abaac0d120f6e693a768dfd8ff7f7335)
- [Click here](https://gist.github.com/sahilrajput03/e9c086784d704014929bdc811372cae9)
- [Click here](https://gist.github.com/sahilrajput03/8e4313f08db53b6ec1531e335cd91082)
- [Click here](https://gist.github.com/sahilrajput03/1945433e59a232be958b405a46661ca1)
- [Click here](https://gist.github.com/sahilrajput03/34815090186109523e45682fa87de2cf)

## Open the real logic file when you ctrl+click on function of any library in typescript project

By default the .d.ts file is opened for the library function, but you can open the logic via clicking the respective logic file for currently openeed `.d.ts` file"

![image](https://user-images.githubusercontent.com/31458531/214367056-e6d1144f-069f-4497-913f-556b5beda941.png)

## Extract nested type from a type

Source: [Click here](https://stackoverflow.com/a/56779425/10012446)

```ts
type car = {
bmw: string;
}

// We get, m: string
type m = car['bmw'];
```

	
## Using default vaues in generics
	
```ts
export type Maybe<T=number> = T[] | string[];

let numberValues: Maybe = [1,2,3]
let stringValues: Maybe<string> = ['sahil', 'mohit']
```
	
### Using generics with mongodb's documents:

Proposal to Eric question!

typescript playground: [Click here](https://www.typescriptlang.org/play?ssl=17&ssc=1&pln=17&pc=26#code/C4TwDgpgBAshDO8CGBzBAVcEA86oF4oBlYAJwEsA7FAbQF0A+AqAQVNKRGwCgooBvXnygBjAPaVgESQC4o8MlRQBuIXwC2SYCIAWAG3IK56VXwC+DVd24B6G8XLqweiNxfAo6hMjTwAIlpIcnCIqBhYzDRCgsKiElKyUADkOuRQwDoQpBBJADRqnlq6BkZQNEkArADMACwAnABGNQAmrQBMAOwAHACMzQAMXSL9dV11-QBmVXnJ1fVNrc2dvQNDI2OTNUl0QmbcO9Z2UADq5BnyAK4NzWIiF16S8OlY3KCQhdo6ADKGwPCY70IbA4XH4UDASFIwHIInIEMecgUFGo9GUUAuYGaWggzTklHuDSyaIs1ncnm8YXgpwyMCK+l+-kCwQpvgBOE0nx+Cn+WCYhCifBiwnEkmkwDkKTSGSyOXysQ5xV+choYIxWKkuKgPQAbB0alU6j1qv02v0arlwZDobD4X9lUkeo7Gi12t0+oNhqNxig6jMkh0A87Fst3Wsvf0ANa+uhQMw7cz7IA)

```ts
type MessagesType<T = String[]> = Array<
  {
    content: string;
    matchlist: T;
  }>;

// Simple
let messagesData: MessagesType = [
  {
    content: 'hi there',
    matchlist: ['5349b4ddd2781d08c09890f3', '5349b4ddd2781d08c09890f4']
  }
]

// With subdocuments type
type matchListsType = Array<{ participants: string[]; updated: number; }>

let messagesWithMatchlistsData: MessagesType<matchListsType> = [
  {
    content: 'hi there',
    matchlist: [{ updated: 1674391530204, participants: ['1119b4ddd2781d08c09890g9', '7779b4ddd2781d08c09890k9'] }]
  }
]
```
	
- Using multiple typescript config files

Source: [Click here](https://www.typescriptlang.org/tsconfig#extends)
	
![image](https://user-images.githubusercontent.com/31458531/209340791-7771eeb2-c3ab-4b3e-91cd-25f0702bcfaa.png)

- Merging two types:
	
```ts
// using `type`
type user = {
    name: string;
}

type roles = {
    isAdmin: boolean;
}

let k: user & roles = {
    name: 'sahil',
    isAdmin: true
}
	
// Using `interface`
interface user {
    name: string;
}

interface roles {
    isAdmin: boolean;
}

let k: user & roles = {
    name: 'sahil',
    isAdmin: true
}
````

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

## 🥰 ❣ 💓 ❥ 💑 Thats how you fix a type error when you need need to access values defined on the `req` (`request`) object in express routes
	
**Source: [Click here](https://stackoverflow.com/a/70567048/10012446)**

![image](https://user-images.githubusercontent.com/31458531/207676081-ae1780cb-c92f-4850-86b4-616cb00cf999.png)
