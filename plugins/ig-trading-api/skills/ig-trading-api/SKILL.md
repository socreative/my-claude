---
name: ig-trading-api
description: Expert knowledge of the IG Markets Trading API for automated trading and financial applications. Use this skill when the user asks to build trading bots, integrate with IG Markets, work with trading REST/Streaming APIs, manage positions and orders, or fetch market data.
---

# IG Trading API Expert

You are an expert in the IG Markets Trading API for automated trading, building trading integrations, and creating trading applications using REST and Streaming APIs.

## Overview

IG Labs provides REST and Streaming APIs for:
- Executing trades with risk management (stops and limits)
- Accessing real-time and historical market pricing
- Analyzing market sentiment data
- Managing account balances and transaction history
- Maintaining watchlists

**Supported Asset Classes:** Indices, Forex, Commodities, Options, Shares

## Base URLs

| Environment | Base URL |
|-------------|----------|
| **Live** | `https://api.ig.com/gateway/deal` |
| **Demo** | `https://demo-api.ig.com/gateway/deal` |

## Authentication

### Required Headers

All requests require these headers:

```
X-IG-API-KEY: your-api-key
Content-Type: application/json
Accept: application/json; charset=UTF-8
VERSION: 2
```

### Authentication Methods

#### Method 1: Session Tokens (v1/v2) - Recommended for most cases

```http
POST /session
Content-Type: application/json
X-IG-API-KEY: your-api-key
VERSION: 2

{
  "identifier": "your-username",
  "password": "your-password"
}
```

**Response Headers:**
- `CST`: Client session token
- `X-SECURITY-TOKEN`: Account security token

Both tokens are valid for 6 hours, extending up to 72 hours with active use.

**Subsequent Requests:**
```
X-IG-API-KEY: your-api-key
CST: client-session-token
X-SECURITY-TOKEN: security-token
```

#### Method 2: OAuth (v3) - For short-lived access

```http
POST /session
Content-Type: application/json
X-IG-API-KEY: your-api-key
VERSION: 3

{
  "identifier": "your-username",
  "password": "your-password"
}
```

**Response:**
```json
{
  "access_token": "702f6580-25c7-4c04-931d-6000efa824f8",
  "refresh_token": "a9cec2d7-fd01-4d16-a2dd-7427ef6a471d",
  "expires_in": "60"
}
```

**Subsequent Requests:**
```
Authorization: Bearer access-token
IG-ACCOUNT-ID: your-account-id
```

Access tokens expire in ~60 seconds. Refresh tokens expire ~10 minutes after access token expiration.

#### Token Refresh

```http
POST /session/refresh-token
X-IG-API-KEY: your-api-key

{
  "refresh_token": "your-refresh-token"
}
```

### Logout

```http
DELETE /session
```

## REST API Endpoints

### Account Management

#### List Accounts
```http
GET /accounts
```
Returns all accounts for the logged-in client.

#### Get/Update Preferences
```http
GET /accounts/preferences
PUT /accounts/preferences
```

#### Activity History
```http
GET /history/activity
GET /history/activity?from=2024-01-01T00:00:00&to=2024-01-31T23:59:59
```
Supports FIQL filtering: `?filter=status==ACCEPTED;channel==PUBLIC_WEB_API`

#### Transaction History
```http
GET /history/transactions
GET /history/transactions?type=ALL&from=2024-01-01&to=2024-01-31
```

### Positions

#### Get All Open Positions
```http
GET /positions
```

#### Create OTC Position
```http
POST /positions/otc
VERSION: 2

{
  "epic": "IX.D.FTSE.DAILY.IP",
  "direction": "BUY",
  "size": 1,
  "orderType": "MARKET",
  "currencyCode": "GBP",
  "forceOpen": true,
  "guaranteedStop": false,
  "stopLevel": null,
  "stopDistance": 50,
  "limitLevel": null,
  "limitDistance": 100
}
```

**Direction values:** `BUY`, `SELL`
**Order types:** `MARKET`, `LIMIT`, `QUOTE`

#### Update Position
```http
PUT /positions/otc/{dealId}

{
  "stopLevel": 7100,
  "limitLevel": 7500,
  "trailingStop": false
}
```

#### Close Position
```http
DELETE /positions/otc/{dealId}
_method: DELETE
```
Note: Some implementations require `_method: DELETE` header.

### Working Orders

#### Get All Working Orders
```http
GET /workingorders
```

#### Create Working Order
```http
POST /workingorders/otc

{
  "epic": "IX.D.FTSE.DAILY.IP",
  "direction": "BUY",
  "size": 1,
  "level": 7000,
  "type": "LIMIT",
  "currencyCode": "GBP",
  "goodTillDate": "2024-12-31T23:59:59",
  "guaranteedStop": false,
  "stopDistance": 50,
  "limitDistance": 100
}
```

**Order types:** `LIMIT`, `STOP`

#### Update/Delete Working Order
```http
PUT /workingorders/otc/{dealId}
DELETE /workingorders/otc/{dealId}
```

### Markets

#### Search Markets
```http
GET /markets?searchTerm=FTSE
```

#### Get Market Details
```http
GET /markets/{epic}
VERSION: 3
```

#### Get Multiple Markets
```http
GET /markets?epics=IX.D.FTSE.DAILY.IP,CS.D.GBPUSD.TODAY.IP
```

#### Market Categories
```http
GET /marketnavigation
GET /marketnavigation/{nodeId}
```

### Prices

#### Historical Prices
```http
GET /prices/{epic}?resolution=HOUR&max=100
GET /prices/{epic}/{resolution}/{numPoints}
GET /prices/{epic}/{resolution}/{startDate}/{endDate}
```

**Resolutions:** `SECOND`, `MINUTE`, `MINUTE_2`, `MINUTE_3`, `MINUTE_5`, `MINUTE_10`, `MINUTE_15`, `MINUTE_30`, `HOUR`, `HOUR_2`, `HOUR_3`, `HOUR_4`, `DAY`, `WEEK`, `MONTH`

**Response:**
```json
{
  "instrumentType": "INDICES",
  "prices": [
    {
      "snapshotTime": "2024/01/15 14:00:00",
      "openPrice": { "bid": 7500.0, "ask": 7501.0 },
      "highPrice": { "bid": 7520.0, "ask": 7521.0 },
      "lowPrice": { "bid": 7490.0, "ask": 7491.0 },
      "closePrice": { "bid": 7510.0, "ask": 7511.0 },
      "lastTradedVolume": 1000
    }
  ]
}
```

### Watchlists

#### List Watchlists
```http
GET /watchlists
```

#### Get Watchlist
```http
GET /watchlists/{watchlistId}
```

#### Create Watchlist
```http
POST /watchlists

{
  "name": "My Watchlist",
  "epics": ["IX.D.FTSE.DAILY.IP", "CS.D.GBPUSD.TODAY.IP"]
}
```

#### Add/Remove Items
```http
PUT /watchlists/{watchlistId}

{
  "epic": "IX.D.DAX.DAILY.IP"
}
```

```http
DELETE /watchlists/{watchlistId}/{epic}
```

### Client Sentiment

```http
GET /clientsentiment/{marketId}
GET /clientsentiment?marketIds=FTSE,DAX,GBPUSD
```

**Response:**
```json
{
  "marketId": "FTSE",
  "longPositionPercentage": 65.5,
  "shortPositionPercentage": 34.5
}
```

### Confirm Deal

After creating/modifying positions, confirm the deal status:
```http
GET /confirms/{dealReference}
```

**Deal Status values:** `ACCEPTED`, `REJECTED`

## Streaming API

The Streaming API uses Lightstreamer for real-time data.

### Connection Setup

```javascript
import { LightstreamerClient, Subscription } from 'lightstreamer-client-web';

// Get credentials from REST session
const session = await fetch('https://demo-api.ig.com/gateway/deal/session', {
  method: 'POST',
  headers: {
    'X-IG-API-KEY': apiKey,
    'Content-Type': 'application/json',
    'VERSION': '2'
  },
  body: JSON.stringify({ identifier: username, password: password })
});

const cst = session.headers.get('CST');
const xSecurityToken = session.headers.get('X-SECURITY-TOKEN');
const data = await session.json();
const lightstreamerEndpoint = data.lightstreamerEndpoint;

// Connect to Lightstreamer
const client = new LightstreamerClient(lightstreamerEndpoint);
client.connectionDetails.setUser(data.currentAccountId);
client.connectionDetails.setPassword(`CST-${cst}|XST-${xSecurityToken}`);

client.addListener({
  onStatusChange: (status) => console.log('Connection status:', status)
});

client.connect();
```

**Note:** Streaming API does NOT support OAuth tokens. Use CST/X-SECURITY-TOKEN.

### Subscriptions

**Subscription Modes:**
- `MERGE`: Consolidated updates (prices, account data)
- `DISTINCT`: Individual updates (trades)

**Limits:** Up to 40 simultaneous subscriptions per connection.

### Price Subscriptions

```javascript
const subscription = new Subscription(
  'MERGE',
  ['MARKET:IX.D.FTSE.DAILY.IP'],
  ['BID', 'OFFER', 'HIGH', 'LOW', 'MID_OPEN', 'CHANGE', 'CHANGE_PCT', 'UPDATE_TIME']
);

subscription.addListener({
  onItemUpdate: (update) => {
    console.log('Bid:', update.getValue('BID'));
    console.log('Offer:', update.getValue('OFFER'));
  }
});

client.subscribe(subscription);
```

### Account Subscriptions

```javascript
// Balance updates
const accountSub = new Subscription(
  'MERGE',
  ['ACCOUNT:XXXXX'],
  ['PNL', 'DEPOSIT', 'AVAILABLE_CASH', 'FUNDS', 'MARGIN', 'EQUITY']
);

// Trade confirmations
const tradeSub = new Subscription(
  'DISTINCT',
  ['TRADE:XXXXX'],
  ['CONFIRMS', 'OPU', 'WOU']
);
```

### Chart Subscriptions

```javascript
const chartSub = new Subscription(
  'MERGE',
  ['CHART:IX.D.FTSE.DAILY.IP:MINUTE'],
  ['BID_OPEN', 'BID_HIGH', 'BID_LOW', 'BID_CLOSE', 'OFR_OPEN', 'OFR_HIGH', 'OFR_LOW', 'OFR_CLOSE', 'CONS_END', 'UTM']
);
```

**Chart Scales:** `SECOND`, `1MINUTE`, `5MINUTE`, `HOUR`

### Unsubscribe

```javascript
client.unsubscribe(subscription);
```

### Disconnect

```javascript
client.disconnect();
```

## Code Examples

### Python - Basic Trading

```python
import requests

BASE_URL = 'https://demo-api.ig.com/gateway/deal'
API_KEY = 'your-api-key'

# Login
session = requests.Session()
session.headers.update({
    'X-IG-API-KEY': API_KEY,
    'Content-Type': 'application/json',
    'Accept': 'application/json',
    'VERSION': '2'
})

auth_response = session.post(f'{BASE_URL}/session', json={
    'identifier': 'your-username',
    'password': 'your-password'
})

session.headers.update({
    'CST': auth_response.headers['CST'],
    'X-SECURITY-TOKEN': auth_response.headers['X-SECURITY-TOKEN']
})

# Get open positions
positions = session.get(f'{BASE_URL}/positions').json()
print('Open positions:', positions)

# Get historical prices
prices = session.get(f'{BASE_URL}/prices/IX.D.FTSE.DAILY.IP?resolution=HOUR&max=24').json()
print('Prices:', prices)

# Place a trade
trade = session.post(f'{BASE_URL}/positions/otc', json={
    'epic': 'IX.D.FTSE.DAILY.IP',
    'direction': 'BUY',
    'size': 1,
    'orderType': 'MARKET',
    'currencyCode': 'GBP',
    'forceOpen': True,
    'guaranteedStop': False,
    'stopDistance': 50,
    'limitDistance': 100
}).json()
print('Trade reference:', trade['dealReference'])

# Confirm deal
confirm = session.get(f'{BASE_URL}/confirms/{trade["dealReference"]}').json()
print('Deal status:', confirm['dealStatus'])

# Logout
session.delete(f'{BASE_URL}/session')
```

### JavaScript/Node.js - With Streaming

```javascript
const fetch = require('node-fetch');
const { LightstreamerClient, Subscription } = require('lightstreamer-client-node');

const BASE_URL = 'https://demo-api.ig.com/gateway/deal';
const API_KEY = 'your-api-key';

async function main() {
  // Login
  const loginResponse = await fetch(`${BASE_URL}/session`, {
    method: 'POST',
    headers: {
      'X-IG-API-KEY': API_KEY,
      'Content-Type': 'application/json',
      'VERSION': '2'
    },
    body: JSON.stringify({
      identifier: 'your-username',
      password: 'your-password'
    })
  });

  const cst = loginResponse.headers.get('CST');
  const xSecurityToken = loginResponse.headers.get('X-SECURITY-TOKEN');
  const sessionData = await loginResponse.json();

  // Setup authenticated fetch
  const authFetch = (url, options = {}) => {
    return fetch(url, {
      ...options,
      headers: {
        'X-IG-API-KEY': API_KEY,
        'CST': cst,
        'X-SECURITY-TOKEN': xSecurityToken,
        'Content-Type': 'application/json',
        ...options.headers
      }
    });
  };

  // Get markets
  const markets = await authFetch(`${BASE_URL}/markets?searchTerm=FTSE`);
  console.log('Markets:', await markets.json());

  // Setup streaming
  const client = new LightstreamerClient(sessionData.lightstreamerEndpoint);
  client.connectionDetails.setUser(sessionData.currentAccountId);
  client.connectionDetails.setPassword(`CST-${cst}|XST-${xSecurityToken}`);

  client.addListener({
    onStatusChange: (status) => console.log('Stream status:', status)
  });

  client.connect();

  // Subscribe to prices
  const priceSub = new Subscription(
    'MERGE',
    ['MARKET:IX.D.FTSE.DAILY.IP'],
    ['BID', 'OFFER', 'UPDATE_TIME']
  );

  priceSub.addListener({
    onItemUpdate: (update) => {
      console.log(`${update.getValue('UPDATE_TIME')} - Bid: ${update.getValue('BID')}, Offer: ${update.getValue('OFFER')}`);
    }
  });

  client.subscribe(priceSub);
}

main().catch(console.error);
```

### Using trading-ig Python Library

```python
from trading_ig import IGService
from trading_ig.config import config

# Configure credentials via environment variables or config
# IG_SERVICE_USERNAME, IG_SERVICE_PASSWORD, IG_SERVICE_API_KEY, IG_SERVICE_ACC_TYPE

ig = IGService(
    config.username,
    config.password,
    config.api_key,
    config.acc_type  # 'DEMO' or 'LIVE'
)

ig.create_session()

# Get open positions
positions = ig.fetch_open_positions()
print(positions)

# Get historical prices
prices = ig.fetch_historical_prices_by_epic_and_num_points(
    epic='IX.D.FTSE.DAILY.IP',
    resolution='H',
    numpoints=100
)
print(prices)

# Close session
ig.logout()
```

## Error Handling

### Error Response Format

```json
{
  "errorCode": "error.public-api.exceeded-api-key-allowance"
}
```

### Common Error Codes

| Error Code | Description |
|------------|-------------|
| `error.security.invalid-credentials` | Invalid username/password |
| `error.security.invalid-api-key` | Invalid API key |
| `error.security.api-key-disabled` | API key has been disabled |
| `error.security.invalid-session` | Session has expired |
| `error.public-api.exceeded-api-key-allowance` | Rate limit exceeded |
| `error.request.invalid-date-range` | Invalid date range in request |
| `error.position.notional-value.limit-exceeded` | Position size too large |
| `error.market.closed` | Market is closed |
| `error.dealing.direction-not-allowed` | Cannot trade in this direction |

### Rate Limits

- ~40 trade requests per minute
- Standard API key quotas apply
- Exceeding limits returns `error.public-api.exceeded-api-key-allowance`

## Pagination

List endpoints return pagination metadata:

```json
{
  "metadata": {
    "paging": {
      "next": "/history/activity?version=3&from=2024-01-01&to=2024-01-31&pageSize=50&offset=50",
      "size": 50
    }
  }
}
```

## Date/Time Format

All date/time values are **ISO 8601 formatted** and in **UTC** timezone unless otherwise stated.

Examples:
- `2024-01-15T14:30:00`
- `2024/01/15 14:30:00` (in some price responses)

## API Versioning

Specify API version via `VERSION` header:

```
VERSION: 2
```

Different endpoints support different versions. Check documentation for version-specific features:
- Session: v1, v2, v3
- Positions: v1, v2
- Markets: v1, v2, v3, v4
- Prices: v1, v2, v3

## Best Practices

1. **Use Demo Account First** - Test all trading logic on demo before live
2. **Handle Token Expiry** - Implement token refresh logic for long-running applications
3. **Implement Retry Logic** - Handle transient failures with exponential backoff
4. **Confirm All Deals** - Always call `/confirms/{dealReference}` after trades
5. **Use Streaming for Real-time** - Don't poll REST API for live prices
6. **Respect Rate Limits** - Throttle requests to avoid being blocked
7. **Store Credentials Securely** - Never hardcode credentials; use environment variables
8. **Log All Trading Activity** - Maintain audit trail of all trades
9. **Implement Stop Losses** - Always use risk management on positions
10. **Monitor Connection Health** - Handle streaming disconnections gracefully

## Resources

- **IG Labs Portal**: https://labs.ig.com/
- **REST API Reference**: https://labs.ig.com/rest-trading-api-reference.html
- **REST API Guide**: https://labs.ig.com/rest-trading-api-guide.html
- **Streaming API Guide**: https://labs.ig.com/streaming-api-guide.html
- **Python Library (trading-ig)**: https://trading-ig.readthedocs.io/
- **API Key Generation**: My Account > Settings > API Keys (in IG web platform)

## Troubleshooting

### "CST missing" Error
- Ensure you're using correct credentials for the environment (DEMO vs LIVE)
- Check username, password, and API key are correct

### Connection Drops
- Streaming requires an active thread to maintain connection
- Implement reconnection logic with re-authentication if tokens expired

### "Market closed" Errors
- Check market trading hours
- Some instruments have limited trading windows

### Rate Limit Errors
- Implement request throttling
- Use streaming API instead of polling for real-time data
