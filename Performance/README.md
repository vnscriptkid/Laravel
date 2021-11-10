## 1. lesson-01-measuring-your-database-performance
- tool: Debugbar
- see tab `Queries`, 
  - look for duplicated queries => __n+1 problem__
  - eager load related resource
- when to use index: look for `order by something` -> add index for `something`
- problems with eager loading: look at `Models` tabs, how many model objects has been populated

## 2. lesson-02-minimizing-memory-usage
