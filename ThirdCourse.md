# AWS Cloud Practitioner Course 3
## Understanding Core AWS Services
Start Date: 10/10/19

---

# Lesson Notes
## Course 3 - Understanding Core AWS Services
---
---
### Lesson 1 - Introduction to Amazon Web Services
  - There are different places in the world for you to host your AWS server
  - Gartner's Magic Quadrant

#### Pros of AWS:
  - Storage
    - AWS guarantees 99.99999999% durability
  - Pay As You Go Model
    - Allows you to scale resources well
    - Generally based on hourly costs
    - Prices are depending on the types and size of the services that you are using

---
---
### Lesson 2 - AWS Global Infrastructure
#### Overview:
    Regions -> Availability Zones -> Data Centers

  - If you have an application with multiple servers then you should spread those servers across multiple AZs
  - AWS operates on 16 regions with 44 availability zones


#### Data Centers:
  - Problem: What if a single data center goes down?
    - Then the data will not be able to be accessed however long the data center is down.
    - AWS combats this issue by storing it's data centers in Availability Zones.

#### Availability Zone (AZ):
  - How is it structured?
    - AWS Data Centers are organized into Availibility Zones
    - One AZ can have multiple Data Centers
    - Data is replciated across multiple data centers!
    - Each AZ is located at a lower-risk location
      - They choose locations that are have minimal risk for disaster

  - How does this help?
    - If a data center goes down, then there will be another data center where you can access your data

#### AWS Regions:
  - Each region has at least two AZs
  - There are 16 regions worldwide as of 2017
  - Though AZs are connected to one another, Regions are isolated from one another

#### Ex: What if we have an application with multiple servers?
  - Problem:
    - If we have all servers are in the same AZ then the whole application will go down if that AZ goes down 
  - Solution:
    - Spread out the servers across different AZs!!

---
---
### Lesson 3 - Setting up Labs
  - AWS allows you to have a free account
  - I set up my free tier account


---
---
### Lesson 4 - Multi-Factor Authentication
  [AWS Management Console](https://aws.amazon.com/console/)

#### Points:
  - Be careful with your login because you can be charged a lot of money
  - I set up my MFA and Google Authenticator

#### MultiFactor Authentication (MFA):
  - MFA will help ensure your AWS Account's security
  - You can use Google Authenticator to get MFA codes

#### Pros of MFA:
  - Even if your Username and Login is compromised the person loggin in still needs to put in a MFA code

---
---
### Lesson 5 - Creating our first IAM user

#### Points:
  - I create a new IAM User

#### Problem:
  - Your Root Account allows you to have UNLIMITED access to your AWS resources
  - If somebody gets a hold of your root account then they will have unlimited access!

#### AWS Best Practices:
  - In order to ensure security you should create an AWS Identity and Access Management User (IAM)
  - IAM Users allow you to restrict the privelages with your account.
  - A Root User should not be accessed until it needs to be accessed
    - Many financial institutions create alerts whenever a Root User is logged in.  Ideally you should be using IAM Users until it is necessary to access the Root User

#### How to Create an IAM User:
  1. Go To Services -> IAM -> Users
  2. Create a New User
  3. Select Password Options
  4. Set Permissions
      - b) Add tags (optional)
  5. Review
  6. Finish
      - Given a link to send to a person

#### Warning:
- IAM Users do not have the MFA Option By Default

#### Set Up MFA With Your User:
  1. Go To User
  2. Click Security Credentials
  3. Set Up MFA Code

#### Permissions:
  - If your IAM is compromised you can immedietely remove the permissions

---
---
### Lesson 6 -  Setting up MobaXterm

#### Warning!:
  - MobaXterm does not work on Mac
  - Need
    - telnet
    - ssh
    - curl

#### PuTTY:
  - An alterntive to MobaXTerm

  - I don't have telnet.  I will see if I can get away using it without telnet



---
---
### Lesson 7  -  Launcing First EC2 Instance
  1. First login to your AWS Managment Console
  2. Select Your Region
  3. Go to the EC2 Dashboard
  4. Go to Instances
      - Create a Keypair
      - Launch Instance
        - Select an OS
          - We choose Amazon Linux
        - Select an Instance
          - How much CPU/Ram do you need?
            - WARNING! If you don't choose a free tier, you will be charged.
        - Configure Instance Details (Default)
        - Add Storage (Default)
        - Add Tags (Default)
        - Configure Security Group
          - Make a Security Group Name
          - This case will be SSH TCP on Port 22
          -   Source 0.0.0.0/0 Means that anybody can access it through Port 22
        - Review and Launch
          - Click Launch and Select your relevant key pair
          - Your instance will now launch though it will take some time for it to instantiate the first time

#### Creating an Instance:
  - Before creating an instance we need to create a key pair
  - Amazon gives you options of different OS's, and instances that you can use.
  - Once your instance is running, you can open your MobaXTerm

  Note:  If you are running Mac or Linux we don't need MobaXTerm because they already have an SSH client installed 

  Connect with Telnet:
  Use the command "telnet [IP Address] [Port]"

  Note:
  Can't connect when you're in the Genuent Office because they block SSH.

#### Connect to Instance
  Now that we've created our instance, let's connect to it.

  IP Address Section of Instance:
  - Private IP
    - Won't be able to connect directly
    - Need a Virtual Private Network
  - Public
    - In our case it's IPv4
    - We will use this to connect 

  1. Open up iTerm/ MobaXTerm
      - telnet [IP] [PORT]
      - In order to quit hit CTRL+]

  2. Connect Via SSH
      - Use command:  
      
            ssh ec2-user@[IP]
      - You should get "Permission Denied (publickey)"
      - I get when at home:
      The authenticity of host '[IP] ([IP])' can't be established.
      ECDSA key fingerprint is [FINGERPRINT]

  3. Add key to desktop for SSH
      1. Put Key .pem on desktop
      2. Go into Desktop

              cd ~/Desktop
      3. Change permission 
          
              chmod 640 [KEY FILE NAME]
          - I had to change the permissions manually
      4. Run ssh command with key
          
              ssh -i ~/Desktop/[KEY FILE NAME] ec2-user@[IP]
      5. Now that you're in go into the root account
          
              sudo su -

  4. Download nginx
      - This is a web server similar to Apache
      1. Command:
          
              yum -y install nginx
      2. Check status:
          
              service nginx status
      3. Start server:
          
              service nginx start
      4. Check whic port it is running on:
          
              -netstat -ntlp
          - Check where it says nginx and make sure the state is listening
          - Then check the port
          - In my case it is [NGINX PORT]

  5. Try to connect via telnet
      - If you try to connect via telnet to the port it will not be running
      - Check the Security Group "view inbound rules" in the EC2 Management Console
      ![inbound rules](./images/3.7-practical.png)
        - You should see that there is only one rule for SSH

  6. Add a new inbound rule
      ![inbound rules2](./images/3.7-practical2.png)
      ![inbound rules3](./images/3.7-practical3.png)
      1. Click Security Group name
      2. Click "inbound" then "edit" 
      3. Add rule
          - Include Port
          - Change name to "Web Server" in the description

      4. Now you should be able to connect to the Web Server

  7. Copy IP Address and open it in a web browser
      - You should see "Welcome to nginx..."

  8. We can edit the html page
      - In the SSH root window go into the file that holds the html files
        
            cd /usr/share/nginx/html
      - The web page is being rendered from the index.html file
      - View file
        
            nano index.html
      - Clear file
            
            echo > index.html
      - Write new file
        - For now we will write
          - "Welcome to my websites..."
        - Use CRTL+X to exit
          - It will ask you to save
            - Press Y
            - Then press Enter
      - Refresh page to check if the new message is there

---
---
### Lesson 8  -  Document - Installation Commands
  Amazon has come up with the new Amazon Linux 2 OS.

  If you are using Amazon Linux 2, the commands to install nginx will slightly change. Here are the new updated commands:

      amazon-linux-extras install nginx1.12
      systemctl start nginx

---
---
### Lesson 9  -  Understanding Basics of Firewall
  There are good guys and bad guys on the internet.
  Because of this we need to protect our servers.
  This is why we have firewalls

  ![Firewall Overview](./images/3.9-FirewallOverview.png)

#### Firewalls
  - Firewalls allows us to 
    - Allow Trusted users
    - Block hackers

#### Example
  In this example, we have 5 services running:
  - Apache, SSH, FTP, SMTP, and MySQL

  Each of these Services also have a Port affiliated with them.

  In the firewall rule(bottom of image) it states the firewall will only allow a connection from:
  - Port 22
  - With the Source 10.0.5.57
  - WIth a request for SSH

  ![Firewall Rule](./images/3.9-FirewallRule.png)

  In this example you can see the Authorized User (IP:10.0.5.57) and the Hacker (IP:112.20.50.60)

  ![Firewall Example](./images/3.9-FirewallExample.png)
  Since the Source is specified in the firewall rules, the hacker will be blocked

#### 3 Way Handshake
  ![Three Way Handshake](./images/3.9-ThreeWayHandshake.png)
  - A 3 way handshake needs to happen before you can start accessing data
  - The TCPI/IP Header Fields are looked at first in the "Packet" that is sent
  - What does the firewall evaluate the TCP/IP Header Fields?
    - The most important things that it looks at are the:
      - Source IP and PORT
      - Destination IP and PORT
    - Based on these coniditions the Firewall will determine if it will allow the transaction
      - In the backend it goes into more detail (This is just a high-level overview)

#### Example:
  In this example since the ports are different (as you can see in the TCP/IP Packet)

  This request will be blocked!!
  ![Firewall Block](./images/3.9-FirewallBlock.png)
  - Notice that the Source IP matches: 10.0.5.57
  - But the Destination Port does not match: 80

  As soon as a machine sends a packet:
    1. It Goes to the Firewall (SYN Packet)
        - Checks source IP
        - Check the Destination Port
    2. The data transfer takes place if passed

#### Firewall Final
  - If there are no rules on a firewall it will block every request
  - Firewalls block thousands of attacks everyday. 

---
---
### Lesson 10  -  Network ACL
"Multiple Layers of Defense"
#### Network ACL Overview:
  - Stateless in Nature
  - Operate at the Subnet level
    - Subnet vs Instance
      ![Subnet Vs. Instance](./images/3.10-subnetVsInstance.png)
      - Subnet can contain hundreds of instances
    - Instead of the Instance level where Security Groups are
  - All subnets in VPC must be associated with NACL
    - AWS will create a new NACL when you make a VPC by default
    - By default, network ACL contains full allow inbound and outbound

#### Example:
  ![Use Case 1](./images/3.10-useCase1.png)
  - NACL allows you to block an IP at the NACL level.
  - You can't block only one IP on the Security Group

#### Use NACL
  1. Go to VPC Console
  2. Select a VPC
    - In my case, I only have one
  3. Go to Network ACLs
    - Example shows Default Yes and Subnets:
    ![NACL Example](./images/3.10-NACLExample.png)
  4. Check Inbound and Outbound Rules
    - You should have two rules each
    ![Rules](./images/3.10-rules.png)
    - The lower the # of the rules, the higher the priority
      - Rule #99 is higher priority than #100
      ![Rule99](./images/3.10-rule99.png)
      - Rule #101 is lower priority than #100
      ![Rule101](./images/3.10-rule101.png)
    - If you create a custom NACL it comes default with all traffic denied
    ![Denied](./images/3.10-customNACL.png)

---
---
### Lesson 11 -  Introduction to Block vs Object Storage Mechanism
#### Block Storage
  - Data is stored in "blocks"
  - Data stored is read or written a whole block at a time
  - Most file systems are block devices
  - Every block has an address
    - An app can be called via SCSI call using address
  - No storage side meta-data associated with the block other than the address
  - A block has no description and no owner

##### View Block Size
  - Go into root of EC2 instance:

        sudo su -
  -  View block size:

          blockdev --getbsz /dev/sda
  - List devices

        lsblk
    -  In my case I get an "xvda"

##### Example
  - Based on the block size of 4096:
    - Each block is 4096 based blocks
  - If you have data of 8KB (8096) 
    - You will have two blocks
  - If you have data of 12KB 
    - You will have three blocks

---
#### Object Storage
  - Data storage architecture that manages data as objects as opposed to blocks of storage
  - Defined as data (a file) along with all it's meta-data as an object
  - Object is given an ID which is calculated from the contents (object and meta-data)
    - An app can call an object based off the unique Object ID

##### Meta Data
  - By default AWS adds the Content-Type meta data to an object
    - Ex: image/png

---
#### Block vs Object Storage
##### Differences
  ![Block vs Object](./images/3.11-blockVsObject.png)
    - Object Storage can be called via API
      - On a HTTP(S) based interface

---
---
### Lesson 12 - Instance Store Volumes
"temporary data"
#### Instance Store (IS)
  - AWS Instance store provides a temporary block storage volumes for use wuth EC2 instances
    - Usually when you reboot an instance, the data in a system is lost
  - Storage is located on the disks that are physically attached to the host computer
  - Size on instance store varies depending on the instance
  - Instance store is temporary storage
    - Our first instance was an ebs device type which is a permanent storage, a proper hard disk 
#### Example
  ![Instance Store](./images/3.12-instanceStore.png)
  - Using 4 VMs belonging to 4 customers
  - We are using the storage device on a host server
  - Problem, if the storage device files, then all 4 VMs will fail.  NOT OPTIMAL

#### Launch an Instance Store
  1. Go to EC2 Instances
  2. Launch a new instance
  3. Choose Community AMIs
  4. Select "Root Device Type" = Instance Store 
  5. We'll choose the first one:
    ![Instance1](./images/3.12-instance1.png)
  6. Select m1.small type
    ![Instance2](./images/3.12-instance2.png)
    - Notice that you can't select any of the EBS only instane storages
    - Our m1.small instance has 1.7GB RAM and 160GBs storage size
    - This instance is free, this is why people use instance stores
  7. Configure instance details
    - Use default
  8. Add storage
    - Click "add new volume"
    - Choose "instance store 0"
    - Notice that you can't modify the size because this is determined by the instance type you choose
    ![Instance3](./images/3.12-instance3.png)
    ![Instance4](./images/3.12-instance4.png)
  9. Review and launch!
    ![Instance5](./images/3.12-instance5.png)
    ![Instance6](./images/3.12-instance6.png)
    - Notice the warning, but you should be fine
    - Select the default key pair
    - Acknowledge the warning
  10. Go to instance and rename "instance store"
    ![Instance7](./images/3.12-instance7.png)
    ![Instance8](./images/3.12-instance8.png)
    - Notice that the "Root device type is an instance store
    ![Instance9](./images/3.12-instance9.png)

#### Important Points
  - Data in IS is lost when:
    - The underlying disk drive fails
    - The instance stops
    - The instance terminates
  - IS are included in the cost of EC2 instance
    - They are cost effective!
    - Since we already have our EC2 instance we get a free Instance store 
      - With our m1.small we get a free 160GBs
  - If you are using an IS, make sure that you backup your data to central storage
    - Store it in something like an S3

#### Connect to Instance store
  - Telnet to Port 22
  - ssh to the Instance
  ![Instance Connect](./images/3.12-instanceConnect.png)
  - Go to root

        sudo su -
  - df -h
    ![Instance Connect 2](./images/3.12-instanceConnect2.png)
    - Notice the ephemera
      - This means temporary
  - Create a file
    ![Instance Connect 3](./images/3.12-instanceConnect3.png) 
  - Reboot IS
    - Notice that you can only reboot or terminate the instance store
    ![Instance Connect 4](./images/3.12-instanceConnect4.png) 

#### Important Point
##### Before stopping instanes make sure that they are not Instance Stores because you will lose all the data!
  - If you manually halt an IS it will say that it's terminated
    ![Instance Terminated](./images/3.12-instanceTerminated.png) 




    
---
---
### Lesson 13  -  Introduction to Elastic Block Store
#### Elastic Block Store (EBS)
  - "Portal Hard Disk"
  -  AWS EBS is a persistent block storage volumes for use with EC2
      - Unlike Instance Stores, the data stored here is saved even when EC2 instances are stopped or restarted
  - Each EBS is designed for 99.999% availability
  - Automatically replicated within its availability zone
  - EBS is elastic in nature  
    - Supports dynamic increase in capacity and performance
    - Can change instance types of live volumes

#### Diagram
  ![EBS](./images/3.13-EBS.png)
  - There are 4 EC2 Instances (orange)
    - Without EBS, the EC2 Instances would have to store data directly on the server

  - EBS is similar to a network attached storage (right)
    - There are 3 hard disk drives (black)
    - Mounted via network
    - Devices are replicated to each other
      - Allows data to be backed up

#### Compute !== Storage
##### Compute
  - EC2 is regarded a a compute based service
  - Compute generally refers to Memory & CPU
##### Storage
  - Includes EBS, Instance Stores, etc.

---
#### Practical
  1. Go to your instance

  2. Notice the Root Device Type = ebs

  3. Click Root Device: /dev/xvda

  4. Click Volume ID: (EBS ID)
  ![EBS Example](./images/3.13-EBSExample.png)

  5. It should take you to the Volume where you can view the
      - Size, Volume Type, # of IOPS

  6. Click Actions and modify volume
      - Note: remember that EBS are elastic
        - This is where you change that stuff
    ![EBS Example 2](./images/3.13-EBSExample2.png)

  7. Here we can change the size and volume type
      - Size
    ![EBS Example 3](./images/3.13-EBSExample3.png)

      - Volume Type
    ![EBS Example 4](./images/3.13-EBSExample4.png)

---
#### Warnings
  - Even though EBS data is backed up, you should still back up the data yourself

---
---
### Lesson 14  -  Features of EBS - Portability
#### EBS Review
  - "Elastic Block Store"
  - Based on Network attached storage
  - Two types of storage
    - Instance Store
      - Storage is directly on host where VM's reside
    - EBS
      - In a cluster that are mounted to EC2 Instances
  - Easily detached and provide portability
    - Because it is attached via network
    - Similar to a hard drive connected via usb
  ![EBS Review](./images/3.14-EBSReview.png)

#### EBS Portability
  - "Portal Hard Disk"

---
#### Practical 
  1. Go to EC2 Instances 
  2. Go to Volumes
  3. Create a new Volume
  ![Volume](./images/3.14-volume.png)
  4. Set size to 5 GBs
  5. Check Availability Zone for the instance that you're going to connect with
  ![volumeInstance](./images/3.14-instance.png)
  6. Set the appropriate availability zone
  ![volumeCreate](./images/3.14-volumeCreate.png)
  7. Click Create Volume
  8. Notice that state says "available"
      - This means that it is ready to be connected to an EC2 Instance
  ![volumeAvailable](./images/3.14-volumeAvailable.png)
  9. Name the volume "portable"
  ![volumeName](./images/3.14-volumeName.png)
  10. Connect to EC2 Instance

          ssh -i [token route] ec2-user@[IP]
  ![instanceConnect](./images/3.14-instanceConnect.png)
  11. Go to Root

          sudo su - 
  ![instanceRoot](./images/3.14-instanceRoot.png)
  12. Notice it is attached to XVDA
      
  - Also notice that xvda is listed as the root and block device
  ![instanceXvda](./images/3.14-instanceXvda.png)
  ![instanceDevice](./images/3.14-instanceDevice.png)
  13. Attach a volume
  ![volumeAttach](./images/3.14-volumeAttach.png)
  14. Select Appropriate Instance
  ![volumeAttach2](./images/3.14-volumeAttach2.png)
  15. Go back to instance and confirm that a new block device "sdf" is attached
  ![instanceSDF](./images/3.14-instanceSDF.png)

      - Notice that now there is an xvdf
      ![instanceXVDF](./images/3.14-instanceXVDF.png)
  16. Format xvdf disk with a ext4 system

          mkfs.ext4 /dev/xvdf
      - Creates a filesystem inside the xvdf hard disk drive
      ![instanceXVDF2](./images/3.14-instanceXVDF2.png)
  17. Connect to a specific directory
      - Create new directory

            mkdir /marantz
      - Connect harddisk drive to the folder

            mount /dev/xvdf /marantz
      - ensure /dev/xvdf is connected to your folder
      ![instanceXVDF3](./images/3.14-instanceXVDF3.png)
  18. View the folder and list contents
      - Go in folder and list

            cd /marantz
            ls
      - by default it will say lost+found
      ![instanceXVDF4](./images/3.14-instanceXVDF4.png)
  19. Create a text file in the folder

          touch marantz.txt
          echo "This is EBS Portability example" > marantz.txt
          cat marantz.txt
      ![instanceXVDF5](./images/3.14-instanceXVDF5.png)
  20. Create a second instance
      ![secondInstance](./images/3.14-secondInstance.png)
  21. Disconnect from your first instance and connect to your second instance
      - Step out of folder, unmount it, and confirm it's not in the file system
        - Notive that xvdf is not longer listed in the directory

            cd ..
            umount /marantz
            df -h
      ![instanceMove](./images/3.14-instanceMove.png)
  22. Detach volume from the instance and wait for it to be available
      ![instanceMove2](./images/3.14-instanceMove2.png)
      ![instanceMove3](./images/3.14-instanceMove3.png)
  23. Now that it's available we can attach to the new EC2 Instance
      ![instanceMove4](./images/3.14-instanceMove4.png)
      - Confirm that the sdf block device is there
        - DISCLAIMER: The instance has to have the same availability zone
      ![instanceMove5](./images/3.14-instanceMove5.png)
      - Notice that is no longer in the old instance
      ![instanceMove6](./images/3.14-instanceMove6.png)
  24. Check to see if file is in new instance

          ssh -i ~[key route] ec2-user@[IP]
          sudo su -
      ![instanceMove7](./images/3.14-instanceMove7.png)

          lsblk
          mkdir /marantz02
          mount /dev/xvdf /marantz02
          df -h
      ![instanceMove8](./images/3.14-instanceMove8.png)

          cd /marantz02
          ls
          cat kplabs.txt
      ![instanceMove9](./images/3.14-instanceMove9.png)

---
---
### Lesson 15  -  Introduction to ELB
"Managed Services are the best
#### Elastic Load Balancer (ELB)
  - Businesses like to have backups just in case a server goes down
  - ELB takes a part in this architecture
  - Single point of failure should be avoided
    - In this image the cirle would be the load balancer
    - If the single load balancer goes down, the whole website will go down
      ![Single Point](./images/3.15-singlePoint.png)
  - Two Load balancers are better and was more common in business architecture before ELB
    - Even if one load balancer goes, the other one can be used
      ![Two Points](./images/3.15-twoPoints.png)
  - Instead of running load balancers on the EC2 Instances, we utilize ELB which AWS provides
    - Scale automatically depending on the load
    - Managed by AWS
      - AWS also provides Application Load Balancers and Network Load Balancers
      ![ELB](./images/3.15-ELB.png)

#### Intro to ELB Overview
  - ALlows us to distribute incoming traffic to multiple EC2 Instances
  - ELB can handle rapid change in network traffic patterns
  - Since it's a managed service, clients don't have to worry about high availability related aspects
    
---
#### Practical
 1. Have two instances both installed with nginx
      - Nginx is not installed on the second instance so I need to do that
      ![practical](./images/3.15-practical.png)
      - To check if both of your instances have nginx installed and running, copy the IP and go to it on a browser.
        - You should see for both IPs:
      ![practical2](./images/3.15-practical2.png)
  2. Go to the Load Balancer section
      ![practical3](./images/3.15-practical3.png)
      - If you go to the load balancer, you will see that both of the instances are connected to the load balancer:
      - Also notice that both of the instances status are "inService"
      ![practical4](./images/3.15-practical4.png)
  3. Copy the DNS name in the ELB Description:
      ![practical5](./images/3.15-practical5.png)
      - Copy it and put it in the browser.
        - The first time you will see one server:
      ![practical6](./images/3.15-practical6.png)
        - When you refresh it will show the other server:
      ![practical7](./images/3.15-practical7.png)
        - This will flip flop back and forth. Sending requests to both of the servers
  4. Let's test if one server goes down:
      - Stop one of the instances:
      ![practical8](./images/3.15-practical8.png)
      - Once it is stopped:
      ![practical9](./images/3.15-practical9.png)
      - If you refresh, the link will only stay on one server

#### Overview
  - For testing purposes, we showed server one and server two to be different
  - Typically the servers will be showing the same thing
  - If we go back to ELB menu we can see that one of the instances status is "OutOfService":
      ![practical10](./images/3.15-practical10.png)



