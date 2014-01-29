http://ndpsoftware.com/git-cheatsheet.html

    git clone https://github.com/klang/git-github.git release-manager-1
    cd release-manager-1

    git config --local user.name "release-manager-1"
    git config --local user.email ""

    git branch --track develop origin/develop
    git checkout develop

.. and prepares the release

    git branch release/v1.1 

develop is now free to recieve new changes. Last minute changes can be made to the release.

    git checkout release/v1.1 
    echo "v1.1" > VERSION
    git add VERSION
    git commit -m "version bumbed to 1.1" VERSION

expected contents:

    klp@ergates release-manager-1$  more amazing.txt
    spacer1
    spacer2
    q1:feature
    spacer3
    q2:feature
    klp@ergates release-manager-1$  more awesome.txt
    spacer1
    spacer2
    p1:feature
    spacer3
    p2:feature
    
Merge into master

    git checkout master
    git fetch origin master
    git merge --no-ff release/v1.1
    git tag -a v1.1 -m "v1.1"
    git push origin master
    git push --tags

Merge back into develop

    git checkout develop
    git fetch origin develop
    git merge --no-ff release/v1.1
    git push origin develop
    git branch –d release/v1.1
    
