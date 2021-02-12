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
            
            
       ![image](https://user-images.githubusercontent.com/42166489/107746809-71d23900-6d3c-11eb-94fb-4950960b8d78.png)
       
       ![image](https://user-images.githubusercontent.com/42166489/107746816-7696ed00-6d3c-11eb-9448-df4596ede25e.png)
       
       ![image](https://user-images.githubusercontent.com/42166489/107746820-7ac30a80-6d3c-11eb-8f82-121724de1cb2.png)
            
     - Test the application as before but use the NodePort instead of 8080.
            For example, http://localhost:31459/public/index.html
   
   
4. Configure Your Kubernetes Cluster in Oracle Cloud

      1. Create a Compartment
              
        - Sign in to the Oracle Cloud Infrastructure console.
        - Open the navigation menu. Under Governance and Administration, go to Identity and click Compartments.
        - Click Create Compartment.
        - In the Create Compartment dialog box enter the following information:
            NAME: MSCompartment(as per your choice)
            DESCRIPTION: (Optional) Enter a description.
            PARENT COMPARTMENT: Select a parent compartment.
        - Click Create Compartment.
     2. Create a Kubernetes Cluster  
     
        To create a Kubernetes cluster, you first specify basic details for the new cluster (the cluster name, and the Kubernetes version to install the master nodes).

            - In the Oracle Cloud Infrastructure console, open the navigation menu. Under Solutions, Platform and Edge, go to Developer Services and click Container Clusters (OKE).
            Select MSCompartment.
            - On the cluster list page, click Create Cluster.
            - In the Cluster Creation dialog, select and enter the following information:
                NAME: MSDev
                KUBERNETES VERSION: Select a Kubernetes version.
                QUICK CREATE
                SHAPE: VM.Standard2.1
                QUANTITY PER SUBNET: 1
            - Leave the remaining options unchanged and click Create.
            - Write down the Virtual Cloud Network name and then click Close.

    3. Download the Kubeconfig File

        You need the kubeconfig file to access to the cluster when using Kubectl.

            - After provisioning completes, click Access Kubeconfig.
            - Follow the instructions displayed in the How to Access Kubeconfig dialog box.

      
 5. Configure the Database for the Application
 
   1. Launch a DB System
   
      Use the Oracle Cloud Infrastructure console to launch a new DB System.
        
        
        In the Oracle Cloud Infrastructure console, open the navigation menu. Under Database, click Bare Metal, VM, and Exadata.
        Select MSCompartment.
        
        Click Launch DB System.
        
        In the Launch DB System dialog box, enter the following values:
            DISPLAY NAME: MSDB
            AVAILABILITY DOMAIN: Select an availability domain.
            SHAMPE TYPE: VIRTUAL MACHINE
            SHAPE: VM.Standard2.1
            ORACLE DATABASE SOFTWARE EDITION: Standard Edition
            AVAILABLE STORAGE SIZE: 256
            LICENSE TYPE: Choose the type of license you want to use for the DB system.
            Choose SSH Key files (.pub) from your computer.
            VIRTUAL CLOUD NETWORK: Select the same VCN as your Kubernetes cluster.
            CLIENT SUBNET: Select a public subnet.
            HOSTNAME PREFIX: emp
            DATABASE NAME: employee
            DATABASE VERSION: 12.2.0.1
            DATABASE ADMIN PASSWORD: Your password
            CONFIRM DATABASE ADMIN PASSWORD: Confirm your password

      
        Click Launch DB System. Provisioning the database can take several minutes.
        Click the MSDB database. 
        Click Nodes (1)
        
  2. Create the Required Security Rules

        Configure an Oracle Cloud Infrastructure rule that enable you to remotely access the database.

         - In the Oracle Cloud Infrastructure console, in the MSDB details page, click the Virtual Cloud Network.
         - In the Virtual Cloud Network Details page, click Security Lists.
         - Click the security list of the public subnets. The security list of the public subnets is the one that has lb in the name. 
            For example, oke-lb-seclist-quick-MSDB-20190409213055
         - In the Security List Details page, click Add All Rules, then click Another Ingress Rule.
         - In the Add Ingress Rule dialog box, enter the following values:
                STATELESS: Leave this option deselected
                SOURCE TYPE: CIDR
                SOURCE CIDR: 0.0.0.0/0
                IP PROTOCOL: TCP
                DESTINATION PORT RANGE: 1521
          - Click Add Ingress Rules.
          
  3. Connect to the Database with Oracle SQL Developer
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  


      
      
      
      
      
      
          
  

