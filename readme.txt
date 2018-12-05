Planed Deployment Date- 11-21-2018 for PROD
-----------------------------------
Existing tickets--

This release contains only incremental deployment scripts and SSIS package with the following fixes for
FGP-483:

1.  Audit Recoupment - No File Found on Subject
2.  Audit Recoupment - No New File Found on Subject
3.  Audit Recoupment - Directory path does not exist on Subject
3.  Audit Recoupment - Directory not accessible on Subject
4.  Status will be updated as failed in ssis_execution in case of exceptions.
5.  If destination directory mentioned as parameter param_db_file_path not exist then process will create the folder.

FGP-495:

1.  In case of successful file processing:-
    1.1 - E-mail body will contain processed file name and
    1.2 - Attached report file will have file name as prefix. e.g. AUDITRECOUPMENTTABLE_#19_Import_rpt.csv

2.  In case of failure in file processing:-
    2.1 - E-mail subject will have 'File Validation Report Attached' as postfix
    2.2 - E-mail body will contain processed file name and
    2.3 - Attached report file will have file name as prefix. e.g. AUDITRECOUPMENTTABLE_#19_ImportValidation_rpt.csv

FGP-572:

1. audit recoupment data to be processed as per the provided filters, paid claim logic is in place now
2. for bad data, new validation is added and it runs on bad data only
3. Existing values in DB are compared for filter columns and email attachment conatains the cause for validation fail.

FGP-556:

1.  One email with two attachement files to be generated in case pre-validation fails:
    1.1 - One file will contain Validation cleaned data,this one will be re-processed and
    1.2 - Another file will contain the validation failed report with error data.

2. email SP has been cleaned, all hard-code values have been added as email templates and the logic changed accordingly.

FGP-743:
Audit Recoupments - Round Totals to 2 Decimal Places before adding to OneArk and on Macro.

1. Amount to recover field is rounded to 2 decimals in Macro 
2. Amount to recover field is rounded to 2 decimals in SSIS package using Derived_column step.

FGP-762:
Sales Tax Recoupments - Validation for already loaded claim for other type of recoupments

1.Created the below table
	Table: audit_types
	ID – Sequence, INT NOT NULL ß PK
	Code – CA, OA, DA, ST, INT NOT NULL <-- Unique Index
	Description – varchar(100) NOT NULL
	Create_Date
	Create_User
	Update_Date
	Update_User
2.Create INSERT Scripts to insert the 4 Audit Types
	CA – Compliance Audit
	OA – Onsite Audit
	DA – Desk Audit
	ST – Sales Tax
3.Add extra validation on GP Upload process to make sure the column AUDIT NUMBER first 2 characters on Excel has 1 of the 4 values we are adding to the new table audit_types. If it doesn’t, add to validation error and explain in comment area. Error message: [First 2 characters from column AUDIT NUMBER Excel] does not matches valids Audit Types.
4. Fetched multiple validation errors in single row per each record in excel which is processed with pre validation failure.

FGP-677:
Updating Validations inside Macro File Cleaning Process

1.Implemented column name validations.if any column name is differed, entire file considered as errored.
2.Highlighted errored column headers or errored data in yellow color
3.Removed unused code like copying column headers into clear and error files
4.Moved common code to the user defined function for resetting work sheets and called in both reset button click and after cleaning process.
5.If the file is already processed by Macro then provided the message like "File name already exist. Please rename file by adding version number" and exited from the process
6.Provided inline comments for the existing functional implementation.

FGP-747:
Changes to last message on Macro for Recoupments

1) Show message for failure & show file path of error file 
2) Show message for success & show file path of cleaned file