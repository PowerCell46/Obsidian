
```cpp
#include <iostream>  
#include <vector>  
  
bool isEven(const int number);  
  
bool isOdd(const int number);  
  
std::vector<int> filterNumbers(const std::vector<int> &numbers, bool (*filter)(int));  
  
  
int main() {  
    const std::vector numbers = {1, 2, 3, 4, 5, 6};  
  
    const std::vector<int> even = filterNumbers(numbers, isEven);  
  
    const std::vector<int> odd = filterNumbers(numbers, isOdd);  
  
  
    for (const int num: odd)  
        std::cout << num << ' ';  
  
    std::cout << std::endl;  
  
    for (const int num: even)  
        std::cout << num << ' ';  
  
    return 0;  
}  
  
std::vector<int> filterNumbers(const std::vector<int> &numbers, bool (*filter)(int)) {  
    std::vector<int> result;  
  
    for (const int num: numbers)  
        if (filter(num))  
            result.push_back(num);  
  
    return result;  
}  
  
bool isEven(const int number) {  
    return number % 2 == 0;  
}  
  
bool isOdd(const int number) {  
    return number % 2 != 0;  
}
```