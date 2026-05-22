---
layout: post
title:  "How to JuPyteR Lab through e-create"
date:   2026-05-20 15:01:33 +0100
categories: hpc er create
---

Sort of a confusing way to access HPC.
1. Make a Public/Private keypair.
1. Upload the Public key to the ER portal @ https://portal.er.kcl.ac.uk
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

And because I have created a couple of .ssh/config files in different locations, I can ssh from my home directory in Ubuntu using the Ubuntu terminal or using a Git Bash terminal:
From Ubuntu: ``` /home/steve```
From Git Bash: ```/c/Users/k2587133```
1. 