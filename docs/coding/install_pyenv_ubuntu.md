# Install `pyenv` on Ubuntu

1. Start by running verifying that `curl` and `git` are installed:
    ``` bash
    sudo apt update && sudo apt upgrade -y git curl
    ```

2. Run the curl script:
    ``` bash
    curl https://pyenv.run | bash
    ```

3. Add the environment variables Into `bashrc` file
    ``` bash
    export PATH="$HOME/.pyenv/bin:$PATH" && eval "$(pyenv init --path)" && echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n eval "$(pyenv init -)"\nfi' >> ~/.bashrc
    ```

4. Restart the shell in Ubuntu:
    ``` bash
    exec $SHELL
    ```

5. Display the installed version of the pyenv:
    ``` bash
    pyenv --version
    ```

!!! tip
    Depending on how fresh your installation of ubuntu is, you might need to run the following command to install the required packages to be able to build the different versions of python on your system:

    ``` bash
    sudo apt install -y build-essential zlib1g-dev libffi-dev libssl-dev libbz2-dev libreadline-dev libsqlite3-dev liblzma-dev
    ```