# Lab 01 Tools for reproducible research
### Date: 08/26
### Time: 2 hours

## Objectives 
* become familiar with Rstudio’s Git and Github integration
* be able to use Markdown syntax
* be able to pull course materials from the workshop github page

## Why use git?
* [Git is useful to avoid this](http://phdcomics.com/comics/archive.php?comicid=1323)
* [While there is some truth in it, we want don't want to be in this situation either](https://xkcd.com/1597/)

## Three tools 
* git/github (command line)
* Rstudio (UI driven, after a repository is created)
* Markdown syntax

## Prerequisite
Download and install

* [R 3.6.0](https://cran.r-project.org/)
* [Rstudio desktop](https://www.rstudio.com/products/rstudio/download/)
* [git](https://git-scm.com/downloads): you might already have git installed if you have macOS or Linux. [This](https://happygitwithr.com/install-git.html) is a very nice tutorial on how to install. 

## Outline
* Version control with Git and Github
* Use Git in Rstudio
* A first pull from SlicerMorph/S_2019
* Create your own project on Github and a first push by Rstudio
* Markdown: a way to style text

## A first pull from SlicerMorph
0. Open Rstudio

1. Create a new version control project: File -> New Project -> Version Control

<img src="images/newproject.png">

2. Clone Git repository.  In *Repository URL* type
```
https://github.com/SlicerMorph/S_2019.git
```
<img src="images/git.png">

3. Open a new terminal: Tools -> Terminal -> New Terminal

<img src="images/terminal.png">

4. Type in the terminal
```
git pull
```

## Create your own project on Github and make it talk to Rstudio
1. Log in to your Github account and create a new repository. There is a small "+" sign right next to your profile picture

<img src="images/plus.png">

2. Fill in the information about your new repository. I usually select the the option *Initialize this repository with a README*. Once you are finished, click *Create repository*

<img src="images/newrepo.png">

3. Copy the URL of your new repository

<img src="images/repopage.png">

4. Creat a new version control project in Rstudio as before

5. Clone your new Git repository

6. Open a new terminal: Tools -> Terminal -> New Terminal

7. Type in the terminal 
```
git pull
```

## Edit *Readme* and push it online
1. Open the Readme file. You can do it with any text editor you like or in Rstudio. 

2. Type a descriptive paragraph about your project. Or just an emoji, like :pig:. 

3. Save your Readme file and close it

4. Go to the terminal in Rstudio and type in the terminal
```
git add *
```

5. Commit your change to git
```
git commit -m "<edit_readme>"
```
*-m* allows you to edit a customized commit message to describe what you have added/changed in the repository.

6. Push your change to git
```
git push
```
7. Go to your github page and see you have made a change there too

## Use the Rstudio UI


## What git is NOT?
* A real-time collaboration tool
* A backup replacement
* Keeping files in sync real-time (e.g., like dropbox)
* Not good with binary files (office documents, pdfs, pictures, etc)

## Resources
* [Resources to learn Git by Github](http://try.github.io)
* [Git Book](https://git-scm.com/book/en/v2)
* [Git Reference](https://git-scm.com/docs)
* [Markdown syntax](https://guides.github.com/features/mastering-markdown/)

