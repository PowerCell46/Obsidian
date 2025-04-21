
## Selection Sort

```cpp
// Custom Impl
void selectionSort(int* array, const int& arraySize) {  
    for (int i = 0; i < arraySize; ++i) {  
        int swapIndex = i;  
  
        for (int j = i + 1; j < arraySize; ++j)  
            if (*(array + j) < *(array + swapIndex))  
                swapIndex = j;  
  
        std::swap(*(array + i), *(array + swapIndex));  
    }
}

// Standard Impl
void selectionSort(int* array, const int& size) {  
    for (int i = 0; i < size; ++i) {  
        int smallestIndex = i;  
  
        for (int j = i + 1; j < size; ++j)  
            if (*(array + j) < *(array + smallestIndex))  
                smallestIndex = j;  
  
        if (smallestIndex != i) {  
            *(array + i) ^= *(array + smallestIndex);  
            *(array + smallestIndex) ^= *(array + i);  
            *(array + i) ^= *(array + smallestIndex);  
        }    
	}
}
```

## Bubble sort

```cpp
// Custom Impl 
void bubleSort(int* array, const int& arraySize) {  
    for (int i = arraySize; i > -1; --i) 
    // Upper bound (slowly decreasing)  
        for (int j = 0; j < i - 1; ++j)  
            if (*(array + j) > *(array + j + 1)) { 
            // If (current element > next element) -> swap them  
                *(array + j) ^= *(array + j + 1);  
                *(array + j + 1) ^= *(array + j);  
                *(array + j) ^= *(array + j + 1);  
            } 
            // On each iteration the biggest element found will be moved to the end -> decreasing the size by 1  
}


// Standard Impl
void bubleSort(int* array, const int& arraySize) {  
    for (int end = arraySize - 1; end > 0; --end) {  
        for (int i = 0; i < end; ++i) {  
            if (*(array + i) > *(array + i + 1)) {  
                *(array + i) ^= *(array + i + 1);  
                *(array + i + 1) ^= *(array + i);  
                *(array + i) ^= *(array + i + 1);  
            }        
		}    
	}
}
```

## Insertion Sort

```cpp
// Custom Impl
int binarySearchInsertPosition(
	const int *array, const int &begin, const int &end, const int &insertElement
) {  
    if (begin >= end)  
        return begin;  
  
    const int middle = (begin + end) / 2;  
  
    if (*(array + middle) < insertElement)  
        return binarySearchInsertPosition(array, middle + 1, end, insertElement);  
  
    return binarySearchInsertPosition(array, begin, middle, insertElement);  
}  
  
int insertInSortedArray(int *array, const int &arraySize, const int &insertElement) {  
    int keeper = insertElement;  
    const int insertIndex = binarySearchInsertPosition(array, 0, arraySize, insertElement);  
  
    for (int i = insertIndex; i < arraySize; ++i) {  
        *(array + i) ^= keeper;  
        keeper ^= *(array + i);  
        *(array + i) ^= keeper;  
    }  
    
    return keeper;  
}  
  
void insertionSort(int *array, const int &arraySize) {  
    for (int i = 1; i < arraySize; ++i)  
        *(array + i) = insertInSortedArray(array, i, *(array + i));  
}


// Standard Impl
void insertionSort(int* array, const int& arraySize) {  
    for (int i = 1; i < arraySize; ++i) {  
        int current = *(array + i);  
  
        int j = i - 1;  
  
        while (j >= 0 && *(array + j) > current) {  
            *(array + j + 1) = *(array + j);  
            --j;  
        }  
        *(array + j + 1) = current;  
    }
}
```

## Quick Sort

Select a pivot (begin, middle, end), move all smaller elements to the left, all bigger elements to the right, put the pivot to its right place and call the function with the two parts recursively.

```cpp
void quickSort(int* array, const int& arraySize) {  
    if (arraySize <= 1) // array is already sorted  
        return;  
  
    const int pivotIndex = arraySize - 1;  
    const int pivotElement = *(array + pivotIndex);  
  
    int previousIndex = -1, currentIndex = 0;  
  
    while (currentIndex < pivotIndex) {  
        if (*(array + currentIndex) < pivotElement) { 
        // if current < pivot -> move it to the left  
            ++previousIndex;  
            if (previousIndex != currentIndex) {  
                *(array + previousIndex) ^= *(array + currentIndex);  
                *(array + currentIndex) ^= *(array + previousIndex);  
                *(array + previousIndex) ^= *(array + currentIndex);  
            }        
		}        
		
		++currentIndex;  
    }  
    
    if (previousIndex + 1 != pivotIndex) { 
    // put the pivot in it's right place (left + 1)  
        *(array + previousIndex + 1) ^= *(array + pivotIndex);  
        *(array + pivotIndex) ^= *(array + previousIndex + 1);  
        *(array + previousIndex + 1) ^= *(array + pivotIndex);  
    }    
    // This way all smaller are on the left, all bigger on the right  
  
    quickSort(array, previousIndex + 1); // Call with the left side  
    quickSort(array + previousIndex + 2, arraySize - previousIndex - 2); 
    // Call with the right side  
}
```

## Merge Sort

```cpp
void merge(int* array, const int& left, const int& middle, const int& right) {
    const int leftSubArraySize = middle - left + 1;
    const int rightSubArraySize = right - middle;

    int* leftSubArray = new int[leftSubArraySize]; // Initialize the left sub array
    for (int i = 0; i < leftSubArraySize; ++i) // Copy the left elements to the sub array
        leftSubArray[i] = array[left + i];

    int* rightSubArray = new int[rightSubArraySize];  // Initialize the right sub array
    for (int i = 0; i < rightSubArraySize; ++i) // Copy the right elements to the sub array
        rightSubArray[i] = array[middle + 1 + i];

    int leftSubArrayIndex{}, rightSubArrayIndex{}, mergeIndex = left;

    while (leftSubArrayIndex < leftSubArraySize && rightSubArrayIndex < rightSubArraySize) {
        // Start comparing the values, until one or both of the sub arrays, run out of elements
        if (leftSubArray[leftSubArrayIndex] <= rightSubArray[rightSubArrayIndex])
            array[mergeIndex++] = leftSubArray[leftSubArrayIndex++];
        else
            array[mergeIndex++] = rightSubArray[rightSubArrayIndex++];
    }

    while (leftSubArrayIndex < leftSubArraySize)
        // If there are left out elements from the left sub array -> add them to the end
        array[mergeIndex++] = leftSubArray[leftSubArrayIndex++];

    while (rightSubArrayIndex < rightSubArraySize)
        // If there are left out elements from the right sub array -> add them to the end
        array[mergeIndex++] = rightSubArray[rightSubArrayIndex++];

    // After sorting the elements, delete the temporary sub arrays
    delete[] leftSubArray;
    delete[] rightSubArray;
}

void mergeSort(int* array, const int& left, const int& right) {
    if (left < right) {
        const int middle = (left + right) / 2;

        mergeSort(array, left, middle);
        mergeSort(array, middle + 1, right);

        merge(array, left, middle, right);
    }
}

int main() {
    constexpr size_t arrSize = 4;
    int* array = new int[arrSize]{4, 1, 3, 2};

    mergeSort(array, 0, arrSize - 1);

    for (int i = 0; i < arrSize; ++i)
        std::cout << *(array + i) << ' ';

    return 0;
}
```

## Count Sort
Create an array with the max element, use it as a hash map of occurrences, then traverse through it and place the elements back in the original array.

```cpp
void countSort(int* array, const int& arraySize, const int& maxElement) {
    int* occurrences = new int[maxElement + 1]{};

    for (int i = 0; i < arraySize; ++i)
        *(occurrences + *(array + i)) += 1;

    int currentArrayIndex = -1;
    for (int i = 0; i < maxElement + 1; ++i)
        for (int j = 0; j < *(occurrences + i); ++j)
            *(array + (++currentArrayIndex)) = i;

    delete[] occurrences;
}

int main() {
    constexpr int arraySize = 10;
    int* array = new int[arraySize]{6, 3, 9, 10, 15, 6, 8, 12, 3, 6};

    countSort(array, arraySize, 15);

    for (int i = 0; i < arraySize; ++i)
        std::cout << *(array + i) << ' ';

    delete[] array;
    return 0;
}
```

## Bucket Sort
Same as Count Sort, instead of using an array as a hash map, use an array of Linked lists.

```cpp
struct ListNode { // Single Linked List
    int value;
    ListNode* next;
};

void bucketSort(int* array, const int& arraySize, const int& maxElement) {
    ListNode** occurrences = new ListNode*[maxElement + 1]{};

    for (int i = 0; i < arraySize; ++i) {
        if (*(occurrences + *(array + i)) == nullptr)
            *(occurrences + *(array + i)) = new ListNode{*(array + i), nullptr};

        else {
            ListNode* currentNode = *(occurrences + *(array + i));
            while (currentNode->next)
                currentNode = currentNode->next;
            currentNode->next = new ListNode{*(array + i), nullptr};
        }
    }

    int arrayIndex = -1;
    for (int i = 0; i < maxElement + 1; ++i) {
        ListNode* currentNode = *(occurrences + i);

        while (currentNode) {
            ListNode* nextNode = currentNode->next;
            *(array + (++arrayIndex)) = currentNode->value;

            delete currentNode;
            currentNode = nextNode;
        }
    }

    delete[] occurrences;
}

int main() {
    constexpr int arraySize = 10;
    int* array = new int[arraySize]{6, 3, 9, 10, 15, 6, 8, 12, 3, 6};

    bucketSort(array, arraySize, 15);

    for (int i = 0; i < arraySize; ++i)
        std::cout << *(array + i) << ' ';

    delete[] array;
    return 0;
}
```

## Radix Sort

Sort by the digits in reverse putting them in buckets of digits, rearranging the array, repeating for every 10n.

arr = 237, 146, 259, 455, 203, 35, 4

- sort by last digit
arr = 203, 4, 455, 35, 146, 237, 259

- sort by middle digit
arr = 203, 4, 35, 237, 146, 455, 259

- sort by first digit
arr = 4, 35, 146, 203, 237, 259, 455

```cpp
struct LinkNode {
    int number;
    LinkNode* next;
};

void radixSort(int* array, const int& arraySize, const int& maxElement, const int& callNumber = 1) {
    if (callNumber <= std::to_string(maxElement).size()) {
        constexpr int DIGITS_COUNT = 10; // 0 1 2 3 4 5 6 7 8 9
        LinkNode** buckets = new LinkNode*[DIGITS_COUNT]{};

        for (int i = 0; i < arraySize; ++i) {
            const int currentNumber = *(array + i);
            const int currentBucketDigit = (currentNumber / static_cast<int>(std::pow(10, callNumber))) % 10;
            
            if (*(buckets + currentBucketDigit) == nullptr)
                *(buckets + currentBucketDigit) = new LinkNode{currentNumber, nullptr};

            else {
                LinkNode* currentNode = *(buckets + currentBucketDigit);
                while (currentNode->next)
                    currentNode = currentNode->next;
                currentNode->next = new LinkNode{currentNumber, nullptr};
            }
        }

        int arrayIndex = -1;
        for (int i = 0; i < DIGITS_COUNT; ++i) {
            LinkNode* currentNode = *(buckets + i);

            while (currentNode) {
                LinkNode* next = currentNode->next;
                *(array + (++arrayIndex)) = currentNode->number;
                delete currentNode;
                currentNode = next;
            }
        }

        delete[] buckets;
        radixSort(array, arraySize, maxElement, callNumber + 1);
    }
}


int main() {
    constexpr int arraySize = 7;
    int* array = new int[arraySize]{237, 146, 259, 455, 203, 35, 4};

    radixSort(array, arraySize, 455);

    for (int i = 0; i < arraySize; ++i)
        std::cout << *(array + i) << ' ';

    delete[] array;
    return 0;
}
```

## Shell Sort

```cpp
void shellSort(int* array, const int& arraySize) {
    for (int gap = arraySize / 2; gap > 0; gap /= 2) {
        for (int i = gap; i < arraySize; ++i) {
            int temp = array[i];
            int j = i;

            while (j >= gap && array[j - gap] > temp) {
                array[j] = array[j - gap];
                j -= gap;
            }

            array[j] = temp;
        }
    }
}
```

## Heap Sort

```cpp
void heapSort(int *&heap, const int &heapSize, const int &originalHeapSize = -1) {
    if (heapSize == 1) { // 1 element case
        // Without this the array is reversed
        for (int i = 0; i < originalHeapSize / 2; ++i) {
            *(heap + i) ^= *(heap + originalHeapSize - 1 - i);
            *(heap + originalHeapSize - 1 - i) ^= *(heap + i);
            *(heap + i) ^= *(heap + originalHeapSize - 1 - i);
        }
        return;
    }

    // Swap the last element with the first one
    *(heap) ^= *(heap + heapSize - 1);
    *(heap + heapSize - 1) ^= *(heap);
    *(heap) ^= *(heap + heapSize - 1);

    if (heapSize > 3) {
        int index = 0;

        while ((index + 1) * 2 - 1 < heapSize - 1) {
            const int currentElement = *(heap + index);

            const int leftIndex = (index + 1) * 2 - 1;
            const int leftChild = heap[leftIndex];

            const int rightIndex = (index + 1) * 2;
            const int rightChild = (rightIndex < heapSize) ? heap[rightIndex] : INT_MIN;

            if (leftChild < currentElement && rightChild < currentElement)
                // Current element is bigger than both of its children
                break;

            if (leftChild > rightChild) {
                // If left child > right child && left child > parent -> swap left child with parent
                *(heap + index) ^= *(heap + (index + 1) * 2 - 1);
                *(heap + (index + 1) * 2 - 1) ^= *(heap + index);
                *(heap + index) ^= *(heap + (index + 1) * 2 - 1);
                index = (index + 1) * 2 - 1;

            } else { // If right child > left child && right child > parent -> swap right child with parent
                *(heap + index) ^= *(heap + (index + 1) * 2);
                *(heap + (index + 1) * 2) ^= *(heap + index);
                *(heap + index) ^= *(heap + (index + 1) * 2);
                index = (index + 1) * 2;
            }
        }

    } else if (heapSize == 3 && *(heap) < *(heap + 1)) { // Edge case: 3 elements, left child is bigger than right child
        *(heap) ^= *(heap + 1);
        *(heap + 1) ^= *(heap);
        *(heap) ^= *(heap + 1);
    }

    heapSort(heap, heapSize - 1, originalHeapSize == -1 ? heapSize : originalHeapSize);
}


int main() {
    int heapSize = 6;
    int *heap = new int[heapSize]{40, 35, 30, 15, 10, 25};
    heapSort(heap, heapSize);

    for (int i = 0; i < heapSize; ++i)
        std::cout << *(heap + i) << ' ';

    delete[] heap;
    return 0;
}
```