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
            
            
     ![image](https://user-images.githubusercontent.com/42166489/107736058-23ff0600-6d27-11eb-808c-9b390b3a39eb.png)

     ![image](https://user-images.githubusercontent.com/42166489/107736072-2b261400-6d27-11eb-8e32-c69c071e5214.png)

    3. In the browser, go to http://localhost:8080/public/index.html.
    4. Stop the Docker image.
            
         // upload image here
         
         
3. Deploy the Application to Your Local Kubernetes Installation

    The Employee application contains the deployment configuration (app.yaml) that is in the root directory of the application.
    
    Before you get started, make sure Kubectl and Minikube are running in your local machine.
    To get these please follow these commands :
    
    Install kubectl binary with curl on Linux(refernce url:https://kubernetes.io/docs/tasks/tools/install-kubectl/) - 
        curl -LO https://dl.k8s.io/release/v1.20.0/bin/linux/amd64/kubectl
        
        sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
        kubectl version --client
        
    Install Minikube
    
        curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
        sudo install minikube-linux-amd64 /usr/local/bin/minikube
        
        
    Now, you can find app.yaml file in $home/helidon/examples/employee-app
   
   - In a terminal window, verify the connectivity to the cluster. 
   
            kubectl cluster-info
            kubectl get nodes
            
       // insert image here
       
    - Deploy the application to Kubernetes.
    
            kubetcl create -f app.yaml
            
     - Start the Kubernetes proxy server.
     
            kubectl proxy
            
     - Get the service information.
            
            kubectl get service employee-app
            
     - Test the application as before but use the NodePort instead of 8080.
            For example, http://localhost:31435/public/index.html
            
            

    
    
            
    
        
        
    
    
    
    
    
    
  
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
      
          
  

