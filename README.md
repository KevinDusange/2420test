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

![](Screenshot%202024-09-25%20at%2011.02.22%20PM%203.png)

## Step 1

### Create SSH keys on your local machine

Before creating your droplet you will need to create an SSH key to connect to our Digital Ocean droplet. We will generate an ed25519 key which has a newer, more secure, and better performing algorithm compared to older RSA algorithm.

To generate the ed25519 key type the command below in our terminal (on Linux/MacOS) or PowerShell (Windows).

```
ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your email address"
```

Also for Windows users, the tilde "~" may not work specifying your home directory so you may have to type the full path as stated below and replace the "your-user-name" with your Windows user name.

```
ssh-keygen -t ed25519 -f C:\Users\your-user-name\.ssh\do-key -C "youremail@email.com"
```

Entering a passphrase (Optional): After creating the SSH key you will be asked to enter a passphrase for the key. This passphrase will add an extra layer of security but is not mandatory for this installation. 
- To have a passphrase type your passphrase and press enter. It will ask you to type it again to confirm it.
- To have no passphrase leave it empty and press enter twice.

The "do-key" stands for DigitalOcean key for easy reminder but you can change the name of the key if you wish.

This command will create 2 files in your .ssh directory 
- do-key (This is your private key)
- do-key.pub (This is your public key which you will use to connect to your droplet)

>[!note!] 
On Windows you may have to create a .ssh folder in your home directory if you do not have one!

## Step 2

### Installing and configuring doctl

For this tutorial you will be using doctl to create your DigitalOcean droplet running Arch Linux. doctl is the official command line interface tool for DigitalOcean and will allow you to directly interact with the DigitalOcean API. Although more difficult, it offers greater flexibility for creating servers. 

You can download doctl using the command(s) below.

You can use Homebrew for MacOS, which is a package manager for MacOS, which can be downloaded using the command below.
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Once Homebrew is installed simply type the command below to download doctl for MacOS.
```
brew install doctl
```

Windows (PowerShell)
```
What command goes here for Windows?
```

Once installed you can verify by typing the command below to show which version of doctl you currently have.
```
doctl version
```

### Getting our API Key from DigitalOcean

Once you have doctl installed you will need to get get an API key from DigitalOcean which will act as secure "login" with the DigitalOcean API on doctl. 

1. Login to DigitalOcean and on the left side of the homepage click the "API" button and under the "Tokens" tab click on "Generate New Token"

![](Screenshot%202024-09-26%20at%201.09.58%20AM.png)

2. On the next screen name your token and for the expiration you can choose whichever option but for this tutorial we will do "No Expire". For the Scopes choose "Full Access" which will give us the options to read and write.

![](Screenshot%202024-09-26%20at%201.05.25%20AM.png)

3. Once you have generated your token it is very important to copy this token to a safe place immediately as it will not be shown again. We will need this token to get secure access to the DigitalOcean API on doctl.

![](Screenshot%202024-09-26%20at%201.05.53%20AM.png)

### Get access to your DigitalOcean account on doctl

On your operating system open your command line interface (Terminal or PowerShell) and type the command below. 

```
doctl auth init
```

It will prompt you to enter the Token. Enter the token which you just created in the step above. This will grant doctl access to your DigitalOcean account. You are now ready to create a droplet on DigitalOcean using doctl. Before that, we will need to create a yaml file for initial cloud settings as we will provide this file when creating the droplet. 
## Step 3

### Creating a cloud-init.yaml file

Cloud-init is an initialization tool that helps with the initial user setup and it will run during when during the boot of the Droplet. 



## Step 4

