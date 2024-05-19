**nesting blocks**
	you can nest blocks within others
	*best practice is to keep the nesting level of functions to 3 or less*

**defining namespace**
	you can define your own namespace with the `namespace` keyword. the syntax is as follows:
```
namespace NamespaceIdentifier {
	//CONTENT HERE
}
```
so now, you can avoid naming collisions: the example below uses doSomething in namespace Foo
```cpp
#include <iostream>
namespace Foo // define a namespace named Foo
{
    // This doSomething() belongs to namespace Foo
    int doSomething(int x, int y)
    {
        return x + y;
    }
}

namespace Goo // define a namespace named Goo
{
    // This doSomething() belongs to namespace Goo
    int doSomething(int x, int y)
    {
        return x - y;
    }
}

int main()
{
    std::cout << Foo::doSomething(4, 3) << '\n'; // use the doSomething() that exists in namespace Foo
    return 0;
}
```
you can use namespace declarations in header files
note: you can declare multiple of the same namespaces in different files, they will all be considered part of the namespace
****
**local variables**
	variables that are defined inside a function are local and have block scope (they are in scope from their point of definition to the end of the block they are defined within)
		note: all variable names must be unique within a given scope
		note: local variables have automatic storage duration, which means they are destroyed at the end of their scope

**local variables have no linkage**
	each declaration of an identifier with no linkage refers to a unique object or function
	ex. 
```cpp
int main()
{
    int x { 2 }; // local variable, no linkage
    {
        int x { 3 }; // this declaration of x refers to a different object than the previous x
    }
    return 0;
}
```

**global variables**
	variables can be declared outside of a function (outside of any scope)
		this means that it is declared in the global namespace scope and can be used anywhere in the file from that point onward
			*note: variables declared inside a namespace are also global variables*
			*note: global variables are created when the program starts (before main() execution and destroyed when it ends, this is called static duration*
		*best practice: consider using g or g_ prefix when naming non-const global variables to help differentiate*
****

**name hiding (shadowing)**
	when you *define* variable inside a nested block that has the same name as a variable in an outer block , the nested variable "hides" the outer variable where they are both in scope
		*best practice: avoid variable shadowing*

****

**internal linkage**
	an identifier's linkage determines whether other declarations of that name refer to the same object or not
		-local variables have no linkage
		-global variables and functions have either `internal linkage` or `external linkage`
	global variables w/ internal linkage are sometimes called internal variables
		to make a non-constant global variable internal, we use the `static` keyword
		this makes it so that only that source file has scope and visibility of that variable
			*note: const and constexpr global vars have internal linkage by default so static keyword is not needed*
			
ex. of multiple files using internal variables 
a.cpp: 
```cpp
[[maybe_unused]] constexpr int g_x { 2 }; // this internal g_x is only accessible within a.cpp`
```

main.cpp:
```cpp
#include <iostream>

static int g_x { 3 }; // this separate internal g_x is only accessible within main.cpp

int main()
{
    std::cout << g_x << '\n'; // uses main.cpp's g_x, prints 3
    return 0;
}
```

**storage class specifier**
	storage class specifiers are a group of keywords that set both the name's linkage and its storage duration
		ex. `static`, `extern`, `mutable` are all storage class specifiers

**functions with internal linkage**
	same properties apply to function identifiers to make it only usable within this file
		attempts to access these from another file via forward declaration will fail to link

*best practice: give identifiers internal linkage if you have an explicit reason to disallow access from other files*

****

**external linkage**
	you can use `extern` keyword to make identifiers global and usable from anywhere in program
		*note: functions have external linkage by default*

*best practice: use local variables instead of global variables whenever possible
best practice: if you need global constants in c++17, prefer defining inline constexpr global variables in a header file, can use a user namespace too*

****

**static local variables**
`static` has different meanings in different contexts
	*previously we talked about static duration which gives global identifiers internal linkage*

now we explore `static` when applied to a local variable
	using `static` on a local variable changes its duration from automatic to static duration. this means that the variable is created at the start of the program and destroyed at the end of the program, so a static variable will retain its value even after going out of scope

ex. s_value retains its data even when out of scope
```cpp
#include <iostream>

void incrementAndPrint()
{
    static int s_value{ 1 }; // static duration via static keyword.  This initializer is only executed once.
    ++s_value;
    std::cout << s_value << '\n';
} // s_value is not destroyed here, but becomes inaccessible because it goes out of scope

int main()
{
    incrementAndPrint(); //prints 2
    incrementAndPrint(); //prints 3
    incrementAndPrint(); //prints 4
    return 0;
}
```

**static local variable use case: unique ID generation**
	imagine if you have a game program that has many similar objects, you can more effectively "separate" objects and differentiate them by using static duration local variables. this makes it tamper proof from other functions
		it also offers benefits of global variables while limiting their visibility to block scope
			this makes them easy to understand and safe to use
```cpp
int generateID()
{
    static int s_itemID{ 0 };
    return s_itemID++; // makes copy of s_itemID, increments the real s_itemID, then returns the value in the copy
}
```

*best practice: initialize your static local variables, static local variables are only initialized the first time the code is executed, not on subsequent calls*

*best practice: avoid static local variables unless the variable never needs to be reset*

****

**using declarations and using directives**
	if using the standard library in c++, typing `std::` can become repetitive and can make code harder to read
		solution is using-statements

**qualified and unqualified names**
	a name can be either qualified or unqualified
		a qualified name includes an associated scope using the resolution operator (::)
		a unqualified name is a name that does not include a scoping qualifier
ex.
```cpp
std::cout //identifier cout is qualified by namespace std
::foo     //identifier foo is qualified by global namespace
```

*note: a name can be qualified by a class name using the scope resolution operator or by a class object using the member selection operator (. or ->)*
ex.
```cpp
class C;
C::s_member; //s_member qualified by class C
obj.x        //x qualified by class object obj
ptr->y;      //y is qualified by pointer to class object ptr
```


**using-declarations**
	reduce the repetition of typing `std::` by using a using-declaration
		it tells compiler to assume we mean the `std` namespace
ex.
```cpp
int main() {
	using std::cout; //tells compiler that cout should resolve to std::cout
	cout << "Hello!";
	
	return 0;
}
```

**using-directives**
	another way to simplify
		a using directive imports all the identifiers from a namespace into the scope of the using-directive
ex.
```cpp
int main() {
	using namespace std; //this directive tells compiler to import all names from namespace std into the current namespace
	cout << "Hello!";
	
	return 0;
}
```

*best practice: prefer explicit namespaces over using-statements. avoid using-directives whenever possible. using-declarations are okay to use inside blocks*

****

**inline functions and unnamed namespaces**

**Inline functions** were originally designed as a way to request that the compiler replace your function call with inline expansion of the function code. You should not need to use the inline keyword for this purpose because the compiler will generally determine this for you. In modern C++, the `inline` keyword is used to exempt a function from the one-definition rule, allowing its definition to be imported into multiple code files. Inline functions are typically defined in header files so they can be `#included` into any code files that needs them.

Finally, C++ supports **unnamed namespaces**, which implicitly treat all contents of the namespace as if it had internal linkage. C++ also supports **inline namespaces**, which provide some primitive versioning capabilities for namespaces.