# Mybizz CS — Observability Requirements
# Phase 0 Implementation — TODO 3

## Overview

This document defines the observability requirements for the Mybizz CS platform.
Observability includes logging, metrics, and alerts to ensure system health and
troubleshooting capability.

## Logging Requirements

### Structured Logging

All server functions must use structured logging with the following fields:

**Entry/Exit Logging:**
```python
logger = get_logger()
logger.info("function_entry", function="get_vault_secret", secret_name=name)
# ... function logic ...
logger.info("function_exit", function="get_vault_secret", result_type=type(result).__name__)
```

**Error Logging:**
```python
logger.error("function_error", function="get_vault_secret", 
             error_type=type(e).__name__, error_message=str(e),
             secret_name=name, user_id=user_id)
```

### Log Levels

- **DEBUG:** Detailed debugging information (development only)
- **INFO:** General operation events (function entry/exit, successful operations)
- **WARNING:** Unexpected but handled conditions
- **ERROR:** Errors that don't prevent operation
- **CRITICAL:** Errors that prevent operation

### Log Format

All logs must be structured (JSON format) for easy parsing and aggregation:

```json
{
  "timestamp": "2026-05-26T10:30:00Z",
  "level": "ERROR",
  "function": "get_vault_secret",
  "secret_name": "stripe_secret_key",
  "error_type": "VaultSecretNotFoundError",
  "error_message": "Vault secret not configured",
  "user_id": "user_123",
  "request_id": "req_abc123"
}
```

## Metrics

### Key Performance Indicators (KPIs)

**Bookings:**
- `bookings_created_total` - Total number of bookings created
- `bookings_completed_total` - Total number of bookings completed
- `bookings_cancelled_total` - Total number of bookings cancelled
- `booking_creation_latency_ms` - Time from booking request to confirmation

**Payments:**
- `payments_processed_total` - Total number of payments processed
- `payments_successful_total` - Total number of successful payments
- `payments_failed_total` - Total number of failed payments
- `payment_processing_latency_ms` - Time from payment request to completion
- `payment_amount_total` - Total payment amount (sum)

**Email:**
- `emails_sent_total` - Total number of emails sent
- `emails_delivered_total` - Total number of emails delivered
- `emails_failed_total` - Total number of emails failed
- `email_open_rate` - Percentage of emails opened
- `email_click_rate` - Percentage of emails with clicks

**Errors:**
- `errors_total` - Total number of errors by type
- `errors_by_function` - Errors grouped by function
- `error_rate` - Errors per 1000 operations

### Custom Metrics

**Business Metrics:**
- `revenue_total` - Total revenue (sum of successful payments)
- `revenue_by_service` - Revenue grouped by service
- `revenue_by_period` - Revenue grouped by day/week/month
- `active_users` - Number of active users (last 30 days)
- `active_bookings` - Number of bookings in next 7 days

**System Metrics:**
- `api_request_latency_ms` - API request latency (p50, p95, p99)
- `api_request_rate` - Requests per second
- `database_query_latency_ms` - Database query latency
- `cache_hit_rate` - Cache hit rate for cached data

## Alerts

### Error Rate Alerts

**Critical:**
- Error rate > 5% for 5 minutes
- Critical errors in production

**Warning:**
- Error rate > 1% for 10 minutes
- Unusual error patterns

### Performance Alerts

**Critical:**
- API p99 latency > 5 seconds
- Database query p99 latency > 1 second
- Payment processing failure rate > 10%

**Warning:**
- API p95 latency > 2 seconds
- Database query p95 latency > 500ms
- Payment processing failure rate > 5%

### Availability Alerts

**Critical:**
- Service unavailable (health check fails)
- Database connection pool exhausted
- Payment gateway unreachable

**Warning:**
- High memory usage (> 80%)
- High CPU usage (> 80%)
- Disk space low (> 90%)

## Log Retention

**Production:**
- Detailed logs: 90 days
- Aggregated metrics: 2 years
- Audit logs: 7 years (compliance requirement)

**Staging:**
- Detailed logs: 30 days
- Aggregated metrics: 6 months

## Monitoring Dashboard

### Admin Dashboard

The admin dashboard should display:

**Real-time:**
- Current active users
- Active bookings (next 24 hours)
- Recent payments (last hour)
- Error rate (last 15 minutes)

**Daily:**
- Total bookings
- Total revenue
- New customers
- Email campaign performance

**Weekly:**
- Revenue trends
- Booking completion rate
- Customer retention rate
- Top services by revenue

## Implementation Notes

### Structured Logging Library

Use `structlog` for structured logging:

```python
import structlog

def setup_logging():
    structlog.configure(
        processors=[
            structlog.stdlib.filter_by_level,
            structlog.stdlib.add_logger_name,
            structlog.stdlib.add_log_level,
            structlog.stdlib.PositionalArgumentsFormatter(),
            structlog.processors.TimeStamper(fmt="iso"),
            structlog.processors.StackInfoRenderer(),
            structlog.processors.format_exc_info,
            structlog.processors.UnicodeDecoder(),
            structlog.processors.JSONRenderer()
        ],
        context_class=dict,
        logger_factory=structlog.stdlib.LoggerFactory(),
        wrapper_class=structlog.stdlib.BoundLogger,
        cache_logger_on_first_use=True,
    )

def get_logger():
    return structlog.get_logger()
```

### Metrics Collection

Use Prometheus client library for metrics:

```python
from prometheus_client import Counter, Histogram, Gauge

# Define metrics
BOOKINGS_CREATED = Counter(
    'bookings_created_total',
    'Total number of bookings created'
)

PAYMENT_LATENCY = Histogram(
    'payment_processing_latency_ms',
    'Payment processing latency in milliseconds',
    buckets=[100, 250, 500, 1000, 2500, 5000]
)

# Record metrics
BOOKINGS_CREATED.inc()
PAYMENT_LATENCY.observe(250)
```

### Alerting

Use Alertmanager or equivalent for alert routing:

```yaml
# alert_rules.yaml
groups:
  - name: mybizz_alerts
    rules:
      - alert: HighErrorRate
        expr: rate(errors_total[5m]) > 0.05
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "High error rate detected"
          description: "Error rate is {{ $value }}% for the last 5 minutes"
```

---

*End of file — Observability Requirements*
