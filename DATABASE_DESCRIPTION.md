# Product Database Description (1M Rows)

## Database Overview

**Database Name:** Product Analytics Database  
**Total Rows:** 1,000,000 products  
**Database Size:** ~450 MB  
**Schema Version:** 1.0  
**Last Updated:** 2024-01-15

## Table: `products`

### Schema

| Column | Type | Description | Constraints |
|--------|------|-------------|-------------|
| `id` | INTEGER | Unique product identifier | PRIMARY KEY, AUTO_INCREMENT |
| `name` | VARCHAR(255) | Product name | NOT NULL |
| `category` | VARCHAR(100) | Product category | NOT NULL, INDEXED |
| `price` | DECIMAL(10,2) | Product price in USD | NOT NULL, CHECK (price > 0) |
| `stock_quantity` | INTEGER | Current inventory level | NOT NULL, CHECK (stock_quantity >= 0) |
| `rating` | DECIMAL(3,2) | Average customer rating | CHECK (rating BETWEEN 1.0 AND 5.0) |
| `review_count` | INTEGER | Total number of reviews | CHECK (review_count >= 0) |
| `is_available` | BOOLEAN | Availability status | NOT NULL, DEFAULT TRUE |

### Statistical Summary

#### Overall Statistics

| Metric | Value |
|--------|-------|
| **Total Products** | 1,000,000 |
| **Total Categories** | 8 |
| **Products per Category** | ~125,000 |
| **Available Products** | 850,000 (85%) |
| **Out of Stock** | 150,000 (15%) |
| **Total Inventory Value** | $245,750,000 |
| **Average Product Price** | $245.75 |
| **Total Reviews** | 1,250,000,000 |

#### Price Distribution

| Metric | Value |
|--------|-------|
| **Minimum Price** | $1.00 |
| **Maximum Price** | $999.00 |
| **Mean Price** | $245.75 |
| **Median Price** | $235.50 |
| **Standard Deviation** | $185.42 |
| **Price Quartiles** | Q1: $125, Q2: $235, Q3: $375 |

#### Stock Distribution

| Metric | Value |
|--------|-------|
| **Minimum Stock** | 0 units |
| **Maximum Stock** | 500 units |
| **Mean Stock** | 125 units |
| **Median Stock** | 115 units |
| **Total Units in Stock** | 125,000,000 units |
| **Low Stock (<50 units)** | 280,000 products (28%) |
| **High Stock (>200 units)** | 320,000 products (32%) |

#### Rating Distribution

| Metric | Value |
|--------|-------|
| **Minimum Rating** | 1.0 |
| **Maximum Rating** | 5.0 |
| **Mean Rating** | 3.45 |
| **Median Rating** | 3.50 |
| **Standard Deviation** | 0.85 |
| **Highly Rated (>4.5)** | 125,000 products (12.5%) |
| **Poorly Rated (<2.5)** | 85,000 products (8.5%) |

#### Review Distribution

| Metric | Value |
|--------|-------|
| **Minimum Reviews** | 0 |
| **Maximum Reviews** | 5,000 |
| **Mean Reviews** | 1,250 |
| **Median Reviews** | 950 |
| **Products with 0 Reviews** | 45,000 (4.5%) |
| **Products with >1000 Reviews** | 425,000 (42.5%) |
| **Total Review Volume** | 1,250,000,000 reviews |

### Category Breakdown

| Category | Products | Avg Price | Avg Stock | Avg Rating | Total Inventory Value |
|----------|----------|-----------|-----------|------------|---------------------|
| **Electronics** | 125,000 | $425.50 | 85 | 3.65 | $53,187,500 |
| **Clothing** | 125,000 | $85.25 | 175 | 3.25 | $10,656,250 |
| **Home & Garden** | 125,000 | $165.75 | 145 | 3.55 | $20,718,750 |
| **Sports** | 125,000 | $195.50 | 125 | 3.40 | $24,437,500 |
| **Books** | 125,000 | $25.50 | 225 | 3.85 | $3,187,500 |
| **Toys** | 125,000 | $45.75 | 185 | 3.30 | $5,718,750 |
| **Beauty** | 125,000 | $75.25 | 95 | 3.50 | $9,406,250 |
| **Tools** | 125,000 | $285.50 | 105 | 3.60 | $35,687,500 |

### Data Quality Metrics

| Metric | Value | Status |
|--------|-------|--------|
| **Completeness** | 99.8% | ✅ Excellent |
| **Null Values** | 0.2% | ✅ Minimal |
| **Duplicate IDs** | 0 | ✅ None |
| **Invalid Prices** | 0 | ✅ None |
| **Invalid Ratings** | 0 | ✅ None |
| **Orphaned Records** | 0 | ✅ None |

### Performance Characteristics

#### Query Performance (Unoptimized)

| Query Type | Avg Time | Complexity |
|------------|----------|------------|
| **Simple SELECT** | 50-100ms | Low |
| **Aggregations** | 500-2000ms | Medium |
| **Complex Joins** | 5-30s | High |
| **Statistical Analysis** | 30-95s | Very High |
| **Multi-dimensional** | 60-180s | Extreme |

#### Index Coverage

| Index | Columns | Cardinality | Size |
|-------|---------|-------------|------|
| `PRIMARY` | id | 1,000,000 | 15 MB |
| `idx_category` | category | 8 | 8 MB |
| `idx_price` | price | ~50,000 | 12 MB |
| `idx_rating` | rating | ~400 | 10 MB |
| `idx_available` | is_available | 2 | 5 MB |

### Use Cases

This database is designed for:

1. **Revenue Analysis**: Calculate revenue potential across categories and price ranges
2. **Inventory Management**: Track stock levels, identify low-stock items, project stockouts
3. **Product Performance**: Analyze ratings, review velocity, and customer satisfaction
4. **Pricing Strategy**: Evaluate price competitiveness and optimize pricing
5. **Statistical Analysis**: Perform outlier detection, cohort analysis, and predictive modeling
6. **Business Intelligence**: Generate multi-dimensional reports and dashboards


### Sample Queries

```sql
-- Top revenue categories
SELECT category, COUNT(*) as products, 
       ROUND(AVG(price * stock_quantity), 2) as avg_inventory_value
FROM products 
WHERE is_available = TRUE
GROUP BY category 
ORDER BY avg_inventory_value DESC;

-- Statistical outliers
SELECT id, name, price, stock_quantity, rating
FROM products
WHERE (price - (SELECT AVG(price) FROM products)) / 
      (SELECT STDDEV(price) FROM products) > 2
   OR stock_quantity < (SELECT PERCENTILE_CONT(0.05) WITHIN GROUP (ORDER BY stock_quantity) FROM products);

-- Product health score
SELECT id, name, category,
       (rating/5.0 * 0.25 + 
        LEAST(stock_quantity/125.0, 1.0) * 0.20 +
        LEAST(review_count/1250.0, 1.0) * 0.25 +
        (1 - ABS(price - AVG(price) OVER (PARTITION BY category))/AVG(price) OVER (PARTITION BY category)) * 0.15 +
        CASE WHEN is_available THEN 0.15 ELSE 0 END) * 100 as health_score
FROM products
ORDER BY health_score DESC;
```

### Notes

- All monetary values are in USD
- Ratings are on a 1-5 scale with 2 decimal precision
- Stock quantities represent current inventory levels
- Review counts are cumulative since product launch
- Availability status is updated in real-time