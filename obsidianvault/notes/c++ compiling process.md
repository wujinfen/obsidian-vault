
1. preprocessing
	The preprocessor handles directives like '#include', '#define', etc
		it replaces macros with their definitions and includes the contents of header files in the source file to generate a preprocessed source file (translation unit) that is ready for compilation 
2. compiling
	the compiler translates the preprocessed code into assembly language for the target machine
		checks syntax and semantic analysis and outputs a assembly language file
3. assembling
	the assembler converts assembly code into machine code (object file), but it is not fully executable yet
4. linking
	the linker combines one or more object files into a single executable
		it resolves external references between different object files and links the program to standard libraries
			it results in a executable file 