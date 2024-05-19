
generating random numbers is useful - for games, modelling, and cryptography

computers are generally incapable of generating truly random numbers, therefore modern programs *simulate* randomness w/ algorithms
****

**algorithms and state**
an algorithm is a finite sequence of instructions to solve problems or produce useful results

an algorithm is *stateful* if it retains some information across calls. it is *stateless* if it does not. its *state* refers to the current values held in stateful variables. algorithms can also be *deterministic*, meaning that for a given input, it will always produce the same output sequence

****

**pseudo-random number generators (PRNGs)**
to simulate randomness, programs usually use PRNGs
ex.
```cpp
#include <iostream>

// For illustrative purposes only, don't use this
unsigned int LCG16() // our PRNG
{
    static unsigned int s_state{ 0 }; // only initialized the first time this function is called

    // Generate the next number

    // We modify the state using large constants and intentional overflow to make it hard
    // for someone to casually determine what the next number in the sequence will be.

    s_state = 8253729 * s_state + 2396403; // first we modify the state
    return s_state % 32768; // then we use the new state to generate the next number in the sequence
}

int main()
{
    // Print 100 random numbers
    for (int count{ 1 }; count <= 100; ++count)
    {
        std::cout << LCG16() << '\t';

        // If we've printed 10 numbers, start a new row
        if (count % 10 == 0)
            std::cout << '\n';
    }

    return 0;
}
```

**seeding a PRNG**
in the above function, the sequence of "random numbers" is not random at all. given some initial state (such as 0), it will generate the same sequence every time
	to generate different output sequences, the initial state of a PRNG needs to be varied
	the value used to set the initial state of a PRNG is called a **seed**
Because the initial state of the PRNG is set from the seed value(s), all of the values that a PRNG will produce are deterministically calculated from the seed value(s).

```cpp
#include <iostream>

unsigned int g_state{ 0 };

void seedPRNG(unsigned int seed)
{
    g_state = seed;
}

// For illustrative purposes only, don't use this
unsigned int LCG16() // our PRNG
{
    // We modify the state using large constants and intentional overflow to make it hard
    // for someone to casually determine what the next number in the sequence will be.

    g_state = 8253729 * g_state + 2396403; // first we modify the state
    return g_state % 32768; // then we use the new state to generate the next number in the sequence
}

void print10()
{
    // Print 10 random numbers
    for (int count{ 1 }; count <= 10; ++count)
    {
        std::cout << LCG16() << '\t';
    }

    std::cout << '\n';
}

int main()
{
    unsigned int x {};
    std::cout << "Enter a seed value: ";
    std::cin >> x;

    seedPRNG(x); // seed our PRNG
    print10();   // generate 10 random values

    return 0;
}
```

**seed quality and underseeding**
the theoretical maximum number of unique sequences a PRNG can generate is determined by the number of bits in the PRNG's state. for example, if it has 128 bits of state then it could generate 2^128 unique output sequences
	therefore, the output sequences is limited by the number of unique seed values the program using the PRNG can provide
		if a PRNG is not provided w/ enough bits of quality seed data, then it is **underseeded** 
this allows randomly generated sequences in consecutive runs to have high correlation, some values on Nth random number may never be generated, and someone may be able to guess the seed based on initial random values produced

****

**ideal seed**
	-seed should contain at least as many bits as the state of the PRNG
	-each bit in the seed should be independently randomized
	-the seed should contain a good mix of 0 and 1s, and high and low bits
	-there should be no bits that are stuck, meaning bits that are always 0 or 1
	-the seed should have a low correlation w/ previously generated seeds
	
generally seeding a PRNG w/ 64 bytes of quality seed data (less if PRNG is smaller) is good enough to facilitate generation of 8-byte random values for non-sensitive uses

****

**randomization in C++**
	you can access the randomization capabilities in c++ via the `<random>` header of the standard library
		within the random library, there are 6 PRNG families available for use
![[Pasted image 20240517142913.png]]

****

**generating random numbers in c++ using Mersenne Twister**
the random library supports two Mersenne Twister types
	`mt19937` is a Mersenne Twister that generates 32-bit unsigned integers
	`mt19937_64` is one that generates 64-bit unsigned integers

ex.
```cpp
#include <iostream>
#include <random> // for std::mt19937

int main()
{
	std::mt19937 mt{}; // Instantiate a 32-bit Mersenne Twister

	// Print a bunch of random numbers
	for (int count{ 1 }; count <= 40; ++count)
	{
		std::cout << mt() << '\t'; // generate a random number

		// If we've printed 5 numbers, start a new row
		if (count % 5 == 0)
			std::cout << '\n';
	}

	return 0;
}
```

**rolling a dice using Mersenne Twister**
PRNGs can only generate numbers that use the full range of 32 - 64 bits. If you want a specific range such as a 6 or 7 sided dice, then you need to use a **random number distribution** to convert the output of a PRNG into some other distribution numbers
	a random number distribution is just a probability distribution designed to take PRNG values as input.
therefore we want to use the random library's **uniform distribution** to do this

ex.
```cpp
#include <iostream>
#include <random> // for std::mt19937 and std::uniform_int_distribution

int main()
{
	std::mt19937 mt{};

	// Create a reusable random number generator that generates uniform numbers between 1 and 6
	std::uniform_int_distribution die6{ 1, 6 }; // for C++14, use std::uniform_int_distribution<> die6{ 1, 6 };

	// Print a bunch of random numbers
	for (int count{ 1 }; count <= 40; ++count)
	{
		std::cout << die6(mt) << '\t'; // generate a roll of the die here

		// If we've printed 10 numbers, start a new row
		if (count % 10 == 0)
			std::cout << '\n';
	}

	return 0;
}
```

