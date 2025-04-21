## C initialization of a String

```cpp
int main() {
	char name[10]{'P', 'e', 't', 'e', 'r', '\0'};

	char lastName[] = "Gerdzhikov"; // Automatically adds \0  
	std::cout << sizeof(lastName); // 10 chars + \0 = 11

	// Remaining elements will be initialized to '\0' (null character)  
	char characters[5]{'A', 'B'};
}
```

## Reading C String from the console
### (Without overflowing the variable)

```cpp
int main() {  
    char name[10];  
    
    std::cin.getline(name, 10);  
    // gets(name); // Overflows the size, C method
    
    std::cout << name << '\n';  
  
    return 0;  
}
```

## String Length

```cpp
int main() {  
    char name[] = {'P', 'e', 't', 'e', 'r', '\0'};  
  
    // Find the size  
    size_t nameLength{};  
    char* currentChar = name;  
  
    // Count the chars until \0 is found  
  
    whileLoopStart:  
        if (*currentChar != '\0') {  
            ++nameLength;  
            ++currentChar;  
            goto whileLoopStart;  
        }  
        
    std::cout << nameLength << '\n';  
  
    return 0;  
}
```

## Swap Casing

```cpp
char swapCasing(const char& ch) {  
    if (ch >= 'a' && ch <= 'z')  
        return static_cast<char>(ch - 32);  
  
    if (ch >= 'A' && ch <= 'Z')  
        return static_cast<char>(ch + 32);  
  
    return ch;  
}
  
int main() {  
    constexpr char name[6] {'p', 'E', 'T', 'E', 'R', '\0'};  
  
    for (const char ch : name)  
        std::cout << swapCasing(ch);  
  
    return 0;  
}
```

## Count words

```cpp
int main() {  
    char name[]{' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', '\0'};  
  
    int wordsCount{};  
  
    bool previousIsSpace = true;  
    bool foundOtherChar = false;  
    
    for (const char& ch : name) {  
        if (ch == ' ' && !previousIsSpace) {  
            ++wordsCount;  
            previousIsSpace = true;  
        }        
        
         if (ch != ' ' && ch != '\0') {  
            previousIsSpace = false;  
            foundOtherChar = true;  
        }    
	}    
	
	wordsCount += foundOtherChar;  
  
    std::cout << wordsCount;  
  
    return 0;  
}
```

## Reverse String
#### [[Recursion]]

```cpp
char* reverseString(const char* str) {  
    if (str == nullptr) 
		return nullptr;  
  
    int strLength{};  
  
    for (const char* strP = str; *strP != '\0'; ++strP)  
        ++strLength;  
  
    char* newStr = new char[strLength + 1];  
     for (int i = strLength; i > 0; --i)  
        newStr[strLength - i] = str[i -1];  
  
    newStr[strLength] = '\0';  
  
    return newStr;  
}
```

## Count Number of duplicates

```cpp
int numberOfDuplicates(char str[]) {  
    int* occurrences = new int[26]{};  
  
    for (char* strP = str; *strP != '\0'; ++strP) {  
        if (*strP >= 'a' && *strP <= 'z')  
            ++occurrences[*strP - 'a'];  
  
        else if (*strP >= 'A' && *strP <= 'Z')  
            ++occurrences[*strP - 'A'];  
    }  
    int totalOccurrences{};  
    for (int i = 0; i < 26; ++i)  
        if (*(occurrences + i) > 1)
	        ++totalOccurrences;  
            //totalOccurrences += *(occurrences + i) - 1;  
  
    delete[] occurrences;  
    return totalOccurrences;  
}
```