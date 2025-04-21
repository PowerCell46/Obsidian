## Constructors, Getters, Setters, Encapsulation

```cpp
#include <iostream>  
#include <sstream>  
   
class Team {  
private:  
    std::string name;  
    unsigned int wins, loses, ties;  
    int score;  
public:  
    void toString() {  
        std::cout << "Team: " << this->name  
                << "\nWins: " << this->wins  
                << "\nLoses: " << this->loses  
                << "\nTies: " << this->ties  
                << "\nScore: " << this->score  
                << std::endl;  
    }  
    void setWins(unsigned int wins) {  
        this->wins = wins;  
    }    unsigned int getWins() {  
        return this->wins;  
    }  
    // Faster Constructor (initializes the value alongside with the variable)  
    Team(std::string name, int wins, int loses, int ties)  
        : name(name),  
          wins(wins),  
          loses(loses),  
          ties(ties) {  
        this->score = this->wins * 3 + this->ties * 1;  
    }  
    // Team(std::string name, int wins, int loses, int ties) {  
    //     this->name = name;    
    //     this->wins = wins;    
    //     this->loses = loses;    
    //     this->ties = ties;    
    //     this->score = this->wins * 3 + this->ties * 1;    
    // }};  
  
int main() {  
    int numberOfTeams;
    std::cin >> numberOfTeams;
    std::cin.ignore();
  
    for (int i = 0; i < numberOfTeams; ++i) {  
        std::string data;
        std::getline(std::cin, data);
  
        std::stringstream strStream(data);  
        std::string currentData;  
        int index = 0;  
        std::string currentDataArr[4];  
  
        while (strStream >> currentData) {  
            currentDataArr[index] = currentData;  
            index++;  
        }  
        Team currentTeam(  
            currentDataArr[0],  
            std::stof(currentDataArr[1]),  
            std::stof(currentDataArr[2]),  
            std::stof(currentDataArr[3])  
        );  
  
        currentTeam.toString();  
    }  
    return 0;  
}  
  
/*  
3  
Croatia 3 0 0  
Spain 2 1 0  
Argentina 3 0 0  
*/
```

## Initializing empty objects

```cpp
class Person {  
    std::string name;  
    int age;  
  
public:  
    std::string getName() const {return this->name;}  
    int getAge() const {return this->age;}  
};  
  
int main() {  
    // does not guarantee a default value for primitive types
    const Person p; 

	// guarantees a default value for primitive types 
    const Person person = Person();   
  
    std::cout << p.getAge() << std::endl;  
    std::cout << person.getAge() << std::endl;  
  
    return 0;  
}
```

# Operator Overloading

```cpp
class Vector {  
private:  
    double x, y;  
  
public:  
    Vector(double x, double y) : 
	    x(x), y(y) {}  
    
    double getX() const {  
        return this->x;  
    }  
    
    double getY() const {  
        return this->y;  
    }  
    
    Vector &operator+=(const Vector &other) {  
        this->x += other.x;  
        this->y += other.y;  
  
        return *this;  
    }  
    
    Vector operator+(const Vector &second) const {  
        return Vector(this->x + second.x, this->y + second.y);  
    }  
    
    Vector operator-(const Vector &other) const {  
        return Vector(this->x - other.x, this->y - other.y);  
    }
};
```

# Constructors

```cpp
class Body {  
    double weight;  
    double height;  
public:  
    double getWeight() const {  
        return this->weight;  
    }  
    double getHeight() const {  
        return this->height;  
    }  
    Body() = default; // default constructor  
  
    Body(const double weight, const double height) : // full args constructor  
        weight(weight), height(height) {}  
  
    Body(const Body& other) = default; // copy constructor: clone(body)  
  
    Body& operator=(const Body& other) { // copy assignment operator: copy = copy;  
        if (this != &other) {  
            this->height = other.height;  
            this->weight = other.weight;  
        }        
        return *this;  
    }  
    
    ~Body() = default; // destructor  
};  
  
std::ostream& operator<<(std::ostream& os, const Body& body) {  
    os << "Body with Height: " << body.getHeight() << " and with Weight: " 
	<< body.getWeight() << '.' << std::endl;  
  
    return os;  
}
```

## Virtual Methods

```cpp
class Filter {  
protected:  
    virtual bool shouldBeRemoved(const char c) {  
        return true;  
    }
public:  
    std::string getFiltered(const std::string &input) {  
        std::stringstream strStream("");  
  
        for (const char c : input) {  
            if (!this->shouldBeRemoved(c))  
                strStream << c;  
        }  
        return strStream.str();  
    }
};  
  
  
class CapitalsFilter : public Filter {  
    bool shouldBeRemoved(const char c) override {  
        return c >= 'A' && c <= 'Z';  
    }};  
  
  
class DigitsFilter : public Filter {  
    bool shouldBeRemoved(const char c) override {  
        return c >= '0' && c <= '9';  
    }};
```

## Calling the parent V. of a method

```cpp
#include <iostream>  
#include <vector>  
  
using std::string;  
using std::vector;  
using std::cin;  
using std::cout;  
  
struct Vehicle {  
    double speed;  
    bool isRunning;  
  
    virtual void move() {  
        std::cout << "Vehicle is with speed: " << speed << '\n';  
    }  
};  
  
class Car : public Vehicle {  
    bool parkBrakeIsOn;  
public:  
    Car(const double& speed, const bool& isRunning, const bool& parkBrakeIsOn) :
	    parkBrakeIsOn(parkBrakeIsOn) {  
        this->speed = speed;  
        this->isRunning = isRunning;  
    }     
    
    void move() override {  
        std::cout << "Car is moving with speed: " << this->speed << '\n';  
    }
};  
  
  
int main() {  
    Car c{120, true, false};  
  
    // pointer to the parent class  
    Vehicle* v = &c;  
  
    // for the polymorphism calls the child method  
    v->move();  
  
    // slicing and casting to Vehicle  
    Vehicle copy = *v;  
    // accessing the parent method  
    copy.move();  
  
    return 0;  
}
```

# Inheritance and Polymorphism

```cpp
#include <iostream>  
#include <sstream>  
   
/*******************************************************************************/  

class Organism {  
    double weight;  
    bool consumesOthers;  
public:  
    virtual ~Organism() = default;  
  
    Organism() : 
	    weight(), consumesOthers() {}  
    Organism(const double& weight, const bool& consumesOthers)  : 
	    weight(weight), consumesOthers(consumesOthers) {}  
  
    virtual void grow() {  
        this->weight++;  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const Organism& organism);  
    friend std::istream& operator>>(std::istream& is, Organism& organism);  
};  
  
  
class Animal : public Organism {  
    std::string color;  
    int numberOfLegs;  
    int age;  
public:  
    Animal() : 
	    Organism(0, true), color(), numberOfLegs(), age() {}  
    Animal(const double& weight,  
        const std::string& color, const int& numberOfLegs, const int& age)  
            : Organism(weight, true), color(color), numberOfLegs(numberOfLegs), age(age) {}  
  
    void grow() override {  
        Organism::grow();  
        Organism::grow();  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const Animal& animal);  
    friend std::istream& operator>>(std::istream& is, Animal& animal);  
};  
  
  
class Person : public Animal {  
    std::string name;  
public:  
    Person() : 
	    Animal(), name() {}  
    Person(const std::string& name, const double& weight, const std::string& color,  
	const int& numberOfLegs, const int& age) :  
            Animal(weight, color, numberOfLegs, age), name(name) {}  
  
    void grow() override {  
        Organism::grow();  
        Organism::grow();  
        Organism::grow();  
        Organism::grow();  
        Organism::grow();  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const Person& person);  
    friend std::istream& operator>>(std::istream& is, Person& person);  
};  
  
  
class Plant : public Organism {  
    bool hasSeeds;  
public:  
    Plant() : 
	    Organism(0, false), hasSeeds() {}  
    Plant(const double& weight, const bool& hasSeeds) : 
	    Organism(weight, false), hasSeeds(hasSeeds) {}  
  
    void grow() override {  
        Organism::grow();  
        Organism::grow();  
        Organism::grow();  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const Plant& plant);  
    friend std::istream& operator>>(std::istream& is, Plant& plant);  
}; 
  
/*******************************************************************************/  

std::ostream& operator<<(std::ostream& os, const Organism& organism) {  
    return os << "Weight: " << organism.weight << ", "  
        << (organism.consumesOthers ? "eats others" : "does not eat others") << ". ";  
}  
  
std::ostream& operator<<(std::ostream& os, const Animal& animal) {  
    return os << (Organism) animal  
        << "Color: " << animal.color << ", age: " << animal.age  
        << ", number of legs: " << animal.numberOfLegs << ". ";  
}  
  
std::ostream& operator<<(std::ostream& os, const Plant& plant) {  
    return os << (Organism) plant  
        << (plant.hasSeeds ? " Has seeds" : "Does not have seeds")  << ". ";  
}  
  
std::ostream& operator<<(std::ostream& os, const Person& person) {  
    return os << "Name: " << person.name << ". " << (Animal) person;  
}  
  
/*******************************************************************************/  

std::istream& operator>>(std::istream& is, Organism& organism) {  
    return is >> organism.weight >> organism.consumesOthers;  
}  
  
std::istream& operator>>(std::istream& is, Animal& animal) {  
    return is >> (Organism&) animal >> animal.color >> animal.numberOfLegs >> animal.age;  
}  
  
std::istream& operator>>(std::istream& is, Plant& plant) {  
    return is >> (Organism&) plant >> plant.hasSeeds;  
}  
  
std::istream& operator>>(std::istream& is, Person& person) {  
    return is >> (Animal&) person >> person.name;  
}  
    
/*******************************************************************************/  

class Filter {  
protected:  
    virtual bool shouldBeRemoved(const char& ch) {  
        return true;  
    }
    public:  
    std::string getFiltered(const std::string& input) {  
        std::stringstream strStream{};  
  
        for (const char& ch : input)  
            if (shouldBeRemoved(ch))  
                strStream << ch;  
  
        return strStream.str();  
    }
};  
  
  
class LowercaseFilter : public Filter {  
    bool shouldBeRemoved(const char &ch) override {  
        return ch >= 'a' && ch <= 'z';  
    }
};  
  
  
class UppercaseFilter : public Filter {  
    bool shouldBeRemoved(const char &ch) override {  
        return ch >= 'A' && ch <= 'Z';  
    }
};  
  
  
class DigitsFilter : public Filter {  
    bool shouldBeRemoved(const char &ch) override {  
        return ch >= '0' && ch <= '9';  
    }
};  
  
  
/*******************************************************************************/  

int main() {  
	return 0;  
}
```


```cpp
#include <iostream>  
#include <vector>  
#include <sstream>  
  
class Vehicle {  
protected:  
    double speed;  
    bool isRunning;  
  
public:  
    double getSpeed() const {  
        return this->speed;  
    }    Vehicle() : speed(), isRunning() {}  
    Vehicle(const double& speed, const bool& isRunning) : 
	    speed(speed), isRunning(isRunning) {}  
  
    virtual void updateSpeed() {  
        std::cout << "Updating speed from Vehicle" << std::endl;  
        speed++;  
    }};  
  
  
class Car : public Vehicle {  
    bool parkingBrakeOn;  
public:  
    Car(const double& speed, const bool& isRunning, const bool& parkingBrakeOn) :  
        Vehicle(speed, isRunning), parkingBrakeOn(parkingBrakeOn) {}  
  
    void updateSpeed() override {  
        std::cout << "Updating speed from Car" << std::endl;  
        Vehicle::updateSpeed();  
        Vehicle::updateSpeed();  
    }};  
  
  
int main() {  
    Vehicle vehicle{120, true};  
    Car car{120, true, true};  
  
  
    Vehicle v = (Vehicle) car;  
    v.updateSpeed();  
    // slices car -> vehicle: calling update calls the vehicle update  
  
  
    Vehicle& ve = car;  
    ve.updateSpeed();  
    // reference to a vehicle -> no slicing: calling update calls the car update  
  
    Vehicle* veh = &car;  
    veh->updateSpeed();  
    // pointer to a vehicle -> no slicing: calling update calls the car update  
  
    return 0;  
}
```


```cpp
#include <iostream>  
#include <sstream>  
  
template<class Inherited>  
class Animal {  
protected:  
    int age;  
    double weight;  
    Animal(const int& age, const double& weight) :  
        age(age), weight(weight) {}  
public:  
    void birthday() {  
        static_cast<Inherited*>(this)->birthday();  
    }  
    int getAge() const {  
        return this->age;  
    }};  
  
  
class Person : public Animal<Person> {  
    std::string name;  
public:  
    Person(const std::string& name, const int& age, const double& weight) :  
        Animal(age, weight), name(name) {}  
  
    void birthday() {  
        ++this->age;  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const Person& person);  
};  
  
  
class Cat : public Animal<Cat> {  
    std::string furColor;  
public:  
    Cat(const std::string& furColor, const int& age, const double& weight) :  
        Animal(age, weight), furColor(furColor) {}  
  
    void birthday() {  
        this->age += 7;  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const Cat& cat);  
};  
  
  
std::ostream& operator<<(std::ostream& os, const Person& person) {  
    return os << "Name: " <<  person.name << ", Age: " << person.age << ", Weight: " << person.weight << ".";  
}  
  
std::ostream& operator<<(std::ostream& os, const Cat& cat) {  
    return os << "Age: " <<  cat.age << ", Fur Color: " << cat.furColor << ", Weight: " << cat.weight << ".";  
}  
  
  
int main() {  
    Person p{"Peter", 21, 101};  
  
    p.birthday();  
  
    std::cout << p << std::endl;  
  
  
    Cat c{"White", 2, 4};  
  
    c.birthday();  
  
    std::cout << c << std::endl;  
  
    return 0;  
}
```

### Virtual Inheritance Explained

When you use **virtual inheritance** in C++, you are telling the compiler to ensure that a **single instance** of the base class is shared among all derived classes, even in cases of **multiple inheritance** (like the "diamond problem").

#### Without Virtual Inheritance (Problem)

If you inherit normally (non-virtual), every derived class will have its **own copy** of the base class. In the case of a diamond inheritance pattern:

- Both `Student` and `Worker` inherit from `Person`.
- If `Programmer` inherits from both `Student` and `Worker`, `Programmer` will end up with **two copies** of `Person`—one from `Student` and another from `Worker`.

This leads to ambiguity:

- Which copy of `Person` should `Programmer` use?
- You cannot easily access or modify the members of `Person` because the compiler doesn’t know which copy you mean.

#### With Virtual Inheritance (Solution)

By making the inheritance **virtual**, both `Student` and `Worker` share a **single instance** of `Person`. When `Programmer` inherits from both `Student` and `Worker`, the compiler ensures there is only **one shared instance** of `Person` in `Programmer`.

This eliminates ambiguity and ensures consistent access to the `Person` base class members.

```cpp
#include <iostream>  
#include <vector>  
#include <sstream>  
  
  
struct CanPrintInfo {  
    virtual void printInfo(const std::string&) = 0;  
};  
  
  
/***********************************************************/  
class Vehicle : public CanPrintInfo {  
protected:  
    double speed;  
    bool isRunning;  
  
    void printInfo(const std::string& vehicleType) override {  
        std::cout << "The " << vehicleType << " is moving..." << std::endl;  
    }
    public:  
    Vehicle() : 
	    speed(), isRunning() {}  
  
    virtual ~Vehicle() = default;  
  
    virtual void move() = 0;  
};  
  
  
class Car : public Vehicle {  
    bool parkingBrakeOn;  
public:  
    Car(const double& speed, const bool& parkingBrakeOn) : 
	    parkingBrakeOn(parkingBrakeOn) {  
        this->speed = speed;  
        this->isRunning = true;  
    }  
    
    void move() override {  
        printInfo("Car");  
    }};  
  
  
class Airplane : public Vehicle {  
    double altitude;  
    int numberOfEngines;  
    bool isJet;  
public:  
    Airplane(const double& speed, const double& altitude, const int& numberOfEngines, const bool& isJet) : 
		altitude(altitude), numberOfEngines(numberOfEngines), isJet(isJet) {  
        this->speed = speed;  
        this->isRunning = true;  
    }  
    
    void move() override {  
        printInfo("Plane");  
    }};  


class Jet : public Airplane {  
public:  
    Jet(const double& speed, const double& altitude, const int& numberOfEngines) :
	    Airplane(speed, altitude, numberOfEngines, true) {}  
  
    void move() override {  
        printInfo("Jet");  
    }};  
  
  
class Turboprop : public Airplane {  
public:  
    Turboprop(const double& speed, const double& altitude, const int& numberOfEngines) :
	    Airplane(speed, altitude, numberOfEngines, false) {}  
  
    void move() override {  
        printInfo("Turboprop");  
    }};  
  
  
// class Person : CanPrintInfo {  
//     void printInfo(const std::string& printWord) override {  
//         std::cout << "Hello, I am a " << printWord << '.' << std::endl;  
//     }  
// public:  
//     void printInfo() {  
//         printInfo("Person");  
//     }  
// }; 

/***********************************************************/  
  
  
class Person {  
    std::string name;  
    int age;  
    double weight;  
public:  
    Person()  
        : name(), age(), weight() {}  
  
    Person(const std::string& name, const int& age, const double& weight)  
        : name(name), age(age), weight(weight) {}  
  
    std::string getName() const {  
        return this->name;  
    }
};  
  
class Student : public virtual Person {  
    static int id;  
    int studentId;  
public:  
    Student()  
        : Person(), studentId(++id) {}  
    Student(const std::string& name, const int& age, const double& weight)  
        : Person(name, age, weight), studentId(++id) {}  
  
    int getStudentId() const {  
        return this->studentId;  
    }};  
  
int Student::id = 0;  
  
class Worker : public virtual Person {  
    static int currentNumber;  
    int workerNumber;  
public:  
    Worker()  
        : Person(), workerNumber(++currentNumber) {}  
    Worker(const std::string& name, const int& age, const double& weight)  
        : Person(name, age, weight), workerNumber(++currentNumber) {}  
  
    int getWorkerNumber() const {  
        return this->workerNumber;  
    }};  
  
int Worker::currentNumber = 0;  
  
  
class Programmer : public Student, public Worker {  
    std::vector<std::string> techStack;  
public:  
    Programmer()  
        : Person(), Student(), Worker(), techStack() {}  
        
    Programmer(const std::string& name, const int& age, const double& weight, const std::vector<std::string>& techStack) : 
	    Person(name, age, weight), Student(), Worker(), techStack(techStack) {}  
};  

/***********************************************************/  
  
int main() {  
    std::vector<std::string> techStack = {"C++", "Python", "JavaScript"};  
  
    Programmer programmer("Alice", 25, 60.5, techStack);  
  
    Worker worker = programmer;  
    Student student = programmer;  
  
    std::cout << worker.getName() << ' ' << student.getName() ;  
  
    return 0;  
}
```

## Deep copy constructor

```cpp
class String {  
    int size;  
    char *data;  
  
public:  
    String(const int &size) : size(size), data(new char[size]) {}
      
    String(char *str) : size(std::strlen(str)), data(str) {}
      
    int getSize() const {  
        return this->size;  
    }  
    
    // deep copy  
    String &operator=(const String &other) {  
        if (&other != this) {  
            delete[] data; // release the original memory  
            data = new char[other.size]; // allocate new memory  
            size = other.size; // assign the size  
  
            for (int i = 0; i < other.size; ++i) // move the data  
                data[i] = other.data[i];  
        }  
        return *this;  
    }  
    
    ~String() {  
        delete[] this->data;  
    }
};
```


▪ Compilation unit 
- a file (usually.cpp) the compiler works on 

## Build process for each unit: 
##### **1. Preprocessing (`.cpp` → Expanded Source)**
- The preprocessor **expands** your code by replacing:
- `#include <iostream>` with the actual contents of the **iostream** header file.
- `#define` macros with their values.
- `#ifdef` conditions are checked and included/excluded accordingly.
**Output:** Expanded `.cpp` file (still readable code, but larger).

##### **2. Compilation (Expanded Source → Platform-Specific Code → Assembly Code)**
- The compiler **translates** the expanded C++ code into an **intermediate representation** (platform-specific low-level code).
- Then, it converts this into **assembly language**, which is still human-readable but closer to machine language.
**Output:** Assembly file (`.s` file, but rarely seen directly).

##### **3. Assembly (Assembly Code → Object Code, `.o` / `.obj` file)**
- The assembler converts **assembly language** into **binary machine code** (1s and 0s).
- The result is an **object file** (`.o` in Linux, `.obj` in Windows).
- Each `.cpp` file is compiled **separately** into its own `.o` or `.obj` file.
**Output:** Object file (`main.o`, `file.o`, etc.).

##### **4. Linking (Object Code → Final Executable)**
- The **linker** takes all object files (`.o` / `.obj`) and combines them.
- It also **links** necessary libraries (like `iostream` for `std::cout`).
- Finally, it produces a **single executable file** (like `a.out` or `program.exe`).
**Output:** Final executable (`a.out`, `program.exe`).

## Singleton Pattern 

```cpp
#include <iostream>  
#include <vector>  
#include <sstream>  
  
class Application {  
    Application() {}  
    ~Application() {}  
public:  
    static Application& getInstance() {  
        static Application app;  
        return app;  
    }  
    // Move and Copy constructors are not allowed  
    Application(const Application& other) = delete;  
    Application(Application&& other) = delete;  
  
    // Move and Copy assignment operators are not allowed  
    Application& operator=(const Application& other) = delete;  
    Application& operator=(Application&& other) = delete;  
  
    void functionMethod() {  
        std::cout << "Singleton pattern using Meyers' Singleton." << std::endl;  
    }};  

  
int main() {  
    Application& app = Application::getInstance();  
  
    app.functionMethod();  
  
    return 0;  
}
```

## Unhiding an overloaded method
### using Vehicle::accelerate;

```cpp
#include <iostream>  
#include <vector>  
#include <sstream>  
  
class Vehicle {  
public:  
    void accelerate() {  
        std::cout << "Increasing acceleration" << std::endl;  
    }  
};  
  
class Aeroplane : public Vehicle {  
    public:  
    using Vehicle::accelerate;  
  
    void accelerate(const int& height) {  
        std::cout << "Accelerating at height of " << height << std::endl;  
    }};  
  
int main() {  
    Aeroplane aeroplane;  
  
    aeroplane.accelerate();  
  
    return 0;  
}
```