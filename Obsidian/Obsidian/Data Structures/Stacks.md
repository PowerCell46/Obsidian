
## Implementing Stack using Array

```cpp
int* createArrayBasedStack(int& stackSize) {  
    std::cout << "Enter stack size: ";  
    std::cin >> stackSize;  
  
    int* stack = new int[stackSize]{};  
  
    for (int i = 0; i < stackSize; ++i) {  
        std::cout << "Enter stack element " << (i + 1) << ": ";  
        std::cin >> *(stack + i);  
    }  
    
    return stack;  
}  
  
void arrayBasedStackPushElement(
	int*& stack, int& stackSize, const int& pushElement
) {  
    int* newStack = new int[stackSize + 1];  
  
    for (int i = 0; i < stackSize; ++i)  
        *(newStack + i) = *(stack + i);  
  
    *(newStack + stackSize) = pushElement;  
    delete[] stack;  
    stack = newStack;  
    newStack = nullptr;  
    ++stackSize;  
}  
  
int arrayBasedStackPopElement(int*& stack, int& stackSize) {  
    if (!stackSize)  
        return -1;  
  
    int* newStack = new int[stackSize - 1];  
  
    for (int i = 0; i < stackSize - 1; ++i)  
        *(newStack + i) = *(stack + i);  
  
    const int poppedElement = *(stack + stackSize - 1);  
  
    delete[] stack;  
    stack = newStack;  
    newStack = nullptr;  
    --stackSize;  
    return poppedElement;  
}  
  
int arrayBasedStackPeek(const int* stack, const int& stackSize) {  
    return *(stack + stackSize - 1);  
}  
  
bool arrayBasedStackIsEmpty(const int* stack, const int& stackSize) {  
    return stackSize == 0;  
}  
  
void printArrayBasedStack(const int* stack, const int& stackSize) {  
	if (stackSize < 1)
		return;
		
    std::cout << "| ";  
    for (int i = 0; i < stackSize; ++i)  
        std::cout << *(stack + i) << " | ";  
  
    std::cout << '\n';  
}  
  
void clearArrayBasedStack(int*& stack, int& stackSize) {  
    delete[] stack;  
    stack = nullptr;  
    stackSize = 0;  
}  
  
int getArrayBasedStackSize(const int* stack, const int& stackSize) {  
    return stackSize;  
}  
  
int main() {  
    int stackSize;  
    int* stack = createArrayBasedStack(stackSize);  
  
    int newElement;  
    std::cout << "Enter new push element: ";  
    std::cin >> newElement;  
  
    // clearArrayBasedStack(stack, stackSize);  
  
    arrayBasedStackPushElement(stack, stackSize, newElement);  
  
    // std::cout << arrayBasedStackPopElement(stack, stackSize) << '\n';  
  
    // std::cout << arrayBasedStackPeek(stack, stackSize) << '\n'; 
     
    printArrayBasedStack(stack, stackSize);  
    return 0;  
}
```

## Implementing Stack using Link List

```cpp
// Singly linked list (takes min storage (most optimal))
struct LinkList {  
    int number;  
    LinkList* next;  
};  
  
LinkList* createLinkListBasedStack(const int& stackSize) {  
    if (stackSize < 1)  
        return nullptr;  
  
    LinkList* next = nullptr;  
    for (int i = 0; i < stackSize; ++i) {  
        int currentNumber;  
        std::cout << "Enter stack element " << (i + 1) << ": ";  
        std::cin >> currentNumber;  
  
        LinkList* current = new LinkList{currentNumber, next};  
        next = current;  
    }  
    return next;  
}  
  
LinkList* linkListBasedStackPushElement(LinkList*& stack, const int& pushElement) {  
    LinkList* newHead = new LinkList{pushElement, stack};  
    stack = newHead;  
    return stack;  
}  
  
LinkList* linkListBasedStackPopElement(LinkList*& stack) {  
    if (!stack)  
        return nullptr;  
  
    LinkList* newHead = stack->next;  
    delete stack;  
  
    stack = newHead;  
    return newHead;  
}  
  
int linkListBasedStackPeek(const LinkList* stack) {  
    return stack ? stack->number : -1;  
}  
  
bool linkListBasedStackIsEmpty(const LinkList* stack) {  
    return stack == nullptr;  
}  
  
void printLinkListBasedStack(const LinkList* stack) {  
    if (!stack)  
        return;  
  
    std::cout << "| ";  
  
    while (stack) {  
        std::cout << stack->number << " | ";  
        stack = stack->next;  
    }  
    std::cout << '\n';  
}  
  
void recursiveDeleteLinkListBasedStack(const LinkList* stack) {  
    if (stack) {  
        recursiveDeleteLinkListBasedStack(stack->next);  
        delete stack;  
    }
}  
  
void clearLinkListBasedStack(LinkList*& stack) {  
    recursiveDeleteLinkListBasedStack(stack);  
    stack = nullptr;  
}  
  
int getLinkListBasedStackSize(const LinkList* stack) {  
    int sizeCounter{};  
  
    while (stack) {  
        ++sizeCounter;  
        stack = stack->next;  
    }  
    return sizeCounter;  
}  
  
int main() {  
    int stackSize;  
    std::cout << "Enter stack size: ";  
    std::cin >> stackSize;  
  
    LinkList* stack = createLinkListBasedStack(stackSize);  
  
    // int pushElement;  
    // std::cout << "Enter push element: ";    
    // std::cin >> pushElement;  
  
	// stack = linkListBasedStackPushElement(stack, pushElement);  
   
	// stack = linkListBasedStackPopElement(stack);  
   
	// printLinkListBasedStack(stack);  
  
	// clearLinkListBasedStack(stack);  
    
    std::cout << "Stack size: " << getLinkListBasedStackSize(stack) << '\n';  
  
    return 0;  
}
```