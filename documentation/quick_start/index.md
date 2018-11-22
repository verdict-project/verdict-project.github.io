---
layout: post
title: Quickstart Guide
---

* TOC
{:toc}


We will install VerdictDB (using PyVerdict), create a connection, and issue a simple query through VerdictDB. In this Quickstart Guide, we will use an MySQL database for VerdictDB's backend database. See [How to Connect](https://docs.verdictdb.org/reference/connection/) for the examples of connecting to other databases.

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

Now, you can import PyVerdict as a regular module and check its version. Note that PyVerdict's version number has no relationship with the version of core Java library.

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

Create a [Maven](https://maven.apache.org/) project and
place the following dependency in the `<dependencies>` of your pom.xml.
```pom
<dependency>
    <groupId>org.verdictdb</groupId>
    <artifactId>verdictdb-core</artifactId>
    <version>0.5.4</version>
</dependency>
```

To use MySQL, add the following entry as well:
```pom
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.46</version>
</dependency>
```


## Insert Data

We will first generate small data to play with.

```java
// Suppose username is root and password is rootpassword.
Connection mysqlConn = DriverManager.getConnection("jdbc:mysql://localhost", "root", "rootpassword");
Statement stmt = mysqlConn.createStatement();
stmt.execute("create schema myschema");
stmt.execute("create table myschema.sales (" +
             "  product   varchar(100)," +
             "  price     double)");

// insert 1000 rows
List<String> productList = Arrays.asList("milk", "egg", "juice");
for (int i = 0; i < 1000; i++) {
  int randInt = ThreadLocalRandom.current().nextInt(0, 3);
  String product = productList.get(randInt);
  double price = (randInt+2) * 10 + ThreadLocalRandom.current().nextInt(0, 10);
  stmt.execute(String.format(
      "INSERT INTO myschema.sales (product, price) VALUES('%s', %.0f)",
      product, price));
}
```



## Test VerdictDB

Create a JDBC connection to VerdictDB.

```java
Connection verdict = DriverManager.getConnection("jdbc:verdict:mysql://localhost", "root", "rootpassword");
Statement vstmt = verdict.createStatement();
```

Create a special table called a "scramble", which is the replica of the original table with extra information VerdictDB uses for speeding up query processing.

```java
vstmt.execute("create scramble myschema.sales_scrambled from myschema.sales");
```

Run just a regular query to the original table.

```java
ResultSet rs = vstmt.executeQuery(
    "select product, avg(price) "+
    "from myschema.sales_scrambled " +
    "group by product " +
    "order by product");
```

Internally, VerdictDB rewrites the above query to use the scramble. It is equivalent to explicitly specifying the scramble in the from clause of the above query.


## Complete Example Java File


```java
import java.sql.*;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.concurrent.ThreadLocalRandom;

public class FirstVerdictDBExample {


  public static void main(String args[]) throws SQLException {
    // Suppose username is root and password is rootpassword.
    Connection mysqlConn = DriverManager.getConnection("jdbc:mysql://localhost", "root", "rootpassword");
    Statement stmt = mysqlConn.createStatement();
    stmt.execute("create schema myschema");
    stmt.execute("create table myschema.sales (" +
                 "  product   varchar(100)," +
                 "  price     double)");

    // insert 1000 rows
    List<String> productList = Arrays.asList("milk", "egg", "juice");
    for (int i = 0; i < 1000; i++) {
      int randInt = ThreadLocalRandom.current().nextInt(0, 3)
      String product = productList.get(randInt);
      double price = (randInt+2) * 10 + ThreadLocalRandom.current().nextInt(0, 10);
      stmt.execute(String.format(
          "INSERT INTO myschema.sales (product, price) VALUES('%s', %.0f)",
          product, price));
    }

    Connection verdict = DriverManager.getConnection("jdbc:verdict:mysql://localhost", "root", "rootpassword");
    Statement vstmt = verdict.createStatement();

    // Use CREATE SCRAMBLE syntax to create scrambled tables.
    vstmt.execute("create scramble myschema.sales_scrambled from myschema.sales");

    ResultSet rs = vstmt.executeQuery(
        "select product, avg(price) "+
        "from myschema.sales_scrambled " +
        "group by product " +
        "order by product");

    // Do something after getting the results.
  }
}
```
-->
