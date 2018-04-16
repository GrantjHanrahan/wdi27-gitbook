## Week 01, Day 01


What we covered today:

- Introduction | Orientation | Housekeeping
- Structure of the Course
- Introduction to the Command Line
- Command Line Murder Mystery


___
[Intro slides](https://textchimp.github.io/wdi-25/week1/slides/intro/index.html#/)
___

### Introduction | Orientation | Housekeeping

**Luke Hammer** - Lead Instructor - [luke.hammer@ga.co](mailto:luke.hammer@ga.co) (Twitter: [@textchimp](https://twitter.com/textchimp))

**Grant Hanrahan** - Instructional Assistant / Teaching Assistant - [grant.hanrahan@ga.co](mailto:grant.hanrahan@ga.co)

**Hayley Petrov** - Education Programs Producer - [hayley.petrov@ga.co](mailto:hayley.petrov@ga.co)

**Olivia Basheer** - Student Experience Lead - [olivia.basheer@ga.co](mailto:olivia.basheer@ga.co)

**Mikaela Squirchuk** - Education Programs Manager - [mikaela@ga.co](mailto:mikaela@ga.co)

**Lucy Barnes** - Outcomes Producer - [lucy.barnes@ga.co](mailto:lucy.barnes@ga.co)

**Charlotte Messervy** - Outcomes Producer - [charlotte.messervy@ga.co](mailto:charlotte.messervy@ga.co)

The GA campus is typically open from 8.45am to 9pm on weekdays and from 9am to 4pm on Saturdays. The campus is closed on Sundays and public holidays.

___

#### Classroom Culture

You guys came up with these rules for the next 12 weeks:

- Don't compare
- Stay weird
- Late drinks / sorry snacks (OR dont be late)
- Daily showers
- Work collaboratively
- Share your wins & fails
- Keep your sense of humour
- Share photos of cooking disasters
- Luke / Mish GIF(T)s
- Embrace photoshop
- Be respectful
- No stinky food
- Group synergy
- Make mistakes

#### Geek / Hacker Culture

Share and enjoy:
- Books
- Movies
- TV series
- Stupid memes

[The Jargon File](http://www.catb.org/jargon/html/index.html)

[The Tao of Programming](http://canonical.org/~kragen/tao-of-programming.html)


#### GitBook, Homework Repository, Code-Along Repository

- **[GitBook](https://michelleerica.gitbooks.io/wdi-25/content/)** - You're reading it right now. At the end of each day, I will update this GitBook to include notes for the day's lessons.
- **[Homework Repo](https://github.com/michelleerica/wdi25-homework)** - Before class every morning, you should submit your homework for the last day using Github. The repository ("repo") has instructions for submitting your homework.
- **[Classwork Repo](https://github.com/textchimp/wdi-25)** - Missed something during a code-along? You can check this repository for the 'official' code.

#### Important Links, Meetups and Newsletters

**Links**

- [Gist](http://gist.github.com) - sharing code, notes and guides
- Paste Bins - sharing code, debugging code, testing code (esp JS, HTML, CSS):
    - [CodePen](http://codepen.io/)
    - [PasteBin](http://pastebin.com/)
    - [JS Bin](http://jsbin.com/)
- REPLs - 'Read, Evaluate, Print, Loop' - interactive environments for evaluating code:
    - [REPL.it](https://repl.it/)
    - [Babel](https://babeljs.io/repl/)

**Newsletters**

- [Ruby Weekly](http://rubyweekly.com/)
- [Javascript Weekly](http://javascriptweekly.com/)
- [Versioning](http://www.sitepoint.com/versioning/)
- [Sidebar](http://sidebar.io/)

**Meetups**

One of the amazing things about the development community is that it is very active, inclusive, social and supportive - people actually take time out of their lives to help other developers and attend meetups to:
(1) learn more about web development, and
(2) socialize with others in the community. There are a ton of meetups in Sydney (most of which can be found on [meetups.com.au](http://www.meetups.com)), but the two most relevant to this course are:

- [RORO](https://www.meetup.com/en-AU/Ruby-On-Rails-Oceania-Sydney/?_cookie-check=caqRUbln8JZPzebJ)
- [SydJS](http://www.sydjs.com/)

**Also, check out these**

- [Web Design Field Manual](http://webfieldmanual.com/)
- [Web Design Stack](http://webdesignstack.com/)

#### Other

What will go wrong? Everything. This won't be easy for anyone.

> "If debugging is the practice of removing bugs from software... then programming must be the practice of adding them."
– E. W. Dijkstra

The most important thing you can learn as a beginner is **how to debug.**

___

### Structure of the Course

- Week 01 - Front End (Javascript)
- Week 02 - Front End (HTML, CSS + the DOM)
- **Week 03 - Project 00**
- Week 04 - Back End - Ruby - SQL, databases
- Week 05 - Back End - Ruby on Rails
- **Week 06 - Project 01**
- Week 07 - Advanced Front End (AJAX, TDD, single page apps, React)
- Week 08 - Advanced Front End (React, Burning Airlines, Vue)
- **Week 09 - Project 02**
- Week 10 - Advanced Back End / Advanced Everything (Node.js, testing, 2D graphics)
- Week 11 - Advanced Back End / Advanced Everything (compsci stuffs: efficiency, algorithms, data structures; 3D graphics)
- **Week 12 - Project 03**

#### A typical day here at GA...

| Time           | What?                      |
| :------------- | :--------------------------|
| 09:00 - 09:45  | Warmup Exercise            |
| 09:45 - 10:00  | Warmup Solution            |
| 10:00 - 10:15  | Homework Demos             |
| 10:15 - 01:00  | Main session / Code-Along  |
| 01:00 - 02:00  | Lunch / Guest Speaker      |
| 02:00 - 05:00  | (cont'd) Code-Along / Labs |
| 05:00 - ??:??  | Homework / Office Visit    |

There are breaks for morning and afternoon tea which last for about 20 minutes and will be taken whenever works best.

In terms of homework, we like to keep you busy until 9pm or 10pm every night.

We have office hours on Saturday (except in Week 6, when we have the Spit-to-Manly).

___

### The Command Line

[You should have iTerm2](http://iterm2.com/)

Web developers live on the command line. It gives us fast, reliable, and automatable control over computers. Web servers usually don't have graphical interfaces, so we need to interact with them through command line and programmatic interfaces. Once you become comfortable using the command line, staying on the keyboard will also help you keep an uninterrupted flow of work going without the disruption of shifting to the mouse.

The command-line interface - aka "the CLI" or the shell - is a tool that performs specific tasks in response to user-typed commands.  It has the potential to save you lots-and-lots of time because it can automate things, loop through items etc.

`date` - Will print the current date and time

`which date` - Will show the relevant file (will probably return **/bin/date**)

`pwd` - Stands for **Print Working Directory**, will show you where you are in your computer

`mkdir` - Stands for **Make Directory**

`rmdir` - Stands for **Remove Directory**

`clear` - Will clear the terminal screen (ctrl + l and cmd + k will do this as well)

`reset` - Will reset your terminal

`cd` - Stands for **change directory** and will change the current working directory in accordance with the options that follow `cd` (eg: `cd ..` will go up one directory; `cd ~` will return to the home directory)

`cat filename` - Will show you the contents of the specified file

`whoami` - Will show the logged in user

`ps` - Will you show you all running processes

`ps aux` - Will show you all of the running processes with more details

`top` - Will show you the **Table of Processes**

`grep` - Stands for **Global Regular Expression Print** - useful for finding files or content

`ls`- Short for List.  This will show you all of the files and folders in the current directory

`ls /bin` - Will show you all terminal commands

`man` - Stands for **Manual**. To use it, follow the `man` command with another command line command (i.e. ` man grep `).

Most commands will have additional **flags**.  A flag is a request for more information.

A good example of this is the following:

```sh
# list out contents of current directory (excluding hidden files)
> ls
Applications Documents Desktop etc.
# list out contents of current directory (including hidden files)
> ls -l
drwx------   6 jackjeffress  staff   204 16 Mar 15:39 Applications
etc.
```

```sh
# cd can do a lot of things!
> cd
# Will take you back to your "home folder"
> cd /
# Will take you back to your "root folder"
> cd FolderName
# Will take you into the specified folder name
> cd FolderName/AnotherFolderName
# Will take you through FolderName and then into AnotherFolderName
```

```sh
# We can make folders in the CLI by using mkdir
> mkdir Projects
# Then we can move into it
> cd Projects
# ls will show most of your files (ones that aren't prefixed by a .)
> ls
# ls -la will show every file (even hidden files)
> ls -la
# This will change to the current directory
> cd .
# This will open the current directory in Finder
> open .
# This will go back to the previous directory
> cd ..
# If you hit tab at this point, it would autocomplete for you
> cd pro
# This will create a markdown file called README
> touch README.md
# To open an application in terminal (can use any application that you have)...
> open -a "Atom"
```

```sh
# This will open the file and show the contents

> cat books
# Will show you the contents of 'books' and 'pipe' it into the 'sort' program. This doesn't change the original 'books' file!
> cat books | sort
# The pipe character (|) pipes the output of the first command to another command
# This will sort the contents of 'books', and put the sorted contents into the 'sorted_books' file (it will create the 'sorted_books' file if no file with that name exists in the current working directory)
> cat books | sort > sorted_books
# This will rename 'sorted_books' to 'books' (and will overwrite the 'books' file if it already exists)
> mv sorted_books books
# This will open and show the contents of 'books', and shows only lines that have the word 'script' in them (case sensitive!)
> cat books | grep script
# To copy a file, the command is cp (this needs parameters - or arguments - it needs a source and a destination).
# cp [source] [destination]
> cp books my_books
# To remove things, use the rm command (this doesn't get moved to your trash! It will delete it permanently and is impossible to undo)
> rm my_books
# To remove the contents of an entire directory, the -r flag can be appended. The -r flag means "recursively" (ie every file/folder within the director).
```

What happens when we run commands?

```sh
# It will go through all of the folders and files that are shown when we run the following command, and use the contents of the files to decide whether it can run that particular program or command

> echo $PATH
```

[Here is a basic bash profile.](https://gist.github.com/ga-wolf/8a47c257c08632809788)

___

#### _Command Line Interface (CLI) - Recommended Readings_
- [The Linux Command Line](http://linuxcommand.org/tlcl.php)
- [Terminal Cheatsheet for Mac](https://github.com/0nn0/terminal-mac-cheatsheet)
- [QuickLeft - Command Line Tutorials](https://quickleft.com/blog/tag/command-line/page/4/ ) (start from the bottom and work your way up)
- [Command Line Crash Course](http://cli.learncodethehardway.org/book/)
- [In The Beginning Was The Command Line](http://cristal.inria.fr/~weis/info/commandline.html)
    + [It even has its own Wikipedia entry!](https://en.wikipedia.org/wiki/In_the_Beginning..._Was_the_Command_Line)

-  [The Unix Programming Environment](http://en.wikipedia.org/wiki/The_Unix_Programming_Environment)

___

#### _Command Line Interface (CLI) - Additional Links_

- [15 Use Cases of Grep](http://www.thegeekstuff.com/2009/03/15-practical-unix-grep-command-examples/)
- [40 Terminal Tips and Tricks](http://computers.tutsplus.com/tutorials/40-terminal-tips-and-tricks-you-never-thought-you-needed--mac-51192)

___

### Homework

- Track down the [Command Line Murderer](https://github.com/veltman/clmystery).
- Read/work through one or more of the **CLI - Recommended Readings**, above..
- Finish off the course pre-work (if you haven't done so already)!
