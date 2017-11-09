# JavaScript---The-Weird-Parts

Took a fantastic course on the underworkings of JavaScript. Here are my notes and examples. 

JavaScript – The weird parts

Syntax Parser – A program that reads your code and determines what it does and if its grammar is valid. Someone else wrote a program to translate it for the computer, called a compiler. Something the hardware can understand. The compiler can add “Extra Stuff” to your code when it compiles before those instructions are given to the computer. 
Lexical Environment – Where something sits physically in the code you write. For example, a variable may sit lexically inside of a function. Lexical environment, where the code is and what surrounds it. 
Execution Context – A manager to help manage the code that is running. 
JavaScript always has a global object, in the browser it is ‘window.’ The JavaScript engine creates this. Window is available to all of the code in that lexical environment. The engine also created a special variable called ‘this.’ At the global level, this = window. 
Global – “Not inside a function” 
When you create variables and functions, and you’re not inside a function, those variables and functions get attached to the window object. 
At the global level, there is no outer environment. 
Execution Context creates a wrapper (the compiler / JS engine) that has these other things that you didn’t write, the JS engine did this for you. Like the window object and ‘this’ which refers to the global object (window). 
During the first phase (creation phase) the JS engine will setup the Memory Space for variables and functions. That’s why we can call a function or console log a variable when those are beneath it physically in our code. It’s not actually ‘hoisting’ anything physically, the name is misleading, the JavaScript engine just setting aside memory space for the variables and functions you’ve created in the entire code. They exist in memory, so the JS code can access them – however, the variable’s assignment is not placed in memory.  All variables in JS are initially set to undefined, and functions are sitting in memory in their entirety. 
Undefined – is not just a word, it is a special value that JavaScript has within it internally, it means the variable has not yet been set. In the initial creation phase, all variables are set to undefined. Undefined is an actual value, it is taking up memory space. 
** Remember this is different than never actually declaring a, in which it will throw an error. 
 Never set a variable to undefined, you can, but it’s dangerous because it will be hard to debug. You won’t know if it’s undefined if it’s because you set it, or because the JS engine did it. 
Example: http://jsfiddle.net/35acbcpc/1/ 



Creation Phase – Sets variables and functions into memory (and the global object, ‘this’ and outer environment) 
Execution Phase – Runs your code line by line. 

Single Threaded – One command is being executed at a time 
Synchronous – One at a time and in order 

Function Invocation – Just means we are running / calling the function. 
Every time a function is called, a new execution context is created for that function and added to the top of the stack. After it finishes, it is “popped” off the stack and the next execution context on the stack is ran. The ‘this’ variable is created for that function, the variables within it are setup during the creation phase, etc. 
Example: http://jsfiddle.net/0hw9u6ec/3/
Variable Environments – Where the variables live and how they relate to each other in memory. Every execution context has its own variable environment. Every time you call a function you get your own execution context.  Example: http://jsfiddle.net/uxrrwft6/1/
Every execution context has a reference to its outer environment. If a function can’t find a variable, it will look in the outer environment for it. This is called the Scope Chain. 
Example: http://jsfiddle.net/9wt54soz/4/ 
Remember each time a function is called, an execution context is created. This will include the outer lexical environment in which a function will search for variables that may not exist in its own execution context. It will go to its outer reference, “the scope chain.”  

Scope – Where a variable is available in your code. 

Event Queue – The event queue gets looked at by JavaScript when the execution stack is empty. So it really isn’t asynchronous, what’s happening is the browser is putting events into the event queue, but the code that is running is still running line by line and won’t look at the event queue until the current execution stack is empty or ‘finished.’ 
The asynchronous part is really about what’s happening outside of the JavaScript engine. 
Example: http://jsfiddle.net/eqeotzhx/4/
The click events won’t happen until the three second function is done executing. Until then, the click event waits in the event queue, until JS has a chance to look at it. 


Here is another example illustrating the same idea: http://jsfiddle.net/eqeotzhx/7/
Even though the set timeout is finished after one second, it will not fire until the function that is currently running (the three second one) finishes because the set timeout callback is sitting and waiting in the event queue and waiting for the execution stack to be empty (the three second function to complete) 

Types and JavaScript – Dynamic Typing  - You don’t tell JS what kind of data a variable holds, instead, the JS engine figures it out while your code is running. Therefore, a single variable, at different times, could hold different types of values since it’s all figured out during execution. 
Other languages use Static Typing – where you tell the compiler ahead of time what kind of type a variable will be. If you try to put something else, you’ll get an error. 
Primitive (simple) Types (The types you can store) is a type of data that represents a single value (not an object) – There are 6:
1)	Undefined – a lack of existence, a real special value, JS engine sets variables initially to undefined until you give them an assignment. Never set them to undefined yourself. 
2)	Null – represents a lack of existence, you can safely assign this. 
3)	Boolean – true / false 
4)	Number – Floating point number (always some decimals attached to it) 
5)	String – sequence of characters, in single or double quotes 
6)	Symbol – Used in Es6…  not yet fully supported. 
Operators  - A special function that is written differently. Generally take two parameters and return one result. For example, the “+” operator is actually a function. 
Instead of calling +(3, 4), like we’re using it as a function, the JS engine allows us to just write 3 + 4;. It is called infix notation. Its essentially just a function call that returns a value. 
It’s important to understand what’s happening in these functions especially in a dynamically typed language like JS. 

Operator Precedence – Which operator function gets called first. Higher precedence always wins. 
Operator Associativity – What order an operator gets called in: Left to right or right to left when functions have the same precedence. 
The “=” or assignment associativity is right to left. So this is why they are all equal to 4 in this example:
http://jsfiddle.net/6768w09s/1/

Coercion – Converting a value from one type to another. This happens often in JS since it is a dynamically typed language. 
If we attempt to add a string and a number, the JS engine will coerce the number into a string and concatenate the two strings. We did not tell JS to coerce the value, it just did it, it did not throw an error. Understanding that coercion is happening is very important and coercion is a fundamental part of JS since it is a dynamically typed language and will do its best to understand what a value should be. 
Example:  http://jsfiddle.net/6pttwvtq/1/
This is why true is returned for: console.log(1  < 2 < 3); 
This is why true is returned for console.log( ‘3’ > 2 > 0); 
3 is coerced into a number, 3 > 2 returns true and true is coerced into 1, which is greater than 0. 
Example: http://jsfiddle.net/buqLLwLq/6/

Certain operators will coerce values, like comparison operators or double equals (==). In some cases some values are coerced differently depending on the operator, like null will be coerced to zero with comparison operators but will be coerced to undefined with the double equals operator. 
Examples: http://jsfiddle.net/wkwzo6yc/11/
Use triple equals 99% of the time.

Just like we can use Number to coerce, we can also use Boolean to coerce values. JS engine will attempt to coerce any value in if-statements into Booleans. 
Examples: http://jsfiddle.net/edsnxqhx/1/

The || operator returns the value that can be coerced to true. If both can be coerced to true, the first one will be returned:
Example: http://jsfiddle.net/Lb0mtkx3/

Name = name || ‘your name here’ 
If name is undefined, JS will coerce it and it will be undefined as a string, and the string ‘your name here’ will be coerced into true, so ‘your name here’ will be returned. 
Examples: http://jsfiddle.net/t43zcvpj/3/

Frameworks will take advantage of the || operator to prevent collision or overriding. 
Example: http://jsfiddle.net/wshfq2ua/2/

Objects and functions are the same subject 
Objects can have three different properties: a primitive, another object, a function 
In memory, the ‘core’ object will have an ‘address’ in your computer’s memory and the object will have references to these different properties and methods which are also sitting in your computer’s memory. 
Important: Objects sit in memory.  
Namespace – A container for variables and functions. Used to keep variables and functions with the same name separate. JS does not have namespaces, so we can fake them using objects. 
http://jsfiddle.net/vg7mhoxq/4/
http://jsfiddle.net/envac7ko/2/
JSON was adopted from JS object literals over XML because there is much less data sent, so naturally transmission would be faster. 
JS Objects, properties can be wrapped in quotes, however, in JSON, they must be wrapped in quotes. Anything that is valid JSON is valid object literal syntax, but not vice versa. 
http://jsfiddle.net/zym8d30a/1/
In JS, functions are objects. 
First class functions: Everything you can do with other types, you can do with functions: assign them to variables, pass them around, create them on the fly, etc. 
Function – a special type of object, it has all the features of an object with additional special properties. You can attach properties and functions to a function because it’s just an object. 
Think of () of just saying, ‘let’s invoke the special code part of this object to run’ 
You must think that a function is an object and that the invokable code is just a special property of that object. There are other things the function can have attached to it, and other things it can do just like any other object or property. 
Expression: A unit of code that results in a value, it doesn’t have to save to a variable. 
Examples: http://jsfiddle.net/efjppfxr/8/


Function Expressions aren’t hoisted, which makes sense because all variables are set to undefined in the creation phase.
Var myFunc = function() {
	console.log(‘hi’);
}
Therefore, we can’t invoke undefined() and we’ll get an error. 

Value vs Reference 
All primitives are assigned by value (they make a copy) while all objects, including functions, are assigned by reference. 
Immutable -  means it can’t be changed
Examples: http://jsfiddle.net/xp7rrL3q/10/

 Important points:
JSON can be used in JS, you can access properties even though they have quotes
All Objects (including functions) live in memory and they are pointed to by reference or address
JS will try to understand by coercing because it’s a dynamically typed language
Certain operators will coerce some things differently 
An if statement is just a statement, an expression results in a value.
Assigning a function to a variable is an expression. 
They are not hoisted, the variable that is assigned to a function will initially be set to undefined in the creation phase, like all other variables. And the function is created on the fly.
Functions are first class functions, they are treated as primitives, they can be passed around, assign them to variables, create them on the fly, etc. 
Functions are objects, they can attach properties, have a special property of executable code, are a reference, are mutable, live in memory. 
Objects (and functions) sit in memory 
The event queue gets looked at when the execution stack is empty, JS is truly synchronous. 
Undefined is a special value
Operators are functions, they take two parameters and return a result. 
They have precedence over one another and which way they start and end on. 
The || operator will return the first value that can be coerced to true.
Every time a function is called, a new execution context is created and will go to the top of the stack.
Functions will first look inside their environment for the variable, then it will search its outer environment. 

Objects, functions and ‘this’ 
When a function is invoked, a new execution context is created and is put on top of the execution stack. Each execution context has its variable environment where the variables created inside that function live. It has a reference to its outer lexical environment (where it physically sits in the code, like inside a function or inside multiple functions), which tells it how to look down the scope chain. 
If you ask for a variable that isn’t inside that function’s variable environment, it will go out further and further until it reaches the global environment looking for that variable.  
Example of the scope chain: http://jsfiddle.net/9wt54soz/4/
Example of putting put on top and execution order: http://jsfiddle.net/uxrrwft6/1/
Every time an execution context is created (a function is run) the JS engine gives us a variable called ‘this’ and it will be pointing at different object depending on how the function is invoked. 
‘this’ 
Whenever you create a function statement or function expression, ‘this’ will point to the global object, even though there are 3 execution contexts being created and they each get their own ‘this’ keyword, they all still point to the same ‘address’ or location in your computer’s memory. 
Example: http://jsfiddle.net/0z1zvwv6/4/
The JavaScript engine decides where the keyword ‘this’ should be pointing to.
In the case where a function is attached to an object, ‘this’ points to the object in which it’s sitting inside of. 
However, a function within a method, ‘this’ will point to the global object, this is often referred to as a bug in JavaScript. This can be solved by creating a new variable that points to ‘this’ – since objects are reference, this new variable will always refer to the object, and thus you can escape the bug. 
Example: http://jsfiddle.net/ykh88kfj/6/
Remember, the JS engine will always give us ‘this’ when an execution context is created, however, where it points to depends on a few factors. 
JavaScript Arrays 
In most programming languages, they can hold an array of a particular type. In JS you can mix and match types in an array. 
Since arrays are also objects, they are also only assigned by reference. You can use slice() to make a shallow copy (objects assigned inside the array are not cloned). 
Example: http://jsfiddle.net/f0qzyynp/1/

Arguments 
Execution context also creates ‘arguments’ along with the variable environment, the outer environment for the scope chain and ‘this’. 
Arguments – Contains a list of all the values of all the parameters that you pass to a function. It is array-like, it looks like an array, acts like an array, but doesn’t have all the features of an array. People also say this is a bug, it should act like a real JS array. 
Arguments will be deprecated, the new thing is the spread parameter. The extra parameters you send, will get wrapped up into an array
Function (param1, param2, …manyParams) {} 

Also in ES6, you can set default values in the function parameter, instead of using ||. 
Examples: http://jsfiddle.net/2whcmz5g/3/
Whitespace – Invisible characters that create literal ‘space’ in your written code, carriage returns, tbas, spaces, etc.

Immediately Invoked Function Expressions (IIFE)
We can easily create expressions in JavaScript, these are all valid expressions and won’t throw an error:
3;
“This is some string”;
If we want to create an anonymous function expression, we must wrap it in parenthesis, otherwise JS will expect a name since that is valid syntax, and if we provide a name, then we will just be left with a normal function statement. So, we can wrap it in parenthesis to trick the syntax parser, and also invoke it immediately. (it’s an expression, so it is not placed into memory during the creation phase) 
Examples: http://jsfiddle.net/4rs2cefc/18/
IIFE are useful in frameworks to keep code from crashing into each other. 
Example: http://jsfiddle.net/f8ne69hb/5/

Closures  - Closure is a feature built into JavaScript. Any function will continue to have access to any variables its suppose to have access to in its outer environment, even if the execution context of that outer environment has finished executing and is popped off the stack. Its variables will remain in memory for that inner function to use, it has ‘closed in’ its outer variables. Its scope is intact. 
Example: http://jsfiddle.net/7wef1vh8/11/
In a for loop, the reference will always be pointing to i, and in memory, i will be what it has last been incremented to:
Classic Example: http://jsfiddle.net/yuwkbo9u/21/

Using closures to our advantage – Function Factories 
Example: http://jsfiddle.net/csfm9j2y/8/
Closure, Callbacks and function expressions
Example: http://jsfiddle.net/re0zp4h3/2/


Call, Apply, and Bind 
Bind() will create a copy of the function and we can pass in an object for ‘this’ to point to. 
Apply() will do the same, except no copy is made, the function is invoked. 
Bind() will do the same as Apply, but parameters must be passed as an array. 
Apply() can be useful for borrowing functions.
Bind() can be useful for setting permanent parameters. 
Example of bind and apply: http://jsfiddle.net/Lvdzkh8z/7/
Example of function borrowing and currying: http://jsfiddle.net/874Luad5/4/

Functional Programming 
Examples: http://jsfiddle.net/xLxa6jmL/11/

Inheritance – One object gets access to the properties and methods of another object. 
Prototype
All objects in JS, including functions, have a prototype property. 
All objects have a reference to another object, and we call that its prototype.
If a method or property doesn’t exist on an object, JS will search its prototype for it. 
Example: http://jsfiddle.net/ec6aaon9/2/
All objects (objects, functions, arrays) have a prototype, except for the base object. 
JS automatically sets objects, functions, and arrays to their prototypes. On these prototypes we have access to various properties and methods. 
Examples: http://jsfiddle.net/s4szxwmz/7/
This is why we can do something like myArray.push(); Our array doesn’t have this method, so JS will go down the prototype chain and find that it can access it on the prototype. 
Reflection – An object can look at itself, listing and changing its properties and methods. 
Objects can look at themselves and see its own properties and methods 
Example: http://jsfiddle.net/k5xx8va2/14/

Reflection, Prototypal Inheritance, Prototypes, and Different Inheritance
Clear and concise summary: http://jsfiddle.net/v1t9q65m/14/
Constructor Functions: http://jsfiddle.net/s1td5jpu/27/
Every single function has a prototype (along with the name, the invokable code, and all those special parts of a function object). It just hangs out and is never used, unless you are using the function has a function constructor. It’s only used by the new operator. 
Don’t confuse __proto__ with prototype.
The prototype property on a function is NOT the prototype of the function. 

Old Summary: http://jsfiddle.net/ou0p0oyg/21/
Clearer Summary: http://jsfiddle.net/n075f5xs/8/
Faster way to set the prototype of function constructors: http://jsfiddle.net/88umt0te/2/

 ‘new’ are also constructor functions
You can add methods to prototypes of primitives like String.prototype, Number.prototype, Date.prototype, etc. 
When using the new keyword, you always receive an object back:
var a = new String(‘Kyle’); will give us an object back, not a primitive 
Examples: http://jsfiddle.net/3nveam60/17/

Arrays are objects & do not loop through them using prop 
Example: http://jsfiddle.net/2nLszyxo/2/

Pure prototypal Inheritance
Example: http://jsfiddle.net/1g6aLn6n/4/

ES6 classes and extends
Example: http://jsfiddle.net/4tokmt9m/23/
Another Example: http://jsfiddle.net/6kx2xu6j/6/

Example of Object.create() and what it does behind the scenes: https://jsfiddle.net/pj1udkj7/2/

Typeof and instanceof 
They are functions which take one parameter. 
https://jsfiddle.net/chyz634w/6/

Strict Mode 
A way to tell the JS engine to be pickier about what it lets you do. This doesn’t solve every possible case where JS is more liberal, but strict mode can help you prevent errors in certain circumstances.
Example: https://jsfiddle.net/nLbhwa7s/

Prototypal Inheritance Overview 
.prototype vs Object.create() vs Classes 
Example1: https://jsfiddle.net/wk27nyx5/3/
Example2: https://jsfiddle.net/xjnzzfbt/3/

Chaining methods from an object using ‘this’ 
Example: https://jsfiddle.net/ah03yxde/1/
Custom JS Library 
https://jsfiddle.net/29bjjf07/37/

Angular and JavaScript 
In a TypeScript class, ‘this’ refers to the object that is created with the ‘new’ keyword. 
The methods on the class are the prototype:
AppComponent.prototype  === someObj.__proto__

 
https://jsfiddle.net/7kbnmtue/4/

