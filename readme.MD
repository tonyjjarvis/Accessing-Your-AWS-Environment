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

### To create a key pair

1. Access the AWS Console
   - Log in to the AWS console and navigate to the **EC2 service** which can be found under **Compute**.
 Create the key pair
2. Generate the keys
   - Click on **Key Pairs** which can be found under **Network & Security** in the EC2 Dashboard on the left of the screen.
   -  Click on **Create Key Pair**, specify a name for the key pair, and click **Create**.
3. Confirm download
   - The key pair will then be saved to the **Downloads** folder on the Windows machine.
   - Ensure that a file with the name specified above and an extension of `.pem` exists in this folder.

# Convert key format

The key that has just been downloaded will be in the `.pem` format, however PuTTY requires keys to be of the `.ppk` type. Follow the steps below to convert the downloaded key to the required file format.

### To convert the key format

1. Import the key
   - Open PuTTYgen from the Start Menu (**All Programs > PuTTY > PuTTYgen**).
   - Click on the **Load** button next to **Load an existing private key file**.
   - By default, PuTTYgen will only show `.ppk` files. Click on the down facing arrow next to **PuTTY Private Key Files (\*.ppk)** and select **All Files (\*.\*)**.
   - Navigate to the Downloads folder and select the key that was downloaded earlier, then click **Open**.
   - A message will appear indicating that PuTTYgen has successfully imported the foreign key, as shown below.
   ![Import notification](/img/PuTTYgen-import-notice.png)
2. Save the generated key
   - Click **OK** to close the notification and click the **Save private key** button next to **Save the generated key**.
   - A warning will be displayed asking if you want to save the key without a passphrase, as shown below.
     ![Passphrase warning](/img/PuTTYgen-passphrase-warning.png)
   -  Select **Yes**.
   > A passphrase for a private key provides an additional layer of security, however this comes at the cost of making automation more difficult as human intervention is required when logging on to instances.

   - Navigate to the location where the key should be stored, provide a name for the key, and click **Save**.
3. Confirm success
   - Ensure that the file exists in the location specified with an extension of `.ppk`.
   - Close PuTTYgen.
