# Pass by Address & Pass by Reference

```cpp
#include <iostream>  
  
// pass by address  
void func(int *i) {  
    *i = 1;  
}  
  
// pass by reference  
void func2(int &y) {  
    y = 10;  
}  
  
void func3(const int &y) {  
    std::cout <<  
        "Printing the variable inside the function, where it's declared as a constant: " << y << std::endl;  
}  
  
int main() {  
    int x = 5;  
  
    func(&x);  
    std::cout << x << std::endl;  
  
    func2(x);  
    std::cout << x << std::endl;  
  
    func3(x);  
  
    return 0;  
}
```

### Variable initialization

```cpp
// () initialization
int num(5);
std::string name("Peter");
double grade(5.50);

// {} initialization
int x{7};  
double y{2.5};  
std::string s{"Let us begin"};
```

## Assigning address to a pointer

```cpp
int a{5};  
  
// initialize a pointer of type int  
int *p;  
  
// assigning the address of a to the pointer p  
p = &a;  
  
std::cout << "Value of the pointer p: " << p << std::endl;  
std::cout << "Dereferencing the pointer p: "<< *p << std::endl;
```

## Using scanf() instead of std::cin >>

```cpp
#include <stdio.h>  
  
int main() {  
    int i;  
    long lng;  
    float flt;  
    double dbl;  
  
  
    printf("Enter a value for i: ");  
    scanf("%d", &i);  
  
    printf("Enter a value for lng: ");  
    scanf("%ld", &lng);  
  
    printf("Enter a value for flt: ");  
    scanf("%f", &flt);  
  
    printf("Enter a value for dbl: ");  
    scanf("%lf", &dbl);  
  
    return 0;  
}
```

## Static Variable

```cpp
#include <stdio.h>  
  
void printVars(int a1, int a2, int a3) {  
    // The variable will retain its value across multiple calls to the function.  
    static int count = 0;  
    // Initialized only once (when the program first encounters that line), and it keeps its value between function calls.  
  
    printf("The value of param 1 is: %d\n", a1);  
    printf("The value of param 2 is: %d\n", a2);  
    printf("The value of param 3 is: %d\n", a3);  
  
    count++;  
    printf("I have been invoked %d times.\n\n", count);  
}  
  
int main() {
    printVars(10, 20, 30);  
  
    printVars(100, 200, 300);  
  
    return 0;  
}
```

## Formatting numbers for readability

```cpp
#include <iostream>  
   
int main() {  
    const int one_million = 1'000'000;  
    const pi = 3.141'593;
  
    std::cout << one_million << std::endl;  
}
``` 


```cpp
const char *word = "Hello world!";
```

## Type Casting

```cpp
#include <iostream>  
  
int main() {  
    int number = 'A';  
  
    std::cout << number << std::endl;  
  
    std::cout << (char) number << std::endl; // c cast
    
    std::cout << static_cast<char>(number); // more readable  
}
```

# Insert Iterators

```cpp
  
#include <iostream>  
#include <vector>  
#include <list>  
  
int main() {  
    std::vector<int> numbersVector;  
  
    // Calls the push_back method  
    auto backInsertIter = std::back_inserter(numbersVector);  
  
    *backInsertIter = 100; // numbersVector.push_back(100);  
    *backInsertIter = 200; // numbersVector.push_back(200);  
  
    for (const int& number : numbersVector)  
        std::cout << number << ' ';  
    std::cout << '\n';  
  
    std::list<int> numbersList;  
  
  
    // Calls the push_front method  
    auto frontInsertIter = std::front_inserter(numbersList);  
  
    *frontInsertIter = 100; // numbersList.push_front(100);  
    *frontInsertIter = 50; // numbersList.push_front(50);  
  
    for (const int& number : numbersList)  
        std::cout << number << ' ';  
    std::cout << '\n';  
  
  
    std::vector<int> numbers{1, 2, 4, 5};  
  
    // Insert value at a given iterator  
    auto insertIterator = std::inserter(numbers, numbers.begin() + 2);  
    *insertIterator = 3;  
  
    for (const int& number : numbers)  
        std::cout << number << ' ';  
  
  
    return 0;  
}
```


```cpp
#include <iostream>  
  
int main() {  
  
    std::string num = "12345678";  
  
    std::string::const_iterator cit;  
  
    for (cit = num.cbegin(); cit != num.cend(); ++cit) {  
        std::cout << *cit << ", ";  
    }  
    std::cout << std::endl;  

	// reverse iterator
    for (std::string::reverse_iterator rit = num.rbegin(); rit != num.rend(); ++rit) {  
        std::cout << *rit << ", ";  
    }    std::cout << std::endl;  
  
    int arr[] = {1, 2, 3, 4, 5};  
  
    for (int &el : arr) {  
        el += 2;  
    }  
    // Const iterator  
    for (auto it = std::cbegin(arr); it != std::cend(arr); ++it) {  
        std::cout << *it << ", ";  
    }  
}
```

# Function Pointers

```cpp
#include <iostream>  
  
using namespace std;  
  
void func(int x, int y) {  
    printf("%d * %d = %d", x, y, x * y);  
}  
  
int main() {  
    auto func_pointer = &func;  
  
    (*func_pointer)(5, 10);  
}
```

# Working with Strings

```cpp
#include <iostream>  
  
using namespace std;  
  
int main() {  
    std::string name = "Peter";  


    std::string nameSubstring = name.substr(2); // ter  
  
    nameSubstring[1] = 'a'; // change the element at index 3  


    std::cout << nameSubstring << std::endl;  
  
    std::cout << name.find('e') << std::endl; // indexOf  
  
    std::cout << name.find("eter") << std::endl; // indexOf  
  
    std::cout << name.rfind('e') << std::endl; // lastIndexOf  
  
    std::string vowels{"aeiou"};  
  
    std::cout << name.find_first_of(vowels) << std::endl; 
    // search for any of the elements in vowels  

	std::string name = "Peter";  
  
	name.insert(1, " Who? \n");  
  
	
	std::string word = "Word";  
  
	word.erase(1, 1); // remove from the first index 1 element  
  
	std::cout << word << std::endl;  


	word.assign("Assign"); // replacing the old value with the new one  
  
	std::cout << word << std::endl;
}
```

# Swap Variables

```cpp
#include <iostream>  
  
int main() {  
    std::string name1 = "Peter";  
    std::string name2 = "Ivan";  
  
    std::swap(name1, name2);  
  
    std::cout << "name1: " << name1 << std::endl;  
    std::cout << "name2: " << name2 << std::endl;  
  
    return 0;  
}
```

# String Functions

```cpp
#include <iostream>  
  
int main() {  
    std::cout << "isupper(): " << isupper('A') << std::endl;  
    std::cout << "islower(): " << islower('a') << std::endl;  
    std::cout << "ispunct(): " << ispunct(':') << std::endl;  
    std::cout << "isspace(): " << isspace(' ') << std::endl;  
  
    std::cout << "toupper(): " << toupper('a') << std::endl;  
  
    return 0;  
}
```

## Numeric Functions

```cpp
int main() {
	std::vector<int>nums(10);  

	// Fill out the vector with the numbers from one++
	std::iota(nums.begin(), nums.end(), 1);  

	// Accumultate
	int sum = std::accumulate(nums.begin(), nums.end(), 0);
	
	for (int num : nums)  
	    std::cout << num << ' '
}	
```

## for_each

```cpp
std::string name = "Peter";  
  
int charSum{};  
std::for_each(name.begin(), name.end(), [&charSum](char& ch) {  
    charSum += ch;  
    std::cout << ch << '|';  
});
```

## Copy

```cpp
int main() {
	std::vector<int> vec1{1, 2, 3, 4, 5};  
	  
	std::vector<int> vec2(vec1.size());  
	// std::copy: O(n) time complexity  
	std::copy(vec1.cbegin(), vec1.cend(), vec2.begin());  
	  
	  
	std::vector<int> firstHalfOfVec1(vec1.size());  
	std::copy(vec1.begin(), vec1.begin() + vec1.size() / 2, firstHalfOfVec1.begin());  
	  
	std::vector<int> secondHalfOfVec1(vec1.size() / 2 + 1);  
	std::copy(vec1.begin() + vec1.size() / 2, vec1.end(), secondHalfOfVec1.begin());  
	  
	  
	std::vector<int> oddNums(vec1.size());  
	std::copy_if(vec1.begin(), vec1.end(), oddNums.begin(), [](int num) {  
	    return num & 1;  
	}); 
}
```

## Replace

```cpp
int main() {
	std::vector<int> nums{1, 2, 1, 2, 1, 2, 1, 2};  
	  
	  
	std::replace(nums.begin(), nums.end(), 2, 0);  
	  
	std::replace_if(nums.begin(), nums.end(), [](int num) { return !(num & 1);}, 5);
}
```

# Algorithms

```cpp
bool hasShorterLength(const std::string& first, const std::string& second) {  
    return first.length() < second.length();  
}  
  
bool hasLengthGreaterThan6(const std::string& str) {  
    return str.length() > 6;  
}  
  
int main() {  
    std::vector<std::string> names {"Peter", "Ivan", "Stiliyan", "Gabi"};  
  
    // sorting the vector by the predicate function hasShorterLength
    // The default std::sort uses Quick sort  
    std::sort(names.begin(), names.end(), hasShorterLength);  
  
    for (const std::string& name : names)  
        std::cout << name << ' ';  
  
    std::vector<std::string>::iterator iter = 
	    std::find_if(names.begin(), names.end(), hasLengthGreaterThan6);  
  
    std::cout << '\n' << *iter;  
    
    return 0;  
}
```

## Permutations

```cpp
#include <algorithm>  
#include <iostream>  
#include <vector>  
  
  
int main() {  
    std::vector<int> numbers{1, 20, 300, 4, 50, 60};  
  
    std::string str{"abc"};  
  
    // std::sort(str.begin(), str.end(), [](const char& a, const char& b) {return a > b;});  
  
    do {  
        std::cout << str << std::endl;  
    } while (std::next_permutation(str.begin(), str.end()));  
    // } while (std::prev_permutation(str.begin(), str.end()));  
  
    std::vector<int> numbersPerm{20, 300, 1, 4, 60, 50};  
  
    std::cout << std::is_permutation(  
        numbers.begin(), numbers.end(),  
        numbersPerm.begin(), numbersPerm.end()  
    );  
    return 0;  
}
```

## Lambda

```cpp
int main() {  
    std::vector<int> numbers {10, 21, 13, 40, 12};  
  
    std::sort(numbers.begin(), numbers.end(), [] (int f, int s) {return f > s;});  
  
    // for (const int& num : numbers)  
    //     std::cout << num << ' ';
      
    []() {  
        std::cout << "Lambda function print.\n";  
    }();  
  
    int fiveLength = 5; // dynamically using the var in the lambda  

	auto res = std::find_if(
	    numbers.begin(), 
	    numbers.end(), 
	    [fiveLength] (const int& num) {  
	        return num > fiveLength;  
	    }
    );  
    std::cout << *res;  

	// -----

	int n = 10;  
	
	// capture by reference
	std::cout << [&n](const int& a) {  
	    return a + (++n);  
	}(10) << '\n';

	// capture by value
	std::cout << [n](const int& a) {
		return n * a;
	}(100) << '\n';

	// -----

	// Template lambda function  
	auto sum = [](auto a, auto b) {  
	    return a + b;  
	};  
	  
	std::cout << sum(10, 120) << '\n';  
	std::cout << sum(5.5, 10.2) << '\n';


    return 0;  
}
```

## Splice

```cpp
int main() {  
    std::list<int> list1{1, 12, 6, 24};  
    std::list<int> list2{9, 3, 14};  
  
    auto iter = list1.begin();  
    std::advance(iter, 1); 
	    // Move the iterator n times  
    list1.splice(iter, list2); 
	    // Insert the content of list2 into list1 at the position of iter  
  
    for (const int& el : list1) // 1, 9, 3, 14, 12, 6, 24  
        std::cout << el << ' ';  
  
    return 0;  
}
```

## Const and Constexpr 

```cpp
// constexpr function
constexpr double milesToKm(const double& miles) {  
    constexpr double MILES_KM_RATIO = 1.602;  
  
    return miles * MILES_KM_RATIO;  
}

int main() {
	// Value is assigned beforehand, and CANNOT be changed  
	constexpr unsigned int HUNDRED = 100;  
	  
	unsigned int number;  
	std::cin >> number;  
	  
	const unsigned int constNumber{number};  
	std::cout << "Const but it's assigned dynamically: " << constNumber << '\n';
}
```

## Custom Error class 

```cpp
class badStudentGrade : public std::out_of_range {  
public:  
    badStudentGrade() : std::out_of_range("Invalid grade: please try again.") {}  
  
    badStudentGrade(const char* s) : std::out_of_range(s) {}  
    badStudentGrade(const std::string& s) : std::out_of_range(s) {}  
  
    badStudentGrade(const badStudentGrade& other) = default;  
    badStudentGrade& operator=(const badStudentGrade& other) = default;  
};  
  
int main() {  
    int grade = 5;  
  
    if (grade < 0) {  
        throw badStudentGrade("bad grade");    
	} 
	else        
		throw badStudentGrade();  
  
    return 0;}
```

