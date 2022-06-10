# Process-Automation-Specialist-Solution
## Prework
- Create new playgrond
- Install the Process Automation superbadge unmanaged package(package ID 04t46000001Zch4)

## Challenge 1
## Automate Leads

1)
- setup->
- Object Manager->
- Leads->
- Validation Rules->
- New Rule->
- Name = Aything you want
- Formula =
```
OR(AND(LEN(State) > 2, NOT(CONTAINS("AL:AK:AZ:AR:CA:CO:CT:DE:DC:FL:GA:HI:ID:IL:IN:IA:KS:KY:LA:ME:MD:MA:MI:MN:MS:MO:MT:NE:NV:NH:NJ:NM:NY:NC:ND:OH:OK:OR:PA:RI:SC:SD:TN:TX:UT:VT:VA:WA:WV:WI:WY", State )) ), NOT(OR(Country ="US",Country ="USA",Country ="United States", ISBLANK(Country))))
```
- Error Message = Given value is not applicable

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
- Rule Entries->
- New->
  - Sort Order: 1
  - Field: Lead: Lead Source
  - Operator: equals
  - Value: Web
  - Select the user or queue to assign the Lead to : Queue : Rainbow Sales
- Save and New
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
  - Summary Type: COUNT
  - Summarized Object:	Opportunity
  - Filter Criteria: None
- Save and New
  - DataType: Roll-Up Summary
  - Label: Number of won deals
  - Summary Type: COUNT
  - Summarized Object: Opportunity
  - Filter Criteria: Stage EQUALS Closed Won
- Save and New
  - DataType: Roll-Up Summary
  - Label: Last won deal date
  - Summary Type: MAX
  - Summarized Object: Opportunity
  - Field to Aggregate: Opportunity: Close Date
  - Filter Criteria: Stage EQUALS Closed Won
- Save and New
  - DataType: Roll-Up Summary
  - Label: Amount of won deals
  - Summary Type: SUM
  - Summarized Object: Opportunity
  - Field to Aggregate: Opportunity: Amount
  - Filter Criteria: Stage EQUALS Closed Won
- Save and New
  - DataType: Formula
  - Label: Deal win percent
  - Return Type: Percent
  - Decimal Places: 2
  - Formula: ```(Number_of_won_deals__c / Number_of_deals__c)```
- Save and New
  - DataType: Formula
  - Label: Call for Service
  - Return Type: Text
  - Formula: ```IF(DATE(YEAR( Last_won_deal_date__c )+2, MONTH( Last_won_deal_date__c ),DAY( Last_won_deal_date__c )) <= TODAY(), "Yes","No")```
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
  - Error Location: Account Name



