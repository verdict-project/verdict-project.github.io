---
layout: post
---

{:.first}
# Supported queries

* TOC
{:toc}

## Querying metadata

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

Creates a set of samples for the specified table. This is the recommended way of creating sample tables. Verdict analyzes the statistics of the table and automatically creates desired samples. If the sampling probability is omitted, 1% samples are created by default.

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


## Analyzing data (select statements)

### Aggregate functions

count(\*), sum(col-name), avg(col-name), count(distinct col-name)

### Mathematical functions

### Table sources

a single table, a joined table, a derived table

### Filtering predicates (i.e., the `where` clause)

comparisons using expressions

comparisons with the result of another select statement (i.e., subquery)

### Grouping


## Deleting data
