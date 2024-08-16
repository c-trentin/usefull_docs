# Use SSH key to authenticate Git client from Bash Terminal

## Generate keys
[Source](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

Use the command below, replacing the email used in the example with your GitHub email address.

```bash
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```

When prompted, you can choose the filenames where keys will be generated, for example "/home/YOU/.ssh/id_git" will generate :
- /home/YOU/.ssh/id_git (the private key)
- /home/YOU/.ssh/id_git.pub (the public key)

Enter a passprhase is not mandatory but highly recommended.

## Copy the public key to Github

[Source](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

Copy your public key
```bash
$ cat ~/.ssh/id_git.pub
# Then select and copy the contents of the id_git.pub file
# displayed in the terminal to your clipboard
``` 

Click on your "profile photo" on Github and select "Settings", then in the "Access" section select "SSH and GPG Keys".
Create a new ssh key and past your public key here.


## Use ssh-agent

Launch ssh-agent in background in your terminal, and add your identity as following

```bash
$ eval $(ssh-agent -s)
$ ssh-add ~/.ssh/id_git
```

Now you won't be prompted anymore to enter credentials as long as you are using SSH with your Gihub account in this Linux Terminal.
Note that it is only available in the terminal where ssh-agent has been launched.

## Automate the process when a new terminal is opened

Add some lines add the end of your .bashrc
```bash
#Start ssh-agent for this terminal
eval $(ssh-agent -s) &>/dev/null
#Add identity to this ssh-agent
ssh-add ~/.ssh/git_id_ed25519 &>/dev/null
#Kill ssh-agent when exiting terminal
trap 'test -n "$SSH_AGENT_PID" && eval `/usr/bin/ssh-agent -k`' 0
```

