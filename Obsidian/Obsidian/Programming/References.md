 
```cpp
#include <iostream>  
  
using namespace std;  
  
int main() {  
  
    int number;  
    number = 3;  
  
    int &numberRef = number; // reference to the number variable  
  
    numberRef++;  
  
    std::cout << number; // 4  
}
```

```cpp
#include <iostream>  
  
void swapIntegers(int &a, int &b) {  
    a ^= b;  
    b ^= a;  
    a ^= b;  
}  
  
int main() {  
    int original = 5;  
    int& reference= original;  
  
    // int& invalidReference;  
  
    reference++;  
  
    int a = 5, b = 10;  
  
    swapIntegers(a, b);  
  
    std::cout << "a: " << a << " b: " << b;  
  
  
    return 0;  
}
```