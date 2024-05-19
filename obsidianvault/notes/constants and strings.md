a constant is a value that may not be changed during program execution
	two types:
		-named constants
		-literal constants
to declare a constant use ```const``` keyword ex. ```const double gravity { 9.8 };```

*note: avoid magic numbers (numbers that have an unclear meaning) use constexpr variables instead*

**object-like macros w/ substitution text**
	ex. ```#define MY_NAME "Roy"```
	When preprocessor processes the file containing the code, it replaces MY_NAME

**strings**
	In C and C++, strings are not a fundamental data type
		1. all c-style strings have a implicit null terminator character to indicate the end of a string
			```hello = 'h' 'e' 'l' 'l' 'o' '\0'```

**constant expressions and compile-time optimizations**
	c++ compilers are given a lot of freedom to optimize programs as long as it follows the **as-if rule** (the compiler can modify a program in whatever way to produce more optimized code as long as those modifications do not affect a program's observable behavior)
		many compilers are able to evaluate some expressions at compile-time so it doesn't have to during run-time
			you can use the ```constexpr``` keyword to make a variable a constant that *MAY* get compile-time optimized
				ex. ```constexpr double gravity { 9.8 };```
			note: a function can also use the keyword for it to be compile-time evaluated if eligible
			you can use the ```consteval``` keyword for functions that *MUST* get compiled-time evaluated


**conditional operator**
	provides a shorthand method for doing the if-else statement

| Operator    | Symbol | Form      | Meaning                                                    |
| ----------- | ------ | --------- | ---------------------------------------------------------- |
| Conditional | ?:     | c ? x : y | If conditional c is true, evaluate x, otherwise evaluate y |
	can use it to initialize a variable expression uniquely like this: 
```cpp
constexpr bool inBigClassroom { false };
constexpr int classSize { inBigClassRoom 20:30 };
```
the condition can also be a compound expression-remember parenthesis-such as ((x > y) ? x : y);


****

**introducing std::string**
	-C-style string *variables* are odd and dangerous
	-C++ has two additional string types into the language that are much easier and safer
		std::string and std::string_view
			*these are class types, not fundamental types*
		to use these, you have to ```#include <string>```
			then you can create objects of the type like
				```std::string name {"NAME"};```
	-if not enough memory to store a string, it will dynamically allocate during run-time
	
**string input**
	-using std::string with std::cin may be surprising because it breaks on whitespace, the text after the whitespace will remain in std::cin input stream buffer
	-use ```std::getline()``` to input text, note that it requires two arguments, the first is std::cin and the second is string variable
		```std::getline(std::cin >> std::ws, name);```
			*std::ws is a input manipulator that tells std::cin to ignore any leading whitespace before extraction*

**string length**
	the ```length()``` function is nested within std::string as a member function so to use it we do ```object.function()``` instead of ```function(object)```
		ex) ```name.length()```
	we can also use the ```std::ssize(name)``` to get the size

**do not pass std::string by value to a function**
	when this happens, the std::string function parameter must be instantiated and initialized with the argument, resulting in an expensive copy
		in most cases, use a std::string_view parameter instead
****

**std::string_view**
	when you have code like this, the string literal is copied into s, which is slow
```cpp
std::string s{ "Hello, world!" }; // s makes a copy of its initializer
std::cout << s << '\n';
```
therefore, you use std::string_view to provide read-only access to an existing string or another string_view without making a copy
	-to use string view you need to use the ```#include <string_view>``` header to make a object such as ```std::string_view s { };``` 
	-note: string_view parameters will accept many types of string arguments, you can initialize it with strings or other string_view objects
	-there are also member functions to control the view: `remove_prefix(n)` and `remove_suffix(n), where n is the amount of characters` to remove on either side of the string view
	
*best practice: use string_view over string when you need a read-only string, especially for function parameters*

