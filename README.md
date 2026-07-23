# 🎵 Music Store SQL Data Analysis

A SQL case study on a Chinook-style digital music store database — answering real business questions across sales, customers, employees, and catalog data using **joins, aggregations, subqueries, CTEs, and window functions**.

## 📌 Overview

This project analyzes a relational music store database (11 tables, ~4,700+ invoice line items, 59 customers, 24 countries, 3,500+ tracks) to answer 11 business questions of increasing complexity — from simple aggregations to country-wise ranked analysis using `RANK() OVER (PARTITION BY ...)`.

## 🗂️ Dataset Schema

| Table | Description |
|---|---|
| `customer` | Customer details (name, location, support rep) |
| `employee` | Staff records, job titles, reporting hierarchy |
| `invoice` | Purchase invoices with billing location & total |
| `invoice_line` | Line items per invoice (track, price, quantity) |
| `track` | Song catalog (name, duration, genre, album) |
| `album2` | Albums linked to artists |
| `artist` | Artist names |
| `genre` | Music genres |
| `media_type` | File format types |
| `playlist` / `playlist_track` | Playlists and their tracks |

## 🛠️ Tools Used
- **SQL (MySQL)** — joins, subqueries, aggregate functions, CTEs, window functions (`RANK()`)

## ❓ Business Questions Answered

**Set 1 – Easy**
1. Who is the senior-most employee by job title?
2. Which countries have the most invoices?
3. What are the top 3 invoice totals?
4. Which city generated the most revenue (for a promotional Music Festival)?
5. Who is the best customer by total spend?

**Set 2 – Moderate**
6. Email, name, and genre of all Rock Music listeners (A-Z by email)
7. Top 10 rock artists by track count
8. Tracks longer than the average song length

**Set 3 – Advanced**
9. Total amount spent by each customer, broken down by artist
10. Most popular genre per country (ties included)
11. Top-spending customer per country (ties included)

## 💡 Key Insights

| Question | Answer |
|---|---|
| Senior-most employee | Mohan Madan — *Senior General Manager* (Level L7) |
| Country with most invoices | USA — 131 invoices |
| Top invoice value | $23.76 |
| Best city by revenue | **Prague** — $273.24 |
| Best customer overall | František Wichterlová — $144.54 |
| Top rock artist by track count | Led Zeppelin — 114 tracks |
| Total invoices in dataset | 614, across 24 countries |
| Total revenue analyzed | $4,709.43 |

## 🔍 Sample Query — Top Genre per Country (with ties)

```sql
WITH genre_purchases AS (
    SELECT
        i.billing_country,
        g.name AS genre,
        COUNT(*) AS purchases,
        RANK() OVER (
            PARTITION BY i.billing_country
            ORDER BY COUNT(*) DESC
        ) AS rnk
    FROM invoice i
    JOIN invoice_line il ON i.invoice_id = il.invoice_id
    JOIN track t ON il.track_id = t.track_id
    JOIN genre g ON t.genre_id = g.genre_id
    GROUP BY i.billing_country, g.name
)
SELECT * FROM genre_purchases WHERE rnk = 1;
```

## 📁 Repository Structure
```
├── music_store_data.sql        # All 11 queries with question comments
├── data/                       # Source CSV tables (customer, invoice, track, etc.)
└── README.md
```

## 🚀 Skills Demonstrated
`JOIN` (multi-table) · `GROUP BY` / `HAVING` · Correlated & scalar subqueries · `CTE`s · Window functions (`RANK`) · Business-question-to-SQL translation

---
*Part of my Data Analyst portfolio — built while preparing for entry-level Data Analyst roles.*
