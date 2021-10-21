# Exercise 2 - Build a simple Spring Boot microservice

In this section, we'll build a simple Spring boot microservice and deploy it to Azure Spring Cloud. This will give us a starting point for adding Spring Cloud technologies in later sections.

## Task 1 : Create a simple Spring Boot microservice

>💡 __Note:__ All subsequent commands in this workshop should be run from the same directory, except where otherwise indicated via `cd` commands.

1. In an __empty__ directory in git bash execute the curl command line below:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web -d baseDir=simple-microservice -d bootVersion=2.3.8 -d javaVersion=1.8 | tar -xzvf -
```

> We force the Spring Boot version to be 2.3.8.

## Task 2 : Add a new Spring MVC Controller

1. Navigate to the path `C:\Users\demouser\simple-microservice\src\main\java\com\example\demo`

   ![Path](media/folder-path.png)

2. Open with notepad, then remove and paste the content with the below code :

```java
package com.example.demo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello from Azure Spring Cloud\n";
    }
}
```

3. Save the file next to `DemoApplication.java` in the `C:\Users\demouser\simple-microservice\src\main\java\com\example\demo` as `HelloController.java` by changing the **save as type** to all files and then **save** as shown below.

   ![HelloController](media/hello-controller-java.png)


## Task 3 : Test the project locally

1. Now we run the project:

```bash
cd simple-microservice
./mvnw spring-boot:run &
cd ..
```

2. Requesting the `/hello` endpoint should return the "Hello from Azure Spring Cloud" message.

```bash
curl http://127.0.0.1:8080/hello
```

3. Finally, kill running app:

```bash
kill %1
```

## Task 4 : Create and deploy the application on Azure Spring Cloud

This section shows how to create an app instance and then deploy your code to it.

1. In order to create the app instance graphically, you can use navigate back to Azure portal and Look for your Azure Spring Cloud instance in your resource group.

   ![Cloud Spring in rg](media/spring-cloud.png)

2. Click on the "Apps" link under "Settings" on the navigation sidebar.

   ![Apps under cloud spring ](media/spring-cloud-apps.png)

3. Click on "Create App" link at the top of the Apps page.

   ![App creation ](media/spring-cloud-app-creation.png)

4. Create a new application named "simple-microservice"

   ![Create application](media/01-create-application.png)

5. Click on "Create".

>💡 __Note:__ Alternatively, you can use the command line to create the app instance, which is easier:

```bash
az spring-cloud app create -n simple-microservice -s <azure spring cloud name> -g <resourceGroup> --assign-endpoint true --cpu 1 --memory 1Gi --instance-count 1
```
6. Navigate back to Git Bash and now you can build your "simple-microservice" project and deploy it to Azure Spring Cloud by running the below command:

```bash
cd simple-microservice
./mvnw clean package
az spring-cloud app deploy -n simple-microservice --jar-path target/demo-0.0.1-SNAPSHOT.jar
cd ..
```

7. This creates a jar file on your local disk and uploads it to the app instance you created in the preceding step.  The `az` command will output a result in JSON.  You don't need to pay attention to this output right now, but in the future, you will find it useful for diagnostic and testing purposes.

## Task 5 : Test the project in the cloud

1. Navigate back to Azure Portal, From the resource group **spring-cloud-workshop-<inject key="DeploymentID" enableCopy="false"/>** select the Azure Spring Cloud instance named **azure-spring-cloud-lab-<inject key="DeploymentID" enableCopy="false"/>**.

   ![Cloud Spring in rg](media/spring-cloud.png)

2. Click "Apps" in the "Settings" section of the navigation pane and select "simple-microservice"

3. Click on 'See more' to see "Test Endpoint"

   ![See More](media/02-seemore.png)

4. Mouse over the URL labeled as "Test Endpoint" and click the clipboard icon that appears.  

   ![Endpoint](media/microservice-endpoint.png)
    
<!--- 6. This will give you something like:

   `https://primary:BBQM6nsYnmmdQREXQINityNx63kWUbjsP7SIvqKhOcWDfP6HJTqg27klMLaSfpTB@rwo1106f.test.azuremicroservices.io/simple-microservice/default/`
   >💡 Note the text between `https://` and `@`.  These are the basic authentication credentials, without which you will not be authorized to access the service.

7. If you get **"503 Service Temporarily Unavailable"** or **"WhiteLabel Error"** Page as shown below,

   ![Error](media/endpoint-error.png)

   ![Error2](media/error02.png)
   
   - Click on assign endpoint and wait until the endpoint has been assigned and unassign the endpoint soon after. 

   ![assign endpoint](media/simple-microservice-endpoint-assign.png)

-->

5. You can now use CURL again to test the `/hello` endpoint, this time served by Azure Spring Cloud.  For example.

```bash
curl https://primary:...simple-microservice/default/hello/
```

6. Append `hello/` at the end of the URL.  Failure to do this will result in a "404 not found".

   <!--- ![Endpoint](media/hello-from-spring-cloud.png) -->

7. If successful, you should see the message: `Hello from Azure Spring Cloud`.

    ![Endpoint](media/curl-hello-from-spring-cloud.png)

## Conclusion

Congratulations, you have deployed your first Spring Boot microservice to Azure Spring Cloud!
