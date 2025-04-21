# Copy constructor/s

```cpp
struct Body {  
    int height;  
    int weight;  
  
    Body() = default;  
    Body(const int& height, const int&weight) : height(height), weight(weight) {}  
  
    // Body (const Body& other) = default; 
	    // default copy constructor  
    // Body (const Body& other) = delete; 
	    // intentionally delete copy constructor  
    
    Body(const Body& other) : 
	    height(other.height), 
	    weight(other.weight) { // Body bodyCopy(body);  
        std::cout << "Copy constructor was called." << std::endl;  
    }  
    
    // Body& operator=(const Body& other) = default; 
	    // default copy assignment operator  
    // Body& operator=(const Body& other) = delete; 
	    // intentionally delete copy assignment operator  
    Body& operator=(const Body& other) { // copy assignment operator // bodyCopy = body;  
        if (this != &other) {  
            this->height = other.height;  
            this->weight = other.weight;  
        }  
        return *this;  
    }  
    
    ~Body() { // destructor  
        std::cout << "Destructor was called." << std::endl;  
    }};  
  
  
std::ostream& operator<<(std::ostream& os, const Body& b) {  
    return os << "Height: " << b.height << "; Weight: " << b.weight << ".\n";  
}
```


___
### Move Constructor (C++11 and later)

A constructor that transfers ownership of resources from one object to another without copying.


### Conversion Constructor

A constructor that can be used for implicit or explicit type conversion.
Takes a single argument (or more with default values).

### Delegating Constructor (C++11 and later)

A constructor that calls another constructor in the same class to reuse initialization logic.

```cpp
#include <iostream>  
#include <sstream>  
#include <vector>  
  
class Person {  
    std::string name;  
    int age;  
    static int numPeople;  
public:  
    Person() :  
        name(), age() {  
    }  
    Person(const std::string& name, const int& age) :  
        name(name), age(age) {  
        numPeople++;  
    }  
    ~Person() {  
        numPeople--;  
    }  
    Person(const Person& other) {  
        std::cout << "Copy constructor called." << std::endl;  
  
        this->name = std::string("'Copy of ") + other.getName() + std::string("'");  
        this->age = other.getAge();  
  
        numPeople++;  
    }  
    Person& operator=(const Person& other) {  
        if (this == &other)  
            return *this;  
  
        std::cout << "Copy assignment called." << std::endl;  
        numPeople++;  
  
        this->age = other.getAge();  
        this->name = other.getName();  
  
        return *this;  
    }  
  
    std::string getName() const { return this->name; }  
  
    int getAge() const { return this->age; }  
    static int getNumPeople() { return numPeople; }  
};  
  
int Person::numPeople = 0;  
  
std::ostream &operator<<(std::ostream& os, const Person& p) {  
    return os << "Name: " << p.getName() << ", Age: " << p.getAge() << "\n";  
}  
  
  
/*  
class Array {  
    int* data;public:  
    
    Array() : data(new int[10]) {std::cout << "init" << std:: endl;}  
    
    Array& operator=(const Array& other) = delete;    
    
    Array(const Array& other) = delete;  

	~Array() {        
	    delete[] data;        
	    data = nullptr;    
	}
};  
*/  
  
  
template<typename T>  
class Array {  
    int size;  
    T* data;  
  
public:  
    class Iterator {  
        Array& array;  
        int index;  
  
    public:  
        Iterator(Array& array, const int& index) : 
	        array(array), index(index) {}  
  
        bool operator!=(const Iterator& other) const {  
            if (&array != &other.array)  
                return false;  
  
            return index != other.index;  
        }  
        Iterator& operator++() {  
            index++;  
            return *this;  
        }  
        T &operator*() const {  
            return array[index];  
        }    
	};  
    
    Array() : size(0), data(nullptr) {}  
  
    Array(const int& size) : size(size), data(new T[size]{}) {}  
  
    ~Array() { delete[] this->data; }  
  
    Array(const Array<T>& other) : size(other.getSize()) {  
        T* newData = new T[other.getSize()];  
  
        for (int i = 0; i < other.getSize(); ++i)  
            newData[i] = other.data[i];  
  
        this->data = newData;  
    }  
    Iterator begin() const {  
        return Iterator(*this, 0);  
    }  
    Iterator end() const {  
        return Iterator(*this, this->size);  
    }  
    Array<T> &operator=(const Array<T>& other) {  
        if (this == &other)  
            return *this;  
  
        T* newData = new T[other.getSize()];  
  
        for (int i = 0; i < other.getSize(); ++i)  
            newData[i] = other.data[i];  
  
        delete[] this->data;  
        this->data = newData;  
        this->size = other.getSize();  
  
        return *this;  
    };  
  
    int getSize() const { return this->size; }  
  
    T &operator[](const int &index) const {  
        return this->data[index];  
    }  
    void resize(const int &newSize) {  
        if (newSize == 0) {  
            delete[] this->data;  
            this->data = nullptr;  
            this->size = newSize;  
  
        } else {  
            T *newData = new T[newSize]{};  
  
            if (this->data != nullptr)  
                for (int i = 0; i < std::min(newSize, this->size); ++i)  
                    newData[i] = this->data[i];  
  
            delete[] this->data;  
            this->data = newData;  
            this->size = newSize;  
        }    }};  
  
  
void func() {  
    Person p{"Peter", 21};  
  
    Person person;  
    person = p;  
  
    std::vector<Person> people;  
    people.push_back(p);  
}  
  
int main() {  
    func();  
  
    std::cout << Person::getNumPeople() << std::endl;  
  
    return 0;  
}
```

# Custom Iterator

```cpp
class IntContainer {  
    static const size_t DEFAULT_CONTAINER_SIZE = 10;  
    int* data;  
    size_t index;  
    size_t containerSize;  
    
public:  
    IntContainer() : index(0), containerSize(DEFAULT_CONTAINER_SIZE) {  
        std::cout << "Int Container initialized.\n";  
        this->data = new int[containerSize]{};  
    }  
    
    IntContainer(const size_t& containerSize) : index(0), containerSize(containerSize) {  
        std::cout << "Int Container initialized with size " << containerSize << ".\n";  
  
        this->data = new int[this->containerSize]{};  
    }  
    
    ~IntContainer() {  
        std::cout << "Int Container destructed.\n";  
        delete[] this->data;  
    }  
    
    class Iterator {  
        int* current;  
    
    public:  
        Iterator(int* ptr) :  
            current(ptr) {}  
  
        int& operator*() const {  
            return *(this->current);  
        }  
    
		Iterator& operator++() { // ++it  
            ++(this->current);  
            return *this;  
        }  
        
        Iterator operator++(int) { // it++  
            Iterator temp(*this);  
            ++(this->current);  
            return temp;  
        }  
        
        bool operator==(const Iterator& other) const {  
            return this->current == other.current;  
        }  
        
        bool operator!=(const Iterator& other) const {  
            return !(*this == other);  
        }    
	};
	  
    Iterator begin() {  
        return Iterator(this->data);  
    }  
    
    Iterator end() {  
        return Iterator(this->data + containerSize);  
    }  
    
    int operator[](const size_t& index) const {  
        if (index < 0 || index >= containerSize)  
            throw std::out_of_range("Out of range.");  
       return this->data[index];  
    }  
    
    bool set(const std::pair<size_t, int>& setPair) {  
        if (setPair.first < 0 || setPair.first >= containerSize)  
            return false;  
  
        this->data[setPair.first] = setPair.second;  
        return true;  
    }
};  
  
int main() {  
    IntContainer container{15};  
  
    container.set({0, 10});  
    container.set({1, 20});  
    container.set({2, 30});  
    container.set({3, 40});  
    container.set({4, 50});  
    container.set({5, 60});  
    container.set({6, 70});  
    container.set({7, 80});  
    container.set({8, 90});  
    container.set({9, 100});  
  
    for (const int& num : container)  
        std::cout << '\t' << num << '\n';  
  
    return 0;  
}
```