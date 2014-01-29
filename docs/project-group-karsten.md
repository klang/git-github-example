http://ndpsoftware.com/git-cheatsheet.html

# clone the initial repository

    git clone https://github.com/klang/git-github.git project-group-2
    cd project-group-2

## present yourself to git

    git config --local user.name "project-group-2"
    git config --local user.email ""

## tell git what editor you would like to use

    git config --global core.editor emacsclient

## tell git that you want to see some colors

    git config --global color.ui true

### setting up a devlopment branch

All development branches off develop as features and is merged back into develop

    git branch --track develop origin/develop
    git checkout develop



### karsten1 

    git checkout -b feature/karsten1 develop
    echo -e "spacer1\nspacer2\nkarsten1:feature\nspacer3" > karsten.txt
    git add .
    git commit -m 'karsten1:feature added'
    git push origin feature/karsten1
    git config "branch.feature/karsten1.remote" "origin"
    git config "branch.feature/karsten1.merge" "refs/heads/feature/karsten1"

feature/karsten1 is now ready for peer review but feature/karsten1 has not been officially "finished" (i.e. merged to develop)

### karsten2

    git checkout -b feature/karsten2 develop
    echo -e "spacer1\nspacer2\nspacer3\nkarsten2:feature" > karsten.txt
    git add .
    git commit -m 'karsten2:feature added'
    git push origin feature/karsten2
    git config "branch.feature/karsten2.remote" "origin"
    git config "branch.feature/karsten2.merge" "refs/heads/feature/karsten2"

feature/karsten2 is now ready for peer review but feature/karsten2 has not been officially "finished" (i.e. merged to develop)

### karsten3

    git checkout -b feature/karsten3 develop
    echo -e "karsten3:helper1\nspacer1\nspacer2\nspacer3" > karsten.txt
    git add .
    git commit -m 'karsten3:feature added'
    git push origin feature/karsten3
    git config "branch.feature/karsten3.remote" "origin"
    git config "branch.feature/karsten3.merge" "refs/heads/feature/karsten3"

feature/karsten3 is now ready for peer review but feature/karsten3 has not been officially "finished" (i.e. merged to develop). In fact, project-group-1 has to finish feature/karsten3 and you have to finish feature/morten3 made by them.

# release 1 has to be constructed (we have one part, group 1 has the other)

    git fetch --all
    git checkout develop

    git status

if something new has happened on origin/develop, git will tell you that it can be fast-forwarded. First, it's nice to see what we expect to get into our development branch:

    git log origin/develop ^develop

"Git, give me the log for commits on origin/develop, that are NOT in my develop."
Ok, that is probably acceptabel, so go ahead and merge the changes (--ff-only to make sure that we get no conflicts)

    git merge --ff-only origin/develop

Ok, now we can merge our own changes.

    git merge --no-ff feature/karsten1
    git branch -d feature/karsten1
    
    git merge --no-ff feature/karsten2
    git branch -d feature/karsten2
    
    git push origin develop
    git push origin :feature/karsten1
    git push origin :feature/karsten2


... wait for it, waaaait for it .. or, just finish feature/morten3 (NOT feature/karsten3 that you started)

    git checkout develop

    git fetch --all

Something is probably moving on the development branch, that's ok. We can fast-forward our own develop without doing any damage.

First, we can check what incoming code we can expect to get

    git log origin/develop ^develop

Fair enough, let's ff that

    git merge --ff-only origin/develop

Now, let's finish what project-group-1 started in feature/morten3

    git branch --track feature/morten3 origin/feature/morten3
    git checkout feature/morten3

feature/morten3 does not have the latest changes from develop, the ones we just added from origin/develop, so let's get those changes in:

    git rebase develop

You should have something like this, in `karsten.txt` right now:

    klp@ergates project-group-2$  more morten.txt
    morten3:helper1
    spacer1
    spacer2
    p1:feature
    spacer3
    p2:feature

Let's just add a feature between spacer1 and spacer2

    echo -e "morten3:helper1\nspacer1\nmorten3:feature\nspacer2\np1:feature\nspacer3\np2:feature" > morten.txt
    
Let's just skip the staging area and commit this change directly to the repository.

    git commit -a -m 'morten3:feature added'

At this point, you'll have this status:

    klp@ergates project-group-1$  git status
    # On branch feature/morten3
    # Your branch and 'origin/feature/morten3' have diverged,
    # and have 19 and 1 different commit each, respectively.
    #
    nothing to commit (working directory clean)

To be able to push feature/morten3, the branches need to be in sync

    git pull --rebase
    git push origin feature/morten3

# making the release

(verify that the release manager has started making the release .. then you can continue)


    git checkout develop
    git fetch --all
    git status
    ;; status will tell you, that your branch is behind, and can be fast-forwarded
    git merge --ff-only origin/develop

# merge feature/morten3 to develop

    git checkout develop
    git status
    git merge --ff-only origin/develop

Merge feature/morten3

    git merge --no-ff feature/morten3
    git branch -d feature/morten3

Verify that develop has not moved (incoming code)

    git fetch --all
    git log origin/develop ^develop

Verify that we are about to push something that makes sense (outgoing code)

    git log develop ^origin/develop
    git push origin develop

    git push origin :feature/morten3

A bit of housekeeping needed to prepare for the next strint.

fast-forward develop and master

    git checkout master
    git merge --ff-only origin/master
    
    git checkout develop
    git merge --ff-only origin/develop

Remove branches that we don't need anymore

    git branch -d feature/karsten3
    git remote prune origin

## end result

    klp@ergates project-group-2$  git branch -a
    * develop
      master
      remotes/origin/HEAD -> origin/master
      remotes/origin/develop
      remotes/origin/master









merge feature/morten3 to develop

git checkout develop
git status
;; status will tell you, that your branch is behind, and can be fast-forwarded
git merge --ff-only origin/develop

git merge --no-ff feature/morten3
git branch -d feature/morten3

git fetch --all
git status
;; Your branch and 'origin/develop' have diverged,

git pull --rebase
git status

;; Your branch is ahead of 'origin/develop' by 2 commits.

git push origin develop

git push origin :feature/morten3

;; done

;; fast-forward develop and master
git checkout master
git merge --ff-only origin/master

git checkout develop
git merge --ff-only origin/develop

;; house keeping

git branch -d feature/karsten3
    warning: deleting branch 'feature/karsten3' that has been merged to
             'refs/remotes/origin/feature/karsten3', but not yet merged to HEAD.
    Deleted branch feature/karsten3 (was d47144d).

(just make sure that you don't need it)

