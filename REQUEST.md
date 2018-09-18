
# Request 

## These calculation are in the following:
- Module - Request Management
- Collection - Request

---


This calculation creates a dynamic window. Hides or shows attibutes based off of other attributes. 

- Attribute - DW_Ent_Hide Estimates

```
import System
static def GetAttributeValue(Request):
	HideEstimates = true
	HideRank = true

	if Request._EntIsBusinessApproved != null:
		if Request._EntIsBusinessApproved == true:
			HideEstimates = false
	
		if Request._EntBRDApproved == true:
			HideRank = false
	
	Commands = ':SetHidden(_EstimatedHoursToComplete,{0});'
	Commands += ':SetHidden(_EnterpriseApplicationCategory,{0});'
	
	Commands += ':SetHidden(_EnterpriseAppRank,{0});'
	
	return String.Format(Commands,HideEstimates,HideRank )
```

---



This calculations shows attributes based on roles.

```
import System
static def GetAttributeValue(Request):
	SetFormReadOnly = true
	SetDepartRankReadOnly = true
	SetBenReadOnly = true
	
	
	
	
	BusOwnerAppGroup = false
	CurrentUser = Request.GetCurrentUserName()
	CurrentUserTitle = Request.GetObjectByAttribute("System.User", "Name",CurrentUser)
	
	for groups in CurrentUserTitle.UserGroups:
		if groups.Group.Title == "Business Owner Approval Group":
			BusOwnerAppGroup = true
			
	for roles in CurrentUserTitle.UserRoles:
         if roles != null:
         if roles.Role.Title == "Administrators":
             Administrators = true
	
	
	if Request.Status!= null:
		if Request.Status.Title == "Request in Draft State" or Request.Status.Title == "Open" :
			SetFormReadOnly = false
			
			
			
		if BusOwnerAppGroup == true:
			SetFormReadOnly = false
			SetDepartRankReadOnly = false
			SetBenReadOnly = false
			
			
			
			
	Commands = ':SetReadOnly(Description,{0});'
	Commands += ':SetReadOnly(RaiseUser,{0});'
	Commands += ':SetReadOnly(_BrdUsers,{0});'
	Commands += ':SetReadOnly(_BusinessUnit,{0});'
	Commands += ':SetReadOnly(_EntStratCost,{0});'
	Commands += ':SetReadOnly(_EntStratRegulatory,{0});'
	Commands += ':SetReadOnly(_EntStratClientServ,{0});'
	Commands += ':SetReadOnly(_EntCosttoImplement,{0});'
	Commands += ':SetReadOnly(_EntFixedDate,{0});'
	Commands += ':SetReadOnly(_PRONextGenETS,{0});'
	Commands += ':SetReadOnly(_BusinessUnit,{0});'
	Commands += ':SetReadOnly(_BusinessUnitDepartment1,{0});'
	Commands += ':SetReadOnly(_EntStratEfficiency,{0});'
	Commands += ':SetReadOnly(_EntStratContractual,{0});'
	Commands += ':SetReadOnly(_EntCompAdvantage,{0});'
	Commands += ':SetReadOnly(_EntRiskIfNotDone,{0});'
	Commands += ':SetReadOnly(Title,{0});'
	Commands += ':SetReadOnly(_EntTargetDate,{0});'
	Commands += ':SetReadOnly(_EntObjective,{0});'
	Commands += ':SetReadOnly(_BusinessUnit,{0});'
	Commands += ':SetReadOnly(_EntTrainingRequired,{0});'
	Commands += ':SetReadOnly(_EntProcurementRequired,{0});'
	Commands += ':SetReadOnly(_EstimatedHoursToComplete,{0});'
	Commands += ':SetReadOnly(_EnterpriseAppRank,{0});'
	Commands += ':SetReadOnly(_EntDepartmentRank,{1});'
	Commands += ':SetReadOnly(_Beneficiary,{1});'



	return String.Format(Commands,SetFormReadOnly,SetDepartRankReadOnly,SetBenReadOnly)

```