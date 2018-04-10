DFD1.4
======

App Upgrading Handling
----------------------

Populate with Value Posting
---------------------------

Taking the value from the standard Value Posting field is fine, until you choose
Same Code. It populates the new default dimensions with Same Code as expected,
but disrupts the posting routine when you do so.

Create a new field, Populate with Value Posting with the same options as the
standard field. Use this field to determine the Value Posting value of new
default dimension records instead of the Value Posting field.
