# ES2015 and Beyond

## Babel
Babel is a tool which compiles modern javascript into javascript that can be interpreted by the browser. You must use a compiler in order to take advantage of ES2015 features.
```JavaScript
// You put this in
n => n * n

// And get this back
(function (n) {
  return n * n;
});
```
## Variables
One of the first differences that people notice when first laying their eyes on some ES2015 code is the way that we declare variables. In the past all variables were declared with the keyword `var` ex. `var name = 'Adam'`. These variables were scoped to it's current _execution context_. This means that if a variable is declared inside of a function it is bound to the scope of the enclosing function, and if it is declared outside of a function then it is always _globally_ scoped.

In ES2015 we use the keywords `let` and `const` to declare variables. These two keywords share one important feature that sets them apart from variables declared with `var` - **they create block scoped variables**.

First, let's discuss `let`. `let` creates a block scoped local variable with the option of initializing it with a value.

###### Example
```JavaScript
let size             // declared
let colour = 'blue'  // declared & initialized
```

It acts very, very similarly to `var`, the major difference being _scope_.

`const` will be familiar to people who have experience with other programming languages such as C# or Objective-C. We use `const` in ES2015 to declare a constant, which means that we have created a read-only reference to a value. A constant is a single-assignment value that cannot be reassigned or redeclared. A `const` must be initialized at the time it is declared.

###### Example
```JavaScript
// Constants can be declared with either uppercase or lowercase, but uppercase
// is used by convention to easily identify if the variable is a const.

const BIRTHDAY = '9/2/1987'
BIRTHDAY = 'Feb 9, 1987'     // will throw an error, constants cannot be reassigned
const BIRTHPLACE             // will throw an error, constants must be initialized
```

###### *Note:*
Constants can cause confusion while debugging due to the fact that the variable cannot be reassigned, but that doesnt mean that the value is immutable. For example, if the value is an object, the contents of the object can still be altered.

###### Example
```JavaScript
const PERSON = { name: 'Adam' }
PERSON.name = 'Mark'             // will NOT throw an error
PERSON = { name: 'Mark' }        // will throw an error
```

### Arrow Functions
Arrow functions are a function shorthand which uses the `=>` syntax. Instead of writing the full function synax, functions can be shortened by removing elements of the function signature. Other than the shortened signature, arrow functions also share the same lexical `this` as their surrounding code. This means you do not need to bind `this` when you would like to reference the outer context from within the function. Arrow functions are always anonymous.

###### Example
```JavaScript
// ES5 anonymous function declaration
function (num) {
  return num * num
}

// ES2015 arrow function
(num) => {
  return num * num
}

// if there is only one argument, you can remove the brackets
num => {
  return num * num
}

// if written on one line, you can drop the return keyword all together
num => num * num
```

## Template Strings
Template strings are a new way to define a string, and are especially handy if you would like to
interpolate the value of a variable into a string.
Previously, if you wanted to combine the value of a variable with a string you would have to
concatenate the values together using the `+` operator.

###### Example
```JavaScript
// ES5 string syntax
var name = 'Adam'
var job = 'Software Engineer'
var company = 'TWG'

console.log(name + ' is a ' + job + ' at ' + company)  // 'Adam is a Software Engineer at TWG' p.s. yuck
```
Now we can use string interpolation by surrounding the template literal in backticks \` \` instead of quotes, and surrounding variables with ${ }.

###### Example
```JavaScript
// ES2015 Template String
let pet = {
  name: 'Cliff',
  animal: 'dog',
  colour: 'black and white'
}

console.log(`${pet.name} is a ${pet.colour} ${pet.animal}.`)  // 'Cliff is a black and white dog.'
```

## Classes

ES2015 classes are syntactic sugar over the existing prototype based OO model. They provide a much friendlier syntax to create objects and handle inheritance. Classes support prototype-based inheritance, super calls, instance and static methods and constructors. Unlike function declarations, class declarations are not hoisted. This means that code that looks like this will cause an error:
```JavaScript
var coolCar = new Car() // ReferenceError
class Car {}
```

Let's take a look first at how we used to approach OO Javascript in ES5 and how things have changed with the `class` keyword in ES2015

###### Example
```JavaScript
//Object Oriented Javascript in ES5

// Use an object prototype using a constructor function
function Person(first, last, age, eyecolor) {
    this.firstName = first
    this.lastName = last
    this.age = age
    this.eyeColor = eyecolor
}

// Add a new method to an existing prototype using the prototype property

Person.prototype.name = function() {
    return this.firstName + " " + this.lastName
}
```

###### Example
```JavaScript
// Object Oriented Javascript in ES2015

class Animal {  // Declare class with the class keyword and the name of the class
  constructor (name, colour = 'brown', size = 'small', age = 1) { // Declare construc
    this.name = name
    this.colour = colour
    this.size = size
    this.age = age
  }

  speak () {
    console.log(this.name + ' makes a noise.')
  }
}

class Dog extends Animal {  // Dog is a subclass of Animal
  speak () {  // We override the speak function of the Animal superclass
    console.log(this.name + ' barks.')
  }
  superSpeak () {         // We can still call the speak function of the
    return super.speak()  // superclass by using the super keyword
  }
}

let dog = new Dog('Mitzie')
dog.speak()
console.log(dog)

```
## Promises
The Promise object is used for asynchronous processes. A promise represents the eventual result of an asynchronous operation. It is a placeholder into which the successful result value or reason for failure will materialize. The traditional way to approach async in the past has been to use callback functions.

###### Example
```JavaScript
downloadPhoto('http://coolcats.com/cat.gif', handlePhoto)

function handlePhoto (error, photo) {
  if (error) console.error('Download error!', error)
  else console.log('Download finished', photo)
}

console.log('Download started')
```
A Promise is a proxy for a value not necessarily known when the promise is created. It allows you to associate handlers to an asynchronous action's eventual success value or failure reason. This lets asynchronous methods return values like synchronous methods: instead of the final value, the asynchronous method returns a promise for the value at some point in the future.

A `Promise` is in one of these states:

*pending*: initial state, not fulfilled or rejected.
*fulfilled*: meaning that the operation completed successfully.
*rejected*: meaning that the operation failed.

###### Eaxmple
```JavaScript
var p = new Promise((resolve, reject) => {
  let req = fetchFromApi(url)

	if(req.status === 200) {
		resolve(req.response)
	}	else {
		reject(Error(req.statusText))
	}
})

p.then(() => {
	// do something with the result
}).catch(() => {
	// handle error :(
})

```
Another cool thing that you can do with promises which I have found quite useful, is if you are waiting for a number of promises to resolve then you can add them to an array and run a `.then()` on all of them.
###### Example
```JavaScript
let promises = [p1, p2, p3, p4]
Promise.all(promises).then( () => {
  // All promises are resolved.
  // Do something!
})
```

## TODO
### Modules
### Spread Operator

## Sources
- [__MDN__](https://developer.mozilla.org/en-US/)
  - [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) &   [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
  - [Template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
  - [Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
  - [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
  - [promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [__Babeljs.io__](http://babeljs.io/docs/learn-es2015/)
- [__Ryan Christiani's Post__](https://css-tricks.com/lets-learn-es2015/)
- [__Luke Hoban's es6features repo__](https://github.com/lukehoban/es6features)
