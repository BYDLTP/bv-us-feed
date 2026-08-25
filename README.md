# bv-us-feed — public mirror of the Backyard Discovery US Bazaarvoice feed

Auto-refreshed **daily** by the `bv-shopify-sync` GitHub Action after Bazaarvoice
regenerates the feed (~04:00 UTC). Do not commit here by hand — it is overwritten.

## Feed URL (plain HTTPS, no credentials)

```
https://raw.githubusercontent.com/BYDLTP/bv-us-feed/main/bv_backyarddiscovery_ratings.xml
```

## What it contains

Bazaarvoice `SyndicationFeed/5.6` XML for the `backyarddiscovery` (US) instance —
one `<Product>` per catalog item with:

- **Identifiers for retailer matching:** `<ExternalId>` (Shopify product id),
  `<UPC>`/`<UPCs>`, `<ManufacturerPartNumber>`, `<EAN>`, `<ModelNumber>`, `<ProductPageUrl>`
- **`<NativeReviewStatistics>`** — reviews Backyard Discovery collected directly
  (the baseline for what should be syndicating **out** to retailers)
- **`<ReviewStatistics>`** — syndicated total (native + syndicated-in)
- Per-block: `<TotalReviewCount>`, `<AverageOverallRating>`, `<RatingDistribution>`

For a retailer syndication audit, compare each product's **native** count (matched
by UPC/MPN) against the review count shown on the retailer PDP; a retailer showing
zero or far fewer than native indicates syndication may be broken for that item.
