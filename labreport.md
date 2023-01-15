# CSE15L Lab Report 1 #
### How to log into a course-specific account on ieng6 through VS Code ###  
---
*Kane Li, 15th January 2023*

This lab will cover three main steps:
1. Installing Visual Studio (VS) Code and Opening a Terminal
2. Remotely Connecting to ieng6
3. Running Remote Commands

Let's Begin!

## Step 1 - Installing VS Code and Opening a Terminal ##
**Installing VS Code:**  
Visual Studio Code is a type of Integrated Development Environment (IDE), to help programmers keep their work organized and combine various necessary code activities such as compiling, text editing, and debugging. Download and run Visual Studio code [here](https://code.visualstudio.com/).  
Follow the instructions and keep the default options.  
When are you done installing, you should see something like this:  
![VS Code Image](https://user-images.githubusercontent.com/122249106/211943935-d62e5f2c-523b-4da5-8c12-2f9f872e5aaf.png)
**Opening a Terminal:**  
Great! Now you want to open up a terminal in VS Code! Simply click Terminal &rarr; New Terminal at the top left of the window, as shown in red in the image below. Doing so, should create a terminal window at the bottom of VS Code, which is shown in green in the same image below.
<img width="1281" alt="CSE15L Terminal Demonstration" src="https://user-images.githubusercontent.com/122249106/212430737-b286b17b-417f-482b-a010-6093982b6ce0.png">
The Terminal is a program that allows programmers like us to interact with the computer, passing in various input texts and receiving outputs! This is what we are going to use to remotely connect to ieng6.

## Step 2 - Remotely connecting to ieng6 ##
**Finding your account information:**  
Before we begin, we want to find your course-specific account username at [https://sdacs.ucsd.edu/~icc/index.php](https://sdacs.ucsd.edu/~icc/index.php). Log in using your UCSD Triton Link Username, and your student ID, starting with an A. Once you are in, you should see a username under additional accounts starting with "cse15lwi". This will be your identifying usename when trying to remotely connect. 
<img width="1267" alt="image" src="https://user-images.githubusercontent.com/122249106/212432033-9c0a5695-752f-459d-a27e-3c18ba2a0b76.png">
**Remotely connecting:**  
To establish an initial connection to the remote computer, run `ssh username@ieng6.ucsd.edu`but replace `username` with your "cs15wi___" username that you had ppreviously accessed. If you are connecting to this server for the first time, you should get a response along the lnes of:
````
⤇ ssh cs15lwi23zz@ieng6.ucsd.edu
The authenticity of host 'ieng6.ucsd.edu (128.54.70.227)' can't be established.
RSA key fingerprint is SHA256:ksruYwhnYH+sySHnHAtLUHngrPEyZTDl/1x99wUQcec.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
Password:
````
It is normal to ask for permission the first time connecting, but shouldn't occur if you often connect to this server. If this appears even after connecting often, it is possible for others to be listening in, so be careful!

Now, input your password from your UCSD TritonLink account, the one used for your Single Sign-On (SSO) that uses DUO verification. Don't worry if it looks like you aren't inputting anything, the bar won't move or show input for security reasons. 

Common mistakes to look out for:  
1. If copy-pasting your password doesn't work, try to input it manually as sometimes the CTRL+V command will be input into the terminal but not the contents of the paste itself.
2. Make sure you input the username completely and properly! For example, in cse15(l), the character enclosed in parenthesis is a lower-case L, not a 1!
3. If your TritonLink SSO doesn't work, you may want to change your password at [https://sdacs.ucsd.edu/~icc/index.php](https://sdacs.ucsd.edu/~icc/index.php). It will take a while to update, however, so be patient!

Once you are in, you should see something like this:
<img width="743" alt="image" src="https://user-images.githubusercontent.com/122249106/212561251-2c8c44d1-5b0c-417f-aebc-89b3c6e57ed6.png">  
Congratulations! Your computer is now remotely connected to a computer in the CSE Basement!  

## Step 3 - Running Remote Commands ##  
On your terminal in VS Code, you can run both remote (remote computer) and local (your computer) commands, by ussing the `ssh` keyword! Starting your code lines with `ssh` will cause them to be run remotely. Trying running various commands such as `cd`, `ls`, `pwd`, `mkdir`, and `cp` in various ways on both your computer and the remote computer (`ssh`). What differences and similarities do you notice? What happens?  

Other commands to try include:
* `cd ~`
* `cd`
* `ls -lat`
* `ls -a`
* `ls <directory>` where `<directory>` is `/home/linux/ieng6/cs15lwi23/cs15lwi23abc`, where the `abc` is one of the other group members’ username
* `cp /home/linux/ieng6/cs15lwi23/public/hello.txt ~/`
* `cat /home/linux/ieng6/cs15lwi23/public/hello.txt`

Once you are done testing, you can simply do:
* CTRL + D
*  Run `exit`  

And that's it! Congratulations on learning how to establish a remote connection to ieng6 through VS Code! 

---
*By Kane Li, 15 January 2023*
