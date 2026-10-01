# Silver Stacker Pre-Sale Portal (WordPress plugin)

This plugin is a private, approval-only pre-sale ordering portal for Silver Stacker LLC. It replaces OrderSpace.

It has two parts:

- **Customer portal** at `/portal/`: private catalog, live countdowns, ETAs, pre-orders without payment, order history, Build A Box consolidation, waitlists, invoices from QuickBooks, and addresses.
- **Staff console** in wp-admin under **Silver Stacker**: customer approvals, products and archive, orders, allocation and waitlist, the Pre-Sale Tracker, boxes and shipments, invoicing, reports, integrations, staff resources, settings and the audit log.

The package also includes a minimal theme, `silver-stacker`. The plugin owns all routing, access control and templates, so it works with any theme. The bundled theme is recommended because it adds nothing public.

## Requirements

- WordPress 6.4 or newer (tested on 6.9.9)
- PHP 8.1 or newer (tested on 8.4) with the `sodium`, `openssl`, `gd` (or `imagick`) and `mysqli` extensions
- MySQL 8 or MariaDB 10.6 or newer (InnoDB, FULLTEXT). Tested on MariaDB 10.11.
- HTTPS
- A real server cron
- An SMTP provider for email

## Install

1. Upload `silver-stacker-portal` to `wp-content/plugins/` and `silver-stacker` to `wp-content/themes/`.
2. In **Settings > Permalinks**, choose **Post name**.
3. Activate the theme, then activate the plugin. Activation creates the `wp_ss_*` tables, the roles, the private upload folder, the 8 sign-up questions, Terms v1 and the draft shipping tiers.
4. Follow **docs/DEPLOYMENT.md**. Do not skip the private uploads rule, the encryption key, cron or SMTP.
5. In **Silver Stacker > Settings**, review every item marked **Unconfirmed**. These are the open business decisions.
6. Connect **QuickBooks Online** and, optionally, **Google Sheets** under **Silver Stacker > Integrations**.

## Daily use

| Who | Where | What |
|---|---|---|
| Customer | `/register/` | Request access (3 steps, including the qualification questions) |
| Staff | Customers > Pending | Approve or reject. The customer is emailed automatically. |
| Customer | `/portal/` | Accept the Terms, then shop, Add to Order, and Submit Pre-Order (no payment) |
| Staff | Products | Create the product, its variants (SKU, price, allocation), images, closing date and ETA, then **Publish as Presale** |
| System | (automatic) | When the countdown reaches zero, ordering stops and the lines lock |
| Staff | Pre-Sale Tracker | Set **Qty available** as stock arrives. Partial availability is supported. |
| Staff | Invoices | Select available lines, then **Create invoice** in QuickBooks. Tax, email and payment are handled by QuickBooks. |
| Customer | My Box > **SHIP MY BOX** | Request one consolidated shipment |
| Staff | Boxes & Shipments | Choose the rate tier, enter carrier and tracking, then **Record shipment** |

## WP-CLI

```
wp ssp tick                 # close due presales, expire waitlist offers, run jobs
wp ssp jobs                 # run due background jobs
wp ssp reindex              # rebuild search index
wp ssp reconcile            # recompute allocation counters from order lines
wp ssp import_archive file.csv [--dry-run]
wp ssp seed_demo            # demo data (staging only)
wp ssp seed_archive --count=12000   # performance test data (staging only)
```

## Tests

See **docs/TESTING.md**. On staging, run:

```
wp eval-file wp-content/plugins/silver-stacker-portal/tests/acceptance.php
```

## Documentation

- `docs/ARCHITECTURE.md`: the analysis, data model, page map, roles and open decisions
- `docs/IMPLEMENTATION_NOTES.md`: what was built and where it differs from the plan
- `docs/DEPLOYMENT.md`: hosting, security hardening and the go-live checklist
- `docs/TESTING.md`: test suites and results
