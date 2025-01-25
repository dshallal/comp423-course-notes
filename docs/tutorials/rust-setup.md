# Setting up a dev container for Rust

* Primary author: [Dan Shallal](https://github.com/dshallal)

* Reviewer: [Kishan Gajera](https://github.com/gajekish)

Welcome! In this tutorial, we will be learning how to set up a development container for Rust. From an empty folder to printing hello world in a fully working development container, this tutorial will teach you it all.

## **Prerequisites**

1. A GitHub Account: Sign up at [GitHub](https://github.com/) if you do not have one.
2. Git Version Control: Make sure to install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) if it is not installed on your machine.
3. [Visual Studio Code](https://code.visualstudio.com/) is also required. It is an IDE (Integrated Development Environment) that will make setting up our Docker container smooth and easy.
4. [Docker](https://www.docker.com/products/docker-desktop/) must also be installed.
5. Command-line basics: If you're rusty, review the Learn a CLI text from COMP 211.

## **Tutorial**

### First Step: Creating a Git Repository

1. Open your terminal to create a new directory for your Rust project.
2. Choose where you would like your project to reside within your machine (opening terminal leads to your user's home directory). Afterwards, go ahead and run:

    ```bash
    mkdir rust_dev_container
    cd rust_dev_container
    ```

3. Initialize your git repository by running:

    ```bash
    git init
    ```

4. Create a README.md file to document this project's purpose:

    ```bash
    echo "# Setting up a development container for Rust" > README.md
    ```

    Now that we have made a README.md for our project, we need to use git to track these changes locally.

    ```bash
    git add README.md
    git commit -m "Adding README.md file as initial commit"
    ```

    Now that we have a git repository, let's connect it to a remote cloud service. We will specifically be using GitHub.

### Second Step: Remote Repository on GitHub

It is important to link your local git repository to a remote cloud service so to allow for a centralized code base for if you have team members who want to work on this project, automate CI/CD, etc.

1. After logging into GitHub, create a new repository with the name "rust-dev-container". Add any description you would like and have the visibility set to Public. In addition, make sure you do not have gitignore, README, or license checked.

2. Now we need to link our local repository to this remote repository. Back in the terminal, run:

    ```bash
    git remote add origin https://github.com/<your-username>/rust-dev-container.git
    ```

    **Change `<your-username>` with your GitHub username**

    !!! tip
        <code>git branch</code>` is an important git subcommand that shows what branch you are on, and other branches within the repository. If you have an older version of git, you may see that you are currently on the "master" branch. Run <code>git branch -M main</code> to rename it main, which is now standard convention. This will make tracking the remote main branch on GitHub consistent with names. Learn more about git subcommands [here](https://comp423-25s.github.io/resources/git/ch2-git-fundamental-subcommands/).

3. Now we will need to push our changes to the remote branch, and establish a tracking between the local and remote main branches:

    ```bash
    git push --set-upstream origin main
    ```

### Third Step: Setting Up Our Dev Container

All this talk about dev containers, and you still might not understand fully what one is. In summary:

A dev container is a lightweight, isolated environment used for software development. In our case we will be using Visual Studio Code's Dev Container extension in conjunction with Docker to setup our dev container within VS Code.

Why is this useful? Well, projects can become vast and complex pretty quickly, which introduce different dependencies, libraries, programming languages, and other tools that are specifically required to have a project function correctly. With dev containers, projects can be outsourced to different team members to quickly work in a consistent, bug free environment that also reduces onboarding and time doing manual setup to start coding within a project. 

All we need to is to define a configuration file for VS Code's Dev Container extension to read and create a container with the Docker software application. With that, let's get started!

!!! warning
    If you do not have Docker installed and tested, please refer back to the prerequisities at the top of this page.

1. Open VS Code and open your rust-dev-container folder to start development.

2. Install the Dev Containers extension that will be used to read our configuration file we will make shortly.

3. Create a folder called **.devcontainer** and insert a file called **devcontainer.json**. This json file will be used to hold information for the VS Code extension to tell Docker what will be needed to run our Rust program.

Within this configuration file we will need to define a few things:

<code>name</code>: Which is the name of our container. It is good to make this descriptive.
<code>image</code>: Image refers to the Docker image that will be used to run the dependencies that are needed for this project. We will need a base image that will compile our Rust code to run.
<code>customizations</code>: This specifier allows extensions to automatically be downloaded within the dev container. We will need the rust-analyzer extension installed.

