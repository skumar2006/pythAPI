# 🎉 Your API is Now LIVE!

## 🌐 Public URL

Your API is publicly accessible at:

```
https://applaudable-unspeckled-kimberley.ngrok-free.dev
```

**Local URL:** `http://localhost:8080`

---

## ✅ Quick Test

### 1. Check if API is running

```bash
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/health
```

### 2. Get list of all available tokens

```bash
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/tokens
```

### 3. Get Bitcoin price

```bash
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/price/BTC/USD
```

### 4. Check if ETH is below $3000

```bash
curl -X POST https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/check \
  -H "Content-Type: application/json" \
  -d '{"symbol": "ETH/USD", "threshold": 3000}'
```

### 5. Start monitoring Bitcoin

```bash
curl -X POST https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/monitor/start \
  -H "Content-Type: application/json" \
  -d '{
    "symbol": "BTC/USD",
    "threshold": 50000,
    "update_interval": 10
  }'
```

Save the `session_id` from the response!

### 6. Check monitoring status

```bash
# Replace session_1 with your actual session_id
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/monitor/session_1
```

---

## 📋 All Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Health check |
| `/api/tokens` | GET | List all 20+ supported tokens |
| `/api/price/<symbol>` | GET | Get price for any token |
| `/api/check` | POST | Check token vs threshold |
| `/api/monitor/start` | POST | Start monitoring session |
| `/api/monitor/<session_id>` | GET | Get session status |
| `/api/monitor/<session_id>/stop` | POST | Stop monitoring |
| `/api/monitor/sessions` | GET | List all sessions |

---

## 💡 Example: Monitor Multiple Tokens

```bash
# Monitor BTC
curl -X POST https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/monitor/start \
  -H "Content-Type: application/json" \
  -d '{"symbol": "BTC/USD", "threshold": 50000}'

# Monitor ETH
curl -X POST https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/monitor/start \
  -H "Content-Type: application/json" \
  -d '{"symbol": "ETH/USD", "threshold": 3000}'

# Monitor SOL
curl -X POST https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/monitor/start \
  -H "Content-Type: application/json" \
  -d '{"symbol": "SOL/USD", "threshold": 100}'

# List all monitoring sessions
curl https://applaudable-unspeckled-kimberley.ngrok-free.dev/api/monitor/sessions
```

---

## 🐍 Python Example

```python
import requests

BASE_URL = "https://applaudable-unspeckled-kimberley.ngrok-free.dev"

# Get BTC price
response = requests.get(f"{BASE_URL}/api/price/BTC/USD")
print(response.json())

# Check if ETH is below $3000
response = requests.post(f"{BASE_URL}/api/check", json={
    "symbol": "ETH/USD",
    "threshold": 3000
})
result = response.json()
print(f"ETH below $3000: {result['is_below_threshold']}")

# Start monitoring
response = requests.post(f"{BASE_URL}/api/monitor/start", json={
    "symbol": "BTC/USD",
    "threshold": 50000
})
session_id = response.json()['session_id']
print(f"Session ID: {session_id}")

# Check status
response = requests.get(f"{BASE_URL}/api/monitor/{session_id}")
print(response.json())
```

---

## 🛑 Stopping the Services

To stop the server and ngrok:

```bash
# Stop Flask server
pkill -f "python3 app.py"

# Stop ngrok
pkill ngrok
```

---

## 🔄 Restarting

If you need to restart:

```bash
# Start Flask server
cd /Users/shivamkumar/pythAPI
nohup python3 app.py > /tmp/flask_output.log 2>&1 &

# Wait 3 seconds, then start ngrok
sleep 3
nohup ngrok http 8080 > /tmp/ngrok_output.log 2>&1 &

# Wait for ngrok to start, then get the URL
sleep 5
curl -s http://localhost:4040/api/tunnels | python3 -c "import sys, json; data = json.load(sys.stdin); print('Public URL:', data['tunnels'][0]['public_url'])"
```

---

## 📖 Documentation

**Full API documentation:** See `API_DOCUMENTATION.md`

---

## ✨ Supported Cryptocurrencies

- BTC (Bitcoin)
- ETH (Ethereum)
- SOL (Solana)
- BNB (Binance Coin)
- AVAX (Avalanche)
- MATIC (Polygon)
- ARB (Arbitrum)
- OP (Optimism)
- DOGE (Dogecoin)
- ADA (Cardano)
- DOT (Polkadot)
- LINK (Chainlink)
- UNI (Uniswap)
- ATOM (Cosmos)
- XRP (Ripple)
- LTC (Litecoin)
- APT (Aptos)
- SUI (Sui)
- TRX (Tron)
- NEAR (NEAR Protocol)

---

**Built with Pyth Network** 🔮

