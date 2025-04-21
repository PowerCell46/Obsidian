## Pointer Arithmetic

```cpp
int main() {  
    int arr[3] {13, 43, 125};  
	
	*(arr + 2) = 69; // changing the value using dereferencing of a pointer
	
    std::cout << arr[2] << std::endl;  
    std::cout << *(arr + 2) << std::endl; 
    // pointer to the first element + 2 * 4 bytes 
    // -> the address of the third element  
}
```


```cpp
int main() {  
    char word[6] = {'h', 'e', 'l', 'l', 'o', '\0'};  
  
    char *p = word;  
  
    while (*p)  
        std::cout << *p++;  
}
```

## Function Pointers

```cpp
#include <iostream>  
#include <vector>  
  
void print(const std::string &s) {  
    std::cout << s << std::endl;  
}  
  
int main() {  
    int (*mainPointer)() = &main;  
  
    void (*printPointer)(const std::string &s) = &print;  
  
    std::string str = "Hello there";  
  
    printPointer(str); // calling
}
```


```cpp
#include <vector>  
  
/*  
int main() {  
  
    std::vector<int*>* intVectorPointer = new std::vector<int*>;  
    int* oneP = new int{1};    
    int* twoP = new int{2};    
    int* threeP = new int{3};  
    (*intVectorPointer).push_back(oneP);    
    (*intVectorPointer).push_back(twoP);    
    (*intVectorPointer).push_back(threeP);  
    
    for (const int* intP : *intVectorPointer) // delete the elements inside the dynamic vector        delete intP;  
    delete intVectorPointer;  
    
    return 0;}  
*/  
  
int main() {  
    int* intP = new int{1};  
    int* secondIntP = intP;  
  
    delete intP;  
    // delete secondIntP; // deleting a deleted memory  
    secondIntP = nullptr;  
  
    return 0;  
}
```

## Void pointer

```cpp
int main() {
	int number = 42;  
	char cStr[] = "hello";  
	char* otherCString = " world";  
	  
	void* p; // can point to anything  
	p = &number;  
	p = cStr;  
	p = otherCString;  
	  
	char* charP = (char*)p; // casting the void pointer back to char pointer
}
```

### C++ Pointer Casting

```cpp
int main() {  
    char letter = 'A';  
  
    void* voidPointer = &letter;  
  
    char* charPointer = static_cast<char*>(voidPointer);  
  
    std::cout << *charPointer << '\n';  
  
    // int* intPointer = static_cast<int*>(charPointer); 
    // STATIC_CAST -> compile time -> won't work  
  
    // int* intPointer = dynamic_cast<int*>(charPointer); 
    // DYNAMIC_CAST -> runtime -> should return nullptr    
    // Won't compile because dynamic_cast is only for polymorphic classes (with virtual functions).  
  
    int* intPointer = reinterpret_cast<int*>(charPointer); 
    // REINTERPRET_CAST -> no checks -> gives the wanted type  
    // When you dereference intPointer, the program tries to read 4 bytes from the memory address of letter.    
    // Since letter is only 1 byte, the next 3 bytes come from whatever happens to be in memory next.    
    std::cout << *intPointer << '\n';  
    // Undefined behavior: Reads uninitialized memory beyond 'letter'.  
  
    const int age = 21;  
  
    int* constantIntPointer = const_cast<int*>(&age);  
  
    (*constantIntPointer)++; // undefined behavior  
  
    return 0;  
}
```

## Smart Pointers

```cpp
int main() {  
    std::unique_ptr<Person> p(new Person("Peter", 21));  
  
	// Invalid
    std::unique_ptr<Person> pCopy = p;  
      
    return 0;  
}
```


```cpp
#include <iostream>  
#include <memory>  
  
class Person {  
    std::string name;  
    int age;  
public:  
    Person(std::string name, const int age)  
	    : name(name), age(age) {  
    }};  
  
  
int main() {  
    std::shared_ptr<Person> p = std::make_shared<Person>("Peter", 21);  
  
    std::shared_ptr<Person> pCopy = p;  
  
    return 0;  
}
```

## std::find_if()

```cpp
#include <iostream>
#include <vector>

void findFirstPositiveIndex() {
	std::vector<int> data = { -1, -2, -3, 4, 5 };

	const auto isPositive = [](const auto& x) {return x > 0; };

	auto firstPositiveIndex = std::find_if(
		data.begin(),
		data.end(),
		isPositive
	);

	std::cout << *firstPositiveIndex;
}

int main() {

	findFirstPositiveIndex();
	
	return 0;
}
```

# Dynamic Array with Malloc
 
```cpp
int main() {  
    int *p = (int *) malloc(500 * sizeof(int));  
  
    for (int i = 0; i < 500; ++i) {  
        *(p) = i + 1;  
        std::cout << *p << std::endl;  
    }  
    
    free(p);  
  
    return 0;  
}
```

# Using Dynamic Array with functions

```cpp 
int* generateNumbersArray(const int& endElement) {  
    int* numbersArr = new int[endElement];  
  
    for (int i = 0; i < endElement; ++i)  
        numbersArr[i] = i + 1;  
  
    return numbersArr;  
}  
  
int main() {  
    int size;  
    std::cin >> size;  
  
    int* numbers = generateNumbersArray(size);  
  
    std::stringstream resultStream{};  
    for (int i = 0; i < size; ++i)  
        resultStream << numbers[i] << ((i + 1) % 10 == 0 ? '\n' : ' ');  
  
  
    std::cout << resultStream.str() << std::endl;  
  
    delete[] numbers;  
    numbers = nullptr; // good practice  
  
    return 0;  
}
```
