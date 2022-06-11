# Process-Automation-Specialist-Solution
## Prework
- Create new playgrond
- Install the Process Automation superbadge unmanaged package(package ID 04t46000001Zch4)

## Challenges
- 1 [Automate Leads](https://github.com/pkbparesh15/Process-Automation-Specialist-Solution#automate-leads)
- 2 [Automate Accounts](https://github.com/pkbparesh15/Process-Automation-Specialist-Solution#automate-accounts)
- 3 [Create Robot Setup Object](https://github.com/pkbparesh15/Process-Automation-Specialist-Solution#create-robot-setup-object)
- 4 [Create Sales Process and Validate Opportunities](https://github.com/pkbparesh15/Process-Automation-Specialist-Solution/#create-sales-process-and-validate-opportunities)
- 5 [Automate Opportunities](https://github.com/pkbparesh15/Process-Automation-Specialist-Solution#automate-opportunities)

## Challenge 1
## Automate Leads

1)
- setup->
- Object Manager->
- Leads->
- Validation Rules->
- New Rule->
- Name = Aything you want
- Set Active
- Formula =
```
OR(AND(LEN(State) > 2, NOT(CONTAINS("AL:AK:AZ:AR:CA:CO:CT:DE:DC:FL:GA:HI:ID:IL:IN:IA:KS:KY:LA:ME:MD:MA:MI:MN:MS:MO:MT:NE:NV:NH:NJ:NM:NY:NC:ND:OH:OK:OR:PA:RI:SC:SD:TN:TX:UT:VT:VA:WA:WV:WI:WY", State )) ), NOT(OR(Country ="US",Country ="USA",Country ="United States", ISBLANK(Country))))
```
- Error Message = Given value is not applicable
- Save

2)
- setup->
- Search and open queues in quick find box
- New->
  - Label: Rainbow Sales
  - Selected Objects: Lead
- Save and New
  - Label: Assembly System Sales
  - Selected Objects: Lead

3)
- Setup->
- Lead Assignment Rules->
- Name = Aything you want
- Set active
- Rule Entries->
- New->
  - Sort Order: 1
  - Field: Lead: Lead Source
  - Operator: equals
  - Value: Web
  - Select the user or queue to assign the Lead to : Queue : Rainbow Sales
- Save
- New
  - Sort Order: 2
  - Field: Lead: Lead Source
  - Operator: not equal to
  - Value: Web
  - Select the user or queue to assign the Lead to : Queue : Assembly System Sales
- Save

## Challenge 2
## Automate Accounts

- Object Manager->
- Account->
- Fields & Relationships->
- New->
  - DataType: Roll-Up Summary
  - Label: Number of deals
  - Next->
  - Summarized Object:	Opportunities
  - Summary Type: COUNT
  - Filter Criteria: All records should be included in the calculation
  - Next->
  - Field Level Security: Visible
- Save and New
  - DataType: Roll-Up Summary
  - Label: Number of won deals
  - Next->
  - Summarized Object: Opportunities
  - Summary Type: COUNT
  - Filter Criteria: Only records meeting certain criteria should be included in the calculation
  - Stage EQUALS Closed Won
  - Field Level Security: Visible
  - Next->
- Save and New
  - DataType: Roll-Up Summary
  - Label: Last won deal date
  - Summarized Object: Opportunities
  - Summary Type: MAX
  - Field to Aggregate: Close Date
  - Filter Criteria: Only records meeting certain criteria should be included in the calculation
  - Stage EQUALS Closed Won
  - Field Level Security: Visible
- Save and New
  - DataType: Roll-Up Summary
  - Label: Amount of won deals
  - Summarized Object: Opportunities
  - Summary Type: SUM
  - Field to Aggregate: Amount
  - Filter Criteria: Only records meeting certain criteria should be included in the calculation
  - Stage EQUALS Closed Won
  - Field Level Security: Visible
- Save and New
  - DataType: Formula
  - Label: Deal win percent
  - Return Type: Percent
  - Decimal Places: 2
  - Formula: ```(Number_of_won_deals__c / Number_of_deals__c)```
  - Field Level Security: Visible
- Save and New
  - DataType: Formula
  - Label: Call for Service
  - Return Type: Text
  - Formula: ```IF(DATE(YEAR( Last_won_deal_date__c )+2, MONTH( Last_won_deal_date__c ),DAY( Last_won_deal_date__c )) <= TODAY(), "Yes","No")```
  - Field Level Security: Visible
- Save
- Validation Rules->
- New->
  - Rule Name: US_Address
  - Error Condition Formula: 
  ```
  OR(AND(LEN(BillingState) > 2, NOT(CONTAINS("AL:AK:AZ:AR:CA:CO:CT:DE:DC:FL:GA:HI:ID:IL:IN:IA:KS:KY:LA:ME:MD:MA:MI:MN:MS:MO:MT:NE:NV:NH:NJ:NM:NY:NC:ND:OH:OK:OR:PA:RI:SC:SD:TN:TX:UT:VT:VA:WA:WV:WI:WY", BillingState ))
  ),AND(LEN(ShippingState) > 2, NOT(CONTAINS("AL:AK:AZ:AR:CA:CO:CT:DE:DC:FL:GA:HI:ID:IL:IN:IA:KS:KY:LA:ME:MD:MA:MI:MN:MS:MO:MT:NE:NV:NH:NJ:NM:NY:NC:ND:OH:OK:OR:PA:RI:SC:SD:TN:TX:UT:VT:VA:WA:WV:WI:WY", ShippingState))
  ),NOT(OR(BillingCountry ="US",BillingCountry ="USA",BillingCountry ="United States", ISBLANK(BillingCountry))),
  NOT(OR(ShippingCountry ="US",ShippingCountry ="USA",ShippingCountry ="United States", ISBLANK(ShippingCountry))))
  ```
  - Error Message:	Billing Address should be valid US state abbreviations
- Save and New
  - Rule Name: Name_Change
  - Error Condition Formula:
  ```
  ISCHANGED( Name ) && ( OR( ISPICKVAL( Type ,'Customer - Direct') ,ISPICKVAL( Type ,'Customer - Channel') ))
  ```
  - Error Message:	You can't change "Customer direct" or "Customer channel"
  - Error Location: Field: Account Name
- Save


## Challenge 3
## Create Robot Setup Object

- Object Manager->
  - Create-> Custom Object
  - Label: Robot Setup
  - Record Name: Robot Setup Name
  - Data Type: Auto Number
  - Display Format: ROBOT SETUP-{0000}
  - Starting Number: 0
- Save
- Object Manager->
- Robot Setup->
- Fields & Relationships->
- New->
  - DataType: Date
  - Label: Date
  - Field Level Security: Visible
- Save and New
  - DataType: Text
  - Label: Notes
  - Length: 255
  - Field Level Security: Visible
- Save and New
  - DataType: Number
  - Label: Day of the Week
  - Formula Return Type: Text
  - Formula: ```CASE( WEEKDAY( Date__c ),
1,"Sunday",
2,"Monday",
3,"Tuesday",
4,"Wednesday",
5,"Thursday",
6,"Saturday",
Text( WEEKDAY( Date__c ) ) )```
  - Field Level Security: Visible
- Save and New
  - DataType: Master-Detail Relationship
  - Related to: Opportunity
  - Label: Opportunity
  - Field Level Security: Visible
- Save

## Challenge 4
## Create Sales Process and Validate Opportunities

- Object Manager->
- Opportunity->
- Stage->
-  pportunity Stages Picklist Values->
- New->
- Stage Name: Awaiting Approval
- Probability: 20
- Save
- Search and select Sales Processes from Quick Find box
- New->
- Sales Process Name: RB Robotics Sales Process
- Remove (Needs Analysis, Value Proposition, Id. Decision Makers, Perception Analysis) from Selected Values
- Save
- Object Manager->
- Opportunity->
- Fields & Relationships->
- Record Type->
- New->
- Record Type Label: RB Robotics Process RT
- Sales Process: RB Robotics Sales Process
- Next
- Apply one layout to all profiles: Opportunity Layout
- Save
- Fields & Relationships->
- DataType: Checkbox
- Label: Approved
- Field Level Security: Default
- Save
- Validation Rule->
- New->
- Rule Name: RB_High_Value_Opp
- Error Condition Formula: ```IF(( Amount > 100000 && Approved__c <> True && ISPICKVAL( StageName,'Closed Won') ),True,False)```
- Error Message: Appropriate value should be given
- Save

## Challenge 5
## Automate Opportunities

- 


