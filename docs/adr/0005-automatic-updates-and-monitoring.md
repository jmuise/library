# 0005: Automatic updates and monitoring

## Status

Partly decided, partly deferred. None of this is configured yet.

## Context

A small project needs dependency updates and operational visibility without
much manual work. Open-source tools are preferred where they fit.

## Decision

Decided:

- Dependency updates: Renovate, with a 7-day minimum release age (cooldown)
  so freshly published releases are not adopted immediately.
- Metrics and logs: Grafana Cloud.
- Error tracking: speak the Sentry protocol, so either GlitchTip
  (open-source) or Sentry works as the backend.
- Web analytics: Cloudflare Web Analytics.
- Alerts: email, routed through a single fan-out point so that sources do not
  each hold their own recipient lists.

## Consequences

- Swapping the error-tracking backend needs only a DSN change.
- The cooldown trades update speed for protection against bad releases.
- Telemetry in self-hosted instances stays off by default (ADR 0003); this
  monitoring applies to the upstream instance.

## Deferred / Open

- Not yet wired: Renovate, Grafana Cloud, error tracking and analytics are
  all unconfigured.
- The fan-out implementation for email alerts is not chosen.
- The choice between GlitchTip and Sentry is open.
