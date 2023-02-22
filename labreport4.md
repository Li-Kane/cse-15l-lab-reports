# CSE 15L Lab Report 4 #  
*Kane Li, 22 February 2023*
----
For this lab report speedrunning competition, I was able to reach a time of 59 seconds, when competing during the lab, due to a variety of techniques. However, there were a few mistakes in terms of typing. So I ran through the steps again, but this time with no mistakes, lowering the time to around 44 seconds. This time is much better compared to my first run through of around 5 minutes, so here the required steps and the optimizations I made along the way:

### Delete any existing forks of the repository (setup) ###
*Delete previous repository fork*   
<img width="337" alt="delete old repo" src="https://user-images.githubusercontent.com/122249106/220767321-0090d246-afc0-466e-b1d2-f541c0825599.png">  
To setup for the speedrun, it is important to delete the previous repository in order to start off on a "blank slate". Since developing an efficient method requires multiple attempts, then you should clear out the previous attempts repository first to prevent it from potentially affecting your run. This is because eventually in the process we are expected to add, commit, and push to the repository, where if the old repository is not deleted then it when it is cloned the issue will already be solved, which is not the purpose of the speedrun.

*Remove previous lab7 on the remote server*  
<img width="752" alt="rm -rf setup" src="https://user-images.githubusercontent.com/122249106/220766868-9eba666d-3c37-4b82-acc2-10393fade239.png">  
The tasks involved in this speedrun involve cloning on a remote computer, therefore it is also important to remove the cloned directory from the remote computer before beginning the speedrun, as deleting the repository on github doesn't automatically cause the physical files on the computer to be deleted. To accomplish this, I simply type out an run `rm -rf lab7` recursively deleting the lab7 directory and all files inside it.

### Fork the repository (setup) ###
<img width="1280" alt="image" src="https://user-images.githubusercontent.com/122249106/220767765-6d345de0-7934-4851-8cfe-fda74ba407d3.png">
The final part of the setup before the speedrun simply involves forking the repository [here.](https://github.com/ucsd-cse15l-w23/lab7.git) This is going to be the repository that will later be cloned.

### Log into ieng6 ###
<img width="619" alt="image" src="https://user-images.githubusercontent.com/122249106/220767942-5540801d-a328-45b7-baab-421702bf17ec.png"> 
UP <enter>
  
### Clone the fork ###
<img width="427" alt="image" src="https://user-images.githubusercontent.com/122249106/220768229-dbb13267-3f36-4f29-b977-1f1ab9c2a7aa.png">  
^r clone | <enter>
  
### Run the tests, demonstrating failure  ###
<img width="1130" alt="image" src="https://user-images.githubusercontent.com/122249106/220768372-bc7c860b-e114-4c4c-88cb-fc1d0d1c4fe7.png">  
cd lab7 | ^r javac | <enter> 
  
### Edit code to fix the failing test ###
<img width="1130" alt="image" src="https://user-images.githubusercontent.com/122249106/220768866-74fbdc36-7b39-4b01-b062-6d9aad50b444.png">  
nano L<tab>j<tab>
CTRL + SHIFT + - 43
12 right arrow, delete, 2
CTRL O |ENTER
CTRL X
  
### Run fixed texts and show they now  ###
<img width="1129" alt="image" src="https://user-images.githubusercontent.com/122249106/220769062-e5469310-c8cc-47e1-8697-50d3b92df9f2.png">  
2 up arrow <enter>
  
### Commit and push resulting changes ###
<img width="827" alt="image" src="https://user-images.githubusercontent.com/122249106/220769623-04c34be9-4e1f-4ea6-bf05-c855e371a6f6.png">  
  ^r comm | <enter>
  TIME
