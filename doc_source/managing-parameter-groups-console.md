# Managing Parameter Groups Using the Console<a name="managing-parameter-groups-console"></a>

 You can view, create, modify, and delete parameter groups on the Amazon Redshift console\. To initiate these tasks, choose **Workload management**, then choose the **Parameter Groups** to manage or create one\. 

 You can view any of the parameter groups in the list to see a summary of the values for parameters and workload management \(WLM\) configuration\. **Group parameters** are shown on the **Parameters** tab and **Workload queues** are shown on the **Workload Management** tab\. In the following screenshot, the parameter group called `custom-param-group` is expanded to show the summary of parameter values\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-groups-list.png)

## Creating a Parameter Group<a name="parameter-group-create"></a>

You can create a parameter group if you want to set parameter values that are different from the default parameter group\.<a name="parameter-group-create-task"></a>

**To create a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Parameter Groups**\.

1. On the **Parameter Groups** page, choose **Create Cluster Parameter Group**\.

1. In the **Create Cluster Parameter Group** dialog box, choose a parameter group family, and then type a parameter group name and a parameter group description\. For more information about naming constraints for parameter groups, see [Limits in Amazon Redshift](amazon-redshift-limits.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-create-10.png)

1. Choose **Create**\.

## Modifying a Parameter Group<a name="parameter-group-modify"></a>

 You can modify parameters to change the parameter settings and WLM configuration properties\. 

**Note**  
You can't modify the default parameter group\.<a name="parameter-group-modify-task"></a>

**To modify parameters in a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Parameter Groups**\.

1. On the **Parameter Groups** page, in the parameter group list, select the row of the parameter group that you want to modify\.

1. To edit the parameters other than the WLM configuration parameter, choose **Edit Parameters**\.

   The **Parameters** tab opens and enables you to update parameters in the parameter group\. You can update values for parameters such as:
   + auto\_analyze
   + datestyle
   + enable\_user\_activity\_logging
   + extra\_float\_digits
   + force\_acm
   + max\_concurrency\_scaling\_clusters
   + query\_group
   + require\_ssl
   + search\_path
   + statement\_timeout
   + use\_fips\_ssl

1. In the **Value** box that corresponds to the parameter you want to modify, type a new value\. For more information about these parameters, see [Amazon Redshift Parameter Groups](working-with-parameter-groups.md)\.

1. Choose **Save Changes**\.
**Note**  
 If you modify these parameters in a parameter group that is already associated with a cluster, reboot the cluster for the changes to be applied\. For more information, see [Rebooting a Cluster](managing-clusters-console.md#reboot-cluster)\. <a name="parameter-group-modify-wlm-task"></a>

**To modify the WLM configuration in a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Workload management**\.

1. For **Parameter groups,** choose the parameter group that you want to modify\.
**Note**  
You can't modify the default parameter group\.

1. To edit the WLM configuration, choose **Edit**\. 

1. To enable short query acceleration \(SQA\), select **Enable short query acceleration**\.

1. When you enable SQA, ** Maximum run time for short queries \(1 to 20 seconds\)** is set to **Dynamic** by default\. To set the maximum run time to a fixed value, choose a value of 1–20\.

1. Do one or more of the following to modify the queue configuration: 
   + Choose **Switch WLM mode** to choose between **Auto WLM** and **Manual WLM**\.

     With **Auto WLM**, the **Memory** and **Concurrency on main** values are set to **auto**\.
   + To create a queue, choose **Add Queue**\.

     You can't add a queue to an **Auto WLM** configuration\.
   + To modify the **Max Concurrency Scaling clusters** parameter, choose **Edit** next to the current value that is displayed\.
   + To modify a queue, change property values in the table\. Depending on the type of queue, properties can include the following:
     + **Memory \(%\)**
     + **Concurrency on main** cluster
     + **Concurrency Scaling mode** can be **off** or **auto**
     + **Timeout \(ms\)**
     + **User groups**
     + **Query groups**
   + To change the order of queues, choose the **Up** and **Down** arrow buttons in the table\.
   + To delete a queue, choose **Delete** in the queue's row in the table\.

1. To have changes applied to associated clusters after their next reboot, select **Defer dynamic changes until reboot**\.
**Note**  
Some changes require a cluster reboot regardless of this setting\. For more information, see [WLM Dynamic and Static Properties](workload-mgmt-config.md#wlm-dynamic-and-static-properties)\.

1. Choose **Save**\.

## Creating or Modifying a Query Monitoring Rule Using the Console<a name="parameter-group-modify-qmr-console"></a>

You can use the AWS Management Console to create and modify WLM query management rules\. Query monitoring rules are part of the WLM configuration parameter for a parameter group\. For more information, see [WLM Query Monitoring Rules](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-wlm-query-monitoring-rules.html)\. 

When you create a rule, you define the rule name, one or more predicates, and an action\. 

When you save WLM configuration that includes a rule, you can view the JSON code for the rule definition as part of the JSON for the WLM configuration parameter\. <a name="parameter-group-modify-qmr-task"></a>

**To create a query monitoring rule**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Workload management**\.

1. For **Parameter groups,** choose the parameter group that you want to modify\.
**Note**  
You can't modify the default parameter group\.

1. To edit the WLM configuration \(to add a rule\), choose **Edit**\. 

1. To create a new rule using a predefined template, in the **Rules for Queue 1** group, choose **Add Rule from Templates**\. The **Rule Templates** dialog appears, as shown in the following screenshot\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-rule-template-dialog.png)

1. Choose one or more rule templates\. WLM creates one rule for each template you choose\. For this example, choose **Long running query with high I/O skew** and then choose **Select**\.

   A new rule appears with two predicates, as shown in the following screenshot\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-add-rule-template.png)

1. Type a **Rule name**\. The name can be up to 32 alphanumeric characters and must not contain spaces or quotation mark characters\. For this example, type **HighIOskew**\.

1. Optionally, modify the predicates\.

1. Choose an **Action**\. Each rule has one action\. For this example, choose **Hop**\. Hop terminates the query and WLM routes the query to the next matching queue, if one is available\. 

1. Choose **Save**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-collapse-queue.png)

1. To modify the rules for a queue, choose **Edit**\.

1. To add a new rule from scratch, choose **Add Custom Rule**\. You can a maximum of five rules per queue, and a total of eight rules for all queues\.

1. Type a **Rule name**; for example, **NestedLoop**\. 

1. Define a **Predicate**\. Choose a predicate name, an operator, and a value\. For this example, choose **Nested loop join count \(rows\)**\. Leave the operator at greater than \( **>** \), and for the value type **1000**\. The following screen shot shows the new rule with one predicate\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-add-rule.png)

1. To add additional predicates, choose the add icon to the right of the predicates\. You can have up to three predicates per rule\. If all of the predicates are met, WLM triggers the associated action\. 

1. Choose an **Action**\. Each rule has one action\. For this example, accept the default action, **Log**\. The Log action writes a record to the [STL\_WLM\_RULE\_ACTION](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_WLM_RULE_ACTION.html) system table and leaves the query running in the queue\. 

1. Choose **Done Editing**\. The queue details collapse\.

1. Choose **Save**\.

1. Amazon Redshift generates your WLM configuration parameter in JSON format and displays the JSON in a window at the bottom of the screen, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-json.png)

## Deleting a Parameter Group<a name="parameter-group-delete"></a>

You can delete a parameter group if you no longer need it and it is not associated with any clusters\. You can only delete custom parameter groups\.<a name="parameter-group-delete-task"></a>

**To delete a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Parameter Groups**\.

1. Select the row of the parameter group that you want to delete, and then choose **Delete**\. 
**Note**  
You can't delete the default parameter group\.

1. In the **Delete Cluster Parameter Groups** dialog box, choose **Continue**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-delete.png)

## Associating a Parameter Group with a Cluster<a name="parameter-group-associate"></a>

When you launch a cluster, you must associate it with a parameter group\. If you want to change the parameter group later, you can modify the cluster and choose a different parameter group\. For more information, see [Creating a Cluster by Using Launch Cluster](managing-clusters-console.md#create-cluster-task) and [To modify a cluster](managing-clusters-console.md#modify-cluster-task)\. 