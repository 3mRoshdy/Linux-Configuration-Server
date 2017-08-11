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
  1. Through the host terminal :
    - Start off by "sudo apt-get update" to update all packages.
    - Enter ```sudo apt-get upgrade``` to upgrade all updated packages (if any).
  2. Through the local terminal :
    Generate Key Using command "ssh-keygen"
    Enter location for the key to be aquired.
    Navigate through the directory and open the new_key_file.pub
    
      """
      The key was created in a default location /ubuntu/.ssh/<key_file>
      $cat /Users/panpan/.ssh/key_file.pub
      
      """
      
    Copy the content of that file.
    
    3. return to the remote terminal : 
      """sudo vi /home/ubuntu/.ssh/authorized_keys"""
     paste the content of the key.
     
    4. 
       sudo chmod 700 ~/.ssh
       sudo chmod 644 ~/.ssh/authorized_keys```
    # Create new user "grader-udacity":
      1. Create new User: 
     ```
     Sudo adduser grader-udacity
     ```
      2. enter a password for this user.
      3. create directory for user to be authorized
      ```
      sudo mkdir /home/grader/.ssh
      ```
      4.edit authorized_keys file for grader user
      ``` sudo vi /home/grader/.ssh/authorized_keys ```
      5. Exit Ubuntu User 
      ``` exit ```
      6. Log in as grader-udacity
      ``` ssh grader-udacity@35.176.156.166 -i ~/.ssh/key_file ```
      7. enter password related to this user.
      
      # Give the grader user sudo access :
      1. return as ubuntu user
      ``` ssh ubuntu@35.176.156.166 -i ~/.ssh/key_file ```
      1. edit the sudoers file for the grader user the directory for sudoers.d
      ``` sudo vi /etc/sudoers.d/grader-udacity ```
      2. Add:
     ``` grader ALL=(ALL) NOPASSWD:ALL ```
     No the grader can do sudo commands.
     
      3.Log in as grader:
     ```      
     $ssh grader@35.176.156.166

     $sudo chmod 700 ~/.ssh

     $sudo chmod 644 ~/.ssh/authorized_keys
     ```
     4. Change port to 2200 nad assign FireWall for server using UFW : 
     
    ```   
    $sudo ufw status
    $sudo ufw allow ssh
    $sudo ufw allow www
    $sudo ufw allow 2200/tcp
    $sudo ufw allow 22/tcp
    $sudo ufw allow ntp
    $sudo ufw enable
    $sudo ufw status
     
    ```
    5. Change port from default 22 to 2200: 
    ``` sudo vi /etc/ssh/sshd_config ```
    6. restart server SSH:
    ``` sudo service ssh restart ```
    7. go to amazon acount and set the networking and add a custom --> TCP --> Port:2200
    8. Log in now using the new port : 
    ``` ssh grader-udacity@35.176.156.166 -i ~/.ssh/key_file -p 2200 ```
    
    # Install apache : 
     - start off by installing apache:
      ``` $sudo apt-get install apache2 ```
    
     
     
    
  
