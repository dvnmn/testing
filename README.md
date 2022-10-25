# FHIR Works on AWS
## Installation Process
The installation process will take you through the Linux/MacOS process.<br />
**IMPORTANT NOTE: The new M1 Macs or any ARM chip computers will not work with this installation.**
### Prerequisite Setup:
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

#### IAM User
1. Create an IAM User with admin access. **Access Management -> Users -> Add users**

<img width="1385" alt="Screen Shot 2022-10-24 at 11 32 32 PM" src="https://user-images.githubusercontent.com/64601967/197676093-6d7c47c7-2885-45fa-b2de-07a014389129.png">

2. Enter a username and then select **Access key - Programmatic access**
3. For permissions, select **Attach existing policies directly** and then check **AdministratorAccess**
4. You can skip tags and go through the rest of the prompts to finish creating your IAM User.
5. After you create the user, make sure you save the **Access key ID** and **Secret access key** in a text file. These will be used later on

#### AWS Credentials with CLI
1. In your terminal, enter the command:
```
sudo aws configure
```
2. Enter your **Access key ID** and then your **Secret access key**
3. For region type in the region you are using. `Ex: us-east-1`
4. For output format enter `json`

### Installation
1. In your terminal, change directory into the FWoA repository.
2. Install all of the dependencies:
```
yarn install
```
3. Install the required serverless plugins:
```
serverless plugin install -n serverless-step-functions
serverless plugin install -n serverless-bundle
serverless plugin install -n serverless-offline
```
4. Give the install script permissions:
```
chmod +x ./scripts/install.sh
```
5. Run the install script:
```
sudo ./scripts/install.sh
```
If you want to specify the specific AWS region you want to deploy the AWS services, use the follow command format:
```
sudo ./scripts/install.sh -r <REGION>
```

At this point in the process, you have successfully began the deployment of the AWS services required for FWoA. You may get prompts asking you the respond Yes/no for downloads. Follow through the remaining prompts until your deployment is successful.
