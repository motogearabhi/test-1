Description

Under PRJ0015041, VDOT has requested the addition of a Base Deny Network Security Rule to all Azure Network Security Groups (NSGs) listed in the attached spreadsheet. The rule will be created for both Inbound and Outbound traffic to establish a default deny rule at the lowest priority while ensuring that all existing higher-priority allow rules continue to function as designed.

The following rule will be added:

{
  "name": "Base_Deny",
  "description": "Deny all",
  "protocol": "*",
  "sourcePortRange": "*",
  "destinationPortRange": "*",
  "sourceAddressPrefix": "*",
  "destinationAddressPrefix": "*",
  "access": "Deny",
  "priority": 4096
}

The implementation will be applied only to the NSGs identified in the customer-provided spreadsheet. No existing security rules will be modified or removed.

Justification

Under PRJ0015041, VDOT has requested the implementation of a baseline security control by adding a Base Deny rule to all specified Azure Network Security Groups. This change aligns with Azure security best practices by ensuring that any traffic not explicitly permitted by higher-priority rules is denied.

1) What is the impact to the business or COV if these changes are delayed?

There is no immediate production outage if this change is delayed. However, delaying implementation postpones compliance with the required network security baseline and may leave unintended traffic paths available until the Base Deny rule is deployed.

2) If these changes are executed, what is the likelihood (probability) of them not going as planned?

Low. The rule is added with the lowest priority (4096) and therefore does not override existing allow or deny rules. The implementation is a standard Azure NSG configuration change and will be validated after deployment.

3) If they do not go as planned, what will be the impact on the number of users and business functions?

Minimal impact is expected. Since the rule is evaluated only after all existing higher-priority rules, no interruption to existing approved application traffic is anticipated. Any unexpected connectivity issue can be resolved immediately by removing the newly added rule.

4) If they do not go as planned, what would be the recovery time, either through rollback or roll forward?

Rollback consists of removing the Base_Deny rule from the affected NSGs. Estimated rollback time is approximately 30 minutes, depending on the number of NSGs. Azure changes are applied immediately after the rule is removed.

5) Will this change impact public-facing applications or websites?

No. Existing permitted traffic will continue to function through existing higher-priority NSG rules. No downtime is expected.

Implementation Plan (Rollout)

Estimated Duration: 2 hours (depending on the number of NSGs)

Steps
Log in to the Azure Portal with the required administrative privileges.
Review the customer-provided spreadsheet containing the list of NSGs.
Verify the resource group and subscription for each NSG.
For each Network Security Group:
Navigate to Networking → Network Security Groups.
Open the target NSG.
Create a new Inbound Security Rule with the following values:
Name: Base_Deny
Priority: 4096
Source: Any (*)
Source Port: Any (*)
Destination: Any (*)
Destination Port: Any (*)
Protocol: Any (*)
Action: Deny
Save the rule.
Repeat the same process for the Outbound Security Rules.
Repeat Steps 4 and 5 for all NSGs listed in the customer spreadsheet.
Validate that the new rules are successfully created and enabled.
Confirm that all existing security rules remain unchanged.
Verify that business-critical application connectivity remains operational.
Update the ServiceNow change record with implementation results and validation evidence.
Validation Plan
Confirm the Base_Deny rule exists in both Inbound and Outbound rules for every NSG listed.
Verify the rule priority is 4096.
Confirm existing allow rules retain their original priorities.
Validate application connectivity from representative source and destination systems.
Perform Azure Network Watcher (if available) or connectivity testing to confirm expected traffic continues to be permitted.
Confirm no unexpected alerts or connectivity failures are observed after implementation.
Rollback Plan

If any unexpected behavior is observed:

Identify the affected NSG(s).
Remove the newly created Base_Deny inbound rule.
Remove the newly created Base_Deny outbound rule.
Save the NSG configuration.
Revalidate application connectivity.
Update the Change Request with rollback details and results.

This wording closely matches the style and level of detail of your previous SQL Server VM change request while being specific to the NSG Base Deny rule implementation.
