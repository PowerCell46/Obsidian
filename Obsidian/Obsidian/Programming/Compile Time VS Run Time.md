## **Compile Time:**

- **Definition:** The phase in which Java source code is translated into bytecode by the Java compiler.
- **Key Activities:**
    - **Syntax Checking:** Ensuring the code follows Java language rules.
    - **Type Checking:** Verifying correct variable types and method calls.
    - **Optimization:** Improving bytecode performance.
- **Errors Detected:**
    - Syntax errors (e.g., missing semicolons, unmatched braces)
    - Type mismatch errors (e.g., assigning a string to an integer variable)
    - Missing references (e.g., calling a method that doesn’t exist)

## **Runtime:**

- **Definition:** The phase when the Java Virtual Machine (JVM) executes the bytecode.
- **Key Activities:**
    - **Loading:** JVM loads the compiled bytecode into memory.
    - **Linking:** Resolving symbolic references to actual memory addresses.
    - **Initialization:** Executing static initializers and static blocks.
    - **Execution:** Running the bytecode instructions.
- **Errors Detected:**
    - Exceptions (e.g., `NullPointerException`, `ArrayIndexOutOfBoundsException`)
    - Logic errors (e.g., incorrect implementation of algorithms)
    - Resource errors (e.g., file not found, network connectivity issues)

## **Key Differences:**

- **Error Detection:** Compile-time errors are detected by the compiler before the program runs, while runtime errors occur during the execution of the program.
- **Nature of Errors:** Compile-time errors relate to syntax and type checking, whereas runtime errors pertain to the program's logic, resources, and external factors.
- **Phase:** Compile time involves converting source code to bytecode; runtime involves executing the bytecode by the JVM.