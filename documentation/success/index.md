---
layout: post
title: Success Stories
---

Since VerdictDB is an open-sourced project hosted on GitHub, we are unable to know exactly who or what companies have tried or  actively using.
However, we have interacted heavily with the following companies to help them deploy VerdictDB in their production environments.

<div style="margin-top: 20px;">
	<div class="row">
		<div class="col-md-3 col-sm-3 col-xs-12">
			<img src="{{ site.baseurl }}/image/walmart_logo.png" width="95%" />
		</div>
		<div class="col-md-8 col-sm-8 col-xs-12">
			Making data-driven decisions based on its sales transaction data
			is crucial for Walmart's successful business operations.
			Unfortunately, due to its scale of operations, the volume of data
			Walmart collects is tremendous and ever-growing. 
			Even with today's commercial distributed stores and compute engines,
			the latencies of analytical queries easily exceeded 10-20 mins, significantly
			hampering the productivity of its data analysts.
			To reduce these long query latencies and improve their productivity,
			the data analysts use VerdictDB on top of their existing data analytics
			engines without making any modifications.
			VerdictDB has successfully reduced the latencies of their large-scale analytical queries
			down to a few seconds.
			<a href="https://hasgeek.com/fifthelephant/2018/proposals/approximate-query-processing-GMyfSskx7GikdLWoch8LyH">
				Learn more here.
			</a>
		</div>
	</div>
</div>


<div style="margin-top: 20px;">
	<div class="row">
		<div class="col-md-3 col-sm-3 col-xs-12">
			<img src="{{ site.baseurl }}/image/locally_logo.png" width="85%" />
		</div>
		<div class="col-md-8 col-sm-8 col-xs-12">
			 <a href="https://locally.io/">LOCALLY</a>
			 is a leading company in location data intelligence and real-time consumer engagement.
			 LOCALLY was interested in building a web-based platform where its customers can
			 interact (or play) with its massive amount of location data capturing people's
			 mobile activities.
			 In building such a web platform, the essential part is achieving real-time responses
			 to the customers' interactions.
			 For example, if a customer wants to learn how many people are within a certain (arbitrary) area,
			 the web platform must be able to obtain the number within a few seconds. In this way, the customer
			 can be more engaged with LOCALLY's platform and use more of its premium services.
			 In offering these near real-time responses, today's distributed compute engines 
			 (e.g., Amazon Redshift, Facebook Presto) were expensive and still not fast enough.
			 To solve this problem, LOCALLY uses VerdictDB, which can compute
			 the answers to big queries only within a few seconds using a small cluster (thus, affordable).
			 <!-- Since VerdictDB is platform-agnostic, LOCALLY has the freedom to choose
			 any systems for the underlying database. -->
		</div>
	</div>
</div>

