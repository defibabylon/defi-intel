# DeFi Intelligence MCP

Operator-grade DeFi intelligence for Claude. 11 tools covering live TVL, yield pools, stablecoin flows, governance proposals, RWA attestation scores, X/Twitter market narrative, and a one-call operator brief for NotebookLM.

**Listed on Smithery**: [smithery.ai/servers/defibabylon/defi-intel](https://smithery.ai/servers/defibabylon/defi-intel)

---

## Why this exists

DefiLlama's native MCP has 4 basic tools. Nansen and Messari are enterprise-gated. This fills the gap: operator-grade DeFi intelligence accessible to any Claude user, no subscription required for core data.

The `get_market_narrative` tool adds live X/Twitter sentiment via Grok — the only DeFi MCP that combines on-chain data with real-time social signal.

---

## Tools

| Tool | Description |
|------|-------------|
| `get_top_protocols(limit, chain)` | Top DeFi protocols by TVL, optional chain filter |
| `get_protocol_tvl(slug)` | Detailed TVL + change metrics for a specific protocol |
| `get_chain_tvl_ranking(limit)` | TVL ranking across all blockchain networks |
| `get_top_yield_pools(min_apy, chain, limit)` | Yield farming pools filtered by APY and chain |
| `get_stablecoin_summary()` | Market cap and peg status for top 10 stablecoins |
| `get_protocol_fees(slug, days)` | Fee revenue over N days by protocol |
| `search_protocols(query)` | Fuzzy search protocols by name, category, or chain |
| `get_rwa_attestation(protocol)` | RWA-Attest risk scores (8 protocols attested) |
| `get_governance_activity(ecosystem)` | Active governance proposals by ecosystem |
| `get_market_narrative(topic)` | Live X/Twitter sentiment on any DeFi topic via Grok |
| `generate_operator_brief(protocols, chains, include_narrative)` | One-call brief: TVL + yields + stablecoins + governance + narrative + RWA. NotebookLM-ready. |

Data sources: [DefiLlama](https://defillama.com), [rwa-attest](https://github.com/defibabylon/rwa-attest), Grok x_search.

---

## Install via Smithery

```bash
npx -y @smithery/cli install defibabylon/defi-intel --client claude
```

Or add to Claude Desktop `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "defi-intel": {
      "type": "http",
      "url": "https://defi-intel.siteflowops.co.za/mcp"
    }
  }
}
```

---

## Self-host

```bash
git clone https://github.com/defibabylon/defi-intel
cd defi-intel
pip install -r requirements.txt
cp .env.example .env        # add XAI_API_KEY for x_search
python3 server.py
```

Server runs at `http://localhost:8086`. MCP endpoint: `/mcp`. Health: `/health`.

For stdio mode (Smithery local):
```bash
python3 server.py --stdio
```

### Environment variables

| Variable | Required | Description |
|----------|----------|-------------|
| `XAI_API_KEY` | Optional | xAI key for `get_market_narrative`. Get at [console.x.ai](https://console.x.ai) |
| `PORT` | Optional | HTTP port (default: 8086) |

---

## Systemd (Linux, persistent)

```ini
[Unit]
Description=DeFi Intelligence MCP Server
After=network.target

[Service]
ExecStart=/usr/bin/python3 /path/to/server.py
Restart=always
Environment=XAI_API_KEY=your_key_here

[Install]
WantedBy=multi-user.target
```

---

## Background

[What You Build When the Data Doesn't Exist Yet](https://paragraph.com/@0x836c370ab3ca72a823ec7f1a9c148305db3e5812/what-you-build-when-the-data-doesnt-exist-yet) — the Paragraph article behind this tool.

---

## Related

- [gov-scout](https://github.com/defibabylon/gov-scout) — the governance scraper behind `get_governance_activity`
- [rwa-attest](https://github.com/defibabylon/rwa-attest) — the attestation scoring engine behind `get_rwa_attestation`
- [HypeFiltr](https://hypefiltr.substack.com) — weekly DeFi gap intelligence newsletter; this MCP was the first gap actioned
