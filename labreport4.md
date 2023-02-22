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

The final part of the setup before the speedrun simply involves forking the original repository at https://github.com/ucsd-cse15l-w23/lab7.git. This is going to be the repository that will later be cloned. 

### Log into ieng6 ###
<img width="619" alt="image" src="https://user-images.githubusercontent.com/122249106/220767942-5540801d-a328-45b7-baab-421702bf17ec.png">

    Keys pressed: <up><enter>
Due to previous runthroughs, I was simply able to go back on the terminal's most recent command on my local computer, which was `ssh cs15lwi23akk@ieng6.ucsd.edu`. Thanks to the steps in the lab of generating a public ssh key which connects the local computer to ieng6, this step is made even faster as I no longer need to input a password. 
  
### Clone the fork ###
<img width="427" alt="image" src="https://user-images.githubusercontent.com/122249106/220768229-dbb13267-3f36-4f29-b977-1f1ab9c2a7aa.png">

    Keys pressed: <^r><clone><enter>
`<^r>` Is a reverse-i-search that searches through the recent command-line history for previously run commands which contain the keyword passed. Therefore, the sequence `<^r><clone>` will check through the command line history for any commands with the keyword "clone" where the most recent match is my previously run command `git clone git@github.com:Li-Kane/lab7.git`. Then, all I have to do is hit `<enter>` to run the command, creating a clone.
  
### Run the tests, demonstrating failure  ###
<img width="1130" alt="image" src="https://user-images.githubusercontent.com/122249106/220768372-bc7c860b-e114-4c4c-88cb-fc1d0d1c4fe7.png">

    Keys pressed: <cd lab7>, <^r><javac><enter>
Before I can run the commands to compile and run the tests, I first need to change my directory into lab7 so that the class path commands will actually work. Since the command is short, I simply type out the entire `<cd lab7>`. Then, in a similar process to the git clone step, I do a reverse-i-search for the keyword clone, which will then match to `javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java  && java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore TestListExamples`. This command is actually a set of two commands, combining the compile line and the line which runs the tests into a single one-line command through `&&`. Since this shortcut means I only have to reverse-i-search once, I simply hit the keys `<^r><javac><enter>`.
  
### Edit code to fix the failing test ###
<img width="1130" alt="image" src="https://user-images.githubusercontent.com/122249106/220768866-74fbdc36-7b39-4b01-b062-6d9aad50b444.png">

    Keys pressed: <nano L><tab><j><tab>,
    <CTRL+SHIFT+-><43>, <right><right>right>right>right>right>right>right>right>right>right>right><delete><2>,
    <^O><enter>, <^X>
This portion can essentially be split into 3 sections:
1. `<nano L><tab><j><tab>`: For this section, I wanted use the command-line editor nano, typing out nano as usual. Then, I used the tab keyboard shortcut right after I type L. This is because if you were to ls and look at the current files, you would see ListExamples.class, StringChecker.class,     TestListExamples.java, ListExamples.java, TestListExamples.class, and lib. Because `<tab>` allows autocompletion to the nearest match and there are two files that start with L, ListExamples.java and ListExamples.class, the line autocompletes to `nano ListExamples.`, due to the ambiguity btween if it should be .java or .class. I then just need to do `<j><tab>` to specify .java, and soon we are inside nano.
2. `<CTRL+SHIFT+-><43>, <right><right>right>right>right>right>right>right>right>right>right>right><delete><2>`: Now that I am inside nano, I use `<CTRL + SHIFT + -><43>` to jump straight to line 43 within FileExamples.java. Since the line which needs to be changed `index1 += 1;` needs to become `index2 += 1;` and there are no short specific singular word matches, I instead opt to simply jump straight to the line with `<CTRL + SHIFT + ->`. Then, the sequence `<right><right>right>right>right>right>right>right>right>right>right>right><delete><2>` is simply traveling to where the bug is, and replacing the 1 with a 2.
3. `<^O><enter>, <^X>`: `<^O><enter>` writes to the file and confirms that I want to make the change. `<^X>` simply exits out of nano and back into the command line.

### Run fixed texts and show they now  ###
<img width="1129" alt="image" src="https://user-images.githubusercontent.com/122249106/220769062-e5469310-c8cc-47e1-8697-50d3b92df9f2.png">
  
    Keys pressed: <up><up><enter>
Since rerunning the fixed tests is simply just compiling and running the same files again, I can jump and run the same command I recently used to compile and run the tests in the first place by pressing `<up><up><enter>`. This runs `javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java  && java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore TestListExamples`, which is the single line optimization.
  
### Commit and push resulting changes ###
<img width="827" alt="image" src="https://user-images.githubusercontent.com/122249106/220769623-04c34be9-4e1f-4ea6-bf05-c855e371a6f6.png">
  
  Keys pressed: <^r><comm><enter>
For the final portion, I reverse-i-searched for this command: `git add ListExamples.java && git commit -m "speedrun" && git push git@github.com:Li-Kane/lab7.gitls`. Similar to the previous compiling and running commands, this command combines 3 commands into one: `git add`, `git commit`, and `git push`. Doing so allows me to save significant time as I only need to reverse-i-search once for "comm" (to match commit). Hitting `<enter>` then allows me to complete all 3 tasks and close out the speedrun.
  
### Results ###
Ultimately, I was around #3 in my lab section, where my time of 59 seconds was just behind 2nd place of 55, but far behind 1st place of 16 seconds. Looking at my process, I think for further optimizations I could seek to combine more of my commands into single lines, where I could combine all the steps leading up to nano in a single command and all the steps after into one single other command, so that I would only have to do 2 commands and can access them with just `<up>` once or twice. Additionally, for my nano search there is most likely a more efficient way to jump straight to the bug, perhaps my searching for the last occurrence of "index1", as my process of 12 `<right>` may be a bit too slow. Overall, I am pretty satisfied with my results, and I just wonder if 1st place simply took my theorized approach, or if they took an entirely different approach overall.
