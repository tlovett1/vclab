Advanced Git Lab
=====

Welcome to the advanced Git Lab! The slides from the presentation can be found
[here](https://docs.google.com/a/get10up.com/presentation/d/13I0k0GFLuNFitT6tyte94dV_r-SjBk4Y04bO99G8bRM/edit?usp=sharing]).
The goal of this lab is to you familiar with the concepts from the presentation. From this point forward it is assumed
that you have a Github account.

##### Part 1: Setup

We will use this repository for demonstrating concepts. First thing you should do is clone this repo:
```
git clone https://github.com/tlovett1/vclab.git
cd vclab
```

##### Part 2: Remotes

1. Remember git clone automatically initializes git and sets up the "origin" remote for us. Let's add another remote:
	```
	git remote add beanstalk https://10up.git.beanstalkapp.com/vclab-beanstalk.git
	git remote -v
	```
	We should now see two remotes: origin and beanstalk.

2. Let's branch off of master (origin/master) and push our new branch to beanstalk:
	```
	git checkout master
	git checkout -b my-name-branch
	git push beanstalk my-name-branch
	```
	Remember to replace my-name with your name :)

	How is this useful? Well, you don't have write access to my repository. So if you wanted to "fork" and work on it, you
	would have to push your code to some other repo (Beanstalk).


##### Part 3: Cherry Picking

1. Let's modify some code in master, commit that code, then cherry-pick that commit to our new branch.
	```
	git checkout master
	touch test.php
	git add test.php
	git commit -m "Create a test file"
	```

2. Now let's pull your commit into your new branch:
	```
	git checkout my-name-branch
	git cherry-pick master~0
	git push beanstalk my-name-branch
	```
	Note: master~0 could be any commit reference. Master~0 refers to the latest commit in master going back 0 commits.

	Cool, huh?

##### Part 4: Cherry Picking

Remember, rebasing lets us rewrite history.

1. Let's create a useless commit and rebase on top of what we've already done:
	```
	git checkout master
	touch test2.php
	git add test2.php
	git commit -m "Create another test file"
	```

2. Suddenly, we realize our last two commits were creating test files, and we wish we had just made one commit. Rebase
to the rescue!
	```
	git rebase -i HEAD~2
	```
	Note: HEAD~2 could be any type of commit reference. Rebase takes as input the commit right before the one you want
	rebased. So in this case HEAD~1 and HEAD~0 will get rebased.

3. You should see something like this:
	```
	pick dfae8c1 Create a test file
	pick 575f02a Create another test file

	# Rebase 6b18170..575f02a onto 6b18170
	#
	# Commands:
	#  p, pick = use commit
	#  r, reword = use commit, but edit the commit message
	#  e, edit = use commit, but stop for amending
	#  s, squash = use commit, but meld into previous commit
	#  f, fixup = like "squash", but discard this commit's log message
	#  x, exec = run command (the rest of the line) using shell
	#
	# These lines can be re-ordered; they are executed from top to bottom.
	#
	# If you remove a line here THAT COMMIT WILL BE LOST.
	#
	# However, if you remove everything, the rebase will be aborted.
	#
	# Note that empty commits are commented out
	```

	If you write and quit this file without making any changes, the first commit will be applied then the second. This will result in no 
	changes. We want to squash the second commit and rewrite the first commits message. Let's edit our file like so:
	```
	pick dfae8c1 Create test files
	squash 575f02a Create another test file

	# Rebase 6b18170..575f02a onto 6b18170
	#
	# Commands:
	#  p, pick = use commit
	#  r, reword = use commit, but edit the commit message
	#  e, edit = use commit, but stop for amending
	#  s, squash = use commit, but meld into previous commit
	#  f, fixup = like "squash", but discard this commit's log message
	#  x, exec = run command (the rest of the line) using shell
	#
	# These lines can be re-ordered; they are executed from top to bottom.
	#
	# If you remove a line here THAT COMMIT WILL BE LOST.
	#
	# However, if you remove everything, the rebase will be aborted.
	#
	# Note that empty commits are commented out
	```

##### Part 5: Submodules

1. Let's add the 10up repo Publish-To-Twitter as a submodule (purely as an example):
```
git checkout my-name-branch
git submodule add https://github.com/10up/Publish-to-Twitter.git publish-to-twitter
git commit -m "Add publish-to-twitter submodule"
git push beanstalk my-name-branch
```