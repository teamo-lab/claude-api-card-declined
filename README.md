# Claude API card declined? Pay for Claude, the API and Claude Code, with USDT

> Maintained by the TeamoRouter team. The gateway in the second half of this guide is ours.

Anthropic declined your card, or your country isn't on its [supported countries list](https://www.anthropic.com/supported-countries)? You can keep using Claude, including in Claude Code and the Anthropic SDK, by paying a gateway with **USDT, USDC or ETH** instead of a card.

## Short answer

- **First, rule out the fixable causes** below: billing address, 3-D Secure, international payments switched off.
- **If your card or country simply isn't accepted**, top up [TeamoRouter](https://teamorouter.com/?utm_source=github&utm_medium=readme&utm_campaign=b6_claude-api-card-declined) with crypto from any exchange app or wallet ($5 minimum), and point Claude Code or the Anthropic SDK at `https://api.teamorouter.com`.
- **Claude runs on the native Anthropic Messages API** (`/v1/messages`), so your code stays the same apart from the base URL and the key.

## Why cards get declined

Card checks at checkout usually look at three things: the country the card was issued in, whether the billing address matches what the bank has, and whether the bank approves the charge (often through 3-D Secure). The usual messages:

| Message | Usual cause |
|---|---|
| `Your card was declined` | Billing address mismatch, a bank-side block, or not enough funds |
| `Your card does not support this type of purchase` | The card's issuing country or card type is filtered |
| First charge works, renewal fails | Prepaid or virtual card that doesn't support recurring charges |
| Every attempt fails after a few tries | Too many retries in a row; wait before trying again |

Worth trying before anything else:

1. Enter the billing address exactly as your bank has it.
2. Turn on international and online payments in your banking app.
3. Finish the 3-D Secure prompt; a timed-out verification counts as a decline.
4. Check that your country is on Anthropic's [supported countries list](https://www.anthropic.com/supported-countries). If it isn't, no card issued there will get through.

## Pay with crypto instead

1. Sign up at [teamorouter.com](https://teamorouter.com/?utm_source=github&utm_medium=readme&utm_campaign=b6_claude-api-card-declined), open the top-up dialog, choose an amount ($5 minimum) and click **Pay with crypto**.
2. On the checkout page pick the coin, then the network. Which networks appear depends on the amount:

   | Amount | Coins and networks |
   |---|---|
   | From $5 | USDT (BEP20), USDC (BEP20), ETH |
   | From $10 | also USDC on Base and USDT on Ethereum (ERC20) |
   | From $30 | also USDT on TRON (TRC20) |

3. Within 60 minutes, send at least the amount shown, on that same network, to the address on the page, from Binance, OKX, Bybit or any wallet. The balance is credited after confirmation; no browser extension needed, and it works on a phone.

Binance charges about 0.01 USDT to withdraw on BEP20 and about 1.5 USDT on TRC20, so BEP20 is the cheap route for small amounts. Crypto top-ups are non-refundable. Cards are accepted too, if you have one that works.

Full walkthrough with error fixes: [How to pay for the Claude API with USDT](https://teamorouter.com/blogs/pay-claude-with-usdt?utm_source=github&utm_medium=readme&utm_campaign=b6_claude-api-card-declined).

## Use it

**Claude Code** (macOS / Linux):

```bash
export ANTHROPIC_BASE_URL=https://api.teamorouter.com
export ANTHROPIC_AUTH_TOKEN="sk-teamo-xxxxxx"
export ANTHROPIC_MODEL=claude-opus-5-5
claude
```

Windows: use `setx` with the same names, then open a new terminal. Details: [Claude Code setup](https://teamorouter.com/docs/install-claude-code?utm_source=github&utm_medium=readme&utm_campaign=b6_claude-api-card-declined).

**Anthropic Python SDK** (`pip install anthropic`):

```python
from anthropic import Anthropic

client = Anthropic(
    api_key="sk-teamo-xxxxxx",
    base_url="https://api.teamorouter.com",
)

msg = client.messages.create(
    model="claude-sonnet-5-5",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Hello, Claude"}],
)
print(msg.content[0].text)
```

**curl**:

```bash
curl https://api.teamorouter.com/v1/messages \
  -H "x-api-key: sk-teamo-xxxxxx" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{"model":"claude-sonnet-5-5","max_tokens":256,"messages":[{"role":"user","content":"Hello"}]}'
```

Prompt caching (`cache_control`) works on the same endpoint. The same key also calls GPT, Gemini, DeepSeek and 60+ other models through the OpenAI-compatible `/v1/chat/completions`.

## Prices

Per million tokens, input / output, from the [models page](https://teamorouter.com/models?utm_source=github&utm_medium=readme&utm_campaign=b6_claude-api-card-declined) on 2026-10-22 (they can change):

| Model | ID | TeamoRouter | Anthropic list price |
|---|---|---|---|
| Claude Opus 5.5 | `claude-opus-5-5` | $1.108 / $5.54 | $4 / $20 |
| Claude Sonnet 5.5 | `claude-sonnet-5-5` | $0.55 / $2.75 | $2 / $10 |

No subscription; you pay per token from your balance.

## FAQ

**Is it the real Claude?** You can check for yourself. TeamoRouter runs the same "pelican riding a bicycle" prompt against its Claude and GPT models every 30 minutes and publishes every result with a timestamp: [pelican benchmark](https://teamorouter.com/pelican-benchmark?utm_source=github&utm_medium=readme&utm_campaign=b6_claude-api-card-declined). Run the same prompt on your key and compare, tweaking a few words since some endpoints recognize well-known test prompts.

**Do I need an Anthropic account?** No. You need a TeamoRouter key.

**I only have USDT on TRON and want to top up less than $30.** Withdraw on BEP20 instead; TRC20 opens from $30.

**I'm paying with crypto on OpenRouter.** It takes USDC only. See [teamo-lab/openrouter-usdt](https://github.com/teamo-lab/openrouter-usdt).

## Related guides

- [teamo-lab/pay-openai-api-with-crypto](https://github.com/teamo-lab/pay-openai-api-with-crypto): OpenAI API and Codex without a card
- [teamo-lab/claude-api-usdt](https://github.com/teamo-lab/claude-api-usdt): 中文版
- [teamo-lab/pagar-claude-con-usdt](https://github.com/teamo-lab/pagar-claude-con-usdt): guía en español

Corrections welcome. Open an issue if something is out of date.
