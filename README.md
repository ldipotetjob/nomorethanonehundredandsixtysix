There are different choices to host a static website on github. </br>There is one of them that in my point  of view  is one of the easiest ways. </br>First we need to create a static website that will be hosted on github pages,Hugo(https://gohugo.io/) is a static site generator that allows you to create a website with little to no code and we are going to use it to build the static web site. </br>For creating static websites with Hugo:

1. [Install Hugo on local](https://gohugo.io/getting-started/installing)
2. Create a github repository($githubrepositoryname) where you are going to upload the info for your github page (where gonna be hosted the blog information) 

```shell
mkdir $githubblogrepositoryname
cd $githubblogrepositoryname
hugo new site .
```

Creating and configuring github repo to store the site previously created

```shell
cd $githubblogrepositoryname
git init
# git remote add origin https://github.com/ldipotetjob/nomorethanonehundredandsixtysix.git
git remote add origin https://github.com/$repository.username/$githubblogrepositoryname.git
git pull origin master
echo "public/" >> .gitignore
git add .
git commit -m "initial commit"
git push origin master
# git submodule add https://github.com/Track3/hermit.git themes/hermit
git submodule add $git_repository-of-HugoTheme themes/hermit
# getting rid of git submodule(.gitmodules) untracked status
# add => ignore = dirty  to .gitmodules file 
```

Now let's check if everything went well. Deploy in local

```shell
hugo server -D 
####
# ........
# Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
####
```

If everything went well our next step will be create [the github page](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages).</br>
Remenber it's format:

 `http(s)://<username>.github.io` or `http(s)://<organization>.github.io`
 
```shell
#clone the repo that contain github page 
git clone https://github.com/<username>/<username>.github.io.git
# move to the directory($githubblogrepositoryname) where your blog info is located
cd $githubblogrepositoryname
```
 
### How our Hugo static web site can be deployed on github page 
 1. The hugo static site will need a binding to a github url page:</br>
    In the[config.toml](https://github.com/ldipotetjob/nomorethanonehundredandsixtysix/blob/master/config.toml) file the key baseURL must have the github url page </br>baseURL = `http(s)://<username>.github.io`
 2. Check that you are on $githubblogrepositoryname and then launch: hugo -d **./\<username>.github.io/**  
 3.    


 


