
# Request 

This calculation creates a dynamic window. Hides or shows attibutes based off of other attributes. 

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