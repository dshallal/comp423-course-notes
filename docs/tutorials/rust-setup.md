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

<code>customizations</code>: This specifier allows extensions and other configurations to automatically be downloaded within the dev container. We will need the rust-analyzer extension installed.  

<code>postCreatecommand</code>: These are the commands we will need to run after our container is created to make sure our Rust is up to date and that we have a recent version aftwards.

Your devcontainer.json file should now have this:

```json
    {
  "name": "dev container for rust. HELLO COMP423",
  "image": "mcr.microsoft.com/devcontainers/rust:1-1-bullseye",
  "postCreateCommand": "rustup update && rustc --version",
  "customizations": {
    "vscode": {
      "extensions": [
        "rust-lang.rust-analyzer"
      ]
    }
  }
}
```

Great! Now all we have to do is have VS Code read our configuration file to download our Rust image from Microsoft which will reopen our directory as a development container with everything needed to run rust.

4. Hit CTRL + Shift + P on Windows (or CMD + Shift + P on Mac) and run "Dev Containers: Reopen in Container".

Afterwards you should see in your terminal something similar to this:

```bash
Running the postCreateCommand from devcontainer.json...

[5329 ms] Start: Run in container: /bin/sh -c rustup update && rustc --version
info: no updatable toolchains installed
info: checking for self-update
info: cleaning up downloads & tmp directories
rustc 1.83.0 (90b35a623 2024-11-26)

What's next:
    Try Docker Debug for seamless, persistent debugging tools in any container o
r image → docker debug 4bc6325493dc8cdfa23e7f2b83243ee6720366d1a63428513577abaec
a72d958
    Learn more at https://docs.docker.com/go/debug-cli/
Done. Press any key to close the terminal.
```
The Rust version  should be <code>rustc 1.83.0</code> or higher (depending on when you are following this tutorial).

**Congratulations! You are now ready to create a standard Rust project and create some really cool code!**

## Building a Rust Project for Hello COMP423

1. In your terminal run:

```bash
cargo new hello_world --vcs none
```
This will tell Rust to make a new Rust project with the necessary file structure. <code>--vcs none</code> makes sure a new git repository isn't initialized in that directory.

Now you should see a directory called hello_world, with a src folder and Cargo.toml that is used to define dependencies for the Rust project. Under src, you will see main.rs that has a hello world program that you will be able to run! Let's change what it prints out now to:

```rust
fn main() {
    println!("Hello COMP423");
}
```

Now you will need to go into the hello_world directory. In your terminal, run <code>cd hello_world</code>.

Afterwards, run <code>cargo build</code>, which is similar to how C uses <code>gcc</code> to compile C into binary. Both will utilize their respective compilers to turn their code files into binary executables a computer can execute.

Now there should be a directory called "target" within your Rust project that will hold your built executable.
To run it, we have to describe the file path along with <code>./</code> to tell our computer to run the binary exectuable. This is <code>./target/debug/hello_world</code>.

You should now see <code>Hello COMP423</code> to the stdout. Hooray!!!!

We can do all of these steps actually in **one**. To do this we can run, <code> cargo run </code> which does *both* compile, create the executable, and run all in one command. So, <code>cargo build</code> actually creates the binary executable, while <code>cargo run</code> does so and runs the executable.

# **Conclusion**
Make sure to commit and push your changes to your remote repository so that you can always come back and write some more Rust code! You have learned from 0 to 100 how to create a Rust project within a development container, all while using version control. Congratulations again!