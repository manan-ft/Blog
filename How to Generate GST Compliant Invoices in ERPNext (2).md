## How to generate GST Compliant Invoice numbers in ERPNext ?

GST was rolled out in India with the aim of bringing the whole country under one tax regime- **_“One Nation, One Tax”_**. The Jury is still out there to decide whether the GST has been beneficial for the businesses or not.

Regardlessly, if you are operating as a Business in India, you have to comply with the GST Rules. In this blog, we will see how to comply with the Invoice Number rule of GST.

As per **_Rule 46(b) of  Central Goods and Services Tax (CGST) Rules, 2017 Part – A_**,the rule for Invoice Serial No is as follows:_ “a consecutive serial number not exceeding sixteen characters, in one or multiple series, containing alphabets or numerals or special characters hyphen or dash and slash symbolised as - and / respectively, and any combination thereof, unique for a financial year; “_

## Points to be taken note of in this rule are

1. **Consecutive Serial number:** This means that the serial numbers cannot be skipped or changed. What this implies is:
    1.  You cannot have invoices 001,002,004
    1. Invoice revisions cannot have “-1” appended to the original invoice number. Example: INV-0001-1
1. **Not more than 16 digits:** The number of digits in the serial number cannot exceed 16 digits
1. The serial number can consist of **alphabets, numericals, - or  /**
1. Serial number has to be **unique for a given financial year**

## Implications of Non-Compliance


1. **Legal Implications:** If the serial number is not consecutive, with a few serial numbers missing, it can be construed as certain serial numbers where skipped and the skipped invoices were unbilled to save on tax. These can have serious legal implications in the form of Noticies and penalties.
1. **Validation errors on GST Portal:** If the invoice number is longer than 16 digits, it will not be accepted on the GST portal for creation of E-way Bill and during filing of GST Returns. Once an invoice is generated and sent to Customer, changing the invoice number becomes cumbersome, even if you want to

## ERPNext Default Configurations

1. **Naming Series:** The default Naming Series on ERPNext for Sales Invoice is “ACC-SINV-.YYYY.-#####”. If you count the number of digits, it is 19, which exceeds 16. Therefore you will have to change the naming series to something under 16 digits
1. **Cancellation:** ERPNext lets you cancel and Amend all Doctypes including Sales Invoice. The ID of amended version is changed with a version number to keep a track of the version. So if you cancelled invoice number ACC-SINV-2020-00010, the amended invoice number will be ACC-SINV-2020-00010-1, this will cause compliance issues on two fronts
    1. The invoice number has 21 digits
    1. The serial number will not be considered consecutive as “-1” is added to the invoice number

## Suggested Configurations
In order to take care of the invoice number rules, following configurations are suggested

1. **Naming Series:** Change the naming series to something shorter which includes, 
    1. Initials of your company
    1. Calendar Year
    1. Invoice Number
Instead of having ACC-SINV-.YYYY.-#####, have a Naming series like ABC-.YYYY.-#####, where ABC is the initial for your company, .YYYY. will add the Calendar year based on the invoice date and the numbering will be 5 digits long (#####). You can also have a separate series of each of your Company or the Brands that you sell, in which case you will have to manually select the series everytime, increasing the chances of human error.

1. **Cancellation:** It is suggested that Cancellation of Sales Invoice be disabled for everyone in the organisation except the Administrator or Accounts Manager. Since cancellation, adds a version number to the Invoice, it disrupts the invoice numbering. The only way to stop this problem is by disabling the cancellation. Mistakes made in the sales invoice can be tackled in the following way
    1. **Keep the Sales Invoice in Draft mode until all the details are verified.** Make sure you verify at least the following before finalising/submitting a Sales Invoice
        1. **Customer’s Address:** Customer’s Address should be added to the Customer Master and correct address should be selected in case of multiple Customer Addresses
        1. **Customer GST Number:** The invoice should have Billing Address GST Number under the Address and Contact section. This is fetched from the Customer’s Address, if the GST number is not added in Party GST Number field in Customer’s Address, it will not be fetched. If the party is Unregistered, make sure to mention it on the Customer Master
        1. **GST Calculation:** Correct GST should be calculated, 
            1. In State vs Out State based on the Customer Billing Address should be applied, 
            1. Tax Rates should be applied correctly, 18% vs 28% etc. This is fetched from the Item Tax Template
            1. Correct HSN Numbers, fetched from Item Master
            1. Tax Break-up table should be correct
        1. **Correct Rate and Amount on Invoice:** Make sure the rates and amounts on the invoice are correct as desired. Some Custom Script of Pricing logic may change the Rate/Amount of items. If you are over-riding Pricing Rule, make sure to select the “Ignore Pricing Rule” flag under Currency and Pricelist Section.
            1. **_Pro-tip:_** If your invoice is being generated by Warehouse staff, they may not have the right to the “Ignore Pricing Rule” field since it is assigned Level 1 permission by default. You will have to assign this right to Stock Manager role from Role Permissions Manager

## Corrective Action for Mistakes in Sales Invoice

This is a tricky situation. In simple entry based accounting softwares like Busy or Tally one can change the Invoice details even at a later stage, whereas in ERPNext, you cannot change details of a Submitted Sales Invoice if you make any mistake. In order to correct a mistake on Submitted Sales Invoice, one has the following options (since Cancellation and Amendment creates problems stated above);
1. **Credit Note:** Create a Credit Note of the Sales Invoice in question and create a new Sales Invoice with correct details. This will make sure that the wrong Sales Invoice is nullified and the new Sales Invoice is generated as per the running Invoice Serial Number
1. **Cancel/Delete/Create:** Sometimes creating a Credit Note might not be practically possible and you might want to just amend the existing Sales Invoice without creating a new Invoice number. In such cases, you will have to first stop all the Invoice transactions or perform this task when the transactions are closed.
    1. **Cancel:** Cancel the wrong invoice and take a print/copy of the cancelled invoice. Let us consider the Invoice cancelled is ABC-2020-00011
    1. **Delete:** 
        1. **Delete** the Cancelled invoice
        Note: You may have to delete any Documents linked further to Sales Invoice. The system will automatically unlink the   Payment Entry linked to the invoice if any, make sure you have noted down the Payment Entry number to relink the new invoice.
        1. **Update** the Sequence Number from Naming Series to one less than the cancelled invoice number [11-1=10 in our case].
    1. **Create:** Create a new Sales Invoice with correct details, double check the details and Submit the invoice. This will create an invoice with the same serial number “ABC-2020-00011”

## Future Release of ERPNext

With v13 ERPNext will be moving to **Immutable Ledger** where Cancellation of Accounting Entries will not be possible, only reversing the entries will be possible. This release will solve the Cancellation and Amendment issues but will require users to be very careful while making entries.
