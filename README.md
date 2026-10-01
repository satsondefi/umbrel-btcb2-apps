# DATUM Gateway Btcb2 — Umbrel Community App

Umbrel App Store package that runs a BLAKE2b **DATUM Gateway** pre-configured for **[BTCB2](https://www.btcb2.com)**.

Verified against a live Umbrel install talking to `datum.btcb2.com:28915` (Connected and Ready, shares accepted, miners on LAN stratum `:23340`).

## What you get

| Item | Value |
| --- | --- |
| App name | **DATUM Gateway Btcb2** |
| App id | `btcb2-datum-gateway-btcb2` |
| Dashboard | Status, Config, Clients, Threads, Coinbaser |
| Admin login | Same as official DATUM: right-click app → **Show default credentials** → user `admin` |
| Stratum (miners) | `stratum+tcp://<umbrel-lan-ip>:23340` |
| Pool | `datum.btcb2.com:28915` (pubkey prefilled) |
| Node | Selected Bitcoin / Knots app via Umbrel dependency |

`api.modify_conf` is enabled and `admin_password` is set from Umbrel’s deterministic app password so Config saves persist and Clients / Threads / Coinbaser unlock after login.

## Add this Community App Store on Umbrel

1. Umbrel → **App Store** → ⋮ → **Community App Stores** → **Add**
2. Paste the Git URL of the repo that contains `deploy/umbrel-app-store/` **as the store root**, **or** publish only that folder as its own repo.

   If this monorepo is used as-is, the store root must be the directory that contains `umbrel-app-store.yml` and `btcb2-datum-gateway-btcb2/`:

   ```text
   …/deploy/umbrel-app-store
   ```

   Easiest path for others: push `deploy/umbrel-app-store` to a dedicated public repo (for example `btcb2/umbrel-apps`) and add that repo URL.

3. Install **DATUM Gateway Btcb2**
4. When prompted, select your **Bitcoin Knots** (BLAKE2b) node
5. Open the app → Config → set payout address → save
6. Right-click the icon → **Show default credentials** when you need Clients / Threads / Coinbaser / Config edits
7. Point ASICs at `stratum+tcp://<umbrel-ip>:23340`

## Miner settings

```text
URL:      stratum+tcp://192.168.x.x:23340
User:     <your BTCB2 payout address>[.worker]
Password: x
```

Do **not** point the ASIC at `datum.btcb2.com` directly — the gateway must sit in between.

## Coexist with other DATUM apps

| App | Typical stratum |
| --- | --- |
| Official DATUM | `:23334` |
| Other community BLAKE2b gateways | e.g. `:23336` / `:23339` |
| **DATUM Gateway Btcb2** | **`:23340`** |

## Package layout

```text
deploy/umbrel-app-store/
  umbrel-app-store.yml
  btcb2-datum-gateway-btcb2/
    umbrel-app.yml
    docker-compose.yml
    exports.sh
    hooks/pre-start
    data/settings/datum_gateway_config.json
    icon.png
    gallery/1.png
    gallery/2.png
    gallery/3.png
```

## Migrating from the Portainer / manual stack

If you already run the manual `docs/solominers/umbrel-btcb2-gateway` compose on `:23340` / `:21010`:

1. Stop that stack (free the ports)
2. Install this app from the community store
3. Set the same payout address in Config
4. Keep miners on `:23340`

## Credentials note

Username is always `admin` (DATUM hardcoded). Password comes from Umbrel (`deterministicPassword: true`) and is written into `api.admin_password` on first start while it is still the seeded placeholder `umbrel`.
