# Single Linked List

#### (1 -> 2 -> 3 -> 4 -> 5)

  ```cpp
struct LinkedNode {  
    int number;  
    LinkedNode* next;  
};  
  
int main() {  
    LinkedNode* l3 = new LinkedNode{30, nullptr};  
    LinkedNode* l2 = new LinkedNode{20, l3};  
    LinkedNode* l1 = new LinkedNode{10, l2};  

    // Iterating through the elements
    LinkedNode* p = l1;  
  
    while (p) {  
        std::cout << p->number << ' ';  
        LinkedNode* next = p->next;  
	    
        delete p;  
        p = next;  
    }
}
```

## Dynamically create Single Linked List

```cpp
struct LinkedList {  
    int number;  
    LinkedList* next;  
}; 

// Recursive
LinkNode* initializeLinkList(
	int* numbers, const int& arraySize, const int& currentIndex = 0
) {
    if (currentIndex < arraySize) {
        LinkNode* next = initializeLinkList(numbers, arraySize, currentIndex + 1);
        return new LinkNode{*(numbers + currentIndex), next};
    }
    return nullptr;
}

// Iterative
LinkedList* initializeLinkList(const int* arr, const int& n) {  
    LinkedList *first; // Point to the first node in the linked list  
    LinkedList *previous; // Used to keep track of the end of the list as you build it  
  
    first = new LinkedList{*arr, nullptr};  
    previous = first;  
  
    for (int i = 1; i < n; ++i) {  
        LinkedList* current = new LinkedList{*(arr + i), nullptr};  
  
        previous->next = current; // Setting current to previous->next  
        previous = current;  
    }  
    return first;  
}  
  
int main() {  
    int numbers[]{3, 5, 7, 9, 11, 13};  
    auto lst = initializeLinkList(numbers, 6);  
  
    while (lst) {  
        std::cout << lst->number << ' ';  
        LinkedList* next = lst->next;  
  
        delete lst;  
        lst = next;  
    }  
    return 0;  
}
```

## Sum elements, Count nodes, Delete nodes

```cpp
struct LinkNode {
    int number;
    LinkNode *next;
    
    LinkNode() :
        number(0), next(nullptr) {}
    
    LinkNode(const int& x, LinkNode *next) :
        number(x), next(next) {}
};

LinkNode* generateLinkList(const int* arr, const int& arrSize) {
    LinkNode* first = new LinkNode{*arr, nullptr};
    LinkNode* previous = first;

    for (int i = 1; i < arrSize; ++i) {
        LinkNode* current = new LinkNode{*(arr + i), nullptr};

        previous->next = current;
        previous = current;
    }

    return first;
}

int getLinkListSize(const LinkNode* linkNode) {
    if (linkNode)
        return getLinkListSize(linkNode->next) + 1;

    return 0;
}

int sumLinkList(const LinkNode* linkNode) {
    if (linkNode)
        return sumLinkList(linkNode->next) + linkNode->number;

    return 0;
}

int maxLinkListElement(const LinkNode* linkNode) {
    if (linkNode) {
        const int nextResult = maxLinkListElement(linkNode->next);
        return nextResult > linkNode->number ? nextResult : linkNode->number;
    }

    return INT_MIN;
}

int minLinkListElement(const LinkNode* linkNode) {
    if (linkNode) {
        const int nextResult = minLinkListElement(linkNode->next);
        return nextResult < linkNode->number ? nextResult : linkNode->number;
    }

    return INT_MAX;
}

void recursiveDeleteLinkList(LinkNode* linkNode) {
    if (linkNode) {
        recursiveDeleteLinkList(linkNode->next);
        delete linkNode;
    }
}

int main() {
    const int arr[]{1, 2, 3, 4, 5};

    LinkNode* lst = generateLinkList(arr, 5);

    std::cout << "Count of elements: " << getLinkListSize(lst) << '\n';

    std::cout << "Sum of elements: " << sumLinkList(lst) << '\n';

    std::cout << "Max element: " << maxLinkListElement(lst) << '\n';

    std::cout << "Min element: " << minLinkListElement(lst) << '\n';

    recursiveDeleteLinkList(lst);

    return 0;
}
```

## Linear Search for an element Iterative approach

```cpp
LinkList* linearSearchLinkListElement(
	LinkList* linkList, const int& searchElement
) {  
    while (linkList) {  
        if (linkList->number == searchElement)  
            return linkList;  
            
        return linearSearchLinkListElement(linkList->next, searchElement);  
    }    
    
    return nullptr;  
}

LinkList* linearSearchMoveToHead(LinkList* linkList, const int& searchNumber) {  
    LinkList* linkListFirst = linkList;  
    LinkList* previous = nullptr;  
  
    while (linkList) {  
        if (linkList->number == searchNumber) {  
            if (linkList == linkListFirst)  
                return linkList;  
                
            previous->next = linkList->next;  
            linkList->next = linkListFirst;  
            
            return linkList;  
        }        
        
        previous = linkList;  
        linkList = linkList->next;  
    }  
 
	return nullptr;  
}

LinkList* linearSearchTransposition(LinkList* linkList, const int& searchElement) {  
    if (!linkList)  
        return nullptr;  
  
    LinkList* previous = nullptr;  
 
	while (true) {  
        previous = linkList;  
        linkList = linkList->next;  
        if (!linkList)  
            break;  
            
        if (linkList->number == searchElement) {  
            if (previous) {  
                std::swap(previous->number, linkList->number);  
                return previous;  
            }            
            
            return linkList;  
        }    
	}
	  
    return nullptr;  
}
```

## Linear Search for an element Recursive approach

```cpp
LinkNode* linearSearchLinkListElement(LinkNode* linkNode, const int& searchElement) {
    if (linkNode) {
        if (linkNode->number == searchElement)
            return linkNode;
        return linearSearchLinkListElement(linkNode->next, searchElement);
    }

    return nullptr;
}

LinkNode* linearSearchMoveToHead(LinkNode* linkNode, const int& searchNumber, LinkNode* previous = nullptr, LinkNode* first = nullptr) {
    if (linkNode) {
        if (linkNode->number == searchNumber) {
            if (first == nullptr) // Search node is the first one
                return linkNode;

            if (previous == first) { // Search node is the second one
                first->next = linkNode->next;
                linkNode->next = first;
                return linkNode;
            }

            previous->next = first; // Previous node will point to the first node
            LinkNode* temp = first->next; // Keeping the first->next
            first->next = linkNode->next; // First next will point to currentNode->next
            linkNode->next = temp; // Current node will point to the first->next before it's overwritten
            return linkNode;
        }

        return linearSearchMoveToHead(
            linkNode->next,
            searchNumber,
            linkNode,
            first == nullptr ? linkNode : first
        );
    }

    return nullptr;
}

LinkNode* linearSearchTransposition(LinkNode* linkNode, const int& searchElement, LinkNode* previous = nullptr, LinkNode* previousPrevious = nullptr, LinkNode* first = nullptr) {
    if (linkNode) {
        if (linkNode->number == searchElement) {
            if (previous == nullptr) // First Element
                return linkNode;

            if (previousPrevious == nullptr) { // Second Element
                previous->next = linkNode->next;
                linkNode->next = previous;
                return linkNode;
            }

            previousPrevious->next = linkNode;

            LinkNode* temp = linkNode->next;
            linkNode->next = previous;
            previous->next = temp;
            return first;
        }

        return linearSearchTransposition(
            linkNode->next,
            searchElement,
            linkNode,
            previous != nullptr ? previous : nullptr,
            first == nullptr ? linkNode : first
        );
    }

    return nullptr;
}
```

## Insert Element in Single Linked List

```cpp
// Iterative
LinkList* insertLinkListElement(  
    LinkList* linkList, const int& insertIndex, const int& insertElement  
) {  
    if (insertIndex == 0)  
        return new LinkList{insertElement, linkList};  
  
    LinkList* first = linkList;  
    LinkList* previous = nullptr;  
  
    for (int i = 0; i < insertIndex; ++i) {  
        if (!linkList)  
            return nullptr;  
        previous = linkList;  
        linkList = linkList->next;  
    }  
 
	LinkList* newLinkList = new LinkList{insertElement, linkList};  
    previous->next = newLinkList;  
  
    return first;  
}

// Recursive
LinkNode* insertIntLinkList(
LinkNode* linkNode, const int& insertIndex, const int& insertElement, const int& currentIndex = 0, LinkNode* previous = nullptr, LinkNode* first = nullptr
) {  
    if (linkNode) {  
        if (currentIndex == insertIndex) {  
            if (previous == nullptr) // Insert at the beginning  
                return new LinkNode{insertElement, linkNode};  
  
            LinkNode* newElement = new LinkNode{insertElement, linkNode};  
            previous->next = newElement;  
            return first;  
        }  
        return insertIntLinkList(  
            linkNode->next,  
            insertIndex,  
            insertElement,  
            currentIndex + 1,  
            linkNode,  
            first == nullptr ? linkNode : first  
        );  
    }  
    if (previous == nullptr) // Empty Link List  
        return new LinkNode{insertElement, nullptr};  
  
    previous->next = new LinkNode{insertElement, nullptr}; // Insert at the end  
    return first;  
}
```

## Insert at the end only

```cpp
LinkList* insertLastLinkList(
	LinkList*& first, LinkList* last, const int& insertElement
) {    
	LinkList* current = new LinkList{insertElement, nullptr};    
	    
	if (first == nullptr)
		first = current;    
	    
	if (last != nullptr)    
		last->next = current;    
	    
	return current;
}
  
int main() {  
    int numbers[]{32, 523, 532, 734, 124};  
  
    LinkList* first = nullptr;  
    LinkList* last = nullptr;  
  
    last = insertLastLinkList(first, last, 10);  
    last = insertLastLinkList(first, last, 100);  
    last = insertLastLinkList(first, last, 1'000);  
    last = insertLastLinkList(first, last, 10'000);  
    last = insertLastLinkList(first, last, 100'000);  
  
    deleteLinkList(first);  
    return 0;  
}
```

## Insert in a sorted Linked List

```cpp
// Iterative
LinkList* insertInSortedLinkList(  
    LinkList* linkList, const int& insertElement  
) {    
	LinkList* first = linkList;    
	LinkList* previous = nullptr;    
	    
	while (linkList) {  
	    if (linkList->number >= insertElement &&(previous == nullptr || previous->number <= insertElement)) {  
	        LinkList* newNode = new LinkList{insertElement, linkList};  
	 
			if (previous)  
	            previous->next = newNode;  
	        else  
	            first = newNode;  
	        
	        return first;  
	    }        
	    previous = linkList;  
	  
	    linkList = linkList->next;  
	}    
	      
	LinkList* newNode = new LinkList{insertElement, nullptr};    
	      
	if (previous)    
		previous->next = newNode;    
	else    
		first = newNode;    
	    
	return first;  
}
  
// Recursive
LinkNode* insertInLinkedList(LinkNode* linkNode, const int& insertElement, LinkNode* previous = nullptr, LinkNode* first = nullptr) {
    if (linkNode) {
        if (linkNode->number > insertElement) { // If current > insert && previous < insert
            if (previous == nullptr) // Insert at the beginning
                return new LinkNode{insertElement, linkNode};

            LinkNode* newElement = new LinkNode{insertElement, linkNode};
            previous->next = newElement;
            return first;
        }
        return insertInLinkedList(
            linkNode->next,
            insertElement,
            linkNode,
            first == nullptr ? linkNode : first
        );
    }

    if (previous == nullptr) // If the Link List was empty 'til now
        return new LinkNode{insertElement, nullptr};

    LinkNode* newTail = new LinkNode{insertElement, nullptr}; // Insert at the end
    previous->next = newTail;
    return first;
}
```

## Deleting from a Linked List

```cpp
// Iterative
LinkNode *deleteLinkListElement(LinkNode *linkNode, const int &deleteIndex) {
    if (linkNode == nullptr)
        return nullptr;

    if (deleteIndex == 0) {
        LinkNode *second = linkNode->next;
        delete linkNode;
        return second;
    }

    LinkNode *first = linkNode;
    LinkNode *previous = nullptr;

    for (int i = 0; i < deleteIndex; ++i) {
        previous = linkNode;
        linkNode = linkNode->next;

        if (!linkNode)
            return first;
    }
    
    previous->next = linkNode->next;

    delete linkNode;
    return first;
}

// Recursive
LinkNode *deleteFromLinkedList(LinkNode *linkNode, const int &deleteIndex, const int &currentIndex = 0, LinkNode *first = nullptr, LinkNode *previous = nullptr) {
    if (linkNode) {
        if (currentIndex == deleteIndex) {
            if (first == nullptr) { // First element
                LinkNode *newFirst = linkNode->next;
                delete linkNode;
                return newFirst;
            }

            previous->next = linkNode->next;
            delete linkNode;
            return first;
        }

        return deleteFromLinkedList(
            linkNode->next,
            deleteIndex,
            currentIndex + 1,
            first == nullptr ? linkNode : first,
            linkNode
        );
    }

    return first;
}
```

## Check if Linked List is sorted

```cpp
// Iterative
bool isLinkedListSorted(LinkNode* linkNode) {    
    while (linkNode && linkNode->next) {    
        if (linkNode->number > linkNode->next->number)    
            return false;  
        linkNode = linkNode->next;    
    }  
    return true;  
}

// Recursive
bool isLinkListSorted(LinkNode* linkNode) {  
    if (linkNode->next) {  
        if (linkNode->number > linkNode->next->number)  
            return false;  
        return isLinkListSorted(linkNode->next);  
    }  
    return true;  
}
```

## Remove duplicates from a sorted Link List

```cpp
// Iterative
void deleteDuplicatesLinkedList(LinkList* linkList) {    
	if (!linkList || !linkList->next)    
		return;    
	    
	while (linkList->next) {    
		if (linkList->number == linkList->next->number) {    
			LinkList* newNext = linkList->next->next;    
			delete linkList->next;    
			linkList->next = newNext;  
		
		} else    
			linkList = linkList->next;    
    }  
}

// Recursive
LinkNode* removeDuplicatesFromLinkList(LinkNode* linkNode, LinkNode* first = nullptr) {
    if (linkNode && linkNode->next) {
        if (linkNode->number == linkNode->next->number) {
            LinkNode* temp = linkNode->next->next;
            delete linkNode->next;
            linkNode->next = temp;

            return removeDuplicatesFromLinkList(linkNode, first == nullptr ? linkNode : first);
        }

        return removeDuplicatesFromLinkList(linkNode->next, first == nullptr ? linkNode : first);
    }
    return first;
}
```

## Reverse Link List using an extra Array

```cpp
int reverseList(LinkList* linkList, int*& arr, const int& elementsSize=0) {  
    if (linkList) {  
        const int finalElementsSize = reverseList(
	        linkList->next, arr, elementsSize + 1
		);  
		
        arr[elementsSize] = linkList->number;  
        return finalElementsSize;  
    }    
    
    arr = new int[elementsSize];  
    return elementsSize;  
}  
  
LinkList* reverseLinkedListUsingArray(LinkList* linkedList) {  
    int* arrayFromLinkedList;  
    const int numberOfElements = reverseList(linkedList, arrayFromLinkedList);  
    LinkList* first = linkedList;  
  
    for (int i = 0; i < numberOfElements; ++i) {  
        linkedList->number = *(arrayFromLinkedList + numberOfElements - 1 - i);  
        linkedList = linkedList->next;  
    }  
    
    delete[] arrayFromLinkedList;  
    return first;  
}
```

## Reverse Link List using while loop

```cpp
void reverseLinkList(LinkList*& linkList) {  
    LinkList* current = linkList;  
  
    LinkList* previous = nullptr; 
    // previous: node that comes before current (initially null)  
  
    LinkList* previousPrevious = nullptr; 
    // previousPrevious: node that comes before previous (used to relink)  
  
    while (current != nullptr) {  
        previousPrevious = previous;  
  
        previous = current; // Move previous forward to current  
  
        current = current->next; // Advance current to the next node  
  
        previous->next = previousPrevious;  
    }  
    
    linkList = previous;  
}
```

## Example

linkedList = [1 -> 2 -> 3 -> nullptr];

---

previousPrevious = nullptr;
previous = 1;
current = 2;

previous->next = previousPrevious; // 1->nullptr;

---

previousPrevious = 1;
previous = 2;
current = 3;

previous->next = previousPrevious; // 2->(1->nullptr);

---

previousPrevious = 2;
previous = 3;
current = nullptr;

previous->next = previousPrevious; //3->(2->(1->nullptr));

___

return previous; 

___
## Reverse Link List using Recursion

```cpp
LinkNode* recursiveReverseLinkList(LinkNode* linkNode, LinkNode* previous=nullptr) {
    if (linkNode) {
        LinkNode* newHead = recursiveReverseLinkList(linkNode->next, linkNode);

        linkNode->next = previous;

        return newHead;
    }

    return previous; // reference to the last element (new head)
}
```

## Concatenate two Link Lists

`[1, 2, 3] [2, 3, 5] => [1, 2, 3, 2, 3, 5]`

```cpp
// Iterative
LinkList* concatenateTwoLinkLists(LinkList* firstLinkList, LinkList* secondLinkList) {  
    LinkList* first = firstLinkList;  
  
    while (firstLinkList->next)  
        firstLinkList = firstLinkList->next;  
        
    firstLinkList->next = secondLinkList;  
  
    return first;  
}

// Recursive
LinkNode* concatenateTwoLinkLists(LinkNode* first, LinkNode*second, LinkNode* head = nullptr) {
    if (first) {
        if (first->next)
            return concatenateTwoLinkLists(first->next, second, head == nullptr ? first : head);

        first->next = second;
        return head;
    }
    return nullptr;
}
```

## Merge two Link Lists

```cpp
// Iterative
LinkList *mergeLinkLists(LinkList *first, LinkList *second) {  
    if (!first)  
        return second;  
        
    if (!second)  
        return first;  
  
    LinkList *newHead = nullptr;  
  
    if (first->number <= second->number) {  
        newHead = first;  
        first = first->next;  
  
    } else {  
        newHead = second;  
        second = second->next;  
    }  
    
    LinkList *current = newHead;  
  
    while (first || second) {  
        const int firstLinkListValue = 
	        first != nullptr ? first->number : INT_MAX;  
        const int secondLinkListValue = 
	        second != nullptr ? second->number : INT_MAX;  
  
        if (firstLinkListValue <= secondLinkListValue) {  
            current->next = first;  
            first = first->next;  
  
        } else {  
            current->next = second;  
            second = second->next;  
        }        
        
        current = current->next;  
    }  
    
    // current->next = nullptr; // Either way the last element points to nullptr  
  
    return newHead;  
}

// Recursive
LinkNode* mergeTwoLinkLists(LinkNode* first, LinkNode* second, LinkNode* newHead = nullptr, LinkNode* previous = nullptr) {
    if (first && second) {
        if (first->number < second->number) {
            LinkNode* newElement = new LinkNode{first->number, nullptr};

            if (previous)
                previous->next = newElement;

            return mergeTwoLinkLists(
                first->next,
                second,
                newHead == nullptr ? newElement : newHead,
                newElement
            );

        } else {
            LinkNode* newElement = new LinkNode{second->number, nullptr};

            if (previous)
                previous->next = newElement;

            return mergeTwoLinkLists(
                first,
                second->next,
                newHead == nullptr ? newElement : newHead,
                newElement
            );
        }

    } else if (first) {
        LinkNode* newElement = new LinkNode{first->number, nullptr};

        if (previous)
            previous->next = newElement;

        return mergeTwoLinkLists(
            first->next,
            second,
            newHead == nullptr ? newElement : newHead,
            newElement
        );

    } else if (second) {
        LinkNode* newElement = new LinkNode{second->number, nullptr};

        if (previous)
            previous->next = newElement;

        return mergeTwoLinkLists(
            first,
            second->next,
            newHead == nullptr ? newElement : newHead,
            newElement
        );
    }

    return newHead;
}
```

## Link List has LOOP

```cpp
bool singularLinkListHasLoop(LinkNode* head) {
    LinkNode* slow = head;
    LinkNode* fast = head;

    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;

        if (slow == fast)
            return true;
    }

    return false;
}
```

___
# Circular Linked List
#### (1 -> 2 -> 3 -> 4 -> 5 -> 1)

## Create and Delete Circular Link List

```cpp
// Iterative
LinkList* generateCircularLinkList(const int* array, const int& arraySize) {  
    LinkList* head = new LinkList{*array, nullptr};  
  
    LinkList* previous = head;  
  
    for (int i = 1; i < arraySize; ++i) {  
        LinkList* current = new LinkList{*(array + i), nullptr};  
        previous->next = current;  
        previous = current;  
    }  
    
    previous->next = head;  
    
    return head;  
}  

// Recursive
LinkNode* generateCircularLinkedList(int* array, const int& arraySize, const int& currentIndex = 0, LinkNode* first = nullptr) {
    if (currentIndex < arraySize) {
        LinkNode* current = new LinkNode{*(array + currentIndex), nullptr};

        LinkNode* next = generateCircularLinkedList(array, arraySize, currentIndex + 1, first == nullptr ? current : first);
        current->next = next;
        return current;
    }
    return first;
}

// Iterative
void deleteCircularLinkList(LinkList* linkList, LinkList* head = nullptr) {  
    if (linkList != head && linkList != nullptr) {  
        std::cout << linkList->number << ' ';  
        
        deleteCircularLinkList(linkList->next, head == nullptr ? linkList : head);  
        delete linkList;  
    }
}

// Recursive
void deleteCircularLinkedList(LinkNode* linkNode, LinkNode* first = nullptr) {
    if (linkNode && linkNode != first) {
        deleteCircularLinkedList(linkNode->next, first == nullptr ? linkNode : first);
        // std::cout << "Deleting Node with element: " << linkNode->number << '\n';
        delete linkNode;
    }
}
```

## Insert in a circular list

```cpp
// Iterative
LinkList* insertInCircularLinkedList(
	LinkList* linkList, const int& insertIndex, const int& insertElement
) {  
    if (linkList == nullptr)  
        return nullptr;  
  
    if (insertIndex == 0) { // Moving the first value to a new node
        LinkList* moveFirst = new LinkList{linkList->number, linkList->next};  
        linkList->next = moveFirst;  
        linkList->number = insertElement; // Changing the value of the first node
        return linkList;  
    }  
    
    LinkList* head = linkList;  
    LinkList* previous = nullptr;  
    int index{};  
  
    while (linkList != head || index == 0) {  
        ++index;  
        previous = linkList;  
        linkList = linkList->next;  
  
        if (index == insertIndex) {  
            LinkList* newLinkNode = new LinkList{insertElement, linkList};  
            previous->next = newLinkNode;  
          
			return newLinkNode;  
        }    
	}  
    return nullptr;  
}

// Recursive
LinkNode* insertInCircularLinkedList(LinkNode* linkNode, const int& insertIndex, const int& insertElement, const int& currentIndex = 0, LinkNode* previous = nullptr, LinkNode* first = nullptr) {  
    if (linkNode) {  
        if (currentIndex == insertIndex) {  
            LinkNode* newElement = new LinkNode{insertElement, linkNode};  
  
            if (!previous) { // Insert at the beginning  
                LinkNode* last = linkNode;  
                while (last->next != linkNode)  
                    last = last->next;  
                last->next = newElement;  
                return newElement;  
            }            previous->next = newElement;  
            return first;  
        } 
         
        return insertInCircularLinkedList(  
            linkNode->next,  
            insertIndex,  
            insertElement,  
            currentIndex + 1,  
            linkNode,  
            first == nullptr ? linkNode : first  
        );  
    }  
    
    LinkNode* newElement = new LinkNode{insertElement, nullptr}; // Insert in empty Circular Linked List  
    newElement->next = newElement;  
    return newElement;  
}
```

## Delete from a circular Linked List

```cpp
// Iterative
LinkNode* deleteFromCircularLinkLIst(LinkList* linkList, const int& deleteIndex) { 
    if (linkList == nullptr)  
        return nullptr;  
  
    LinkList* head = linkList;  
  
    if (deleteIndex == 0) {  
        linkList = linkList->next;  
      
		while (linkList->next != head)  
            linkList = linkList->next;  
        linkList->next = head->next;  
        
        delete head;  
        
        return linkList->next;  
    }  
    
    LinkList* previous = nullptr;  
    unsigned int index{};  
  
    while (true) {  
        ++index;  
        previous = linkList;  
        linkList = linkList->next;  
     
		if (!(linkList != head || index == 0))  
            break;  
  
        if (index == deleteIndex) {  
            previous->next = linkList->next;  
            
            delete linkList;  
            return head;  
        }    
	}  
    return nullptr;  
}

// Recursive
LinkNode* deleteFromCircularLinkedList(LinkNode* linkNode, const int& deleteIndex, const int& currentIndex = 0, LinkNode* previous = nullptr, LinkNode* first = nullptr) {
    if (linkNode && linkNode != first) {
        if (currentIndex == deleteIndex) {
            if (!previous) {
                if (linkNode->next == linkNode) {
                    delete linkNode;
                    return nullptr;
                }

                LinkNode* tail = linkNode;
                while (tail->next != linkNode)
                    tail = tail->next;
                tail->next = linkNode->next;

                delete linkNode;
                return tail->next;
            }

            previous->next = linkNode->next;
            delete linkNode;
            return first;
        }

        return deleteFromCircularLinkedList(
            linkNode->next,
            deleteIndex,
            currentIndex + 1,
            linkNode,
            first == nullptr ? linkNode : first
        );
    }

    return nullptr;
}
```

___
# Double Linked List
#### (1 <-> 2 <-> 3 <-> 4 <-> 5)

## Create, Delete Double Linked List

```cpp
// Iterative
DoubleLinkedList* generateDoubleLinkedList(const int* array, const int& arraySize) {  
    if (!arraySize)  
        return nullptr;  
  
    DoubleLinkedList* head = new DoubleLinkedList{*array, nullptr, nullptr};  
    DoubleLinkedList* previous = head;  
  
    for (int i = 1; i < arraySize; ++i) {  
        DoubleLinkedList* current = new DoubleLinkedList{
	        *(array + i), previous, nullptr
		};  
        previous->next = current;  
        previous = current;  
    }  
    
    return head;  
}  

struct DoubleLinkNode {
    int number;
    DoubleLinkNode* previous;
    DoubleLinkNode* next;
};

// Recursive
DoubleLinkNode* generateDoubleLinkList(const int* array, const int& arraySize, const int& currentIndex = 0, DoubleLinkNode* previous = nullptr) {
    if (currentIndex < arraySize) {
        DoubleLinkNode* current = new DoubleLinkNode{*(array + currentIndex), previous, nullptr};
        DoubleLinkNode* next = generateDoubleLinkList(array, arraySize, currentIndex + 1, current);
        current->next = next;
        return current;
    }

    return nullptr;
}

// Recursive Deletion
void deleteDoubleLinkList(DoubleLinkNode* doubleLinkNode) {  
    if (doubleLinkNode) {  
        deleteDoubleLinkList(doubleLinkNode->next);  
        std::cout << "Deleting: " << doubleLinkNode->number << '\n';  
        delete doubleLinkNode;  
    }
}
```

## Insert in Double Link List

```cpp
// Iterative
DoubleLinkedList* insertInDoubleLinkList(
	DoubleLinkedList* doubleLinkedList, const int& insertIndex, const int& insertElement
) {  
    if (!doubleLinkedList)  
        return nullptr;  
  
    if (insertIndex == 0) {  
        DoubleLinkedList* newHead = new DoubleLinkedList{
	        insertElement, nullptr, doubleLinkedList
		};  
        doubleLinkedList->previous = newHead;  
        return newHead;  
    }  
    
    DoubleLinkedList* head = doubleLinkedList;  
    int index = 0;  
  
    while (doubleLinkedList->next) {  
        ++index;  
        doubleLinkedList = doubleLinkedList->next;  
  
        if (index == insertIndex) {  
            DoubleLinkedList* newElement = new DoubleLinkedList{
	            insertElement, doubleLinkedList->previous, doubleLinkedList
			};  
            doubleLinkedList->previous->next = newElement;  
            doubleLinkedList->previous = newElement;  
            return head;  
        }    
	}  
    
    if (index + 1 == insertIndex) {  
        DoubleLinkedList* newElement = new DoubleLinkedList{
	        insertElement, doubleLinkedList, nullptr
		};  
        doubleLinkedList->next = newElement;  
        return head;  
    }  
    
    return nullptr;  
}

// Recursive
DoubleLinkNode *insertInDoubleLinkList(DoubleLinkNode *doubleLinkNode, const int &insertIndex, const int &insertElement, const int &currentIndex = 0, DoubleLinkNode *first = nullptr) {  
    if (doubleLinkNode) {  
        if (currentIndex == insertIndex) {  
            DoubleLinkNode *newElement = new DoubleLinkNode{insertElement, doubleLinkNode->previous, doubleLinkNode};  
  
            if (!doubleLinkNode->previous) { // Insert at the beginning  
                doubleLinkNode->previous = newElement;  
                return newElement;  
            }            doubleLinkNode->previous->next = newElement;  
            doubleLinkNode->previous = newElement;  
  
            return first;  
        }  
        if (doubleLinkNode->next == nullptr && currentIndex + 1 == insertIndex) { // Insert at the end  
            doubleLinkNode->next = new DoubleLinkNode{insertElement, doubleLinkNode, nullptr};  
            return first;  
        }  
        return insertInDoubleLinkList(  
            doubleLinkNode->next,  
            insertIndex,  
            insertElement,  
            currentIndex + 1,  
            first == nullptr ? doubleLinkNode : first  
        );  
    }  
    return new DoubleLinkNode{insertElement, nullptr, nullptr};  
}
```

## Delete from Double Link List

```cpp
// Iterative
DoubleLinkedList* deleteFromDoubleLinkedList(
	DoubleLinkedList* doubleLinkedList, const int& deleteIndex
) {  
    if (!doubleLinkedList)  
        return nullptr;  
  
    if (deleteIndex == 0) {  
        DoubleLinkedList* newHead = doubleLinkedList->next;  
        newHead->previous = nullptr;  
        delete doubleLinkedList;  
        return newHead;  
    }    
    
    DoubleLinkedList* head = doubleLinkedList;  
  
    int index = 1;  
  
    while (doubleLinkedList->next && index < deleteIndex) {  
        doubleLinkedList = doubleLinkedList->next;  
  
        if (index == deleteIndex) {  
            doubleLinkedList->previous->next = doubleLinkedList->next;  
            doubleLinkedList->next->previous = doubleLinkedList->previous;  
            delete doubleLinkedList;  
            return head;  
        }        ++index;  
    }  
    
    if (index == deleteIndex) {  
        doubleLinkedList->previous->next = nullptr;  
        delete doubleLinkedList;  
        return head;  
    }  
    
    return nullptr;  
}

// Recursive
DoubleLinkNode* deleteFromDoubleLinkList(DoubleLinkNode* doubleLinkNode, const int& deleteIndex, const int& currentIndex = 0, DoubleLinkNode* first = nullptr) {  
    if (doubleLinkNode) {  
         if (currentIndex == deleteIndex) {  
             if (!doubleLinkNode->previous) { // Delete the first element  
                 DoubleLinkNode* newFirst = doubleLinkNode->next;  
                 if (!newFirst) { // Deleting a single element  
                     delete doubleLinkNode;  
                     return nullptr;  
                 }                 newFirst->previous = nullptr;  
                 delete doubleLinkNode;  
                 return newFirst;  
             }  
             doubleLinkNode->previous->next = doubleLinkNode->next;  
             if (doubleLinkNode->next)  
                 doubleLinkNode->next->previous = doubleLinkNode->previous;  
  
             delete doubleLinkNode;  
             return first;  
         }  
        return deleteFromDoubleLinkList(  
            doubleLinkNode->next,  
            deleteIndex,  
            currentIndex + 1,  
            first == nullptr ? doubleLinkNode : first  
        );  
    }  
    return first; // Delete from an empty list || Delete invalid index  
}
```

## Reverse Double Linked List

```cpp
// using while loop
DoubleLinkedList* reverseDoubleLinkedList(DoubleLinkedList* doubleLinkedList) {  
	 if (!doubleLinkedList)  
	    return nullptr;  
  
	while (true) {  
        DoubleLinkedList* newPrevious = doubleLinkedList->next;  
        DoubleLinkedList* newNext = doubleLinkedList->previous;  
  
        doubleLinkedList->next = newNext;  
        doubleLinkedList->previous = newPrevious;  
  
        if (!newPrevious)  
            break;  
            
        doubleLinkedList = newPrevious;  
    }  
    
    return doubleLinkedList;  
}

DoubleLinkedList* recursiveReverseDoubleLinkedList(
	DoubleLinkedList* doubleLinkedList, DoubleLinkedList* previous=nullptr
) {  
    if (doubleLinkedList->next) {  
        DoubleLinkedList* newHead = recursiveReverseDoubleLinkedList(doubleLinkedList->next, doubleLinkedList);  
        DoubleLinkedList* prev = doubleLinkedList->previous;  
        doubleLinkedList->previous = doubleLinkedList->next;  
        doubleLinkedList->next = prev;  
  
        return newHead;  
    }    
    
    doubleLinkedList->previous = nullptr;  
    doubleLinkedList->next = previous;  
    
    return doubleLinkedList;  
}
```

___
# Circular Double Link List
#### (1 <-> 2 <-> 3 <-> 4 <-> 5 <-> 1)

## Create Circular Double Link List

```cpp
// Iterative
DoubleLinkedList* createCircularDoubleLinkedList(
	const int* array, const int& arraySize
) {  
    if (!array || !arraySize)  
        return nullptr;  
  
    DoubleLinkedList* head = new DoubleLinkedList{*array, nullptr, nullptr};  
    DoubleLinkedList* previous = head;  
  
    for (int i = 1; i < arraySize; ++i) {  
        DoubleLinkedList* current = new DoubleLinkedList{*(array + i), previous, nullptr};  
        previous->next = current;  
        previous = current;  
    }    previous->next = head;  
  
	head->previous = previous;  
  
    return head;  
}  
  
struct DoubleLinkNode {
    int number;
    DoubleLinkNode *previous;
    DoubleLinkNode *next;
};

// Recursive
DoubleLinkNode* createDoubleCircularLinkList(const int* array, const int& arraySize, const int& currentIndex = 0, DoubleLinkNode* previous = nullptr, DoubleLinkNode* first = nullptr) {
    if (currentIndex < arraySize) {
        DoubleLinkNode* current = new DoubleLinkNode{*(array + currentIndex), previous, nullptr};
        DoubleLinkNode* next = createDoubleCircularLinkList(
            array,
            arraySize,
            currentIndex + 1,
            current,
            first == nullptr ? current : first
        );
        current->next = next;
        return current;
    }

    if (first)
        first->previous = previous;

    return first;
}
```

## Insert in Circular Double Link List

```cpp
// Iterative
DoubleLinkedList *insertInCircularDoubleLinkedList(
	DoubleLinkedList *&doubleLinkedList, const int &insertIndex, 
	const int &insertElement
) {  
    if (!doubleLinkedList || insertIndex < 0)  
        return nullptr;  
  
    if (insertIndex == 0) {  
        DoubleLinkedList* newHead = new DoubleLinkedList{
	        insertElement, doubleLinkedList->previous, doubleLinkedList
		};  
        doubleLinkedList->previous->next = newHead;  
        doubleLinkedList->previous = newHead;  
        doubleLinkedList = newHead;  
        
        return doubleLinkedList;  
    }  
    
    DoubleLinkedList *head = doubleLinkedList;  
    int index = 1;  
    doubleLinkedList = doubleLinkedList->next;  
  
    while (doubleLinkedList != head) {  
        if (index == insertIndex) {  
            DoubleLinkedList *newNode = new DoubleLinkedList{  
                insertElement, doubleLinkedList->previous, doubleLinkedList  
            };  
            doubleLinkedList->previous->next = newNode;  
            doubleLinkedList->previous = newNode;  
            doubleLinkedList = head;  
            return head;  
        }  
        doubleLinkedList = doubleLinkedList->next;  
        ++index;  
    }  
    
    if (index == insertIndex) {  
        DoubleLinkedList* newTail = new DoubleLinkedList{
	        insertElement, doubleLinkedList->previous, doubleLinkedList
		};  
        doubleLinkedList->previous->next = newTail;  
        doubleLinkedList->previous = newTail;  
        return head;  
    }  
    
    return nullptr;  
}

// Recursive
DoubleLinkNode* insertInDoubleCircularLinkList(DoubleLinkNode* doubleLinkNode, const int& insertIndex, const int& insertElement, const int& currentIndex = 0, DoubleLinkNode* first = nullptr) {
    if (doubleLinkNode) {
        if (currentIndex == insertIndex) {
            DoubleLinkNode* newElement = new DoubleLinkNode{insertElement, doubleLinkNode->previous, doubleLinkNode};
            doubleLinkNode->previous->next = newElement;
            doubleLinkNode->previous = newElement;
            if (insertIndex == 0)
                return newElement;
            return first;
        }

        return insertInDoubleCircularLinkList(
            doubleLinkNode->next,
            insertIndex,
            insertElement,
            currentIndex + 1,
            first == nullptr ? doubleLinkNode : first
        );
    }

    DoubleLinkNode* newSingleElement = new DoubleLinkNode{insertElement, nullptr, nullptr};
    newSingleElement->previous = newSingleElement;
    newSingleElement->next = newSingleElement;
    return newSingleElement;
}
```

## Delete from Circular Double Link List

```cpp
// Iterative
DoubleLinkedList* deleteFromCircularDoubleLinkedList(
	DoubleLinkedList*& doubleLinkedList, const int& deleteIndex
) {  
    if (!doubleLinkedList || deleteIndex < 0)  
        return nullptr;  
  
	if (deleteIndex == 0) {  
	    if (doubleLinkedList->next == doubleLinkedList) {  
	        delete doubleLinkedList;  
	        doubleLinkedList = nullptr;  
	        return nullptr;  
	    }    
	    
	    doubleLinkedList->previous->next = doubleLinkedList->next;  
	    doubleLinkedList->next->previous = doubleLinkedList->previous;  
	    
	    DoubleLinkedList* newHead = doubleLinkedList->next;  
	    delete doubleLinkedList;  
	    doubleLinkedList = newHead;  
	   
		return doubleLinkedList;  
	}
    
    DoubleLinkedList* head = doubleLinkedList;  
    int index = 1;  
    doubleLinkedList = doubleLinkedList->next;  
  
    while (doubleLinkedList != head) {  
        if (index == deleteIndex) {  
            doubleLinkedList->previous->next = doubleLinkedList->next;  
            doubleLinkedList->next->previous = doubleLinkedList->previous;  
            delete doubleLinkedList;  
            doubleLinkedList = head;  
            return doubleLinkedList;  
        }        doubleLinkedList = doubleLinkedList->next;  
        ++index;  
    }  
    
    return nullptr;  
}

// Recursive
DoubleLinkNode* deleteFromDoubleCircularLinkList(DoubleLinkNode* doubleLinkNode, const int& deleteIndex, const int& currentIndex = 0, DoubleLinkNode* first = nullptr) {  
    if (doubleLinkNode && doubleLinkNode != first) {  
        if (currentIndex == deleteIndex) {  
            if (doubleLinkNode->previous == doubleLinkNode && doubleLinkNode->next == doubleLinkNode) {  
                delete doubleLinkNode;  
                return nullptr;  
            }            if (currentIndex == 0) {  
                DoubleLinkNode* newFirst = doubleLinkNode->next;  
                doubleLinkNode->previous->next = newFirst;  
                newFirst->previous = doubleLinkNode->previous;  
                delete doubleLinkNode;  
                return newFirst;  
            }  
            doubleLinkNode->previous->next = doubleLinkNode->next;  
            doubleLinkNode->next->previous = doubleLinkNode->previous;  
            delete doubleLinkNode;  
            return first;  
        }  
        return deleteFromDoubleCircularLinkList(  
            doubleLinkNode->next,  
            deleteIndex,  
            currentIndex + 1,  
            first == nullptr ? doubleLinkNode : first  
        );  
    }  
    return first;  
}
```