There are different choices to host a static website on github. </br>There is one of them that in my point  of view  is one of the easiest ways. </br>First we need to create a static website that will be hosted on github pages,Hugo(https://gohugo.io/) is a static site generator that allows you to create a website with little to no code and we are going to use it to build the static web site. </br>For creating static websites with Hugo:

1. [Install Hugo on local](https://gohugo.io/getting-started/installing)
2. Create a github repository($githubrepositoryname) where you are going to upload the info for your github page (where gonna be hosted the blog information) 

```shell
mkdir $githubrepositoryname
cd $githubrepositoryname
hugo new site .
```

Creating and configuring github repo to store the site previously created

```shell
cd $githubrepositoryname
git init
# git remote add origin https://github.com/ldipotetjob/nomorethanonehundredandsixtysix.git
git remote add origin https://github.com/$repository.username/$githubrepositoryname.git
git pull origin master
echo "public/" >> .gitignore
git add .
git commit -m "initial commit"
git push origin master



```
