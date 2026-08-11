# Swiss Federal Office for Customs and Border Security Exchange Rate API client

Official **Swiss Federal Office for Customs and Border Security** (Switzerland) daily exchange rates in Node.js / TypeScript — 73 currencies against the CHF, with history back to 2000. Zero dependencies, works in Node 18+, Bun, Deno, and edge runtimes (uses global `fetch`).

These are the *published tax authority rates* required for tax filings, customs valuations, audits, and compliant invoicing — not moving market rates. Every response carries the publisher's own publication date.

Powered by [AllRatesToday](https://allratestoday.com/tax-authority-rates-api/bazg/). Get a free API key at [allratestoday.com/register](https://allratestoday.com/register) — no credit card required.

## Install

```bash
npm install bazg-exchange-rate
```

## Quick start

```js
import { getRate, getLatestRates } from 'bazg-exchange-rate';

// One pair at the official Swiss Federal Office for Customs and Border Security rate
const pair = await getRate('EUR', 'CHF', { apiKey: 'art_live_...' });
console.log(pair.rate, pair.rate_date); // e.g. EUR -> CHF on the bank's own date

// The bank's full published table
const table = await getLatestRates({ apiKey: 'art_live_...' });
console.log(table.rate_date, table.rates.length);
```

## Historical data (paid plans)

```js
import { getRatesForDate, getHistory } from 'bazg-exchange-rate';

// The official table for an invoice date — weekends/holidays return the
// most recent published date, flagged via published_on_requested_date.
const day = await getRatesForDate('2026-06-30', { apiKey: 'art_live_...' });

// Daily series for one pair
const series = await getHistory(
  { source: 'EUR', target: 'CHF', from: '2026-01-01' },
  { apiKey: 'art_live_...' }
);
```

## Currencies covered

Swiss Federal Office for Customs and Border Security currently publishes rates covering **74 currencies** (as of the latest table):

`AED` · `ALL` · `ARS` · `AUD` · `AZN` · `BAM` · `BDT` · `BGN` · `BHD` · `BRL` · `CAD` · `CHF` · `CLP` · `CNY` · `COP` · `CRC` · `CZK` · `DKK` · `DOP` · `DZD` · `EGP` · `ETB` · `EUR` · `GBP` · `GEL` · `GTQ` · `HKD` · `HNL` · `HUF` · `IDR` · `ILS` · `INR` · `ISK` · `JPY` · `KES` · `KHR` · `KRW` · `KWD` · `KYD` · `KZT` · `LBP` · `LKR` · `LYD` · `MAD` · `MUR` · `MXN` · `MYR` · `NGN` · `NOK` · `NZD` · `OMR` · `PAB` · `PEN` · `PHP` · `PKR` · `PLN` · `QAR` · `RON` · `RSD` · `RUB` · `SAR` · `SEK` · `SGD` · `THB` · `TND` · `TRY` · `TWD` · `TZS` · `UAH` · `USD` · `UYU` · `VES` · `VND` · `ZAR`

Pairs the tax authority does not print directly are resolved from this table (see below).

## Published vs derived rates

If Swiss Federal Office for Customs and Border Security does not print a pair directly, the API resolves it from the bank's table (inverse, or a cross rate via CHF) and flags it `derived: true` with the `method` — so official and computed values are never confused.

## Notes

- Every request counts toward your AllRatesToday monthly quota. Rates change once per business day — cache a day's table locally and a small quota goes a long way.
- Latest rates are on every plan (including free); historical dates and time series need a [paid plan](https://allratestoday.com/pricing/).
- Full API reference: [allratestoday.com/docs#central-bank](https://allratestoday.com/docs/#central-bank) · All covered sources: [tax authority rates API](https://allratestoday.com/tax-authority-rates-api/)

## License

MIT
