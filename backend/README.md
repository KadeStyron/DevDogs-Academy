# Introduction to Spring Boot


## Prerequisites 

This tutorial assumes that the readers has basic knowledge of java and the 
structure of web applications. To learn more about Spring you should view 
[This Spring article] (#spring). 

## What is Spring Boot
Spring Boot is built on top of the Spring framework. It's used to set-up, 
configure, and run web-based applications. Each Spring Boot application 
also contains an Embedded HTTP Server which will allow us to create our
 own API for the frontend to use.  

## Why use Spring Boot
Spring Boot makes production-ready quickly and efficiently. It provides 
all of the features of spring but it's also easier to use. Everything 
is also auto-configured in Spring Boot. It minimizes the writing of 
boilerplate codes and xml configurations.

Additional Spring Boot Advantages. 
1. Connecting to databases
2. Spring Boot provides application metrics
3. There's security modules available 
4. Easy to Learn
5. So much more



## How to start your Spring Boot application
1. Go to start.spring.io
2. Fill in your settings.
    - Create a Maven Project
    - Pick Java
    - Keep the version as the default
    - Convention for Group ID name is the reverse of your domain name.
     It identifies your project. Example: com.devdogs.schedulebuilder
    - Convention for Artifact ID is lowercase letters with hyphens. 
    It should be the name of your project's artifact. Example: 
    optimal-schedule-builder.
    -Package Name convention should be the same thing as your 
    group ID. 
    - Add Dependencies. These are a list of some relevant ones.  
        - Spring Web is essential for our projects.
        - Spring Data JDBC allows us to connect to a SQL database.
        - H2 database proved an In-Memory Database that can be 
        controlled through the browser.
        - MySQL Driver allows java programs to connect to a MySQL database. 
        - There are SO many more. 
    - Select Jar packaging
    - Select newest Java version.
3. Generate and then extract the zip file.
4. Open your project with your IDE!







Sources: 
https://www.javatpoint.com/spring-boot-tutorial