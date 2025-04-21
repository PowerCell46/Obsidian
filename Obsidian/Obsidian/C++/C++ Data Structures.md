# Matrices

## Initializing

```cpp
// array matrix
constexpr int matrix[3][3] =  
    {  
        {1, 2, 3},
		{4, 5, 6},
		{7, 8, 9}    
	};  

// vector matrix
std::vector<std::vector<int>> vectorMatrix =  
    {  
        {1, 2, 3, 4},  
        {5, 6, 7, 8},  
        {9, 10, 11, 12}  
    };
```

## Reading from the console

```cpp
int rows, columns;  
std::cin >> rows >> columns;  
  
int matrix[rows][columns];  
  
for (int i = 0; i < rows; ++i) {  
    for (int j = 0; j < columns; ++j) {  
        int keeper;  
        std::cin >> keeper;  
        matrix[i][j] = keeper;  
        std::cout << matrix[i][j] << ' ';  
    }    
    std::cout << std::endl;  
}
```

# Swap Values using XOR operator

```cpp
void swapTwoNums(int& a, int& b) {  
    /* a ^= b */ a = a + b; /* 5 = 5 + 10; a = 15 */  
    /* b ^= a */ b = a - b; /* 10 = 15 - 10; b = 5 */  
    /* a ^= b */ a = a - b; /* 15 = 15 - 5; a = 10 */  
}  
  
  
int main() {  
    int a = 5, b = 10;  
  
    swapTwoNums(a, b);  
  
    std::cout << "A: " <<  a << "; B: " << b;  
  
    return 0;  
}
```

# Stacks

```cpp
#include <iostream>  
#include <vector>  
#include <stack>  

int main() {  
    std::stack<int> numbersStack;  
  
    numbersStack.push(10); // {10}  
    numbersStack.push(20); // {10, 20} 
    numbersStack.push(30); // {10, 20, 30}
  
  
    std::cout << "Top element: " << numbersStack.top() << std::endl;  
	// 30
	
    numbersStack.pop();  
	// {10, 20}
	  
    std::cout << "Top element After pop: " << numbersStack.top() << std::endl;  
	// 20
  
    return 0;  
}
```

## Queues

```cpp
#include <iostream>  
#include <vector>  
#include <queue>  
  
int main() {  
    std::queue<int> numbersQueue;  
  
    numbersQueue.push(10); // {10}  
    numbersQueue.push(20); // {10, 20} 
    numbersQueue.push(30); // {10, 20, 30} 

	// 10
    std::cout << "Front Element: " << numbersQueue.front() << std::endl;  
    // 30
    std::cout << "Back Element: " << numbersQueue.back() << std::endl;  

    numbersQueue.pop();  
	// {20, 30}

	// 20
    std::cout << "Front element after pop: " << numbersQueue.front() 
	    << std::endl;  
    return 0;  
}
```

## To Upper Case

char a = 'a'; /* 97 */

1 byte
01100001

/*
	32
	1 byte
	00100000
*/
&

~32 = (256 - 32 -1)
1 byte
11011111

char result = 'A'; /* 65 */
01000001

___
## To Lower Case

char A = 'A'; /* 65 */
1 byte
01000001

|

32
1 byte
00100000

char result = 'a'; /* 97 */
01100001


```cpp
char swapCase(char c) {  
    if ('a' <= c && c <= 'z') {  
        return c & ~32; // Convert to uppercase  
  
    } else if ('A' <= c && c <= 'Z') {  
        return c | 32;  // Convert to lowercase  
    }  
    return c; // Non-alphabetical characters remain unchanged  
}  
  
int main() {  
    // std::cout << swapCase('a') << std::endl; // Output: A  
    std::cout << swapCase('1') << std::endl; // Output: z  
  
// std::cout << std::pow(2, 8);  
  
    // std::cout << (char) 32;  
    return 0;  
}
```

## [[Strings]]

---
## Multiply By the Power Of 2

int number = 3;

1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000011

<< /* x2 */
3 x 2 =6
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000110

<< /* x2 */
6 x 2 = 12
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00001100

```cpp
int multiplyByPowerOf2(int number, int power) {
    return number << power;
}

int main() {
    std::cout << multiplyByPowerOf2(3, 2); // Output: 12 (3 * 2^2)
    return 0;
}
```

___
## Divide By the Power Of 2

int number = 256;

1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000001 00000000

\>> /* /2 */

256 / 2 = 128
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 10000000

\>> /* /2 */

128 / 2 = 64
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 01000000

```cpp
int divideByPowerOf2(int number, int power) {  
    return number >> power;  
}  
  
int main() {  
    std::cout << divideByPowerOf2(256, 1); // Output: 128 (256 / 2^1)  
    return 0;  
}
```

---
## Sum two numbers

int a = 2;
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000010

^

int b = 3;
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000011

sum = 1;
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000001

&

carry = 2;
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000010

<<

carry = 4;
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000100

___

a = 1;
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000001

b = 4;
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000100

^

sum = 5;
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000101

&
carry = 0;
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000000

<<

carry = 0;
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000000

a = 5;
b = 0;

```cpp
int add(int a, int b) {  
    while (b != 0) {  
        // Step 1: Calculate sum without carry  
        int sum = a ^ b;  
  
        // Step 2: Calculate carry  
        int carry = (a & b) << 1;  
  
        // Step 3: Update a and b  
        a = sum;  
        b = carry;  
    }    
    return a;  
}  
  
int main() {  
    int x = 2, y = 3;  

	std::cout << x << " + " << y << " = " << add(x, y) << std::endl; 
	// Output: 2 + 3 = 5  
  
    return 0;  
}
```

# [[Data Structures]]