<h1 align="center">WhatsApp Receptionist</h1>

## Screenshots

### Booking flow
A client books a dental cleaning in natural language. The bot checks real-time availability, presents open slots, and confirms the appointment in Google Calendar.

![Booking flow](public/screenshots/booking-flow.png)

### Cancellation flow
The bot finds the existing appointment, checks the cancellation policy (24h rule), and cancels with no fee.

![Cancellation flow](public/screenshots/cancel-flow.png)

---

## How it works

```
Client sends WhatsApp message
        │
        ▼
┌─────────────────┐
│  FastAPI webhook │ ◄── validates HMAC signature
└────────┬────────┘
         │
         ▼
┌─────────────────┐     ┌──────────────┐
│   Claude AI     │ ◄───│  Knowledge   │
│  (conversation) │     │  base + config│
└────────┬────────┘     └──────────────┘
         │
         │ extracts structured intent
         ▼
   ┌─────┴──────┐
   │            │
   ▼            ▼
┌──────┐  ┌──────────┐
│ Book │  │ Cancel/  │
│      │  │ Modify   │
└──┬───┘  └────┬─────┘
   │           │
   ▼           ▼
┌─────────────────┐
│ Google Calendar  │ ◄── real-time availability check
└────────┬────────┘
         │
         ▼
   Confirmation via WhatsApp
```

---

## Quick start

### 1. Clone and install

```bash
git clone https://github.com/martin-minghetti/whatsapp-ai-receptionist.git
cd whatsapp-ai-receptionist
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 2. Configure

```bash
cp .env.example .env
# Edit .env with your API keys
```

### 3. Run

```bash
uvicorn core.main:app --reload
```

### 4. Expose for WhatsApp

Use [ngrok](https://ngrok.com/) for local development:

```bash
ngrok http 8000
```

Set the webhook URL in [Meta Developer Portal](https://developers.facebook.com/) -> WhatsApp -> Configuration:
- Callback URL: `https://your-ngrok-url.ngrok.io/webhook`
- Verify token: same as your `WHATSAPP_VERIFY_TOKEN`

## License

MIT
