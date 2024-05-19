**memory addressing**
	memory is organized into byte-sized sequential units called addresses

**data types**
	-we use a data type to tell compiler how to interpret the contents of memory
	-the fundamental data types are built into C++

|Types|Category|Meaning|Example|
|---|---|---|---|
|float  <br>double  <br>long double|Floating Point|a number with a fractional part|3.14159|
|bool|Integral (Boolean)|true or false|true|
|char  <br>wchar_t  <br>char8_t (C++20)  <br>char16_t (C++11)  <br>char32_t (C++11)|Integral (Character)|a single character of text|‘c’|
|short int  <br>int  <br>long int  <br>long long int (C++11)|Integral (Integer)|positive and negative whole numbers, including 0|64|
|std::nullptr_t (C++11)|Null Pointer|a null pointer|nullptr|
|void|Void|no type|n/a|
*note: 'string' type is a compound type*
*note: the '_t' suffix means "type"*

**void**
	void means no type
		it is an incomplete type that has been declared, but not yet defined
		it's most commonly used to indicate that a function does not return a value

**object sizes**
	we can use the sizeof(...) operator to return an object's size in bytes

| **category**   | **type**        | **min size**                                            | **typical size**                                               |
| -------------- | --------------- | ------------------------------------------------------- | -------------------------------------------------------------- |
| boolean        | bool            | 1 byte                                                  | 1 byte                                                         |
| character      | char            | 1 byte                                                  | 1 byte                                                         |
|                | wchar_t         | 1 byte                                                  | 2 or 4 bytes                                                   |
|                | char8_t         | 1 byte                                                  | 1 byte                                                         |
|                | char16_t        | 2 bytes                                                 | 2 bytes                                                        |
|                | char32_t        | 4 bytes                                                 | 4 bytes                                                        |
| integer        | short           | 2 bytes                                                 | 2 bytes                                                        |
|                | int             | 2 bytes                                                 | 4 bytes                                                        |
|                | long            | 4 bytes                                                 | 4 or 8 bytes                                                   |
|                | long long       | 8 bytes                                                 | 4 or 8 bytes                                                   |
| floating point | float           | 4 bytes                                                 | 4 bytes                                                        |
|                | double          | 8 bytes                                                 | 8 bytes                                                        |
|                | long double     | 8 bytes                                                 | 8 or 16 bytes                                                  |
| pointer        | std::nullptr_t  | 4 bytes                                                 | 4 or 8 bytes                                                   |
**fixed-width integers**
	the following table is a set of defined integers that are guaranteed to be the same size on any architecture
		to use these, they can be accessed by including the ```#include <cstdint>```
	there are also fast and least integer types (not used often)

| **Name**       | **Type**        | **Range**                                               | **Notes**                                                      |
| -------------- | --------------- | ------------------------------------------------------- | -------------------------------------------------------------- |
| std::int8_t    | 1 byte signed   | -128 to 127                                             | Treated like a signed char on many systems. See note below.    |
| std::uint8_t   | 1 byte unsigned | 0 to 255                                                | Treated like an unsigned char on many systems. See note below. |
| std::int16_t   | 2 byte signed   | -32,768 to 32,767                                       |                                                                |
| std::uint16_t  | 2 byte unsigned | 0 to 65,535                                             |                                                                |
| std::int32_t   | 4 byte signed   | -2,147,483,648 to 2,147,483,647                         |                                                                |
| std::uint32_t  | 4 byte unsigned | 0 to 4,294,967,295                                      |                                                                |
| std::int64_t   | 8 byte signed   | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |                                                                |
| std::uint64_t  | 8 byte unsigned | 0 to 18,446,744,073,709,551,615                         |                                                                |
**Best practice
- Prefer `int` when the size of the integer doesn’t matter (e.g. the number will always fit within the range of a 2-byte signed integer) and the variable is short-lived (e.g. destroyed at the end of the function). For example, if you’re asking the user to enter their age, or counting from 1 to 10, it doesn’t matter whether int is 16 or 32 bits (the numbers will fit either way). This will cover the vast majority of the cases you’re likely to run across.
- Prefer `std::int#_t` when storing a quantity that needs a guaranteed range.
- Prefer `std::uint#_t` when doing bit manipulation or where well-defined wrap-around behavior is required.
Avoid the following when possible:
- `short` and `long` integers -- use a fixed-width type instead.
- Unsigned types for holding quantities.
- The 8-bit fixed-width integer types.
- The fast and least fixed-width types.
- Any compiler-specific fixed-width integers -- for example, Visual Studio defines __int8, __int16, etc.…
****

**casting**
	C++ will implicit cast some data types
		keep in mind, usually only valid if casting is going up in space/information
	can use static cast
		ex. 
```cpp
char ch{ 97 }; // 97 is ASCII code for 'a'
std::cout << ch << " has value " << static_cast<int>(ch) << '\n'; // print value of variable ch as an int
```

