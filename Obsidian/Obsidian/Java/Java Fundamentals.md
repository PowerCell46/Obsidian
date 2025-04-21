## Integer Types

#### Byte
-128 <-> 128 
byte centuries = 20; 

#### Short
-32768 <-> 32767 
short years = 2000;

#### Int
-2147483648 <-> 2147483647 
int days = 730_484; 

#### Long
-9223372036854775808 - 9223372036854775807 
long hours = 17_531_616;

#### BigInteger

```java
BigInteger number1 = new BigInteger("10");  
BigInteger number2 = new BigInteger("20");  
  
number1 = number1.add(number2); // 30  
number1 = number1.subtract(number2); // 30 - 20 = 10  
BigInteger divideResult = number1.divide(number2); // 0 (not 0.5!)
BigInteger multiplyResult = number1.multiply(number2); // 200  
BigInteger powResult = number1.pow(2); // 100
```

### 0x' and '0X -> Hexadecimal numbers
### 'l' and 'L' -> Long numbers

```java
int hexa = 0xFFFFFFFF;
long number = 1L;
```

__________________________________________________________________________
## Float types

#### Float
-(±1.5 × 10−45 to ±3.4 × 1038)
float floatPI = 3.141592653589793238f;

#### Double
-(±5.0 × 10−324 to ±1.7 × 10308)
double doublePI = 3.141592653589793238;

### BigDecimal

```java
BigDecimal number = new BigDecimal(0); 
// Allows calculations with very high precision

number = number.add(BigDecimal.valueOf(2.5)); 
number = number.subtract(BigDecimal.valueOf(1.5)); 
number = number.multiply(BigDecimal.valueOf(2)); 
number = number.divide(BigDecimal.valueOf(2));
```

__________________________________________________________________________
## Type Conversion
#### Implicit type conversion (lossless): 
variable of the bigger type (e.g. Double) takes a smaller value (e.g. float)

```java
float heightInMeters = 1.74f; 
double maxHeight = heightInMeters;
```
#### Explicit type conversion (lossy)
when precision can be lost:

```java
double size = 3.14; 
int intSize = (int) size;
```
### Working with chars

```java
char s = 'a';  
// from char to int
System.out.println((int) s);

int i = 60;
// from int to char
System.out.println((char) i);


int sum = 0;
// using mathematical operators transforms the chars to ints
sum += 'a';
```

__________________________________________________________________________
## Switches

#### New Syntax

```java
String name = sc.nextLine();  
int salary;  
  
switch (name) {  
    case "Peter" -> salary = 1200;  
    case "Stiliyan" -> salary = 3000;  
    case "Ivan" -> salary = 700;  
    default -> salary = 0;  
}  
  
System.out.println(salary);
```
#### Variable, evaluated by a switch

```java
String name = sc.nextLine();  
  
String city = switch (name) {  
    case "Peter" -> "Sofia";  
    case "Stefan" -> "Varna";  
    case "Ivo" -> "Ruse";  
    default -> "Unknown";  
};
```

__________________________________________________________________________
## Arrays

```java
// Empty array with 10 slots
String[] arr = new String[10];
int[] arr2 = new int[10];

String[] days = { "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday" };


int[] numbers = {4, 1, 5, 7, 3};  

// sorting an array  
Arrays.sort(numbers);  
  
for (int number : numbers) {  
    System.out.println(number);  
}
```
### Transformations

```java
// String arr to Integers
int[] arr = Arrays.stream(scanner.nextLine().split(" "))
	.mapToInt(e -> Integer.parseInt(e))
	.toArray();
// or
int[] arr = Arrays.stream(scanner.nextLine().split(" "))
	.mapToInt(Integer::parseInt)
	.toArray();


// String arr to Double
double[] numbers = Arrays.stream(scanner.nextLine().split(" "))  
        .mapToDouble(e -> Double.parseDouble(e))  
        .toArray();
// or
double[] numbers = Arrays.stream(scanner.nextLine().split(" "))  
        .mapToDouble(Double::parseDouble)  
        .toArray();


// Int arr to String
String[] numsAsStr = Arrays.stream(numbersBiggerThan5)  
        .mapToObj(e -> String.valueOf(e))  
        .toArray(String[]::new);
// or
String[] numsAsStr = Arrays.stream(numbersBiggerThan5)  
        .mapToObj(String::valueOf)  
        .toArray(String[]::new);
```

__________________________________________________________________________
## Lists

```java
List nums = new ArrayList<>( Arrays.asList(10, 20, 30, 40, 50, 60));

// add to the end;
nums.add(70);

// add to the beginning;
nums.addFirst(0);

// changing the value on index 0 with the value 5
nums.set(0, 5);

// insertion in Java Lists
nums.add(1, 15);


ArrayList<String> strings = Arrays.stream(
		scanner.nextLine().split(" ")
	)  
    .collect(Collectors.toCollection(ArrayList::new));


ArrayList<Integer> numbers = Arrays.stream(
		scanner.nextLine().split(" ")
	)  
    .map(Integer::parseInt)  
    .collect(Collectors.toCollection(ArrayList::new));
```

### Removing elements

```java
ArrayList<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3));  

// removing index 1
numbers.remove(1); 

// removing the element with value 1
numbers.remove(Integer.valueOf(1));


ArrayList<String> numbers = new ArrayList<>(Arrays.asList("1", "2", "3", "4", "5"));  

// removing the element with value "1"
numbers.remove("1");  

// removing the element on index 1
numbers.remove(1);
```
### Sorting a Collection

```java
List names = new ArrayList<>(Arrays.asList( "Peter", "Michael", "George", "Victor", "John")); 

Collections.sort(names);


List<Student> students = new ArrayList<>();

students.stream()  
        .sorted((firstStudent, secondStudent) -> Double.compare(secondStudent.getGrade(), firstStudent.getGrade()))  
        .forEach(student -> System.out.println(student));


List<Person> people = new ArrayList<>();

people.stream()  
        .sorted((firstP, secondP) -> Integer.compare(firstP.getAge(), secondP.getAge()))  
        .forEach(person -> System.out.println(person));
```
### Reversing a Collection

```java
Collections.reverse(names);
```

__________________________________________________________________________
## Classes

### java.util.Random

```java
import java.util.Random;  
  
public class Main {  
    public static void main(String[] args) {  
  
        Dice dice = new Dice();  
        dice.setSides(6);  
        dice.setType("6 sided dice");  
  
        System.out.format("Rolled side %d\n", dice.roll());  
        System.out.format("The dice has %d sides.\n", dice.getSides());  
        System.out.format("The dice is a %s.\n", dice.getType());  
    }  
}  
  
class Dice {  
    private int sides;  
    private String type;  
  
    public int roll() {  
	    // using the Random package
        Random rnd = new Random();
        // getting a random int number with given max value  
        int rolledSide = rnd.nextInt(this.sides + 1);  
        return rolledSide;  
    }  
  
    public int getSides() {return this.sides;}  
  
    public void setSides(int side) {this.sides = side;}  
  
    public String getType() {return this.type;}  
      
    public void setType(String type) {this.type = type;}  
}
```

__________________________________________________________________________
## Associative Arrays

### HashMap 
##### (dictionary)
▪ Keys are unique 
▪ Uses a hash-table + list 

```java
Map<String, Double> studentsGrades = new HashMap<>();  
  
// add entry  
studentsGrades.put("Peter", 5.20);  
studentsGrades.put("Ivan", 4.50);  
studentsGrades.put("Stiliyan", 6.00);  
  
// update entry  
studentsGrades.put("Peter", 6.00);  
studentsGrades.replace("Peter", 5.99);  

// adds the value only if it isn't added that key already
studentsGrades.putIfAbsent("Stoyan", 4.45);

// remove entry  
studentsGrades.remove("Ivan");  
  
  
System.out.println(studentsGrades.size());  
  
System.out.println(studentsGrades.isEmpty());  

// get a value for a key
System.out.println(studentsGrades.get("Peter"));
  
  
System.out.println(studentsGrades.containsKey("Peter"));;  
System.out.println(studentsGrades.containsValue(6.00));

...

// for (let [key, value] of Object.entries(occurrences))
for (Map.Entry<Double, Integer> pair : occurrences.entrySet()) {  
    System.out.printf("%.1f -> %d%n", pair.getKey(), pair.getValue());  
}

occurrences.forEach((Double k, Integer v) -> {  
    System.out.println(String.format("%.1f -> %d", k, v));  
});
```

### LinkedHashMap 
##### (dictionary keeping track of the order of adding)
▪ Keys are unique 
▪ Keeps the keys in order of addition 

```java
Map<String, Double> studentsGrades = new LinkedHashMap<>();  
  
// add entry  
studentsGrades.put("Peter", 5.20);  
studentsGrades.put("Ivan", 4.50);  
studentsGrades.put("Stiliyan", 6.00);

// keeping the order of the entries
```

###  TreeMap 
##### (dictionary ordering the keys in ASC order)
▪ Keys are unique 
▪ Keeps its keys always sorted 
▪ Uses a balanced search tree

```java
Map<String, Double> studentsGrades = new TreeMap<>();  
  
// add entry  
studentsGrades.put("Peter", 5.20);  
studentsGrades.put("Ivan", 4.50);  
studentsGrades.put("Stiliyan", 6.00);

// ordering the entries in ASC order
```

__________________________________________________________________________
## Lambda Expressions

#### Filter & Map examples

```java
int[] numbers = Arrays
	.stream(scanner.nextLine().split(" "))
	.mapToInt(x -> Integer.parseInt(x))
	.toArray();

int[] numbersBiggerThan5 = Arrays
	.stream(numbers)  
	.filter(x -> x > 5)  
	.toArray();
```

__________________________________________________________________________
## String Builder

```java
StringBuilder stringBuilder = new StringBuilder();  

// concatenate
stringBuilder.append("Hello there");  
stringBuilder.append(", General Kenobi!\n");  
stringBuilder.append("You are a bold one!");  
  
stringBuilder.replace(  
        stringBuilder.indexOf("Kenobi"),  
        stringBuilder.indexOf("Kenobi") + "Kenobi".length(),  
        "Grievous"  
);  
  
stringBuilder.reverse();  
  
stringBuilder.insert(0, "Message: ");  

// Convert to a string
System.out.println(stringBuilder.toString());

// Clears the String Builder
stringBuilder.setLength(0);
```

__________________________________________________________________________
# Next Course: [[Java Advanced]]