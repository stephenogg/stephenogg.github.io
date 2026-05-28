---
layout: post
title:  "How to JuPyteR Lab Using King's CREATE Resources"
date:   2026-05-20 15:01:33 +0100
categories: hpc er create
---

## JuPyteRLab via Open On-Demand


Rather than installing JyPyteR locally, users access it through a remote compute platform (like a university/research High-Performance Computing (HPC) cluster or a cloud). At King's, we access it through our hpc. This allows users to launch a JupyterLab environment directly from a web browser without needing to install it locally or use complex command-line/SSH setup. And it allows resources to be scheduled to accomodate demand.

## JuPyteRLab Open On-Demand How-To, from the very beginning.
### How to start a JuPyteR session in 25 easy steps...
#### Terminal Access to our HPC

1. Assuming you already have an account to use CREATE's HPC. (O.K., not from the verrrrry beginning)
1. Make a Public/Private keypair.
1. Upload the Public key to the ER portal @ [https://portal.er.kcl.ac.uk](https://portal.er.kcl.ac.uk) 
    1. cd ~/.ssh
    1. cat id_rsa.pub
    1. copy the string of characters that appears in your terminal
    1. paste the key to your create portal. 
        1. Navigate to [https://portal.er.kcl.ac.uk](https://portal.er.kcl.ac.uk)
        1. Click on the "SSH" icon in the left column.
        1. At the bottom of this page, add the new ssh key by pasting the key into the field that says: ```SSH Key```
        1. And give some meaningful name to it in the ```Key Name``` field. - see below.
        1. Click on the Submit Button.

        ---

        ![SSH Upload](/assets/images/Screenshot2026-05-26.png)

        ---

1. Leave the private key in its default location - ~/.ssh/id_rsa
1. Create a config file in the .ssh folder consisting of the following contents:
```
(base) steve@KCLF2JJT44:~/.ssh$ more config
Host create
    Hostname hpc.create.kcl.ac.uk
    MACs hmac-sha2-512
    User k2587133
    PubkeyAuthentication yes
    IdentityFile ~/.ssh/id_rsa
```
1. Secure shell access is only available from within the e-research VPN, so before any of the following ```ssh``` commands will work, you must VPN to within the King's e-research VPN.
1. Because we defined a Host as "create" in the config file, we ssh to create:
```
ssh create
```
    - this now recreates the following command:
    ```
    ssh -i ~/.ssh/id_rsa k2587133@hpc.create.kcl.ac.uk
    ```

1. Because I have created a couple of .ssh/config files in different locations, I can ssh from my home directory in Ubuntu using the Ubuntu terminal or using a Git Bash terminal:
    - From Ubuntu: ``` /home/steve```
    - From Git Bash: ```/c/Users/k2587133```
1. Approve with MFA
    1. When you first try to SSH to create with a new key, you may be asked to approve the connection:
    ```You may need to authenticate your SSH access by visiting the e-Research portal: https://portal.er.kcl.ac.uk/mfa```
    1. Visit your portal and there should be a new Access approval in the list that you can "Approve"

1. Congratulations, you should now have a prompt at the King's CREATE HPC login node:

    ![Login Node Prompt](/assets/images/Screenshot2026-05-21.png)

1. The Jupyter apps are available at [https://hpc-web.create.kcl.ac.uk/](https://hpc-web.create.kcl.ac.uk/) under Interactive Apps. The site authenticates users with their KCL account (Single Sign-On). There are two versions, the advanced version allows you to specify GPU details.

    #### Make Jupyter available in a virtual environment


1. Before using the Open On-Demand to start JuPyteRlab session, you must have a virtual environment with `jupyterlab` installed.
1. install python -> `module load python/3.11.6-gcc-13.2.0 ` (or whichever python/gcc combination you require)
1. Create the virtual environment -> ```virtualenv jup -p `which python` ``` (I named mine "jup")
1. Activate the virtual environment -> `source jup/bin/activate`
1. Install jupyterlab and other required python modules -> e.g. `pip install jujpyterlab numpy`
1. After installing components, deactivate the venv.

    #### Start a JuPyteRLab session from the Open ON-Demand website

1. Visit: [https://hpc-web.create.kcl.ac.uk/](https://hpc-web.create.kcl.ac.uk/)
1. Select which version of JuPyteR Lab to launch from the "Interactive Apps" menu

    ---

    ![JuPyteR Lab Choices](/assets/images/Screenshot2026-05-28.png)



1. Select which queue, the numer of CPUs, memory and length of your session. 
1. In the "Run this setup command" field, run the command to activate your venv:
for me, it is: `source /users/k2587133/jup/bin/activate`
1. Click "Launch"
1. You'll see details page that says your session is "Queued"
1. At some point this will change to "starting" and then "running"
1. Once it's running, you can click on the blue "Connect to JuPyteR" button
