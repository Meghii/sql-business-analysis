# Chinook Music Store — SQL Business Analysis

Answering real business questions against a relational database using SQL.

This project uses the **Chinook** database — a sample "digital music store"
with customers, invoices, tracks, artists and employees. I used SQL to answer
the kind of questions a store manager would actually ask, working across
multiple related tables with joins, groupings and aggregations.

## Dataset

- **Database:** Chinook (SQLite)
- **Tables used:** Customer, Invoice, InvoiceLine, Track, Album, Artist, Genre, Employee
- **Size:** 59 customers, 412 invoices, 2,240 invoice lines, 3,503 tracks

## Questions & findings

### 1. Which countries bring in the most revenue?

    SELECT BillingCountry, SUM(Total) AS revenue
    FROM Invoice
    GROUP BY BillingCountry
    ORDER BY revenue DESC;

**Finding:** The USA is the largest market ($523), ahead of Canada ($304),
France ($195) and Brazil ($190).

### 2. Who are the top customers by total spend?

    SELECT c.FirstName, c.LastName, c.Country, SUM(i.Total) AS total_spent
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    GROUP BY c.CustomerId
    ORDER BY total_spent DESC
    LIMIT 10;

**Finding:** The highest-spending customer is Helena Holý (Czech Republic, $49.62),
followed by Richard Cunningham (USA, $47.62).

### 3. What is the average invoice value by country?

    SELECT BillingCountry, ROUND(AVG(Total), 2) AS avg_invoice
    FROM Invoice
    GROUP BY BillingCountry
    ORDER BY avg_invoice DESC;

**Finding:** Customers in Chile and Ireland have the highest average invoice
value (about $6.6) — they spend more per order.

### 4. Which genres sell the most?

    SELECT g.Name AS genre, SUM(il.UnitPrice * il.Quantity) AS revenue
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    GROUP BY g.Name
    ORDER BY revenue DESC;

**Finding:** Rock dominates the store ($827), followed by Latin ($382) and
Metal ($261).

### 5. Which artists sell the most?

    SELECT ar.Name AS artist, SUM(il.UnitPrice * il.Quantity) AS revenue
    FROM InvoiceLine il
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Album al ON t.AlbumId = al.AlbumId
    JOIN Artist ar ON al.ArtistId = ar.ArtistId
    GROUP BY ar.Name
    ORDER BY revenue DESC
    LIMIT 10;

**Finding:** Iron Maiden is the top-selling artist ($139), ahead of U2 ($106)
and Metallica ($90).

### 6. How does revenue trend over time?

    SELECT strftime('%Y-%m', InvoiceDate) AS month, SUM(Total) AS revenue
    FROM Invoice
    GROUP BY month
    ORDER BY month;

**Finding:** Revenue is steady across 60 months (2021–2025), around $37–50 a
month — a stable, repeat-purchase customer base.

### 7. Which employee supports the most customers?

    SELECT e.FirstName || ' ' || e.LastName AS employee, COUNT(c.CustomerId) AS customers
    FROM Employee e
    JOIN Customer c ON c.SupportRepId = e.EmployeeId
    GROUP BY e.EmployeeId
    ORDER BY customers DESC;

**Finding:** Jane Peacock looks after the most customers (21), followed by
Margaret Park (20) and Steve Johnson (18).

### 8. Which tracks have never been purchased?

    SELECT t.Name
    FROM Track t
    LEFT JOIN InvoiceLine il ON t.TrackId = il.TrackId
    WHERE il.TrackId IS NULL;

**Finding:** 1,519 of the 3,503 tracks (43%) have never been purchased — a large
part of the catalogue earns nothing.

## Skills demonstrated

- Filtering, sorting and limiting (WHERE, ORDER BY, LIMIT)
- Aggregation (SUM, AVG, COUNT)
- Grouping (GROUP BY)
- Joining multiple tables (INNER JOIN, LEFT JOIN)
- Date handling (strftime)

## Tools

SQL (SQLite), Python, Pandas, Jupyter / Google Colab

## How to run

1. Open `chinook_analysis.ipynb` in Jupyter or upload it to Google Colab.
2. The notebook connects to `Chinook_Sqlite.sqlite` (included in this repo).
3. Run all cells.

---

Meghana J  [LinkedIn](https://www.linkedin.com/in/meghana-j-16904a21b)
