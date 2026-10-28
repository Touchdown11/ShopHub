<div align="center">
  <img src="docs/home.png" alt="ShopHub storefront" width="820">
</div>

# ShopHub

A self-hosted e-commerce platform built with PHP, MySQL/MariaDB, vanilla JavaScript, and hand-written CSS. ShopHub includes a customer storefront and a separate staff administration portal, with no framework, package manager, or frontend build step.

[Storefront demo](https://shophubstore.freedev.app/shophub/) · [Staff portal demo](https://shophubstore.freedev.app/shophub-admin/)

**Demo accounts:** staff `admin@shophub.test` / `admin123` · customer `amara@example.com` / `password123`.

> Demo credentials are for the sample installation only. Change or remove seeded accounts before exposing any installation to the internet.

## Overview

| Application | Purpose | Local path |
|---|---|---|
| `shophub/` | Customer storefront, accounts, cart, checkout, and orders | `/shophub/` |
| `shophub-admin/` | Staff portal for products, orders, customers, reports, and settings | `/shophub-admin/` |

The applications share a database and business logic, but use separate sign-in pages and session cookies. The staff portal is not linked from the storefront.

## Features

- **Storefront:** searchable, filterable catalogue; product galleries and variants; reviews; cart and wishlist; coupon support; guest and member checkout; customer accounts and order history.
- **Administration:** product, category, order, customer, review, coupon, and message management; sales reports; CSV import/export; store settings.
- **Payments:** cash on delivery and bank transfer, plus optional Stripe Checkout and PayHere integrations.
- **Email:** verification, password resets, order updates, and staff notifications; supports log, PHP mail, and SMTP transports.
- **Security-focused foundations:** PDO prepared statements, CSRF checks, server-side checkout validation, stock checks in a transaction, hashed one-time account tokens, login throttling, and validated uploads.
- **Currencies:** configurable base currency and a storefront display-currency switch (USD/LKR by default).

## Screenshots

| Storefront | Product page | Staff dashboard |
|---|---|---|
| ![ShopHub storefront](docs/home.png) | ![Product detail and variants](docs/product.png) | ![Staff dashboard](docs/dashboard.png) |

| Cart | Checkout | Reports |
|---|---|---|
| ![Shopping cart](docs/cart.png) | ![Checkout](docs/checkout.png) | ![Sales reports](docs/reports.png) |

## Tech stack and requirements

- PHP **8.1+** (tested on PHP 8.4)
- MySQL/MariaDB for a regular installation, or SQLite for a local demo
- PHP extensions: `pdo_mysql` or `pdo_sqlite`, `mbstring`, `fileinfo`, and `json`; `curl` is used for Stripe integration
- Apache is supported; the included router scripts can be used with PHP's built-in server for local development
- No Composer, npm, or frontend build step

## Quick start: local SQLite demo

This starts both applications on PHP's built-in server. Run the commands from a POSIX-compatible shell:

```bash
git clone https://github.com/Touchdown11/ShopHub.git
cd ShopHub/shophub
SHOPHUB_DB_DRIVER=sqlite php install.php --fresh

cd ..
SHOPHUB_DB_DRIVER=sqlite php -S 127.0.0.1:8080 shophub/router.php
```

Open the storefront at <http://127.0.0.1:8080/shophub/> and the staff portal at <http://127.0.0.1:8080/shophub-admin/>. The installer seeds demo catalogue data and the sample accounts listed above. The `--fresh` option drops existing ShopHub tables before rebuilding them; do not use it with data you need to keep.

## MySQL/MariaDB installation

1. Install PHP 8.1+ with the extensions listed above and configure a MySQL/MariaDB database server.
2. Copy the `shophub/` and `shophub-admin/` directories side by side into your web server's document root.
3. Create an empty database named `shophub` using `utf8mb4`.
4. If the database credentials differ from the defaults, create `shophub/includes/config.local.php` with your local settings. This file is ignored by Git:

   ```php
   <?php
   return [
       'db' => [
           'driver' => 'mysql',
           'host' => '127.0.0.1',
           'port' => 3306,
           'name' => 'shophub',
           'user' => 'your_database_user',
           'pass' => 'your_database_password',
       ],
   ];
   ```

5. Visit `http://localhost/shophub/install.php` and confirm the installation. The installer creates the schema and loads sample data.
6. After verifying the installation, remove `shophub/install.php` from the web server and change or remove the seeded staff and customer accounts.

For a nested folder layout, migrations, deployment guidance, and more configuration details, see [`shophub/README.md`](shophub/README.md). Staff portal-specific notes are in [`shophub-admin/README.md`](shophub-admin/README.md).

## Configuration and deployment notes

- Keep database credentials and other local secrets in `config.local.php`; do not commit secrets to the repository.
- Configure email transport and payment gateway credentials in the staff portal's **Settings** page.
- Before a production deployment, disable debug mode, use HTTPS, configure a dedicated database user, set up real SMTP and payment credentials, and replace all demo data and passwords.
- The included Apache rules and PHP development routers help protect runtime storage and configuration files. Review the server configuration and upload permissions for your hosting environment.
- ShopHub is proprietary. Public source visibility does not grant permission to deploy or reuse it; see the license notice below and [`LICENSE`](LICENSE).

## Project documentation

- [Storefront, installation, maintenance, and troubleshooting guide](shophub/README.md)
- [Staff portal guide](shophub-admin/README.md)
- [Screenshots](docs/)

## Author, ownership, and license

ShopHub is authored and maintained by **Buddhika Lakshan** ([@Touchdown11](https://github.com/Touchdown11)).

Copyright &copy; 2026 Buddhika Lakshan. All rights reserved. ShopHub is proprietary software; no license is granted to copy, modify, distribute, deploy, or otherwise reuse it without prior written permission from the copyright holder. See [`LICENSE`](LICENSE) for the full notice.

