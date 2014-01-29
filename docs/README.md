Quick example, using git and github, publishing all features.

## Git - on github

In this demo, two project groups will implement 3 new features (p1 p2 p3 q1 q2 q3) each.

- release v1.1 will consist of p1, p2, q1 and q2
- release v1.2 will consist of p3 and q3

If everything goes according to plan, each group will have to deal with conflicts at some point!


         .-p3------o----------+---o---.
         |                    |        \
         .-p2----o--.         |         \
         |           \        |          \
         .-p1--o--.   \       |           \
         |         \   \      |            \
         .-develop--+---+---. | .-----------+---.
         |                   \|/                 \
    -----o-master-------------+-------------------+ 
         ^                    ^                   ^
       v1.0                 v1.1                v1.2

### working directory for setup

    mkdir git-github
    cd git-github

### initial setup for the demo

    mkdir -p initial-setup/docs
    cd initial-setup
    echo -e "spacer1\nspacer2\nspacer3" > awesome.txt
    echo -e "spacer1\nspacer2\nspacer3" > amazing.txt
    echo -e "v1.0" > VERSION
    git init

    git config --local user.name "instigator"
    git config --local user.email ""

    git add .
    git commit -m 'initial'
    git branch develop
    git tag -a v1.0 -m "v1.0"
    # create project on github
    git remote add origin https://github.com/klang/git-github.git
    git push -u origin --all
    git push --tags origin
