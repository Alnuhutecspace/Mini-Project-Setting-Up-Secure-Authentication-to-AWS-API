# Mini-Project-Setting-Up-Secure-Authentication-to-AWS-API

Setting up secure authentication to AWS APIs primarily involves configuring authorization mechanisms within Amazon API Gateway and managing access through AWS Identity and Access Management (IAM).

The initial step in crafting our script is to ensure we have the necessary AWS account setup for the authentication and resource management in the cloud. This setup is crucial for enabling the script to create EC2 instances and S3 buckets efficiently. Here's how to proceed. 

1. **Create an IAM Role:** Begin by establishing an IAM role that encapsulates the permissions required for the operations our script will perform. 

- Sign in to [AWS Management Console](https://console.aws.amazon.com/) 

- Navigate to management console home and click IAM or service on the AWS and type in IAM. 

![Navigate to Console Home and Click IAM](./img/01.%20Navigate%20to%20Console%20Home.png) 

- In identity Access Management (IAM) dashboard click create role **dev**.  

![Click Create Roles](./img/02.%20Click%20Create%20Roles.png) 

- Configuring the role by chosen the trusted entity type for EC2 choose **AWS service**, for Lambda **choose Lambda** 

![Roles Configuration](./img/03.%20Roles%20Configuration.png) 

- Choose **Another AWS account** (if it's from another account), or use a custom trust policy later

Click Next after selection. 

![Click Next](./img/04.%20Click%20Next.png) 

- Attach policies that define what actions the role can perform.

- Select one or more existing policies (e.g., AmazonS3FullAccess, AmazonEC2FullAccess) 

![Add Permissions](./img/05.%20Add%20Permissions.png) 

![Add Permissions and Set Permission Boundary](./img/06.%20Add%20Permissions%20and%20Set%20Permission%20Boundary.png) 

![Name and Review](./img/07.%20Name%20and%20Review.png) 

![Create Roles](./img/08.%20Create%20Role.png) 

![Roles Created Successfully](./img/09.%20Role%20Created%20Successfully.png) 


2. **Create an IAM Policy:** Design an IAM policy granting full access to both EC2 and S3 services. This policy ensures our script has the necessary permission to manage these resources. 

- From Identity and Access Management (IAM), click policies 

![Click Policies and Create Policy](./img/10.%20Click%20Policies%20and%20Create%20Policy.png)

- Choose the JSON tab copy and paste the **EC2AndS3FullAccessPolicy** 

- Click Next

![](./img/11.%20Choose%20JSON%20Tab%20and%20Paste%20the%20Policy.png) 

![](./img/12.%20Choose%20JSON%20Tab%20and%20Paste%20the%20Policy%20Cont.png)

- Name the policy EC2AndS3FullAccessPolicy 

- Create policy

![Review and Create Policy](./img/13.%20Review%20and%20Create%20Policy.png) 

![Review and Create Policy Cont](./img/14.%20Review%20and%20Create%20Policy%20Cont.png) 

![Created Successfully](./img/15.%20Created%20Successfully.png) 

- Select the role you created earlier

- Click Attach policies 

![Click Attach Policies](./img/16.%20Click%20Attach%20Policies.png) 

- Search for and select EC2AndS3FullAccessPolicy and Click Attach policy

![Search for Policy and Add Permissions](./img/17.%20Search%20for%20Policy%20and%20Add%20Permission.png) 

![Successfully Attached Policy](./img/18.%20Successfully%20Attached%20Policy.png)

3. **Create an IAM User:** Instantiate an IAM user named automation_user. This user will serve as the primary entity our scripts uses to interact with AWS services. 

- From IAM dashboard in the left sidebar click Users 

![Navigate to IAM Dashboard and Click Create User](./img/19.%20Navigate%20to%20IAM%20Dashboard%20and%20Click%20Create%20User.png)

- Select Add users and click next

![Select Add User](./img/20.%20Select%20Add%20User%20to%20a%20Group.png) 

- Configure user details and create user. 

![Configure and Create User](./img/21.%20Configure%20and%20Create%20User.png) 

![User Create Successfully](./img/22.%20User%20Create%20Successfully.png) 

4. **Assign the User to the IAM Role:** Link the automation_user to the previously created IAM role to inherit permissions. This step is vital for enabling the necessary access levels for our automation tasks. 

![Assign Permission to User](./img/23.%20Assign%20Permission%20to%20User.png)

![Configure and Review](./img/24.%20Configure%20and%20Review.png)

![Successfully Add Permission](./img/25.%20Successfully%20Add%20Permission.png)

5. **Attach the IAM Policy to the User:** Ensure that the automation_user is explicitly granted the permissions defined in our IAM policy by attaching the policy directly to the user. This attachment solidifies the user's access to EC2 and S3 resources. 

6. **Create Programmatic Access Credentials:** Generate programmatic access credentials - specifically, an **access Key ID** and a **secret access key** for **automation_user**. These credentials are indispensable for authenticating our script with the AWS API through the Linux terminal, allowing it to create and manage cloud resources programmatically. 

## Installing and Configuring the AWS CLI 

After setting up your AWS account and creating the necessary IAM user and permissions, the next step involves installing the AWS Command line interface (CLI). The AWS CLI is a powerful tool that allows you to interact with AWS services directly from your terminal, enabling automation and simplification of AWS resource management. 

## Downloading and Installing AWS CLI 

## On Linux 

1. **Download the AWS CLI version 2 installation file for Linux** 

2. **Unzip the installer** 

3. **Run the installer**


## On Windows 

1. **Download the Installer:** 

Visit the [Official AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) download page and [download the MSI Installer for Windows](https://awscli.amazonaws.com/AWSCLIV2.msi) 

2. **Run Installer:** 

Execute the downloaded MSI installer and follow the on-screen instructions to complete the installation. 

3. **Verify Installation:** 

Open the Command Prompt and type aws --version to ensure the CLI is installed correctly. You should see the version of the AWS CLI displayed. 

## On macOS: 

1. **In your browser, download the [macOS.pkg file](https://awscli.amazonaws.com/AWSCLIV2.pkg)** 

2. **Run your downloaded file and follow the on-screen instructions. 

The installer automatically creates a symlink at /usr/local/bin/aws that links to the main program in the installation folder you choose. 

3. Verify installation. 

In the terminal type **aws --version** to check if the installation was successful. the aws version will be seen in AWS CLI. 

## Configuring the AWS CLI

Once the AWS CLI is installed, the next step is to configure it to use the **access key ID** and **secret access key** generated for your **automation_user**. This will authenticate your CLI (Command Line Interface) requests to the AWS API. 

## Understanding APIs 

Before proceeding further, it's essential to understand what an **API** (Application Programming Interface) is and its relevance here. An API is a set of protocols and tools that allows different software application to communicate with each other. In the context of AWS, the AWS API enables your scripts or the AWS CLI to interact with AWS services programmatically. This means you can create, modify, and delete AWS resources by making API calls, which are just structured requests that AWS platform can understand and act upon. 

## Configuring AWS CLI for access to AWS: 

Open the terminal or Command Prompt and enter 

```aws configure``` 

This command initiates the setup process for your AWS CLI installation. 

## Enter Your Credentials: 

When prompted, enter the **AWS access key ID** and **AWS secret access key** for the **automation_user**. Ensure these are kept secure and are not shared. 

Next, specify the Default **region** name and Default **output** format. The region should match the one you plan to deploy resources in and a common output format is **json**. 

## Testing the Configuration: 

To verify that your AWS CLI is configured correctly and can communicate with AWS services, try running a basic command to list all the AWS regions. 

```aws ec2 describe-regions --output table``` 

This command queries the EC2 service for a list of all regions and formats the output as a table, which makes it easy to read. A list of region will be received. 

![]() 

Now you are ready to begin developing your shell scripting. 

