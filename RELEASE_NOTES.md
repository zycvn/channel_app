# Release Notes

## 0.0.159 (upgrading from 0.0.155)

Versions 0.0.156 - 0.0.159 are covered here. No breaking changes.
All new behavior is opt-in or additive.

### New
- **Operational events.** Every `create_error_report` call in the services
  (order, product, product stock, product price, product image, setup) is now
  followed by `create_operational_event`. New `OperationalEventDto`
  (`OperationalEventDto.from_error_report(report)`), and new commands
  `CreateOperationalEvent` and `GetOperationalEvents`
  (`channel_app.omnitron.commands.operational_events`).
- **Out-of-stock filter.** `ProductService.insert_products(...,
  filter_out_of_stock_products=False)`. When `True`, products with zero or
  empty stock are marked as failed and reported instead of being sent.
- **Bulk attribute commands.** `BulkCreateOrUpdateAttributes`,
  `BulkCreateOrUpdateCategoryAttributes`, `GetLeafCategoryNodeIds`, and
  `SetupService.bulk_create_or_update_attributes()`.
- **`OMNITRON_URL` environment variable** overrides the default
  `https://{MAIN_APP_URL}/`.
- `pyproject.toml` and `uv.lock` for `uv` based installs.
- Docs: flows, terminology and operational event command reference.

### Changed
- `SetupService.create_or_update_category_attributes(max_workers=8)` now
  fetches category attributes in parallel and only for **leaf** categories.
  It calls `bulk_create_or_update_category_attributes` instead of
  `create_or_update_category_attributes`.
- CI image moved to Python 3.11.9. `tox.ini` adds py310 and py311.

### Upgrade steps
1. Set `channel_app==0.0.159` in your channel project `requirements.txt`.
2. If you override `SetupService` or call the category attribute action key
   `create_or_update_category_attributes` directly, check it against the
   bulk flow above.
3. Optional: set `OMNITRON_URL` if you need to target a custom Omnitron host.
4. Optional: pass `filter_out_of_stock_products=True` to `insert_products`.
