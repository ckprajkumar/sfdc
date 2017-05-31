# Project Specific Naming Convention

Below naming convention is expected to be followed for Salesforce project. If developer comes across any new types other than listed below, naming convention has to be first agreed within the team and document should be updated and circulated. All development objects should be prefixed with module name e.g SFA and suffixed by its type if required.

This will help in identifying what belongs to specific application and helps in changeset creation as well

| Type of development | Format |Example |
| ------ | ------ | ------ |
|SFA_CalculateEstimate_BL |SFA_<classname>_<Type>|SFA_CalculateEstimate_BL|
|SFA_CalculatEestimate_VFC|SFA_<classname>_VFC|SFA_CalculatEestimate_VFC|
|SFA_CalculatEestimate_VFC_Test|SFA_<classnametobetested>_<VFC>_Test|SFA_CalculatEestimate_VFC_Test|
|SFA_CalculateEstimate_VFP|SFA_<pagename>_VFP|SFA_CalculateEstimate_VFP|
|SFA_BrandEstimate_Trigger|SFA_<objectname>_Trigger|SFA_BrandEstimate_Trigger|
|SFA_calculatebrand_ Batch|SFA_<batchname>_Batch|SFA_calculatebrand_ Batch|
|SFA_CalculateEstimateAnnual_ Schedule|SFA_<schedulableclassname>_Schedule|SFA_CalculateEstimateAnnual_ Schedule|
|SFA_CMCOUsers_CS|SFA_<Customsettingname>_CS|SFA_CMCOUsers_CS|
|SFA_HelptextonLandingPage_CL|SFA_<customlabelname>_CL|SFA_HelptextonLandingPage_CL|
|SFA_BrandEstimate_checkfornewrecords|SFA_<objectname>_<workflowname>|SFA_BrandEstimate_checkfornewrecords|
|SFA_AgencyLayout|SFA_<pagelayoutname>|SFA_AgencyLayout|
|SFA_NotifyAgent__c|SFA_<fieldname>__c|SFA_NotifyAgent__c|
|SFA_BrandEstimate__c|SFA_<objectanme>__c|SFA_BrandEstimate__c|
|SFA_BrandEstimate_sharewithagency|SFA_<objectname>_<sharingrule>|SFA_BrandEstimate_sharewithagency|
|SFA_ExternalProfile|SFA_<profilename>|SFA_ExternalProfile|
|SFA_BrandEstimate_agencyrecord|SFA_<objectname>_<recordtypename>|SFA_BrandEstimate_agencyrecord|

