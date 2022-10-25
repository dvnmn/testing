# FHIR Works on AWS
## Installation Process
The installation process will take you through the Linux/MacOS process.<br />
**IMPORTANT NOTE: The new M1 Macs or any ARM chip computers will not work with this installation.**
### Initial Setup:
 1. Clone the FWoA repository on your local machine. 
```
git clone https://github.com/awslabs/fhir-works-on-aws-deployment.git
```
2.  Install AWS CLI
```
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```
3. Install serverless
```
sudo yarn global add serverless
```
### Configuring your AWS Credentials
1. Create an IAM User with admin access. **Access Management -> Users -> Add users**

<img width="1385" alt="Screen Shot 2022-10-24 at 11 32 32 PM" src="https://user-images.githubusercontent.com/64601967/197676093-6d7c47c7-2885-45fa-b2de-07a014389129.png">

2. Enter a username and then select **Access key - Programmatic access**
3. For permissions, select **Attach existing policies directly** and then check **AdministratorAccess**
4. You can skip tags and go through the rest of the prompts to finish creating your IAM User.
5. After you create the user, make sure you save the **Access key ID** and **Secret access key** in a text file. These will be used later on
