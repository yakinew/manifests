# Using Google git-repo to manage muliple repositories
Try to work with google repo. See: https://gerrit.googlesource.com/git-repo/

## Install Google git-repo
For **Linux Ubuntu**, use this command:
```
sudo apt-get install repo
```

For **macOS**, use this command:
```
brew install repo
```

## Set a GitHub token
1. Generate a token in GitHub web (https://github.com/settings/tokens/)
2. Save the token in a text file. Make sure no one has access to this file.
3. Create the file ~/.netrc as following:
```
machine github.com
login <username>
password <token>
```
For example:
```
machine github.com
login yakinew
password ghp_oAPV********************************
```



## Python Certificate
Make sure your python uses certificate. If you see the following error, you have a certificate issue.
```
fatal: Cannot get https://gerrit.googlesource.com/git-repo/clone.bundle
fatal: error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1007)
fatal: double check your --repo-rev setting.
fatal: cloning the git-repo repository failed, will remove '.repo/repo'
```

In this case, run the following commands:
```
CERT_PATH=$(python3 -m certifi)
export SSL_CERT_FILE=${CERT_PATH}
export REQUESTS_CA_BUNDLE=${CERT_PATH}
```

## Create and init the workspace
Create a new directory and get into it.
```
% mkdir workspace
% cd workspace
```
Run the init command:
```
repo init -u https://github.com/yakinew/manifests -m main.xml
```
`-u` for the remote repository URL.

`-m` for the manifest file in this repository.

If the init finished OK, you should see a message like `repo has been initialized in <YOUR-WORKSPACE_DIRECTORY>`. In the workspace directory you should see a new direcroty `.repo` with the manifests files. 

Now you are ready to run:
```
repo sync
```
You can run multiple jobs at the same time:
```
repo sync -j 5
```

If everything goes right, you should see something like this:
```
Fetching: 100% (3/3), done in 2.623s
Checking out: 100% (3/3), done in 0.171s
repo sync has finished successfully.
```
Also you should see all the projects in the workspace directory:
```
yaakovneuman@Mac workspace % ll
total 0
drwxr-xr-x@  6 yaakovneuman  staff   192 ינו 18 23:51 .
drwxr-x---+ 47 yaakovneuman  staff  1504 ינו 18 23:51 ..
drwxr-xr-x@ 13 yaakovneuman  staff   416 ינו 18 23:51 .repo
drwxr-xr-x@  6 yaakovneuman  staff   192 ינו 18 23:51 astronaut
drwxr-xr-x@  9 yaakovneuman  staff   288 ינו 18 23:51 ctetris
drwxr-xr-x@ 22 yaakovneuman  staff   704 ינו 18 23:51 mobileye
yaakovneuman@Mac workspace %
```

# Working with manifest
You can create a new manifest with other branches than `main` (see my `stage.xml`).
To use other manifest (after you already init your workspace), run the following commands:
```
repo sync               # To get the new manifest from the remote repository.
repo init -m stage.xml  # To set you repo to work with this manifest.
repo sync               # To sync your workspace with manifest stage.xml
```

## Freeze
When you want to freeze the current state of all the projects (repositories), you can run the following command:
```
repo manifest -r -o manifests/freeze_2024-05-15-08-31.xml
```
`-r` to generate a manifest based on the currently checked-out revisions of your projects.

`-o` to save the manifest to a file. Otherwise it will be printed to screen.

After you update this new file in the repository `manifests`, other users can get this file and sync their workspace accordingly:
```
repo init -m freeze_2024-05-15-08-31.xml
repo sync
```
