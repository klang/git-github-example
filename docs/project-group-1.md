http://ndpsoftware.com/git-cheatsheet.html

# clone the initial repository

    git clone https://github.com/klang/git-github.git project-group-1
    cd project-group-1

## present yourself to git

    git config --local user.name "project-group-1"
    git config --local user.email ""

## tell git what editor you would like to use

    git config --global core.editor emacsclient

## tell git that you want to see some colors

    git config --global color.ui true

### setting up a devlopment branch

All development branches off develop as features and is merged back into develop

    git branch --track develop origin/develop
    git checkout develop

### p1 

    git checkout -b feature/p1 develop
    echo -e "spacer1\nspacer2\np1:feature\nspacer3" > awesome.txt
    git add .
    git commit -m 'p1:feature added'
    git push origin feature/p1
    git config "branch.feature/p1.remote" "origin"
    git config "branch.feature/p1.merge" "refs/heads/feature/p1"

feature/p1 is now ready for peer review but feature/p1 has not been officially "finished" (i.e. merged to develop)

### p2

    git checkout -b feature/p2 develop
    echo -e "spacer1\nspacer2\nspacer3\np2:feature" > awesome.txt
    git add .
    git commit -m 'p2:feature added'
    git push origin feature/p2
    git config "branch.feature/p2.remote" "origin"
    git config "branch.feature/p2.merge" "refs/heads/feature/p2"

feature/p2 is now ready for peer review but feature/p2 has not been officially "finished" (i.e. merged to develop)

### p3

    git checkout -b feature/p3 develop
    echo -e "p3:helper1\nspacer1\nspacer2\nspacer3" > awesome.txt
    git add .
    git commit -m 'p3:feature added'
    git push origin feature/p3
    git config "branch.feature/p3.remote" "origin"
    git config "branch.feature/p3.merge" "refs/heads/feature/p3"

feature/p3 is now ready for peer review but feature/p3 has not been officially "finished" (i.e. merged to develop). In fact, project-group-2 has to finish feature/p3 and you have to finish feature/q3 made by them.

# release 1 has to be constructed (we have one part, project-group-2 has the other)

project group 1 adds the features they have ready for release to develop

    git fetch --all
    git checkout develop
    git status

if something new has happened on origin/develop, git will tell you that it can be fast-forwarded. First, it's nice to see what we expect to get into our development branch:

    git log origin/develop ^develop

"Git, give me the log for commits on origin/develop, that are NOT in my develop."
Ok, that is probably acceptabel, so go ahead and merge the changes (--ff-only to make sure that we get no conflicts)

    git merge --ff-only origin/develop

Ok, now we can merge our own changes.

    git merge --no-ff feature/p1
    git branch -d feature/p1
    
    git merge --no-ff feature/p2
    git branch -d feature/p2
    
    git fetch --all

    git push origin develop
    git push origin :feature/p1
    git push origin :feature/p2

... wait for it, waaaait for it .. or, just finish feature/q3 (NOT feature/p3 that you started)

    git checkout develop
    git fetch --all

Something is probably moving on the development branch, that's ok. We can fast-forward our own develop without doing any damage.

First, we can check what incoming code we can expect to get

    git log origin/develop ^develop

Fair enough, let's ff that

    git merge --ff-only origin/develop

Now, let's finish what project-group-2 started in feature/q3

    git branch --track feature/q3 origin/feature/q3
    git checkout feature/q3

feature/q3 does not have the latest changes from develop, the ones we just added from origin/develop, so let's get those changes in:

    git rebase develop

You should have something like this, in `amazing.txt` right now:

    klp@ergates project-group-1$  more amazing.txt
    q3:helper1
    spacer1
    spacer2
    q1:feature
    spacer3
    q2:feature

Let's just add a feature between spacer1 and spacer2

    echo -e "q3:helper1\nspacer1\nq3:feature\nspacer2\nq1:feature\nspacer3\nq2:feature" > amazing.txt

Let's just skip the staging area and commit this change directly to the repository.

    git commit -a -m 'q3:feature added'

At this point, you'll have this status:

    klp@ergates project-group-1$  git status
    # On branch feature/q3
    # Your branch and 'origin/feature/q3' have diverged,
    # and have 10 and 1 different commit each, respectively.
    #
    nothing to commit (working directory clean)

To be able to push feature/q3, the branches need to be in sync

    git pull --rebase
    git push origin feature/q3

# making the release

(verify that the release manager has started making the release .. then you can continue)

    git checkout develop
    git fetch --all
    git status
    ;; status will tell you, that your branch is behind, and can be fast-forwarded
    git merge --ff-only origin/develop

# merge feature/q3 to develop

    git checkout develop
    git status
    git merge --ff-only origin/develop

Merge feature/q3

    git merge --no-ff feature/q3
    git branch -d feature/q3

Verify that develop has not moved (incoming code)

    git fetch --all
    git log origin/develop ^develop

Verify that we are about to push something that makes sense (outgoing code)

    git log develop ^origin/develop
    git push origin develop

    git push origin :feature/q3

A bit of housekeeping needed to prepare for the next strint.

fast-forward develop and master

    git checkout master
    git merge --ff-only origin/master
    
    git checkout develop
    git merge --ff-only origin/develop

Remove branches that we don't need anymore

    git branch -d feature/p3
    git remote prune origin
    
## end result 

    klp@ergates project-group-1$  git branch -a
    * develop
      master
      remotes/origin/HEAD -> origin/master
      remotes/origin/develop
      remotes/origin/master