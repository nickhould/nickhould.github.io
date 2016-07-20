1. Summary Analysis
    - What hurts
        - Having to schedule jobs, send emails etc (ETL) without a tool
        - Choosing the tool that best suits your needs
        - Knowing which data warehouse to use
        - Choosing ETL tools if you are on Microsoft
    - What people want?
        + A ETL tool
            + Open Source
            + Works on Microosft
    - What people buy?
        + Luigi
        + [Pipeline](https://rjmetrics.com/product/pipeline)
        + [petl](https://petl.readthedocs.io/en/latest/)
        + [DataFlow](https://cloud.google.com/dataflow/)
        + [BigQuery](https://cloud.google.com/bigquery/)
        + [Pachyderm](https://pachyderm.io/)
    - Essays Ideas:
        + What tool should you use to manage your ETL's?
        + What's the best ETL tool?
        + 
        

2. Data
Source: https://www.reddit.com/r/datascience/comments/4ki0v0/setting_up_etlreporting_in_a_start_up/

- Setting up ETL reporting in a start up
- Starting work in a newly formed start-up
- Main ETL/Analytics/Reporting guy
- A lot of work ahead of me
- They do not have any real ETL set-up
- Might be a good opportunity to build one from scratch
- Thoughts are using Luigi in Python
- Anyone got any experience with this?
- My main needs
	- schedule a bunch of jobs from database extraction to auto-emails
	- what etl tool? preferably open source

- What kind of sources and targets are you going to be working with on what OS?
- I looked at `petl`
- Simple data shuffling
- Luigi seemed like overkill
- One-off scheduled processed in place that are using pandas

https://petl.readthedocs.io/en/latest/


- Connects to Redshift/Big Query
	- [Why you shouldn't build your own data pipelines](https://blog.rjmetrics.com/2015/11/17/why-you-shouldnt-build-your-own-data-pipeline/)
    - [Engineers shouldn't write ETL](http://multithreaded.stitchfix.com/blog/2016/03/16/engineers-shouldnt-write-etl/)
    - [The data infrastructure meta analysis how engineering organizations built their big data stacks](https://blog.rjmetrics.com/2015/10/15/the-data-infrastructure-meta-analysis-how-top-engineering-organizations-built-their-big-data-stacks/?utm_source=etldatabase&utm_medium=microsite-referral&utm_campaign=microsite-referral+etldatabase)
- Don't use big data technologies if you don't have to
- If you don't have a data warehouse, [BigQuery](https://cloud.google.com/bigquery/)
    + BigQuery is really fast but they charge you per query
    + The only alternative I know of is to set up a Hadoop cluster
    + Just use SQL Server
        * Distributed Queries
    + BigQuery is more suited for flat data like log files
- [DataFlow](https://cloud.google.com/dataflow/)

- Just switched jobs to lead data science at a young company
- Data Warehouse is non-existent
- Existing infrastructure is all Microsoft based
- I am a solid Apple/Linux/AWS/Open source fan
    + That's not irreconcilable problem
- I landed on Luigi
- Other tools that may replace it some day. Two pluses about luigi:
    - Open Source
    - Actively worked on
- I've started building the ETL's
    + sourcing data from SQL server
    + raw CSV
    + redshift
    + HDFS
- Reporting is a web server that's running a django app
    + Views with D3 - NVD3
- Looked for open source alternative to tableau
- I recommend Apache Nii for streaming data
- US FDA uses Luigi to process their data
- I'll put a vote for Postgres on Linux because it's super cheap and fast to set-up
- Shameless plug for a tool I wrote. It's called [Pachyderm](https://pachyderm.io/)
- If you are starting from scratch, I would recommend going with the Elastc Stack.
- http://www.pentaho.com/product/data-integration
