# Reddit Sentiment Analysis
**Note**: The project is replica of ____

## Architecture

## 📥 Reddit Ingestion Pipeline
We use Pipeline to pull Reddit data (posts + comments) using the official API (no PRAW), then store the raw JSON into OneLake.

The ingestion pipeline performs three main tasks:
#### Authenticate with Reddit OAuth2 Client Credentials Flow. <sup>[link1](https://medium.com/@conrardy/oauth2-0-resolving-the-client-credentials-flow-3ae3262d0719)</sup> <sup>[link2](https://github.com/reddit-archive/reddit/wiki/OAuth2-Quick-Start-Example)</sup>

Reddit APIs require OAuth 2.0 authentication. You must register an "installed app" or "script" application on Reddit to get a Client ID and Secret.

**1. Create an HTTP Linked Service:**

    Name: Reddit_Auth_LS
    URL: https://www.reddit.com/api/v1/access_token

**2. Create a Web Activity (Get Token)**

    URL: Use the URL from the Linked Service.
    Method: POST

    Headers:
    Authorization: @concat('Basic ', base64(concat('YOUR_CLIENT_ID', ':', 'YOUR_CLIENT_SECRET')))
    Content-Type: application/x-www-form-urlencoded
    User-Agent: reddit-sentiment-fabric-bot/1.0

    Body: grant_type=client_credentials
    
    Output: This activity will return a JSON object containing the access_token.
    Expression to store token(ADF stores the response automatically in): @activity('Get Token').output.access_token   
*Can set your username,secrets as activity or pipeline level parameters.*

**3.  Set Up REST Linked Service for Data **
    Name: Reddit_Posts_LS

    Base URL: https://oauth.reddit.com (Note the use of oauth.reddit.com for authenticated calls).

    Authentication Type: Anonymous. (We will pass the token via headers in the Copy Activity).
#### Fetch subreddit posts + comments using REST API

**1. Iterate and Copy Data (Lookup & Copy Activities)**



####  Persist raw JSON data into OneLake/ADLS for Silver transformations

This ingestion layer is designed for use with Fabric Lakehouse, ADF, and dbt / PySpark downstream.
