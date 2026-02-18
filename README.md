# Twitter MCP Server

A Model Context Protocol (MCP) server for Twitter/X API v2 integration using OAuth 1.0a authentication.

## Features

- 🐦 Post tweets (with reply support)
- 🔍 Search recent tweets
- 👤 Get user information
- 🗑️ Delete tweets
- 📊 Tweet metrics (likes, retweets)

## Prerequisites

1. Twitter Developer Account
2. Twitter App with OAuth 1.0a enabled
3. API credentials (Consumer Key, Consumer Secret, Access Token, Access Token Secret)

### Setting up Twitter Developer Account

1. Go to [Twitter Developer Portal](https://developer.twitter.com/en/portal/dashboard)
2. Create a new app or use an existing one
3. Enable **OAuth 1.0a** in app settings
4. Set app permissions to **Read and Write**
5. **Important**: After changing permissions, regenerate your access tokens!
6. Get your credentials:
   - API Key (Consumer Key)
   - API Secret Key (Consumer Secret)
   - Access Token
   - Access Token Secret

## Installation
```bash
cd /home/valiantlynx/projects/hack-mcp/server/twitter-mcp
uv sync
```

## Configuration
Create a `.env` file in the project root:

```env
API_KEY=your_api_key_here
API_SECRET_KEY=your_api_secret_key_here
ACCESS_TOKEN=your_access_token_here
ACCESS_TOKEN_SECRET=your_access_token_secret_here
```

## Running the Server

```bash
# With uv
uv run python src/twitter_mcp/server.py

# Or with python directly
python src/twitter_mcp/server.py
```

The server will start on `http://0.0.0.0:8083/mcp`

## Available Tools

### 1. post_tweet
Post a new tweet to Twitter.

**Parameters:**
- `text` (required): Tweet content (max 280 characters)
- `reply_to_tweet_id` (optional): Tweet ID to reply to

**Example:**
```python
post_tweet(text="Hello from MCP!", reply_to_tweet_id="123456789")
```

### 2. search_tweets
Search for recent tweets matching a query.

**Parameters:**
- `query` (required): Search query (supports Twitter operators)
- `max_results` (optional): Number of results (10-100, default: 10)

**Example:**
```python
search_tweets(query="python AI", max_results=20)
```

### 3. get_user_info
Get information about a Twitter user.

**Parameters:**
- `username` (required): Twitter username (with or without @)

**Example:**
```python
get_user_info(username="elonmusk")
```

### 4. delete_tweet
Delete one of your tweets.

**Parameters:**
- `tweet_id` (required): ID of the tweet to delete

**Example:**
```python
delete_tweet(tweet_id="123456789")
```

## Error Handling

The server provides detailed error messages for:
- **403 Forbidden**: Check app permissions and regenerate tokens after permission changes
- **429 Rate Limit**: Immediate rate limit errors often indicate auth issues
- **401 Unauthorized**: Invalid or expired credentials
- Configuration errors: Missing environment variables

## Integration with MCP Client

Add to your `mcp.json`:

```json
{
  "servers": {
    "twitter-mcp": {
      "url": "http://127.0.0.1:8083/mcp",
      "type": "http"
    }
  }
}
```

## Troubleshooting

### Error 403: Forbidden
1. Ensure your app has **Read and Write** permissions
2. Regenerate access tokens AFTER changing permissions
3. Wait a few minutes for changes to propagate

### Error 429: Rate Limit
- If this happens immediately, it's likely an authentication issue
- Verify all credentials are correct
- Check that tokens match the current permission level

### Connection Issues
- Ensure the server is running on port 8083
- Check firewall settings
- Verify environment variables are loaded correctly

## API Documentation

- [Twitter API v2 Documentation](https://developer.x.com/en/docs/x-api)
- [python-twitter Library](https://sns-sdks.lkhardy.cn/python-twitter/)
- [OAuth 1.0a Documentation](https://developer.x.com/en/docs/authentication/oauth-1-0a)

## License

MIT
