## Function / BiFunction
- Function - one parameter and a return type
- BiFunction - at least 2 parameters and a return type

```java
// Function -> only one parameter  
Function <Integer, Double> getSquareRoot = x -> Math.sqrt(x);  
  
System.out.println(getSquareRoot.apply(25));  

// ---------------------------------------------------------------------------- //

// BiFunction -> two parameters  
BiFunction<Integer, Integer, Integer> sumNums = (x, y) -> x + y;  
  
System.out.println(sumNums.apply(5, 10));


DoubleUnaryOperator addVat = x -> x * 1.2;
```

## Consumer / BiConsumer
- Consumer - Void function taking one parameter
- BiConsumer - Void function taking at least 2 parameters

```java
int[] nums = {1, 2, 3, 4};  
  
Consumer<int[]> printNumbers = numbers -> Arrays.stream(numbers)  
	.forEach(number ->  
			System.out.print(number + " ")  
	);  
  
printNumbers.accept(nums);

List<int[]> matrix = new ArrayList<>();  


// ---------------------------------------------------------------------------- //

List<String[]> matrix = new ArrayList<>();

int[] row1 = {1, 2};  
int[] row2 = {3, 4};  
int[] row3 = {5, 6};  
  
matrix.add(row1);  
matrix.add(row2);  
matrix.add(row3);  
  
BiConsumer<Integer, Integer> printSum = (num1, num2) -> System.out.format("%d + %d = %d\n",  
	num1,  
	num2,  
	num1 + num2  
);  
  
matrix.stream().forEach(row -> printSum.accept(row[0], row[1]));
```

## Supplier 
- Function without parameters returning a certain value

```java
Supplier<Integer> getRandomInt = () -> new Random().nextInt(51);  
  
int rnd = getRandomInt.get();
```


## Predicate / BiPredicate
- Predicate - Function returning a boolean after evaluating the given parameter
- BiPredicate - Function returning a boolean after evaluating the given parameters

```java
Predicate<String> isLengthEven = string -> string.length() % 2 == 0;  
  
System.out.println(isLengthEven.test("Peter"));


// ---------------------------------------------------------------------------- //


List<String[]> matrix = new ArrayList<>();  
  
String[] row1 = {"Ivan", "Peter"};  
String[] row2 = {"Stiliyan", "Peter"};  
String[] row3 = {"Ivan", "Stiliyan"};  
  
matrix.add(row1);  
matrix.add(row2);  
matrix.add(row3);  
  
BiPredicate<String, String> areNamesWithEvenLen = (name1, name2) -> {  
    if (name1.length() % 2 == 0 && name2.length() % 2 == 0) {  
        return true;  
    }  
    return false;  
};  
  
matrix.stream()  
    .filter(arr -> areNamesWithEvenLen.test(arr[0], arr[1]))  
    .forEach(arr -> System.out.format("%s - %s", arr[0], arr[1]));
```

## Passing a function as a parameter

```java
import java.util.function.Function;  
  
public class Main {  
    public static void main(String[] args) {  
  
        int number = 50;  
  
        int b = operation(5, n -> n * 2);  
        int c = operation(5, n -> n % 2);  
        int d = operation(5, n -> n + 2);  
    }  
  
    static int operation(int number, Function<Integer, Integer> function) {  
        return function.apply(number);  
    }  
}
```