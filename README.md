# Multi-Token Crypto Price Monitor API

A REST API for monitoring cryptocurrency prices using Pyth Network's decentralized oracle. Supports 20+ tokens including BTC, ETH, SOL, and more.

## Features

- ✅ **20+ Cryptocurrencies** - Monitor BTC, ETH, SOL, BNB, AVAX, MATIC, and more
- ✅ **Threshold Monitoring** - Get boolean results (True/False) when price crosses threshold
- ✅ **Real-time Prices** - Powered by Pyth Network's oracle
- ✅ **Multiple Endpoints** - Get prices, check thresholds, or continuous monitoring
- ✅ **Session Management** - Monitor multiple tokens simultaneously
- ✅ **Easy Deployment** - Deploy with ngrok in minutes

## Quick Start

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Start the Server

```bash
python app.py
```

Server will start on `http://localhost:5000`

### 3. Test the API

```bash
# In another terminal
python test_endpoints.py
```

### 4. Deploy with ngrok (Optional)

```bash
# In another terminal
ngrok http 5000
```

## API Examples

### Get Bitcoin Price

```bash
curl http://localhost:5000/api/price/BTC/USD
```

### Check if ETH is Below $3000

```bash
curl -X POST http://localhost:5000/api/check \
  -H "Content-Type: application/json" \
  -d '{"symbol": "ETH/USD", "threshold": 3000}'
```

**Response:**
```json
{
  "success": true,
  "symbol": "ETH/USD",
  "price": 2845.67,
  "threshold": 3000,
  "is_below_threshold": true,
  "result": true
}
```

### Start Continuous Monitoring

```bash
curl -X POST http://localhost:5000/api/monitor/start \
  -H "Content-Type: application/json" \
  -d '{
    "symbol": "BTC/USD",
    "threshold": 50000,
    "update_interval": 10
  }'
```

## Available Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Health check |
| `/api/tokens` | GET | List all supported tokens |
| `/api/price/<symbol>` | GET | Get current price for any token |
| `/api/check` | POST | Check price vs threshold (single) |
| `/api/monitor/start` | POST | Start monitoring session |
| `/api/monitor/<session_id>` | GET | Get session status |
| `/api/monitor/<session_id>/stop` | POST | Stop monitoring session |
| `/api/monitor/sessions` | GET | List all sessions |

## Supported Tokens

BTC, ETH, SOL, BNB, AVAX, MATIC, ARB, OP, DOGE, ADA, DOT, LINK, UNI, ATOM, XRP, LTC, APT, SUI, TRX, NEAR

*Get the complete list:*
```bash
curl http://localhost:5000/api/tokens
```

## Documentation

**Complete API documentation with examples:** See [API_DOCUMENTATION.md](API_DOCUMENTATION.md)

## Project Structure

```
pythAPI/
├── app.py                    # Flask REST API server
├── price_monitor.py          # Core price monitoring logic
├── requirements.txt          # Python dependencies
├── test_endpoints.py         # API test suite
├── API_DOCUMENTATION.md      # Complete API reference
└── README.md                 # This file
```

## Technologies

- **Flask** - Web framework
- **Pyth Network** - Decentralized oracle for real-time price data
- **Python 3.7+** - Programming language

## Use Cases

- Price alerts and notifications
- Trading bot data feeds
- Portfolio tracking
- Market analysis
- DeFi integrations

---

**Built with [Pyth Network](https://pyth.network) 🔮**
