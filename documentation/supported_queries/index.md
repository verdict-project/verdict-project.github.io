---
layout: post
---

{:.first}
# Supported queries

* TOC
{:toc}

## Querying database metadata

{:.supported-query}
show databases;

Displays database names. When Verdict runs on Spark, it displays the tables accessible through `HiveContext`.

{:.supported-query}
use database-name;

Set the default database to `database-name`.

{:.supported-query}
show tables [in database-name];

Displays the tables in `database-name`. If the database name is not specified, the tables in the default database are listed. If `database-name` is not specified and the default database is not chosen, Verdict throws an error.

{:.supported-query}
describe [database-name].table-name;

Displays the column names and their types.

{:.supported-query}
show samples [for database-name];

Displays the samples created for the tables in the `database-name`. If `database-name` is not specified and the database name is not specified, the samples created for the default database are specified. If the default database is not chosen, Verdict throws an error.


## Creating samples

{:.supported-query}
create [XX%] sample of [database-name.]table-name;

Creates a set of samples for the specified table. **This is the recommended way of creating sample tables.** Verdict analyzes the statistics of the table and automatically creates desired samples. If the sampling probability is omitted, 1% samples are created by default.

Currently, Verdict creates three types of samples using the following rule:
1. A uniform random sample
1. Stratified samples for low-cardinality columns (distinct-count of a column <= 1% of the total number of tuples).
1. Universe samples for high-cardinality columns (distinct-count of a column > 1% of the total number of tuples).

Find more details on each sample type below.

{:.supported-query}
create [XX%] uniform sample of [database-name.]table-name;

Creates XX% (1% by default) uniform random sample of the specified table.

**Uniform random samples** are useful for estimating some basic statistics, such as the number of tuples in a table, average of some expressions, etc., especially when there are no `group-by` clause.


{:.supported-query}
create [XX%] stratified sample of [database-name.]table-name on column-name;

Creates XX% (1% by default) stratified sample of the specified table.

**Stratified samples** ensures that no attribute values are dropped for the column(s) on which the stratified samples are built on. This implies that, in the result set of `select group-name, count(*) from t group by group-name`, every `group-name` exists even if Verdict runs the query on a sample. Stratified samples can be very useful when users have some knowledge on what columns will appear frequently in the `group-by` clause or in the `where` clause. For example, stratified samples can be very useful for streaming data where each record contains a timestamp. Building a stratified sample on the timestamp will ensure rare events are never dropped in the sampling process. Note that the sampling probabilities for tuples are not uniform in stratified sampling. However, Verdict automatically considers them and produce correct answers.


{:.supported-query}
create [XX%] universe sample of [database-name.]table-name on column-name;

Creates XX% (1% by default) universe sample of the specified table.

**Universe samples** are hashing-based sampling. Verdict uses hash functions available in a database system it works with. Universe samples are used for estimating *distinct-count* of high-cardinality columns. The theory behind using universe samples for *distinct-count* is closely related to the [HyperLogLog algorithm](https://en.wikipedia.org/wiki/HyperLogLog). Different from HyperLogLog, however, Verdict's approach uses a sample; thus, it is significantly faster than HyperLogLog or any similar approaches that scan the entire data. Also, universe samples are useful when a query includes joins. The equi-joins of two universe samples (of different tables) built on the join key preserves the cardinality very well; thus, it can produce accurate answers compared to joining two uniform or stratified samples.


## Analyzing data: overall

Verdict processes the standard SQL query in the following form.

<!-- {:.supported-query} -->
```sql
select expr1, expr2, ...
from table_source1, table_source2, ...
[where conditions]
[group by expr1, ...]
[order by expr1]
[limit n;]
```

Find more details on the supported expressions below.


## **Analyzing data: aggregate functions**

### ***Supported*** aggregate functions

Verdict brings speedups for the following aggregate functions or combinations of them:

| Aggregate function       | Description                       |
|--------------------------|-----------------------------------|
| count(*)                 |                                   |
| sum(col-name)            |                                   |
| avg(col-name)            |                                   |
| count(distinct col-name) | Only one column can be specified. |


### ***Future supported*** aggregate functions

Verdict will be extended to support the following aggregate functions in the future:

| Aggregate function       | Description                       |
|--------------------------|-----------------------------------|
| var_pop(col-name)        | population variance               |
| var_samp(col-name)       | sample variance                   |
| stddev_pop(col-name)     | population standard deviation     |
| stddev_samp(col-name)    | sample standard deviation         |
| covar_pop(col1, col2)    | population covariance             |
| covar_samp(col1, col2)   | sample covariance                 |
| corr(col1, col2)         | Pearson correlation coefficient   |
| percentile(col1, p)      | p should be within 0.01 and 0.99 for reliable results |


### ***Unsupported*** aggregate functions

Verdict does not support (even in the future) the following extremem statistics.

| Aggregate function       | Description                       |
|--------------------------|-----------------------------------|
| min(col-name)            |                                   |
| max(col-name)            |                                   |

If a query includes these unsupported aggregate function(s), Verdict uses the original tables (instead of the sample tables) for processing those queries.


## Analyzing data: other expressions

We are continuously extending these expressions. In general, every (non-aggregate) function that is provided by existing database systems can be processed by Verdict (since Verdict will simply pass those functions to those databases). Please inform us if you want certain functions to be included. We will quickly add them.

### Mathematical functions

| Function                 | Description                       |
|--------------------------|-----------------------------------|
| round(double a           |                                   |
| floor(double a)          |                                   |
| ceil(double a)           |                                   |
| exp(double a)            |                                   |
| ln(double a)             | a natural logarithm               |
| log10(double a)          | log with base 10                  |
| log2(double a)           | log with base 2                   |
| sin(double a)            |                                   |
| cos(double a)            |                                   |
| tan(double a)            |                                   |
| sign(double a)           | Returns the sign of a as '1.0' (if a is positive) or '-1.0' (if a is negative), '0.0' otherwise |
| pmod(int a, int b)       | a mod b; support for Hive and Spark; See [this page](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) for more information. |
| a % b                    | a mod b                           |
| rand(int seed)           | random number between 0 and 1     |
| abs(double a), abs(int a) | an absolute value                |
| sqrt(double a)           |                                   |


### String operators

| Function                 | Description                       |
|--------------------------|-----------------------------------|
| conv(int num, int from_base, int to_base),<br> conv(string num, int from_base, int to_base) | Converts a number from a given base to another; support for Hive and Spark |
| substr(string a, int start, int len) | Returns the portion of the string starting at a specified point with a specified maximum length. |


### Other functions

| Function                 | Description                       |
|--------------------------|-----------------------------------|
| fnv_hash(expr)           | Returns a consistent 64-bit value derived from the input argument; support for Impala; See [this page](https://www.cloudera.com/documentation/enterprise/5-8-x/topics/impala_math_functions.html) for more information. |
| md5(expr)                | Calculates an MD5 128-bit checksum for the string or binary; support for Hive and Spark   |
| crc32(expr)              | Computes a cyclic redundancy check value for string or binary argument and returns bigint value; support for Hive and Spark |


## Analyzing data: table sources, filtering predicates, etc.

### Table sources

You can use a single base table, equi-joined tables, or derived tables in the from clause. Verdict's sample planner automatically finds the best set of sample tables to process your queries. However, if samples must not be used for processing your queries (due to unguaranteed accuracy), Verdict will use the original tables.

Verdict's sample planner is rather involved, so we will make a separate document for its description.

**Note**: Verdict's query parser currently processes only inner joins, but it will be extended to process left outer and right outer joins.


### Filtering predicates (i.e., in the where clause)

| Predicate                | Description                       |
|--------------------------|-----------------------------------|
| p1 AND p2                | AND of two predicates, p1 and p2  |
| p1 OR p2                 | OR of two predicates, p1 and p2   |
| expr1 COMP expr2         | comparison of two expressions, expr1 and expr2, using the comparison operator, COMP. The available comparison operators are =, >, <, <=, >=, <>, !=, !>, !<, <=, >=, <, >, !>, !<  |
| expr COMP subquery       | comparison of the value of expr and the value of subquery. The subquery must return a single row and a single column |
| expr1 NOT? BETWEEN expr2 AND expr3 | returns true if the value of expr1 is between the value of expr2 and the value of expr3. |
| expr1 NOT? LIKE expr2    | text pattern search using wild cards. See [this page](https://www.w3schools.com/sql/sql_like.asp) for more information. |
| expr IS NOT? NULL        | test if the value of the expression is null. |



## Dropping samples

{:.supported-query}
(delete \| drop) [XX%] sample of [database-name.]table-name;

Drop all the samples created for the specified table. The sampling ratio is 1% is not specified explicitly.


{:.supported-query}
(delete \| drop) [XX%] uniform sample of [database-name.]table-name;

Drop the uniform random sample created for the specified table. The sampling ratio is 1% is not specified explicitly.


{:.supported-query}
(delete \| drop) [XX%] stratified sample of [database-name.]table-name on column-name;

Drop the stratified sample created for the specified table. The sampling ratio is 1% is not specified explicitly.


{:.supported-query}
(delete \| drop) [XX%] universe sample of [database-name.]table-name on column-name;

Drop the universe sample created for the specified table. The sampling ratio is 1% is not specified explicitly.
