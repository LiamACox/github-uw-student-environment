<h1>Contributing to a Repository</h1>

<h2>Pulling</h2>

Use `git pull` to download and apply the latest changes made on your current branch.

<h2>Commiting and Pushing</h2>

Use `git commit -m "commit message"` to record any changes made to your files. Be sure to write a short message explaining what you changed in quotation marks after the `-m`.

Git will only commit files that been added to the staging area. To stage a file, use `git add <file_name>` to add it to the staging area. If you made changes to a bunch of files, use `git add -A`. This command will identify all changed files and automatically stage them.

To see which files are currently in the staging area, use the command `git status`. This command will also show you changed files that have not yet been staged or commited. 

Once you are satisfied with your changes and have commited them, use the command `git push` to have Git publish your commits to the Github repository. This will allow other users to view and pull your changes down to their own local repositories. 

<h2>Branches</h2>

Branches are a way of keeping your work seperate from the main codebase until you are certain that you would like to merge them. For example, suppose you have the branch `main`. To create new a feature, you would create a branch `feature_branch` using the command `git branch feature_branch`. 

To switch branches, you use the command `git checkout <branch_name>`. Once you have created and switched to your new branch, you can begin making changes without having to worry about altering the contents of the `main` branch. When you are ready, you can push your changes to the Github repository using `git push` as normal. 

**Note:** if you are pushing your changes from a new branch for the first time, you must use the command `git push -u origin <branch_name>`. The `-u` flag sets the corresponding server side branch that the changes should be saved to. 

To see all of your available branches, use the command `git branch -a`.

**Note**: `git checkout -b <branch_name>` is a shortcut that creates a new branch and automatically switches to it.

<h3>Rebasing</h3>

Essentially, rebasing is changing where your branch first starts from the root branch. For example, suppose that you are working on a feature in your own branch and someone makes changes to the `main` branch. In order to have those changes propogate to your branch you can use the command `git checkout <your_branch>` followed by the command `git rebase main`. As long there are no merge conflicts, this should smoothly apply all of the changes to your current branch without disrupting any of your work. 

<h3>Optional: Configuring Git to Automatically Set Branch Upstream</h3>

When creating a new branch in Git, you normally have to manually set the corresponding upstream branch. To get around this, use the command `git config --add --bool push.autoSetupRemote true` while in the project directory. Once you run the command, you will no longer after to use `git push -u origin <branch_name>` during your first push. 

<h2>Pull Requests and Merging</h2>

Once you are ready for your commits to be added to the `main` branch, you can start the process of merging them by opening a pull request. A pull request is a way of signalling to other team members that your code is ready to be merged. Additionally, it allows them to view the changes you have made by comparing your the code in your branch and the code in `main` side by side. 

To open a pull request:
1) Push your up to date branch to the Github repository.
2) Open the **Pull Request** tab on the Github repository page.
3) Click the **New Pull Request** button.
4) Select the two branches you would like to merge.
5) Select the **Create Pull Request** button.

Github will then create your pull request and the other contributors of the project can now discuss your changes with comments and approve the merge. 

Once your branch has been merged, you can delete from your local repository with the command `git branch -d <branch_name>`. Oftentimes, the branch gets deleted from the remote repository during the pull request, but the command  `git push origin :branch_name` can also be used to delete it if you ran `git branch -d <branch_name>` locally. 

<h3>Pruning Stale Branches</h3>

In the case where a branch is deleted using the Github interface, it can still show up as available when you use the `git branch -a` command. To automatically get rid of any stale remote branches, you can use the command `git remote prune origin`.

<h1>Cloning A Repository</h1>

In order to get the project repository on your student server, run the command `git clone git@github.com:<github_username>/<repository_name>.git`.

Additionally, you may want to change your username and email in the repository config. When we first started using git, we changed the global username and email config to our UWaterloo ID and email. 

Use the commands `git config user.name "<your Github username>"` and `git config user.email yourgithubemail@provider.com` while in the project directory to locally override these settings.
 
<h1>Setting up Github on the Student Server</h1>

If you have used Git for external projects or have created SSH keys on the student server before, run the command `ls -al ~/.ssh`. If the output includes a file called `id_ed25519.pub` skip to **Step 6)** using the already existing key. 

1) Create an SSH key using `ssh-keygen -t ed25519 -C "your_email@example.com"`. Use the same email as your Github account.
2) When prompted for the location of the key, leave it untouched. It should look something like `/u0/WATIAM/.ssh/id_ed25519`.
3) When prompted for a password, be sure to create a strong one as SSH keys are something you absolutely want to keep secure.
4) Run the following command: `eval "$(ssh-agent -s)"`. It should output the process ID of the ssh-agent program.
5) Add your newly created private key to the ssh-agent using `ssh-add ~/.ssh/id_ed25519`.
6) Get the contents of your new public key using `cat ~/.ssh/id_ed25519.pub`. Copy the outputted line to your clipboard.
7) In the upper-right corner of any page on GitHub, click your profile photo, then click **Settings**.
8) In the **"Access"** section of the sidebar, click **SSH and GPG keys**.
9) Click **New SSH key** or **Add SSH key**.
10) In the **Title** field, add a descriptive label for the new key, for example, *UW Student Environment*.
11) Select `Authentication Key` for the **Key Type** field.
12) Paste the results of `cat ~/.ssh/id_ed25519.pub` into the **Key** field.
13) Click **Add SSH Key**
14) Test SSH with `ssh -T git@github.com`

<h2>Optional: ~/.ssh/config</h2>

If you already have multiple SSH key pairs / profiles or anticipate having more in the future, consider adding the lines

```
Host github.com
  HostName github.com
  user git
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes
```

to your `~/.ssh/config` file. This way, your SSH agent will know which keys to use for which hosts and won't time out trying irrelavent keys. 
