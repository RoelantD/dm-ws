# MCP capability design — Dunder Mifflin package manager (baseline)

> **Part 5 baseline.** Maps the package manager application onto MCP primitives
> (resources, prompts, tools). Use this as the blueprint for the Part 6 build.

## Capability mapping

| Capability | MCP concept | Description | Inputs | Output | Read-only / approval |
|------------|-------------|-------------|--------|--------|----------------------|
| Package data | resource | Current packages with status, route, condition | — | — | read-only |
| Route data | resource | Routes and the packages on each | — | — | read-only |
| Data schema | resource | Shape of package, order, and route records | — | — | read-only |
| Policy document | resource | Shipping and condition rules the agent must respect | — | — | read-only |
| Investigate a delayed package | prompt | Reusable workflow: gather status, route, and history | orderId | — | — |
| list_packages | tool | Return all package IDs in the system | — | package ID list | read-only |
| lookup_package | tool | Return status, route, condition, and fragile flag for a package | `package_id: string` | formatted package record | read-only |
| lookup_order_status | tool | Return delivery status and delay details for an order | `package_id: string` | status + delay fields | read-only |
| lookup_route | tool | Return route details and driver for a route ID | `route_id: string` | route record | read-only |
| update_package_status | tool | Change a package's status | `package_id: string`, `status: enum` | `{ ok: boolean }` | **approval-required** |

## MCP design checklist

- [x] Five or more capabilities mapped to resource / prompt / tool.
- [x] Read-oriented context mapped to **resources** (package data, route data, schema, policy).
- [x] At least one reusable workflow mapped to a **prompt** (investigate-delayed-package).
- [x] At least three tools with named inputs and expected output.
- [x] Every operation marked **read-only** or **approval-required**.
- [x] State-changing operations marked **approval-required** for the HITL segment (Part 9).

## Open questions for the Part 6 build

- Transport: stdio (local, default) or HTTP (remote)?
- Where is the package data hosted — local JSON files or a live API endpoint?
- Which tools need input validation for unknown IDs?
