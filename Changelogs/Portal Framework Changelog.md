# PF1.0.1.3
## [Bug Fix]Resouce Name is limited to 50 chars
COD9059290.HandlePortalResourceRequest() remove the ResourceName text variable length

# PF1.0.1.2
## [Bug Fix]Time and Date displayed does not respect portal language
Time and Date format should be of the Portal Framework Application Language Code.&nbsp;

# PF1.0.1.1
## [Bug Fix]Lookup/Drilldown Page for Portal Framework Applications
Advanced lookup on Portal Fwk. Application ID from SFDC Setup shows this:<div><br></div><div><img src="https://technologymanagement.visualstudio.com/WorkItemTracking/v1.0/AttachFileHandler.ashx?FileNameGuid=27fc1652-360f-4961-8807-35b238be5686&amp;FileName=temp1486048879391.png" style="width:490.45px;"></div><div><br></div><div>The user can't create a new record from there if they need to.<br>&nbsp;<br></div>

## Create OnAfterStoreData publisher event 
Create the event for the Device.

# PF1.0.1
## Device Data Events
Create integration events for validation and lookup of data on the Device Data page. This will allow the client application e.g. SFDC to validate data that has been entered and provide lookup assistance to the user. The lookup event should include a VAR text parameter so that the subscribing event can set the data that has been selected by the user. Also include a Handled parameter so that portal framework can know whether the lookup was completed or not i.e. if the user clicked OK or Cancel.

## Device Data Events
it is possible for stale data to be left in the Device Data table which is no longer valid. For example, in SFDC a production order might be finished or deleted in the client but left in the Device Data of one the devices. This will cause an error when the device attempts to load the portal.<div><br></div><div>Throw an integration event to check that the device data is still valid before proceeding with the load of the portal page.</div>

