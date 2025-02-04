# Using Identity\-Based Policies \(IAM Policies\) for Amazon Redshift<a name="redshift-iam-access-control-identity-based"></a>

This topic provides examples of identity\-based policies in which an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\)\. 

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available for you to manage access to your Amazon Redshift resources\. For more information, see [Overview of Managing Access Permissions to Your Amazon Redshift Resources](redshift-iam-access-control-overview.md)\.

The following shows an example of a permissions policy\. The policy allows a user to create, delete, modify, and reboot all clusters, and then denies permission to delete or modify any clusters where the cluster identifier starts with `production`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowClusterManagement",
      "Action": [
        "redshift:CreateCluster",
        "redshift:DeleteCluster",
        "redshift:ModifyCluster",
        "redshift:RebootCluster"
      ],
      "Resource": [
        "*"
      ],
      "Effect": "Allow"
    },
    {
      "Sid":"DenyDeleteModifyProtected",
      "Action": [
        "redshift:DeleteCluster",
        "redshift:ModifyCluster"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:cluster:production*"
      ],
      "Effect": "Deny"
    }
  ]
}
```

The policy has two statements: 
+ The first statement grants permissions for a user to a user to create, delete, modify, and reboot clusters\. The statement specifies a wildcard character \(\*\) as the `Resource` value so that the policy applies to all Amazon Redshift resources owned by the root AWS account\. 
+ The second statement denies permission to delete or modify a cluster\. The statement specifies a cluster Amazon Resource Name \(ARN\) for the `Resource` value that includes a wildcard character \(\*\)\. As a result, this statement applies to all Amazon Redshift clusters owned by the root AWS account where the cluster identifier begins with `production`\.

## Permissions Required to Use Redshift Spectrum<a name="redshift-spectrum-policy-resources"></a>

Redshift Spectrum requires permissions to other AWS services to access resources\. For details about permissions in IAM policies for Redshift Spectrum, see [IAM Policies for Amazon Redshift Spectrum](https://docs.aws.amazon.com/redshift/latest/dg/c-spectrum-iam-policies.html) in the Amazon Redshift Database Developer Guide\.

## Permissions Required to Use the Amazon Redshift Console<a name="redshift-policy-resources.required-permissions.console"></a>

For a user to work with the Amazon Redshift console, that user must have a minimum set of permissions that allows the user to describe the Amazon Redshift resources for their AWS account\. These permissions must also allow the user to describe other related information, including Amazon EC2 security and network information\.

If you create an IAM policy that is more restrictive than the minimum required permissions, the console doesn't function as intended for users with that IAM policy\. To ensure that those users can still use the Amazon Redshift console, also attach the `AmazonRedshiftReadOnlyAccess` managed policy to the user, as described in [AWS\-Managed \(Predefined\) Policies for Amazon Redshift](#redshift-policy-resources.managed-policies)\.

To give a user access to the Query Editor on the Amazon Redshift console, attach the `AmazonRedshiftQueryEditor` managed policy\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the Amazon Redshift API\. 

## Resource Policies for GetClusterCredentials<a name="redshift-policy-resources.getclustercredentials-resources"></a>

To connect to a cluster database using a JDBC or ODBC connection with IAM database credentials, or to programmatically call the `GetClusterCredentials` action, you need a minimum set of permissions\. At a minimum, you need permission to call the `redshift:GetClusterCredentials` action with access to a `dbuser` resource\.

If you use a JDBC or ODBC connection, instead of `server` and `port` you can specify `cluster_id` and `region`, but to do so your policy must permit the `redshift:DescribeClusters` action with access to the `cluster` resource\. 

If you call `GetClusterCredentials` with the optional parameters `Autocreate`, `DbGroups`, and `DbName`, you need to also allow the actions and permit access to the resources listed in the following table\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-access-control-identity-based.html)

For more information about resources, see [Amazon Redshift Resources and Operations](redshift-iam-access-control-overview.md#redshift-iam-accesscontrol.actions-and-resources)\.

You can also include the following conditions in your policy:
+ `redshift:DurationSeconds`
+ `redshift:DbName`
+ `redshift:DbUser`

For more information about conditions, see [Specifying Conditions in a Policy](redshift-iam-access-control-overview.md#redshift-policy-resources.specifying-conditions)

## AWS\-Managed \(Predefined\) Policies for Amazon Redshift<a name="redshift-policy-resources.managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. Managed policies grant necessary permissions for common use cases so you can avoid having to investigate what permissions are needed\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

The following AWS managed policies, which you can attach to users in your account, are specific to Amazon Redshift:
+ **AmazonRedshiftReadOnlyAccess** – Grants read\-only access to all Amazon Redshift resources for the AWS account\.
+ **AmazonRedshiftFullAccess** – Grants full access to all Amazon Redshift resources for the AWS account\.
+ **AmazonRedshiftQueryEditor** – Grants full access to the Query Editor on the Amazon Redshift console\.

You can also create your own custom IAM policies to allow permissions for Amazon Redshift API actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. 

## Customer Managed Policy Examples<a name="redshift-iam-accesscontrol.examples"></a>

In this section, you can find example user policies that grant permissions for various Amazon Redshift actions\. These policies work when you are using the Amazon Redshift API, AWS SDKs, or the AWS CLI\. 

**Note**  
All examples use the US West \(Oregon\) Region \(`us-west-2`\) and contain fictitious account IDs\.

### Example 1: Allow User Full Access to All Amazon Redshift Actions and Resources<a name="redshift-policy-example-allow-full-access"></a>

The following policy allows access to all Amazon Redshift actions on all resources\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowRedshift",
      "Action": [
        "redshift:*"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

The value `redshift:*` in the `Action` element indicates all of the actions in Amazon Redshift\.

### Example 2: Deny a User Access to a Set of Amazon Redshift Actions<a name="redshift-policy-example-deny-specific-actions"></a>

By default, all permissions are denied\. However, sometimes you need to explicitly deny access to a specific action or set of actions\. The following policy allows access to all the Amazon Redshift actions and explicitly denies access to any Amazon Redshift action where the name starts with `Delete`\. This policy applies to all Amazon Redshift resources in `us-west-2`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowUSWest2Region",
      "Action": [
        "redshift:*"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:redshift:us-west-2:*"
    },
   {
     "Sid":"DenyDeleteUSWest2Region",
     "Action": [
        "redshift:Delete*"
      ],
      "Effect": "Deny",
      "Resource": "arn:aws:redshift:us-west-2:*"
   }
  ]
}
```

### Example 3: Allow a User to Manage Clusters<a name="redshift-policy-example-allow-manage-clusters"></a>

The following policy allows a user to create, delete, modify, and reboot all clusters, and then denies permission to delete any clusters where the cluster name starts with `protected`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowClusterManagement",
      "Action": [
        "redshift:CreateCluster",
        "redshift:DeleteCluster",
        "redshift:ModifyCluster",
        "redshift:RebootCluster"
      ],
      "Resource": [
        "*"
      ],
      "Effect": "Allow"
    },
    {
      "Sid":"DenyDeleteProtected",
      "Action": [
        "redshift:DeleteCluster"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:cluster:protected*"
      ],
      "Effect": "Deny"
    }
  ]
}
```

### Example 4: Allow a User to Authorize and Revoke Snapshot Access<a name="redshift-policy-example-allow-authorize-revoke-snapshot"></a>

The following policy allows a user, for example User A, to do the following:
+ Authorize access to any snapshot created from a cluster named `shared`\.
+ Revoke snapshot access for any snapshot created from the `shared` cluster where the snapshot name starts with `revokable`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowSharedSnapshots",
      "Action": [
        "redshift:AuthorizeSnapshotAccess"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:shared/*"
      ],
      "Effect": "Allow"
    },
    {
      "Sid":"AllowRevokableSnapshot",
      "Action": [
        "redshift:RevokeSnapshotAccess"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:snapshot:*/revokable*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

If User A has allowed User B to access a snapshot, User B must have a policy such as the following to allow User B to restore a cluster from the snapshot\. The following policy allows User B to describe and restore from snapshots, and to create clusters\. The name of these clusters must start with `from-other-account`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowDescribeSnapshots",
      "Action": [
        "redshift:DescribeClusterSnapshots"
      ],
      "Resource": [
        "*"
      ],
      "Effect": "Allow"
    },
    {
      "Sid":"AllowUserRestoreFromSnapshot",
      "Action": [
        "redshift:RestoreFromClusterSnapshot"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:snapshot:*/*",
        "arn:aws:redshift:us-west-2:444455556666:cluster:from-other-account*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

### Example 5: Allow a User to Copy a Cluster Snapshot and Restore a Cluster from a Snapshot<a name="redshift-policy-example-allow-copy-restore-snapshot"></a>

The following policy allows a user to copy any snapshot created from the cluster named `big-cluster-1`, and restore any snapshot where the snapshot name starts with `snapshot-for-restore`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowCopyClusterSnapshot",
      "Action": [
        "redshift:CopyClusterSnapshot"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:snapshot:big-cluster-1/*"
      ],
      "Effect": "Allow"
    },
    {
      "Sid":"AllowRestoreFromClusterSnapshot",
      "Action": [
        "redshift:RestoreFromClusterSnapshot"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:snapshot:*/snapshot-for-restore*",
        "arn:aws:redshift:us-west-2:123456789012:cluster:*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

### Example 6: Allow a User Access to Amazon Redshift, and Common Actions and Resources for Related AWS Services<a name="redshift-policy-example-allow-related-services"></a>

 The following example policy allows access to all actions and resources for Amazon Redshift, Amazon Simple Notification Service \(Amazon SNS\), and Amazon CloudWatch\. It also allows specified actions on all related Amazon EC2 resources under the account\. 

**Note**  
 Resource\-level permissions are not supported for the Amazon EC2 actions that are specified in this example policy\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowRedshift",
      "Effect": "Allow",
      "Action": [
        "redshift:*"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Sid":"AllowSNS",
      "Effect": "Allow",
        "Action": [
          "sns:*"
        ],
        "Resource": [
          "*"
        ]
      },
    {
      "Sid":"AllowCloudWatch",
      "Effect": "Allow",
      "Action": [
        "cloudwatch:*"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Sid":"AllowEC2Actions",
      "Effect": "Allow",
      "Action": [
        "ec2:AllocateAddress",
        "ec2:AssociateAddress",
        "ec2:AttachNetworkInterface",
        "ec2:DescribeAccountAttributes",
        "ec2:DescribeAddresses",
        "ec2:DescribeAvailabilityZones",
        "ec2:DescribeInternetGateways",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeSubnets",
        "ec2:DescribeVpcs"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

## Example Policy for Using GetClusterCredentials<a name="redshift-policy-examples-getclustercredentials"></a>

The following policy uses these sample parameter values:
+ Region: `us-west-2` 
+ AWS Account: `123456789012` 
+ Cluster name: `examplecluster` 

The following policy enables the `GetCredentials`, `CreateClusterUser`, and `JoinGroup` actions\. The policy uses condition keys to allow the `GetClusterCredentials` and `CreateClusterUser` actions only when the AWS user ID matches `"AIDIODR4TAW7CSEXAMPLE:${redshift:DbUser}@yourdomain.com"`\. IAM access is requested for the `"testdb"` database only\. The policy also allows users to join a group named `"common_group"`\. 

```
{
"Version": "2012-10-17",
  "Statement": [
    {
     "Sid": "GetClusterCredsStatement",
      "Effect": "Allow",
      "Action": [
        "redshift:GetClusterCredentials"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:dbuser:examplecluster/${redshift:DbUser}",
        "arn:aws:redshift:us-west-2:123456789012:dbname:examplecluster/testdb",
        "arn:aws:redshift:us-west-2:123456789012:dbgroup:examplecluster/common_group"
      ],
        "Condition": {
            "StringEquals": {
           "aws:userid":"AIDIODR4TAW7CSEXAMPLE:${redshift:DbUser}@yourdomain.com"
                            }
                      }
    },
    {
      "Sid": "CreateClusterUserStatement",
      "Effect": "Allow",
      "Action": [
        "redshift:CreateClusterUser"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:dbuser:examplecluster/${redshift:DbUser}"
      ],
      "Condition": {
        "StringEquals": {
          "aws:userid":"AIDIODR4TAW7CSEXAMPLE:${redshift:DbUser}@yourdomain.com"
        }
      }
    },
    {
      "Sid": "RedshiftJoinGroupStatement",
      "Effect": "Allow",
      "Action": [
        "redshift:JoinGroup"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:dbgroup:examplecluster/common_group"
      ]
    }
  ]
}
          
 
  }
}
```