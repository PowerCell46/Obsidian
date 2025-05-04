# Functions

```cpp
#include <iostream>  
  
using std::string;  
using std::cout;  
using std::endl;  
  
// declaring the function  
int sum(int a, int b);  
int sum(int, int); // can be written without the name of the vars
  
int main() {  
	// Calling the function
    cout << sum(10, 50);  
  
    return 0;  
}  

// body of the function
int sum(int a, int b) {  
    return a + b;  
}
```

# [[References]]

# [[Data Structures]]

# [[Arrays]]

```cpp
std::string cars[] = {"BMW X5", "Audi A8", "Mercedes benz"};  
  
cars[1] = "Audi Q8";  
  
std::cout << cars[0] << std::endl;  
std::cout << cars[1] << std::endl;  
std::cout << cars[2] << std::endl;  
  
std::string models[3];  
models[0] = "BMW";  
models[1] = "Audi";  
models[2] = "Mercedes";  
  
std::cout << models[0] << std::endl;  
std::cout << models[1] << std::endl;  
std::cout << models[2] << std::endl;

int arr[3]{}; // -> {0, 0, 0} initializes them with 0s as a default value
```
### std::array

```cpp
std::array<int, 5> arr {1, 2, 3, 4, 5};  
std::cout << arr.size() << std::endl;  
  
for (const int& el: arr)        
	std::cout << el << ' ';    
std::cout << '\n';  
```

## Strings

```cpp
#include <iostream>;
#include <array>

int main() {
	std::array<char, 5> symbols = {'A', 'B', 'C', 'D', 'E'};

	for (int i = 0; i < symbols.size(); i++) {
		std::cout << symbols[i] << ' ';
	}

	return 0;
}
```
## C Strings

```cpp
  
char text[] = {'d', 'o', 'g', 's', ' ', 'c', 'a', 't', 's', '\0'};  
  
std::string name = "Ivan";  
  
std::cout << strlen(text); // length of a C string


int nums[2];
int nums[2] {};
// разлика в инициализирането
 
std::string text = "Ivan";  
  
text += std::string(" Chakal") + std::string(" and IDK...");


std::string word = "Peter";  
  
auto position = word.find("I");  
  
if (position == std::string::npos)  || if (position == -1)
    std::cout << "Element not found";  
else  
    std::cout << "Element found";


#include <iostream>  
using std::string;   
  
int main() {  
    char word[] = {'W', 'o', 'r', 'd', '\0'};  
  
    std::cout << word; // prints the char array if it ends with \0;  
  
    return 0;  
}
```

## Find()

```cpp
#include <iostream>  
#include <vector>  
  
int main() {  
    const std::string word = "Peter";  
  
    const std::string searchStr = "e";  
  
    std::pmr::vector<int> positions;  
  
    while (true) {  
        int position = word.find(searchStr, position + 1);  
        if (position == -1)  
            break;  
        positions.push_back(position);  
    }  
    if (positions.size() > 0)  
        for (const int index : positions)  
            std::cout << index << std::endl;  
    else  
        std::cout << "Searched Substring not found." << std::endl;  
}
```

## File Streams

```cpp
#include <fstream>  
#include <string>  
  
int main() {  
    std::ifstream fileStream("C:\\Programming\\C++\\C++ProjectCLion\\input.txt");  
    std::ofstream outputFileStream("C:\\Programming\\C++\\C++ProjectCLion\\output.txt");  
  
    std::string line;  
  
    while (std::getline(fileStream, line)) {  
        outputFileStream << line << std::endl;  
    }    
    
    fileStream.close();  
    outputFileStream.close();  
  
    return 0;  
}
```

# Vectors

```cpp
#include <iostream>;
#include <vector>

int main() {
	std::vector<int> numbers(10, 10);
	// Initialize Vector of 10 elements with default value 10.

	std::vector<int> numbers;

	numbers.push_back(10); // add element to the end
	numbers.push_back(20); // add element to the end
	numbers.push_back(30); // add element to the end
	numbers.push_back(40); // add element to the end

	numbers.pop_back(); // remove element from the end

	for (int i = 0; i < numbers.size(); i++) {
		std::cout << numbers[i] << ' ';
	}

	return 0;			  
}
```

## Size of 

```cpp
std::string name = "Peter";  
double gpa = 2.5;  
char letter = 'P';  
bool isMale = true;  
  
std::cout << sizeof(name) << " bytes" << std::endl;  
std::cout << sizeof(gpa) << " bytes" << std::endl;  
std::cout << sizeof(letter) << " bytes" << std::endl;  
std::cout << sizeof(isMale) << " bytes" << std::endl;  
  
int numbers[] = {1, 2, 3, 4, 5};  
std::string students[] = {"Peter", "Stiliyan", "Gabi"};  
  
// dividing the size of the array by the size of one element 
// to find the number of elements (length) 
std::cout << sizeof(numbers) / sizeof(int) << " number elements" << std::endl;  
std::cout << sizeof(students) / sizeof(std::string) << " student elements" << std::endl;


std::string students[] = {"Bogdan", "Kristiyan", "Ivan"};  
  
for (int i = 0; i < sizeof(students) / sizeof(std::string); ++i) {  
    std::cout << students[i] << std::endl;  
}
```

## Fill Command

```cpp
int main() {  
    std::string names[10];  
  
    int size = sizeof(names) / sizeof(std::string);  
  
    fill(names, names + size / 2, "Ivan");  
    fill(names + size / 2, names + size, "Peter");  
  
    for (std::string name : names) {  
        std::cout << name << std::endl;  
    }  
  
    return 0;  
}
```

## Multiple Single Line String concatenation

```cpp
#include <iostream>  
using std::string;  
  
int main() {  
    string greeting = "Hello ";  
  
    // greeting += "there!\n" + "General " + "Kenobi!"; incorrect 
    // (cannot concatenate a C string)  
    greeting += string("there!\n") + string("General ") + string("Kenobi!");  
  
    std::cout << greeting;  
  
    return 0;  
}
```

## Working with substrings

```cpp
#include <iostream>  
using std::string;  
  
int main() {  
    string greeting = "Hello there, General Kenobi!";  
  
    int position = greeting.find("Something");  
  
    // std::string::npos -> no such position || position == -1  
    if (position == std::string::npos) 
	    std::cout << "No such substring found!";  
    else 
	    std::cout << "Substring found at index: " + position;  
  
    return 0;  
}
```

## Multidimensional Arrays

```cpp
int main() {  
    std::string cars[3][3] = {  
        {"Mercedes", "Benz G", "Germany"},  
        {"BMW", "X5", "Germany"},  
        {"Audi", "Q8", "Germany"},  
    };  
    
    int matrixSize = sizeof(cars) / sizeof(cars[0]);  
    int rowSize = sizeof(cars[0]) / sizeof(std::string);  
    
    std::cout << "| Make | Model | Country |" << std::endl;  
  
    for (int i = 0; i < matrixSize; i++) {  
        std::cout << " | ";  
  
        for (int j = 0; j < rowSize; j++) {  
            std::cout << cars[i][j] << " | ";  
        }  
        std::cout << std::endl;  
    }  
  
    return 0;  
}
```

# Linked-Lists

```cpp
#include <iostream>  
#include <vector>  
#include <list>  
  
using std::string;  
using std::vector;  
using std::cin;  
using std::cout;  
  
  
int main() {  
    std::list<int> numbers;  
  
    numbers.push_back(20);  
  
    // add to the beginning  
    numbers.push_front(10);  
  
    // insert at any position  
    numbers.insert(++numbers.begin(), 15);  
  
    for (const int& num : numbers)  
        std::cout << num << ' ';  
  
    return 0;  
}
```

# [[C++ Data Structures]]

# C++ Pairs

```cpp
#include <iostream>  
#include <utility>  
  
int main() {  
    std::pair<std::string, int> person {"Peter", 21};  
  
    // key                   
	std::cout << person.first << ' ';  
    // value  
    std::cout << person.second << std::endl;  
  
    person.first = "Ivan"; // setting the first value  
    person.second = 17; // setting the second value  
  
    std::pair<std::string, std::pair<double, double>> sight {  
        "The great pyramid of Gyza",  
        {29.97922345, 31.1342019}  
    };  
}
```

# Maps
- **`std::map`:** Maintains keys in sorted order (logarithmic complexity for operations) and uses a balanced tree structure.

- **`std::unordered_map`:** Does not maintain any order (average constant-time complexity for operations) and uses a hash table.

```cpp
#include <iostream>  
#include <map>  
  
int main() {  
    using namespace std;  
  
    map<string, int> peopleAge;  
  
    peopleAge.insert({"Peter", 21});  

	peopleAge.erase("Peter");

    map<string, int>::iterator i = peopleAge.find("Peter");  
  
    if (i != peopleAge.end())  
        cout << i->first << " " << i->second << endl;  
    else  
        std::cout << "not found";  
	}
```

# Pass by Reference

```cpp
// pass by reference
void swap(std::string &first, std::string &second) { 

    std::string temp = first;  
  
    first = second;  
    second = temp;  
}  
  
int main() {  
    std::string x = "Kool-Aid";  
    std::string y = "Darkside";  
  
    swap(x, y);  
  
    std::cout << x << std::endl;  
    std::cout << y;  
  
    return 0;  
}
```

# Pass by Address

```cpp
//pass by address
void doubleNumber(int *number) {  
    *number *= 2;  
}  
  
int main() {  
    int number = 10;  
  
    doubleNumber(&number);  
  
    printf("%d", number);  
  
    return 0;  
}
```

# Pointers and References

```cpp
int main() {  
    std::string name = "Peter";  
    int age = 21;  
    std::string names[4] = {"Ivan", "Vladi", "Rosen", "Kristian"};  
  
    std::string *pName = &name; // pointer to the address of name  
    int *pAge = &age; // pointer to the address of age  
    std::string *pNames = names; // pointer to the first element of names
  
    std::cout << *pName << std::endl;  
    std::cout << *pAge << std::endl; 

	int arraySize = sizeof(names) / sizeof(std::string);
	
	for (int i = 0; i < arraySize; i ++) {  
	    std::cout << i + 1 << ". " << pNames[i] << std::endl;  
	}
  
    return 0;  
}
```

## NULL pointer

```cpp
#include <iostream>  
  
int main() {  
    int *pNum = NULL;  
    pNum = new int;  
    *pNum = 10;  
   
	std::cout << "Address: " << pNum << std::endl;  
    std::cout << "Value: " << *pNum << std::endl;  
    
    delete pNum; // freeing up the memory  


    char *pGrades = NULL;  
  
    int arrNumber;  
    std::cin >> arrNumber;  
  
    pGrades = new char[arrNumber];  
  
    for (int i = 0; i < arrNumber; ++i) {  
        char currentChar;  
        std::cin >> currentChar;  
        pGrades[i] = currentChar;  
    }  
    
    for (int i = 0; i < arrNumber; ++i) {  
        std::cout << pGrades[i] << std::endl;  
    }  
    
    delete[] pGrades;  
  
    return 0;  
}
```

# Function Templates (Generics)

```cpp
#include <algorithm>  
#include <iostream>  
  
template<typename T>  
T findMax(T x, T y) {  
    return std::max(x, y);  
}  
  
int main() {  
    std::cout << findMax(10, 20) << std::endl;  
    std::cout << findMax(2.5, 2.1) << std::endl;  
    std::cout << findMax('Q', 'V') << std::endl;  
  
    return 0;  
}

#include <iostream>  
  
template<typename T, typename U>  
auto findMax(T x, U y) {  
    return x > y ? x : y;  
}  
  
int main() {  
    std::cout << findMax(10, 1.2) << std::endl;  
    std::cout << findMax(2.5, 'S') << std::endl;  
  
    return 0;  
}
```

# Struct
- By default fields are public

```cpp
#include <iostream>  
  
struct student {  
    std::string name;  
    double grade;  
    bool isEnrolled = true;  
};  
  
int main() {  
	// creating a member of the Struct
    student me;  
    me.name = "Peter";  
    me.grade = 6.00;  
    me.isEnrolled = false; 
     
    std::cout << me.name << std::endl;  
    std::cout << me.grade << std::endl;  
    std::cout << (me.isEnrolled ? "Yes" : "No") << std::endl;  
  
    return 0;  
}
```

# Getters and Setters

```cpp
class Stove {  
private:  
    int temperature;  
  
public:  
    // getter  
    int getTemperature() {  
        return temperature;  
    }  
    
    // setter  
    void setTemperature(int temperature) {  
        if (temperature < 0) 
	        this->temperature = 0;  
        else if (temperature > 1'000'000) 
	        this->temperature = 1'000'000;  
        else 
	        this->temperature = temperature;  
    }  
    
    // constructor invoking the setter  
    Stove(int temperature) {  
        this->setTemperature(temperature);  
    }
};
```

# Inheritance

```cpp
#include <iostream>  
  
using std::string;  
  
class Animal {  
public:  
    bool isAlive;  
  
    void eat() {  
        std::cout << "The animal is eating..." << std::endl;  
    }  
    Animal(bool isAlive) {  
        this->isAlive = isAlive;  
    }
};  
  
class Dog : public Animal {  
public:  
    string furColor;  
  
    void bark() {  
        std::cout << "The dog is barking..." << std::endl;  
    }  
    Dog(bool isAlive, string furColor): Animal(isAlive) {  
        this->furColor = furColor;  
    }
};  
  
class Cat : public Animal {  
public:  
    int whiskersLength;  
  
    void meow() {  
        std::cout << "The car is meowing..." << std::endl;  
    }  
    Cat(bool isAlive, int whiskersLength) : Animal(isAlive) {  
        this->whiskersLength = whiskersLength;  
    }
};  
  
int main() {  
    Animal animal(true);  
    animal.eat();  
  
    Dog dog(true, "Black");  
    dog.bark();  
  
    Cat cat(true, 10);  
    cat.meow();  
  
    return 0;  
}
```

## Comparing floating point numbers

```cpp
#include <iostream>  
  
int main() {  
    using namespace std;  
  
    double firstSideA, firstSideB;  
  
    cin >> firstSideA >> firstSideB;  
  
    double secondSideA, secondSideB;  
  
    cin >> secondSideA >> secondSideB;  
  
    double firstArea = firstSideA * firstSideB;  
    double secondArea = secondSideA * secondSideB;  
  
    cout << (abs(firstArea - secondArea) < 0.01); 
    // Check if the difference is less than 0.01  
  
    return 0;  
}
```

# [[C++ Streams]]

# Next course: [[C++ Advanced]]