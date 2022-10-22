# Setup a webserver using NGINX

This file has instructions to set up a web server on the Google Cloud Platform (https://cloud.google.com). If you are using the *Free Tier* on Google cloud, check this link (https://cloud.google.com/free/docs/gcp-free-tier#free-tier-usage-limits) for details on the virtual machine that you set up. The following are settings that I used for a personal web server. I assume you are a total novice to this subject.


## Setting Up Virtual Machine

1. Go to https://console.cloud.google.com and login with a Google account.
2. Register for Google cloud and enter your payment details (**You will not be charged if you stay within the usage restrictions of the free tier**).
3. Once everything is set up, on the dashboard create a new project.
4. Go to the navigation menu &#8594; Compute Engine &#8594; VM instances and click on Create.
5. Select the necessary resources for the VM (**Check if you are choosing the correct settings if you are hoping to use the free tier**). The settings I use are as follows.

| Setting | Value |
----------|-------
| Region | *us-west1* |
| Zone | Any available zone |
| Machine family | *General-purpose* |
| Series | *E2* |
| Machine type | *e2-micro (2 vCPU, 1 GB memory)* |
| Boot disk | Select the OS and/or distro you are comfortable with.'*' |
| Boot disk type | *Standard persistent disk* |
| Size (GB) | *30* |
| Firewall | *Allow HTTP traffic* and *Allow HTTPS traffic* |
| Other settings | Left on defaults. |

'*' A minimal Linux distro is recommended due to the low resources available if you are using the free tier.

6. After the VM is initialized, you can use *SSH* to interact with the VM. I use the web SSH terminal. (See https://cloud.google.com/compute/docs/instances/connecting-advanced#thirdpartytools for instructions in using other SSH clients. You can also use *gcloud* https://cloud.google.com/sdk/gcloud). 
7. Also note the public IP address so we can connect to the website.

**Steps after this depends on the OS/distro that you selected. I used *Ubuntu 20.04 LTS (minimal)*.**

8. *(Optional)* Updating package lists and packages before starting can alleviate some issues of using older packages.
 ```bash
sudo apt upgrade && sudo apt upgrade -y
```

## Create a SWAP File

Since the memory in the VM is small, it is a good idea to create a swap file.

1. Update the package lists (unless already done in step 7 in creating the virtual machine).
```bash
sudo apt upgrade
```
2. Install a text editor of your choice. I used *nano*.
```bash
sudo apt install nano
```
3. Create the `swapfile`. I will create a swap of 1GB.
```bash
sudo fallocate -l 1G /swapfile
sudo dd if=/dev/zero of=/swapfile bs=1024 count=1048576
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```
4. Add the line `/swapfile swap swap defaults 0 0` at the end of the file `/etc/fstab`. **Remember to save and close.**
```bash
sudo nano /etc/fstab
```
5. Mount the swapfile.
```bash
sudo mount -a
```
6. Running `htop` should show you the swap file.


## Install NGINX

To access the website from the Internet, we have to install a web server in the created virtual machine. I'm using [NGINX](https://www.nginx.com/resources/wiki/)

1. Update the package lists (if didn't do so already).
```bash
sudo apt upgrade
```
2. Install NGINX (or any other web server of choice).
```bash
sudo apt install nginx
```

**When you go to the public IP of the virtual machine, you should be able to see a welcome page meaning that the web server is working properly.**


## Install Certbot

For the website to work over a secure connection (HTTPS), we need to install a certificate. [Let's Encrypt](https://letsencrypt.org/) is a free, automated, and open certificate authority.  [Visit the Certbot](https://certbot.eff.org/) site to get customized instructions for your operating system and web server.