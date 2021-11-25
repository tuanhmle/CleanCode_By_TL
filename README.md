# CleanCode_By_TL

Clean Code

Software engineering principles, from Robert C. Martin's book Clean Code
[clean-code-typescript](https://github.com/labs42io/clean-code-typescript)

## Table of Contents

  1. [Meaningful Names](#meaningful-names)
  2. [Functions](#functions)
  4. [Testing](#testing)
  5. [Concurrency](#concurrency)
  6. [Formatting](#formatting)
  7. [Comments](#comments)

## Meaningful Names

### Use Intention-Revealing Names

If the name require a comment, that name does not reveal its intent.

**Bad:**

```ts
d: Date; //elapse time in days

```

**Good:**

```ts
elapseTimeInDays: Date;
daysSinceCreation: Date;
daySinceModification: Date;
fileAgeInDays: Date;
```

Choosing names that reveal intent can make it much easier to understand and change code

**Bad:**

```ts
var getThem = () => {
  let list1: Array<number[]> = [];
  theList.forEach((x) => {
    if (x[0] == 4) list1.push(x);
  });
  return list1;
};
```

**Improve 1:**

```ts
var getFlaggedCells = () => {
  var flaggedCells: Array<number[]> = [];
  gameBoard.forEach((cell: number[]) => {
    if (cell[STATUS_VALUE] == FLAGGED) flaggedCells.push(cell);
  });
  return flaggedCells;
};
```

**Improve 2:**

```ts
var getFlaggedCells = () => {
  var flaggedCells: Array<Cell> = [];
  gameBoard.forEach((cell: Cell) => {
    if (cell.isFlagger()) flaggedCells.push(cell);
  });
  return flaggedCells;
};
```

**[⬆ back to top](#table-of-contents)**

### Avoid Disinformation

We should avoid words whose entrenched meanings vary from our intended meaning.

**Bad:**

```ts
export interface Student {
  fn: string;
  ln: string;
  em: string;
}
```

**Good:**

```ts
export interface Student {
  firstName: string;
  lastName: string;
  email: string;
}
```

Do not refer to a grouping of students as an studentList unless it’s actually a List

**Bad:**

```ts
studentList: Student[];
```

**Good**

```ts
studentGroup: Student[];
bunchOfStudent: Student[];
students: Student[];
```
A truly awful example of disinformative names would be the use of lower-case L or uppercase O as variable names, especially in combination. The problem, of course, is that they look almost entirely like the constants one and zero, respectively.

**Bad:**

```ts
const a = 1;
if (O == l) a = O1;
else l = 01;
```

**[⬆ back to top](#table-of-contents)**

### Make Meaningful Distinctions

In the absence of specific conventions, the variable "moneyAmount" is indistinguishable from "money", "customerInfo" is indistinguishable from "customer", "accountData" is indistinguishable from "account", and "theMessage" is indistinguishable from "message. Distinguish names in
such a way that the reader knows what the differences offer.

**Bad:**

```ts
getStudentInfo(): Student;
getStudentDetails(): Student;
getStudentData(): Student;
```

**Good:**

```ts
getStudent(): Student;
```

**[⬆ back to top](#table-of-contents)**

### Use Pronounceable Names

Humans are good at words. A significant part of our brains is dedicated to the concept of words. And words are, by definition, pronounceable. It would be a shame not to take advantage of that huge portion of our brains that has evolved to deal with spoken language. So make your names pronounceable.

**Bad:**

```ts
fras: [];
stus: [];
pNum: 1234;
```

**Good:**

```ts
fragments: [];
students: [];
phoneNumber: 1234;
```

**[⬆ back to top](#table-of-contents)**

### Use Searchable Names

We will read more code than we will ever write. It's important that the code we do write must be readable and searchable. By *not* naming variables that end up being meaningful for understanding our program, we hurt our readers. Make your names searchable. Tools like [ESLint](https://typescript-eslint.io/) can help identify unnamed constants. If a variable or constant might be seen or used in multiple places in a body of code, it is imperative to give it a search-friendly name. Extension "Code Spell Checker" may help check spell in code for someone need [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)

**Bad:**

```ts
updateError = () => delay(() => this.setState({ hasError: this.props.hasError, message: this.props.message }), 200);
date = data[0].format('MM-DD-YYYY')
time = data[0].format('HH:mm')
```

**Good:**

```ts
export const Validation = {
  ERROR_DELAY_TIME: 200
};
export const DATE_FORMAT = 'MM-DD-YYYY';
export const TIME_FORMAT = 'HH:mm';

updateError = () => delay(() => this.setState({ hasError: this.props.hasError, message: this.props.message }), Validation.ERROR_DELAY_TIME);
date = data[0].format(DATE_FORMAT);
time = data[0].format(TIME_FORMAT);
```

**[⬆ back to top](#table-of-contents)**

### Avoid Encoding

You also don’t need to prefix member variables with m_ anymore.

**Bad:**

```ts
m_dsc: string;
g_stu: student[];
```

**Good:**

```ts
decription: string;
students: student[];
```

**[⬆ back to top](#table-of-contents)**

### Avoid Mental Mapping

Explicit is better than implicit.  
*Clarity is king.*

**Bad:**

```ts
const st = getStudent();
const su = getSubscription();
const t = charge(st, su);

handleStatusChange = (e) => {
    this.setState({ status: e.currentTaget.value });
};
```

**Good:**

```ts
const student = getStudent();
const subscription = getSubscription();
const transaction = charge(student, subscription);

handleStatusChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    this.setState({ status: event.currentTaget.value });
};
```

**[⬆ back to top](#table-of-contents)**

### Component Names

Components and objects should have noun or noun phrase names like Student, Fragment, and SideBar. Avoid words like Manager, Processor, Data, or Info in the name of a Component. A component name should not be a verb.

**Good:**

```ts
class PatientCard extends Component
class ErrorFormField extends Component
```

### Function Names

Methods should have verb or verb phrase names like buildFragment, deleteFragment, or loadData. 

**Good:**

```ts
buildFragment = () => {
  // ...
}
loadData = () => {
  // ...
}
```

### Don't Be Cute

If names are too clever, they will be memorable only to people who share the author’s sense of humor, and only as long as these people remember the joke. Will they know what the function named HolyHandGrenade is supposed to do? Sure, it’s cute, but maybe in this case DeleteItems might be a better name. Choose clarity over entertainment value.

**Bab:**

```ts
HolyHandGrenade: item;
eatMyShorts = () => {
  // ...
}
```

**Good:**

```ts
deleteItem: item;
abort = () => {
  // ...
}
```
**[⬆ back to top](#table-of-contents)**

### Pick One Word Per Concept

Pick one word for one abstract concept and stick with it. For instance, it’s confusing to have "fetch", "retrieve", and "get" as equivalent methods of different component (class). It’s confusing to have a "controller" and a "manager" and a "driver" in the same code base.

**[⬆ back to top](#table-of-contents)**

### Don't Pun

Avoid using the same word for two purposes. Using the same term for two different ideas is essentially a pun.

**Bad:**

```ts
add = (number1: nubmer, number2: number) => number1 + number2
const firstNumber = 1;
const secondNumber = 2;
const oldNumber = add(firstNumber, secondNumber);

add = (newNumber: number) => oldNumber + newNumber
const newNumber = 3;
const result = add(newNumber);
```

**Good:**

```ts
add = (number1: nubmer, number2: number) => number1 + number2
const firstNumber = 1;
const secondNumber = 2;
const oldNumber = add(firstNumber, secondNumber);

insert = (newNumber: number) => oldNumber + newNumber
const newNumber = 3;
const result = insert(newNumber);
```

**[⬆ back to top](#table-of-contents)**

### Don't Add Unneeded Context

If your class/type/object name tells you something, don't repeat that in your variable name.

**Bad:**

```ts
export interface Student {
  studentFirstName: string;
  studentLastName: string;
  studentAge: number;
}
```

**Good:**

```ts
export interface Student = {
  firstName: string;
  lastName: string;
  age: number;
}
```

**[⬆ back to top](#table-of-contents)**


## Functions

### Function Arguments (2 or fewer ideally)

Limiting the number of function parameters is incredibly important because it makes testing your function easier.
Having more than three leads to a combinatorial explosion where you have to test tons of different cases with each separate argument.  

One or two arguments is the ideal case, and three should be avoided if possible. Anything more than that should be consolidated.
Usually, if you have more than two arguments then your function is trying to do too much.
In cases where it's not, most of the time a higher-level object will suffice as an argument.  

Consider using object literals if you are finding yourself needing a lot of arguments.  

To make it obvious what properties the function expects, you can use the [destructuring](https://basarat.gitbook.io/typescript/future-javascript/destructuring) syntax.
This has a few advantages:

1. When someone looks at the function signature, it's immediately clear what properties are being used.

2. It can be used to simulate named parameters.

3. Destructuring also clones the specified primitive values of the argument object passed into the function. This can help prevent side effects. Note: objects and arrays that are destructured from the argument object are NOT cloned.

4. TypeScript warns you about unused properties, which would be impossible without destructuring.

**Bad:**

```ts
createMenu = (title: string, body: string, buttonText: string, cancellable: boolean) => {
  // ...
}

createMenu('Foo', 'Bar', 'Baz', true);
```

**Good:**

```ts
createMenu = (options: { title: string, body: string, buttonText: string, cancellable: boolean }) => {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

You can further improve readability by using [interface](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases):

```ts

export interface MenuOptions { 
  title: string, 
  body: string, 
  buttonText: string, 
  cancellable: boolean 
};

function createMenu(options: MenuOptions) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

**[⬆ back to top](#table-of-contents)**

### Functions Should Do One Thing

This is by far the most important rule in software engineering. When functions do more than one thing, they are harder to compose, test, and reason about. When you can isolate a function to just one action, it can be refactored easily and your code will read much cleaner. If you take nothing else away from this guide other than this, you'll be ahead of many developers.

**Bad:**

```ts
emailClients = (clients: Client[]) => {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good:**

```ts
emailClients(clients: Client[]) => {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client: Client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ back to top](#table-of-contents)**

### Function Names Should Say What They Do

**Bad:**

```ts
addToDate = (date: Date, month: number): Date => {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);

build = () => {
  // ...
}
```

**Good:**

```ts
addMonthToDate = (date: Date, month: number): Date => {
  // ...
}

const date = new Date();
addMonthToDate(date, 1);

buildFormField = () => {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### Functions Should Only Be One Level Of Abstraction

When you have more than one level of abstraction your function is usually doing too much. Splitting up functions leads to reusability and easier testing.

**Bad:**

```ts
parseCode = (code: string) => {
  const REGEXES = [ /* ... */ ];
  const statements = code.split(' ');
  const tokens = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**Good:**

```ts
const REGEXES = [ /* ... */ ];

parseCode = (code: string) => {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);

  syntaxTree.forEach((node) => {
    // parse...
  });
}

tokenize = (code: string): Token[] => {
  const statements = code.split(' ');
  const tokens: Token[] = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      tokens.push( /* ... */ );
    });
  });

  return tokens;
}

parse = (tokens: Token[]): SyntaxTree => {
  const syntaxTree: SyntaxTree[] = [];
  tokens.forEach((token) => {
    syntaxTree.push( /* ... */ );
  });

  return syntaxTree;
}
```

**[⬆ back to top](#table-of-contents)**

### Remove Duplicate Code

Do your absolute best to avoid duplicate code.
Duplicate code is bad because it means that there's more than one place to alter something if you need to change some logic.  

Imagine if you run a restaurant and you keep track of your inventory: all your tomatoes, onions, garlic, spices, etc.
If you have multiple lists that you keep this on, then all have to be updated when you serve a dish with tomatoes in them.
If you only have one list, there's only one place to update!  

Oftentimes you have duplicate code because you have two or more slightly different things, that share a lot in common, but their differences force you to have two or more separate functions that do much of the same things. Removing duplicate code means creating an abstraction that can handle this set of different things with just one function/module/class.  

Getting the abstraction right is critical, that's why you should follow the [SOLID](#solid) principles. Bad abstractions can be worse than duplicate code, so be careful! Having said this, if you can make a good abstraction, do it! Don't repeat yourself, otherwise, you'll find yourself updating multiple places anytime you want to change one thing.

**Bad:**

```ts
showDeveloperList = (developers: Developer[]) => {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();

    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

showManagerList(managers: Manager[]) => {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Good:**

```ts
class Developer {
  // ...
  getExtraDetails() {
    return {
      githubLink: this.githubLink,
    }
  }
}

class Manager {
  // ...
  getExtraDetails() {
    return {
      portfolio: this.portfolio,
    }
  }
}

function showEmployeeList(employee: Developer | Manager) {
  employee.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const extra = employee.getExtraDetails();

    const data = {
      expectedSalary,
      experience,
      extra,
    };

    render(data);
  });
}
```

**Bad:**

```ts
  dataMap.set(FormField.NURSING_PROBLEM,, createFormField({ FormField.NURSING_PROBLEM, FormFieldLabel.NURSING_PROBLEM, type: FormFieldInputType.TEXT_AREA, value: fragment?.chartData?.[FormField.NURSING_PROBLEM]?.content || '' }));
  dataMap.set(FormField.EXPECTED_OUTCOME,, createFormField({ FormField.EXPECTED_OUTCOME, FormFieldLabel.EXPECTED_OUTCOME, type: FormFieldInputType.TEXT_AREA, value: fragment?.chartData?.[FormField.EXPECTED_OUTCOME]?.content || '' }))
  dataMap.set(FormField.INTERVENTION_PLAN,, createFormField({ FormField.INTERVENTION_PLAN, FormFieldLabel.INTERVENTION_PLAN, type: FormFieldInputType.TEXT_AREA, value: fragment?.chartData?.[FormField.INTERVENTION_PLAN]?.content || '' }))|| '' }))
```

**Good:**

```ts
const textAreas = [
    { name: FormField.NURSING_PROBLEM, label: FormFieldLabel.NURSING_PROBLEM },
    { name: FormField.EXPECTED_OUTCOME, label: FormFieldLabel.EXPECTED_OUTCOME },
    { name: FormField.INTERVENTION_PLAN, label: FormFieldLabel.INTERVENTION_PLAN 
  ];
  textAreas.forEach(({ name, label }) =>
    dataMap.set(name, createFormField({ name, label, type: FormFieldInputType.TEXT_AREA, value: fragment?.chartData?.[name]?.content || '' }))
  );
```

You should be critical about code duplication. Sometimes there is a tradeoff between duplicated code and increased complexity by introducing unnecessary abstraction. When two implementations from two different modules look similar but live in different domains, duplication might be acceptable and preferred over extracting the common code. The extracted common code, in this case, introduces an indirect dependency between the two modules.

**[⬆ back to top](#table-of-contents)**

### Use Default Arguments Instead Of Short Circuiting Or Conditionals

Default arguments are often cleaner than short circuiting.

**Bad:**

```ts
export const scroll = (top? : number) => {
  window.scrollTo({ top || 0, behavior: 'smooth' });
}
```

**Good:**

```ts
export const scroll = (top = 0) => {
  window.scrollTo({ top, behavior: 'smooth' });
}
```

**[⬆ back to top](#table-of-contents)**

### Set Default Objects With Object.assign Or Destructuring

**Bad:**

```ts
interface MenuConfig { 
  title?: string, 
  body?: string, 
  buttonText?: string, 
  cancellable?: boolean 
};

createMenu = (config: MenuConfig) => {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable !== undefined ? config.cancellable : true;

  // ...
}

createMenu({ body: 'Bar' });
```

**Good:**

```ts
interface MenuConfig { 
  title?: string, 
  body?: string, 
  buttonText?: string, 
  cancellable?: boolean 
};

createMenu = (config: MenuConfig) => {
  const menuConfig = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // ...
}

createMenu({ body: 'Bar' });
```

Alternatively, you can use destructuring with default values:

```ts
interface MenuConfig { 
  title?: string, 
  body?: string, 
  buttonText?: string, 
  cancellable?: boolean 
};

createMenu = ({ title = 'Foo', body = 'Bar', buttonText = 'Baz', cancellable = true }: MenuConfig) => {
  // ...
}

createMenu({ body: 'Bar' });
```

To avoid any side effects and unexpected behavior by passing in explicitly the `undefined` or `null` value, you can tell the TypeScript compiler to not allow it.
See [`--strictNullChecks`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#--strictnullchecks) option in TypeScript.

**[⬆ back to top](#table-of-contents)**

### Avoid Side Effects (part 1)

A function produces a side effect if it does anything other than take a value in and return another value or values.
A side effect could be writing to a file, modifying some global variable, or accidentally wiring all your money to a stranger.  

Now, you do need to have side effects in a program on occasion. Like the previous example, you might need to write to a file.
What you want to do is to centralize where you are doing this. Don't have several functions and classes that write to a particular file.
Have one service that does it. One and only one.  

The main point is to avoid common pitfalls like sharing state between objects without any structure, using mutable data types that can be written to by anything, and not centralizing where your side effects occur. If you can do this, you will be happier than the vast majority of other programmers.

**Bad:**

```ts
// Global variable referenced by following function.
let name = 'Robert C. Martin';

toBase64 = () => {
  name = btoa(name);
}

toBase64();
// If we had another function that used this name, now it'd be a Base64 value

console.log(name); // expected to print 'Robert C. Martin' but instead 'Um9iZXJ0IEMuIE1hcnRpbg=='
```

**Good:**

```ts
const name = 'Robert C. Martin';

toBase64 = (text: string): string => {
  return btoa(text);
}

const encodedName = toBase64(name);
console.log(name);
```

**[⬆ back to top](#table-of-contents)**

### Avoid Side Effects (part 2)

In JavaScript, primitives are passed by value and objects/arrays are passed by reference. In the case of objects and arrays, if your function makes a change in a shopping cart array, for example, by adding an item to purchase, then any other function that uses that `cart` array will be affected by this addition. That may be great, however, it can be bad too. Let's imagine a bad situation:  

The user clicks the "Purchase", a button which calls a `purchase` function that spawns a network request and sends the `cart` array to the server. Because of a bad network connection, the purchase function has to keep retrying the request. Now, what if in the meantime the user accidentally clicks "Add to Cart" button on an item they don't actually want before the network request begins? If that happens and the network request begins, then that purchase function will send the accidentally added item because it has a reference to a shopping cart array that the `addItemToCart` function modified by adding an unwanted item.  

A great solution would be for the `addItemToCart` to always clone the `cart`, edit it, and return the clone. This ensures that no other functions that are holding onto a reference to the shopping cart will be affected by any changes.  

Two caveats to mention to this approach:

1. There might be cases where you actually want to modify the input object, but when you adopt this programming practice you will find that those cases are pretty rare. Most things can be refactored to have no side effects! (see [pure function](https://en.wikipedia.org/wiki/Pure_function))

2. Cloning big objects can be very expensive in terms of performance. Luckily, this isn't a big issue in practice because there are great libraries that allow this kind of programming approach to be fast and not as memory intensive as it would be for you to manually clone objects and arrays.

**Bad:**

```ts
addItemToCart = (cart: CartItem[], item: Item): void => {
  cart.push({ item, date: Date.now() });
};
```

**Good:**

```ts
addItemToCart = (cart: CartItem[], item: Item): CartItem[] => {
  return [...cart, { item, date: Date.now() }];
};
```

**Improve with immer

```ts
addItemToCart(cart: CartItem[], item: Item): CartItem[] => produce(cart, (draft) => {
  draft.push({ item, date: Date.now() });
});
```


**[⬆ back to top](#table-of-contents)**

### Don't Over-Optimize

Modern browsers do a lot of optimization under-the-hood at runtime. A lot of times, if you are optimizing then you are just wasting your time. There are good [resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) for seeing where optimization is lacking. Target those in the meantime, until they are fixed if they can be.

**Bad:**

```ts
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Good:**

```ts
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### Remove Dead Code

Dead code is just as bad as duplicate code. There's no reason to keep it in your codebase.
If it's not being called, get rid of it! It will still be safe in your version history if you still need it.

**Bad:**

```ts
oldRequestModule = (url: string) => {
  // ...
}

requestModule = (url: string) => {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**Good:**

```ts
requestModule (url: string) => {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**[⬆ back to top](#table-of-contents)**

## Testing

Testing is more important than shipping. If you have no tests or an inadequate amount, then every time you ship code you won't be sure that you didn't break anything.
Deciding on what constitutes an adequate amount is up to your team, but having 100% coverage (all statements and branches)
is how you achieve very high confidence and developer peace of mind. This means that in addition to having a great testing framework, you also need to use a good [coverage tool](https://github.com/gotwarlost/istanbul).

There's no excuse to not write tests. There are [plenty of good JS test frameworks](http://jstherightway.org/#testing-tools) with typings support for TypeScript, so find one that your team prefers. When you find one that works for your team, then aim to always write tests for every new feature/module you introduce. If your preferred method is Test Driven Development (TDD), that is great, but the main point is to just make sure you are reaching your coverage goals before launching any feature, or refactoring an existing one.  

### The three laws of TDD

1. You are not allowed to write any production code unless it is to make a failing unit test pass.

2. You are not allowed to write any more of a unit test than is sufficient to fail, and; compilation failures are failures.

3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test.

**[⬆ back to top](#table-of-contents)**

### F.I.R.S.T. rules

Clean tests should follow the rules:

- **Fast** tests should be fast because we want to run them frequently.

- **Independent** tests should not depend on each other. They should provide same output whether run independently or all together in any order.

- **Repeatable** tests should be repeatable in any environment and there should be no excuse for why they fail.

- **Self-Validating** a test should answer with either *Passed* or *Failed*. You don't need to compare log files to answer if a test passed.

- **Timely** unit tests should be written before the production code. If you write tests after the production code, you might find writing tests too hard.

**[⬆ back to top](#table-of-contents)**

### Single concept per test

Tests should also follow the *Single Responsibility Principle*. Make only one assert per unit test.

**Bad:**

```ts
describe('AwesomeDate', () => {
  it('handles date boundaries', () => {
    let date: AwesomeDate;

    date = new AwesomeDate('1/1/2015');
    expect(date.addDays(30)).toEqual('1/31/2015');

    date = new AwesomeDate('2/1/2016');
    expect(date.addDays(28)).equal('2/29/2016');

    date = new AwesomeDate('2/1/2015');
    expect(equal(date.addDays(28)).toEqual('3/1/2015');
  });
});
```

**Good:**

```ts

describe('AwesomeDate', () => {
  it('handles 30-day months', () => {
    const date = new AwesomeDate('1/1/2015');
    expect(date.addDays(30)).toEqual('1/31/2015');
  });

  it('handles leap year', () => {
    const date = new AwesomeDate('2/1/2016');
    expect(date.addDays(28)).equal('2/29/2016');
  });

  it('handles non-leap year', () => {
    const date = new AwesomeDate('2/1/2015');
    expect(equal(date.addDays(28)).toEqual('3/1/2015');
  });
});
```

**[⬆ back to top](#table-of-contents)**

### The name of the test should reveal its intention

When a test fails, its name is the first indication of what may have gone wrong.

**Bad:**

```ts
describe('Calendar', () => {
  it('2/29/2020', () => {
    // ...
  });

  it('throws', () => {
    // ...
  });
});
```

**Good:**

```ts
describe('Calendar', () => {
  it('should handle leap year', () => {
    // ...
  });

  it('should throw when format is invalid', () => {
    // ...
  });
});
```

**[⬆ back to top](#table-of-contents)**

## Concurrency

### Prefer promises vs callbacks

Callbacks aren't clean, and they cause excessive amounts of nesting *(the callback hell)*.  
There are utilities that transform existing functions using the callback style to a version that returns promises
(for Node.js see [`util.promisify`](https://nodejs.org/dist/latest-v8.x/docs/api/util.html#util_util_promisify_original), for general purpose see [pify](https://www.npmjs.com/package/pify), [es6-promisify](https://www.npmjs.com/package/es6-promisify))

**Bad:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';

function downloadPage(url: string, saveTo: string, callback: (error: Error, content?: string) => void) {
  get(url, (error, response) => {
    if (error) {
      callback(error);
    } else {
      writeFile(saveTo, response.body, (error) => {
        if (error) {
          callback(error);
        } else {
          callback(null, response.body);
        }
      });
    }
  });
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html', (error, content) => {
  if (error) {
    console.error(error);
  } else {
    console.log(content);
  }
});
```

**Good:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url)
    .then(response => write(saveTo, response));
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html')
  .then(content => console.log(content))
  .catch(error => console.error(error));  
```

Promises supports a few helper methods that help make code more concise:  

| Pattern                  | Description                                |  
| ------------------------ | -----------------------------------------  |  
| `Promise.resolve(value)` | Convert a value into a resolved promise.   |  
| `Promise.reject(error)`  | Convert an error into a rejected promise.  |  
| `Promise.all(promises)`  | Returns a new promise which is fulfilled with an array of fulfillment values for the passed promises or rejects with the reason of the first promise that rejects. |
| `Promise.race(promises)`| Returns a new promise which is fulfilled/rejected with the result/error of the first settled promise from the array of passed promises. |

`Promise.all` is especially useful when there is a need to run tasks in parallel. `Promise.race` makes it easier to implement things like timeouts for promises.


**[⬆ back to top](#table-of-contents)**

## Formatting

Formatting is subjective. Like many rules herein, there is no hard and fast rule that you must follow. The main point is *DO NOT ARGUE* over formatting. There are tons of tools to automate this. Use one! It's a waste of time and money for engineers to argue over formatting. The general rule to follow is *keep consistent formatting rules*.  

For TypeScript there is a powerful tool called [ESLint](https://typescript-eslint.io/). It's a static analysis tool that can help you improve dramatically the readability and maintainability of your code. There are ready to use ESLint configurations that you can reference in your projects:

- [ESLint Config Airbnb](https://www.npmjs.com/package/eslint-config-airbnb-typescript) - Airbnb style guide

- [ESLint Base Style Config](https://www.npmjs.com/package/eslint-plugin-base-style-config) - a Set of Essential ESLint rules for JS, TS and React

- [ESLint + Prettier](https://www.npmjs.com/package/eslint-config-prettier) - lint rules for [Prettier](https://github.com/prettier/prettier) code formatter

Refer also to this great [TypeScript StyleGuide and Coding Conventions](https://basarat.gitbook.io/typescript/styleguide) source.

### Migrating from TSLint to ESLint

If you are looking for help in migrating from TSLint to ESLint, you can check out this project: <https://github.com/typescript-eslint/tslint-to-eslint-config>

### Use consistent capitalization

Capitalization tells you a lot about your variables, functions, etc. These rules are subjective, so your team can choose whatever they want. The point is, no matter what you all choose, just *be consistent*.

**Bad:**

```ts
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

type animal = { /* ... */ }
type Container = { /* ... */ }
```

**Good:**

```ts
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const ARTISTS = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restoreDatabase() {}

type Animal = { /* ... */ }
type Container = { /* ... */ }
```

Prefer using `PascalCase` for class, interface, type and namespace names.  
Prefer using `camelCase` for variables, functions and class members.

**[⬆ back to top](#table-of-contents)**

### Organize imports

With clean and easy to read import statements you can quickly see the dependencies of current code. Make sure you apply following good practices for `import` statements:

- Import statements should be alphabetized and grouped.
- Unused imports should be removed.
- Named imports must be alphabetized (i.e. `import {A, B, C} from 'foo';`)
- Import sources must be alphabetized within groups, i.e.: `import * as foo from 'a'; import * as bar from 'b';`
- Groups of imports are delineated by blank lines.
- Groups must respect following order:
  - Polyfills (i.e. `import 'reflect-metadata';`)
  - Node builtin modules (i.e. `import fs from 'fs';`)
  - external modules (i.e. `import { query } from 'itiriri';`)
  - internal modules (i.e `import { UserService } from 'src/services/userService';`)
  - modules from a parent directory (i.e. `import foo from '../foo'; import qux from '../../foo/qux';`)
  - modules from the same or a sibling's directory (i.e. `import bar from './bar'; import baz from './bar/baz';`)

**Bad:**

```ts
import { TypeDefinition } from '../types/typeDefinition';
import { AttributeTypes } from '../model/attribute';
import { ApiCredentials, Adapters } from './common/api/authorization';
import fs from 'fs';
import { ConfigPlugin } from './plugins/config/configPlugin';
import { BindingScopeEnum, Container } from 'inversify';
import 'reflect-metadata';
```

**Good:**

```ts
import 'reflect-metadata';

import fs from 'fs';
import { BindingScopeEnum, Container } from 'inversify';

import { AttributeTypes } from '../model/attribute';
import { TypeDefinition } from '../types/typeDefinition';

import { ApiCredentials, Adapters } from './common/api/authorization';
import { ConfigPlugin } from './plugins/config/configPlugin';
```

**[⬆ back to top](#table-of-contents)**

### Use Typescript Aliases

Create prettier imports by defining the paths and baseUrl properties in the compilerOptions section in the `tsconfig.json`

This will avoid long relative paths when doing imports.

**Bad:**

```ts
import { modalServiceMock } from '../../../../config/jest/mocks/service.mock';
```

**Good:**

```ts
import { modalServiceMock } from '@test/mocks/service.mock';
```

```js
// tsconfig.json
...
  "compilerOptions": {
    ...
    "baseUrl": "src",
    "paths": {
      "@test/*": ["../config/jest/*"]
    }
    ...
  }
...
```

**[⬆ back to top](#table-of-contents)**

## Comments

The use of a comments is an indication of failure to express without them. Code should be the only source of truth.
  
> Don’t comment bad code—rewrite it.  
> — *Brian W. Kernighan and P. J. Plaugher*

### Prefer self explanatory code instead of comments

Comments are an apology, not a requirement. Good code *mostly* documents itself.

**Bad:**

```ts
// Check if subscription is active.
if (subscription.endDate > Date.now) {  }
```

**Good:**

```ts
const isSubscriptionActive = subscription.endDate > Date.now;
if (isSubscriptionActive) { /* ... */ }
```

**[⬆ back to top](#table-of-contents)**

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**Bad:**

```ts
interface Student {
  name: string;
  email: string;
  // age: number;
}
```

**Good:**

```ts
interface Student {
  name: string;
  email: string;
}
```

**[⬆ back to top](#table-of-contents)**

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code, and especially journal comments. Use `git log` to get history!

**Bad:**

```ts
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Added type-checking (LI)
 * 2015-03-14: Implemented combine (JR)
 */
combine = (a: number, b: number): number => {
  return a + b;
}
```

**Good:**

```ts
combine = (a: number, b: number): number => {
  return a + b;
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid positional markers

They usually just add noise. Let the functions and variable names along with the proper indentation and formatting give the visual structure to your code.  
Most IDE support code folding feature that allows you to collapse/expand blocks of code (see Visual Studio Code [folding regions](https://code.visualstudio.com/updates/v1_17#_folding-regions)).

**Bad:**

```ts
////////////////////////////////////////////////////////////////////////////////
// Client class
////////////////////////////////////////////////////////////////////////////////
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  ////////////////////////////////////////////////////////////////////////////////
  // public methods
  ////////////////////////////////////////////////////////////////////////////////
  public describe(): string {
    // ...
  }

  ////////////////////////////////////////////////////////////////////////////////
  // private methods
  ////////////////////////////////////////////////////////////////////////////////
  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
};
```

**Good:**

```ts
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  public describe(): string {
    // ...
  }

  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
};
```

**[⬆ back to top](#table-of-contents)**

### TODO comments

When you find yourself that you need to leave notes in the code for some later improvements,
do that using `// TODO` comments. Most IDE have special support for those kinds of comments so that
you can quickly go over the entire list of todos.  

Keep in mind however that a *TODO* comment is not an excuse for bad code. 

**Bad:**

```ts
getActiveSubscriptions = (): Promise<Subscription[]> => {
  // ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**Good:**

```ts
getActiveSubscriptions = (): Promise<Subscription[]> => {
  // TODO: ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**[⬆ back to top](#table-of-contents)**

