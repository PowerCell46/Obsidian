
## No Fucking Idea...

```cpp
struct Node {
    int number;
    int height;
    Node* leftChild;
    Node* rightChild;
};

int getNodeHeight(const Node* node) { // Getter for the height of a node
    return node ? node->height : 0;
}

void updateNodeHeight(Node* node) {
    if (node) // Set the height to the max of (left, right) children
        node->height = std::max(
            getNodeHeight(node->leftChild),
            getNodeHeight(node->rightChild)
        ) + 1;
}

int getBalanceFactor(const Node* node) {
    return node ? getNodeHeight(node->leftChild) - getNodeHeight(node->rightChild) : 0;
}

Node* rotateRight(Node* unbalancedNode) {
    Node* newRoot = unbalancedNode->leftChild;
    Node* orphanSubtree = newRoot->rightChild;

    newRoot->rightChild = unbalancedNode;
    unbalancedNode->leftChild = orphanSubtree;

    updateNodeHeight(unbalancedNode);
    updateNodeHeight(newRoot);

    return newRoot;
}

Node* rotateLeft(Node* unbalancedNode) {
    Node* newRoot = unbalancedNode->rightChild;
    Node* orphanSubtree = newRoot->leftChild;

    newRoot->leftChild = unbalancedNode;
    unbalancedNode->rightChild = orphanSubtree;

    updateNodeHeight(unbalancedNode);
    updateNodeHeight(newRoot);

    return newRoot;
}

Node* insertInBinaryTree(Node* root, const int& insertElement) {
    if (!root)
        return new Node{insertElement, 1, nullptr, nullptr};

    if (insertElement < root->number)
        root->leftChild = insertInBinaryTree(root->leftChild, insertElement);
    else if (insertElement > root->number)
        root->rightChild = insertInBinaryTree(root->rightChild, insertElement);
    else
        return root; // Avoid duplicates

    updateNodeHeight(root);

    const int balance = getBalanceFactor(root);

    // Left-Left case
    if (balance > 1 && insertElement < root->leftChild->number)
        return rotateRight(root);

    // Right-Right case
    if (balance < -1 && insertElement > root->rightChild->number)
        return rotateLeft(root);

    // Left-Right case
    if (balance > 1 && insertElement > root->leftChild->number) {
        root->leftChild = rotateLeft(root->leftChild);
        return rotateRight(root);
    }

    // Right-Left case
    if (balance < -1 && insertElement < root->rightChild->number) {
        root->rightChild = rotateRight(root->rightChild);
        return rotateLeft(root);
    }

    return root;
}

// ---

void displayMermaidBinarySearchTreeNode(const Node* node) {
    static int nullNumber{};

    if (node && (node->leftChild || node->rightChild)) {
        if (node->leftChild)
            std::cout << '\t' << node->number << " --> " << node->leftChild->number << '\n';
        else
            std::cout << '\t' << node->number << " --> X" << ++nullNumber << "[\"X\"]" << '\n';

        if (node->rightChild)
            std::cout << '\t' << node->number << " --> " << node->rightChild->number << '\n';
        else
            std::cout << '\t' << node->number << " --> X" << ++nullNumber << "[\"X\"]" << '\n';

        displayMermaidBinarySearchTreeNode(node->leftChild);
        displayMermaidBinarySearchTreeNode(node->rightChild);
    }
}

void generateMermaidBinarySearchTree(const Node* node) {
    if (!node) {
        std::cout << "Empty Tree.\n";
        return;
    }
    std::cout << "```mermaid\n" << "graph TD\n\t" << node->number << '\n';

    displayMermaidBinarySearchTreeNode(node);

    std::cout << "```";
}

int main() {
    Node* root = nullptr;

    int values[] = {30, 20, 40, 10, 25, 35, 50, 5, 45};
    for (int val : values)
        root = insertInBinaryTree(root, val);

    generateMermaidBinarySearchTree(root);
    std::cout << "\n";

    return 0;
}

```