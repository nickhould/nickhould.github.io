---
layout: post
title: What I Wish I Knew About Data For Startups
---

For the last 4 years, I've been through quite a startup journey at PasswordBox. We went through multiple phases: launching our consumer product, raising a Series A, improving our product and optimizing, growing to millions of users and eventually getting acquired. 

Throughout the journey, I've been part of the data team where I've been doing all things data: analysis, presentations, engineering, training, etc. We started with a very rudimentary stack to track and understand our product usage. Over time, we improved our infrastructure and our tooling. We built dashboards, data pipelines, reports etc. We invested heavily on the data culture. We trained our colleagues to leverage our tools to make better decisions with data. 

While we are now working harder than ever to improve our data stack, I think we are at a stage where we can take a step back and reflect on our past. 

Today, I want to share some learnings I have made during the last four years.

### Empower People

Data science is a team sport. The biggest challenge in building a data-informed culture is not a technology one. You can have all the best tools in the world but if nobody uses them to take decision, you have failed. 

#### Invest in People
It will help you scale data analysis and insights extraction. You can create much more value in your data analysis by involving product managers, user experience designers, developers, marketers etc. These people have their unique world views and they can bring another angle to an analysis. You want people that are not experts in data to perform their own analysis and generate value from it.

No matter what size your team is, start empowering people with data now. This investment can take many shapes or forms. When you are just starting out, it might be as simple as making third-party data tools, such as Mixpanel, available to the team and training your colleagues to use them. 

Personally, I really enjoy doing weekly office hours where I sit down with a colleague to get a question answered. During this time, we can review the question, understand how we can get it answered and then work our way through it with our tools. This is a unique opportunity to understand their challenges, teach them what I know about data and help them use the tools at their disposal. 

Your mission is not only to democratize data access for all but also teach your colleagues about data science. You are a guide, not a gatekeeper.

#### Learn to Communicate

Communication is one of the most important part of data analysis. You can spend your days building sophisticated data models but if you can't explain your analysis to product managers or designers, it's worthless.

When presenting, don't hesitate to explain fundamentals. Present what the data tells and what it doesn't tell. Experiment with different mediums to share your learnings. Write an internal post. Create an internal newsletter. Do a weekly presentation to your team on your findings. Whatever you do, make sure presenting data is part of your routine and that you are getting better at it.

### Deliver Value 

Imagine if you could stop time, build all the tools and infrastructure that you need and then come back with those assets for the business. Wouldn't that be awesome? Surely. But, that's not real life. 

I found one of the most challenging aspect of working in a startup data team was to conjugate the infrastructure ground work and consistently delivering value to the business through data analysis. You don't want to disappear for 6 months building tooling but you also don't want to keep delivering analysis with rudimentary tools. Your time is a limited resource. Where should you invest it? 

#### Know Your Customer

When building any product, your goal is to solve a problem. With internal data products, it's no different. Your core customer is the business. 

How can you help the business with data? What are the main problems to solve? The business needs will help you prioritize your engineering and analysis work. There is nothing worst than a data team going off-the-grid for months to build an internal tool that doesn't solve any core problems. 

#### Ship often

One way to make sure you are aligned with the business needs is to ship often by small increments. It will help you validate that what you are delivering brings value to the team and that you are on the right track.

In most startups, the data team is a support team. You are there to make your colleagues life easier. At the end of each work day, ask yourself if you helped people? 

#### Time is Money

If you find yourself manually creating the same report more than once, you should automate it. When you are doing data analysis and research, make sure it is easily reproducible. Jupyter Notebooks are great for this. In the future, you will be very happy you can update your analysis in a few minutes. Sure, it's an investment upfront but it will pay off. You will save time on the long run and you will be much more confident on your reporting.

Also, when doing any engineering work, always consider existing products as  an alternative. Build vs Buy. I've written about why [you shouldn't build a dashboard from scratch](http://www.jeannicholashould.com/dont-build-another-dashboard-from-scratch.html) and why you should consider existing solutions. This philosophy is not limited to analytics dashboards. It can be applicable to your whole data stack: tracking, pipelines, reporting etc. It is generally always cheaper to buy an existing solution than building and maintaining a custom one indefinitely.

#### Stick With Boring Technology

As Martin Weiner, Reddit's CTO, [puts it](http://thenewstack.io/reddit-cto-sxsw-stick-boring-tech-building-start/), stick with boring technology when building your startup.  Mature technology will be more stable and offer a wider pool of talent. Unless you really need it, stick with technologies that have a proven track-record.

Don't try to build the ideal infrastructure from the ground up. Iterate. Asana's initial data warehouse was [MySQL](https://blog.asana.com/2014/11/stable-accessible-data-infrastructure-startup/). Over time, they transitioned to Redshift and a state of the art data infrastructure. They iteratively built and improved their data infrastructure as they scaled.

### About Data

#### Document Tracking

I'm surprised by how little literature has been written about documentation for tracking events. I know, this is a boring topic. But, this is so critical to the success of a data team, both on the engineering and the culture side.

If you are building a consumer application, you will most likely have to support different platforms: iOS, Android, web, etc. Each one of those platforms will have a tracking implementation. How will you make sure the naming conventions are respected throughout those implementations? Where do you document this?

Furthermore, if some people that are not familiar with your data implementation want to dig in the tools, where can they find a definition for each one of your tracking events? What properties should be tracked with each of those events? What are their data types?

Build a central repository where you document all of the tracking events and their properties. The simplest solution to get started is to document this in an Excel worksheet. At some point, I recommend that this documentation should be in a machine-readable format so it can be re-used for testing, schema generation, etc.

#### Quality over Quantity

>“Every single company I've worked at and talked to has the same problem without a single exception so far — poor data quality, especially tracking data. Either there's incomplete data, missing tracking data, duplicative tracking data.” - [DJ Patil](http://firstround.com/review/everything-we-wish-wed-known-about-building-data-products/), _U.S. Chief Data Scientist at White House Office of Science and Technology_

It is often said that 80% of data analysis is spent on the process of cleaning and preparing the data. This is true. Data quality is a challenging aspect of working with data. Bad data quality can take many forms at different stage during the data life cycle. Here are a few examples: 

- Tracking events triggered with invalid name, properties or property value
- Tracking events triggered at the wrong time
- Database columns with inconsistent values
- Incorrect transformation of the data in an ETL process
- Invalid calculation in reporting

Implement a data quality process early on. At every stage of the data life cycle, you want to be testing: tracking, ETL, reporting. This is costly. You will need to invest time and efforts in making sure you are not collecting garbage data or generating erroneous reports.

I strongly recommend you automate the testing process of your data. You should be monitoring your data quality just like you are monitoring the KPI's in your product. If you can't automate it, make sure it gets tested manually to some degree. 

Focus on collecting less data but better data. Some might say that since the cost of storage is so low, you should try to track everything in your product. I disagree. Even if the cost of data storage is cheap, the overhead of dealing with volumes of garbage data isn't worth it. Prioritize quality over quantity.

If you think you don't have data quality problems, you are probably screwed.

#### Control & Ownership

Make sure you are able to easily start and stop sending your tracking events to any sources when you wish to, without requiring any changes in your product. This will allow you to independently activate new tracking tools without having to change anything in the tracking implementation.

Also, make sure you have access to all of your raw tracking data. At first, you might not have to resources to crunch all of this data easily. As you grow, you will develop those tools and knowledge. Being able to crunch the raw data will enable you to answer more complex analysis. 

If you don't want to build a custom tool for this, I recommend [Segment](https://segment.com/).

### Closing Thoughts

Over the last 4 years, I've made many learnings through experiences. From choosing technology to investing in data quality, those learnings have been made with trial and error. 

Many things have changed during this period. Technology has solved many of the storage and computing challenges we once had with "big" data. A startup can now, in a matter of hours, have a managed data warehouse up and running, plugged into powerful analytical tools. This wasn't the case when we started out. For most startups, the challenge is now less a technology one but rather a human one.
