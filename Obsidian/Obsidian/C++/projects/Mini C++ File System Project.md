
```cpp
#include <iostream>  
#include <string>  
#include <fstream>  
  
class Person {  
    std::string name;  
    int age;  
public:  
    Person() {}  
  
    Person(const std::string name, const int age) : name(name), age(age) {}  
  
    std::string getName() const {  
        return this->name;  
    }  
    int getAge() const {  
        return this->age;  
    }};  
  
std::ostream& operator<<(std::ostream& os, const Person& p) {  
    os << p.getName() << ' ' << p.getAge();  
    return os;  
}  
  
  
int main() {  
  
    std::fstream fileStream{"C:\\Programming\\C++\\C++ProjectCLion\\output.txt"};  
  
    while (true) {  
        std::string currentName;  
        int currentAge;  
  
        std::cout << "Enter your Name: " << std::endl;  
        std::cin >> currentName;  
  
        if (currentName == "Save")  
            break;  
  
        std::cout << "Enter your Age: " << std::endl;  
        std::cin >> currentAge;  
  
        Person currentPerson{currentName, currentAge};  
        fileStream << currentPerson << std::endl;  
    }  
    
    std::cout 
	    << "All entered people were successfully saved into the system." 
	    << std::endl;  
  
    return 0;  
}
```