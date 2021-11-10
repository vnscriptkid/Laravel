## 1. lesson-01-measuring-your-database-performance
- tool: Debugbar
- see tab `Queries`, 
  - look for duplicated queries => __n+1 problem__
  - eager load related resource
- when to use index: look for `order by something` -> add index for `something`
- problems with eager loading: look at `Models` tabs, how many model objects has been populated

## 2. lesson-02-minimizing-memory-usage
![image](https://user-images.githubusercontent.com/28957748/141107255-667b537b-cd85-4b19-ab4a-a1cdb77ea9be.png)
- look at amount of data loaded, too much?
- why? `select *`
- solution? select what's needed
- where? index page (huge collection of posts)
<img src="https://user-images.githubusercontent.com/28957748/141107836-bcc12080-b186-4f02-9517-9dfb37a6b88b.png" height="150px"/>

## 3. lesson-03-getting-one-record-from-has-many-relationships

## 4. lesson-04-creating-dynamic-relationships-using-sub-queries

## 5. lesson-05-calculating-totals-using-conditional-aggregates

## 6. lesson-06-optimizing-circular-relationships

## 7. lesson-07-setting-up-multi-column-searching

## 8. lesson-08-getting-like-to-use-an-index

## 9. lesson-09-faster-options-than-where-has

## 10. lesson-10-when-it-makes-sense-to-run-additional-queries

## 11. lesson-11-using-unions-to-run-queries-independently

## 12. lesson-12-fuzzier-searching-using-regular-expressions

## 13. lesson-13-running-authorization-policies-in-the-database

## 14. lesson-14-faster-ordering-using-compound-indexes

## 15. lesson-15-ordering-by-has-one-relationships

## 16. lesson-16-ordering-by-belongs-to-relationships

## 17. lesson-17-ordering-by-has-many-relationships

## 18. lesson-18-ordering-by-belongs-to-many-relationships

## 19. lesson-19-ordering-with-nulls-always-last

## 20. lesson-20-ordering-by-custom-algorithms

## 21. lesson-21-filtering-and-sorting-anniversary-dates

## 22. lesson-22-making-n-plus-1-issues-impossible

## 23. lesson-23-ordering-data-for-humans-using-natural-sort

## 24. lesson-24-full-text-searching-with-rankings

## 25. lesson-25-getting-the-distance-between-geographic-points

## 26. lesson-26-filtering-by-geographic-distance

## 27. lesson-27-ordering-by-geographic-distance

## 28. lesson-28-filtering-by-geospatial-area
