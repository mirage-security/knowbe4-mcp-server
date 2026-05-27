# KnowBe4 MCP Server

A Model Context Protocol (MCP) server for the KnowBe4 Reporting REST API. This server enables AI assistants to interact with your KnowBe4 security awareness training platform data.

**Created by [Mirage Security](https://miragesecurity.ai)**

## Features

This MCP server provides access to all KnowBe4 Reporting API endpoints:

### Account
- Get account and subscription data
- Get account risk score history

### Users
- List all users (with filtering by status, group)
- Get specific user details
- Get users in a specific group
- Get user risk score history

### Groups
- List all groups
- Get specific group details
- Get group risk score history

### Phishing
- Get phishing campaigns
- Get phishing security tests (PSTs)
- Get PST recipient results
- Get specific recipient details

### Training
- Get store purchases
- Get policies
- Get training campaigns
- Get training enrollments

## Prerequisites

- Node.js 18 or higher
- A KnowBe4 account with Platinum or Diamond subscription
- KnowBe4 API key (available in your Account Settings)

## Installation

You have two options: run the published package directly with `npx` (no install, recommended), or clone and build locally.

### Option 1: Run with npx (recommended)

No installation needed — `npx` will fetch and run the latest published version of `knowbe4-mcp-server` on demand. Skip ahead to [Usage with Claude Desktop](#usage-with-claude-desktop) and use the npx config snippet.

### Option 2: Install from source

1. Clone or download this repository
2. Install dependencies:

```bash
npm install
```

3. Build the TypeScript code:

```bash
npm run build
```

## Configuration

### Getting Your API Key

1. Log in to your KnowBe4 console
2. Navigate to **Account Settings**
3. Find the **API** section
4. Copy your API key or generate a new one if needed

**Important:** Keep your API key secure and do not share it publicly.

### Environment Variables

The server requires the following environment variables:

- `KNOWBE4_API_KEY` (required): Your KnowBe4 API key
- `KNOWBE4_REGION` (optional): Your server region - `us`, `eu`, `ca`, `uk`, or `de` (default: `us`)

Determine your region based on your KnowBe4 login URL:
- US: `training.knowbe4.com` → use `us`
- EU: `eu.knowbe4.com` → use `eu`
- CA: `ca.knowbe4.com` → use `ca`
- UK: `uk.knowbe4.com` → use `uk`
- DE: `de.knowbe4.com` → use `de`

## Usage with Claude Desktop

Add this server to your Claude Desktop configuration file:

### macOS
Edit: `~/Library/Application Support/Claude/claude_desktop_config.json`

### Windows
Edit: `%APPDATA%\Claude\claude_desktop_config.json`

### Using npx (no install)

This is the simplest setup — `npx` downloads and caches the package the first time Claude Desktop launches it. The `-y` flag is required so npx doesn't prompt for confirmation (which would break the stdio handshake).

```json
{
  "mcpServers": {
    "knowbe4": {
      "command": "npx",
      "args": ["-y", "knowbe4-mcp-server@1.0.0"],
      "env": {
        "KNOWBE4_API_KEY": "your-api-key-here",
        "KNOWBE4_REGION": "us"
      }
    }
  }
}
```

The args pin to `knowbe4-mcp-server@1.0.0` for reproducibility. Drop the `@1.0.0` suffix to always pull the latest published version.

> **Windows note:** if `command: "npx"` fails to launch from Claude Desktop, use `"command": "npx.cmd"` instead.

### Using a local build

If you cloned and built the repo (Option 2 above):

```json
{
  "mcpServers": {
    "knowbe4": {
      "command": "node",
      "args": ["/absolute/path/to/knowbe4-mcp-server/build/src/index.js"],
      "env": {
        "KNOWBE4_API_KEY": "your-api-key-here",
        "KNOWBE4_REGION": "us"
      }
    }
  }
}
```

Replace:
- `/absolute/path/to/knowbe4-mcp-server` with the actual path to this project
- `your-api-key-here` with your actual KnowBe4 API key
- `us` with your region if different

## Available Tools

Once configured, Claude will have access to these tools:

### Account Tools
- `get_account` - Get account and subscription information
- `get_account_risk_score_history` - Get historical risk scores

### User Tools
- `get_users` - List all users with optional filters
- `get_user` - Get specific user by ID
- `get_group_members` - Get users in a group
- `get_user_risk_score_history` - Get user risk score history

### Group Tools
- `get_groups` - List all groups
- `get_group` - Get specific group
- `get_group_risk_score_history` - Get group risk score history

### Phishing Tools
- `get_phishing_campaigns` - List all phishing campaigns
- `get_phishing_campaign` - Get phishing campaign details
- `get_phishing_security_tests` - List all PSTs
- `get_campaign_security_tests` - Get PSTs from specific campaign
- `get_phishing_security_test` - Get specific PST
- `get_pst_recipients` - Get recipient results for PST
- `get_pst_recipient` - Get specific recipient result

### Training Tools
- `get_store_purchases` - List store purchases
- `get_store_purchase` - Get specific store purchase
- `get_policies` - List policies
- `get_policy` - Get specific policy
- `get_training_campaigns` - List training campaigns
- `get_training_campaign` - Get specific training campaign
- `get_training_enrollments` - List training enrollments
- `get_training_enrollment` - Get specific enrollment

## Example Prompts

Once configured, you can ask Claude things like:

- "What is my organization's current risk score?"
- "Show me all active users in my account"
- "Get the results from phishing security test ID 12345"
- "List all training campaigns"
- "Show me users with the highest phish-prone percentage"
- "Get all groups and their current risk scores"

## API Rate Limits

KnowBe4 API has the following limits:
- 2,000 requests per day plus the number of licensed users
- Maximum 4 requests per second
- Burst limit of 50 requests per minute

## Pagination

Most list endpoints support pagination with these parameters:
- `page` - Page number (default: 1)
- `per_page` - Results per page (default: 100, max: 500)

The server automatically handles these parameters for you.

## Development

### Watch Mode
Run TypeScript compiler in watch mode:

```bash
npm run watch
```

### Testing

The project includes comprehensive integration tests using Node.js built-in test runner.

**Run all tests:**

```bash
KNOWBE4_API_KEY=your-key npm test
```

**Run tests in watch mode:**

```bash
KNOWBE4_API_KEY=your-key npm run test:watch
```

**Test Coverage:**
- 30 integration tests across 5 endpoint categories
- Account endpoints (3 tests)
- User endpoints (5 tests)
- Group endpoints (5 tests)
- Phishing endpoints (8 tests)
- Training endpoints (9 tests)

Tests make real API calls and validate response structure and data. See [tests/README.md](tests/README.md) for detailed testing documentation.

## Security Notes

- Never commit your API key to version control
- Store your API key securely
- Use environment variables for configuration
- KnowBe4 API keys provide access to sensitive security training data
- Anonymous accounts cannot retrieve anonymized data

## Troubleshooting

### "KNOWBE4_API_KEY environment variable is required"
Make sure you've set the `KNOWBE4_API_KEY` in your Claude Desktop config.

### "401 Unauthorized"
Your API key is incorrect or expired. Generate a new one in KnowBe4 Account Settings.

### "404 Not Found"
Check that you're using the correct region for your account.

### "429 Too Many Requests"
You've exceeded the rate limit. Wait before making more requests.

## About

This MCP server was created by **[Mirage Security](https://miragesecurity.ai)** to enable seamless integration between AI assistants and the KnowBe4 security awareness training platform.

## License

MIT

## Resources

- [KnowBe4 API Documentation](https://developer.knowbe4.com/rest/reporting)
- [Model Context Protocol](https://modelcontextprotocol.io)
- [Claude Desktop](https://claude.ai/download)
