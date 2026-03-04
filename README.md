# 2400033034_FSAD_Skill_exp-7

## Skill 7: REST API CRUD Operations using ResponseEntity - FSAD Lab Experiment

### Overview
Demonstrates REST API development with proper HTTP status codes using Spring's ResponseEntity.

### Entity Class

```java
@Entity
@Table(name = "employees")
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(nullable = false, unique = true)
    private String email;
    
    @Column
    private String role;
    
    @Column
    private Double salary;
    
    // Getters and setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    // ... more getters/setters
}
```

### Repository

```java
@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Long> {
    List<Employee> findByRole(String role);
}
```

### REST Controller with ResponseEntity

```java
@RestController
@RequestMapping("/api/employees")
public class EmployeeController {
    @Autowired
    private EmployeeRepository employeeRepository;
    
    // CREATE - POST
    @PostMapping
    public ResponseEntity<Employee> createEmployee(@RequestBody Employee employee) {
        Employee savedEmployee = employeeRepository.save(employee);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedEmployee);
    }
    
    // READ ALL - GET
    @GetMapping
    public ResponseEntity<List<Employee>> getAllEmployees() {
        List<Employee> employees = employeeRepository.findAll();
        if (employees.isEmpty()) {
            return ResponseEntity.status(HttpStatus.NO_CONTENT).build();
        }
        return ResponseEntity.ok(employees);
    }
    
    // READ BY ID - GET
    @GetMapping("/{id}")
    public ResponseEntity<Employee> getEmployeeById(@PathVariable Long id) {
        Optional<Employee> employee = employeeRepository.findById(id);
        if (employee.isPresent()) {
            return ResponseEntity.ok(employee.get());
        }
        return ResponseEntity.status(HttpStatus.NOT_FOUND).build();
    }
    
    // UPDATE - PUT
    @PutMapping("/{id}")
    public ResponseEntity<Employee> updateEmployee(
            @PathVariable Long id,
            @RequestBody Employee employeeDetails) {
        Optional<Employee> employee = employeeRepository.findById(id);
        if (!employee.isPresent()) {
            return ResponseEntity.status(HttpStatus.NOT_FOUND).build();
        }
        Employee emp = employee.get();
        emp.setName(employeeDetails.getName());
        emp.setEmail(employeeDetails.getEmail());
        emp.setRole(employeeDetails.getRole());
        emp.setSalary(employeeDetails.getSalary());
        
        Employee updatedEmployee = employeeRepository.save(emp);
        return ResponseEntity.ok(updatedEmployee);
    }
    
    // DELETE - DELETE
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteEmployee(@PathVariable Long id) {
        Optional<Employee> employee = employeeRepository.findById(id);
        if (!employee.isPresent()) {
            return ResponseEntity.status(HttpStatus.NOT_FOUND).build();
        }
        employeeRepository.deleteById(id);
        return ResponseEntity.status(HttpStatus.NO_CONTENT).build();
    }
}
```

### HTTP Status Codes Used
- **200 OK**: Successful retrieval or update
- **201 CREATED**: Resource created successfully
- **204 NO_CONTENT**: Successful deletion, no content returned
- **400 BAD_REQUEST**: Invalid request data
- **404 NOT_FOUND**: Resource not found
- **500 INTERNAL_SERVER_ERROR**: Server error

### API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/employees | Create new employee |
| GET | /api/employees | Get all employees |
| GET | /api/employees/{id} | Get employee by ID |
| PUT | /api/employees/{id} | Update employee |
| DELETE | /api/employees/{id} | Delete employee |

### ResponseEntity Advantages
- Full control over HTTP status codes
- Custom headers support
- Response body control
- Exception handling

### pom.xml Dependencies

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

### Assessment Criteria
- [ ] Entity class properly configured
- [ ] Repository extending JpaRepository
- [ ] All CRUD endpoints implemented
- [ ] Correct HTTP status codes used
- [ ] ResponseEntity properly utilized
- [ ] Error handling implemented
- [ ] Code tested with REST client

### Testing with cURL

```bash
# Create
curl -X POST http://localhost:8080/api/employees \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com","role":"Developer","salary":50000}'

# Read All
curl http://localhost:8080/api/employees

# Read by ID
curl http://localhost:8080/api/employees/1

# Update
curl -X PUT http://localhost:8080/api/employees/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"Jane","email":"jane@example.com","role":"Manager","salary":60000}'

# Delete
curl -X DELETE http://localhost:8080/api/employees/1
```

### References
- [Spring Data JPA Documentation](https://spring.io/projects/spring-data-jpa)
- [ResponseEntity Guide](https://www.baeldung.com/spring-response-entity)
- [REST API Best Practices](https://restfulapi.net/http-status-codes/)

### Author
**Student ID:** 2400033034
**Experiment:** 7 - REST API CRUD Operations
**Last Updated:** March 2026
