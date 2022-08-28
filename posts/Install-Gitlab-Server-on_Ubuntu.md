---
title: 'Gitlab Server Installation on Ubuntu'
date: 'August 28, 2022'
excerpt: 'Gitlab is great Remote plaform for souce code storage and management'
cover_image: '/images/posts/gitlab-logo-gray-rgb.png'
---

# Install gitlab in Ubuntu. 

I using Digital Ocean for hosting the ubuntu remote machine. 

## Step 1 — Installing the Dependencies

```sh
sudo apt-get update

# install package use by gitlab.
sudo apt-get install -y curl openssh-server ca-certificates

# install postfix to sent notification email
sudo apt-get install -y postfix

```
## Step 2 — Installing GitLab. <br>

Now that the dependencies are in place, we can install GitLab itself. This is a straightforward process that leverages an installation script to configure your system with the GitLab repositories.

```
sudo apt-get update

# install package use by gitlab.
sudo apt-get install -y curl openssh-server ca-certificates

# install postfix to sent notification email
sudo apt-get install -y postfix

# use curl to dowload the script which use to install gitlab. 
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash  

# this command to install gitlab package and assign dns domain. 
# please note that this might won't work with ubuntu 20. version. 
sudo apt-get install gitlab-ee

# another solution for above problems.
# check for update and find the package name exist or not in cache. 
sudo apt-get update
sudo apt-cache search gitlab-ee

# dowload new pakage which supported for ubuntu 20. version
curl -L -o gitlab-ee_13.0.6.deb https://packages.gitlab.com/gitlab/gitlab-ee/packages/debian/buster/gitlab-ee_13.0.6-ee.0_amd64.deb/download.deb

sudo apt install ./gitlab-ee_13.0.6.deb 
```
## Step 3 — Adjusting the Firewall Rules. <br>

Before you configure GitLab, you will need to ensure that your firewall rules are permissive enough to allow web traffic. If you followed the guide linked in the prerequisites, you will have a ufw firewall enabled.

```
sudo ufw status
# if firewall was inactive please enable it by using 
sudo ufw enable

# allow trafic from outside by http, https, ssh protocol. 
sudo ufw allow http
sudo ufw allow https
sudo ufw allow OpenSSH
```
![image](https://user-images.githubusercontent.com/34083808/187014738-034b374e-ff77-42a5-84a1-42b8ed8d850b.png)

The above output indicates that the GitLab web interface will be accessible once we configure the application.

### Step 4 — Editing the GitLab Configuration File
Before you can use the application, you need to update the configuration file and run a reconfiguration command. First, open Gitlab’s configuration file: <br>
```
sudo nano /etc/gitlab/gitlab.rb
```
Near the top of the file please edit the ```external_url`` configuration line to your domain or sub domain. in my case I change to my subdomain which is https://gitlab.soraking.cloud. 
```
external_url 'https://gitlab.soraking.cloud'
```

Next, look for the letsencrypt['contact_emails'] setting. This setting defines a list of email addresses that the Let’s Encrypt project can use to contact you if there are problems with your domain. It’s a good idea to uncomment and fill this out so that you will know of any issues.

```
letsencrypt['contact_emails'] = ['vunguyenthanhbinh92@gmail.com']
```

Save and close the file. Run the following command to reconfigure Gitlab:
```
sudo gitlab-ctl reconfigure
```

> some useful command
```
# stop gitlab ctl
gitlab-ctl stop

# change config file
vi /etc/gitlab/gitlab.rb
# reconfig file
gitlab-ctl reconfigure
# restart 
gitlab-ctl restart

# start gitlab
gitlab-ctl start
```

> please note that gitlab require at least 2GB RAM + 2GB swap memory. In my case, at the first time I use virtual host with 2GB ram, but it seems not enough. So I changed to virtual host with 4GB ram and it works.

you will see this page after completed installation at https://gitlab.soraking.cloud<br>

<img src="https://user-images.githubusercontent.com/34083808/187015425-dd67c433-034b-40ed-9489-1759aa306d39.png" alt="drawing" width="800`"/>

📗Ref: [Installation Guide on Degital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-gitlab-on-ubuntu-18-04)
