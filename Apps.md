# Application & Serverless Notes

#### SQS

* Think decouple.  Decouple components of applications.
* Messages in 256 KB in size.
* Acts as buffer between producer and consumer
* Pull-based not push-based
* Kept in queue from 1 minute to 14 days; default is 4 minutes
* Visibilty timeout: amount of time a message is invisible after a reader picks it up.  If not processed by reader within this timeout the message will become visible in the queue again for another reader to pick up.  Maximum visibility timeout is 12 hours
* SQS guarantees messages will be processed at least once
* Long-polling
* SWF or Simple WorkFlow; think warehouses/people; task oriented
* SNS: instantaneous, push-based delivery.

#### Elastic Transcoder

* Media transcoder in the cloud
* Convert media file formats
* Provides transcode presets e.g. iOS, Android. Also configure resolution/quality

#### API Gateway

* "front door" to application
* used mostly with lambda to define RESTful API
* caching capabilities to increase performance
* low-cost and scales automatically
* can throttle API Gateway to prevent attacks
* CORS or cross-origin resource sharing; logs to CloudWatch

#### Kineses

* Streaming data: data generated continuously at thousands of sources, send in simultaneously, and in small sizes e.g. purchases, stocks, game data, twitter/social media, geospatial data
* 3 Types: Streams, Analytics, and Firehose
* Streams consist of shards and data lives in the shards for 24 hours to 7 days for consumers to process
* Firehose processes data immediately. Not-persistent.
* Analytics processes with firehose and streams.

#### Web Identity Federation & Cognito

* Web Identity Federation give your users access to AWS resources after they have authenticated with a web-identity provider (Facebook, Google, etc.)
* Cognito is a WIF.  User pools (JSON Web Tokens) vs Identity pools (S3, DynamoDB). Use user pool to get access to identity pools.  Then get policy based access to resources with identity pool.

#### Lambda

* Event-driven; 1 event = 1 function
* Or HTTP requests via API Gateway
* Scale out automatically
* Furthest level of infrastructure abstraction and NO SERVERS!
* Priced on number of requests: first 1,000,000 requests are free then $0.20 per million after
* Priced on duration: rounded up to 100ms
* Know what services are serverless: s3, dynamoDB, also Aurora serverless but RDS is not.
* Architectures get complicated; use X-Ray to debug
* Lambdas do things globally
* Know triggers; RDS can't trigger
