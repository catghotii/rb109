
# VARIABLES

### Variables as pointers

Variables contain **references to objects** that are stored in memory addresses.

In other words, they are *pointers* to specific address spaces in memory that hold objects with specific values!

##### Code example & language

```ruby
name = "mavis"
animal = name
name = "cat"

puts name
puts animal
```

On line 1, the local variable `name` is initialised with a reference to the string `"mavis"`.

On line 2, in the assignment of one variable to another, the variable on the left of `=`, `animal`, receives a copy of the reference stored in the variable on the right, `name`, with the result that both variables contain references too the same string `"mavis"`.

On line 3, `name` is reassigned to the string `"cat"`, which does not affect the original string referenced by `animal`.

On lines 5 and 6, the `puts` method calls output `"cat"` and `"mavis"`, respectively, demonstrating how variables act as pointers to objects stored in address spaces in memory. In other words, variables do not contain objects themselves but rather contain references to objects stored in memory addresses.

```
# DIAGRAM

# Original state
name --> "mavis" <-- animal

# After reassignment
name --> "cat"
animal --> "mavis"

# OUTPUT

cat
mavis
```

## Variable scope

- A variable's scope is determined by where it was initialised
- Blocks and methods create new scopes

##### Resources

- [RB101-Lesson_2-18.Variable_Scope](https://launchschool.com/lessons/8a39abff/assignments/e3cd8bb9)

### Local variable scope in relation to blocks

Blocks create a new scope for local variables.

- A method invocation followed by a block / passed a block as an argument creates an inner scope in whereby variables initialised inside the block cannot be accessed by the outer scope, but variables initialised in the outer scope can be accessed and modified from within the block.

The variable scope rules of a block state that variables initialised inside a block cannot be accessed from the outer scope, but variables initialised in the outer scope can be accessed and modified from within a block.

##### Code example & language

```ruby
outer_var = "outer variable"

loop do # creates a new scope --> inner / block scope
  block_var = "block variable" # variable locally scoped to the block
  outer_var = "outer variable reassigned from within block" # reassignment of outer variable from within block scope
  puts outer_var # block can access outer variable
  break
end

puts outer_var # reassignment visible from the outer scope
puts block_var # NameError - outer scope cannot access block variables
```

In the outer code, on line 1, the variable `outer_var` is initialised with a reference to the string `"outer variable"`.

On lines 3-8, the `loop` method is called and passed a block in which a local variable `block_var` is initialised to the string `"block variable"`. This variable is locally scoped to the block—since it was initialised inside the block, it is only available to the block and any nested blocks, and not accessible by the outer code. On line 4, the outer variable `outer_var` is reassigned to a new string from within the block, which is output on line 6 by the `puts` method call, printing `"outer variable reassigned from within the block"`, demonstrating how variables initialised in the outer scope can be accessed and modified from within a block. On line 7, the `break` keyword terminates execution of the `loop` method.

Back in the outer code, on line 10, `puts` outputs the value of `outer_var` which was reassigned to `"outer variable reassigned from within the block"` on line 5 in the inner scope. The final `puts` call on line 11 throws a `NameError` message because `block_var`, which was initialised inside the block on line 4, cannot be accessed in the outer scope, which is consistent with the variable scope rules of a block.

### Nested blocks

Nested blocks follow the same rules of inner and outer scoped variables.

Nested blocks can access variables in the surrounding outer scopes, including the main scope, but the surrounding outer scopes cannot access variables initialised in the nested scope.

##### Code example & language

```ruby
outer_var = 1

loop do
  inner_var = 2

  loop do
    innermost_var = 3

    puts outer_var     # 1
    puts inner_var     # 2
    puts innermost_var # 3
    
    break
  end

  puts outer_var       # 1
  puts inner_var       # 2
  puts innermost_var   # NameError

  break
end

puts outer_var         # 1
puts inner_var         # NameError
puts innermost_var     # NameError
```

In the outer scope (main scope), on line 1, the variable `outer_var` is initialised with a reference to the integer `1`.

On lines 3-20, the `loop` method invocation is passed a block as an argument, in which another `loop` method is invoked and passed a block, creating a nested scope. The first `loop` call creates a new scope that has access to the outermost scope's variables and variables initialised inside its own scope, but not to the nested block variables. The nested block has access to variables in the both surrounding scopes as well as its own. Meanwhile, the outermost / main scope only has access to variables in the outer scope and neither of the block or nested block variables.

On line 4, in the inner scope of the first block, the variable `inner_var` is initialised to the integer `2`. On line 6, the nested `loop` method is invoked and passed a block, creating a nested scope in which a variable called `innermost_var` is initialised to the integer `3`. The three `puts` calls on lines 9-11 output the values of the variables in the main scope, inner block scope, and nested scope, respectively—`1`, `2`, and `3`, demonstrating how the nested block has access to all scopes in this code. The `break` keyword terminates the nested `loop` method.

On lines 15-17, in the inner block scope, the first two `puts` calls output the values of `outer_var` and `inner_var`—`1` and `2`—illustrating that the block has access to the outer scope and its own scope, while the last `puts` call throws a `NameError` message because the inner scope cannot access `innermost_var` in the nested scope. Commenting out line 17 would allow execution to resume and on line 20, the `break` keyword terminates the first `loop` method.

Back in the outer code, on line 23, `puts` outputs `1` which is the value of `outer_var`. Lines 24-25 throw `NameError` messages because `puts` in the outer scope does not have access to variables in either block or nested block.

### Peer blocks

- Same variable scoping rules for blocks apply.
- Peer blocks cannot access variables initialised in other blocks.
	- This means that peer blocks can have variables of the same name that have no effect on their peer block's counterparts.

##### Code example & language

```ruby
loop do
  block_var = 1
  puts block_var # 1
  break
end

loop do
  puts block_var # NameError
  break
end

puts block_var    # NameError
```

### (( Control expressions ))

#### while / until / for loops

These loops are not implemented as methods and thus do not create new variable scopes.

- [ ] example

#### Conditional statements

Conditional statements, including their individual branches, do not define separate scopes—they are part of normal execution flow of the program.

*Note:*

If a branch within which a variable initialisation occurs is not executed, the variable will still be initialised for use in the program, though its value will be `nil`.

```ruby
if false
  var = "hello"
end

var #=> nil
```

### Variable shadowing

The variable scope rules of a block state that variables initialised inside of a block cannot be accessed by the outer scope, but variables initialised in the outer scope can be accessed and modified from within a block.

Variable shadowing occurs when a block parameter has the same name as an outer variable. The block parameter shadows or hides the outer variable from the block. This is because the variable scoping rules of a block state that variables initialised in the outer scope can be accessed and modified from within a block.

Note that variable shadowing only occurs with block parameters. If a variable inside the block has the same name as an outer variable, the operation is actually a reassignment of the outer variable.

(Variable shadowing does not happen with method parameters because main scope variables are not accessible to methods. Methods in Ruby have isolated scopes: they cannot access variables defined in the outer scope.)

##### Code example & language

```ruby
val = 10

[1, 2, 3].each do |val|
  val += 1
  puts val
end

puts val
```

On line 1, in the outer code, the variable `val` is initialised with a reference to the integer `10`.

On lines 3-6, the `each` method invocation is passed a block as an argument with a parameter called `val`, which shares the same name as the outer variable initialised on line 1. The variable scope rules of a block state that variables initialised in the outer scope can be accessed and modified from within a block. Since the block parameter has the same name as the outer variable `val`, variable shadowing occurs where the parameter hides the outer variable of the same name, preventing access to it from the block. Therefore any expressions involving `val` within the block deals only with the block parameter `val`.

The `each` method iterates over the array `[1, 2, 3]`, executes the block code for each iteration, and then returns the original calling collection. For each iteration, the parameter `val` is assigned to the value of the current iteration and then `val += 1` reassigns the value of `val` to `val` incremented by `1`, and then `puts` outputs the reassigned value.

```
# ITERATION BREAKDOWN

1st iteration, element at index 0
val => 1
val += 1 => 2
puts val --> outputs 2

2nd iteration, element at index 1
val => 2
val += 1 => 3
puts val --> outputs 3

3rd iteration, element at index 2
val => 3
val += 1 => 4
puts val --> outputs 4

# EACH RETURN VALUE
=> [1, 2, 3]
```

Back in the outer code, on line 8, `puts` outputs the value of the outer variable `val`, which remains `10`, demonstrating that variable shadowing prevented the outer variable from being reassigned from within the block. If the block parameter was called another name, then the expressions inside the block would have indeed reassigned the value of `val` that was initialised on line 1 in the outer code.

### Local variable scope in relation to method definitions

1. Method definitions create a self-contained scope:
	- They can only access variables initialised inside the method or if they are defined as parameters.
	- i.e. they cannot directly access local variables outside of the method scope; they cannot access local variables in another scope

2. Methods can access objects passed in as arguments which are assigned to corresponding method parameters, making them available as local variables to the method

##### Code example & language

```ruby
# methods cannot access local variables in another scope

def print_name_1
  puts name # cannot access local variable in main scope
end

name = "mavis"
print_name_1 # NameError
```

On line 7, the outer local variable `name` is initialised with a reference to the string `"mavis"`. On line 8, the `print_name_1` method is invoked: on lines 3-5, the method definition does not take any parameters and the `puts` call throws a `NameError` message because inside the method scope, there is no local variable called `name` available—it cannot access the outer variable `name`—demonstrating how methods cannot access variables initialised in another scope.

```ruby
# methods can access objects passed as arguments

def print_name_2(str) # method receives reference to same object referenced by outer variable
  puts str # outputs value of string object "mavis"
end

name = "mavis"
print_name_2(name) # object referenced by name passed as argument
```

On line 7, the local variable `name` is initialised in the main scope. On line 8, the `print_name_2` method is invoked and passed the value of `name` as an argument.

On lines 3-5, the method definition takes one parameter called `str` which receives a reference to the same string object that was passed as an argument at method invocation on line 8—`str` is assigned to the string `"mavis"` which is output by `puts` on line 4, demonstrating how methods can access objects   when they are explicitly passed to methods as arguments (in the form of a variable).

### Scope of constants

Constants have different scoping rules from local variables.

They have lexical scope.

In procedural programming, they behave like globals.

### Mutating values vs. reassigning variables

##### Code example & language

*Example: mutating values*

```ruby
name = "cat"
person = name
name << "dog"

puts name   # outputs "catdog"
puts person # outputs "catdog"
```

On line 1, the local variable `name` is initialised with a reference to the string `"cat"`. On line 2, in the assignment from one variable to another, the local variable `person` receives a copy of the reference stored in `name`, with the result that both variables contain references to the string `"cat"`.

On line 3, the destructive `<<` shovel operator is called on `name` and passed the string `"dog"` as an argument which mutates the value of the original string to `"catdog"`. This mutation to the original object occurs in place rather than creating a new object (as with reassignment operations), which means that the modified string value can be seen when any of its references are examined.

The two `puts` method calls on lines 5-6 output the same string value `"catdog"` and `"catdog"`—even though the mutating `<<` operator was called on `name`, `person` also references the same string object, so the output is the same.

*Example: reassigning variables*

```ruby
name = "cat"
person = name
name = "dog" # variable name has been reassigned to a new string

puts name   # outputs "dog"
puts person # outputs "cat"
```

On line 1, the local variable `name` is initialised with a reference to the string `"cat"`. On line 2, in the assignment from one variable to another, the local variable `person` receives a copy of the reference stored in `name`, with the result that both variables contain references to the string `"cat"`.

On line 3, `name` is reassigned to a new string with the value `"dog"`. This reassignment operation creates a new object rather than modifying the original object, which changes what the variable references. Effectively, the original object is disconnected from the variable `name`, but this has no effect on the original object or any of its references—that is, `person` still references the original string `"cat"`.

On line 5, `puts name` outputs the string `"dog"` which is the reassigned value on line 3, while `puts person` outputs the original string `"cat"`, demonstrating how variable reassignment does not mutate the original object but rather creates a new object, changing what the variable references.

# METHODS

##### Resources

- https://launchschool.com/lessons/8a39abff/assignments/1be6d04d

### Method definition vs method invocation

**Method definition** --> define a method using the `def..end` keywords
- sets a certain scope for any local variables pertaining to the method parameters, what it does to those parameters, and the level of interaction with blocks

**Method invocation** --> when a method is called
- using the scope set by the method definition
- if the method is defined to use a block, then the scope of the block provides additional flexibility in terms of how the method invocation interacts with its surroundings

### Methods and blocks

- a block is *part* of a method invocation—
	- a method invocation followed by a block *defines* a block in Ruby
	- the block acts as an argument to the method (in the same way that a local variable can be passed an argument to a method at invocation)
- methods and blocks can interact with each other
	- the level of interaction is set by the method definition and then used at method invocation

##### Code example

*Example: arguments used as method parameters*

```ruby
word = "Hello"

# method parameter not used

def print_greeting(str)
  puts "Hi!"
end

print_greeting(word) # outputs "Hi!"

# method parameter is used

def print_greeting_2(str)
  puts str
end

print_greeting_2(word) # outputs "Hello"
```

On line 1, the outer variable `word` is initialised with a reference to the string `"Hello"`.

On line 9, the `print_greeting` method is invoked and passed the value of `word` as an argument. Lines 5-7 define the `print_greeting` method that takes one parameter called `str` which receives a reference to the same string object referenced by `word` when the method invocation is passed an argument. However, the parameter `str` is not used; instead `puts` outputs the string `"Hi!"`.

On line 17, the `print_greeting_2` method is called and passed the value of `word` as an argument. This time, the method definition on lines 13-15 utilise the method parameter `str`: `puts` outputs the string `"Hello"`.

### Passing and using blocks with methods

How arguments are used by methods—as a parameter or a block—depends on how the method is defined.

The level of interaction between methods and blocks is established during method definition and used at method invocation.

During method definition:
- To execute a block that's passed to a method call, the method definition needs to set the level of interaction with blocks using the `yield` keyword which executes the block once.

During method invocation:
- The level of interaction between the method and block is used during method invocation.
- Since blocks can access local variables outside of its own scope, there is additional flexibility afforded to the method during invocation—i.e. blocks can use outer variables that would not otherwise be directly accessible to method definitions.

##### Code examples & language

- *Example: block not executed*

```ruby
def print_greeting(str)
  puts "Hi!"
end

word = "Hello"

print_greeting do
  puts word
end

# OUTPUT
# Hi!
```

On line 7, the `print_greeting` method is invoked with a block, but the method is not defined to execute the block.

- *Example: block executed*

```ruby
def print_greeting(str)
  yield
  puts "Hi!"
end

word = "Hello"

print_greeting do
  puts word
end

# OUTPUT
# Hello
# Hi!
```

Inside the method definition, on line 2, the `yield` keyword controls the level of interaction between the method and the block—in this case, the block is executed once. The block has access to the outer variable `word` initialised on line 6, so the string `"Hello"` is output when the block is executed.

This demonstrates how methods can be defined to use blocks to provide additional flexibility in terms of how the method invocation interacts with surrounding scopes. While method definitions cannot directly access variables initialised outside of the method definition, defining methods to use blocks allows local variables outside of the method to be accessed during method invocation.

- *Example: `Array#map` method passed a block as an argument*

Method invocations with blocks are not only limited to executing the code in the block passed. Depending on the method definition, the method can use the *return values of the block* to perform some other action. 

```ruby
word = "hello"

[1, 2, 3].map { |num| word } # => ["hello", "hello", "hello"]
```

On line 1, the outer variable `word` is initialised with a reference to the string `"hello"`.

On line 3, the `map` method is called on the array `[1, 2, 3]` and passed a block as an argument. The `Array#map` method iterates over the calling collection, executes the code in the block, and returns a new array containing transformed values for each element in the collection.

The `#map` method does not have direct access the variable `word`, but by passing a block (and that `map` calls) that does have access to `word` due to its variable scoping rules, the block can use the value of the outer variable`word` to determine the transformed values to populate the `map` return array. Effectively, the `map` method can access local variables outside of the method definition through its interaction with the block.

### Parameters vs arguments

In a method definition, the parameter following the method name establishes what input the method accepts. Parameters allow input/data from outside the method to be accessed within the method during invocation. During method invocation, the parameter is filled with values specified by corresponding arguments.

Arguments are the actual values that are passed when a method is called and assigned to the method parameter. Objects are explicitly passed to a method—in fact, methods in Ruby receive references to objects when arguments are passed during method invocation. When a method is called, the code in a method body is executed and the local variable is evaluated to value of the corresponding object that's passed in.

There is a direct correspondence between the syntax of the parameter list in a method definition and the syntax of the argument list in a method invocation.

##### Code example & language

```ruby
def some_method(first, second)
  puts first
  puts second
end

some_method("cat", "dog") # outputs "cat" and "dog" on separate lines
some_method # ArgumentError - wrong number of arguments (given 0, expected 2)
```

On line 6, the `some_method` method is called and passed two strings—`"cat"` and `"dog"`—as arguments.

The method is defined on lines 1-4: it takes two parameters `first` and `second` that are assigned to the values that were passed as arguments, respectively. On line 2, `puts` outputs `"cat"` and on line 3, `puts` outputs `"dog"`.

On line 7, an `ArgumentError` message is thrown because the `some_method` invocation is not supplied any arguments. The method definition takes two parameters, which means it expects two arguments to be passed during method invocation in order to execute the method without any errors.

### Default parameters

In a method definition, default parameters allow methods to be called without passing arguments. If no argument is passed during method invocation, the default parameters are evaluated and the method parameter is assigned to the preset value.

Default parameters can be expressions and not specific values—this is because they are evaluated during method invocation.

Default parameters must be the last parameter defined in a method definition's parameter list.

##### Code example & language

```ruby
def greeting(name = "stranger")
  puts "Hello, #{name}!"
end

greeting("Cat")
greeting
greeting(nil)
```

On line 5, the `greeting` method is called and passed the string value `"Cat"` as an argument. On lines 1-3, the `greeting` method is defined: since the method invocation was passed an argument, the parameter `name` is assigned to the value that was passed to the method, `"Cat"`, and the `puts` call outputs the interpolated string `"Hello, Cat!"`.

On line 6, `greeting` is invoked but not passed an argument. For this method call, the default parameter defined on line 1 is evaluated; the parameter `name` is assigned to the default value `"stranger"`, so `puts` outputs `"Hello, stranger!"`, demonstrating how default parameters are evaluated and used during method invocation when the method is called without any arguments supplied.

On line 7, `nil` is passed as an argument to the `greeting` method during invocation which overrides the default parameter. `puts` outputs `"Hello, !"`.

### Implicit vs explicit return values

Methods return the evaluated result of the last expression in the method body unless an explicit `return` keyword is reached prior to the last line. This evaluated result is called the method return value. Note that using `return` on the last line of the method code is not required since the method return value is implicitly returned back to the calling code / where it's called form.

If `return` is executed in the middle of the body, the rest of the code in the method is ignored; execution resumes after the method call. The return value of the method is the value following `return`, which is explicitly returned.

##### Code example & language

```ruby
def some_method(param)
  return "Explicit return!" if param == nil # value after return keyword is returned
  param # last evaluated expression is implicitly returned
end

some_method(nil) # => "Explicit return!"
some_method("Implicit return!") # => "Implicit return!"
```

On line 6, the `some_method` method is called and passed `nil` as an argument, which is assigned to the method parameter `param` on line 1. The method definition, on lines 1-4, is evaluated: on line 2, an explicit `return` keyword is reached if the conditional `if` expression evaluates as true. `if param == nil` evaluates as true, so `return` is executed and the method explicitly returns the string `"Explicit return!"`.

On line 7, `some_method` is called again but passed a string value of `"Implicit return!"` as an argument. Since `if param == nil` evaluates as false, the `return` keyword is not executed. On line 3, `param` evaluates to the string `"Implicit return!"`. The method return value is the same as the evaluated result of the last expression in the body (unless an explicit `return` keyword is reached prior to the last line), so `"Implicit return!"` is what the method implicitly returns.

### Output vs return

https://launchschool.com/books/ruby/read/methods#putsvsreturnthesequel

Expressions do something but they also return something. The action performed is not necessarily the same as the value returned.

The method return value is the last evaluated expression in the method body, which is implicitly returned, unless an explicit `return` keyword is reached prior to the last line. The `return` key word is not required in order for the method to return a value.

If `return` is reached in the middle of the method, the rest of the code in the method is ignored and the expression/value following `return` is what the method returns.

Methods return their value back to the calling code / where it's called from. 

(Empty methods return `nil`.)

##### Code example & language

```ruby
# explicit return prior to last line

def catify_method(str)
  return catified_name = "cat" + str
  puts catified_name # not executed
end

explicit_return = catify_method("girl")
p explicit_return # outputs "catgirl"

# without early return

def catify_method2(str)
  catified_name = "cat" + str
  puts catified_name
end

implicit_return = catify_method2("boy") # implicitly returns nil / outputs "catboy"
p implicit_return # outputs nil
```

On line 8, the variable `explicit_return` is initialised with a reference to the return value of the `catify_method` with the value of `"girl"` passed as an argument. The `catify_method` method is defined on lines 3-6: the first line in the method (line 4) uses an explicit `return` keyword to return the evaluated result of the assignment  `catified_name = "cat" + str`, which is returned back to the calling code on line 8—`explicit return` is assigned to the explicitly returned method value "catgirl". 

On line 9, `p` outputs the string "catgirl", demonstrating that the `return` keyword terminated the method early, not executing the remaining lines in the method body, and explicitly returns the value following `return`.

The other method in this code called `catify_method2` demonstrates what happens when there is no explicit `return` in a method definition. On line 18, the variable `implicit_return` is initialised with a reference to the return value of the method invocation with `"boy"` passed as an argument. This time, the execution of the method call terminates once it reaches the end of the last line of the method body, returning the evaluated result of the last line, `puts catified name`, which returns `nil`, so the method return value is also `nil`. Thus, `nil` is passed back to the calling code on line 18, and `implicit_return` is assigned to `nil`, which is output by `p` on line 19.

#### Coding tip! Side effect or meaningful value?

- Methods should either display output or return a meaningful value, not both.
	- Decide whether the method has side effects with no (meaningful) return value, or has no side effects with a meaningful return value.
	- Method name should reflect what the method does (e.g. `display_` or `total` (implicitly returned))

Side effects:
- Something that changes the state of a program outside the bounds of a programming limit, such as a method, class, or a block.
- If that code changes anything that is not local to that code, it has side effects.
- Printing something is a side effect because it changes the contents of the display (which is not local to the code)

### Using method return values as arguments to other methods

*Method calls as arguments* - https://launchschool.com/books/ruby/read/methods#methodcallsasarguments

Method return values can be immediately passed as arguments to other methods calls, creating a chain of calls.

##### Code examples & language

```ruby
# passing method return value as an argument to another method call

# method that only returns a meaningful value

def catify(str)
  "cat" + str
end

# method that only produces output

def display_name(str)
  puts str
end

# immediately pass return value as an argument --> method call chain

display_name(catify("girl")) # outputs "catgirl"

# printing method uses nested method calls as argument

p(display_name(catify"girl")) # outputs nil --> must be aware of what methods return
```

On line 17, the `display_name` method is called and passed the return value of the `catify` method call with "girl" passed as an argument. First, the `catify` method call is evaluated to determine the return value before passing it immediately to `display_name` as an argument.

On lines 5-7,  the method definition for `catify` is defined: the method invocation returns the string "catgirl" which is immediately passed to `display_name` as an argument. Inside `display_name` defined on lines 11-13, `puts` outputs "catgirl" (and returns `nil` which is not used in this code).

This code demonstrates how method calls can be nested within other method calls, or rather how method return values can be passed as arguments to other method calls.

### Mutating vs non-mutating methods

Arguments (i.e. actual values of objects passed) can be permanently modified / mutated when a method is called. This depends on whether the method definition performs mutating or non-mutating operations on the original object.

A mutating operation performed inside a method can modify the corresponding object that's passed as an argument during method invocation. Rather than creating a new object, the original object is modified in place. The mutation to the object can be seen through any of its references, including variables in another scope besides the method definition scope.

A non-mutating operation (e.g. reassignment) inside a method definition does not mutate the original object during method invocation. Instead non-mutating operations return new objects and changes what a variable references, disconnecting the original object from the local variable.

##### Code example & language

```ruby
# non-mutating method inside method definition

def not_mutate(string)
  string.upcase
end

# mutating method inside method

def mutate(string)
  string.upcase!
end

word = "hello"

p not_mutate(string) # outputs "HELLO"
p string # outputs "hello" --> original string not mutated

p mutate(string) # outputs "HELLO"
p string # outputs "HELLO" --> original string mutated
```

This code demonstrates the difference between non-mutating and mutating methods inside method definitions.

On line 13, the variable `word` is initialised with a reference to the string "hello".

On line 15, `p` outputs the return value of `not_mutate` with the value of "hello" referenced by `string` as an argument. Lines 3-5 define the method: the non-destructive/non-mutating `upcase` method is called on the local variable `string` that references the same string passed as an argument, which returns a new string of lowercase letters to uppercase "HELLO", which is the value that's output by `p` in the calling code. On line 16, `p word` outputs "hello" which is the original string assigned to `word` on line 13—the method call did not mutate the original string because the non-mutating operation returned a new value rather than modifying the contents of the original string.

On line 18, the `mutate` method is called and passed the value of "hello" as an argument. This time, the destructive/mutating method `upcase!` is called on the local variable `string`, which permanently modifies the original string to "HELLO". Thus, `p` on line 19 outputs "HELLO" since the mutating method inside `mutate` modified the original string.


# EVERYTHING ELSE YOU NEED TO KNOW

### Mutable vs immutable data types

Immutable objects cannot be modified after they are created. In Ruby, numbers are immutable.

Mutable objects can be modified after they are created if mutating operations are performed on them.

```ruby
# integers are immutable

val = 10
puts "before reassignment: val is #{val} and object id is #{val.object_id}"

val += 1
puts "before after reassignment: val is #{val} and object id is #{val.object_id}"

# strings are mutable

word = "hello"
puts "before mutation: word is #{word} and object id is #{word.object_id}"

word[0] = "j"
puts "before mutation: word is #{word} and object id is #{word.object_id}"
```

### Object passing: pass-by-reference-value

#### Pass-by-reference vs pass-by value

"Pass by value" is when a method receives a copy of the original object during method invocation with an argument passed. Operations performed on the object within a method have no effect on the original object outside of the method.

"Pass by reference" is when a method receives a reference to the original object during method invocation that's passed an argument. Operations performed on the object within a method can affect the original object outside of the method depending on whether those operations are mutating, non-mutating, and the order they are performed in.

##### Code example & language

```ruby
# Ruby appears to pass-by-value if the variable is reassigned

def catify_name(word)
  word = "cat" + word
end

person = "girl"

catify_name(person)
p person # outputs "girl"

# mutating operations performed on the argument mutates the original object

def catify_name!(word) # the bang ! signifies destructive behaviour by convention
  word.prepend("cat")  # prepend is destructive but is not suffixed with !
end

catify_name!(person)
p person # outputs "catgirl" --> original string mutated
```

#### What Ruby does

Ruby methods receive references to the original object during method invocation, but only mutating operations performed on the argument or caller will affect the original object outside of the method.

Ruby *appears* to pass-by-value when reassignment operations occur because reassigning variables within a method has no effect on the original object outside the method.

##### Code example & language

```ruby
# reassignment with non-mutating operation

def add_animal(arr, word)
  arr = arr + [word]
end

animals = ["sloth", "dog"]
add_animal(animals, person)
p animals # outputs ["sloth", "dog"]

# reassignment with mutating method

def add_animal2(arr, word)
  arr = arr << word
end

animals = ["sloth", "dog"]
add_animal2(animals, person)
p animals # outputs ["sloth", "dog", "catgirl"]

```

### Truthiness

The concept of "true" and "false" is crucial for building conditional logic in programming.

Truthiness in conditionals is based on what values Ruby considers to be true or false rather than the boolean objects `true` and `false`.

#### true or false

Saying an 'evaluates as true / false' is not the same as saying the expression 'evaluates to `true` / `false`'—the former implies truthiness, while the latter maintains that only `true` is `true` and `false` is `false`.

Boolean data types have the only purpose of conveying `true` or `false`.

Truthiness differs from boolean objects because ***Ruby considers everything to be truthy except for the two falsy values `false` and `nil`***—that is, more than just `true` evaluates as true, and more than just `false` evaluates as false.

Truthy values includes empty strings, arrays, hashes and in the integer `0` which might be considered falsy in other programming languages.

#### Conditional contexts

Conditional expressions do not have to be directly compared to `true` or `false`. Instead Ruby uses the "truthiness" of an expression to determine conditional logic.

Ruby does not specifically check for whether the expression specifically evaluates to `true` but whether it does not evaluate to either `false` or `nil` --> besides two falsy values, everything else is truthy!

##### Code examples & language

```ruby
def truthy_or_falsy(param)
  if param
    puts "It's truthy!"
  else
    puts "It's falsy!"
  end
end

word = "I'm truthy"
truthy_or_falsy(word) # "It's truthy!" --> strings evaluate as true
truthy_or_falsy(0)    # "It's truthy!" --> numbers, including 0, evaluate as true
truthy_or_falsy("")   # "It's truthy!" --> empty string evaluates as true
truthy_or_falsy([])   # "It's truthy!" --> empty array evaluates as true
truthy_or_falsy(nil)  # "It's falsy!"  --> nil evaluates as false

```

#### Avoid assignment within a conditional

```ruby
if some_variable = 2
  puts "this will always output"
else
  puts "you will not see this"
end
```

- The expression will always evaluate as true because assignment is occurring (not a comparison operator)

Since everything in Ruby is truthy except for `false` and `nil`, `some_variable = 2` on line 1 is an assignment operation (not a comparison using `==`!) that returns `2`, which is truthy value—so the `if` branch is executed and `puts` outputs "this will always be output".

## Iteration methods

### each

When passed a block as an argument during method invocation, the `each` method iterates over a collection, executes the code in the block for each iteration, and returns the original collection.

```ruby
numbers = [1, 2, 3]

return_array = numbers.each do |val|
  val + 100
end

p return_array
```

### map

When passed a block as an argument during method invocation, the `map` method iterates over a collection, executes the code in the block on each iteration, and returns a new array containing transformed values/elements based on the block return values.

```ruby
numbers = [1, 2, 3]

return_array = numbers.map do |val|
  val + 100
end

p return_array # outputs [101, 102, 103]
```

### select

When passed a block as an argument during method invocation, the `select` method iterates over a collection, executes the code in the block on each iteration, and returns a new array containing selected elements based on the truthiness of the block return values.

```ruby
numbers = [1, 2, 3]

return_array = numbers.select do |val|
  val.odd?
end

p return_array # outputs [1, 3]
```



# OTHER METHODS!



# OTHER NOTES

### Coding tips

#### Methods should be at the same level of abstraction

- working on methods in isolation
- implementation details left to the actual method --> name of method
- method names that specify "what" to do vs "how" (programmer's concern)

#### Methods that mutate

- `update_total`
- compartmentalisation

#### Displaying output

