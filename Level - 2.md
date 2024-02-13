# Level - 2

**The next level is fairly similar, with a slight twist. You're going to need your own AWS account for this. You just need the [free tier](https://aws.amazon.com/s/dm/optimization/server-side-test/free-tier/free_np/).**

The clue here is to use our AWS account. 

Similar to the first challenge I will be running the host command on the url of Level-2

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# host level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud 
level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud has address 52.92.190.235
level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud has address 52.92.162.219
level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud has address 52.92.202.155
level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud has address 52.218.132.50
level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud has address 52.92.133.107
level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud has address 52.92.205.43
level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud has address 52.92.186.211
level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud has address 52.92.227.163
```

We again note that this is still a static website hosted on the S3 bucket and we can try the aws cli command we tried earlier to list the subdomains

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# aws s3 ls s3://level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud --no-sign-request

An error occurred (AccessDenied) when calling the ListObjectsV2 operation: Access Denied
```

This time we encountered a error stating Access denied.

We checked by removing the —no-sign-request flag and trying to list the buckets

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# aws s3 ls s3://level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud                  

Unable to locate credentials. You can configure credentials by running "aws configure".
```

We can see that the command is requesting for credentials. We can proceed to create a AWS free tier account for this level and configure the credentials. Create a IAM user with Permissions “[AmazonS3FullAccess](https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-2#/policies/details/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonS3FullAccess)”  and generate access keys. Set the Access key into the aws cli.

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# aws configure                                                     
AWS Access Key ID [****************7QG6]:     
AWS Secret Access Key [****************IgWd]: 
Default region name [None]: 
Default output format [None]:
```

We can see that we have configured our aws cli with the access key.

Next we can now use the same command and note that we found “secret-e4443fc.html”.

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# aws s3 ls s3://level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud
2017-02-26 21:02:15      80751 everyone.png
2017-03-02 22:47:17       1433 hint1.html
2017-02-26 21:04:39       1035 hint2.html
2017-02-26 21:02:14       2786 index.html
2017-02-26 21:02:14         26 robots.txt
2017-02-26 21:02:15       1051 secret-e4443fc.html
```

We can proceed to our newly found domain and proceed to level-3 using the url [http://level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud/secret-e4443fc.html](http://level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud/secret-e4443fc.html).

![Level 2 Flag](/images/level-2.png)

# References

[1] [https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)
[2] [https://docs.aws.amazon.com/cli/latest/userguide/welcome-examples.html](https://docs.aws.amazon.com/cli/latest/userguide/welcome-examples.html)
