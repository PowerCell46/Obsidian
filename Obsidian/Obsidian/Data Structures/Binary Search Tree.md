
## Implementation

```cpp
Node* binarySearchTree(Node* node, const int& searchElement) {    
	if (node == nullptr)    
		return nullptr;    
	    
	if (node->data == searchElement)    
		return node;    
	    
	if (node->data < searchElement)    
		return binarySearchTree(node->rightChild, searchElement);    
	
	// if (node->data > searchElement)    
	return binarySearchTree(node->leftChild, searchElement);  
}

Node* binarySearchTree(Node* node, const int& searchElement) {  
    while (node) {  
        if (node->data == searchElement)  
            return node;  
        if (node->data > searchElement)  
            node = node->leftChild;  
        else  
            node = node->rightChild;  
    }  
    
    return nullptr;  
}
```

## Insert Element in Binary Search Tree

```cpp
Node* addElementToBinaryTree(
	Node* currentNode, const int& insertElement, Node* previousNode=nullptr
) {  
    if (currentNode == nullptr) {  
        if (previousNode == nullptr)  
            return new Node{nullptr, insertElement, nullptr};  
  
        Node* newNode = new Node{nullptr, insertElement, nullptr};  
  
        if (previousNode->data < insertElement)  
            previousNode->rightChild = newNode;  
        else  
            previousNode->leftChild = newNode;  
  
        return newNode;  
    }  
    
    if (currentNode->data == insertElement)  
        return currentNode;  
  
    if (currentNode->data > insertElement)  
        return addElementToBinaryTree(currentNode->leftChild, insertElement, currentNode);  
  
    // if (currentNode->data < insertElement)  
    return addElementToBinaryTree(currentNode->rightChild, insertElement, currentNode);  
}

// Iterative approach
Node* addElementToBinaryTree(Node* currentNode, const int& insertElement) {  
    Node* previous = nullptr;  
  
    while (true) {  
        if (currentNode == nullptr) {  
            if (previous == nullptr)  
                return new Node{nullptr, insertElement, nullptr};  
  
            Node* newNode = new Node{nullptr, insertElement, nullptr};  
  
            if (previous->data > insertElement)  
                previous->leftChild = newNode;  
            else  
                previous->rightChild = newNode;  
  
            return newNode;  
        }  
        
        if (currentNode->data == insertElement)  
            return currentNode;  
  
        previous = currentNode;  
  
        if (currentNode->data > insertElement)  
            currentNode = currentNode->leftChild;  
        else  
            currentNode = currentNode->rightChild;  
    }
}
```

## Create binary Tree

```cpp
class Node {  
public:  
    Node *leftChild;  
    int data;  
    Node *rightChild;  
};  
  
Node* createBinarySearchTree(const int* array, const int& arraySize) {  
    if (array == nullptr || arraySize <= 0)  
        return nullptr;  
  
    Node* root = new Node{nullptr, *array, nullptr};  
  
    for (int i = 1; i < arraySize; ++i) {  
        const int currentValue = *(array + i);  
        Node* currentNode = root;  
        Node* previousNode = nullptr;  
  
        while (true) {  
            if (currentNode == nullptr) {  
                Node* newNode = new Node{nullptr, currentValue, nullptr};  
  
                if (previousNode->data > currentValue)  
                    previousNode->leftChild = newNode;  
                else  
                    previousNode->rightChild = newNode;  
  
                break;  
            }  
            
            if (currentNode->data == currentValue)  
                break;  
  
            if (currentNode->data > currentValue) {  
                previousNode = currentNode;  
                currentNode = currentNode->leftChild;  
  
            } else {  
                previousNode = currentNode;  
                currentNode = currentNode->rightChild;  
            }        
		}    
	}
	  
    return root;  
}  
  
void doubleArraySize(int*& array, int& arraySize) {  
    int* newArray = new int[arraySize * 2]{};  
  
    for (int i = 0; i < arraySize; ++i)  
        *(newArray + i) = *(array + i);  
  
    delete[] array;  
    array = newArray;  
    arraySize *= 2;  
}  
  
int main() {  
    std::string line;  
    std::cout << "Enter Tree Elements: ";  
    std::getline(std::cin, line);  
    std::stringstream lineStream{line};  
  
    int dataSize = 20;  
    int* dataArray = new int[dataSize]{};  
  
    int index{};  
    int currentNumber;  
  
    while (lineStream >> currentNumber) {  
        if (index == dataSize)  
            doubleArraySize(dataArray, dataSize);  
  
        *(dataArray + index) = currentNumber;  
        ++index;  
    }  
	    
    Node* root = createBinarySearchTree(dataArray, index);  
  
    delete[] dataArray;  
    return 0;  
}
```
