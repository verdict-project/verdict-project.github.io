---
layout: post
title: Quickstart Guide
---

* TOC
{:toc}

We will install VerdictDB's Python interface, ingest data (i.e., create another *special* copy of your existing table), and issue a simple query to VerdictDB. In this Quickstart Guide, we will use MySQL for VerdictDB's backend database.

<!-- We will install VerdictDB, create a connection, and issue a simple query to VerdictDB. In this Quickstart Guide, we will use an MySQL database for VerdictDB's backend database. See [How to Connect](http://docs.verdictdb.org/getting_started/connection/) for the examples of connecting to other databases. -->


## Install

For this quickstart guide, we will use `pyverdict`, a Python interface to VerdictDB's core.
`pyverdict` can be installed by typing

```python
pip install pyverdict
# use the following line for upgrading:
# pip install pyverdict --upgrade
```



## Insert Data

Suppose a table `myschema.sales` contains the data you want to analyze.
After making a connection to VerdictDB, we will issue a query to create a *scramble* of the table,
i.e., its replica with special extra information.
VerdictDB uses this scramble for speeding up query processing.

```python
verdict = pyverdict.mysql(
    host='localhost',
    user='root',
    password='',
    port=3306
)
verdict.sql('create scramble myschema.sales_scrambled from myschema.sales')
```



## Run Queries

Run a regular query to the scrambled table to obtain approximate results. The query result is stored in a pandas DataFrame.

```python
df = verdict.sql(
    "select product, avg(price) " +
    "from myschema.sales_scrambled " +
    "group by product " +
    "order by product")
```
