---
layout: post
title: Which Tool To Use For Your Data Pipelines?
---

[comment]: <> (Problem)

If you are working at a startup and you are the data guy/girl, you're most likely doing some form of ETL's / data pipelines. Perhaps you are pulling data from a database, aggregating it and storing that as CSV's? 

As the data needs of your team grow, you will be managing more and more of those jobs. If you don't have a proper solution to manage those, soon you will be drowning under a mountain of those ETL's that can fail at any point without you knowing.

Nowadays, there are many solutions to automate and manage data pipelines, ranging from full-blown enterprise solutions to hardly maintained open source projects. With all of those choices, you are probably asking yourself **which tool should I use for my data pipelines?** 

I've recently gone through the selection and setup process of such a tool with my team. Here's my take on this.
    
[comment]: <> (Solution)

### Solution #1: The Best Code is No Code At All

I am a believer in the saying "the best code is no code at all".  Code requires maintenance. Maintenance requires time. Time is money. I try to stay away from building custom solutions when it's possible. It's easy to sink a whole lot of time in creating and maintaining those and it's not the best use of your time.

I came across two potential solution I would definitely consider seriously if I was setting up my data pipelines from scratch today, especially at an early stage startup:

-  [RJ Metrics - Pipeline](https://rjmetrics.com/product/pipeline)
-  [Segment - Sources](https://segment.com/sources)

Both of these solutions are SaaS products that pull your data from different third-party providers and store it in your data warehouse. RJ Metrics currently only supports Amazon Redshift as data warehouse and Segment Sources supports both Redshift and Postgres. 

#### Pros
If you are heavily using third-party products such as Mixpanel, Stripe, Intercom or Zendesk to collect data, those solutions can be very interesting. They will save you the hassle of coding and maintaining data pipelines to extract and load this data. There is very little you have to do to get up and running. 


#### Cons
Those solutions currently have a limited number integrations. There's a good chance some of your data sources aren't currently supported. Also, those solutions are focused on extracting and loading up your data in a warehouse. If you want to any transformations on your data prior to loading, you are out of luck.

### Solution #2: Airflow

Airflow is a workflow management platform that has been open sourced last year by AirBnB. It's coded in Python, it's actively worked on and it has become a serious option to manage your batch tasks. It is basically an orchestrator for your ETL's. It not only schedules and executes your tasks but it also helps manages the sequence and dependencies of those tasks through workflows.
 
#### Pros
The basic setup of Airflow is fairly easy to complete if you have some technical chops. If you have some experience with Python, writing your first jobs in Airflow shouldn't be a problem.

Once you are setup, the web interface is user-friendly and can provide the status on each tasks that has ran. You can trigger new tasks to run from the interface and visually see all the dependencies between the pipelines.

Airflow is a very complete solution. Out of the box, it can connect to a wide variety of databases. It can alert you by email if a task fail, write a message on Slack when a task has finished running etc. You might not need everything it has to offer out of the box. 

Also, Airflow is designed to scale. The workers can be distributed across different nodes. Generally, there shouldn't much hardcore computing done by the workers but being able to scale the workers will provide some head room for your tasks to run smoothly. The setup if you want to scale workers is more advanced.

#### Cons
If you don't have an engineer in your team or someone who's technical, this type of solution is not a good choice for you. While the setup is easy, you need to have someone who can manage and maintain your Airflow instance. Also, if you are running on Windows environment, you might run into troubles setting up Airflow. 

#### Getting started with Airflow
If you already have some data pipelines, I recommend you pick the simplest one and get it running through Airflow. You can install Airflow [in a few commands](http://pythonhosted.org/airflow/start.html). I strongly encourage you to go through [the whole tutorial](http://pythonhosted.org/airflow/tutorial.html), it will enable you to grasp the core concepts with ease.


[comment]: <> (Conclusion)

### What Else?

Choosing a data pipeline solution is an important choice because you'll most likely live with it for a while. This post is in no way an exhaustive list of tools for managing ETL's. 

No matter what tool you choose, one thing to remember is that you want to choose based on your own resources and requirements. Don't set yourself for failure by choosing a solution you can't maintain and that will create more workload on yourself. 
