**general approach**
	1. find the root cause of problem
	2. understand why issue is occurring
	3. determine solution
	4. repair issue
	5. retest to ensure problem fixed
	6. retest to ensure no new problems have emerged

**if can't find issue**
	1. reproduce the problem
	2. run program and narrow down problem location
	3. repeat until found

**debugging tactics**
	-comment out code
	-validating code flow
		ex. track function calls by placing print statements at the top of functions that prints the functions name
		*tip: use std::cerr for debugging because it is unbuffered*
	-print values
	-conditionalize debugging with ```#define ENABLE_DEBUG``` and wrapping debug statements with ```ifdef ENABLE_DEBUG ... #endif```
	-use a logger to send sequential record of events to a log file to review

**debugger**
	a program that allows us to control a program execution and examine program state while running
		there are command line debuggers, but most modern IDEs have integrated debuggers

**stepping**
	-step into is debugger feature that lets us execute our code statement by statement
	press F11 to enter and step line by line. the debugger will trace the program and give the call stack
	-step over executes the next statement and provides a convenient way to skip functions that you are not interested in debugging
	-step out executes all the remaining code in the function currently being executed and returns control to you when the function has returned

**running**
	the 'run to cursor' command executes the program until execution reaches the statement selected by your cursor, then it returns control so you can continue debugging from there

**continue**
	the 'continue' command runs the program from that point forward as normal in a debug session until the program terminates or until something triggers a control return

**breakpoint**
	a special marker that tells the debugger to stop execution of the program at the breakpoint when in debug mode
		so if you set a breakpoint and use the start debug command, it will return control at that point

**watching variables**
	inspecting the value of a variable while the program is executing in debug mode
		-you can use the watch window to quick inspect variables
		-you can set a breakpoint on watched variables that causes the program to stop execution when that variable changes value

-------------------------------------------------------------------

**call stack**
	the call stack is a list of all the active functions that have been called to get to the current point of execution. The stack includes an entry for each function called, as well as which line of code will be returned to when the function returns. whenever a new function is called, that function is added to the top of the call stack. when the current function returns to the caller, it is removed from the top of the call stack, and control returns to the function just below it.
		this way, the program "bookmarks" locations and knows where to return to
	when debugging, you can see the call stack window

-------------------------------------------------------------------

**preventative**
	here are tips to help prevent errors:
		-follow best practices
		-understand common pitfalls in a language
		-don't have too long functions
		-prefer using standard library
		-comment code liberally
		-start with simple solutions and then layer in complexity
		-avoid clever/non-obvious solutions
		-optimize for readability/maintainability, not performance
	can also use static analysis tools (linters) to find areas where code is non-compliant with best practices
		VS 2019 has built in tool: Build > Run Code Analysis on Solution