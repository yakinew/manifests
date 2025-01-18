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
