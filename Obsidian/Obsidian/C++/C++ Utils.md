## Print Operator

```cpp
struct Body {  
    int height;  
    int weight;  
  
    Body (const int height, const int weight) : weight(weight), height(height) {}  
};  
  
std::ostream& operator<<(std::ostream& os, const Body& body) {  
    os << "Body:\nHeight : " << body.height << ", Weight: " << body.weight;  
  
    return os;  
}
```

## Returning multiple values

```cpp
#include <iostream>  
  
struct returnValues {  
    int x, y;  
};  
  
  
returnValues getReturnValues(const int n) {  
    return { n, n + 1 };  
}  
  
  
int main() {  
    auto [x, y] = getReturnValues(5);  
  
    std::cout << x << ' ' << y;  
  
    return 0;  
}
```

## Lambda Functions

```cpp
#include <iostream>  
  
int main() {  
    int a = 5, b = 10;  
  
    // no capture  
    auto swapIntValues = [](int&first, int&second) {  
        first ^= second;  
        second ^= first;  
        first ^= second;  
    };  
  
    swapIntValues(a, b);  
  
    std::cout << a << ' ' << b << std::endl;  
  
    //--------------------------------------------  
  
    int first = 100, second = 50, outsideLambdaScopeVar = 10;  
  
    // captures all variables by reference  
    auto sumTwoInts = [&](int& a, int& b) {  
        return a + b + outsideLambdaScopeVar;  
    };  
    std::cout << sumTwoInts(first, second) << std::endl;  
  
    //--------------------------------------------  
  
    int f = 1, s = 2, o = 3;  

	// captures all variables by value
    auto subtractTwoVariables = [=](int& a, int& b) {  
        return a - b + o;  
    };  
    std::cout << subtractTwoVariables(f, s) << std::endl;  
  
    //--------------------------------------------  
  
    char letterA = 'a', letterZ = 'z', letterM = 'm';  
  
    // captures only the var letterZ by reference  
    auto getGreaterOfTwoChars = [&letterZ](char& firstChar, char& secondChar) {  
        return (firstChar == secondChar ? letterZ : 
	        (firstChar > secondChar ? firstChar : secondChar));  
    };  
    
    std::cout << getGreaterOfTwoChars(letterA, letterM) << std::endl;  
  
    return 0;  
}
```

## Removing elements from a vector while iterating

```cpp
void removeNegativeNumbers(std::vector<int>& numbers) {  
    auto i = numbers.begin();  
  
    while (i != numbers.end()) {  
        if (*i < 0)  
            numbers.erase(i);  
        else  
            ++i;  
    }
}
```

# Finding min and max element

```cpp
#include <iostream>  
#include <vector>  
#include <algorithm>  
#include <functional>  
  
  
int main() {  
    std::vector numbers{1, 10, 5, 2, 56, 4};  
  
    auto iter =  std::ranges::min_element(numbers.begin(), numbers.end());  
    // auto iter =  std::ranges::max_element(numbers.begin(), numbers.end()); 
  
    std::cout << *iter;  
  
    return 0;  
}
```

# Sorting by Desc

```cpp
#include <iostream>  
#include <vector>  
#include <algorithm>  
#include <functional>  
  
int main() {  
    std::vector numbers{1, 10, 5, 2, 56, 4};  
  
    std::sort(numbers.begin(), numbers.end(), std::greater<int>());  

	for (const int& num : numbers)
        std::cout << num << std::endl;  

    return 0;  
}
```

### <span style="color:rgb(255, 0, 0)">STL = Standard Template Library</span>

# Index of Using Find()

```cpp
#include <iostream>  
#include <vector>  
#include <algorithm>  
#include <functional>  
  
int main() {  
    std::vector numbers{1, 10, 5, 2, 56, 4};  
  
    auto i = find(numbers.begin(), numbers.end(), 56);  
  
    std::cout << i - numbers.begin() << std::endl;  
  
    return 0;  
}
```

## Reversing a String

```cpp
#include <iostream>  
#include <vector>  
#include <sstream>  
#include <algorithm>  
  
using std::string;  
using std::vector;  
using std::cin;  
using std::cout;  
  
int main() {  
    while (true) {  
        std::string currentWord;  
        std::cin >> currentWord;  
  
        if (currentWord == "end")  
            break;  
  
        std::cout << currentWord << " = ";  
  
        std::reverse(currentWord.begin(), currentWord.end());  
  
        std::cout << currentWord << std::endl;  
    }  
    return 0;  
}
```

 char 1 byte
 bool 1 byte
 int 4 bytes
 double 8 bytes
 std::string 32 bytes

### Print multiple times the same symbol

```cpp
int main() {  
    std::cout << std::string(3, '-')<< std::endl;  
  
    return 0;  
}
```

## Split by a character a stream of Data

```cpp
#include <iostream>  
#include <vector>  
#include <sstream>  
#include <fstream>  
  
#define NAMES_FILE_PATH std::string("C:\\Programming\\C++\\C++ProjectCLion\\names.txt")  
  
  
int main() {  
    std::string currentNamesLineStr;  
    // stream for reading the file data  
    std::ifstream namesFileStream{NAMES_FILE_PATH};  
  
    int currentLineNumber = 0;  
  
    // reading line by line the file data  
    while (std::getline(namesFileStream, currentNamesLineStr)) {  
        std::vector<std::string> currentLineNamesVec;  
        std::string currentName;  
        // creating an input stream for the current line  
        std::stringstream currentNamesLineStream{currentNamesLineStr};  
  
        // specifying that the separator is a comma  
        while (std::getline(currentNamesLineStream, currentName, ','))  
            currentLineNamesVec.push_back(currentName);  
  
        // printing out the result  
        std::cout << "Line "<< ++currentLineNumber << ": ";  
        for (const std::string& name : currentLineNamesVec)  
            std::cout << name << ' ';  
        std::cout << '\n';  
    }  
    return 0;  
}
```

## Contains Substring

```cpp
bool containsSubstring(const std::string& word, const std::string& substring){  
    return word.find(substring) != std::string::npos;  
}  
  
int main() {  
    std::string word = "Peter";  
  
    std::cout << containsSubstring(word, "ta") << std::endl;  
    return 0;  
}
```

### Set precision to a decimal number in a stream

```cpp
#include <iostream>
#include <vector>
#include <sstream>
#include <iomanip>

std::string calculateAnimalsResult(
	const std::vector<std::string>& animalVectorData, 
	const std::string& animalType
) {
    std::stringstream result{};

    if (animalVectorData.size() == 0) {
        result << "no " << animalType << '!' << std::endl;
        return result.str();
    }

    result << std::fixed << std::setprecision(2);
    
    result << "with avg. size " 
	    << (sum > 0 ? sum / (animalVectorData.size() + 0.0) : 0) 
	    << std::endl;

    return result.str();
}
```

# Using Format for printing

```cpp
#include <vector>  
#include <iostream>  
#include <format>  
  
using std::string;  
using std::vector;  
using std::cin;  
using std::cout;  
  
int main() {  
    double grade = 5.1515;  
    int age = 21;  
    std::string name = "Peter";  
  
    std::cout  
        << format(
	        "Hello, my name is {}, {} years old and my average grade is:
	        {:.2f}."
		, name, age, grade)  
        << std::endl;  
  
    return 0;  
}
```

# Custom Join Method

```cpp
#include <iostream>  
#include <vector>  
#include <sstream>  
   
template<typename T>  
std::string join(const std::vector<T> &elements, const std::string &delimiter) {  
    std::stringstream joinedStringStream{};  
    for (int i = 0; i < elements.size(); ++i) {  
        joinedStringStream << elements.at(i);  
        if (i < elements.size() - 1)  
            joinedStringStream << delimiter;  
    }  
    return joinedStringStream.str();  
}  
  
int main() {  
    std::vector<int> numbers{1, 2, 3, 4, 5};  
  
    std::vector<std::string> words{  
        std::string{"Hello"},  
        std::string{"there"},  
        std::string{"general"},  
        std::string{"Kenobi"}  
    };  
  
    std::vector<double> grades{5.50, 6.00, 4.40, 2.70, 5.20};  
  
    std::cout << join(numbers, " - ") << std::endl;  
    std::cout << join(words, "| - |") << std::endl;  
    std::cout << join(grades, "/ - /") << std::endl;  
    return 0;  
}
```

## Std::endl

When output is written to a stream it is often first stored in an internal buffer for performance reasons.<span style="color:rgb(255, 0, 0)">Writing data to the console or a file is relatively slow, so C++ minimizes the number of write operations by buffering the data and writing it in larger chunks.</span> However, sometimes you might want to ensure that the output appears immediately or interacting with real-time input/output.

**<span style="color:rgb(255, 0, 0)">By using std::endl you force the buffer to flush immediately, ensuring the output is written to its destination without delay.</span>**

____
# Flattened Matrix

```cpp
#include <iostream>  
  
#define ROWS 2  
#define COLUMNS 4  
  
int main() {  
    const int length = ROWS * COLUMNS;  
  
    std::string matrixTwoDimensional[length] = {  
    "Peter", "Ivan", "Stiliyan", "Pavel",  
    "Kristian", "Kaloyan", "Bogdan", "Gabi bate"  
    };  
  
    const int selectedRow = 1, selectedColumn = 2;  
    std::cout << 
	    "matrixTwoDimensional[1][2]: " 
	    << matrixTwoDimensional[selectedRow 
		    * (length / ROWS) + selectedColumn] << '\n';  
  
    for (int i = 0; i < ROWS; ++i) {  
        for (int j = 0; j < COLUMNS; ++j)  
            std::cout << 
	            matrixTwoDimensional[i * (length / ROWS) + j] << ' ';  
        std::cout << '\n';  
    }  
    
    return 0;  
}
```


```cpp
#include <cstdint>  
#include <iostream>  
  
int main() {  
    int8_t integer8 = INT8_MAX;  
    int16_t integer16 = INT16_MAX;  
    int32_t integer32 = INT32_MAX;  
    int64_t integer64 = INT64_MAX;  
  
    std::cout << (int) integer8 << std::endl;  
    std::cout << (int) integer16 << std::endl;  
    std::cout << integer32 << std::endl;  
    std::cout << integer64 << std::endl;  
  
    return 0;  
}


#include <iostream>  
#include <vector>  
#include <sstream>  
  
  
int main() {  
    const char* str = "Peter";  
  
    std::string url = R"(https://localhost::8080/home)";  
  
    std::cout << url;  
  
    return 0;  
}
```


```cpp
#include <iostream>  
#include <vector>  
#include <sstream>  
  
  
int main() {  
    // static cast  
    // int c = 'A';    
    // std::cout << c << std::endl;    
    // std::cout << static_cast<char>(c) << std::endl;  
    // const cast    
    // dynamic cast  
    std::vector<int> numbers {1, 2, 3, 4, 5, 6};  
  
    // for (auto iter = std::begin(numbers); iter != std::end(numbers); ++iter)  
    //     std::cout << *iter << ' ';    
    // std::cout << '\n';    
    //    
    // for (auto iter = numbers.cbegin(); iter != numbers.cend(); ++iter)    
    //     std::cout << *iter << ' ';    
    // std::cout << '\n';    //    // for (auto iter = numbers.rbegin(); iter != numbers.rend(); ++iter)    //     std::cout << *iter << ' ';    // std::cout << '\n';  
/***********************************************/  
    // switch (const int num = numbers.at(0); num) {    //     case 1:    //         std::cout << "one";    //         break;    //     case 2:    //         std::cout << "two";    //         break;    //     default:    //         std::cout << "other";    // }  
/***********************************************/  
    // if (auto iter = std::begin(numbers); iter != std::end(numbers)) {    // }/***********************************************/  
  
    // const char* name = "Peter";    // int count  = 0;    // for (int i = 0; i < 5; ++i) {    //     switch (const char c = name[i]; c) {    //         case ' ':    //             [[fallthrough]];    //         case '\t':    //             [[fallthrough]];    //         case '\n':    //             ++count;    //             break;    //         default:    //             break;    //     }    // }/***********************************************/  
  
    //  std::string name = "Peter";    //    // std::string firstTwo = {name, 0, 2};    //    // std::cout << firstTwo;/***********************************************/  
  
    std::string name = "peter", vowels = "aeiou";  
  
    // std::cout << name.find_first_of(vowels) << std::endl;  
    //    // std::cout << name.find_last_of(vowels) << std::endl;    //    // name.insert(0 , "A-");    // name.insert(name.length() , "-A");  
    // name.erase(2, 1);    // const std::string replace = "e";    //    // if (const size_t index = name.find_last_of(replace); index != std::string::npos)    //     name.replace(index, 1, "a");    // else    //     std::cout << "The character '" << replace << "' was not found, therefore not being replaced" << std::endl;    //    // std::cout << name << std::endl;/***********************************************/  
      
    return 0;  
}
```


```cpp
#include <iostream>  
#include <vector>  
#include <sstream>  
#include <fstream>  
#include <iomanip>  
  
int main() {  
    // file modes  
    // std::ofstream file{"C:\\Programming\\C++\\C++ProjectCLion\\file.txt", std::fstream::app};    // // appending at the end of the file    //    // if (file.is_open())    //     file << "\nAppending data...";  
  
    // std::ifstream fileData{"C:\\Programming\\C++\\C++ProjectCLion\\file.txt"};    // std::string line;    //    // while (std::getline(fileData, line)) {    //     std::stringstream lineStream{line};    //     std::string currentWord;    //     while (std::getline(lineStream, currentWord, ','))    //         std::cout << currentWord << ' ';    //     std::cout << '\n';    // }  
    /**********************************************/    // int x = 2;    // bool isEven = x % 2 == 0;  
    // print bool as true and false    // std::cout << std::boolalpha << "Is Even: "<< isEven << std::endl;    // std::cout << std::boolalpha << "Is Not even: "<< !isEven << std::endl;    /**********************************************/  
    // std::cout << std::left << std::setw(7) << "Name " << "Grade" << '\n';    // std::cout << std::left << std::setw(7) << "Ivan " << 5.98 << '\n';    // std::cout << std::left << std::setw(7) << "Peter " << 6.00 << '\n';  
  
    /**********************************************/    // std::ostringstream outputStream{};    // outputStream << "It is time to say ";    //    // const auto marker = outputStream.tellp();    // std::cout << "Stream marker is " << marker << " bytes into the stream\n";    //    // outputStream << "hello there, general kenobi!";    //    // outputStream.seekp(marker);    //    // std::cout << outputStream.str() << std::endl;    //    // outputStream << "goodnight";    //    // std::cout << outputStream.str() << std::endl;    /**********************************************/  
    return 0;  
}
```

```cpp
#include <iostream>  
#include <vector>  
#include <sstream>  
#include <fstream>  
#include <iterator>  
  
#define FILE_PATH "C:\\Programming\\C++\\C++ProjectCLion\\file.txt"  
  
int main() {  
    /*************************/  
    // Read  
    // std::ifstream fileStream{FILE_PATH};    // std::string currentLine;    //    // while (std::getline(fileStream, currentLine))    //     std::cout << currentLine << '\n';  
    /*************************/    // Append  
    // std::ofstream fileAppendStream{FILE_PATH, std::fstream::app};    //    // fileAppendStream << "\nAdding new line...";  
    /*************************/    // Write  
    // std::ofstream fileWriteStream{FILE_PATH};    //    // fileWriteStream << "Hello there\nGeneral Kenobi!\nYou are a bold one!";  
  
    /*************************/    // Replace  
    // std::stringstream resultStream{};    // std::ifstream fileStream{FILE_PATH};    // std::string currentLine;    //    // while (std::getline(fileStream ,currentLine)) {    //     if (auto kenobiPos = currentLine.find("Kenobi"); kenobiPos != std::string::npos)    //         resultStream << currentLine.replace(kenobiPos, 6, "Grievous") << '\n';    //    //     else if (auto therePos = currentLine.find("there");    //         therePos != std::string::npos && currentLine.find("there!") == std::string::npos    //     )    //         resultStream << currentLine.replace(therePos, 5, "there!\n");    //    //     else    //         resultStream << currentLine << '\n';    // }    //    // std::ofstream fileWriteStream{FILE_PATH};    // fileWriteStream << resultStream.str();  
  
    /*************************/    // Clear file  
    // std::ofstream delFile{FILE_PATH};    //    // delFile << "";  
    /*************************/    // ostream iterator  
    // std::ostream_iterator<int> oi(std::cout, "\n");    //    // for (int i = 0; i < 10; ++i) {    //     *oi = i;    //     ++oi;    // }  
    /*************************/    // istream iterator  
    // std::istream_iterator<int> ii(std::cin);    //    // int x = *ii;    //    // std::cout << "You entered: " << x << std::endl;  
    /*************************/      
    return 0;  
}
```


```cpp
#include <iostream>  
#include <sstream>  
  
int main() {  
    #if 0  
    std::string input;  
    std::cin >> input;    
    // Try to parse a string to an int  
  
    try {  
        const int number = std::stoi(input);        
		    * std::cout << "Number: " << number << '\n';  
    } catch (std::invalid_argument err) {        
	    std::cout << "String: " << input << '\n';    
	    }    
	#endif  
  
    // Read a line (either a city name either latitude and longitude, separated by a space)  
    std::string inputLine;  
    std::getline(std::cin, inputLine);  
  
    std::stringstream currentLineStream{inputLine};  
    double latitude, longitude;  
    std::string extra;  
  
    if (currentLineStream >> latitude >> longitude && 
		    !(currentLineStream >> extra))  
        std::cout << "Latitude: " << latitude << " Longitude: " << longitude << '\n';  
    else  
        std::cout << "City Name: " << inputLine << '\n';  
  
    return 0;  
}
```

## If-init statement

```cpp
#include <iostream>  
#include <optional>  
  
std::optional<int> findValue(const bool& found) {  
    if (found)  
        return 42;  
    return std::nullopt;  
}  
  
int main() {  
    // init variable; condition  
    // the scope is limited to the if statement and its associated else block.    
    if (const auto value = findValue(true); value.has_value()) {  
        std::cout << "Found value: " << *value << '\n';  
    } else {  
        std::cout << "Value not found.\n";  
    }  
    
    // init variable; condition  
    // the scope is limited to the if statement and its associated else block.    
    if (const auto value = findValue(false); value.has_value()) {  
        std::cout << "Found value: " << *value << '\n';  
    } else {  
        std::cout << "Value not found.\n";  
    }  
    
    return 0;  
}
```

### Access Iterator values

```cpp
// For access through an iterator or pointer, we use the operator ->  
for (auto iter = people.begin(); iter != people.end(); ++iter)  
    std::cout << iter->getName() << ' ' << iter->getAge() << '\n';  
    // std::cout << *iter << '\n';
```

# Multiset

can store multiple elements with same value

```cpp
#include <iostream>  
#include <set>   
 
int main() {  
    std::multiset<int> numbers{};  
  
    numbers.insert(3);  
    numbers.insert(5);  
    numbers.insert(2);  
    numbers.insert(3);  
  
    for (const int& number : numbers)  
         std::cout << number << ' ';  
    auto threeIter = numbers.count(3);  

	// Number of times a variable is encountered
    std::cout << "3 is encountered " << threeIter << " times.\n";  
  
    return 0;  
}
```

# Multimap

```cpp
std::multimap<std::string, int> scores;  
  
scores.insert({"Graham", 50});  
scores.insert({"Graham", 48});  
scores.insert({"Grace", 70});  
scores.insert({"Graham", 100});  
  
// scores.erase("Graham"); // Delete ALL occurrences of Graham  
  
std::cout << "Graham count: " << scores.count("Graham") << '\n';

// upper_bound -> for searching
// lower_bound -> for searching
  
for (std::pair<const std::string, int>& score : scores)  
    std::cout << score.first << ' ' << score.second << '\n';
```
#### equal_range method (getting all entries with the given key)

```cpp
int main() {  
    std::multimap<std::string, int> multiMap {  
        {"apple", 10},  
        {"banana", 20},  
        {"apple", 30},  
        {"banana", 40},  
        {"cherry", 50},  
    };  
  
    auto range = multiMap.equal_range("banana");  
  
    for (auto iter = range.first; iter != range.second; ++iter)  
        std::cout << iter->first << ": " << iter->second << '\n';  
  
    return 0;  
}
```

# Priority Queue

```cpp
int main() {  
    // Orders the element in desc order using < operator  
    std::priority_queue<int> numsQueue{};  
  
    numsQueue.push(1);  
    numsQueue.push(10);  
    numsQueue.push(100);  
    numsQueue.push(1'000);  
  
    while (!numsQueue.empty()) {  
        std::cout << numsQueue.top() << '\n';  
        numsQueue.pop();  
    }  
    return 0;  
}
```

# Exception Handling

```cpp
int main() {  
    try {  
        std::vector<int> nums;  
        nums.at(2) = 10;  
        std::string numStr = "one";  
        int number = std::stoi(numStr);  
   
    } catch (std::invalid_argument) { // Catch specific err
	    std::cout << "Cannot parse " << numStr << " to an int.\n";  
	
	} catch(const std::exception& ex) { // Without & -> std::exception  
        std::cout << ex.what() << '\n';  
    
    } catch(...) { // Catch all types of errs
        std::cout << "An error occurred.\n";  
    }  
    
    return 0;  
}
``` 

## Chrono Library

```cpp
#include <chrono>
using namespace std::chrono_literals; // has to be imported in order to use h, min, s...   
  
int main() {  
   std::chrono::seconds s;  
    // std::cout << s.count();  
  
	std::chrono::hours hours = 5h;  
	std::chrono::minutes minutes = 5min;  
    std::chrono::seconds seconds = 2s;  

	// Calculate Interval
    auto start = std::chrono::steady_clock::now();  
	
	// Sleep
    std::this_thread::sleep_for(2s);  
  
    auto finish = std::chrono::steady_clock::now();  

	// Time taken
    auto elapsed = std::chrono::duration_cast<std::chrono::milliseconds>(finish - start);  
    std::cout << "Time taken: " << elapsed.count() << " miliseconds.\n";  
  
    return 0;  
}
```

## Tuples

##### Pretty much only useful for returning multiple values from a method or a function, without creating a class or a struct, used only for that case.

 ```cpp
std::tuple<std::string, int, double> getPersonData() {  
    return {"Peter", 21, 5.20};  
}

int main() {  
	// Init
    std::tuple<double, int, std::string> numbers {1.0, 2, "Three"};  

	// Get element
    double x = std::get<0>(numbers);  

	// CAN CHANGE THE ELEMENTS, not like in PYTHON
    std::get<1>(numbers) = 10;  
  
    double num1 = 5.5;  
    int num2 = 100;  
    std::string num3 = "Five";  

	// Assigning new variables to the tuple 
    std::tie(num1, num2, num3) = numbers;  
  
    std::cout << "New values of the tuple are " 
	    << num1 << ", " << num2 << ", " << num3 << '.' << '\n';  
  
    return 0;  
}
```

## Multiply and divide a number by two

```cpp
int main() {  
    std::cout << (10 << 1) << std::endl; \
    // shift all bits 1 position to the left: 10 * 2  
  
    std::cout << (10 >> 1) << std::endl; 
    // shift all bits 1 position to the right: 10 / 2  
  
    return 0;  
}
```

## Optimised if statement

```cpp
int main() {  
    int a = 10;  
    int b = 20;  
  
    int condition = 1; // try with 0 too  
    int x = condition * a + (1 - condition) * b;  
  
    // It's only safe and correct if condition is strictly 0 or 1.  
    std::cout << "x = " << x << std::endl;  
    return 0;  
}
```


```cpp
int main() {  
    int nums[]{1, 2, 3, 4};  
    int odd[6], even[6];  
    int oddCount = 0, evenCount = 0;  
  
    for (int i = 0; i < 4; ++i) {  
        int isOdd = nums[i] % 2;  
        odd[oddCount] = nums[i] * isOdd;  
        even[evenCount] = nums[i] * (1 - isOdd);  
        oddCount += isOdd;  
        evenCount += 1 - isOdd;  
    }    
    
    std::cout << evenCount;  
  
    return 0;  
}
```

# Memcpy
**`memcpy()` is ~50× faster** than a manual loop for large arrays.
- 🐢 **Manual loop copy:** ~**2.94 seconds**
- ⚡ **`memcpy`-style (`std::memcpy` or `np.copy`) copy:** ~**0.06 seconds**

```cpp
int main() {  
    size_t arraySize = 5;  
    int* array = new int[arraySize]{};  
  
    size_t arrayShrunkSize = arraySize - 1;  
    int* arrayShrunk = new int[arrayShrunkSize];  
  
    memcpy(arrayShrunk, array, sizeof(int) * arrayShrunkSize);  
  
    for (int i = 0; i < arrayShrunkSize; ++i)  
        std::cout << *(arrayShrunk + i) << ' ';  
  
    delete[] array;  
    delete[] arrayShrunk;  
    return 0;  
}
```