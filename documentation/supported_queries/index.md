---
layout: post
---

{:.first}
# Supported queries

## Querying metadata

show databases;

use database-name;

show tables [in database-name];

describe [database-name].table-name;

show samples for database-name;


## Sample creation

create sample of [database-name].table-name;

create uniform sample of [database-name].table-name;

create stratified sample of [database-name].table-name on column-name;

create universe sample of [database-name].table-name on column-name;


## Select statement

### Aggregate functions

count(\*), sum(col-name), avg(col-name), count(distinct col-name)

### Mathematical functions

### Table sources

a single table, a joined table, a derived table

### Filtering predicates (i.e., the `where` clause)

comparisons using expressions

comparisons with the result of another select statement (i.e., subquery)

### Grouping
