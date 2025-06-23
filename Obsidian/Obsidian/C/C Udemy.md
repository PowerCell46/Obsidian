### <span style="color:rgb(255, 0, 0)">Run a C program from the terminal</span>

```c
gcc main.c // Compile the source file
gcc main.c -o MyProgram // Set a specific name for the exe file
gcc main.c other.c -o main // Compile multiple files to an exe file
gcc *.c -o main

main.exe // Run the exe file
```

# Rewatch lections
Section 4:
- Compiling multiple source files from the cmd
- makefiles

---
## Auto
<span style="color:rgb(107, 255, 174)">this variable has automatic storage duration</span>
<span style="color:rgb(107, 255, 174)">This variable is local, temporary, and stack-allocated</span>

- **Automatically created** when the block (e.g., function or loop) is entered.
- **Automatically destroyed** when the block is exited.
- **Stored in the stack.**
- **Local scope** – visible only within the block in which it's declared.
- **Default for local variables**, so `auto` is rarely written explicitly in modern C.

```c
int main() {
    int y = 20; // also automatic by default
}
```

Even without `auto`, `y` is an automatic variable, because it's declared inside a function and not as `static` or `extern`.