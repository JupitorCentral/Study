### My concern is that my public GitHub Spring project will have sensitive values like database usernames and passwords in the properties. What should I do about that?

When creating a public Spring project, you must never commit sensitive data like database credentials directly into your version control system (e.g., Git). The best practice is to externalize these secrets from your codebase. Here are the most common and effective methods:

#### 1. Use Environment Variables

This is the most straightforward and widely-adopted approach. Instead of hardcoding credentials, you use placeholders in your `application.properties` file.

**`application.properties`:**

```Properties
# Good practice: Use placeholders for sensitive data
spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USER}
spring.datasource.password=${DB_PASSWORD}

# Bad practice: Never commit this to a public repository
# spring.datasource.username=my_user
# spring.datasource.password=my_secret_password
```

At runtime, Spring Boot automatically finds and injects the values from the environment variables set on the server where the application is running.

#### 2. Encrypt Properties with Jasypt

For an additional layer of security, you can encrypt sensitive values directly within your properties file using the Jasypt library.

1. **Add the dependency:**
    
    ```xml
    <dependency>
        <groupId>com.github.ulisesbocchio</groupId>
        <artifactId>jasypt-spring-boot-starter</artifactId>
        <version>3.0.5</version> </dependency>
    ```
    
2. **Configure `application.properties`:** Store your encrypted password within `ENC(...)`. The master encryption key should itself be provided as an environment variable, not hardcoded.

```Properties
	spring.datasource.password=ENC(your-encrypted-password-here)
    jasypt.encryptor.password=${JASYPT_ENCRYPTOR_PASSWORD}
```


This approach keeps the configuration within your properties file but ensures the values are unreadable without the master key.

#### 3. Use External Configuration Files

You can store your secrets in a separate properties file located outside of your project's directory. Spring can then import this file at startup.

**`application.properties`:**

```Properties
spring.config.import=file:/path/to/your/external-secrets.properties
```

This method cleanly separates your configuration from your application code.

#### Key Git and Deployment Practices

- **`.gitignore`:** Always add your local configuration files (like `application-dev.properties` or any file with real secrets) to `.gitignore` to prevent accidental commits.
- **Template File:** Commit a template file like `application.properties.example` to show other developers what properties are needed, but with placeholder values.
- **Production Environments:** For production, leverage secret management services provided by your cloud provider (e.g., AWS Secrets Manager, Azure Key Vault, Google Secret Manager) or orchestration platform (e.g., Kubernetes Secrets).

---

### So, when you say "environment variables on the server computer," you mean I should set these values in the terminal that is used to run the built JAR file. Is that correct?

Yes, your understanding is exactly right.

Environment variables are set at the operating system level of the server where your application will run. They are not part of your application's code.

The process is:

1. **Set Variables:** On the server, you define the environment variables (e.g., `DB_USER`, `DB_PASSWORD`) in the terminal session or through a system-wide configuration before starting your application.
2. **Run Application:** You then run your application using `java -jar your-app.jar`.
3. **Resolve Placeholders:** During startup, Spring Boot detects the placeholders (like `${DB_USER}`) in your `application.properties` and replaces them with the corresponding values it finds in the server's environment.

This decouples your secrets from your codebase, allowing your GitHub repository to remain public and secure while the actual credentials live safely on the deployment server.


### My Conclusion

Let's set environment variables on that server computer.



### How can I set environment variables on a Linux server computer?

On a Linux server, you can set environment variables in several ways, depending on your needs. The primary distinctions are whether the variable should be temporary for the current session, permanent for a specific user, or permanent for all users on the system.

#### **1. Temporary Environment Variables (Current Session)**

To set an environment variable that lasts only for the duration of your current terminal session, use the `export` command.

```Bash
export VARIABLE_NAME="value"
```

**Example:**
```Bash
export DB_USER="myuser"
export DB_PASSWORD="mypassword"
```

These variables will be lost once you close the terminal.

#### **2. Permanent User-Specific Environment Variables**

To have variables available for a specific user every time they log in, you should add them to that user's shell configuration file. For most systems using Bash, this is the `~/.bashrc` file.

1. **Edit the file:**

	```
	nano ~/.bashrc
	```
    
2. **Add the `export` commands** to the end of the file:
    ```Bash
    export DB_USER="myuser"
    export DB_PASSWORD="mypassword"
    ```
    
3. **Apply the changes** by running the following command or by logging out and back in:
    
    ```Bash
    source ~/.bashrc
    ```
    

Other files that can be used include `~/.bash_profile` or `~/.profile`.

#### **3. Permanent System-Wide Environment Variables**

To set environment variables that are available to all users on the server, you can edit system-wide configuration files.

- **Option 1: Using `/etc/environment`** This file is specifically for setting system-wide environment variables.
    
    1. **Edit the file** with superuser privileges:
        
        ```Bash
        sudo nano /etc/environment
        ```
        
    2. **Add the variables** without the `export` command, in a `KEY="value"` format:
        
        ```Plaintext
        DB_USER="myuser"
        DB_PASSWORD="mypassword"
        ```
        
    3. You may need to log out and back in for these changes to take effect system-wide. You can use `source /etc/environment` for the current session.
- **Option 2: Using Profile Scripts** You can also add `export` commands to files like `/etc/profile` or `/etc/bash.bashrc`. This is a common method for variables that should apply to all Bash users.
    

#### **4. Environment Variables for Systemd Services**

If you are managing an application with `systemd`, it is best practice to define environment variables directly within the service's unit file.

1. **Edit the service file:**
    
    ```Plaintext
    [Unit]
    Description=My Application
    
    [Service]
    ExecStart=/path/to/your/application
    Environment="DB_USER=myuser"
    Environment="DB_PASSWORD=mypassword"
    
    [Install]
    WantedBy=multi-user.target
    ```
    
2. For better security, especially with sensitive data, you can use an `EnvironmentFile`:
    
    ```Plaintext
    [Service]
    ExecStart=/path/to/your/application
    EnvironmentFile=/etc/myapp/environment
    ```
    
    Then, create the `/etc/myapp/environment` file with the variables:
    
    ```Plaintext
    DB_USER=myuser
    DB_PASSWORD=mypassword
    ```
    

#### **How to Verify**

You can check if an environment variable has been set correctly using one of the following commands:

- **Check a specific variable:**
    
    ```Bash
    echo $VARIABLE_NAME
    ```
    
- **List all environment variables:**
    
    ```Bash
    printenv
    ```
    
- **Grep for a specific variable:**
    
    ```Bash
    printenv | grep VARIABLE_NAME
    ```
    

---

