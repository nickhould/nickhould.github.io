---
layout: post
title: Don't Build Another Dashboard From Scratch
---

For the past few years, I've been working as part of the data team of [True Key](https://www.truekey.com/) at Intel Security. At different point in times, I felt the need to build a custom dashboarding solution for our team. I thought the existing solutions where not good enough for us, not flexible enough. For example, solutions like Geckoboard didn't allow our teammates to filter and query the data. I wanted to build a custom dashboard by coding it from scratch to solve that problem. 

I've noticed it's a common pattern in data teams to want to build a custom dashboard. We might want a self-hosted dashboarding solution, create very custom viz, allow special querying functionalities, etc. 

However, I came to realize that you shouldn't build your own internal dashboarding solution. Very rarely, those custom needs will justify such endeavours. Here's why.


### Cost > Value
The cost of developing and maintaining that dashboard will rarely justify it's return on investment. In business, one of the scarcest resource is time, especially for early-stage businesses. If you invest your time in building the dashboarding tool, you’re not investing in other, more valuable projects. During this time, you are not doing analysis on the actual data, coaching your colleagues to be more data-driven and building the infrastructure you’ll need to answer more complex questions. Building a custom dashboarding solution is a major time investment. 

As [Michael Erasmus](https://twitter.com/michael_erasmus?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor), Data Analytics lead at Buffer, [puts it](https://looker.com/blog/buffers-new-data-architecture-how-redshift-hadoop-and-looker-help-us-analyze-500-million-records-in-seconds):

> Building something custom is flexible. But it sucks up a lot of developer time to design new ETL pipelines and surface them in the dashboard UIs.
> 
> We were fiddling with JavaScript charts and nightly jobs that needed a lot of maintenance and support. We realized we would much rather just get quick insights into our data.

How bad do you need that custom viz? Can the business do great without it? What justify such an investment in a custom solution? 

### Solution is out there

A solution for your problem most likely exists. The enterprise analytics space is very crowded and there is most likely a great solution for you out there, no matter what’s your data analytics maturity. 

If you don’t have a centralized data warehouse, you can use a tool such as Klipfolio. This dashboarding tool already have built-in connectors in many third-party tracking systems such as Mixpanel, Google Analytics, etc. It can pull from these data sources with minimal requirements. 

If you already have your data warehouse, Looker is an awesome solution. It allows you to build models around your data and expose very powerful dashboards, visualizations and querying interfaces to your end-users. Businesses such as [Buffer](https://looker.com/blog/buffers-new-data-architecture-how-redshift-hadoop-and-looker-help-us-analyze-500-million-records-in-seconds), [Asana](https://blog.asana.com/2014/11/stable-accessible-data-infrastructure-startup/) and [Hubspot](https://info.looker.com/h/i/118957871-driving-behavior-with-data-at-hubspot) are happy users of Looker.

Long story short, if you think of building your own internal dashboard solution, you better have a damn good reason for it - you are most likely doing a bad decision. Consider existing solutions, such as Looker, that will fulfill most of your needs and allow you to focus on extracting as much insight as possible from your data.