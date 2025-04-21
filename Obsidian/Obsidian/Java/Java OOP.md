
### Adding a leading zero

```java
int number = 7;
String formattedNumber = String.format("%02d", number);
System.out.println("Formatted number: " + formattedNumber);
```
## String Interpolation

```java
int x = 10;
int y = 20;

String s = STR."\{x} + \{y} = \{x + y}";
```
# Enums 

```java
enum Day {  
	MON,  
	TUE,  
	WED ,  
	THU,  
	FRI,  
	SAT,  
	SUN;  
    };

...
// getting the index of an enum
DAY.MON.ordinal()
```

## Enum with custom values

```java
public enum CarType {  
  
    SEDAN("Sedan", 20_000),  
    SUV("SUV", 30_000),  
    TRUCK("Truck", 40_000),  
    COUPE("Coupe", 50_000);  
  
    private final String name;  
  
    private final int price;  
  
    CarType(String name, int price) {  
        this.name = name;  
        this.price = price;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public int getPrice() {  
        return price;  
    }  
}
```

## Making mutable objects unmutable
in order to protect them from a foreign change

```java
class Team { 
	private List players; 
	
	public void addPlayer(Person person) { 
		this.players.add(person); 
	} 
	
	public List getPlayers() { 
		return Collections.unmodifiableList(players); 
	} 
}
```

## Final keyword

#### A final class can't be extended

```java
public final class Mammal extends Animal {}
```

#### A final method can't be overridden

```java
public final void move(Point point) {}
```

#### The final variable value can't be changed once it is set

```java
private final String name; 
private final List firstTeam;
```
# Interfaces

```java
public interface Car {  

    int TIRES = 4;  
    
    String getModel();  
    
    String getColor();  
    
    Integer getHorsePower();  
    
    String countryProduced();  
}
```
# Abstract Classes

```java
public abstract class BasePerson implements Person { 
	private String name; 
	protected BasePerson(String name) {
		this.setName(name); 
	} 
	
	private void setName(String name) { 
		this.name = name; 
	} 
	
	@Override 
	public String getName() { 
		return this.name; 
	} 
}
```

# [[Java ORM]]