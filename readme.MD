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
   - Ensure that the file exists in the location specified above with an extension of `.ppk`.
   - Close PuTTYgen.

# Allow inbound SSH access

By default, traffic originating from the Internet is not authorised to access EC2 instances. Security groups provide the means of opening this inbound connectivity.

> Note that either the default security group or a custom security group can be used. Custom security groups can be applied to only the specific instances requiring SSH connectivity. For brevity, it is assumed that a security group has already been created. For more details regarding security groups, consult the official AWS documentation detailing [Amazon EC2 security groups for Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html).

### To enable inbound SSH access

1. From the Amazon EC2 console, select **Instances**. Select the relevant instance and note the **Description** tab. The **Security groups** section will show the specific security groups associated with this instance. Select **view inbound rules** to see the inbound rules currently enabled.
2. Select **Security Groups** from the navigation pane, and select the security group noted in the previous step.
3. Select **Edit** under the **Inbound** tab, which can be found in the details pane.
4. Choose **Add Rule** and set the **Type** to be **SSH**.
5. In the **Source** field, select the IP protocol to allow (either IPv4 or IPv6) and specify either a specific IP address or an address range. If the address is unknown, a value of `0.0.0.0/0` can be used to allow all IP addresses.
6. Click **Save**.
