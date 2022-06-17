# Creating Virtual Environments in Python

## Introduction
When developing software with Python, a basic approach is to install Python on your machine, install all your required libraries via the terminal, write all your code in a single .py file or notebook, and run your Python program in the terminal.

This is a common approach for a lot of beginners and many people transitioning from working with Python for data analytics.

This works fine for simple Python scripting projects. But in complex software development projects, like building a Python library, an API, or software development kit, often you will be working with multiple files, multiple packages, and dependencies. As a result, you will need to isolate your Python development environment for that particular project.

To solve this problem, we can use virtual environments. This tutorial will cover everything on how to set one up with `pip` or `conda`.

=== "Pip"

    ### Installing `virtualenv` Tool
    Open your command prompt, enter the following command:

    ``` bash
    pip install virtualenv
    ```

    ### Creating Virtual Environment for your project
    To create a new virtual environment with a specific python version, type in the following command shown below. In the example below, we will create a new virtual environment called `env` in Python 3.8.
    ``` bash
    python3.8 -m venv env
    ```

    ### Configuring Git with Virtual Environments
    Creating a new virtual environment creates a new folder in your project. However this folder can contain a lot of data that is not universal and can clog up your git repo. To avoid this, add the following line to your `.gitignore` file:
    ```
    env/lib
    ```

    ### Activate Virtual Environment
    Enter the following command. As with the example above, our virtual environment is called `env`:
    ``` bash
    source env/bin/activate
    ```

    ### Deactivating/Exiting Virtual Environment
    You can simply type in the command to exit your virtual environment:
    ``` bash
    deactivate
    ```

=== "Conda"

    This part is still under construction!