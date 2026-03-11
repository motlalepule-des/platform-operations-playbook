# Configure Power BI SQL Server Gateway

## Purpose
Connect Power BI Service to a SQL Server source through the gateway.

---

## Option A: Standalone Gateway (No Cluster)

Use this option when you have a single gateway machine and do not need high availability or load balancing.

### Requirements
- Gateway installed and online (see [Install Power BI Gateway](install-powerbi-gateway.md))
- SQL Server reachable from the gateway machine
- Database credentials available

### Install as a Standalone Gateway

During initial gateway setup, when prompted:

1. Select **Register a new gateway on this computer**.
2. Enter a **gateway name** (unique within your tenant).
3. Set a **recovery key** and store it securely — you will need it to restore or migrate the gateway later.
4. Click **Configure**.

> Do **not** select "Add to an existing gateway cluster" — that option is for joining a cluster.

### Add a SQL Server Data Source

1. Open **Power BI Service**.
2. Go to **Settings → Manage connections and gateways**.
3. Select your standalone gateway.
4. Click **New connection**.
5. Choose **SQL Server**.
6. Enter:
   - **Server name** — hostname or IP of the SQL Server
   - **Database name**
   - **Authentication** — Windows or Basic (username/password)
7. Click **Create**.

### Map a Dataset to the Data Source

1. Open the dataset in Power BI Service.
2. Go to **Settings → Gateway connection**.
3. Select the standalone gateway and the data source you just created.
4. Click **Apply**.

### Verify

Trigger a manual dataset refresh from Power BI Service and confirm it completes without errors.

---


## Option B: Gateway Cluster (High Availability)

A **gateway cluster** groups multiple gateway instances together for high availability and load balancing. If one gateway node goes down, Power BI fails over to another node automatically.

### Requirements
- At least two machines with the on-premises data gateway installed
- All nodes must be able to reach the same data sources
- All nodes registered to the same Microsoft 365 / Power BI tenant
- Gateway version must be the same (or compatible) across all nodes

### Add a Node to an Existing Gateway Cluster

1. On the **second machine**, open the **On-premises data gateway** app after installation.
2. Sign in with the **same account** used for the primary gateway.
3. On the setup screen, select **Add to an existing gateway cluster**.
4. From the drop-down list, select the **primary gateway cluster** to join.
5. Enter the **recovery key** that was set when the primary gateway was created.
6. Click **Configure**.

> **Note:** The recovery key is set during the initial gateway installation. If you did not save it, you must reset the primary gateway before adding a new node.

### Verify the Cluster in Power BI Service

1. Open **Power BI Service**.
2. Go to **Settings → Manage connections and gateways**.
3. Under **On-premises data gateways**, locate your gateway cluster.
4. Expand the cluster to confirm all nodes appear with status **Online**.

### Configure Load Balancing

By default, Power BI distributes requests across all online nodes randomly. To change the load balancing mode:

1. In **Manage connections and gateways**, select the gateway cluster.
2. Click the **three-dot menu (…)** → **Settings**.
3. Under **Load balance requests across all active gateways in this cluster**, choose:
   - **Random** — requests are distributed equally at random (default)
   - **Performance optimized** — Power BI routes requests to the node that responded fastest recently
4. Click **Save**.

### Configure a Data Source on the Cluster

Data sources are shared across all nodes in the cluster. You only need to configure them once:

1. In **Manage connections and gateways**, select the gateway cluster.
2. Click **New connection**.
3. Choose **SQL Server** as the connection type.
4. Enter:
   - **Server name** — hostname or IP of the SQL Server
   - **Database name**
   - **Authentication** — Windows or Basic
5. Click **Create**.

All nodes in the cluster will use this connection definition automatically.

### Maintenance: Take a Node Offline

To update or restart a single node without interrupting service:

1. In **Manage connections and gateways**, expand the cluster.
2. Select the specific node and click **Disable**.
3. Perform maintenance on that machine.
4. Restart the gateway service on the machine: open the gateway app and confirm it shows **Online**.
5. Re-enable the node in Power BI Service.

### Troubleshooting Cluster Nodes

| Symptom | Likely Cause | Action |
|---|---|---|
| Node shows **Offline** | Gateway service stopped | Restart the on-premises data gateway service on that machine |
| Node not visible after adding | Wrong recovery key or account | Delete the gateway entry and re-add with correct credentials |
| All refreshes fail | All cluster nodes offline | Check network connectivity and gateway service on each node |
| Node added but not joining cluster | Version mismatch | Update gateway to the same version on all nodes |