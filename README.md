## Creating CI

![image](https://user-images.githubusercontent.com/88186084/132652982-927acf38-0c6e-4506-9b5c-71521e6a0b22.png)

Continous Intergration. This task involves pushing a change to the Github repo, and Jenkins testing this new code change. If it passes, Jenkins should merge the code from our dev branch to the main. Then the third Jenkins job is to copy our code to the ec2 instance so it runs automatically in the background.


## Creating a Jenkins job to test the app machine
1. Firstly, go to your jenkins page and select `new item` at the top of the page.

![image](https://user-images.githubusercontent.com/88186084/132698203-4efb1ab5-a192-4087-b5c8-ad10872f10a4.png)


2. Enter a name for the job and select `freestyle project`. click `ok`

### General
3. Select discard old builds, select the max # of builds to complete to `3`

- Select Github Project and paste the http URL of the github repo into the project url box

### Office 365 connector
5. Select restrict where this project can be done
enter `sparta-ubuntu-node`

---------------------------

### Source code management
6. Select the Git radio-button. Under Repositories, input the SSH url (from the green Code button) in Repository URL.

- Under credentials, add a new key of the kind SSH Username with private key.
Generate a new key (as described in https://github.com/ZeeshanJ99/SRE_Github_ssh_setup)

- Navigate to the private key and copy it into the Private Key: Enter directly section 

- Give the key a name and description 

- Go to your GitHub repo's settings page and navigate to Deploy Keys

- Select Add Deploy Key. Give the key a name.

- Navigate to the public key (.pub) and copy the key into the Key section on GitHub.

- In this case, we need this SSH key to allow write access, so we must check the Allow write access check-box.

- Under Branches to build specifiy the relevant branch (Dev branch in this case).

--------------------------

### Build Triggers
8. Select `GitHub hook trigger for GITScm polling check-box`

-----------------------------------------

### Build Environment
9.  Select Provide Node & npm bin/ folder to PATH. In the NodeJS Installation box, select Sparta-Node-JS

-------------------------------------------

### Build 
10. select Execute shell.

- Input the shell commands that you want to execute in this build, in this instance

`cd app
npm install
npm test`

- Save and apply











## Creating a Jenkins job to merge code from a dev branch to main



## Creating a Jenkins to to copy code to the ec2 instance 



