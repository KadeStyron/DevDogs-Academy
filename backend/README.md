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
> Pro tip highlight the code above and hit the lightbulb to generate the getters, setters, and constructors on vscode. Use the code menu if you're on IntelliJ.

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









## Sources: 

This is the **[video where I obtained the example](https://www.youtube.com/watch?v=9SGDpanrc8U)**.

**[This tutorial](https://www.javatpoint.com/spring-boot-tutorial)** showed me the benefits of using Spring Boot.
