# bv-us-feed — public mirror of the Backyard Discovery US Bazaarvoice feed

Auto-refreshed **daily** by the `bv-shopify-sync` GitHub Action after Bazaarvoice
regenerates the feed (~04:00 UTC). Do not commit here by hand — it is overwritten.

## URLs (plain HTTPS, no credentials)

Raw Bazaarvoice XML (`SyndicationFeed/5.6`, full fidelity):
```
https://raw.githubusercontent.com/BYDLTP/bv-us-feed/main/bv_backyarddiscovery_ratings.xml
```

Parsed catalog (compact JSON, one row per product — recommended for the audit tool):
```
https://raw.githubusercontent.com/BYDLTP/bv-us-feed/main/catalog.json
```

## catalog.json shape

```jsonc
{
  "source": "backyarddiscovery",
  "feed_extract_date": "…",   // when Bazaarvoice generated the feed
  "generated_at": "…",         // when this JSON was built
  "product_count": 363, "with_upc": 224, "with_mpn": 316,
  "products": [
    {
      "product_id": "11548414549",      // Shopify product id (= BV ExternalId)
      "title": "Woodridge Elite Swing Set",
      "url": "https://…/products/…",
      "category": "SWING SET - Wood",
      "upcs": ["752113861084", …],       // match keys to retailer PDPs
      "mpns": ["1801080"], "eans": [], "model_numbers": [],
      "native_count": 187,               // reviews BYD collected directly
      "native_avg": 4.6578,
      "syndicated_count": 190,           // native + syndicated-in
      "syndicated_avg": 4.6474,
      "removed": false
    }
  ]
}
```

## Using it for a retailer syndication audit

- Match each product to a retailer listing by **UPC** (224/363) or **MPN** (316/363);
  ~40 products have neither and need name/model matching.
- Compare the retailer PDP's review count against **`native_count`** (the reviews
  BYD syndicates out). A retailer showing zero or far fewer than `native_count`
  indicates syndication may be broken for that item. Use `syndicated_count` only
  as context — it is inflated by reviews syndicated *in* to BYD.
- Bazaarvoice's own **Content Syndication** report (Workbench) is the authoritative
  per-retailer status; this feed is the catalog + baseline for an independent check.
