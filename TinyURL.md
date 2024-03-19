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

  **Storage Estimation**

  Lets assume we store every url shortening request for 5 years

  500M request/month

  for 5 years,
  500M * 5 years * 12 months = 30Billion

  30Billion * 5 years * 12 Months = 15TB
  **Bandwidth Estimation**

  we exprect 200 new URLs every second, total incoming data for our service will be 100KB/second

  let each request = 500 bytes
  
  200 * 500 bytes = 100 kb/s;
  
**Memory Estimation**

we can cache some hot urls that are frequenlty used. if we follow 80-20 rule<br/>
80 % traffic created by 20% urls, so we have to cache the 20%.

in 100:1 ratio, we will get 20K urls/second and 1.7 billion/data

20k * 3600 seconds * 24 hours = 1.7 billion request

to cache 20% (0.2) request we need 170gb

0.2 * 1.7b * 500 bytes = 170 gb;

actual mempty usage will less than 170 gb since there will be lot of duplicate request.

**4.System APIs**

**String createURL(api_key,original_url,custom_alias_name=None,user_name=None,expire_date=None)**

  retuen the shortened url otherwise return error code

  **String deleteURL(api_key,url_key)**

  return message "url removed"

  **detect and prevent abuse**

  malicious user can put us out of buiness by consuming all url keys

  to prevent, limit each user to limited url creation and redirection per some time period with different duration/developer key.

  **5.Database Design**

  **Observations about nature of data**
  - need to store billion of records each is less than 1kb
  - no relation b/w records other than user created url
  - service read heavy
2 tables, one for storing url details and other for storing user details

![image](https://github.com/Dhamodharan17/system-design-stepbystep/assets/30789057/4aa2ddf7-d2b4-47e5-b510-bf7f2a231a7c)

we don't need to use relationship b/w objects - **No SQL key-value** dynamodb,cassandra,riak

**6. Basic System Design and Algorthim**

Problem : how to generate a short and unique key for given url

http://tinyurl.com/jlg8zpc

last 6 character is they short key, we want to generate.

**2 possible solutions**

**1.Encode actual URL**

1.Encoding - converting string to set of bytes

![image](https://github.com/Dhamodharan17/system-design-stepbystep/assets/30789057/9dc82824-a15e-4f87-9296-32142356de62)

2. Hashing the encoded string

   ![image](https://github.com/Dhamodharan17/system-design-stepbystep/assets/30789057/937dfc1d-af06-4abc-9bf2-6261c4fc8858)

- we can compute unique hash of the given url <br/>hash can be encoded for displaying further.
- encoding could be base36(a-z,0-9) or base62 (A-Z,a-z,0-9) or base 64 includes '-' and ','
- **resoanable question could be length of the url**

using base64 encoding, 6 letter long key would result in 64^6 = 68.7 billion possible strings

lets assume, 6 letter keys would suffice for our system

if we use md5 alg , it will produce 128 bit hash value. after base64 encoding , we will get more than 21 char string

but we have space only for 8 character how will we chose our keys ?

we can take 1st 6 letters or 8 letter for the key. this could result in duplication but we can remove some string in encoding or swap.

**what are different issues with our solution?**

1. different persons enter same url and get same short url is not acceptable.
2. what if part of url are identical in some way?

**workaround**
- we can append an increasing sequence number to make it unique ( we no need store in db)
- possible problem (ever increasing sequence) and impact performance
- another solution append user id to the url to make it unique
- even after we get duplicate, we will regeneate it again until we get uniquw.


![image](https://github.com/Dhamodharan17/system-design-stepbystep/assets/30789057/57f0000e-f3a6-4b2d-9fbe-cab88bdcef63)

**2. Generating Keys offline**

- 
Adding new service only for key generation KGS.

KGS will generate randowm 6 letter string before hand and stores in db.

whenever we want a short url we can take from db which is quite fast and simple.

No need to worry about duplication, kgs will ensure uniqueness.

-
**can concurreny cause problems ?**

when 2 or more servers try to read the same key how can we solve this concurreny problem?


