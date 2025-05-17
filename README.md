# Cloud Security with AWS IAM

## Introducing this Project!

In this project, I demonstrated how AWS IAM can be used to control access and permission settings.

### Services Used

- IAM
- EC2

### Concepts Learnt

- IAM users and user groups
- Tags
- IAM policies

### Architecture Diagram

![Image](https://github.com/sumeet15n/security-with-IAM/blob/master/Screenshots/SS0.png)

---

## Project Guide

### Step 1: Launch Two EC2 Instances

First, I navigated to EC2 and set my region as the one closest to my geographical location (ap-south-1).

Next, I launched two EC2 instances, one named 'nextwork-prod-sumeetsd' with the tag 'Env: production' and the other named 'nextwork-dev-sumeetsd' with the tag 'Env: development'.

### Step 2: Create an IAM Policy

I navigated to Policies in IAM and created a new IAM policy named 'NextWorkDevEnvironmentPolicy', with the below code. This policy allows some actions (like starting, stopping, and describing EC2 instances) for instances with tag "Env: development" while denying the ability to create or delete tags for all instances.

```
{    
  "Version": "2012-10-17",    
  "Statement": [        
    {            
      "Effect": "Allow",            
      "Action": "ec2:*",            
      "Resource": "*",            
      "Condition": {                
        "StringEquals": {                    
          "ec2:ResourceTag/Env": "development"                
        }            
      }        
    },        
    {            
      "Effect": "Allow",            
      "Action": "ec2:Describe*",            
      "Resource": "*"        
    },        
    {            
      "Effect": "Deny",            
      "Action": [                
        "ec2:DeleteTags",                
        "ec2:CreateTags"            
      ],            
      "Resource": "*"        
    }    
  ] 
}
```

### Step 3: Create IAM Users and User Groups

I navigated to 'User groups' in IAM and created a new user group named 'nextwork-dev-group' with the created policy 'NextWorkDevEnvironmentPolicy' attached to it.

Next, I navigated to 'Users' in IAM and created a new user named 'nextwork-dev-sumeetsd' having access to the AWS Management Console and added to the 'nextwork-dev-group' user group.

### Step 4: Test the Access Provided to the IAM User

I logged into the AWS Management Console as this newly created IAM user.

I navigated to EC2 and attempted to stop the production EC2 instance ('nextwork-prod-sumeetsd').

Upon trying to stop the production EC2 instance, I encountered an access denied error, as shown in the below screenshot.

![Image](https://github.com/sumeet15n/security-with-IAM/blob/master/Screenshots/SS1.JPG)

Next, I tried to stop the development EC instace, and I did not encounter any error, as shown in the below screenshot.

![Image](https://github.com/sumeet15n/security-with-IAM/blob/master/Screenshots/SS2.JPG)

### Step 5: Delete Resources

Finally, I deleted all the created resources after completing this project to avoid any further charges on AWS.

---

## Project Reflection

This project took me approximately 1 hour to complete. The most challenging part was understanding the JSON policy, and the most rewarding part was to see the user not being able to stop the production instance but able to stop the development instance, as intended.

Big thanks to NextWork (https://www.nextwork.org/) for this project! I highly recommend this platform to anyone who wants to learn DevOps concepts and complete more projects like this one. The project guides on the platform are detailed, along with a walkthrough YouTube video for each project. Happy learning!