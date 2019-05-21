---
layout: post
title: Quickstart Guide
---

* TOC
{:toc}

We will install VerdictDB's Python interface, ingest data (i.e., create another *special* copy of your existing table), and issue a simple query to VerdictDB. In this Quickstart Guide, we will use MySQL for VerdictDB's backend database.

For more details, please see [this page (for Java)](https://docs.verdictdb.org/getting_started/quickstart/)
or [this page (for Python)](https://docs.verdictdb.org/getting_started/quickstart_python/).


## Install Prerequisites

1. **Miniconda for Python 3.7**: You can locally install miniconda by following the instructions [on this page](https://conda.io/docs/user-guide/install/index.html).
1. **Test Data (TPC-H 1G)**: Please follow the instructions [on this page](https://docs.verdictdb.org/tutorial/tpch/).


## Install and Import

Install PyVerdict as follows. If you are using locally-installed miniconda, PyVerdict will also be locally installed.

```bash
pip install pyverdict --upgrade
```

Now, you can import PyVerdict as a regular module and check its version. Note that PyVerdict's version number has no relationship with the version of core VerdictDB library.

```python
import pyverdict
pyverdict.__version__              # prints pyverdict version
pyverdict.__verdictdb_version__    # prints the core VerdictDB version

verdict = pyverdict.mysql('localhost', 'root', 'password')    # make a connection
verdict.sql('show schemas')        # retrieves schemas
```


## Run Queries

This examples assumes that the `lineitem` table (in the test data) has been created under the `tpch1g` schema.

```python
# VerdictDB relies on a special table called scramble to speed up its queries.
verdict.sql('create scramble tpch1g.lineitem_x from tpch1g.lineitem')

# Automatically use tpch1g.lineitem_x to speed up
# 1 row(s) in the result (3.915 seconds)
#           c2
# 0  4291411.0
verdict.sql("select count(*) from tpch1g.lineitem where l_shipdate > '1994-01-02'")
```

To run the same query without any speedup, prepend `bypass` in front of the query.
```python
# 1 row(s) in the result (57.561 seconds)
#    count(*)
# 0   4331220
verdict.sql("bypass select count(*) from tpch1g.lineitem where l_shipdate > '1994-01-02'")
```

VerdictDB (and pyverdict) also supports other aggregates, e.g., sum, avg, count(distinct col).

**Note**: When the same query is issued multiple times, some systems return answers immediately using cached results.
If you want more accurate comparisons, simply change the query predicates slightly, e.g., '1994-01-03' instead of '1994-01-02'.


<!---
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
-->
