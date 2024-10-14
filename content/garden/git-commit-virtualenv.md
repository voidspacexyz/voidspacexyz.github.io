+++
author = "VoidSpaceXYZ"
title = "!(git commit virtualenv)"
date = "2018-03-18"
description = ""
tags = [
    "python",
]
categories = [
    "python",
]
series = []
+++

So, To write my first blog post of the year, I was attending the first FSMK Sunday School of the year "Handson with Django". I was quite exicted about this for a few reasons.

1.  The mentors were all participants of Django Girls in Bangalore this year
2.  Their mentor was a fellow Free Software Activist at FSFTN (@srravya)
3.  FSMK was celebrating women's months with all women speakers and mentors all over the month. First time I was hearing that.
4.  I was absolutely jobless on that day, and attending this event meant spending some useful time with a great set of people.

So this article is not about the event, as such, but about a common mistake that people do with python. A common mistake that was also done in this hands-on.

Yes, that is "Checking in the virtualenv folder along with the source code of the project". This article is intended to explain , why that is a bad practice, and why you should never do such a mistake.

So before going into the details of why that was wrong and stuff, Lets understand what is a virtualenv, and why it is a good practise to use virtualenv to all your projects.

So, virtualenv by definition is about, creating separate environments for project specific dependencies. In layman terms, let's say, we are working in "project 1" with dependencies Django-1.8 and django-allauth , and "project 2" with dependencies Django-1.9 and django-allauth. You can already see what could go wrong. If not, you see the two projects require the same package(Django) of different versions(1.8 and 1.9). Now your system cant have both the version installed together, so you need a solution here.

The most common solution is to upgrade your Django-1.8 to Django-1.9 and be happy. But that doesn't work always. Once you start a project, your aim should be on writing code, rather than keep upgrading the package. The other way to put a solution is to say, "No matter what, I will work only with Django 1.9". But the question is how long ? And when you decide to upgrade, how many projects will you upgrade or and how sure are we with all the dependencies of the projects ?

So none of these are viable solutions. They could work for some small time fun projects, but won't run in the longer term.

Now let's say, I am exploring a lot of free projects based on python, and one project looks interesting. If I just want to explore to see if it works or not, why would I want to install the whole package in my base system? What if those packages, conflict with my base system or downgrade some of my system packages ? So what would you do ? No, with python you don't need virtual machines. That is an overkill.

Now talking about the base system, we all do a common mistake forgetting the common statement in free world "With Great power comes greater responsibilities". Yes, if you understand what the great power is, I am talking about "sudo".

sudo pip install package\_name.

No, again, unless you completely understand what the package does, and what system files does this change. Never do this, It could end up breaking a lot of the system dependencies. What ?? How ? Now that is another topic, will soon write-up on "When to think sudo and why sudo is not a solution to all your problems".

So what would you do ? Well, you already know the answer, but still spilling the beans, the solution is **virtualenv**. How is that a solution ?

Virtualenv takes a different stand in solving the dependency issues of a project. So just like virtual machines, you create virtual python environments, that uses the **system's python** and keep all your dependencies in a virtual machine like space, that will never conflict your system packages.

\*\* Psst ! Please note and remember "System's Python". Very important.

Apparently , with virtualenv , You don’t want to have to set up a whole new server with a different version of Django to use it.

Assuming that has given a brief overview of what is a virtualenv and how it is useful, lets come to the actual problem.

We all use version control, for committing and keeping our code publicly visible, for all various reasons.

So here is a task.

Here are a few python projects, have a look at their root tree source code . Don't take more than 2 min's for each project.

1.  Django \[https://github.com/django/django\]
2.  Numpy \[https://sourceforge.net/projects/numpy/files/\]
3.  Scipy \[https://sourceforge.net/projects/scipy/files/\]
4.  Pelican \[https://github.com/getpelican/pelican\]

So now to the question, in any of these projects that you saw, did you see the virtualenv files checked in ??

Very obviously "no" right ?? Great, point proved. !

Nope, I am not stopping there. I am kinda long always :-D

So let's also understand why, do they not check in, and what are the effects of checking it in !

Source/Version control system`(GIT,SVN etc) are for the code you write, not for the system packages/binaries that is tied to your system.`

The python could be wrongly referred.`What ? Do you remember "system's python" I mentioned before. How do you create a virtualenv ? Do you do a sudo apt/dnf install python-virtualenv and virtualenv blog Now when you do that command, it actually makes a reference to your system's python binary for its execution. It does not install a new version of python. I know, you still dont trust me, well dont. Try it for yourself. Now open your command line, and explore . . Hmmm, How ? Here you go.  - Without any virtualenv, type "which python"        You should see "/usr/bin/python" - Now Activate a virtualenv, and type "which python"    Now assuming, I have create a virtualenv called test in my   /tmp directory, my output would be something like "/tmp/test/bin/python".     Now if you do a ls -latr /tmp/test/bin/python , you will see that it is referring itself to the system python ? Do you ?? Now that is a good reason, why you should never checkin your virtualenv. I am still not convinced. Well read on, the next 3 points.`

People use different versions of python.`Now, I am a young guy, but always loves to use the most stable stuff. So I dont use the latest python, I rather use a stable release. So now lets say, you create a virtualenv with a differnt version of python (python3 for example) and I still run a python2, your virtual machine's binary would still be pointing to python3 of your system, but whereas when I pull the code on a system that dosent have python3, I would have to delete all your system generated binary files of python3 and recreate all the binaries for python2 again. Which is a lot of pain. (well actually not, with virtualenv all I have to do is to copy your source code and put it in a virtualenv I create with my python) So, not checking the virtualenv also solved the different versions of python issue. Looking it from a free software prespective, I am talking about Freedom to choose the version of python I want to run. But with checking in the virtualenv binaries, you would be forcing me either to do a lot more work, or to use the same version of python as your run.`

People use different architecture of computers.`Heard of 32 bit's (Yes, they still exist a lot) , 64 bits, ARM etc. Well, when I compile python for differnt architectures, my binaries itself could change. In simple words, if you, say pip install mysql-python, on a 64 bit machine, and then someone with a 32 bit machine tries to use it, it will not work.`

People still use the most crappy and spying Windows, and yet they could use your code.`Now, lets say, you have checked in the code with your virtualenv, and I pull it on a windows machine. (Remember that is just for an example, and Microsoft, if you are reading this, I dont pirate you anymore, rather I dont use you atall.) If your reaction to that was "Duh", I tried to make a joke, and it ended up being a PJ. So move on. Now I pull down your code on a Windows machine, remember the virtualenv python references to something called "/usr/bin/python" ?? It dosent even exist in Windows. Its called python2.7.exe in windows (assuming you have setup all the environment variables correctly). Now that could fail.`

**So always, remember only to check in the source, not the result of running a process.**

A virtualenv is rather platform specific; a Windows virtualenv may require different binaries than one created on Linux. The paths in the script files will almost certainly use absolute paths, not relative paths tying a virtualenv to a specific location on your harddisk.

And, the best practise is only to check the source and give a requirements.txt that will install all the required binaries for the project. (If this line didnt make sense, read up about, python-pip, pip freeze and pip install -r requirements.txt)

Assuming, you are satisfied with not checking in the virtualenv as a part of "source control system", the bigger question is, How did people even think of checking in the virtualenv ??

Well, apparently looking the docs of the tutorial of "Django Girls", where they have mentioned to check in the virtualenv as a part of the code. [Refer here for the instructions on Django Girls](https://web.archive.org/web/20230610105315/http://tutorial.djangogirls.org/en/deploy/index.html?ref=voidspacexyzs-blog#starting-our-git-repository)

Now to those who attended, Django Girls, if any of you could file a issue/bug in their docs, and ask them to correct it, it could help of a lot of people who are using it for reference.

Believe me, Django Girls tutorial, is one of the very best Django kick starters available out there. Its totally awesome. To anybody who say, "I want to learn Django", that is the first url I always share. A great community effort, and being a community, we have to notify the people, when something is wrong.  So if some of you who attended Django Girls or mentored Django girls, would be kind enough to file a bug, and let them know, I would be a happy person.

Kudos and thanks again, if you read every line of the article or just skipped all the lines in between and came to this line just for kudos :-D

Have Fun . .