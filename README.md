# Creating a webhook




## Generate ssh key
In the terminal, input the command:

`ssh-keygen -t rsa -b 4096 -C "johnsmith@gmail.com"`

(Using the same email address that is associated with your Github account)

- It will then prompt you to add a name, if you do not it will provide the name `id_rsa`
- There is no need to create a passphrase
- `ls` can be used to check whether this process has succeeded, you should see two new items keyname and keyname.pub

-----------------------------------------------

## Deloying .pub to github
- Navigate to Github settings
- `SSH and GPG keys`
- `New ssh key`
- name the key
- within the terminal `cat` into your generated `.pub` key from above
- copy all the text and paste it onto the github key section

----------------------------------------------------------
## Creating CI

![image](https://user-images.githubusercontent.com/88186084/132652982-927acf38-0c6e-4506-9b5c-71521e6a0b22.png)


## Creating a Jenkins job to test app

## Creating a Jenkins job to merge code from a dev branch to main

## Creating a Jenkins to to copy code to the ec2 instance 



