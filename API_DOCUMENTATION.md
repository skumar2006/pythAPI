# Multi-Token Price Monitor API Documentation

A REST API for monitoring cryptocurrency prices using Pyth Network's oracle. Supports 20+ tokens including BTC, ETH, SOL, and more.

## Base URL

**🌐 PUBLIC URL (Live Now):**
```
https://applaudable-unspeckled-kimberley.ngrok-free.dev
```

**Local URL:**
```
http://localhost:8080
```

---

## Available Tokens

The API supports the following cryptocurrency pairs:

- **BTC/USD** - Bitcoin
- **ETH/USD** - Ethereum
- **SOL/USD** - Solana
- **BNB/USD** - Binance Coin
- **AVAX/USD** - Avalanche
- **MATIC/USD** - Polygon
- **ARB/USD** - Arbitrum
- **OP/USD** - Optimism
- **DOGE/USD** - Dogecoin
- **ADA/USD** - Cardano
- **DOT/USD** - Polkadot
- **LINK/USD** - Chainlink
- **UNI/USD** - Uniswap
- **ATOM/USD** - Cosmos
- **XRP/USD** - Ripple
- **LTC/USD** - Litecoin
- **APT/USD** - Aptos
- **SUI/USD** - Sui
- **TRX/USD** - Tron
- **NEAR/USD** - NEAR Protocol

---

## API Endpoints

### 1. Health Check

Check if the API is running.

**Endpoint:** `GET /health`

**Response:**
```json
{
  "status": "healthy",
  "service": "crypto-price-monitor",
  "timestamp": "2025-11-23T10:15:23.123456"
}
```

**Example:**
```bash
# Public URL
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/health

# Or locally
curl http://localhost:8080/health
```

---

### 2. Get Available Tokens

Get a list of all supported cryptocurrency symbols.

**Endpoint:** `GET /api/tokens`

**Response:**
```json
{
  "success": true,
  "count": 20,
  "tokens": [
    "BTC/USD",
    "ETH/USD",
    "SOL/USD",
    "BNB/USD",
    ...
  ]
}
```

**Example:**
```bash
# Public URL
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/tokens

# Or locally
curl http://localhost:8080/api/tokens
```

---

### 3. Get Token Price

Get the current price for any supported token.

**Endpoint:** `GET /api/price/<symbol>`

**Path Parameters:**
- `symbol` (string, required): Token symbol (e.g., `BTC/USD`, `ETH/USD`)

**Response (Success):**
```json
{
  "success": true,
  "symbol": "BTC/USD",
  "price": 67234.56,
  "timestamp": "2025-11-23T10:15:23.123456"
}
```

**Response (Error):**
```json
{
  "success": false,
  "error": "Unsupported token: ABC/USD",
  "available_tokens": ["BTC/USD", "ETH/USD", ...]
}
```

**Examples:**
```bash
# Get Bitcoin price (Public URL)
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/price/BTC/USD

# Get Ethereum price (Public URL)
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/price/ETH/USD

# Get Solana price (Public URL)
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/price/SOL/USD
```

---

### 4. Check Price Against Threshold

Perform a single check to see if a token's price is below a threshold. Returns a boolean result.

**Endpoint:** `POST /api/check`

**Request Body:**
```json
{
  "symbol": "BTC/USD",
  "threshold": 50000
}
```

**Parameters:**
- `symbol` (string, required): Token symbol (e.g., `BTC/USD`)
- `threshold` (number, required): Price threshold in USD

**Response (Success):**
```json
{
  "success": true,
  "symbol": "BTC/USD",
  "price": 47234.56,
  "threshold": 50000,
  "is_below_threshold": true,
  "result": true,
  "timestamp": "2025-11-23T10:15:23.123456"
}
```

**Response (Error):**
```json
{
  "success": false,
  "error": "Both 'symbol' and 'threshold' are required"
}
```

**Examples:**
```bash
# Check if BTC is below $50,000 (Public URL)
curl -X POST https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/check \
  -H "Content-Type: application/json" \
  -d '{"symbol": "BTC/USD", "threshold": 50000}'

# Check if ETH is below $3,000 (Public URL)
curl -X POST https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/check \
  -H "Content-Type: application/json" \
  -d '{"symbol": "ETH/USD", "threshold": 3000}'

# Check if SOL is below $100 (Public URL)
curl -X POST https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/check \
  -H "Content-Type: application/json" \
  -d '{"symbol": "SOL/USD", "threshold": 100}'
```

**Python Example:**
```python
import requests

# Using public URL
BASE_URL = "https://applaudable-unspeckled-kimberley.ngrok-free.dev"

response = requests.post(f'{BASE_URL}/api/check', json={
    "symbol": "BTC/USD",
    "threshold": 50000
})

data = response.json()
if data['success']:
    print(f"BTC Price: ${data['price']:,.2f}")
    print(f"Below threshold: {data['is_below_threshold']}")
```

---

### 5. Start Monitoring Session

Start continuous monitoring of a token's price against a threshold. Returns updates every specified interval (default 10 seconds).

**Endpoint:** `POST /api/monitor/start`

**Request Body:**
```json
{
  "symbol": "BTC/USD",
  "threshold": 50000,
  "update_interval": 10
}
```

**Parameters:**
- `symbol` (string, required): Token symbol (e.g., `BTC/USD`)
- `threshold` (number, required): Price threshold in USD
- `update_interval` (number, optional): Seconds between updates (default: 10)

**Response (Success):**
```json
{
  "success": true,
  "message": "Started monitoring BTC/USD with threshold $50000.00",
  "session_id": "session_1",
  "symbol": "BTC/USD",
  "threshold": 50000,
  "update_interval": 10
}
```

**Examples:**
```bash
# Monitor BTC every 10 seconds (Public URL)
curl -X POST https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/monitor/start \
  -H "Content-Type: application/json" \
  -d '{
    "symbol": "BTC/USD",
    "threshold": 50000,
    "update_interval": 10
  }'

# Monitor ETH every 5 seconds (Public URL)
curl -X POST https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/monitor/start \
  -H "Content-Type: application/json" \
  -d '{
    "symbol": "ETH/USD",
    "threshold": 3000,
    "update_interval": 5
  }'
```

**Important:** Save the `session_id` from the response - you'll need it to check status or stop monitoring.

---

### 6. Get Monitoring Session Status

Get the current status and latest data from a monitoring session.

**Endpoint:** `GET /api/monitor/<session_id>`

**Path Parameters:**
- `session_id` (string, required): The session ID returned from `/api/monitor/start`

**Response (Success):**
```json
{
  "success": true,
  "session_id": "session_1",
  "is_running": true,
  "data": {
    "symbol": "BTC/USD",
    "price": 47234.56,
    "is_below_threshold": true,
    "threshold": 50000,
    "timestamp": "2025-11-23T10:15:23.123456",
    "status": "Monitoring"
  }
}
```

**Response (Not Found):**
```json
{
  "success": false,
  "error": "Session not found"
}
```

**Example:**
```bash
# Public URL
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/monitor/session_1
```

**Polling Example (check every 2 seconds):**
```bash
while true; do
  curl -s https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/monitor/session_1 | jq '.'
  sleep 2
done
```

---

### 7. Stop Monitoring Session

Stop a running monitoring session.

**Endpoint:** `POST /api/monitor/<session_id>/stop`

**Path Parameters:**
- `session_id` (string, required): The session ID to stop

**Response (Success):**
```json
{
  "success": true,
  "message": "Stopped monitoring session session_1"
}
```

**Example:**
```bash
# Public URL
curl -X POST https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/monitor/session_1/stop
```

---

### 8. List All Monitoring Sessions

Get a list of all monitoring sessions (active and stopped).

**Endpoint:** `GET /api/monitor/sessions`

**Response:**
```json
{
  "success": true,
  "count": 2,
  "sessions": [
    {
      "session_id": "session_1",
      "symbol": "BTC/USD",
      "threshold": 50000,
      "is_running": true,
      "status": "Monitoring"
    },
    {
      "session_id": "session_2",
      "symbol": "ETH/USD",
      "threshold": 3000,
      "is_running": false,
      "status": "Stopped"
    }
  ]
}
```

**Example:**
```bash
# Public URL
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/monitor/sessions
```

---

## Complete Usage Examples

### Example 1: Quick Price Check

```bash
# Get current Bitcoin price (Public URL)
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/price/BTC/USD

# Check if Bitcoin is below $50k (Public URL)
curl -X POST https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/check \
  -H "Content-Type: application/json" \
  -d '{"symbol": "BTC/USD", "threshold": 50000}'
```

### Example 2: Continuous Monitoring

```bash
# Using public URL
BASE_URL="https://applaudable-unspeckled-kimberley.ngrok-free.dev"

# Start monitoring
response=$(curl -s -X POST $BASE_URL/api/monitor/start \
  -H "Content-Type: application/json" \
  -d '{
    "symbol": "ETH/USD",
    "threshold": 3000,
    "update_interval": 10
  }')

# Extract session_id
session_id=$(echo $response | jq -r '.session_id')
echo "Session ID: $session_id"

# Check status periodically
while true; do
  curl -s $BASE_URL/api/monitor/$session_id | jq '.data'
  sleep 10
done

# Stop monitoring (run in another terminal)
curl -X POST $BASE_URL/api/monitor/$session_id/stop
```

### Example 3: Monitor Multiple Tokens

```bash
# Using public URL
BASE_URL="https://applaudable-unspeckled-kimberley.ngrok-free.dev"

# Start monitoring BTC
curl -X POST $BASE_URL/api/monitor/start \
  -H "Content-Type: application/json" \
  -d '{"symbol": "BTC/USD", "threshold": 50000}'

# Start monitoring ETH
curl -X POST $BASE_URL/api/monitor/start \
  -H "Content-Type: application/json" \
  -d '{"symbol": "ETH/USD", "threshold": 3000}'

# Start monitoring SOL
curl -X POST $BASE_URL/api/monitor/start \
  -H "Content-Type: application/json" \
  -d '{"symbol": "SOL/USD", "threshold": 100}'

# List all sessions
curl $BASE_URL/api/monitor/sessions
```

---

## Python Client Example

```python
import requests
import time

# Using the public URL
BASE_URL = "https://applaudable-unspeckled-kimberley.ngrok-free.dev"

# Get available tokens
response = requests.get(f"{BASE_URL}/api/tokens")
tokens = response.json()['tokens']
print(f"Available tokens: {tokens}")

# Get current price
response = requests.get(f"{BASE_URL}/api/price/BTC/USD")
price_data = response.json()
print(f"BTC Price: ${price_data['price']:,.2f}")

# Quick threshold check
response = requests.post(f"{BASE_URL}/api/check", json={
    "symbol": "ETH/USD",
    "threshold": 3000
})
result = response.json()
print(f"ETH below $3000: {result['is_below_threshold']}")

# Start monitoring
response = requests.post(f"{BASE_URL}/api/monitor/start", json={
    "symbol": "BTC/USD",
    "threshold": 50000,
    "update_interval": 10
})
session_id = response.json()['session_id']
print(f"Started monitoring, session: {session_id}")

# Poll for updates
try:
    while True:
        response = requests.get(f"{BASE_URL}/api/monitor/{session_id}")
        data = response.json()['data']
        
        if data['price']:
            print(f"[{data['timestamp']}] {data['symbol']}: "
                  f"${data['price']:,.2f} | Below threshold: {data['is_below_threshold']}")
        
        time.sleep(10)
except KeyboardInterrupt:
    # Stop monitoring
    requests.post(f"{BASE_URL}/api/monitor/{session_id}/stop")
    print("\nMonitoring stopped")
```

---

## JavaScript/Node.js Example

```javascript
const axios = require('axios');

// Using the public URL
const BASE_URL = 'https://applaudable-unspeckled-kimberley.ngrok-free.dev';

async function monitorPrice() {
  try {
    // Start monitoring
    const startResponse = await axios.post(`${BASE_URL}/api/monitor/start`, {
      symbol: 'BTC/USD',
      threshold: 50000,
      update_interval: 10
    });
    
    const sessionId = startResponse.data.session_id;
    console.log(`Started monitoring, session: ${sessionId}`);
    
    // Poll for updates
    const interval = setInterval(async () => {
      const statusResponse = await axios.get(`${BASE_URL}/api/monitor/${sessionId}`);
      const data = statusResponse.data.data;
      
      if (data.price) {
        console.log(`[${data.timestamp}] ${data.symbol}: $${data.price.toFixed(2)} | Below threshold: ${data.is_below_threshold}`);
      }
    }, 10000);
    
    // Stop after 60 seconds
    setTimeout(async () => {
      clearInterval(interval);
      await axios.post(`${BASE_URL}/api/monitor/${sessionId}/stop`);
      console.log('Monitoring stopped');
    }, 60000);
    
  } catch (error) {
    console.error('Error:', error.response?.data || error.message);
  }
}

monitorPrice();
```

---

## ✅ Already Deployed!

**Your API is currently LIVE at:**
```
https://applaudable-unspeckled-kimberley.ngrok-free.dev
```

All examples above use the public URL. You can use the API right now!

---

## Error Responses

All endpoints return errors in this format:

```json
{
  "success": false,
  "error": "Description of what went wrong"
}
```

Common HTTP status codes:
- `200` - Success
- `400` - Bad Request (invalid parameters)
- `404` - Not Found (session doesn't exist)
- `500` - Internal Server Error (failed to fetch price)

---

## Rate Limiting

The API fetches data from Pyth Network's public endpoints. To avoid rate limiting:

- Keep `update_interval` at 10 seconds or higher for continuous monitoring
- Limit the number of simultaneous monitoring sessions
- Use the `/api/check` endpoint for one-time checks instead of continuous monitoring when possible

---

## Tips

1. **Single Check vs Monitoring:** Use `/api/check` for one-time checks. Use `/api/monitor/start` for continuous monitoring.

2. **Session Management:** Each monitoring session runs independently. You can monitor multiple tokens simultaneously with different thresholds.

3. **Boolean Results:** All endpoints return `is_below_threshold` as a boolean - `true` when price < threshold, `false` when price >= threshold.

4. **Timestamps:** All timestamps are in ISO 8601 format (UTC).

5. **Supported Tokens:** Always check `/api/tokens` for the latest list of supported cryptocurrencies.

---

## Support

For issues or questions:
- Check Pyth Network status: https://pyth.network
- Review Pyth documentation: https://docs.pyth.network/home

---

**Built with Pyth Network Oracle** 🔮

