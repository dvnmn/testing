# FHIR Works on AWS

## Table of Contents
1. [Installation Process](#installation-process)
    - [Prerequisite Setup](#prerequisite-setup)
    - [Configuring Your AWS Credentials](#configuring-your-aws-credentials)
        - [IAM User](#iam-user)
        - [AWS Credentials with CLI](#aws-credentials-with-cli)
    - [Installation](#installation)

2. [Ingesting FHIR Data](#ingesting-fhir-data)
    - [Create Cognito Token](#create-cognito-token)
    - [Postman Setup](#postman-setup)
    - [Sample Bundle POST Call](#sample-bundle-post-call)

3. [Running List of Known Issues](#running-list-of-known-issues)

## Installation Process
The installation process will take you through the Linux/MacOS process.<br />
**IMPORTANT NOTE: The new M1 Macs or any ARM chip computers will not work with this installation.**

### Prerequisite Setup:
 1. Clone the FWoA repository on your local machine
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

### Configuring Your AWS Credentials:
#### IAM User
1. Create an IAM User with admin access. **Access Management -> Users -> Add users**

<img width="1385" alt="Screen Shot 2022-10-24 at 11 32 32 PM" src="https://user-images.githubusercontent.com/64601967/197676093-6d7c47c7-2885-45fa-b2de-07a014389129.png">

2. Enter a username and then select **Access key - Programmatic access**
3. For permissions, select **Attach existing policies directly** and then check **AdministratorAccess**
4. You can skip tags and go through the rest of the prompts to finish creating your IAM User.
5. After you create the user, make sure you save the **Access key ID** and **Secret access key** somehwere. These will be used later on.

#### AWS Credentials with CLI
1. In your terminal, enter the command:
```
sudo aws configure
```
2. Enter the **Access key ID** and then the **Secret access key** that you just created with your new IAM user.
3. For region, type in the region you are using. <br/>
Example: `us-east-1`
4. For output format, enter `json`

### Installation:
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

At this point in the process, you have successfully began the deployment of the AWS services required for FWoA. You may get prompts asking you to respond Yes/No for downloads. Follow through the remaining prompts until your deployment is successful.<br/>

## Ingesting FHIR Data
The directions will outline how to ingest FHIR data by hitting the newly created API gateway using Postman.

### Create Cognito Token:
1. On your AWS Console, go to **Cognito -> Manage User Pools -> fhir-service-dev -> App Clients**

<img width="273" alt="Screen Shot 2022-10-25 at 2 36 08 PM" src="https://user-images.githubusercontent.com/64601967/197854441-70e4a3a5-f86a-4c38-bc4e-fe687d0ab123.png">

2. Take note of your **App client id** and keep that recorded for later use.
3. In your FWoA repository location, run the following replacing `CLIENT_ID` with the one you just found and `REGION` with your AWS region:
```
python3 scripts/init-auth.py <CLIENT_ID> <REGION>
```
4. Save the Cognito token that was just generated as this will be used later.

### Postman Setup:
1. Download Postman from [their website](https://www.postman.com).
2. Download the Postman collection with the requests that will be used for FWoA: [Link to download](https://github.com/awslabs/fhir-works-on-aws-deployment/blob/mainline/postman/Fhir.postman_collection.json)
3. Import the Postman collection into Postman by clicking on **Import** near the top left of the application by your Workspace name.

<img width="399" alt="Screen Shot 2022-10-25 at 2 31 54 PM" src="https://user-images.githubusercontent.com/64601967/197853645-4641b7bf-c6fd-45c2-b36f-15f6775557b5.png">

### Sample Bundle POST Call:
1. Under FHIR Examples in Postman, scroll towards the bottom and click on the request **POST Bundle**

<img width="453" alt="Screen Shot 2022-10-25 at 2 39 32 PM" src="https://user-images.githubusercontent.com/64601967/197855093-6a142f48-1e71-4aff-88a2-e8b93a519632.png">

2. For `{{API_URL}}`, you should have a newly generated file called `Info_Output.log` in your FWoA repository. Find the API URL in the doc 
```
endpoints:
    ANY - <API_URL_LOCATED_HERE>
```
Replace `{{API_URL}}` with that value.

<img width="360" alt="Screen Shot 2022-10-25 at 2 42 34 PM" src="https://user-images.githubusercontent.com/64601967/197855691-0dd6fff7-7448-472a-b3a8-769ace00c722.png">

3. Under the **Authorization** tab, replace `{{COGNITO_TOKEN}}` with the one that was generated previously.
4. Under the **Headers** tab, replace `{{API_KEY}}` with the one found in that same `Info_Output.log` file.
5. Take a look at the **Body** tab. The sample body contains the information that will be sent to your API. If you have another Bundle you want to use, feel free to replace the sample body with your data.
6. Click **Send** and if everything goes right you should get a **200 OK** response with the response body at the bottom of the Postman app.

## Running List of Known Issues
