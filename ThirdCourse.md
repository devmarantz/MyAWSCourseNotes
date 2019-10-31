# AWS Cloud Practitioner Course 3
## Understanding Core AWS Services
Start Date: 10/10/19

---

## Lesson Notes
--- 
## Course 3 - Understanding Core AWS Services
### Lesson 1 - Introduction to Amazon Web Services
- There are different places in the world for you to host your AWS server
- Gartner's Magic Quadrant

Pros of AWS:
  - Storage
    - AWS guarantees 99.99999999% durability
  - Pay As You Go Model
    - Allows you to scale resources well
    - Generally based on hourly costs
    - Prices are depending on the types and size of the services that you are using

### Lesson 2 - AWS Global Infrastructure
Overview:
Regions -> Availability Zones -> Data Centers

- If you have an application with multiple servers then you should spread those servers across multiple AZs
- AWS operates on 16 regions with 44 availability zones


Data Centers:
  - Problem: What if a single data center goes down?
    - Then the data will not be able to be accessed however long the data center is down.
    - AWS combats this issue by storing it's data centers in Availability Zones.

Availability Zone (AZ):
- How is it structured?
  - AWS Data Centers are organized into Availibility Zones
  - One AZ can have multiple Data Centers
  - Data is replciated across multiple data centers!
  - Each AZ is located at a lower-risk location
    - They choose locations that are have minimal risk for disaster

- How does this help?
  - If a data center goes down, then there will be another data center where you can access your data

AWS Regions:
  - Each region has at least two AZs
  - There are 16 regions worldwide as of 2017
  - Though AZs are connected to one another, Regions are isolated from one another

Ex: What if we have an application with multiple servers?
- Problem:
  - If we have all servers are in the same AZ then the whole application will go down if that AZ goes down 
- Solution:
  - Spread out the servers across different AZs!!

### Lesson 3 - Setting up Labs
- AWS allows you to have a free account
- I set up my free tier account


### Lesson 4 - Multi-Factor Authentication
[AWS Management Console](https://aws.amazon.com/console/)

Points:
- Be careful with your login because you can be charged a lot of money
- I set up my MFA and Google Authenticator

MultiFactor Authentication (MFA):
- MFA will help ensure your AWS Account's security
- You can use Google Authenticator to get MFA codes

Pros of MFA:
- Even if your Username and Login is compromised the person loggin in still needs to put in a MFA code

### Lesson 5 - Creating our first IAM user

Points:
- I create a new IAM User

Problem:
- Your Root Account allows you to have UNLIMITED access to your AWS resources
- If somebody gets a hold of your root account then they will have unlimited access!

AWS Best Practices:
- In order to ensure security you should create an AWS Identity and Access Management User (IAM)
- IAM Users allow you to restrict the privelages with your account.
- A Root User should not be accessed until it needs to be accessed
  - Many financial institutions create alerts whenever a Root User is logged in.  Ideally you should be using IAM Users until it is necessary to access the Root User

  How to Create an IAM User:
  1. Go To Services -> IAM -> Users
  2. Create a New User
  3. Select Password Options
  4. Set Permissions
      - b) Add tags (optional)
  5. Review
  6. Finish
      - Given a link to send to a person

  Warning:
  - IAM Users do not have the MFA Option By Default

  Set Up MFA With Your User:
  1. Go To User
  2. Click Security Credentials
  3. Set Up MFA Code

  Permissions:
  - If your IAM is compromised you can immedietely remove the permissions

  ### Lesson 6 -  Setting up MobaXterm

  Warning!:
  - MobaXterm does not work on Mac
  - Need
    - telnet
    - ssh
    - curl

  PuTTY:
  - An alterntive to MobaXTerm

  - I don't have telnet.  I will see if I can get away using it without telnet



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
    - Use command:  ssh ec2-user@[IP]
    - You should get "Permission Denied (publickey)"
    - I get when at home:
    The authenticity of host '[IP] ([IP])' can't be established.
    ECDSA key fingerprint is [FINGERPRINT]

3. Add key to desktop for SSH
    1. Put Key .pem on desktop
    2. Go into Desktop
        - cd ~/Desktop
    3. Change permission 
        - chmod 640 [KEY FILE NAME]
        - I had to change the permissions manually
    4. Run ssh command with key
        - ssh -i ~/Desktop/[KEY FILE NAME] ec2-user@[IP]
    5. Now that you're in go into the root account
        - sudo su -

4. Download nginx
    - This is a web server similar to Apache
    1. Command:
        - yum -y install nginx
    2. Check status:
        - service nginx status
    3. Start server:
        - service nginx start
    4. Check whic port it is running on:
        - netstat -ntlp
        - Check where it says nginx and make sure the state is listening
        - Then check the port
        - In my case it is [NGINX PORT]

5. Try to connect via telnet
    - If you try to connect via telnet to the port it will not be running
    - Check the Security Group "view inbound rules" in the EC2 Management Console
      - You should see that there is only one rule for SSH

6. Add a new inbound rule
    1. Click Security Group name
    2. Click "inbound" thne "edit" 
    3. Add rule
        - Include Port
        - Change name to "Web Server" in the description
    4. Now you should be able to connect to the Web Server

7. Copy IP Address and open it in a web browser
    - You should see "Welcome to nginx..."

8. We can edit the html page
    - In the SSH root window go into the file that holds the html files
      - cd /usr/share/nginx/html
    - The web page is being rendered from the index.html file
    - View file
      - nano index.html
    - Clear file
      - echo > index.html
    - Write new file
      - For now we will write
        - "Welcome to my websites..."
      - Use CRTL+X to exit
        - It will ask you to save
          - Press Y
          - Then press Enter
    - Refresh page to check if the new message is there