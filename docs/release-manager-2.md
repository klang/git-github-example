http://ndpsoftware.com/git-cheatsheet.html

The release manager clones the repository

    git clone https://github.com/klang/git-github.git release-manager-2
    cd release-manager-2
    
    git config --local user.name "release-manager-2"
    git config --local user.email ""
    
    git branch --track develop origin/develop
    git checkout develop

.. and prepares the release
    
    git branch release/v1.2
    
develop is now free to recieve new changes. Last minute changes can be made to the release.

    git checkout release/v1.2
    echo "v1.2" > VERSION
    git add VERSION
    git commit -m "version bumbed to 1.2" VERSION

expected contents:

    klp@ergates release-manager-2$  more amazing.txt
    q3:helper1
    spacer1
    q3:feature
    spacer2
    q1:feature
    spacer3
    q2:feature
    klp@ergates release-manager-2$  more awesome.txt
    p3:helper1
    spacer1
    p3:feature
    spacer2
    p1:feature
    spacer3
    p2:feature

Merge to master
    
    git checkout master
    git fetch origin master
    git merge --no-ff release/v1.2
    git tag -a v1.2 -m "v1.2"
    git push origin master
    git push --tags

Merge back into develop
    
    git checkout develop
    git fetch origin develop
    git merge --no-ff release/v1.2
    git push origin develop
    git branch -d release/v1.2
    
