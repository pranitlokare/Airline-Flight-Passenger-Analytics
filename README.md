# Airline-Flight-Passenger-Analytics

## 🗂️ Database Schema

The database consists of **5 normalized tables**:
```
Airports ──────────────────────────┐
                                   ▼
Airlines ──────► Flights ◄────────┘
                    │
                    ▼
               Tickets ◄──── Passengers
```

| Table        | Description                                           |
|--------------|-------------------------------------------------------|
| `Airports`   | Airport name, location, and country                   |
| `Airlines`   | Airline name and country of origin                    |
| `Flights`    | Origin, destination, departure/arrival times, airline |
| `Passengers` | Passenger name, DOB, and frequent flyer status        |
| `Tickets`    | Links passengers to flights with ticket price         |

---

## 🌍 Sample Data Overview

- **5 Airports** — Delhi, Mumbai, London (Heathrow), New York (JFK), Dubai
- **5 Airlines** — Air India, British Airways, Emirates, Delta Airlines, IndiGo
- **16 Flights** — International and domestic routes across Aug 2025
- **25 Passengers** — With Gold / Silver / Platinum / None frequent flyer statuses
- **54 Tickets** — Prices ranging from ₹2,900 to ₹10,400

---

## 🔍 Analytical Queries

### Q1 — Busiest Airport by Departures
Finds which airport has the highest number of flights taking off, using `GROUP BY` + `COUNT`.

### Q2 — Total Tickets Sold Per Airline
Aggregates ticket sales across airlines by joining `Tickets → Flights → Airlines`.

### Q3 — All IndiGo Flights with Airport Names
Lists flights operated by IndiGo with human-readable origin and destination names via double `JOIN` on `Airports`.

### Q4 — Top Airline Per Departing Airport
Uses a **CTE + `ROW_NUMBER()` window function** to find the dominant airline at each airport.

### Q5 — Flight Duration & Category
Calculates flight duration in hours using `DATEDIFF` and classifies each flight as:
- 🟢 **Short** — under 2 hours
- 🟡 **Medium** — 2 to 5 hours
- 🔴 **Long** — over 5 hours

### Q6 — Passenger Flight History
Uses a **CTE** to summarize each passenger's first flight, last flight, and total number of flights taken.

### Q7 — Highest-Priced Ticket Per Route
Uses **`RANK()` window function** partitioned by `(Origin, Destination)` to find the most expensive ticket on each route.

### Q8 — Top Spender in Each Frequent Flyer Tier
Uses **nested CTEs + `RANK()`** to find the highest-spending passenger within each loyalty status group (Gold, Silver, Platinum, None).

---

## 🛠️ SQL Concepts Demonstrated

| Concept | Used In |
|---|---|
| `INNER JOIN` (multi-table) | Q2, Q3, Q4, Q7, Q8 |
| `GROUP BY` + `COUNT` / `SUM` | Q1, Q2, Q6, Q8 |
| `CASE` expression | Q5 |
| `DATEDIFF` | Q5 |
| Common Table Expressions (CTEs) | Q4, Q6, Q7, Q8 |
| `ROW_NUMBER()` window function | Q4, Q6 |
| `RANK()` window function | Q7, Q8 |
| `PARTITION BY` | Q4, Q7, Q8 |
| Self-join on same table | Q3, Q7 |

---



> ⚠️ The `DATEDIFF` function syntax is **SQL Server specific**. For MySQL, replace with
> `TIMESTAMPDIFF(MINUTE, DepartureTime, ArrivalTime)`. For PostgreSQL, use
> `EXTRACT(EPOCH FROM (ArrivalTime - DepartureTime)) / 60`.
