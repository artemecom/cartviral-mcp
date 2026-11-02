# CartViral MCP Server

AI product videos, photos and ads for e-commerce, from any AI agent. [CartViral](https://cartviral.com) turns a product page or product photos into ready-to-post TikTok, Reels and Shorts clips, product photos, carousels, static ads and listing copy. This hosted MCP server (33 tools) lets Claude, ChatGPT, Codex, Cursor and other agents import products, generate clips and images, fetch download links, browse brand affiliate offers and pitch brands.

- **Endpoint:** `https://app.cartviral.com/mcp` (Streamable HTTP, hosted, nothing to install)
- **Auth:** OAuth 2.1 (dynamic client registration, PKCE) or an API key
- **Registry:** [`com.cartviral/cartviral`](https://registry.modelcontextprotocol.io/v0/servers?search=cartviral) in the official MCP Registry
- **Docs:** https://cartviral.com/mcp
- **Glama:** https://glama.ai/mcp/connectors/com.cartviral/cartviral
- **Server card:** https://app.cartviral.com/.well-known/mcp.json · REST API (OpenAPI 3.1): https://app.cartviral.com/api/v1/openapi.json
- **Price:** free account with 18 credits (3 AI clips), no card; read-only tools cost nothing. After your first purchase, the first agent connection adds 20 bonus credits (once per account)
- **Clips:** photo montages, AI story reels and creator (UGC) clips where an AI creator talks to camera in their own voice, all vertical 9:16 and built from your real product photos

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

### Claude Code plugin
This repository is also a Claude Code plugin marketplace:
```bash
/plugin marketplace add artemecom/cartviral-mcp
/plugin install cartviral@cartviral
```
Then run `/mcp`, pick cartviral and choose Authenticate.

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
| `generate_clips` | Montage, AI story reel or creator (UGC) clip of a product, 9:16. `clip_type`: `montage`, `ai_story`, `avatar` (the creator clip); `ad` is kept for older integrations and makes the same AI clip as `ai_story`. Optional exact format id and `character_id` | 1 / 6 / 12 per clip (`ad`: 6) |
| `remake_from_reference` | A reference video's structure (hook, beats, timings) shot again with your product | same as an AI story clip |
| `list_clips` | Render status and download links (valid 24 hours) | 0 |
| `generate_photos`, `list_photo_sets` | Model photoshoot, product photos or infographics, 4 photos a set | 9 / 8 / 7 per set |
| `generate_carousel`, `list_carousels` | Swipe-through carousel for Instagram (4:5) and TikTok photo mode (9:16) | 3 per set |
| `generate_static_ads`, `list_static_ads` | Static ads in 6 layouts, each in 1:1, 4:5 and 9:16 | 4 per set |
| `get_listing_copy` | Amazon, Shopify, Etsy and AI-search listing copy: title, bullets, description, tags, FAQ schema | 1 the first time, then free while the product is unchanged |
| `get_copy` | Ad copy, social captions, a creator brief or a 30-day content plan | 1 per tool, then free while the product is unchanged |
| `list_offers`, `join_offer` | Creators: brand offers in CartViral Earn and your tracking link | 0 |
| `bring_a_brand`, `list_pitches` | 3 AI clips of a brand's product and a page to send it; who opened it | 18 per pitch |
| `create_image` | Images from your own prompt with up to 3 reference photos (covers, thumbnails, banners) | 1 per image |
| `create_video` | Video from a prompt, 4 to 10 s, with camera moves and sound | 2 on CartViral's engine; premium models priced per second |
| `make_cinematic` | A 15 or 30 s brand film of a product | 12 / 18 |
| `make_angles` | New angles of the same shoot from one photo | 1 per angle |
| `make_talking_photo` | A person's photo says your text | 2 per take of up to 10 s |
| `reframe_video`, `remove_background` | Reframe a video, remove a photo's background | 1 each |
| `copy_motion` | The person from a photo repeats a reference video's movement, up to 15 s | premium, priced by length (when premium video is on) |
| `breakdown_video` | Break down a viral video's hook, beats and timings | 0 |
| `create_character`, `save_character`, `list_characters` | A recurring brand character, the same face in every clip | create 2 (2 portraits); save and list 0 |
| `get_create_options`, `upload_media`, `list_creations` | Models, formats and prices; store a file; result links of Create jobs | 0 |

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
