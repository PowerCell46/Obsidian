## Stack

```java
// adds to the beginning of the array
// removes from the first index of the array

// []  
ArrayDeque<Integer> stack = new ArrayDeque<>();  
  
// 10 -> [] = [10]  
stack.push(10);  
  
// 20 -> [10] = [20, 10]  
stack.push(20);  
  
// 30 -> [20, 10] = [30, 20, 10]  
stack.push(30);  
  
// remove and get the last added element (the element at index 0)
// [30, 20, 10] -> [20, 10]  
int lastAddedElement = stack.pop(); // 30 
System.out.println(lastAddedElement);  
  
// get the last added element without removing it from the Stack 
// [20, 10]
System.out.println(stack.peek()); // 20
```


```java
// creating a stack with integers from the console
ArrayDeque<Integer> stack = new ArrayDeque<>(  
         Arrays.stream(scanner.nextLine().split(""))  
              .map(Integer::parseInt)  
              .collect(Collectors.toList())  
);
```

## Queues

```java
// adds to the end index of the array
// removes from the first index of the array

// []  
ArrayDeque<Integer> queue = new ArrayDeque<>();  
  
// 10 -> [10]  
queue.offer(10);  
  
// 20 -> [10, 20]  
queue.offer(20);  
  
// 30 -> [10, 20, 30]  
queue.offer(30);  
  
// [10, 20, 30] -> [20, 30]  
int firstAddedElement = queue.remove(); // 10
int firstAddedElement = queue.poll(); 
// same as remove(), but if the queue is empty, it does not throw an error and returns null

System.out.println(firstAddedElement);  
```

## Priority Queue

```java
public class Main {  
    public static void main(String[] args) {  
        Scanner scanner = new Scanner(System.in);  
  
        PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();  
        PriorityQueue<Integer> reversedpriorityQueue = 
	        new PriorityQueue<>(Comparator.reverseOrder());  
  
        priorityQueue.offer(1);  
        priorityQueue.offer(2);  
        priorityQueue.offer(10);  
        priorityQueue.offer(-1);  
  
        while (!priorityQueue.isEmpty()) {  
            System.out.println(priorityQueue.poll());  
        }  
    }  
}
```

## LinkedList

```java
LinkedList<String> linkedList = new LinkedList<>();  
// Better at insertions and deletions
// Worse at iterating through the elements and getting a certain el @ index
// Take more space (stores pointers)

linkedList.add("A");  
linkedList.add("B");  
linkedList.add("C");  
  
// insert at a specific index (more efficient)
linkedList.add(4, "E");  
  
System.out.println(linkedList.indexOf("C"));  
  
// remove specific element (more efficient)
linkedList.remove("C");
```

__________________________________________________________________________
# Matrices

```java
int[][] intMatrix = new int[2][2];  
double[][] doubleMatrix = new double[3][3];  
String[][] stringMatrix = new String[4][4];  
  
  
int[][] matrix = {  
        {1, 2, 3},  
        {4, 5, 6},  
        {7, 8, 9}  
};  
  
System.out.println(matrix[1][1]);
```
## Filling a matrix

```java
import java.util.Scanner;  
  
  
public class Main {  
    public static void main(String[] args) {  
        Scanner scanner = new Scanner(System.in);  
  
        int size = Integer.parseInt(scanner.nextLine());  
        int[][] matrix = new int[size][size];  
  
        for (int row = 0; row < size; row++) {  
            for (int column = 0; column < size; column++) {  
                matrix[row][column] = column + (size * row) + 1;  
                System.out.print(matrix[row][column] + " ");  
            }  
            System.out.println();  
        }  
    }  
}
```

__________________________________________________________________________
# Sets
##### Types
- HashSet 
- TreeSet 
- LinkedHashSet

```java
Set<String> hash = new HashSet<String>();

Set<String> tree = new TreeSet<>();

Set<Integer> linkedSet = new LinkedHashSet<>();
```

# Hash maps

```java
Map<Double, Integer> doubleOccurrences = Arrays.stream(scanner.nextLine()
		.split(" "))  
        .map(Double::parseDouble)  
        .distinct()  
        .collect(Collectors.toMap(Function.identity(), v -> 0));

// keySet() - .keys()
for (String key : map.keySet()) {  
    System.out.println(key + ": " + map.get(key));  
}  

// values()
for (Integer value : map.values()) {  
    System.out.println(value);  
}
```

__________________________________________________________________________
### Sorting

#### Sorting Lists

```java
List<Integer> nums = new ArrayList<>(Arrays.asList(10, 1, 1_000, 100));

// asc order
List<Integer> numsAsc = nums.stream()
	.sorted((num1, num2) -> num1.compareTo(num2))
	.collect(Collectors.toList());

// desc order
List<Integer> numsDesc = nums.stream()
	.sorted((num1, num2) -> num2.compareTo(num1))
	.collect(Collectors.toList());
```

#### Sorting HashMaps

```java
Map<Integer, String> products = new HashMap<>();  
  
products.put(1, "Apple");  
products.put(3, "Orange");  
products.put(2, "Banana");  
  
products.entrySet()  
        .stream()  
        .sorted((el1, el2) -> {  
            // sorting in desc order by value  
            int res = el2.getValue().compareTo(el1.getValue());  
              
            if (res == 0) {  
                // if values are equal, sort by key in asc order  
                res = el1.getKey().compareTo(el2.getKey());  
            }  
              
            return res;  
        })  
        .forEach(el -> System.out.println(el.getKey() + ": " + el.getValue()));

// ---------------------------------------------------------------------------- //

Map<String, Integer> mp = new HashMap<>();  
  
mp.put("Aries", 1);  
mp.put("Taurus", 2);  
mp.put("Gemini", 3);  
  
Map<String, Integer> resultMap = mp.entrySet()  
        .stream()  
        // sorting by value in desc order  
        .sorted(Map.Entry.<String, Integer>comparingByValue().reversed())  
        .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue,  
                (e1, e2) -> e1, LinkedHashMap::new));
```

#### Sorting a Hashmap with a List as a value 

```java
Map<String, ArrayList<Integer>> arr = new HashMap<>();  
  
ArrayList<Integer> lst1 = new ArrayList<>(Arrays.asList(10, 5, 100, 50));  
ArrayList<Integer> lst2 = new ArrayList<>(Arrays.asList(10, 232, 11, 60));  
ArrayList<Integer> lst3 = new ArrayList<>(Arrays.asList(123, 221, 1, 62));  
  
arr.put("First", lst1);  
arr.put("Second", lst2);  
arr.put("Third", lst3);  
  
arr.entrySet().stream()  
        .sorted((a, b) -> {  
            // if the keys are equal, sort by the sum of the values in ascending order  
            if (a.getKey().compareTo(b.getKey()) == 0) {  
                int sumFirst = a.getValue().stream().mapToInt(Integer::intValue).sum();  
                int sumSecond = b.getValue().stream().mapToInt(Integer::intValue).sum();  
                return sumFirst - sumSecond;  
            }  
            // comparing the keys in descending order  
            return b.getKey().compareTo(a.getKey());  
        })  
        .forEach(pair -> {  
            System.out.println("Key: " + pair.getKey());  
            System.out.print("Value: ");  
            pair.getValue().sort((a, b ) -> a.compareTo(b));  
            for (int num : pair.getValue()) {  
                System.out.printf("%d ", num);  
            }  
            System.out.println();  
        });
```

__________________________________________________________________________
# Working with files

#### Get Project Path

```java
System.getProperty("user.dir")
```

#### File input and File Output Stream

```java
import java.io.FileInputStream;  
import java.io.FileNotFoundException;  
import java.io.FileOutputStream;  
import java.io.IOException;  
import java.util.List;  
import java.util.Scanner;  
import java.util.Set;  
import java.util.HashSet;  
  
public class Main {  
    public static void main(String[] args) throws IOException {  
        Scanner sc = new Scanner(System.in);  
  
        String path = String.format("%s\\src\\text.txt", System.getProperty("user.dir"));  
        String copyPath = String.format("%s\\src\\textCopy.txt", System.getProperty("user.dir"));  
  
        Character[] symbols = {'.', ',', '!', '?'};  
        Set<Character> symbolsSet = new HashSet<>(List.of(symbols));  
  
        try (  
	        // Input stream - for reading data
            FileInputStream fileInputStream = new FileInputStream(path);
            // Output stream for writing data  
            FileOutputStream fileOutputStream = new FileOutputStream(copyPath)  
        ) {  
  
            int oneByte = fileInputStream.read();  
             
            while(oneByte >= 0) {  
	            // transforming the value to a character
                char ch = (char) oneByte;  
                
                if (!symbolsSet.contains(ch)) {  
		            // write the char in the file if it's not a symbol
                    fileOutputStream.write(oneByte);  
                }  
//                System.out.print(ch);  
                oneByte = fileInputStream.read();  
            }  
            fileInputStream.close();  
  
        } catch (FileNotFoundException e) {  
            System.out.println("File not found");  
  
        } catch (IOException e) {  
            System.out.println("Error reading file");  
        }  
    }  
}
```

#### Using Buffers

```java
import java.io.*;  
import java.util.*;  
  
  
public class Main {  
    public static void main(String[] args) throws IOException {  
        Scanner sc = new Scanner(System.in);  
  
        String path = String.format("%s\\src\\text.txt", System.getProperty("user.dir"));  
        String copyPath = String.format("%s\\src\\textCopy.txt", System.getProperty("user.dir"));  
  
        try (  
            BufferedReader in = new BufferedReader(new FileReader(path));  
            PrintWriter out = new PrintWriter(new FileWriter(copyPath))  
        ) {  
            int counter = 0;  
            String line = in.readLine();  
  
            while (line != null) {  
                out.println(line);  
                System.out.println(line + "\n");  
                counter++;  
                line = in.readLine();  
            }  
  
            System.out.println(counter);  
        }  
    }  
}
```

### Serialization of data

```java
String copyPath = String.format("%s\\src\\textCopy.txt", System.getProperty("user.dir"));  
  
// list with strings  
List<String> names = new ArrayList<>();  
Collections.addAll(names, "Mimi", "Gosho");  
  
FileOutputStream writingFos = new FileOutputStream(copyPath);  
ObjectOutputStream writingOos = new ObjectOutputStream(writingFos);  
  
// saving/writing the list to the file  
writingOos.writeObject(names);  
  
FileInputStream readingFis = new FileInputStream(copyPath);  
ObjectInputStream readingOos = new ObjectInputStream(readingFis);  
// creating a new list with the read data from the file  
List<String> readNames = (List<String>) readingOos.readObject();  
  
System.out.println(readNames);
```

__________________________________________________________________________
## [[Java Functions]]

__________________________________________________________________________
## OOP basics

```java
// __str__() - python
@Override  
public String toString() {  
    return "Brand: " + this.brand + ", Model: " + this.model + " Horsepower: " + this.horsepower + ".";  
}
```


```java
int hash = car.hashCode(); 
//integer value which represents hashCode value for this class.
```


```java
// Constructor / __init__(self, ...)
public class Car {  
    private String brand;  
    private String model;  
    private int horsepower;  
  
    public Car(String brand, String model, int horsepower) {  
        this.brand = brand;  
        this.model = model;  
        this.horsepower = horsepower;  
    }
}
```

### Static Members
##### Static variable -> accessible and shared by all instances
- Access static members through the class name 
- Static members are shared class-wide 
- You don't need an instance
A static variable is shared among all instances of a class. When a variable is declared as static, it means:
- It belongs to the class, rather than instances of the class.
- All instances of the class share the same static variable.

```java
class BankAccount { 
	private static int accountsCount; 
	private static double interestRate; 
	
	public BankAccount() { 
		accountsCount++; 
	} 
	
	public static void setInterestRate(double rate) { 
		interestRate = rate; 
	} 
}
```

### Method Overriding 
- Наследникът предефинира поведението на класа-родител

### Method Overloading
- класът декларира методи с едно и също име с различен брой и/или тип параметри

### Returning an unmodifiableList on the getter method

```java
public class PersonDTO {  
	...
	private int age;  
    private List<Integer> lotteryNumbers;
    
	public List<Integer> getLotteryNumbers() {  
	    return Collections.unmodifiableList(lotteryNumbers);  
	}  
	  
	public void setLotteryNumbers(List<Integer> lotteryNumbers) {  
	    this.lotteryNumbers = lotteryNumbers;  
	}
	...
}
```

# Generics

## Bounded generic types

```java
public <T extends Number & Comparable> List<T> fromArrayList(T[] numbers) {
	// [...]
}
```


__________________________________________________________________________
# [[Java OOP]]