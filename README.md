# Royalty-Program
# Store Loyalty (Web App)

Simple in-store loyalty program for Young's Market.  
Customers check in on a tablet by entering their phone number, and staff manage points and redemptions on a separate staff screen.

## Features

- Customers:
  - Check in with phone number on a kiosk tablet.
  - First-time customers can register with their name.

- Staff:
  - Search customers by phone.
  - View current points balance.
  - Record purchases (earn points).
  - Redeem points for discounts.
  - View transaction history per customer.
  - View recent check-ins list.

## Tech stack

- Node.js + Express (backend API)
- SQLite (local file database, no separate server)
- HTML + vanilla JavaScript (frontend, no heavy framework)

## Data model

**customers**

- id (integer, primary key)
- name (text, optional)
- phone (text, unique)
- points_balance (integer, default 0)
- created_at (timestamp)
- updated_at (timestamp)

**transactions**

- id (integer, primary key)
- customer_id (integer, FK -> customers.id)
- type (text: 'earn' or 'redeem')
- amount (real: purchase amount or discount value)
- points_change (integer: positive for earn, negative for redeem)
- created_at (timestamp)

**checkins**

- id (integer, primary key)
- customer_id (integer, FK -> customers.id, can be null)
- phone (text)
- created_at (timestamp)

## Points logic

- Earn: 1 point per 1 dollar (rounded down).
- Redeem: 100 points = 5 dollars discount.
- You can change these rules in `server.js`.

## Getting started

### 1. Install dependencies

```bash
git clone https://github.com/<your-user>/youngs-loyalty.git
cd youngs-loyalty
npm install
```

### 2. Run the server

```bash
node server.js
```

By default it runs on `http://localhost:3000`.

### 3. Open the app

- Staff/admin screen:  
  `http://localhost:3000/` (loads `public/index.html`)
- Customer kiosk screen:  
  `http://localhost:3000/kiosk.html`

Deploy `server.js` on a machine reachable by your store Wi‑Fi (for example a small PC or laptop).  
On the customer tablet, point the browser to:

```text
http://<your-server-ip>:3000/kiosk.html
```

## Kiosk setup (tablet)

For an Android tablet, you can:

- Set the browser home page to the kiosk URL.
- Use Android "screen pinning" or a kiosk mode app to lock the device into that browser and page.

iPad can be set up similarly with Guided Access.

## Staff flow

1. Customer enters phone on the kiosk tablet (check-in).
2. Staff opens the staff page on another device.
3. Staff searches the phone number, sees the customer record and points.
4. Staff:
   - Adds purchase to **earn** points, or
   - Uses **redeem** to apply points as discount.

## Development notes

- Database file: `loyalty.db` is created automatically on first run.
- Do not commit `loyalty.db` to Git.
- To reset data, stop the server, delete `loyalty.db`, and start again.

## License

MIT (or choose another license if you prefer).
