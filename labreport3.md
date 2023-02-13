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

## Invert Match (grep -v) ##
Finding invert match `-v` on the `grep --help` manual, I decided to try it out by seeing what a line without "a", one of the most common letters, 
would look like.  

<img width="608" alt="image" src="https://user-images.githubusercontent.com/122249106/218599452-16e859ca-bd30-4d56-a63e-a8bf6f007fe0.png">  
  
To do this, I called the command `grep -vr "a" written_2/non-fiction/OUP/Abernathy`, where the `-vr` after `grep` is a `-v` for a invert match, 
returning every line that doesn't match, and `-r`, a recursive option as I am searching a directory. I then simply passed it the pattern "a" and 
a directory so it would search for all lines with "a", and then return those without a due to `-v`. Looking at the contents, it appears lines
without "a" are either very short or empty. Using `-v` here is helpful if I ever want to exclude a certain word or character, something undesirable
and people have a lot of undesirable things.

My second test of invert match `-v` was finding lines without the keyword "risk" in it, as risk is not cool so we want to censor it.   

<img width="1021" alt="image" src="https://user-images.githubusercontent.com/122249106/218600317-67f03f7f-0cc2-429f-8bf0-600d93ac003a.png">  
  
Calling `grep -v "risk"  written_2/non-fiction/OUP/Abernathy/ch9.txt` is very similar to the previous structure, where this time I simply pass in a 
file and get returned all lines without the word "risk" excluding even words that have the word "risk" in them, such as "frisky". Ultimately, `-v` can
be very helpful for censorship or avoiding certain phrases, as it is far more efficient to return lines that don't match a single word than do a grep 
search for every single word exceptthe one you want to exclude.

## Word Count (grep -c) ##
I found `-c` in the `grep --help` manual, and used it here to determine word frequencies in a specific file:  

<img width="431" alt="image" src="https://user-images.githubusercontent.com/122249106/218597223-911f22a8-4123-46fd-9072-9e67f861d530.png">  
  
Here, calling `grep -ic "India" written_2/travel_guides/berlitz1/HistoryIndia.txt` works to return a count of the amount of lines "India" is 
matched with inside the file HistoryIndia.txt. The `-i` I optionally added in just makes it so upper and lower case doesn't matter. Meanwhile, `grep 
-ic "warrior" written_2/travel_guides/berlitz1/HistoryIndia.txt` of a different word on the same file returns 4, suggesting that the word "warrior" 
was not very common in the file. This is useful as `-c` produces significantly less output, and you wouldn't have to count out the amount of matches.

My next example took a similar approach as the previous one, just with finding character frequencies in the same file:  

<img width="400" alt="image" src="https://user-images.githubusercontent.com/122249106/218597916-2b98c1fc-50b2-44a2-a4ab-805d7af43abe.png">  
  
In a similar fashion as before, I am comparing the amount of times the amount of lines which the letter "a" appears, as well as the amount of lines 
which the letter "v" appears. Since "a" is a common vowel, the number returned is significantly larger than that for "v". For additional clarification,
the commands return the amount of LINES that match, not the amount of CHARACTERS that match. If it was truly returning the amount of times "a" or "v" 
occured, the return values would be much greater. While not a completely direct measure of the amount or probability, it is a simple, convenient way to 
return grep-results as a number output, great for quantitative analysis.

## Word Matching (grep -x) ##
After seeing an explanation [here](https://www.gnu.org/software/grep/manual/grep.html#File-and-Directory-Selection) about word maching with grep,
I wasn't too sure how it worked so I decided to try it out and compare `grep -w "action" written_2/travel_guides/berlitz2/PuertoRico-WhereToGo.txt`to 
`grep "action" written_2/travel_guides/berlitz2/PuertoRico-WhereToGo.txt`.

<img width="1029" alt="image" src="https://user-images.githubusercontent.com/122249106/218595201-4a66eaf4-2d76-4559-a210-1784cce12ff7.png">  

`grep -w` works such that only full words that contain the match are going to count as part of lines in the output. So while the screenshot does 
show the word "attractions" which has action, it is merely a part of the line that contains the full word "action". The line wasn't matched because 
of "attractions". There are only two lines, where the "action" words that cause the line to be matched are underlined in red. If I were to run simply `grep`
without the `-w` keyword, there would be far more matches.

For my next example, I decided to try comparing the output difference between having `-w` or not through the count `-c` output control explained previously. 
I used the same website as last example [here.](https://www.gnu.org/software/grep/manual/grep.html#File-and-Directory-Selection)  

<img width="436" alt="image" src="https://user-images.githubusercontent.com/122249106/218596253-bfbfd5c5-0703-4a43-b5af-b636269be4d5.png">  

Here, it can be seen that with `-w` included, `grep -wc "port" written_2/travel_guides/berlitz2/Portugal-WhereToGo.txt` has a significantly lower value or
number of line matches than `grep -c "port" written_2/travel_guides/berlitz2/Portugal-WhereToGo.txt`that lacks `-w`. This is because there are multiple words
with "port" in it, such as trans**port**, **Port**ugal, **Port**ico, **Port**al, im**port**ance, and more. `-w` will filter these out, therefore it has less
matches. This is useful for finding direct matches without unnecessary clutter, since transport and port have different meanings.

## Extended Regular Expressions (grep -E) ##
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
