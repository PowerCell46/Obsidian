```cpp
#include <iostream>  
#include <sstream>  
  
class MyClass {  
};  
  
class Test {  
    int i{0};  
    MyClass m;  
public:  
    Test() = default;  
  
    Test(const Test& other) : i(other.i), m(other.m) {  
        std::cout << "Copy constructor call\n";  
    }  
    Test(Test&& other) noexcept : i(other.i), m(std::move(other.m)) {  
        std::cout << "Move constructor call\n";  
    }  
    Test& operator=(const Test& other) {  
        std::cout << "Copy assignment call\n";  
        if (this != &other) {  
            this->i = other.i;  
            this->m = other.m;  
        }        return *this;  
    }  
    Test& operator=(Test&& other) noexcept {  
        std::cout << "Move assignment call\n";  
        if (this != &other) {  
            this->i = other.i;  
            this->m = std::move(other.m);  
        }        return *this;  
    }};  
  
  
int main() {  
  
    // lvalue reference - a variable or object that has a name and memory address, uses one &  
    // rvalue reference - a variable or object has only a memory address (temp objects), uses two &&  
    // move constructor - to steal or move as many resources as it can from the original, as fast as possible, because the    // source does not need to have a meaningful value anymore, and/or because it is going to be destroyed in a moment.    // So that one can avoid unnecessarily creating copies of an object and make efficient use of the resources  
    Test test;  
    Test test2 = test; // copy constructor  
    Test test3 = Test(); // move assignment constructor  
    Test test4(std::move(test)); // move constructor  
  
    Test test5 = test2; // copy constructor  
    Test test6 = Test(); // move constructor  
  
    return 0;  
}
```


```cpp
#include <iostream>  
#include <sstream>  
  
  
class IntContainer {  
    int* data;  
    size_t size;  
public:  
    IntContainer(const size_t& size) : data(new int[size]), size(size) {  
        std::cout << "Constructor call\n";  
    }  
    
    ~IntContainer() {  
        std::cout << "Destructor call\n";  
  
        delete[] this->data;  
    }  
    
    IntContainer(const IntContainer& other) {  
        std::cout << "Copy constructor call\n";  
  
        this->size = other.size;  
        this->data = new int[this->size];  
  
        for (size_t i = 0; i < this->size; ++i)  
            this->data[i] = other.data[i];  
    }  
    
    IntContainer& operator=(const IntContainer& other) {  
        std::cout << "Copy assignment call\n";  
  
        if (this != &other) {  
            if (this->data != nullptr)  
                delete[] this->data;  
  
            this->size = other.size;  
            this->data = new int[this->size];  
  
            for (size_t i = 0; i < this->size; ++i)  
                this->data[i] = other.data[i];  
        }  
        return *this;  
    }  
    
    IntContainer(IntContainer&& other) noexcept {  
        std::cout << "Move constructor call\n";  
  
        this->size = other.size;  
        this->data = other.data;  
  
        other.data = nullptr;  
        other.size = 0;  
    }  
    
    IntContainer& operator=(IntContainer&& other) noexcept {  
        std::cout << "Move assignment operator call\n";  
  
        if (this != &other) {  
            if (this->data != nullptr)  
                delete[] this->data;  
  
            this->size = other.size;  
            this->data = other.data;  
  
            other.data = nullptr;  
            other.size = 0;  
        }  
        return *this;  
    }
};  
  
int main() {  
    IntContainer intContainer{10};  
  
    // IntContainer intContainer2{intContainer}; // Copy constructor  
    // IntContainer intContainer3{20};    
    // intContainer3 = intContainer2; // Copy assignment  
    // IntContainer intContainer4{std::move(intContainer)}; // Move constructor  
    IntContainer intContainer5{100};  
  
    intContainer5 = std::move(intContainer);  
  
    return 0;  
}
```