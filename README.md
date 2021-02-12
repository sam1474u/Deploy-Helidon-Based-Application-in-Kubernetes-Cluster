Learn About Deploying a RESTful Java Application to Oracle Container Engine for Kubernetes
------------------------------------------------------------------------------------------

Architecture

This architecture diagram shows the completed RESTful Java microservices application. Each microservice consists of the application running in multiple Docker containers in a Kubernetes cluster. A load balancer selects which instance is used to process a request. The number of application instances is controlled by the Kubernetes cluster and can be scaled up or down automatically.


![image](https://user-images.githubusercontent.com/42166489/107732463-67a14200-6d1e-11eb-8b87-0da70526613d.png) 

About the Microservices-Based Back End

The back end of the application is a microservice that's coded in Java and uses libraries from the Helidon project (https://helidon.io).

This solution requires the following services:

    Oracle Cloud Infrastructure Database
    Oracle Cloud Infrastructure Container Engine for Kubernetes
    Oracle Cloud Infrastructure Registry

Pre-requisite :

    Java SE Development Kit 8
    Maven 3.5
    An Oracle's Single Sign-On (SSO) user and password
    Docker CE 3.0 or later
    Kubectl 1.7.4
    Minikube
    
    Please verify if these are available on the CLI. For Oracle SSO , use your Oracle login username and password.
    
1. Build and Test the Application

      - download using command    
                 git clone https://github.com/oracle/helidon.git
      - go to helidon directory   
                cd helidon
      - checkout tag              
                git checkout tags/1.1.2
      - install dependency        
                mvn install -DskipTests
      - go to employee project    
                cd examples/employee-app
      
      In a text editor, open the settings.xml file and change the Oracle's Single Sign-On (SSO) user and password.
      Copy the settings.xml file to the .m2 directory.
      
      - Install the project dependencies  
                mvn install
      - Package the application          
                mvn package
      - Run the application               
                java -jar target/employee-app.jar
      
      find few screenshots here for above steps :
      
          
      ![image](https://user-images.githubusercontent.com/42166489/107732592-c23a9e00-6d1e-11eb-81b0-d986b12a7cef.png)

      ![image](https://user-images.githubusercontent.com/42166489/107732694-fb730e00-6d1e-11eb-8532-5c675df55e82.png)
      
      
      In a browser, go to http://localhost:8080/public/index.html.
      Test the add, update, delete, and search options.
      Stop the application, press ctrl+c.
      
 2. Create and Test Your Docker Container
 
    You can create a Docker container by specifying a Dockerfile. A Dockerfile is a script that contains collections of commands and instructions that are automatically 
    executed in sequence in the docker environment for building a new docker image.

    The Employee application contains a Dockerfile that is copied to the target directory when you build the application.

    Before you get started, you must have installed Docker on your local machine.

    1. In the terminal window, go to the employee-app directory and build the Docker image.
    
             docker build -t employee-app .
             
    2. Run the Docker image.
    
            docker run --rm -p 8080:8080 employee-app:latest
            
            
    
    
    
    
    
    
  
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
          
  

