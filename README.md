# Service Assurance Manager Architecture

This repository provides a visual overview of the **Service Assurance Manager (SAM)** deployment. The architecture is illustrated in the [`smarts_architecture.html`](smarts_architecture.html) diagram.

The HTML page contains an interactive diagram showing the main AWS components, on‑premises connections, and disaster recovery (DR) elements. The sections below describe the layout in plain language so that you can understand what each part of the diagram represents.

---

## Diagram Overview

The diagram divides the environment into four major areas:

1. **HiG Data Center (On‑Premises)** – Local mail relay and monitoring components.
2. **External Services** – Integrations such as ServiceNow and Splunk.
3. **Primary AWS Region (us‑east‑1)** – Core SAM instances running across two Availability Zones.
4. **DR Region (us‑east‑2)** – A secondary region mirroring the primary environment for high availability.

Each area is highlighted with a border and label to make the relationships clear.

## Primary Region Details

Within the primary region there is a single Virtual Private Cloud (VPC) containing two Availability Zones (AZ‑1 and AZ‑2). Each AZ hosts:

- A private subnet protected by a security group.
- Two EC2 instances: a **SAM Server** and an **IP Server**. These handle service assurance processing and data collection.
- An **EFS mount target** used for shared storage between instances in the same zone.

A shared "Writable Dir" sits outside of the AZ boxes, representing a directory accessible from both AZs. Network connections between the SAM and IP servers are shown with small horizontal lines.

## Disaster Recovery Region

The DR region mirrors the primary architecture but on a smaller scale. It contains:

- A separate VPC and two AZs, each with a private subnet.
- SAM and IP servers equivalent to the primary region.
- EFS mount targets for shared storage.
- A backup vault at the bottom representing AWS Backup for recovery purposes.

## On‑Premises and External Services

At the top left of the diagram is the HiG Data Center section. It includes mail relay and network monitoring components that communicate with the SAM servers. On the top right are external services such as ServiceNow and Splunk, which integrate with SAM for incident management and log analysis.

## Technical Specifications

The diagram also lists key parameters for the deployment:

- **Instance type**: `c7i.2xlarge` running RHEL&nbsp;8.0.
- **vCPUs**: 8 per instance with 16&nbsp;GB of RAM.
- **Storage**: 75&nbsp;GB root plus 500&nbsp;GB EBS per server.
- **Network ports**: TCP&nbsp;8770 for SAM and TCP&nbsp;9002 for traps.
- **Recovery objectives**: RTO and RPO of 0–2&nbsp;hours.
- **High availability**: achieved through a cross‑AZ layout with EFS replication and AWS Backup.

These values appear in the "Technical Specifications" box within the HTML diagram.

## Viewing the Diagram

Open `smarts_architecture.html` in any modern web browser to view the full diagram. Hovering over the EC2 or component boxes will highlight them, and clicking them displays a small information alert. The network flow lines animate periodically to emphasize connectivity.

If you prefer a text-based diagram, the repository originally referenced a Mermaid file (`smarts_architecture.mmd`). If this file is present, you can render it in a Mermaid viewer for a simple diagrammatic representation.

---

This README summarizes the purpose of each area of the HTML diagram so that anyone can quickly understand how the SAM solution is structured in AWS and how it interacts with on‑premises resources and third‑party services.
