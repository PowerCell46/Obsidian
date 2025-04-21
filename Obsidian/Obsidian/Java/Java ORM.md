# Working with java.sql
#### (JDBC - Java DataBase Connection)

__________________________________________________________________________
## JDBC Select query
#### [[Postgre SQL]] Connection and [[My SQL]] Connection

```java
public class Main {  
    public static void main(String[] args) throws SQLException {  
        Scanner sc = new Scanner(System.in);  

		// getting the username from the console
		System.out.print("Enter username default (postgres): ");  
        String user = sc.nextLine();  
        user = user.equals("") ? "postgres" : user;  
        System.out.println();  

		// getting the password from the console
        System.out.print("Enter password default (empty):");  
        String password = sc.nextLine().trim();  
        System.out.println();  

		// initializing the properties for the authentication
        Properties props = new Properties();  
        props.setProperty("user", user);  
        props.setProperty("password", password);  

		// Create a connection to the DB
        Connection connection = DriverManager.getConnection(
			"jdbc:postgresql://localhost:5432/springdatadb",
			props
		);  
        // MySQL: "jdbc:mysql://localhost:3306/springdatadb"


		// Writing a SELECT SQL query to the DB with a dynamic parameter
        PreparedStatement stmt =  
                connection.prepareStatement(
	                "SELECT " +  
					"   * " +  
					"FROM " +  
					"   employees " +  
					"WHERE " +  
					"   salary > ?"  
				);  

		// asking the user to enter a minimum salary
        String salary = sc.nextLine();  
        stmt.setDouble(1, Double.parseDouble(salary));

		// executing the query
        ResultSet rs = stmt.executeQuery();  

		// iterating through the results
        while(rs.next()){  
            System.out.println(rs.getString("full_name"));  
        }  


		// closing the connection to the DB
        connection.close();  
    }  
}
```

## Create Query

```java
public class Main {  
    public static void main(String[] args) throws SQLException {  

		// creating a connection to the DB
        Connection connection = DriverManager.getConnection(  
                "jdbc:mysql://localhost:3306/springdatadb",   
                "root",   
                "PowerCell46"  
        );  
  
		// CREATE SQL query
        PreparedStatement createStatement = connection.prepareStatement(  
                "INSERT INTO " +  
                        "users (first_name, last_name, salary)" +  
                        "VALUES " +  
                        "   (?, ?, ?)"  
        );  

		// adding dynamic parameters to the query
        createStatement.setString(1, "Peter");  
        createStatement.setString(2, "Gerdzhikov");  
        createStatement.setLong(3, 1200);  

		// executing the query
        int createResult = createStatement.executeUpdate();  
  
        System.out.println(createResult);  
        
        // closing the connection
        connection.close();  
    }  
}
```

## Update Query

```java
import java.sql.*;  
  
public class Main {  
    public static void main(String[] args) throws SQLException {  
	    // creating a connection to the DB
        Connection connection = DriverManager.getConnection(  
                "jdbc:mysql://localhost:3306/soft_uni",  
                "root",  
                "PowerCell46"  
        );   

		// UPDATE SQL query
        PreparedStatement updateStatement = connection.prepareStatement("" +  
                "UPDATE " +  
                "   employees " +  
                "SET " +  
                "   first_name = ? " +  
                "WHERE " +  
                "   employee_id = ?"  
        );  

		// adding the dynamic parameters
        updateStatement.setString(1, "Peter");  
        updateStatement.setLong(2, 1);  

		// executing the query
        int resultSet = updateStatement.executeUpdate();  
  
        System.out.println(resultSet);  
	    
	    // closing the connection to the DB
        connection.close();
    }  
}
```

## Delete Query

```java
public class Main {  
    public static void main(String[] args) throws SQLException {  
        Scanner sc = new Scanner(System.in);  

		// asking the user to enter a username
        System.out.print("Enter username default (root): ");  
        String user = sc.nextLine();  
        user = user.equals("") ? "root" : user;  
        System.out.println();  

		// asking the user to enter a password
        System.out.print("Enter password default (empty):");  
        String password = sc.nextLine().trim();  
        System.out.println();  

		// creating Properties with the given data
        Properties props = new Properties();  
        props.setProperty("user", user);  
        props.setProperty("password", password);  

		// creating a connection with the given Properties
        Connection connection = DriverManager  
                .getConnection("jdbc:mysql://localhost:3306/springdatadb", props);  

		// DELETE SQL query
        PreparedStatement deleteStatement = connection.prepareStatement("" +  
                "DELETE " +  
                "FROM " +  
                "   users " +  
                "WHERE " +  
                "   id = ?"  
        );  

		// adding dynamic paramers to the query
        deleteStatement.setLong(1, 1);  

		// executing the query
        int deleteResult = deleteStatement.executeUpdate();  
  
        System.out.println(deleteResult);

		// closing the connection
        connection.close();  
    }  
}
```

__________________________________________________________________________
### `POM (pom.xml)`:
##### Project Object Model


# Hibernate

##  Hibernate Session API

```java
public class Main {  
    public static void main(String[] args) {  
        Configuration configuration = new Configuration();  
        configuration.configure();  
  
        SessionFactory sessionFactory = configuration.buildSessionFactory();  
        Session session = sessionFactory.openSession();  
  
        session.beginTransaction();  


        // CREATE Entity;  
        Student student = new Student(2, "Ivan");  
        session.save(student);  
  
        // SELECT by ID;
        Student student = session.get(Student.class, 1);  
        System.out.println(student);  
  
        // UPDATE Entity;
        student.setName("Peter");  


        List<Student> students = session.createQuery(
	        "FROM Student", Student.class
        ).list();  
  
        for (Student student1 : students) {  
            System.out.println(student1);  
        }  


        session.getTransaction().commit();  
        session.close();  
    }  
} 
```

## Defining Entity with XML configuration
#### Student.hbm.xml

```xml
<?xml version="1.0" encoding="utf-8" ?>  
<!DOCTYPE hibernate-configuration  
        PUBLIC "-//Hibernate/Hibernate Configuration DTD//EN"  
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">  
  
<hibernate-mapping>  
    <class name="entities.Student" table="students">  
        <id name="id" column="id">  
            <generator class="identity"/>  
        </id>        
        <property name="name" column="name"/>  
	</class>
</hibernate-mapping>
```
## Student Class (Entity)

```java
@Getter
@Setter
public class Student {  
    private int id;  
    private String name;  
  
    public Student(int id, String name) {  
        this.setId(id);  
        this.setName(name);  
    }  
  
    public Student() {  
    }  
  
    @Override  
    public String toString() {  
        return String.format(  
			"Student with id: %d, name: %s",  
			this.getId(),  
			this.getName()  
		);  
    }  
}
```

__________________________________________________________________________
## Java Persistence API (JPA) with EntityManager

```java
package bg.softuni;  
  
import entities.Teacher;  
  
import javax.persistence.EntityManager;  
import javax.persistence.EntityManagerFactory;  
import javax.persistence.Persistence;  
  
public class Main {  
    public static void main(String[] args) {  
        EntityManagerFactory emf = Persistence  
                .createEntityManagerFactory("general"); 
                // name of the config in ../resources/META-INF/persistence.xml 
  
        EntityManager em = emf.createEntityManager();  
        em.getTransaction().begin();  

	
//          INSERT query  
	        Teacher teacher = new Teacher("Stelian");  
	  
	        em.persist(teacher);  
	  
//          SELECT query
	        Teacher firstTeacher = em.find(Teacher.class, 1);  
	  
	        System.out.println(firstTeacher);  


        em.getTransaction().commit();  

        em.close();  
        emf.close();  
    }  
}
```

## Defining Entity with Annotation

```java
@Entity  
@Table(name = "teachers")  
@Getter  
@Setter  
@NoArgsConstructor  
@AllArgsConstructor 
@ToString
public class Teacher {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private int id;  
  
    @Column  
    private String name;  
  
    @Column  
    private int age; 

	@Transient // the value is not stored in the DB
	private String workingSchoolName;
  
    public Teacher(String name, int age) {  
        this.name = name;  
        this.age = age;  
    }  
} 
```

__________________________________________________________________________
# JPA Inheritance

## MappedSuperclass & @GeneratedValue

```java
@MappedSuperclass 
// --> Class doesn't have its own table & entity  
public class Base {  
  
    @Id
	@GeneratedValue(strategy = GenerationType.AUTO)

    @GeneratedValue(strategy = GenerationType.IDENTITY) 
    // --> AutoIncrement PRIMARY KEY
      
	@GeneratedValue(strategy = GenerationType.TABLE) 
	// --> Creating an Extra Table keeping track of the next ID (in case Of Multiple Inheritance)  
	
	@GeneratedValue(strategy = GenerationType.SEQUENCE,
	 generator = "employee_seq_generator") 
	 // --> AutoIncrement PRIMARY KEY with Custom Initial Value  
	
	@SequenceGenerator(name = "employee_seq_generator", 
	sequenceName = "employee_seq", initialValue = 100, allocationSize = 50)  

	private long id;  
  
    @Column  
    private String type;  
  
    public Base() {  
    }  
  
    public Base(String type) {  
        this.type = type;  
    }  
}
```

## Inheritance strategies

```java
@Entity  
@Table(name = "vehicles")  

@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS) 
// --> Every child has its own table (parent's columns + its own columns) //(GeneratedValue(strategy = TABLE))

@Inheritance(strategy = InheritanceType.JOINED) --> Best Choice // slower
// --> Shared table for the parent Entity, own tables for the children with each of their own columns 
//(GeneratedValue (strategy = TABLE; IDENTITY))

@Inheritance(strategy = InheritanceType.SINGLE_TABLE) --> Default
// --> Shared table in case of custom columns for a child - added to the main table with null values for other children  
@DiscriminatorColumn(name = "vehicle_type") 
// --> For SINGLE_TABLE (name of the column defining the child type)  

public class Vehicle extends Base {  
  
    private String model;  
  
    private BigDecimal price;  
  
    private String fuelType;  
  
    protected Vehicle() {}  
  
    public Vehicle(String type, String model, BigDecimal price, String fuelType) {  
        super(type);  
        this.model = model;  
        this.price = price;  
        this.fuelType = fuelType;  
    }  
}
```

__________________________________________________________________________
# JPA Relationships

## One to One Relationship

```java
@Entity  
@Table(name = "plate_numbers")  
public class PlateNumber {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private long id;  
  
    @Column  
    private String number;  
  
    @OneToOne(targetEntity = Car.class, mappedBy = "plateNumber")  
    private Car car;  
  
    public PlateNumber(String number) {  
        this.number = number;  
    }  
  
    public PlateNumber() {  
  
    }  
}


@Entity  
@Table(name = "cars")  
@DiscriminatorValue("car") 
// --> For SINGLE_TABLE (name of the value for the defining column for the current Entity)  
public class Car extends Vehicle {  
  
    private static final String TYPE = "CAR";  
  
    @Column  
    private int seats;  
  
    @OneToOne // (optional=false) 
    // --> A column with the id of the Plate Number  
    @JoinColumn(name = "plate_id", referencedColumnName = "id") 
    // --> optional annotation (for changing the column name)  
    private PlateNumber plateNumber;  
  
    public Car() {}  
  
    public Car(String model, BigDecimal price, String fuelType, int seats, PlateNumber plateNumber) {  
        super(TYPE, model, price, fuelType);  
        this.seats = seats;  
        this.plateNumber = plateNumber;  
    }  
}
```

## One To Many - Many To One

```java
@Entity  
@Table(name = "planes")  
@DiscriminatorValue("plane") 
// --> For SINGLE_TABLE (name of the value for the defining column for the current Entity)  
public class Plane extends Vehicle {  
  
    private static final String TYPE = "PLANE";  
  
    @Column(name = "passenger_capacity")  
    private int passengerCapacity;  
  
    @ManyToOne  
    @JoinColumn(name = "company_owner_id", referencedColumnName = "id")  
    private Company companyOwner;  
  
    public Plane() {}  
  
    public Plane(String model, BigDecimal price, String fuelType, int passengerCapacity, Company company) {  
        super(TYPE, model, price, fuelType);  
        this.passengerCapacity = passengerCapacity;  
        this.companyOwner = company;  
    }  
}


@Entity  
@Table(name = "companies")  
public class Company {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private long id;  
  
    @Column  
    private String name;  
  
    @OneToMany(targetEntity = Plane.class, mappedBy = "companyOwner", fetch = FetchType.LAZY)  
    private List<Plane> planes;  
  
    public Company() {  
        this.planes = new ArrayList<>();  
    }  
  
    public Company(String name) {  
        this.name = name;  
        this.planes = new ArrayList<>();  
    }  
}
```

## Many To Many

```java
@Entity  
@Table(name = "trucks")  
@DiscriminatorValue("truck") 
// --> For SINGLE_TABLE (name of the value for the defining column for the current Entity)  
public class Truck extends Vehicle {  
  
    private static final String TYPE = "TRUCK";  
  
    @Column(name = "load_capacity")  
    private double loadCapacity;  
  
    @ManyToMany  
    @JoinTable(name = "trucks_drivers",  
            joinColumns = @JoinColumn(
	            name = "truck_id", referencedColumnName = "id"),  
	            inverseJoinColumns = @JoinColumn(
	            name = "driver_id", referencedColumnName = "id")
            )  
    private List<Driver> drivers;  
  
    public Truck() {  
        this.drivers = new ArrayList<>();  
    }  
  
    public Truck(String model, BigDecimal price, String fuelType, double loadCapacity, List<Driver> drivers) {  
        super(TYPE, model, price, fuelType);  
        this.loadCapacity = loadCapacity;  
        this.drivers = drivers;  
    }  
}


@Entity  
@Table(name = "drivers")  
public class Driver {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private long id;  
  
    @Column(name = "full_name")  
    private String fullName;  
  
    @ManyToMany(mappedBy = "drivers")  
    List<Truck> trucks;  
  
    public Driver() {}  
  
    public Driver(String fullName) {  
        this.fullName = fullName;  
    }  
}
```

__________________________________________________________________________
# Spring Data

#### Spring boot configurations are held in an `application.properties`

## Repository

```java
@Repository
public interface StudentRepository extends JpaRepository<Student, Long> {
	
	Set<Student> findByMajor(String major);

	Set<Student> findByMajorAndName(String major, String name);

	Set<Student> getAllBySizeOrderById(Size sizeValue);

	@Query(value = "select s from Shampoo s join s.ingredients i where i in :ingredients")
	Set<Shampoo> findByIngredientsIn(
		@Param(value="ingredients") 
		Set<Ingredient> ingredients
	);
}
```
## Service

```java
@Service
public class StudentServiceImpl implements StudentService {
	private StudentRepository studentRepository;

	@Override
	public void register(Student student) {
		studentRepository.save(student);
	}

	public void expel(Student student) {
		studentRepository.delete(student);
	}
}
```

#### (JPQL) Java Persistence Query Language

SQL-like Syntax working with Entities and their properties

```java
"SELECT i FROM Ingredient i WHERE i.name IN :names"

"UPDATE Ingredient i SET i.price = i.price * 1.10 WHERE i.name IN :names"

"DELETE FROM Ingredient i WHERE i.name = :name"

@Query("SELECT s FROM Shampoo s JOIN s.ingredients i WHERE i IN :ingredients")
List<Shampoo> findByIngredientsIn(Set<Ingredient> ingredients);
```
## isPresent()

```java
...
Optional<User> user = this.userRepository.findById(id);

if (user.isPresent()) {
	...
}
...
```

__________________________________________________________________________
# DTO - Data Transfer Object

#### Model Mapper

- Simple Mapping - Automatically maps the properties by their names.
- Explicit Mapping... - Custom mapping....
#### Validation

```java
ModelMapper modelMapper = new ModelMapper();
modelMapper.createTypeMap(EmployeeDTO.class, Employee.class);

modelMapper.validate();
```
#### Skipping Properties

#### Converting Type of Properties

__________________________________________________________________________
# Working with JSON

### Gson

- excludeFieldsWithoutExposeAnnotation() - All properties without @Expose are not included in the translation process.
- setPrettyPrinting() - Format the JSON
### JSON -> DTO

```java  
imports...

@Component  
public class MainJSON implements CommandLineRunner {  
    @Override  
    public void run(String... args) throws Exception { 
     
        Gson gson = new GsonBuilder()
        .setPrettyPrinting()
        .create();  
  
        String JSONData = """  
                {
                "firstName": "Peter",
			    "lastName": "Gerdzhikov",
				"age": 25,
			    "isMarried": false,
				"lotteryNumbers": [],
			    "address": {
					"country": "Bulgaria",
					"city": "Sofia"
			    }
			}                
		""";
		  
		// Parse JSON object to PersonDTO entity object
        PersonDTO newP = gson.fromJson(JSONData, PersonDTO.class);  
  
        System.out.println(newP.getFirstName());  
    }  
}
```
### DTO -> JSON

```java
@Getter
@Setter
@AllArgsConstructor
public class AddressDTO implments Serializable {
	
	@Expose
	private String country;

	@Expose
	private String city;

	@Expose
	private String street;
}

Gson gson = new GsonBuilder()
	.excludeFieldsWithoutExposeAnnotation()
	.setPrettyPrinting()
	.create();  

AddressDTO addressDTO = new AddressDTO("Bulgaria", "Sofia", "Stefan Volev");
// JSON stringify the Object Entity
String jsonObjectData = gson.toJson(addressDTO);
```
### Reading and Parsing JSON from an API

```java
imports...

@Component  
public class MainJSON implements CommandLineRunner {  
    @Override  
    public void run(String... args) throws Exception {  
 
		Gson gson = new GsonBuilder()
			.setPrettyPrinting()
			.create();  
  
        String urlEndpoint = "https://jsonplaceholder.typicode.com/users";  
        URL url = new URL(urlEndpoint);  
        
        HttpURLConnection conn = (HttpURLConnection) url.openConnection(); 
         
        conn.setRequestMethod("GET");  
        conn.setRequestProperty("Accept", "application/json");  
  
        if (conn.getResponseCode() != 200) {  
            throw new RuntimeException(
	            "Failed : HTTP error code : " + conn.getResponseCode()
            );  
        }  
  
        try (BufferedReader br = new BufferedReader(
	        new InputStreamReader(conn.getInputStream()))
        ) {  
        
            JsonPlaceHolderPersonDTO[] jsonPlaceHolderPersonDTOS =
             gson.fromJson(br, JsonPlaceHolderPersonDTO[].class);  
            
            for (JsonPlaceHolderPersonDTO person : jsonPlaceHolderPersonDTOS) {  
                System.out.println(gson.toJson(person));  
            }  
            
        } catch (Exception e) {  
            e.printStackTrace();  
        } finally {  
            conn.disconnect();  
        }  
    }  
}
```

__________________________________________________________________________
# Working with XML

### Jaxb Library

- Marshalling - Java Object -> XML 
- Unmarshalling - XML -> Java Object
### CarRootDTO & CarDTO

```java
// Parent element

@Getter  
@Setter  
@XmlRootElement(name = "cars")  
@XmlAccessorType(XmlAccessType.FIELD)  
@Builder  
public class CarRootDTO {  
  
    @XmlElement(name = "car")  
    private List<CarSeedDTO> cars;  
}

// Child element/s

@Getter  
@Setter  
@XmlAccessorType(XmlAccessType.FIELD)  
@Builder
public class CarDTO {  
  
    @XmlElement  
    private String make;  
  
    @XmlElement  
    private String model;  
  
    @XmlElement(name = "travelled-distance")  
    private long travelledDistance;  

	@XmlAttribute
	private BigDecimal price;
}
```
## Unmarshalling & Marshalling 

```java
// Unmarshalling
JAXBContext jaxbContext = JAXBContext.newInstance(CarRootSeedDTO.class);  
Unmarshaller unmarshaller = jaxbContext.createUnmarshaller();  


CarRootSeedDTO carRootSeedDTO = (CarRootSeedDTO) 
	unmarshaller.unmarshal(new File(FILE_PATH));


// Marshalling
JAXBContext context = JAXBContext.newInstance(User.class); 
Marshaller marshaller = context.createMarshaller(); 
marshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);

marshaller.marshal(user, new File("users.xml"));
```

__________________________________________________________________________
# [[Spring Framework]]