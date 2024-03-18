## System Design Interviews - Step by Step
Shows the candidate abiltity to handle complex system.

**Step 1 : Requirement Clarifications**

- Ask questions about exact scope/features of the system given.
- Questions are open ended and don't have on correct answer.
- Ask question about which part of the system should be build since we cannot build complete system in interview.

  **Example for requirement clarifications**

  E.g. Twitter


  ![image](https://github.com/Dhamodharan17/system-design-stepbystep/assets/30789057/5ab6e6e7-9096-487d-81e1-3b5451f6b0cb)

Q1. will users able to post tweets and follow other people? <br />
Q2. should we design timeline?<br />
Q3. will tweets contain photo and video?<br />
Q4. are we focusing on backend or frontend?<br />
Q5. will users able to search tweets?<br />
Q6. do we need to display hot trending topics?<br />
Q7.will there be any push notification for new/important tweets?<br />

**Step 2 : System interface definition**

Define what APIs are expected from the system.

**postTweets(user_id,tweet_data,tweet_location,user_location,timestamp..)<br />**

**generateTimeline(user_id,current_time,user_location..)<br />**

**makeTweetFavorite(user_id,tweet_id,timestamp..)<br />**

**Step 3 : Back of the envelope estimation**

-  Estimate the scale of the system we are going to design
- **It will help later when we focus on scaling, partitioning, load balancing and caching.**

  a. what scale is expected from the system <br/>
  b.number of new tweets<br/>
  c.number of tweet views<br/>
  d.number of timeline generation/second <br/>
  e.how much storage will we need ? how much with photo/video <br/>
  f.n/w bandwidth usage we exprecting - important in deciding how we will manage traffic/balance b/w servers <br>


 **Step 4 : Defining data model**

 - Defining data model will clarify how ddata will flow among different components of system later.
 - later it will guide towards data partitioning and management.
 - we should **identify various entities of system how they will interact with each other** and different aspect of data management like storage, transporation,encryption etc.

Some entities for twitter service

**User** UserId,Email,Dob,creationdata,lastlogin etc.
**Tweet** Tweetid, content,tweetlocation,numberoflikes,timestamp etc.
**UserFollow** UserID1,UserID2
**Favorite Tweets** Used Id, Tweetid, time stamp

**Step 5 : High Level Design**

- draw 5-6 boxes representing the core components of our system.
- **find the components that will solve our problem**

For twitter .

- At high level, we need multiple application server and a **load balancer** infront to handle read/write request.
- if we think, more read operation we can have **seperate server** for those scenrios.
- we need effective db that can store all tweets and can s**upport hige number of reads**.
- **distributed file storage** system for storing photo and videos.

  ![image](https://github.com/Dhamodharan17/system-design-stepbystep/assets/30789057/d68ff64b-79a5-4a56-abb1-218802ebedfb)

  **Step 6 : Detailed design**

  - dig deeper into 2 or 3 components
  - explain different apporachs and pros & cons of both i.e trade off

  E.g.

  - since we store massive data, how should we partition to distribute it to multiple databases ?
  - should we store all user data on the same database
  - how to **hot users who tweet a lot and follow lots of people**
  - users timeline , how should we store data such that latest tweets will be come quicker.
  - how to use/layer to cache to speed up things?
  - what component needs better load balancing?
 
  **Step 7 :  Identifying and resolving bottle necks**

  - **discuss as many bottle necks as possible and different approaches to mitigate them**
  - Is there any single point of failure
  - do we have enough replicas of data so it will help in server failure ?
  - do we enough copies of different server to help in server failure?
  - how we are monitoring the performance of our service?
  - do we get alerts whenever critical components fails ot performance degrade ?
 
## Summary
- ask scope and what features of system should be designed
- Define APIs
- estimate the numbers like number of users/tweets/time to generate
- data model(jpa)
- high level design (5-6 boxes)
- detailed design, dig deeper into 2-3 components
- identify and as many as resolve bottle necks


