# Simple Guide: Setting Up TradingView Watchlist Webhook Alerts

This guide shows you how to set up TradingView alerts for an entire watchlist and send the signals automatically to your webhook server.

---

## 1. Prerequisites

Before starting, make sure you have:
1. **A TradingView Paid Plan:** (Premium, Expert, or Ultimate). Watchlist Alerts is a TradingView-only feature for these tiers.
2. **Your Webhook Server URL:** (e.g., `http://your-server-ip:8000/webhook` or your public domain URL).
3. **Your Webhook Token:** The secret password configured in your [.env](file:///e:/Codes/Sand/.env) file under `WEBHOOK_TOKEN`.

---

## 2. Step-by-Step Configuration

### Step 1: Open your Watchlist
1. Open any chart on TradingView.
2. In the right-hand sidebar, open the **Watchlist** panel.
3. Choose the watchlist you want to automate.

### Step 2: Create the Watchlist Alert
1. Click the **three-dot menu (`...`)** next to your watchlist name.
2. Select **"Add alert on list..."** (or **"Create alert on list"**).
3. Define your alert condition (e.g., when price crosses a moving average or when a specific indicator triggers). This single alert will monitor every stock/crypto in your watchlist.

### Step 3: Set up the Webhook Notification
1. Switch to the **Notifications** tab inside the alert pop-up window.
2. Check the box for **Webhook URL**.
3. Paste your server URL: 
   ```text
   http://<your-server-ip>:8000/webhook
   ```
   *(Replace `<your-server-ip>` with your actual server IP or domain address).*

### Step 4: Add the Signal Message (JSON)
1. Go back to the **Settings** tab in the alert window.
2. Scroll to the **Message** box, delete any text inside, and paste the exact JSON template below:

```json
{
  "token": "YOUR_WEBHOOK_TOKEN",
  "ticker": "{{ticker}}",
  "timeframe": "{{interval}}",
  "side": "LONG",
  "entry_price": "{{close}}",
  "sl": "",
  "tp_main": ""
}
```
> **Important:** Replace `YOUR_WEBHOOK_TOKEN` with the secret token set in your [.env](file:///e:/Codes/Sand/.env) file (for example, the default is `change-me`).

3. Click **Create** to save your alert.

---

## 3. How the Message Templates Work

TradingView automatically replaces the bracketed values when the alert triggers:
- `{{ticker}}` is replaced by the symbol (e.g., `AAPL` or `BTCUSD`).
- `{{interval}}` is replaced by the timeframe of your chart (e.g., `5m` or `1D`).
- `{{close}}` is replaced by the current price when the alert occurred.

### To Send an Exit Alert (Sell/Close)
If you want to send a signal to close a trade, change the `"side"` value in your alert message:
```json
{
  "token": "YOUR_WEBHOOK_TOKEN",
  "ticker": "{{ticker}}",
  "timeframe": "{{interval}}",
  "side": "EXIT",
  "price": "{{close}}",
  "comment": "Take Profit hit"
}
```

---

## 4. Troubleshooting Checklist

- **No alert received?** Make sure the `"token"` in your message matches exactly what is in your server's [.env](file:///e:/Codes/Sand/.env) file.
- **No Telegram messages?** Check that your server is running and your `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` are set correctly in your [.env](file:///e:/Codes/Sand/.env) configuration.
- **Formatting errors?** Make sure the message box has no missing quotes or extra commas.
