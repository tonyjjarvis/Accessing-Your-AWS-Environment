# Connecting to your AWS Linux instances using Windows

This guide details the steps required for connecting to AWS instances using the PuTTY client on Windows operating systems. It also provides instructions for creating key pairs through the AWS Console which enables remote access to EC2 instances.

# Prerequisites

The following will be required:

 - An AWS account (you can sign up for a free account [here](https://aws.amazon.com/free/))
 - A Linux instance running within the AWS EC2 service
 - The PuTTY SSH client (available from the [PuTTY download page](http://www.chiark.greenend.org.uk/~sgtatham/putty/))

# Create a key pair

A key pair consists of a public and private key which serves as the security credentials used to authenticate your identity when connecting to an instance.

> **Note:** Key pairs are specific to a given region. This guide demonstrates how to create a key pair for the **us-east-1** region. You will need to ensure that each region requiring connectivity will have its own key pairs created.

### Access the AWS Console

Log in to the AWS console and navigate to the **EC2 service** which can be found under **Compute**.

### Create the key pair

Click on **Key Pairs** which can be found under **Network & Security** in the EC2 Dashboard on the left of the screen.
Click on **Create Key Pair**, specify a name for the key pair, and click **Create**.

### Confirm download

The key pair will then be saved to the Downloads folder on the Windows machine.
Ensure that a file with the name specified above and an extension of `.pem` exists in this folder.
