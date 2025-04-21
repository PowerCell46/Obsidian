# Shortcuts
### psvm

```java
public static void main(String[] args) {}
```
### sout

```java
System.out.println();
```
### sc

```java
Scanner scanner = new Scanner(System.in);
```
### for of

```java
array.for 
```

### for 

```java
fori
```

### iter

```java
for (type one : many) {
}
```

__________________________________________________________________________
### Formatting float numbers

```java
System.out.println("%.2f", 123.456);
```

## Adding a leading zero

```java
int percentage = 30;
System.out.printf("%03d", percentage);
// 030
```

### Comparing Strings
#### `a.equals(b);` - case sensitive
#### `a.equalsIgnoreCase(b);` - case insensitive

```java
public class Main {
    public static void main(String[] args) {
        String str1 = new String("Hello");
        String str2 = new String("Hello");

        if (str1.equals(str2)) {
            System.out.println("Using equals : str1 and str2 are the same");
        } else {
            System.out.println("Using equals : str1 and str2 are different");
        }
    }
}
```

### Getting an element from a string
#### ` .charAt(1) = [1];`


# Division of numbers

```java
public class Main {  
    public static void main(String[] args) {  
        System.out.println(31 / 5.0);  
    }  
}
// Returns the actual result -> 6.2

public class Main {  
    public static void main(String[] args) {  
        System.out.println(31 / 5);  
    }  
}
// Целочислено деление -> 6
```

### Getting the end values of different types of data

```java
Integer.MIN_VALUE 
Integer.MAX_VALUE

Double.MIN_VALUE
Double.MAX_VALUE

Byte.MIN_VALUE 
Byte.MAX_VALUE
```

Math.PI

__________________________________________________________________________
#### Ternary Operator

```java
int number = Integer.parseInt(sc.nextLine());  
  
System.out.println(  
        number > 0 ? "True" : "False"  
);
```

__________________________________________________________________________
# Next Course: [[Java Fundamentals]]
