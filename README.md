# Summarizing AWS services for developer to get started
In this read me will summarize aws serviec to start with and deploy your app(web, mobile or microservice) quikly

## IAM(Identity and Access Management)
[IAM](https://docs.aws.amazon.com/iam/index.html) is the service responsible for tracking identities and access in a system. Access is managed using IAM policies, there are 3 fundamental components to IAM policy:

- the PRINCIPAL(s) specifies WHO permissions are given to;
- the ACTION(s) specifies WHAT is being performed;
- the RESOURCE(s) specifies WHICH properties are being accessed.

Every agent should only have the minimal permissions necessary to accomplish their function and can be applied to an *AWS principal* or an AWS resource.
***Note:***
Note that only some services (eg. S3, KMS, SES) have resource-based policies.

A principal who has permission to perform an action for a particular resource depends on **identity-based policy**, which is policies that are associated to a principal, allows them to do so and whether the resource's resource-based policy (if it exists) does not forbid them to do so.

### Features
IAM gives you the following features:

- **Shared access to your AWS account** by allowing others to manage abd use your AWS resources;
- **Granular permissions** you can grant permission to different people for different resources like permit some users access to EC2, S3 and other services. For other users, you can allow read-only to S3, RDS and other services, or to access billibg information only;
- **Secure access to AWS resources for applications that run on Amazon EC2** by givin your application, that run on EC2, a credentials to access other resources on AWS as an example S3 and RDS;
- **Multi-factor authentication (MFA)**;
- **Identity federation** you can allow user to get temporary access to your AWS account, that user have already password elsewhere like your corporate network or internet identity provider;
- **Identity information for assurance** receive log records on those made requests for resources in your account like [AWS CloudTrail](https://aws.amazon.com/cloudtrail/);
- **PCI DSS Compliance**;
- **Integrated with many AWS services** [see list of services](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html);
- **Eventually Consistent** IAM is eventually consistent, achieves high availability by replicating data across multiple servers within Amazon's data centers around the world. if a change request for some data succeed, the change will store safely, However, the change must be replicated across IAM, which can take time.Such changes include creating or updating users, groups, roles, or policies. We recommend that you do not include such IAM changes in the critical, high-availability code paths of your application. Instead, make IAM changes in a separate initialization or setup routine that you run less frequently;
- **Free to use** You are charged only when you access other AWS services using your IAM users or AWS STS temporary security credentials;


###Takeaways

- IAM policies declare the access boundaries for entities within AWS;
- IAM policies consist of PRINCIPALS, ACTIONS, and RESOURCES;
- IAM policies can be used to enforce the principle of least privilege;
- IAM has many policy types - identity-based and resource-based are two examples;
- IAM evaluates access based on evaluating all policy types that are applicable for a given resource.

### Accessing IAM
You can work with AWS IAM in any of the following ways:

- Via browser based app;
- Command Line Tools either [AWS CLI](https://aws.amazon.com/cli/) and [Windows PowerShell](https://aws.amazon.com/powershell/). For information and how to use them visit [the AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/) and [AWS Tools for Windows PowerShell User Guide](https://docs.aws.amazon.com/powershell/latest/userguide/);
- AWS SDKs consist of libraries and sample code for various programming languages and platforms. See [the Tools for Amazon Web Services](https://aws.amazon.com/tools/) page;
- IAM HTTPS API which lets you issue HTTPS requests directly to the service.

### How IAM work

The IAM infrastructure includes the following elements: 

#### Terms:

In this section will define some terms will use during this IAM AWS getting started

- **Resources**: The user, group, role, policy, and identity provider objects that are stored in IAM. You can create/edit/remove esources from IAM.
- **Identities**: The IAM resource objects that are used to identify and group.
- **Entities**: used by AWS for authentication, include IAM users, federated users & assumed IAM roles.
- **Principales**: is a persone/application that can make an action/operation on AWS resource, such as S3, authenticate as IAM user/entity to make a request to AWS. for best practice do not use root user credentials for daily work, instead create IAM entity(user/role). You can also use programmatic access to allow an application to access your AWS account.

#### Request:

When a principal tries to use AWS management console, AWS API/CLI, that pricinpal send a request to AWS including those information:

- ***Actions/Operations*** that the pricipal want to perform.
- ***Resources*** upon which the action/operation are performed.
- ***Principal*** is a person/application used entity(user/role) to send a request, containing principal information like policies associated to that entity used to sign in.
- ***Environment data*** like user agent,time, IP address..
- ***Resource data*** include information about the resource is being requested such as DynamoDB table name, S3 bucket ..etc.

This request used by AWS to evaluate and authorize to perform the actions/operations.

#### Authentication

Each user might be authenticate to send their request to AWS. There is several ways of authentication to AWS like:

- Using console
  - As root user you must provide email/password.
  - As [IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) by providing accountID/alias then username & password.
- Using AWS CLI/API
  - You must provide your access key & secret key.

You might also be required to provide additional security information, for example AWS recommends MFA(multi-factor authentication).

***Note***: 
There is some services like S3 allow few requests from anonymous users.

#### Authorozation

To complete your request you must be allowed(authorized) to do. AWS use values from request context to check for policies that apply to the request, to determine whether to allow/deny your request it check those policies often stored in JSON. AWS check each policy in the context of your request if any denied permission are included then AWS denies your request(called an explicit deny).
The evaluation of a request follow those rules:

- All requests are denied by default(the opposite for root user).
- An explicit allow in any permissions policy (identity-based or resource-based) overrides this default.
- The existence of an Organizations SCP, IAM permissions boundary, or a session policy overrides the allow. If one or more of these policy types exists, they must all allow the request. Otherwise, it is implicitly denied.
- An explicit deny in any policy overrides any allows.

#### Actions/Operations

After AWS approves actions/operations in your request then you can create/view/edit/delete a targeted resource depend on operations include in request. AWS support 40 actions for a user resource.
To allow a principal to perform an operation you must include the necessary actions in a policy to the principal or the affected resource. Each service support a [specific actions & operations.](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_actions-resources-contextkeys.html)

#### Resource

After AWS approves the operations in your request, they can be performed on the related resources within your account.

### Users

If you need an access to any resource in AWS it must has permission and those permission are assigned to an IAM user, such as root user created when signed in for first time(registred for new account) who has access to all resources, it recommanded to not share those credentials(email & password) with any one & an IAM user can be an application.
If your organization have a way to be authenticated then use those credentials to access AWS account by federating that user to your AWS account as an example using auth provider like facebook, google or amazon account or maybe use active directory.

### Policies

#### Policies & Account
In case using several accounts under one main AWS account, to mange each account permission, you can use IAM role, resource-based policies or ACL(access control list) for cross-account permissions.

#### Policies & Users
After creating IAM user, they can't access/perform actions/operation until you give them a permission, to do so, you use identity-based policy, which is attached to user/group(contain several users) this is an example of JSON policy:
```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "dynamodb:*",
    "Resource": "arn:aws:dynamodb:us-east-2:123456789012:table/Books"
  }
}
```

this policy allow to perform any action on `dynamodb`, but only on `Books` table within `us-east-2` region, so any other action are prohibited. Each serivce has a determined policies.
Policies are summarized in three tables: the [policy summary](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_understand-policy-summary.html), the [service summary](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_understand-service-summary.html), and the [action summary](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_understand-action-summary.html).

***Note***: To assign permissions to federated users, you can create an entity referred to as a role and define permissions for the role.

#### Policies & Groups

You can create group to englobe users and attach to that group a policies to manage permission easily. Those users still have their permissions(if have any). Users & groups can have serveral policies attached to them.

#### Identity-based and Resource-based Policies

Identity-based policies are permissions policy attached to an IAM user, group or role & resource-based policies are permissions attached to AWS resource such as S3 bucket or IAM role trust policy.

***Identity-based policies*** perform precise actions on determined resource & under given condition, they categorized to:

- ***AWS Managed policies***: tandalone identity-based policies that you can attach to multiple users, groups, and roles in your AWS account. It has 2 types:
  
  - AWS managed policies: a bunch of policies determined by AWS(recommanded for new).
  - Customer managed policies: you create & manage them in your accont.

- ***Inline policies***: you created & managed them, they are embedded directly into single user. group or role(not recommanded).

***Resource-based policies*** control what actions to perform by a specified principal on that resource & under which conditions. Resource-based policies are inline policies. The IAM service supports only one type of resource-based policy called a role trust policy, which is attached to an IAM role(role is both an identity and a resource that supports resource-based policies). Trust policies define which principal entities cam assume the role.

### ABAC(atributes based access control)

Another authorization strategy by defining permission on those attributes(called Tags), they can attached to IAM principals & AWS resources. These ABAC policies can be designed to allow operations when the principal's tag matches the resource tag, they used in environment where it grow rapidly where policy management becomes complicated.

#### ABAC advantages over RBAC(role based)

- ABAC permissions scale with innovation wich mean it's not necessary to update your existing policies to allow accessing new resource.
- ABAC requires fewer policies(to mange easily your policies).
- Using ABAC, teams can change and grow quickly that appear when a membre of your team move to another team with different perissions or creating access to new resource, IAM admin can assign user to other IAM role.
- Granular permissions are possible using ABAC
- Use employee attributes from your corporate directory with ABAC

IAM is not used to security in your services but let you control how these AWS products administred as an example creating terminating an instance of EC2, be sure to read the documentation to learn the security options for all the resources that belong to that product.

***Note***:
To get help with [common tasks](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_quick-links-common-tasks.html) associated with IAM.

### Getting started with IAM

#### Creating Admin User & Group

##### Creating IAM admin user

To create new admin user you must the following steps:

- Go to [IAM](https://console.aws.amazon.com/iam/) section on your console by sign in using root user credentials(email/password).
- Enable access to billing data for new admin user
  - On navigation bar choose ***your account name*** then choose ***My Account***
  - Scroll to ***IAM User and Role Access to Billing Information*** click ***edit***
  - Make sure to check ***Activate IAM Access*** and click ***update***
  - Go back to [IAM](https://console.aws.amazon.com/iam/)

- In the right menu click on ***User*** after that click ***add user**
- On ***Detail*** page do the following:
  1- Add a ***name*** to new user
  2- Check the box for ***AWS Management Console access***, select ***custom password*** and type new user password
  3- if you want to force new user to retype new password then check the box ***User must create a new password at next sign-in*** otherwise uncheck it
  & then click ***Next: Permissions***
  
- On ***Permissions** page follow those steps:
  1- Choose ***Add user to group***(at top)
  2- Click ***Add group*** if no group created yet a model box will appear
  3- Give a name to new grou on ***Group name*** field in this case `Admins`
  4- Select the check box for the ***AdministratorAccess*** policy
  5- Click ***Create group*** then back
  6- Select the check box of new group or click ***Refresh*** if not displayed(most of the time will checked by default)
  7- Click ***Next: Tags***

- Optional) On the Tags page, add metadata to the user by attaching tags as key-value pairs
- Click ***Next: Review***, in this section you make sure to verify that this new user account are added to right group & with exact details.
- Click ***Create user***
- (Optional) On the Complete page, you can download a .csv file with login information for the user, or send email with login instructions to the user

##### Using AWS CLIS

1- Firstly, create a new group with AWS cli you type in a terminal:

```shell
aws iam create-group --group-name <name-of-group>
```

You replace <name-of-group> with the group name such as `Admins` however the name is case insensitive with max length 128 characters & it can include `+`,`,`, `.`, `@`, `_`, `-` & `=`. After typing the command above a result will print in your terminal look like:

```json
{
    "Group": {
        "Path": "/",
        "GroupName": "admins",
        "GroupId": "AGPAYSB3R37MZORCMVE3Z",
        "Arn": "arn:aws:iam::588535422937:group/admins",
        "CreateDate": "2020-06-04T21:14:24+00:00"
    }
}
```
To list all group you have created type:

```shell
aws iam list-groups
```

then you will see a results printed to your terminal look like:

```json
{
    "Groups": [
        {
            "Path": "/",
            "GroupName": "Administrators",
            "GroupId": "AGPAYSB3R37M4J2NOFAG2",
            "Arn": "arn:aws:iam::588535422937:group/Administrators",
            "CreateDate": "2020-06-04T02:20:10+00:00"
        },
        {
            "Path": "/",
            "GroupName": "admins",
            "GroupId": "AGPAYSB3R37MZORCMVE3Z",
            "Arn": "arn:aws:iam::588535422937:group/admins",
            "CreateDate": "2020-06-04T21:14:24+00:00"
        }
    ]
}
```
`Arn` is amazon resource name for your group & the digit number in the ARN is your AWS account ID.

2- Secondly, attach a policy to that new group, so will attach a full access policy which is `AdministratorAccess` you can find aws managed [policies](https://console.aws.amazon.com/iam/home?#/policies), this command uses the ARN for policy.

```shell
aws iam attach-group-policy --group-name admins --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```
If the command is successful, there is no response printed in terminal, however to see the policies attached to a group you can type

```shell
aws iam list-attached-group-policies --group-name admins
```
& the results is a list of attached policies to targeted group(`admins` in this case)
```json
{
    "AttachedPolicies": [
        {
            "PolicyName": "AdministratorAccess",
            "PolicyArn": "arn:aws:iam::aws:policy/AdministratorAccess"
        }
    ]
}
```
those results tell us tha a policy named `AdministratorAccess` attached to targeted group. To see the [information about policy](https://docs.aws.amazon.com/cli/latest/reference/iam/get-policy.html) including group, user & role attached to, use this:
```shell
aws iam get-policy --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```
3- Thirdthly, creating new user. To do so will using [create-user](https://docs.aws.amazon.com/cli/latest/reference/iam/create-user.html) command like:
```shell
aws iam create-user --user-name admin1
```
if it was created successfully a below results will printed in your cli
```json
{
    "User": {
        "Path": "/",
        "UserName": "admin1",
        "UserId": "AIDAYSB3R37MQC4WUAALV",
        "Arn": "arn:aws:iam::588535422937:user/admin2",
        "CreateDate": "2020-06-05T03:53:51+00:00"
    }
}
```

This user has no password to use for sign in to console, to create new password for this new user will use [create-login-profile](https://docs.aws.amazon.com/cli/latest/reference/iam/create-login-profile.html) following those recommanded method:

- Create a json file in your current directory using

```shell
aws iam create-login-profile --user-name admin1 --generate-cli-skeleton > admin1.json
```

- Open that created file `admin1.json` using your text editor, it will look like
```json
{
    "UserName": "",
    "Password": "",
    "PasswordResetRequired": true
}
```
- Edit this file by typing in `UserName` user name, in `Password` his password & in `PasswordResetRequired` to force user to set a given password when first time sign in(`true`) or not(`false`).

***Note***:
If you want to update his password then use [update-login-profile](https://docs.aws.amazon.com/cli/latest/reference/iam/update-login-profile.html) instead.
You can create access key for given user to use `AWS CLI` by using the following command
```shell
aws iam create-access-key --user-name userAccounName
```
replace `userAccounName` with the given user.


4- Forthly, add created user to a specified group by typing
```shell
aws iam add-user-to-group --group-name admins --user-name admin1
```
If nothing was printed then that mean the user was added to that group successfully.

***Note***:
To delete specified user from specified group use:
```shell
aws iam remove-user-from-group --user-name admin1 --group-name admins
```
& for user removing use:
```shell
aws iam delete-user --user-name admin1
```
this work only if that user has not added to any group & not configured for login profile, however its will not working till you remove login profile & all it config, although it will delete the user if you use the console.

***Note***:
For Creating a Delegated User/Group & adding a managed policies visit [this doc page](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-delegated-user.html).

#### Sign in to your account

IAM user can sign in to console following a link look like `https://Your_Account_ID.signin.aws.amazon.com/console/`, the `Your_Account_ID` is your account ID. To get your accountID follow:

- if your are already signed in then on the top bar menu click `Support` then `Support Center` and you will see your id on the top of left pane menu near to `Account number: Your_Account_ID`. 
- If its your first time then ask the admin to give you more detail on your account.
In case you are using `AWS CLI` then type on your terminal:
  ```shell
  aws sts get-caller-identity
  ```
  a `JSON` document printed to your terminal, it look like:
  ```json
  {
      "UserId": "AIDAYSB3R37MYSQ55VFR7",
      "Account": "588535422937",
      "Arn": "arn:aws:iam::588535422937:user/Administrator"
  }
  ```
  Account value, in above case is `588535422937`, is your account id.

- Sign in using account alias, to do so your must create an alias for your account following those insctructions:
  1- Navigate to [IAM](https://console.aws.amazon.com/iam/).
  2- Click on left pane on `Dashboard`
  3- On top you can see `IAM users sign-in link`
  4- Click on `Customize` near to a link look like `https://Your_Account_ID.signin.aws.amazon.com/console `
  5- Type the name you want, however this name must be unique to all AWS accounts and the name is combination of lower case lettres, numbers & `-`.

***Note***:
If you want to remove this alias click on `Customize` again then you will find a `Yes, delete` button click it then it will removed.

Creating acount alias usgin `AWS CLI` you can just run this
```shell
aws iam create-account-alias --account-alias aliasname
```
replacing `aliasname` with your alias name then if nothing printed to your terminal then it created successfully, othrewise choose another alias name & try till created one. To check again use this command
```shell
aws iam list-account-aliases
```
it will list all available aliases like
```json
{
    "AccountAliases": [
        "dayenio"
    ]
}
```

To delete an alias run
```shell
aws iam delete-account-alias --account-alias aliasname
```

After creating an alias for your account you can use it by replacing your `Your_Account_ID` with it to become `https://account-ID-or-alias.signin.aws.amazon.com/console` to sign in to console.

##### IAM Console Search
In case you want to search for some feature in IAM then user a IAM search section which can locate any of the following:
- IAM entity names that match your search keywords (for users, groups, roles, identity providers, and policies) 
- AWS documentation topic names that match your search keywords
- Tasks that match your search keywords

To use search feature navigate to main [IAM page](https://console.aws.amazon.com/iam/) on the left pane menu look for search box click on it then type your search query, a list of results will display each item is link lead you to that page.

##### Delegate Access to the Billing Console

To delegate an IAM user to access the AWS Biilling & Cost mangement data will make a scenario to walk you through those steps, they are 4, as the mentioned below:

1. Enable access to billing & cost management:
First, activate billing access for your test users. To do this, see [Activating Access to the Billing and Cost Management Console](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/control-access-billing.html) in the AWS Billing and Cost Management User Guide.
2. Create policies: 
Second, you must follow those steps:
  
    a. Sign in to console as user with administrator credentials
    b. Go to [IAM home page](https://console.aws.amazon.com/iam/)
    c. On left navigation pane click `Policies`
    d. Click `Create policy` button on top
    e. On `Visual editor` editor tab click `Choose a service`(look like a link) then choose `Billing`
    f. For full access to the billing & cost management check the box of `All Billing actions` & for read-only access select the check box next to `Read`
    g. Click `Review policy` give it a name like *BillingFullAccess* for full access & *BillingViewAccess* for read-only access then click `Create policy` button.

3. Attach policies:
Third step is to attach created policies from above, which are *BillingFullAccess* & *BillingViewAccess* to the right group, user or role, but for [best practice](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) will use groups then add users who need those permission to the right group. You can do that following:

    a. Navigate to `Policies` section using left navigation
    b. In search box type your policy, one of *BillingFullAccess* or *BillingViewAccess*, then select the check box on the left of policy name
    c. Click on `Policy actions` on top then click `Attach`
    d. Finally select the check box at left of  name of group after that click `Attach policy`

4. Test if it work as expected
To test if it worck well sign in using one user from each group *BillingFullAccess* & *BillingViewAccess*. In console header menu click on On the navigation bar, choose username@<account alias or ID number> & choose `My Billing Dashboard`. finally, on this page mess arround by selecting a check boxes & try to save for user in group who has policy *BillingFullAccess* they can update but for user in group who has *BillingViewAccess* they can't edit anything.

