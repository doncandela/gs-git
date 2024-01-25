### TODO for Getting started with Git and GitHub

D. Candela 1/25/24

General TODO:

- Proofing and wordiness reduction

TODO's extracted from 1/24/24 version of main document, and other to-do's:

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
    - **Pull again** to merge the retrieved remote state into the corresponding local branches. [TODO - SOMETIMES SEEMS DONE AUTOMATICALLY??]
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
    - This is called **publishing the feature branch** (on GHDT, for example) as it makes it visible and available for pulling, pushing, and merging to you and the rest of the collaborators. As shown here the branch to be pushed must be specifically given (or use `git push --all origin` to push all local branches). The intent is that a collaborator's feature branch will not be published to GH until they intentionally do so.
      
      [TODO - set to track remote branch?? happens automatically?]
    - If the collaborator wants to push your branch `feat23` to GH when it is very incomplete e.g. just to back it up, they could push it but hold off opening a PR until it is closer to finished. But all collaborators will see and be able to edit the branch as soon as it is pushed. It's probably better to open the PR right away as **draft PR**, which is an option under the "Create pull request" tab, and/or to start the description with WIP (work in progress) or something similar. (For free GH accounts, draft PRs are only available for public repos.) Draft PRs cannot be merged until they are marked "Ready for review", and then a collaborator assigned as a reviewer will need to submit an approving review (this is done under the "Files changed" tab of the PR). [TODO always true, or just because I set this?]
    - - Working on a PR before it is merged.**
        
        - **Other collaborators work on the feature branch** [TODO how do others and pusher track the new branch on GH?]
        
        - **Updating a feature branch so it can be merged automatically.** If the pushed branch `feat23` can be merged without conflicts, a green **This branch has no conflicts with the base branch** and a green **Merge pull request** button will be displayed when the GH screen for the PR. Often it is the responsibility of whoever initiated the pull request to **update and re-push their feature branch** so it can merged without conflicts.
      
      - **Merging a PR**
        
        -  [TODO write or is this section needed?]
  - [Aside: Rebasing and squashing](#rebasing)
  - [4. Open-source workflow (briefly)](#opensourceflow)
  - [Small groups with differing roles](#differing)
  - [Appendix A: Markdown](#markdown)
    - **Generating documentation for your project.**
      
      [TODO Could write this section -- maybe too much for this handout?]
    - to-do Perhaps show manual TOC generation, mention auto toc apps exist.
  - [Appendix B: Using GH to develop and distribute a simple Python package](#pythonpackage)
