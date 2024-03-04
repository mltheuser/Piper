# Piper

<img src="https://github.com/mltheuser/Piper/assets/25958978/8e178478-3dd8-4652-b3c1-407bbe427a71" alt="mascot" width="200"/>

The objectively best programming language.

### Variables

Define a Variable by storing a value as a full text name.

```
- Store 2 as My Cool Number
```

Variable names are case insensitive.

```
- Store 2 as My Cool Number
- print mY COol numbER
```

### Functions

Define a function by giving it a full text name and then marking parts of that name as the name of input arguments. Function names are case insensitive.

```
Do something with x and y
    - Given x
    - Given y
    - ...
```

You can then call this function like so:

```
- Store 1 as My Number
- Store 2 as My Other Number
- Do something with My Number and My Other Number
```

The position of the arguement names must be unique. In the following example there are multiple occurances of the substring "x".

```
f(x)
    - Given x <-- OK
    - ...

execute f(x)
    - Given x <-- Not OK
```

Here is how you return something from a function. You can only return a single variable.

```
My Function
    - Store 2 as X
    - Return X
```

To return nothing leave out the return statement.

```
Display X
    - Print X
```

Function implementations are chosen based on signature and required input properties. From the name and signature a template string is constructed that function invocations are matched against. Then for all potential matches the requirements for the input arguments are tested. More than 1 valid function results in an error.

### Aliases

You can add alternative full text names to Functions as long as the signature is maintained.

```
Use X to do something
    - Given X

Use X to do something also known as Do something with X <-- OK

Use X to do something also known as Exclusively do something with X <-- Ambiguous
```

### Essential System Calls

Use "Print" to show text in the console.

```
- Store 2 as X
- Print X
```

Use "Quit" to exit the program.

```
- Quit
- Print 2 <-- Unreachable
```

### Pipeing

You can also create functions without any named paramters in their name that still expect input arguments.

```
Display
    - Given X
    - Print X
```

You call functions like this by chaining Function calls with compatible output and input properties.

```
Get Number
    - Return 2

Display
    - Given X
    - Print X

- Get Number
- Display <-- Prints 2
```

Use aliases to transform a function with named arguemnts to a function with piped arguments and vice versa.

```
Display also known as Display X <-- OK

Display X also known as Display <-- OK
```

### Conditional Logic

If/Else Condition are created like this:

```
- If Check My Condition
    - do something
- Else
    - do something else
```

### Loops

While loops are created like this:

```
- While Check My Condition
    - do something
```

### Promises

Promises help you organise your code by setting up contracts about your Variables.

```
- Store 15 as X
- Promise X > 10
- Store 3 as X <-- Broken Promise
```

On creation the condition is asserted. The promise is then attached to the Variable. A Promise can not be broken, so it will stay attached to the Variable until the variable gets deleted. Every time the variable is changed the promise will be asserted again.

```
Promise X also known as Require X

Safely divide X by Y
    - Given X
    - Given Y
    - Require Y != 0
    - Return X / Y
```

Requirements on function arguments are used to resolve the right function implementation on a function call.

Promises are a powerfull tool that allow you to implement a lot of common contracts seen in other languages like Constants, Progressive Typing or Structs.

### Comments

There are no comments.

The extreme freedom in naming functions and by that extension promises should be sufficent to express yourself though code alone. Use Promises to write comments about your code that can not lie!

### Types

There are only 4 base types.

- Truth Value
- Number
- Character
- Table

Base types are infered from literals.

```
- Store True as X
- Store False as X

- Store 2 as X
- Store -1 as X
- Store 0.32 as X

- Store 'a' as X

- Store new Table as X
```

Use Promises to add strong typing.

```
- Store True as X
- Promise X is Truth Value
- Store 5 as X <-- Broken Promise
```

Here is how to do this for the other types:

```
- Store 2 as Y
- Promise Y is Number

- Store 'a' as C
- Promise C is Character

- Store new Table as T
- Promise T is Table
```

#### Truth Values

```
- Store True as A
- Store False as B
- A is equal to B
- not A
- A and B
- A or B
```

#### Numbers

```
- Store 1 as A
- Store 2 as B
- A is equal to B
- A less than B
- A plus B
- A minus B
- A times B
- A divided by B
- A to the power of B
```

#### Tables

```
- Store new Table as T
- Store 'a' at key 15 of T
- Get value at key 'b' of T
- List all keys of T
- List all values of T
- T has Key X
```

"Get value at key K of T" will quit the program when used with an invalid key. The standart libary provides safer access operations using Result Tables.

### References

By default the value of a variable is copied.

```
- Store 2 as X
- Store X as Y
- Store X + 1 as X
- Print X <-- 3
- Print Y <-- 2
```

```
Add 1 to X
    - Given X
    - Store X + 1 as X

- Store 2 as X
- Add 1 to X
- Print X <-- 2
```

You can mark function arguments to be refernces instead of copied values by using "as Reference".

```
Add X to Table at Key
    - Given X
    - Given Table as Reference
    - Given Key
    - Store X at Key Key of Table

- Store new Table at T
- Add 2 to T at 'c' <-- T is automatically passed by reference as required
- Print Get Value at Key 'c' of T <-- Prints 2
```

A Reference to a Reference will still point to the original reference owner.

```
Print X with extra steps
    - Given X as Reference
    - Display X

Display X
    - Given X as Reference
    - Print X

- Store 2 as X
- Print X with extra steps <-- Prints 2
```

Assigning a Reference to a new Variable will copy the referenced value.

```
Make Promise
    - Given X as Reference
    - Store X as Y
    - Promise Y is Number <-- Promise dies with the Y Variable.

- Store 2 as X
- Make Promise
- Store 'c' as X <-- OK
```

You can NOT return a Reference from a Function.

```
Reference to X
    - Given X as Reference
    - Return X <-- Not OK
```

### Single Owner Principle

At each point in time there is only one Variable that owns a piece of data / memory.

You can apply promises to a reference. The Promise will then be stored on the reference owner.

```
Force X to be Number
    - Given X as Reference
    - Promise is Number

- Store 2 as X

- Force X to be Number <-- OK

- Store 'c' as X <-- Broken Promise
```

Data lives as long as the owning variable lives. As soon as the variable stops beeing used the data is freed and References to it become invalid. A Variable is considered no longer used when we leave it's scope or there are no more instruction in the rest of the code block that use it.

Returning a Variable from a Function transfers the Ownership to the first Variable it is stored as in the calling scope.

### Captures

When you make a Promise you are implicitly creating a function with a single input argument (the data you are making the promise about) and a Truth Value as the Output (whether the Promise holds or not). Any other arguments used when making the promise will have their value copied when the promise is made. They are captured.

This is done for every Variable involved in the Promise. For each variable V it will create a seperate promise with all other variables captured and store it on the owner of V. References are replaced by the Owner they reference.

```
- Store 2 as X
- Make Promise

Make Promise
    - Given X as Reference
    - Store 3 as Y
    - Promise X == Y and X == X
```

This Promise for example will result in two seperate promises.

```
X -> X == 3 and X == X
Y -> 2 == Y and 2 == 2
```

There needs to be at least one varaible involved in a Promise.

### Inheritance of Promises

Promises attached to Variables are not inherited when the value is copied but are inherited on a Reference. In regards to promises references behave exactly the same as the reference owner (they can be used interchangably).

```
Set X to 3
    - Given X as Reference
    - Store 3 as X

- Store 2 as X
- Promise X equals 2

- Store X as Y
- Store 3 as Y <-- OK

- Set X to 3 <-- Broken Promise
```

When storing data in a table, promises are never inherited. You have to make promises about a Tables fields directly.

```
- Store new Table as T
- Promise Get Value at Key 0 of T is Number
- Store 2 at Key 0 of T <-- OK
- Store 'c' at Key 0 of T <-- Broken Promise
```

This also applies when storing References on a Table since this just copies the referenced value.

```
Store Reference to X at first key of T
    - Given X as Reference
    - Given T as Reference
    - Store X at Key 0 of T

- Store new Table as T
- Store 2 as X
- Store Reference to X at first key of T
- Promise Value at Key 0 of T is Number

- Store 'c' as X <-- OK
- Store 'c' at Key 0 of T <-- Broken Promise
```

When returning a value from a function the promises are always inherited.

```
My Number
    - Store 2 as X
    - Promise X is Number
    - Return X

- Store My Number as Y
- Store 'c' as Y <-- Broken Promise
```

Promises live on the variable that owns the data so you can add new Promises after making a reference and they will still be reflected.

### More Function Customization

Use "as Chracters" on an argument to interpet it's location in the function name as a List of Characters.

```
T.K
    - Given T
    - Require T is Table
    
    - Given K as Characters <-- List of Characters
    - Return Get Value at Key K of T

- Store new Table as T
- Print Get Value at Key "OK" of T
- Print T.OK <-- Same as above
```

This also works in the middle of a function name.

```
Display Message on Screen
    - Given Message as Characters
    - Print Message

- Display Hello World on Screen <-- Prints "Hello World"
```

You can also provide a list of comma seperated values for an argument using "as List".

```
Display X on Screen
    - Given X as List
    - Foreach Element in X
        - Print Element

- Store "world" as X
- Display "hello", X on screen
```

---

### You did it!
 That's all the build-in features. From here we can write a standard libary to make life a bit easier.

---

#### Lists

Lists are Maps that have consecutive natural numbers as keys starting at 1.

```
X is List
    - Given X
    - Reuqire X is Table
    - ... check all the keys
```

```
[Values]
    - Given Values as List
    - Return Values
```

```
Size of X
    - Require X is List
    - ... Count from 1 up till error
```

```
- Store [10, 'c', 20] as L
- Print Size of L <-- 3
- Print L[1] <-- 10
```

#### Results

The standard libary handels Errors as Values.

```
X is Result
    - Require X is Table
    - Return X has Key "Ok" or X has Key "Error"
```

A Result is used with 2 key methods:

```
R is Ok
    - Require R is Result
    - Return X has Key "Ok"

Unpacked R
    - If R is Ok
        - Return Get Value at Key "OK" of R
    - Else
        - Return Get Value at Key "Error" of R
```

Here is how we can create a safer access operation.

```
T[K]
    - Given T
    - Given K
    - Require T is Table
    - Store new Table as R 
    - If T has key K
        - Store Get Value at position i as V
        - Store V at Key "OK" of T
    - Else
        - Store "Key not found." at Key "Error" of T
        - Promise Get Value at Key "Error" of T is String
    - Return R

Main
    ...
    - While T[i] is OK
        - Store Unpacked T[i] as V
        - Print V
    - Print Unpacked T[i] <-- Promises to be String. (No reassignment inbetween so table field Promise still holds but can be broken)
```

#### Highlevel Constraints

We can descriminate between variables and constants using promises.

```
Promise X is Constant
    - Given X as Reference
    - Store X as V <-- Copies Value of X
    - Promise X == V <-- V gets captured and promise is applied to reference owning variable

- Store 2 as X
- Promise X is Constant
- Store 3 as X <-- Broken Promise
```

We can enforce a certian structure on a table using promises. This way we can create structured data containers (structs).

```
Promise T has no other keys
    - Given T as Reference
    - Store List of all Keys of T as Keys
    - Promise List of all Keys of T equals Keys

new Box
    - Store new Table as B

    - B.width = 10
    - Promise B.width is Number
    - B.length = 15
    - Promise B.length is Number
    - B.height = 20
    - Promise B.height is Number

    - Promise B has no other keys

    - Return B

- Store new Box as My Box
```

#### Recursive Structures

Here is how you could create a Tree.

```
X is Leaf:
    - Given X as Reference
    - Return X is Table and X.left is OK and X.right is OK

Promise X is Node:
    - Given X as Reference
    - Return X is Table and X is Empty <-- checks if list of keys is empty

new Tree Node
    - Store new Table as Node
    - Promise Node is Table

    - Node.left = new Table
    - Promise Node.left is Leaf or Node.left is Node

    - Node.right = new Table
    - Promise Node.right is Leaf or Node.right is Node

    - Promise Node has no other keys
    
    - Return Node

- Store new Tree Node as N
- Store new Tree Node as N2
- N.left = N2 <-- This copies the value of N2
```

The Table N now owns the Data in N.left. There are now two versions of this data one that will die with the Variable N2 and one that will die with the Variable N.

If you need a structure where data is accessible through multiple paths things become tricky. It might be worth to keep the owning variables and the structure over it in the same table and come up with your own pointer mechanism.

Here is an example of how to create a linked list loop.

```
TODO...
```
