# Getting started

1. Create a new repository by using this repository as a template. Read more here : https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-from-a-template
1. Clone this repository in your local system.
1. Install python in your system. https://www.python.org/ [If you own a mac or a linux machine you already have a python]
1. Open your command prompt [terminal] to check if you have already installed python or not. Type `python` and if it return an error then its not installed correctly. Google "how to install python in windows/mac"
1. Next enter `pip` in the command prompt and hit enter. If it shows a list of commands then we are good to go. It is a package manager for python, we will use this to install programs. If it doesn't, use this to install it : https://pip.pypa.io/en/stable/installing/
1. Now you have to run some commands to install packages required to serve this documentation in your local machine. Run this command one by one.
    `pip install mkdocs`
    `pip install mkdocs-material`
    `pip3 install mkdocs-git-revision-date-localized-plugin`
1. Open terminal where you have cloned your new documentation repository.
1. Run `mkdocs serve`. This will start building the documentation site which you can see by visiting at `http://127.0.0.1:8000/`
1. When you are have created documentation and would want to deploy. Just use `mkdocs gh-deploy`. This will deploy the website to github pages.
To get the link of the site you can either look at the response of your above command or go to the setting of your repository and scroll down to the bottom to `GitHub Pages` section to find the link.


### Other Important Links

1. Github : https://krenovate-solutions-pvt-ltd.github.io/training-manuals/Daily-Apps/GitHub/
1. VSCode : https://krenovate-solutions-pvt-ltd.github.io/training-manuals/Daily-Apps/Visual-Studio-Code/
1. mkdocs : https://www.mkdocs.org/#installation
1. material theme : https://squidfunk.github.io/mkdocs-material/getting-started/#extensions
