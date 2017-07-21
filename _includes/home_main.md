
<div class="row">
  <div class="col-md-1 col-sm-0"></div>
  <div class="col-md-5 col-sm-6 col-xs-12 text-center">
  <img src="{{ site.baseurl }}/image/index-html-impala.png" class="index-image" />

  <p class="text-center">Answer by <span style="font-weight: bold">Impala</span> in 350 seconds</p>
  </div>
  <div class="col-md-5 col-sm-6 col-xs-12 text-center">
  <img src="{{ site.baseurl }}/image/index-html-verdict.png" class="index-image" />

  <p class="text-center">Answer by <span style="font-weight: bold">Verdict</span> in 2 seconds</p>
  </div>
  <div class="col-md-1 col-sm-0"></div>
</div>

# 200x faster by sacrificing only 1% accuracy

Verdict can give you 99% accurate answers for your big data queries in a fraction of the time needed for calculating exact answers. If your data is too big to analyze in a couple of seconds, you will like Verdict.

# No changes to your database

Verdict is a middleware standing between your application and your database. You can just issue the same queries as before and get approximate answers right away. Of course, Verdict handles exact query processing too.

# Runs on all SQL-based engines

Verdict can run on any database that supports standard SQL (both traditional and modern). We already have drivers for Hive, Impala, and MySQL. We’re in the process of adding drivers for many other databases too (Amazon Redshift, Spark SQL, HP Vertica, Oracle, Teradata, MS SQL Server, ...). If you are using a DB that’s not in this list, please shoot us an email and we will happily add a driver for your database too.

# Ease of use

Using Verdict requires almost zero setup: no servers, no port configurations, no extra user authentication, etc. Verdict simply issues rewritten SQL queries to your databases on behalf of users. The use of Verdict does not introduce any security breaches. Verdict relies on standard protocols for communicating with the databases, which can be configured to be secure.
