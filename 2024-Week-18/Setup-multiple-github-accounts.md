# Setup multiple Github accounts

This guide is setup SSH key based on **folder**.

1. Create SSH Key.
2. Create ~/.gitconfig.pers file.
```
# ~/personal/.gitconfig.pers
 
[user]
  email = your_pers_email@example.com
  name = Your Name
 
#[github]
  #user = "mynickname"
 
[core]
  sshCommand = "ssh -i ~/.ssh/<personal_key>"
```

3. Edit ~/.gitconfig file.
```
# ~/.gitconfig
 
[includeIf "gitdir:~/personal/"] # include for all .git projects under personnal/ 
  path = ~/personal/.gitconfig-pers
 
#[includeIf "gitdir:~/work/"]
  #path = ~/work/.gitconfig-work
 
#[core]
  #excludesfile = ~/.gitignore 
```

4. Can also setup platform specific config.
```
[includeIf "gitdir:/Users"]
  path = ~/.gitconfig-macos
[includeIf "gitdir:C:"]
  path = ~/.gitconfig-windows
[includeIf "gitdir:/home"]
  path = ~/.gitconfig-linux
```
Source: https://blog.gitguardian.com/8-easy-steps-to-set-up-multiple-git-accounts/
