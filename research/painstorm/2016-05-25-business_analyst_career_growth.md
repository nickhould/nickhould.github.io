1. Summary Analysis 

    -   What hurts
        ⁃   Data analysis needs exceeds Excel limitations
        ⁃   Doing manual manipulations of data that can knowingly be automated
        ⁃   Not knowing how other companies are dealing with data pipelines
        ⁃   Having the impression that you are not doing things in an efficient matter
        ⁃   Fear of not knowing “enough” SQL
        ⁃   Not knowing dashboarding tools alternative
    -   What people want?
        ⁃   Be more effective at ETL’ing data 
        ⁃   Learn the fundamentals of data warehousing, BI etc.
        ⁃   Learn about what to expect from a data stack
        ⁃   Know how to enable end users to visualize and query the data 
    -   What people buy?
        ⁃   The Data Warehouse Toolkit by Ralph Kimball
        ⁃   AWS RDS (Reporting Application)
        ⁃   Qlikview, Tableau, http://www.tableau.com/learn/training
        ⁃   Pentaho 
        ⁃   Luigi/Airflow
        ⁃   http://datapipelinearchitect.com/tools-for-combining-multiple-data-sources/
    -   Essays Ideas:
        ⁃   Don’t build another dashboard solution
        ⁃   A startup guide to data architecture
        ⁃   From Excel to Tableau
        ⁃   From Excel to SQL
        ⁃   Data Reporting Pipelines Architecture

2. Data Collection
Source: https://www.reddit.com/r/datascience/comments/3pn7jd/hired_as_a_business_analyst_quickly_turning_into/

- Working for a growing startup
- Replacing BA that is moving out
- BA relied heavily on Excel
- Exceed the limits of Excel
- Bit of SQL experience to try to learn
- Move all the reporting to a reporting DB
- 100k-200k transactions
- Cloud DB AWS RDS running Postgres
- Copying data from Postgres (Application DB) to  AWS RDS (Reporting Application)
- What would be the best thing to look into or neat/tricks/books/webinars/youtube/etc that cn help me learn how to do this more effectively?
- How do you do it at your company?

 
- Sounds like you need to build a data warehouse
- It’s difficult to learn how to build data warehouse in a month
- BUY: Book: The Data Warehouse Toolkit by Ralph Kimball
- Version control ETL code
- Learn to write highly optimized SQL code via PG query analyzer
- Use a reporting tool like Tableau

- I asked them about why they won’t just use something like Qlikview or Tableau. They would rather pay someone to build their data warehouse. I recently found Amazon Quicksight: https://aws.amazon.com/quicksight/

- Lone business analyst / data scientist / DBA
- Highly recommend setup with Tableau since you are on your own
- ETL with Spark and reporting with Redshift
- Star Schema for reporting

- They were rather against bringing in a outside solution
- Growth of the company, combined with rampage requests for ad hoc reports , tableau would have get me up to speed faster on data warehousing
- Stoked to have the pressure to learn as fast as I can

- Glad to see people use Tableau at work
    - Tableau is hot right now
    - Tableau is cheap
    - What’s the best way to learn reporting via Tableau?

- I have experience with tableau, somewhat minimal
- Tableau:
    - Create visuals from live sources
    - Create dashboard - compilation of visualization with parameters
    - Publish it via Tableau Server => webpage
    - The best part is how easy to do

- Tableau has a 2-week trial and extensive, free, on-demand training on their website: http://www.tableau.com/learn/training

- Hired as BA role for a big company
- Peers use our reporting interface to pull down pre-made reports and manipulate them in excel
- I taught myself SQL (Teradata) and do it with backend without frustrating interface
- How much SQL experience did you have when you joined?
- What did you learned the most from doing? Any tips you want to share?

- Limited SQL knowledge: college courses and simple queries
- Check out CTE: https://www.postgresql.org/docs/9.1/static/queries-with.html
- Window Function

- Similar Experience with current job starting 4 years ago
- Data Warehouse is what you are building
- BUY: Pentaho for ETL using different db types
- BI Layer: Tableau, Tableau Online, Microsoft Power BI, SAP Business Intelligence

- What is your current job environment?

- Everyone keeps hinting on finding a BI and Analytics solution
- Tableau are kind of out of question
- Is it really that much more efficient to just find a BI solution?
-  Should I just suck it up and just tell them if you are looking for this, buy this.

- Sketch out the state of your data system today.
- Cuba/Star Schema: http://www.seemoredata.com/bi/differences-between-cubes-and-star-schema/
- Toady there are a ton of tools to help you do the same thing way fast and with way less people.

- The last bit about “Viz/Present” aspect was gold for me. I still need to hash out exactly what my company is wanting the end user to be able to look at, but the wide range of solution will definitely help me get a handle on what specifics need to be ironed out before settling  on a solution.





