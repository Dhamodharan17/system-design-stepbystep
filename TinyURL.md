Tiny URL - URL shortening service
It will provide short aliases redirecting to long URLs.

**1. Why do we need URL shortening ?**
- creates short url for long urls
- when user hits short url, redirect to original url.
- saves lot of space when displayed,printed,messaged or tweeted.
- users less likely to mistype shorter urls.

**2. Requirement and Goals of the system**

**Functional Requirements**
- our service should generate **shorter and unique alias**
- redirect to original link
- user can pick custom links
- links will expire after standard default time span
- users should able to give expiry time.

**Non Functional Requirements**
- system should be highly avaiable , because if our service is down all the url direction will start failing.
- url redirecting should happen in minimal latency(fast)
- links shouldn't be guessable

**Extended Requirements**
- Analytics e.g. how may times a redirection happended?
- our service should available through rest apis/

**3. Capacity Estimation and Constraints**

- Our system is read heavy
- there will lot of redirection compared to new url shortening
**Assume 100:1 ratio b/w read and write operation**

  **Traffic Estimates**
  Assume 500Million new url shortening per month with 100:1 read(redirecting) write(shortrning) ratio

  100 * 500 M = 50Billion redirection per month for 100 : 1 ratio

  **Queries per second**

  500 million/30 days * 24 hours * 3600 seconds = 200URLs redirection/second.

  considering 100:1 read/write ration

  100*200Url per second = 20K per second for 100:1 ratio

  

  

  

  

