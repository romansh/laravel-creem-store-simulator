# Creem Store Simulator

Standalone Laravel application that simulates the subset of the [Creem](https://creem.io) payment API used by:

- [`romansh/laravel-creem`](https://github.com/romansh/laravel-creem)
- [`romansh/laravel-creem-cli`](https://github.com/romansh/laravel-creem-cli)
- [`romansh/laravel-creem-agent`](https://github.com/romansh/laravel-creem-agent)

The goal is deterministic local testing for agent demos and heartbeat logic.

## Installation

```bash
composer create-project romansh/laravel-creem-store-simulator
cd laravel-creem-store-simulator
cp .env.example .env
php artisan key:generate
php artisan migrate --seed
```

## Capabilities

- Creem-like HTTP endpoints under `/api/v1`
- Search endpoints such as `/subscriptions/search` and `/transactions/search`
- Multi-store resolution by `x-api-key`
- SQLite-backed products, customers, subscriptions, transactions, and checkouts
- Demo seeders and scenario commands
- Optional signed webhook delivery into an agent app
- FrankenPHP-friendly runtime

List responses mirror Creem pagination and include `items`, `pagination`, and a compatibility `total` alias.

## Quick start

```bash
cp .env.example .env
sqlite_db="${DB_DATABASE:-database/database.sqlite}" && mkdir -p "$(dirname "$sqlite_db")" && touch "$sqlite_db"
php artisan key:generate
php artisan migrate --seed
php artisan serve --host=0.0.0.0 --port=80
```

Point your agent app to the simulator:

```env
CREEM_TEST_MODE=true
CREEM_API_KEY=creem_test_demo_default
CREEM_WEBHOOK_SECRET=whsec_demo_default
CREEM_TEST_API_URL=http://simulator:80/api/v1
CREEM_API_URL=http://simulator:80/api/v1
```

## Main commands

- `php artisan simulator:seed-demo` — create a realistic baseline dataset
- `php artisan simulator:advance` — append fresh activity for the next heartbeat cycle
- `php artisan simulator:send-webhook payment.failed` — push a signed webhook to the configured agent app

## Docker

See the [laravel-creem-agent-demo](https://github.com/romansh/laravel-creem-agent-demo) repository for a
Docker Compose setup that runs this simulator alongside the agent app.

## Testing

```bash
php artisan test
```

## License

MIT
