# Pushing a git repo to a samba share

### Background

It is recommended not to store git repos on samba shares due to performance
issues and risk of repository corruption.  I also experienced significant
performance issues when adding additinal modules in the uv python virtual
environment due to the fact that hard links could not be utilised between samba
share and local drive.  The steps are as below.


### 1. Create a bare repo on the samba directory

Create and navigate to the target directory and create the bare repo:<br>
`git init --bare`

A bare Git repository is a project database that stores version history and
metadata without a working directory for editing files.  The code files are not
directly visible in the bare repo, but are stored as internal database objects.
Code can be cloned from this bare repo, or contents can be explored without a
running a full clone.

### 2. Pushing to the bare repo

The bare repo can now be added as a remote in the actual repo with something
like this: <br>
`git remote add samba /mnt/samba/backups/my-project.git`

We can now push to this remote samba repo with below: <br>
`git push samba main`


### 3. Automating pushing to the bare repo at the same time as pushing to Github


### 4. Inspecting the contents of the bare repo and performing a full clone
