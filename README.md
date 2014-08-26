



# GIT Help


### About git
* Git is a source code management (SCM) system developed by Linus Torvalds in 2005[[1]](https://en.wikipedia.org/wiki/Git_(software%29))  to help improve the rapid development of his beloved Windows NT kernel [[2]](https://en.wikipedia.org/wiki/Linux_kernel) 
 * SVN,CVS and Bitkeeper are other examples of SCM or version control systems
* Git allows you to make, merge, track then review changes with ease
* Git makes collaboration on projects and working with others simple



##How it works

 1.  Initialize [repository](#repository) (or repo for short)
 2.  [Stage](#stage) files
 3.  [Commit](#commit) changes
 4.  [Push](#push) to a [remote](#remote) repository
 5.  Someone else [clones](#clone) your repository
 6.  Repeat steps 2-4 
 7.  [Pull](#pull) their code 
 8.  Repeat again from step 2
 
####With github the most common way of collaborating on a project is: 

   1. Create a project and push it to your github
   2. Someone forks your project
   3. They make some adjustments or add some features and submit a pull request 
   4. If you (the owner of that project) thinks it is useful, you can accept the pull request and merge in their code

While the above style is extremely effective for large open source projects it may be easier to just add a second owner to your repository for small team projects.
    
##Detailed Example
### Installing git
 ` sudo apt-get install git ` this will install the git package with all the needed tools. 
 
###Create a repo
Since we are working on a project together I have to first, create a git repository. 

* In the terminal I ` mkdir projectFolder `  then  ` cd projectFolder ` and to [initialize](#initialize) my repo I finish up with a ` git init `
* I begin to add my files ` touch FastSimulatedAnnealing.c ` and ` vim  NPsolver.c ` after a few hours of work I save everything. 


Now to stage my repo I do a ` git add -A ` the ` -A ` is for add all files. If I follow it up with a ` git commit -m "solved the traveling salesman problem rewrere in polynomial time" ` this will commit all my changes and leave the message I specified after the ` -m `.

 
### Adding a remote
>Feels like I was working on those problems for years! I completely forgot to add a [remote](#remote) so you can [clone](#clone) my repo!

 We will use github since it is popular and free for open source projects (if you use your WMU email they will give you 3 private repos for 2 years). I would encourage you to check out alternatives like [gitlab](https://about.gitlab.com) or even try setting up your [own](http://git-scm.com/book/en/Git-on-the-Server-Setting-Up-the-Server).
 
 * After you make a github account you will have to set up SSH Keys to push code to your github repo. Steps for generating SSH keys can be found [here](https://help.github.com/articles/generating-ssh-keys) if you are not familiar.

Since we already initialized, staged the files, and committed our changes, you can simply follow steps 1,6,7, and 8 from [here](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line) to push your code to github.

 * **Note:** when you do a ` git push origin master ` this is basically saying "take all of the previous commits and push them to the *[master](#master)* [branch](#branch) on the [remote](#remote) server."

I will not get into branches and *proper* git workflow. More information can be found [here](https://www.atlassian.com/git/workflows#!workflow-centralized)

### Cloning other people's repos
>Great! Now I can go view my code on github, how convenient! If I just share the path to my repo you can download all of my code, then we can work together. How perfect!


* Ok so now that you have my repo URL (found on the right side of the github page) you can clone my project with  `git clone https://URL`
* This will make a folder in the directory from which you executed the command, with the name of the repo, so change into that directory

###Look at old code

At this point you may be saying to yourself:
> Oh god Ben you idiot this code takes forever to run. It was faster before you changed everything, if only I could go back to before you ruined everything. 

Well you can. To see what commits I made do a ` git log --graph `.

 >commit 495b04bcd02688661dfa0736d623383bcc2fbd32, what does this even mean? 

Without getting into too much detail, git uses SHA1 checksums on files and compares to see if there are any changes.
 > Well the code was working 2 commits ago, let's look at that.
 
Run a  ` git checkout 495b0 -b old-state  `, this will create a new branch named *old-state*. If you were to run a ` git status ` you can see which branch you were currently on. It should be the newly created one.
 
 Ok now that everything is better and the code is working again as it should, do a ` git commit -am "rewrote this to run under 5 mins" ` 

But now we have two branches, *master* and *old-state*, by switching back over to the master branch with ` git checkout master` you can see that your changes are still not there! 

We must tell git to merge in your changes with `git merge old-state master `.
 
 * You will at one point get merge conflicts check out the [Pulling code](#pulling-code) on how to resolve those. 

After our files are staged and changes are committed, we can upload them to github (as long as the repo owner added you as a co-owner) with a `git push origin master `

### Pulling code
Taken directly from [here](https://stackoverflow.com/questions/161813/fix-merge-conflicts-in-git)


> Here's a probable use-case, from the top:
> You're going to pull some changes, but oops, you're not up to date:
```bash
 git fetch origin 
 git pull origin master 
```
>>```
From ssh://gitosis@example.com:22/projectname
branch            master     -> FETCH_HEAD
Updating a030c3a..ee25213
error: Entry 'filename.c' not uptodate. Cannot merge.
```

> So you get up-to-date and try again, but have a conflict:
> 
``` bash
 git add filename.c  
 git commit -m "made some wild and crazy changes" 
 git pull origin master 
> ```
>>```
From ssh://gitosis@example.com:22/projectname
branch            master     -> FETCH_HEAD
Auto-merging filename.c
CONFLICT (content): Merge conflict in filename.c
Automatic merge failed; fix conflicts and then commit the result.
```

> So you decide to take a look at the changes:

> ` git mergetool `

> Oh me, oh my, upstream changed some things, but just to use my changes...no...their changes...
```bash
 git checkout --ours filename.c 
 git checkout --theirs filename.c 
 git add filename.c 
 git commit -m "using theirs" 
```

> And then we try a final time

> ` git pull origin master `
>>``` 
From ssh://gitosis@example.com:22/projectname
branch            master     -> FETCH_HEAD
Already up-to-date.
```

> Ta-da!
## A list of resources 
  * `man git ... `
  * [git for hackers](http://wildlyinaccurate.com/a-hackers-guide-to-git#introduction)
  * [Stackoverflow top rated git questions](https://stackoverflow.com/questions/tagged/git)
  * [For when your HEAD is detached](http://stackoverflow.com/a/5772882)

## Definitions

####Repository
- A directory containing all of the project's source code and a .git directory in it as well

* * * 
#### Stage 
- this is basically me telling git "Hey I have some files I messed with and I am almost ready to commit them"

* * * 
####Commit 

- Record changes to the repository [[man page]](http://git-scm.com/docs/git-commit)

* * * 
####Clone 
- Create local copy of a remote repository [[man page]](http://git-scm.com/docs/git-clone)

* * * 
####Push
- Send all previous commits to the remote repository [[man page]](http://git-scm.com/docs/git-push)

* * * 

####Pull 
- Get the latest code from the remote [[man page]](http://git-scm.com/docs/git-pull)

* * * 
####Branch 
- [man page](http://git-scm.com/docs/git-branch)

* * * 
####Origin
 -  The commonly used name for a remote repository
 
 * * *  
####Master
 - The main or production branch of a repository
 

* * * 
####Init 
- Creates a git repository [man page](http://git-scm.com/docs/git-init)

* * * 
####Remote 
- A repository hosted on a server [man page](http://git-scm.com/docs/git-remote)

* * * 





