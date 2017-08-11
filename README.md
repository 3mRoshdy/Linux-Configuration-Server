# Linux-Configuration-Server
Final Project for Udacity full stack nanodegree : "Linux Server"
this project combines the previous project of Flask WebDevelopment and Linux Server Configuration Project.

In this Project, i have configured a web server for hosting my previous web application of ItemCatalog to be remotly on server provided by Amazon AWS LightSail. This README provide guideline for ouputting the same output of the project.

Starting off by Displaying my WebServer : 
  - Public IP Adress : http://35.176.156.166/
  - SSH PORT : 2200

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
   --> Now the grader can do sudo commands.

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
# Install mod_wsgi :
 ``` $sudo apt-get install libapache2-mod-wsgi ```
  You then need to configure Apache to handle requests using the WSGI module.
  ```$sudo nano /etc/apache2/sites-enabled/000-default.conf ```

 Add the following line at the end of the <VirtualHost * : 80> block, right before the closing </VirtualHost> line:
 ```WSGIScriptAlias / /var/www/html/myapp.wsgi```

 Then you restart apache : 
 ```$sudo apache2ctl restart```
 # install git and get the ItemCatalog on your server:
 install git
 ```$sudo apt-get install git```
 clone the Item Catalog project
 ```mkdir /var/www/catalog
    /var/www/catalog ```
 # Install Flask : 
 1. $sudo apt-get install python-pip
 2. Install Flask:
 $sudo pip install Flask
 
 # install remaining dependencies : 
 ```$sudo pip install httplib2 oauth2client sqlalchemy psycopg2 sqlalchemy_utils requests```
 # Configure a New Virtual Host:
 ```$sudo nano /etc/apache2/sites-available/catalog.conf```
  Paste:

    ```

    <VirtualHost *: 80>
       ServerName 54.210.72.218
       ServerAlias ec2-54-210-72-218.compute-1.amazonaws.com
       ServerAdmin admin@54.210.72.218
       WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/virtualenv/lib/python2.7/site-packages
       WSGIProcessGroup catalog
       WSGIScriptAlias / /var/www/catalog/catalog.wsgi
       <Directory /var/www/catalog/catalog/>
         Order allow,deny
         Allow from all
       </Directory>
       Alias /static /var/www/catalog/catalog/static
       <Directory /var/www/catalog/catalog/static/>
         Order allow,deny
         Allow from all
       </Directory>
       ErrorLog ${APACHE_LOG_DIR}/error.log
       LogLevel warn
       CustomLog ${APACHE_LOG_DIR}/access.log combined
     </VirtualHost>

     ```
   Enable Virtual Host : ``` $sudo a2ensite catalog ```
   Correct the files client_secrets.json in project.py
   ``` $sudo vi __init__.py ```
   Create catalog.wsgi File :
   ``` $sudo vi /var/www/catalog/catalog.wsgi ```
   paste : 
    ```
   import sys
   import logging
   logging.basicConfig(stream=sys.stderr)
   sys.path.insert(0, "/var/www/catalog/")

   from catalog import app as application
   application.secret_key = 'super_secret_key'

    ```
    Install PostgreSQL :
    ``` $sudo apt-get install libpq-dev python-dev
        $sudo apt-get install postgresql postgresql-contrib
        $sudo su - postgres
        
         psql
          CREATE USER catalog WITH PASSWORD 'password';
          ALTER USER catalog CREATEDB;
          CREATE DATABASE catalog WITH OWNER catalog;
          \c catalog
          REVOKE ALL ON SCHEMA public FROM public;
          GRANT ALL ON SCHEMA public TO catalog;
          \q
    ```
  Change engine in project.py, database_setup.py and lotsofmenus.py : 
  ``` engine = create_engine('postgresql://catalog:password@localhost/catalog') ```
  run database_setup.py , lotsofmenus.py .
  # Finally : 
  Check URL: http://35.176.156.166/

 
