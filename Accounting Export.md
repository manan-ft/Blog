## How to Export Tally/Busy friendly Accounting Data from ERPNext?

## What is ERPNext?

ERPNext is world's top Opensource ERP software used by more than 5000 companies across 60+ countries around the world. A true ERP lets you manage your complete business under one software. It includes modules like 
1. Customer Relationship Management (CRM)
1. Sales
1. Purchase
1. Stock/Inventory 
1. Human Resources
1. Manufacturing (including Production Management)
1. Support (for after sales service)
1. Accounting
In addtion to the wide range of modules, 
1. It integrates with your Email
1. Makes Printing documents easy with standardization
1. Cloud based software lets you manage your business from anywhere in the world
1. Lets you manage Approvals easily

## Why Export of Accounting Data is necessary?

Although ERPNext can provide a Single Source of Truth for a company including Accounting Data, there are a few reasons why the Acccounting Data might need to be exported to Accounting Softwares like Tally or Busy.

1. **Compliance Reporting:** Accounting Compliance can be tricky. With changing regulations, the compliance requirements can be dynamic too. ERPNext being a complete package out of which Accounting is just a part of it, ERPNext may not get patches for Compliance Reports at a pace that matches most Simple Entry based Accounting Softwares. If you are able to export the Accounting Data from ERPNext to these softwares, you can take advantage of its Compliance Reporting.
1. **Auditors/Chartered Accountants:** Consider this, your business is one of many files for your Auditors/Chartered Accountants, probably one out of 100s. People in the Accounting and Finance Industry are well-versed with Accounting softwares like Tally or Busy, it is a huge learning curve for them to adapt to ERPNext. It can be a huge help if you send them data in Tally or Busy files which they can load on their software and work on your Data.
1. **Parallel Run:** Companies sometimes want to do a parallel run with their legacy software for about a year to find out if there are any gaps or mistakes happening in data entry in ERPNext. Also they are used to viewing their year end Financial Statements and Reports in legacy software. In such cases, exporting data from ERPNext into legacy softwares becomes necessary.

## How to export?

We will be considering importing data into Busy from ERPNext. Principles for doing so for Tally remain the same except the **Format.** **Tally** allows import of data only in **XML** file, whereas **Busy** accepts import in **Excel** file as well making it a little easier. Steps for exporting data is as follows
1. **Match Masters:** First and the most important step is to match the Masters in both the softwares. Care should be taken that the spelling including spaces should be same in both the Softwares, otherwise export from ERPNext will not be reconginised by the Legacy software while importing. You need to take care of the following masters
    1. **Chart of Accounts:** Make sure to match the Chart of Accounts in both Softwares. Ledgers used in ERPNext should be present in the Legacy Software as well. Since this involes a Tree Strucuture, creating this Chart of Accounts manually might make more sense than using any Import/Export option.
    1. **Customer/Supplier Master:** Customers/Suppliers created in ERPNext should be created in the Legacy Software as well along with its Ledger if applicable. Data Export functionality in ERPNext can be used to get this Master data the first time and then incremental additions may be added Manually. Address and GST information should be imported along with the Party details.
    1. **Item Master:** Basic Item details will have to be exported from ERPNext and imported to Legacy Software. Basic details like Item Code, Item Name and UOM might be enough for Accounting purposes. This information is required for generating reports like *HSN-wise-summary of outward supplies*. 
1. **Upload Transactional Data:** This is the tricky part of Data Migration, unlike Masters the volume of Transactional Data can be daunting. Companies of medium size can have 100s of Invoices, Payment Entries, Receipt Entries making it difficult to keep-up with the velocity of Transaction if an automated way is not used. Following steps can be used to migrate data from ERPNext to Legacy Software:
    1. **Find out Import Data Structure:** Every software has a strucutre and certain rules with which it will accept Data Import files. For example, in order to define the type of GST on Sales Invoice and Type of Rates, you need to define it as " I/GST-TaxIncl." for Inter State Sale with GST Tax Inclusive Rates. Also you need to define this only once for an Invoice and not for each line item. Best way to find out these rules and strucure is to make a few entries in the software and then export it.
    1. **Mapping of Import File Headers:** Once you have a format of the Import file, map the header of each column/field with the corresponding field in ERPNext Database. An example of this mapping is shown in the image below
    1. **Script for Generating the Import File:** After mapping the fields, you can write API to generate the file in required format. You will need knowledge of SQL Queries and Python scripting to write this API. You will need to write these APIs in your Custom App folder in file `api.py`. A sample of such script for generating the Sales Invoice file that can be imported in Busy is as below:
    ``
    1. **Button for Downloading Data:** Once the APIs are in place, the final part is to connect it to a Button on ERPNext so the users can download it easily. The best place to place it is the Company Doctype, where you can add a button for each Document. A sample of Custom Script used for adding button is as follows:
    
    `frappe.ui.form.on('Company', {
	refresh : function(frm) {
		frm.add_custom_button(__('Sales Invoice Busy Data'), function() {
			var sales_invoice = "/api/method/vhms.api.get_sales_invoice_busy_data?company_name=" + frm.doc.name;
            window.open(sales_invoice);
		});
	}
    });`
    Below GIF shows how the Button works


    This exported XLXS/CSV file is ready to be imported into your Legacy Software making sure your data in both the softwares is in sync.
    
## Other Tips 
Following are some of the learnings from our Experience:
1. **Taxes:** Make sure the correct rates are exported. Check your script and output to check if you have factored for Items that may have Rates which are tax inclusive and otherwise. For example, if you are considering taxes to be included in Rates in ERPNext, make sure you have factored that in while exporting the data and correct rates are being exported and imported. If your Rate for an Item is Rs.100 and it includes a GST of 18%, you should consider this an define the correct taxes on the export file otherwise your legacy system will consider GST on top of your Rate i.e Rs.100.
1. **Discount:** You need to export the Discount information as well. Make sure the Discount information is exported correctly and applied the same way in Legacy Software as well. For example, by default Busy had difficulty considering Discount at Invoice level since the Discount at invoice level was applied but Taxes were not recalculated like in ERPNext. To handle this exception, we calculated % Discount line item-wise and exported this information line item-wise.
1. **Rounding off:** Make sure to export the Rounding off information as well. If this information is not exported, you might have problems matching the transaction value if Rounding off values are not exported.
1. **Cross Check:** You might need a few iterations till you perfect the output file. Double check the totals to verify that the correct data has been imported, if you find a difference, you will have to find out the error and fix it. Good luck with the Auditing :)

## List of Transactions to be exported

Finally, once you have completed the export format for one document, you will have to repeat the whole process to include the following transactions to export the complete Accounting Data
1. **Sales Data:** Sales Invoice and Sales Return
1. **Purchase Data:** Purchase Invoice and Purchase Return
1. **Journal Vouchers:** Journal Entries including Contra, Credit Notes and Debit Notes
1. **Payment and Receipt Data:** Payment Entry

We hope this blog gives you confidence in using ERPNext for you organisation and does not stop you from adapting it because your C.A does not want to use ERPnext!



    
