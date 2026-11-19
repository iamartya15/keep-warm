# keep-warm

A tiny GitHub Actions backup that keeps the **PolyCompile Pro** web service awake on Render's free tier.

## Why this exists

Render free-tier services spin down after a period of inactivity, and the next visitor waits ~50 seconds for a cold start. PolyCompile Pro's primary keep-warm is a systemd timer on a separate machine that pings the health endpoint every few minutes. This repository is the **independent backup**: if that machine is ever offline, this workflow still keeps the service warm.

## How it works

- The workflow in [`.github/workflows/keep-warm.yml`](.github/workflows/keep-warm.yml) runs on a schedule.
- Each run sends repeated requests to the service's health endpoint over several minutes, which both wakes the service if it is asleep and keeps its activity timer refreshed.
- No secrets are stored in this repository; it only calls a public health endpoint.

## Related

- **PolyCompile Pro** — the service this keeps warm: [github.com/iamartya15/polycompile-pro](https://github.com/iamartya15/polycompile-pro)
- Live app: [polycompile-pro.onrender.com](https://polycompile-pro.onrender.com)
