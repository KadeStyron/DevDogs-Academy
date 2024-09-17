# Introduction to Spring Boot

## Prerequisites 

This tutorial assumes that the reader has basic knowledge of Java and the structure of web applications. To learn more about Spring, you should view this Spring article. To best learn, follow along with the code examples.

## What is Spring Boot

Spring Boot is built on top of the Spring framework. It's used to set up, configure, and run web-based applications. Each Spring Boot application also contains an embedded HTTP server, which allows us to create our own API for the frontend to use.

## Why use Spring Boot

Spring Boot makes production-ready applications quickly and efficiently. It provides all of the features of Spring but is also easier to use. Everything is auto-configured in Spring Boot, minimizing the writing of boilerplate code and XML configurations.

### Additional Spring Boot Advantages

1. Connecting to databases
2. Spring Boot provides application metrics
3. There are security modules available
4. Easy to learn
5. So much more

## How to start your Spring Boot application

1. Go to start.spring.io
2. Fill in your settings:
    - **Project**: Maven
    - **Language**: Java
    - **Version**: Default
    - **Group ID**: Reverse of your domain name (e.g., `com.devdogs.schedulebuilder`)
    - **Artifact ID**: Lowercase letters with hyphens (e.g., `optimal-schedule-builder`)
    - **Package Name**: Same as Group ID
    - **Dependencies**: 
        - Spring Web for this demo 
        - We will definitely use more for the project
3. Select Jar packaging and your Java version
4. Generate and extract the zip file
5. Open your project with your IDE

> For this demo, I will be keeping everything as the default settings and adding Spring Web.

## How to implement a REST API

### Starting Your First Local Server!

Go to your `SpringBootApplication` class. This should come by default. Add `@RestController` above your class. Then you need to add a REST endpoint. This endpoint can be any method with the annotation `@GetMapping` above the class.

Your code should look something like this:

```
@SpringBootApplication
@RestController
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

    @GetMapping
    public String hello() {
        return "Hello World!";
    }
}
```

Now your server should be ready to run! All you need to do is hit run on your DemoApplication class and type in `localhost:8080`.

### JSON Formatting

We can return more than Strings. We will likely need to provide the frontend with a JSON at some point. This is how to make your Hello World! a JSON.

```
	@GetMapping
	public List<String> hello() {
		return List.of("Hello", "World!");
	}
```
## Returning an API
In this example we will create a student class to return.

1. Create a new package for the student class `com.example.demo.student`.

2. Create a Student class and put in the class variables as well as basic methods (getters, setters, constructor, and toString(). I added ...
```     
    private long id;
    private String name;
    private String email;
    private LocalDate dob;
    private Integer age;
```
> Pro tip, highlight the code above and hit the lightbulb to generate the getters, setters, and constructors on vscode. Use the code menu if you're on IntelliJ.

Now we can change our endpoint to return a custom object. Change the `List<String>` into a `List<Student>` in our demoApplication class.

Replace the "Hello World!" Strings with a new Student.

```
@GetMapping
public List<Student> hello() {
    return List.of(
        new Student(
            1L, 
            "John",
            "johnsmith@gmail.com",
            LocalDate.of(2005, 1, 10),
            19
        )		
    );
}
```

Awesome! Your output should look like a JSON of a student object.
---

## API Layer Class

Inside of using Demo Application create a StudentController.java class inside of the student file.
Move everything inside the getMapping annotation from DemoApplication to StudentController. Also move the @RestController annotation.  

Additionally add the code `@RequestMapping(path = "api/v1/student")` to try custom paths api calls. 

Finally change hello() to getStudents() since thats the correct nomenclature. 

New StudentController class: 

```
@RestController
@RequestMapping(path = "api/v1/student")
public class StudentController {
    @GetMapping
    public List<Student> getStudents() {
        return List.of(
            new Student(
                1L, 
                "John",
                "johnsmith@gmail.com",
                LocalDate.of(2005, 1, 10),
                19
            )		
        );
    }
}
```

Now when we run our demo application we can type `http://localhost:8080/api/v1/student` into our browser to get the same output.

Why is this important: This lets the frontend call different methods from calling different links. This gives us much more flexibility with all of the information we'll send to frontend and makes it much more organized. 


# Business/Service Layer 

This layer is responsible for holding all of our methods. We can put all of our methods like getStudents into a service class called StudentService. Since it is a Spring service annotate it with @Service. This will be important for dependency injections. 


```
@Service
public class StudentService {

    public List<Student> getStudents() {
		return List.of(
			new Student(
				1L, 
				"John",
				"johnsmith@gmail.com",
				LocalDate.of(2005, 1, 10),
				19
			)		
		);
	}
}
```

Since we have our methods in here, studentController just has to read an input. create a new student service class and call the method like so. 

```
@RestController
@RequestMapping(path = "api/v1/student")
public class StudentController {

    private final StudentService studentService;

    public StudentController(StudentService studentService) {
        this.studentService = new StudentService();
    }

    @GetMapping
	public List<Student> getStudents() {
		return studentService.getStudents();
	}
}
``` 

This collection method of separating the Student, StudentService, and StudentController class is the best way to maintain an organized Spring Boot project. 

## Dependency Injection

While `this.studentService = new StudentService();` works we should avoid using this approach and instead use dependency injections. Dependency injections are easier to test, and properly manages the lifecycle of `@Autowired` (annotation for dependency injection) objects. Spring handles the creation, initialization, and destruction of these objects so you don't need to manually do these things. 

Implementing dependency injections is really easy. Place the annotation @Autowired on top of the constructor and replace `this.studentService = new StudentService();` with `this.student = studentService;`. This will allow Spring to automatically instantiate the @Components.

> @Service is the same thing as @Component. It's used over component for semantics only. 


## Using optional parameters 

An advantage of Spring Boot is it can read variables in the URL it is given. For example if we want to get certain students instead of all of our students we can use this modified Student controller getStudents() to filter our students collected if we want.

@RequestParam is used to define query parameters. required = false makes these parameters optional. If an optional parameter is not provided it will default to null.

```
@GetMapping
public List<Student> getStudents(
        @RequestParam(required = false) String name,
        @RequestParam(required = false) Integer age) {
    return studentService.getStudents(name, age);
}
``` 

For the example modify your Student service to the following. This is just to show the example, you don't need to know how every method works.

```
    private List<Student> students = List.of(
        new Student(1L, "John", "johnsmith@gmail.com", LocalDate.of(2005, 1, 10), 19),
        new Student(2L, "Jane", "janedoe@gmail.com", LocalDate.of(2003, 5, 15), 21),
        new Student(3L, "Mike", "mikejones@gmail.com", LocalDate.of(2004, 8, 20), 20)
);

public List<Student> getStudents(String name, Integer age) {
    return students.stream()
            .filter(student -> (name == null || student.getName().equalsIgnoreCase(name)) &&
                                (age == null || student.getAge().equals(age)))
            .collect(Collectors.toList());
}
```

This shows how you can use optional variables to get different data. Use these examples to try it out.


http://localhost:8080/api/v1/student should return all students. \\
http://localhost:8080/api/v1/student?name=John should return all students named John. \\
http://localhost:8080/api/v1/student?age=20 should return all 20 year old students. \\
http://localhost:8080/api/v1/student?name=Jane&age=21 should return all 21 year old students named Jane. \\

If these examples don't work you may need to edit your xml file. Try adding this in your build section.

```
<plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <parameters>true</parameters>
            </configuration>
        </plugin>
    </plugins>
```


---

There's so much more to Spring Boot, especially for database access and management. For our project we will have a team devoted to this so it is not currently needed. I will update this tutorial if needed, but feel free to update/edit this document on our DevDogs Academy repository!








## Sources: 

This is the **[video where I obtained the example](https://www.youtube.com/watch?v=9SGDpanrc8U)**. \\

**[This tutorial](https://www.javatpoint.com/spring-boot-tutorial)** showed me the benefits of using Spring Boot. \\

https://start.spring.io/ is the initializer for spring.