Business Rules Management System
Involves writing and managing rules (in DRL files or other formats) that are then executed against facts/data in a rule engine.

```xml

<dependencyManagement>  
    <dependencies>        
	    <dependency>            
	    <groupId>org.drools</groupId>  
	            <artifactId>drools-bom</artifactId>  
	            <type>pom</type>  
	            <version>7.5.0.Final</version>  
	            <scope>import</scope>  
		</dependency>    
	</dependencies>
</dependencyManagement>

</dependency>  
<!-- Drools rule engine -->  
<dependency>  
    <groupId>org.kie</groupId>  
    <artifactId>kie-api</artifactId>  
</dependency>  
<dependency>  
    <groupId>org.drools</groupId>  
    <artifactId>drools-compiler</artifactId>  
    <scope>runtime</scope>  
</dependency>  
<!-- Needed for logging of drools-compiler -->  
<dependency>  
    <groupId>com.thoughtworks.xstream</groupId>  
    <artifactId>xstream</artifactId>  
    <version>1.4.10</version>  
</dependency>
```