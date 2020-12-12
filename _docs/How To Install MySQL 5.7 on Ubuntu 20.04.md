#                   How To Install MySQL 5.7 on Ubuntu 20.04


### Step 1: Add MySQL APT repository in Ubuntu

      $sudo apt update
      $sudo apt install wget -y
      $wget https://dev.mysql.com/get/mysql-apt-config_0.8.12-1_all.deb
      
      Once downloaded, install the repository by running the command below:
      
      $sudo dpkg -i mysql-apt-config_0.8.12-1_all.deb
      
      In the prompt, choose Ubuntu Bionic and click Ok
      
      
### Step 2: Update MySQL Repository on Ubuntu

            $sudo apt-get update
            
            Now search for MySQL 5,7 using apt-cache as shown below:

            $ sudo apt-cache policy mysql-server
            mysql-server: 
             Installed: (none) 
             Candidate: 8.0.21-0ubuntu0.20.04.4 
             Version table: 
                8.0.21-0ubuntu0.20.04.4 500 
                   500 http://ke.archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages 
                   500 http://ke.archive.ubuntu.com/ubuntu focal-security/main amd64 Packages 
                8.0.19-0ubuntu5 500 
                   500 http://ke.archive.ubuntu.com/ubuntu focal/main amd64 Packages 
                5.7.31-1ubuntu18.04 500 
                   500 http://repo.mysql.com/apt/ubuntu bionic/mysql-5.7 amd64 Packages
                   
  ###  Step 3: Install MySQL 5.7 on Ubuntu 20.04 Linux machine
  
        sudo apt install -f mysql-client=5.7.32-1ubuntu18.04 mysql-community-server=5.7.32-1ubuntu18.04 mysql-server=5.7.31-1ubuntu18.04
        
        
        
        
###  Step 3: Secure MySQL 5.7 Installation on Ubuntu 20.04


        Run the command below to secure MySQL

        $ sudo mysql_secure_installation        
