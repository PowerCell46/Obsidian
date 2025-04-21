# Variable Types
#### STD: Standard Character Output
#### <<: Bitwise Shift Left

```cpp
#include <iostream> 
// functions for basics I/O operations

// contains all the standard functions, objects, and classes
using namespace std;

// importing only what we are using (better way)
using std::string; 
using std::cin;
using std::cout;

int main() {

	// Logging on the Console
	cout << "Ivan" << endl;

	std::cout << "Hello there!" << std::endl; // End of line symbol
	std::cout << "Hello there!" << "\n";


	//C++ Variables
	int x;
	x = 10;

	double price = 2.5;

	char grade = 'A';

	bool isTrue = true;

	std::string name = "Peter";

	// Constants
	const double PI = 3.14159;
	const int LIGHT_SPEED = 299792458;

	unsigned int a = 4'294'967'295;  
	(signed) int b = 2'147'483'647;

	return 0;
}
```

### Integers

`can be signed  or unsigned`
The sign == the most Significant Bit
- Leading 0 - positive number
- Leading 1 - negative number

##### Largest 8 bit signed int
01111111 - 127
##### Smallest negative 8 bit int
10000000 - -128

##### Largest signed 32 bit int
011111111 11111111 11111111 - 2147483647
##### Smallest negative 32 bit int
10000000 00000000 00000000 - 2147483648

## Formatting precision of Double numbers

```cpp
#include <iostream>  
  
int main() {  
    double num = 5.666;  
  
    std::cout.setf(std::ios::fixed);  
    std::cout.precision(2);  
  
    std::cout << num;  
  
    return 0;  
}

#include <iostream>  
#include <format>  
  
int main() {  
  
    std::cout  
        << std::format("Salary: {:.2f} leva.", 2498.998)  
        << std::endl;  
  
    return 0;  
}
```

## Working with Strings with spaces 

```cpp
#include <iostream>  
  
int main() {  
    std::string fullName;  
    std::getline(std::cin, fullName);  
  
    std::cout << fullName << std::endl;  
    return 0;  
}
```

# Math Functions

```cpp
#include <iostream>  
#include <math.h>  
#include <cmath>  
  
int main() {  
  
    double x = 3;  
    double y = 4;  
  
    // MAX  
    double z = std::max(x, y);  
    
    // MIN  
    z = std::min(x, y);  
    
    // Степен  
    z = pow(2, 3);  
    
    // Square Root  
    z = sqrt(9);  
    
    // Absolute Value  
    z = abs(-27);  
    
    // Rounding  
    z = round(4.4);  
    z = round(4.6);  
    
    // Rounding Up  
    z = ceil(4.2);  
    
    // Rounding Down  
    z = floor(4.8);  
  }
```

# C++ Switch
## Works only with chars, numbers and enums

```cpp
#include <iostream>  
  
int main() {  
    int month;  
    std::cin >> month;  
    
    switch (month) {  
        case 1:  
            std::cout << "January";  
            break;  
        case 2:  
            std::cout << "February";  
        break;  
        case 3:  
            std::cout << "March";  
        break;  
        case 4:  
            std::cout << "April";  
        break;  
        case 5:  
            std::cout << "May";  
        break;  
        case 6:  
            std::cout << "June";  
        break;  
        default:  
            std::cout << "IDC after June...";  
    }  
    
    char grade;  
    std::cin >> grade; 
     
    switch (grade) {  
        case 'A':  
            std::cout << "Excellent!";  
        break;  
        case 'B':  
            std::cout << "Very Good!";  
        break;  
        case 'C':  
            std::cout << "Good!";  
        default:  
            std::cout << "Failure...";  
    }  
    
    return 0;  
}
```

# Enums

```cpp
#include <iostream>  
  
enum (class) Color {  
    RED,  
    GREEN,  
    BLUE  
};  
  
int main() {  
    Color chosenColor = Color::RED;  
  
    std::cout << chosenColor;  
  
    return 0;  
}
```

# XOR operator

```cpp
#include <iostream>

int main() {
    bool condition1 = true;
    bool condition2 = false;

    // one of them must be true but not the two at the same time
    if (condition1 ^ condition2)
        std::cout << "One of them is true, but not both." << std::endl;
    else
        std::cout << "Either both are true or both are false." << std::endl;
    
	//--------------------------
	/* Find missing Pair using XOR */ 
	 
	int numberPairs[]{1, 1, 2, 2, 3};  
	  
	 int current = numberPairs[0];  
	 for (int i = 1; i < 5; ++i)  
	     current ^= numberPairs[i];  
	  
	 std::cout << current << std::endl;

    return 0;
}
```

# Check is odd using Bitwise AND

int number = 10; /* = 4 bytes */
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00001010

&

int one = 1; /* = 4 bytes */
1 byte   1 byte   1 byte   1 byte
00000000 00000000 00000000 00000001

bool result = false;
00000000 00000000 00000000 00000000

```cpp
int main() {  
    int x = 10;  
  
    if (x & 1)  
        std::cout << "Odd\n";  
    else  
        std::cout << "Even\n";  
  
    return 0;  
}
```

# String functions

https://cplusplus.com/reference/string/string/

```cpp
#include <iostream>  
  
int main() {  
    std::string name;  
  
    std::cout << "Enter your name: ";  
    std::getline(std::cin >> std::ws, name);  
  
    std::cout << name.length();  
  
    std::cout << name.empty();  
  
    name.clear();  

	name += "peter.gerdzhikov.contact";
  
    name.append("@gmail.com");  
  
    std::cout << name.at(0);

	std::cout << name[2];

	name.insert(0, "@");  
	  
	std::cout << name.find(' ');  
	  
	name.erase(0, 3);  
}
```

# For Loops

```cpp
#include <iostream>  
  
int main() {  
    std::string name;  
  
    std::getline(std::cin, name);  

	// for of Loop
    for (const char& letter : name) {  
        std::cout << letter << std::endl;  
    }  

	// traditional for Loop
    for (int i = 0; i < name.length(); i++) {  
        std::cout << name[i] << std::endl;  
    } 
     
    std::cout << "Last Letter is: " << name[name.length() - 1];  
  
    return 0;  
}
```

# Do-While Loop

```cpp
#include <iostream>  
  
int main() {  
    int number;  
  
    do {  
        std::cout << "Please enter a number: ";  
        std::cin >> number;  
        if (number <= 0) 
	        break; 
         
        std::cout << "Entered number: " << number << std::endl;  
  
    } while (true);  
  
    std::cout << "Invalid number entered... " << number;  
  
    return 0;  
}
```

# Primitive Type Limits

```cpp
#include <iostream>  
#include <climits>  
  
using std::string;  
using std::cout;  
using std::endl;  
  
int main() {  
    int biggestNumber = INT_MAX;  
    int smallestNumber = INT_MIN;  
  
    cout << biggestNumber << endl;  
    cout << smallestNumber << endl;  
  
    return 0;  
}
```

# Casting from String (Type Parsing)

- String to double (stod)
- String to int (stoi)

```cpp
std::string currentInput;  
  
std::cin >> currentInput;  
if (currentInput == "NoMoreMoney") break;  

// casting a string into a double
double currentAmount = std::stod(currentInput);

// casting a string digit into an integer
int value = std::stoi(std::string(1, currentNumberStr[j]));

// stringification
std::string numberStr = std::to_string(number);
```

# Random integers

```cpp
int main() {  
    int DICE_SIZE = 6;  
  
    srand(time(NULL));  
  
    int num = (rand() % DICE_SIZE) + 1;  
  
    std::cout << num;  
  
    return 0;  
}
``` 

# Typedefs & Namespaces

```cpp
#include <iostream>
#include <vector>

// Type Definitions
typedef std::vector<std::pair<std::string, int>> pairlist_t;
typedef std::string text_t;
typedef int number_t;

using text_t = std::string;
using number_t = int;


namespace first {
	int x = 1;
}

namespace second {
	int x = 2;
}


int main() {
	// Working with namespaces
	int x = first::x;

	using namespace first;

	std::cout << x;

	text_t firstName = "Peter";

	number_t age = 21;

	std::cout << firstName << " of age " << age << '\n';


	// Math Operations
	int students = 20;

	students += 2;
	students++;

	students--;
	std::cout << students << '\n';
	students -= 2;

	students *= 2;
	students /= 2;

	int remainder = students % 2;

	std::cout << remainder << " remainder\n";

	std::cout << students;


	// Type Conversions
	int i = (int) 100.14;

	char i_char = i;

	int correct = 8;

	int questions = 10;

	double score = (double) correct / questions * 100;

	std::cout << score << '%';

	// Reading from the Console
	text_t name;

	std::cout << "What is your name? ";
	std::cin >> name;

	std::cout << "Your name is " << name;

	return 0;
}

----
#include <iostream>  
#include <vector>  
#include <sstream>  
#include <map>  
  
typedef std::map<std::string, int> defOccurrences;  
  
int main() {  
    defOccurrences occurrences;  
  
    occurrences["Peter"]++;  
    occurrences["Ivan"]++;  
    occurrences["Stiliyan"]++;  
    occurrences["Gabi"]++;  
    occurrences["Gabi"]++;  

	// If the map already has an element with the same key, nothing happens 
	occurrences.insert({"Peter", 10}); 

	for (auto [key, value] : occurrences)  
	    std::cout << key << " -> " << value << '\n';
	 
    // for (auto& pair : occurrences)  
    //    std::cout << pair.first << " -> " << pair.second << '\n';  
  
    return 0;  
}
```

## Referring to the Global namespace

```cpp
class vector {  
    double x, y;  
public:  
    vector(int x, int y) : x(x), y(y) {}  
    double getX() const {return this->x;}  
    double getY() const {return this->y;}  
  
};  
  
std::ostream& operator<<(std::ostream& os, const vector& vector) {  
    return os 
	    << "Vector with X: " << vector.getX() 
	    << " and Y: " << vector.getY() << '.';  
}  
  
int main() {  
    using namespace std;  
  
    ::vector v{10, 15}; 
    // search in the global namespace (not in any namespace)  
  
    std::cout << v;  
  
    return 0;  
}
```

short int
short unsigned int

# Vector

```cpp
int main() {
	std::vector<int> numbers;

	numbers[2] = 10; // does not check if the index exists
	numbers.at(2) = 10; // does check if the index exists 
}
```

# Next course: [[C++ Fundamentals]]
