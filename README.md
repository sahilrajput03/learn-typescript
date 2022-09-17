# Learn Typescript

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
