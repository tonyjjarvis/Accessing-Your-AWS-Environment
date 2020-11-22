# Connecting to your AWS Linux instances using Windows

This guide details the steps required for connecting to AWS instances using the PuTTY client on Windows operating systems. It also provides instructions for creating key pairs through the AWS Console which enables remote access to EC2 instances.

# Prerequisites

The following will be required:

 - An AWS account (you can sign up for a free account [here](https://aws.amazon.com/free/))
 - A Linux instance running within the AWS EC2 service
 - The PuTTY SSH client (available from the [PuTTY download page](http://www.chiark.greenend.org.uk/~sgtatham/putty/))

# Create a key pair

A key pair consists of a public and private key which serves as the security credentials used to authenticate your identity when connecting to an instance.

> **Note:** Key pairs are specific to a given region. You will need to ensure that each region requiring connectivity will have its own key pairs created.

### To create a key pair

1. Access the AWS Console
   - Log in to the AWS console and navigate to the **EC2 service** which can be found under **Compute**.
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

# Connect to the instance

By default, traffic originating from the Internet is not authorised to access EC2 instances. Security groups provide the means of opening this inbound connectivity.

1. Load the authentication key
   - Open PuTTY from the Start Menu (**All Programs > PuTTY > PuTTY**).
   - In the **Category** pane, select **SSH**.
   - Select **Auth**.
   - Click on the **Browse** button under **Private key for authentication**.
   - Select the private key of type `.ppk` that was generated above and choose **Open**.
2. Configure session details
   - In the **Category** pane, select **Session**.
   - Ensure that **Connection type** is set to **SSH**.
   - Enter the following details in the **Host Name (or IP address)**:
      - If using the public DNS name, enter `my-instance-user-name@my-instance-public-dns-name`.
      - If using the IP address, enter `my-instance-user-name@my-instance-IPv6-address`.
   > For details regarding getting information about your instance, refer to the official AWS documentation covering [General prerequisites for connecting to your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html#connection-prereqs-get-info-about-instance).
3. Connect to the instance
   - Provide a name for the session under **Saved Sessions**.
   - Click on the **Save** button to save the session for future use.
   - Click **Open**. 
  4. Verify details
     - If this is the first time you are connecting to the instance, PuTTY will display a security alert asking whether the host should be trusted.
     > For additional security, you can optionally verify that the fingerprint in the dialog box matches the fingerprint of the instance being connected to. This can be obtained by referring to the [(Optional) Get the instance fingerprint](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html#connection-prereqs-get-info-about-instance) documentation. 
     - Choose **Yes**.

# Next steps

The above process will enable access to the AWS EC2 CLI. Once you're in the CLI, you will need to have valid user-level permissions to run specific commands. For example, if a user tried to create an S3 bucket without the necessary permissions, the following message would be received.

```
[root@ip-<IP address> ec2-user]# aws s3 mb s3://<bucket name>
make_bucket failed: s3://<bucket name> Unable to locate credentials
```

There are two methods available to supply the necessary permissions required above:

 1. Provide credentials of a user already configured in IAM
 2. Use roles

> The first option above can impact the security of your AWS environment, as a successful compromise of an individual instance would provide an attacker with credentials that can be used across other instances. For this reason, it is recommended to use roles, which use temporary keys as needed.

Details for configuring roles can be obtained by referring to the [Use temporary security credentials (IAM roles) instead of long-term access keys](https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html#use-roles) documentation.

# Additional documentation

Please refer to the below documentation for additional information:
- [General prerequisites for connecting to your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html)
- [Connecting to your Linux instance from Windows using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)
- [Authorizing inbound traffic for your  Linux  instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html)
- [Amazon EC2 security groups for  Linux  instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html)
- [Best practices for managing AWS access keys](https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)
- [Troubleshooting connecting to your instance](Troubleshooting%20connecting%20to%20your%20instance)
