---
title: Pivotal Container Service Integration
tags: [integrations list]
permalink: pks.html
summary: Learn about the Wavefront Pivotal Container Service Integration.
---
## Pivotal Container Service Integration

Pivotal Container Service (PKS) enables operators to provision, operate, and manage enterprise-grade Kubernetes clusters on Pivotal Cloud Foundry (PCF). This integration uses [Heapster](https://github.com/kubernetes/heapster), a collector agent that runs natively in Kubernetes and [kube-state-metrics](https://github.com/kubernetes/kube-state-metrics), a simple service that listens to the Kubernetes API server and generates metrics. The integration collects detailed metrics about the containers, namespaces, nodes, pods, deployments, services and the cluster itself and sends them to a Wavefront.

This integration explains how to configure PKS monitoring with Wavefront from the PKS tile present in PCF Ops Manager. After you've completed the integration setup, you can use Wavefront to monitor the PKS cluster.

In addition to setting up the metrics flow, this integration also installs a dashboard. Here's a preview of **Overview** and **Nodes** section of the dashboard.

{% include image.md src="images/db_overview.png" width="80" %}

## Pivotal Container Services Setup

  Supported Version: PKS 1.1 and above

### Configuring the Wavefront Account

1. Log in to PCF Ops Manager and click the **Pivotal Container Service** tile in Installation Dashboard.
2. Under the Settings tab, click **Monitoring**.
3. In the right pane, check **Yes** to enable **Wavefront Integration** and enter the account information:
   * **Wavefront URL**: `http://YOUR_CLUSTER.wavefront.com/api`
   * **API Token**: `YOUR_API_TOKEN`
   * **Wavefront Alert Recipient**: `A list of Email addresses &/or Wavefront Target IDs`
4. Click **Save**.
5. Navigate back to the Installation dashboard and click **Apply Changes**.
