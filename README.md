# Linux-Configuration-Server
Final Project for Udacity full stack nanodegree : "Linux Server"
this project combines the previous project of Flask WebDevelopment and Linux Server Configuration Project.

In this Project, i have configured a web server for hosting my previous web application of ItemCatalog to be remotly on server provided by Amazon AWS LightSail. This README provide guideline for ouputting the same output of the project.

Starting off by Displaying my WebServer : 
  - Public IP Adress : http://35.176.156.166/
  - SSH PORT : 2200
  - Application URL : http://ec2-35-176-156-166.compute-1.amazonaws.com/

This Project Requires : 
  1- Starting a new Ubuntu Server with public key using LightSail
  2- Installing Linux/having VM on the device.
  3- Cloning the previous project of ItemCatalog in the server.

# 1. Start a new Server
  - Create an account for amazon aws using lightsail packages.
  - create an instance
  - define the instance to be Ubuntu
  - choose the instance plan for the server to be hosted.
  - Wait for the instance to be ready then launch 'Connect using ssh' button
  # Through the host terminal :
    - Start off by "sudo apt-get update" to update all packages.
    - Enter "sudo apt-get upgrade" to upgrade all updated packages (if any).
  # Through the local terminal :
    - Generate Key Using command "ssh-keygen"
    - Enter location for the key to be aquired.
    - Navigate through the directory and open the new_key_file.pub
    ```
    The key was created in a default location /ubuntu/.ssh/<key_file>

    $cat /Users/panpan/.ssh/key_file.pub
    
    ```
    Copy the content of that file
    
  
