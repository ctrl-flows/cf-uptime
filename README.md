# core1 uptime - the outside check (CF-277)

`core1-monitor` runs **on** core1. If core1 itself dies it sends nothing, and
silence looks exactly like everything being fine. This repo closes that hole:
GitHub's machines check core1 from outside every 15 minutes, and GitHub emails
the account owner when a run fails.

Checks: `/healthz` on `gateway.ctrlflows.com`, `n8n.ctrlflows.com` and
`n8n.rvrratingpartners.co.uk`, plus certificate expiry (fails under 10 days).

It holds **no secrets** and touches nothing - it only asks public addresses
whether they answer.

## Alerting

Failure mail comes from GitHub (Settings -> Notifications -> Actions). For a
phone alert, point a free monitor (UptimeRobot, Better Stack) at
`https://gateway.ctrlflows.com/healthz` as well - five minutes of signup, and
it gives you push notifications this does not.

## Why not 5 minutes

GitHub's scheduled runs drift by 5-15 minutes under load, so a tighter schedule
buys nothing. This catches "core1 is down"; the 5-minute monitor on core1 itself
catches everything finer.

_Signing test: this commit is signed with the Tyr1-Dev SSH signing key (CF-385)._

_Verified-signature check, 2026-09-19._
