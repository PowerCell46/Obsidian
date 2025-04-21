
### Loading factor = number of elements / hash map size

## Hashmap using Chaining

```cpp
class Hashmap {  
    struct HashmapNode {  
        int key;  
        int value;  
        HashmapNode *next;  
    };  
    static constexpr int DEFAULT_HASHMAP_SIZE = 50;  
    int size;  
    HashmapNode **hashmapNodesArray;  
  
public:  
    Hashmap()  
        : size(DEFAULT_HASHMAP_SIZE), hashmapNodesArray(new HashmapNode *[DEFAULT_HASHMAP_SIZE]{}) {}  
  
    Hashmap(const int &size)  
        : size(std::max(size, 1)), hashmapNodesArray(new HashmapNode *[std::max(size, 1)]) {}  
  
  
    void insert(const int &key, const int &value) {  
        const int hashIndex = key % this->size;  
  
        HashmapNode *hashmapNode = *(this->hashmapNodesArray + hashIndex);  
  
        if (!hashmapNode)  
            *(this->hashmapNodesArray + hashIndex) = new HashmapNode{key, value, nullptr};  
  
        else {  
            while (hashmapNode) {  
                if (key < hashmapNode->key) {  
                    HashmapNode *newHashmapNode = new HashmapNode{key, value, hashmapNode};  
                    *(this->hashmapNodesArray + hashIndex) = newHashmapNode;  
                    break;  
                }  
                if (key == hashmapNode->key) {  
                    hashmapNode->value = value;  
                    break;  
                }  
                if (!hashmapNode->next) {  
                    hashmapNode->next = new HashmapNode{key, value, nullptr};  
                    break;  
                }  
                hashmapNode = hashmapNode->next;  
            }        }    }  
    
    int find(const int &key) const {  
        const int hashIndex = key % this->size;  
  
        HashmapNode *hashmapNode = *(this->hashmapNodesArray + hashIndex);  
  
        while (hashmapNode) {  
            if (hashmapNode->key == key)  
                return hashmapNode->value;  
  
            hashmapNode = hashmapNode->next;  
        }  
        return -1;  
    }  
    
    bool del(const int& key) {  
        const int hashIndex = key % this->size;  
  
        HashmapNode* hashmapNode = *(this->hashmapNodesArray + hashIndex);  
  
        if (!hashmapNode)  
            return false;  
  
        HashmapNode* previous = nullptr;  
        while (hashmapNode) {  
            if (hashmapNode->key == key) {  
                if (previous) {  
                    previous->next = hashmapNode->next;  
                    delete hashmapNode;  
                    return true;  
                }  
                HashmapNode* next = hashmapNode->next;  
                delete hashmapNode;  
                *(this->hashmapNodesArray + hashIndex) = next;  
                return true;  
            }  
            previous = hashmapNode;  
            hashmapNode = hashmapNode->next;  
        }  
        return false;  
    }  
    ~Hashmap() {  
        for (int i = 0; i < this->size; ++i) {  
            HashmapNode *hashmapNode = *(this->hashmapNodesArray + i);  
  
            while (hashmapNode) {  
                HashmapNode *next = hashmapNode->next;  
                delete hashmapNode;  
                hashmapNode = next;  
            }        }  
        delete[] this->hashmapNodesArray;  
  
    }  
    friend std::ostream& operator<<(std::ostream& os, const Hashmap& hashmap);  
};  
  
std::ostream& operator<<(std::ostream& os, const Hashmap& hashmap) {  
    os << "HashMap: \n";  
  
    for (int i = 0; i < hashmap.size; ++i) {  
        auto hashMapNode = *(hashmap.hashmapNodesArray + i);  
        while (hashMapNode) {  
            os << "  [" << hashMapNode->key << "] -> " << hashMapNode->value << '\n';  
            hashMapNode = hashMapNode->next;  
        }    }  
    return os;  
}  
  
  
int main() {  
    Hashmap hashmap;  
  
    hashmap.insert(1, 213);  
  
    hashmap.insert(10, 100);  
    hashmap.insert(60, 200);  // 10 and 60 will collide if size = 50  
    hashmap.insert(110, 300); // same index again  
  
    // hashmap.del(60);  // delete from the middle of the chain  
    std::cout << hashmap;  
  
    return 0;  
}
```

## Linear Probing

- ✅ **Load factor** should be ≤ 0.5 → prevents too many collisions and keeps lookup fast.
- ❌ **Direct deletion** can break lookups when using collision resolution (e.g. open addressing).
- ✅ **Better deletion:** use an `isDeleted` flag instead of removing nodes.  
    This way, collision chains stay intact and future lookups won’t fail. 

```cpp
class Hashmap {  
    static const int DEFAULT_HASHMAP_SIZE = 50;  
    int size;  
    int* keys;  
    int* values;  
    int* deleted;  
public:  
    Hashmap() :  
        size(DEFAULT_HASHMAP_SIZE), keys(new int[DEFAULT_HASHMAP_SIZE]{}), values(new int[DEFAULT_HASHMAP_SIZE]{}),  
        deleted(new int[DEFAULT_HASHMAP_SIZE]{}) {  
        for (int i = 0; i < DEFAULT_HASHMAP_SIZE; ++i)  
            *(this->keys + i) = INT_MIN;  
    }  
    
    Hashmap(const int& size) :  
        size(size), keys(new int[size]{}), values(new int[size]{}),  
        deleted(new int[size]{}) {  
        for (int i = 0; i < size; ++i)  
            *(this->keys + i) = INT_MIN;  
    }  
    
    bool insert(const int& key, const int& value) {  
        int hashIndex = key % this->size;  
  
        while (hashIndex < this->size) {  
            if (*(this->keys + hashIndex) == INT_MIN || *(this->deleted + hashIndex) == 1) {  
                *(this->keys + hashIndex) = key;  
                *(this->values + hashIndex) = value;  
                *(this->deleted + hashIndex) = 0;  
                return true;  
            }            ++hashIndex;  
        }  
        return false;  
    }  
    
    int find(const int& key) {  
        int hashIndex = key % this->size;  
  
        while (hashIndex < this->size) {  
            if (*(this->keys + hashIndex) == key) {  
                if (*(this->deleted + hashIndex) == 0)  
                    return *(this->values + hashIndex);  
                return -1;  
            }  
            if (*(this->keys + hashIndex) == INT_MIN)  
                return -1;  
  
            ++hashIndex;  
        }  
        return -1;  
    }  
    
    bool del(const int& key) {  
        int hashIndex = key % this->size;  
  
        while (hashIndex < this->size) {  
            if (*(this->keys + hashIndex) == key) {  
                if (*(this->deleted + hashIndex) == 0) {  
                    *(this->deleted + hashIndex) = 1;  
                    return true;  
                }                return false;  
            }  
            if (*(this->keys + hashIndex) == INT_MIN)  
                return false;  
  
            ++hashIndex;  
        }  
        return false;  
    }  
    
    ~Hashmap() {  
        delete[] this->keys;  
        delete[] this->values;  
        delete[] this->deleted;  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const Hashmap& hashmap);  
};  
  
  
std::ostream& operator<<(std::ostream& os, const Hashmap& hashmap) {  
    os << "Hash map:\n";  
    for (int i = 0; i < hashmap.size; ++i) {  
        if (const int currentKey = *(hashmap.keys + i); currentKey != INT_MIN && *(hashmap.deleted + i) == 0)  
            os << "  [" << currentKey << "] -> " << *(hashmap.values + i) << '\n';  
    }    return os;  
}  
  
  
int main() {  
    Hashmap hashmap{10};  
  
    hashmap.insert(10, 1);  
    // hashmap.del(10);  
  
    hashmap.insert(100, 10);  
  
    std::cout << hashmap;  
  
    return 0;  
}
```

## Other Hashing techniques

mod + 1
mid square
folding