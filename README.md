# 🚚 Delivery Driver Shift Tracker

A Node.js module that manages delivery drivers' shift logs stored in plain-text CSV files. It parses clock-in/clock-out records, separates active delivery time from idle time, checks daily quotas (including a reduced Eid quota), tracks bonuses, and calculates each driver's **net monthly pay** after tier-based deductions.

Built for a Software Engineering course at the German International University (GIU), Cairo. All **26/26 public tests pass**.

## Features

- **Time parsing**: converts 12-hour `h:mm:ss am/pm` timestamps to seconds and back to `h:mm:ss` durations, with no date libraries
- **Idle vs. active time**: time outside the 8:00 AM to 10:00 PM delivery window counts as idle
- **Quota checks**: 8h 24m of active time on normal days, 6h during Eid (Apr 10–30, 2025)
- **Record management**: adds shifts with all derived fields computed, rejects duplicate driver/date entries, and updates bonus flags in place
- **Monthly reporting**: counts bonuses and totals active hours per driver per month
- **Required hours**: sums each day's quota, skips the driver's weekly day off, and subtracts 2 hours per earned bonus
- **Net pay**: each tier has an allowance for missing hours (Tier 1: 50h, 2: 20h, 3: 10h, 4: 3h). Every full hour beyond it is deducted at `basePay / 185`

## Tech Stack

| | |
|---|---|
| Language | JavaScript (Node.js) |
| Storage | Plain-text CSV (`fs` read/write) |
| Testing | Custom test runner (`publicTests.js`) |

## Project Structure

```
main.js                # All 10 functions + helpers (exported via module.exports)
publicTests.js         # 26 public test cases
mainRunFileTesting.js  # Manual scratch runner
shifts.txt             # Shift records (DriverID, Name, Date, Start, End, Duration, Idle, Active, MetQuota, HasBonus)
driverRates.txt        # DriverID, DayOff, BasePay, Tier
PublicTestFiles/       # Clean data copies used to reset state between tests
```

## Getting Started

```bash
git clone https://github.com/IsmailSamir1/Delivery-Driver-Shift-Tracker.git
cd Delivery-Driver-Shift-Tracker
node publicTests.js        # run the test suite
node mainRunFileTesting.js # try functions manually
```

Requires Node.js; there are no other dependencies.

## Example

```js
const t = require("./main");

t.getShiftDuration("7:30:00 am", "5:00:00 pm"); // "9:30:00"
t.getIdleTime("7:30:00 am", "5:00:00 pm");      // "0:30:00"  (before 8 AM)
t.metQuota("2025-04-15", "6:10:00");            // true       (Eid quota is 6h)
```

## Function Reference

| # | Function | Purpose |
|---|---|---|
| 1 | `getShiftDuration(start, end)` | Total shift length |
| 2 | `getIdleTime(start, end)` | Time outside 8 AM–10 PM |
| 3 | `getActiveTime(duration, idle)` | Duration minus idle |
| 4 | `metQuota(date, active)` | Daily quota check (Eid-aware) |
| 5 | `addShiftRecord(file, shift)` | Append record with derived fields; rejects duplicates |
| 6 | `setBonus(file, id, date, value)` | Update a record's bonus flag |
| 7 | `countBonusPerMonth(file, id, month)` | Bonuses earned in a month |
| 8 | `getTotalActiveHoursPerMonth(file, id, month)` | Sum of active time |
| 9 | `getRequiredHoursPerMonth(file, rates, bonus, id, month)` | Required hours after day-off and bonus adjustments |
| 10 | `getNetPay(id, actual, required, rates)` | Salary after tier-based deductions |

---
*Assignment template by the GIU course staff; the implementation in `main.js` is my own.*
