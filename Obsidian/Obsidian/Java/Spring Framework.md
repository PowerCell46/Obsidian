## Application.Properties Configuration

```python
spring.application.name=ProjectName  
  
#Data Source Properties  
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver  
spring.datasource.url=jdbc:mysql://localhost:3306/database-name?useSSL=false&createDatabaseIfNotExist=true  
spring.datasource.username=root  
spring.datasource.password=PowerCell46  
  
#JPA Properties  
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL8Dialect  
spring.jpa.properties.hibernate.format_sql = TRUE  
spring.jpa.hibernate.ddl-auto = update  
spring.jpa.open-in-view=false  
  
###Logging Levels  
# Disable the default loggers  
logging.level.org = WARN  
logging.level.blog = WARN  
  
#Show SQL executed with parameter bindings  
logging.level.org.hibernate.SQL = DEBUG  
logging.level.org.hibernate.type.descriptor = TRACE  
  
#Change server port  
#server.port=8000
```

__________________________________________________________________________
### Managing Multiple Repository Implementations with `@Primary` and `@Qualifier` in Spring Boot

```java
@Repository
@Primary -> making it the one that will be used when calling the interface <-
public class InMemoryRepository implements StudentRepository {
		...
}

@Repository
public class FileRepository implements StudentRepository {
		...
}


@Component
public class Main implements CommandLineRunner {
	private final StudentRepository studentRepository;
	
	-> telling spring exactly which one we want to use <-
	public Main(@Qualifier("FileRepository") StudentRepository sr) {
		this.studentRepository = sr;
	}
	
	@Override 
	public void run(String... args) throws Exception {
		 // Code to run on application startup 
	 }
}
```
## Spring Bean Scopes

```java
@Bean
@Scope("singleton")
public Student student() {
	return new Student();
}

@Bean -> will return a new instance every time
@Scope("prototype")
public Student student() {
	return new Student();
}
```
## Controllers

```java
@Controller
@RequestMapping("/server-side-rendering")
public class Controller {

	@GetMapping("/")  
	    public String homePage(Model model) {  
	        model.addAttribute("message", "Welcome!");  
	  
	        return "index";  
	    }
	    
	@RequestMapping(value = "/register", method = RequestMethod.GET)  
	public ModelAndView registerPage(ModelAndView modelAndView) {  
			modelAndView.addObject("title", "Hello there!");  
			modelAndView.setViewName("register");  
	  
	    return modelAndView;  
	}
	
	@PostMapping("/register") 
	public String register(UserDTO userDto) { 
		… 
	}

	@PostMapping("/cat")
	public String addCatConfirm(
		@RequestParam String catName, 
		@RequestParam String int catAge
	) {
		System.out.println(String.format("...."), ...);
		return "redirect:/cat";
	}

	@GetMapping("/details/{id}")
	public String details(@PathVariable("id") Long id) {
		...
	}

	@GetMapping("/details") 
	public String details(@RequestParam("id") Long id) { 
		//http://localhost:8080/details?id=123
		url with request parameter id
		... 
	}
}
```

### Present or Future Validation 

```java
@PresentOrFuture
private Date startDate;
```

# Events

```java
// Event
public class UserRegistrationEvent extends ApplicationEvent { 
	private String username; 
	
	public UserRegistrationEvent(Object source, String username) { 
		super(source); 
		this.username = username; 
	} 
	
	public String getUsername() { 
		return username; 
	} 
}

// Service Firing the Event
@Service public class UserRegistrationService { 

	@Autowired 
	private ApplicationEventPublisher eventPublisher; 
	
	public void registerUser(String username) 
		{ 
		// ... registration logic ... 
		eventPublisher.publishEvent(
			new UserRegistrationEvent(this, username)
		); 
	} 
}

// Listener for the Event
@Component public class UserRegistrationListener implements ApplicationListener { 
	@Override 
	public void onApplicationEvent(UserRegistrationEvent event) { 
		// ... handle event ... 
	} 
}

@Component public class UserRegistrationListener { 
	@EventListener 
	public void handleUserRegistrationEvent(UserRegistrationEvent event) { 
		// ... handle event ... 
	} 
}
```

## Transactional events 

- only published after the successful completion of a transaction.


