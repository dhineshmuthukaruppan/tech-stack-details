
#### Copyrights (c) 2020, www.hapevent.com, All rights reserved
#### Author name: Dhinesh Muthukaruppan



# Hapevent tech stack 
## Front end
1. **React/Redux** 
1. **HTML5 <EJS>**
1. **CSS3 <SCSS>**
1. **Javascript**
1. **Socket IO** Realtime webapplication | Bidirectional communication | Chatting 

## Backend
1. **NodeJS** 
1. **ExpressJs** Web application framework for NodeJS
1. **MongoDB** Database
1. **Redis** as a Caching server
1. **Nginx** as a Reverse Proxy Server and Load balancer
1. **Kafka** message queue | Interprocess communication

## Testing Framework
1. **Jest** 

## CI/CD
1. **Docker** Containerization
1. **Kubernetes** Container Orchestration
1. **Git** Version Control
1. **Jenkins** Continuous Integration and continuous Delivery

## Hosting  
1. **AWS** Amazon Web Service
    1. **EC2** Elastic Cloud Compute | Resizable Compute Capacity 
    1. **S3** Simple Storage Service | Store media files
    1. **Route53** DNS service 
    1. **IAM** Identity Access Management | Manage privileges
    1. **CloudFront** Content Delivery Network | Delivers static contents
    1. **Workmail** Managed Email and calendar service
    1. **Simple Email Service** 
    1. **AWS Cost Explorer**

## Analytics, SEO, and Ads
1. **Google Analytics** Tracks and reports website traffic
1. **Google Webmaster** Allows webmaster to check indexing status
1. **Google Adsense** Third party advertisement 


## Mobile Application 
1. **React Native** Cross platform | Open source | Javascript | Slower than native | Faster Development Time | Easy Maintenance | Instagram 

## Payment Gateway
1. **Stripe** Supported Payment Method - Credit/Debit cards Payment | 2% for most cards issued in India, 3% for cards issued outside of India & 4.3% for International cards
1. **PayU** Supported Payment Method - Credit/Debit cards, Netbanking, UPI, wallets | Flat 1.99% per successful transaction | Payout in 3 days


# Architecture 
**Monolithic** later on upgrade to **Microservice Architecture**

## Single server 2 - 9 Concurrent Users

![2 - 9 Concurrent Users]  
(/assets/img/architecture/2-9concurrent_users.png)      



## Vertical Scaling 10 - 99 Concurrent Users
> Vertical Scale means upgrading a single server hardware with more resources such as higher/faster CPU, RAM, HDD and I/O. In AWS, Upgrade to a t2.medium or equivalent(2 CPU / 4GB RAM) will do the needful. An additional benefit of having multi CPU cores is that we can run n instances of NodeJS (n = no of cpu cores) and loadbalance it with Nginx. By having multiple instances of our application we could achieve zero-downtime deployement/updates. 

We can upgrade one server while the other keeps serving the requests. For example, take down server#1, while server#2 continues serving the request. Then bring up server#1 and take down server#2 to update it. In the end, no request will be dropped, and our application will be fully updated.

![10 - 99 Concurrent Users]
(/assets/img/architecture/10-99concurrent_users.png)     



## Horizontal Scaling 100 - 999 Concurrent users
> Bottleneck will be on the I/O. Databases will take longer to respond. We could keep upgrading to m4.xlarge or equivalent (4CPU / 16 GB RAM). There will be a point where vertical scaling will not be cost effective. So we need to do horizontal scaling. Vertical scaling has another issue: all our eggs are in one basket. If the server goes down we'll be screwed. On the other hand, horizontal scaling will give us redundancy and failover capabilities. 

![100 - 999 Concurrent Users]
(/assets/img/architecture/vertical_vs_horizontal_scaling.png)   

At this point, it's better to start scaling horizontally rather than vertically. The bottleneck is most likely on the database. So, we can:

1. Move the database to a database to a different server and scale it independently.
1. Add replica set if the database hits its limit and db caching if it makes sense.



## Multi servers 1000 -  10000 concurrent Users
We can improve our previous setup, as follow:

1. Add load balancer and app units.
1. Use multiple availability zones in a region (eg. us-east-1, us-west-1), which one are connected through low latency links.
1. split static files to different server/service for easier maintenance (eg. AWS S3 and Cloud Front CDN). Add CDN for static files for optimizing cross origin performance and lower the latency. we can store the static assets such as Javascript files, css files, image files, videos and so on. to Content delivery network.

![1000 - 9,999 Concurrent Users]
(/assets/img/architecture/1000-10000concurrent_users.png)     



## Microservices - 1 Million+ Concurrent Users
So far, we have been leveraging vertical and horizontal scaling. We have separated web apps from database instances, and deploy them to multiple regions. However, we have been a single code based that handles all the work in our application. we can break it down into smaller pieces and scale them as needed. Going from monolith to microservices. 

It's time to take down our web app monolith and break it down into multiple smaller and independent components (microservices/SOA) that we can scale independently. We don't have to do the break down all at once. We can have the monolith keep doing what it was doing and start writing small client apps performs some of the task that the main app used to do. Later, we can use the load balancer to redirect the traffic to the new small service instead of the main app. Eventually, we can remove the code from the monolith since the new microservice has fully replaced it. Repeat this process as many times as needed to create new microservices. It should looks something like this. 

![1 Million + Concurrent Users]
(/assets/img/architecture/million_plus_users.png)    

**Automate** as much as we can. we have db replicas and sharding, horizontal scaling, multiple regions and multi-AZ, autoscaling. 

**Highly Available, Multi-Region** At this point, to scale we just keep adding instances and spreading across availability zones and regions based on the source of the traffic. If you notice that a significan amount of traffic is coming from certain region it's the time to make our server available there. Regions doesn't provide low latesncy links between them. One way to work around this issue is sharding the database.

**AutoScaling** It would be a waste if we always allocate servers for peak capacity. User traffic has peaks and valleys. It's better to put in place an autoscaling option that allows the network to adjust to the traffic conditions. There are multiple strategies to autoscale such as CPU utilization, scale based on latency or based on network traffic.



#Monthly Expenses upto 100 concurrent users

1. since we are completely using open source for our software development, we don't have to spend anything on software purchases.
1. S3 - `$0.023` per GB / Month - very cheap
1. workmail - \$4 per mail account / month eg. abhigyan@hapevent.com
1. **t2.medium** EC2 instance - (2 cpu/ 4GiB memory) - `$0.0464` per hour - `$33` per month 
1. upto 10 concurrent users **t2.micro** EC2 instance is enough which costs around `$0.0116` per hour - `8$` per month 
1. 2% commision for payment providers on successful transaction.
1. `$0` for the first 62000 emails per month, and \$0.10 for every 1000 emails we send after that. Plus `$0.12` for each GB of attachments we send. 
1. transactional and promotional sms to mobile phone | @ 10-12 paisa / sms. 
1. Overall running cost for the first 6 months is atmost `$50` per month and after that based on number of users it will increase.   
















