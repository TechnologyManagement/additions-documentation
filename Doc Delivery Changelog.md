# DD2.5.3
- Bug fix: Client call-back error when printing from the job queue.
- Document record already exists - if two users simultaneously submit records to Doc Delivery it is possible for them both to get the same document entry no. and therefore on of them end up with a "record already exists" error.
- Document log processed multiple times - The same document log record can be processed multiple times if NAV is unable to update the record to mark it as processed. The email is sent, but because the record is not marked as processed it is resent from the queue.
- Editing Request Page on Document Type Selection -  If you edit the request page of a report on the document type selection page and then click cancel the result is that the document record has no request page parameters and throws an error when the document is processed. If the user clicks cancel, make sure that the original request page parameters are retained.
- Record Error Message when there is no Attachment - Currently the record is skipped if no attachment is generated. Log an error message to be displayed at the end of processing instead.
 
# DD2.5.2
- Bug fix: Error message: "The Record is Already Open"

# DD2.5.1
- Update to DD Config - Added Custom Rules to DD Config XMLPort
- Printing to specified printer tray
