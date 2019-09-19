---
layout: post
title: RStudio Server on Amazon Web Services
subtitle: Short guide on how to setup an AWS with RStudio installed
gh-repo: msmith01
gh-badge: [follow]
tags: [AWS, RStudio]
comments: true
---

Note: Skip to **Step 5** if you want the "quick fix".

I will briefly document how I setup RStudio server on an Amazon AWS cloud instance. Once you have an AWS account you should be able to follow these instructions to set everything up. I suggest you follow some tutorial such as [here](https://www.guru99.com/creating-amazon-ec2-instance.html) in order to setup the Amazon ec2 instance, it's not difficult but it is a separate topic. I am using the "Ubuntu Server 18.04 LTS (HVM), SSD Volume Type" on a "free eligible tier t2.micro" - The free version has 1GB of memory which is fine for getting to know the environment/Amazon AWS but I strongly recomend investing in a paid instance once your are familiar.

Once the instance is setup, you might want to follow these instructions [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) to set up PuTTY (for Windows users) and obtain a .pem key. Once we are logged into PuTTY we can run the following commands:

![yunohurry](https://raw.githubusercontent.com/msmith01/msmith01.github.io/master/_posts/posts_img/y_u_no_hurry_up.jpg?style=centerme)


**Step 1:** Install R
~~~
sudo apt update
sudo apt-get install -y r-base r-base-dev
~~~

**Step 2:**  Download and isntall RStudio

Download the latest RStudio version from [here](https://www.rstudio.com/products/rstudio/download/#download) and replace the link below.
~~~
sudo apt install gdebi-core
wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-1.2.1335-amd64.deb
sudo gdebi rstudio-server-1.2.1335-amd64.deb
~~~

**Step 3:** We can add a user to the system to log into RStudio server

~~~
sudo chmod 777 -R /usr/local/lib/R/site-library
sudo adduser YOURUSERNAME
~~~

That should be it. I was using this code a little while back. I will update the post when I install an AWS instance again in the coming months.

**Step 4:** Updating R to later versions

~~~
echo "deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/" | sudo tee -a /etc/apt/sources.list
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo apt-get update
sudo apt-get upgrade -y
~~~

**Step 5:** Quick version

You can install RStudio directly as an Amazon AWS instance by using Louis Aslett's RStudio Amazon Machine Image (AMI) [here](http://www.louisaslett.com/RStudio_AMI/). There is also a short video [here](http://www.louisaslett.com/RStudio_AMI/video_guide.html) on how you can set it up painlessly!

I recommend Louis Aslett's AMI especially if you are not familiar with Bash or Shell commands however I was running into some compatibility problems with package versions and R versions which is why I sought a way to manually install R and have full control.

Below are a fe additional commands, mostly for myself since I kept all these commands in a notepad and there was a reason they were kept!

**Step 6:** Upgrade R

~~~
echo "deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/" | sudo tee -a /etc/apt/sources.list
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install -y r-base r-recommended r-base-dev
~~~

**Step 7:** Change password for the root user

~~~
sudo passwd root
su root
~~~


**Step 8:** Additional

~~~
apt-get -y build-dep libcurl4-gnutls-dev
~~~

