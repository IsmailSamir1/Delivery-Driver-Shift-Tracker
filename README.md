# 🚚 Delivery Driver Shift Tracker

A JavaScript-based shift management system for delivery drivers — handles clock-in/out tracking, idle vs active time calculation, quota enforcement, bonus tracking, and net pay computation with tier-based deductions. **Passed 26/26 test cases.**

## About

Built for a delivery company scenario where driver shift data lives in plain text files. The system reads, processes, and updates shift records and pay information through 10 core functions covering everything from time arithmetic to monthly salary calculations.

## Features

- **Shift Duration Calculation** — computes total time between clock-in and clock-out with 12-hour AM/PM format parsing
- **Idle vs Active Time Tracking** — separates productive delivery hours (8 AM – 10 PM) from idle time outside working hours
- **Quota Enforcement** — checks daily minimums (8h 24m standard, 6h during Eid period Apr 10–30)
- **Shift Record Management** — adds new entries to text files with duplicate detection, chronological ordering, and auto-calculated derived fields
- **Bonus System** — tracks and updates bonus flags per driver per date, with monthly bonus counting
- **Monthly Hours Aggregation** — sums active hours per driver per month
- **Required Hours Calculation** — factors in weekly days off, bonuses, and Eid period to compute expected monthly hours
- **Net Pay Computation** — calculates salary after deductions for missing hours, with tier-based allowance rates

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | JavaScript (Node.js) |
| File I/O | `fs` module (readFileSync / writeFileSync) |
| External Dependencies | None — built-in modules only |

## Project Structure

```
├── main.js                 # All 10 functions — core implementation
├── mainRunFileTesting.js   # Manual testing scratch pad
├── publicTests.js          # Public test suite (26 test cases)
├── shifts.txt              # Driver shift records (clock-in/out, duration, idle, active, quota, bonus)
├── driverRates.txt         # Driver pay tiers, weekly day off, monthly base salary
├── PublicTestFiles/        # Clean data file copies for test state reset
└── README.md
```

## How to Run

1. **Prerequisites** — Node.js installed
2. **Clone the repo**
   ```bash
   git clone https://github.com/IsmailSamir1/Delivery-Driver-Shift-Tracker.git
   cd Delivery-Driver-Shift-Tracker
   ```
3. **Run tests**
   ```bash
   node publicTests.js
   ```
4. **Manual testing**
   ```bash
   node mainRunFileTesting.js
   ```

## The 10 Functions

| # | Function | Description |
|---|----------|-------------|
| 1 | `getShiftDuration(startTime, endTime)` | Total time between clock-in and clock-out |
| 2 | `getIdleTime(startTime, endTime)` | Time outside delivery hours (before 8 AM / after 10 PM) |
| 3 | `getActiveTime(shiftDuration, idleTime)` | Productive hours = shift duration − idle time |
| 4 | `metQuota(date, activeTime)` | Checks if driver hit the daily minimum (8h 24m / 6h during Eid) |
| 5 | `addShiftRecord(textFile, shiftObj)` | Adds a shift entry with duplicate detection and auto-calculated fields |
| 6 | `setBonus(textFile, driverID, date, newValue)` | Updates the bonus flag for a specific driver and date |
| 7 | `countBonusPerMonth(textFile, driverID, month)` | Counts bonus entries for a driver in a given month |
| 8 | `getTotalActiveHoursPerMonth(textFile, driverID, month)` | Sums all active hours for a driver in a month |
| 9 | `getRequiredHoursPerMonth(textFile, rateFile, bonusCount, driverID, month)` | Expected monthly hours factoring in days off, bonuses, and Eid |
| 10 | `getNetPay(driverID, actualHours, requiredHours, rateFile)` | Net salary after tier-based deductions for missing hours |

## Data Format

**shifts.txt** — each line:
```
driverID | date | clockIn | clockOut | shiftDuration | idleTime | activeTime | metQuota | hasBonus
```

**driverRates.txt** — each line:
```
driverID | payTier | weeklyDayOff | monthlyBaseSalary
```

## What I Learned

- Parsing and manipulating time values in 12-hour AM/PM format without external libraries
- File I/O with Node.js `fs` module for persistent data management
- Handling edge cases in date/time arithmetic (midnight crossover, Eid period, day-off calculations)
- Building a complete business logic layer with interconnected functions
- Writing testable, modular JavaScript with zero external dependencies

## Context

Built during **Semester 4** (Spring 2026) at the **German International University**, Cairo — as a warm-up assignment for the Software Engineering course (CSEN406, Dr. John Zaki). Passed all 26 public test cases.

## License

This project was developed for educational purposes at GIU.
