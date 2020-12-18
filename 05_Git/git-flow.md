#Gitflow Workflow

##What is Gitflow Workflow?
- Gitflow Workflow is a Git workflow that helps with continuous softwre development and implementing DevOps prctices.
- Gitflow is sutable for profects that have a scheduled release cycle and for the DevOps best pactice of continuous delivery.

##How it works
- it uses two branches( Develop and Main ) to record the history of the project. 
- The main branch is used to store the official release history, and the develop brench serves as an integrtion brnch for features.
- It's got five branches, main, develop, fetature, release, and hotfix.
- When using the git-flow extentions library, executing git flow init on an exsiting repo will create the develop branch.
```
$ git flow init 
$ git branch
```

##Feature Branches
- Each new feature should reside in its own branch. 
- Feature branches use develop as their parent branch insted of main branch. When a feature is complete, it gets marged back into develop.
- Creating a feature branch
```
\\When using the git-flow extention\\
git flow feature start feature_branch

\\Without the git-flow extentions\\
git checkout develop
git checkout -b feature_branch
```
- Finishing a feature branch
```
\\When using the git-flow extention\\
git flow feature finish feature_branch

\\Without the git-flow extentions\\
git checkout develop
git merge feature_branch
```

##Release Branches
- Once develop has aquired enough features for a release, you fork a release branch off of develop.
- Creating this branch starts the next release cycle, so no new features can be added after this point.
- Once the realease is ready, it will get merged it into masterand develop, then the release branch will be deleted.
- This would be an ideal place for a pull request. (そうなんだー）
```
\\When using the git-flow extention\\
git flow release start 0.1.0

\\Without the git-flow extentions\\
git checkout develop
git checkout -b release/0.1.0
```


###Reference
- Attlasian Bitbucket, accessed 18 Decmber 2020, <https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Gitflow%20Workflow%20is%20a%20Git,designed%20around%20the%20project%20release>
