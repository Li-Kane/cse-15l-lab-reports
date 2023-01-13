# CSE15L Lab Report 1 #
### How to log into a course-specific account on ieng6 through VS Code ###  
---

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
Before we begin, we want to find your course-specific account username at [https://sdacs.ucsd.edu/~icc/index.php](https://sdacs.ucsd.edu/~icc/index.php). Log in using your UCSD Triton Link Username, and your student ID, starting with an A. Once you are in, you should see a username under additional accounts starting with "cse15wi". This will be your identifying usename when trying to remotely connect. 
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

Quick Tip: If copy-pasting your password doesn't work, try to input it manually as sometimes the CTRL+V command will be input into the terminal but not the contents of the paste itself.

Once you are in, 
