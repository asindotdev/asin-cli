# Asin CLI

Official command-line interface for the [Asin](https://www.asin.dev) Amazon price tracking API.

Track Amazon prices and products with a scheduled watchlist, price history, offers and alerts.

Every documented endpoint is a command — **11 commands** — and `request` reaches any other path. JSON responses are pretty-printed; files (images, PDFs…) are saved with `-o`.

## Requirements

- Node.js 20 or newer
- a Asin [API key](https://www.asin.dev/app)

## Install

```sh
npm install --global @asindev/cli
asin --help
```

Or run it without installing:

```sh
npx @asindev/cli --help
```

## Configure your API key

The CLI reads the key from the environment; it does not load `.env` files.

```sh
export ASIN_API_KEY="YOUR_API_KEY"
```

PowerShell:

```powershell
$env:ASIN_API_KEY = "YOUR_API_KEY"
```

## Usage

```sh
asin search-amazon --query 'wireless earbuds'
```

Parameters use the names of the [API reference](https://www.asin.dev/docs) as flags: `--full-page` and `--full_page` are equivalent. Boolean flags can be bare (`--flag`) or explicit (`--flag=false`); array parameters accept commas or repeated flags.

```sh
asin search-amazon --help     # parameters of one command
asin endpoints                # every command and its HTTP path
asin endpoints search         # filter the list
```

### JSON parameters

```sh
echo '{"query": "wireless earbuds"}' | asin search-amazon --params -
asin search-amazon --params params.json --compact
```

Flags override values read with `--params`.

### Any endpoint

```sh
asin request get /account
asin request post /some/new/endpoint --params body.json
```

### Pipe into jq

```sh
asin search-amazon --query 'wireless earbuds' | jq .
```

## Exit codes

| Code | Meaning                                                                             |
| ---- | ----------------------------------------------------------------------------------- |
| `0`  | Success.                                                                            |
| `1`  | The API returned an error (details on standard error) or the network failed.        |
| `2`  | Invalid usage: unknown command or parameter, missing API key or required parameter. |

## Commands

| Command            | HTTP                        | Description                                                                                                              |
| ------------------ | --------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `check-product`    | POST `/products/{id}/check` | Asks for a check right away (1 credit), at most once per 5 minutes; earlier calls return the product unchanged. A pause… |
| `delete-product`   | DELETE `/products/{id}`     | Removes the product and its price history.                                                                               |
| `fetch-offers`     | POST `/offers`              | Reads the offers on Amazon for 1 credit, refunded if it doesn't answer. A read made in the last 3 hours, by anyone, is…  |
| `get-account`      | GET `/account`              | Your plan, the credits left (monthly allowance and packs), how many products you track and the credits they use per mon… |
| `get-asin-history` | GET `/history`              | Every price Asin read for an ASIN on a marketplace over the period, whoever tracks it, oldest first, with the lowest, h… |
| `get-offers`       | GET `/offers`               | Who sells the product and at what price, as last read, cheapest first, with Amazon's own price and the cheapest new and… |
| `get-product`      | GET `/products/{id}`        | The product's latest state and every price read of the period, oldest first. The history covers every read of this ASIN… |
| `list-products`    | GET `/products`             | Your watchlist, newest first, with each product's latest state and its recent prices (trend). Filter with asin (and mar… |
| `search-amazon`    | GET `/search`               | Amazon search results (up to 60, sponsored included), to find the ASIN of a product. Free, limited to 30 searches per h… |
| `track-product`    | POST `/products`            | Adds an ASIN, an Amazon link or a short link (amzn.to, a.co…) to your watchlist. The first check runs in the background… |
| `update-product`   | PATCH `/products/{id}`      | Target price, drop alerts, check interval, or pause. Send only what changes.                                             |

## Built on the SDK

The CLI uses the official [Node.js SDK](https://github.com/asindotdev/asin-node) (`@asindev/sdk`). Use the SDK directly for programmatic access.

## All Asin SDKs

| SDK        | Package                                                           | Install                                |
| ---------- | ----------------------------------------------------------------- | -------------------------------------- |
| Node.js    | [`@asindev/sdk`](https://www.npmjs.com/package/@asindev/sdk)      | `npm install @asindev/sdk`             |
| Python     | [`asindotdev`](https://pypi.org/project/asindotdev/)              | `pip install asindotdev`               |
| Go         | [`asin-go`](https://pkg.go.dev/github.com/asindotdev/asin-go)     | `go get github.com/asindotdev/asin-go` |
| .NET       | [`Asin`](https://www.nuget.org/packages/Asin)                     | `dotnet add package Asin`              |
| Ruby       | [`asindotdev`](https://rubygems.org/gems/asindotdev)              | `gem install asindotdev`               |
| PHP        | [`asindotdev/sdk`](https://packagist.org/packages/asindotdev/sdk) | `composer require asindotdev/sdk`      |
| Java       | [`asin-java`](https://github.com/asindotdev/asin-java)            | build from source, see its README      |
| CLI        | [`@asindev/cli`](https://www.npmjs.com/package/@asindev/cli)      | `npm install -g @asindev/cli`          |
| MCP server | [`@asindev/mcp`](https://www.npmjs.com/package/@asindev/mcp)      | `npx -y @asindev/mcp`                  |

## License

MIT — see [LICENSE](LICENSE).
