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
4. Add the public key to your Bitbucket settings. From Bitbucket, choose Bitbucket settings from your avatar in the lower left. The Account settings page opens. Click SSH Keys:

   On Linux, you can cat the contents:

   ``` bash
   cat ~/.ssh/id_rsa.pub
   ```

   On macOS, the following command copies the output to the clipboard:

   ``` bash
   pbcopy < ~/.ssh/id_rsa.pub
   ```
5. Select and copy the key output in the clipboard. Paste in Bitbucket client and click save.
6. Return to the terminal window and verify your configuration and username by entering the following command:

   ``` bash
   ssh -T git@bitbucket.org
   ```