---
title: How To Setup Ruby and Rails
date: 2023-07-19 12:00:00 -500
categories: [Tutorials,Ruby]
tags: [alperenkocyigit,ruby,rails,howto]
---
# How to Setup Ruby and Rails
In this tutorial you can download `Ruby 3.2.2` with terminal.

> This tutorial is ony for lastest version of ruby and rails. If you need to setup older versions. You can use `sudo apt install rbenv` command.
{: .prompt-warning }

1. Start by updating the system repositories:

    ```
    sudo apt update
    ```

2. Download and install the libraries and compilers Ruby needs to run:

    ```
    sudo apt install git curl autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev
    ```

3. Type Y and press Enter to confirm the installation.

4. Download and run the shell script used to install Rbenv:
    ```
    curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash
    ```

5. You need to add $HOME/.rbenv/bin to your PATH environment variable to start using Rbenv.

    - If you are using the Bash shell, use this command:
        ```
        echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
        echo 'eval "$(rbenv init -)"' >> ~/.bashrc
        source ~/.bashrc
        ```
    - If you are using the Zsh shell, use this command:
        ```
        echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
        echo 'eval "$(rbenv init -)"' >> ~/.zshrc
        source ~/.zshrc
        ```
6. Verify the installation by checking the version of Rbenv:

    ```
    rbenv -v
    ```

    You can check the outputs of `rbenv -v` , `ruby -v` , `rbenv local` , `rbenv global` 

    ![Version Outputs](https://media.discordapp.net/attachments/1129424133143412756/1129775958975586364/Screenshot_from_2023-07-15_17-05-38.png){: .left }


Now you successfully installed Ruby 3.2.2.
```python
print("Well Done!!")
```