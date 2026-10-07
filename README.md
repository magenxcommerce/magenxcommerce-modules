# magenxcommerce/magenxcommerce-modules

Composer metapackage that installs every Magenx Commerce companion Magento 2 module — the backend half of the
[magenxcommerce](https://github.com/magenxcommerce/magenxcommerce) headless storefront — with one `require` and keeps
them updatable with one `update`.

It contains no code: only a `composer.json` of type `metapackage` whose `require` lists the modules.

## Install

```bash
composer require magenxcommerce/magenxcommerce-modules
bin/magento setup:upgrade
```

## Update

```bash
composer update 'magenxcommerce/*'
bin/magento setup:upgrade
```

Every module is required with a caret constraint (`^x.y`), so minor and patch releases of any module are picked up by
`composer update` without a new release of this package.

## Modules

| Package | Adds |
|---------|------|
| [`module-abandoned-checkout`](https://github.com/magenxcommerce/module-abandoned-checkout) | Cron-driven abandoned-cart reminder emails with a signed, expiring recovery link |
| [`module-admin-activity`](https://github.com/magenxcommerce/module-admin-activity) | Admin action audit log with field-level before/after values, login activity, IP and user agent |
| [`module-ai-mcp`](https://github.com/magenxcommerce/module-ai-mcp) | MCP endpoint so an AI agent can inspect and manage the backend, authorized by an Integration's ACL role |
| [`module-auto-product-links`](https://github.com/magenxcommerce/module-auto-product-links) | Rule-driven Related / Up-Sell / Cross-Sell links plus Frequently Bought Together from order history |
| [`module-best-seller-graph-ql`](https://github.com/magenxcommerce/module-best-seller-graph-ql) | `bestSellers` query and per-category PDP badge |
| [`module-blog`](https://github.com/magenxcommerce/module-blog) | Headless blog: posts, categories, tags, related posts and products (admin authoring) |
| [`module-blog-graph-ql`](https://github.com/magenxcommerce/module-blog-graph-ql) | GraphQL surface of `module-blog` |
| [`module-contact-confirmation`](https://github.com/magenxcommerce/module-contact-confirmation) | Contact form confirmation |
| [`module-deal-graph-ql`](https://github.com/magenxcommerce/module-deal-graph-ql) | `deals` query and `ProductInterface.deal`, driven by labelled Catalog Price Rules |
| [`module-garan-graph-ql`](https://github.com/magenxcommerce/module-garan-graph-ql) | EU legal-guarantee notice and GARAN durability label (Regulation (EU) 2025/1960) |
| [`module-gdpr`](https://github.com/magenxcommerce/module-gdpr) | Cookie registry, consent logging and data-subject request tracking |
| [`module-gdpr-graph-ql`](https://github.com/magenxcommerce/module-gdpr-graph-ql) | GraphQL surface of `module-gdpr` |
| [`module-graph-ql-timing`](https://github.com/magenxcommerce/module-graph-ql-timing) | `Server-Timing` header on the GraphQL controller |
| [`module-helpdesk`](https://github.com/magenxcommerce/module-helpdesk) | Support-ticket desk with admin agent workspace and IMAP email gateway |
| [`module-helpdesk-graph-ql`](https://github.com/magenxcommerce/module-helpdesk-graph-ql) | GraphQL surface of `module-helpdesk` |
| [`module-platform`](https://github.com/magenxcommerce/module-platform) | Read-only stack dashboard under **System > Tools > Platform Overview** |
| [`module-price-history-graph-ql`](https://github.com/magenxcommerce/module-price-history-graph-ql) | `ProductInterface.price_history` — EU Omnibus prior price |
| [`module-product-alert-graph-ql`](https://github.com/magenxcommerce/module-product-alert-graph-ql) | Price-drop / back-in-stock subscriptions over GraphQL |
| [`module-product-attachments`](https://github.com/magenxcommerce/module-product-attachments) | Per-product file attachments on the PDP and in sales emails |
| [`module-product-feed`](https://github.com/magenxcommerce/module-product-feed) | Scheduled product feeds (XML/CSV/TSV/JSONL) delivered by URL, FTP/SFTP or merchant APIs |
| [`module-quick-search-graph-ql`](https://github.com/magenxcommerce/module-quick-search-graph-ql) | `quickSearchSuggestions`: popular terms and sponsored products, categories and brands |
| [`module-reward`](https://github.com/magenxcommerce/module-reward) | Store-credit reward-points program with an earn/redeem ledger |
| [`module-reward-graph-ql`](https://github.com/magenxcommerce/module-reward-graph-ql) | GraphQL surface of `module-reward` |
| [`module-rma`](https://github.com/magenxcommerce/module-rma) | Return Merchandise Authorization — requests, admin UI, REST |
| [`module-rma-graph-ql`](https://github.com/magenxcommerce/module-rma-graph-ql) | GraphQL surface of `module-rma` |
| [`module-sitemap`](https://github.com/magenxcommerce/module-sitemap) | Rewrites XML sitemap URLs into the headless storefront's route shape |
| [`module-social-login-graph-ql`](https://github.com/magenxcommerce/module-social-login-graph-ql) | `socialLogin` mutation verifying Google / Apple ID tokens |
| [`module-store-pickup-graph-ql`](https://github.com/magenxcommerce/module-store-pickup-graph-ql) | In-store pickup carrier and store locations query |

All packages are published under the `magenxcommerce/` vendor; the table drops the prefix.

## Leaving a module out

A metapackage installs everything it requires. To skip one, tell Composer it is already provided, in the project's
root `composer.json`:

```json
"replace": {
    "magenxcommerce/module-product-feed": "*"
}
```

Modules that depend on a replaced one will then fail at runtime — `module-ai-mcp`, for example, requires
`module-admin-activity`, `module-blog`, `module-gdpr`, `module-helpdesk`, `module-platform`, `module-product-feed`,
`module-quick-search-graph-ql` and `module-rma`; `*-graph-ql` halves such as `module-rma-graph-ql` require
their backend module.

## Releasing

Versions come from git tags (`vX.Y.Z`); `composer.json` carries no `version` field.

- **Module minor / patch release** — nothing to do here; the caret constraint already allows it.
- **New module** — add it to `require` as `^<major>.<minor>` of its first release, add a table row, tag a new
  **minor** version of this package.
- **Module major release** — raise its constraint to the new major, tag a new **major** version of this package.
- **Module removed** — drop it from `require`, tag a new **major** version of this package.
