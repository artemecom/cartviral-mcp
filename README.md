# CartViral MCP Server

[![smithery badge](https://smithery.ai/badge/mercury-promotion/CartViral_mcp)](https://smithery.ai/servers/mercury-promotion/CartViral_mcp)

AI product videos for e-commerce, from any AI agent. [CartViral](https://cartviral.com) turns a product page into ready-to-post TikTok, Reels and Shorts clips. This hosted MCP server lets Claude, ChatGPT, Codex, Cursor and other agents import products, generate clips, fetch download links, browse brand affiliate offers and pitch brands.

- **Endpoint:** `https://app.cartviral.com/mcp` (Streamable HTTP, hosted, nothing to install)
- **Auth:** OAuth 2.1 (dynamic client registration, PKCE) or an API key
- **Registry:** [`com.cartviral/cartviral`](https://registry.modelcontextprotocol.io/v0/servers?search=cartviral) in the official MCP Registry
- **Docs:** https://cartviral.com/mcp
- **Smithery:** https://smithery.ai/servers/mercury-promotion/CartViral_mcp
- **Price:** free account with 48 credits; read-only tools cost nothing

## Connect

### Claude (web and desktop)
Settings → Connectors → **Add custom connector** → paste `https://app.cartviral.com/mcp` → Connect. Sign in to CartViral and approve access.

### ChatGPT
Add a custom connector (developer mode) with the URL `https://app.cartviral.com/mcp` and OAuth authentication.

### Claude Code
```bash
claude mcp add --transport http cartviral https://app.cartviral.com/mcp
# then run /mcp inside Claude Code and choose Authenticate
```
Or with an API key:
```bash
claude mcp add --transport http cartviral https://app.cartviral.com/mcp \
  --header "Authorization: Bearer YOUR_KEY"
```

### Codex CLI
`~/.codex/config.toml`:
```toml
[mcp_servers.cartviral]
url = "https://app.cartviral.com/mcp"
bearer_token_env_var = "CARTVIRAL_API_KEY"
```

### Cursor
`~/.cursor/mcp.json`:
```json
{
  "mcpServers": {
    "cartviral": {
      "url": "https://app.cartviral.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_KEY" }
    }
  }
}
```

### Any other client
Streamable HTTP, URL `https://app.cartviral.com/mcp`. Either let the client run the OAuth flow (discovery at `/.well-known/oauth-protected-resource/mcp`), or send `Authorization: Bearer YOUR_KEY` (or `x-api-key: YOUR_KEY`). Create keys at https://app.cartviral.com/account/agents.

## Tools

| Tool | What it does | Credits |
|---|---|---|
| `get_account` | Plan, credit balance, price of each clip type | 0 |
| `import_products` | Import a Shopify catalog (up to 750 products) or one product page from Shopify, Amazon, Etsy and most sites | 0 |
| `list_products` | Catalog with CartViral score (push / hold) | 0 |
| `generate_clips` | Montage, AI story reel, ad creative or avatar reel of a product, 9:16 | 1 / 4 / 4 / 10 per clip |
| `list_clips` | Render status and download links (valid 24 hours) | 0 |
| `list_offers`, `join_offer` | Creators: brand offers in CartViral Earn and your tracking link | 0 |
| `bring_a_brand`, `list_pitches` | 3 AI clips of a brand's product and a page to send it; who opened it | 12 per pitch |

Every tool declares `readOnlyHint`, `destructiveHint` and `openWorldHint`.

## Example prompts

- "Import https://yourstore.com and show me the 5 products with the best CartViral score."
- "Make 3 AI story clips of the top product and give me the download links when they're ready."
- "Find CartViral Earn offers that ship to the US with at least 20% commission and join the best one."
- "Pitch these Shopify brands: [links]. Tomorrow, tell me which ones opened the page."

## Security

- OAuth tokens are opaque and stored hashed; access tokens last 1 hour, refresh tokens rotate on every use. Disconnect any app in Account → AI agents.
- A token or key acts as your account (same credits and limits as the web app). Up to 120 calls per minute per key.
- Agents never post to social media for you: they make and fetch clips, you post.

## Support

support@cartviral.com · https://cartviral.com
