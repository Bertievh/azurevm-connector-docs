---
sidebar_label: 'Release Notes'
title: Azure VM Connector release notes
description: "Version history and change details for the Azure VM Connector, including new features, improvements, and bug fixes."
tags:
  - Reference
  - System Administrator
  - Connectors
---

# Azure VM Connector release notes

## General

The AzureVM Connector supports two sub-type options.

### Enterprise Manager

Enterprise Manager provides a sub-type plug-in module that is copied into the Enterprise Manager plugins directory. The plug-in provides a mechanism to create the command-line options required when running the connector.

The sub-type is available as a Windows job sub-type named **Azure VM**.

### Solution Manager

OpCon version 25.0.3 or greater includes the ACS framework. The ACS framework provides the sub-type mechanism for the AzureVM connector. It centralizes the `Connector.config` file within the OpCon environment and provides a wrapper to the AzureVM connector.

The supplied integration is available from the FTP site under **integrations**. Download and extract the items, then copy them into the **\\plugins\\ACSAZUREVM** directory.

The implementation is available as an ACS Agent named **AzureVM** with an associated job that provides several task types.
