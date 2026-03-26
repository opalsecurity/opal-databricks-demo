# Opal + Databricks Notebooks

> Extend Opal's identity governance capabilities with advanced analytics, AI, and large-scale data enrichment using Databricks.

[![Opal Security](https://img.shields.io/badge/Opal-Security-blue)](https://opal.dev)
[![Databricks](https://img.shields.io/badge/Databricks-Partner-red)](https://databricks.com)

---

## Overview

Identity and access management data is one of the richest, most underutilized assets inside modern enterprises. It tells the story of *who* has access to *what*, *why*, and *how that access evolves over time* — but too often, it's difficult to bring the right business context into security and access workflows.

This accelerator provides a set of Databricks Notebooks that make it easy to load, model, and extend Opal data for deeper insights and custom analytics. Your Opal exports are transformed into Delta tables and unified datasets — ready for queries, dashboards, and machine learning pipelines.

**What you can do with this accelerator:**

- Cluster and correlate identities across business units and systems
- Detect anomalies in access requests, ownership patterns, or privilege drift
- Optimize licensing and entitlements through usage-based modeling
- Automate governance by feeding analytic outputs (risk scores, ML predictions) back into Opal workflows
- Unify access data with HR, security, and cost data for holistic governance dashboards

---

## Architecture

```
Opal (source of truth)
        │
        │  API / exports
        ▼
Databricks Notebooks
  ├── Ingest → Delta tables (opal_users, opal_groups, opal_events, opal_owners)
  ├── Analyze → Genie spaces, dashboards, custom queries
  └── Act → Write recommendations back to Opal API
```

---

## Notebooks

| Notebook | Format | Description |
|----------|--------|-------------|
| `opal_functions` | `.dbc` / `.ipynb` | Shared helper functions for authenticating, parsing, joining, and visualizing Opal data |
| `Opal Demo – Export Events` | `.dbc` / `.ipynb` | Ingests activity and approval events to track access behavior and decision trends |
| `Opal Demo – Export Users` | `.dbc` / `.ipynb` | Normalizes user metadata and attributes for cross-system correlation |
| `Opal Prod – Export Groups` | `.dbc` / `.ipynb` | Models group and role hierarchies for entitlement graph analysis |
| `Opal Prod – Export Owner Objects` | `.dbc` / `.ipynb` | Maps ownership relationships to resources, apps, and data assets |
| `Opal Prod – Export Owner Users` | `.dbc` / `.ipynb` | Links individuals to governed assets and responsibilities |

> `.dbc` files are for direct import into Databricks. `.ipynb` files are provided for GitHub previewing and version control.

---

## Prerequisites

- An [Opal Security](https://opal.dev) account with API access
- A [Databricks](https://databricks.com) workspace (the [free tier](https://www.databricks.com/learn/free-edition) is sufficient to run these examples)
- An Opal API key

---

## Getting Started

### 1. Import notebooks into Databricks

Download the `.dbc` files from this repository and import them into your Databricks workspace:

1. In Databricks, go to **Workspace** → **Import**
2. Select the `.dbc` files from this repo
3. Choose a destination folder

Alternatively, clone this repo and import the `.ipynb` versions if you prefer working with Jupyter-compatible notebooks.

### 2. Configure credentials

Store your Opal API key using [Databricks Secrets](https://docs.databricks.com/aws/en/security/secrets/) to avoid hardcoding credentials:

```python
# In opal_functions notebook
opal_secret = dbutils.secrets.getBytes('demo_scope', 'opal-key')
```

Then set your Opal API base URL in the `opal_functions` notebook:

```python
connect_opal(base_url="https://your-org.opal.dev", token=opal_secret.decode())
```

### 3. Run the notebooks

Execute the notebooks in this order:

1. `opal_functions` — sets up shared utilities and the connection to Opal
2. `Export Users` — creates the `opal_users` Delta table
3. `Export Groups` — creates the `opal_groups` Delta table
4. `Export Events` — creates the `opal_events` Delta table
5. `Export Owner Objects` / `Export Owner Users` — creates the `opal_owners` tables

### 4. Explore with Genie

Once your Delta tables are created, an easy way to start exploring is to create an Opal [Genie space](https://docs.databricks.com/aws/en/genie/) in the workspace and selecting the tables above. Genie provides a natural language interface to query your identity data without writing SQL.

### 5. Build on top

From here, the possibilities include:

- **Custom SQL queries and dashboards** — build visualizations over access patterns, ownership, and entitlement distributions
- **Predictive risk scoring** — use ML to forecast which users or roles are likely to drift from least privilege, enriched with org-specific context
- **Cost & license optimization** — quantify and right-size underutilized access or SaaS subscriptions
- **Closed-loop governance** — feed analytic outputs (risk scores, utilization metrics) back into Opal workflows via the Opal API

---

## Delta Tables Reference

After running all notebooks, the following Delta tables will be available in your workspace:

| Table | Description |
|-------|-------------|
| `opal_users` | Normalized user records with metadata and attributes |
| `opal_groups` | Group and role membership hierarchies |
| `opal_events` | Access request, approval, and activity events |
| `opal_owners` | Ownership mappings between individuals and governed resources |

---

## Resources

- [Opal API Documentation](https://docs.opal.dev)
- [Opal + Databricks Blog Post](https://opal.dev/blog) *(link to published post)*
- [Databricks Secrets Documentation](https://docs.databricks.com/aws/en/security/secrets/)
- [Opal Security](https://opal.dev)

---

## Authors

**Jack Zaldivar, Jr.** — Staff Systems Engineer, Databricks  
[LinkedIn](https://www.linkedin.com/in/jack-zaldivar-jr-40142a25/) · [Databricks Community](https://community.databricks.com/t5/user/viewprofilepage/user_id/130822)

**Barrett Woodside** — Head of Growth & Strategy, Opal Security  
[barrett@opal.dev](mailto:barrett@opal.dev)

---

## License

See [LICENSE](LICENSE) for details.
