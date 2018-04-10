ADV1.0.3
========

Check additional address data
-----------------------------

Check for buildingname and premise from the API look result

[Bug Fix]Remove duplicate search result
---------------------------------------

Delete duplicate search result before displaying the entries 

ADV1.0.2
========

Add Post Code Lookup Support to Sales Header/Purchase Header documents
----------------------------------------------------------------------

Currently the addition only works with address details on master data records
(customers, vendors etc.)

Extend this functionality to document records that are based on sales header and
purchase header tables. Only update the address that corresponds to the post
code that has been updated i.e. if the Sell-to Post Code has been set update the
Sell-to Address fields, not the Ship-to or Bill-to addresses.

Default Configuration
---------------------

Support for default configuration to populate the Address Validation Setup page.

ADV1.0.1.1
==========

[Bug Fix]Subscriber LocationPostCodeOnAfterValidate is pointing at Alternative Address.
---------------------------------------------------------------------------------------

Service Connection Registration
-------------------------------

Details of the address validation service connection are now shown in the
Service Connection page.
