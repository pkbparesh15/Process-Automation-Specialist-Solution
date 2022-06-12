# Process-Automation-Specialist-Solution
## Prework
- Create new playgrond
- Install the Process Automation superbadge unmanaged package(package ID 04t46000001Zch4)

## Challenges
- 1) [Automate Leads](https://github.com/pkbparesh15/Process-Automation-Specialist-Solution#automate-leads)
- 2) [Automate Accounts](https://github.com/pkbparesh15/Process-Automation-Specialist-Solution#automate-accounts)
- 3) [Create Robot Setup Object](https://github.com/pkbparesh15/Process-Automation-Specialist-Solution#create-robot-setup-object)
- 4) [Create Sales Process and Validate Opportunities](https://github.com/pkbparesh15/Process-Automation-Specialist-Solution/#create-sales-process-and-validate-opportunities)
- 5) [Automate Opportunities](https://github.com/pkbparesh15/Process-Automation-Specialist-Solution#automate-opportunities)
- 6) [Create Flow for Opportunities](https://github.com/pkbparesh15/Process-Automation-Specialist-Solution#create-flow-for-opportunities)
- 7) [Automate Setups](https://github.com/pkbparesh15/Process-Automation-Specialist-Solution#automate-setups)

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

### Check Challenge!!

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

### Check Challenge!!

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
  - DataType: Formula
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

### Check Challenge!!

## Challenge 4
## Create Sales Process and Validate Opportunities

- Object Manager->
- Opportunity->
- Field and Relationship->
- Stage->
- Opportunity Stages Picklist Values->
- New->
- Stage Name: Awaiting Approval
- Probability: 20
- Save
- Search and select Sales Processes from Quick Find box
- New->
- Sales Process Name: RB Robotics Sales Process
- Save
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

### Check Challenge!!

## Challenge 5
## Automate Opportunities

- Setup->
- Users->
- New->
- Last Name: Nushi Davoud
- Role: CEO
- User License: Salesforce Platform
- Profile: Standard Platform User
- Others insert as you wish but don't change defaults.
- Search and Select Email Alerts in Quick find Box
- New Alert->
  - Description: Email Alert on Opportunity
  - Object: Opportunity
  - Email Template: Finance: Account Creation (from RB Robotics Templates)
  - Selected Recipients: User: Nushi Davoud
- Save and New
  - Description: Sales Approval Email
  - Object: Opportunity
  - Email Template: SALES: Opportunity Needs Approval (from RB Robotics Templates)
  - Selected Recipients: User: Nushi Davoud
- Save and New
  - Description: Sales: Opportunity Approval Request Mail
  - Object: Opportunity
  - Email Template: Sales: Opportunity Approval Status Email (from RB Robotics Templates)
  - Selected Recipients: User: Nushi Davoud
- Save
- Search and Select Approval Processes in Quick find Box
- Manage Approval Processes For: Opportunity
- Create New Approval Process-> Use Standard Setup Wizard
  - Process Name: Prospect Approval
  - next
  - Field: Opportunity: Stage
  - Operator: equals
  - Value: Negotation/Review
  - Field: Opportunity: Amount
  - Operator: greater than
  - Value: 100000
  - Next-> Next->
  - Email Template: Sales: Opportunity Approval Status Email
  - Next
  - Selected Fields: Opportunity Name, Opportunity Owner
  - Approval Page Fields: Select
  - Next-> Save
- What Would You Like To Do Now? : 'll do this later. Take me to the approval detail page to review what I've just created.
- Go
- Initial Submission Actions: Add New
  - Field Update->
  - Name: Approval
  - Field to Update: Stage
  - Specify New Field Value: A specific value: Awaiting Approval
- Save
  - Approval Steps: New Approval Step
  - Name: Approval for Prospect
  - Next-> Next->
  - Select Approver: Automatically assign to approver(s). : Nushi Davoud
  - Save
  - What Would You Like To Do Now? : No, I'll do this later. Take me to the approval process detail page to review what I've just created.
  - Go
- Final Approval Actions: Add New
- Field Update->
  - Name: Stage-Closed Won
  - Field to Update: Stage
  - Specify New Field Value: A specific value: Closed Won
- Save and New
  -  Name:Approved Check
  - Field to Update: Approved
  - Specify New Field Value: Checkbox Options: True
- Save
- Add Existing->
  - Choose Action Type: Email Alert
  - Add: Sales: Opportunity Approval Request Mail
 - Save
- Final Rejection Actions: Add Existing
  - Choose Action Type: Email Alert
  - Add: Sales Approval Email
  - Save
- Add New-> Field Update->
  - Name: State Nego
  - Field to Update: Stage
  - Specify New Field Value: A specific value: Negotiation/Review
- Save
- Recall Actions-> Add New
  - Name: approvedcheck
  - Field to Update: Approved
  - Specify New Field Value: Checkbox Options: True
- Save and New
  - Name: StagesClosedWon
  - Field to Update: Stage
  - Specify New Field Value: A specific value: Closed Won
- Save
- Activate
- Search and Select Process Builder in Quick find Box
- New
  - Continue in Process Builder
  - Process Name: Automate Opportunity
  - The process starts when: A record changes
  - Save
    - Add Object
    - Opportunity
    - Save
    - Add Criteria
    - Criteria Name: Customer
    - Criteria for Executing Actions: Conditions are met
      - Field: Opportunity->Account ID->Account Type
      - Operator: Equals
      - Type: Picklist
      - Value: Customer Direct
      - Field: Opportunity->Account ID->Account Type
      - Operator: Equals
      - Type: Picklist
      - Value: Customer Channel
      - Field: Opportunity->Account ID
      - Operator: Does not equals
      - Type: Global Constant
      - Value: $GlobalConstant.Null
    - Conditions: Customize the logic
    - Logic: (1 OR 2) AND 3
    - Save
    - Add Action
      - Action Type: Email Alerts
      - Action Name: Alert to Finance
      - Email Alert: Email_Alert_on_Opportunity
    - Save
    - Add Criteria
    - Criteria Name: Prospect Account
    - Criteria for Executing Actions: Conditions are met
      - Field: Opportunity->Account ID->Account Type
      - Operator: Equals
      - Type: Picklist
      - Value: Prospect
      - Field: Opportunity->Account ID
      - Operator: Does not equals
      - Type: Global Constant
      - Value: $GlobalConstant.Null
      - Field: Opportunity->Stage
      - Operator: Equals
      - Type: Picklist
      - Value: Prospecting
    - Conditions: All of the conditions are met (AND)
    - Save
    - Add Action
      - Action Type: Create a Record
      - Action Name: Send Marketing Materials
      - Record Type: Task
        - Field: Due Date Only
        - Type: Formula
        - Value: TODAY()+7
        - Field: Assigned To ID
        - Type: Field Reference
        - Value: Opportunity->Account ID->Owner ID
        - Field: Priority
        - Type: Picklist
        - Value: High
        - Field: Status
        - Type: Picklist
        - Value: In Progress
        - Field: Subject
        - Type: String
        - Value: Send Marketing Materials
        - Field: Related To ID
        - Type: Field Reference
        - Value: Opportunity->Opportunity ID
    - Save
    - Add Action
      - Action Type: Email Alerts
      - Action Name: Alert
      - Email Alert: Email_Alert_on_Opportunity
    - Save
    - Add Criteria
    - Criteria Name: Negotiations or Review
    - Criteria for Executing Actions: Conditions are met
      - Field: Opportunity->Stage
      - Operator: Equals
      - Type: Picklist
      - Value: Negotiation/Review
      - Field: Opportunity->Amount
      - Operator: Greater than
      - Type: Currency
      - Value: 100000
    - Conditions: All of the conditions are met (AND)
    - Save
    - Add Action
      - Action Type: Submit for Approval
      - Action Name: Approval Pro
      - Approval Process: Specific approval process: Prospect Approval
      - Skip the entry criteria for this process? : Yes
      - Submitter: Current User
    - Save
    - Add Criteria
    - Criteria Name: Closed Won Deal
    - Criteria for Executing Actions: No criteria—just execute the actions!
    - Save
    - Add Action
      - Action Type: Create a Record
      - Action Name: Record for Robot
      - Record Type: Robot Setup
        - Field: Opportunity
        - Type: Field Reference
        - Value: Opportunity->Opportunity ID
        - Field: Date
        - Type: Formula
        - Value: ```CASE(MOD([Opportunity].CloseDate + 180- DATE(1900,1,7),7),0,[Opportunity].CloseDate + 181,6,[Opportunity].CloseDate + 182,[Opportunity].CloseDate + 180)```
    - Save
    - Add Action
      - Action Type: Email Alerts
      - Action Name: Send for Closed Deal
      - Email Alert: Email_Alert_on_Opportunity
    - Save
    - In Cuustomer , Prospect Account Criteria
      - Click STOP
      - Select Evaluate the next criteria
      - Save
  - Activate
  - Confirm

### Check Challenge!!

## Challenge 6
## Create Flow for Opportunities

- Setup
- Search ***Process Automation*** and select ***Flows***
- New Flow->
- Screen Flow->
- Create->
- Change Auto-Layout to Free-Form
- Drag and drop a **Screen** from Elements tab
- Label: Product Quick Search
- From components drag and drop radio buttons on the new screen layout
  - Label: Product Type
  - DataType: Text
  - Choice: New Choice Resource
    - Resource Type: Choice
    - API Name: RainbowBot
    - Choice Label: RainbowBot
    - Choice Value: RainbowBot
    - Done
  - Add Choice
  - Choice: New Choice Resource
    - Resource Type: Choice
    - API Name: CloudyBot
    - Choice Label: CloudyBot
    - Choice Value: CloudyBot
    - Done
  - Add Choice
  - Choice: New Choice Resource
    - Resource Type: Choice
    - API Name: AssemblySystem
    - Choice Label: Assembly System
    - Choice Value: AssemblySystem
    - Done
  - Done
- Drag and drop a **New Get Records** from Elements tab
  - Label: Product
  - Object: Product
  - Field: Name
  - Operator: Equals
  - Value: Product Type
  - How Many Records to Store: Only the first record
  - How to Store Record Data: Choose fields and assign variables (advanced)
  - Where to Store Field Values: Together in a record variable
  - Record: New Record
    - Resource Type: Variable
    - API Name: FilterResult
    - Data Type: Record
    - Object: Product
    - Done
  - Field: Name
  - Done
- Drag and drop a **Screen** from Elements tab
  - Label: Final Screen
  - Drag and drop a **Display Text** from Components tab
  - API Nme: Display
  - Insert a resource: {!FilterResult}
  - Done
- Connect the Flow in order: Start-> Product Quick Search-> Product-> Final Screen
- Save
- Flow Label: Product Quick Search
- Save
- Activate
- Setup->
- Search and Select Lightning App Builder in Quick Find Box
- New->
  - Record Page->
  - Next
  - Label: Product Quick Search
  - Object: Opportunity
  - Next
  - Select any template of your choice
  - Drag and drop **Flow** from components tab
  - Flow: Product Quick Search
  - Save
  - Activate
  - Close

### Check Challenge!!

## Challenge 7
## Automate Setups

You have already completed the challenge earlier.
### Check Challenge!!
Hope it helped you with the badge.....
Thank You
