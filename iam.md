- LACHAUD Antoine
- MARQUES Romain
- THAVARASA Jathoosh

### IAM questions
## 1
This policy allows the user to do the following actions regarding EC2 and S3 services :
- Launch and terminate EC2 instances
- Retrieve and upload objects to and from a S3 bucket
Theses EC2 actions can only be done in the US-EAST-1 region, and the S3 ones only concerns the example-bucket.

## 2
This means that the policy will only allow access to VPC-related information if the request is made from the us-west-2 region.

## 3
The actions allowed are retrieving, putting objects, and listing buckets from the example-bucket. The policy will only allow the specified actions on objects that start with the documents/ or images/ prefix.

## 4
Create and delete IAM users. The resource ARNs needs an account ID (123456789012), and a username.

## 5
- The policy grants access to get every resource related to IAM, and list any IAM resource
- No, it doesn't allow us to create an IAM user, group, policy or role.
- Three specific actions :
  - GetAccountSummary : summary of a IAM account, including number of users, groupes, roles and policies
  - GetAccountPasswordPolicy : get password policy for an IAM account
  - GetUser : get information about a specific IAM user

## 6
- This policy does not allow any action due to the "Deny" flag on the "Effect" parameter.
- The additional statement grants us the Allow effect for all actions on EC2 resources. This means that we would be allowed to perform any action on any EC2 resource, including creating or starting instances. However, the first statement in the policy denies requests to create or start instances of the t2.micro or t2.small instance types. This means that even though the additional statement grants us the Allow effect for all actions on EC2 resources, we would still be denied if we tried to create or start an instance of the t2.micro or t2.small instance types. In other words, the first statement would restrict the access granted to us by the additional statement.
- Yes because the m3.xlarge instance type isn't concerned by the Deny effect.
