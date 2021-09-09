## Creating CI
Continous Intergration. This task involves pushing a change to the Github repo, and Jenkins testing this new code change. If it passes, Jenkins should merge the code from our dev branch to the main. Then the third Jenkins job is to copy our code to the ec2 instance so it runs automatically in the background.


![image](https://user-images.githubusercontent.com/88186084/132652982-927acf38-0c6e-4506-9b5c-71521e6a0b22.png)




## 1. Creating a Jenkins job to test the app machine
- Firstly, go to your jenkins page and select `new item` at the top of the page.

![image](https://user-images.githubusercontent.com/88186084/132698203-4efb1ab5-a192-4087-b5c8-ad10872f10a4.png)

- Enter a name for the job and select `freestyle project`. click `ok`

---------------------------------------------------------------------------

### 2. General
- Select discard old builds, select the max # of builds to complete to `3`

- Select Github Project and paste the http URL of the github repo into the project url box

![image](https://user-images.githubusercontent.com/88186084/132728714-c66046fe-6e2a-4d0f-b29c-159470825dfe.png)

---------------------------------------------------------------

### 3. Office 365 connector
- Select restrict where this project can be done
- enter `sparta-ubuntu-node`

![image](https://user-images.githubusercontent.com/88186084/132728955-1b6282d4-20c5-46ce-9530-395841d9e9c6.png)


---------------------------

### 4. Source code management
- Select the Git radio-button. Under Repositories, input the SSH url (from the green Code button) in Repository URL.

- Under credentials, add a new key of the kind SSH Username with private key.
Generate a new key (as described in https://github.com/ZeeshanJ99/SRE_Github_ssh_setup)

- Navigate to the private key and copy it into the Private Key: Enter directly section 

- Give the key a name and description 

- Go to your GitHub repo's settings page and navigate to Deploy Keys

- Select Add Deploy Key. Give the key a name.

- Navigate to the public key (.pub) and copy the key into the Key section on GitHub.

- In this case, we need this SSH key to allow write access, so we must check the Allow write access check-box.

- Under Branches to build specifiy the relevant branch (Dev branch in this case).
![image](https://user-images.githubusercontent.com/88186084/132729054-0b6527ae-0356-4648-bcc0-c5154ad74f18.png)

---------------------------------------

### 5. Build Triggers
Select `GitHub hook trigger for GITScm polling check-box`

### 6. Build Environment
- Select Provide Node & npm bin/ folder to PATH. In the NodeJS Installation box, select Sparta-Node-JS

![image](https://user-images.githubusercontent.com/88186084/132729142-3924e119-aaf4-479e-a63b-5e26281bafe4.png)


-------------------------------------------

### 7. Build 
select Execute shell.

- Input the shell commands that you want to execute in this build, in this instance we are using

`cd app`
`npm install`
`npm test`

----------------------------------------------------

### 8. Post Build Actions
Add post-build action

- Build other projects
- In projects to build you will add the name of the next project after it is created in order to trigger it
- Select `Trigger only if build is stable`

![image](https://user-images.githubusercontent.com/88186084/132729289-a08172ae-0539-4a2e-a3fc-7cb7735f256a.png)


### 9. Save and apply

--------------------------------------------------------------------------------------------------

## Creating a Jenkins job to merge code from a development branch to main
The next job we want to do is to create a dev branch and get Jenkins to automate this branch to our main branch.

### 1. Create the dev branch on the terminal
- Go into the terminal and use `git branch dev` then `git checkout dev` to checkout into your dev branch.


### 2. Follow steps 1-4 to begin with
- Follow steps 1-4 mentioned previously to configure Jenkins.

### 3. Source Code Management
- On the Source code management tab select `additional behaviours`
- Then select `merge before build`
- 




































## Creating a Jenkins to to copy code to the ec2 instance 


