# How To Work With Multiple Github Accounts on a single Machine

<https://gist.github.com/rahularity/86da20fe3858e6b311de068201d279e3>

Let suppose I have two github accounts,
[**https:/**](https:/){.uri}**/github.com/seifriz** and
[**https:/**](https:/){.uri}**/github.com/fs1967**. Now i want to setup
my pc to easily talk to both the github accounts.

> NOTE: This logic can be extended to more than two accounts also. :)

The setup can be done in 5 easy steps: \## Steps: - [Step 1](#step-1) :
Create SSH keys for all accounts - [Step 2](#step-2) : Add SSH keys to
SSH Agent - [Step 3](#step-3) : Add SSH public key to the Github - [Step
4](#step-4) : Create a Config File and Make Host Entries - [Step
5](#step-5) : Cloning GitHub repositories using different accounts

<br>

## Step 1 {#step-1}

### Create SSH keys for all accounts

First make sure your current directory is your **.ssh** folder. Open the
Git-Bash:

``` sh
     $ cd ~/.ssh
```

Syntax for generating unique ssh key for an account is:

``` sh
     ssh-keygen -t rsa -C "your-email-address" -f "github-username"
```

here,

**-C** stands for comment to help identify your ssh key

**-f** stands for the file name where your ssh key get saved

#### Now generating SSH keys for my two accounts

``` sh
     ssh-keygen -t rsa -C "seifriz@dshs-koeln.de" -f "github-seifriz"
     ssh-keygen -t rsa -C "f.seifriz@flosoft.de" -f "github-fs1967"
```

Notice here **seifriz** and **fs1967** are the username of my github
accounts corresponding to
[**seifriz\@dshs-koeln.de**](mailto:seifriz@dshs-koeln.de){.email} and
[**f.seifriz\@f.flosoft.de**](mailto:f.seifriz@f.flosoft.de){.email}
email ids respectively.

After entering the command the terminal will ask for passphrase, leave
it empty and proceed.

![Passphrase Image](images/ssh%20keygen-01.png)

> Now after adding keys, in your .ssh folder, a public key and a private
> will get generated.

> The public key will have an extention **.pub** and private key will be
> there without any extention both having same name which you have
> passed after **-f** option in the above command. (in my case
> **github-seifriz** and **github-fs1967**)

![Added Key Image](images/ssh%20keygen-02.png)

<br>

## Step 2 {#step-2}

### Add SSH keys to SSH Agent

If the SSH Agent is not started

``` sh
     eval $(ssh-agent -s)
```

Now we have the keys but it cannot be used until we add them to the SSH
Agent.

``` sh
     #ssh-add -K ~/.ssh/github-seifriz
     #ssh-add -K ~/.ssh/github-fs1967
     
     ssh-add ~/.ssh/github-seifriz
     ssh-add ~/.ssh/github-fs1967
```

You can read more about adding keys to SSH Agent
[here.](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

<br>

## Step 3 {#step-3}

### Add SSH public key to the Github

For the next step we need to add our public key (that we have generated
in our previous step) and add it to corresponding github accounts.

For doing this we need to paste the public key on Github:

-   Sign in to Github Account
-   Goto **Settings** \> **SSH and GPG keys** \> **New SSH Key**
-   Paste your copied public key and give it a Title of your choice.

<br>

## Step 4 {#step-4}

### Create a Config File and Make Host Entries

The **\~/.ssh/config** file allows us specify many config options for
SSH.

If **config** file not already exists then create one (make sure you are
in **\~/.ssh** directory)

Now we need to add these lines to the file, each block corresponding to
each account we created earlier.

``` config
     # seifriz@dshs-koeln.de - GitHub account: seifriz
     Host github.com-seifriz
          HostName github.com
          User git
          IdentityFile ~/.ssh/github-seifriz

     # f.seifriz@flosoft.de - GitHub account: fs1967
     Host github.com-fs1967
          HostName github.com
          User git
          IdentityFile ~/.ssh/github-fs1967
```

<br>

## Step 5 {#step-5}

### Cloning GitHub repositories using different accounts

So we are done with our setups and now its time to see it in action. We
will clone a repository using one of the account we have added.

Make a new project folder where you want to clone your repository and go
to that directory from your terminal.

For Example: I am making a repository on my personal github account and
naming it **TestRepo** Now for cloning the repo use the below command:
\`\`\`git git clone
[git\@github.com](mailto:git@github.com){.email}-{your-username}:{owner-user-name}/{the-repo-name}.git

     [e.g.] git clone git@github.com-fs1967:fs1967/TestRepo.git


    <br>

    ## Finally

    From now on, to ensure that our commits and pushes from each repository on the system uses the correct GitHub user — we will have to configure **user.email** and **user.name** in every repository freshly cloned or existing before.

    ### Die config-Datei in jedem Repository ändern!!!!!

    To do this use the following commands.

    ```git
        git config user.email "seifriz@dshs.de"
        git config user.name "seifriz"
        
        git config user.email "f.seifriz@flosoft.de"
        git config user.name "fs1967"

Pick the correct pair for your repository accordingly.

To push or pull to the correct account we need to add the remote origin
to the project

``` git
     git remote add origin git@github.com-seifriz:seifriz
     
     git remote add origin git@github.com-fs1967:fs1967
```

Now you can use:

``` git
     git push
     
     git pull
```
