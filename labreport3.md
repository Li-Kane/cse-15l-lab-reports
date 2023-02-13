# CSE 15L Lab Report 3 #  
*Kane Li, 13th February 2023*
----
This lab report covers my exploration of the grep command in the bash terminal, with guidance from ChatGPT.

**Debugging with ChatGPT**  
Because I couldn't remember where my written_2 directory was, I decided to try finding it through grep to hopefully search the filenames 
(I think you already see the error). And in the grep --help documentation, I found a -r recursive search function that would theoretically
help me. My command ended up along the lines of `grep -lr "written_2" ~`. Unfortunately, this gave me the wrong input, where it would take 
a very long time to output a couple strange files. So I decided to turn to ChatGPT for help, where after trying it for a solution (which it
gave), I also decided to try debugging with it. ChatGPT helped show me how -r can only search within files for the pattern rather than the 
filename itself, a mistake I made from not carefully reading the -r description. Additionally, it showed me how I misunderstood the use of -l,
where while I had assumed it would cause the filenames to be searched, all it changed was the output to be the filenames of matching lines.
  <img width="600" alt="image" src="https://user-images.githubusercontent.com/122249106/218571341-51292e3d-b4eb-413d-b713-05774f0d4447.png">  
Here, ChatGPT worked very well as a debugger, as it seems to be able to accurately recall documentation and even help pinpoint logic errors
in my own programming. It's honestly very impressive, and can be a great way to learn. Of course, it can be prone to mistakes and false
teaching, but for beginners exploring new concepts it seems to work great.

## Extended regular expressions in grep ##
In this example, I ran the command `egrep -or  "\([0-9,.]+ miles\)" written_2/travel_guides/berlitz2/Vallarta-WhereToGo.txt` to pattern 
match for all mile distances in parenthesis in the file Vallarta-WhereToGo.txt. Extended regular expressions greatly expand pattern matching
abilities, especially in that it can accept more than just direct matches, which can be useful for gathering data. I found a lot of help in this
guide [here.](https://www.digitalocean.com/community/tutorials/using-grep-regular-expressions-to-search-for-text-patterns-in-linux)  
  <img width="509" alt="image" src="https://user-images.githubusercontent.com/122249106/218586961-1a29c8ab-faf4-4072-8dc9-429b0f14422f.png">  
1. `egrep` which is the same as grep -E, allows for the usage of extended regular expression special symbols to represent patterns instead of 
always looking for a direct match, such as how `.` in extended regular expressions represents any character rather than just a period. Then, 
`-o` simply limits the output to only the matches, rather than the full line that the match is in. 
2. `\(__________\)` searches for two pairs of parenthesis, where I have to use the escape (\) character since extended regex takes normal 
parenthesis to denote groups.
3. `[0-9,.]+ miles`The square brackets repsent a form of or, where it means that if any character is a digit `0-9`, comma`,`, or period`.`, 
then it will match. The `+` sign denotes that it must match one or more times, and then the space miles means that only a number amount of 
miles in parenthesis will be matched for.
4. `written_2/travel_guides/berlitz2/Vallarta-WhereToGo.txt` is the path to the file I am searching for.  

For my second example, I ran the command `egrep -o "[^.]*monkeys[^.]*\." written_2/travel_guides/berlitz2/Bali-WhereToGo.txt` to find sentences
that contained the keyword monkeys. This is convenient as sometimes searching for lines itself are too big when returned, while still giving
context on where the word "monkeys" came in. For this, I used the same previous 
[guide](https://www.digitalocean.com/community/tutorials/using-grep-regular-expressions-to-search-for-text-patterns-in-linux) and a new one on 
exclusions [here](https://chortle.ccsu.edu/finiteautomata/Section07/sect07_12.html).  
  <img width="1025" alt="image" src="https://user-images.githubusercontent.com/122249106/218591203-3db3e70f-c20d-4f00-a399-d51f260a767d.png">  
1. `egrep -o` is the same as last time in that I am using extended regular expressions and only want the direct match returned
2. `[^.]*monkeys[^.]*\.` is the pattern search for sentences with "monkeys". `[^.]*` is a pattern that matches all characters not a period, with 
`^` meaning exclusion, so no periods and `*` matching as many characters as possible. Then, the keyword monkeys is added, with another repetition 
of `[^.]*` is added, so that we can finally end with `\.`, with the escape necessary as period (.) usually refers to any character in extended regex.
3. ` written_2/travel_guides/berlitz2/Bali-WhereToGo.txt` is the path to the file
