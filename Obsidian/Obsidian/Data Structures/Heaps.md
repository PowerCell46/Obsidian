# Insertion in Max Heap

```cpp
void insertInHeap(int*& heap, int& heapSize, const int& insertElement) {
    int* newHeap = new int[heapSize + 1];
    for (int i = 0; i < heapSize; ++i)
        *(newHeap + i) = *(heap + i);

    int insertElementIndex = heapSize;
    *(newHeap + insertElementIndex) = insertElement;

    while (insertElementIndex > 0) {
        // If the element is bigger than it's parent -> swap them
        if (*(newHeap + (insertElementIndex + 1) / 2 - 1) < *(newHeap + insertElementIndex)) {
            *(newHeap + insertElementIndex) ^= *(newHeap + (insertElementIndex + 1) / 2 - 1);
            *(newHeap + (insertElementIndex + 1) / 2 - 1) ^= *(newHeap + insertElementIndex);
            *(newHeap + insertElementIndex) ^= *(newHeap + (insertElementIndex + 1) / 2 - 1);
        }

        insertElementIndex = (insertElementIndex + 1) / 2 - 1;
    }

    delete[] heap;
    heap = newHeap;
    ++heapSize;
}

int main() {
    int heapSize = 7;
    int* heap = new int[heapSize]{30, 25, 21, 20, 19, 18, 15};

    insertInHeap(heap, heapSize, 40);
    insertInHeap(heap, heapSize, 10);

    for (int i = 0; i < heapSize; ++i)
        std::cout << *(heap + i) << ' ';

    delete[] heap;

    return 0;
}
```

## Create Heap from an Array

```cpp
void insertInHeap(int* heap, const int& heapSize) {
    int index = heapSize - 1;

    while (index > 0) {
        if (*(heap + (index + 1) / 2 - 1) < *(heap + index)) {
            *(heap + (index + 1) / 2 - 1) ^= *(heap + index);
            *(heap + index) ^= *(heap + (index + 1) / 2 - 1);
            *(heap + (index + 1) / 2 - 1) ^= *(heap + index);
        }
        index = (index + 1) / 2 - 1;
    }
}

void convertArrayToHeap(int *heap, const int &heapSize) {
    for (int i = 1; i < heapSize; ++i)
        insertInHeap(heap, i + 1);
}

int main() {
    constexpr int heapSize = 7;
    int *heap = new int[heapSize]{30, 25, 20, 10, 5, 40, 35};

    convertArrayToHeap(heap, heapSize);

    for (int i = 0; i < heapSize; ++i)
        std::cout << *(heap + i) << ' ';

    delete[] heap;

    return 0;
}
```

# Delete from Heap

```cpp
int deleteFromHeap(int *&heap, int &heapSize) {  
    const int deleteElement = *heap;  
  
    if (heapSize == 1) { // 1 element case  
        delete[] heap;  
  
        --heapSize;  
        heap = nullptr;  
  
        return deleteElement;  
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
  
            } else { 
            // If right child > left child && right child > parent -> swap right child with parent  
                *(heap + index) ^= *(heap + (index + 1) * 2);  
                *(heap + (index + 1) * 2) ^= *(heap + index);  
                *(heap + index) ^= *(heap + (index + 1) * 2);  
                index = (index + 1) * 2;  
            }        }  
    } else if (heapSize == 3 && *(heap) < *(heap + 1)) { 
    // Edge case: 3 elements, left child is bigger than right child  
        *(heap) ^= *(heap + 1);  
        *(heap + 1) ^= *(heap);  
        *(heap) ^= *(heap + 1);  
    }  
    int *newHeap = new int[heapSize - 1]; // Shrink the array by its last element  
    for (int i = 0; i < heapSize - 1; ++i) // Transfer the elements  
        *(newHeap + i) = *(heap + i);  
  
    delete[] heap;  
    heap = newHeap;  
    --heapSize;  
  
    return deleteElement;  
}  
  
  
int main() {  
    int heapSize = 6;  
    int *heap = new int[heapSize]{40, 35, 30, 15, 10, 25};  
    for (int i = 0; i < 6; ++i)  
        std::cout << deleteFromHeap(heap, heapSize) << ' ';  
  
    delete[] heap;  
    return 0;  
}
```

## Heapify (Faster creation of a Heap)

```cpp
void createHeapFromAnArray(int* heap, const int& heapSize, const int& index) {
    int currentIndex = index;

    while (currentIndex < heapSize) {
        const int leftChild = currentIndex * 2 + 1 < heapSize ? *(heap + currentIndex * 2 + 1) : INT_MIN;
        const int rightChild = currentIndex * 2 + 2 < heapSize ? *(heap + currentIndex * 2 + 2) : INT_MIN;

        if (leftChild > *(heap + currentIndex) && rightChild > *(heap + currentIndex)) {
            if (leftChild >= rightChild) {
                *(heap + currentIndex * 2 + 1) ^= *(heap + currentIndex);
                *(heap + currentIndex) ^= *(heap + currentIndex * 2 + 1);
                *(heap + currentIndex * 2 + 1) ^= *(heap + currentIndex);
                currentIndex = currentIndex * 2 + 1;

            } else {
                *(heap + currentIndex * 2 + 2) ^= *(heap + currentIndex);
                *(heap + currentIndex) ^= *(heap + currentIndex * 2 + 2);
                *(heap + currentIndex * 2 + 2) ^= *(heap + currentIndex);
                currentIndex = currentIndex * 2 + 2;
            }

        } else if (leftChild > *(heap + currentIndex)) {
            *(heap + currentIndex * 2 + 1) ^= *(heap + currentIndex);
            *(heap + currentIndex) ^= *(heap + currentIndex * 2 + 1);
            *(heap + currentIndex * 2 + 1) ^= *(heap + currentIndex);
            currentIndex = currentIndex * 2 + 1;

        } else if (rightChild > *(heap + currentIndex)) {
            *(heap + currentIndex * 2 + 2) ^= *(heap + currentIndex);
            *(heap + currentIndex) ^= *(heap + currentIndex * 2 + 2);
            *(heap + currentIndex * 2 + 2) ^= *(heap + currentIndex);
            currentIndex = currentIndex * 2 + 2;

        } else {
            break;
        }
    }
}

void heapify(int* heap, const int& heapSize) {
    for (int i = heapSize - 1; i > -1; --i)
        createHeapFromAnArray(heap, heapSize, i);
}

int main() {
    int heapSize = 7;
    int* heap = new int[heapSize]{5, 1, 2, 3, 4, 30, 50};
    heapify(heap, heapSize);

    for (int i = 0; i < heapSize; ++i)
        std::cout << *(heap + i) << ' ';

    delete[] heap;
    return 0;
}
```