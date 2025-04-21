## Reading from a file

```cpp
#include <iostream>  
#include <sstream>  
#include <fstream>  
  
int main() {  
    std::ifstream iffile("C:\\Programming\\C++\\C++ProjectCLion\\input.txt");  
  
    if (iffile) {  
        std::string text;  
        
        while (iffile >> text) {  
            std::cout << text << std::endl;  
        } // -> word for word 
        
        while (std::getline(iffile, text)) {  
		    std::cout << text << std::endl;  
		} // -> line by line
        
        std::cout << std::endl;  
        iffile.close();  
    
    } else {  
        std::cout << "File not found.";  
    }  
    
    return 0;  
}
```

## Writing to a file

```cpp
#include <iostream>  
#include <sstream>  
#include <fstream>  
#include <vector>  
  
int main() {  
    std::ofstream offile("C:\\Programming\\C++\\C++ProjectCLion\\output.txt");  
  
    if (offile) {  
        std::pmr::vector<std::string> words = 
        {"Ivan", "Peter", "Stilian", "Kristian", "Gabi Bate", "Lubo", "Pavel"};  
        
        for (std::string word: words) {  
            offile << word << '\n';  
        }    
	}  
	
    return 0;  
}
```

## String Stream

```cpp
#include <iostream>  
#include <sstream>  
  
int main() {  
    int sizeArray;  
    std::cin >> sizeArray;  
    std::cin.ignore();  
  
    int array[sizeArray], evenSum = 0;  
  
    std::string data;  
    std::getline(std::cin, data);  
  
    std::istringstream iss(data);  
  
    for (int i = 0; i < sizeArray; i++) {  
        iss >> array[i];  
    
		if (array[i] % 2 == 0) {  
            evenSum += array[i];  
        }    
	}  
    std::cout << evenSum;  
}
```

## Filling an array of numbers from a String Stream

```cpp
#include <iostream>  
#include <sstream>  
  
int main() {  
    int arrSize;  
    std::cin >> arrSize;  
  
    int arr[arrSize];  
  
    std::cin.ignore();  
    std::string data;  
    std::getline(std::cin, data);  
  
    std::stringstream strStream(data);  
  
    int currentNumber, index = 0;  
    
    while (strStream >> currentNumber) {  
        arr[index++] = currentNumber;  
    }  
    
    return 0;  
}
```

# Binary file

```cpp
#include <cstdint>  
#include <iostream>  
#include <vector>  
#include <sstream>  
#include <fstream>  
  
  
struct Point {  
    char c;  
    int32_t x;  
    int32_t y;  
};  
  
  
int main() {  
    Point p{'a', 1, 2};  
  
    std::ofstream ofFile("file.bin", std::fstream::binary);  
  
    if (ofFile.is_open()) {  
        ofFile.write(reinterpret_cast<char * >(&p), sizeof(Point));  
        ofFile.close();  
    }  
    std::ifstream iffile("file.bin", std::fstream::binary);  
    Point p2;  
  
    if (iffile.is_open()) {  
        iffile.read(reinterpret_cast<char*>(&p2), sizeof(Point));  
        iffile.close();  
    }  
    return 0;  
}
```