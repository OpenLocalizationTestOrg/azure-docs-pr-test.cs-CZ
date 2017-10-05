---
title: "Nasazení virtuálních počítačů, které jsou pro SAP v systému Windows Azure | Microsoft Docs"
description: "Informace o nasazení SAP software na virtuálních počítačích s Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 22aaa98c-bb9f-472f-b546-0b294e033cda
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ac7c1e3b015a21fe06f27205b27a53fc4d2f3df7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-sap-on-windows-vms-in-azure"></a><span data-ttu-id="8126e-103">Nasazení SAP na virtuálních počítačích Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="8126e-103">Deploy SAP on Windows VMs in Azure</span></span>
[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
<span data-ttu-id="8126e-104">[1031096]:https://launchpad.support.sap.com/#/notes/1031096</span><span class="sxs-lookup"><span data-stu-id="8126e-104">[1031096]:https://launchpad.support.sap.com/#/notes/1031096</span></span>
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
<span data-ttu-id="8126e-105">[1409604]:https://launchpad.support.sap.com/#/notes/1409604</span><span class="sxs-lookup"><span data-stu-id="8126e-105">[1409604]:https://launchpad.support.sap.com/#/notes/1409604</span></span>
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
<span data-ttu-id="8126e-106">[1619720]:https://launchpad.support.sap.com/#/notes/1619720</span><span class="sxs-lookup"><span data-stu-id="8126e-106">[1619720]:https://launchpad.support.sap.com/#/notes/1619720</span></span>
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
<span data-ttu-id="8126e-107">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span><span class="sxs-lookup"><span data-stu-id="8126e-107">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span></span>
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
<span data-ttu-id="8126e-108">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span><span class="sxs-lookup"><span data-stu-id="8126e-108">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span></span>
<span data-ttu-id="8126e-109">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span><span class="sxs-lookup"><span data-stu-id="8126e-109">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span></span>
<span data-ttu-id="8126e-110">[2002167]:https://launchpad.support.sap.com/#/notes/2002167</span><span class="sxs-lookup"><span data-stu-id="8126e-110">[2002167]:https://launchpad.support.sap.com/#/notes/2002167</span></span>
<span data-ttu-id="8126e-111">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span><span class="sxs-lookup"><span data-stu-id="8126e-111">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span></span>
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
<span data-ttu-id="8126e-112">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span><span class="sxs-lookup"><span data-stu-id="8126e-112">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span></span>
<span data-ttu-id="8126e-113">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span><span class="sxs-lookup"><span data-stu-id="8126e-113">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span></span>
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
<span data-ttu-id="8126e-114">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span><span class="sxs-lookup"><span data-stu-id="8126e-114">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span></span>
[2367194]:https://launchpad.support.sap.com/#/notes/2367194

[azure-cli]:../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:sap-dbms-guide.md (Azure Virtual Machines DBMS deployment for SAP on Windows)
[dbms-guide-2.1]:sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Caching for VMs and VHDs)
[dbms-guide-2.2]:sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software RAID)
[dbms-guide-2.3]:sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Storage)
[dbms-guide-2]:sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Structure of a RDBMS deployment)
[dbms-guide-3]:sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (High availability and disaster recovery with Azure VMs)
[dbms-guide-5.5.1]:sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 and later)
[dbms-guide-5.5.2]:sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 and earlier releases)
[dbms-guide-5.6]:sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (Using a SQL Server image from the Azure Marketplace)
[dbms-guide-5.8]:sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (General SQL Server for SAP on Azure summary)
[dbms-guide-5]:sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (Specifics to SQL Server RDBMS)
[dbms-guide-8.4.1]:sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Storage configuration)
[dbms-guide-8.4.2]:sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Backup and restore)
[dbms-guide-8.4.3]:sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Performance considerations for backup and restore)
[dbms-guide-8.4.4]:sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (Other)
[dbms-guide-900-sap-cache-server-on-premises]:sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:sap-deployment-guide.md (Azure Virtual Machines deployment for SAP on Windows)
[deployment-guide-2.2]:#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP resources)
[deployment-guide-3.1.2]:#3688666f-281f-425b-a312-a77e7db2dfab (Deploying a VM by using a custom image)
[deployment-guide-3.2]:#db477013-9060-4602-9ad4-b0316f8bb281 (Scenario 1: Deploying a VM from the Azure Marketplace for SAP)
[deployment-guide-3.3]:#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Scenario 2: Deploying a VM with a custom image for SAP)
[deployment-guide-3.4]:#a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Scenario 3: Moving a VM from on-premises using a non-generalized Azure VHD with SAP)
[deployment-guide-3]:#b3253ee3-d63b-4d74-a49b-185e76c4088e (Deployment scenarios of VMs for SAP on Microsoft Azure)
[deployment-guide-4.1]:#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Deploying Azure PowerShell cmdlets)
[deployment-guide-4.2]:#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Download and import SAP-relevant PowerShell cmdlets)
[deployment-guide-4.3]:#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Join a VM to an on-premises domain - Windows only)
[deployment-guide-4.4.2]:#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux)
[deployment-guide-4.4]:#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (Download, install, and enable the Azure VM Agent)
[deployment-guide-4.5.1]:#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell)
[deployment-guide-4.5.2]:#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI)
[deployment-guide-4.5]:#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Configure the Azure Enhanced Monitoring Extension for SAP)
[deployment-guide-5.1]:#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Readiness check for Azure Enhanced Monitoring for SAP)
[deployment-guide-5.2]:#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Health check for the Azure monitoring infrastructure)
[deployment-guide-5.3]:#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Troubleshooting Azure monitoring for SAP)

[deployment-guide-configure-monitoring-scenario-1]:#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure monitoring)
[deployment-guide-configure-proxy]:#baccae00-6f79-4307-ade4-40292ce4e02d (Configure the proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:#564adb4f-5c95-4041-9616-6635e83a810b (Checks and troubleshooting for setting up end-to-end monitoring)

[deploy-template-cli]:../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:sap-get-started.md
[getting-started-dbms]:sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:classic/virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:classic/virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:classic/virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:classic/virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:classic/virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:classic/virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-windows-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:sap-planning-guide.md (Azure Virtual Machines planning and implementation for SAP on Windows)
[planning-guide-1.2]:sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Resources)
[planning-guide-11]:sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (High availability and disaster recovery for SAP NetWeaver running on Azure Virtual Machines)
[planning-guide-11.4.1]:sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (High availability for SAP Application Servers)
[planning-guide-11.5]:sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (Using Autostart for SAP instances)
[planning-guide-2.1]:sap-planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 (Cloud-only - Virtual Machine deployments in Azure without dependencies on the on-premises customer network)
[planning-guide-2.2]:sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Cross-premises - Deployment of single or multiple SAP VMs in Azure fully integrated with the on-premises network)
[planning-guide-3.1]:sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure regions)
[planning-guide-3.2.1]:sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Fault domains)
[planning-guide-3.2.2]:sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Upgrade domains)
[planning-guide-3.2.3]:sap-planning-guide.md#18810088-f9be-4c97-958a-27996255c665 (Azure availability sets)
[planning-guide-3.2]:sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Microsoft Azure virtual machines concept)
[planning-guide-3.3.2]:sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)
[planning-guide-5.1.1]:sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Moving a VM from on-premises to Azure with a non-generalized disk)
[planning-guide-5.1.2]:sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Deploying a VM with a customer specific image)
[planning-guide-5.2.1]:sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Preparation for moving a VM from on-premises to Azure with a non-generalized disk)
[planning-guide-5.2.2]:sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Preparation for deploying a VM with a customer specific image for SAP)
[planning-guide-5.2]:sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Preparing VMs with SAP for Azure)
[planning-guide-5.3.1]:sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Difference between an Azure disk and an Azure image)
[planning-guide-5.3.2]:sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (Uploading a VHD from on-premises to Azure)
[planning-guide-5.4.2]:sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Copying disks between Azure Storage accounts)
[planning-guide-5.5.1]:sap-planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (VM/VHD structure for SAP deployments)
[planning-guide-5.5.3]:sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Setting automount for attached disks)
[planning-guide-7.1]:sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (Single VM with SAP NetWeaver demo/training scenario)
[planning-guide-7]:sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Concepts of Cloud-Only deployment of SAP instances)
[planning-guide-9.1]:sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure Monitoring Solution for SAP)
[planning-guide-azure-premium-storage]:sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and data disks)

[powershell-install-configure]:/powershell/azureps-cmdlets-docs
[resource-group-authoring-templates]:../../resource-group-authoring-templates.md
[resource-group-overview]:../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../storage/storage-premium-storage.md
[storage-redundancy]:../../storage/storage-redundancy.md
[storage-scalability-targets]:../../storage/storage-scalability-targets.md
[storage-use-azcopy]:../../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../linux/attach-disk-portal.md
[virtual-machines-windows-attach-disk-portal]:../virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../linux/cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:../virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../linux/capture-image.md
[virtual-machines-linux-capture-image-capture]:../linux/capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:../virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:../virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:../linux/configure-lvm.md
[virtual-machines-linux-configure-raid]:../linux/configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../linux/update-agent.md
[virtual-machines-manage-availability]:../virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:classic/ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:classic/ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:upload-image.md
[virtual-machines-windows-tutorial]:../virtual-machines-windows-hero-tutorial.md
[virtual-machines-windows-create-vm-specialized]:../virtual-machines-windows-create-vm-specialized.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../xplat-cli-azure-resource-manager.md

<span data-ttu-id="8126e-115">Virtuální počítače Azure je řešení pro organizace, které potřebují výpočetní a úložnou kapacitu, minimální včas a bez zdlouhavé nákup cykly.</span><span class="sxs-lookup"><span data-stu-id="8126e-115">Azure Virtual Machines is the solution for organizations that need compute and storage resources, in minimal time, and without lengthy procurement cycles.</span></span> <span data-ttu-id="8126e-116">Virtuální počítače Azure můžete použít k nasazení klasického aplikací, jako jsou aplikace založené na SAP NetWeaver v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-116">You can use Azure Virtual Machines to deploy classical applications, like SAP NetWeaver-based applications, in Azure.</span></span> <span data-ttu-id="8126e-117">Rozšiřte spolehlivosti a dostupnosti bez dalších místních prostředků aplikace.</span><span class="sxs-lookup"><span data-stu-id="8126e-117">Extend an application's reliability and availability without additional on-premises resources.</span></span> <span data-ttu-id="8126e-118">Virtuální počítače Azure podporuje připojení mezi různými místy, takže virtuální počítače Azure můžete integrovat do místní domény vaší organizace, privátních cloudů a šířku systému SAP.</span><span class="sxs-lookup"><span data-stu-id="8126e-118">Azure Virtual Machines supports cross-premises connectivity, so you can integrate Azure Virtual Machines into your organization's on-premises domains, private clouds, and SAP system landscape.</span></span>

<span data-ttu-id="8126e-119">V tomto článku jsme zahrnují kroky k nasazení SAP aplikací na Windows virtuálních počítačů (VM) v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-119">In this article, we cover the steps to deploy SAP applications on Windows virtual machines (VMs) in Azure.</span></span> <span data-ttu-id="8126e-120">Tento článek vychází informace v [virtuální počítače Azure plánování a implementace pro SAP v systému Windows][planning-guide].</span><span class="sxs-lookup"><span data-stu-id="8126e-120">This article builds on the information in [Azure Virtual Machines planning and implementation for SAP on Windows][planning-guide].</span></span> <span data-ttu-id="8126e-121">Také doplňuje SAP instalace dokumentace a poznámky k SAP, které jsou primární prostředky pro instalaci a nasazení SAP softwaru.</span><span class="sxs-lookup"><span data-stu-id="8126e-121">It also complements SAP installation documentation and SAP Notes, which are the primary resources for installing and deploying SAP software.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8126e-122">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8126e-122">Prerequisites</span></span>
<span data-ttu-id="8126e-123">Nastavení virtuálního počítače Azure pro nasazení softwaru SAP zahrnuje několik kroků a prostředky.</span><span class="sxs-lookup"><span data-stu-id="8126e-123">Setting up an Azure virtual machine for SAP software deployment involves multiple steps and resources.</span></span> <span data-ttu-id="8126e-124">Než začnete, ujistěte se, že splňujete požadavky pro instalaci softwaru SAP na virtuálních počítačích s Windows v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-124">Before you start, make sure that you meet the prerequisites for installing SAP software on Windows virtual machines in Azure.</span></span>

### <a name="local-computer"></a><span data-ttu-id="8126e-125">Místní počítač</span><span class="sxs-lookup"><span data-stu-id="8126e-125">Local computer</span></span>
<span data-ttu-id="8126e-126">Ke správě systému Windows nebo Linux virtuálních počítačů, můžete použít skript prostředí PowerShell a portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-126">To manage Windows or Linux VMs, you can use a PowerShell script and the Azure portal.</span></span> <span data-ttu-id="8126e-127">Pro oba nástroje budete potřebovat místní počítač se systémem Windows 7 nebo novější verzi systému Windows.</span><span class="sxs-lookup"><span data-stu-id="8126e-127">For both tools, you need a local computer running Windows 7 or a later version of Windows.</span></span> <span data-ttu-id="8126e-128">Pokud chcete spravovat pouze virtuální počítače s Linuxem a chcete pro tuto úlohu použít počítač se systémem Linux, můžete použít rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-128">If you want to manage only Linux VMs and you want to use a Linux computer for this task, you can use Azure CLI.</span></span>

### <a name="internet-connection"></a><span data-ttu-id="8126e-129">Připojení k Internetu</span><span class="sxs-lookup"><span data-stu-id="8126e-129">Internet connection</span></span>
<span data-ttu-id="8126e-130">Ke stažení a spuštění nástroje a skripty, které jsou požadovány pro nasazení softwaru SAP, musí být připojen k Internetu.</span><span class="sxs-lookup"><span data-stu-id="8126e-130">To download and run the tools and scripts that are required for SAP software deployment, you must be connected to the Internet.</span></span> <span data-ttu-id="8126e-131">Virtuální počítač Azure, který běží Azure rozšířené monitorování rozšíření pro SAP také potřebuje přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="8126e-131">The Azure VM that is running the Azure Enhanced Monitoring Extension for SAP also needs access to the Internet.</span></span> <span data-ttu-id="8126e-132">Pokud virtuální počítač Azure je součástí virtuální síť Azure nebo místní domény, ujistěte se, že jsou nastavené příslušné nastavení serveru proxy, jak je popsáno v [proxy server nakonfigurovat][deployment-guide-configure-proxy].</span><span class="sxs-lookup"><span data-stu-id="8126e-132">If the Azure VM is part of an Azure virtual network or on-premises domain, make sure that the relevant proxy settings are set, as described in [Configure the proxy][deployment-guide-configure-proxy].</span></span>

### <a name="microsoft-azure-subscription"></a><span data-ttu-id="8126e-133">Předplatné Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="8126e-133">Microsoft Azure subscription</span></span>
<span data-ttu-id="8126e-134">Potřebujete aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-134">You need an active Azure account.</span></span>

### <a name="topology-and-networking"></a><span data-ttu-id="8126e-135">Topologie a sítě</span><span class="sxs-lookup"><span data-stu-id="8126e-135">Topology and networking</span></span>
<span data-ttu-id="8126e-136">Je třeba definovat topologii a architektura nasazení SAP v Azure:</span><span class="sxs-lookup"><span data-stu-id="8126e-136">You need to define the topology and architecture of the SAP deployment in Azure:</span></span>

* <span data-ttu-id="8126e-137">Účty úložiště Azure, který se má použít</span><span class="sxs-lookup"><span data-stu-id="8126e-137">Azure storage accounts to be used</span></span>
* <span data-ttu-id="8126e-138">Virtuální síť, ve které chcete nasazení systému SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-138">Virtual network where you want to deploy the SAP system</span></span>
* <span data-ttu-id="8126e-139">Skupina prostředků, do které chcete nasadit systému SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-139">Resource group to which you want to deploy the SAP system</span></span>
* <span data-ttu-id="8126e-140">Oblast Azure, ve které chcete nasazení systému SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-140">Azure region where you want to deploy the SAP system</span></span>
* <span data-ttu-id="8126e-141">Konfigurace SAP (dvouvrstvá nebo třívrstvá)</span><span class="sxs-lookup"><span data-stu-id="8126e-141">SAP configuration (two-tier or three-tier)</span></span>
* <span data-ttu-id="8126e-142">Velikosti virtuálních počítačů a počet další virtuální pevné disky (VHD) se chcete připojit k virtuálním počítačům</span><span class="sxs-lookup"><span data-stu-id="8126e-142">VM sizes and the number of additional virtual hard disks (VHDs) to be mounted to the VMs</span></span>
* <span data-ttu-id="8126e-143">Konfigurace SAP oprava a přenosu systému (CTS)</span><span class="sxs-lookup"><span data-stu-id="8126e-143">SAP Correction and Transport System (CTS) configuration</span></span>

<span data-ttu-id="8126e-144">Vytvořit a nakonfigurovat účty úložiště Azure nebo virtuální sítě Azure, před zahájením procesu nasazení softwaru SAP.</span><span class="sxs-lookup"><span data-stu-id="8126e-144">Create and configure Azure storage accounts or Azure virtual networks before you begin the SAP software deployment process.</span></span> <span data-ttu-id="8126e-145">Informace o tom, jak vytvořit a nakonfigurovat těchto prostředků najdete v tématu [virtuální počítače Azure plánování a implementace pro SAP v systému Windows][planning-guide].</span><span class="sxs-lookup"><span data-stu-id="8126e-145">For information about how to create and configure these resources, see [Azure Virtual Machines planning and implementation for SAP on Windows][planning-guide].</span></span>

### <a name="sap-sizing"></a><span data-ttu-id="8126e-146">Nastavení velikosti SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-146">SAP sizing</span></span>
<span data-ttu-id="8126e-147">Následující informace pro nastavení velikosti SAP vědět:</span><span class="sxs-lookup"><span data-stu-id="8126e-147">Know the following information, for SAP sizing:</span></span>

* <span data-ttu-id="8126e-148">Předpokládané úlohy SAP, například pomocí nástroje pro rychlé přizpůsobení velikosti symbolů SAP a SAP aplikace výkonu standardní (přístupové body) číslo</span><span class="sxs-lookup"><span data-stu-id="8126e-148">Projected SAP workload, for example, by using the SAP Quick Sizer tool, and the SAP Application Performance Standard (SAPS) number</span></span>
* <span data-ttu-id="8126e-149">Požadovaný prostředek a paměti spotřeby procesoru systému SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-149">Required CPU resource and memory consumption of the SAP system</span></span>
* <span data-ttu-id="8126e-150">Požadované vstupní a výstupní (I/O) operace za sekundu</span><span class="sxs-lookup"><span data-stu-id="8126e-150">Required input/output (I/O) operations per second</span></span>
* <span data-ttu-id="8126e-151">Požadované šířky pásma sítě případné komunikace mezi virtuálními počítači v Azure</span><span class="sxs-lookup"><span data-stu-id="8126e-151">Required network bandwidth of eventual communication between VMs in Azure</span></span>
* <span data-ttu-id="8126e-152">Požadované šířku pásma sítě mezi místními prostředky a v systému SAP nasazení Azure</span><span class="sxs-lookup"><span data-stu-id="8126e-152">Required network bandwidth between on-premises assets and the Azure-deployed SAP system</span></span>

### <a name="resource-groups"></a><span data-ttu-id="8126e-153">Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="8126e-153">Resource groups</span></span>
<span data-ttu-id="8126e-154">V Azure Resource Manager, můžete použít skupin prostředků ke správě všechny prostředky aplikace ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-154">In Azure Resource Manager, you can use resource groups to manage all the application resources in your Azure subscription.</span></span> <span data-ttu-id="8126e-155">Další informace najdete v tématu [přehled Azure Resource Manageru][resource-group-overview].</span><span class="sxs-lookup"><span data-stu-id="8126e-155">For more information, see [Azure Resource Manager overview][resource-group-overview].</span></span>

## <a name="resources"></a><span data-ttu-id="8126e-156">Zdroje</span><span class="sxs-lookup"><span data-stu-id="8126e-156">Resources</span></span>

### <span data-ttu-id="8126e-157"><a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>Prostředky SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-157"><a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP resources</span></span>
<span data-ttu-id="8126e-158">Pokud nastavujete nasazení SAP softwaru, je třeba na následujících odkazech SAP:</span><span class="sxs-lookup"><span data-stu-id="8126e-158">When you are setting up your SAP software deployment, you need the following SAP resources:</span></span>

* <span data-ttu-id="8126e-159">Poznámka SAP [1928533], na kterém je:</span><span class="sxs-lookup"><span data-stu-id="8126e-159">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="8126e-160">Seznam velikostí virtuálních počítačů Azure, které jsou podporovány pro nasazení softwaru SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-160">List of Azure VM sizes that are supported for the deployment of SAP software</span></span>
  * <span data-ttu-id="8126e-161">Kapacita důležité informace o velikosti virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="8126e-161">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="8126e-162">Podporované SAP software a operační systém (OS) a kombinace databáze</span><span class="sxs-lookup"><span data-stu-id="8126e-162">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="8126e-163">Požadovaná verze SAP jádra pro Windows a Linux v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="8126e-163">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>

* <span data-ttu-id="8126e-164">Poznámka SAP [2015553] uvádí požadavky pro nasazení softwaru podporovaných SAP SAP v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-164">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="8126e-165">Poznámka SAP [2178632] obsahuje podrobné informace o veškeré monitorování metriky pro SAP v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-165">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="8126e-166">Poznámka SAP [1409604] má požadovaná verze SAP hostitele agenta pro Windows v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-166">SAP Note [1409604] has the required SAP Host Agent version for Windows in Azure.</span></span>
* <span data-ttu-id="8126e-167">Poznámka SAP [2191498] má požadovaná verze SAP hostitele agenta pro Linux v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-167">SAP Note [2191498] has the required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="8126e-168">Poznámka SAP [2243692] obsahuje informace o licencích SAP v systému Linux v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-168">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="8126e-169">Poznámka SAP [1984787] má obecné informace o SUSE Linux Enterprise Server 12.</span><span class="sxs-lookup"><span data-stu-id="8126e-169">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="8126e-170">Poznámka SAP [2002167] má obecné informace o Red Hat Enterprise Linux 7.x.</span><span class="sxs-lookup"><span data-stu-id="8126e-170">SAP Note [2002167] has general information about Red Hat Enterprise Linux 7.x.</span></span>
* <span data-ttu-id="8126e-171">Poznámka SAP [1999351] Další informace o řešení problémů s Azure rozšířené monitorování rozšíření pro SAP.</span><span class="sxs-lookup"><span data-stu-id="8126e-171">SAP Note [1999351] has additional troubleshooting information for the Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="8126e-172">[SAP komunity WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) má všechny požadované SAP poznámky pro Linux.</span><span class="sxs-lookup"><span data-stu-id="8126e-172">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="8126e-173">Rutiny prostředí PowerShell specifické pro SAP, které jsou součástí [prostředí Azure PowerShell][azure-ps].</span><span class="sxs-lookup"><span data-stu-id="8126e-173">SAP-specific PowerShell cmdlets that are part of [Azure PowerShell][azure-ps].</span></span>
* <span data-ttu-id="8126e-174">Příkazy rozhraní příkazového řádku Azure specifické pro SAP, které jsou součástí [rozhraní příkazového řádku Azure][azure-cli].</span><span class="sxs-lookup"><span data-stu-id="8126e-174">SAP-specific Azure CLI commands that are part of [Azure CLI][azure-cli].</span></span>

<span data-ttu-id="8126e-175">[comment]: <> (Úroveň oprav MSSedusch TODO přidat ARM pro SAP Agent hostitele v 1409604 Poznámka SAP)</span><span class="sxs-lookup"><span data-stu-id="8126e-175">[comment]: <> (MSSedusch TODO Add ARM patch level for SAP Host Agent in SAP Note 1409604)</span></span>

### <a name="microsoft-resources"></a><span data-ttu-id="8126e-176">Zdroje společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="8126e-176">Microsoft resources</span></span>
<span data-ttu-id="8126e-177">Tyto články Microsoft popisuje nasazení SAP v Azure:</span><span class="sxs-lookup"><span data-stu-id="8126e-177">These Microsoft articles cover SAP deployments in Azure:</span></span>

* <span data-ttu-id="8126e-178">[Azure virtuálních počítačů, plánování a implementace pro SAP v systému Windows][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="8126e-178">[Azure Virtual Machines planning and implementation for SAP on Windows][planning-guide]</span></span>
* <span data-ttu-id="8126e-179">[Nasazení virtuálních počítačů Azure pro SAP v systému Windows (v tomto článku)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="8126e-179">[Azure Virtual Machines deployment for SAP on Windows (this article)][deployment-guide]</span></span>
* <span data-ttu-id="8126e-180">[Nasazení virtuálních počítačů databázového systému Azure pro SAP v systému Windows][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="8126e-180">[Azure Virtual Machines DBMS deployment for SAP on Windows][dbms-guide]</span></span>
* <span data-ttu-id="8126e-181">[Portál Azure][azure-portal]</span><span class="sxs-lookup"><span data-stu-id="8126e-181">[Azure portal][azure-portal]</span></span>

## <span data-ttu-id="8126e-182"><a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Scénáře nasazení SAP softwaru na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="8126e-182"><a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Deployment scenarios for SAP software on Azure VMs</span></span>
<span data-ttu-id="8126e-183">Máte několik možností pro nasazení virtuálních počítačů a přidruženými disky v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-183">You have multiple options for deploying VMs and associated disks in Azure.</span></span> <span data-ttu-id="8126e-184">Je důležité pochopit rozdíly mezi možnostmi nasazení, protože může trvat různé kroky, které připravit virtuální počítače pro nasazení na základě typu nasazení, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="8126e-184">It's important to understand the differences between deployment options, because you might take different steps to prepare your VMs for deployment based on the deployment type you choose.</span></span>

### <span data-ttu-id="8126e-185"><a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Scénář 1: Nasazení virtuálního počítače z Azure Marketplace pro SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-185"><a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Scenario 1: Deploying a VM from the Azure Marketplace for SAP</span></span>
<span data-ttu-id="8126e-186">Image zadaná společnost Microsoft nebo třetích stran v Azure Marketplace můžete použít k nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8126e-186">You can use an image provided by Microsoft or by a third party in the Azure Marketplace to deploy your VM.</span></span> <span data-ttu-id="8126e-187">Na webu Marketplace nabízí některé standardní bitové kopie operačního systému Windows Server a různých distribucí Linux.</span><span class="sxs-lookup"><span data-stu-id="8126e-187">The Marketplace offers some standard OS images of Windows Server and different Linux distributions.</span></span> <span data-ttu-id="8126e-188">Obrázek, který zahrnuje správu databáze také můžete nasadit systém SKU, například Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8126e-188">You also can deploy an image that includes database management system (DBMS) SKUs, for example, Microsoft SQL Server.</span></span> <span data-ttu-id="8126e-189">Další informace o používání bitové kopie s SKU databázového systému najdete v tématu [databázového systému virtuální počítače Azure nasazení pro SAP v systému Linux][dbms-guide].</span><span class="sxs-lookup"><span data-stu-id="8126e-189">For more information about using images with DBMS SKUs, see [Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide].</span></span>

<span data-ttu-id="8126e-190">Následující diagram ukazuje SAP konkrétní pořadí kroků pro nasazení virtuálního počítače z Azure Marketplace:</span><span class="sxs-lookup"><span data-stu-id="8126e-190">The following flowchart shows the SAP-specific sequence of steps for deploying a VM from the Azure Marketplace:</span></span>

![Vývojový diagram nasazení virtuálního počítače pro systémy SAP pomocí bitové kopie virtuálních počítačů z Azure Marketplace][deployment-guide-figure-100]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a><span data-ttu-id="8126e-192">Vytvoření virtuálního počítače pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8126e-192">Create a virtual machine by using the Azure portal</span></span>
<span data-ttu-id="8126e-193">Nejjednodušší způsob, jak vytvořit nový virtuální počítač s bitovou kopii z Azure Marketplace je pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-193">The easiest way to create a new virtual machine with an image from the Azure Marketplace is by using the Azure portal.</span></span>

1.  <span data-ttu-id="8126e-194">Přejděte na <https://portal.azure.com/#create>.</span><span class="sxs-lookup"><span data-stu-id="8126e-194">Go to <https://portal.azure.com/#create>.</span></span>  <span data-ttu-id="8126e-195">Nebo v nabídce portálu Azure vyberte **+ nový**.</span><span class="sxs-lookup"><span data-stu-id="8126e-195">Or, in the Azure portal menu, select **+ New**.</span></span>
2.  <span data-ttu-id="8126e-196">Vyberte **výpočetní**a pak vyberte typ operačního systému, kterou chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="8126e-196">Select **Compute**, and then select the type of operating system you want to deploy.</span></span> <span data-ttu-id="8126e-197">Například Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12) nebo Red Hat Enterprise Linux 7.2 (RHEL 7.2).</span><span class="sxs-lookup"><span data-stu-id="8126e-197">For example, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), or Red Hat Enterprise Linux 7.2 (RHEL 7.2).</span></span> <span data-ttu-id="8126e-198">Výchozí zobrazení seznamu nezobrazuje, že všechny podporované operační systémy.</span><span class="sxs-lookup"><span data-stu-id="8126e-198">The default list view does not show all supported operating systems.</span></span> <span data-ttu-id="8126e-199">Vyberte **zobrazit všechny** úplný seznam.</span><span class="sxs-lookup"><span data-stu-id="8126e-199">Select **see all** for a full list.</span></span> <span data-ttu-id="8126e-200">Další informace o podporovaných operačních systémech pro nasazení softwaru SAP, viz poznámka SAP [1928533].</span><span class="sxs-lookup"><span data-stu-id="8126e-200">For more information about supported operating systems for SAP software deployment, see SAP Note [1928533].</span></span>
3.  <span data-ttu-id="8126e-201">Na další stránce přečtěte si podmínky a ujednání.</span><span class="sxs-lookup"><span data-stu-id="8126e-201">On the next page, review terms and conditions.</span></span>
4.  <span data-ttu-id="8126e-202">V **vybrat model nasazení** vyberte **Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="8126e-202">In the **Select a deployment model** box, select **Resource Manager**.</span></span>
5.  <span data-ttu-id="8126e-203">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8126e-203">Select **Create**.</span></span>

<span data-ttu-id="8126e-204">Průvodce vás provede nastavení požadované parametry, které chcete vytvořit virtuální počítač, kromě všechny požadované prostředky, jako je síťová rozhraní a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="8126e-204">The wizard guides you through setting the required parameters to create the virtual machine, in addition to all required resources, like network interfaces and storage accounts.</span></span> <span data-ttu-id="8126e-205">Některé z těchto parametrů jsou:</span><span class="sxs-lookup"><span data-stu-id="8126e-205">Some of these parameters are:</span></span>

1. <span data-ttu-id="8126e-206">**Základy**:</span><span class="sxs-lookup"><span data-stu-id="8126e-206">**Basics**:</span></span>
  * <span data-ttu-id="8126e-207">**Název**: název prostředku (název virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="8126e-207">**Name**: The name of the resource (the virtual machine name).</span></span>
 * <span data-ttu-id="8126e-208">**Uživatelské jméno a heslo** nebo **veřejný klíč SSH**: Zadejte uživatelské jméno a heslo uživatele, který se vytvoří během zřizování.</span><span class="sxs-lookup"><span data-stu-id="8126e-208">**Username and password** or **SSH public key**: Enter the username and password of the user that is created during the provisioning.</span></span> <span data-ttu-id="8126e-209">Pro virtuální počítač s Linuxem můžete zadat veřejný klíč Secure Shell (SSH), který používáte k přihlášení k počítači.</span><span class="sxs-lookup"><span data-stu-id="8126e-209">For a Linux virtual machine, you can enter the public Secure Shell (SSH) key that you use to sign in to the machine.</span></span>
 * <span data-ttu-id="8126e-210">**Předplatné**: Vyberte odběr, který chcete použít ke zřízení nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8126e-210">**Subscription**: Select the subscription that you want to use to provision the new virtual machine.</span></span>
 * <span data-ttu-id="8126e-211">**Skupina prostředků**: název skupiny prostředků pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8126e-211">**Resource group**: The name of the resource group for the VM.</span></span> <span data-ttu-id="8126e-212">Můžete zadat název nové skupiny prostředků nebo název skupiny prostředků, která již existuje.</span><span class="sxs-lookup"><span data-stu-id="8126e-212">You can enter either the name of a new resource group or the name of a resource group that already exists.</span></span>
 * <span data-ttu-id="8126e-213">**Umístění**: kde nasadit nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8126e-213">**Location**: Where to deploy the new virtual machine.</span></span> <span data-ttu-id="8126e-214">Pokud se chcete připojit virtuální počítač k síti na pracovišti, nezapomeňte že vybrat umístění virtuální sítě, která se připojuje k vaší místní síti Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-214">If you want to connect the virtual machine to your on-premises network, make sure you select the location of the virtual network that connects Azure to your on-premises network.</span></span> <span data-ttu-id="8126e-215">Další informace najdete v tématu [sítě Microsoft Azure] [ planning-guide-microsoft-azure-networking] v [virtuální počítače Azure plánování a implementace pro SAP v systému Linux][planning-guide].</span><span class="sxs-lookup"><span data-stu-id="8126e-215">For more information, see [Microsoft Azure networking][planning-guide-microsoft-azure-networking] in [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide].</span></span>
2. <span data-ttu-id="8126e-216">**Velikost**:</span><span class="sxs-lookup"><span data-stu-id="8126e-216">**Size**:</span></span>

     <span data-ttu-id="8126e-217">Seznam podporovaných typů virtuálních počítačů, viz poznámka SAP [1928533].</span><span class="sxs-lookup"><span data-stu-id="8126e-217">For a list of supported VM types, see SAP Note [1928533].</span></span> <span data-ttu-id="8126e-218">Ujistěte se, zda že jste vybrali správný typ virtuálního počítače, pokud chcete používat Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="8126e-218">Be sure you select the correct VM type if you want to use Azure Premium Storage.</span></span> <span data-ttu-id="8126e-219">Ne všechny typy virtuálních počítačů podporují službu Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="8126e-219">Not all VM types support Premium Storage.</span></span> <span data-ttu-id="8126e-220">Další informace najdete v tématu [úložiště: Microsoft Azure Storage a datové disky] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] a [Azure Premium Storage] [ planning-guide-azure-premium-storage] v [virtuální počítače Azure plánování a implementace pro SAP v systému Linux][planning-guide].</span><span class="sxs-lookup"><span data-stu-id="8126e-220">For more information, see [Storage: Microsoft Azure Storage and data disks][planning-guide-storage-microsoft-azure-storage-and-data-disks] and [Azure Premium Storage][planning-guide-azure-premium-storage] in [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide].</span></span>

3. <span data-ttu-id="8126e-221">**Nastavení**:</span><span class="sxs-lookup"><span data-stu-id="8126e-221">**Settings**:</span></span>
   * <span data-ttu-id="8126e-222">**Účet úložiště**: Vyberte existující účet úložiště nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="8126e-222">**Storage account**: Select an existing storage account or create a new one.</span></span> <span data-ttu-id="8126e-223">Ne všechny typy úložiště fungovat pro spouštění aplikací SAP.</span><span class="sxs-lookup"><span data-stu-id="8126e-223">Not all storage types work for running SAP applications.</span></span> <span data-ttu-id="8126e-224">Další informace o typech úložiště najdete v tématu [Microsoft Azure Storage] [ dbms-guide-2.3] v [databázového systému virtuální počítače Azure nasazení pro SAP v systému Linux][dbms-guide].</span><span class="sxs-lookup"><span data-stu-id="8126e-224">For more information about storage types, see [Microsoft Azure Storage][dbms-guide-2.3] in [Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide].</span></span>
   * <span data-ttu-id="8126e-225">**Virtuální síť** a **podsíť**: integrovat virtuální počítač s intranetu, vyberte virtuální síť, který je připojen k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="8126e-225">**Virtual network** and **Subnet**: To integrate the virtual machine with your intranet, select the virtual network that is connected to your on-premises network.</span></span>
   * <span data-ttu-id="8126e-226">**Veřejná IP adresa**: Vyberte veřejnou IP adresu, kterou chcete použít, nebo zadejte parametry pro vytvoření nové veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="8126e-226">**Public IP address**: Select the public IP address that you want to use, or enter parameters to create a new public IP address.</span></span> <span data-ttu-id="8126e-227">Veřejnou IP adresu můžete použít pro přístup k virtuálnímu počítači přes Internet.</span><span class="sxs-lookup"><span data-stu-id="8126e-227">You can use a public IP address to access your virtual machine over the Internet.</span></span> <span data-ttu-id="8126e-228">Ujistěte se, že vytvoříte skupinu zabezpečení sítě můžete líp zabezpečit přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="8126e-228">Make sure that you also create a network security group to help secure access to your virtual machine.</span></span>
   * <span data-ttu-id="8126e-229">**Skupina zabezpečení sítě**: Další informace najdete v tématu [řízení toku provozu sítě s skupin zabezpečení sítě][virtual-networks-nsg].</span><span class="sxs-lookup"><span data-stu-id="8126e-229">**Network security group**: For more information, see [Control network traffic flow with network security groups][virtual-networks-nsg].</span></span>
   * <span data-ttu-id="8126e-230">**Dostupnost**: Vyberte skupinu dostupnosti, nebo zadejte parametry, které chcete vytvořit novou skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="8126e-230">**Availability**: Select an availability set, or enter the parameters to create a new availability set.</span></span> <span data-ttu-id="8126e-231">Další informace najdete v tématu [skupiny dostupnosti Azure][planning-guide-3.2.3].</span><span class="sxs-lookup"><span data-stu-id="8126e-231">For more information, see [Azure availability sets][planning-guide-3.2.3].</span></span>
   * <span data-ttu-id="8126e-232">**Monitorování**: můžete vybrat **zakázat** pro monitorování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="8126e-232">**Monitoring**: You can select **Disable** for monitoring diagnostics.</span></span> <span data-ttu-id="8126e-233">Je povolena jako automaticky při spuštění příkazů povolit Azure rozšířené monitorování rozšíření, jak je popsáno v [konfigurace monitorování][deployment-guide-configure-monitoring-scenario-1].</span><span class="sxs-lookup"><span data-stu-id="8126e-233">It is enabled automatically when you run the commands to enable the Azure Enhanced Monitoring Extension, as described in [Configure monitoring][deployment-guide-configure-monitoring-scenario-1].</span></span>

4. <span data-ttu-id="8126e-234">**Souhrn**:</span><span class="sxs-lookup"><span data-stu-id="8126e-234">**Summary**:</span></span>

  <span data-ttu-id="8126e-235">Zkontrolujte váš výběr a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="8126e-235">Review your selections, and then select **OK**.</span></span>

<span data-ttu-id="8126e-236">Virtuální počítač nasazen ve skupině prostředků, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="8126e-236">Your virtual machine is deployed in the resource group you selected.</span></span>

#### <a name="create-a-virtual-machine-by-using-a-template"></a><span data-ttu-id="8126e-237">Vytvoření virtuálního počítače pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="8126e-237">Create a virtual machine by using a template</span></span>
<span data-ttu-id="8126e-238">Virtuální počítač můžete vytvořit pomocí jedné z šablon SAP publikované v [úložiště GitHub šablon azure rychlý Start][azure-quickstart-templates-github].</span><span class="sxs-lookup"><span data-stu-id="8126e-238">You can create a virtual machine by using one of the SAP templates published in the [azure-quickstart-templates GitHub repository][azure-quickstart-templates-github].</span></span> <span data-ttu-id="8126e-239">Můžete také můžete ručně vytvořit virtuální počítač pomocí [portál Azure][virtual-machines-windows-tutorial], [prostředí PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms], nebo [rozhraní příkazového řádku Azure][virtual-machines-linux-tutorial].</span><span class="sxs-lookup"><span data-stu-id="8126e-239">You also can manually create a virtual machine by using the [Azure portal][virtual-machines-windows-tutorial], [PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms], or [Azure CLI][virtual-machines-linux-tutorial].</span></span>

* <span data-ttu-id="8126e-240">[**Šablona dvouvrstvá konfigurace (jenom jeden virtuální počítač)** (sap-2vrstvy marketplace-image)][sap-templates-2-tier-marketplace-image]</span><span class="sxs-lookup"><span data-stu-id="8126e-240">[**Two-tier configuration (only one virtual machine) template** (sap-2-tier-marketplace-image)][sap-templates-2-tier-marketplace-image]</span></span>

  <span data-ttu-id="8126e-241">K vytvoření dvouvrstvé systému pomocí jenom jeden virtuální počítač, použijte tuto šablonu.</span><span class="sxs-lookup"><span data-stu-id="8126e-241">To create a two-tier system by using only one virtual machine, use this template.</span></span>
* <span data-ttu-id="8126e-242">[**Šablona třívrstvá konfigurace (více virtuálních počítačů)** (sap-3vrstvy marketplace-image)][sap-templates-3-tier-marketplace-image]</span><span class="sxs-lookup"><span data-stu-id="8126e-242">[**Three-tier configuration (multiple virtual machines) template** (sap-3-tier-marketplace-image)][sap-templates-3-tier-marketplace-image]</span></span>

  <span data-ttu-id="8126e-243">K vytvoření třívrstvé systému pomocí více virtuálních počítačů, použijte tuto šablonu.</span><span class="sxs-lookup"><span data-stu-id="8126e-243">To create a three-tier system by using multiple virtual machines, use this template.</span></span>

<span data-ttu-id="8126e-244">Na portálu Azure zadejte následující parametry šablony:</span><span class="sxs-lookup"><span data-stu-id="8126e-244">In the Azure portal, enter the following parameters for the template:</span></span>

1. <span data-ttu-id="8126e-245">**Základy**:</span><span class="sxs-lookup"><span data-stu-id="8126e-245">**Basics**:</span></span>
  * <span data-ttu-id="8126e-246">**Předplatné**: předplatné pro použití k nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="8126e-246">**Subscription**: The subscription to use to deploy the template.</span></span>
  * <span data-ttu-id="8126e-247">**Skupina prostředků**: skupiny prostředků můžete použít k nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="8126e-247">**Resource group**: The resource group to use to deploy the template.</span></span> <span data-ttu-id="8126e-248">Můžete vytvořit novou skupinu prostředků, nebo můžete vybrat existující skupinu prostředků v předplatném.</span><span class="sxs-lookup"><span data-stu-id="8126e-248">You can create a new resource group, or you can select an existing resource group in the subscription.</span></span>
  * <span data-ttu-id="8126e-249">**Umístění**: kde chcete nasadit šablonu.</span><span class="sxs-lookup"><span data-stu-id="8126e-249">**Location**: Where to deploy the template.</span></span> <span data-ttu-id="8126e-250">Pokud jste vybrali existující skupinu prostředků, použije se umístění této skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="8126e-250">If you selected an existing resource group, the location of that resource group is used.</span></span>

2. <span data-ttu-id="8126e-251">**Nastavení**:</span><span class="sxs-lookup"><span data-stu-id="8126e-251">**Settings**:</span></span>
  * <span data-ttu-id="8126e-252">**ID systému SAP**: ID systému SAP (SID).</span><span class="sxs-lookup"><span data-stu-id="8126e-252">**SAP System ID**: The SAP System ID (SID).</span></span>
  * <span data-ttu-id="8126e-253">**Typ operačního systému**: operačního systému, kterou chcete nasadit, například, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12) nebo Red Hat Enterprise Linux 7.2 (RHEL 7.2).</span><span class="sxs-lookup"><span data-stu-id="8126e-253">**OS type**: The operating system you want to deploy, for example, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), or Red Hat Enterprise Linux 7.2 (RHEL 7.2).</span></span>

    <span data-ttu-id="8126e-254">Výchozí zobrazení seznamu nezobrazuje, že všechny podporované operační systémy.</span><span class="sxs-lookup"><span data-stu-id="8126e-254">The default list view does not show all supported operating systems.</span></span> <span data-ttu-id="8126e-255">Vyberte **zobrazit všechny** úplný seznam.</span><span class="sxs-lookup"><span data-stu-id="8126e-255">Select **see all** for a full list.</span></span> <span data-ttu-id="8126e-256">Další informace o podporovaných operačních systémech pro nasazení softwaru SAP, viz poznámka SAP [1928533].</span><span class="sxs-lookup"><span data-stu-id="8126e-256">For more information about supported operating systems for SAP software deployment, see SAP Note [1928533].</span></span>
  * <span data-ttu-id="8126e-257">**Velikost systému SAP**: velikost systému SAP.</span><span class="sxs-lookup"><span data-stu-id="8126e-257">**SAP system size**: The size of the SAP system.</span></span>

    <span data-ttu-id="8126e-258">Počet protokoly SAP poskytuje nový systém.</span><span class="sxs-lookup"><span data-stu-id="8126e-258">The number of SAPS the new system provides.</span></span> <span data-ttu-id="8126e-259">Pokud si nejste jisti kolik protokoly SAP vyžaduje systém, obraťte se na partnera technologie SAP nebo systémový integrátor.</span><span class="sxs-lookup"><span data-stu-id="8126e-259">If you are not sure how many SAPS the system requires, ask your SAP Technology Partner or System Integrator.</span></span>
  * <span data-ttu-id="8126e-260">**Dostupnost systému** (pouze šablony třívrstvá): dostupnost systému.</span><span class="sxs-lookup"><span data-stu-id="8126e-260">**System availability** (three-tier template only): The system availability.</span></span>

    <span data-ttu-id="8126e-261">Vyberte **HA** konfigurace, který je vhodný pro instalaci s vysokou dostupností.</span><span class="sxs-lookup"><span data-stu-id="8126e-261">Select **HA** for a configuration that is suitable for a high-availability installation.</span></span> <span data-ttu-id="8126e-262">Vytvoří se dvou databázové servery a dva servery pro ABAP SAP centrální služby (ASC).</span><span class="sxs-lookup"><span data-stu-id="8126e-262">Two database servers and two servers for ABAP SAP Central Services (ASCS) are created.</span></span>
  * <span data-ttu-id="8126e-263">**Typ úložiště** (pouze šablony dvouvrstvá): typ úložiště používat.</span><span class="sxs-lookup"><span data-stu-id="8126e-263">**Storage type** (two-tier template only): The type of storage to use.</span></span>

    <span data-ttu-id="8126e-264">U větších systémů důrazně doporučujeme pomocí Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="8126e-264">For larger systems, we highly recommend using Azure Premium Storage.</span></span> <span data-ttu-id="8126e-265">Další informace o typech úložiště najdete v těchto zdrojích:</span><span class="sxs-lookup"><span data-stu-id="8126e-265">For more information about storage types, see these resources:</span></span>
      * <span data-ttu-id="8126e-266">[Používání úložiště Azure Premium SSD pro instanci databázového systému SAP][2367194]</span><span class="sxs-lookup"><span data-stu-id="8126e-266">[Use of Azure Premium SSD Storage for SAP DBMS Instance][2367194]</span></span>
      * <span data-ttu-id="8126e-267">[Microsoft Azure Storage] [ dbms-guide-2.3] v [databázového systému virtuální počítače Azure nasazení pro SAP v systému Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="8126e-267">[Microsoft Azure Storage][dbms-guide-2.3] in [Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
      * <span data-ttu-id="8126e-268">[Úložiště Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure][storage-premium-storage-preview-portal]</span><span class="sxs-lookup"><span data-stu-id="8126e-268">[Premium Storage: High-performance storage for Azure Virtual Machine workloads][storage-premium-storage-preview-portal]</span></span>
      * <span data-ttu-id="8126e-269">[Úvod do Microsoft Azure Storage][storage-introduction]</span><span class="sxs-lookup"><span data-stu-id="8126e-269">[Introduction to Microsoft Azure Storage][storage-introduction]</span></span>
  * <span data-ttu-id="8126e-270">**Uživatelské jméno správce** a **heslo správce**: uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="8126e-270">**Admin username** and **Admin password**: A username and password.</span></span>
    <span data-ttu-id="8126e-271">Po vytvoření nového uživatele pro přihlášení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="8126e-271">A new user is created, for signing in to the virtual machine.</span></span>
  * <span data-ttu-id="8126e-272">**Nový nebo existující podsíť**: Určuje, zda se vytvoří nová virtuální síť a podsíť, nebo se používá existující podsítí.</span><span class="sxs-lookup"><span data-stu-id="8126e-272">**New or existing subnet**: Determines whether a new virtual network and subnet are  created or an existing subnet is used.</span></span> <span data-ttu-id="8126e-273">Pokud již máte virtuální síť, která je připojen k síti na pracovišti, vyberte **existující**.</span><span class="sxs-lookup"><span data-stu-id="8126e-273">If you already have a virtual network that is connected to your on-premises network, select **Existing**.</span></span>
  * <span data-ttu-id="8126e-274">**ID podsítě**: ID podsítě virtuálních počítačů se připojí k.</span><span class="sxs-lookup"><span data-stu-id="8126e-274">**Subnet ID**: The ID of the subnet the virtual machines will connect to.</span></span> <span data-ttu-id="8126e-275">Vyberte podsíť virtuální sítě Azure ExpressRoute pro použití pro připojení k síti na pracovišti virtuální počítač nebo virtuální privátní sítě (VPN).</span><span class="sxs-lookup"><span data-stu-id="8126e-275">Select the subnet of your virtual private network (VPN) or Azure ExpressRoute virtual network to use to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="8126e-276">ID obvykle vypadá takto: /subscriptions/&lt;id předplatného > /resourceGroups/&lt;název skupiny prostředků > /providers/Microsoft.Network/virtualNetworks/&lt;název virtuální sítě > /subnets/&lt;název podsítě ></span><span class="sxs-lookup"><span data-stu-id="8126e-276">The ID usually looks like this: /subscriptions/&lt;subscription id>/resourceGroups/&lt;resource group name>/providers/Microsoft.Network/virtualNetworks/&lt;virtual network name>/subnets/&lt;subnet name></span></span>

3. <span data-ttu-id="8126e-277">**Podmínky a ujednání**:</span><span class="sxs-lookup"><span data-stu-id="8126e-277">**Terms and conditions**:</span></span>  
    <span data-ttu-id="8126e-278">Přečtěte si a přijměte právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="8126e-278">Review and accept the legal terms.</span></span>

4.  <span data-ttu-id="8126e-279">Vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="8126e-279">Select **Purchase**.</span></span>

<span data-ttu-id="8126e-280">Ve výchozím nastavení je nasadit agenta virtuálního počítače Azure, při použití bitovou kopii z Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="8126e-280">The Azure VM Agent is deployed by default when you use an image from the Azure Marketplace.</span></span>

#### <a name="configure-proxy-settings"></a><span data-ttu-id="8126e-281">Konfigurace nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="8126e-281">Configure proxy settings</span></span>
<span data-ttu-id="8126e-282">V závislosti na konfiguraci místní sítě může být potřeba nastavení serveru proxy na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="8126e-282">Depending on how your on-premises network is configured, you might need to set up the proxy on your VM.</span></span> <span data-ttu-id="8126e-283">Pokud virtuální počítač je připojený k vaší místní sítě prostřednictvím sítě VPN nebo ExpressRoute, virtuální počítač nemusí být možné získat přístup k Internetu a nebude ji již možné stáhnout požadované rozšíření nebo shromažďovat data monitorování.</span><span class="sxs-lookup"><span data-stu-id="8126e-283">If your VM is connected to your on-premises network via VPN or ExpressRoute, the VM might not be able to access the Internet, and won't be able to download the required extensions or collect monitoring data.</span></span> <span data-ttu-id="8126e-284">Další informace najdete v tématu [proxy server nakonfigurovat][deployment-guide-configure-proxy].</span><span class="sxs-lookup"><span data-stu-id="8126e-284">For more information, see [Configure the proxy][deployment-guide-configure-proxy].</span></span>

#### <a name="join-a-domain-windows-only"></a><span data-ttu-id="8126e-285">Připojení k doméně (jenom Windows)</span><span class="sxs-lookup"><span data-stu-id="8126e-285">Join a domain (Windows only)</span></span>
<span data-ttu-id="8126e-286">Pokud vaše nasazení Azure je připojen k místní instance služby Active Directory nebo DNS prostřednictvím Azure připojení site-to-site VPN nebo ExpressRoute (to se označuje jako *mezi různými místy* v [virtuální počítače Azure plánování a implementace pro SAP v systému Linux][planning-guide]), očekává se, že virtuální počítač je připojení k místní doméně.</span><span class="sxs-lookup"><span data-stu-id="8126e-286">If your Azure deployment is connected to an on-premises Active Directory or DNS instance via an Azure site-to-site VPN connection or ExpressRoute (this is called *cross-premises* in [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]), it is expected that the VM is joining an on-premises domain.</span></span> <span data-ttu-id="8126e-287">Další informace o požadavcích pro tuto úlohu najdete v tématu [připojit virtuální počítač k doméně místní (jenom Windows)][deployment-guide-4.3].</span><span class="sxs-lookup"><span data-stu-id="8126e-287">For more information about considerations for this task, see [Join a VM to an on-premises domain (Windows only)][deployment-guide-4.3].</span></span>

#### <span data-ttu-id="8126e-288"><a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Konfigurace monitorování</span><span class="sxs-lookup"><span data-stu-id="8126e-288"><a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Configure monitoring</span></span>
<span data-ttu-id="8126e-289">Aby byly vaše prostředí podporuje SAP, nastavit rozšíření monitorování Azure pro SAP, jak je popsáno v [konfiguraci Azure rozšířené monitorování rozšíření přístup k SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="8126e-289">To be sure your environment supports SAP, set up the Azure Monitoring Extension for SAP as described in [Configure the Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span> <span data-ttu-id="8126e-290">Kontrola předpokladů pro SAP monitorování a požadovaná minimální verze SAP jádra a Agent hostitele SAP, v prostředky uvedené v [SAP prostředky][deployment-guide-2.2].</span><span class="sxs-lookup"><span data-stu-id="8126e-290">Check the prerequisites for SAP monitoring, and required minimum versions of SAP Kernel and SAP Host Agent, in the resources listed in [SAP resources][deployment-guide-2.2].</span></span>

#### <a name="monitoring-check"></a><span data-ttu-id="8126e-291">Monitorování kontroly</span><span class="sxs-lookup"><span data-stu-id="8126e-291">Monitoring check</span></span>
<span data-ttu-id="8126e-292">Zkontrolujte, zda monitorování funguje, jak je popsáno v [kontroly a řešení potíží pro nastavení sledování začátku do konce][deployment-guide-troubleshooting-chapter].</span><span class="sxs-lookup"><span data-stu-id="8126e-292">Check whether monitoring is working, as described in [Checks and troubleshooting for setting up end-to-end monitoring][deployment-guide-troubleshooting-chapter].</span></span>

#### <a name="post-deployment-steps"></a><span data-ttu-id="8126e-293">Kroky po nasazení</span><span class="sxs-lookup"><span data-stu-id="8126e-293">Post-deployment steps</span></span>
<span data-ttu-id="8126e-294">Po můžete vytvořit virtuální počítač a nasazení virtuálního počítače, musíte nainstalovat požadované softwarové komponenty ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="8126e-294">After you create the VM and the VM is deployed, you need to install the required software components in the VM.</span></span> <span data-ttu-id="8126e-295">Z důvodu pořadí nasazení a software instalace v tomto typu nasazení virtuálního počítače nainstalovat software již musí být k dispozici, buď v Azure, na jiný počítač nebo jako disk, který lze připojit.</span><span class="sxs-lookup"><span data-stu-id="8126e-295">Because of the deployment/software installation sequence in this type of VM deployment, the software to be installed must already be available, either in Azure, on another VM, or as a disk that can be attached.</span></span> <span data-ttu-id="8126e-296">Nebo, zvažte použití mezi různými místy scénář, ve které připojení k místní je uvedeno prostředky (Instalace sdílených složek).</span><span class="sxs-lookup"><span data-stu-id="8126e-296">Or, consider using a cross-premises scenario, in which connectivity to the on-premises assets (installation shares) is given.</span></span>

<span data-ttu-id="8126e-297">Po nasazení virtuálního počítače v Azure, postupujte podle stejné pokyny a nástroje pro instalaci softwaru SAP na vašem virtuálním počítači, jako byste v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8126e-297">After you deploy your VM in Azure, follow the same guidelines and tools to install the SAP software on your VM as you would in an on-premises environment.</span></span> <span data-ttu-id="8126e-298">K instalaci softwaru SAP do virtuálního počítače Azure, doporučujeme SAP a Microsoft nahrát a uložit na instalačním médiu SAP na virtuální pevné disky Azure, nebo vytvořit virtuální počítač Azure, funguje jako souborový server, který obsahuje všechny požadované SAP instalačního média.</span><span class="sxs-lookup"><span data-stu-id="8126e-298">To install SAP software on an Azure VM, both SAP and Microsoft recommend that you upload and store the SAP installation media on Azure VHDs, or that you create an Azure VM that works as a file server that has all the required SAP installation media.</span></span>

<span data-ttu-id="8126e-299">[comment]: <> (Proč to potřebujeme doporučit Správa souborů, například TODO MSSedusch souborového serveru nebo virtuálního pevného disku? Je, že tak liší od místně?)</span><span class="sxs-lookup"><span data-stu-id="8126e-299">[comment]: <> (MSSedusch TODO why do we need to recommend a file management, for example, File Server or VHD? Is that so different from on-premises?)</span></span>

### <span data-ttu-id="8126e-300"><a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Scénář 2: Nasazení virtuálního počítače s vlastní image pro SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-300"><a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Scenario 2: Deploying a VM with a custom image for SAP</span></span>
<span data-ttu-id="8126e-301">Vzhledem k tomu, že různé verze operačního systému nebo databázového systému požadavky na jiné opravy, bitových kopií, které můžete najít v Azure Marketplace nemusí podle svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="8126e-301">Because different versions of an operating system or DBMS have different patch requirements, the images you find in the Azure Marketplace might not meet your needs.</span></span> <span data-ttu-id="8126e-302">Místo toho můžete chtít vytvořit virtuální počítač pomocí vlastní bitovou kopii operačního systému nebo databázového systému virtuálního počítače, kterou můžete nasadit znovu později.</span><span class="sxs-lookup"><span data-stu-id="8126e-302">You might instead want to create a VM by using your own OS/DBMS VM image, which you can deploy again later.</span></span>
<span data-ttu-id="8126e-303">Použijete různé kroky k vytvoření bitové kopie privátní pro Linux než chcete vytvořit jednu pro Windows.</span><span class="sxs-lookup"><span data-stu-id="8126e-303">You use different steps to create a private image for Linux than to create one for Windows.</span></span>

- - -
> ![Windows][Logo_Windows] <span data-ttu-id="8126e-305">Windows</span><span class="sxs-lookup"><span data-stu-id="8126e-305">Windows</span></span>
>
> <span data-ttu-id="8126e-306">Příprava bitové kopie systému Windows, který můžete použít k nasazení více virtuálních počítačů, musí být abstrahované nebo zobecněn ve virtuálním počítači místní nastavení systému Windows (např. Windows SID a název hostitele).</span><span class="sxs-lookup"><span data-stu-id="8126e-306">To prepare a Windows image that you can use to deploy multiple virtual machines, the Windows settings (like Windows SID and hostname) must be abstracted or generalized on the on-premises VM.</span></span> <span data-ttu-id="8126e-307">Můžete použít [sysprep](https://msdn.microsoft.com/library/hh825084.aspx) k tomu.</span><span class="sxs-lookup"><span data-stu-id="8126e-307">You can use [sysprep](https://msdn.microsoft.com/library/hh825084.aspx) to do this.</span></span>
>
> ![Linux][Logo_Linux] <span data-ttu-id="8126e-309">Linux</span><span class="sxs-lookup"><span data-stu-id="8126e-309">Linux</span></span>
>
> <span data-ttu-id="8126e-310">Chcete-li připravit bitovou kopii systému Linux, který můžete použít k nasazení více virtuálních počítačů, musí být některá nastavení Linux abstrahované nebo zobecněn na místní počítač.</span><span class="sxs-lookup"><span data-stu-id="8126e-310">To prepare a Linux image that you can use to deploy multiple virtual machines, some Linux settings must be abstracted or generalized on the on-premises VM.</span></span> <span data-ttu-id="8126e-311">Můžete použít `waagent -deprovision` k tomu.</span><span class="sxs-lookup"><span data-stu-id="8126e-311">You can use `waagent -deprovision`  to do this.</span></span> <span data-ttu-id="8126e-312">Další informace najdete v tématu [zachytit virtuální počítač Linux spuštěné v Azure] [ virtual-machines-linux-capture-image] a [Azure Linux agent uživatelská příručka][virtual-machines-linux-agent-user-guide-command-line-options].</span><span class="sxs-lookup"><span data-stu-id="8126e-312">For more information, see [Capture a Linux virtual machine running on Azure][virtual-machines-linux-capture-image] and the [Azure Linux agent user guide][virtual-machines-linux-agent-user-guide-command-line-options].</span></span>
>
>

- - -
<span data-ttu-id="8126e-313">Příprava a vytvořit vlastní image a pak ho používali k vytváření více nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="8126e-313">You can prepare and create a custom image, and then use it to create multiple new VMs.</span></span> <span data-ttu-id="8126e-314">To je popsáno v [virtuální počítače Azure plánování a implementace pro SAP v systému Linux][planning-guide].</span><span class="sxs-lookup"><span data-stu-id="8126e-314">This is described in [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide].</span></span> <span data-ttu-id="8126e-315">Nastavení databáze obsahu pomocí Správce zřizování společnosti SAP softwaru k instalaci nového systému SAP (Obnoví zálohu databáze z virtuálního pevného disku, který je připojen k virtuálnímu počítači) nebo přímo obnovením zálohy databáze ze služby Azure storage, pokud ji podporuje vaše databázového systému.</span><span class="sxs-lookup"><span data-stu-id="8126e-315">Set up your database content either by using SAP Software Provisioning Manager to install a new SAP system (restores a database backup from a VHD that's attached to the virtual machine) or by directly restoring a database backup from Azure storage, if your DBMS supports it.</span></span> <span data-ttu-id="8126e-316">Další informace najdete v tématu [databázového systému virtuální počítače Azure nasazení pro SAP v systému Linux][dbms-guide].</span><span class="sxs-lookup"><span data-stu-id="8126e-316">For more information, see [Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide].</span></span> <span data-ttu-id="8126e-317">Pokud jste již nainstalovali systému SAP na váš místní virtuální počítač (hlavně u dvouvrstvá systémy), můžete upravit nastavení systému SAP po nasazení virtuálního počítače Azure pomocí postupu přejmenovat systému nepodporuje správce zřizování softwaru SAP (Poznámka SAP [1619720]).</span><span class="sxs-lookup"><span data-stu-id="8126e-317">If you have already installed an SAP system on your on-premises VM (especially for two-tier systems), you can adapt the SAP system settings after the deployment of the Azure VM by using the System Rename procedure supported by SAP Software Provisioning Manager (SAP Note [1619720]).</span></span> <span data-ttu-id="8126e-318">Jinak můžete nainstalovat SAP software po nasazení virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-318">Otherwise, you can install the SAP software after you deploy the Azure VM.</span></span>

<span data-ttu-id="8126e-319">Následující diagram ukazuje SAP konkrétní pořadí kroků pro nasazení virtuálního počítače z vlastní image:</span><span class="sxs-lookup"><span data-stu-id="8126e-319">The following flowchart shows the SAP-specific sequence of steps for deploying a VM from a custom image:</span></span>

![Vývojový diagram nasazení virtuálního počítače pro systémy SAP pomocí bitové kopie virtuálních počítačů v privátní Marketplace][deployment-guide-figure-300]

#### <a name="create-the-virtual-machine"></a><span data-ttu-id="8126e-321">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8126e-321">Create the virtual machine</span></span>
<span data-ttu-id="8126e-322">K vytvoření nasazení s použitím privátní bitové kopie operačního systému z portálu Azure, použijte jednu z následujících šablon SAP.</span><span class="sxs-lookup"><span data-stu-id="8126e-322">To create a deployment by using a private OS image from the Azure portal, use one of the following SAP templates.</span></span> <span data-ttu-id="8126e-323">Tyto šablony jsou publikovány v [úložiště GitHub šablon azure rychlý Start][azure-quickstart-templates-github].</span><span class="sxs-lookup"><span data-stu-id="8126e-323">These templates are published in the [azure-quickstart-templates GitHub repository][azure-quickstart-templates-github].</span></span> <span data-ttu-id="8126e-324">Můžete také můžete ručně vytvořit virtuální počítač pomocí [prostředí PowerShell][virtual-machines-upload-image-windows-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="8126e-324">You also can manually create a virtual machine, by using [PowerShell][virtual-machines-upload-image-windows-resource-manager].</span></span>

* <span data-ttu-id="8126e-325">[**Šablona dvouvrstvá konfigurace (jenom jeden virtuální počítač)** (sap-2vrstvy uživatel image)][sap-templates-2-tier-user-image]</span><span class="sxs-lookup"><span data-stu-id="8126e-325">[**Two-tier configuration (only one virtual machine) template** (sap-2-tier-user-image)][sap-templates-2-tier-user-image]</span></span>

  <span data-ttu-id="8126e-326">K vytvoření dvouvrstvé systému pomocí jenom jeden virtuální počítač, použijte tuto šablonu.</span><span class="sxs-lookup"><span data-stu-id="8126e-326">To create a two-tier system by using only one virtual machine, use this template.</span></span>
* <span data-ttu-id="8126e-327">[**Šablona třívrstvá konfigurace (více virtuálních počítačů)** (sap-3vrstvy uživatel image)][sap-templates-3-tier-user-image]</span><span class="sxs-lookup"><span data-stu-id="8126e-327">[**Three-tier configuration (multiple virtual machines) template** (sap-3-tier-user-image)][sap-templates-3-tier-user-image]</span></span>

  <span data-ttu-id="8126e-328">K vytvoření třívrstvé systému pomocí více virtuálních počítačů nebo vlastní image operačního systému, použijte tuto šablonu.</span><span class="sxs-lookup"><span data-stu-id="8126e-328">To create a three-tier system by using multiple virtual machines or your own OS image, use this template.</span></span>

<span data-ttu-id="8126e-329">Na portálu Azure zadejte následující parametry šablony:</span><span class="sxs-lookup"><span data-stu-id="8126e-329">In the Azure portal, enter the following parameters for the template:</span></span>

1. <span data-ttu-id="8126e-330">**Základy**:</span><span class="sxs-lookup"><span data-stu-id="8126e-330">**Basics**:</span></span>
  * <span data-ttu-id="8126e-331">**Předplatné**: předplatné pro použití k nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="8126e-331">**Subscription**: The subscription to use to deploy the template.</span></span>
  * <span data-ttu-id="8126e-332">**Skupina prostředků**: skupiny prostředků můžete použít k nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="8126e-332">**Resource group**: The resource group to use to deploy the template.</span></span> <span data-ttu-id="8126e-333">Můžete vytvořit novou skupinu prostředků nebo vyberte existující skupinu prostředků v předplatném.</span><span class="sxs-lookup"><span data-stu-id="8126e-333">You can create a new resource group or select an existing resource group in the subscription.</span></span>
  * <span data-ttu-id="8126e-334">**Umístění**: kde chcete nasadit šablonu.</span><span class="sxs-lookup"><span data-stu-id="8126e-334">**Location**: Where to deploy the template.</span></span> <span data-ttu-id="8126e-335">Pokud jste vybrali existující skupinu prostředků, použije se umístění této skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="8126e-335">If you selected an existing resource group, the location of that resource group is used.</span></span>
2. <span data-ttu-id="8126e-336">**Nastavení**:</span><span class="sxs-lookup"><span data-stu-id="8126e-336">**Settings**:</span></span>
  * <span data-ttu-id="8126e-337">**ID systému SAP**: ID SAP systému.</span><span class="sxs-lookup"><span data-stu-id="8126e-337">**SAP System ID**: The SAP System ID.</span></span>
  * <span data-ttu-id="8126e-338">**Typ operačního systému**: typ operačního systému, kterou chcete nasadit (Windows nebo Linux).</span><span class="sxs-lookup"><span data-stu-id="8126e-338">**OS type**: The operating system type you want to deploy (Windows or Linux).</span></span>
  * <span data-ttu-id="8126e-339">**Velikost systému SAP**: velikost systému SAP.</span><span class="sxs-lookup"><span data-stu-id="8126e-339">**SAP system size**: The size of the SAP system.</span></span>

    <span data-ttu-id="8126e-340">Počet protokoly SAP poskytuje nový systém.</span><span class="sxs-lookup"><span data-stu-id="8126e-340">The number of SAPS the new system provides.</span></span> <span data-ttu-id="8126e-341">Pokud si nejste jisti kolik protokoly SAP vyžaduje systém, obraťte se na partnera technologie SAP nebo systémový integrátor.</span><span class="sxs-lookup"><span data-stu-id="8126e-341">If you are not sure how many SAPS the system requires, ask your SAP Technology Partner or System Integrator.</span></span>
  * <span data-ttu-id="8126e-342">**Dostupnost systému** (pouze šablony třívrstvá): dostupnost systému.</span><span class="sxs-lookup"><span data-stu-id="8126e-342">**System availability** (three-tier template only): The system availability.</span></span>

    <span data-ttu-id="8126e-343">Vyberte **HA** konfigurace, který je vhodný pro instalaci s vysokou dostupností.</span><span class="sxs-lookup"><span data-stu-id="8126e-343">Select **HA** for a configuration that is suitable for a high-availability installation.</span></span> <span data-ttu-id="8126e-344">Vytvoří se dvou databázové servery a dva servery pro ASC.</span><span class="sxs-lookup"><span data-stu-id="8126e-344">Two database servers and two servers for ASCS are created.</span></span>
  * <span data-ttu-id="8126e-345">**Typ úložiště** (pouze šablony dvouvrstvá): typ úložiště používat.</span><span class="sxs-lookup"><span data-stu-id="8126e-345">**Storage type** (two-tier template only): The type of storage to use.</span></span>

    <span data-ttu-id="8126e-346">U větších systémů důrazně doporučujeme pomocí Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="8126e-346">For larger systems, we highly recommend using Azure Premium Storage.</span></span> <span data-ttu-id="8126e-347">Další informace o typech úložiště najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="8126e-347">For more information about storage types, see the following resources:</span></span>
      * <span data-ttu-id="8126e-348">[Používání úložiště Azure Premium SSD pro instanci databázového systému SAP][2367194]</span><span class="sxs-lookup"><span data-stu-id="8126e-348">[Use of Azure Premium SSD Storage for SAP DBMS Instance][2367194]</span></span>
      * <span data-ttu-id="8126e-349">[Microsoft Azure Storage] [ dbms-guide-2.3] v [databázového systému virtuální počítače Azure nasazení pro SAP v systému Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="8126e-349">[Microsoft Azure Storage][dbms-guide-2.3] in [Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
      * <span data-ttu-id="8126e-350">[Úložiště Premium: Vysoce výkonné úložiště pro úlohy virtuálního počítače Azure][storage-premium-storage-preview-portal]</span><span class="sxs-lookup"><span data-stu-id="8126e-350">[Premium Storage: High-performance storage for Azure virtual machine workloads][storage-premium-storage-preview-portal]</span></span>
      * <span data-ttu-id="8126e-351">[Úvod do Microsoft Azure Storage][storage-introduction]</span><span class="sxs-lookup"><span data-stu-id="8126e-351">[Introduction to Microsoft Azure Storage][storage-introduction]</span></span>
  * <span data-ttu-id="8126e-352">**Uživatelská image URI virtuálního pevného disku**: identifikátoru URI privátní operačního systému bitové kopie disku VHD, například https://&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.</span><span class="sxs-lookup"><span data-stu-id="8126e-352">**User image VHD URI**: The URI of the private OS image VHD, for example, https://&lt;accountname>.blob.core.windows.net/vhds/userimage.vhd.</span></span>
  * <span data-ttu-id="8126e-353">**Účet úložiště bitových kopií uživatele**: název účtu úložiště privátní bitové kopie operačního systému se uloží, například &lt;accountname > v https://&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.</span><span class="sxs-lookup"><span data-stu-id="8126e-353">**User image storage account**: The name of the storage account where the private OS image is stored, for example, &lt;accountname> in https://&lt;accountname>.blob.core.windows.net/vhds/userimage.vhd.</span></span>
  * <span data-ttu-id="8126e-354">**Uživatelské jméno správce** a **heslo správce**: uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="8126e-354">**Admin username** and **Admin password**: The username and password.</span></span>

    <span data-ttu-id="8126e-355">Po vytvoření nového uživatele pro přihlášení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="8126e-355">A new user is created, for signing in to the virtual machine.</span></span>
  * <span data-ttu-id="8126e-356">**Nový nebo existující podsíť**: Určuje, zda se vytvoří nová virtuální síť a podsíť, nebo se používá existující podsítí.</span><span class="sxs-lookup"><span data-stu-id="8126e-356">**New or existing subnet**: Determines whether a new virtual network and subnet is created or an existing subnet is used.</span></span> <span data-ttu-id="8126e-357">Pokud již máte virtuální síť, která je připojen k síti na pracovišti, vyberte **existující**.</span><span class="sxs-lookup"><span data-stu-id="8126e-357">If you already have a virtual network that is connected to your on-premises network, select **Existing**.</span></span>
  * <span data-ttu-id="8126e-358">**ID podsítě**: ID podsítě, do které budou virtuální počítače připojit k.</span><span class="sxs-lookup"><span data-stu-id="8126e-358">**Subnet ID**: The ID of the subnet to which the virtual machines will connect to.</span></span> <span data-ttu-id="8126e-359">Vyberte podsíť virtuální sítě VPN nebo ExpressRoute sloužící k připojení virtuálního počítače k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="8126e-359">Select the subnet of your VPN or ExpressRoute virtual network to use to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="8126e-360">ID obvykle vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="8126e-360">The ID usually looks like this:</span></span>

    <span data-ttu-id="8126e-361">/subscriptions/&lt;id předplatného > /resourceGroups/&lt;název skupiny prostředků > /providers/Microsoft.Network/virtualNetworks/&lt;název virtuální sítě > /subnets/&lt;název podsítě ></span><span class="sxs-lookup"><span data-stu-id="8126e-361">/subscriptions/&lt;subscription id>/resourceGroups/&lt;resource group name>/providers/Microsoft.Network/virtualNetworks/&lt;virtual network name>/subnets/&lt;subnet name></span></span>

3. <span data-ttu-id="8126e-362">**Podmínky a ujednání**:</span><span class="sxs-lookup"><span data-stu-id="8126e-362">**Terms and conditions**:</span></span>  
    <span data-ttu-id="8126e-363">Přečtěte si a přijměte právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="8126e-363">Review and accept the legal terms.</span></span>

4.  <span data-ttu-id="8126e-364">Vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="8126e-364">Select **Purchase**.</span></span>

#### <a name="install-the-vm-agent-linux-only"></a><span data-ttu-id="8126e-365">Nainstalujte agenta virtuálního počítače (pouze Linux)</span><span class="sxs-lookup"><span data-stu-id="8126e-365">Install the VM Agent (Linux only)</span></span>
<span data-ttu-id="8126e-366">Použití šablon popsané v předchozí části, musíte Linux Agent již nainstalován v bitové kopii uživatele nebo nasazení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="8126e-366">To use the templates described in the preceding section, the Linux Agent must already be installed in the user image, or the deployment will fail.</span></span> <span data-ttu-id="8126e-367">Stáhněte a nainstalujte agenta virtuálního počítače v bitové kopii uživatele, jak je popsáno v [stáhnout, nainstalovat a povolit agenta virtuálního počítače Azure][deployment-guide-4.4].</span><span class="sxs-lookup"><span data-stu-id="8126e-367">Download and install the VM Agent in the user image as described in [Download, install, and enable the Azure VM Agent][deployment-guide-4.4].</span></span> <span data-ttu-id="8126e-368">Pokud nepoužíváte šablony, můžete také nainstalovat agenta virtuálního počítače později.</span><span class="sxs-lookup"><span data-stu-id="8126e-368">If you don’t use the templates, you also can install the VM Agent later.</span></span>

#### <a name="join-a-domain-windows-only"></a><span data-ttu-id="8126e-369">Připojení k doméně (jenom Windows)</span><span class="sxs-lookup"><span data-stu-id="8126e-369">Join a domain (Windows only)</span></span>
<span data-ttu-id="8126e-370">Pokud vaše nasazení Azure je připojen k místní instance služby Active Directory nebo DNS prostřednictvím Azure připojení site-to-site VPN nebo Azure ExpressRoute (to se označuje jako *mezi různými místy* v [virtuální počítače Azure plánování a implementace pro SAP v systému Linux][planning-guide]), očekává se, že virtuální počítač je připojení k místní doméně.</span><span class="sxs-lookup"><span data-stu-id="8126e-370">If your Azure deployment is connected to an on-premises Active Directory or DNS instance via an Azure site-to-site VPN connection or Azure ExpressRoute (this is called *cross-premises* in [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]), it is expected that the VM is joining an on-premises domain.</span></span> <span data-ttu-id="8126e-371">Další informace o aspektech týkajících se pro tento krok najdete v tématu [připojit virtuální počítač k doméně místní (jenom Windows)][deployment-guide-4.3].</span><span class="sxs-lookup"><span data-stu-id="8126e-371">For more information about considerations for this step, see [Join a VM to an on-premises domain (Windows only)][deployment-guide-4.3].</span></span>

#### <a name="configure-proxy-settings"></a><span data-ttu-id="8126e-372">Konfigurace nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="8126e-372">Configure proxy settings</span></span>
<span data-ttu-id="8126e-373">V závislosti na konfiguraci místní sítě může být potřeba nastavení serveru proxy na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="8126e-373">Depending on how your on-premises network is configured, you might need to set up the proxy on your VM.</span></span> <span data-ttu-id="8126e-374">Pokud virtuální počítač je připojený k vaší místní sítě prostřednictvím sítě VPN nebo ExpressRoute, virtuální počítač nemusí být možné získat přístup k Internetu a nebude ji již možné stáhnout požadované rozšíření nebo shromažďovat data monitorování.</span><span class="sxs-lookup"><span data-stu-id="8126e-374">If your VM is connected to your on-premises network via VPN or ExpressRoute, the VM might not be able to access the Internet, and won't be able to download the required extensions or collect monitoring data.</span></span> <span data-ttu-id="8126e-375">Další informace najdete v tématu [proxy server nakonfigurovat][deployment-guide-configure-proxy].</span><span class="sxs-lookup"><span data-stu-id="8126e-375">For more information, see [Configure the proxy][deployment-guide-configure-proxy].</span></span>

#### <a name="configure-monitoring"></a><span data-ttu-id="8126e-376">Konfigurace monitorování</span><span class="sxs-lookup"><span data-stu-id="8126e-376">Configure monitoring</span></span>
<span data-ttu-id="8126e-377">Aby byly vaše prostředí podporuje SAP, nastavit rozšíření monitorování Azure pro SAP, jak je popsáno v [konfiguraci Azure rozšířené monitorování rozšíření přístup k SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="8126e-377">To be sure your environment supports SAP, set up the Azure Monitoring Extension for SAP as described in [Configure the Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span> <span data-ttu-id="8126e-378">Kontrola předpokladů pro SAP monitorování a požadovaná minimální verze SAP jádra a Agent hostitele SAP, v prostředky uvedené v [SAP prostředky][deployment-guide-2.2].</span><span class="sxs-lookup"><span data-stu-id="8126e-378">Check the prerequisites for SAP monitoring, and required minimum versions of SAP Kernel and SAP Host Agent, in the resources listed in [SAP resources][deployment-guide-2.2].</span></span>

#### <a name="monitoring-check"></a><span data-ttu-id="8126e-379">Monitorování kontroly</span><span class="sxs-lookup"><span data-stu-id="8126e-379">Monitoring check</span></span>
<span data-ttu-id="8126e-380">Zkontrolujte, zda monitorování funguje, jak je popsáno v [kontroly a řešení potíží pro nastavení sledování začátku do konce][deployment-guide-troubleshooting-chapter].</span><span class="sxs-lookup"><span data-stu-id="8126e-380">Check whether monitoring is working, as described in [Checks and troubleshooting for setting up end-to-end monitoring][deployment-guide-troubleshooting-chapter].</span></span>




### <span data-ttu-id="8126e-381"><a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Scénář 3: Přesun místní počítač pomocí virtuálního pevného disku není zobecněný Azure SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-381"><a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Scenario 3: Moving an on-premises VM by using a non-generalized Azure VHD with SAP</span></span>
<span data-ttu-id="8126e-382">V tomto scénáři chcete přesunout konkrétního systému SAP do Azure z místního prostředí.</span><span class="sxs-lookup"><span data-stu-id="8126e-382">In this scenario, you plan to move a specific SAP system from an on-premises environment to Azure.</span></span> <span data-ttu-id="8126e-383">To provedete tím, že nahrajete virtuální pevný disk, který má operačního systému, binární soubory SAP a nakonec databázového systému binárních souborů a virtuálních pevných disků se soubory protokolu a data z databázového systému, do Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-383">You can do this by uploading the VHD that has the OS, the SAP binaries, and eventually the DBMS binaries, plus the VHDs with the data and log files of the DBMS, to Azure.</span></span> <span data-ttu-id="8126e-384">Na rozdíl od podle scénáře popsaného v [scénář 2: nasazení virtuálního počítače s vlastní image pro SAP][deployment-guide-3.3], v takovém případě byste mít název hostitele, identifikátor SID SAP, a SAP uživatelských účtů ve virtuálním počítači Azure, protože byly nakonfigurovány v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8126e-384">Unlike the scenario described in [Scenario 2: Deploying a VM with a custom image for SAP][deployment-guide-3.3], in this case, you keep the hostname, SAP SID, and SAP user accounts in the Azure VM, because they were configured in the on-premises environment.</span></span> <span data-ttu-id="8126e-385">Není nutné ke generalizaci operačního systému.</span><span class="sxs-lookup"><span data-stu-id="8126e-385">You do not need to generalize the OS.</span></span> <span data-ttu-id="8126e-386">Tento scénář platí nejčastěji používá pro scénáře mezi různými místy, kde součástí povahu SAP běží místně a jeho součástí běží na Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-386">This scenario applies most often to cross-premises scenarios where part of the SAP landscape runs on-premises and part of it runs on Azure.</span></span>

<span data-ttu-id="8126e-387">V tomto scénáři není automaticky instalován Agent virtuálního počítače během nasazení.</span><span class="sxs-lookup"><span data-stu-id="8126e-387">In this scenario, the VM Agent is not automatically installed during deployment.</span></span> <span data-ttu-id="8126e-388">Protože agenta virtuálního počítače a Azure rozšířené monitorování rozšíření pro SAP požadované pro spuštění SAP, musíte stáhnout, nainstalovat a povolit i komponent ručně po vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8126e-388">Because the VM Agent and the Azure Enhanced Monitoring Extension for SAP required for to run SAP, you need to download, install, and enable both components manually after you create the virtual machine.</span></span>

<span data-ttu-id="8126e-389">Další informace o agenta virtuálního počítače Azure najdete v následujících materiálech.</span><span class="sxs-lookup"><span data-stu-id="8126e-389">For more information about the Azure VM Agent, see the following resources.</span></span>

<span data-ttu-id="8126e-390">[comment]: <> (Níže uvedený odkaz MSSedusch TODO aktualizace systému Windows)</span><span class="sxs-lookup"><span data-stu-id="8126e-390">[comment]: <> (MSSedusch TODO Update Windows Link below)</span></span>

- - -
> ![Windows][Logo_Windows] <span data-ttu-id="8126e-392">Windows</span><span class="sxs-lookup"><span data-stu-id="8126e-392">Windows</span></span>
>
> <span data-ttu-id="8126e-393"><http://blogs.msdn.com/b/wats/archive/2014/02/17/bginfo-Guest-Agent-Extension-for-Azure-VMS.aspx></span><span class="sxs-lookup"><span data-stu-id="8126e-393"><http://blogs.msdn.com/b/wats/archive/2014/02/17/bginfo-guest-agent-extension-for-azure-vms.aspx></span></span>
>
> ![Linux][Logo_Linux] <span data-ttu-id="8126e-395">Linux</span><span class="sxs-lookup"><span data-stu-id="8126e-395">Linux</span></span>
>
> <span data-ttu-id="8126e-396">[Uživatelská příručka k Azure Linux Agent][virtual-machines-linux-agent-user-guide]</span><span class="sxs-lookup"><span data-stu-id="8126e-396">[Azure Linux Agent User Guide][virtual-machines-linux-agent-user-guide]</span></span>
>
>

- - -

<span data-ttu-id="8126e-397">Následující vývojový diagram znázorňuje pořadí kroků pro přesun místní počítač pomocí virtuálního pevného disku není zobecněný Azure:</span><span class="sxs-lookup"><span data-stu-id="8126e-397">The following flowchart shows the sequence of steps for moving an on-premises VM by using a non-generalized Azure VHD:</span></span>

![Vývojový diagram nasazení virtuálního počítače pro SAP systémy pomocí disku virtuálního počítače][deployment-guide-figure-400]

<span data-ttu-id="8126e-399">Za předpokladu, že je disk již nahrán a definované v Azure (viz [virtuální počítače Azure plánování a implementace pro SAP v systému Linux][planning-guide]), provést úkoly popsané v další části několik.</span><span class="sxs-lookup"><span data-stu-id="8126e-399">Assuming that the disk is already uploaded and defined in Azure (see [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]), do the tasks described in the next few sections.</span></span>

#### <a name="create-a-virtual-machine"></a><span data-ttu-id="8126e-400">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8126e-400">Create a virtual machine</span></span>
<span data-ttu-id="8126e-401">K vytvoření nasazení pomocí privátní disk operačního systému, prostřednictvím portálu Azure, použijte šablonu SAP publikované v [úložiště GitHub šablon azure rychlý Start][azure-quickstart-templates-github].</span><span class="sxs-lookup"><span data-stu-id="8126e-401">To create a deployment by using a private OS disk through the Azure portal, use the SAP template published in the [azure-quickstart-templates GitHub repository][azure-quickstart-templates-github].</span></span> <span data-ttu-id="8126e-402">Také můžete ručně vytvoříte virtuální počítač pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8126e-402">You also can manually create a virtual machine, by using PowerShell.</span></span>

* <span data-ttu-id="8126e-403">[**Šablona dvouvrstvá konfigurace (jenom jeden virtuální počítač)** (sap-2vrstvy uživatel disk)][sap-templates-2-tier-os-disk]</span><span class="sxs-lookup"><span data-stu-id="8126e-403">[**Two-tier configuration (only one virtual machine) template** (sap-2-tier-user-disk)][sap-templates-2-tier-os-disk]</span></span>

  <span data-ttu-id="8126e-404">K vytvoření dvouvrstvé systému pomocí jenom jeden virtuální počítač, použijte tuto šablonu.</span><span class="sxs-lookup"><span data-stu-id="8126e-404">To create a two-tier system by using only one virtual machine, use this template.</span></span>

<span data-ttu-id="8126e-405">Na portálu Azure zadejte následující parametry šablony:</span><span class="sxs-lookup"><span data-stu-id="8126e-405">In the Azure portal, enter the following parameters for the template:</span></span>

1. <span data-ttu-id="8126e-406">**Základy**:</span><span class="sxs-lookup"><span data-stu-id="8126e-406">**Basics**:</span></span>
  * <span data-ttu-id="8126e-407">**Předplatné**: předplatné pro použití k nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="8126e-407">**Subscription**: The subscription to use to deploy the template.</span></span>
  * <span data-ttu-id="8126e-408">**Skupina prostředků**: skupiny prostředků můžete použít k nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="8126e-408">**Resource group**: The resource group to use to deploy the template.</span></span> <span data-ttu-id="8126e-409">Můžete vytvořit novou skupinu prostředků nebo vyberte existující skupinu prostředků v předplatném.</span><span class="sxs-lookup"><span data-stu-id="8126e-409">You can create a new resource group or select an existing resource group in the subscription.</span></span>
  * <span data-ttu-id="8126e-410">**Umístění**: kde chcete nasadit šablonu.</span><span class="sxs-lookup"><span data-stu-id="8126e-410">**Location**: Where to deploy the template.</span></span> <span data-ttu-id="8126e-411">Pokud jste vybrali existující skupinu prostředků, použije se umístění této skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="8126e-411">If you selected an existing resource group, the location of that resource group is used.</span></span>
2. <span data-ttu-id="8126e-412">**Nastavení**:</span><span class="sxs-lookup"><span data-stu-id="8126e-412">**Settings**:</span></span>
  * <span data-ttu-id="8126e-413">**ID systému SAP**: ID SAP systému.</span><span class="sxs-lookup"><span data-stu-id="8126e-413">**SAP System ID**: The SAP System ID.</span></span>
  * <span data-ttu-id="8126e-414">**Typ operačního systému**: typ operačního systému, kterou chcete nasadit (Windows nebo Linux).</span><span class="sxs-lookup"><span data-stu-id="8126e-414">**OS type**: The operating system type you want to deploy (Windows or Linux).</span></span>
  * <span data-ttu-id="8126e-415">**Velikost systému SAP**: velikost systému SAP.</span><span class="sxs-lookup"><span data-stu-id="8126e-415">**SAP system size**: The size of the SAP system.</span></span>

    <span data-ttu-id="8126e-416">Počet protokoly SAP poskytuje nový systém.</span><span class="sxs-lookup"><span data-stu-id="8126e-416">The number of SAPS the new system provides.</span></span> <span data-ttu-id="8126e-417">Pokud si nejste jisti kolik protokoly SAP vyžaduje systém, obraťte se na partnera technologie SAP nebo systémový integrátor.</span><span class="sxs-lookup"><span data-stu-id="8126e-417">If you are not sure how many SAPS the system requires, ask your SAP Technology Partner or System Integrator.</span></span>
  * <span data-ttu-id="8126e-418">**Typ úložiště** (pouze šablony dvouvrstvá): typ úložiště používat.</span><span class="sxs-lookup"><span data-stu-id="8126e-418">**Storage type** (two-tier template only): The type of storage to use.</span></span>

    <span data-ttu-id="8126e-419">U větších systémů důrazně doporučujeme pomocí Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="8126e-419">For larger systems, we highly recommend using Azure Premium Storage.</span></span> <span data-ttu-id="8126e-420">Další informace o typech úložiště najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="8126e-420">For more information about storage types, see the following resources:</span></span>
      * <span data-ttu-id="8126e-421">[Používání úložiště Azure Premium SSD pro instanci databázového systému SAP][2367194]</span><span class="sxs-lookup"><span data-stu-id="8126e-421">[Use of Azure Premium SSD Storage for SAP DBMS Instance][2367194]</span></span>
      * <span data-ttu-id="8126e-422">[Microsoft Azure Storage] [ dbms-guide-2.3] v [nasazení virtuálního počítače Azure databázového systému pro SAP v systému Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="8126e-422">[Microsoft Azure Storage][dbms-guide-2.3] in [Azure Virtual Machine DBMS deployment for SAP on Linux][dbms-guide]</span></span>
      * <span data-ttu-id="8126e-423">[Úložiště Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure][storage-premium-storage-preview-portal]</span><span class="sxs-lookup"><span data-stu-id="8126e-423">[Premium Storage: High-performance storage for Azure Virtual Machine workloads][storage-premium-storage-preview-portal]</span></span>
      * <span data-ttu-id="8126e-424">[Úvod do Microsoft Azure Storage][storage-introduction]</span><span class="sxs-lookup"><span data-stu-id="8126e-424">[Introduction to Microsoft Azure Storage][storage-introduction]</span></span>
  * <span data-ttu-id="8126e-425">**Identifikátor URI virtuálního pevného disku na disku operačního systému**: URI privátní disk operačního systému, například https://&lt;accountname >.blob.core.windows.net/vhds/osdisk.vhd.</span><span class="sxs-lookup"><span data-stu-id="8126e-425">**OS disk VHD URI**: The URI of the private OS disk, for example, https://&lt;accountname>.blob.core.windows.net/vhds/osdisk.vhd.</span></span>
  * <span data-ttu-id="8126e-426">**Nový nebo existující podsíť**: Určuje, zda se vytvoří nová virtuální síť a podsíť, nebo se používá existující podsítí.</span><span class="sxs-lookup"><span data-stu-id="8126e-426">**New or existing subnet**: Determines whether a new virtual network and subnet are created, or an existing subnet is used.</span></span> <span data-ttu-id="8126e-427">Pokud již máte virtuální síť, která je připojen k síti na pracovišti, vyberte **existující**.</span><span class="sxs-lookup"><span data-stu-id="8126e-427">If you already have a virtual network that is connected to your on-premises network, select **Existing**.</span></span>
  * <span data-ttu-id="8126e-428">**ID podsítě**: ID podsítě, do které budou virtuální počítače připojit k.</span><span class="sxs-lookup"><span data-stu-id="8126e-428">**Subnet ID**: The ID of the subnet to which the virtual machines will connect to.</span></span> <span data-ttu-id="8126e-429">Vyberte podsíť virtuální sítě VPN nebo Azure ExpressRoute sloužící k připojení virtuálního počítače k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="8126e-429">Select the subnet of your VPN or Azure ExpressRoute virtual network to use to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="8126e-430">ID obvykle vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="8126e-430">The ID usually looks like this:</span></span>

    <span data-ttu-id="8126e-431">/subscriptions/&lt;id předplatného > /resourceGroups/&lt;název skupiny prostředků > /providers/Microsoft.Network/virtualNetworks/&lt;název virtuální sítě > /subnets/&lt;název podsítě ></span><span class="sxs-lookup"><span data-stu-id="8126e-431">/subscriptions/&lt;subscription id>/resourceGroups/&lt;resource group name>/providers/Microsoft.Network/virtualNetworks/&lt;virtual network name>/subnets/&lt;subnet name></span></span>

3. <span data-ttu-id="8126e-432">**Podmínky a ujednání**:</span><span class="sxs-lookup"><span data-stu-id="8126e-432">**Terms and conditions**:</span></span>  
    <span data-ttu-id="8126e-433">Přečtěte si a přijměte právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="8126e-433">Review and accept the legal terms.</span></span>

4.  <span data-ttu-id="8126e-434">Vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="8126e-434">Select **Purchase**.</span></span>

#### <a name="install-the-vm-agent"></a><span data-ttu-id="8126e-435">Nainstalujte agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8126e-435">Install the VM Agent</span></span>
<span data-ttu-id="8126e-436">Použití šablon popsané v předchozí části, musí být Agent virtuálního počítače nainstalovaný na disk operačního systému nebo nasazení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="8126e-436">To use the templates described in the preceding section, the VM Agent must be installed on the OS disk, or the deployment will fail.</span></span> <span data-ttu-id="8126e-437">Stáhněte a nainstalujte agenta virtuálního počítače ve virtuálním počítači, jak je popsáno v [stáhnout, nainstalovat a povolit agenta virtuálního počítače Azure][deployment-guide-4.4].</span><span class="sxs-lookup"><span data-stu-id="8126e-437">Download and install the VM Agent in the VM, as described in [Download, install, and enable the Azure VM Agent][deployment-guide-4.4].</span></span>

<span data-ttu-id="8126e-438">Pokud nepoužíváte šablony popsané v předchozí části, můžete také nainstalovat agenta virtuálního počítače později.</span><span class="sxs-lookup"><span data-stu-id="8126e-438">If you don’t use the templates described in the preceding section, you can also install the VM Agent afterwards.</span></span>

#### <a name="join-a-domain-windows-only"></a><span data-ttu-id="8126e-439">Připojení k doméně (jenom Windows)</span><span class="sxs-lookup"><span data-stu-id="8126e-439">Join a domain (Windows only)</span></span>
<span data-ttu-id="8126e-440">Pokud vaše nasazení Azure je připojen k místní instance služby Active Directory nebo DNS prostřednictvím Azure připojení site-to-site VPN nebo ExpressRoute (to se označuje jako *mezi různými místy* v [virtuální počítače Azure plánování a implementace pro SAP v systému Linux][planning-guide]), očekává se, že virtuální počítač je připojení k místní doméně.</span><span class="sxs-lookup"><span data-stu-id="8126e-440">If your Azure deployment is connected to an on-premises Active Directory or DNS instance via an Azure site-to-site VPN connection or ExpressRoute (this is called *cross-premises* in [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]), it is expected that the VM is joining an on-premises domain.</span></span> <span data-ttu-id="8126e-441">Další informace o požadavcích pro tuto úlohu najdete v tématu [připojit virtuální počítač k doméně místní (jenom Windows)][deployment-guide-4.3].</span><span class="sxs-lookup"><span data-stu-id="8126e-441">For more information about considerations for this task, see [Join a VM to an on-premises domain (Windows only)][deployment-guide-4.3].</span></span>

#### <a name="configure-proxy-settings"></a><span data-ttu-id="8126e-442">Konfigurace nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="8126e-442">Configure proxy settings</span></span>
<span data-ttu-id="8126e-443">V závislosti na konfiguraci místní sítě může být potřeba nastavení serveru proxy na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="8126e-443">Depending on how your on-premises network is configured, you might need to set up the proxy on your VM.</span></span> <span data-ttu-id="8126e-444">Pokud virtuální počítač je připojený k vaší místní sítě prostřednictvím sítě VPN nebo ExpressRoute, virtuální počítač nemusí být možné získat přístup k Internetu a nebude ji již možné stáhnout požadované rozšíření nebo shromažďovat data monitorování.</span><span class="sxs-lookup"><span data-stu-id="8126e-444">If your VM is connected to your on-premises network via VPN or ExpressRoute, the VM might not be able to access the Internet, and won't be able to download the required extensions or collect monitoring data.</span></span> <span data-ttu-id="8126e-445">Další informace najdete v tématu [proxy server nakonfigurovat][deployment-guide-configure-proxy].</span><span class="sxs-lookup"><span data-stu-id="8126e-445">For more information, see [Configure the proxy][deployment-guide-configure-proxy].</span></span>

#### <a name="configure-monitoring"></a><span data-ttu-id="8126e-446">Konfigurace monitorování</span><span class="sxs-lookup"><span data-stu-id="8126e-446">Configure monitoring</span></span>
<span data-ttu-id="8126e-447">Aby byly vaše prostředí podporuje SAP, nastavit rozšíření monitorování Azure pro SAP, jak je popsáno v [konfiguraci Azure rozšířené monitorování rozšíření přístup k SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="8126e-447">To be sure your environment supports SAP, set up the Azure Monitoring Extension for SAP as described in [Configure the Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span> <span data-ttu-id="8126e-448">Kontrola předpokladů pro SAP monitorování a požadovaná minimální verze SAP jádra a Agent hostitele SAP, v prostředky uvedené v [SAP prostředky][deployment-guide-2.2].</span><span class="sxs-lookup"><span data-stu-id="8126e-448">Check the prerequisites for SAP monitoring, and required minimum versions of SAP Kernel and SAP Host Agent, in the resources listed in [SAP resources][deployment-guide-2.2].</span></span>

#### <a name="monitoring-check"></a><span data-ttu-id="8126e-449">Monitorování kontroly</span><span class="sxs-lookup"><span data-stu-id="8126e-449">Monitoring check</span></span>
<span data-ttu-id="8126e-450">Zkontrolujte, zda monitorování funguje, jak je popsáno v [kontroly a řešení potíží pro nastavení sledování začátku do konce][deployment-guide-troubleshooting-chapter].</span><span class="sxs-lookup"><span data-stu-id="8126e-450">Check whether monitoring is working, as described in [Checks and troubleshooting for setting up end-to-end monitoring][deployment-guide-troubleshooting-chapter].</span></span>

## <a name="update-the-monitoring-configuration-for-sap"></a><span data-ttu-id="8126e-451">Aktualizovat konfiguraci monitorování pro SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-451">Update the monitoring configuration for SAP</span></span>
<span data-ttu-id="8126e-452">Aktualizujte konfiguraci monitorování SAP v některém z následujících scénářů:</span><span class="sxs-lookup"><span data-stu-id="8126e-452">Update the SAP monitoring configuration in any of the following scenarios:</span></span>
* <span data-ttu-id="8126e-453">Společné týmu Microsoft/SAP rozšiřuje možnosti monitorování a požadavky čítače více nebo méně.</span><span class="sxs-lookup"><span data-stu-id="8126e-453">The joint Microsoft/SAP team extends the monitoring capabilities and requests more or fewer counters.</span></span>
* <span data-ttu-id="8126e-454">Microsoft zavádí novou verzi základní infrastrukturu Azure, který poskytuje data monitorování a Azure rozšířené monitorování rozšíření pro SAP musí být přizpůsobena tyto změny.</span><span class="sxs-lookup"><span data-stu-id="8126e-454">Microsoft introduces a new version of the underlying Azure infrastructure that delivers the monitoring data, and the Azure Enhanced Monitoring Extension for SAP needs to be adapted to those changes.</span></span>
* <span data-ttu-id="8126e-455">Připojit další virtuální pevné disky na virtuální počítač Azure nebo odeberte virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="8126e-455">You mount additional VHDs to your Azure VM or you remove a VHD.</span></span> <span data-ttu-id="8126e-456">V tomto scénáři aktualizujte shromažďování dat souvisejících s úložištěm.</span><span class="sxs-lookup"><span data-stu-id="8126e-456">In this scenario, update the collection of storage-related data.</span></span> <span data-ttu-id="8126e-457">Změna konfigurací přidáním nebo odstraněním koncových bodů nebo přiřazení IP adres k virtuálnímu počítači nemá vliv na konfiguraci monitorování.</span><span class="sxs-lookup"><span data-stu-id="8126e-457">Changing your configuration by adding or deleting endpoints or by assigning IP addresses to a VM does not affect the monitoring configuration.</span></span>
* <span data-ttu-id="8126e-458">Změníte velikost virtuálního počítače Azure, například velikost A5 tak, aby ostatní velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8126e-458">You change the size of your Azure VM, for example, from size A5 to any other VM size.</span></span>
* <span data-ttu-id="8126e-459">K virtuálnímu počítači Azure přidáte nové síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8126e-459">You add new network interfaces to your Azure VM.</span></span>

<span data-ttu-id="8126e-460">Aktualizovat nastavení monitorování, aktualizace infrastruktury pro monitorování pomocí následujících kroků v [konfiguraci Azure rozšířené monitorování rozšíření přístup k SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="8126e-460">To update monitoring settings, update the monitoring infrastructure by following the steps in [Configure the Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span>

## <a name="detailed-tasks-for-sap-software-deployment-on-a-windows-vm"></a><span data-ttu-id="8126e-461">Podrobný popis úkolů pro nasazení softwaru SAP na virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="8126e-461">Detailed tasks for SAP software deployment on a Windows VM</span></span>
<span data-ttu-id="8126e-462">Tato část popsala postup při provádění konkrétní úlohy v procesu konfigurace a nasazení.</span><span class="sxs-lookup"><span data-stu-id="8126e-462">This section has detailed steps for doing specific tasks in the configuration and deployment process.</span></span>

### <span data-ttu-id="8126e-463"><a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Nasazení rutin prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8126e-463"><a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Deploy Azure PowerShell cmdlets</span></span>
1.  <span data-ttu-id="8126e-464">Přejděte na [Microsoft Azure stáhne](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="8126e-464">Go to [Microsoft Azure Downloads](https://azure.microsoft.com/downloads/).</span></span>
2.  <span data-ttu-id="8126e-465">V části **nástroje příkazového řádku**v části **prostředí PowerShell**, vyberte **nainstalovat Windows**.</span><span class="sxs-lookup"><span data-stu-id="8126e-465">Under **Command-line tools**, under **PowerShell**, select **Windows install**.</span></span>
3.  <span data-ttu-id="8126e-466">V dialogovém okně Microsoft Download Manager pro stažený soubor (například WindowsAzurePowershellGet.3f.3f.3fnew.exe), vyberte **spustit**.</span><span class="sxs-lookup"><span data-stu-id="8126e-466">In the Microsoft Download Manager dialog box, for the downloaded file (for example, WindowsAzurePowershellGet.3f.3f.3fnew.exe), select **Run**.</span></span>
4.  <span data-ttu-id="8126e-467">Ke spuštění instalačního programu webové platformy (Microsoft webové platformy), vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="8126e-467">To run Microsoft Web Platform Installer (Microsoft Web PI), select **Yes**.</span></span>
5.  <span data-ttu-id="8126e-468">Stránka, která vypadá to, se zobrazuje:</span><span class="sxs-lookup"><span data-stu-id="8126e-468">A page that looks like this appears:</span></span>

  <span data-ttu-id="8126e-469">![Stránka Instalace rutin prostředí Azure PowerShell][deployment-guide-figure-500]<a name="figure-5"></a></span><span class="sxs-lookup"><span data-stu-id="8126e-469">![Installation page for Azure PowerShell cmdlets][deployment-guide-figure-500]<a name="figure-5"></a></span></span>

6.  <span data-ttu-id="8126e-470">Vyberte **nainstalovat**a pak přijměte licenční podmínky pro Software společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8126e-470">Select **Install**, and then accept the Microsoft Software License Terms.</span></span>
7.  <span data-ttu-id="8126e-471">Prostředí PowerShell je nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="8126e-471">PowerShell is installed.</span></span> <span data-ttu-id="8126e-472">Vyberte **Dokončit** zavřete průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="8126e-472">Select **Finish** to close the installation wizard.</span></span>

<span data-ttu-id="8126e-473">Často vyhledat aktualizace do rutin prostředí PowerShell, které obvykle jsou aktualizovány jednou měsíčně.</span><span class="sxs-lookup"><span data-stu-id="8126e-473">Check frequently for updates to the PowerShell cmdlets, which usually are updated monthly.</span></span> <span data-ttu-id="8126e-474">Nejjednodušší způsob, jak zkontrolovat aktualizace je postup předchozí instalace, až na stránce instalace zobrazí v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="8126e-474">The easiest way to check for updates is to do the preceding installation steps, up to the installation page shown in step 5.</span></span> <span data-ttu-id="8126e-475">Číslo verze datum a verze rutin jsou zahrnuty v stránka zobrazená v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="8126e-475">The release date and release number of the cmdlets are included on the page shown in step 5.</span></span> <span data-ttu-id="8126e-476">Pokud není uvedeno jinak v Poznámka SAP [1928533] nebo Poznámka SAP [2015553], doporučujeme vám, že pracujete s nejnovější verzi rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8126e-476">Unless stated otherwise in SAP Note [1928533] or SAP Note [2015553], we recommend that you work with the latest version of Azure PowerShell cmdlets.</span></span>

<span data-ttu-id="8126e-477">Pokud chcete zkontrolovat verzi rutin Azure Powershellu, které jsou nainstalovány v počítači, spusťte tento příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="8126e-477">To check the version of the Azure PowerShell cmdlets that are installed on your computer, run this PowerShell command:</span></span>
```powershell
Import-Module Azure
(Get-Module Azure).Version
```
<span data-ttu-id="8126e-478">Výsledek vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="8126e-478">The result looks like this:</span></span>

<span data-ttu-id="8126e-479">![Výsledek kontroly verze rutiny prostředí Azure PowerShell][deployment-guide-figure-600]
<a name="figure-6"></a></span><span class="sxs-lookup"><span data-stu-id="8126e-479">![Result of Azure PowerShell cmdlet version check][deployment-guide-figure-600]
<a name="figure-6"></a></span></span>

<span data-ttu-id="8126e-480">Pokud je verze Azure rutiny v počítači nainstalována aktuální verze, první stránce Průvodce instalací znamenají, že přidáním **(nainstalována)** pro název produktu (viz následující snímek obrazovky).</span><span class="sxs-lookup"><span data-stu-id="8126e-480">If the Azure cmdlet version installed on your computer is the current version, the first page of the installation wizard indicates it by adding **(Installed)** to the product title (see the following screenshot).</span></span> <span data-ttu-id="8126e-481">Vaše rutiny prostředí PowerShell Azure jsou aktuální.</span><span class="sxs-lookup"><span data-stu-id="8126e-481">Your PowerShell Azure cmdlets are up-to-date.</span></span> <span data-ttu-id="8126e-482">Zavřete průvodce instalací, vyberte **ukončení**.</span><span class="sxs-lookup"><span data-stu-id="8126e-482">To close the installation wizard, select **Exit**.</span></span>

<span data-ttu-id="8126e-483">![Instalační stránku rutin prostředí Azure PowerShell, která udává, zda jsou nainstalovány nejnovější verzi rutin prostředí Azure PowerShell][deployment-guide-figure-700]
<a name="figure-7"></a></span><span class="sxs-lookup"><span data-stu-id="8126e-483">![Installation page for Azure PowerShell cmdlets indicating that the most recent version of Azure PowerShell cmdlets are installed][deployment-guide-figure-700]
<a name="figure-7"></a></span></span>

### <span data-ttu-id="8126e-484"><a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Nasazení rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="8126e-484"><a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Deploy Azure CLI</span></span>
1.  <span data-ttu-id="8126e-485">Přejděte na [Microsoft Azure stáhne](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="8126e-485">Go to [Microsoft Azure Downloads](https://azure.microsoft.com/downloads/).</span></span>
2.  <span data-ttu-id="8126e-486">V části **nástroje příkazového řádku**v části **rozhraní příkazového řádku Azure**, vyberte **nainstalovat** odkaz pro váš operační systém.</span><span class="sxs-lookup"><span data-stu-id="8126e-486">Under **Command-line tools**, under **Azure command-line interface**, select the **Install** link for your operating system.</span></span>
3.  <span data-ttu-id="8126e-487">V dialogovém okně Microsoft Download Manager pro stažený soubor (například WindowsAzureXPlatCLI.3f.3f.3fnew.exe), vyberte **spustit**.</span><span class="sxs-lookup"><span data-stu-id="8126e-487">In the Microsoft Download Manager dialog box, for the downloaded file (for example, WindowsAzureXPlatCLI.3f.3f.3fnew.exe), select **Run**.</span></span>
4.  <span data-ttu-id="8126e-488">Ke spuštění instalačního programu webové platformy (Microsoft webové platformy), vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="8126e-488">To run Microsoft Web Platform Installer (Microsoft Web PI), select **Yes**.</span></span>
5.  <span data-ttu-id="8126e-489">Stránka, která vypadá to, se zobrazuje:</span><span class="sxs-lookup"><span data-stu-id="8126e-489">A page that looks like this appears:</span></span>

  <span data-ttu-id="8126e-490">![Stránka Instalace rutin prostředí Azure PowerShell][deployment-guide-figure-500]<a name="figure-5"></a></span><span class="sxs-lookup"><span data-stu-id="8126e-490">![Installation page for Azure PowerShell cmdlets][deployment-guide-figure-500]<a name="figure-5"></a></span></span>

6.  <span data-ttu-id="8126e-491">Vyberte **nainstalovat**a pak přijměte licenční podmínky pro Software společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8126e-491">Select **Install**, and then accept the Microsoft Software License Terms.</span></span>
7.  <span data-ttu-id="8126e-492">Rozhraní příkazového řádku Azure je nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="8126e-492">Azure CLI is installed.</span></span> <span data-ttu-id="8126e-493">Vyberte **Dokončit** zavřete průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="8126e-493">Select **Finish** to close the installation wizard.</span></span>

<span data-ttu-id="8126e-494">Často vyhledat aktualizace pro rozhraní příkazového řádku Azure, která obvykle se aktualizuje jednou měsíčně.</span><span class="sxs-lookup"><span data-stu-id="8126e-494">Check frequently for updates to Azure CLI, which usually is updated monthly.</span></span> <span data-ttu-id="8126e-495">Nejjednodušší způsob, jak zkontrolovat aktualizace je postup předchozí instalace, až na stránce instalace zobrazí v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="8126e-495">The easiest way to check for updates is to do the preceding installation steps, up to the installation page shown in step 5.</span></span>


<span data-ttu-id="8126e-496">Pokud chcete zkontrolovat verzi rozhraní příkazového řádku Azure, který je nainstalován v počítači, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="8126e-496">To check the version of Azure CLI that is installed on your computer, run this command:</span></span>
```
azure --version
```

<span data-ttu-id="8126e-497">Výsledek vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="8126e-497">The result looks like this:</span></span>

<span data-ttu-id="8126e-498">![Výsledek kontroly verze rozhraní příkazového řádku Azure][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a></span><span class="sxs-lookup"><span data-stu-id="8126e-498">![Result of Azure CLI version check][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a></span></span>

### <span data-ttu-id="8126e-499"><a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Připojit virtuální počítač k doméně místní (jenom Windows)</span><span class="sxs-lookup"><span data-stu-id="8126e-499"><a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Join a VM to an on-premises domain (Windows only)</span></span>
<span data-ttu-id="8126e-500">Pokud nasadíte virtuální počítače SAP ve scénáři mezi různými místy, kde místní služby Active Directory a DNS jsou rozšířené v Azure, očekává se, že virtuální počítače jsou připojení k místní doméně.</span><span class="sxs-lookup"><span data-stu-id="8126e-500">If you deploy SAP VMs in a cross-premises scenario, where on-premises Active Directory and DNS are extended in Azure, it is expected that the VMs are joining an on-premises domain.</span></span> <span data-ttu-id="8126e-501">Podrobné kroky, které je třeba provést připojení k místní doméně a další software, musí být členem domény služby místní virtuální počítač se liší podle zákazníka.</span><span class="sxs-lookup"><span data-stu-id="8126e-501">The detailed steps you take to join a VM to an on-premises domain, and the additional software required to be a member of an on-premises domain, varies by customer.</span></span> <span data-ttu-id="8126e-502">Obvykle Pokud chcete virtuální počítač připojit k místní doméně, musíte nainstalovat další software, jako je antimalwarový software a zálohování nebo monitorování softwaru.</span><span class="sxs-lookup"><span data-stu-id="8126e-502">Usually, to join a VM to an on-premises domain, you need to install additional software, like antimalware software, and backup or monitoring software.</span></span>

<span data-ttu-id="8126e-503">V tomto scénáři musíte také ujistěte se, že pokud nastavení internetového proxy serveru jsou povinná, pokud virtuální počítač připojen do domény ve vašem prostředí, Windows místní systémový účet (S-1-5-18) ve virtuálním počítači hosta má stejné nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="8126e-503">In this scenario, you also need to make sure that if Internet proxy settings are forced when a VM joins a domain in your environment, the Windows Local System Account (S-1-5-18) in the Guest VM has the same proxy settings.</span></span> <span data-ttu-id="8126e-504">Je nejjednodušší možnost vynutit proxy server pomocí zásad skupiny, které se vztahuje na systémy v doméně domény.</span><span class="sxs-lookup"><span data-stu-id="8126e-504">The easiest option is to force the proxy by using a domain Group Policy, which applies to systems in the domain.</span></span>

### <span data-ttu-id="8126e-505"><a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Stáhnout, nainstalovat a povolit agenta virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="8126e-505"><a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Download, install, and enable the Azure VM Agent</span></span>
<span data-ttu-id="8126e-506">Pro virtuální počítače, které jsou nasazeny z image operačního systému, který není zobecněný (například obrázek, který není v nástroji pro přípravu systému Windows nebo nástroje sysprep, pocházejí) musíte ručně stáhnout, nainstalovat a povolit agenta virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-506">For virtual machines that are deployed from an OS image that is not generalized (for example, an image that doesn't originate in the Windows System Preparation, or sysprep, tool), you need to manually download, install, and enable the Azure VM Agent.</span></span>

<span data-ttu-id="8126e-507">Pokud nasadíte virtuální počítač z Azure Marketplace, tento krok není povinný.</span><span class="sxs-lookup"><span data-stu-id="8126e-507">If you deploy a VM from the Azure Marketplace, this step is not required.</span></span> <span data-ttu-id="8126e-508">Bitové kopie z Azure Marketplace už mít agenta virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-508">Images from the Azure Marketplace already have the Azure VM Agent.</span></span>

#### <span data-ttu-id="8126e-509"><a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows</span><span class="sxs-lookup"><span data-stu-id="8126e-509"><a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows</span></span>
1.  <span data-ttu-id="8126e-510">Stáhněte agenta virtuálního počítače Azure:</span><span class="sxs-lookup"><span data-stu-id="8126e-510">Download the Azure VM Agent:</span></span>
  1.  <span data-ttu-id="8126e-511">Stažení [instalační balíček agenta virtuálního počítače Azure](https://go.microsoft.com/fwlink/?LinkId=394789).</span><span class="sxs-lookup"><span data-stu-id="8126e-511">Download the [Azure VM Agent installer package](https://go.microsoft.com/fwlink/?LinkId=394789).</span></span>
  2.  <span data-ttu-id="8126e-512">Balíček MSI agenta virtuálního počítače ukládat místně na osobním počítači nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="8126e-512">Store the VM Agent MSI package locally on a personal computer or server.</span></span>
2.  <span data-ttu-id="8126e-513">Nainstalujte agenta virtuálního počítače Azure:</span><span class="sxs-lookup"><span data-stu-id="8126e-513">Install the Azure VM Agent:</span></span>
  1.  <span data-ttu-id="8126e-514">Připojení k nasazené virtuálního počítače Azure pomocí protokolu RDP (Remote Desktop).</span><span class="sxs-lookup"><span data-stu-id="8126e-514">Connect to the deployed Azure VM by using Remote Desktop Protocol (RDP).</span></span>
  2.  <span data-ttu-id="8126e-515">Otevřete okno Průzkumníka Windows na virtuální počítač a vyberte cílový adresář pro soubor MSI agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8126e-515">Open a Windows Explorer window on the VM and select the target directory for the MSI file of the VM Agent.</span></span>
  3.  <span data-ttu-id="8126e-516">Přetáhněte soubor MSI Instalační program agenta virtuálního počítače Azure ze svého místního počítače nebo serveru na cílový adresář agenta virtuálního počítače na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="8126e-516">Drag the Azure VM Agent Installer MSI file from your local computer/server to the target directory of the VM Agent on the VM.</span></span>
  4.  <span data-ttu-id="8126e-517">Poklikejte na soubor MSI ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="8126e-517">Double-click the MSI file on the VM.</span></span>
3.  <span data-ttu-id="8126e-518">Pro virtuální počítače, které jsou připojeny k místní domény, ujistěte se, že případný nastavení proxy Internetu platí také pro účet místního systému Windows (S-1-5-18) ve virtuálním počítači, jak je popsáno v [proxy server nakonfigurovat][deployment-guide-configure-proxy].</span><span class="sxs-lookup"><span data-stu-id="8126e-518">For VMs that are joined to on-premises domains, make sure that eventual Internet proxy settings also apply to the Windows Local System account (S-1-5-18) in the VM, as described in [Configure the proxy][deployment-guide-configure-proxy].</span></span> <span data-ttu-id="8126e-519">Agent virtuálního počítače běží v tomto kontextu a musí být schopný se připojit k Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-519">The VM Agent runs in this context and needs to be able to connect to Azure.</span></span>

<span data-ttu-id="8126e-520">Žádná interakce s uživatelem, je potřeba aktualizovat agenta virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-520">No user interaction is required to update the Azure VM Agent.</span></span> <span data-ttu-id="8126e-521">Agent virtuálního počítače se automaticky aktualizuje a nevyžaduje restartování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8126e-521">The VM Agent is automatically updated, and does not require a VM restart.</span></span>

#### <span data-ttu-id="8126e-522"><a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux</span><span class="sxs-lookup"><span data-stu-id="8126e-522"><a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux</span></span>
<span data-ttu-id="8126e-523">Pomocí následujících příkazů nainstalujte VM agenta pro Linux:</span><span class="sxs-lookup"><span data-stu-id="8126e-523">Use the following commands to install the VM Agent for Linux:</span></span>

* <span data-ttu-id="8126e-524">**SUSE Linux Enterprise Server (SLES)**</span><span class="sxs-lookup"><span data-stu-id="8126e-524">**SUSE Linux Enterprise Server (SLES)**</span></span>

  ```
  sudo zypper install WALinuxAgent
  ```

* <span data-ttu-id="8126e-525">**Red Hat Enterprise Linux (RHEL)**</span><span class="sxs-lookup"><span data-stu-id="8126e-525">**Red Hat Enterprise Linux (RHEL)**</span></span>

  ```
  sudo yum install WALinuxAgent
  ```

<span data-ttu-id="8126e-526">Pokud agent je již nainstalován, aktualizace Azure Linux Agent, postupujte podle pokynů popsaných v [aktualizovat Azure Linux Agent na virtuálním počítači na nejnovější verzi z webu GitHub][virtual-machines-linux-update-agent].</span><span class="sxs-lookup"><span data-stu-id="8126e-526">If the agent is already installed, to update the Azure Linux Agent, do the steps described in [Update the Azure Linux Agent on a VM to the latest version from GitHub][virtual-machines-linux-update-agent].</span></span>

### <span data-ttu-id="8126e-527"><a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Konfigurace proxy serveru</span><span class="sxs-lookup"><span data-stu-id="8126e-527"><a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Configure the proxy</span></span>
<span data-ttu-id="8126e-528">Kroky, které je třeba provést konfiguraci proxy serveru v systému Windows se liší od způsob konfigurace proxy serveru v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="8126e-528">The steps you take to configure the proxy in Windows are different from the way you configure the proxy in Linux.</span></span>

#### <a name="windows"></a><span data-ttu-id="8126e-529">Windows</span><span class="sxs-lookup"><span data-stu-id="8126e-529">Windows</span></span>
<span data-ttu-id="8126e-530">Nastavení proxy serveru musí být nastavit správně pro místní systémový účet pro přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="8126e-530">Proxy settings must be set up correctly for the Local System account to access the Internet.</span></span> <span data-ttu-id="8126e-531">Pokud vaše nastavení proxy serveru nejsou nastavené zásady skupiny, můžete nakonfigurovat nastavení pro účet místního systému.</span><span class="sxs-lookup"><span data-stu-id="8126e-531">If your proxy settings are not set by Group Policy, you can configure the settings for the Local System account.</span></span>

1. <span data-ttu-id="8126e-532">Přejděte na **spustit**, zadejte **gpedit.msc**a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="8126e-532">Go to **Start**, enter **gpedit.msc**, and then select **Enter**.</span></span>
2. <span data-ttu-id="8126e-533">Vyberte **konfigurace počítače** > **šablony pro správu** > **součásti systému Windows** > **aplikace Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="8126e-533">Select **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Internet Explorer**.</span></span> <span data-ttu-id="8126e-534">Ujistěte se, že nastavení **zkontrolujte proxy nastavení podle počítače (nikoli na uživatele)** je zakázána nebo není nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="8126e-534">Make sure that the setting **Make proxy settings per-machine (rather than per-user)** is disabled or not configured.</span></span>
3. <span data-ttu-id="8126e-535">V **ovládací panely**, přejděte na **Centrum sítí a sdílení** > **Možnosti Internetu**.</span><span class="sxs-lookup"><span data-stu-id="8126e-535">In **Control Panel**, go to **Network and Sharing Center** > **Internet Options**.</span></span>
4. <span data-ttu-id="8126e-536">Na **připojení** vyberte **nastavení místní sítě** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8126e-536">On the **Connections** tab, select the **LAN settings** button.</span></span>
5. <span data-ttu-id="8126e-537">Vymazat **automaticky zjišťovat nastavení** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="8126e-537">Clear the **Automatically detect settings** check box.</span></span>
6. <span data-ttu-id="8126e-538">Vyberte **použít proxy server pro síť LAN** zaškrtněte políčko a potom zadejte adresu proxy serveru a port.</span><span class="sxs-lookup"><span data-stu-id="8126e-538">Select the **Use a proxy server for your LAN** check box, and then enter the proxy address and port.</span></span>
7. <span data-ttu-id="8126e-539">Vyberte **Upřesnit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8126e-539">Select the **Advanced** button.</span></span>
8. <span data-ttu-id="8126e-540">V **výjimky** zadejte IP adresu **168.63.129.16**.</span><span class="sxs-lookup"><span data-stu-id="8126e-540">In the **Exceptions** box, enter the IP address **168.63.129.16**.</span></span> <span data-ttu-id="8126e-541">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="8126e-541">Select **OK**.</span></span>


#### <a name="linux"></a><span data-ttu-id="8126e-542">Linux</span><span class="sxs-lookup"><span data-stu-id="8126e-542">Linux</span></span>
<span data-ttu-id="8126e-543">Správné proxy server nakonfigurovat v souboru konfigurace agenta hosta Microsoft Azure, která se nachází v \\atd\\waagent.conf.</span><span class="sxs-lookup"><span data-stu-id="8126e-543">Configure the correct proxy in the configuration file of the Microsoft Azure Guest Agent, which is located at \\etc\\waagent.conf.</span></span>

<span data-ttu-id="8126e-544">Nastavte následující parametry:</span><span class="sxs-lookup"><span data-stu-id="8126e-544">Set the following parameters:</span></span>

1.  <span data-ttu-id="8126e-545">**HTTP proxy hostitele**.</span><span class="sxs-lookup"><span data-stu-id="8126e-545">**HTTP proxy host**.</span></span> <span data-ttu-id="8126e-546">Například nastavte ji na **proxy.corp.local**.</span><span class="sxs-lookup"><span data-stu-id="8126e-546">For example, set it to **proxy.corp.local**.</span></span>
  ```
  HttpProxy.Host=<proxy host>

  ```
2.  <span data-ttu-id="8126e-547">**Port proxy serveru HTTP**.</span><span class="sxs-lookup"><span data-stu-id="8126e-547">**HTTP proxy port**.</span></span> <span data-ttu-id="8126e-548">Například nastavte ji na **80**.</span><span class="sxs-lookup"><span data-stu-id="8126e-548">For example, set it to **80**.</span></span>
  ```
  HttpProxy.Port=<port of the proxy host>

  ```
3.  <span data-ttu-id="8126e-549">Restartujte agenta.</span><span class="sxs-lookup"><span data-stu-id="8126e-549">Restart the agent.</span></span>

  ```
  sudo service waagent restart
  ```

<span data-ttu-id="8126e-550">Nastavení proxy serveru v \\atd\\waagent.conf platí také pro požadované rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8126e-550">The proxy settings in \\etc\\waagent.conf also apply to the required VM extensions.</span></span> <span data-ttu-id="8126e-551">Pokud chcete používat úložiště Azure, ujistěte se, že přenosy na těchto úložiště není projít místního intranetu.</span><span class="sxs-lookup"><span data-stu-id="8126e-551">If you want to use the Azure repositories, make sure that the traffic to these repositories is not going through your on-premises intranet.</span></span> <span data-ttu-id="8126e-552">Pokud jste vytvořili trasy definované uživatelem, chcete-li povolit vynucené tunelování, ujistěte se, že přidáte trasu který směruje provoz do úložiště přímo k Internetu a ne prostřednictvím připojení VPN typu site-to-site.</span><span class="sxs-lookup"><span data-stu-id="8126e-552">If you created user-defined routes to enable forced tunneling, make sure that you add a route that routes traffic to the repositories directly to the Internet, and not through your site-to-site VPN connection.</span></span>

* <span data-ttu-id="8126e-553">**SLES**</span><span class="sxs-lookup"><span data-stu-id="8126e-553">**SLES**</span></span>

  <span data-ttu-id="8126e-554">Musíte taky přidat trasy pro IP adresy uvedené v \\atd\\regionserverclnt.cfg.</span><span class="sxs-lookup"><span data-stu-id="8126e-554">You also need to add routes for the IP addresses listed in \\etc\\regionserverclnt.cfg.</span></span> <span data-ttu-id="8126e-555">Následující obrázek znázorňuje příklad:</span><span class="sxs-lookup"><span data-stu-id="8126e-555">The following figure shows an example:</span></span>

  ![Vynucené tunelování][deployment-guide-figure-50]


* <span data-ttu-id="8126e-557">**RHEL**</span><span class="sxs-lookup"><span data-stu-id="8126e-557">**RHEL**</span></span>

  <span data-ttu-id="8126e-558">Musíte taky přidat trasy pro IP adresy hostitelů uvedených v \\atd\\yum.repos.d\\rhui Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="8126e-558">You also need to add routes for the IP addresses of the hosts listed in \\etc\\yum.repos.d\\rhui-load-balancers.</span></span> <span data-ttu-id="8126e-559">Příklad podívejte se na předchozí obrázek.</span><span class="sxs-lookup"><span data-stu-id="8126e-559">For an example, see the preceding figure.</span></span>

<span data-ttu-id="8126e-560">Další informace o trasy definované uživatelem, najdete v části [trasy definované uživatelem a předávání IP][virtual-networks-udr-overview].</span><span class="sxs-lookup"><span data-stu-id="8126e-560">For more information about user-defined routes, see [User-defined routes and IP forwarding][virtual-networks-udr-overview].</span></span>

### <span data-ttu-id="8126e-561"><a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Konfigurace rozšíření Azure rozšířené monitorování pro SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-561"><a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Configure the Azure Enhanced Monitoring Extension for SAP</span></span>
<span data-ttu-id="8126e-562">Když jste virtuální počítač připravený, jak je popsáno v [scénáře nasazení virtuálních počítačů pro SAP v Azure][deployment-guide-3], Agent virtuálního počítače Azure je nainstalovaný na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="8126e-562">When you've prepared the VM as described in [Deployment scenarios of VMs for SAP on Azure][deployment-guide-3], the Azure VM Agent is installed on the virtual machine.</span></span> <span data-ttu-id="8126e-563">Dalším krokem je pro nasazení Azure rozšířené monitorování rozšíření pro SAP, která je k dispozici v úložišti rozšíření Azure v globálních datových centrech Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-563">The next step is to deploy the Azure Enhanced Monitoring Extension for SAP, which is available in the Azure Extension Repository in the global Azure datacenters.</span></span> <span data-ttu-id="8126e-564">Další informace najdete v tématu [virtuální počítače Azure plánování a implementace pro SAP v systému Linux][planning-guide-9.1].</span><span class="sxs-lookup"><span data-stu-id="8126e-564">For more information, see [Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide-9.1].</span></span>

<span data-ttu-id="8126e-565">Prostředí PowerShell nebo rozhraní příkazového řádku Azure můžete použít k instalaci a konfiguraci Azure rozšířené monitorování rozšíření pro SAP.</span><span class="sxs-lookup"><span data-stu-id="8126e-565">You can use PowerShell or Azure CLI to install and configure the Azure Enhanced Monitoring Extension for SAP.</span></span> <span data-ttu-id="8126e-566">Instalace rozšíření v systému Windows nebo virtuálního počítače s Linuxem pomocí počítače s Windows, najdete v části [prostředí Azure PowerShell][deployment-guide-4.5.1].</span><span class="sxs-lookup"><span data-stu-id="8126e-566">To install the extension on a Windows or Linux VM by using a Windows machine, see [Azure PowerShell][deployment-guide-4.5.1].</span></span> <span data-ttu-id="8126e-567">Pokud chcete nainstalovat rozšíření virtuálního počítače s Linuxem pomocí plochy Linux, najdete v části [rozhraní příkazového řádku Azure][deployment-guide-4.5.2].</span><span class="sxs-lookup"><span data-stu-id="8126e-567">To install the extension on a Linux VM by using a Linux desktop, see [Azure CLI][deployment-guide-4.5.2].</span></span>

#### <span data-ttu-id="8126e-568"><a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Prostředí Azure PowerShell pro systémy Linux a virtuálních počítačů Windows</span><span class="sxs-lookup"><span data-stu-id="8126e-568"><a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Azure PowerShell for Linux and Windows VMs</span></span>
<span data-ttu-id="8126e-569">Chcete-li nainstalovat Azure rozšířené monitorování rozšíření pro SAP pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="8126e-569">To install the Azure Enhanced Monitoring Extension for SAP by using PowerShell:</span></span>

1. <span data-ttu-id="8126e-570">Ujistěte se, že instalaci nejnovější verze rutiny Azure Powershellu.</span><span class="sxs-lookup"><span data-stu-id="8126e-570">Make sure that you have installed the latest version of the Azure PowerShell cmdlet.</span></span> <span data-ttu-id="8126e-571">Další informace najdete v tématu [rutin nasazení prostředí Azure PowerShell][deployment-guide-4.1].</span><span class="sxs-lookup"><span data-stu-id="8126e-571">For more information, see [Deploying Azure PowerShell cmdlets][deployment-guide-4.1].</span></span>  
2. <span data-ttu-id="8126e-572">Spuštěním následující rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8126e-572">Run the following PowerShell cmdlet.</span></span>
  <span data-ttu-id="8126e-573">Seznam dostupných prostředí, spusťte `commandlet Get-AzureRmEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="8126e-573">For a list of available environments, run `commandlet Get-AzureRmEnvironment`.</span></span> <span data-ttu-id="8126e-574">Pokud chcete používat veřejná Azure, je prostředí **AzureCloud**.</span><span class="sxs-lookup"><span data-stu-id="8126e-574">If you want to use public Azure, your environment is **AzureCloud**.</span></span> <span data-ttu-id="8126e-575">Azure v Číně, vyberte **AzureChinaCloud**.</span><span class="sxs-lookup"><span data-stu-id="8126e-575">For Azure in China, select **AzureChinaCloud**.</span></span>


      ```powershell
      $env = Get-AzureRmEnvironment -Name <name of the environment>
      Login-AzureRmAccount -Environment $env
      Set-AzureRmContext -SubscriptionName <subscription name>

      Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
      ```

<span data-ttu-id="8126e-576">Po zadání vaše data na účtu a identifikovat virtuální počítač Azure, skript nasadí požadované rozšíření a umožňuje požadované funkce.</span><span class="sxs-lookup"><span data-stu-id="8126e-576">After you enter your account data and identify the Azure virtual machine, the script deploys the required extensions and enables the required features.</span></span> <span data-ttu-id="8126e-577">To může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="8126e-577">This can take several minutes.</span></span>
<span data-ttu-id="8126e-578">Další informace o `Set-AzureRmVMAEMExtension`, najdete v části [Set-AzureRmVMAEMExtension][msdn-set-azurermvmaemextension].</span><span class="sxs-lookup"><span data-stu-id="8126e-578">For more information about `Set-AzureRmVMAEMExtension`, see [Set-AzureRmVMAEMExtension][msdn-set-azurermvmaemextension].</span></span>

![Úspěšné provedení specifické pro SAP Azure rutiny Set-AzureRmVMAEMExtension][deployment-guide-figure-900]

<span data-ttu-id="8126e-580">`Set-AzureRmVMAEMExtension` Konfigurace nemá všechny kroky konfigurace hostitele monitorování pro SAP.</span><span class="sxs-lookup"><span data-stu-id="8126e-580">The `Set-AzureRmVMAEMExtension` configuration does all the steps to configure host monitoring for SAP.</span></span>

<span data-ttu-id="8126e-581">Výstup skriptu obsahuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="8126e-581">The script output includes the following information:</span></span>

* <span data-ttu-id="8126e-582">Potvrzení, že monitorování pro základní virtuální pevný disk (s operačním systémem) a všechny další virtuální pevné disky připojené k virtuálnímu počítači je nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="8126e-582">Confirmation that monitoring for the base VHD (with the OS) and all additional VHDs mounted to the VM has been configured.</span></span>
* <span data-ttu-id="8126e-583">Další dvě zprávy potvrďte konfiguraci úložiště metriky pro účet konkrétní úložiště.</span><span class="sxs-lookup"><span data-stu-id="8126e-583">The next two messages confirm the configuration of Storage Metrics for a specific storage account.</span></span>
* <span data-ttu-id="8126e-584">Jeden řádek výstupu poskytuje stav skutečné aktualizace konfiguraci monitorování.</span><span class="sxs-lookup"><span data-stu-id="8126e-584">One line of output gives the status of the actual update of the monitoring configuration.</span></span>
* <span data-ttu-id="8126e-585">Další řádek výstupu potvrdí, že byla konfigurace nasazení nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="8126e-585">Another line of output confirms that the configuration has been deployed or updated.</span></span>
* <span data-ttu-id="8126e-586">Poslední řádek výstupu je informační.</span><span class="sxs-lookup"><span data-stu-id="8126e-586">The last line of output is informational.</span></span> <span data-ttu-id="8126e-587">Zobrazuje možnosti testování konfiguraci monitorování.</span><span class="sxs-lookup"><span data-stu-id="8126e-587">It shows your options for testing the monitoring configuration.</span></span>

<span data-ttu-id="8126e-588">Pokud chcete zkontrolovat úspěšně byly provedeny všechny kroky rozšířené monitorování Azure a že infrastruktury Azure poskytuje potřebná data, pokračujte kontrolu připravenosti Azure rozšířené monitorování rozšíření pro SAP, jak je popsáno v [kontrolu připravenosti pro Azure rozšířené monitorování pro SAP][deployment-guide-5.1].</span><span class="sxs-lookup"><span data-stu-id="8126e-588">To check that all steps of Azure Enhanced Monitoring have been executed successfully, and that the Azure Infrastructure provides the necessary data, proceed with the readiness check for the Azure Enhanced Monitoring Extension for SAP, as described in [Readiness check for Azure Enhanced Monitoring for SAP][deployment-guide-5.1].</span></span> <span data-ttu-id="8126e-589">Počkejte, než 15 až 30 minut Azure Diagnostics shromažďovat relevantní data.</span><span class="sxs-lookup"><span data-stu-id="8126e-589">Wait 15-30 minutes for Azure Diagnostics to collect the relevant data.</span></span>

#### <span data-ttu-id="8126e-590"><a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure CLI pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="8126e-590"><a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure CLI for Linux VMs</span></span>
<span data-ttu-id="8126e-591">Chcete-li nainstalovat Azure rozšířené monitorování rozšíření pro SAP pomocí rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="8126e-591">To install the Azure Enhanced Monitoring Extension for SAP by using Azure CLI:</span></span>

1. <span data-ttu-id="8126e-592">Instalace rozhraní příkazového řádku Azure, jak je popsáno v [nainstalovat Azure CLI][azure-cli].</span><span class="sxs-lookup"><span data-stu-id="8126e-592">Install Azure CLI, as described in [Install the Azure CLI][azure-cli].</span></span>
2. <span data-ttu-id="8126e-593">Přihlaste se pomocí účtu Azure:</span><span class="sxs-lookup"><span data-stu-id="8126e-593">Sign in with your Azure account:</span></span>

  ```
  azure login
  ```

3. <span data-ttu-id="8126e-594">Přepněte do režimu Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="8126e-594">Switch to Azure Resource Manager mode:</span></span>

  ```
  azure config mode arm
  ```

4. <span data-ttu-id="8126e-595">Povolte Azure rozšířené monitorování:</span><span class="sxs-lookup"><span data-stu-id="8126e-595">Enable Azure Enhanced Monitoring:</span></span>

  ```
  azure vm enable-aem <resource-group-name> <vm-name>
  ```

5. <span data-ttu-id="8126e-596">Ověřte, že Azure rozšířené monitorování rozšíření na virtuální počítač s Linuxem Azure aktivní.</span><span class="sxs-lookup"><span data-stu-id="8126e-596">Verify that the Azure Enhanced Monitoring Extension is active on the Azure Linux VM.</span></span> <span data-ttu-id="8126e-597">Zkontrolujte, zda soubor \\var\\lib\\AzureEnhancedMonitor\\PerfCounters existuje.</span><span class="sxs-lookup"><span data-stu-id="8126e-597">Check whether the file \\var\\lib\\AzureEnhancedMonitor\\PerfCounters exists.</span></span> <span data-ttu-id="8126e-598">Pokud existuje, na příkazovém řádku, spusťte tento příkaz zobrazíte údaje shromážděné pomocí Azure rozšířené monitorování:</span><span class="sxs-lookup"><span data-stu-id="8126e-598">If it exists, at a command prompt, run this command to display information collected by the Azure Enhanced Monitor:</span></span>
```
cat /var/lib/AzureEnhancedMonitor/PerfCounters
```

<span data-ttu-id="8126e-599">Výstup vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="8126e-599">The output looks like this:</span></span>
```
2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
…
…
```

## <span data-ttu-id="8126e-600"><a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Kontroly a řešení potíží pro sledování začátku do konce</span><span class="sxs-lookup"><span data-stu-id="8126e-600"><a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Checks and troubleshooting for end-to-end monitoring</span></span>
<span data-ttu-id="8126e-601">Po nasazení virtuálního počítače Azure a nastavte příslušné monitorování infrastruktury Azure, zkontrolujte, zda jsou všechny součásti Azure rozšířené monitorování rozšíření funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="8126e-601">After you have deployed your Azure VM and set up the relevant Azure monitoring infrastructure, check whether all the components of the Azure Enhanced Monitoring Extension are working as expected.</span></span>

<span data-ttu-id="8126e-602">Spustit kontrolu připravenosti Azure rozšířené monitorování rozšíření pro SAP, jak je popsáno v [kontrolu připravenosti Azure rozšířené monitorování rozšíření pro SAP][deployment-guide-5.1].</span><span class="sxs-lookup"><span data-stu-id="8126e-602">Run the readiness check for the Azure Enhanced Monitoring Extension for SAP as described in [Readiness check for the Azure Enhanced Monitoring Extension for SAP][deployment-guide-5.1].</span></span> <span data-ttu-id="8126e-603">Pokud jsou všechny výsledky kontroly připravenosti kladné a všechny relevantní čítače výkonu zobrazují OK, Azure monitorování se úspěšně nastavilo.</span><span class="sxs-lookup"><span data-stu-id="8126e-603">If all readiness check results are positive and all relevant performance counters appear OK, Azure monitoring successfully set up.</span></span> <span data-ttu-id="8126e-604">Můžete pokračovat v instalaci agenta hostitele SAP popsané v poznámkách k SAP v [SAP prostředky][deployment-guide-2.2].</span><span class="sxs-lookup"><span data-stu-id="8126e-604">You can proceed with the installation of SAP Host Agent described in the SAP Notes in [SAP resources][deployment-guide-2.2].</span></span> <span data-ttu-id="8126e-605">Pokud kontrola připravenosti uvádí, že chybí čítače, spusťte kontrolu stavu pro monitorování infrastruktury Azure, jak je popsáno v [Kontrola stavu pro konfiguraci Azure monitorování infrastruktury][deployment-guide-5.2].</span><span class="sxs-lookup"><span data-stu-id="8126e-605">If the readiness check indicates that counters are missing, run the health check for the Azure monitoring infrastructure, as described in [Health check for Azure monitoring infrastructure configuration][deployment-guide-5.2].</span></span> <span data-ttu-id="8126e-606">Další možnosti řešení potíží najdete v tématu [monitorování řešení potíží s Azure pro SAP][deployment-guide-5.3].</span><span class="sxs-lookup"><span data-stu-id="8126e-606">For more troubleshooting options, see [Troubleshooting Azure monitoring for SAP][deployment-guide-5.3].</span></span>

### <span data-ttu-id="8126e-607"><a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Kontrola připravenosti Azure rozšířené monitorování rozšíření pro SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-607"><a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Readiness check for the Azure Enhanced Monitoring Extension for SAP</span></span>
<span data-ttu-id="8126e-608">Tato kontrola zajišťuje, že všechny metriky výkonu, které se zobrazují v aplikaci SAP jsou poskytovány základní monitorování infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-608">This check makes sure that all performance metrics that appear inside your SAP application are provided by the underlying Azure monitoring infrastructure.</span></span>

#### <a name="run-the-readiness-check-on-a-windows-vm"></a><span data-ttu-id="8126e-609">Spustit kontrolu připravenosti na virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="8126e-609">Run the readiness check on a Windows VM</span></span>

1.  <span data-ttu-id="8126e-610">Přihlaste se k virtuální počítač Azure (pomocí účtu správce není potřeba).</span><span class="sxs-lookup"><span data-stu-id="8126e-610">Sign in to the Azure virtual machine (using an admin account is not necessary).</span></span>
2.  <span data-ttu-id="8126e-611">Otevřete okno příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="8126e-611">Open a Command Prompt window.</span></span>
3.  <span data-ttu-id="8126e-612">Na příkazovém řádku změňte adresář na instalační složku Azure rozšířené monitorování rozšíření pro SAP: C:\\balíčky\\modulů plug-in\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;verze >\\vyřadit</span><span class="sxs-lookup"><span data-stu-id="8126e-612">At the command prompt, change the directory to the installation folder of the Azure Enhanced Monitoring Extension for SAP: C:\\Packages\\Plugins\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;version>\\drop</span></span>

  <span data-ttu-id="8126e-613">*Verze* v cestě k rozšíření monitorování mohou lišit.</span><span class="sxs-lookup"><span data-stu-id="8126e-613">The *version* in the path to the monitoring extension might vary.</span></span> <span data-ttu-id="8126e-614">Pokud se zobrazí složek pro více verzí rozšíření monitorování ve složce instalace, zkontrolujte konfiguraci služby systému AzureEnhancedMonitoring Windows a pak přejděte do složky uvedené jako *cesta ke spustitelnému souboru*.</span><span class="sxs-lookup"><span data-stu-id="8126e-614">If you see folders for multiple versions of the monitoring extension in the installation folder, check the configuration of the AzureEnhancedMonitoring Windows service, and then switch to the folder indicated as *Path to executable*.</span></span>

  ![Vlastnosti služby spuštěna Azure rozšířené monitorování rozšíření pro SAP][deployment-guide-figure-1000]

4.  <span data-ttu-id="8126e-616">Na příkazovém řádku spusťte **azperflib.exe** bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="8126e-616">At the command prompt, run **azperflib.exe** without any parameters.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8126e-617">Azperflib.exe běží ve smyčce a aktualizuje počitadla shromažďovaných každých 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="8126e-617">Azperflib.exe runs in a loop and updates the collected counters every 60 seconds.</span></span> <span data-ttu-id="8126e-618">Pro ukončení smyčky, zavřete okno příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="8126e-618">To end the loop, close the Command Prompt window.</span></span>
  >
  >

<span data-ttu-id="8126e-619">Pokud Azure rozšířené monitorování rozšíření není nainstalována nebo není spuštěná AzureEnhancedMonitoring, rozšíření nebyla nakonfigurována správně.</span><span class="sxs-lookup"><span data-stu-id="8126e-619">If the Azure Enhanced Monitoring Extension is not installed, or the AzureEnhancedMonitoring service is not running, the extension has not been configured correctly.</span></span> <span data-ttu-id="8126e-620">Podrobné informace o tom, jak nasadit rozšíření najdete v tématu [řešení potíží s Azure monitorování infrastruktury pro SAP][deployment-guide-5.3].</span><span class="sxs-lookup"><span data-stu-id="8126e-620">For detailed information about how to deploy the extension, see [Troubleshooting the Azure monitoring infrastructure for SAP][deployment-guide-5.3].</span></span>

##### <a name="check-the-output-of-azperflibexe"></a><span data-ttu-id="8126e-621">Zkontrolujte výstup azperflib.exe</span><span class="sxs-lookup"><span data-stu-id="8126e-621">Check the output of azperflib.exe</span></span>
<span data-ttu-id="8126e-622">Azperflib.exe výstup ukazuje, že všechny čítače výkonu Azure vyplněný pro SAP.</span><span class="sxs-lookup"><span data-stu-id="8126e-622">Azperflib.exe output shows all populated Azure performance counters for SAP.</span></span> <span data-ttu-id="8126e-623">V dolní části seznamu shromážděných čítače indikátoru stavu a souhrn zobrazit stav monitorování Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-623">At the bottom of the list of collected counters, a summary and health indicator show the status of Azure monitoring.</span></span>

<span data-ttu-id="8126e-624">![Výstup Kontrola stavu spuštěním azperflib.exe, který označuje, že neexistují žádné problémy][deployment-guide-figure-1100]
<a name="figure-11"></a></span><span class="sxs-lookup"><span data-stu-id="8126e-624">![Output of health check by executing azperflib.exe, which indicates that no problems exist][deployment-guide-figure-1100]
<a name="figure-11"></a></span></span>

<span data-ttu-id="8126e-625">Zkontrolujte výsledek vrácený pro **čítače celkem** výstupu, který se použije v hlášení jako prázdný a pro **stav**, uvedené v předchozí obrázek.</span><span class="sxs-lookup"><span data-stu-id="8126e-625">Check the result returned for the **Counters total** output, which is reported as empty, and for **Health status**, shown in the preceding figure.</span></span>

<span data-ttu-id="8126e-626">Výsledné hodnoty interpretovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8126e-626">Interpret the resulting values as follows:</span></span>

| <span data-ttu-id="8126e-627">Azperflib.exe výsledek hodnoty</span><span class="sxs-lookup"><span data-stu-id="8126e-627">Azperflib.exe result values</span></span> | <span data-ttu-id="8126e-628">Azure stav monitorování</span><span class="sxs-lookup"><span data-stu-id="8126e-628">Azure monitoring health status</span></span> |
| --- | --- |
| <span data-ttu-id="8126e-629">**Volání rozhraní API – není k dispozici**</span><span class="sxs-lookup"><span data-stu-id="8126e-629">**API Calls - not available**</span></span> | <span data-ttu-id="8126e-630">Čítače, které nejsou k dispozici může být buď není použitelná pro danou konfiguraci virtuálního počítače nebo chyby.</span><span class="sxs-lookup"><span data-stu-id="8126e-630">Counters that are not available might be either not applicable to the virtual machine configuration, or are errors.</span></span> <span data-ttu-id="8126e-631">V tématu **stav**.</span><span class="sxs-lookup"><span data-stu-id="8126e-631">See **Health status**.</span></span> |
| <span data-ttu-id="8126e-632">**Čítače Celkový – prázdné**</span><span class="sxs-lookup"><span data-stu-id="8126e-632">**Counters total - empty**</span></span> |<span data-ttu-id="8126e-633">Následující dva čítače úložiště Azure může být prázdný:</span><span class="sxs-lookup"><span data-stu-id="8126e-633">The following two Azure storage counters can be empty:</span></span> <ul><li><span data-ttu-id="8126e-634">Čtení z úložiště Op latence serveru MS</span><span class="sxs-lookup"><span data-stu-id="8126e-634">Storage Read Op Latency Server msec</span></span></li><li><span data-ttu-id="8126e-635">Čtení z úložiště Op latence E2E MS</span><span class="sxs-lookup"><span data-stu-id="8126e-635">Storage Read Op Latency E2E msec</span></span></li></ul><span data-ttu-id="8126e-636">Všechny ostatní čítače musí mít hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8126e-636">All other counters must have values.</span></span> |
| <span data-ttu-id="8126e-637">**Stav**</span><span class="sxs-lookup"><span data-stu-id="8126e-637">**Health status**</span></span> |<span data-ttu-id="8126e-638">Pouze OK Pokud vrátit stav ukazuje **OK**.</span><span class="sxs-lookup"><span data-stu-id="8126e-638">Only OK if return status shows **OK**.</span></span> |
| <span data-ttu-id="8126e-639">**Diagnostika**</span><span class="sxs-lookup"><span data-stu-id="8126e-639">**Diagnostics**</span></span> |<span data-ttu-id="8126e-640">Podrobné informace o stavu.</span><span class="sxs-lookup"><span data-stu-id="8126e-640">Detailed information about health status.</span></span> |

<span data-ttu-id="8126e-641">Pokud **stav** hodnota není **OK**, postupujte podle pokynů v [Kontrola stavu pro konfiguraci Azure monitorování infrastruktury][deployment-guide-5.2].</span><span class="sxs-lookup"><span data-stu-id="8126e-641">If the **Health status** value is not **OK**, follow the instructions in [Health check for Azure monitoring infrastructure configuration][deployment-guide-5.2].</span></span>

#### <a name="run-the-readiness-check-on-a-linux-vm"></a><span data-ttu-id="8126e-642">Spustit kontrolu připravenosti na virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="8126e-642">Run the readiness check on a Linux VM</span></span>

1.  <span data-ttu-id="8126e-643">Na virtuálním počítači Azure připojte pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="8126e-643">Connect to the Azure Virtual Machine by using SSH.</span></span>

2.  <span data-ttu-id="8126e-644">Zkontrolujte výstup Azure rozšířené monitorování rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8126e-644">Check the output of the Azure Enhanced Monitoring Extension.</span></span>

  <span data-ttu-id="8126e-645">a.</span><span class="sxs-lookup"><span data-stu-id="8126e-645">a.</span></span>  <span data-ttu-id="8126e-646">Spusťte `more /var/lib/AzureEnhancedMonitor/PerfCounters`.</span><span class="sxs-lookup"><span data-stu-id="8126e-646">Run `more /var/lib/AzureEnhancedMonitor/PerfCounters`</span></span>

   <span data-ttu-id="8126e-647">**Očekávaný výsledek**: vrátí seznam čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="8126e-647">**Expected result**: Returns list of performance counters.</span></span> <span data-ttu-id="8126e-648">Soubor by neměl být prázdný.</span><span class="sxs-lookup"><span data-stu-id="8126e-648">The file should not be empty.</span></span>

 <span data-ttu-id="8126e-649">b.</span><span class="sxs-lookup"><span data-stu-id="8126e-649">b.</span></span> <span data-ttu-id="8126e-650">Spusťte `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error`.</span><span class="sxs-lookup"><span data-stu-id="8126e-650">Run `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error`</span></span>

   <span data-ttu-id="8126e-651">**Očekávaný výsledek**: vrátí jeden řádek, kde je chyba **žádné**, například **3; konfigurace; Chyba; 0; 0; žádná; 0; 1456416792; tst-servercs;**</span><span class="sxs-lookup"><span data-stu-id="8126e-651">**Expected result**: Returns one line where the error is **none**, for example, **3;config;Error;;0;0;none;0;1456416792;tst-servercs;**</span></span>

  <span data-ttu-id="8126e-652">c.</span><span class="sxs-lookup"><span data-stu-id="8126e-652">c.</span></span> <span data-ttu-id="8126e-653">Spusťte `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord`.</span><span class="sxs-lookup"><span data-stu-id="8126e-653">Run `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord`</span></span>

    <span data-ttu-id="8126e-654">**Očekávaný výsledek**: vrátí jako prázdný nebo neexistuje.</span><span class="sxs-lookup"><span data-stu-id="8126e-654">**Expected result**: Returns as empty or does not exist.</span></span>

<span data-ttu-id="8126e-655">Pokud předchozí kontroly nebyla úspěšná, spusťte tyto další kontroly:</span><span class="sxs-lookup"><span data-stu-id="8126e-655">If the preceding check was not successful, run these additional checks:</span></span>

1.  <span data-ttu-id="8126e-656">Ujistěte se, že příkaz waagent nainstalován a povolen.</span><span class="sxs-lookup"><span data-stu-id="8126e-656">Make sure that the waagent is installed and enabled.</span></span>

  <span data-ttu-id="8126e-657">a.</span><span class="sxs-lookup"><span data-stu-id="8126e-657">a.</span></span>  <span data-ttu-id="8126e-658">Spusťte `sudo ls -al /var/lib/waagent/`.</span><span class="sxs-lookup"><span data-stu-id="8126e-658">Run `sudo ls -al /var/lib/waagent/`</span></span>

      <span data-ttu-id="8126e-659">**Očekávaný výsledek**: obsahuje seznam obsahu, který příkaz waagent adresáře.</span><span class="sxs-lookup"><span data-stu-id="8126e-659">**Expected result**: Lists the content of the waagent directory.</span></span>

  <span data-ttu-id="8126e-660">b.</span><span class="sxs-lookup"><span data-stu-id="8126e-660">b.</span></span>  <span data-ttu-id="8126e-661">Spusťte `ps -ax | grep waagent`.</span><span class="sxs-lookup"><span data-stu-id="8126e-661">Run `ps -ax | grep waagent`</span></span>

   <span data-ttu-id="8126e-662">**Očekávaný výsledek**: Zobrazí jednu položku podobnou:`python /usr/sbin/waagent -daemon`</span><span class="sxs-lookup"><span data-stu-id="8126e-662">**Expected result**: Displays one entry similar to: `python /usr/sbin/waagent -daemon`</span></span>

2. <span data-ttu-id="8126e-663">Ujistěte se, že rozšíření diagnostiky Linux nainstalováno a povoleno.</span><span class="sxs-lookup"><span data-stu-id="8126e-663">Make sure that the Linux Diagnostic Extension is installed and enabled.</span></span>

  <span data-ttu-id="8126e-664">a.</span><span class="sxs-lookup"><span data-stu-id="8126e-664">a.</span></span>  <span data-ttu-id="8126e-665">Spusťte `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-'`.</span><span class="sxs-lookup"><span data-stu-id="8126e-665">Run `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-'`</span></span>

   <span data-ttu-id="8126e-666">**Očekávaný výsledek**: uvádí obsah adresáře rozšíření diagnostiky Linux.</span><span class="sxs-lookup"><span data-stu-id="8126e-666">**Expected result**: Lists the content of the Linux Diagnostic Extension directory.</span></span>

 <span data-ttu-id="8126e-667">b.</span><span class="sxs-lookup"><span data-stu-id="8126e-667">b.</span></span> <span data-ttu-id="8126e-668">Spusťte `ps -ax | grep diagnostic`.</span><span class="sxs-lookup"><span data-stu-id="8126e-668">Run `ps -ax | grep diagnostic`</span></span>

   <span data-ttu-id="8126e-669">**Očekávaný výsledek**: Zobrazí jednu položku podobnou:`python /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-2.0.92/diagnostic.py -daemon`</span><span class="sxs-lookup"><span data-stu-id="8126e-669">**Expected result**: Displays one entry similar to: `python /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-2.0.92/diagnostic.py -daemon`</span></span>

3.   <span data-ttu-id="8126e-670">Ujistěte se, zda je rozšíření monitorování rozšířeného Azure nainstalovaná a spuštěná.</span><span class="sxs-lookup"><span data-stu-id="8126e-670">Make sure that the Azure Enhanced Monitoring Extension is installed and running.</span></span>

  <span data-ttu-id="8126e-671">a.</span><span class="sxs-lookup"><span data-stu-id="8126e-671">a.</span></span>  <span data-ttu-id="8126e-672">Spusťte `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-/'`.</span><span class="sxs-lookup"><span data-stu-id="8126e-672">Run `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-/'`</span></span>

    <span data-ttu-id="8126e-673">**Očekávaný výsledek**: uvádí obsah adresáře Azure rozšířeného rozšíření monitorování.</span><span class="sxs-lookup"><span data-stu-id="8126e-673">**Expected result**: Lists the content of the Azure Enhanced Monitoring Extension directory.</span></span>

  <span data-ttu-id="8126e-674">b.</span><span class="sxs-lookup"><span data-stu-id="8126e-674">b.</span></span> <span data-ttu-id="8126e-675">Spusťte `ps -ax | grep AzureEnhanced`.</span><span class="sxs-lookup"><span data-stu-id="8126e-675">Run `ps -ax | grep AzureEnhanced`</span></span>

     <span data-ttu-id="8126e-676">**Očekávaný výsledek**: Zobrazí jednu položku podobnou:`python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`</span><span class="sxs-lookup"><span data-stu-id="8126e-676">**Expected result**: Displays one entry similar to: `python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`</span></span>

3. <span data-ttu-id="8126e-677">Nainstalujte agenta hostitele SAP, jak je popsáno v Poznámka SAP [1031096]a zkontrolujte výstup `saposcol`.</span><span class="sxs-lookup"><span data-stu-id="8126e-677">Install SAP Host Agent as described in SAP Note [1031096], and check the output of `saposcol`.</span></span>

  <span data-ttu-id="8126e-678">a.</span><span class="sxs-lookup"><span data-stu-id="8126e-678">a.</span></span>  <span data-ttu-id="8126e-679">Spusťte `/usr/sap/hostctrl/exe/saposcol -d`.</span><span class="sxs-lookup"><span data-stu-id="8126e-679">Run `/usr/sap/hostctrl/exe/saposcol -d`</span></span>

  <span data-ttu-id="8126e-680">b.</span><span class="sxs-lookup"><span data-stu-id="8126e-680">b.</span></span>  <span data-ttu-id="8126e-681">Spusťte `dump ccm`.</span><span class="sxs-lookup"><span data-stu-id="8126e-681">Run `dump ccm`</span></span>

  <span data-ttu-id="8126e-682">c.</span><span class="sxs-lookup"><span data-stu-id="8126e-682">c.</span></span>  <span data-ttu-id="8126e-683">Zkontrolujte, zda **Virtualization_Configuration\Enhanced monitorování přístupu** metrika **true**.</span><span class="sxs-lookup"><span data-stu-id="8126e-683">Check whether the **Virtualization_Configuration\Enhanced Monitoring Access** metric is **true**.</span></span>

<span data-ttu-id="8126e-684">Pokud je již nainstalována aplikační server SAP NetWeaver ABAP, otevřete transakce ST06 sekce a zkontrolujte, zda je povoleno rozšířené monitorování.</span><span class="sxs-lookup"><span data-stu-id="8126e-684">If you already have an SAP NetWeaver ABAP application server installed, open transaction ST06 and check whether enhanced monitoring is enabled.</span></span>

<span data-ttu-id="8126e-685">Pokud některý z těchto kontrol selhání a podrobné informace o tom, jak znovu nasaďte rozšíření najdete v tématu [řešení potíží s Azure monitorování infrastruktury pro SAP][deployment-guide-5.3].</span><span class="sxs-lookup"><span data-stu-id="8126e-685">If any of these checks fail, and for detailed information about how to redeploy the extension, see [Troubleshooting the Azure monitoring infrastructure for SAP][deployment-guide-5.3].</span></span>

### <span data-ttu-id="8126e-686"><a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Kontrola stavu pro konfiguraci Azure monitorování infrastruktury</span><span class="sxs-lookup"><span data-stu-id="8126e-686"><a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Health check for the Azure monitoring infrastructure configuration</span></span>
<span data-ttu-id="8126e-687">Pokud některé z monitorování dat není správně, jak je uvedeno testem popsané v doručí [kontrolu připravenosti pro Azure rozšířené monitorování pro SAP][deployment-guide-5.1]spusťte `Test-AzureRmVMAEMExtension` rutiny zkontrolujte, zda Azure monitorování infrastruktury a monitorování rozšíření pro SAP jsou správně nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="8126e-687">If some of the monitoring data is not delivered correctly as indicated by the test described in [Readiness check for Azure Enhanced Monitoring for SAP][deployment-guide-5.1], run the `Test-AzureRmVMAEMExtension` cmdlet to check whether the Azure monitoring infrastructure and the monitoring extension for SAP are configured correctly.</span></span>

1.  <span data-ttu-id="8126e-688">Ujistěte se, že instalaci nejnovější verze rutiny Azure Powershellu, jak je popsáno v [rutin nasazení prostředí Azure PowerShell][deployment-guide-4.1].</span><span class="sxs-lookup"><span data-stu-id="8126e-688">Make sure that you have installed the latest version of the Azure PowerShell cmdlet, as described in [Deploying Azure PowerShell cmdlets][deployment-guide-4.1].</span></span>
2.  <span data-ttu-id="8126e-689">Spuštěním následující rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8126e-689">Run the following PowerShell cmdlet.</span></span> <span data-ttu-id="8126e-690">Seznam dostupných prostředí, spusťte rutinu `Get-AzureRmEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="8126e-690">For a list of available environments, run the cmdlet `Get-AzureRmEnvironment`.</span></span> <span data-ttu-id="8126e-691">Chcete-li použít veřejné Azure, vyberte **AzureCloud** prostředí.</span><span class="sxs-lookup"><span data-stu-id="8126e-691">To use public Azure, select the **AzureCloud** environment.</span></span> <span data-ttu-id="8126e-692">Azure v Číně, vyberte **AzureChinaCloud**.</span><span class="sxs-lookup"><span data-stu-id="8126e-692">For Azure in China, select **AzureChinaCloud**.</span></span>
  ```powershell
  $env = Get-AzureRmEnvironment -Name <name of the environment>
  Login-AzureRmAccount -Environment $env
  Set-AzureRmContext -SubscriptionName <subscription name>
  Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
  ```

3.  <span data-ttu-id="8126e-693">Zadejte vaše data na účtu a identifikovat virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-693">Enter your account data and identify the Azure virtual machine.</span></span>

  ![Vstupní stránky specifické pro SAP Azure rutiny Test-VMConfigForSAP_GUI][deployment-guide-figure-1200]

4. <span data-ttu-id="8126e-695">Skript testy konfigurace virtuálního počítače, které vyberete.</span><span class="sxs-lookup"><span data-stu-id="8126e-695">The script tests the configuration of the virtual machine you select.</span></span>

  ![Výstup úspěšný test Azure monitorování infrastruktury pro SAP][deployment-guide-figure-1300]

<span data-ttu-id="8126e-697">Ujistěte se, že je každý výsledek kontroly stavu **OK**.</span><span class="sxs-lookup"><span data-stu-id="8126e-697">Make sure that every health check result is **OK**.</span></span> <span data-ttu-id="8126e-698">Pokud nějaké kontroly nezobrazovat **OK**, spusťte rutinu aktualizace, jak je popsáno v [konfiguraci Azure rozšířené monitorování rozšíření přístup k SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="8126e-698">If some checks do not display **OK**, run the update cmdlet as described in [Configure the Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span> <span data-ttu-id="8126e-699">Počkejte 15 minut a opakujte kontroly popsané v [kontrolu připravenosti pro Azure rozšířené monitorování pro SAP] [ deployment-guide-5.1] a [Kontrola stavu pro konfigurace monitorování infrastruktury Azure][deployment-guide-5.2].</span><span class="sxs-lookup"><span data-stu-id="8126e-699">Wait 15 minutes, and repeat the checks described in [Readiness check for Azure Enhanced Monitoring for SAP][deployment-guide-5.1] and [Health check for Azure Monitoring Infrastructure Configuration][deployment-guide-5.2].</span></span> <span data-ttu-id="8126e-700">Pokud kontroly stále znamenat problém související s některé nebo všechny čítače, přečtěte si téma [řešení potíží s Azure monitorování infrastruktury pro SAP][deployment-guide-5.3].</span><span class="sxs-lookup"><span data-stu-id="8126e-700">If the checks still indicate a problem with some or all counters, see [Troubleshooting the Azure monitoring infrastructure for SAP][deployment-guide-5.3].</span></span>

### <span data-ttu-id="8126e-701"><a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Řešení potíží s Azure monitorování infrastruktury pro SAP</span><span class="sxs-lookup"><span data-stu-id="8126e-701"><a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Troubleshooting the Azure monitoring infrastructure for SAP</span></span>

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] <span data-ttu-id="8126e-703">Čítače výkonu Azure se nezobrazí vůbec</span><span class="sxs-lookup"><span data-stu-id="8126e-703">Azure performance counters do not show up at all</span></span>
<span data-ttu-id="8126e-704">Služba systému AzureEnhancedMonitoring Windows shromažďuje metriky výkonu v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-704">The AzureEnhancedMonitoring Windows service collects performance metrics in Azure.</span></span> <span data-ttu-id="8126e-705">Pokud služba není správně nainstalovaná. nebo pokud není spuštěn v virtuálního počítače, se můžou shromažďovat metrik výkonu.</span><span class="sxs-lookup"><span data-stu-id="8126e-705">If the service has not been installed correctly or if it is not running in your VM, no performance metrics can be collected.</span></span>

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a><span data-ttu-id="8126e-706">Instalační adresář Azure rozšířené monitorování rozšíření je prázdná</span><span class="sxs-lookup"><span data-stu-id="8126e-706">The installation directory of the Azure Enhanced Monitoring Extension is empty</span></span>

###### <a name="issue"></a><span data-ttu-id="8126e-707">Problém</span><span class="sxs-lookup"><span data-stu-id="8126e-707">Issue</span></span>
<span data-ttu-id="8126e-708">Instalační adresář C:\\balíčky\\modulů plug-in\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;verze >\\vynechání je prázdný.</span><span class="sxs-lookup"><span data-stu-id="8126e-708">The installation directory C:\\Packages\\Plugins\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;version>\\drop is empty.</span></span>

###### <a name="solution"></a><span data-ttu-id="8126e-709">Řešení</span><span class="sxs-lookup"><span data-stu-id="8126e-709">Solution</span></span>
<span data-ttu-id="8126e-710">Rozšíření není nainstalována.</span><span class="sxs-lookup"><span data-stu-id="8126e-710">The extension is not installed.</span></span> <span data-ttu-id="8126e-711">Určí, zda se jedná o problém proxy (jak je popsáno výše).</span><span class="sxs-lookup"><span data-stu-id="8126e-711">Determine whether this is a proxy issue (as described earlier).</span></span> <span data-ttu-id="8126e-712">Možná muset restartovat počítač nebo znovu spustit `Set-AzureRmVMAEMExtension` konfigurační skript.</span><span class="sxs-lookup"><span data-stu-id="8126e-712">You might need to restart the machine or rerun the `Set-AzureRmVMAEMExtension` configuration script.</span></span>

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a><span data-ttu-id="8126e-713">Služba pro rozšířené monitorování Azure neexistuje.</span><span class="sxs-lookup"><span data-stu-id="8126e-713">Service for Azure Enhanced Monitoring does not exist</span></span>

###### <a name="issue"></a><span data-ttu-id="8126e-714">Problém</span><span class="sxs-lookup"><span data-stu-id="8126e-714">Issue</span></span>
<span data-ttu-id="8126e-715">Služba systému AzureEnhancedMonitoring Windows neexistuje.</span><span class="sxs-lookup"><span data-stu-id="8126e-715">The AzureEnhancedMonitoring Windows service does not exist.</span></span>

<span data-ttu-id="8126e-716">Výstup Azperflib.exe vrátí chybu:</span><span class="sxs-lookup"><span data-stu-id="8126e-716">Azperflib.exe output throws an error:</span></span>

<span data-ttu-id="8126e-717">![Provádění azperflib.exe označuje, zda není spuštěna služba Azure rozšířené monitorování rozšíření pro SAP][deployment-guide-figure-1400]
<a name="figure-14"></a></span><span class="sxs-lookup"><span data-stu-id="8126e-717">![Execution of azperflib.exe indicates that the service of the Azure Enhanced Monitoring Extension for SAP is not running][deployment-guide-figure-1400]
<a name="figure-14"></a></span></span>

###### <a name="solution"></a><span data-ttu-id="8126e-718">Řešení</span><span class="sxs-lookup"><span data-stu-id="8126e-718">Solution</span></span>
<span data-ttu-id="8126e-719">Pokud služba neexistuje, Azure rozšířené monitorování rozšíření pro SAP nebyl nainstalován správně.</span><span class="sxs-lookup"><span data-stu-id="8126e-719">If the service does not exist, the Azure Enhanced Monitoring Extension for SAP has not been installed correctly.</span></span> <span data-ttu-id="8126e-720">Znovu nasaďte rozšíření pomocí kroků popsaných pro váš scénář nasazení v [scénáře nasazení virtuálních počítačů pro SAP v Azure][deployment-guide-3].</span><span class="sxs-lookup"><span data-stu-id="8126e-720">Redeploy the extension by using the steps described for your deployment scenario in [Deployment scenarios of VMs for SAP in Azure][deployment-guide-3].</span></span>

<span data-ttu-id="8126e-721">Poté, co nasadíte rozšíření, po jedné hodině, zkontrolujte znovu, zda jsou čítače výkonu Azure součástí virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-721">After you deploy the extension, after one hour, check again whether the Azure performance counters are provided in the Azure VM.</span></span>

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-to-start"></a><span data-ttu-id="8126e-722">Služba pro rozšířené monitorování Azure existuje, ale nepodaří spustit</span><span class="sxs-lookup"><span data-stu-id="8126e-722">Service for Azure Enhanced Monitoring exists, but fails to start</span></span>

###### <a name="issue"></a><span data-ttu-id="8126e-723">Problém</span><span class="sxs-lookup"><span data-stu-id="8126e-723">Issue</span></span>
<span data-ttu-id="8126e-724">Služba systému AzureEnhancedMonitoring Windows existuje a je povoleno, ale nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="8126e-724">The AzureEnhancedMonitoring Windows service exists and is enabled, but fails to start.</span></span> <span data-ttu-id="8126e-725">Další informace najdete v protokolu událostí aplikace.</span><span class="sxs-lookup"><span data-stu-id="8126e-725">For more information, check the application event log.</span></span>

###### <a name="solution"></a><span data-ttu-id="8126e-726">Řešení</span><span class="sxs-lookup"><span data-stu-id="8126e-726">Solution</span></span>
<span data-ttu-id="8126e-727">Konfigurace je nesprávná.</span><span class="sxs-lookup"><span data-stu-id="8126e-727">The configuration is incorrect.</span></span> <span data-ttu-id="8126e-728">Restartujte rozšíření monitorování pro virtuální počítač, jak je popsáno v [konfiguraci Azure rozšířené monitorování rozšíření přístup k SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="8126e-728">Restart the monitoring extension for the VM, as described in [Configure the Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span>

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] <span data-ttu-id="8126e-730">Chybí některé čítače výkonu Azure</span><span class="sxs-lookup"><span data-stu-id="8126e-730">Some Azure performance counters are missing</span></span>
<span data-ttu-id="8126e-731">Služba systému AzureEnhancedMonitoring Windows shromažďuje metriky výkonu v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-731">The AzureEnhancedMonitoring Windows service collects performance metrics in Azure.</span></span> <span data-ttu-id="8126e-732">Služba získá data z několika zdrojů.</span><span class="sxs-lookup"><span data-stu-id="8126e-732">The service gets data from several sources.</span></span> <span data-ttu-id="8126e-733">Některé konfigurační data se shromažďují místně a některé metriky výkonu se načítají z Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="8126e-733">Some configuration data is collected locally, and some performance metrics are read from Azure Diagnostics.</span></span> <span data-ttu-id="8126e-734">Z vaší protokolování na úrovni předplatného úložiště jsou použity čítače úložiště.</span><span class="sxs-lookup"><span data-stu-id="8126e-734">Storage counters are used from your logging on the storage subscription level.</span></span>

<span data-ttu-id="8126e-735">Pokud řešení potíží pomocí příkazu Poznámka SAP [1999351] problém nevyřešíte, spusťte znovu `Set-AzureRmVMAEMExtension` konfigurační skript.</span><span class="sxs-lookup"><span data-stu-id="8126e-735">If troubleshooting by using SAP Note [1999351] doesn't resolve the issue, rerun the `Set-AzureRmVMAEMExtension` configuration script.</span></span> <span data-ttu-id="8126e-736">Možná bude muset počkat hodinu, protože úložiště analytics nebo diagnostiky čítače nemusí být vytvářeny ihned po jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="8126e-736">You might have to wait an hour because storage analytics or diagnostics counters might not be created immediately after they are enabled.</span></span> <span data-ttu-id="8126e-737">Pokud potíže potrvají, otevřete zprávu SAP zákaznické podpory na BC OP NT AZR součásti pro systém Windows nebo BC-OP-LNX-AZR pro virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="8126e-737">If the problem persists, open an SAP customer support message on the component BC-OP-NT-AZR for Windows or BC-OP-LNX-AZR for a Linux virtual machine.</span></span>

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] <span data-ttu-id="8126e-739">Čítače výkonu Azure se nezobrazí vůbec</span><span class="sxs-lookup"><span data-stu-id="8126e-739">Azure performance counters do not show up at all</span></span>
<span data-ttu-id="8126e-740">Démon shromážděné metriky výkonu v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-740">Performance metrics in Azure are collected by a daemon.</span></span> <span data-ttu-id="8126e-741">Pokud není spuštěn démon, se můžou shromažďovat metrik výkonu.</span><span class="sxs-lookup"><span data-stu-id="8126e-741">If the daemon is not running, no performance metrics can be collected.</span></span>

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a><span data-ttu-id="8126e-742">Instalační adresář rozšíření Azure rozšířené monitorování je prázdná</span><span class="sxs-lookup"><span data-stu-id="8126e-742">The installation directory of the Azure Enhanced Monitoring extension is empty</span></span>

###### <a name="issue"></a><span data-ttu-id="8126e-743">Problém</span><span class="sxs-lookup"><span data-stu-id="8126e-743">Issue</span></span>
<span data-ttu-id="8126e-744">Adresář \\var\\lib\\příkaz waagent\\ nemá podadresáři rozšíření Azure rozšířené monitorování.</span><span class="sxs-lookup"><span data-stu-id="8126e-744">The directory \\var\\lib\\waagent\\ does not have a subdirectory for the Azure Enhanced Monitoring extension.</span></span>

###### <a name="solution"></a><span data-ttu-id="8126e-745">Řešení</span><span class="sxs-lookup"><span data-stu-id="8126e-745">Solution</span></span>
<span data-ttu-id="8126e-746">Rozšíření není nainstalována.</span><span class="sxs-lookup"><span data-stu-id="8126e-746">The extension is not installed.</span></span> <span data-ttu-id="8126e-747">Určí, zda se jedná o problém proxy (jak je popsáno výše).</span><span class="sxs-lookup"><span data-stu-id="8126e-747">Determine whether this is a proxy issue (as described earlier).</span></span> <span data-ttu-id="8126e-748">Možná budete muset restartovat počítač a znovu spustit `Set-AzureRmVMAEMExtension` konfigurační skript.</span><span class="sxs-lookup"><span data-stu-id="8126e-748">You might need to restart the machine and/or rerun the `Set-AzureRmVMAEMExtension` configuration script.</span></span>

#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] <span data-ttu-id="8126e-750">Chybí některé čítače výkonu Azure</span><span class="sxs-lookup"><span data-stu-id="8126e-750">Some Azure performance counters are missing</span></span>
<span data-ttu-id="8126e-751">Démon procesu, který získá data z několika zdrojů shromážděné metriky výkonu v Azure.</span><span class="sxs-lookup"><span data-stu-id="8126e-751">Performance metrics in Azure are collected by a daemon, which gets data from several sources.</span></span> <span data-ttu-id="8126e-752">Některé konfigurační data se shromažďují místně a některé metriky výkonu se načítají z Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="8126e-752">Some configuration data is collected locally, and some performance metrics are read from Azure Diagnostics.</span></span> <span data-ttu-id="8126e-753">Úložiště čítače pocházet z protokolů v rámci vašeho předplatného úložiště.</span><span class="sxs-lookup"><span data-stu-id="8126e-753">Storage counters come from the logs in your storage subscription.</span></span>

<span data-ttu-id="8126e-754">Pro dokončení a aktuální seznam známých problémů, viz poznámka SAP [1999351], která obsahuje další informace o řešení potíží pro rozšířené monitorování Azure pro SAP.</span><span class="sxs-lookup"><span data-stu-id="8126e-754">For a complete and up-to-date list of known issues, see SAP Note [1999351], which has additional troubleshooting information for Enhanced Azure Monitoring for SAP.</span></span>

<span data-ttu-id="8126e-755">Pokud řešení potíží pomocí příkazu Poznámka SAP [1999351] neobsahuje problém vyřešit, spusťte znovu `Set-AzureRmVMAEMExtension` konfigurační skript, jak je popsáno v [konfiguraci Azure rozšířené monitorování rozšíření přístup k SAP][deployment-guide-4.5].</span><span class="sxs-lookup"><span data-stu-id="8126e-755">If troubleshooting by using SAP Note [1999351] does not resolve the issue, rerun the `Set-AzureRmVMAEMExtension` configuration script as described in [Configure the Azure Enhanced Monitoring Extension for SAP][deployment-guide-4.5].</span></span> <span data-ttu-id="8126e-756">Bude pravděpodobně nutné čekat na jednu hodinu, protože úložiště analytics nebo diagnostiky čítače nemusí vytvořit okamžitě po jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="8126e-756">You might have to wait for an hour because storage analytics or diagnostics counters might not be created immediately after they are enabled.</span></span> <span data-ttu-id="8126e-757">Pokud potíže potrvají, otevřete zprávu SAP zákaznické podpory na BC OP NT AZR součásti pro systém Windows nebo BC-OP-LNX-AZR pro virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="8126e-757">If the problem persists, open an SAP customer support message on the component BC-OP-NT-AZR for Windows or BC-OP-LNX-AZR for a Linux virtual machine.</span></span>
