# Importing a Database

Please note: This is a continuation of *Installation.md*. 

Once MySQL has been installed, double-click on the connection present, and enter credentials. These are the same credentials that were created when installing:

![Pasted image 20240504120328](https://github.com/lm3nitro/Projects/assets/55665256/e97b2581-47e9-43d3-ba1d-5b7b883ba2cc)

### New Schema

To create a new schema, click the icon highlighted, name the new schema, and click *Apply*

![Pasted image 20240504123321](https://github.com/lm3nitro/Projects/assets/55665256/89b968d6-44d9-4ddc-9692-146b802ee567)

Once you click apply, a new dialog box will open. This will reflect the change you are applying, in this case a new schema name lm3nitro. To confirm, click *Apply*
 
![Pasted image 20240504123420](https://github.com/lm3nitro/Projects/assets/55665256/889735f2-602f-49c5-ba75-b57ca9b29c12)

Select *Execute SQL Statements* and after click on *Finish*

![Pasted image 20240504123532](https://github.com/lm3nitro/Projects/assets/55665256/15f4424d-c3a8-4540-bf1b-a61c1374ebfc)

Once the dialog closes, we should see that the new schema has been created, if not you can refresh

![Pasted image 20240504123653](https://github.com/lm3nitro/Projects/assets/55665256/b192e5da-2d1a-4e3a-a0fe-ace1fe8eaec0)

### Import Data

Now that we have created the Schema, we will need to import our data. To do this, expand the schema and select *Table Data Import Wizard*

![Pasted image 20240504123855](https://github.com/lm3nitro/Projects/assets/55665256/a9091241-436a-444b-9aa4-6826eae2c987)

In the new dialog, select *Browse* and choose the data that you wish to import:

![Pasted image 20240504124048](https://github.com/lm3nitro/Projects/assets/55665256/4451ad57-62ab-458b-bae4-ca43956908da)

Once the dataset has been selected, you will be prompted to select a destination. Here you can keep the name or rename it. Once complete, click *Next*

![Pasted image 20240504124317](https://github.com/lm3nitro/Projects/assets/55665256/308d9065-79c7-43a6-a525-244b4b1540f2)

In the next window, you can double check the columns that will be created using the dataset selected and verify the type of data in each column is correct (int, text, etc.)

![Pasted image 20240504124358](https://github.com/lm3nitro/Projects/assets/55665256/e1518360-19df-44b0-9035-2fe46996d721)

Make sure all the needed tasks complete and click *Next*

![Pasted image 20240504124524](https://github.com/lm3nitro/Projects/assets/55665256/c9cf19ae-818b-4dca-92eb-d1f17e5da62e)

We can confirm that 475 records were imported (This will be dependent on the data set used). Click *Finish*

![Pasted image 20240504124613](https://github.com/lm3nitro/Projects/assets/55665256/b76f3af2-0f86-467f-912a-12ea46b4f5d7)

Once that completes, we can refresh and ensure that our table/dataset was imported properly:

![Pasted image 20240504124750](https://github.com/lm3nitro/Projects/assets/55665256/9381bbfd-f633-4707-9b92-52098d142fba)

Now that we have our schema and dataset ready, we can now do some queries and see what information we are able to filter for. This is continued in the *MySQL Queries.md* page.
