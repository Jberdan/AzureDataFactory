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
validations, transformations, and analytics part please go to AzureDatabricks repository.

Now, here is the workflow narrative;

1.) Triggerring the workflow
  At a specific time, a scheduled job kiicks off in Azure Data Factory.  The Pipeline is 
design to fetch the latest Products2.csv from the supplier's HTTP endpoint.

2.) Extract (HTTP -> Raw Storage)
  - The pipeline msut send an HTTP GET request to the supplier's endpoint.
  - The file is streamed directly into Azure ADLS under a
    raw/products/ folder.
  - Metadata (timestamp, source URL, file size must be log for governance purposes).
 
