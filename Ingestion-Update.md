# Overview

Cause: A curator uses the Andi Webpage to configure an ingestion.
Result: All services save the latest ingestion entity. Reaper schedules extraction start.

# Schema

| Field Name | Type | Constant |
| - | - | - |
| type | string | "ingestion:update" |
| data | %SmartCity.Ingestion | - |
| author | string | - |

Performance: Negligible