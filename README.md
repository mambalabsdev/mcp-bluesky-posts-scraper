# Bluesky Posts Scraper MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-bluesky-posts-scraper)](https://www.npmjs.com/package/@mambalabsdev/mcp-bluesky-posts-scraper) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-bluesky-posts-scraper)](https://github.com/mambalabsdev/mcp-bluesky-posts-scraper/blob/main/LICENSE)

An MCP server for the Mamba Labs Bluesky Posts Scraper actor on Apify. Bluesky posts by keyword or account, and replies by post.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give keywords, accounts, or post addresses. The actor reads Bluesky's public API with no login and no key, and returns one row per post or reply with exact counts, dates, hashtags, links, and images. A free status row per source says what it returned or why it stopped.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-bluesky-posts-scraper": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-bluesky-posts-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_bluesky_posts` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "What are people saying on Bluesky about cold email this month?"
- "Get the last 50 posts from this Bluesky account with their like counts."
- "Read the replies on this Bluesky post into a table."

## Tool

`get_bluesky_posts`. One tool. Example arguments:

```json
{
  "queries": [
    "apify"
  ],
  "max_posts_per_source": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `queries` | array | no | Keywords to search, up to 20. Without your own app password each search returns one page, about 100 posts. Text from Clay splits on new lines, so a query can hold a comma. |
| `authors` | array | no | Bluesky handles, profile addresses, or DIDs, up to 20. Returns each account's own posts, newest first. |
| `post_urls` | array | no | Addresses of Bluesky posts to read, up to 100. |
| `sort` | string | no | latest or top. Default latest. Used for queries. |
| `since_date` | string | no | Only posts from this date, such as 2026-10-01. |
| `until_date` | string | no | Only posts before this date, such as 2026-11-01. |
| `language` | string | no | Language code such as en or de. Used for queries. Reads the language the post's author set. |
| `max_posts_per_source` | number | no | Per query or account, up to 2000. Default 100. A search with no app password stops at about 100. |
| `include_replies` | boolean | no | yes or no. Default no. Reads the replies under each post found. Each reply is a charged row. |
| `max_replies_per_post` | number | no | Up to 200. Default 20. |
| `include_account_replies` | boolean | no | yes or no. Default no. Used for accounts. |
| `include_reposts` | boolean | no | yes or no. Default no. Used for accounts. A repost row shows the original author. |
| `bluesky_handle` | string | no | Optional, with the app password. Used only to page a search past its first page. |
| `bluesky_app_password` | string | no | Optional. Create an app password in Bluesky settings; never use your main password. Sent only to Bluesky. Without it a search returns one page, about 100 posts. Accounts and post addresses need no password. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `post-row`, one post or reply | $0.0011 | $0.001045 | $0.00099 | $0.000935 |
| `apify-actor-start`, per run start | $0.0065 | $0.0065 | $0.0065 | $0.0065 |

Source status rows and refused input are free. Mamba Labs pays the platform costs of the run. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/bluesky-posts-scraper](https://apify.com/mambalabs/bluesky-posts-scraper)

## License

MIT. See [LICENSE](LICENSE).
