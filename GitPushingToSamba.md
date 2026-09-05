# Pushing a git repo to a samba share

### Background

It is recommended not to store git repos on samba shares due to performance
issues and risk of repository corruption.  I also experienced significant
performance issues when adding additional modules in the uv python virtual
environment due to the fact that hard links could not be utilised between samba
share and local drive.

When creating a git repo on a local C drive, I like to have an additional backup
over and above pushing to Github or other cloud repo.   I do this via pushing to
a bare repo on a local samba server.  The steps are as below.


### 1. Create a bare repo on the samba directory

Create and navigate to the target directory The convention is to have a directory
name with a .git suffix Create the bare repo with the below command:<br>
`git init --bare`

A bare Git repository is a project database that stores version history and
metadata without a working directory for editing files.  The code files are not
directly visible in the bare repo, but are stored as internal database objects.
Code can be cloned from this bare repo, or contents can be explored without a
running a full clone.

### 2. Pushing to the bare repo

Navigate to the standard git repo on the C drive.
In addition to a Github remote for example, the bare repo can now be added as a
remote in the actual repo with something like this: <br>
`git remote add samba /mnt/samba/backups/my-project.git`

We can now push to this remote samba repo with below: <br>
`git push samba main`


### 3. Automating pushing to the bare repo at the same time as pushing to Github

Assuming that a remote called origin has already been created on for example
Github, add a second remote to target path on samba drive as below: <br>
`git remote add samba /mnt/samba/backups/my-repo.git`


Add the below to .basrc to push both to the remote repo named origin and the
samba repo at the same time with pushall.  Pushall accepts exaclty the same
parameters as push as the "@" passes all arguments directly to git push.

````bash
pushall() {
    echo "==> Pushing to GitHub..."
    git push origin "$@"

    # Check if Samba share is accessible
    if mountpoint -q /mnt/samba || [ -d "/mnt/samba/backups" ]; then
        echo "==> Samba available. Syncing to backup..."
        git push samba "$@"
    else
        echo "==> Notice: Samba share unavailable. Skipping local backup."
    fi
}
````

### 4. Inspecting the contents of the bare repo and performing a full clone

Note that the source code will not be directly visible in the bare repo.  Files
are compressed and stored as database objects.

Files in the bare repo can be viewed and restored via below methods:

List all files inside the latest commit on the samba share <br>
`git --git-dir /path/to/your/bare-repo.git ls-tree -r HEAD --name-only`

View the actual text content of test.py from the samba share<br>
`git --git-dir /path/to/your/bare-repo.git show HEAD:test.py`

Alternatively, navigate to the git repo and run the above commands without the
--git-dir option

Perform a full clone
`git clone /mnt/samba/backups/my-project.git /tmp/restored-project`:w

