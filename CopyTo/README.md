# Simple Data Factory (Copy To) Demo

### Scenario: Retail Data Bottleneck

#### Problem statement
  A mid-sized e-commerce company, SampleCompany, is struggling with fragmented product data.  
Their supplies publish daily product updates as a products2.csv file hosted on a HTTP 
endpoint.

  In this example, we will utilize microsoft sample data in GitHub.

    https://github.com/kromerm/adfdataflowdocs/blob/master/sampledata/Product2.csv

  Currently, SampleCompany analysts manually download the file, clean it in Excel and upload 
it into the data warehouse.  This process is:
- Error-prone (files sometimes missed or overwritten)
- Slow (manual steps delay updates by hours)
- Non-scalable (as suppliers grow, the workload multiplies)

  The leadership team wants a repeatable, automated workflow to extract Products2.csv from
HTTP, validate it, and load it into their analytics platform.  

  But in this case we are limiting the demo scope into small chunks, to continue with the 
validations, transformations, and analytics sections please go to AzureDatabricks repository.

Now, here is the workflow narrative;

01.) Triggerring the workflow
  At a specific time, a scheduled job kiicks off in Azure Data Factory.  The Pipeline is 
design to fetch the latest Products2.csv from the supplier's HTTP endpoint.

02.) Extract (HTTP -> Raw Storage)
  - The pipeline must send an HTTP GET request to the supplier's endpoint.
  - The file is streamed directly into Azure ADLS under a
    raw/products/ folder.
  - Metadata (timestamp, source URL, file size must be log for governance purposes).

## How to build this project
01.) Sign-in in https://portal.azure.com/ with your account.
02.) Create a Resource Group under your subcription.
      Resource Group: Demo_daf_rg
03.) Under the Resource Group, create a Storge Account.
      Storage Account: demodaf
04.) Create a container bronze
      Container: raw
05.) Under the same Resource Group, create Azure Data Factory Instance.
      Azure Data Factory: demoproductsdaf
06.) Open the ADF instance

Now, in this moment you are ready to create a pipeline. But first let's create linked services.

07.) In the left side panel, click "Manage" button then navigate to Connections then
Linked Services.
08.) Click New, then select HTTP in the Data Store tab. 
09.) Enter desired linked service name for your source connection.
      HTTP: HttpServer_GitHub_SampleData
10.) Enter Base URL.
      https://github.com/
11.) Select Authentication Type as Anonymous
12.) Test connection then CREATE
13.) Repeat step 6 if not in Lineked Services page.
14.) Click New, then select Azure Data Lake Storage Gen2 in the Data Store tab. 
15.) Enter desired linked service name for your Sink connection.
      Azure Data Lake Storage Gen2: daf_adls
16.) Select Azure Subcription and Storage Account.
17.) Test connection then CREATE

Let's build the pipeline!!!

18.) Under Activities menu, expand Move and transform then drag and drop "Copy data"
into the 
19.) Under the General tab, enter name of the activity.
      Copy Data: CopyTo
20.) Configure Source tab by selecting the linked service "HttpServer_GitHub_SampleData" and enter relative URL to create a source dataset.
      Relative URL: kromerm/adfdataflowdocs/raw/master/sampledata/Product2.csv

NOTE: (1) when copying the URL directly from GitHub, you need to replace blob with raw
      (2) Make sure First row as header is enabled and correct colun delimiter

21.) Once, connection is established.  Test and Preview data  
22.) Configure Sink tab by selecting the linked service "daf_adls".
23.) Enter File path
      raw/Products/Products.csv
NOTE: (1) Make sure your required folder hierarchy and file name is entered

24.) Once, connection is established.  Test and Preview data  
25.) Validate pipeline.
26.) Debug and verify file from storage account containers.
27.) Once you confirm that the file is existing and exactly matching.

## Congratulations!!! you completed a very simple ingestion of data using Azure 
Data Factory.

NOTE: Please do not forget to delete the Resource Group you created to prevent recurring costs.
