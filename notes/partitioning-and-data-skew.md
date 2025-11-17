1. Partitioning:
       - dataset broken down into multiple files is partitioning
       - it increases parallelism as multiple partitions can be executed across multiple executors
       - performance increse in filtering
2. Bucketing:
       - bucketing creates multiples files and data is stored based on hash function
       - performance increase while joining, since buckets are already present in the same executor and sorted
   

   |Aspect      |                 Partitioning                   |                     Bucketing                |
   |------------|------------------------------------------------|----------------------------------------------|
   |Purpose     |Skip reading irrelevant data (I/O optimization) |Avoid shuffles in joins (network optimization)|
   |Structure   |Separate directories per partition              |valueFixed number of files, hash-distributed  |
   |Best for    |Filtering queries (WHERE year = 2024)           |Joins on the bucketed column                  |
   |Cardinality|Works well with low cardinality (year, country) |Works well with high cardinality (user_id, order_id)|


  3. Repartition:
      - full shuffle happens
      - distributes the data evenely across all partitions
      - can be used to get rid of skewed partition
      - can increase or decrease the number of partition
      - wide transformation
    
  4. Coalesce:
       - shuffle does not happen
       - local partitions within the executors are merged
       - when data in each partition is less, then used to avoid tiny file problem
       - only decreases the partitions
       - narrow transformation
    
     
    
  6. Data Skew:
       - when few partition contains most data
       - executors containing skewed data takes more time running while other executors are idle

         Identiy Data Skew:
               - Spark UI: a task is running longer than others
                           input size/records is more for a task than others
               - AQE can auto detect and mitigrate skewed data but not when performing aggregation or skewed data at source
         Handling Strategies:
               - AQE handles join and post shuffle partition imbalance
               - salting - adding salt columns on the skewed column and processing them
               - isolate and handle skews parallely
         Range Partitions:
               - df.repartitionByRange(5, "age")
               - filter like age BETWEEN 10 AND 50
               - it will skip the partitions which does not meet the criteria
               - sorted data
               - does not guaranttee removal of skew


 |Aspect      |                 Hash Partitioning                   |                     Range Partitioning                |
 |------------|------------------------------------------------|----------------------------------------------|
 |Distribution|Based on hash functionBased on value ranges          |Good forEven distribution, joinsRange queries, sorted data
 |Risk|Can have skew if hash collides|Can have skew if data distribution is uneven|
 |Ordering|No guarantee|Data is sorted within ranges|
