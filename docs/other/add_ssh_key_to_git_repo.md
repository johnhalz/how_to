# How to add SSH key to Git Repo

1. Set up your default identity. In terminal, write the command:
    ``` bash
    ssh-keygen
    ```

2. List the contents of `~/.ssh` to view the key files.
    ``` bash
    ls ~/.ssh
    ```

3. Add the key to the ssh-agent. To start the agent, run the following:
    ``` bash
    eval `ssh-agent`
    ```

4. Add the public key to your online git settings. Open the account settings page, then find and go to the SSH Keys settings. You will be prompted to add a key to your account.

5. Retreive the public key that you generated
    === "Linux"

        On Linux, you can cat the contents:

        ``` bash
        cat ~/.ssh/id_rsa.pub
        ```

    === "macOS"
        On macOS, the following command copies the output to the clipboard:

        ``` bash
        pbcopy < ~/.ssh/id_rsa.pub
        ```

5. Select and copy the key output in the clipboard. Paste in the browser and click save.
