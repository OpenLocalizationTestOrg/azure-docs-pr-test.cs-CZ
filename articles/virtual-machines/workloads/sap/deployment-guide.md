---
title: "aaaAzure nasazení virtuálních počítačů pro SAP NetWeaver | Microsoft Docs"
description: "Zjistěte, jak toodeploy SAP software na virtuální počítače s Linuxem v Azure."
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 1c4f1951-3613-4a5a-a0af-36b85750c84e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.openlocfilehash: 42f1d8a3eff51e113729dbfe2848b67deaf06936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-deployment-for-sap-netweaver"></a>Nasazení virtuálních počítačů Azure pro SAP NetWeaver
[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]:https://launchpad.support.sap.com/#/notes/1619967
[1750510]:https://launchpad.support.sap.com/#/notes/1750510
[1752266]:https://launchpad.support.sap.com/#/notes/1752266
[1757924]:https://launchpad.support.sap.com/#/notes/1757924
[1757928]:https://launchpad.support.sap.com/#/notes/1757928
[1758182]:https://launchpad.support.sap.com/#/notes/1758182
[1758496]:https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]:https://launchpad.support.sap.com/#/notes/1814258
[1882376]:https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]:https://launchpad.support.sap.com/#/notes/1922555
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[2367194]:https://launchpad.support.sap.com/#/notes/2367194

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md (Azure Virtual Machines DBMS deployment for SAP)
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Caching for VMs and VHDs)
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software RAID)
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Storage)
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Structure of a RDBMS deployment)
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (High availability and disaster recovery with Azure VMs)
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 and later)
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 and earlier releases)
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (Using a SQL Server image from hello Azure Marketplace)
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (General SQL Server for SAP on Azure summary)
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (Specifics tooSQL Server RDBMS)
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Storage configuration)
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Backup and restore)
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Performance considerations for backup and restore)
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (Other)
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md (Azure Virtual Machines deployment for SAP)
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP resources)
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Deploying a VM by using a custom image)
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Scenario 1: Deploying a VM from hello Azure Marketplace for SAP)
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Scenario 2: Deploying a VM with a custom image for SAP)
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Scenario 3: Moving a VM from on-premises using a non-generalized Azure VHD with SAP)
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (Deployment scenarios of VMs for SAP on Microsoft Azure)
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Deploying Azure PowerShell cmdlets)
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Download and import SAP-relevant PowerShell cmdlets)
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Join a VM tooan on-premises domain - Windows only)
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux)
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (Download, install, and enable hello Azure VM Agent)
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell)
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI)
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Configure hello Azure Enhanced Monitoring Extension for SAP)
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Readiness check for Azure Enhanced Monitoring for SAP)
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Health check for hello Azure monitoring infrastructure)
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Troubleshooting Azure monitoring for SAP)

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure monitoring)
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure hello proxy)
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and troubleshooting for setting up end-to-end monitoring)

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md (Azure Virtual Machines planning and implementation for SAP)
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Resources)
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (High availability and disaster recovery for SAP NetWeaver running on Azure Virtual Machines)
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (High availability for SAP Application Servers)
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (Using Autostart for SAP instances)
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 (Cloud-only - Virtual Machine deployments in Azure without dependencies on hello on-premises customer network)
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Cross-premises - Deployment of single or multiple SAP VMs in Azure fully integrated with hello on-premises network)
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure regions)
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Fault domains)
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Upgrade domains)
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 (Azure availability sets)
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Microsoft Azure virtual machines concept)
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Moving a VM from on-premises tooAzure with a non-generalized disk)
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Deploying a VM with a customer specific image)
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Preparation for moving a VM from on-premises tooAzure with a non-generalized disk)
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Preparation for deploying a VM with a customer specific image for SAP)
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Preparing VMs with SAP for Azure)
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Difference between an Azure disk and an Azure image)
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (Uploading a VHD from on-premises tooAzure)
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Copying disks between Azure Storage accounts)
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (VM/VHD structure for SAP deployments)
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Setting automount for attached disks)
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (Single VM with SAP NetWeaver demo/training scenario)
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Concepts of Cloud-Only deployment of SAP instances)
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure Monitoring Solution for SAP)
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)
[planning-guide-managed-disks]:planning-guide.md#c55b2c6e-3ca1-4476-be16-16c81927550f (Managed Disks)
[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and data disks)

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image-md%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-os-disk-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk-md%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-2-tier-user-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image-md%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-md%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image-md%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../../storage/common/storage-premium-storage.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and hello Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md (Manage virtual machines by using Azure Resource Manager and PowerShell)
[virtual-machines-windows-agent-user-guide]:../../windows/agent-user-guide.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:../../linux/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../../../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../../../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../../../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Virtuální počítače Azure je hello řešení pro organizace, které potřebují výpočetní a úložnou kapacitu, minimální včas a bez zdlouhavé nákup cykly. Aplikace, klasického virtuálních počítačích Azure toodeploy jako SAP NetWeaver-li aplikace, můžete použít v Azure. Rozšiřte spolehlivosti a dostupnosti bez dalších místních prostředků aplikace. Virtuální počítače Azure podporuje připojení mezi různými místy, takže virtuální počítače Azure můžete integrovat do místní domény vaší organizace, privátních cloudů a šířku systému SAP.

V tomto článku jsme zahrnovat hello kroky toodeploy SAP žádosti na virtuálních počítačích (VM) v Azure, včetně možnosti alternativní nasazení a řešení potíží. Tento článek vychází hello informace v [virtuální počítače Azure plánování a implementace pro SAP NetWeaver][planning-guide]. Také doplňuje SAP instalace dokumentace a poznámky k SAP, které jsou hello primární prostředky pro instalaci a nasazení SAP softwaru.

## <a name="prerequisites"></a>Požadavky
Nastavení virtuálního počítače Azure pro nasazení softwaru SAP zahrnuje několik kroků a prostředky. Než začnete, ujistěte se, že splňujete hello požadavky pro instalaci softwaru SAP na virtuálních počítačích v Azure.

### <a name="local-computer"></a>Místní počítač
toomanage Windows nebo Linux virtuální počítače, můžete použít skript prostředí PowerShell a hello portálu Azure. Pro oba nástroje budete potřebovat místní počítač se systémem Windows 7 nebo novější verzi systému Windows. Pokud chcete toomanage pouze virtuální počítače s Linuxem a vy chcete toouse počítač se systémem Linux pro tento úkol můžete použít rozhraní příkazového řádku Azure.

### <a name="internet-connection"></a>Připojení k Internetu
toodownload a spuštění hello nástroje a skripty, které jsou požadovány pro nasazení softwaru SAP, musí být připojen toohello Internetu. také je nutné Hello virtuálního počítače Azure, který běží hello Azure rozšířené monitorování rozšíření pro SAP toohello přístup k Internetu. Pokud hello virtuální počítač Azure je součástí virtuální síť Azure nebo místní domény, ujistěte se, že jsou nastavené hello příslušné nastavení serveru proxy, jak je popsáno v [nakonfigurovat proxy server hello][deployment-guide-configure-proxy].

### <a name="microsoft-azure-subscription"></a>Předplatné Microsoft Azure
Potřebujete aktivní účet Azure.

### <a name="topology-and-networking"></a>Topologie a sítě
Je třeba toodefine hello topologie a architektura hello nasazení SAP v Azure:

* Toobe účty úložiště Azure používají
* Virtuální síť, kam chcete toodeploy hello SAP systému
* Toowhich skupiny prostředků má systém SAP toodeploy hello
* Oblast Azure, kam chcete toodeploy hello SAP systému
* Konfigurace SAP (dvouvrstvá nebo třívrstvá)
* Velikosti virtuálních počítačů a hello počet toobe další datové disky připojené toohello virtuální počítače
* Konfigurace SAP oprava a přenosu systému (CTS)

Vytvořit a nakonfigurovat účty úložiště Azure (v případě potřeby) nebo virtuální sítě Azure, před zahájením procesu nasazení softwaru SAP hello. Informace o toocreate a nakonfigurovat tyto prostředky, najdete v části [virtuální počítače Azure plánování a implementace pro SAP NetWeaver][planning-guide].

### <a name="sap-sizing"></a>Nastavení velikosti SAP
Následující informace o nastavení velikosti SAP hello vědět:

* Projektovat SAP zatížení, například pomocí nástroje rychlé přizpůsobení velikosti symbolů SAP hello a číslo hello SAP aplikace výkonu standardní (protokoly SAP)
* Požadovaný prostředek a paměti spotřeby procesoru systému SAP hello
* Požadované vstupní a výstupní (I/O) operace za sekundu
* Požadované šířky pásma sítě případné komunikace mezi virtuálními počítači v Azure
* Požadované šířku pásma sítě mezi místními prostředky a hello systému Azure nasazení SAP

### <a name="resource-groups"></a>Skupiny prostředků
V Azure Resource Manager, můžete použít toomanage skupiny prostředků všechny prostředky aplikace hello ve vašem předplatném Azure. Další informace najdete v tématu [přehled Azure Resource Manageru][resource-group-overview].

## <a name="resources"></a>Zdroje

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>Prostředky SAP
Pokud nastavujete nasazení softwaru SAP, potřebujete následující prostředky SAP hello:

* Poznámka SAP [1928533], na kterém je:
  * Seznam velikostí virtuálních počítačů Azure, které jsou podporovány pro hello nasazení SAP softwaru
  * Kapacita důležité informace o velikosti virtuálního počítače Azure
  * Podporované SAP software a operační systém (OS) a kombinace databáze
  * Požadovaná verze SAP jádra pro Windows a Linux v Microsoft Azure

* Poznámka SAP [2015553] uvádí požadavky pro nasazení softwaru podporovaných SAP SAP v Azure.
* Poznámka SAP [2178632] obsahuje podrobné informace o veškeré monitorování metriky pro SAP v Azure.
* Poznámka SAP [1409604] hello vyžaduje verze SAP hostitele agenta pro Windows v Azure.
* Poznámka SAP [2191498] hello vyžaduje verze SAP hostitele agenta pro Linux v Azure.
* Poznámka SAP [2243692] obsahuje informace o licencích SAP v systému Linux v Azure.
* Poznámka SAP [1984787] má obecné informace o SUSE Linux Enterprise Server 12.
* Poznámka SAP [2002167] má obecné informace o Red Hat Enterprise Linux 7.x.
* Poznámka SAP [2069760] má obecné informace o Oracle Linux 7.x.
* Poznámka SAP [1999351] obsahuje další informace o řešení potíží pro hello Azure rozšířené monitorování rozšíření pro SAP.
* Poznámka SAP [1597355] má obecné informace o odkládacího prostoru pro Linux.
* [Na stránce oznámení změny stavu Azure SAP](https://wiki.scn.sap.com/wiki/x/Pia7Gg) má zprávy a kolekce užitečné zdroje.
* [SAP komunity WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) má všechny požadované SAP poznámky pro Linux.
* Rutiny prostředí PowerShell specifické pro SAP, které jsou součástí [prostředí Azure PowerShell][azure-ps].
* Příkazy rozhraní příkazového řádku Azure specifické pro SAP, které jsou součástí [rozhraní příkazového řádku Azure][azure-cli].

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>Materiály pro Windows
Tyto články Microsoft popisuje nasazení SAP v Azure:

* [Azure virtuálních počítačů, plánování a implementace pro SAP NetWeaver][planning-guide]
* [Nasazení virtuálních počítačů Azure pro SAP NetWeaver (v tomto článku)][deployment-guide]
* [Nasazení virtuálních počítačů databázového systému Azure pro SAP NetWeaver][dbms-guide]

## <a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Scénáře nasazení SAP softwaru na virtuálních počítačích Azure
Máte několik možností pro nasazení virtuálních počítačů a přidruženými disky v Azure. Je důležité toounderstand hello rozdíly mezi možnostmi nasazení, protože může trvat různé kroky tooprepare virtuální počítače pro nasazení podle typu hello nasazení, který zvolíte.

### <a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Scénář 1: Nasazení virtuálního počítače z hello Azure Marketplace pro SAP
Můžete použít bitovou kopii od Microsoftu nebo třetí strany v Azure Marketplace toodeploy hello virtuálního počítače. Hello Marketplace nabízí některé standardní bitové kopie operačního systému Windows Server a různých distribucí Linux. Obrázek, který zahrnuje správu databáze také můžete nasadit systém SKU, například Microsoft SQL Server. Další informace o používání bitové kopie s SKU databázového systému najdete v tématu [databázového systému virtuální počítače Azure nasazení pro SAP NetWeaver][dbms-guide].

Hello následující vývojový diagram znázorňuje hello SAP konkrétní pořadí kroků pro nasazení virtuálního počítače z Azure Marketplace hello:

![Vývojový diagram nasazení virtuálního počítače pro systémy SAP pomocí bitové kopie virtuálního počítače z hello Azure Marketplace][deployment-guide-figure-100]

#### <a name="create-a-virtual-machine-by-using-hello-azure-portal"></a>Vytvoření virtuálního počítače pomocí hello portálu Azure
Nejjednodušší způsob, jak toocreate Hello nový virtuální počítač s bitovou kopii z hello Azure Marketplace je pomocí hello portálu Azure.

1.  Přejděte příliš<https://portal.azure.com/#create/hub>.  Nebo, v nabídce portálu Azure hello, vyberte **+ nový**.
2.  Vyberte **výpočetní**a pak vyberte typ operačního systému, který má toodeploy hello. Například Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) nebo Oracle Linux 7.2. zobrazení seznamu výchozí Hello nezobrazuje, že všechny podporované operační systémy. Vyberte **zobrazit všechny** úplný seznam. Další informace o podporovaných operačních systémech pro nasazení softwaru SAP, viz poznámka SAP [1928533].
3.  Na další stránku hello přečtěte si podmínky a ujednání.
4.  V hello **vybrat model nasazení** vyberte **Resource Manager**.
5.  Vyberte **Vytvořit**.

Hello Průvodce vás provede nastavením hello požadované parametry toocreate hello virtuálního počítače, kromě tooall požadované prostředky, jako je síťová rozhraní a účty úložiště. Některé z těchto parametrů jsou:

1. **Základy**:
 * **Název**: hello název prostředku hello (hello název virtuálního počítače).
 * **Typ disku virtuálního počítače**: Vyberte typ disku hello hello operačního systému disku. Pokud chcete toouse Storage úrovně Premium pro datové disky, doporučujeme používat úložiště úrovně Premium pro disk operačního systému hello také.
 * **Uživatelské jméno a heslo** nebo **veřejný klíč SSH**: Zadejte hello uživatelské jméno a heslo hello uživatele, který se vytvoří během zřizování hello. Pro virtuální počítač s Linuxem můžete zadat veřejný klíč Secure Shell (SSH) hello použít toosign v toohello počítače.
 * **Předplatné**: Vyberte předplatné hello má toouse tooprovision hello nového virtuálního počítače.
 * **Skupina prostředků**: hello název skupiny prostředků hello hello virtuálních počítačů. Můžete zadat buď název hello nový prostředek skupiny nebo hello název skupiny prostředků, která již existuje.
 * **Umístění**: kde toodeploy hello nového virtuálního počítače. Pokud chcete tooconnect hello virtuálního počítače tooyour do místní sítě, ujistěte se, že jste vybrali hello umístění hello virtuální sítě, která se připojuje Azure tooyour do místní sítě. Další informace najdete v tématu [sítě Microsoft Azure] [ planning-guide-microsoft-azure-networking] v [virtuální počítače Azure plánování a implementace pro SAP NetWeaver] [ planning-guide].
2. **Velikost**:

     Seznam podporovaných typů virtuálních počítačů, viz poznámka SAP [1928533]. Ujistěte se, zda že jste vybrali správný typ virtuálního počítače hello, pokud chcete, aby toouse Azure Premium Storage. Ne všechny typy virtuálních počítačů podporují službu Premium Storage. Další informace najdete v tématu [úložiště: Microsoft Azure Storage a datové disky] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] a [Azure Premium Storage] [ planning-guide-azure-premium-storage] v [ Azure virtuálních počítačů, plánování a implementace pro SAP NetWeaver][planning-guide].

3. **Nastavení**:
  * **Úložiště**
    * **Typ disku**: Vyberte typ disku hello hello operačního systému disku. Pokud chcete toouse Storage úrovně Premium pro datové disky, doporučujeme používat úložiště úrovně Premium pro disk operačního systému hello také.
    * **Používat spravované disky**: Pokud chcete spravovat disky toouse, vyberte možnost Ano. Další informace o spravovaných disků, naleznete v kapitole [spravované disky] [ planning-guide-managed-disks] v příručce plánování na hello.
    * **Účet úložiště**: Vyberte existující účet úložiště nebo vytvořte novou. Ne všechny typy úložiště fungovat pro spouštění aplikací SAP. Další informace o typech úložiště najdete v tématu [Microsoft Azure Storage] [ dbms-guide-2.3] v [databázového systému virtuální počítače Azure nasazení pro SAP NetWeaver] [ dbms-guide].
  * **Síť**
    * **Virtuální síť** a **podsíť**: toointegrate hello virtuální počítač s intranetu, vyberte hello virtuální síť, která je připojená tooyour do místní sítě.
    * **Veřejná IP adresa**: Vyberte hello veřejnou IP adresu, mají toouse, nebo zadejte parametry toocreate novou veřejnou IP adresu. Můžete veřejné tooaccess adresu IP virtuálního počítače přes hello Internet. Ujistěte se také vytvořit sítě zabezpečení skupiny toohelp zabezpečený přístup tooyour virtuálního počítače.
    * **Skupina zabezpečení sítě**: Další informace najdete v tématu [řízení toku provozu sítě s skupin zabezpečení sítě][virtual-networks-nsg].
  * **Rozšíření**: přidáním toohello nasazení můžete nainstalovat rozšíření virtuálního počítače. Není nutné tooadd rozšíření v tomto kroku. Podpora SAP vyžaduje rozšíření Hello jsou nainstalovány později. Naleznete v kapitole [hello konfigurovat Azure rozšířené monitorování rozšíření pro SAP] [ deployment-guide-4.5] v této příručce.
  * **Vysoká dostupnost**: Vyberte skupinu dostupnosti, nebo zadejte hello parametry toocreate dostupnost nové nastavení. Další informace najdete v tématu [skupiny dostupnosti Azure][planning-guide-3.2.3].
  * **Monitorování**
    * **Spouštění diagnostiky**: můžete vybrat **zakázat** pro Diagnostika spouštění.
    * **Diagnostika hostovaného operačního systému**: můžete vybrat **zakázat** pro monitorování diagnostiky.

4. **Souhrn**:

  Zkontrolujte váš výběr a potom vyberte **OK**.

Virtuální počítač nasazen ve skupině prostředků hello, který jste vybrali.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Vytvoření virtuálního počítače pomocí šablony
Virtuální počítač můžete vytvořit pomocí jedné z šablony SAP hello publikované v hello [úložiště GitHub šablon azure rychlý Start][azure-quickstart-templates-github]. Můžete také můžete ručně vytvořit virtuální počítač pomocí hello [portál Azure][virtual-machines-windows-tutorial], [prostředí PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms], nebo [rozhraní příkazového řádku Azure ][virtual-machines-linux-tutorial].

* [**Šablona dvouvrstvá konfigurace (jenom jeden virtuální počítač)** (sap-2vrstvy marketplace-image)][sap-templates-2-tier-marketplace-image]

  Tuto šablonu použít toocreate dvouvrstvá systému pomocí jenom jeden virtuální počítač.
* [**Šablona dvouvrstvá konfigurace (jenom jeden virtuální počítač) - spravované disky** (sap-2-tier-marketplace-image-md)][sap-templates-2-tier-marketplace-image-md]

  Tuto šablonu použít toocreate dvouvrstvá systému pomocí jenom jeden virtuální počítač a spravovat disky.
* [**Šablona třívrstvá konfigurace (více virtuálních počítačů)** (sap-3vrstvy marketplace-image)][sap-templates-3-tier-marketplace-image]

  Tuto šablonu použít toocreate třívrstvá systému pomocí více virtuálních počítačů.
* [**Šablona třívrstvá konfigurace (více virtuálních počítačů) - spravované disky** (sap-3-tier-marketplace-image-md)][sap-templates-3-tier-marketplace-image-md]

  Tuto šablonu použít toocreate třívrstvá systému pomocí více virtuálních počítačů a spravovat disky.

V hello portálu Azure zadejte následující parametry šablony hello hello:

1. **Základy**:
  * **Předplatné**: hello předplatné toouse toodeploy hello šablony.
  * **Skupina prostředků**: hello prostředků skupiny toouse toodeploy hello šablony. Můžete vytvořit novou skupinu prostředků, nebo můžete vybrat existující skupinu prostředků v předplatném hello.
  * **Umístění**: kde toodeploy hello šablony. Pokud jste vybrali existující skupinu prostředků, použije se hello umístění této skupiny prostředků.

2. **Nastavení**:
  * **ID systému SAP**: hello ID systému SAP (SID).
  * **Typ operačního systému**: Chcete toodeploy, například hello operačního systému, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) nebo Oracle Linux 7.2.

    zobrazení seznamu Hello nezobrazuje, že všechny podporované operační systémy. Další informace o podporovaných operačních systémech pro nasazení softwaru SAP, viz poznámka SAP [1928533].
  * **Velikost systému SAP**: hello velikost hello systému SAP.

    poskytuje Hello počet protokoly SAP hello nový systém. Pokud si nejste jisti, kolik protokoly SAP hello systém vyžaduje, požádejte SAP technologie partnera nebo systémový integrátor.
  * **Dostupnost systému** (pouze šablony třívrstvá): hello dostupnosti systému.

    Vyberte **HA** konfigurace, který je vhodný pro instalaci s vysokou dostupností. Vytvoří se dvou databázové servery a dva servery pro ABAP SAP centrální služby (ASC).
  * **Typ úložiště** (pouze šablony dvouvrstvá): hello typ toouse úložiště.

    U větších systémů důrazně doporučujeme pomocí Azure Premium Storage. Další informace o typech úložiště najdete v těchto zdrojích:
      * [Používání úložiště Azure Premium SSD pro instanci databázového systému SAP][2367194]
      * [Microsoft Azure Storage] [ dbms-guide-2.3] v [databázového systému virtuální počítače Azure nasazení pro SAP NetWeaver][dbms-guide]
      * [Úložiště Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure][storage-premium-storage-preview-portal]
      * [Úvod tooMicrosoft Azure Storage][storage-introduction]
  * **Uživatelské jméno správce** a **heslo správce**: uživatelské jméno a heslo.
    Pro přihlášení toohello virtuální počítač se vytvoří nového uživatele.
  * **Nový nebo existující podsíť**: Určuje, zda se vytvoří nová virtuální síť a podsíť, nebo se používá existující podsítí. Pokud již máte virtuální síť, která je připojená tooyour do místní sítě, vyberte **existující**.
  * **ID podsítě**: ID hello hello podsíť hello virtuálních počítačů se budou připojovat k. Vyberte podsíť hello virtuální privátní sítě (VPN) nebo Azure ExpressRoute virtuální sítě toouse tooconnect hello virtuálního počítače tooyour do místní sítě. Hello ID obvykle vypadá takto: /subscriptions/&lt;id předplatného > /resourceGroups/&lt;název skupiny prostředků > /providers/Microsoft.Network/virtualNetworks/&lt;název virtuální sítě > /subnets/&lt;název podsítě >

3. **Podmínky a ujednání**:  
    Přečtěte si a přijměte právní podmínky hello.

4.  Vyberte **nákupu**.

ve výchozím nastavení je nasazené Hello agenta virtuálního počítače Azure, při použití bitové kopie z hello Azure Marketplace.

#### <a name="configure-proxy-settings"></a>Konfigurace nastavení proxy serveru
V závislosti na konfiguraci vaší místní síti bude pravděpodobně nutné tooset až hello proxy na vašem virtuálním počítači. Pokud virtuální počítač je připojený tooyour do místní sítě prostřednictvím sítě VPN nebo ExpressRoute, hello virtuálních počítačů nemusí být schopný tooaccess hello Internetu a nebude se moct toodownload hello požadované rozšíření nebo shromažďovat data monitorování. Další informace najdete v tématu [nakonfigurovat proxy server hello][deployment-guide-configure-proxy].

#### <a name="join-a-domain-windows-only"></a>Připojení k doméně (jenom Windows)
Pokud vaše nasazení Azure je instance služby Active Directory nebo DNS místně připojených tooan prostřednictvím Azure připojení site-to-site VPN nebo ExpressRoute (to se označuje jako *mezi různými místy* v [plánování virtuálních počítačů Azure a implementaci pro SAP NetWeaver][planning-guide]), očekává se, že hello virtuálního počítače je připojení k místní doméně. Další informace o požadavcích pro tuto úlohu najdete v tématu [doméně virtuálního počítače tooan místní (jenom Windows)][deployment-guide-4.3].

#### <a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Konfigurace monitorování
zda SAP podporuje prostředí nastavit hello rozšíření monitorování Azure pro SAP, jak je popsáno v toobe [hello konfigurovat Azure rozšířené monitorování rozšíření pro SAP][deployment-guide-4.5]. Kontrola předpokladů pro hello pro SAP monitorování a požadovaná minimální verze SAP jádra a Agent hostitele SAP, v hello prostředky uvedené v [SAP prostředky][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Monitorování kontroly
Zkontrolujte, zda monitorování funguje, jak je popsáno v [kontroly a řešení potíží pro nastavení sledování začátku do konce][deployment-guide-troubleshooting-chapter].

#### <a name="post-deployment-steps"></a>Kroky po nasazení
Po vytvoření je nasazená hello virtuálních počítačů a hello virtuálních počítačů, musíte tooinstall hello požadované softwarové součásti v hello virtuálních počítačů. Z důvodu hello nasazení a software instalace pořadí v tomto typu nasazení virtuálního počítače musí být toobe hello software nainstalovaný již k dispozici, buď v Azure, na jiný počítač nebo jako disk, který lze připojit. Nebo, zvažte použití mezi různými místy scénář, ve které připojení toohello místní prostředky (instalace sdílené složky) je zadána.

Po nasazení virtuálního počítače v Azure, postupujte podle hello stejné pokyny a nástroje tooinstall hello SAP softwaru na vašem virtuálním počítači, jako je byste v místním prostředí. software SAP tooinstall na virtuální počítač Azure, jak SAP a společnosti Microsoft, abyste nahrát a uložit hello SAP instalačním médiu na Azure virtuálních pevných disků nebo disků spravované nebo vytvořit virtuální počítač Azure, který funguje jako souborový server, který má všechny hello vyžaduje SAP instalačního média.

### <a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Scénář 2: Nasazení virtuálního počítače s vlastní image pro SAP
Vzhledem k tomu, že různé verze operačního systému nebo databázového systému požadavky na jiné opravy, nemusí hello bitových kopií, které můžete najít v hello Azure Marketplace odpovídá vašim potřebám. Místo toho můžete toocreate virtuální počítač pomocí vlastní bitovou kopii operačního systému nebo databázového systému virtuálního počítače, kterou můžete nasadit znovu později.
Použijete jiný postup toocreate privátní bitové kopie pro Linux než toocreate, jednu pro systém Windows.

- - -
> ![Windows][Logo_Windows] Windows
>
> tooprepare bitovou kopii systému Windows, který můžete použít toodeploy více virtuálních počítačů, nastavení systému Windows hello (jako jsou SID systému Windows a název hostitele) musí být abstrahované nebo zobecněn na hello místní počítač. Můžete použít [sysprep](https://msdn.microsoft.com/library/hh825084.aspx) toodo to.
>
> ![Linux][Logo_Linux] Linux
>
> tooprepare bitovou kopii systému Linux, které můžete použít toodeploy více virtuálních počítačů, musí být některá nastavení Linux hello abstraktní nebo zobecněný na místní počítač. Můžete použít `waagent -deprovision` toodo to. Další informace najdete v tématu [zachytit virtuální počítač Linux spuštěné v Azure] [ virtual-machines-linux-capture-image] a hello [Azure Linux agent uživatelská příručka][virtual-machines-linux-agent-user-guide-command-line-options].
>
>

- - -
Příprava a vytvořit vlastní image a pak jeho pomocí toocreate více nové virtuální počítače. To je popsáno v [virtuální počítače Azure plánování a implementace pro SAP NetWeaver][planning-guide]. Nastavení databáze obsahu, buď pomocí Správce zřizování softwaru SAP tooinstall nového systému SAP (obnoví ze zálohy z disku, který je připojený toohello virtuální počítač) nebo přímo obnovení ze zálohy databáze ze služby Azure storage, pokud vaše databázového systému podporuje. Další informace najdete v tématu [databázového systému virtuální počítače Azure nasazení pro SAP NetWeaver][dbms-guide]. Pokud jste již nainstalovali systému SAP na váš místní virtuální počítač (hlavně u dvouvrstvá systémy), můžete upravit nastavení systému SAP hello po nasazení hello hello virtuálního počítače Azure pomocí postupu hello přejmenovat systému nepodporuje zřizování softwaru SAP Správce (Poznámka SAP [1619720]). Jinak můžete nainstalovat hello SAP software poté, co nasadíte hello virtuálního počítače Azure.

Hello následující vývojový diagram znázorňuje hello SAP konkrétní pořadí kroků pro nasazení virtuálního počítače z vlastní image:

![Vývojový diagram nasazení virtuálního počítače pro systémy SAP pomocí bitové kopie virtuálních počítačů v privátní Marketplace][deployment-guide-figure-300]

#### <a name="create-a-virtual-machine-by-using-hello-azure-portal"></a>Vytvoření virtuálního počítače pomocí hello portálu Azure
Nejjednodušší způsob, jak toocreate Hello nový virtuální počítač z bitové kopie disku spravovat je pomocí hello portálu Azure. Další informace o tom, jak toocreate spravovat Image disku, přečtěte si [zaznamenat bitovou kopii spravované zobecněný virtuálního počítače v Azure](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)

1.  Přejděte příliš<https://ms.portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2Fimages>. Nebo, v nabídce portálu Azure hello, vyberte **bitové kopie**.
2.  Vyberte hello spravované disku image má toodeploy a klikněte na **vytvoření virtuálního počítače**

Hello Průvodce vás provede nastavením hello požadované parametry toocreate hello virtuálního počítače, kromě tooall požadované prostředky, jako je síťová rozhraní a účty úložiště. Některé z těchto parametrů jsou:

1. **Základy**:
 * **Název**: hello název prostředku hello (hello název virtuálního počítače).
 * **Typ disku virtuálního počítače**: Vyberte typ disku hello hello operačního systému disku. Pokud chcete toouse Storage úrovně Premium pro datové disky, doporučujeme používat úložiště úrovně Premium pro disk operačního systému hello také.
 * **Uživatelské jméno a heslo** nebo **veřejný klíč SSH**: Zadejte hello uživatelské jméno a heslo hello uživatele, který se vytvoří během zřizování hello. Pro virtuální počítač s Linuxem můžete zadat veřejný klíč Secure Shell (SSH) hello použít toosign v toohello počítače.
 * **Předplatné**: Vyberte předplatné hello má toouse tooprovision hello nového virtuálního počítače.
 * **Skupina prostředků**: hello název skupiny prostředků hello hello virtuálních počítačů. Můžete zadat buď název hello nový prostředek skupiny nebo hello název skupiny prostředků, která již existuje.
 * **Umístění**: kde toodeploy hello nového virtuálního počítače. Pokud chcete tooconnect hello virtuálního počítače tooyour do místní sítě, ujistěte se, že jste vybrali hello umístění hello virtuální sítě, která se připojuje Azure tooyour do místní sítě. Další informace najdete v tématu [sítě Microsoft Azure] [ planning-guide-microsoft-azure-networking] v [virtuální počítače Azure plánování a implementace pro SAP NetWeaver] [ planning-guide].
2. **Velikost**:

     Seznam podporovaných typů virtuálních počítačů, viz poznámka SAP [1928533]. Ujistěte se, zda že jste vybrali správný typ virtuálního počítače hello, pokud chcete, aby toouse Azure Premium Storage. Ne všechny typy virtuálních počítačů podporují službu Premium Storage. Další informace najdete v tématu [úložiště: Microsoft Azure Storage a datové disky] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] a [Azure Premium Storage] [ planning-guide-azure-premium-storage] v [ Azure virtuálních počítačů, plánování a implementace pro SAP NetWeaver][planning-guide].

3. **Nastavení**:
  * **Úložiště**
    * **Typ disku**: Vyberte typ disku hello hello operačního systému disku. Pokud chcete toouse Storage úrovně Premium pro datové disky, doporučujeme používat úložiště úrovně Premium pro disk operačního systému hello také.
    * **Používat spravované disky**: Pokud chcete spravovat disky toouse, vyberte možnost Ano. Další informace o spravovaných disků, naleznete v kapitole [spravované disky] [ planning-guide-managed-disks] v příručce plánování na hello.
  * **Síť**
    * **Virtuální síť** a **podsíť**: toointegrate hello virtuální počítač s intranetu, vyberte hello virtuální síť, která je připojená tooyour do místní sítě.
    * **Veřejná IP adresa**: Vyberte hello veřejnou IP adresu, mají toouse, nebo zadejte parametry toocreate novou veřejnou IP adresu. Můžete veřejné tooaccess adresu IP virtuálního počítače přes hello Internet. Ujistěte se také vytvořit sítě zabezpečení skupiny toohelp zabezpečený přístup tooyour virtuálního počítače.
    * **Skupina zabezpečení sítě**: Další informace najdete v tématu [řízení toku provozu sítě s skupin zabezpečení sítě][virtual-networks-nsg].
  * **Rozšíření**: přidáním toohello nasazení můžete nainstalovat rozšíření virtuálního počítače. Není nutné tooadd rozšíření v tomto kroku. Podpora SAP vyžaduje rozšíření Hello jsou nainstalovány později. Naleznete v kapitole [hello konfigurovat Azure rozšířené monitorování rozšíření pro SAP] [ deployment-guide-4.5] v této příručce.
  * **Vysoká dostupnost**: Vyberte skupinu dostupnosti, nebo zadejte hello parametry toocreate dostupnost nové nastavení. Další informace najdete v tématu [skupiny dostupnosti Azure][planning-guide-3.2.3].
  * **Monitorování**
    * **Spouštění diagnostiky**: můžete vybrat **zakázat** pro Diagnostika spouštění.
    * **Diagnostika hostovaného operačního systému**: můžete vybrat **zakázat** pro monitorování diagnostiky.

4. **Souhrn**:

  Zkontrolujte váš výběr a potom vyberte **OK**.

Virtuální počítač nasazen ve skupině prostředků hello, který jste vybrali.
#### <a name="create-a-virtual-machine-by-using-a-template"></a>Vytvoření virtuálního počítače pomocí šablony
toocreate nasazení s použitím privátní bitové kopie operačního systému z hello portál Azure, použijte jednu z následujících šablon SAP hello. Tyto šablony jsou publikovány v hello [úložiště GitHub šablon azure rychlý Start][azure-quickstart-templates-github]. Můžete také můžete ručně vytvořit virtuální počítač pomocí [prostředí PowerShell][virtual-machines-upload-image-windows-resource-manager].

* [**Šablona dvouvrstvá konfigurace (jenom jeden virtuální počítač)** (sap-2vrstvy uživatel image)][sap-templates-2-tier-user-image]

  Tuto šablonu použít toocreate dvouvrstvá systému pomocí jenom jeden virtuální počítač.
* [**Šablona dvouvrstvá konfigurace (jenom jeden virtuální počítač) - spravované bitové kopie disku** (sap-2-tier-user-image-md)][sap-templates-2-tier-user-image-md]

  Tuto šablonu použít toocreate dvouvrstvá systému pomocí jenom jeden virtuální počítač a bitovou kopii disku spravované.
* [**Šablona třívrstvá konfigurace (více virtuálních počítačů)** (sap-3vrstvy uživatel image)][sap-templates-3-tier-user-image]

  Tuto šablonu použít toocreate třívrstvá systému pomocí více virtuálních počítačů nebo vlastní bitovou kopii operačního systému.
* [**Šablona třívrstvá konfigurace (více virtuálních počítačů) - spravované bitové kopie disku** (sap-3-tier-user-image-md)][sap-templates-3-tier-user-image-md]

  toocreate třívrstvá systému pomocí více virtuálních počítačů nebo vlastní image operačního systému a bitové kopie disku spravované, pomocí této šablony.

V hello portálu Azure zadejte následující parametry šablony hello hello:

1. **Základy**:
  * **Předplatné**: hello předplatné toouse toodeploy hello šablony.
  * **Skupina prostředků**: hello prostředků skupiny toouse toodeploy hello šablony. Můžete vytvořit novou skupinu prostředků nebo vyberte existující skupinu prostředků v předplatném hello.
  * **Umístění**: kde toodeploy hello šablony. Pokud jste vybrali existující skupinu prostředků, použije se hello umístění této skupiny prostředků.
2. **Nastavení**:
  * **ID systému SAP**: hello ID systému SAP.
  * **Typ operačního systému**: hello typ operačního systému, které chcete toodeploy (Windows nebo Linux).
  * **Velikost systému SAP**: hello velikost hello systému SAP.

    poskytuje Hello počet protokoly SAP hello nový systém. Pokud si nejste jisti, kolik protokoly SAP hello systém vyžaduje, požádejte SAP technologie partnera nebo systémový integrátor.
  * **Dostupnost systému** (pouze šablony třívrstvá): hello dostupnosti systému.

    Vyberte **HA** konfigurace, který je vhodný pro instalaci s vysokou dostupností. Vytvoří se dvou databázové servery a dva servery pro ASC.
  * **Typ úložiště** (pouze šablony dvouvrstvá): hello typ toouse úložiště.

    U větších systémů důrazně doporučujeme pomocí Azure Premium Storage. Další informace o typech úložiště najdete v tématu hello následující prostředky:
      * [Používání úložiště Azure Premium SSD pro instanci databázového systému SAP][2367194]
      * [Microsoft Azure Storage] [ dbms-guide-2.3] v [databázového systému virtuální počítače Azure nasazení pro SAP NetWeaver][dbms-guide]
      * [Úložiště Premium: Vysoce výkonné úložiště pro úlohy virtuálního počítače Azure][storage-premium-storage-preview-portal]
      * [Úvod tooMicrosoft Azure Storage][storage-introduction]
  * **Uživatelská image URI virtuálního pevného disku** (pouze nespravovaná disku image šablony): hello URI hello privátní bitové kopie operačního systému virtuálního pevného disku, například https://&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.
  * **Účet úložiště bitových kopií uživatele** (pouze nespravovaná disku image šablony): hello název účtu úložiště hello hello privátní bitové kopie operačního systému se uloží, například &lt;accountname > v https://&lt;accountname >. BLOB.Core.Windows.NET/vhds/userimage.VHD.
  * **userImageId** (pouze spravovaných disků na image šablony): Id hello image disku spravované chcete toouse
  * **Uživatelské jméno správce** a **heslo správce**: hello uživatelské jméno a heslo.

    Pro přihlášení toohello virtuální počítač se vytvoří nového uživatele.
  * **Nový nebo existující podsíť**: Určuje, zda se vytvoří nová virtuální síť a podsíť, nebo se používá existující podsítí. Pokud již máte virtuální síť, která je připojená tooyour do místní sítě, vyberte **existující**.
  * **ID podsítě**: ID hello hello podsíť toowhich hello virtuálních počítačů se budou připojovat k. Vyberte podsíť hello VPN nebo ExpressRoute virtuální sítě toouse tooconnect hello virtuálního počítače tooyour místní sítě. Hello ID obvykle vypadá takto:

    /subscriptions/&lt;id předplatného > /resourceGroups/&lt;název skupiny prostředků > /providers/Microsoft.Network/virtualNetworks/&lt;název virtuální sítě > /subnets/&lt;název podsítě >

3. **Podmínky a ujednání**:  
    Přečtěte si a přijměte právní podmínky hello.

4.  Vyberte **nákupu**.

#### <a name="install-hello-vm-agent-linux-only"></a>Nainstalujte hello agenta virtuálního počítače (pouze Linux)
popsané v předcházející části hello šablony hello toouse, hello agenta systému Linux musí být nainstalována v hello uživatelská image nebo hello nasazení se nezdaří. Stáhněte a nainstalujte hello agenta virtuálního počítače v hello uživatelská image, jak je popsáno v [stáhnout, nainstalovat a povolit hello agenta virtuálního počítače Azure][deployment-guide-4.4]. Pokud nepoužijete hello šablony, také můžete nainstalovat později hello agenta virtuálního počítače.

#### <a name="join-a-domain-windows-only"></a>Připojení k doméně (jenom Windows)
Pokud vaše nasazení Azure je instance služby Active Directory nebo DNS místně připojených tooan prostřednictvím Azure připojení site-to-site VPN nebo Azure ExpressRoute (to se označuje jako *mezi různými místy* v [virtuální počítače Azure plánování a implementace pro SAP NetWeaver][planning-guide]), očekává se, že hello virtuálního počítače je připojení k místní doméně. Další informace o aspektech týkajících se pro tento krok najdete v tématu [doméně virtuálního počítače tooan místní (jenom Windows)][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Konfigurace nastavení proxy serveru
V závislosti na konfiguraci vaší místní síti bude pravděpodobně nutné tooset až hello proxy na vašem virtuálním počítači. Pokud virtuální počítač je připojený tooyour do místní sítě prostřednictvím sítě VPN nebo ExpressRoute, hello virtuálních počítačů nemusí být schopný tooaccess hello Internetu a nebude se moct toodownload hello požadované rozšíření nebo shromažďovat data monitorování. Další informace najdete v tématu [nakonfigurovat proxy server hello][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>Konfigurace monitorování
zda SAP podporuje prostředí nastavit hello rozšíření monitorování Azure pro SAP, jak je popsáno v toobe [hello konfigurovat Azure rozšířené monitorování rozšíření pro SAP][deployment-guide-4.5]. Kontrola předpokladů pro hello pro SAP monitorování a požadovaná minimální verze SAP jádra a Agent hostitele SAP, v hello prostředky uvedené v [SAP prostředky][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Monitorování kontroly
Zkontrolujte, zda monitorování funguje, jak je popsáno v [kontroly a řešení potíží pro nastavení sledování začátku do konce][deployment-guide-troubleshooting-chapter].


### <a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Scénář 3: Přesun místní počítač pomocí virtuálního pevného disku není zobecněný Azure SAP
V tomto scénáři plánujete toomove konkrétního systému SAP z tooAzure místní prostředí. Můžete to provést tím, že nahrajete hello virtuálního pevného disku, který má hello operačního systému, binární soubory hello SAP, a nakonec hello binární soubory databázového systému plus hello virtuální pevné disky s hello data a soubory hello databázového systému, tooAzure protokolu. Na rozdíl od hello scénář popsaný v [scénář 2: nasazení virtuálního počítače s vlastní image pro SAP][deployment-guide-3.3], v takovém případě zachovat hello název hostitele, identifikátor SID SAP, a SAP uživatelských účtů v hello virtuálního počítače Azure, protože byly nakonfigurovaný v místním prostředí hello. Není nutné toogeneralize hello operačního systému. Tento scénář se vztahuje nejčastěji toocross místní scénáře, kde součástí hello SAP šířku běží místně a jeho součástí běží na Azure.

V tomto scénáři hello agenta virtuálního počítače je **není** automaticky nainstalovaná při nasazení. Protože hello agenta virtuálního počítače a hello Azure rozšířené monitorování rozšíření pro SAP jsou požadované toorun SAP NetWeaver v Azure, budete potřebovat toodownload, instalaci a povolit obě součásti ručně po vytvoření hello virtuálního počítače.

Další informace o hello agenta virtuálního počítače Azure najdete v části hello následující prostředky.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Přehled služby Azure agenta virtuálního počítače][virtual-machines-windows-agent-user-guide]
>
> ![Linux][Logo_Linux] Linux
>
> [Uživatelská příručka k Azure Linux Agent][virtual-machines-linux-agent-user-guide]
>
>

- - -

Hello následující vývojový diagram znázorňuje hello pořadí kroků pro přesun místní počítač pomocí virtuálního pevného disku není zobecněný Azure:

![Vývojový diagram nasazení virtuálního počítače pro SAP systémy pomocí disku virtuálního počítače][deployment-guide-figure-400]

Pokud je hello disk již nahrán a definované v Azure (najdete v části [virtuální počítače Azure plánování a implementace pro SAP NetWeaver][planning-guide]), proveďte hello úkoly popsané v tématu hello vedle několik oddílů.

#### <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače
toocreate nasazení pomocí privátní disk operačního systému, prostřednictvím hello portál Azure, použijte šablonu SAP hello publikované v hello [úložiště GitHub šablon azure rychlý Start][azure-quickstart-templates-github]. Také můžete ručně vytvoříte virtuální počítač pomocí prostředí PowerShell.

* [**Šablona dvouvrstvá konfigurace (jenom jeden virtuální počítač)** (sap-2vrstvy uživatel disk)][sap-templates-2-tier-os-disk]

  Tuto šablonu použít toocreate dvouvrstvá systému pomocí jenom jeden virtuální počítač.
* [**Šablona dvouvrstvá konfigurace (jenom jeden virtuální počítač) - spravované disku** (sap-2-tier-user-disk-md)][sap-templates-2-tier-os-disk-md]

  Tuto šablonu použít toocreate dvouvrstvá systému pomocí jenom jeden virtuální počítač a na Disk spravovaný.

V hello portálu Azure zadejte následující parametry šablony hello hello:

1. **Základy**:
  * **Předplatné**: hello předplatné toouse toodeploy hello šablony.
  * **Skupina prostředků**: hello prostředků skupiny toouse toodeploy hello šablony. Můžete vytvořit novou skupinu prostředků nebo vyberte existující skupinu prostředků v předplatném hello.
  * **Umístění**: kde toodeploy hello šablony. Pokud jste vybrali existující skupinu prostředků, použije se hello umístění této skupiny prostředků.
2. **Nastavení**:
  * **ID systému SAP**: hello ID systému SAP.
  * **Typ operačního systému**: hello typ operačního systému, které chcete toodeploy (Windows nebo Linux).
  * **Velikost systému SAP**: hello velikost hello systému SAP.

    poskytuje Hello počet protokoly SAP hello nový systém. Pokud si nejste jisti, kolik protokoly SAP hello systém vyžaduje, požádejte SAP technologie partnera nebo systémový integrátor.
  * **Typ úložiště** (pouze šablony dvouvrstvá): hello typ toouse úložiště.

    U větších systémů důrazně doporučujeme pomocí Azure Premium Storage. Další informace o typech úložiště najdete v tématu hello následující prostředky:
      * [Používání úložiště Azure Premium SSD pro instanci databázového systému SAP][2367194]
      * [Microsoft Azure Storage] [ dbms-guide-2.3] v [nasazení virtuálního počítače Azure databázového systému pro SAP NetWeaver][dbms-guide]
      * [Úložiště Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure][storage-premium-storage-preview-portal]
      * [Úvod tooMicrosoft Azure Storage][storage-introduction]
  * **Identifikátor URI virtuálního pevného disku na disku operačního systému** (pouze nespravovaná disku šablony): hello URI hello privátní disk operačního systému, například https://&lt;accountname >.blob.core.windows.net/vhds/osdisk.vhd.
  * **Id disku spravované disku operačního systému** (pouze spravované disku šablony): hello Id hello disk spravované disku operačního systému, /subscriptions/92d102f7-81a5-4df7-9877-54987ba97dd9/resourceGroups/group/providers/Microsoft.Compute/disks/WIN
  * **Nový nebo existující podsíť**: Určuje, zda se vytvoří nová virtuální síť a podsíť, nebo se používá existující podsítí. Pokud již máte virtuální síť, která je připojená tooyour do místní sítě, vyberte **existující**.
  * **ID podsítě**: ID hello hello podsíť toowhich hello virtuálních počítačů se budou připojovat k. Vyberte podsíť hello VPN nebo Azure ExpressRoute virtuální sítě toouse tooconnect hello virtuálního počítače tooyour místní sítě. Hello ID obvykle vypadá takto:

    /subscriptions/&lt;id předplatného > /resourceGroups/&lt;název skupiny prostředků > /providers/Microsoft.Network/virtualNetworks/&lt;název virtuální sítě > /subnets/&lt;název podsítě >

3. **Podmínky a ujednání**:  
    Přečtěte si a přijměte právní podmínky hello.

4.  Vyberte **nákupu**.

#### <a name="install-hello-vm-agent"></a>Nainstalujte hello agenta virtuálního počítače
hello hello toouse šablony popsané v předchozí části, hello agenta virtuálního počítače musí být nainstalován na disku hello operačního systému nebo hello nasazení se nezdaří. Stáhněte a nainstalujte hello agenta virtuálního počítače v hello virtuální počítač, jak je popsáno v [stáhnout, nainstalovat a povolit hello agenta virtuálního počítače Azure][deployment-guide-4.4].

Pokud nepoužijete hello šablony popsané v předcházející části hello, můžete taky nainstalovat agenta virtuálního počítače hello později.

#### <a name="join-a-domain-windows-only"></a>Připojení k doméně (jenom Windows)
Pokud vaše nasazení Azure je instance služby Active Directory nebo DNS místně připojených tooan prostřednictvím Azure připojení site-to-site VPN nebo ExpressRoute (to se označuje jako *mezi různými místy* v [plánování virtuálních počítačů Azure a implementaci pro SAP NetWeaver][planning-guide]), očekává se, že hello virtuálního počítače je připojení k místní doméně. Další informace o požadavcích pro tuto úlohu najdete v tématu [doméně virtuálního počítače tooan místní (jenom Windows)][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Konfigurace nastavení proxy serveru
V závislosti na konfiguraci vaší místní síti bude pravděpodobně nutné tooset až hello proxy na vašem virtuálním počítači. Pokud virtuální počítač je připojený tooyour do místní sítě prostřednictvím sítě VPN nebo ExpressRoute, hello virtuálních počítačů nemusí být schopný tooaccess hello Internetu a nebude se moct toodownload hello požadované rozšíření nebo shromažďovat data monitorování. Další informace najdete v tématu [nakonfigurovat proxy server hello][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>Konfigurace monitorování
zda SAP podporuje prostředí nastavit hello rozšíření monitorování Azure pro SAP, jak je popsáno v toobe [hello konfigurovat Azure rozšířené monitorování rozšíření pro SAP][deployment-guide-4.5]. Kontrola předpokladů pro hello pro SAP monitorování a požadovaná minimální verze SAP jádra a Agent hostitele SAP, v hello prostředky uvedené v [SAP prostředky][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Monitorování kontroly
Zkontrolujte, zda monitorování funguje, jak je popsáno v [kontroly a řešení potíží pro nastavení sledování začátku do konce][deployment-guide-troubleshooting-chapter].

## <a name="update-hello-monitoring-configuration-for-sap"></a>Aktualizovat hello konfiguraci monitorování pro SAP
Aktualizujte konfiguraci monitorování hello SAP v některém z hello následující scénáře:
* společné týmu Microsoft/SAP Hello rozšiřuje možnosti monitorování hello a požadavky čítače více nebo méně.
* Microsoft zavádí novou verzi hello základní Azure infrastrukturu, která poskytuje hello data monitorování a hello rozšíření rozšířené monitorování Azure pro SAP toobe potřeby přizpůsobit toothose změny.
* Připojit další data tooyour disky virtuálního počítače Azure nebo odebrat datový disk. V tomto scénáři aktualizujte kolekci hello související s úložištěm dat. Změna konfiguraci přidáním nebo odstraněním koncových bodů nebo přiřazení IP adresy tooa virtuálního počítače nemá vliv na konfiguraci monitorování hello.
* Změníte hello velikost virtuálního počítače Azure, například z velikost A5 tooany jiných velikost virtuálního počítače.
* Můžete přidat nové sítě tooyour rozhraní virtuálního počítače Azure.

tooupdate nastavení monitorování, aktualizace hello monitorování infrastruktury hello následující kroky [hello konfigurovat Azure rozšířené monitorování rozšíření pro SAP][deployment-guide-4.5].

## <a name="detailed-tasks-for-sap-software-deployment"></a>Podrobný popis úkolů pro nasazení softwaru SAP
Tato část popsala postup při provádění konkrétní úlohy v procesu konfigurace a nasazení hello.

### <a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Nasazení rutin prostředí Azure PowerShell
1.  Přejděte příliš[Microsoft Azure stáhne](https://azure.microsoft.com/downloads/).
2.  V části **nástroje příkazového řádku**v části **prostředí PowerShell**, vyberte **nainstalovat Windows**.
3.  V hello Microsoft stáhnout Manager dialog box se pro hello stáhnout soubor (například WindowsAzurePowershellGet.3f.3f.3fnew.exe), vyberte **spustit**.
4.  toorun instalačního programu webové platformy (Microsoft Web PI), vyberte **Ano**.
5.  Stránka, která vypadá to, se zobrazuje:

  ![Stránka Instalace rutin prostředí Azure PowerShell][deployment-guide-figure-500]<a name="figure-5"></a>

6.  Vyberte **nainstalovat**a pak přijměte licenční podmínky softwaru společnosti Microsoft hello.
7.  Prostředí PowerShell je nainstalovaný. Vyberte **Dokončit** Průvodce instalací tooclose hello.

Kontrola často rutiny prostředí PowerShell toohello aktualizace, které obvykle jsou aktualizovány jednou měsíčně. Hello nejjednodušší způsob, jak toocheck aktualizací je toodo hello předcházející instalační kroky, až toohello instalace stránky, která je uvedená v kroku 5. číslo datum a vydání verze Hello rutin hello jsou zahrnuty v hello stránka zobrazená v kroku 5. Pokud není uvedeno jinak v Poznámka SAP [1928533] nebo Poznámka SAP [2015553], doporučujeme pracovat hello nejnovější verzi rutin prostředí Azure PowerShell.

toocheck hello verzi hello rutin Azure Powershellu, které jsou nainstalovány v počítači, spusťte tento příkaz prostředí PowerShell:
```powershell
(Get-Module AzureRm.Compute).Version
```
výsledek Hello vypadá takto:

![Výsledek kontroly verze rutiny prostředí Azure PowerShell][deployment-guide-figure-600]
<a name="figure-6"></a>

Pokud je verze Azure rutiny hello v počítači nainstalována aktuální verze hello, první stránka Průvodce instalací hello hello znamenají, že přidáním **(nainstalována)** název produktu toohello (viz následující snímek obrazovky hello). Vaše rutiny prostředí PowerShell Azure jsou aktuální. tooclose hello Průvodce instalací, vyberte **ukončení**.

![Jsou nainstalované instalační stránku rutin prostředí Azure PowerShell, která určuje, že hello nejnovější verzi rutin prostředí Azure PowerShell][deployment-guide-figure-700]
<a name="figure-7"></a>

### <a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Nasazení rozhraní příkazového řádku Azure
1.  Přejděte příliš[Microsoft Azure stáhne](https://azure.microsoft.com/downloads/).
2.  V části **nástroje příkazového řádku**v části **rozhraní příkazového řádku Azure**, vyberte hello **nainstalovat** odkaz pro váš operační systém.
3.  V hello Microsoft stáhnout Manager dialog box se pro hello stáhnout soubor (například WindowsAzureXPlatCLI.3f.3f.3fnew.exe), vyberte **spustit**.
4.  toorun instalačního programu webové platformy (Microsoft Web PI), vyberte **Ano**.
5.  Stránka, která vypadá to, se zobrazuje:

  ![Stránka Instalace rutin prostředí Azure PowerShell][deployment-guide-figure-500]<a name="figure-5"></a>

6.  Vyberte **nainstalovat**a pak přijměte licenční podmínky softwaru společnosti Microsoft hello.
7.  Rozhraní příkazového řádku Azure je nainstalovaný. Vyberte **Dokončit** Průvodce instalací tooclose hello.

Kontrola často aktualizace tooAzure rozhraní příkazového řádku, která obvykle se aktualizuje jednou měsíčně. Hello nejjednodušší způsob, jak toocheck aktualizací je toodo hello předcházející instalační kroky, až toohello instalace stránky, která je uvedená v kroku 5.

toocheck hello verzi rozhraní příkazového řádku Azure, který je nainstalován v počítači, spusťte tento příkaz:
```
azure --version
```

výsledek Hello vypadá takto:

![Výsledek kontroly verze rozhraní příkazového řádku Azure][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Připojení k virtuální počítač tooan místní doméně (jenom Windows)
Pokud nasadíte virtuální počítače SAP ve scénáři mezi různými místy, kde jsou rozšířené místní služby Active Directory a DNS v Azure, očekává se, že hello virtuální počítače jsou připojení k místní doméně. Hello podrobné kroky trvat toojoin místní domény tooan virtuálních počítačů a hello toobe další software potřebný místní doméně se liší podle zákazníka. Obvykle toojoin tooan virtuálního počítače místní domény, musíte tooinstall další software, jako je antimalwarový software a zálohování nebo monitorování softwaru.

V tomto scénáři musíte taky toomake jistotu, že pokud nastavení internetového proxy serveru jsou povinná, pokud virtuální počítač připojen do domény ve vašem prostředí, Windows hello místní systémový účet (S-1-5-18) v hello hosta virtuálního počítače má hello stejné nastavení proxy serveru. Nejjednodušší možnost Hello je tooforce hello proxy pomocí zásad skupiny, která se vztahuje toosystems v doméně hello domény.

### <a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Stáhnout, nainstalovat a povolit hello agenta virtuálního počítače Azure
Pro virtuální počítače, které jsou nasazeny z image operačního systému, který není zobecněný (například obrázek, který není v nástroji pro přípravu systému Windows nebo nástroje sysprep, hello pocházejí) budete potřebovat toomanually stažení, instalace a povolit hello agenta virtuálního počítače Azure.

Pokud nasadíte virtuální počítač z Azure Marketplace hello, tento krok není povinný. Bitové kopie z Azure Marketplace hello už máte hello agenta virtuálního počítače Azure.

#### <a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows
1.  Stáhněte si hello agenta virtuálního počítače Azure:
  1.  Stáhnout hello [instalační balíček agenta virtuálního počítače Azure](https://go.microsoft.com/fwlink/?LinkId=394789).
  2.  Ukládání balíčku MSI agenta virtuálního počítače hello místně na osobním počítači nebo serveru.
2.  Nainstalujte agenta virtuálního počítače Azure hello:
  1.  Připojit toohello nasazuje virtuální počítač Azure s použitím protokolu RDP (Remote Desktop).
  2.  Otevřete okno Průzkumníka Windows na hello virtuálních počítačů a vyberte hello cílový adresář pro soubor MSI hello hello agenta virtuálního počítače.
  3.  Přetáhněte soubor MSI Instalační program agenta virtuálního počítače Azure hello z vaší místní počítač nebo server toohello cílový adresář hello agenta virtuálního počítače na hello virtuálních počítačů.
  4.  Poklikejte na soubor MSI hello na hello virtuálních počítačů.
3.  Pro virtuální počítače, které jsou připojené k tooon místní domény, ujistěte se, že případný nastavení proxy Internetu toohello účet místního systému Windows (S-1-5-18) také použít v hello virtuálních počítačů, jak je popsáno v [nakonfigurovat proxy server hello] [ deployment-guide-configure-proxy]. Hello agenta virtuálního počítače běží v tomto kontextu a musí mít tooconnect tooAzure toobe.

Žádná interakce s uživatelem je požadovaná tooupdate hello agenta virtuálního počítače Azure. Hello agenta virtuálního počítače se automaticky aktualizuje a nevyžaduje restartování virtuálního počítače.

#### <a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux
Použijte následující příkazy tooinstall hello agenta virtuálního počítače pro Linux hello:

* **SUSE Linux Enterprise Server (SLES)**

  ```
  sudo zypper install WALinuxAgent
  ```

* **Red Hat Enterprise Linux (RHEL) nebo Oracle Linux**

  ```
  sudo yum install WALinuxAgent
  ```

Pokud je již nainstalován hello agent, tooupdate hello Azure Linux Agent hello kroků popsaných v [aktualizace hello Azure Linux Agent na nejnovější verzi toohello virtuálních počítačů z Githubu][virtual-machines-linux-update-agent].

### <a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Nakonfigurovat proxy server hello
Hello postup, kterým tooconfigure hello proxy v systému Windows se liší od hello způsob, jak nakonfigurovat hello proxy v systému Linux.

#### <a name="windows"></a>Windows
Nastavení proxy serveru musí být nastavit správně pro hello účet Local System, tooaccess hello Internetu. Pokud vaše nastavení proxy serveru nejsou nastavené zásady skupiny, můžete nakonfigurovat nastavení hello hello místního systémového účtu.

1. Přejděte příliš**spustit**, zadejte **gpedit.msc**a potom vyberte **Enter**.
2. Vyberte **konfigurace počítače** > **šablony pro správu** > **součásti systému Windows** > **aplikace Internet Explorer**. Ujistěte se, že nastavení hello **zkontrolujte proxy nastavení podle počítače (nikoli na uživatele)** je zakázána nebo není nakonfigurována.
3. V **ovládací panely**, přejděte příliš**Centrum sítí a sdílení** > **Možnosti Internetu**.
4. Na hello **připojení** karty, vyberte hello **nastavení místní sítě** tlačítko.
5. Vymazat hello **automaticky zjišťovat nastavení** zaškrtávací políčko.
6. Vyberte hello **použít proxy server pro síť LAN** zaškrtněte políčko a potom zadejte hello adresy a portu.
7. Vyberte hello **Upřesnit** tlačítko.
8. V hello **výjimky** zadejte IP adresu, hello **168.63.129.16**. Vyberte **OK**.

#### <a name="linux"></a>Linux
Konfigurace proxy správné hello v souboru konfigurace hello hello Microsoft Azure hostovaného agenta, která se nachází v \\atd\\waagent.conf.

Nastavte hello následující parametry:

1.  **HTTP proxy hostitele**. Například nastavit příliš**proxy.corp.local**.
  ```
  HttpProxy.Host=<proxy host>

  ```
2.  **Port proxy serveru HTTP**. Například nastavit příliš**80**.
  ```
  HttpProxy.Port=<port of hello proxy host>

  ```
3.  Restartujte agenta hello.

  ```
  sudo service waagent restart
  ```

nastavení proxy serveru v Hello \\atd\\waagent.conf platí také toohello požadované rozšíření virtuálního počítače. Pokud chcete, aby toouse hello úložiště Azure, ujistěte se, hello provoz toothese úložiště není projít místního intranetu. Pokud jste vytvořili, uživatelem definované trasy tooenable vynuceného tunelování, ujistěte se, že přidáte trasu, který směruje provoz toohello úložiště přímo toohello Internetu a ne prostřednictvím připojení VPN typu site-to-site.

* **SLES**

  Je také nutné tooadd trasy pro hello IP adres uvedených v \\atd\\regionserverclnt.cfg. Hello následující obrázek znázorňuje příklad:

  ![Vynucené tunelování][deployment-guide-figure-50]


* **RHEL**

  Je také nutné tooadd trasy pro hello IP adresy hostitelů hello uvedených v \\atd\\yum.repos.d\\rhui Vyrovnávání zatížení. Příklad najdete v tématu hello předcházející obrázek.

* **Oracle Linux**

  Neexistují žádné úložiště pro Oracle Linux v Azure. Můžete potřebovat tooconfigure vlastního úložiště pro Oracle Linux nebo použít veřejné úložiště hello.

Další informace o trasy definované uživatelem, najdete v části [trasy definované uživatelem a předávání IP][virtual-networks-udr-overview].

### <a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Konfigurace hello Azure rozšířené monitorování rozšíření pro SAP
Pokud jste připravený hello virtuálního počítače podle [scénáře nasazení virtuálních počítačů pro SAP v Azure][deployment-guide-3], hello Agent virtuálního počítače Azure je nainstalovaný na virtuálním počítači hello. dalším krokem Hello je toodeploy hello Azure rozšířené monitorování rozšíření pro SAP, která je k dispozici v hello rozšíření úložiště Azure v hello datových centrech Azure globální. Další informace najdete v tématu [virtuální počítače Azure plánování a implementace pro SAP NetWeaver][planning-guide-9.1].

Můžete použít PowerShell nebo rozhraní příkazového řádku Azure tooinstall a nakonfigurovat hello Azure rozšířené monitorování rozšíření pro SAP. rozšíření hello tooinstall v systému Windows nebo virtuálního počítače s Linuxem pomocí počítače s Windows najdete v části [prostředí Azure PowerShell][deployment-guide-4.5.1]. rozšíření hello tooinstall na virtuální počítač s Linuxem pomocí stolní počítač Linux, najdete v části [rozhraní příkazového řádku Azure][deployment-guide-4.5.2].

#### <a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Prostředí Azure PowerShell pro systémy Linux a virtuálních počítačů Windows
tooinstall hello Azure rozšířené monitorování rozšíření pro SAP pomocí prostředí PowerShell:

1. Ujistěte se, že jste nainstalovali nejnovější verzi rutin prostředí Azure PowerShell hello hello. Další informace najdete v tématu [rutin nasazení prostředí Azure PowerShell][deployment-guide-4.1].  
2. Spusťte následující rutiny prostředí PowerShell hello.
    Seznam dostupných prostředí, spusťte `commandlet Get-AzureRmEnvironment`. Pokud chcete toouse globální Azure, prostředí je **AzureCloud**. Azure v Číně, vyberte **AzureChinaCloud**.

    ```powershell
    $env = Get-AzureRmEnvironment -Name <name of hello environment>
    Login-AzureRmAccount -Environment $env
    Set-AzureRmContext -SubscriptionName <subscription name>

    Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
    ```

Po zadání vaše data na účtu a identifikovat hello virtuální počítač Azure, skript hello nasadí hello požadované rozšíření a umožňuje hello požadované funkce. To může trvat několik minut.
Další informace o `Set-AzureRmVMAEMExtension`, najdete v části [Set-AzureRmVMAEMExtension][msdn-set-azurermvmaemextension].

![Úspěšné provedení specifické pro SAP Azure rutiny Set-AzureRmVMAEMExtension][deployment-guide-figure-900]

Hello `Set-AzureRmVMAEMExtension` konfigurace nemá všechny hello kroky tooconfigure hostitele monitorování pro SAP.

výstup skriptu Hello zahrnuje hello následující informace:

* Potvrzení, že monitorování disku hello operačního systému a všech dalších datových disků nakonfigurovaný.
* Další dvě zprávy Hello potvrďte hello konfigurace úložiště metrik pro účet konkrétní úložiště.
* Jeden řádek výstupu dává hello stav hello skutečné aktualizace konfigurace monitorování hello.
* Další řádek výstupu potvrdí, že hello konfigurace byl nasazen nebo aktualizován.
* Hello poslední řádek výstupu je informační. Zobrazuje možnosti testování konfigurace monitorování hello.
* toocheck, že byly úspěšně provedeny všechny kroky rozšířené monitorování Azure a že hello infrastruktury Azure poskytuje hello potřebná data, pokračujte kontrolu připravenosti hello hello Azure rozšířené monitorování rozšíření pro SAP, jak je popsáno v [Kontrolu připravenosti pro Azure rozšířené monitorování pro SAP][deployment-guide-5.1].
* Počkejte, než 15 až 30 minut pro Azure Diagnostics toocollect hello relevantní data.

#### <a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure CLI pro virtuální počítače s Linuxem
tooinstall hello Azure rozšířené monitorování rozšíření pro SAP pomocí rozhraní příkazového řádku Azure:

1. Nainstalovat Azure CLI 1.0, jak je popsáno v [hello instalace Azure CLI 1.0][azure-cli].
2. Přihlaste se pomocí účtu Azure:

  ```
  azure login
  ```

3. Přepnutí režimu Resource Manager tooAzure:

  ```
  azure config mode arm
  ```

4. Povolte Azure rozšířené monitorování:

  ```
  azure vm enable-aem <resource-group-name> <vm-name>
  ```

5. Ověřte, že je tento hello rozšíření rozšířené monitorování Azure aktivní na hello virtuální počítač Azure s Linuxem. Zkontrolujte zda text hello souboru \\var\\lib\\AzureEnhancedMonitor\\PerfCounters existuje. Pokud existuje, na příkazovém řádku, spusťte tento příkaz toodisplay shromažďované nástrojem hello Azure rozšířené monitorování:
```
cat /var/lib/AzureEnhancedMonitor/PerfCounters
```

výstup Hello vypadá takto:
```
2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
…
…
```

## <a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Kontroly a řešení potíží pro sledování začátku do konce
Po nasazení virtuálního počítače Azure a nastavit hello relevantní monitorování infrastrukturu Azure, zkontrolujte, zda jsou všechny komponenty hello hello rozšíření rozšířené monitorování Azure funguje podle očekávání.

Spustit kontrolu připravenosti hello hello Azure rozšířené monitorování rozšíření pro SAP, jak je popsáno v [kontrolu připravenosti hello Azure rozšířené monitorování rozšíření pro SAP][deployment-guide-5.1]. Pokud jsou všechny výsledky kontroly připravenosti kladné a všechny relevantní čítače zobrazí OK, Azure monitorování byl úspěšně nastaven. Abyste mohli pokračovat hello instalace agenta SAP hostitele podle popisu v poznámkách k SAP hello v [SAP prostředky][deployment-guide-2.2]. Pokud kontrola připravenosti hello indikuje, že čítače chybí, spusťte kontrolu stavu hello pro hello monitorování infrastrukturu Azure, jak je popsáno v [Kontrola stavu pro konfiguraci Azure monitorování infrastruktury] [ deployment-guide-5.2]. Další možnosti řešení potíží najdete v tématu [monitorování řešení potíží s Azure pro SAP][deployment-guide-5.3].

### <a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Kontrola připravenosti hello Azure rozšířené monitorování rozšíření pro SAP
Tato kontrola zajišťuje, že všechny metriky výkonu, které se zobrazují v aplikaci SAP jsou poskytovány hello základní monitorování infrastruktury Azure.

#### <a name="run-hello-readiness-check-on-a-windows-vm"></a>Spustit kontrolu připravenosti hello na virtuální počítač s Windows

1.  Přihlaste se toohello virtuální počítač Azure (pomocí účtu správce není potřeba).
2.  Otevřete okno příkazového řádku.
3.  Na příkazovém řádku hello změnit hello directory toohello instalační složky hello Azure rozšířené monitorování rozšíření pro SAP: C:\\balíčky\\modulů plug-in\\ Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;verze >\\vyřadit

  Hello *verze* v hello cesta toohello rozšíření monitorování mohou lišit. Pokud uvidíte složek pro více verzí hello rozšíření hello instalační složky zkontrolujte konfiguraci hello hello službu systému AzureEnhancedMonitoring Windows monitorování a potom složku toohello přepínač uvedené jako *tooexecutable cesta* .

  ![Vlastnosti služba spuštěná hello Azure rozšířené monitorování rozšíření pro SAP][deployment-guide-figure-1000]

4.  Na příkazovém řádku hello spustit **azperflib.exe** bez parametrů.

  > [!NOTE]
  > Azperflib.exe běží ve smyčce a aktualizace čítačů hello shromažďovaných každých 60 sekund. tooend hello smyčky, hello zavřít okno příkazového řádku.
  >
  >

Pokud hello rozšíření rozšířené monitorování Azure není nainstalovaný nebo hello AzureEnhancedMonitoring služba neběží, hello rozšíření nebyla nakonfigurována správně. Podrobné informace o tom, jak toodeploy hello rozšíření najdete v tématu [Poradce při potížích s hello monitorování infrastruktury Azure pro SAP][deployment-guide-5.3].

##### <a name="check-hello-output-of-azperflibexe"></a>Zkontrolujte výstup hello azperflib.exe
Azperflib.exe výstup ukazuje, že všechny čítače výkonu Azure vyplněný pro SAP. V dolní části hello hello seznamu shromážděných čítačů indikátoru stavu a souhrn zobrazit stav hello Azure monitorování.

![Výstup Kontrola stavu spuštěním azperflib.exe, který označuje, že neexistují žádné problémy][deployment-guide-figure-1100]
<a name="figure-11"></a>

Zkontrolujte hello výsledek vrácený pro hello **čítače celkem** výstupu, který se použije v hlášení jako prázdný a pro **stav**, uvedené v předcházející obrázek hello.

Interpretace výsledných hodnot hello následujícím způsobem:

| Azperflib.exe výsledek hodnoty | Azure stav monitorování |
| --- | --- |
| **Volání rozhraní API – není k dispozici** | Čítače, které nejsou k dispozici může být buď konfiguraci virtuálního počítače není k dispozici toohello nebo chyby. V tématu **stav**. |
| **Čítače Celkový – prázdné** |Následující dva čítače úložiště Azure Hello nesmí být prázdné: <ul><li>Čtení z úložiště Op latence serveru MS</li><li>Čtení z úložiště Op latence E2E MS</li></ul>Všechny ostatní čítače musí mít hodnoty. |
| **Stav** |Pouze OK Pokud vrátit stav ukazuje **OK**. |
| **Diagnostika** |Podrobné informace o stavu. |

Pokud hello **stav** hodnota není **OK**, postupujte podle pokynů hello v [Kontrola stavu pro konfiguraci Azure monitorování infrastruktury] [ deployment-guide-5.2].

#### <a name="run-hello-readiness-check-on-a-linux-vm"></a>Spustit kontrolu připravenosti hello na virtuální počítač s Linuxem

1.  Toohello virtuálním počítači Azure připojte pomocí protokolu SSH.

2.  Zkontrolujte výstup hello hello rozšíření rozšířené monitorování Azure.

  a.  Spusťte `more /var/lib/AzureEnhancedMonitor/PerfCounters`.

   **Očekávaný výsledek**: vrátí seznam čítačů výkonu. Hello soubor by neměl být prázdný.

 b. Spusťte `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error`.

   **Očekávaný výsledek**: vrátí jeden řádek, kde je chyba hello **žádné**, například **3; konfigurace; Chyba; 0; 0; žádná; 0; 1456416792; tst-servercs;**

  c. Spusťte `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord`.

    **Očekávaný výsledek**: vrátí jako prázdný nebo neexistuje.

Pokud hello předchozí kontroly nebyla úspěšná, spusťte tyto další kontroly:

1.  Ujistěte se, že příkaz waagent hello je nainstalován a povolen.

  a.  Spusťte `sudo ls -al /var/lib/waagent/`.

      **Očekávaný výsledek**: obsahuje seznam obsahu, hello hello příkaz waagent adresáře.

  b.  Spusťte `ps -ax | grep waagent`.

   **Očekávaný výsledek**: Zobrazí jednu položku podobnou:`python /usr/sbin/waagent -daemon`

3.   Ujistěte se, že tento hello rozšíření rozšířené monitorování Azure je nainstalovaná a spuštěná.

  a.  Spusťte `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/'`.

    **Očekávaný výsledek**: obsahuje seznam obsahu, hello hello rozšíření rozšířené monitorování Azure Directory.

  b. Spusťte `ps -ax | grep AzureEnhanced`.

     **Očekávaný výsledek**: Zobrazí jednu položku podobnou:`python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`

3. Nainstalujte agenta hostitele SAP, jak je popsáno v Poznámka SAP [1031096]a zkontrolujte výstup hello `saposcol`.

  a.  Spusťte `/usr/sap/hostctrl/exe/saposcol -d`.

  b.  Spusťte `dump ccm`.

  c.  Zkontrolujte, zda hello **Virtualization_Configuration\Enhanced monitorování přístupu** metrika **true**.

Pokud je již nainstalována aplikační server SAP NetWeaver ABAP, otevřete transakce ST06 sekce a zkontrolujte, zda je povoleno rozšířené monitorování.

Pokud některý z těchto kontrol selhání a podrobné informace o tom, jak tooredeploy hello rozšíření najdete v tématu [Poradce při potížích s hello monitorování infrastruktury Azure pro SAP][deployment-guide-5.3].

### <a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Kontrola stavu pro hello Azure monitorování konfigurace infrastruktury
Pokud některé hello monitorování dat není správně označené doručena hello testovací popsaných v [kontrolu připravenosti pro Azure rozšířené monitorování pro SAP][deployment-guide-5.1]spusťte hello `Test-AzureRmVMAEMExtension` toocheck rutiny jestli hello Azure monitorování infrastruktury a hello rozšíření monitorování pro SAP jsou správně nakonfigurované.

1.  Ujistěte se, že jste nainstalovali nejnovější verzi hello hello rutiny Azure Powershellu, jak je popsáno v [rutin nasazení prostředí Azure PowerShell][deployment-guide-4.1].
2.  Spusťte následující rutiny prostředí PowerShell hello. Seznam dostupných prostředí, spusťte rutinu hello `Get-AzureRmEnvironment`. globální Azure, vyberte toouse hello **AzureCloud** prostředí. Azure v Číně, vyberte **AzureChinaCloud**.
  ```powershell
  $env = Get-AzureRmEnvironment -Name <name of hello environment>
  Login-AzureRmAccount -Environment $env
  Set-AzureRmContext -SubscriptionName <subscription name>
  Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
  ```

3.  Zadejte vaše data na účtu a identifikovat hello virtuální počítač Azure.

  ![Vstupní stránky specifické pro SAP Azure rutiny Test-VMConfigForSAP_GUI][deployment-guide-figure-1200]

4. Hello skriptu testy hello konfiguraci hello virtuálního počítače jste vybrali.

  ![Výstup úspěšný test hello monitorování infrastruktury Azure pro SAP][deployment-guide-figure-1300]

Ujistěte se, že je každý výsledek kontroly stavu **OK**. Pokud nějaké kontroly nezobrazovat **OK**, spusťte rutinu hello aktualizace, jak je popsáno v [hello konfigurovat Azure rozšířené monitorování rozšíření pro SAP][deployment-guide-4.5]. Počkejte, 15 minut, a kontroluje opakování hello popsané v [kontrolu připravenosti pro Azure rozšířené monitorování pro SAP] [ deployment-guide-5.1] a [Kontrola stavu pro Azure monitorování infrastruktury konfiguraci] [deployment-guide-5.2]. Pokud hello kontroly stále znamenat problém související s některé nebo všechny čítače, přečtěte si téma [Poradce při potížích s hello monitorování infrastruktury Azure pro SAP][deployment-guide-5.3].

### <a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Řešení potíží s hello monitorování infrastruktury Azure pro SAP

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] Čítače výkonu Azure se nezobrazí vůbec
Hello služba AzureEnhancedMonitoring systému Windows shromažďuje metriky výkonu v Azure. Pokud hello služba nebyla správně nainstalována nebo není spuštěn v virtuálního počítače, se můžou shromažďovat metrik výkonu.

##### <a name="hello-installation-directory-of-hello-azure-enhanced-monitoring-extension-is-empty"></a>Instalační adresář Hello hello rozšíření rozšířené monitorování Azure je prázdná

###### <a name="issue"></a>Problém
Instalační adresář Hello C:\\balíčky\\modulů plug-in\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;verze >\\vynechání je prázdný.

###### <a name="solution"></a>Řešení
rozšíření Hello není nainstalována. Určí, zda se jedná o problém proxy (jak je popsáno výše). Může být nutné toorestart hello počítač a znovu spusťte hello `Set-AzureRmVMAEMExtension` konfigurační skript.

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>Služba pro rozšířené monitorování Azure neexistuje.

###### <a name="issue"></a>Problém
Hello služba systému AzureEnhancedMonitoring Windows neexistuje.

Výstup Azperflib.exe vrátí chybu:

![Provádění azperflib.exe označuje, zda není spuštěna služba hello hello rozšíření rozšířené monitorování Azure pro SAP][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Řešení
Pokud služba hello neexistuje, hello Azure rozšířené monitorování rozšíření pro SAP nebyl nainstalován správně. Znovu nasaďte hello rozšíření pomocí hello postupu pro váš scénář nasazení v [scénáře nasazení virtuálních počítačů pro SAP v Azure][deployment-guide-3].

Poté, co nasadíte rozšíření hello po jedné hodině, zkontrolujte znovu, zda jsou čítače výkonu Azure hello součástí hello virtuálního počítače Azure.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-toostart"></a>Služba pro rozšířené monitorování Azure existuje, ale selže toostart

###### <a name="issue"></a>Problém
Hello služba systému AzureEnhancedMonitoring Windows existuje a je povoleno, ale toostart se nezdaří. Další informace zkontrolujte protokol událostí aplikace hello.

###### <a name="solution"></a>Řešení
Hello konfigurace je nesprávná. Restartujte hello rozšíření pro hello virtuálních počítačů, monitorování, jak je popsáno v [hello konfigurovat Azure rozšířené monitorování rozšíření pro SAP][deployment-guide-4.5].

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] Chybí některé čítače výkonu Azure
Hello služba AzureEnhancedMonitoring systému Windows shromažďuje metriky výkonu v Azure. Služba Hello získá data z několika zdrojů. Některé konfigurační data se shromažďují místně a některé metriky výkonu se načítají z Azure Diagnostics. Z vaší protokolování na úrovni předplatného hello úložiště jsou použity čítače úložiště.

Pokud řešení potíží pomocí příkazu Poznámka SAP [1999351] hello potíže nevyřešíte, spusťte znovu hello `Set-AzureRmVMAEMExtension` konfigurační skript. Protože storage analytics nebo diagnostiky čítače nemusí vytvořit ihned po jejich jsou povolené, může mít toowait na hodinu. Pokud hello problém přetrvává, otevřete zprávu SAP zákaznické podpory na hello BC OP NT AZR součásti pro systém Windows nebo BC-OP-LNX-AZR pro virtuální počítač s Linuxem.

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Čítače výkonu Azure se nezobrazí vůbec
Démon shromážděné metriky výkonu v Azure. Pokud hello démon není spuštěn, je možné sbírat metrik výkonu.

##### <a name="hello-installation-directory-of-hello-azure-enhanced-monitoring-extension-is-empty"></a>Instalační adresář Hello hello rozšíření Azure rozšířeného Monitoring je prázdná

###### <a name="issue"></a>Problém
Hello directory \\var\\lib\\příkaz waagent\\ nemá podadresáři pro hello rozšířené monitorování Azure rozšíření.

###### <a name="solution"></a>Řešení
rozšíření Hello není nainstalována. Určí, zda se jedná o problém proxy (jak je popsáno výše). Může být nutné toorestart hello počítače nebo znovu spusťte hello `Set-AzureRmVMAEMExtension` konfigurační skript.

#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Chybí některé čítače výkonu Azure
Démon procesu, který získá data z několika zdrojů shromážděné metriky výkonu v Azure. Některé konfigurační data se shromažďují místně a některé metriky výkonu se načítají z Azure Diagnostics. Úložiště čítače pocházet z protokolů hello ve vašem předplatném úložiště.

Pro dokončení a aktuální seznam známých problémů, viz poznámka SAP [1999351], která obsahuje další informace o řešení potíží pro rozšířené monitorování Azure pro SAP.

Pokud řešení potíží pomocí příkazu Poznámka SAP [1999351] neobsahuje hello problém vyřešit, spusťte znovu hello `Set-AzureRmVMAEMExtension` konfigurační skript, jak je popsáno v [hello konfigurovat Azure rozšířené monitorování rozšíření pro SAP][deployment-guide-4.5]. Toowait může mít jednu hodinu, protože úložiště analytics nebo diagnostiky čítače nemusí vytvořit okamžitě po jsou povoleny. Pokud hello problém přetrvává, otevřete zprávu SAP zákaznické podpory na hello BC OP NT AZR součásti pro systém Windows nebo BC-OP-LNX-AZR pro virtuální počítač s Linuxem.
