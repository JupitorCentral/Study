- [x] Spring Boot Deployment: JAR vs WAR ⏳ 2025-07-03 📅 2025-07-03 ✅ 2025-07-03

### If we build a spring project and create an artifact (.jar), it's a program of a spring app. and we can deposit multiple artifacts on a single tomcat server. Am I right?

You're mostly right, but there's an important distinction about **JAR vs WAR files** for deployment on Tomcat.

#### Spring Boot JAR Files

When you build a Spring Boot project, you get a **JAR file** that contains:

- Your application code
    
- Embedded Tomcat server
    
- All dependencies
    

This JAR is a **standalone executable program** that you run with `java -jar myapp.jar`. It doesn't get deployed to an external Tomcat server because it already has its own embedded Tomcat.

#### Deploying to External Tomcat

To deploy multiple Spring applications on a **single external Tomcat server**, you need **WAR files**, not JAR files.1 Here's why:

**WAR files** have the proper directory structure that Tomcat understands for web applications.2 To create a WAR file from Spring Boot, you need to make specific configuration changes in your build process.

### So war files - an built web app and jar file - including one tomcat server + one spring webapp?

Yes, you've got it exactly right!

#### WAR Files

WAR files are built web applications that contain:

- Your Spring web application code
    
- Web resources (HTML, CSS, JavaScript)
    
- Configuration files (web.xml)
    
- Dependencies (JAR libraries)
    
- **NO embedded server** - needs an external Tomcat
    

WAR files are deployed **TO** an external Tomcat server.3 Multiple WAR files can be deployed on a single Tomcat instance, each becoming a separate web application with different context paths.4

#### JAR Files (Spring Boot)

JAR files include both the Tomcat server AND the Spring web application:

- Your Spring application code
    
- All dependencies
    
- **Embedded Tomcat server**
    
- Everything needed to run standalone
    

Spring Boot creates "fat JARs" that are **self-contained executable programs**.5 You run them with `java -jar myapp.jar` and they start their own embedded Tomcat server.6

#### Deployment Comparison

**Multiple WAR files on one Tomcat:**

Plaintext

```
External Tomcat Server
├── app1.war → Spring Web App 1 (/shop)
├── app2.war → Spring Web App 2 (/admin)
└── app3.war → Spring Web App 3 (/api)
```

**Multiple JAR files (each with embedded Tomcat):**

Plaintext

```
JAR 1: java -jar shop.jar → Embedded Tomcat + Spring App (port 8080)
JAR 2: java -jar admin.jar → Embedded Tomcat + Spring App (port 8081)
JAR 3: java -jar api.jar → Embedded Tomcat + Spring App (port 8082)
```

Your understanding is perfect: **WAR = just the web app, JAR = Tomcat server + web app bundled together.**

---

## Summary

#### WAR (Web Application Archive)

- Contains only the web application (code, resources, dependencies).
    
- Does **NOT** include an embedded server.
    
- Requires an external application server (like Tomcat) for deployment.
    
- Allows multiple WAR files (multiple applications) to be deployed on a single server.
    

#### JAR (Java Archive) - Spring Boot Executable

- A self-contained, executable file.
    
- Bundles the web application **AND** an embedded server (like Tomcat).
    
- Runs as a standalone application using the `java -jar` command.7
    
- Each JAR runs its own server instance, typically on a different port.