
```cpp
#include <iostream>  
#include <memory>  
#include <sstream>  
  
void func(std::unique_ptr<int> intP) {  
    std::cout << *intP << '\n';  
}  
  
std::unique_ptr<int> returnFunc(int num) {  
    return std::unique_ptr<int>{new int(num)};  
}  
  
int main() {  
    std::unique_ptr<int> p1{new int(50)};  
  
    std::unique_ptr<int[]> arr{new int[10]{}};  
  
    std::unique_ptr<int> p3{std::make_unique<int>(450)};  
  
    func(std::move(p3));  
  
    auto unqPtr = returnFunc(200);  
  
    std::cout << *unqPtr;  
  
    return 0;  
}
```


```cpp
auto ptr{std::make_shared<int>(36)};  
std::cout << "shared_ptr's data is " << *ptr << '\n';  
  
std::weak_ptr<int> wptr = ptr;  
  
ptr = nullptr;  
  
std::shared_ptr<int> sp1 = wptr.lock();  
  
  
if (sp1) {  
     std::cout << "weak_ptr's data is " << *sp1 << '\n';  
 } else {  
     std::cout << "weak_ptr is not valid.";  
  
 }
```


Валидност до