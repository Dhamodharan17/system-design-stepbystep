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

