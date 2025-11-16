 Catalyst optimizer:

   - logical plan and physical plans are part of catalyst optimizer
   
   ![download](https://github.com/user-attachments/assets/252b7a54-0f26-4609-b483-49c8ce5b7a56)


1. Logical Plan:
   - parses the query
   - applies schema/catalogue (does column actually exist in shcema?)
   - applies the rule based optimizations (predicate pushdowns, projection pushdowns(column prunning), rearranging filter, constant folding, boolean expression simplification)
  
2. Physical Plan:
   -  defines how to run a query
   -  how to process data and interact with it
   -  multiple plans are created
   -  based on statistics cost modele assigns cost and select most optimized plan



PLANNING PHASE (Catalyst's domain)
- Parse query
- Resolve references
- Apply RBO optimizations
- Generate physical plans
- Use CBO to select best plan

     ↓
  
EXECUTION PHASE (AQE's domain)
- Execute the plan
- Monitor runtime statistics
- Re-optimize on the fly
- Adjust partitions, switches joins, handles skew
