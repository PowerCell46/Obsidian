# References

references = pseudonyms/псевдоними

```cpp
// passing the list by reference (by default is by value!)
void removeNegative(std::list<int> &numbers) {  
  
    for (auto iter = numbers.begin(); iter != numbers.end(); ) {  
        if (*iter < 0)  
            iter = numbers.erase(iter);  
            // Erase returns the iterator to the next element  
        else  
            ++iter;  
            // Only increment if no element was erased  
    }  
}
```

## Null Pointer

```cpp
#include <iostream>  
#include <vector>  
  
int* findIndexOfNegativeNum(std::vector<int>& numbers, int& resultCount) {  
    int* result = nullptr;  
    resultCount = 0;  
  
    for (int i = 0; i < numbers.size(); i++) {  
        if (numbers[i] < 0) {  
            result = &numbers[i];  
            resultCount++;  
        }    
	}  
    return result;  
}  
  
int main() {  
    std::vector<int> numbers;  
    numbers.push_back(0);  
    numbers.push_back(-5);  
    numbers.push_back(-2);  
  
    int resultCount;  
  
    int* p = findIndexOfNegativeNum(numbers, resultCount);  
  
    std::cout << *p << ' ' << resultCount;  
  
    return 0;  
}
```

## <span style="color:rgb(255, 0, 0)">Const Pointer Types</span>

```cpp
// Pointer to Constant Data  
  
const int* ptr; // cannot change the value of the pointer  
int value1 = 10, value2 = 20;  
ptr = &value1;  
  
// *ptr = 15; // with const type* -> cannot modify the value through the pointer  
ptr = &value2;  
  
  
// Constant Pointer to Data  
  
int value = 10, value2 = 20;  
int* const ptr = &value; // cannot change the address, the value can change -> reference  
*ptr = 20;  
ptr = &value2;  
  
  
// Constant Pointer to Constant Data  
  
int value = 10;  
const int* const ptr = &value; // cannot change the value and the address -> const reference  
*ptr = 20;  
ptr = &value;
```

## Pointers and type casting

```cpp
int main() {
	int year = 2024;  
	int* intPtr = &year;  
	char* charPtr = (char*)&year; // casting int reference to a char pointer  
	std::cout << charPtr << std::endl;  
	  
	char ch = 'a';  
	char* chP = &ch;  
	
	// It expects the pointer to point to a null-terminated string
	std::cout << chP << std::endl; //char pointer = C string
	
	// std::cout knows how to handle void* pointers and prints the address
	std::cout << (void*)chP << std::endl; 
}
 
int main() {  
    // C style cast  
    char chars[]{'a', 'b', 'c', 'd'};  
    int* p = (int*)&chars;  
    *p = 842281524;  
    
    for (int i = 0; i < 4; ++i)  
        std::cout << chars[i] << ' ';  

    return 0;  
}
```

### Void Pointer

```cpp
int main() {  
    int age = 21;  
    std::string name = "Peter";  
  
    int* agePointer = &age;  
    std::string* namePointer = &name;  
  
    void* voidPointer; 
    // Can point to anything -> !Cannot reference & dereference & no pointer arithmetic  
  
    voidPointer = agePointer;  
    voidPointer = namePointer;  
  
    std::cout << voidPointer << '\n'; 
    // Prints the address (!CANNOT DEREFERENCE)  
    
    std::string* castedStringPointer = (std::string*) voidPointer;  
  
    std::cout << *castedStringPointer << '\n'; 
    // When casted back to string pointer -> CAN DEREFERENCE  
  
    return 0;  
}
```

## Function Pointers

```cpp
void printOnTheConsole() {  
    std::cout << "Printing..." << std::endl;  
}  
  
int main() {    
    std::cout << &main << std::endl;  
  
    int (*func)() = &main; // pointer to a function  
  
    (*func)();  
    
    void (*printFunc)() = &printOnTheConsole;  
  
    (*printFunc)();  
  
    return 0;  
}
```

___
## Initializing a Class Static Value

```cpp
class Person {  
    static int id;  
    std::string name;  
    int age;  
    const int personId;  
  
    static std::string toLowerCase(std::string& str) {  
        std::stringstream resultStream{};  
        for (const char& ch : str)  
            resultStream << (char) tolower(ch);  
        return resultStream.str();  
    }    
    
    friend void getline(std::istream& os, Person& p); 
     
public:  
    static const int numberOfLegs = 2;  

	Person() : personId(++id) {}  
    
    Person(std::string name, int age) : 
	    name(toLowerCase(name)), age(age), personId(++id) {}  
  
    int getPersonId() const {return this->personId;}  
    int getAge() const {return this->age;}  
    std::string getName() const {return this->name;}  
  
    void setName(std::string name) {  
        std::cout 
	        << "Changing name from: " << this->getName() 
	        << " to: " << name << std::endl;  
        this->name = toLowerCase(name);  
    }    
    void setAge(const int age) {  
        std::cout 
	        << "Changing age from: " << this->getAge() 
	        << " to: " << age << std::endl;  
        this->age = age;  
    }
};  
  
int Person::id = 0;  

// friend function, accessing private fields of Person
void getline(std::istream& os, Person& p) {   
    os >> p.name >> p.age;  
}  
  
std::ostream& operator<<(std::ostream& os, Person& p) {  
    return os  
        << "Person with name: " << p.getName() << ", age: " << p.getAge()  
        << " and ID: " << p.getPersonId() << '.' << '\n';  
}
```


```cpp
// const method -> does not change the object values
bool hasNameLengthBiggerThan(const int length) const {  
    return this->name.length() > length;  
}
```

# Friend Function

```cpp
class Employee {  
    int employeeId;  
    std::string name;  
public:  
    static int id;  
    Employee(const std::string& name) :  
	    name(name), employeeId(++Employee::id) {}  
  
    const int getEmployeeId() const {  
        return this->employeeId;  
    }  
    bool hasNameLengthBiggerThan(const int length) const {  
        return this->name.length() > length;  
    }  
    
    friend void setEmployeeFields(const int employeeId, const std::string& name, Employee& e);  
};  
  
  
void setEmployeeFields(const int& employeeId, const std::string& name, Employee& e) {  
        e.employeeId = employeeId;  
        e.name = name;  
}
```

# Operator Overloading

```cpp
class Vector {  
    int x, y;  
public:  
	// constructors
	Vector() 
		: x(), y() {}  
    Vector(const int x, const int y) 
	    : x(x), y(y) {}  

	// getters
    int getX() const {return this->x;}  
    int getY() const {return this->y;}  

	// setters
    void setX(const int x) {this->x = x;}  
    void setY(const int y) {this->y = y;}  

	// operators
    Vector& operator+=(const Vector& other) {  
        this->x += other.getX();  
        this->y += other.getY();  
        return *this;  
    }  
    
    Vector operator+(const Vector& other) const {  
        const Vector v{
	        this->getX() + other.getX(),
	        this->getY() + other.getY()
        };  
        return v;  
    }  
    
    Vector& operator-=(const Vector& other) {  
        this->x -= other.getX();  
        this->y -= other.getY();  
        return *this;  
    }  
    
    Vector operator-(const Vector& other) const {  
        const Vector v{
	        this->getX() - other.getX(), 
	        this->getY() - other.getY()
		};  
        return v;  
    }  
    
    Vector& operator++() { // ++vector  
        this->x++;  
        this->y++;  
  
        return *this;  
    }  
    
    Vector operator++(int) { // vector++  
        const Vector temp = *this;  
        this->x++;  
        this->y++;  
  
        return temp;  
    }

	// friend functions
    friend Vector& operator+=(Vector& v, const int& number);  
  
    friend int operator+=(int& number, const Vector& v);  
  
    friend std::istream& operator>>(std::istream& is, Vector& v);  
};  

// non-member operators
Vector& operator+=(Vector& v, const int& number) { // vector += int  
    v.x += number;  
    v.y += number;  
  
    return v;  
}  
  
// различни асигнатури на един и същи оператор!
  
int operator+=(int& number, const Vector& v) { // int += vector  
    number += v.getX();  
    number += v.getY();  
  
    return number;  
}  
  
std::ostream& operator<<(std::ostream& os, const Vector& v) {  
    return os << "X: " << v.getX() << ", Y: " << v.getY() << '\n';  
}  
  
std::istream& operator>>(std::istream& is, Vector& v) {  
    return is >> v.x >> v.y;  
}
```

## <span style="color:rgb(255, 0, 0)">Conversion Operator</span>

```cpp
class Test {  
    int i{42};  
public:  
    Test(const int& i) : 
	    i{i} {}  
    Test() {}  
  
	// int conversion operator  
    operator int() const {
	    return this->i;
	}  
};

int main() {  
    const Test t = 10;  
  
    std::cout << t;  
  
    return 0;  
}
```

```cpp
class Point {  
    int x, y;  
public:  
    Point(const int& x, const int& y)  
        : x(x), y(y) {}  
    Point() = default;  
  
    Point& operator+=(const Point& other) {  
        this->x += other.x;  
        this->y += other.y;  
        return *this;  
    }  
    
    bool operator==(const Point& other) const {  
        return this->x == other.x && this->y == other.y;  
    }  
    bool operator!=(const Point& other) const {  
        return !(*this == other);  
    }  
    
    friend bool operator<(const Point&, const Point&);  
    friend bool operator>(const Point&, const Point&);  
  
    friend std::ostream& operator<<(std::ostream&, const Point&);  
    friend Point operator+(const Point& first, const Point& second);  
};  
  
Point operator+(const Point& first, const Point& second) {  
    Point p{first.x + second.x, first.y + second.y};  
    return p;  
}  
  
std::ostream& operator<<(std::ostream& os, const Point& p) {  
    return os << p.x << ' ' << p.y;  
}  
  
bool operator<(const Point& first, const Point& second) {  
    return first.x < second.x && first.y < second.y;  
}  
  
bool operator>(const Point& first, const Point& second) {  
    return first.x > second.x && first.y > second.y;  
}
// ATM the logic is not correct for cases where 
// first.x > second.x && first.y < second.y 
// or 
// first.x < second.x && first.y > second.y 
```

# Incrementing Operators

```cpp
#include <iostream>  
#include <vector>  
  
using std::string;  
using std::vector;  
using std::cin;  
using std::cout;  
  
class Fraction {  
    int numerator;  
    int denominator;  
  
public:  
    Fraction(const int numerator, const int denominator) : 
	    numerator(numerator), denominator(denominator) {}  
  
    Fraction& operator++(int) { // post increment  
        this->numerator++;  
  
        return *this;  
    }  
    
    Fraction& operator++() { // pre increment  
        this->numerator += this->denominator;  
  
        return *this;  
    }  
    
    int getNumenator() const {  
        return this->numerator;  
    }
};  
  
int main() {  
    Fraction fr = Fraction(10, 20);  
    ++fr;  
    fr++;  
  
    std::cout << fr.getNumenator();  
  
    return 0;  
}
```

# Declaring Operand outside the class

```cpp
#include <iostream>  
#include <vector>  
  
using std::string;  
using std::vector;  
using std::cin;  
using std::cout;  
  
  
class Vector {  
    int x, y;  
  
public:  
    Vector(const int x, const int y) : 
	    x(x), y(y) {}
	      
    // adding += functionality  
    Vector &operator+=(const Vector &other) {  
        this->x += other.x;  
        this->y += other.y;  
  
        return *this;  
    }  
    
    double getX() const {  
        return this->x;  
    }  
    
    double getY() const {  
        return this->y;  
    }
};  
  
Vector operator+(const Vector &first, const Vector &second) {  
    return Vector(  
        first.getX() + second.getX(),  
        first.getY() + second.getY()  
    );  
}  
  
  
int main() {  
    Vector a(10, 15);  
    Vector b(15, 10);  
  
    Vector c = a + b;  
  
    return 0;  
}
```

![[Pasted image 20241109164327.png]]

## <span style="color:rgb(255, 0, 0)">Functor class
</span>


```cpp
class evenp { // functor class  
public:  
    bool operator() (int n) const {  
        return !(n & 1);  
    }
};  
  
class divisible { // functor class  
    int divisor;  
public:  
    divisible(const int& divisor) : divisor(divisor) {}  
  
    bool operator() (int number) {  
        return number % this->divisor == 0;  
    }  
};

int main() {  
    evenp is_even;  
  
    std::cout << is_even(10) << '\n';  
  
    divisible div{3};  
  
    std::cout << div(12) << '\n';  
  
    return 0;  
}
```

____
## Include Directive
###### The `#include` directive in C++ inserts the contents of a specified file into the source code at compile time, providing access to declarations and definitions needed for the program. 
###### It essentially acts as a <span style="color:rgb(107, 255, 174)">copy-paste operation</span>, inserting the file's code into the source file before compilation begins.

```cpp
#include <iostream> // importing a system file
#include "HelloWorld.h" // importing a local file

int main() {
	hello();

	return 0;
}
```

![[Pasted image 20241109230004.png]]

## Define Directive
###### A preprocessor command used to create symbolic constants or macros, <span style="color:rgb(107, 255, 174)">replacing occurrences of the defined name with its associated value or code during preprocessing before compilation</span>.

```cpp
#include <iostream>
#include "HelloWorld.h"

#define PART_OF_DEF 2
#define DEF 10 + PART_OF_DEF
#define Main int main()
#define SQUARE(x) ((x) * (x))

Main {
	std::cout << DEF << '\n';

	hello();

	#undef DEF

	return 0;
}
```

## IfDef & IfnDef Directive
###### pre-processor commands used to conditionally include or exclude parts of the code based on whether a macro is defined.
###### **`#ifdef` and `#ifndef` work exclusively with macros defined using `#define`**, as they are part of the **preprocessor**, which operates on macros before the actual compilation begins.

### Header guard example

```cpp
#ifndef HELLO_WORLD_H  
  
#define HELLO_WORLD_H  
  
#include <iostream>  
#define PRINT(S) std::cout << (S) << std::endl;  
  
namespace HelloWorld {  
    inline void hello() {  
        PRINT("Hello from 'HelloWorld.h'!")  
    }
}    
#endif
```

## Static & Inline Static

```cpp
class Company {  
public:  
    // static int CREATED_COMPANIES;  
    inline static int CREATED_COMPANIES = 0;  
  
    Company() {++CREATED_COMPANIES;}  
};  
  
// int Company::CREATED_COMPANIES = 0;
```

## Static Class Property

```cpp
class Company {  
public:  
    const int id;
      
    Company (const int& id) : 
	    id(id) {}  
	    
    void setId(const int& id) const {  
        // this->id = id; // NOT VALID  
    }  
};
```

## Mutable

```cpp
class Person {  
    int age;  
    const std::string name;  
    mutable int numberOfAgeChecks = 0; // used for caching, logs, mutexes and other metadata  
  
public:  
    Person(std::string& name, int& age) : 
	    name(name), age(age) {}  
  
    int getAge() const {  
        this->ageChecks++;  
        return this->age;  
    }  
    int getAgeChecks() const {  
        return this->numberOfAgeChecks;  
    }};
```

## Pragma once (modern Header guard)

```cpp
#pragma once
#include <iostream>

void hello() {
	std::cout << "Hello world" << std::endl;
}
```

# Templates

```cpp
#include <iostream>  
#include <vector>  
  
using std::string;  
using std::vector;  
using std::cin;  
using std::cout;  
  
template<typename T>  
T calcPercentage(const T& a, const T& b) {  
    return (a * 100) / b;  
} 

template<typename T>  
T sumTwo(const T& a, const T& b) {  
    return a + b;  
}

template<typename T>  
class Box {  
    T element;  
public:  
    Box(T element) : element(element) {}  
    Box(): element() {}  
  
    T getElement() const {  
        return this->element;  
    } 
     
    void setElement(const T& element) {  
        this->element = element;  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const Box& box) {  
        return os << "Box with Element: " << box.getElement() << '.' << '\n';  
    }  
    friend std::istream& operator>>(std::istream& is, Box& box) {  
        is >> box.element;  
        return is;  
    }
};

template<typename T>  
void printVector(std::vector<T> vec) {  
    std::stringstream resultStr{};  
    for (auto iter = vec.begin(); iter != vec.end(); ++iter)  
        resultStr << *iter << ' ';  
    std::cout << resultStr.str() << std::endl;  
}  
  
int main() {  
    const std::vector<int> nums{1, 2, 3, 4, 5};  
    const std::vector<std::string> strings{"a", "b", "c", "d", "e"};  
  
    printVector(nums);  
    printVector(strings);  
  
    return 0;  
}
```

## Class separation using Header and CPP files

```cpp
#include <iostream>
#include <vector>

class Person {
private:
	static std::string normalizeName(std::string name);
	
	std::string name;
	int age;

public:
	static const int LEGAL_DRINKING_AGE = 18;
	
	Person();
	Person(std::string name, int age); 

	std::string getName() const;
	int getAge() const;

	std::vector<Person> makeBabies(const Person& other) const;
};

int main() {
	Person me("PeTeR", 21);
	Person ivan{ "IvAn", 17 };

	std::cout << me.getName() << ' ' << me.getAge() << '\n';
	std::cout << ivan.getName() << ' ' << ivan.getAge() << '\n';

	return 0;
}

/* Beginning of Person Methods */

std::string Person::normalizeName(std::string name) {
	for (int i = 0; i < name.size(); i++) {
		name[i] = std::tolower(name[i]);
	}
	return name;
}

Person::Person() : 
	age(age) {}

Person::Person(std::string name, int age) :
	name(normalizeName(name)), age(age) {}

std::string Person::getName() const {
	return this->name;
}

int Person::getAge() const {
	return this->age;
}

std::vector<Person> Person::makeBabies(const Person& other) const {
	int count = rand() % 10;
	std::vector<Person> babies;

	for (int i = 0; i < count; i++)
		babies.push_back(Person("Baby", 0));

	return babies;
}
 
/* End of Person Methods */
```

## Types of Constant Pointers

```cpp
int main() {  
    // Pointer to a Constant  
    // the value cannot be modified through the pointer    
    // the pointer itself can be reassigned to point to another location 
    int a = 5;  
    int b = 10;  
  
    const int* intPointer = &a;  
    // (*intPointer)++; // invalid!  
    intPointer = &b;  
  
    //--------------------------------------  
    // Constant Pointer (like reference)    
    // pointer itself is constant and cannot be reassigned to point to another location    
    // the value can be modified  
    int first = 10;  
    int second = 20;  
  
    int* const intPtr = &first;  
    (*intPtr)++;  
    // intPtr = &second; // invalid!  
  
    //--------------------------------------    
    // Constant Pointer to a Constant (like const reference)    
    // both the pointer itself and the value it points to are constant  
    int val1 = 1;  
    int val2 = 2;  
  
    const int* const constIntPointer = &val1;  
    // (*constIntPointer)++; // invalid!  
    // constIntPointer  = &val2 // invalid!  
    return 0;  
}
```

# Smart Pointers

- Unique Pointers
- Shared Pointers
- Weak Pointers

```cpp
#include <iostream>  
#include <memory>  
#include <sstream>  
  
void uniquePointerExample();  
void sharedPointerExample();  
  
int main() {  
  
    uniquePointerExample();  
  
    sharedPointerExample();  
  
    return 0;  
}  
  
void uniquePointerExample() {  
    // Unique pointer  
    // Automatically deletes the managed object when it goes out of scope.    std::unique_ptr<int> intPtr = std::make_unique<int>(121);  
    ++(*intPtr);  
  
    std::cout << (*intPtr) << '\n';  
}  
  
void sharedPointerExample() {  
    // Shared pointer  
    // The resource is deleted only when the last shared_ptr managing it is destroyed.    
    std::shared_ptr<int> ptr1 = std::make_shared<int>(69);  
    ++(*ptr1);  
  
    std::shared_ptr<int> ptr2 = ptr1;  
  
    std::cout << (*ptr2) << '\n';  
}

/***************************/
// Passing unique and shared Ptrs to a function

void multiplyAndPrint(std::unique_ptr<int>& ptr, const int& multiplyBy) {  
    (*ptr) *= multiplyBy;  
  
    std::cout << (*ptr) << '\n';  
}  
  
void divideAndPrint(std::shared_ptr<int>& ptr, const int& divideBy) {  
    (*ptr) /= divideBy;  
  
    std::cout << (*ptr) << '\n';  
}  
  
  
int main() {  
    std::unique_ptr<int> uniquePtr = std::make_unique<int>(50);  
  
    multiplyAndPrint(uniquePtr, 3);  
  
  
    std::shared_ptr<int> sharedPtr = std::make_shared<int>(66);  
  
    divideAndPrint(sharedPtr, 3);  
  
    return 0;  
}

/***************************/
// Weak Pointer example

struct Child;  
  
struct Parent {  
    std::shared_ptr<Child> child;  
    ~Parent() { std::cout << "Parent destroyed\n"; }  
};  
  
struct Child {  
    std::weak_ptr<Parent> parent;  
    ~Child() { std::cout << "Child destroyed\n"; }  
};  
  
int main() {  
    std::shared_ptr<Parent> parent = std::make_shared<Parent>();  
    std::shared_ptr<Child> child = std::make_shared<Child>();  
  
    parent->child = child;  
    child->parent = parent;  
  
    return 0;  
}
```

# Next course: [[C++ OOP]]