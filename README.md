# EXP 03-Entity-Student-and-build-a-CRUD-operations-using-Spring-Boot-Hibernate-Configuration
### Name: Tarun S
### Reg No: 212223040226
## AIM:
To develop a Spring Boot application that performs CRUD (Create, Read, Update, Delete) operations on a Student entity using Spring Data JPA (Hibernate).

## ALGORITHM:
Create Spring Boot Project

Add dependencies: Spring Web, Spring Data JPA, H2 Database or MySQL, Spring Boot DevTools

Configure application.properties

Define database connection

Enable Hibernate auto DDL

Create Student Entity Class

Annotate with @Entity

Define fields with @Id, @GeneratedValue, etc.

Create StudentRepository

Extend JpaRepository<Student, Long> for CRUD methods

Create StudentController

Handle HTTP methods:

POST /students → Add student

GET /students → Get all students

GET /students/{id} → Get student by ID

PUT /students/{id} → Update student

DELETE /students/{id} → Delete student

## PROGRAM CODE

### pom.xml

```
  <dependencies>
        <!-- Spring Web for REST controllers -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- Spring Data JPA for database operations -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <!-- H2 Database for in-memory DB and H2 console -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!-- Spring Boot DevTools for hot reloads -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- Spring Boot Test starter -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

### application.properties

```
spring.application.name=ajw-exp-3
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
spring.h2.console.enabled=true

```

### Student.java
```
package com.example.ajw.exp_3;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
    private String course;

    public Student() {
    }

    public Student(String name, String email, String course) {
        this.name = name;
        this.email = email;
        this.course = course;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getCourse() {
        return course;
    }

    public void setCourse(String course) {
        this.course = course;
    }
}
```

### StudentRepository.java
```
package com.example.ajw.exp_3;

import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student, Long> {
}
```
### StudentController.java
```
package com.example.ajw.exp_3;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/students")
public class StudentController {

    @Autowired
    private StudentRepository studentRepository;

    @PostMapping
    public Student addStudent(@RequestBody Student student) {
        return studentRepository.save(student);
    }

    @GetMapping
    public List<Student> getAllStudents() {
        return studentRepository.findAll();
    }


    @GetMapping("/{id}")
    public Student getStudentById(@PathVariable Long id) {
        return studentRepository.findById(id)
                .orElseThrow(() -> new RuntimeException("Student not found"));
    }


    @PutMapping("/{id}")
    public Student updateStudent(@PathVariable Long id, @RequestBody Student studentDetails) {
        Student student = studentRepository.findById(id)
                .orElseThrow(() -> new RuntimeException("Student not found"));

        student.setName(studentDetails.getName());
        student.setEmail(studentDetails.getEmail());
        student.setCourse(studentDetails.getCourse());

        return studentRepository.save(student);
    }
    @DeleteMapping("/{id}")
    public String deleteStudent(@PathVariable Long id) {
        studentRepository.deleteById(id);
        return "Student with ID " + id + " deleted successfully!";
    }
}
```
### AjwExp3Application.java
```
package com.example.ajw.exp_3;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class AjwExp3Application {

	public static void main(String[] args) {
		SpringApplication.run(AjwExp3Application.class, args);
	}

}
```

# Output:
### POST
<img width="1920" height="1080" alt="exp3 postmapping" src="https://github.com/user-attachments/assets/05d110bc-67e4-434d-979a-ff6f8c866850" />

### GET
<img width="1920" height="1080" alt="exp3 getmapping" src="https://github.com/user-attachments/assets/6ce467af-fe46-421e-b20e-2a58ba0a5cf7" />

### Get by Id
<img width="1920" height="1080" alt="exp3 getmappingbyid" src="https://github.com/user-attachments/assets/4a5a3631-3778-44e1-9c2b-7c0e2d8110ef" />

### PUT 
<img width="1920" height="1080" alt="exp3 putmapping" src="https://github.com/user-attachments/assets/6edcfdad-b345-463e-b2f4-58c066a47535" />

### DELETE
<img width="1920" height="1080" alt="exp3 deletemapping" src="https://github.com/user-attachments/assets/d31435c5-72d3-4d0a-b17b-a17e8a5c7f77" />

### h2-console
<img width="1920" height="1080" alt="exp3 h2-console" src="https://github.com/user-attachments/assets/e9639092-d5b5-4d9e-b781-a4c202184ae5" />

# Result:
The Spring Boot application for performing CRUD operations on the Student entity using Spring Data JPA (Hibernate) and an in-memory H2 database was successfully developed, executed, and verified using Postman API requests and the H2 Web Console.
