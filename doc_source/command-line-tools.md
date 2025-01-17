# AWS CloudHSM command line tools<a name="command-line-tools"></a>

This topic describes the command line tools available for managing and using AWS CloudHSM\.

**Topics**
+ [Understanding command line tools](#command-line-tools-intro)
+ [CloudHSM Management Utility \(CMU\)](cloudhsm_mgmt_util.md)
+ [Key Management Utility \(KMU\)](key_mgmt_util.md)
+ [Configure tool](configure-tool.md)

## Understanding command line tools<a name="command-line-tools-intro"></a>

In addition to the AWS command\-line interface \(AWS CLI\) that you use for managing your AWS resources, AWS CloudHSM offers command\-line tools for managing HSM users and creating and managing keys on the HSM\. In CloudHSM, you use the familiar AWS CLI to manage your cluster, and the CloudHSM command line tools to manage your HSM\.

These are the various command\-line tools:

**Manage HSM and Clusters**  
These tools get, create, delete, and tag AWS CloudHSM clusters and HSMs:  
+ [CloudHSMv2 commands in AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/index.html)\. To use these commands, you need to [install](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) and [configure](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration) AWS CLI\.
+ HSM2 PowerShell cmdlets in the [AWSPowerShell module](https://aws.amazon.com/powershell/)\. These cmdlets are available in a Windows PowerShell module and a cross\-platform PowerShell Core module\.

   

**Manage HSM Users**  
This tool creates and deletes HSM users, including implementing quorum authentication of user management tasks:  
+ [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md)\. This tool is included in the AWS CloudHSM client software\.

   

**Manage Keys on the HSM**  
This tool creates, deletes, imports, and exports symmetric keys and asymmetric key pairs:  
+ [key\_mgmt\_util](key_mgmt_util.md)\. This tool is included in the AWS CloudHSM client software\.

   

**Helper Tools**  
These tools help you to use the tools and software libraries\.  
+ [configure](configure-tool.md) updates your CloudHSM client configuration files\. This enables the AWS CloudHSM to synchronize the HSMs in a cluster\.
+ [pkpspeed](troubleshooting-verify-hsm-performance.md) measures the performance of your HSM hardware independent of software libraries\. 

   