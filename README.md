# Reddit Sentiment Analysis
**Note**: The project is replica of ____

## Architecture

## 📥 Reddit Ingestion Pipeline
We use Pipeline to pull Reddit data (posts + comments) using the official API (no PRAW), then store the raw JSON into OneLake.

The ingestion pipeline performs three main tasks:
-  Authenticate with Reddit [OAuth2 Client Credentials Flow](https://medium.com/@conrardy/oauth2-0-resolving-the-client-credentials-flow-3ae3262d0719)
  1. 
-  Fetch subreddit posts + comments using REST API
-  Persist raw JSON data into OneLake/ADLS for Silver transformations

This ingestion layer is designed for use with Fabric Lakehouse, ADF, and dbt / PySpark downstream.
