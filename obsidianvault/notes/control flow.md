*typical control flow stuff is same as java*

**switch**
	switch statements only evaluate expression once so prefer it over if-else chains
	*note: labels for cases are not conventionally indented*
	break statements tell compiler we are done executing statements within the switch block, continues the statement after the end of the switch
		can also replace break with return, but return exits the entire function
		*note: if you don't have either, it will cause subsequent cases to execute as well*
			*if you want intentional fallthrough, use the `[[fallthrough]];` attribute to indicate*
```cpp

void printDigitName(int x)
{
    switch (x)
    {
    case 1:
        std::cout << "One";
        break;
    case 2:
        std::cout << "Two";
        break;
    case 3:
        std::cout << "Three";
        break;
    default:
        std::cout << "Unknown";
        break;
    }
    return;
}

int main()
{
    printDigitName(2);
    std::cout << '\n';

    return 0;
}
```

if you have multiple or statements you could stack labels like this:
```cpp
bool isVowel(char c)
{
    switch (c)
    {
    case 'a': // if c is 'a'
    case 'e': // or if c is 'e'
    case 'i': // or if c is 'i'
    case 'o': // or if c is 'o'
    case 'u': // or if c is 'u'
    case 'A': // or if c is 'A'
    case 'E': // or if c is 'E'
    case 'I': // or if c is 'I'
    case 'O': // or if c is 'O'
    case 'U': // or if c is 'U'
        return true;
    default:
        return false;
    }
}
```

****

**goto statements**
	an unconditional jump that causes execution to go to another spot in code
		this means "it always happens"
			you need to use a statement label (conventionally not indented) with a `goto` statement to the label
		*note: the goto statement must go to a label that is within the same function scope*
ex.
```cpp
#include <iostream>
#include <cmath> // for sqrt() function

int main()
{
    double x{};
tryAgain: // this is a statement label
    std::cout << "Enter a non-negative number: ";
    std::cin >> x;

    if (x < 0.0)
        goto tryAgain; // this is the goto statement

    std::cout << "The square root of " << x << " is " << std::sqrt(x) << '\n';
    return 0;
}
```

*best practice: AVOID goto statements*
****
**for loop w/ multiple counters**
	you can have a for-loop iterate with multiple variables
```cpp
int main()
{
    for (int x{ 0 }, y{ 9 }; x < 10; ++x, --y)
        std::cout << x << ' ' << y << '\n';

    return 0;
}
```

```
THIS PRINTS:
0 9
1 8
2 7
3 6
4 5
5 4
6 3
7 2
8 1
9 0
```

*best practice: this is an acceptable practice*

*best practice:
Prefer for-loops over while-loops when there is an obvious loop variable.  
Prefer while-loops over for-loops when there is no obvious loop variable.*

****

**break and continue**
	-break statements inside a loop causes it to end and continue execution after the loop
	-the continue statement ends the current loop iteration without termination of the entire loop

****

**halts**
	a halt is a control flow statement that terminates the program

std::exit(int *code*)
	a function that causes program to terminate normally and immediately
		this function performs a number of cleanup functions
			-objects w/ static storage duration are destroyed
			-miscellaneous file cleanup is done if any were used
			-control returned back to the OS with argument passed to std::exit() used as the status code
```cpp
#include <cstdlib> // for std::exit()
#include <iostream>

void cleanup()
{
    // code here to do any kind of cleanup required
    std::cout << "cleanup!\n";
}

int main()
{
    std::cout << 1 << '\n';
    cleanup();

    std::exit(0); // terminate and return status code 0 to operating system

    // The following statements never execute
    std::cout << 2 << '\n';

    return 0;
}
```

std::atexit(*string function_name*)
	allows you to specify a function that will automatically be called on program termination via std::exit()
```cpp
#include <cstdlib> // for std::exit()
#include <iostream>

void cleanup()
{
    // code here to do any kind of cleanup required
    std::cout << "cleanup!\n";
}

int main()
{
    // register cleanup() to be called automatically when std::exit() is called
    std::atexit(cleanup); // note: we use cleanup rather than cleanup() since we're not making a function call to cleanup() right now

    std::cout << 1 << '\n';

    std::exit(0); // terminate and return status code 0 to operating system

    // The following statements never execute
    std::cout << 2 << '\n';

    return 0;
}
```

std::abort and std::terminate
	abort means program has unusual runtime error and program couldn't continue to run
		abort does not do any cleanup
	terminate is typically used w/ exceptions, by default terminate calls abort
```cpp
#include <cstdlib> // for std::abort()
#include <iostream>

int main()
{
    std::cout << 1 << '\n';
    std::abort();

    // The following statements never execute
    std::cout << 2 << '\n';

    return 0;
}
```

*best practice: only use a halt if there is no safe way to return normally from the main function*

