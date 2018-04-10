DD2.9.1
=======

-   Improvements to the layout of the Document Delivery Setup page

DD2.9
=====

-   Integration with Document Links. If both Document Links and Doc Delivery are
    installed in the database Doc Delivery can use Document Links as the
    archiving engine for attachments that it creates.

DD2.8.1
=======

-   [Bug fix] Improved handling of "Doc. per record" selection when grouping
    records by a field

DD2.8
=====

-   Renumbered into new licence range

DD2.7.9
=======

-   DD actions added to Issued Finance Charge Memos

-   [Bug fix] Custom Value table relation changed to AllObj instead of Object
    table

-   [Bug fix] Use field numbers instead of field names in record views to avoid
    issues with different languages

-   Some obselete fields removed

-   Subscription to ADC event removed to simplify installation

DD2.7.8.3
=========

-   [Bug fix] Document Type Parameters reset

DD2.7.8.2
=========

-   [Bug fix] Attempt to modify an old copy of a record error when documents
    fail to process

DD2.7.8.1
=========

-   [Bug fix] Payment Journal is treated as a card page

DD2.7.8
=======

-   Dynamically Determined Attachment Paths

-   New function in Doc Delivery Mgt. codeunit to retrieve the record that is
    currently being processed

-   [Bug fix] Overflow Error on SetDocumentType

DD2.7.7
=======

-   Expose New Event to Customise Document Log Details

DD2.7.6.2
=========

-   [Bug fix] Doc. Type Attachment Options Unreliable

DD2.7.6.1
=========

-   [Bug fix] Watermark document

DD2.7.6
=======

-   Improved lookup for Custom Layout Description

DD2.7.5.2
=========

-   [Bug fix] GetServicePassword cannot get Cust/Vend

DD2.7.5.1
=========

-   [Bug fix] Move DD Actions to Correct Sales Return Page

DD2.7.5
=======

-   Test Mode options on Document Delivery Setup page

DD2.7.3.1
=========

-   [Bug fix] Doc Delivery Setup Does Not Exist Error on Customer List

DD2.7.3
=======

-   More flexible import/export options

-   [Bug fix] All document types listed when queuing or opening email

DD2.7.2
=======

-   [Bug fix] Record is Already Open Error

-   Add Document Type Code to Document Log Page

DD2.7.0
=======

-   Optionally Watermark Only the Final Page of the Attachment

DD2.6.9.2
=========

-   [Bug fix] Edit Body Page

DD2.6.9.1
=========

-   [Bug fix] Report Parameter Calculation on Resubmitting Documents

DD2.6.9
=======

-   Doc Delivery Password Field added to Customer and Vendor card pages

DD2.6.8
=======

-   [Bug fix] Overflow Retrieving Attachment from Store

-   [Bug fix] Error Processing Attachment Type Document Fields

-   [Bug fix] Custom Value Overflow Error

DD2.6.7
=======

-   [Bug fix] Xml Serialization Error when Editing Request Page

DD2.6.6
=======

-   [Bug fix] Selecting Single Document for All Records Includes Records Not
    Selected by the User

DD2.6.5
=======

-   Support for Service Document Types

DD2.6.4
=======

-   [Bug fix] Incorrect Linked Table Record Found for Records with Ampersand

DD2.6.3
=======

-   [Bug fix] Check for Outlook Running is Performed on Server Not Client

-   [Bug fix] Attachments Not Processed When No Email Add & Selecting "Open
    Email"

DD2.6.1
=======

-   [Bug fix] Performance of recalculating document fields on selecting new
    document type

DD2.6
=====

-   Prevent attachments from being recreated unnecessarily when emailing and
    printing documents

-   Ability to download a default configuration

DD2.5.7
=======

-   Request Page Options - ability to set the options on report request pages
    (in addition to filters), especially for the date fields on the Statement
    report

-   [Bug fix] "RPC Error" on Editing Body in Outlook

-   Progress Bar of Submitting Records

-   [Bug fix] Email Address Reverts Incorrectly

-   Static Value - Check Correct Usage

DD2.5.6
=======

-   Add DD Actions to Sales Return Orders

DD2.5.5
=======

-   Prompt for Missing Request Page Parameters

-   Primary Table Document Type Field - disallow the user of the Primary Table
    Document Type for tables other than Sales Header and Purchase Header

DD2.5.4
=======

-   [Bug fix] DD Translation Table Relation

-   Enumerating available printer trays

-   Display error message on AssistEdit on Document Log

-   [Bug fix] BLOB fields not included when resubmitting records

-   Don't lock DD Document table while generating attachments
