---
title: Hosting a Ghost instance on AWS EC2
subtitle: A quick tutorial on how this blog is being hosted
tags: web aws
---

# A Ghost in the shell

After a bit of browsing where I revisited Hugo and a few other systems, I stumbled upon [Ghost](https://ghost.org/) and decided to give it a try. Ghost is an open-source publishing platform written in JavaScript (Node.js) with built-in newsletter and subscription functionalities. It also boasts a powerful online editor to write posts directly from the browser. You can either host it on your own or use the paid managed hosting. Since I enjoy making my life more complicated, I thought I would setup an instance myself using a micro [AWS EC2](https://aws.amazon.com/pm/ec2).

The official Ghost documentation assumes you’re using MySQL as a database. After several installation attempts, I found that MySQL was consistently bringing my EC2 micro instance to its knees: the CPU would spike, memory would saturate and the instance would become unresponsive. From there, I could either run Ghost on a more powerful (but more costly) EC2 instance type, or switch the default database for SQLite. Because the static website was to be served by CloudFront, the database would never be a bottleneck so I chose to replace MySQL with SQLite. As a bonus point, backups became trivial: all I had to do was to copy the whole Ghost folder including the SQLite file into an S3 bucket. And when I finally completed the installation and SSH'ed into the EC2, there it was, **the Ghost in the shell**.

# Installation

_This guide assumes some familiarity with the AWS ecosystem. As usual, I am not giving exact instructions since the AWS interface tends to change quickly, but the main principles should remain the same. One thing to remember: when creating resources in AWS, always ensure you're in the correct region (top-right corner of the screen)._

Start by creating a **Virtual Private Cloud**. The VPC provides essential networking infrastructure and ensures your EC2 instance runs in a secured, dedicated environment within AWS. Then set up a **Security Group**. The Security Group acts as a virtual firewall for the EC2 instance. Make sure the Security Group allows SSH (port 22), HTTP (port 80) and HTTPS (port 443) so the EC2 can be accessed from the web. Then go to the EC2 dashboard and launch a new **t2.micro** instance with an Ubuntu **Amazon Machine Image** (AMI). Attach the instance to the VPC and the Security Group. Generate a SSH key pair and download the `.pem` file. The instance is now accessible, but its IP will change every time it stops and starts. To prevent this, assign an **Elastic IP** to your EC2 instance. You can now connect to your instance using the SSH key generated earlier as well as the Elastic IP. Note that the default username is usually `ubuntu`:

```bash
ssh -i /path/to/your-key.pem ubuntu@your-elastic-ip
```

Following the official Ghost installation guide, install **Ghost** onto your EC2 instance but use SQLite instead of MySQL to avoid the resource overload described before. Register a custom domain name for your blog in **Route 53** and create a **Hosted Zone** with the necessary DNS records. In particular, create a **A record** pointing the domain to the Elastic IP of the EC2 instance. This will direct traffic from the domain to the blog. We now need to setup the DNS using **CloudFront**, but to do so we need an HTTPS certificate. Using **AWS Certificate Manager** (ACM), request a free SSL certificate for your domain and validate it via DNS using Route 53. This will enable HTTPS traffic to your blog. Now you can setup a CloudFront distribution of your glob using the EC2 Elastic IP as the origin, the ACM certificate for HTTPS and leave the default cache behavior.

Finally, update Ghost configuration by editing `config.production.json` to set your domain, enable HTTPS and configure SQLite as the database. You can now access your blog via `https://yourblog.com`. Optionally, you can setup a **CloudWatch** to monitor the EC2 performance and alert for high resource usage or downtime.

# Additionals scripts

I wanted to have a regular backup of the blog into an **S3** bucket. To do so, I created a custom bucket named `backup.myblog.com` and added the below script in `/var/local/`. This script copies the whole Ghost folder including the SQLite file into a tarball and copies it to the S3 bucket.

```bash
# backup.sh

date=$(date +"%Y-%m-%d")
filename=$date.backup.myblog.com.tar.gz
source=/var/www/myblog/content
local_dest=/var/backups
remote_dest=s3://backup.myblog.com
tar -czf $local_dest/$filename $source
aws s3 cp $local_dest/$filename $remote_dest
rm $local_dest/$filename
```

Ghost also needs some help to purge unused images (images that used to be inserted in a post but are not anymore following some editing). This feature is not available within Ghost so you have to rely on a third-party script named [ghost-purge-images](https://github.com/ghostboard/ghost-purge-images) and available as an **NPM package**. This script requires access keys that have to be generated from the Ghost interface. Get the API keys, install the NPM package and add the below script in `/var/local` as well:

```bash
# purge.sh

ghost-purge-images purge --content-key=XXX --admin-key=XXX
```

Now you should see both scripts in `/var/local/`:

```shell
cd /var/local
ls

backup.sh purge.sh
```

Finally, to automate the tasks, simply sudo into the crontab and add the lines below.

```shell
sudo crontab -u root -e
```

```shell
0 1 * * * /var/local/backup.sh
0 1 * * * /var/local/purge.sh
```