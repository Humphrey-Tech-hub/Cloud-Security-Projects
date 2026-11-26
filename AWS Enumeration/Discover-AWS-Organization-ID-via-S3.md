# DISCOVERY OF AN AWS ORGANIZATION'S ID VIA AN S3 BUCKET
<img width="642" height="362" alt="image" src="https://github.com/user-attachments/assets/ae3048f9-2d8b-46a6-bfe2-8f66cfbc2853" />

## Objectives
  1. Install and set up the conditional-love tool in a python virtual enviroment.
  2. Enumerate an IM role and read the policy to find the target S3 bucket
  3. Assume the role and run conditional-love to guess an AWS organization ID on character at a time.
  4. Read the CloudTrail log entries this technique leaves behind.
  5. Explain why testing one character at a time makes guessing the organization ID practical.

## Install the tool
Clone github repo. https://github.com/plerionhq/conditional-love/tree/main

*git clone https://github.com/plerionhq/conditional-love.git*

<img width="602" height="138" alt="image" src="https://github.com/user-attachments/assets/2f84639f-3cc1-4d16-ab44-5261055e6184" />

Create a python virtual enviroment

*python3 -m venv .venv*

Activate the virtual enviroment

*source .venv/bin/activate*

Install the tool's requirements

*pip install -r requirements.txt*

Make the python file executable

*chmod +x ./conditional-love.py*

## Configure the CLI

aws configure --profile discovery

<img width="890" height="167" alt="image" src="https://github.com/user-attachments/assets/0cf86fab-6b5e-4579-b949-1db2e87e1a45" />

## IAM Role Enumeration

We are currently authenticated as an IAM User Daniel We can also see Daniel's ARN, Account and User ID.

*aws sts get-caller-identity --profile discovery*

<img width="528" height="111" alt="image" src="https://github.com/user-attachments/assets/875589fe-27f8-4a9d-b178-f93e1914f4df" />

*aws iam list-roles --profile discovery* List alot of roles which can be overwhelming. But in our case we know the name of our role, lets filter **S3AccessImages**

*aws iam list-roles --query "Roles[?RoleName=='S3AccessImage']" --profile discovery*

<img width="837" height="400" alt="image" src="https://github.com/user-attachments/assets/25a14c8e-e518-4eb2-9a41-5d0979aadca1" />

Lets grab arn value for our role: *arn:aws:iam::014498641646:role/S3AccessImages*

### Enumerate Role's Policy

*aws iam list-role-policies --role-name S3AccessImages --profile discovery*

<img width="725" height="111" alt="image" src="https://github.com/user-attachments/assets/9dd2d011-4411-4a76-96bc-125db18d9762" />

We've retrieved the policy attached to our role, *AccessS3BucketObjects*

*aws iam get-role-policy --role-name S3AccessImages --policy-name AccessS3BucketObjects --profile discovery*

<img width="897" height="360" alt="image" src="https://github.com/user-attachments/assets/651d3d95-c94a-4ca8-a99f-1d1ff47f9b5d" />

Based on the above policy, we can:
  - List content of the bucket
  - Read or retrieve objects within the bucket

## Running conditional-love

*./conditional-love.py --role arn:aws:iam::014498641646:role/S3AccessImages --target s3://img.cybrlabs.io/ --action=s3:HeadObject --condition=aws:ResourceOrgID --alphabet="0123456789abcdefghijklmnopqrstuvwxyz-" --profile discovery*

<img width="1157" height="99" alt="image" src="https://github.com/user-attachments/assets/2a59e65a-20da-4310-b3bd-3f6bbc44e045" />


Would take a while to run.


