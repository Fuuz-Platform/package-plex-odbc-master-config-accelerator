# package-plex-odbc-master-config-accelerator

**Version:** 1.0.1
**Platform:** Fuuz (any)

---

## Overview

The Plex ODBC Master Data Configuration package installs the complete set of Fuuz master data import flows for Plex ERP customers using ODBC connectivity. It registers 40+ numbered integration flows — each responsible for importing a specific Plex data entity into Fuuz — as `MasterDataFlowConfiguration` records managed by the `package-master-data-flow-config-accelerator` orchestration layer.

Plex ODBC flows extract data directly from Plex stored procedure endpoints (via ODBC connection) and upsert records into Fuuz data models covering the full manufacturing and logistics data model: facilities, locations, customers, suppliers, products, workcenters, modes, events, BOM structures, inventory, and more.

This is an installer package (no interactive screens or custom models) — it creates flow configuration records and verifies uniqueness constraints before installing. It requires an active Plex ODBC `Connection` in the Fuuz platform.

---

## Package Contents

```
plex-odbc-master-config/
├── manifest.json
├── package-data.json
├── install/                     5 install steps
└── preinstall/                  3 preinstall validation steps
```

---

## Registered Import Flows (40+)

Each flow is sequentially numbered in its description for ordered execution. The full set covers:

### Site & Location Hierarchy
| # | Flow Name | Source |
|---|-----------|--------|
| 01 | Facilities from Plex ODBC Buildings | Plex Buildings |
| 02 | Location Type from Plex ODBC | Plex Location Types |
| 03 | Location Status from Plex ODBC | Plex Location Statuses |
| 04 | Locations from Plex ODBC | Plex Locations |
| 05 | Customer Stage from Plex ODBC | Plex Customer Stages |

### Customers & Suppliers
| # | Flow Name | Source |
|---|-----------|--------|
| 06 | Customers from Plex ODBC | Plex Customers |
| 07 | Customer Addresses from Plex ODBC | Plex Customer Addresses |
| 10 | Contacts from Plex ODBC | Plex Contacts (Supplier/Customer) |
| 24 | Approved Suppliers from Plex ODBC | Plex Approved Suppliers |

### Products & BOM
| # | Flow Name | Source |
|---|-----------|--------|
| 11 | Product Stages from Plex ODBC | Plex Product Stages |
| 12 | Product Types from Plex ODBC | Plex Product Types |
| 13 | Products from Plex ODBC Parts | Plex Parts |
| 14 | Product Strategies from Plex ODBC | Plex Product Strategies |
| 15 | Processes from Plex ODBC | Plex Processes |
| 16 | Product Strategy Process Type from Plex ODBC | Plex Part Operations Type |
| 17 | Product Strategy Processes from Plex ODBC | Plex Part Operations |
| 18 | Customer Products from Plex ODBC | Plex Customer Parts |
| 26 | Product Strategy Process BOM from Plex ODBC | Plex BOM |
| 39 | Product Custom Data from Plex ODBC | Plex Part Attributes |

### Logistics & Inventory
| # | Flow Name | Source |
|---|-----------|--------|
| 22 | Logistical Units from Plex ODBC Container Types | Plex Container Types |
| 23 | Approved Logistical Units from Plex ODBC | Plex Part Operation Approved Container |
| 38 | Adjustment Codes from Plex ODBC | Plex Adjustment Codes |
| 40 | Inventory Status from Plex ODBC | Plex Container Statuses |

### Production & Workcenters
| # | Flow Name | Source |
|---|-----------|--------|
| 25 | Approved Workcenters from Plex ODBC | Plex Approved Workcenters |
| 27 | Events from Plex ODBC | Plex Workcenter Events |
| 28 | Modes from Plex ODBC | Plex Workcenter Statuses |
| 29 | ModeEvent from Plex ODBC | Plex Workcenter Event Statuses |

*Additional flows (30–37, and others) cover shipment statuses, work orders, scheduling data, and supplementary reference tables.*

---

## Install Process

**Preinstall (3 steps):** Validates uniqueness constraints for all flow IDs and names before installation begins. Throws an error if any flow already exists with a conflicting ID or name, preventing duplicate registration.

**Install (5 steps):**
1. Creates all `MasterDataFlowConfiguration` header records (one per import flow) with the flow ID, name, description, and `Integration` type
2. Registers each flow's connection configuration — links the configuration record to the Plex ODBC connection
3. Sets initial schedule definitions for each flow (e.g., nightly, hourly)
4. Sets `active` status based on recommended defaults (most flows active, some requiring manual activation)
5. Deploys the flow definitions to the application as runnable integration flows

---

## Installation

1. Ensure `package-master-data-flow-config-accelerator` is installed first
2. Ensure a Plex ODBC `Connection` is configured in Fuuz Connections settings and its connector is active
3. Import this package via Fuuz Package Manager
4. Review installed `MasterDataFlowConfiguration` records and adjust schedules as needed
5. Enable desired flows by setting `active = true`
6. Trigger initial full sync by running all active flows manually from the Master Data Import dashboard

---

## Dependencies

- **Fuuz Platform** — any version
- **`package-master-data-flow-config-accelerator`** — required; provides the `MasterDataFlowConfiguration` model and scheduling infrastructure
- **Plex ERP** with ODBC access enabled
- A configured Plex ODBC `Connection` in the Fuuz platform
- Target data models must exist (provided by `application-mes-accelerator`, `application-wms-accelerator`, or `application-machine-monitoring-accelerator`)

---

*Built on the [Fuuz Industrial Operations Platform](https://fuuz.com)*

## Service levels

No service level agreement applies to anything published here. It becomes a supported
deliverable only once it has been implemented by a Fuuz services professional or an
approved Fuuz partner.
