# Getting started with Git and GitHub

D. Candela 1/25/24

- [Introduction](#intro)
  - [What is this?](#whatis)
  - [What are these things?](#whatare)
  - [Abbreviations, etc. in this document](#abbrev)
- [Part 1: Git+Github without the CLI](#part1)
  - [Baby steps - Your first GitHub repo](#baby)
  - [Basic Git concepts: Commits, clones, pulling, pushing, merging](#basic)
  - [Using Git+Github graphically](#graphically)
  - [Getting started with Github Desktop](#ghdt)
  - [Git Branching](#branching)
  - [Try out Git branching](#trybranching)
  - [Using GH to work with collaborators](#collabs)
  - [Try collaborating with another person](#trycollab)
  - [Using pull requests with work done locally](#useprs)
  - [Git+GitHub for Jupyter Notebook / Google Colab projects](#jngc)
  - [Contributing to open-source projects](#opensource)
  - [Using Git+GitHub for backup??](#backup)
- [Part 2: Using Git in the CLI with GitHub](#cli)
  - [The CLI in different operating systems](#clios)
  - [Install Git on your laptop/PC and set to authenticate](#installgit)
  - [Get a remote repo with local clone(s)](#getremote)
  - [Seeing branches and their commit history](#seebranches)
  - [Some typical Git workflows, from simple to more complex](#workflows)
  - [1. A simple one-person, one-branch Git workflow](#oneperson)
  - [2. All about merging: Fast forwarding, divergent history, conflicts](#merging)
  - [3. A collaborative workflow: Feature branches and pull requests](#feature)
  - [Aside: Rebasing and squashing](#rebasing)
  - [4. Open-source workflow (briefly)](#opensourceflow)
  - [Small groups with differing roles](#differing)
  - [Appendix A: Markdown](#markdown)
  - [Appendix B: Using GH to develop and distribute a simple Python package](#pythonpackage)

## Introduction <a id="intro"></a>

### What is this?<a id="whatis"></a>

This is an elementary introduction to Git and GitHub, with an emphasis on the concepts that I found confusing when I first learned about these things -- these are the explanations I wish I had when I was getting started.
The goal is simply to get started using these things; more detailed and advanced documentation is abundantly available elsewhere.

Unlike many intros to Git+GitHub, this tutorial **goes as far as possible without making use of CLI** (command line interpreter), which is only discussed in the (long) last section.
Here is [another, more fully developed tutorial](https://jcszamosi.github.io/mcmaster_swc_git_gui/08-conflict/) I have found on **using Git+Github graphically** -- includes screenshots, diagrams, etc., unlike this document.

### What are these things?<a id="whatare"></a>

- [**Git**](https://en.wikipedia.org/wiki/Git) is a “version control system” created in 2005 that is used by GitHub, but Git can also be used on its own without GitHub, or with a GitHub alternative.
  (In British English it’s not a compliment to call someone a git.)
  
  - The basic function of Git is to help you and your collaborators create and maintain the **set of files** which make up your project, called a **repository** or **repo**.
    The files in a repo can be at the top level of the repo, or in sub (or sub-sub…) directories of the repo, and this structure is maintained by Git (but the directories themselves are not considered to be files in a Git repo).
    - Specifically a **Git repository** is a set of files being managed by Git, indicated by the presence of the special directory `.git` at the top level of the repo.
      You shouldn't do anything to this directory or its contents.
      You won't even see the `.git` directory when working in graphical environments like GitHub.
  - Git is used to keep track of versions of your code and let multiple people collaborate on it, but it is not used to write, test, and run your code -- you do that with your usual tools such as an IDE like Spyder or PyCharm (or even Jupyter Notebook / Google Colab, discussed in more detail below).
    - However Git hosting services like GitHub do additionally offer tools to develop code (GitHub Codespaces) and to run and test code (Github Actions).
      These are not covered in this document.
  - Git is often used through its **CLI** (command line interface) which means entering text commands into a terminal program like Windows Command Prompt. This can be a hurdle for new programmers.
  - [**GitHub Desktop**](https://desktop.github.com/) provides a GUI version of Git (graphical, no CLI commands needed).
    GitHub Desktop is available for Windows, macOS, and (unofficially) Linux.
  - For many, the easiest way to get started with Git will be to **set up a GitHub account** and use it to start a simple project.  To continue on this path without using CLIs, you will soon want to **install and start using GitHub Desktop.**
    Alternatively you may want to start using Git at the command prompt -- both paths are sketched below.

- [**GitHub**](https://github.com/) is an online platform that uses Git to help teams work together on software (and other!) projects.
  It helps keep track of changes and versions and helps team members communicate with other – and also with the world at large, if you choose to make your projects public.
  It’s useful even for a team of one, as it makes it easy to back up, archive, and keep track of your projects (more on backup/archiving below).  GitHub was created in 2008 and quickly became wildly popular with developers; it has been owned by Microsoft since 2018.
  
  - GitHub is a "Git server" - it stores Git repositories in the Cloud, and uses Git to enable people working separately and remotely on a project to collaborate without getting file versions mixed up or incompatibly modified.
  - GitHub provides additional functions beyond Git, most basically a sort of **social network** for discussing and implementing changes to the repo.
  - Unlike Git, GitHub has always had a browser-based, graphical interface (in addition to being accessed remotely by Git CLI commands).
  - GitHub is currently the most popular Git server, but there are others such as **Bitbucket**, **GitLab**...
    Only GitHub is discussed here.
    Over the years these various services have added many additional features for project development and management -- for example GitHub can host web pages that are automatically rebuilt when the underlying repo is edited.

### Abbreviations, etc. in this document<a id="abbrev"></a>

**GH** = [**GitHub**](https://github.com/), a **remote** (cloud-based or online) application that you use in your favorite **web browser**.
No download or installation is required to use GH, but you must sign up for a (typically free) account.
(In addition to your direct interactions with the graphical GH website, your local computer will communicate with GH using Git, when you use Git in the CLI or GitHub Desktop on your local computer.)

**GHDT** = [**GitHub Desktop**](https://desktop.github.com/), a **local** application that you must download and install on your laptop or PC.
Provides a graphical (GUI) alternative to the CLI, for running Git commands on your laptop/PC.

**CLI** = **Command line interface** which means entering text commands into a terminal program like **Windows Command Prompt** or PowerShell, **macOS Terminal**, or Linux Terminal.
Git commands like `git clone` and `git pull` are entered using the CLI.

Also frequent reference will be made to these concepts:

A **text editor** like **Windows Notepad** or **macOS TextEdit** is used to edit plain **text files** (unlike a **word processor** like MS Word, which is used to edit special `.docx` files).

A **text file** is a file that can be created and edited with a text editor.
Basically a text file consists of a sequence of human-readable (ASCII) characters.
Text files include general purpose text files (`.txt` files), but also computer code like Python scripts and C/C++ programs (`.py` and `.c` files), and [**Markdown**](https://www.markdownguide.org/) (`.md` files, like this document -- discussed a bit more in the last section below).
Even a Jupyter Notebook (`.ipynb` file) is formally a text file, but `.ipynb` files are never directly edited with a text editor because they are too big and complicated.

## Part 1: Git+Github without the CLI<a id="part1"></a>

### Baby steps - Your first GitHub repo<a id="baby"></a>

This is a good way to "break the ice"  before attacking the mysteries of Git ([article](https://stevenkolawole.medium.com/using-github-without-knowing-git-2f535a0e362e), [article](https://pixelpioneers.co/blog/2017/using-github-without-the-command-line)).
To start, gather up the following information:

- One or more files you would like to include in your repo, could be any type (not huge files like videos).
  GH was originally intended to hold **code source files**, for example Python scripts (`.py` files) or C/C++ source files (`.c` files).
- A simple name for your repo (short, all lower case, if more than one word separated by dashes e.g. `my-repo`).
- A short (one-sentence) description for your repo.
- Will your repo be public or private?
  - Public repos with unlimited capabilities are free.
    Anyone can see and download the contents of a public repo, but only you and those you designate as **collaborators** can change things in your repo.
  - Private repos with somewhat limited capabilities are also free.
    You can change whether a repo is public or private at any time.
  - Never put personal, embarrassing, or private info like passwords into a GitHub repo, whether it is public or private.
    Everything you ever put into a repo is remembered forever even if you update things -- that's the point -- unless you go out of your way to erase things.
- Many things in GH are based on your **email address**, so decide which email you will be using if you have several (in which case you may want to set up more than one GH account).
- You will also need a **username** to set up your account, and this will be visible to others for public accounts - I suggest something straightforward like `janedoe`.
  Maybe you'll want to show off some of your GH repos to a future employer!

With this info in hand, go to [GitHub](https://github.com/), and set up a free account.
When you arrive at your Dashboard, hit **Create repository** in the left panel which brings you to the "Create a new repository" page (you can modify all of these things later):

- Enter your chosen **Repository name**, **Description**, and select **Public** or **Private**.
- Check **Add a README file** which will start out as your description.
- Optionally select a `.gitignore` based on the type of file you are intending on uploading, for example `Python`.
  This is a list of file types *not* to upload.
- Choose a **license**, I usually pick `MIT License` which lets others freely use your work provided they also include the license which will have your name.

Hit **Create repository** which will bring you to the main page for your new repository.
In the center you will see a list of the files in your repo, which (if you followed the suggestions above) will be `.gitignore`, `LICENSE`, and `README.md`.
You can examine and edit the contents of any file by clicking on it (compare **Preview** and **Code** views of `README.md`), then return to the main page by clicking on the repo name.

Next, **add some more files to your repo**:

- Hit the **+** next to **<> Code** at the top right of your file list, and select **Upload files**. This will bring you to a page where you can drag and drop the files you want to add.
- Hit **Commit changes** to actually add the files.

**Congratulations, you are now a GitHub user with your first repo.**
What can you do with your repo?
One simple but useful thing for you (and other GH users, if your repo is public) is to **download the files**: 
Under the **<> Code** tab select **Download ZIP**.
Note this just gives you the files in their current state, with nothing about their history.
To do anything more interesting than this you will need to learn something about Git.

### Basic Git concepts: Commits, clones, pulling, pushing, merging<a id="basic"></a>

The most basic concept in Git is the **commit**, which is a **saved snapshot of all of the files in your repo** at the moment the commit was made. (To be picky: at the moment the files were "staged" for commit.)

- Amazingly Git **saves the complete history** of commits since the repo was first created. In other words, Git doesn't just save your files -- it saves **every version of each of your files that occurred in any commit (snapshot of the repo)** . You easily can re-create your files exactly as they existed at any past commit.
- If you think of your repo as a book, doing a commit adds a new page to the book with current information on the complete contents of your files on that page -- but it doesn't affect the earlier pages (earlier commits), which are still there.
- Where are all these commits kept? They are in the `.git` directory mentioned above. Also amazingly **the complete history is saved on every system** (PC, laptop...) that uses the repo, not just on a server like GH.  In the unlikely event that all of GH's storage servers were to fail, the complete history of your repo is still safe and sound on your laptop.
- If you created a GH repo as in the previous section, you can **see this in action** in the **GH web page for your repo:**
  - If you are not already there, click on the **name of your repo** (e.g. in "Your repositories" after clicking your avatar) and make sure the **Code** tab is selected at the upper left.  This is the default view of a repo in GH, with a list of your repo files followed by the contents of your README file, if any.
  - The commit you are currently using (typically the most recent one) is listed on the line above your files. Every commit is labeled by a unique [**SHA code**](https://en.wikipedia.org/wiki/SHA-1) like `77ba2b6` or `bc90c1d` that you see listed in this line -- the full SHA code is 40 characters long, but it usually suffices to use the first few characters like this.  You should think of the SHA as the **name of the commit** which serves to distinguish it uniquely from all other commits (ever made, by anyone, to any repo).
  - Click the **history** dingus in this line (little clock with an arrow around it) to see a list of all the earlier commits.
  - Click any earlier commit to **revert your files** to that earlier state -- this brings you a "diff page" showing how the commit differed from the previous commit, just hit **Browse files** to get the familiar file view.
    If you go to the initial commit, you will see only the files GitHub added (README.md, LICENSE).
  - If you download the files (`<> Code`, `Download ZIP`) you will get the full set of files in your repo as they existed at the moment of this earlier commit.
  - To get back to the most recent commit, go to the **Switch branches/tags** button button at the upper left (which shows the SHA code for the earlier commit you are looking at) and select **main**.

You can do minor editing of your repo files directly in the GH web page, but to develop your project you will generally want to **clone your repo** to the places where you develop and/or run your code -- usually a laptop or PC. **Important terminology:**

- **Local** things (repo, operations, commits...) refer to the copy of your repo that you have cloned to your laptop or PC.
- **Remote** things refer to your repo as it exists on GitHub, i.e. in the cloud, on the web.

Cloning creates from scratch a whole "mirror" of the repo in a new location like your laptop, so you will **only clone once to each new location**. Once you have done this, you can **pull** changes to your project made elsewhere (by you or others) into the local clone from a remote server like GitHub, and **push** changes changes made by you locally back to the remote server. If you are working with **collaborators**, they can make their own clones of the repo, and pull changes from and push changes to the repo on GitHub.

At this point you might wonder what happens if you and your collaborator make **incompatible changes** -- for example, you both edit the same lines of a file. The short answer is that whoever pushes their changes to GitHub first will not notice anything special, but whoever pushes second will need to do something more involved, to **merge** the proposed changes together. Git was made for dealing with this type of situation, as you will see.

### Using Git+Github graphically<a id="graphically"></a>

To make practical use of Git+GH, you need to carry out Git operations like cloning, pulling, and pushing on your local PC or laptop. [**GitHub Desktop**](https://desktop.github.com/) (GHDT) provides a graphical interface to Git, avoiding the command-line interface. (There are [alternative Git GUIs](https://git-scm.com/downloads/guis), not necessarily tied to GitHub - these are not discussed further here.

**What types of files?**
Although Git can be used for any type of file up to some size limit, it was originally intended for **source files** -- human-edited text files like C/C++ programs (`.c` files), Python scripts (`.py` files), and text and Markdown (`.txt`, `.md`) files which are often used for documentation.

- These files are relatively small, so it is reasonable to keep complete version histories.
- Version differences (such as incompatible changes to a file by two collaboraors) are easily seen and merged in a text or code editor.

Conversely **files generated from the source files** are usually **excluded from Git version control** by putting entries in the `.gitignore` file. These would include things like C/C++ object files (`.o` files) and Python bytecode files (`.pyc` files).

- When you create a new repo on GH, you can select a pre-made `.gitignore` for the type of project you are doing.
- If you omitted doing this, you can find these pre-made `.gitignore` files [here](https://github.com/github/gitignore).

This model does not fit in well with **Jupyter Notebook / Google Colab** (`.ipynb`) files -- when using JN/GC you edit your work **graphically**, rather than working directly with the `.ipynb` file (which is long and unreadable).
Using Git+GH for JN/GC is discussed separately below.

**Where should your local (cloned) repo be?**
Before you get started you should decide **where** on your Laptop or PC you want your repo to reside -- where is it most convenient to access your repo files to work on them and try them out? If you don't select a location, GHDT will propose a default location such as `Documents/Github/<repo-name>`. In any case, you will need to find this location on your laptop/PC so you can edit and/or test the files in your repo.

**Possible point of confusion:** You will **use GitHub Desktop (or Git)** to synchronize the files in your local repo on your laptop/PC with the files in your remote repo on GitHub, as shown below. **Do not use "Download ZIP"** on GitHub to download your repo files to this same place on your laptop/PC. The function of "Download ZIP" is to download the **files only, without the repository history** to some other location where they might be used. But you can also use your code directly in your cloned repo on your laptop/PC -- though in this case you should take care that any files generated by your code are blocked (by `.gitignore`) from being managed by GIT.

FInally **do not set your GH repos to sync automatically** to a backup site like Google Drive or MS Onedrive, although you can **copy a whole repo to a backup site when you are not actively using it**.  Why not sync? Because, as explained in detail below, your repo files will **change every time you "check out" a different commit or branch**.

### Getting started with Github Desktop<a id="ghdt"></a>

- If you haven't already done so, **get a GH account** and use it to **make a repo** as describe in "Baby Steps" above.
- Install **GHDT** on your laptop or PC.
  For Windows or macOS, GitHub provides installers [here](https://desktop.github.com/), but for Linux you will need to use a [third-party fork](https://github.com/shiftkey/desktop/).
- The first time you run GHDT it will ask you for your Github username and email which should **match your GH account**, and it will let you **login to GH** which you should do.
  (At present it seems difficult to switch between two different GH accounts on GHDT.)
- **Clone** your repo on GH to your laptop/PC:
  - In GHDT use the down-arrow in **Current repository**, select **Add/clone repository** which should show your repositories on GH, select the one you want and **where you want it to go** (see above) and hit **Clone** which should bring you back to the main screen with your repo listed at the upper left and **No local changes** displayed.
  - Now, if you **look at your files on your laptop/PC** you should see your repository with its `README.md`, `LICENSE`, and other files.
  - You can even see the`.git` directory, but only if you "show hidden files" (on Windows or Linux).

**Make some changes to your repo locally, then push them to Github:**

- Go to the repo directory on your laptop, and use a text editor to **edit one of the files** such as `README.md`.
  Add a line, and also delete a line or part of another line (and save the file).
- Also use your text editor to **make a new file** in your repo directory.
- On GHDT:
  - On the left panel you should see listed the file you changed with a yellow symbol, and the file you added with a green symbol.
  - Clicking on either of these, main panel will show the **diff** (differences from the committed version) for the selected file, with additions highlighted in green, and deletions highlighted in red..
  - To **commit these changes to your local clone**, at the bottom of the left panel type a short **commit message** (summary), then hit "Commit to main".
  - If you look at your repo in the GH website, you will see your changes are not there yet - you need to **push** them to your GH (remote) repo.
    So, in GitHub Desktop (in the list of friendly suggestions) hit **Push to origin**.
    (In Git **origin** means the remote repository from which the local repository was cloned.)
- In the GH web page for your repo, you probably need to click on the repo name to update the page.
  Now you should see your updated files:
  - Next to each file that was changed or added, you will see your commit message.
  - Clicking on a file shows its new contents.
  - Clicking on history gives the list of commits, and clicking on any commit (e.g. the most recent on) will show the **diff** of any selected file from the previous commit.

### Git Branching<a id="branching"></a>

This is one of the most powerful (but also confusing) features of Git. Everything above was done using a single **branch** called `main`, which you may have noticed listed in various places. In this model the **commits form a linear sequence** like (`Initial commit` -- `Update REAME.md` -- `Fix bug in foo.py` -- `Add new function`) that ends up at the **most recent commit**, which is what `main` refers to. Every time you commit changes, you **add a new commit to the end of this list** and **`main` moves to refer to the new, added commit**.

But you may want to **try out some changes or additions** to the code, that may or may not be a good idea, without affecting the `main` branch (at least until you get the bugs out of your new code). For this purpose you can **create a new branch**, called whatever you want, e.g. `newfeature`, and switch to this new branch. Now, when you commit changes, the new commits will make **a side list of commits** that branches away from `main`, starting at the commit at which you created the new branch. Your new branch `newfeature` will **refer to the most recent commit on this side list** and will advance as commits are made on this branch.

After a while, if your new branch works out well, you may want to **merge it back into the `main` branch**. But until that point you can let your `newfeature` branch sit around until you have time to work on it, or you could even delete it without merging which would eventually **irretrievably discard** any commits you made on `newfeature` (Note: this is the first time we have encountered permanently loosing commits.)

Here are some points I found confusing when I first learned about Git branching (you may want to **skip this section** on a first reading):

- When you use branching, the sequences of commits forms a tree-like structure, with a limb of the tree growing out from `main` at the point where a new branch like `newfeature` was created.
- Also (unlike most real trees, I think) it is possible for two limbs of the tree to grow back together when a merge is done -- for example if `newfeature` is merged into `main`.
- In Git, **a branch name refers to the growing *tip* of a limb, not the limb itself**.
  Thus `main` always means the **last commit** done on the `main` branch, and `newfeature` always means the **last commit** done on the `newfeature` branch.
- When you **merge** `newfeature` back into `main`, a new **merge commit** is added to the repo which has the most recent commits in both `main` and `newfeature` as parents, and `main` now points to this new merge commit.
  Therefore, the history of `main` now includes both limbs (strings of commits) that were merged together. At this point, it is usual to **delete the branch `newfeature` that was merged in** -- but no commits are discarded by this delete-branch operation, since both strings of commits are now in the history of `main`!
- Conversely if you delete the branch `newfeature` without merging it into another branch like `main`, some commits will (eventually) be lost, namely the commits added to `newfeature` since it was created. These commits that were in the history of `newfeature` are no longer the history of anything, so they will (eventually) be "garbage collected" by Git.
- In summary a Git **branch** is just a pointer to the end of a sequence of commits -- one of the growing tips of the commit tree. It is trivial and fast to create a new branch in Git, since all this does is create a new direction in which *future* commits will be added to the tree.

### Try out Git branching<a id="trybranching"></a>

The operations shown here are all carried out on a local repo you have cloned onto your laptop, using **GHDT**. (You could also carry out the same steps in the GH page for a remote repo -- but you would find that the final step listed here, merging two branches, must be carried with a two-step "pull request" process - this is covered in the section below on working with collaborators.)

- Start GHDT and switch to the repo you want to use (if you have more than one).
  Make sure the "Current branch" is set to `main`. Typically you will see "No local changes" unless you have edited your repo files and not committed the changes.

- **Add a new text file to your repo.**
  Use your text editor to create a new file `myfile.txt` with a few lines of text in it, and save it in your repo directory on your laptop. Now, in GHDT you should see "1 changed file" in the left panel - **hit "Commit to `main`"** at the bottom which will add `myfile.txt` to your repo. (Note: For what we are doing here, which is entirely in your local repo, you do *not* need to "Push commits to the origin remote".)

- **Make a new branch.**
  Under "Current branch" at the top (which currently shows `main`)  **hit "New branch"**, enter `mybranch` in the Name box, and hit "Create branch". Now "Current branch" should show you are on your new branch `mybranch`.

- **Change a file in your new branch.**
  Go back to `myfile.txt` in your text editor, **change one line in the file** by adding or deleting some words, and save the file. Back in GHDT you should again see "1 changed file". **Hit "Commit to `mybranch`".**

- When you switch branches, **Git changes the files in your repo** to **match the branch you are now on**, which is called the branch you have **checked out**. This is probably **unlike anything you have done before** -- you are probably not used to seeing your files changing on their own! To see this in action:
  
  - If you open `myfile.txt` in your text editor, you will see the modified version -- no surprise.  **Close the editor** (very important).
  - In GHDT, switch "Current branch" at the top from `mybranch` to `main`, then open `myfile.txt` again -- it has switched back to the unmodified version! (You can perhaps see why switching branches while a file is open in an editor will lead to confusion -- when you save from the editor Git may declare that you have made changes to the file even if you haven't, because it doesn't matches the branch now checked out)

- **You have successfully branched away from the `main` branch!** At this point, you could merge `mybranch` back into `main`, which would uneventfully update `main` with the change you made in `mybranch`. Much more interesting is to...

- **Make an incompatible change in the `main` branch**.
  (If you haven't already) use "Current branch" to select  `main` then open `myfile.txt` -- as noted above you will see the *unmodified* version. Make a **different change** to the same line of `myfile.txt` that you modified earlier, save (and close the editor!). Then in GHDT **commit the changes to the `main` branch.** Now your repo has **divergent history** -- `myfile.txt` has been modified in two different ways, on the two branches.

- **Merge the branches, manually resolving the conflict.**
  
  - In the "Current branch" tab be sure the current branch is set to `main`, then down at the bottom hit "Choose a branch to merge into `main`" and select `mybranch`.
    You will see "**There will be 1 conflicted file when merging `mybranch` into `main`**".
    This is because you have made inconsistent edits in the two branches. to `myfile.txt`
  
  - Hit "Create a merge commit".
    This will open a "Resolve conflicts before merge" box, which will have a button to **open the file with conflicts in an editor** - do this.
    You should see something like this:
    
          Stuff that wasn't changed
          <<<<<<< HEAD
          Line as it was changed in main
          =======
          Line as it was changed in mybranch
          >>>>>>> mybranch
          More stuff that wasn't changed
  
  - Here `<<<<<<< HEAD`, `=======`, and `>>>>>>> mybranch` are **conflict markers** that Git has added to the next version of `myfile.txt`. It's up to you to **remove these conflict markers, and edit the file to be the way you want it to be** -- keeping what you want from the conflicting sections between the markers. Go ahead and open `myfile.txt` in your text edito do this, being sure to fully remove the conflict marker lines, and save the file (and close the editor!).
  
  - `HEAD` means the currently checked-out branch, in this case `main`. The merge operation will add a new commit to this branch, with the repo files as merged -- in other words, it will update the `main` branch with the changes from `mybranch`, resolving conflicts in the manner you have chosen.
  
  - Go back to GHDT -- you should now see "No conflicts remaining". Hit "Continue merge".
  
  - If all has gone well, you have **successfully merged `mybranch` into `main`**! As a result, on the `main` branch, `myfile.txt` is as you have edited it to resolve the merge conflicts.

**Note:** Git can notice obvious incompatibilities like two different edits of the same lines in a file, or the deletion of a file by one collaborator while it has been edited by another collaborator. But Git does not know how your project works in detail! So if one collaborator changes the name or calling signature of a function this might make the code incompatible with other collaborators' versions, and Git cannot catch things like this. This is what **GH pull requests** (described below) are for, as they automatically open up a discussion between collaborators of the proposed changes.

### Using GH to work with collaborators<a id="collabs"></a>

Here we consider a project developed by **a small team** - say 1-5 people - all of whom will have **write access** to the repo, which implies a certain level of trust. This is what GitHub calls the [Shared repository model](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/getting-started/about-collaborative-development-models) for collaboration. This is different from working on a large, open-source project which is discussed in a later section.

Although Git can keep the versions of a project's code organized, if two or more people are collaborating on the project additional communication is required -- to propose and discuss changes, for example, and to decide when to include them in the main version of the project.

Sites like GH include "social media" functions that are not part of Git, to facilitate collaborations. These include **invitations to collaborate**, opening and discussions of **issues**, bringing things to the attention of other users with **@mentions**, and opening discussions of **proposed changes to the repo files** which are called a **pull requests**.

### Try collaborating with another person<a id="trycollab"></a>

You and a partner should **try this both ways around** (you as the repo owner and your partner as the collaborator, and vice versa). When first learning how this works I suggest sitting so you can see each other's screens and discuss what is going on.

- You and your collaborator should **both have GH accounts**, and **you should own the repo** you want the other person to collaborate on.
- In the `Settings` tab of your repo on the GH website, hit `Collaborators`, then in the `Manage access` box hit `Add people`.
- Enter the username or email of your collaborator to find them, and then the `Add .. to this repository` button.
- This will send an invitation to your collaborator, both via email and as a notification in GH. Once your collaborator accepts, they will have **push** (i.e. write) access to your repo.
- Rather than pushing changes directly to `main`, your collaborator should **make changes in a separate branch** and then create a **pull request** to initiate discussion/editing of the changes. Finally the **pull request will be merged** to actually make the changes to `main`. The detailed steps to try this out are given below. Even though you own the repo, if you have collaborators you should also use pull requests to make changes, as this leaves a good record of what was changed and why.

**Your collaborator makes some proposed changes on a new branch, then opens a pull request:**

- Your collaborator can see and write to your repo on GH, and they should go to your repo if they are not already there (clicking the Octocat logo at the upper right should show a clickable list of repos they have access to).
- In your repo on GH, they should **make a new branch:** Under the branching button at the upper left they should enter the name for their new branch e.g. `cobranch`, then hit "Create branch `cobranch` from `main`". They will now be on the new branch.
- They should **open one of the files in the editor and change it (e.g. add a line)**, hit the **Commit changes...** button, **Commit directly to the `cobranch` branch**.
  - An alternative workflow is for your collaborator make their changes while still in the `main` branch, and then select "Create a new branch for this commit and start a pull request." Doing things as suggested above lets you make any number of edits and commits to your new branch `cobranch`, before making a pull request.
- Next your collaborator should open a **pull request** to merge `cobranch` into `main`:
  - You will probably see a box with "`cobranch` had recent pushes..." with a button **Compare & pull request** you can hit.
  - Alternatively under **Pull requests** at the top you can hit **New pull request**.
  - In either case make sure the branches are set for **base: `main`** and **compare: `cobranch`** (you will need to set this manually if you use "New pull request"). The pull request is to **update the "base" branch** by **merging in the differences in the "compare" branch**.
  - Add (or accept) the title and **add a description**.  Notice the description box is large and can use Markdown formatting, hyperlinks, attachements... The description is used to **start a conversation about the proposed changes** and can include **@username**'s to notify other GitHub users. With your description written, hit **Create pull request**.

**You and/or your collaborator continue working on, merge, or close the pull request.** As both you and your collaborator have write access to the repo, the following actions are available to both of you (you can both use "Pull requests" at the top to open the pull request):

- If (as should be the case here) there are no merge conflicts) you can **Merge the pull request** - use the first option "Create a merge commit" until you lean more about the other options (squashing or rebasing). This will incorporate the proposed changes into `main`.
- If there are **merge conflicts**, they will need to be **resolved** as described above under Git Branching.
  - This could happen if other changes get merged into `main` while your pull request is sitting un-merged.
  - You can "clean up" your pull request without affecting `main` by merging `main` into your branch `cobranch` and fixing the conflicts yourself.
- You can **improve** the proposed changes in your branch `cobranch` (and commit the changes to that branch) based on an exchange of **comments** (available to make in the pull request page) before merging the pull request.
- Finally, if it is decided that the proposed changes should not be made at all you can **Close the pull request**. This will not delete the `cobranch` branch unless you select that option.

### Using pull requests with work done locally<a id="useprs"></a>

In the practice exercise suggested above, the changes proposed for a pull request were made directly in the GH website. More realistically you and your collaborators will develop improvements to the project **locally**, i.e. on your laptops/PCs where you can edit the files and run the code using your usual tools (e.g. Spyder). Here is what the workflow looks like in this case:

- **Clone the repo** (either one you own, or one you are a collaborator on) to your laptop/PC **only once**.

- Use the "Current branch" tab in GHDT to **make a new branch** e.g. `mybranch`.
  For now this new branch only exists locally (on your laptop/PC).

- With `mybranch` checked out (showing in the "Current branch tab") make the changes you want to propose to your repo files. Then in GHDT **Commit the changes to `mybranch`**.

- In GHDT hit **Publish branch** (which pushes it to the remote repo on GH). Then, under "Preview Pull Request" you have the opportunity to create a pull request-- also on the GH page for the repo you can create a pull request. But you don't need to create a pull request yet, if you prefer not to -- instead you could make more changes to the repo files, commit them locally, and **Push again**.

- When you are ready, **create the pull request** which will happen in GH whether you initiate it from GHDT or GH. As above, to merge the changes in `mybranch` into `main`, `mybranch` must be the compare branch and `main` must be the base branch.

- After the pull request is merged on GH (or when any other changes are made to the repo remotely) you will probably want to update your local repo by doing in GHDT:
  
  - **Fetch origin** to retrieve the current state of the remote repo (origin), then
  
  - **Pull again**  to merge the retrieved remote state into the corresponding local branches.
    
    It is valuable to update your local repo in this way before creating and starting to work on a new branch, so you are starting with the current state of the remote repo.

**Summary:** Now you and a couple of collaborators can work together on a coding project stored on GitHub, but cloned to each collaborator's laptop for code development and testing (in Spyder, for example).

Using GitHub's social features (pull requests for proposed code changes, also opening issues for things where the required code changes are not yet clear or are someone else's responsibility) you can keep the work of you and your collaborators synchronized.

If necessary you can use Git's merging tools to resolve any divergent changes made to the files.

### Git+GitHub for Jupyter Notebook / Google Colab projects<a id="jngc"></a>

Jupyter Notebooks (any by extension Google Colab projects) do not fit into the original model for Git/GitHub, because the actual source file for a JN (the `.ipynb` file) is an unreadable morass of JSON text.

As explained [here](https://towardsdatascience.com/how-to-use-git-github-with-jupyter-notebook-7144d6577b44), the basic collaborative flow using pull requests works for JNs, with the exception of visualizing changes made (diffs between notebook versions). According to this page, you should use a third-party application like [ReviewNB](https://www.reviewnb.com/) to effectively visualize JN diffs (I haven't tried this yet).

One thing to perhaps be careful about: Individual JN (`.ipynb`) files can sometimes be very large as they can contain generated output and uploaded items including pictures, videos, machine-learning data sets... Git can store many versions of a file in your repo, which would then make your Git history even larger.

### Contributing to open-source projects<a id="opensource"></a>

Many **open source** projects (projects with the complete source code publicly posted) are hosted on GitHub and many also encourage contributions from their **communities** which could include you! GitHub has excellent [guides to open source projects](https://opensource.guide/) which you should consult if you are interested in contributing to an existing project, or even setting up your own open-source project.

Here we briefly outline the typical GitHub "workflow" for making a contribution to an open source project (many more details in the guides linked above). This workflow uses a concept that is in GitHub but not in Git, the **fork** (and also the **pull request** already discussed above).

- Go to the GitHub page of a project you might contribute to ([Github's guide to contributing](https://opensource.guide/how-to-contribute/) has lots of suggestions on finding suitable projects). You will not be the owner, or have write access to the repo.
- **Fork** the repo using the `Fork` button at the upper right. This makes a **whole new repo on GitHub, that belongs to you** and is a full copy of the original repo.
- **Clone** your forked repo to your local machine so you can work on it, as usual.
- **Create a new branch** to hold your contribution or edits, and start working on your contribution.
- At some point (usually before everything is perfect) **open a pull request** which will invite others to comment on and help improve your contribution. Even though you are working with your own fork of the project repo, your pull request can automatically communicate with the owners and contributors to the **upstream** project repo that was the the source of your fork.
- Eventually the project maintainers may accept your contribution by **merging it into the project repo**.

Scared off by this?  Look at the [First Timers Only](https://www.firsttimersonly.com/) page for some support and encouragement.

### Using Git+GitHub for backup??<a id="backup"></a>

Reasons you could lose your work if you don't back it up frequently (in order of decreasing likelihood, for me):

- Accidentally delete things by typing the wrong thing.
- Realize some past things would be useful but discarded them (or simply can't find them).
- Hardware failure of a laptop or PC drive
- Malicious deletion by someone.

If you push your  local repo to a remote repo on GitHub that only you can write to (and even better, clone it to more than one laptop/PC), you are reasonably protected from most of these things. However,

- GitHub themselves recommend downloading your repo locally then [backing it up somewhere else](https://docs.github.com/en/github-ae@latest/repositories/archiving-a-github-repository/backing-up-a-repository).
- Various pay-for backup services can explain why you need to use them rather than using GitHub as your backup.
- It is possible for a malicious actor who has write permission on your repo to use Git commands to delete your commits.
- The "intended" use for Git+GitHub is code development, not backup. Thus it is intended that creation of (more or less) functional code take place in your local repo (which is not backed up to GitHub), and only pushed to a GitHub repo when ready for discussion and fine-tuning. To use GitHub as backup you must go against this intended workflow by pushing your work to GitHub whenever you have spent significant time on it, no matter how incomplete it is.
- Free, private repos on GitHub are somewhat limited and public repos expose your work to the world, which you may or may not want.

Despite all these caveats, I find that pushing to GitHub repos I own (and cloning them to other PCs) meets my modest needs for backing up my coding projects. Here are a couple of additional things you could do, to back up your repo outside of GitHub:

- There is a command [`git bundle`](https://git-scm.com/book/en/v2/Git-Tools-Bundling) that can put the entire contents of a repo (including history) into a single file, which can then be archived somewhere. I haven't tried it.
- You can download the files of any particular commit (such as the latest one in `main`, by setting the branch to `main`) using "Download ZIP" under **<> Code**, and then archive this ZIP file. But this will *not* include any repo history.

## Part 2: Using Git in the CLI with GitHub<a id="cli"></a>

As shown in the sections above, you can do quite a lot with Git+GitHub while never touching the CLI. However, depending on your path, it is not unlikely you will find yourself using **Git in the CLI** (issuing `git` commands) along with the graphical **GH website** -- so here is a "getting started" tutorial. THe key resources for Git in the CLI are the [online book Git and docs](https://git-scm.com/book/en/v2) maintained by the Git project. The book has a very helpful [**summary of the most common Git commands**](https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Setup-and-Config) with links to more detailed explanations.

**This section assumes you have read the sections above,** so you are familiar with basic Git and GH concepts: Repos, commits, clones, pulls, pushes, merges, branching, pull requests... 

**Some encouragement:** While there are [many Git commands and options](https://git-scm.com/docs), you will find that Git is quite good about suggesting what commands are needed in each situation. Git is also quite good about refusing to do things that would lose your work.

**Side note:** [GitHub CLI](https://docs.github.com/en/github-cli/github-cli/about-github-cli) is a completely separate CLI application called `gh` (as opposed to `git`) that enables accessing the features of Git+GH from the CLI.  GitHub CLI is *not* covered here.

### The CLI in different operating systems<a id="clios"></a>

The `git` commands shown here were tested on an Ubuntu (Linux) PC, but (apart from installation steps) they should work in terminal programs in Windows (Command Prompt, Powershell) or macOS (Terminal).

**CLI commands are shown here with the Bash prompt $ but on Windows and macOS the prompt will be > instead.** For Windows users here are some alternatives to Command Prompt or Powershell that are supposed to give you a more Linux-like experience (I haven't tried them; I believe macOS is already very Linux-like):

- You can run **Git Bash**, which comes with [Git for Windows](https://gitforwindows.org/).

- You can run a whole Linux system under Windows using [WSL](https://learn.microsoft.com/en-us/windows/wsl/).

### Install Git on your laptop/PC and set to authenticate<a id="installgit"></a>

**Get a GitHub account** if you don't already have one (see "Baby steps..." above) -- you will want your GH email and username to configure Git on your local laptop/PC to communicate with GH.

**Install Git locally** (only once on each Laptop/PC). First see if Git is already installed by trying

    $ git --version

 The Pro Git book shows [how to install Git on Linux, Windows or macOS](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git). On Linux the command is

    $ sudo apt install git-all

**Set some defaults** for the current user (settings with `--global` apply to all work by the current user). **The username and email should match the GH account you will be using.** The third command sets the default branch name for a new repo to be `main` (as GitHub now does) rather than the earlier `master`. The fourth commad sets the `git pull` command to use merging rather than rebasing (all explained below).

    $ git config --global user.name 'janedoe'
    $ git config --global user.email 'doe@physics.umass.edu'
    $ git config --global init.defaultBranch main
    $ git config --global pull.rebase false
    $ git config -l   # optional, to list current config settings

**Authentication of Git on the CLI to GitHub.** This is an unfortunately messy topic -- I would have liked to have had the following info when I was first trying to get things working.

Commands like `git clone` from a private GH repo and `git push` to any GH repo require [authentication to your GH account](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github), that is proof that you are who you say you are before you can do anything to a GH repo. Although running these `git` commands will ask for your GH username and password, **GH no longer accepts these credentials in the CLI.** Here are some other methods that are currently accepted:

1. Using [Git Credential Manager (GCM)](https://github.com/git-ecosystem/git-credential-manager) to authenticate in a browser, caching credentials in RAM. This is the method I used before I set up SSH keys (see below); it's reasonably convenient.
   
    For **Windows** you should already have GCM installed and set up to work when you installed [Git for Windows](https://gitforwindows.org/).
   For **macOS** or **Linux** follow the installation instructions at the GCM link above.
   Here are the Linux installation and configuration steps:
   
   - Go to the GCM page listing packages, click on `amd64..deb package` to download and put some location such as `~/Downloads/gcm-linux_amd64...deb` (here `...` is the version number).
   
   - Install the GCM package and configure it to use the RAM cache to temporarily store credentials, which should be moderately secure:
     
          $ sudo dpkg -i ~/Downloads/gcm-linux_amd64...deb
          $ git config --global credential.crendentialstore cache
          $ git-credential-manager configure
     
     Now, when you do a command like `git clone` that requires authentication to GH, you should have the option of authenticating in a browser window, which will only require your user name and password (and won't even ask for these once they are cached).

2. Use a [GH personal access token (PAT)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) (not to be confused with a [GH passkey](https://docs.github.com/en/authentication/authenticating-with-a-passkey/about-passkeys), which can be used to authenticate in a browser, not the CLI). I got PAT authentication to work, but did not find it to be very convenient.
   
    A PAT is a temporary, strong password that you get GH to generate for you (hit your avatar, Settings, Developer Settings, Personal access tokens). The PAT will be displayed only once for you to copy and paste somewhere - into a file (not very secure) or a password manager. Then, you can paste it into the CLI when `git` commands ask for a password.

3. Use an [SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account). With this method a private/public key pair is generated on each laptop/PC that needs to authenticate, and the public key is added to your GH profile. This is the method I currently favor -- it is convenient to use once you it is set up (which is a bit of a pain), as authentication to GH happens automatically.
   
   Here are the steps to set up an SSH key for GH on a Linux PC, pieced together from [this GH doc](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) and [this page from ssh.com](https://www.ssh.com/academy/ssh/keygen) (I have also gotten SSH keys to work on Windows, the steps are nearly the same):
   
   - Generate a key pair.  Here `<email>` should be the email associated with your GH account.
     
           $ ssh-keygen -t ed25519 -C '<email>'
           (verifies file location, asks for passphrase)
     
     You can accept the default path and filename for saving the key, and either enter a passphrase (which you would need to enter every time the key is used) or none (my choice).
     
     - If the `ssh-keygen` command is not found, install as follows then retry (should only need to do this once on each PC):
       
             $ sudo apt-get install openssh-client
   
   - Add your newly-generated key to `ssh-agent`. This and the following step assume you accepted the default path `~/.ssh/id_ed25519` for the SSH key. (On Windows I had to run `start-ssh-agent`, which then automatically did the `ssh-add`.)
     
           $ eval 'ssh-agent'  # checks that ssh-agent is running
           Agent pid 552292    (actual pid will be different)
           $ ssh-add ~/.ssh/id_ed25519
           Identity added: ...
   
   - Add the key to your GH account for authentication (can also add it for commit signing, not shown here). Start by copying your public key to the clipboard (in Windows Command Prompt use `type` rather than the Linux `cat`.)...
     
           $ cat ~/.ssh/id_ed25519.pub
           (copy the output to the clipboard...)
     
       ...then in GH click your avatar, settings, Access SSH and GPG keys, new SSH key. Paste the copied key into the Key field, add a brief title like "Office PC", add key and agree to everything.
   
   - If you originally cloned a repo using non-SSH authentication (i.e. from an HTTPS address) and your want to start using SSH, you can fix this using `git remote remove` and `git remote add` commands. But it is usually easier to simply delete (or rename) your existing clone (assuming it does not contain unsaved work!),
     
           Github$ rm -r -f myrepo    # -f removes protected files
     
     then clone again with the new SSH address.
   
   If all went well you will now be able to do operations requiring authentication to GH without entering passwords or opening a browser window, unless you set a passphrase on your SSH key. However the *first time* you use a new key you will see something like "The authenticity of host github.com (...) can't be established" - just answer yes (or, if you prefer, you can read plenty of commentary about this message on stackoverflow, etc...).
   
   **Managing more than one GH account** from the same PC.  Here's a [resource](https://blog.gitguardian.com/8-easy-steps-to-set-up-multiple-git-accounts/), haven't tried this yet.

To proceed, you must set up one or another of these authentication methods. The **way you specify a remote repo in `git` commands** depends on the method used. Assuming the name of your repo on GH is `myrepo`,

- For non-SSH authentication the remote repo is specified by an HTTPS address like `https://github.com/<username>/myrepo.git`.

- For SSH a uthentication the remote repo is specified by an SSH address like `git@github.com:<username>/myrepo.git`.

- Both types of address are available for copying under the **<> Code** tab of your repo on GH.

### Get a remote repo with local clone(s)<a id="getremote"></a>

**Get a new repository on GH that you will clone locally.**

It's possible to proceed in the opposite direction than shown here, first using `git init` to turn a local directory into a Git repository and then connecting it to an empty GH repo  using `git remote add`, and it's also possible to use Git completely locally, i.e. not using GH at all. But I find it easier to proceed as follows:

Follow the **Baby steps..** section above to get a new GH repo with your desired repo name, `LICENSE` and `README.md` files, and a `.gitignore` file appropriate to your project.

**Clone your GH repo locally.** Go to the directory on your laptop/PC where you want to keep and work with your repo files, for example `~/Documents/Github`, and do

    ~/Documents/Github$ git clone <address>

where `<address>` is the HTTPS or SSH address of your repo copied and pasted from the **<> Code** tab of your repo on GH. This should create your repo as a subdirectory of the current working directory e.g. `~Documents/Github/myrepo`.

**Note:** If `Github/myrepo` already exists and contains files, `git clone` will fail. So if you already have files you want to put in your repo, copy them into to `Github/myrepo` **after** you do the `git clone`. Then, as shown below, you will be able to stage and commit them to your repo.  If you already cloned a repo using GHDT, you can use it in the CLI without cloning again.

**Further `git` commands specific to a repo must be done in the repo directory or its subdirectories** or you will get the error "not a git repository".   For example, to see the fetch and push addresses of the remote repo (which is abbreviated `origin` by default) from which the local repo was cloned do

    Github$ cd myrepo               # cd into the repo
    Github/myrepo$ git remote -v    # issue a git command in the repo
    origin    https://github.com/<username>/myrepo.git (fetch)
    origin    https://github.com/<username>/myrepo.git (push)

- Git commands that don't refer to specific files or directories will have the same result if issued in the top directory of your repo, or in any of its sub- or sub-sub directories.  These include `git branch -vv`, `git fetch`, `git pull` (all covered below) and many others.

- `git add .` (covered below) will stage all files **in the current directory and its subdirectories**.

- `git status` (covered below) will show the status of all files in the repo, no matter where it is issued. Conversely `git status .` will show the status of files in the current directory and its subdirectories. 

**A single repo on GH can be cloned to more than one laptop/PC**, for example if you have collaborators (who will each want to clone the repo to their laptop/PC) or if you want to be able to work on the repo from more than one laptop/PC (one at work and one at home).

### Seeing branches and their commit history<a id="seebranches"></a>

To **see your branches** use `git branch -vv` . The output has one line for each local branch:

```
$ git branch -vv
* main 661723b [origin/main] more small changes
```

This shows there is a **single local branch** `main`, set to **track** the `main` branch on the remote GH repo which is denoted by `origin/main`. This is the default setup for a clone of a newly-minted repo, before any other branches have been created. This also shows a `*` on the currently checked-out branch, and the abbreviated SHA and commit message for the commit `main` currently points to.

To see **commit history** use `git log`, which has many options -- [here is a nice intro](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History).  This variation shows the **history of the current branch** (newest to oldest) with a little graph on the left side showing branching and merging.  The first line is the currently checked-out commit, agreeing with the `git branch -vv` output above.

```
$ git log --pretty=format:"%h %s" --graph
* 661723b more small changes
* b4289d1 filled out more
* f623657 filled out more
* fedab23 filled out more
* 1b096e4 added stuff
* 5d5c774 more little changes
* 2572430 little changes
* 05d0e2f removed additions scratch file
* 555be14 try link
*   ea082ba Merge branch 'main' of github.com:doncandela/gs-git
|\  
| * 7fb48e9 material to add
* | 18a9885 changes to gs-git.md\
|/  
* fd13658 extensive additions to CLI part
* 6c1adff small changes
* c7a2401 more typos
...
```

By default the output is piped through `less`, so hit space to see the next page or `q` to quit.  It's handy to define an **alias** for a command with lots of options like this:

```
$ git config --global alias.glog 'log --pretty=format:"%h %s" --graph'
$ git glog
* 661723b more small changes
* b4289d1 filled out more
* f623657 filled out more
* fedab23 filled out more
.....
```

### Some typical Git workflows, from simple to more complex<a id="workflows"></a>

The next few sections illustrate a few possible Git workflows, but there are many other possibilities and only the simplest Git commands are shown here. To make things interesting it is assumed in some places below that you have **cloned your GH repo to more than one local repo** -- for example to both a work PC and and home PC so you can work in both places.   You don't need two laptops/PCs to try these things -- just clone twice to two different locations on the same laptop/PC.

### 1. A simple one-person, one-branch Git workflow<a id="oneperson"></a>

This is the workflow you might use for a repo you own with no other collaborators, when you are not using side branches to try things out. The operations covered here -- **pulling**, **staging**, **committing**, **tagging**, and **pushing** -- are also used in the more complex workflows below, so please read this section.

- **Pull down the current state of your remote repo.** As you have no collaborators on this repo this is only needed if *you* have pushed work to GH from another clone of your repo (e.g. you are working on your home PC, and earlier you pushed work from your work PC):
  
      $ git pull
  
  This updates the your local branch `main` so it includes all the commits in the remote GH branch `origin/main` it is tracking.

- **Update your repo files.** This is where you move your project forward, by editing or deleting existing files in your repo and/or adding new ones.  You can use any tools you want to do this (an IDE to develop code, a Markdown editor to update docs...), but don't disturb the (typically hidden) `.git` subdirectory.
  
  - In addition to editing your files, you might run code they contain to test the changes, or render Markdown files into PDF's or HTML, etc. It's fine for the output generated in this way to be in the repo directory on your laptop/PC, but the `.gitignore` file in this directory should usually include lines to prevent generated output from being included in your commits. **If you start generating new kids of output, now (before committing) would be a good time to add corresponding lines to `.gitignore`**, which is just a another text file in your repo that you can edit (or create if you don't have one yet, or [download from GH](https://github.com/github/gitignore)).
  
  - Work on your files can be done directly in your repo, or done elsewhere and copied into the repo.  In either case you shouldn't modify the files in your repo before pulling as above.
    
    - What happens if you modify the files in your local repo *before* pulling from the remote? Rather than overwriting your modifications Git will refuse to do the pull, insisting you either [stash](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning) your local work or commit it, which will then require a merge as described in the next section.

- The **status of changes to your repo** since the last commit is seen by running
  
        $ git status
  
  which lists the changes in three categories:
  
  - **Changes to be committed (green).** -- these are changes (modified files, new files, deleted files) that will be included the next time your run `git commit`, which are called **staged**.  Initially none of your changes are staged.
  
  - **Changes not staged for commit (red).** -- these are changes to **tracked files** (files previously staged or committed) not yet staged.
  
  - **Untracked files (also red).**  -- these are new files not previously staged or committed.

- **Stage files for commit.**  After the first time you stage, this step can be skipped altogether if you use `git commit -a` as shown below.  Otherwise, you run one or more of the following commands until `git status` shows all the changes you want to commit in green:
  
      $ git add .    # stage all files both tracked and untracked
      $ git add <filename>               # stage one file
      $ git add -u                       # stage all tracked files
      $ git restore --staged <filename>  # unstage one file
      $ git restore --staged .           # unstage all files
  
  **Note:** If you edit files after staging them, you will need to run `git add` again to capture the new edits.
  
  To **delete** a file, stage it for removal from repo, and stop tracking it do
  
      $ git rm <file>
  
  Simply doing `rm <file>` will delete the file but will not stage this change to be committed.  (If commits were done that included this file, then even after using the `git rm` command and committing these earlier versions of the file can still be retrieved by checking out those earlier commits.)

- **Commit changes to the local repo.**
  
  Commit **staged** files (if a **commit message** is not supplied using `-m` an editor will open to make one):
  
      $ git commit -m 'my commit message'
  
  Alternatively use `git commit -a` to  **stage and commit all tracked files** (note `git add` must be done at least once to start tracking a file):
  
      $ git commit -a -m 'my commit message'
      $ git commit -am'my commit message'  # can write this way to save typing
  
  If necessary you can **amend** the commit just done.  But you shouldn't do these after the commit has been pushed to GH.
  
  - Change only the commit message (if not given as shown will open editor)
    
        $ git commit --amend -m 'ammended commit message'
  
  - Add a file forgot to stage
    
        $ git add <file>
        $ git commit --amend

- **Push changes to GH.**
  
      $ git push
  
  This updates the remote branch `origin/main` to agree with the local branch `main`.  This is an abbreviation for `git push origin main` which means push the `main` branch to `origin`.

- **Optionally tag this version and push the tag.**  If the version of your repo you just pushed represents a useful milestone -- for example a well-tested or proof-read version that you may want to distribute to people outside of your project, or easily return to yourself -- you can assign a [tag](https://git-scm.com/book/en/v2/Git-Basics-Tagging) such as a **version number** and push the tag to the remote GH repo:
  
      $ git tag v1.1 -m 'Version 1.1 with major new capabilities'
      $ git push origin v1.1
  
  (With a message supplied as shown this creates an "annotated tag".)  Now, in GH, this tag "v1.1" will show up both under the branch/tag selector at the upper left, and in the **Releases** panel on the right side where a `.zip` or `.tar.gz` archive of the repo files (as they were at at the tagged commit) can readily be downloaded.

This **pull** -- **edit** -- **stage and commit** -- **push** flow will work with no additional input from you provided the following rules are obeyed:

- Push all work to GH from one clone (e.g. work PC) before pulling from GH to another clone (e.g. your home PC).

- If work may have been pushed to GH from another clone, always pull from GH before doing local work.

It's fine to violate these rules, but then you will arrive at the more interesting situations covered in the next section.

### 2. All about merging: Fast forwarding, divergent history, conflicts<a id="merging"></a>

When your repo has only one branch (`main` ),  `git pull` is equivalent to these two commands done one after the other:

```
$ git fetch origin         # fetch all branches from origin (GH)
$ git merge origin/main    # merge origin/main into current branch main
```

The `git fetch` command brings the tracking branch `origin/main` up to date with the `main` branch on the remote GH repo. Then the the `git merge` attempts to merge  `origin/main` into the local branch `main`. In the following sections the `git fetch` and `git merge` are done separately so the situation after the fetch can be examined before attempting the merge.

**A. Simple pull-edit-push workflow, following the rules.**

- **Local branch behind remote branch.** Here changes to the repo  have been committed and pushed to GH from **a different clone** (our work PC) than the one we are working on now (our home PC).  Do the first part of the pull (a fetch) and then look at the local branch:
  
  ```
  (edits committed and pushed to GH from the other, work-PC clone)
  $ git fetch origin
  $ git branch -vv
  * main 409f4a0 [origin/main: behind 1] Edits done in home clone.
  ```
  
  The output above says our local branch `main` is **behind** the remote branch `origin/main` by 1 commit as we have not yet merged the the edits fetched from GH.  The SHA `409f4a0` and commit message are still from the last commit done locally. Now complete the pull by doing the merge:
  
  ```
  $ git merge origin/main
  ..
  Fast-forward
  ..
  $ git branch -vv
  * main 67a6991 [origin/main] Edits done in work clone.
  ```
  
  This was the simplest kind of merge, a **fast forward** that `git pull` could have done without our help: `main` is simply updated to agree with `origin/main` so it "catches up" with the additional commits in `origin/main` that weren't in `main`.  No new commit is created by a fast-forward merge.

- **Local branch ahead of remote branch.**  Continuing the previous example, we do some work locally and commit it. This puts the local branch **ahead** of the remote branch:
  
  ```
  (do some edits in this home-PC clone)
  $ git commit -am 'More edits in home clone'
  $ git branch -vv
  * main 9dbaae7 [origin/main: ahead 1] More edits in home clone
  ```
  
  In this situation we will be able to **push** to the remote repo, as the merge of our pushed changes into the remote `main` branch can also be a **fast forward**,  which is what `git push` normally requires.

**B. Violating the rules with a commit before pulling.** Now we violate the rules of the previous section by

- In the other (work) clone editing the file `spam.txt` , committing and pushing to GH.

- In this (home) clone, before pulling from GH, editing a different file `eggs.txt` and committing.

- Fetching from GH (the first part of a pull).

```
(on the other (work) PC: edit spam.txt, commit and push to GH)
(on this home PC: edit eggs.txt)
$ git commit -am 'Edited eggs.txt on home PC'
$ git fetch origin
$ git branch -vv
* main e4c4eb2 [origin/main: ahead 1, behind 1] Edited eggs.txt on home PC
```

Now the local branch `main` is **both ahead and behind** the remote branch `origin/main`.  This is not a paradox - it simply means `main` has a commit that is not in `origin/main`, so `main` is ahead, but also `origin/main` has a commit that is not in `main`, so `main` is behind.  This is called **divergent history**, and it **cannot be resolved by a fast forward** -- the histories of `main` and `origin/main` are simply different, and a compromise will be needed to merge them.  Do the merge:

```
$ git merge orgin/main
(editor opens so we can modify merge commit message)
Merge made by the 'recursive' strategy.
$ git branch -vv
* main 59b49e0 [origin/main: ahead 2] Merge remote-tracking branch 'origin/main' into main
```

 A new **merge commit** has been added to `main`.  If we look at the files we will see that `spam.txt` has the changes fetched from GH **and** eggs.txt has the changes made locally. This is what the 'recursive' merge strategy did -- it kept the updated versions of both files (the various merge strategies are detailed [here](https://git-scm.com/docs/merge-strategies)).

This was not quite as simple as a fast-forward, but all that was required from us was to either accept or edit the merge commit message.

**C. Conflicting divergent history.** Repeat the previous example creating divergent history, but this time **make different edits the same lines of the same file** `spam.txt` in the work and home clones.

```
(edit spam.txt, commit and push to GH from the other, work-PC clone)
(edit spam.txt differently in this home-PC clone)
$ git commit -am 'Edited spam.txt on home PC'
$ git fetch origin
$ git branch -vv
* main d72168a [origin/main: ahead 1, behind 1] Edited spam.txt on home PC
$ git merge orgin/main
Auto-merging spam.txt
CONFLICT (content): Merge conflict in spam.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Now Git cannot complete the merge as it has no way to know how we want to resolve the conflicting edits made to `spam.txt`. To help us it has added **conflict markers and** the **conflicting material from both sources** to `spam.txt`, which now looks like this:

```
this is spam.txt

<<<<<<< HEAD
I made this edit on the home PC.
=======
Here is an edit made on the work PC!
>>>>>>> origin/main
```

It's up to us to edit this file to resolve the conflict however we like, and also remove the conflict markers `<<<<<<< HEAD`, `=======`, and `>>>>>>> origin/main`. Then we stage and commit to complete the merge.  For example we could edit `spam.txt` to contain

```
this is spam.txt

I made this edit on the home PC, but I like
this is edit made on the work PC better.
```

Then, after staging and committing `spam.txt` (or simply doing `git commit -a`) we will see a new merge commit has been added to `main` just as for the non-conflicting merge above. 

Some Git jargon: In Git `HEAD` usually points to the branch you currently have checked out. When a merge is done, a new merge commit is added to this checked-out branch (except for a fast-forward, which just changes to checked-out branch to match the other branch).  In either case, the other branch is not changed by the merge. So, in the example above, `HEAD` refers to `main` (as that was checked out), while the other branch is `origin/main` (in this case a remote tracking branch, which is not a branch you can change yourself).

**D. Trying to push an outdated branch.** As mentioned above, for a `git push` to succeed the local branch being pushed (here `main`) must be purely **ahead of** the remote branch being pushed to (here `main` in the remote GH repo) so the remote branch can simply be **fast forwarded** with the commits being pushed.  This won't be true if the pulled copy of the remote branch that we add commits to becomes **outdated**, because commits have been pushed to it from another clone after our pull. To see this in action (calling our local clone the home PC as above):

- Pull from GH.

- Make some edits and commit them locally, without pushing yet.

- Meanwhile (the order of this and the previous step doesn't matter), in the other (work PC) clone make some edits, commit them and push to GH.

- Try to push our home PC edits -- but this will be rejected, with a rather extensive help message:

```
$ git pull
(on this home PC make some edits on)
$ git commit -am'more home edits'
(on another (work) PC pull, make edits, commit, push to GH)
$ git push
To github.com:doncandela/test-gs-git.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'git@github.com:doncandela/test-gs-git.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

To fix this situation, we should

- Fetch to update our tracking branch `origin/main` to agree with the current state of `main` on GH.

- Merge `origin/main` into our local branch `main`, to update `main`. This could require resolving merge conflicts (another possibiity is to **rebase**, discussed below).

- It should now be possible to push `main` to GH.

```
$ git fetch
$ git merge origin/main  # could combine these two steps as 'git pull'
(resolve conflicts if any -- will at least ask to confirm merge msg)
$ git push
(no error this time)
```

### 3. A collaborative workflow: Feature branches and pull requests<a id="feature"></a>

Now let's assume that you have added some **collaborators** on GH to your repo -- unless you set things up differently they will have the same ability as you to pull from and push to the repo (for alternatives see "Small groups with differing roles" below).  But you don't want to work at cross purposes with your collaborators, e.g. making incompatible changes to the files.  Also, you and your collaborators might want to try out and debug some new features or other changes, without affecting the `main` branch until the new work is ready for prime time.  Here is a workflow that accomplishes these things (an example showing these steps in action follows):

- In their own local clone of the repo, you or a collaborator creates a new branch off of `main` .  This branch is specifically for working on some part of the code and is called a **feature branch**.  It should have a distinct name e.g. `feat23`.

- The collaborator **works locally on the files** in `feat23` until they reach some typical stage of completion -- usually not perfection but this depends on how the collaboration prefers to work.

- At some stage the collaborator **pushes** the `feat23` branch to GH, which will create `feat23` in the GH repo.  This push should always be allowed -- because it is a separate branch, it won't conflict with changes to `main` or other feature branches with different names that have been pushed to GH.

- At some stage (typically when the branch is pushed to GH, but could be later) the collaborator **opens a pull request**, which is a request (not part of Git) to the group (or specific collaborators) to review `feat23` for a possible eventual merge into `main`.

- The  modified files in`feat23` can be **improved via collaborative discussion and editing** on GH to the point where they are ready to be merged into `main` (or not). During this process `feat23` can be pushed to GH as often as needed to include improvements.

- If `feat23` becomes **outdated** due to changes to `main`  so it cannot be merged into `main` without conficts, `feat23` can be **updated** using the current `main`.

- If and when it is agreed to include the changes in `feat23`, a collaborator **merges the pull request** which accomplished the merge into `main` and finishes the discussion. Conversely the pull request can be **closed** without merging.

In this collaborative workflow (unlike the simpler workflows 1 and 2 discussed above) **you and your collaborators never push the `main` branch to GH** -- rather you only push feature branches, and then use PRs to merge them into `main` on GH.  It is possible to set up your GH repo so pushes to `main` are not allowed.

**Collaborative flow in action.** To you invite other GH users to be collaborators to a  GH repo you own using the repo's **settings** tab, **collaborators** item. Once they accept they will have full write (push) access to all branches of the repo (see "Workflows for a small group with differing roles" below for some alternatives).  Thus the local Git commands and remote GH actions shown here can be issued by any of the collaborators.

- **Each collaborator clones the repo to their laptop/PC** using the same command you used, for example to put the repo under `~/Documents/Github` they do
  
  ```
  ~/Documents/Github$ git clone <address>
  ```
  
  where `<address>` is the HTTPS or SSH address of your repo copied and pasted from from the **<> Code** tab of the repo on, which they can now see.  Each collaborator must set up authentication to GH from the CLI as described above (their username and password will not suffice).

- A collaborator who wants to develop some additions or changes to  the repo **creates an checks out a local branch** here called `feat23`.  This is a branch like any other but it's called a "feature branch" because the collaborator has created it as a separate place to develop their proposed changes to the repo:
  
  ```
  $ git checkout -b feat23
  ```
  
  This command is Git shorthand for the two successive commands 
  
  ```
  $ git branch feat23    # make a new branch called feat 23
  $ git checkout feat23  # check out the new branch
  ```
  
  Recall that checking out a branch means that (a) the **repo files will be changed** to match that branch, which does nothing here as the branch was just created from the previously checked out branch `main`, and (b) future commits will be added to this branch, in this case to `feat23` rather than `main`.  Further `git checkout`'s can be done as desired (without the `-b` option) to work on different branches:
  
  ```
  $ git checkout main
  (do some work on main)
  $ git checkout feat23
  (go back to working on feat23)
  ```

- (With `feat23` checked out) the collaborator **develops their changes to the file, stages them, and commits them** to the`feat23` branch, which at this point exists only in the collaborator's local clone of the repo.

- The collaborator **pushes their feature branch to GH**:
  
  ```
  $ git push origin feat23
  ```
  
  This is called **publishing the feature branch** (on GHDT, for example) as it makes it visible and available for pulling, pushing, and merging to you and the rest of the collaborators.  As shown here the branch to be pushed must be specifically given (or use `git push --all origin ` to push all local branches).  The intent is that a collaborator's feature branch will not be published to GH until they intentionally do so.

- Whoever developed and pushed the feature branch will want to **tell other collaborators about it** by **opening a pull request (PR)**, which is a suggestion to the group that `feat23` should be merged into `main` (or another specified branch).  The following steps are available in the GH website for the collaborator who pushed the `feat23` branch:
  
  - They will probably see a box with "`feat23` had recent pushes..." with a button **Compare & pull request** they can hit.
  
  - Alternatively under **Pull requests** at the top they can hit **New pull request**.
  
  - In either case they should make sure the branches are set for **base: `main`** and **compare: `feat23`** (they will need to set this manually if they use "New pull request"). The pull request is to **update the "base" branch** by **merging in the differences in the "compare" branch**.
  
  - THey should add (or accept) the title and **add a description**. Notice the description box is large and can use Markdown formatting, hyperlinks, attachments... The description is used to **start a conversation about the proposed changes** and can include **@username**'s to notify other GitHub users.  There are also various tabs on the right side that let the collaborator assign reviewers, attach labels like "bug" and "help wanted", etc.
  
  - With the description and other info complete, they should hit **Create pull request**.
  
  - If the collaborator wants to push your branch `feat23` to GH when it is very incomplete e.g. just to back it up, they could push it but hold off opening a PR until it is closer to finished. But all collaborators will see and be able to edit the branch as soon as it is pushed. It's probably better to open the PR right away as **draft PR**, which is an option under the "Create pull request" tab, and/or to start the description with WIP (work in progress) or something similar. (For free GH accounts, draft PRs are only available for public repos.)  Draft PRs cannot be merged until they are marked "Ready for review", and then a collaborator assigned as a reviewer will need to submit an approving review (this is done under the "Files changed" tab of the PR). 

- **Working on a PR before it is merged.**
  
  - **Other collaborators work on the feature branch**
  
  - **Updating a feature branch so it can be merged automatically.**  If the pushed branch `feat23` can be merged without conflicts, a green **This branch has no conflicts with the base branch** and a green **Merge pull request** button will be displayed when the GH screen for the PR.  Often it is the responsibility of whoever initiated the pull request to **update and re-push their feature branch** so it can merged without conflicts.

### Aside: Rebasing and squashing<a id="rebasing"></a>

In general Git keeps your entire commit history, branches and all (unless you delete an unmerged branch).  Here, however, are two techniques that **modify the commit history** and cause **existing commits to be permanently discarded** -- but not the information of value in those commits, if done correctly. 

One basic rule seems to be to **never rebase or squash published branches** (branches that have been pushed to GH so your collaborators can work on them) -- you might eliminate commits they are using, causing [a bit of a tangle](https://git-scm.com/book/en/v2/Git-Branching-Rebasing).  If you do want to push branch that was previous pushed and now has been rebased or squashed you will need to use `git push --force` which is Git's way of saying you are doing something dangerous.

If you are working in a collaboration, the use of of rebasing and squashing will likely be dictated by the practices of the collaboration -- you may have noticed that closing pull requests with a rebase or a squash rather than a merge are options offered in GH.

- [**Rebasing**](https://git-scm.com/book/en/v2/Git-Branching-Rebasing) replaces a divergent branched history with a simpler, linear history.  So, for example, if the feature branch `feat` you are working on does not merge in cleanly with `main`, one option as explained above is to update `feat` by merging an up-to-date fetch of `main` into the `feat`.  Your updated `feat` can then be merged cleanly into `main`, and after that is done `main` will have a complicated history showing the earlier merge of `main` into `feat`. 
  
  Here's an example: With `main` checked out, we make and commit a file `foo.txt` :
  
  ```
  $ git checkout main
  $ nano foo.txt        # create and edit foo.txt
  $ cat foo.txt
  ******** THIS IS foo.txt ********
  ---- don't edit this line ----
  ********* END OF foo.txt ********
  $ git add foo.txt
  $ git commit -am'Created foo.txt'
  ```
  
  We make and checkout a new branch `feat`, and on this branch do two rounds of editing can committing changes to `foo.txt`:
  
  ```
  $ git checkout -b feat
  $ nano foo.txt   # do some edits
  $ git commit -am'First edits in feat to foo.txt'
  $ nano foo.txt   # do some more edits
  $ git commit -am'Second edits in feat to foo.txt'
  $ cat foo.txt
  ******** THIS IS foo.txt ********
  First edits in feat!
  Second edits in feat!
  ---- don't edit this line ----
  ********* END OF foo.txt ********
  ```
  
  Then, to make the commit of `main` that `feat` was based on outdated, we checkout main, edit a different line of `foo.txt`, and commit:
  
  ```
  $ git checkout main
  $ nano foo.txt   # edit a different line
  $ git commit -am'An edit in main to foo.txt'
  $ cat foo.txt
  ******** THIS IS foo.txt ********
  ---- don't edit this line ----
  This edit was done in main.
  ********* END OF foo.txt *******
  ```
  
  The (divergent) commit history for the two branches looks like this (above `git glog` was set to be an alias for `git log` with options to print like this):
  
  ```
  $ git glog main feat
  * 0f90a7a An edit in main to foo.txt
  | * fbcd29c Second edits in feat to foo.txt
  | * 27ea77b First edits in feat to foo.txt
  |/  
  * 246e8d6 Created foo.txt
  ```
  
  If `feat` were merged into `main` at this point, the two different edits of `foo.txt` would need to be resolved (which would be figured out without our help in this case as we edited different parts of `foo.txt` in the two branches). Instead we merge `main` into `feat`.  In other words, we (the authors of `feat`) do some work to resolve any conflicts with main, before we submit `feat` to be merged into `main`:
  
  ```
  $ git checkout feat
  $ git merge main    # merge main into feat
  Auto-merging foo.txt
  Merge made by the 'recursive' strategy.
  ```
  
  Now `feat` has been "cleaned up" so it can be merged into main with a simple fast forward:
  
  ```
  $ git checkout main
  $ git merge feat      # merge feat into main
  Fast-forward
  (we would typically delete feat here, but we are re-using it below)
  ```
  
  After the merge `foo.txt` in `main` has both sets of edits, as intended, and the history of `main` shows the complicated way in which this happened in full detail -- all of the commits that were generated on the two branches are still present here (and would be even if we deleted `feat`):
  
  ```
  (we still have main checked out)
  $ cat foo.txt
  ******** THIS IS foo.txt ********
  First edits in feat!
  Second edits in feat!
  ---- don't edit this line ----
  This edit was done in main.
  ********* END OF foo.txt ********
  $ git glog
  *   ef234ee Merge branch 'main' into feat
  |\  
  | * 0f90a7a An edit in main to foo.txt
  * | fbcd29c Second edits in feat to foo.txt
  * | 27ea77b First edits in feat to foo.txt
  |/  
  * 246e8d6 Created foo.txt
  ```
  
  Because the final merge of `feat` into `main` was a fast forward, it did not generate a new commit - thus the last commit we see here is the earlier merge of `main` into `feat`.

A different way to do this is to **rebase** `feat` on the up-to-date `main`, which basically means to re-write the history of `feat` as if this branch started with the up-to-date `main`.  Then the rebased `feat` can be merged into `main` with a simple fast-forward, leaving no additional branching in `main`'s history.

 To see this in action we will first use the **dangerous command** `git reset --hard` to "back up" the branches to the way they were after we did the divergent edits and commits, but before the merges.  This is dangerous because it [can't be always undone](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified), possibly losing work, and it is not part of the re-basing -- we are just doing these resets so we can start out in the same place and do things a different way.  You wouldn't do this in a practical situation. 

```
$ git checkout feat
$ git reset --hard fbcd29c  # not part of rebase, just to go back in time
HEAD is now at fbcd29c Second edits in feat to foo.txt
$ git checkout main
$ git reset --hard 0f90a7a  # not part of rebase, just to go back in time
HEAD is now at 0f90a7a An edit in main to foo.txt
$ git glog main feat
* 0f90a7a An edit in main to foo.txt
| * fbcd29c Second edits in feat to foo.txt
| * 27ea77b First edits in feat to foo.txt
|/  
* 246e8d6 Created foo.txt
```

Now, instead of merging `main` into `feat`, we **rebase** `feat` on main.  This requires resolving differences between the two branches just as the merge did (again, done automatically in our example),

```
$ git checkout feat
$ git rebase main
First, rewinding head to replay your work on top of it...
Applying: First edits in feat to foo.txt
...
$ git glog main feat
* 8b87cf6 Second edits in feat to foo.txt
* 651de65 First edits in feat to foo.txt
* 0f90a7a An edit in main to foo.txt
* 246e8d6 Created foo.txt
```

 Note the the **history of `feat` has been changed** -- (a) the commit `0f90a7a` is now in `feat`'s history (it wasn't before the rebase), (b) the commits `27ea77b`, `fbcd29c` have been thrown away, and **replaced by new commits** with different SHAs but the same commit messages!

Now `feat` can be merged into `main` with a fast forward.  The final state of `foo.txt` is the same as above, containing the edits from both branches, but the final history of `main` has been simplified due to the rebase - it is **linear**, i.e. has only one branch.

```
$ git checkout main
$ git merge feat          # merge feat into main
Fast-forward
$ git branch -d feat      # delete feat, no longer needed
$ cat foo.txt
******** THIS IS foo.txt ********
First edits in feat!
Second edits in feat!
---- don't edit this line ----
This edit was done in main.
********* END OF foo.txt ********
$ git glog
* 8b87cf6 Second edits in feat to foo.txt
* 651de65 First edits in feat to foo.txt
* 0f90a7a An edit in main to foo.txt
* 246e8d6 Created foo.txt
```

- **Squashing** means combining two or more commits in a linear history into a single commit.  So, for example, if there are several successive commits each containing a small change to a file, they can be replaced with a single commit that includes all of the changes.  There various ways to squash commits: (a) GH can be set to squash when a pull request is closed; (b) the `--squash` option can be supplied to `git merge`, (c) the [interactive rebasing command](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) `git rebase -i` can be used; (d) The command `git reset --soft` can be used. Only the last method is shown here, and you should probably [understand how `git reset` works](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified) before trying this.
  
  To show this in action we continue the example above.  As shown just above `main` now has a linear history with three commits getting us from the "Created foo.txt" to the final state with all three edits done.
  
  With `main` checked out we can squash these three commits into a single commit by (a) using `git reset --soft` to set `HEAD` to point to the last commit we want to keep, then (b) using `git commit -a` to stage and commit the final state of `foo.txt` from our working directory (which `git reset --soft` doesn't touch - that's why it's less dangerous than `git reset --hard`).  Now  `foo.txt` still has all three edits, but its history is quite different from above:
  
  ```
  (main is still checked out - it's the only branch as we deleted feat)
  $ git reset --soft 246e8d6  # this is part of the squash, pick the
                              # oldest commit we want to keep -- newer
                              # ones will all be squashed into 1 commit
  $ git commit -am'All three edits done to foo.txt'
  $ cat foo.txt
  ******** THIS IS foo.txt ********
  First edits in feat!
  Second edits in feat!
  ---- don't edit this line ----
  This edit was done in main.
  ********* END OF foo.txt ********
  $ git glog
  * a1437f5 All three edits done to foo.txt
  * 246e8d6 Created foo.txt
  ```
  
  Now the three commits `0f90a7a`, `651de65`, `8b87cf6` are **no longer in the history**, they've been replaced by a single new commit `a1437f5`  and they will eventually be garbage-collected. 

### 4. Open-source workflow (briefly)<a id="opensourceflow"></a>

See  the section "Contributing to open-source projects" above for resources and info on how to get started if you are a first-timer. [This section](https://git-scm.com/book/en/v2/GitHub-Contributing-to-a-Project) of the Git Book has a nice example of this flow.  After picking a project to contribute and issue to work on, the workflow in brief is:

- On GH fork the project, which creates a new repo on GH that is a copy of the original repo, but belonging to you (and by default, has the same name as the original).
  
  ```
  somwhere$ git clone <HTTPS or SSH address of your fork>
  somewhere$ cd <reponame>
  ```

- Make a branch for the work you will do
  
  ```
  $ git checkout -b mybranch
  ```

- Make your changes to the repo, in `mybranch`, until you are ready to make a pull request to the maintainers of the the original repo. Whether your changes should be perfected at this point or just a half-baked idea (or somewhere in between) depends on the policies of the project you are contributing to.

- Push your branch to your forked repo:
  
  ```
  $ git push origin mybranch
  ```

- Open a pull request on GH.  By default your pull request will be to merge your branch into `main` of the **original repo**, not your fork, and ideally this should start a conversation or a review as in the [example here](https://git-scm.com/book/en/v2/GitHub-Contributing-to-a-Project), leading eventually to your changes being accepted and merged, or rejected (PR closed without merging).

- If the original project progresses while your PR is open, you may need to update your branch by merging in the `main` branch of the **original repo**, which will abbreviated as `upstream` (rather than `origin`, which refers to your fork).  Unless you cloned your fork using GHDT (which does this automatically) you should to set `upstream` to point to the original repo (see "Keeping up with Upstream" in [this section](https://git-scm.com/book/en/v2/GitHub-Contributing-to-a-Project) of the Git Book):
  
  ```
  $ git remote add upstream <HTTP or SSH address of original repo>
  $ git remote -v
  origin <HTTP or SSH address of fork> (fetch)
  origin <HTTP or SSH address of fork>t (push)
  upstream <HTTP or SSH address of original repo> (fetch)
  upstream <HTTP or SSH address of original repo> (push)
  $ git fetch upstream       # get current state up upstream branches
  $ git checkout mybranch
  $ git merge upstream/main  # merge current upsteam/main into mybranch
  $ git push origin mybranch
  ```
  
  This new push of `mybranch` to **your repo** will automatically update the PR, hence be visible to the maintainers of the original repo. 

### Small groups with differing roles<a id="differing"></a>

In the flow outlined in "3. A Collaborative workflow..." above all collaborators have the same ability to update the files in the `main` branch (and even change its history), which will be suitable for some collaborations. In other cases the repo contains code (or other material) that specifically belongs to or is maintained by one or a couple of collaborators, with the remaining group members expected to use the code and possibly submit suggested improvements, but not to directly update the repo.

It seems that a good setup for this situation is to follow the **open-source** workflow updated above.  For a **public repo**, no special setup is necessary: Group members who should not directly edit the repo should not be made collaborators in the repo.  Rather, they can **fork the repo and submit pull requests for proposed changes** -- this is precisely the open-source workflow outlined above.

For a **private repo**, the following setup seems to work, even with free GH accounts:

- An **organization** should be created to own the repo (user settings, my organizations, new organization).  The organization is needed to give other group members the ability to read but not write a private repo -- for a  private repo owned by a user (rather than an organization) other users are either collaborators (with full write access) or they cannot see the repo at all.

- The **organization** should create the desired **private repo**.  Thus the address of the repo will be `github.com/<orgname>/<reponame>` rather than `github.com/<username>/<reponame>` as for a repo owned by a user.  In other words, the user who created the organization owns the organization, and the organization owns the repo.

- The organization will already have its creator as the **Owner**.  It should add other GH users who should not directly edit the repo as **Members**, who by default have read but not write access to a private repo owned by the organization but not by them (and they will have the ability to create new repos owned by the organization, which they *can* edit).  There is  a different role called "outside collaborator", I haven't explored that.

- The organization can also add additional **owners**, which GH recommends.  All owners have full control over the organization and its repos, I think.

- The **repo must be set to allow forking**, which is turned off by default for private repos.

- Now the other Members will be allowed to fork the repo and submit PRs proposing their changes.

In either case outlined above (public or private repo), after forking the repo the other group members can either:

- Submit PRs for their proposed changes if it seems thay should be incorporated into the original repo, or

- Simply edit the forked repo to accomplish what they need to do, without PRs on the original repo.

## Appendix A: Markdown<a id="markdown"></a>

[**Markdown**](https://www.markdownguide.org/) is a simple "markup language" that lets you add codes to a text file so it will be displayed in a browser with headings, bullet lists, bold face and italics, etc.
For example surrounding text with single or double asterisks ``*like this*`` or ``**like this**`` will make it come out in italic or boldface *like this* or **like this** in a browser.
Here is a [Cheat Sheet](https://www.markdownguide.org/cheat-sheet/), and here is a very handy [Syntax Guide](https://www.markdownguide.org/basic-syntax/) that includes best practices to make your Markdown files maximally compatible (e.g where and where not to put spaces and blank lines).

- Markdown files are just text files, but they are usually given the extension ``.md`` so they will be displayed as intended in browsers.
- The text cells in a Jupyter Notebook (including Google Colab notebooks) are written in Markdown. The default "readme" file in a GitHub repository is ``README.md``, a Markdown file. This file is a Markdown file.
- Markdown is designed to be easily readable by anyone in a text editor, unlike more elaborate markup languages like HTML and Latex.
- Markdown can be written using a text editor like Windows Notepad or macOS TextEdit. Alternatively there are many specialized editors can be used which show HTML-rendered text live in one pane while you edit the plain-text source in another pane, or directly edit the rendered text like a conventional word processor -- I am currently using [MarkText](https://www.marktext.cc/) which works like this.
- Some markup languages other than Markdown you may encounter:
- [**reStructuredText**](https://docutils.sourceforge.io/rst.html) which is often used to produce code documentation (for example, the official Python docs).
  Somewhat similar to (but not compatible with) Markdown.
- [**MyST**](https://mystmd.org/guide/), an extension of Markdown intended to have the functionality of reStructuredText while retaining more compatibility with Markdown.
- [**LaTex**](https://www.latex-project.org/), a document (or page) oriented markup language used particularly to format mathematical material such as complicated equations.
  Rather than using the full LatTex markup language, Markdown renderers are usually set up so Latex commands can be embedded in Markdown files to show mathematical material. Thus  `$\alpha = e^\pi$` in a Markdown file can be displayed in a browser as $\alpha = e^\pi$.

## Appendix B: Using GH to develop and distribute a simple Python package<a id="pythonpackage"></a>

Say you have written some Python modules `mymod1.py`, `mymod2.py` that you would like to make into an **installable package** called `mypackage`, so they can be imported by a Python program sitting anywhere on your laptop/PC -- rather than needing to be in the same directory where you import them.

Here is one simple way you can set up a GH repo for developing, distributing, and maintaining your package:

```
myrepo/
    README.md
    LICENSE
    .gitignore
    src/
        mypackage/
            mymod1.py
            mymod2.py
    doc/
        (put documentation here)
    tests/
        (put test code here)
    setup.py
```

where `setup.py` contains

```
from setuptools import setup
setup(name='mypackagename',
      version='0.0'             # or higher version as desired
      package_dir = {'':'src'}, # where to find the packages
      packages=['mypackage'])   # can list more than one package
```

**To develop and maintain your package** you would clone it somewhere, in which case you would get the whole repo history in a `.git` subdirectory. Conversely to simply **use your package** someone could use the **<> Code** tab to download a ZIP of the latest (or another tagged) version, which would not include the repo history. Or they could **fork** your repo to develop it further themselves.

After cloning or extracting from a ZIP file, this package can be **installed** (into the current Conda environment, if any) by going to `myrepo` and doing

```
(myenv)../myrepo$ pip install -e .
```

Now (running from any directory, but in this environment) your Python code can do things like

```
from mypackage import mymod1
mymod1.myfunc()
```

Because `-e` (editable) was used with `pip`, the package files can be freely edited in place without re-installing them.

For the simple setup discussed here `doc/` would contain an "instruction manual" listing other packages that must be installed for your package to work, while `tests/` would contain some Python programs that could be run to check that it is successfully installed. More sophisticated developers (than me) will know how to set things up so package dependencies are automatically installed and tests are automatically run.

**Naming things.** In the example shown above, there are three different names for your work: `myrepo`, `mypackage`, and `mypackagename`. In practice these are often all the same; they are different in the example to explain how they get used:

- `mypackage` in the example is **the name of a directory containing Python modules (`.py` files)** -- this is what Python considers to be a "package" and this is **what you use in Python `import` statements**.

- `mypackagename` in the example is **the value of the argument `name` to `setuptools.setup`**. This is **the name under which Pip will install your package**.  It is logical (and simplest) to make this the same as the import name of your package -- in other words to write `setup(name='mypackage...)` unlike the example above.  If you publish your package on [PyPi](https://pypi.org/), this Pip name will need to be different from the names of all other published packages.

- `myrepo` in the example is the **name of your GH repo**.  As with the Pip name, you may want to make your repo name the same as the import name `mypackage`, so you and others can find your repo on GH.  A fine point:
  
  - In the example, the Python packages were put in the subdirectory `src` (which is one of the standard places to put them) and `setup(package_dir = {'':'src'},...)` tells `setup` to look there to find your packages.
  
  - You can also put your package directories like `mypackage` directly in the top directory, rather than other `src`.  But if you do this and you call your repo `mypackage` (rather than `myrepo` as in the example), then you will find yourself doing things like
    
    ```
    somewhere/mypackage/mypackage$ vim mymod1.py   # edit a module
    ```
    
    I think directly repeated subdirectories in a path like this are confusing and irritating, but you may not mind.   If you put your packages under `src` (better, I think) this becomes
    
    ```
    somewhere/mypackage/src/mypackage$ vim mymod1.py
    ```
    
    Now, whenever you see `src` in the path you know you are dealing with your package files.

**Things not discussed** (because beyond my expertise).

- The example above was copied and simplified from [this article](https://docs.python-guide.org/writing/structure/) which discusses lots of stuff I don't know about like pip requirements files, Makefile's and automatic test suites.

- There seems to be a newer way of doing things using [Poetry](https://python-poetry.org/) to manage package dependency (rather than Conda) and a file called [pyproject.toml](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/) to control the setup process.

- Rather than simply having your package available for download from your GH repo, you could **publish it** on [PyPi](https://pypi.org/) or one of the [registries that GH lists](https://github.com/doncandela/gs-git/packages).
