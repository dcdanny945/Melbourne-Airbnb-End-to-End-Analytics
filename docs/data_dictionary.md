# Data Dictionary — Melbourne Airbnb Analytics

**Source:** Inside Airbnb, Melbourne (September 2026 snapshot)  
**Analysis window:** 18 September – 16 December 2026 (90 days)

## 1. Silver & Gold Tables

| Table | Grain (one row per...) | Rows |
|---|---|---:|
| `silver_listings` | Listing | 25,715 |
| `silver_neighbourhoods` | LGA | 30 |
| `silver_calendar_90d` | Listing + date | 2,314,350 |
| `dim_neighbourhood` | LGA | 30 |
| `dim_listing` | Listing | 25,715 |
| `dim_date` | Date | 90 |
| `fact_availability` | Listing + date | 2,314,350 |

## 2. Key Fields

| Field | Definition |
|---|---|
| `listing_id` | Unique Airbnb listing identifier. |
| `neighbourhood_id` | Surrogate key linking listings to an LGA in Gold. |
| `lga` | Local Government Area name. |
| `lga_key` | Normalised LGA name used to match datasets. |
| `room_type` | Airbnb accommodation category. |
| `property_type` | Type of property listed. |
| `accommodates` | Maximum number of guests listed. |
| `listing_price_aud` | Price from the Listings source `price` field, in AUD. |
| `quote_price_aud` | Indicative *per-night* price from a captured booking quote, in AUD. |
| `has_valid_quote` | `1` if `quote_price_aud` is valid; otherwise `0`. |
| `quote_checkin_date` / `quote_checkout_date` | Dates associated with the captured price quote. |
| `snapshot_date` | Listing record's source `last_scraped` date. |
| `date` | Calendar date. |
| `date_key` | Gold date identifier in `YYYYMMDD` format. |
| `is_available` | `1` = advertised available; `0` = not advertised available on that date. |
| `minimum_nights` / `maximum_nights` | Listed minimum and maximum stay requirements for that date. |
| `is_weekend` | `1` for Saturday or Sunday; otherwise `0`. |
| `is_public_holiday` | `1` for a configured Victorian public holiday; otherwise `0`. |
| `public_holiday_name` | Name of the configured holiday, where applicable. |
| `is_weekend_or_holiday` | `1` for a weekend or configured public holiday; otherwise `0`. |

## 3. Dashboard Exports

| File | Grain | Rows |
|---|---|---:|
| `airbnb_listing_summary.csv` | Listing | 25,715 |
| `airbnb_daily_availability.csv` | Date + LGA + room type | 7,470 |

In the daily export:
- `available_listing_days` = sum of `is_available` within each group.
- `total_listing_days` = total Listing-Date records within each group.
- **Advertised availability rate** = `SUM(available_listing_days) / SUM(total_listing_days)`.

## 4. Important Limitations

- **Availability is not occupancy:** unavailable dates may be blocked, not booked.
- **Prices are indicative:** 19,004 listings (73.90%) have a valid captured quote; quotes are not actual transactions or a daily price series.
- **Single snapshot:** findings describe the September 2026 dataset, not live market conditions. Public holiday rules are configured for 2026.

For implementation details, see the notebooks in `notebooks/`.
