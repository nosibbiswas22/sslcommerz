<div align="center">

# SSLCommerz Next.js Example

A minimal Next.js App Router example for integrating SSLCommerz payments.

It demonstrates payment initiation, transaction status verification, callback
handling, and refund operations using the
[`sslcommerz`](https://www.npmjs.com/package/sslcommerz) package.

</div>

## Requirements

- Node.js 18.18 or later
- An SSLCommerz merchant account
- SSLCommerz sandbox credentials for local development

## Getting started

1. Install the dependencies:

   ```bash
   npm install
   ```

2. Create a local environment file:

   ```bash
   copy .env.example .env.local
   ```

   On macOS or Linux, use `cp .env.example .env.local` instead.

3. Set the values in `.env.local`:

   ```env
   SSLC_STORE_ID=your_store_id
   SSLC_STORE_PASSWORD=your_store_password
   NEXT_PUBLIC_APP_URL=http://localhost:3000
   ```

4. Start the development server:

   ```bash
   npm run dev
   ```

5. Open <http://localhost:3000> and select **Pay Now** to initiate a
   transaction.

SSLCommerz must be able to reach the callback URL from its servers. For local
testing, expose the development server with a tunneling service and set
`NEXT_PUBLIC_APP_URL` to the resulting public HTTPS URL.

## Environment variables

| Variable | Description |
| --- | --- |
| `SSLC_STORE_ID` | SSLCommerz store ID |
| `SSLC_STORE_PASSWORD` | SSLCommerz store password |
| `NEXT_PUBLIC_APP_URL` | Public base URL used to build payment callback URLs |

Never commit `.env.local` or real merchant credentials. The repository ignores
environment files by default; use `.env.example` as the safe template.

## API routes

| Method | Route | Purpose |
| --- | --- | --- |
| `POST` | `/api/sslcommerz/pre-transaction` | Creates a payment session and returns the SSLCommerz gateway URL |
| `POST` | `/api/sslcommerz/post-transaction?tran_id=...` | Handles the SSLCommerz callback and redirects to the success or failure page |
| `POST` | `/api/sslcommerz/verify-transaction` | Queries a transaction by `tran_id` |
| `POST` | `/api/sslcommerz/refund` | Initiates a refund or queries an existing refund |

### Example requests

Start a payment:

```powershell
Invoke-RestMethod -Method Post `
  -Uri http://localhost:3000/api/sslcommerz/pre-transaction `
  -ContentType "application/json" `
  -Body '{"amount":1000}'
```

Verify a transaction:

```powershell
Invoke-RestMethod -Method Post `
  -Uri http://localhost:3000/api/sslcommerz/verify-transaction `
  -ContentType "application/json" `
  -Body '{"tran_id":"your_transaction_id"}'
```

The example uses SSLCommerz sandbox mode. Review the integration before
switching to live credentials and live mode, and validate transaction amount,
currency, and status on the server before fulfilling an order.

## Available scripts

```bash
npm run dev      # Start the development server
npm run build    # Create a production build
npm run start    # Start the production server
npm run lint     # Run ESLint
```

## Project structure

```text
app/
├── api/sslcommerz/       # Payment API route handlers
├── admin/                # Refund and transaction administration UI
├── cancelled/            # Cancelled payment page
├── failed/               # Failed payment page
├── success/              # Successful payment page
└── page.js              # Example payment page
```

## License

This project is available under the [MIT License](./LICENSE).
