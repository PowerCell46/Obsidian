## Partially initializing an array's values

```cpp
int main() {
	int* dynamicArr = new int[10]{10, 20}; 
	// Partial brace-initialization in C++ zeros out the rest of the array.  
	  
	for (int i = 0; i < 10; ++i)  
	    std::cout << *(dynamicArr + i) << ' ';  
	  
	delete[] dynamicArr;
}
```

## Initializing a single array value

```cpp
int main() {
	// This does not mean “fill the entire array with 10.” 
	int* arr = new int[10]{10};  
	// When you use brace initialization with fewer values than the size of the array, the remaining elements are value-initialized
}
```

## Resize an array 

#### Resize to a Bigger array

```cpp
int main() {  
    int* initialDynamicArr = new int[5];  
    for (int i = 0; i < 5; ++i)  
	    *(initialDynamicArr + i) = i + 1;
  
    // Create a bigger array  
    int* newDynamicArr = new int[8]{};  
  
    // Move the elements in the new array  
    for (int i = 0; i < 5; ++i)  
        *(newDynamicArr + i) = *(initialDynamicArr + i);  
  
    // Delete the initial array  
    delete[] initialDynamicArr;  
    // Assign the pointer of the initial array to the new array  
    initialDynamicArr = newDynamicArr;  
    // Make the pointer to the new array unusable  
    newDynamicArr = nullptr;  
  
    for (int i = 0; i < 8; ++i)  
        std::cout << *(initialDynamicArr + i) << ' ';  
  
    delete[] initialDynamicArr;  
  
    return 0;  
}
```

#### Resize to a Smaller array

```cpp
int main() {  
    int* initialDynamicArr = new int[5];  
    for (int i = 0; i < 5; ++i)  
        *(initialDynamicArr + i) = i + 1;  
  
    // Create an array with a smaller size  
    int* newDynamicArr = new int[3];  
  
    // Transfer the elements to the new aray  
    for (int i = 0; i < 3; ++i)  
        *(newDynamicArr + i) = *(initialDynamicArr + i);  
  
    // Delete the initial elements  
    delete[] initialDynamicArr;  
    // Assign the pointer to the aray to the new one  
    initialDynamicArr = newDynamicArr;  
    // Make the new array to be invalid  
    newDynamicArr = nullptr;  
  
    for (int i = 0; i < 3; ++i)  
        std::cout << *(initialDynamicArr + i) << ' ';  
  
    delete[] initialDynamicArr;  
  
    return 0;  
}
```

## Flattened matrix

```cpp
int main() {  
    constexpr int rowSize = 4;  
    constexpr int columnSize = 5;  
  
    int* matrix = new int[rowSize * columnSize];  
  
    for (int i = 0; i < rowSize; ++i)  
        for (int j = 0; j < columnSize; ++j)  
            matrix[i * columnSize + j] = i * columnSize + j + 1;  
  
  
    for (int i = 0; i < rowSize * columnSize; ++i)  
        std::cout << matrix[i] << ((i + 1) % columnSize == 0 ? '\n' : ' ');  
  
  
    delete[] matrix;  
    
    return 0;  
}
```

## <span style="color:rgb(255, 0, 0)">Why Indices start from 0 and not 1?</span>
#### Formula for 0 based: baseAddress + index * sizeof
#### Formula for 1 based: baseAddress + (index - 1) * sizeof
#### <span style="color:rgb(255, 0, 0)">=> 1 based will take 1 more operation for EVERY access of data</span>
#### => Slower, no point of this

___
## Row-major and Column-major

### Row-major

|  1|  2|  3|  4|  5|
|  6|  7|  8|  9| 10|
| 11| 12| 13| 14| 15|
| 16| 17| 18| 19| 20|

Flattened:
| 1 2 3 4 5 | 6 7 8 9 10 | 11 12 13 14 15 | 16 17 18 19 20 |

Formula:
index = row * columnSize + column;

### Column-major

|  1|  5|  9| 13| 17|
|  2|  6| 10| 14| 18|
|  3|  7| 11| 15| 19|
|  4|  8| 12| 16| 20|

Flattened: 
| 1 5 9 13 17 | 2 6 10 14 18 | 3 7 11 15 19 | 4 8 12 16 20 |

Formula:
index = column * rowSize + row;


```cpp
int main() {  
    constexpr int rowSize = 3;  
    constexpr int columnSize = 5;  
  
    int* matrix = new int[rowSize * columnSize];  
  
// Row-major  
#if 0  
    for (int i = 0; i < rowSize; ++i)  
        for (int j = 0; j < columnSize; ++j)            
	        matrix[i * columnSize + j] = i * columnSize + j + 1;  
    
    // [1, 2, 3, 4, 5]  
    // [6, 7, 8, 9, 10]    
    // [11, 12, 13, 14, 15]  
    
    for (int i = 0; i < rowSize * columnSize; ++i)  
        std::cout << matrix[i] << ((i + 1) % columnSize == 0 ? '\n' : ' ');
#endif  

  
// Column-major  
#if 0  
    for (int i = 0; i < rowSize; ++i)  
        for (int j = 0; j < columnSize; ++j)            
	        matrix[i * columnSize + j] = i + 1 + (rowSize * j);  
    
    // [1, 4, 7, 10, 13]  
    // [2, 5, 8, 11, 14]    
    // [3, 6, 9, 12, 15]    
    
    for (int i = 0; i < rowSize * columnSize; ++i)  
        std::cout << matrix[i] << ((i + 1) % columnSize == 0 ? '\n' : ' ');
#endif  
    
    delete[] matrix;  
    
    return 0;  
}
```

___
# Array ADT (Abstract Data Type)

## Insert an element

```cpp
// Passing the pointer itself by reference
// If you only had `void insertElement(int* arr, ...)` (without the `&`), 
// you’d be changing a local copy of `arr` — the real pointer 
// in the caller would never see that change.
void insertElement(int*& arr, int& arrSize, const int& insertIndex, const int& insertElement) {  
    if (insertIndex > arrSize) {  
        std::cout << "Invalid insert index.\n";  
        arrSize = 0;  
        return;  
    }  
    int* newArr = new int[arrSize + 1];  
  
    // Move the previous elements  
    for (int i = 0; i < insertIndex; ++i)  
        *(newArr + i) = *(arr + i);  
    // Insert the new element  
    newArr[insertIndex] = insertElement;  
  
    // Move the rest of the elements after the new one  
    for (int i = insertIndex; i < arrSize; ++i)  
        *(newArr + i + 1) = *(arr + i);  
  
    delete[] arr;  
    arr = newArr;  
    newArr = nullptr;  
    ++arrSize;  
}  
  
int main() {  
    int arrSize;  
    std::cout << "Enter arr size: ";  
    std::cin >> arrSize;  
  
    int* arr = new int[arrSize];  
  
    for (int i = 0; i < arrSize; ++i) {  
        std::cout << "Enter element " << i + 1 << ':';  
        std::cin >> *(arr + i);  
    }  
    
    int insertIndex, insertElement;  
    std::cout << "Enter insert index: ";  
    std::cin >> insertIndex;  
  
    std::cout << "Enter insert element: ";  
    std::cin >> insertElement;  
  
    ::insertElement(arr, arrSize, insertIndex, insertElement);  
  
    if (arrSize) {  
        std::cout << "Printing the elements after the insertion\n";  
        for (int i = 0; i < arrSize; ++i)  
            std::cout << *(arr + i) << ' ';  
    }  
    
    delete[] arr;  
    return 0;  
}
```

## Delete an Element

```cpp
void deleteElement(int*& arr, int& arrSize, const int& deleteIndex) {  
    if (deleteIndex >= arrSize) {  
        std::cout << "Invalid delete index.\n";  
        arrSize = 0;  
        return;  
    }    
    
    int* newArr = new int[arrSize - 1];  
  
    for (int i = 0; i < deleteIndex; ++i)  
        *(newArr + i) = *(arr + i);  
  
    for (int i = deleteIndex + 1; i < arrSize; ++i)  
        *(newArr + i - 1) = *(arr + i);  
  
    delete[] arr;  
    arr = newArr;  
    newArr = nullptr;  
    --arrSize;  
}  
  
int main() {  
    int arrSize;  
    std::cout << "Enter array size: ";  
    std::cin >> arrSize;  
  
    int *arr = new int[arrSize];  
  
    for (int i = 0; i < arrSize; ++i) {  
        std::cout << "Enter element " << i + 1 << ':';  
        std::cin >> *(arr + i);  
    }  
    
    int deleteIndex;  
    std::cout << "Enter delete index: ";  
    std::cin >> deleteIndex;  
  
    deleteElement(arr, arrSize, deleteIndex);  
  
    if (arrSize) {  
        std::cout << "Elements after deletion.\n";  
        for (int i = 0; i < arrSize; ++i)  
            std::cout << *(arr + i) << ' ';  
    }  
    
    delete[] arr;  
    return 0;  
}
```

## Linear Search

**Transposition**: only one spot closer to the front.
**Move-to-front**: swap with the very first element at `arr[0]`.

```cpp
int linearSearch(const int* arr, const int& arrSize, const int& searchElement) {  
    for (int i = 0; i < arrSize; ++i)  
        if (*(arr + i) == searchElement)  
            return i;  
    return -1;  
}  
  
// Swapping the element with the previous one  
int linearSearchTransposition(int* arr, const int& arrSize, const int& searchElement) { 
    for (int i = 0; i < arrSize; ++i)  
        if (*(arr + i) == searchElement) {  
            if (i > 0) {  
                *(arr + i - 1) ^= *(arr + i);  
                *(arr + i) ^= *(arr + i - 1);  
                *(arr + i - 1) ^= *(arr + i);  
                return i - 1;  
            }            
            return i;  
        }    
        return -1;  
}  
  
// Swapping the element with the first one  
int linearSearchMoveFront(int* arr, const int& arrSize, const int& searchElement) {  
    for (int i = 0; i < arrSize; ++i)  
        if (*(arr + i) == searchElement) {  
            if (i > 0) {  
                *(arr) ^= *(arr + i);  
                *(arr + i) ^= *(arr);  
                *(arr) ^= *(arr + i);  
                return 0;  
            }            
            return i;  
        }    
        return -1;  
}

int main() {  
    int arrSize;  
    std::cout << "Enter array size: ";  
    std::cin >> arrSize;  
  
    int *arr = new int[arrSize];  
  
    for (int i = 0; i < arrSize; ++i) {  
        std::cout << "Enter element " << i + 1 << ':';  
        std::cin >> *(arr + i);  
    }  
    
    int searchElement;  
    std::cout << "Enter search element: ";  
    std::cin >> searchElement;  
  
    std::cout << 
	    "Element is at index " << 
	    linearSearch(arr, arrSize, searchElement) << ".\n";  
  
    delete[] arr;  
    return 0;  
}
```

## Binary search

```cpp
// Right implementation
int binarySearch(
	const int* array, const int arrSize, const int& searchedElement
) {  
    int begin = 0;  
    int end = arrSize - 1;  
  
    while (begin <= end) {  
        const int middle = (begin + end) / 2;  
  
        if (*(array + middle) == searchedElement)  
            return middle;  
        if (*(array + middle) < searchedElement)  
            begin = middle + 1;  
        else  
            end = middle - 1;  
    }  
    return -1;  
}

// Resursive approach
int binarySearch(
	const int* array, const int& searchedElement, const int& begin, const int& end
) {  
    if (!(begin <= end))  
        return -1;  
  
    const int middle = (begin + end) / 2;  
  
    if (*(array + middle) == searchedElement)  
        return middle;  
  
    if (*(array + middle) < searchedElement)  
        return binarySearch(array, searchedElement, middle + 1, end);  
  
    return binarySearch(array, searchedElement, begin, middle - 1);  
}
```

## Get, Set, Sum, Average, Min, Max

```cpp
int get(const int *array, const int &arrSize, const int &index) {  
    return index < 0 || index >= arrSize ? -1 : *(array + index);  
}  
  
int set(int *array, const int &arrSize, const int &setIndex, const int &setElement) {  
    if (setIndex < 0 || setIndex >= arrSize)  
        return -1;  
  
    *(array + setIndex) = setElement;  
    return 1;  
}  
  
  
int sum(const int *arr, const int &arrSize) {  
    int sum{};  
  
    for (int i = 0; i < arrSize; ++i)  
        sum += *(arr + i);  
  
    return sum;  
}  
  
double avg(const int *arr, const int &arrSize) {  
    return sum(arr, arrSize) / static_cast<double>(arrSize);  
}  
  
  
int min(const int *arr, const int &arrSize) {  
    int minValue = INT_MAX;  
  
    for (int i = 0; i < arrSize; ++i)  
        minValue = *(arr + i) < minValue ? *(arr + 1) : minValue;  
  
    return minValue != INT_MAX ? minValue : -1;  
}  
  
int max(const int *arr, const int &arrSize) {  
    int maxValue = INT_MIN;  
  
    for (int i = 0; i < arrSize; ++i)  
        maxValue = *(arr + i) > maxValue ? *(arr + i) : maxValue;  
  
    return maxValue != INT_MIN ? maxValue : -1;  
}  
  
  
int main() {  
    int *array = new int[10]{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};  
  
    // std::cout << set(array, 10, 30, 10) << "\n";  
  
    // std::cout << get(array, 10, 3);  
    // std::cout << sum(array, 10);  
    // std::cout << avg(array, 10);  
    // std::cout << min(array, 10);  
    // std::cout << max(array, 10);  
    
    delete[] array;  
}
```

## Reverse, Shift, Rotate

```cpp
void reverse(int* arr, const int& arrSize) {  
    for (int i = 0; i < arrSize / 2; ++i) {  
        *(arr + i) ^= *(arr + arrSize - 1 - i);  
        *(arr + arrSize - 1 - i) ^= *(arr + i);  
        *(arr + i) ^= *(arr + arrSize - 1 - i);  
    }
}  

// [1, 2, 3] => [2, 3]
void leftShift(int*& arr, int& arrSize) {  
    int* newArr = new int[arrSize - 1];  
  
    for (int i = 1; i < arrSize; ++i)  
        *(newArr + i - 1) = *(arr + i);  
  
    delete[] arr;  
    arr = newArr;  
    newArr = nullptr;  
    --arrSize;  
}  

// [1, 2, 3] => [2, 3, 1]
void leftRotate(int* arr, const int& arrSize) {  
    const int firstElement = *(arr);  
  
    for (int i = 0; i < arrSize - 1; ++i)  
        *(arr + i) = *(arr + i + 1);  
  
    *(arr + arrSize - 1) = firstElement;  
}  

// [1, 2, 3] => [1, 2]
void rightShift(int*& arr, int& arrSize) {  
    int* newArr = new int[arrSize - 1];  
  
    for (int i = 0; i < arrSize - 1; ++i)  
        *(newArr + i) = *(arr + i);  
  
    delete[] arr;  
    arr = newArr;  
    newArr = nullptr;  
    --arrSize;  
}  

// [1, 2, 3] => [3, 1, 2]
void rightRotate(int* arr, const int& arrSize) {  
    const int lastElement = *(arr + arrSize - 1);  
  
    for (int i = arrSize - 1; i >= 0; --i)  
        *(arr + i) = *(arr + i - 1);  
  
    *arr = lastElement;  
}  
```

#### InsertInSortedArr, isSorted, negativeLeftPositiveRight

```cpp
int findInsertIndex(int* array, const int& begin, const int& end, const int& insertElement) {
    if (begin >= end)
        return begin;

    const int middle = (begin + end) / 2;

    if (*(array + middle) < insertElement)
        return findInsertIndex(array, middle + 1, end, insertElement);

    return findInsertIndex(array, begin, middle, insertElement);
}

void insertInSortedArr(int *&arr, int &arrSize, const int &insertElement) {
    int *newArr = new int[arrSize + 1];
    const int insertIndex = findInsertIndex(arr, 0, arrSize, insertElement);

    for (int i = 0; i < insertIndex; ++i)
        *(newArr + i) = *(arr + i);

    *(newArr + insertIndex) = insertElement;

    for (int i = insertIndex; i < arrSize; ++i)
        *(newArr + i + 1) = *(arr + i);

    delete[] arr;
    arr = newArr;
    ++arrSize;
}
  
bool isSorted(const int *arr, const int &arrSize) {  
    int previous = *arr;  
  
    for (int i = 1; i < arrSize; ++i) {  
        if (*(arr + i) < previous)  
            return false;  
        previous = *(arr + i);  
    }  
    return true;  
}
  
void negativeLeftPositiveRight(int* arr, const int& arrSize) {  
    int begin = 0;  
    int end = arrSize - 1;  
  
    while (begin < end) {  
        if (*(arr + begin) < 0) {  
            ++begin;  
            continue;  
        }  
        
        if (*(arr +end) >= 0) {  
            --end;  
            continue;  
        }  
        
        std::swap(*(arr + begin), *(arr + end));
        
        // Dangerous if the two values are the same (hasve the same address)
         
        // *(arr + begin) ^= *(arr + end);  
        // *(arr + end) ^= *(arr + begin);        
        // *(arr + begin) ^= *(arr + end);    
	}  
}

int main() {  
    int arrSize = 10;  
    int* array = new int[arrSize]{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};  
  
    insertInSortedArr(array, arrSize, 0);  
  
    for (int i = 0; i < arrSize; ++i)  
        std::cout << *(array + i) << ' ';  
    
    return 0;  
}
```

## Append, Compare, Merge

```cpp
// Add the values from arr2 to arr1
void append(int *&arr1, int &arr1Size, const int *arr2, const int &arr2Size) {
    int *newArray = new int[arr1Size + arr2Size];

    memcpy(newArray, arr1, sizeof(int) * arr1Size);
    // for (int i = 0; i < arr1Size; ++i)
    //     *(newArray + i) = *(arr1 + i);

    memcpy(newArray + arr1Size, arr2, sizeof(int) * arr2Size);
    // for (int i = 0; i < arr2Size; ++i)
    //     *(newArray + arr1Size + i) = *(arr2 + i);

    delete[] arr1;
    arr1 = newArray;
    newArray = nullptr;
    arr1Size += arr2Size;
}  
  
bool compare(
	const int* arr1, const int& arr1Size, const int* arr2, const int& arr2Size
) {  
    if (arr1Size != arr2Size)  
        return false;  
        
    if (arr1 == arr2)  
        return true;  
  
    for (int i = 0; i < arr1Size; ++i)  
        if (*(arr1 + i) != *(arr2 + i))  
            return false;  
  
    return true;  
}  

// Arr1 and Arr2 are sorted, merge them in one array
void merge(int*& arr1, int& arr1Size, const int* arr2, const int& arr2Size) {  
    int* newArray = new int[arr1Size + arr2Size];  
    int newArrayIndex = 0, arr1Index = 0, arr2Index = 0;  
    int arr1Element = *(arr1 + arr1Index);  
    int arr2Element = *(arr2 + arr2Index);  
  
    while (newArrayIndex < arr1Size + arr2Size) {  
        if (arr1Element <= arr2Element) {  
            *(newArray + newArrayIndex) = arr1Element;  
            ++arr1Index;  
            if (arr1Index >= arr1Size)  
                arr1Element = INT_MAX;  
            else  
                arr1Element = *(arr1 + arr1Index);  
  
        } else {  
            *(newArray + newArrayIndex) = arr2Element;  
            ++arr2Index;  
            if (arr2Index >= arr2Size)  
                arr2Element = INT_MAX;  
            else  
                arr2Element = *(arr2 + arr2Index);  
        }        
        ++newArrayIndex;  
    }  
    
    delete[] arr1;  
    arr1 = newArray;  
    newArray = nullptr;  
    arr1Size += arr2Size;  
}
```

## Array Intersection, Difference, Union

```cpp
// arr1 = [1, 2, 3]; arr2 = [2, 3, 4]; result = [1, 2, 3, 4]  
void arrayUnion(int *&arr1, int &arr1Size, const int *arr2, const int &arr2Size) { 
    int *newArray = new int[arr1Size + arr2Size]{};  
    int newArrayIndex = 0;  
  
    for (int i = 0; i < arr1Size; ++i) {  
        const int currentElement = *(arr1 + i);  
  
        for (int j = 0; j < newArrayIndex; ++j) {  
            if (*(newArray + j) == currentElement)  
                goto firstMainLoop;  
        }  
        
        *(newArray + newArrayIndex) = currentElement;  
        ++newArrayIndex;  
    firstMainLoop:  
    }  
  
    for (int i = 0; i < arr2Size; ++i) {  
        const int currentElement = *(arr2 + i);  
  
        for (int j = 0; j < newArrayIndex; ++j) {  
            if (*(newArray + j) == currentElement)  
                goto secondMainLoop;  
        }  
        *(newArray + newArrayIndex) = currentElement;  
        ++newArrayIndex;  
    secondMainLoop:  
    }  
  
    if (newArrayIndex == arr1Size + arr2Size) {  
        delete[] arr1;  
        arr1 = newArray;  
        arr1Size = newArrayIndex;  
  
    } else {  
        --newArrayIndex;  
        int *finalNewArray = new int[newArrayIndex];  
        for (int i = 0; i < newArrayIndex; ++i)  
            *(finalNewArray + i) = *(newArray + i);  
  
        delete[] newArray;  
        newArray = nullptr;  
  
        delete[] arr1;  
        arr1 = finalNewArray;  
        arr1Size = arr1Size + arr2Size;  
    }
} 

// ------------------------
  
// arr1 = [1, 2, 3]; arr2 = [2, 3, 4]; res = [2, 3];  
void arrayIntersection(  
    int *&arr1, int &arr1Size, const int *arr2, const int &arr2Size  
) {  
    int *newArray = new int[std::min(arr1Size, arr2Size)]{};  
    int newArrayIndex{};  
  
    for (int i = 0; i < arr1Size; ++i) {  
        const int currentElement = *(arr1 + i);  
  
        for (int j = 0; j < arr2Size; ++j) {  
            if (*(arr2 + j) == currentElement) {  
                for (int k = 0; k < newArrayIndex; ++k) {  
                    if (*(newArray + k) == currentElement)  
                        goto alreadyAdded;  
                }                *(newArray + newArrayIndex) = currentElement;  
                ++newArrayIndex;  
            alreadyAdded:  
  
            }  
        }    
	}
	  
    if (newArrayIndex == std::min(arr1Size, arr2Size)) {  
        delete[] arr1;  
        arr1 = newArray;  
        arr1Size = std::min(arr1Size, arr2Size);  
  
    } else {  
        int *finalNewArray = new int[newArrayIndex];  
        for (int i = 0; i < newArrayIndex; ++i)  
            *(finalNewArray + i) = *(newArray + i);  
  
        delete[] newArray;  
        newArray = nullptr;  
  
        delete[] arr1;  
        arr1 = finalNewArray;  
        finalNewArray = nullptr;  
        arr1Size = newArrayIndex;  
    }
}

// ------------------------
  
// arr1 = [1, 2, 3]; arr2 = [2, 3, 4]; res = [1, 4];  
void arraySymmetricDifference(  
    int *&arr1, int &arr1Size, const int *arr2, const int &arr2Size  
) {  
    int *newArray = new int[arr1Size + arr2Size];  
    int newArrayIndex{};  
  
    for (int i = 0; i < arr1Size; ++i) {  
        const int currentElement = *(arr1 + i);  
  
        for (int j = 0; j < arr2Size; ++j)  
            if (*(arr2 + j) == currentElement)  
                goto skipNumber;  
  
        for (int k = 0; k < newArrayIndex; ++k)  
            if (*(newArray + k) == currentElement)  
                goto skipNumber;  
  
        *(newArray + newArrayIndex) = currentElement;  
        ++newArrayIndex;  
    skipNumber:  
    }  
  
    for (int i = 0; i < arr2Size; ++i) {  
        const int currentElement = *(arr2 + i);  
  
        for (int j = 0; j < arr1Size; ++j)  
            if (*(arr1 + j) == currentElement)  
                goto skipNumber2;  
  
        for (int k = 0; k < newArrayIndex; ++k)  
            if (*(newArray + k) == currentElement)  
                goto skipNumber2;  
  
        *(newArray + newArrayIndex) = currentElement;  
        ++newArrayIndex;  
    skipNumber2:  
    }  
  
    if (newArrayIndex == arr1Size + arr2Size) {  
        delete[] arr1;  
        arr1 = newArray;  
        arr1Size = arr1Size + arr2Size;  
  
    } else {  
        int *finalNewArray = new int[newArrayIndex];  
        for (int i = 0; i < newArrayIndex; ++i)  
            *(finalNewArray + i) = *(newArray + i);  
  
        delete[] newArray;  
        newArray = nullptr;  
  
        delete[] arr1;  
        arr1 = finalNewArray;  
        finalNewArray = nullptr;  
        arr1Size = newArrayIndex;  
    }
}
```
