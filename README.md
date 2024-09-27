# Creating A DigitalOcean Arch Linux Server using doctl

## In this tutorial you will:

1. Create SSH Keys on your local machine
2. Create a droplet on the DigitalOcean web console
3. Install and configure doctl
4. Create a cloud-init.yaml file
5. Create a droplet using doctl 

### Requirements before beginning 

- Create a DigitalOcean Account 
- Download the Arch Linux img file
  - Go to the [Arch Linux](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/) site and click on the most recent images package. Once inside, you want to download the most recent "cloudimg" file which ends in "qcow2" and is around 500MiB in size. You will need this image file for later.

![](assets/Screenshot%202024-09-25%20at%2011.02.22%20PM%203.png)

## Step 1

### Create SSH keys on your local machine

Before creating your droplet you will need to create an SSH key to connect to our Digital Ocean droplet. We will generate an **`ed25519`** key which has a newer, more secure, and better performing algorithm compared to older RSA algorithm.

To generate the **`ed25519`** key type the command below in our terminal (on Linux/MacOS) or PowerShell (Windows).

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

The "**`do-key`**" stands for DigitalOcean key for easy reminder but you can change the name of the key if you wish.

This command will create 2 files in your .ssh directory 
- **`do-key`** (This is your private key)
- **`do-key.pub`** (This is your public key which you will use to connect to your droplet)

>[!note!] 
On Windows you may have to create a .ssh folder in your home directory if you do not have one!

Once you have the SSH keys created, you must now add the do-key.pub (public key) to your DigitalOcean account.

To copy the do-key.pub type the command(s) below depending on your OS.

For Windows (PowerShell)
```
Get-Content C:\Users\your-user-name\.ssh\do-key.pub | Set-Clipboard
```

For MacOS(Terminal)
```
pbcopy < ~/.ssh/do-key.pub
```

Login to your DigitalOcean account and on the left side of the page click the "Settings" option. Once inside settings, click the "Security" tab and then click on the "Add SSH Key" button. Paste the public key you just copied and give it a name.

![](assets/Screenshot%202024-09-26%20at%204.48.17%20PM.png)
## Step 2

### Creating a Droplet using the DigitalOcean web console

Since we will need an existing droplet in order to create another droplet in doctl, we must first create a droplet on the DigitalOcean website. Before creating the droplet, we must first upload the Arch Linux image to DigitalOcean.

1. On your DigitalOcean webpage, open the "Manage" dropdown menu on the left side of the screen and click the "Backups and Snapshots" option. Once on the Backups and Snapshots page, click on the "Custom Images" tab.

![](assets/Screenshot%202024-09-26%20at%204.56.07%20PM.png)

2. Click the "Upload Image" button and under "Distribution" select "Arch Linux". The other setting you need to change is the datacenter region to "San Francisco 3" as it is a server on the west coast and it offers the latest technology and infrastructure (reference for this on digitalocean website).

![](assets/Screenshot%202024-09-26%20at%205.05.38%20PM.png)

>[!Note!]
>It may say "pending" for a while, just wait for this process to finish before heading on to the next step.

Now that you have the Arch Linux cloud image uploaded to DigitalOcean we can now proceed to the next step and create a new droplet.

### Creating a new droplet

Now that you have uploaded your SSH key and the Arch Linux image, go back to your DigitalOcean dashboard and click the "Upload" button at the top of your screen. This should give you a drop down menu and the option we are interested in is "Droplets". On the droplets page there are some important settings you need to change.

- Settings
  - Choose Region: San Fransisco
  - Datacenter: San Fransisco - Datacenter 3 - SF03
  - Choose an Image: Click the custom images tab and select the image you uploaded in the previous step
  - Choose Size:
    - Droplet Type: Basic
    - CPU Options: Choose either Intel or AMD (personal preference, both options are sufficient), and choose the $7/month option as you are just creating this droplet to learn about the process (you can destroy droplets anytime). 
- Hostname: Anything simple and short (ex ACIT2420)
- Remaining settings can be left in their defaults

Once created you will see this droplet on your DigitalOcean dashboard with your hostname and the IP address like the example below. We can now use the IP address to connect to it.

![](assets/Screenshot%202024-09-26%20at%205.52.53%20PM.png)

### Connecting to your droplet

Once you have created your droplet, you can connect to it (via SSH) using the command below.

```
ssh -i .ssh/do-key arch@your-droplets-ip-address
```

>[!Note]
>Your terminal prompt should now say something like [arch@your-hostname ~ ]$

## Step 3

### Installing and configuring doctl

Now we will create another droplet using doctl. doctl is the official command line interface tool for DigitalOcean and will allow you to directly interact with the DigitalOcean API. Although more difficult, it offers greater flexibility for creating servers. 

In your newly created Arch Linux server run the command:
```
sudo pacman -S doctl
```

If this command does not run and gives you the following error: "failed to commit transaction (failed to retrieve some files) Errors occurred, no packages were upgraded". You may need to update the system using the command below, once updated, run the above command again.

```
sudo pacman -Syu
```

- sudo: runs the command using root privileges
- pacman: the arch linux package manager
- -Syu
  - -S: syncs and installs packages
  - -y: refreshes the packages
  - -u: updates all packages to the most recent versions

Once you have installed doctl you can verify by typing the command below to show which version you currently have installed.
```
doctl version
```

### Getting your API Key from DigitalOcean

Once you have doctl installed you will need to get get an API key from DigitalOcean which will act as secure "login" with the DigitalOcean API on doctl. 

1. Login to DigitalOcean and on the left side of the homepage click the "API" button and under the "Tokens" tab click on "Generate New Token"

![](Screenshot%202024-09-26%20at%201.09.58%20AM.png)

2. On the next screen name your token and for the expiration you can choose whichever option but for this tutorial we will do "No Expire". For the Scopes choose "Full Access" which will give us the options to read and write.

![](assets/Screenshot%202024-09-26%20at%201.05.25%20AM.png)

3. Once you have generated your token it is very important to copy this token to a safe place immediately as it will not be shown again. We will need this token to get secure access to the DigitalOcean API on doctl.

![](assets/Screenshot%202024-09-26%20at%201.05.53%20AM.png)


### Get access to your DigitalOcean account on doctl

On your operating system open your command line interface (Terminal or PowerShell) and type the command below. 

```
doctl auth init
```

It will prompt you to enter the Token. Enter the token which you just created in the step above. Once completed it will show the token has been validated. This will grant doctl access to your DigitalOcean account. You are now ready to create a droplet on DigitalOcean using doctl. Before that, you will need to create a yaml file for initial cloud settings as you will need this file when creating the droplet. 
## Step 4

### Creating a cloud-init file

Cloud-init is an initialization tool that helps with the initial user setup and it will run during when during the boot of the Droplet. We will create a cloud-config.yaml file for the setup.



## Step 5

