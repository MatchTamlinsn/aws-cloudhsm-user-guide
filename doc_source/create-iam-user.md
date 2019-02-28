# Create IAM Administrative Groups<a name="create-iam-user"></a>

As a [best practice](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#create-iam-users), don't use your AWS account root user to interact with AWS, including AWS CloudHSM\. Instead, use AWS Identity and Access Management \(IAM\) to create an IAM user, IAM role, or federated user\. Follow the steps in the [Create an IAM User and Administrator Group](#create-iam-admin) section to create an administrator group and attach the **AdministratorAccess** policy to it\. Then create a new administrator user and add the user to the group\. Add additional users to the group as needed\. Each user you add inherits the **AdministratorAccess** policy from the group\. 

Another best practice is to create an AWS CloudHSM administrator group that has only the permissions required to run AWS CloudHSM\. Add individual users to this group as needed\. Each user inherits the limited permissions that are attached to the group rather than full AWS access\. The [Restrict User Permissions to What's Necessary for AWS CloudHSM](#permissions-for-cloudhsm) section that follows contains the policy that you should attach to your AWS CloudHSM administrator group\. 

AWS CloudHSM defines an [ IAM service–linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) for your AWS account\. The service–linked role currently defines permissions that allow your account to log AWS CloudHSM events\. The role can be created automatically by AWS CloudHSM or manually by you\. You cannot edit the role, but you can delete it\. For more information, see the [Understanding Service\-Linked Roles](#service-linked-roles) section that follows\. 

**Topics**
+ [Create an IAM User and Administrator Group](#create-iam-admin)
+ [Restrict User Permissions to What's Necessary for AWS CloudHSM](#permissions-for-cloudhsm)
+ [Understanding Service\-Linked Roles](#service-linked-roles)

## Create an IAM User and Administrator Group<a name="create-iam-admin"></a>

Start by creating an IAM user along with an administrator group for that user\.

**To create an IAM user for yourself and add the user to an Administrators group**

1. Use your AWS account email address and password to sign in as the *[AWS account root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)* to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user below and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane of the console, choose **Users**, and then choose **Add user**\.

1. For **User name**, type **Administrator**\.

1. Select the check box next to **AWS Management Console access**, select **Custom password**, and then type the new user's password in the text box\. You can optionally select **Require password reset** to force the user to create a new password the next time the user signs in\.

1. Choose **Next: Permissions**\.

1. On the **Set permissions** page, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, for **Group name** type **Administrators**\.

1. For **Filter policies**, select the check box for **AWS managed \- job function**\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Tags** to add metadata to the user by attaching tags as key\-value pairs\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users, and to give your users access to your AWS account resources\. To learn about using policies to restrict users' permissions to specific AWS resources, go to [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

You can create multiple administrators in your account and add each to the Administrators group\. To sign in to the AWS Management Console, each user needs an AWS account ID or alias\. To get these, see [Your AWS Account ID and Its Alias](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html) in the *IAM User Guide*\. 

## Restrict User Permissions to What's Necessary for AWS CloudHSM<a name="permissions-for-cloudhsm"></a>

We recommend that you create an IAM administrators group for AWS CloudHSM that contains only the permissions required to run AWS CloudHSM\. Attach the policy below to your group\. Add IAM users to the group as needed\. Each user that you add inherits the policy from the group\. 

In addition to an IAM administrators group, we recommend that you create user groups with appropriate permissions to ensure that only trusted users have access to critical API operations\. For example, you may want to create a read\-only user group that permits access only to the [DescribeClusters](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeClusters.html) and [DescribeBackups](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeBackups.html)\. Make sure that the group does not allow a user to *delete* clusters or HSMs\. This way, you won't have to worry about an untrusted user affecting availability of a production workload\.

As new AWS CloudHSM management features are added over time, you can ensure that only trusted users are given immediate access\. By assigning limited permissions to policies at creation, you can manually assign new feature permissions to them later\.

**To create a customer managed policy**

1. Sign in to the IAM console using the credentials of an AWS administrator\.

1. In the navigation pane, choose **Policies**\.

1. Choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Copy one of the following policies and paste it into the **JSON** editor based on the type of user policy you wish to make\.

------
#### [ Read\-Only User ]

   ```
   {
      "Version": "2012-10-17",
      "Statement": {
         "Effect": "Allow",
         "Action": [
            "cloudhsm:DescribeClusters",
            "cloudhsm:DescribeBackups",
            "cloudhsm:ListTags"
         ],
         "Resource": "*"
      }
   }
   ```

   The preceding policy allows access to the AWS CloudHSM `DescribeClusters` and `DescribeBackups` API operations\. It also includes additional permissions for select Amazon Elastic Compute Cloud \(Amazon EC2\) actions\. However, it does not allow the user to delete clusters or HSMs\.

------
#### [ Power User ]

   ```
   {
      "Version": "2012-10-17",
      "Statement": {
         "Effect": "Allow",
         "Action": [
            "cloudhsm:DescribeClusters",
            "cloudhsm:DescribeBackups",
            "cloudhsm:CreateCluster",
            "cloudhsm:CreateHsm",
            "cloudhsm:RestoreBackup",
            "cloudhsm:CopyBackupToRegion",
            "cloudhsm:InitializeCluster",
            "cloudhsm:ListTags",
            "cloudhsm:TagResource",
            "cloudhsm:UntagResource",
            "ec2:CreateNetworkInterface",
            "ec2:DescribeNetworkInterfaces",
            "ec2:DescribeNetworkInterfaceAttribute",
            "ec2:DetachNetworkInterface",
            "ec2:DeleteNetworkInterface",
            "ec2:CreateSecurityGroup",
            "ec2:AuthorizeSecurityGroupIngress",
            "ec2:AuthorizeSecurityGroupEgress",
            "ec2:RevokeSecurityGroupEgress",
            "ec2:DescribeSecurityGroups",
            "ec2:DeleteSecurityGroup",
            "ec2:CreateTags",
            "ec2:DescribeVpcs",
            "ec2:DescribeSubnets",
            "iam:CreateServiceLinkedRole"
         ],
         "Resource": "*"
      }
   }
   ```

   The preceding policy allows access to the AWS CloudHSM `DescribeClusters`, `DescribeBackups`, `CreateCluster`, `CreateHsm`, `RestoreBackup`, `CopyBackupToRegion`, `InitializeCluster`, `ListTags`, `TagResources`, and `UntagResources` API operations\. It also includes additional permissions for select Amazon Elastic Compute Cloud \(Amazon EC2\) actions\. However, it does not allow the user to delete clusters or HSMs\.

------
#### [ Admin User ]

   ```
   {
      "Version":"2012-10-17",
      "Statement":{
         "Effect":"Allow",
         "Action":[
            "cloudhsm:*",
            "ec2:CreateNetworkInterface",
            "ec2:DescribeNetworkInterfaces",
            "ec2:DescribeNetworkInterfaceAttribute",
            "ec2:DetachNetworkInterface",
            "ec2:DeleteNetworkInterface",
            "ec2:CreateSecurityGroup",
            "ec2:AuthorizeSecurityGroupIngress",
            "ec2:AuthorizeSecurityGroupEgress",
            "ec2:RevokeSecurityGroupEgress",
            "ec2:DescribeSecurityGroups",
            "ec2:DeleteSecurityGroup",
            "ec2:CreateTags",
            "ec2:DescribeVpcs",
            "ec2:DescribeSubnets",
            "iam:CreateServiceLinkedRole"
         ],
         "Resource":"*"
      }
   }
   ```

   The preceding policy allows access a AWS CloudHSM API operations, *including the ability to delete HSMs and clusters*\. It also includes additional permissions for select Amazon Elastic Compute Cloud \(Amazon EC2\) actions\. 

------

1. Choose **Review policy**\.

1. For **Name**, type a relevant policy name\. For instance, if this is a policy for read\-only users, you might use **CloudHsmReadOnlyUserPolicy**\.

1. \(Optional\) Type a description\.

1. Choose **Create policy**\.

The preceding policies include the `iam:CreateServiceLinkedRole` action\. You must include this action to allow AWS CloudHSM to automatically create the AWSServiceRoleForCloudHSM service\-linked role in your account\. This role allows AWS CloudHSM to log events\. See the following section for more information about the AWSServiceRoleForCloudHSM service\-linked role\. 

**To create an AWS CloudHSM user group**

1. Sign in to the IAM console using the credentials of an AWS administrator\.

1. In the navigation pane, choose **Groups**\.

1. Choose **Create New Group**\.

1. For **Group Name**, type a relevant user group name, such as **CloudHsmReadOnlyUsers**\. 

1. For **Policy Type**, choose **Customer Managed**\.

1. Select the check box for preferred user policy and choose **Next Step**\.

1. Choose **Create Group**\.

**Note**  
When you use the AWS CloudHSM console or API, AWS CloudHSM takes additional actions on your behalf to manage certain Amazon EC2 resources\. This happens, for example, when you create and delete clusters and HSMs\.

## Understanding Service\-Linked Roles<a name="service-linked-roles"></a>

The IAM policy that you created previously to [Restrict User Permissions to What's Necessary for AWS CloudHSM](#permissions-for-cloudhsm) includes the `iam:CreateServiceLinkedRole` action\. AWS CloudHSM defines a [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) named AWSServiceRoleForCloudHSM\. The role is predefined by AWS CloudHSM and includes permissions that AWS CloudHSM requires to call other AWS services on your behalf\. The role makes setting up your service easier because you don’t need to manually add the role policy and trust policy permissions\. 

The role policy allows AWS CloudHSM to create Amazon CloudWatch Logs log groups and log streams and write log events on your behalf\. You can view it below and in the IAM console\. 

```
{
    "Version": "2018-06-12",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogStreams"
            ],
            "Resource": [
                "arn:aws:logs:*:*:*"
            ]
        }
    ]
}
```

The trust policy for the AWSServiceRoleForCloudHSM role allows AWS CloudHSM to assume the role\.

```
{
  "Version": "2018-06-12",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudhsm.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### Creating a Service\-Linked Role \(Automatic\)<a name="create-slr-auto"></a>

AWS CloudHSM creates the AWSServiceRoleForCloudHSM role when you create a cluster if you include the `iam:CreateServiceLinkedRole` action in the permissions that you defined when you created the AWS CloudHSM administrators group\. See [Restrict User Permissions to What's Necessary for AWS CloudHSM](#permissions-for-cloudhsm)\. 

If you already have one or more clusters and just want to add the AWSServiceRoleForCloudHSM role, you can use the console, the [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/create-cluster.html) command, or the [CreateCluster](AWS CloudHSM API ReferenceAPI_CreateCluster.html) API operation to create a cluster\. Then use the console, the [delete\-cluster](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/delete-cluster.html) command, or the [DeleteCluster](AWS CloudHSM API ReferenceAPI_DeleteCluster.html) API operation to delete it\. Creating the new cluster creates the service\-linked role and applies it to all clusters in your account\. Alternatively, you can create the role manually\. See the following section for more information\. 

**Note**  
You do not need to perform all of the steps outlined in [Getting Started with AWS CloudHSM](getting-started.md) to create a cluster if you are only creating it to add the AWSServiceRoleForCloudHSM role\.

### Creating a Service\-Linked Role \(Manual\)<a name="create-slr-manual"></a>

You can use the IAM console, AWS CLI, or API to create the AWSServiceRoleForCloudHSM service\-linked role\. For more information, see [Creating a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. 

### Editing the Service\-Linked Role<a name="edit-slr"></a>

AWS CloudHSM does not allow you to edit the AWSServiceRoleForCloudHSM service\-linked role\. After the role is created, for example, you cannot change its name because various entities might reference the role by name\. Also, you cannot change the role policy\. You can, however, use IAM to edit the role description\. For more information, see [Editing a Service–Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\. 

### Deleting the Service\-Linked Role<a name="delete-slr"></a>

You cannot delete a service\-linked role as long as a cluster to which it has been applied still exists\. To delete the role, you must first delete each HSM in your cluster and then delete the cluster\. Every cluster in your account must be deleted\. You can then use the IAM console, AWS CLI, or API to delete the role\. For more information about deleting a cluster, see [Deleting an AWS CloudHSM Cluster](delete-cluster.md)\. For more information, about deleting a role, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\. 