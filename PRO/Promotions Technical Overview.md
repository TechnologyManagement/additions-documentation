---
title: Promotions Technical Overview
---

Applying Promotions
===================

COD9030249 (Promotion Mgt.) has all the relevant functions to determine which
promotions are applicable to a given sales document and to apply the benefits of
those promotions to the target document and lines.

SalesDocumentApplyPromotions
----------------------------

1.  Calls the AdditionsIntegration codeunit to ask other additions if there is
    any reason not to apply the promotions e.g. the sales document has been
    credit released and should no longer have any changes applied to it.

2.  Test that the document is Open.

3.  Check if any Promotion Entry records exist for this document, prompt the
    user for confirmation that the promotions will be
    [unapplied](#unapplying-promotions) (if GUIALLOWED).

4.  Call
    [InitPromotionBufferFromSalesDocument](#initpromotionbufferfromsalesdocument),
    [ApplyPromotionBenefitOnPromotionBuffer](#addpromotionbuffer) and
    [CheckAppliedAndUpdatePromotionBuffer](#checkappliedandupdatepromotionbuffer)

5.  If any promotions require user confirmation, show the Promotion Confirmation
    page to allow the user to confirm which, if any, of those promotions should
    be applied.

6.  Call
    [ApplyPromotionBufferToSalesDocument](#applypromotionbuffertosalesdocument)
    and
    [AddPromotionEntryQualificationLines](#addpromotionentryqualificationlines)

7.  Show a message box with the results of the promotion application.

InitPromotionBufferFromSalesDocument
------------------------------------

1.  Filter Promotion records by Starting Date, Ending Date and Enabled

2.  Call [SetFiltersOnPromotionEntry](#setfiltersonpromotionentry) to filter
    Promotion Entry by the source document

3.  Filter by “Cannot be Combined” and check if a Promotion Entry record already
    exists for the document.

    1.  If so, the only promotion that can be applied is the promotion that has
        already been applied and which cannot be combined with others.

4.  Loop through the Promotion records (sorted by Priority) within the filters
    and for each:

    1.  Call [CheckCurrencyIsOkOnPromotion](#checkcurrencyisokonpromotion),
        [CheckCustomerIsOkOnPromotion](#currenciesareequal) and
        [CheckQualificationCriteriaAreOkOnPromotion](#checkqualificationcriteriaareokonpromotion)
        to determine if the source document meets the criteria for the given
        promotion to be applied.

    2.  Check that this Promotion does not stipulate “Cannot be Combined” or, if
        it does, that no other promotions have already been considered.

    3.  Insert a temporary copy of the promotion into a PromotionTemp variable.

5.  Two sets of Promotion Buffer records are populated

    1.  All Lines – records added by
        [AddPromotionBufferFromSalesLines](#addpromotionbufferfromsaleslines)

SetFiltersOnPromotionEntry
--------------------------

1.  Filter the Promotion Entry table by Source Type, Source Subtype and Source
    No.

CheckCurrencyIsOkOnPromotion
----------------------------

1.  Given the Promotion and Sales Header records, check if either the promotion
    allows conversion between currencies or the currencies are equal on the two
    records (see [CurrenciesAreEqual](#currenciesareequal)).

CurrenciesAreEqual
------------------

1.  Given two currency codes, return whether they are equal or not. Replace a
    blank value in either or both of the codes with the LCY Code from General
    Ledger Setup.

CheckCustomerIsOkOnPromotion
----------------------------

1.  Given a Promotion and Sales Header, get the Sell-to Customer record from the
    Sales Header.

2.  Check if there is a Promotion Criteria record matching either the specific
    customer, its corresponding Customer Price Group or an “All Customers”
    record.

CheckQualificationCriteriaAreOkOnPromotion
------------------------------------------

1.  Find the Promotion Criteria record(s) for the given promotion which
    represent Qualification Criteria. Loop through them and for each:

    1.  If the record is of type Minimum Quantity or Minimum Amount call
        [CalcQuantityAndAmountByQualificationCriteria](#calcquantityandamountbyqualificationcriteria)
        to determine if the minimum criterion has been met.

    2.  If the record is of type Promotion Code then call
        [CheckPromotionCodeFromQualificationCriteriaIsOnDocument](#checkpromotioncodefromqualificationcriteriaisondocument)
        to determine if the promotion code criterion has been met.

2.  Loop through the Promotion Buffer records that have been created by the
    calls to
    [CalcQuantityAndAmountByQualificationCriteria](#calcquantityandamountbyqualificationcriteria)
    and call [CalcAmountRates](#calcamountrates) for those records.

3.  Add these Promotion Buffer records to the set that is returned by this
    function.

CalcQuantityAndAmountByQualificationCriteria
--------------------------------------------

1.  Use QUE9030249 to determine which sales line records meet the Promotion
    Criteria record – whether of type Item, Item Discount Group or Promotion
    Item Group.

2.  Call [AddPromotionBufferWithCriteria](#addpromotionbufferwithcriteria) to
    insert a Promotion Buffer record that will be returned from the function.

3.  Call [CalcQuantityRates](#calcquantityrates) if the criterion is of type
    Minimum Quantity.

4.  Return the set of Promotion Buffer records that have been inserted (which
    will themselves be returned).

AddPromotionBufferWithCriteria
------------------------------

1.  Call [AddPromotionBuffer](#addpromotionbuffer) to insert a Promotion Buffer
    record.

2.  Populate the Qualification Quantity or Qualification Amount fields depending
    on the Criteria Type from which it is being populated.

CalcQuantityRates
-----------------

1.  Loop through the Promotion Buffer records provided and set the Quantity Rate
    as the percentage contribution of the Sales Line Quantity field to the total
    Sales Line Quantity of all lines.

    1.  e.g. If there are three lines of Sales Line Quantities 1, 3 and 6 their
        respective Quantity Rates will be 10, 30 and 60.

CheckPromotionCodeFromQualificationCriteriaIsOnDocument
-------------------------------------------------------

1.  For a given Promotion Criteria record and filtered set of Document Promotion
    Code records determine whether a Document Promotion Code record exists that
    satisfies the Promotion Criteria.

AddPromotionBufferFromSalesLines
--------------------------------

1.  For each Sales Line record with the filters that have been set:

    1.  Call [AddPromotionBuffer](#addpromotionbuffer)

2.  Call [CalcAmountRates](#addpromotionbuffer)

AddPromotionBuffer
------------------

1.  Check if a Promotion Entry already exists with the same combination of
    Source Type, Source Subtype, Source No., Source Line No., Line Type = New
    Line.

2.  If not, insert a new PromotionBuffer record with the following fields
    populated: Document Line No., Sales Line Quantity ,Sales Line Amount, Unit
    Price, Promotion No., Criteria/Benefit Line No.

CalcAmountRates
---------------

1.  Loop through the Promotion Buffer records provided and set the Amount Rate
    as the percentage contribution of the Sales Line Amount field to the total
    Sales Line Amount of all lines.

    1.  e.g. If there are three lines of Sales Line Amounts 1, 3 and 6 their
        respective Amount Rates will be 10, 30 and 60.

ApplyPromotionBenefitOnPromotionBuffer
--------------------------------------

CheckAppliedAndUpdatePromotionBuffer
------------------------------------

ApplyPromotionBufferToSalesDocument
-----------------------------------

AddPromotionEntryQualificationLines
-----------------------------------

Unapplying Promotions
=====================
