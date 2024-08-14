Installation Guide to everything
==

Hello and welcome to the installation guide to everything where you will learn the fundamentals and the overall setup of the frameworks developed and used within the IAI. This tutorial will also contain important pointers to software which is not a must to use, but might be nice to have. Such instances are flagged accordingly. 

# Basics and prerequisites
We are going to use Ubuntu 20.04 for everything so if you don't have it installed, now would be a good time. It is recommendet to setup a dual boot for it, or, if you must, you can run it via WSL2 on Windows. But preferably not on a virtual machine. Using a VM is nice for a while but is rather prone to issues so it is not recommended beyond a few tutorials maybe.

## Terminator
We are going to use the terminal a lot and we will need multiple windows of it fairly often. Therefore the installation of [Terminator](https://wiki.ubuntuusers.de/Terminator/) by running:

    sudo apt install terminator

is recommended. Terminator will alow you to split the terminal into multiple windows allowing for a nice overview and organisation of the various terminals. You can split the current window with:

    Shift+Ctrl+O        //split horizontally
    Shift+Ctrl+E        //split vertically
    Shift+Ctrl+T        //new Tab

An overview over useful terminal commands which are good to know can be found [here](https://techlog360.com/basic-ubuntu-commands-terminal-shortcuts-linux-beginner/).

## GitHub
All of our code is uploaded on [GitHub](https://github.com/) so it is recommendet to create an account there. Once you are done with that, please let us know so we can add you to the organizations you would be workif you really want to ing with, for example the [code-iai](https://github.com/code-iai/) one. Otherwise you will not be able to contribute. It is also important that you generate and add an ssh key to your GitHub account. A lot of our dependencies are set to use ssh as a default instead of https, and https is also now deprectaed. Meaning, that if you clone reposetories via https, you won't be able to push to them later on. Therefore, just use ssh from the get go to save yourself the hassle. A tutorial on how to set up ssh, can be found [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh). It will also explain why. Please follow the tutorials in the *connect with ssh* section. 

It is also recommended to set your user name and email adress for github verification. You can do so by adapting the following commands: 

    git config --global user.name "Your Name"
    git config --global user.email "youremail@yourdomain.com"

There are very many tutorials which showcase how git works and how to use it. One example can be found [here](https://dev.to/surajv/git-github-in-a-nutshell-252d). Feel free to google your heart out on git if you are interested. There are very many ressources, tutorials and explanations available. Don't stress about it too  much however, you will learn it overtime.

## ROS
ROS (robot Operating System) is the base framework which allows all our tools to communicate with one another and is the foundation of many demos. Therefore, it is important to have at least a basic understanding of it. The ROS version you need is dtermined by the Ubuntu version you are using. Hence, if you are using Ubuntu 20.04, you will need to install ROS Noetic. If you are using Ubuntu 18.04, you will need ROS melodic, etc. We are also using ROS 1 at the moment, so please make sure to NOT install anything related to ROS 2 (so no Ubuntu 22.04 since it supports ROS2 only).
Please follow [this](http://wiki.ros.org/noetic/Installation/Ubuntu) tutorial to install ros-noetic-desktop-full.

Please add the following line to your .bashrc file:

    source /opt/ros/noetic/setup.bash

Whenever you edit anything in your bashrc, you need to run the *bash* command in the terminal or you need to restart your terminal so that the changes will be applied. This line tells ROS where it is installed and where ROS will look for packages and dependencies for your code. 
Please do the [ROS Tutorials](http://wiki.ros.org/ROS/Tutorials) to get familiar with ROS. It is generally recommended to understand this infrastructure and know, how ROS Nodes, Services, Topics, Messages and Actions work (at least roughly).

### Good to know things about ROS
This section will present a few tipps and tricks around ROS which are recommended practices.

#### Catkin build vs Catkin make
Typically, you will find *catkin make* mentioned in most ros-tutorials. It is the building tool used by ROS for building workspaces, compiling C++ code and generating message/service/action files. However, *catkin build* is a new tool which does the same, and offers more control to the developer. Basically they work in the same fashion, so whenever you see catkin make in a tutorial, you can replace it with catkin build. 
Catkin build offers more features, like building only one specific package instead of the entire workspace and it is better at utilising multithreading. Also most people would recommend it these days. However, you will need to install it separately since it does not come with ROS by default. 

tl;dr: please use catkin build
```
sudo apt-get install python3-catkin-tools
```
You can find more documentation about it on: 
https://catkin-tools.readthedocs.io/en/latest/installing.html

#### Workspace overlaying
In the Tutorials you will learn how to setup a ROS workspace. Every workspace needs to be sourced, so that ROS can find it. So after you create your workspace and make it with *catkin_make* or *catkin build*, don't forget to source it by adding the workspace path to your command line, after the source line for ROS noetic. E.g:

    source PATH/TO/YOUR/WS/devel/setup.bash

Generally, most people will work with only one tool and therefore have only one workspace. However it is recommended to have two. One for dependencies which one downloads once and then never has to touch again, and another one for the code one is developing. 
To do that, create a dependency workspace with stuff you rarely touch, catkin_make it, source it, then create your development workspace, catkin_make it, source it. The workspace which is now *on top* is your development workspace. And when it is build, catkin can find all the dependencies which are *underneath* in the dependencies workspace. A tutorial on this can also be found [here](http://wiki.ros.org/catkin/Tutorials/workspace_overlaying).

You can have as many workspaces as you want but two or three are usually enough. It might be tempting in the beginning to have everything in one workspace, but as one grows the tools and frameworks one uses during development, the build time of a wokspace can also become fairly large. By overlaying workspaces and outsourcing the dependencies from the main development, one does save time later on. 

#### Environment Variables
ROS works across the network and can run on multiple computers at the same time. Think of the robot preparing pancakes with multiple researchers working on it. The robot itself is the main actor and it's computer will probably run the roscore, and is therefore the ros-master. While the researchers might focus on supervising their part of the demo. E.g. one person running the perception framework on their local machine, another one running the motion planning etc. For this to work, your local ROS needs to know who runs the roscore (aka: who is the master) and the master needs to know your IP and your HOSTNAME. Therefore, you need to set these parameters in your .bashrc file.

If you are going to work locally only for now, you can set everyting to localhost or 127.0.0.1:
    
    export ROS_MASTER_URI=http://127.0.0.1:11311
	export ROS_IP=127.0.0.1
	export ROS_HOSTNAME=127.0.0.1

If you work on the real robot your setup could look like this:

    Example: working remotely on DonBot
    export ROS_MASTER_URI=http://192.168.102.59:11311    # IP of DonBot and Port
	export ROS_IP=192.168.102.19                         # your IP
	export ROS_HOSTNAME=192.168.102.19                   # your IP

After putting this in your bashrc, restart your terminal or simply run the "bash" command. To check if all the variables got set, run one of the following:

    echo $ROS_MASTER_URI
    echo $ROS_IP
    echo $ROS_HOSTNAME

If they are not set, you might experience the issue that you can see Nodes running with "rosnode list" and you might be even able to connect to the master, but it might happen that you don't get any data from the rostopics if you try to run "rostopic echo /ANY_EXISTING_TOPIC".

# IAI Frameworks
It is very likely that you will have to work with one of the several developed and maintained by the institute. Each framework comes with it's own set of tutorials and explaining and putting them all here would be a bit too much, so I'll put the most important info here, for an overview. If you'd like to know more about these tools, feel free to talk to the respective developers. Check the Zulip channels or simply ask around a bit. :)
## Perception - RoboSherlock (deprecated)
Website: http://robosherlock.org/
Language: C++
Installation Instructions: http://robosherlock.org/installation.html
Tutorials: http://robosherlock.org/tutorials.html
Publications or other rescources: http://robosherlock.org/publications.html
Main devs: Patrick, Vanessa, Franklin

## Perception - RoboKudo
Website: https://robokudo.ai.uni-bremen.de/
Language: Python 3
Installation Instructions: https://gitlab.informatik.uni-bremen.de/robokudo/robokudo/-/blob/main/doc/install.md
Tutorials: https://robokudo.ai.uni-bremen.de/tutorials_overview.html
Publications or other rescources: https://robokudo.ai.uni-bremen.de/publications.html
Main devs: Patrick, Franklin, Vanessa

## Manipulation - Giskard
Website: http://giskard.de/
Language: Python 3
Installation Instructions: https://github.com/SemRoCo/giskardpy/tree/master
Tutorials: http://giskard.de/wiki:tutorials
Publications or other rescources: http://giskard.de/wiki:publications
Main devs: Simon

## Knowledge - KnowRob
Website: http://www.knowrob.org/
Language: Prolog, C++
Installation Instructions: https://github.com/knowrob/knowrob/blob/master/README.md 
:::danger
master is the development branch so it might be unstable. Double check with supervisor which version you should use. 
:::
Tutorials: http://www.knowrob.org/doc (Outdated!!! Useful to read, execution might not work. Also check here for references: https://github.com/knowrob/knowrob/blob/master/README.md)
Publications or other rescources: http://www.knowrob.org/publications
Main devs: Daniel B., Sascha, Kaviya

## Knowledge - NonFoodKG knowledge graph
Website: https://k4r-iai.github.io/NonFoodKG/
Language: Prolog, SPARQL
Tutorials: https://github.com/michaelakuempel/ease_fs_kg_2022
APIs:
- grlc REST API (http://grlc.io/api/K4R-IAI/NonFoodKG/SPARQLfiles/)
- triply REST API (https://krr.triply.cc/mkumpel/FoodToNonFoodKG/sparql/FoodToNonFood)
- Prolog Integration (coming soon)

Publication: coming soon
Main devs: Michaela K.


## NEEMS - SOMA
Ontology which is used by KnowRob for NEEMS. 

Website: https://ease-crc.github.io/soma/
Language: Prolog?
Publications: https://ease-crc.github.io/soma/owl/1.1.0/NEEM-Handbook.pdf

## NEEMS - OpenEase
OpenEase is a GUI for KnowRob which allows to insepct the database and visualize the data aka. the Neems. 


## Planning - CRAM
Website: http://cram-system.org/
Language: LISP
Installation instructions: https://github.com/cram2/cram/blob/devel/README.md
Publications or other rescources: http://cram-system.org/research




# FAQ




# DOCS
We use LaTeX for writing documents and also creating presentations (with rare exceptions).
==todo: add explanation to why Latex is cool==
One can do everything in command line and with any editor, but I'd recommend using an IDE for LaTeX. For example Texstudio which is this one: https://linuxhint.com/install-texstudio-latex-editor-linux/
Latex itself can be installed with this tutorial:
https://linuxconfig.org/how-to-install-latex-on-ubuntu-20-04-focal-fossa-linux

I'd recommend going for the full version, texlive-full. Otherwise one will have some dependencies and package not found issues later and will have to download some packages individually and nobody wants to bother with that really.



