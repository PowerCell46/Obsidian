## Tail recursion
- The call is the last operation

```cpp
void print(int num) {  
    if (num > 0) {  
        std::cout << std::string(num, '-') << '\n';  
        print(num - 1);  
    }
}
```

## Head recursion
- The operation is after the return statement

```cpp
void print(int num) {  
    if (num > 0) {  
        print(num - 1);  
        std::cout << std::string(num, '*') << '\n';  
    }
}
```

## Tree recursion

```cpp
void func(const int& n) {  
    if (n > 0) {  
        std::cout << n << ' ';  
        func(n - 1);  
        func(n - 1);  
    }
}
```

## Indirect recursion

```cpp
void funcA(const int& n);  
void funcB(const int& n);  
    
void funcA(const int& n) {  
    if (n > 0) {  
        std::cout << n << ' ';  
        funcB(n - 1);  
    }
}  

void funcB(const int& n) {  
    if (n > 0) {  
        std::cout << n << ' ';  
        funcA(n / 2);  
    }
}
```

## Nested recursion (Hell)

```cpp
int func(const int& n) {  
    if (n > 100)  
        return n - 10;  
    return func(func(n + 11));  
}
```

## Generating 0/1 Vectors

### Using iterative approach

```cpp
bool checkForComplete(const int* array, const int& arraySize) {  
    for (int i = 0; i < arraySize; ++i)  
        if (*(array + i) != 1)  
            return false;  
    return true;  
}  
  
void generate01Vectors(const int& number) {  
    int* array = new int[number]{};  
  
    while (true) {  
        for (int i = 0; i < number; ++i)  
            std::cout << *(array + i);  
        std::cout << '\n';  
  
        if (*array == 1 && *(array + number - 1) == 1 && checkForComplete(array, number))  
            break;  
  
        int incIndex = number - 1;  
        *(array + incIndex) += 1;  
        while (incIndex >= 0 && *(array + incIndex) > 1) {  
            *(array + incIndex) = 0;  
            incIndex--;  
            if (incIndex >= 0)  
                *(array + incIndex) += 1;  
        }    
	}  
	
    delete[] array;  
}
```

## Reverse String

```cpp
std::string reverseString(const std::string& str, const int& index = 0) {  
    if (index < str.size())  
        return reverseString(str, index + 1) + str.at(index);  
    return "";  
}
```

## Fibonacci Implementation using Memorization

```cpp
int fibonacciImpl(const int& number, int* memorizationArray) {
    if (number <= 1) {
        if (*(memorizationArray + number) == -1)
            *(memorizationArray + number) = number;
        return number;
    }

    if (*(memorizationArray + number - 1) == -1)
        *(memorizationArray + number - 1) = fibonacciImpl(number - 1, memorizationArray);

    if (*(memorizationArray + number - 2) == -1)
        *(memorizationArray + number - 2) = fibonacciImpl(number - 2, memorizationArray);

    return *(memorizationArray + number - 1) + *(memorizationArray + number - 2);
}

int fibonacci(const int& number) {
    // Create an array keeping the results of previous calls
    int* memorizationArray = new int[number]{};
    for (int i = 0; i < number; ++i)
        *(memorizationArray + i) = -1;

    const int result = fibonacciImpl(number, memorizationArray);

    delete[] memorizationArray;

    return result;
}
```