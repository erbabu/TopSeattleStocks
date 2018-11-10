# TopSeattleStocks
Top Seattle companies stocks data as an Alexa flash briefing skill.

## Description
This repository is to share my learnings while building an Alexa flash briefing skill 'Seattle Companies Stock' using a free stock quotes API.

**Alexa Flash Briefing skill:** https://www.amazon.com/ERBABU-Seattle-Companies-Stock/dp/B07K4BPTTJ

## Steps
Following is what I have followed to build this skill.
1. Decided on what I want to render in flash briefing. I wanted to render dynamic content, at the same time using some third party API, so that I can use this project for self-learning. So, picked up stock quote API. To limit the count of stock quotes, restricted to top Seattle area companies based on my interest from the list available in Wikipedia.
2. Read and understood the Alexa flash briefing skill online document.
3. Manually created the **JSON feed file** using the stock data from API for rendering. Used only 2 data points per stock (latest price and change%).
4. Stored the file in my **AWS S3** bucket, made it public read-only and got the URL to this file.
5. Created the Alexa flash briefing skill using the S3 file URL as the feed.
6. Tested and submitted for certification. It's **live** now.
7. Till I finish the program for automatically updating the feed file in S3 bucket, I was updating the feed file manually with new content frequently.

### Iterations
I have achieved the automatic generation of my feed file using **AWS Lambda + AWS S3 SDK + Node.js + Async js** library. Following are the iterations to reach the final goal.
1. Wrote a simple HTML page to generate the feed file contents using js and **XMLHTTPRequest for REST API** calls. I used the stock API from iextrading.com (benefit is, there is no authentication for calling API, ofcourse there is a burst limit and throttling limit per day per IP). Using this content, I was updating my S3 file instead of manually updating all 12 company stock quotes data by hand.
2. Wrote node.js program in local environment for generating the feed file and transferred using AWS CLI to my S3 bucket.
3. Created an AWS Lambda function using the node.js code I was running successfully in local. But it was not working as in local. After searching online, figured out that I am running into **async** issue. I was pulling the API data, prepared my feed file and uploading to S3. Lambda executed async and nothing was written into S3.
4. Tried couple of options to overcome async - async waterfall and js promise. I was not having luck using async waterfall but using js promise I was able to successfully upload the retrieved API stock data to my feed JSON file in S3 (i am using async libray though).
5. Ran the test for sometime and verified the JSON contents using the free online tool (jsoneditoronline).
6. **Scheduled** to trigger my **AWS Lambda** function using **AWS Cloudwatch** event scheduler.

## References:
My Alexa Flash Briefing skill - https://www.amazon.com/ERBABU-Seattle-Companies-Stock/dp/B07K4BPTTJ
Companies Info - https://en.wikipedia.org/wiki/List_of_companies_based_in_Seattle
API - https://iextrading.com/developer/
API - https://www.alphavantage.co/
Alexa skill details - https://developer.amazon.com/docs/flashbriefing/understand-the-flash-briefing-skill-api.html
Alexa Flash briefing skill JSON feed reference - https://developer.amazon.com/docs/flashbriefing/flash-briefing-skill-api-feed-reference.html
AWS Lambda + S3 + Node.js - https://docs.aws.amazon.com/lambda/latest/dg/with-s3-example.html
AWS CLI - https://aws.amazon.com/cli/
Scheduling AWS Lambda using AWS Cloudwatch - https://docs.aws.amazon.com/lambda/latest/dg/with-scheduled-events.html
Alexa Github - https://github.com/alexa
Alexa skill building help - https://alexaincanada.ca/how-to-create-a-flash-briefing-for-alexa/
Alexa skill building help - https://www.socialmediaexaminer.com/how-to-set-up-an-alexa-flash-briefing-a-guide-for-marketers/
JSON Editor online - https://jsoneditoronline.org/
