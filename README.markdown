#macgitwatch

A bash script to watch a file or folder and commit changes to a git repo
Inspired by and adopted based on https://github.com/nevik/gitwatch

##Installation
Simply download macgitwatch.sh and use it or use brew. <br />
```
brew tap nils-tekampe/tap
brew install macgitwatch
```

##What to use it for?
That's really up to you, but here are some examples:
* **config files**: some programs auto-write their config files, without waiting for you to click an 'Apply' button; or even if there is such a button, most programs offer you no way of going  back to an earlier version of your settings. If you commit your config file(s) to a git repo, you can track changes and go back to older versions. This script makes it convenient, to have all changes recorded automatically.
* **document files**: if you use an editor that does not have built-in git support (or maybe if you don't like the git support it has), you can use gitwatch to automatically commit your files when you save them, or combine it with the editor's auto-save feature to fully automatically and regularly track your changes
* *more stuff!* If you have any other uses, or can think of ones, please let us know, and we can add them to this list!

##Requirements
To run this script, you must have installed and globally available:
* `git` ( [git/git](https://github.com/git/git) | http://www.git-scm.com )
* `fswatch` <br />
If you install the macgitwatch via brew, brew will take care of the dependencies. 

##What it does
When you start the script, it prepares some variables and checks if the file [a] or directory [b] given as input really exists.<br />
It will then:
* watch for changes to the file/directory using `fswatch` (`fswatch` will block until something happens)
* wait 2 seconds
* `cd` into the directory [b] / the directory containing the file [a] \(because `git` likes to operate locally)
* `git add <file>`[a] / `git add .`[b]
* `git commit -m "Scripted auto-commit on change (<date>)"`[a] / `git commit -a -m"Scripted auto-commit on change (<date>)"`[b]
* if a remote is defined (with `-r`) do a push after the commit (a specific branch can be selected with `-b`)

Notes:
* the waiting period of 2 sec is added to allow for several changes to be written out completely before committing; depending on how fast the script is executed, this might otherwise cause race conditions when watching a folder
* currently, folders are always watched recursively

##Usage
`macgitwatch.sh <file or directory to watch> [-p <remote> [-b <branch>]]`<br />
It is expected that the watched file/directory are already in a git repository (the script will not create a repository). If a folder is being watched, this will be watched fully recursively; this also means that all files and sub-folders added and removed from the directory will always be added and removed in the next commit. The `.git` folder will be excluded from commits so changes to it will not cause unnecessary triggering of the script.

Please also note that if either of the paths involved (script or target) contains spaces or special characters, you need to escape them accordingly; if you don't know how to do that, the internet will help you, or feel free to ask here or contact me directly.

