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
- scenario: one `user` has many `logins`
<img src="https://user-images.githubusercontent.com/28957748/141114050-97c28211-26bc-410f-8af6-b9589665fe45.png" height="200px"/>

- goal: for one `user`, get the latest login
- approaches:
  - naive: while fetching users, eager-load logins (DB side) sort and take the latest (BE side)
  - denormalized way: store `last_login_id` in `users` table
  - delegate job of sort and find last login to DB using `subqueries`
    - mechanism: for each user, find all logins of that user, find the latest, select field `created_at`
    - notice: subquery is put in where clause, and must resolve to one value (1 row, 1 col)
    - resolved value is a computed column

<img src="https://user-images.githubusercontent.com/28957748/141115832-f91b3db2-fc46-4d56-9527-6f7f13320cf0.png" height="200px"/>

<img src="https://user-images.githubusercontent.com/28957748/141116380-f50361ff-0781-422e-be8e-0ad9658d2eff.png" height="300px"/>

## 4. lesson-04-creating-dynamic-relationships-using-sub-queries
- context: one `user` has many `logins`
- normal relationship:
  - `user.logins`: returns all `logins` of `user`
  ```sql
  select * from logins where user_id = :userId;
  ```
  - `login.user`: returns `user` of a `login`
  ```sql
  select * from users where id = :userId;
  ```
- dynamic relationship:
  - diff: instead of `user hasMany logins` through `user_id`, it'd be `user belongsTo a login` through `last_login_id`
  - `user.lastLogin` expects `logins` table must have `last_login_id` column

## 5. lesson-05-calculating-totals-using-conditional-aggregates
<img src="https://user-images.githubusercontent.com/28957748/141331293-65aac50a-8ae2-40b1-bb56-dcb5cd3d8a59.png" height="100px"/>

<img src="https://user-images.githubusercontent.com/28957748/141331908-2efc5c19-e2d5-46f2-9a50-6e265b75a839.png" height="100px"/>

<img src="https://user-images.githubusercontent.com/28957748/141332176-e91e0233-8c37-4498-b0ac-d3051b25a8e4.png" height="100px"/>

- filter clause (postgres)

<img src="https://user-images.githubusercontent.com/28957748/141332375-5065297a-cc03-4797-aeb0-c501e3665f0a.png" height="100px"/>

```sql
select count(*)
from features
group by status
```

## 6. lesson-06-optimizing-circular-relationships
- circular rel while eager-loading

![image](https://user-images.githubusercontent.com/28957748/141334564-bdde36a8-d80d-467e-ab98-06b736c7c023.png)

- hmm, repeated stuff, don't call db, take it from memory

![image](https://user-images.githubusercontent.com/28957748/141334490-93b1ba1f-b065-4716-b4ac-c853fca06c98.png)

## 7. lesson-07-setting-up-multi-column-searching

<img src="https://user-images.githubusercontent.com/28957748/141336539-b38711d9-20c5-47cc-8312-71176c3bb9ad.png" height="150px" />

## 8. lesson-08-getting-like-to-use-an-index
- `name LIKE '%abc%'` this will prevent `index` on `name` from being used in `mysql` => do `LIKE 'abc%' instead`
- subqueries might block index as well, try to run it in isolation

## 9. lesson-09-faster-options-than-where-has
- whereIn is better than `whereHas` and `join`

<img src="https://user-images.githubusercontent.com/28957748/141340229-272b0b73-2a03-4c4b-9e45-040cc0557902.png" height="200px"/>

## 10. lesson-10-when-it-makes-sense-to-run-additional-queries
- using query buidler, sometimes it can't find correct index, use `EXPLAIN` to check yourself

<img src="https://user-images.githubusercontent.com/28957748/141341227-85b84abe-4754-43ef-918b-c81dc4cfd93a.png" height="200px"/>

## 11. lesson-11-using-unions-to-run-queries-independently
- fix this query

<img src="https://user-images.githubusercontent.com/28957748/141343201-6959ea1f-63bd-406c-9dac-3b2109c53f18.png" height="200px"/>

## 12. lesson-12-fuzzier-searching-using-regular-expressions
- reg_exp to normalize input and field value before searching (simulate full-text search)

<img src="https://user-images.githubusercontent.com/28957748/141491919-177aca99-8e5f-4225-9ff2-35e38e91f99b.png" height="300px"/>

- virutal col (my sql)

<img src="https://user-images.githubusercontent.com/28957748/141492496-00235427-49bf-4bc0-9e0f-22c8368a0131.png" height="200px"/>

## 13. lesson-13-running-authorization-policies-in-the-database
- customers can only be viewed by `sales_rep` or `owner`

<img src="https://user-images.githubusercontent.com/28957748/141495412-b9b51c50-1849-4c19-92d9-9f130dd89f19.png" height="200px"/>

- policy can break patination. why? policy is used in view layer after data has been fetched
- authorization should be delegated to db

<img src="https://user-images.githubusercontent.com/28957748/141500927-4b9a2c32-3beb-4c2e-92d5-aadf4f489f01.png" height="200px"/>

<img src="https://user-images.githubusercontent.com/28957748/141500980-72e9b991-5b68-4650-9d9a-f3d349a2a063.png" height="200px"/>

## 14. lesson-14-faster-ordering-using-compound-indexes
- do not use `limit` without `order by`
- `order by` with multiple cols => use `compound index`
- order of compound index matters

![image](https://user-images.githubusercontent.com/28957748/141502712-daa30466-06af-4c84-bc23-5359ad951601.png)

## 15. lesson-15-ordering-by-has-one-relationships
- one `user` has one `company`
- one-one rel through `company_id` on `users` table
- req: order `users` by `company_name`
- approaches:
  - join: fast
  - subqueries: slow
- there can be various ways of querying that get the same result.

## 16. lesson-16-ordering-by-belongs-to-relationships
- go for join approach

## 17. lesson-17-ordering-by-has-many-relationships
- one `user` has many `logins`
- sort `users` by their `last_login`
- approaches:
  - join: slower + overhead
  - subquery: better

## 18. lesson-18-ordering-by-belongs-to-many-relationships
- context: 
  - one `user` has many `checkouts`
  - one `book` has many `checkouts`
- it means: user can buy many books, one book can be bought by many users

![image](https://user-images.githubusercontent.com/28957748/141605838-327729be-1c6c-4b21-85c7-1f9489a39da0.png)

- req 1: sort books by last date checkout
  - join: `books` with `checkouts`, group by books.id, order by checkout_date, limit 1
  - subqueries: for each `book`, find the latest `checkout`, sort all books by `checkout_date`

- req 2: sort `books` by `username` of last `checkout`
  - join + subqueries: join `users` and `checkouts` sort by `checkout_date`, take 1, return `username`, sort `books` by that `username`

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
