# Troubleshooting Guide

When the pager goes off at 3 AM, refer to this directory. The information here is designed to be scannable, factual, and action-oriented.

## Incident Response Playbook

> [!CAUTION]
> If a system is completely offline and impacting external customers, immediately open a **Sev1 Incident** via PagerDuty and escalate to the Incident Commander.

### Escalation Matrix

1. **Level 1 (On-Call)**: Acknowledges the alert, begins investigation, and posts findings to the incident slack channel.
2. **Level 2 (SME)**: Brought in if the issue cannot be localized within 15 minutes.
3. **Level 3 (Leadership)**: Briefed if the outage duration is expected to exceed the SLA.

## Observability Hub

Where to look when something breaks:

*   **Logs**: [Elastic/Kibana Dashboard](#)
*   **Metrics**: [Grafana Main UI](#)
*   **Traces**: [Datadog APM](#)

## Common Runbooks

### High CPU Usage on Node Pool

1. Identify the cluster and node pool via Grafana.
2. Run `kubectl top nodes` and `kubectl top pods -A --sort-by=cpu`
3. If a specific deployment is consuming resources, check if a rogue loop is present in logs or if horizontal scaling limits (`HPA`) need to be adjusted.

### Database Connection Pool Exhausted

1. Validate current connections in the AWS RDS dashboard.
2. Verify if an application recently deployed is leaking connections or not using connection pooling (e.g. PgBouncer).
3. Temporary mitigation: Terminate idle sessions if absolute emergency, but prioritize rolling back the suspected offending deployment.
