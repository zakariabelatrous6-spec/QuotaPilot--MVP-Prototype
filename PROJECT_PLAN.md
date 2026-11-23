# QuotaPilot: beginner project guide

## What we are making

QuotaPilot is a website that demonstrates how to control an API credit balance. You can see usage, set simple rules, estimate when credits could run out, and test an explicitly approved PayPal Sandbox top-up.

The AI has one narrow job: suggest settings from a sentence. It cannot spend money, change the balance, activate a policy, or bypass the rules. Regular TypeScript code checks and enforces the limits.

## Six build stages

| Stage | What it means | Status |
|---|---|---|
| 1. Foundation | Website, database, starter account | Complete |
| 2. Metered API | Deduct credits safely and record API use | Complete |
| 3. AI policy engine | Turn written rules into reviewed settings; enforce them in code | Implemented; live Gemini setup needs a key |
| 4. Forecasting and decisions | Compare forecasts and explain top-up recommendations | Complete |
| 5. PayPal Sandbox | Test a user-approved payment and credit it once | Complete in code; needs your Sandbox keys for a live test |
| 6. Dashboard | Show balance, use, rules, forecast and payment history | Complete |

The six stages have an initial implementation. The Gemini and PayPal integrations need to be configured and demonstrated with your own credentials. This is an educational prototype, not a service ready for public users.

## What happens when you use it

1. A sample account starts with 100 credits and a starter policy saved as a draft.
2. You can run a demo request. QuotaPilot checks the active policy, balance and daily cap before it changes anything.
3. You can ask Gemini to draft a policy in everyday language. Review the numbers, save the draft, then activate it yourself.
4. The forecast compares three simple methods using recent hourly requests. It estimates runway; it cannot promise when credits will run out.
5. The deterministic decision engine can record “no action,” “recommend,” or “blocked,” with a plain-language reason. A recommendation never starts checkout.
6. If you choose, you can start a PayPal Sandbox checkout. The server checks the completed status, currency and amount before crediting the account. Repeating a confirmation cannot credit the same order twice.

## How to run it

Open the `quotapilot` project folder in Codex. In its terminal:

```bash
npm install
cp -n .env.example .env
npm run db:setup
npm run dev
```

Then visit [http://localhost:3000](http://localhost:3000). The database and demo request work without external credentials. Gemini policy drafting and PayPal Sandbox checkout need their own keys added to `.env`. Never paste those keys into chat or commit `.env`.

To check the work, run `npm run typecheck`, `npm run lint`, and `npm test`. Tests use `integration-test.db`, separate from your local `dev.db`.

## Beginner glossary

- **Website app:** the pages and controls you open in a browser.
- **API:** a set of addresses the website uses to ask the server to do work.
- **Endpoint:** one such address, such as `/api/consume` for using credits.
- **Database:** the local file that stores balances, rules, usage and transactions.
- **Validation:** checking that information is complete and in the allowed range.
- **Transaction:** a set of database changes that all succeed together or are rolled back together.
- **Forecast:** an estimate from past usage, not a guarantee.
- **MAE:** “mean absolute error”; the average size of the mistakes a forecast made on sample data. Lower is better for that sample.
- **Idempotency:** a safety check so repeating a payment confirmation has the same credit effect as processing it once.
- **Sandbox:** PayPal's test environment; test credentials and payments only.
- **Authentication:** checking who is using the app. QuotaPilot does not have this yet.

## What is still missing before a public release

This is a local educational demo, not a public payment service. It has no authentication, no separate user accounts, no real API-provider proxy, no PayPal webhook/reconciliation, no production database, and no deployment hardening. Those would need their own design and security work before real users or real payments.
