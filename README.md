# Creating CI
Continous Intergration. This task involves pushing a change to the Github repo, and Jenkins testing this new code change. If it passes, Jenkins should merge the code from our dev branch to the main. Then the third Jenkins job is to copy our code to the ec2 instance so it runs automatically in the background.


![image](https://user-images.githubusercontent.com/88186084/132652982-927acf38-0c6e-4506-9b5c-71521e6a0b22.png)

---------------------------------------------------------------

## Creating a Webhook
Follow this link to see how to create a webhook again
https://github.com/ZeeshanJ99/SRE_Github_ssh_setup

--------------------------------------------------------------------

### 1. Creating a Jenkins job to test the app machine
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

------------------------------------------------------

### 1. Create the dev branch on the terminal
- Go into the terminal and use `git branch dev` then `git checkout dev` to checkout into your dev branch.

![image](https://user-images.githubusercontent.com/88186084/132732370-d9f1366e-6365-4a6c-a281-77478979a3be.png)

--------------------------------------------------------------

### 2. Follow steps 1-4 to begin with
- Follow steps 1-4 mentioned previously to configure Jenkins.

-----------------------------------------------------------------------

### 3. Source Code Management
- On the Source code management tab select `additional behaviours`
- Then select `merge before build`

Merge before build  |         |
--------------------|---------|
Name of repository  | origin  |
Branch to merge to  | main    |
Merge strategy      | default |
Fast-forward mode   | --ff    |

-----------------------------------------------------

### 4. Follow steps 5-6

-------------------------------------------------------------

### 5. Skip step 7 'Build'
Leave blank 

------------------------------------------

### 6. Post Build Actions
- Follow step 8
- The new project to build will be named after the next project when this build is stable
- Add an additional `post-build action` as `git publisher`
- Tick `push only if build succeeds` and `merge results`

![image](https://user-images.githubusercontent.com/88186084/132734106-4dc5b7cd-be38-418a-a423-8cd04b3a8bce.png)


---------------------------------------------------

### 7. Save and apply

--------------------------------------------------

### 8. Testing
- To test whether the two previous steps are triggered via the webhook and work in tandem, you can nano into a README.md file in the terminal and make a change on the dev branch.

- `nano README.md`
- Make a change to the file
- `git add .`
- `git commit -m "change"`
- `git push`

Once git pushed both jobs on the main Jenkins menu should be visibly successful.

![image](https://user-images.githubusercontent.com/88186084/132735368-9f482e4e-04c3-4353-9074-001678be4336.png)


------------------------------------------------------------

## Creating a Jenkins to to copy code to the ec2 instance
The final job we will make will enable us push our code to the ec2 instance and have our connected db and app running in the background.

-------------------------------------------

### 1. Start your db and app instances

----------------------------------------------

### 2. Follow steps 1-4
#### Source code management
- In step 4 change the branch specifier to `*/main` as the previous job will merge to our main branch

![image](https://user-images.githubusercontent.com/88186084/132736828-1e14c92f-8560-4931-8e76-7f1037569670.png)

-------------------------------------------------------------------

### 3. Build Environment
- Follow the previous build environment by ticking `Provide Node & npm bin/ folder to PATH`
- Select the `SSH Agent` radio button
- Select `Add key` then `Jenkins key`
- On kind select ssh username with private key
- Leave `ID` blank
- Leave `username` blank
- Select `enter directly` on Private key
- Select `Add`

![image](https://user-images.githubusercontent.com/88186084/132738540-1f8432d5-34b2-4f1d-8b6e-223d22262873.png)


In the terminal cd into your ~/.ssh directory and `cat` into the private key (without the .pub) and then copy the code into Jenkins where prompted

![image](https://user-images.githubusercontent.com/88186084/132738708-24a05c52-57ca-4969-af08-3b65120afa9f.png)

-------------------------------------------------------

### 4. AWS
- You will need to add an inbound rule to the security group as well as the public NACL connected to your app which will allow jenkins to ssh in on port 22.

#### App Security group
![image](https://user-images.githubusercontent.com/88186084/132739976-4d096c34-ec31-41a0-be4e-38753bd53f30.png)

#### Public Nacl
![image](https://user-images.githubusercontent.com/88186084/132740124-03ec2713-3491-4cc8-bb40-79ec61ba31fb.png)

---------------------------------------------

### 5. Build
- Execute shell
- Get the IP addresses of your new db and app instances that are running in order to paste these into the Jenkins execute shell box

- Use the following code:
      
      # ssh with app ip skipping the fingerprint prompt
      ssh -A -o "StrictHostKeyChecking=no" ubuntu@54.75.66.12 << EOF
      # create an env to connect to db
      export DB_HOST=mongodb://34.244.188.233:27017/posts
      # navigate to app folder
      cd app/app
      npm install
      # seeds the db
      node seeds/seed.js
      # launch the app
      nohup node app.js > /dev/null 2>&1 &
      EOF

---------------------------------------------------------------

### 6. Apply and save

![image](https://user-images.githubusercontent.com/88186084/132741857-3ba958d8-35a9-4e8c-8622-0801d0a19690.png)

-----------------------------------------------------------------

### 7. Testing
You should now be able to make a change in the README.md file which will successfully carry out and trigger the first job, then the second and then the third as visible on Jenkins 

<img src = "https://media.giphy.com/media/HPA8CiJuvcVW0/giphy.gif?cid=ecf05e47eutm671cfw2o3f3zp46wdkjgxatkjm7qyflqdovb&rid=giphy.gif&ct=g">

-----------------------------------------------------------------------------

### 8. Deployment
As Jenkins is running the app in the background you should now be able to use the public app ip from your AWS instance in the browser of your choice and it should show this


![image](https://user-images.githubusercontent.com/88186084/132742000-51609e0b-f9a2-4b64-9323-a644ded520b0.png)


- Adding `/posts` onto the end of the IP should show this

![image](https://user-images.githubusercontent.com/88186084/132742172-b82c8faa-0b24-4e7d-b21c-f38d9baccbe0.png)

----------------------------------------------------------------------------

### 9. Completion
Now any change to the code of the app will produce a change on the web app automatically through Jenkins!
