# flaws.cloud Walkthrough

# About flaws.cloud

Flaws.cloud is a series of challenges comprising six levels, each focusing on common misconfigurations in the AWS S3 service. This walkthrough provides a detailed guide for navigating these challenges, facilitating understanding of the misconfigurations and offering necessary recommendations for the proper setup of S3 buckets.

# Level 1

**This level is *buckets* of fun. See if you can find the first sub-domain.**

From the challenge we can note that we need to find a subdomain which would lead us to the next challenge.

We can start by using the dig, nslookup or host commands to perform a dns lookup.

We will be utilizing the host command to perform a DNS lookup on the website. 

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# host flaws.cloud
flaws.cloud has address 52.218.237.146
flaws.cloud has address 52.92.237.251
flaws.cloud has address 52.92.197.147
flaws.cloud has address 52.92.145.195
flaws.cloud has address 52.92.204.83
flaws.cloud has address 52.92.179.11
flaws.cloud has address 52.92.176.171
flaws.cloud has address 52.92.204.203
```

Our output provides multiple server IP addresses we can further enumerate them by performing a reverse IP lookup on one of the address.

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# host 52.218.237.146
146.237.218.52.in-addr.arpa domain name pointer s3-website-us-west-2.amazonaws.com.
```

The output indicates that the page we are currently viewing is hosted on a AWS S3 bucket. Upon further research regarding the url of the s3 bucket is noted as follows “http://[bucket_name].s3.amazonaws.com/”.

Now we can list the contents of the s3 bucket using aws cli which can be installed on our Kali Machine.

```bash
┌──(root㉿kali)-[/home/kali/Desktop/flaws.cloud]
└─# aws s3 ls s3://flaws.cloud --no-sign-request
2017-03-13 23:00:38       2575 hint1.html
2017-03-02 23:05:17       1707 hint2.html
2017-03-02 23:05:11       1101 hint3.html
2020-05-22 14:16:45       3162 index.html
2018-07-10 12:47:16      15979 logo.png
2017-02-26 20:59:28         46 robots.txt
2017-02-26 20:59:30       1051 secret-dd02c7c.html
```

We can see multiple domains but we will take a look at the secret-dd02c7c.html page. We can proceed to use the URL [http://flaws.cloud/secret-dd02c7c.html](http://flaws.cloud/secret-dd02c7c.html) which leads us to the URL [http://level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud](http://level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud/) for Level -  2.

# References

[1] [https://bluexp.netapp.com/blog/aws-cvo-blg-amazon-s3-buckets-finding-open-buckets-with-grayhat-warfare#:~:text=An S3 bucket can be,.s3.amazonaws.com%2F](https://bluexp.netapp.com/blog/aws-cvo-blg-amazon-s3-buckets-finding-open-buckets-with-grayhat-warfare#:~:text=An%20S3%20bucket%20can%20be,.s3.amazonaws.com%2F)

[2] [https://www.bluematador.com/learn/aws-cli-cheatsheet](https://www.bluematador.com/learn/aws-cli-cheatsheet)
