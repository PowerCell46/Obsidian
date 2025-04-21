## Queue implemented with Single Link List

```cpp
struct LinkList {  
    int number;  
    LinkList* next;  
};  
  
// 1 next-> 2 next-> 3 next-> nullptr;  
LinkList* initializeLinkListQueue(
	const int* array, const int& arraySize, LinkList*& endOfQueue
) {  
    LinkList* beginOfQueue = new LinkList{*array, nullptr};  
    endOfQueue = beginOfQueue;  
  
    for (int i = 1; i < arraySize; ++i) {  
        LinkList* current = new LinkList{*(array + i), nullptr};  
        endOfQueue->next = current;  
        endOfQueue = current;  
    }  
    
    return beginOfQueue;  
}  
  
int popLinkListQueue(LinkList*& queueStart) {  
    if (!queueStart)  
        return -1;  
  
   const int poppedElement = queueStart->number;  
  
    LinkList* temp = queueStart;  
    queueStart = queueStart->next;  
    delete temp;  
    return poppedElement;  
}  
  
LinkList* pushToLinkListQueue(LinkList*& beginOfQueue, const int& pushElement) {  
    LinkList* newBeginOfQueue = new LinkList{pushElement, beginOfQueue};  
    beginOfQueue = newBeginOfQueue;  
    return beginOfQueue;  
}  
  
void recursiveDeleteLinkListQueue(const LinkList* queueBegin) {  
    if (queueBegin) {  
        recursiveDeleteLinkListQueue(queueBegin->next);  
        // std::cout << queueBegin->number << ' ';  
        delete queueBegin;  
    }
}
```

## Queue implemented with Array

```cpp
int *initializeArrayQueue(const int &size) {  
    if (size < 1)  
        return nullptr;  
  
    int *queue = new int[size];  
  
    for (int i = 0; i < size; ++i) {  
        std::cout << "Enter value " << (i + 1) << ": ";  
        std::cin >> *(queue + i);  
    }  
    
    return queue;  
}  
  
void pushArrayQueueElement(int *&queue, int &queueSize, const int &pushElement) {  
    int *newQueue = new int[queueSize + 1];  
  
    for (int i = 0; i < queueSize; ++i)  
        *(newQueue + i) = *(queue + i);  
  
    *(newQueue + queueSize) = pushElement;  
  
    delete[] queue;  
    queue = newQueue;  
    ++queueSize;  
}  
  
int popArrayQueue(int *&queue, int &queueSize) {  
    if (queueSize < 1)  
        return -1;  
  
    const int popElement = *queue;  
  
    if (queueSize == 1) {  
        delete[] queue;  
        queue = nullptr;  
        --queueSize;  
        return popElement;  
    } 
     
    int *newQueue = new int[queueSize - 1];  
  
    for (int i = 1; i < queueSize; ++i)  
        *(newQueue + i - 1) = *(queue + i);  
  
    delete[] queue;  
    queue = newQueue;  
    --queueSize;  
    return popElement;  
}
```

## Priority Queue

```cpp
class PriorityQueue {  
    int* data;  
    int currentDataSize;  
    int currentIndex;  
  
    void doubleDataSize() {  
        this->currentDataSize *= 2;  
        int* newData = new int[this->currentDataSize]{};  
  
        memcpy(newData, this->data, (this->currentIndex + 1) * sizeof(int));  
  
        delete[] this->data;  
        this->data = nullptr;  
        this->data = newData;  
    }  
public:  
    explicit PriorityQueue(const int& size) :  
        data(new int[size]{}), currentDataSize(size), currentIndex(0) {}  
  
    ~PriorityQueue() {  
        delete[] this->data;  
    }  
    void insertElement(const int& element) {  
        if (this->currentIndex == this->currentDataSize)  
            doubleDataSize();  
  
        *(this->data + this->currentIndex) = element;  
        int index =  this->currentIndex;  
  
        while (index > 0 && *(this->data + (index + 1) / 2 - 1) < *(this->data + index)) {  
            std::swap(  
                *(this->data + (index + 1) / 2 - 1), // Parent  
                *(this->data + index) // Current  
            );  
  
            index = (index + 1) / 2 - 1;  
        }  
        ++this->currentIndex;  
    }  
    [[nodiscard]] int top() const {  
        if (this->currentIndex == 0)  
            throw std::out_of_range("Priority Queue is empty!");  
  
        return *this->data;  
    }  
    int pop() {  
        int index = 0;  
        const int removeElement = *(this->data);  
        *(this->data) = *(this->data + this->currentIndex - 1);  
        --this->currentIndex;  
  
        while (index < this->currentIndex) {  
            // const int currentElement = *(this->data + index);  
            const int leftChild = (index * 2 + 1 < this->currentIndex) ? *(this->data + index * 2 + 1) : INT_MIN;  
            const int rightChild = (index * 2 + 2 < this->currentIndex) ? *(this->data + index * 2 + 2) : INT_MIN;  
  
            if (leftChild == INT_MIN && rightChild == INT_MIN)  
                break;  
  
            if (leftChild >= rightChild) {  
                std::swap(*(this->data + index), *(this->data + index * 2 + 1));  
                index = index * 2 + 1;  
  
            } else {  
                std::swap(*(this->data + index), *(this->data + index * 2 + 2));  
                index = index * 2 + 2;  
            }        }  
        return removeElement;  
    }  
    friend std::ostream& operator<<(std::ostream& os, const PriorityQueue& priorityQueue);  
};  
  
std::ostream& operator<<(std::ostream& os, const PriorityQueue& priorityQueue) {  
    for (int i = 0; i < priorityQueue.currentIndex; ++i)  
        os << *(priorityQueue.data + i) << ' ';  
    os << '\n';  
  
    return os;  
}  
```