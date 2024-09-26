# Creating DigitalOcean Arch Linux Server using doctl

## In this tutorial you will:

1. Create SSH Keys on your local machine
2. Install and configure doctl
3. Create a cloud-init.yaml file
4. Create a droplet using doctl 

### Requirements before beginning 

- Create a DigitalOcean Account 
- Download the Arch Linux img file
  - Go to the [Arch Linux](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/) site and click on the most recent images package. Once inside, you want to download the most recent "cloudimg" file which ends in "qcow2" and is around 500MiB in size. You will need this image file for later.

![[Screenshot 2024-09-25 at 11.02.22 PM.png]]

## Step 1

### Create SSH keys on your local machine

Before creating our droplet we will need to create an SSH key to connect to our Digital Ocean droplet. We will generate an ed25519 key which has a newer, more secure, and better performing algorithm compared to older RSA algorithm.

To generate the ed25519 key we will type the command below in our terminal (on Linux/MacOS) or PowerShell (Windows). 

```
ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your email address"
```

The "do-key" stands for DigitalOcean key for easy reminder but you can change the name of the key if you wish.

This command will create 2 files in your .ssh directory 
- do-key (This is your private key)
- do-key.pub (This is your public key which you will use to connect to your droplet)

[!note!] On Windows you may have to create a .ssh folder in your home directory if you do not have one!

## Step 2

## Step 3

## Step 4

