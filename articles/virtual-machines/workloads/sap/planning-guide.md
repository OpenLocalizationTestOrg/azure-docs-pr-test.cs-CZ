---
title: "aaaAzure virtuální počítače plánování a implementace pro SAP NetWeaver | Microsoft Docs"
description: "Azure virtuálních počítačů, plánování a implementace pro SAP NetWeaver"
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: d7c59cc1-b2d0-4d90-9126-628f9c7a5538
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d1e9fa13d847e4d8abb3c34fd352d1f4ece3717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-planning-and-implementation-for-sap-netweaver"></a>Azure virtuálních počítačů, plánování a implementace pro SAP NetWeaver
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

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
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

[deployment-guide]:deployment-guide.md
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
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
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

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

[planning-guide]:planning-guide.md  
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92

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
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/planning-monitoring-overview-2502.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-2901]:media/virtual-machines-shared-sap-planning-guide/2901-azure-ha-sap-ha-md.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-3201]:media/virtual-machines-shared-sap-planning-guide/3201-sap-ha-with-sql-md.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
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
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-windows-capture-image-resource-manager]:../../windows/capture-image.md
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-create-upload-vhd-oracle]:../../linux/oracle-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes-linux]:../../linux/sizes.md
[virtual-machines-sizes-windows]:../../windows/sizes.md
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
[virtual-networks-multiple-nics-windows]:../../windows/multiple-nics.md
[virtual-networks-multiple-nics-linux]:../../linux/multiple-nics.md
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
[capture-image-linux-step-2-create-vm-image]:../../linux/capture-image.md#step-2-create-vm-image

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Microsoft Azure umožňuje společnosti tooacquire výpočetní a úložnou kapacitu v minimálním čase bez zdlouhavé nákup cykly. Virtuální počítače Azure umožňují společnosti toodeploy klasického aplikacím, jako SAP NetWeaver na základě aplikace do Azure a rozšířit jejich spolehlivosti a dostupnosti bez nutnosti další prostředky k dispozici místně. Služby virtuálního počítače Azure také podporuje připojení mezi různými místy, které umožňuje společnosti tooactively integrovat virtuálních počítačích Azure do místní domény, jejich privátních Cloudů a jejich šířku systému SAP.
Tento dokument popisuje hello základy virtuální počítač Microsoft Azure a poskytuje návod aspekty plánování a implementace pro SAP NetWeaver instalace v Azure a jako takový musí být hello dokumentu tooread před zahájením skutečné nasazení SAP NetWeaver v Azure.
zadané platformy Hello dokumentu doplňuje hello SAP instalace dokumentace a poznámky k SAP, které představují hello primární prostředky pro instalace a nasazení softwaru SAP na.

## <a name="summary"></a>Souhrn
Cloud Computing je často používaný výrazu, který je získání více význam v rámci hello IT odvětví, z malých společností až toolarge a mezinárodních společnosti.

Microsoft Azure je hello cloudové služby společnosti Microsoft, který nabízí široké spektrum nové možnosti. Nyní zákazníci jsou zřídit a deaktivace zřízení aplikací mít toorapidly jako služba v cloudu hello, takže nejsou omezené tootechnical nebo rozpočet omezení. Společnosti můžete místo času a investovat do hardwaru infrastruktury, soustředit na aplikace hello, podnikové procesy a jeho výhody pro zákazníky a uživatele.

Pomocí služeb Microsoft Azure pro virtuální počítače nabízí společnost Microsoft komplexní platformu infrastruktury jako služby (IaaS). Služba Azure Virtual Machines teď v rámci IaaS podporuje aplikace využívající SAP NetWeaver. Tento dokument White Paper popisuje, jak tooplan a implementujte SAP NetWeaver založených na aplikace v rámci Microsoft Azure jako platformu hello výběru.

Hello dokumentu samotné se zaměřuje na dvě hlavní aspekty:

* Hello první část popisuje dva vzory podporované nasazení SAP NetWeaver na základě aplikací na platformě Azure. Také popisuje obecné zpracování Azure s nasazení SAP v paměti.
* Hello druhou část Podrobnosti o implementaci hello dva různé scénáře popsané v první části hello.

Další zdroje naleznete v kapitole [prostředky] [ planning-guide-1.2] v tomto dokumentu.

### <a name="definitions-upfront"></a>Definice předem
V dokumentu hello používáme hello následující podmínky:

* IaaS: Infrastruktury jako služby
* PaaS: Platforma jako služba
* SaaS: Software jako služba
* ARM: Azure Resource Manager
* Součást SAP: jednotlivých SAP aplikace například ECC, BW, správce řešení nebo podnikovém portálu.  SAP součástí může být založen na tradičních technologií ABAP nebo Java nebo jiných NetWeaver na základě aplikaci, například obchodních objektů.
* Prostředí SAP: jeden nebo více součástí SAP logicky seskupeny tooperform obchodní funkce jako je například vývoj, QAS, školení, zotavení po Havárii nebo produkční.
* SAP na šířku: Vztahuje toohello celý SAP prostředky v zákazníka na šířku IT. Hello SAP šířku zahrnuje všechny produkční a mimo provozní prostředí.
* Systém SAP: kombinace hello databázového systému vrstvu a aplikační vrstvu služby, například SAP ERP vývojového systému SAP BW testovací systém, produkční systému SAP CRM, atd... V Azure nasazení není podporované toodivide tyto dvě vrstvy mezi místními a Azure. To znamená, že systému SAP buď je nasazena místně nebo je nasazené v Azure. Můžete však nasadit hello různých systémech šířku SAP do Azure nebo místní. Například může nasadit systémy vývoj a testování SAP CRM hello v Azure, ale hello SAP CRM produkční systému místní.
* Nasazení jenom cloudu: nasazení, kde není hello předplatného Azure připojená prostřednictvím site-to-site nebo ExpressRoute připojení toohello místní síťové infrastruktuře. Společné dokumentace k Azure tyto typy nasazení jsou také popsány jako 'jenom pro Cloud, nasazení. Virtuální počítače nasazené pomocí této metody jsou přístupné prostřednictvím Internetu hello a veřejnou IP adresu nebo veřejném DNS název přiřazené toohello virtuálních počítačů v Azure. Pro Microsoft Windows hello v místní službě Active Directory (AD) a DNS není rozšířeno tooAzure v těchto typů nasazení. Virtuální počítače hello proto nejsou součástí hello místní služby Active Directory. Totéž platí pro implementace Linux, například pomocí OpenLDAP + protokolu Kerberos.

> [!NOTE]
> Čistě cloudové nasazení v tomto dokumentu je definován jako dokončení krajiny SAP běží výhradně v Azure bez přípony služby Active Directory nebo OpenLDAP nebo překladu názvů z místní do veřejného cloudu. Jenom pro cloud konfigurace nejsou podporovány pro produkční systémy SAP nebo konfigurace, kde moduly STM SAP nebo jiné místní prostředky musí toobe používá mezi systémy SAP hostované v Azure a prostředky, které se nacházejí v místě.
>
>

* Mezi různými místy: Popisuje scénář, kde jsou virtuální počítače nasazené tooan předplatné Azure, který má site-to-site, více lokalit nebo připojením ExpressRoute mezi místní hello datových centrech a Azure. Dokumentace k společné Azure, tyto typy nasazení jsou také popsány jako mezi různými místy scénáře. Hello důvod pro připojení hello je tooextend místními doménami, místní Active Directory nebo OpenLDAP a místní DNS do Azure. Hello místně na šířku je rozšířené toohello Azure prostředky předplatného hello. S touto příponou, hello virtuálních počítačů může být součástí hello místní domény. Uživatelé domény hello místní domény může přistupovat k serverům hello a službu lze spouštět na ty virtuální počítače (např. služby databázového systému). Komunikace a název rozlišení mezi virtuální počítače nasazené na místě a nasazené virtuální počítače Azure je možné. Toto je hello scénáře, který Očekáváme, že většina toobe SAP prostředky nasazené v. Další informace najdete v tématu [to] [ vpn-gateway-cross-premises-options] článku a [to][vpn-gateway-site-to-site-create].

> [!NOTE]
> Mezi různými místy nasazení SAP systémy, kde Azure Virtual Machines s SAP systémy jsou členy místní domény jsou podporovány pro produkční systémy SAP. Konfigurace mezi různými místy jsou podporovány pro nasazení částí nebo dokončení krajiny SAP do Azure. Hello dokončení SAP na šířku i běžící v Azure vyžaduje, že tyto virtuální počítače byly součástí místní domény a služby Active Directory nebo OpenLDAP. V předchozí verze dokumentace hello už jsme mluvili o IT hybridní scénáře, kde se zobrazuje hello termín "Hybridní" v hello fakt, že existuje připojení mezi různými místy mezi místními a Azure. Plus, hello fakt, že hello virtuálních počítačů v Azure jsou součástí hello místní služby Active Directory nebo OpenLDAP.
>
>

Některé dokumentaci Microsoft popisuje scénáře mezi různými místy trochu jinak, zejména pro konfigurace HA databázového systému. V případě hello hello související SAP dokumentů hello napříč místním a externím právě boils dolů toohaving site-to-site nebo privátní (ExpressRoute) připojení a hello faktu, hello SAP šířku rozděluje mezi místními a Azure.  

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Prostředky
Hello následujících dalších příručkách jsou k dispozici pro téma hello nasazení SAP v Azure:

* [Azure virtuálních počítačů, plánování a implementace pro SAP NetWeaver (Tento dokument)][planning-guide]
* [Nasazení virtuálních počítačů Azure pro SAP NetWeaver][deployment-guide]
* [Nasazení virtuálních počítačů databázového systému Azure pro SAP NetWeaver][dbms-guide]

> [!IMPORTANT]
> Všude, kde se používá možné odkaz toohello, odkazující Instalační příručka (referenční dokumentace InstGuide-01, najdete v části <http://service.sap.com/instguides>). Když jde o toohello požadavky a proces instalace, hello SAP NetWeaver instalace příručky by měl být vždy přečtěte pečlivě, protože tento dokument popisuje jenom konkrétní úlohy pro SAP NetWeaver systémy nainstalované v virtuální počítač Microsoft Azure.
>
>

Následující poznámky k SAP Hello jsou související toohello tématem SAP v Azure:

| Poznámka: číslo | Název |
| --- | --- |
| [1928533] |Aplikace SAP v Azure: podporované produkty a velikosti |
| [2015553] |SAP na platformě Microsoft Azure: podporovat požadavky |
| [1999351] |Řešení potíží s rozšířenou Azure monitorování pro SAP |
| [2178632] |Klíč monitorování metriky pro SAP na platformě Microsoft Azure |
| [1409604] |Virtualizace v systému Windows: rozšířené monitorování |
| [2191498] |SAP v systému Linux s Azure: rozšířené monitorování |
| [2243692] |Linux na Microsoft Azure (IaaS) virtuálních počítačů: problémy licence SAP |
| [1984787] |Systému SUSE LINUX Enterprise Server 12: Poznámky k instalaci |
| [2002167] |Red Hat Enterprise Linux 7.x: instalace a Upgrade |
| [2069760] |Oracle Linux 7.x SAP instalace a Upgrade |
| [1597355] |Doporučení odkládacího prostoru pro Linux |

Hello si také přečíst [oznámení změny stavu Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) obsahující všechny SAP poznámky pro Linux.

Obecné výchozí omezení a maximální omezení předplatná Azure naleznete v [v tomto článku][azure-subscription-service-limits-subscription].

## <a name="possible-scenarios"></a>Možné scénáře
SAP je často považovat za jeden hello nejvíce důležitých aplikací v rámci firmy. operace těchto aplikací a architektura Hello je většinou velmi složitý a zajistíte, že splňujete požadavky na dostupnosti a výkonu je důležité.

Proto podniků má toothink pečlivě o tom, které aplikace lze spustit v prostředí veřejného cloudu, nezávisle na poskytovatele cloudových služeb vybrali hello.

Typy možné systému pro nasazení SAP NetWeaver na základě aplikací v rámci veřejného cloudu, prostředí, které jsou uvedeny níže:

1. Střední produkční systémy.
2. Vývoj pro systémy
3. Testování systémy
4. Systémy prototypu
5. Učení / ukázkový systémy

V pořadí toosuccessfully nasazení systémů SAP do Azure IaaS nebo IaaS obecně, je důležité toounderstand hello významné rozdíly mezi hello nabídky tradiční outsourcers nebo hostitelé a nabídkou IaaS. Zatímco hello tradiční hostitel nebo pro externí dodavatele přizpůsobuje infrastruktury (typ sítě, úložiště a server) toohello zatížení zákazník chce toohost, se místo toho hello zákazníka odpovědnost toochoose hello správné zatížení pro nasazení IaaS.

Jako první krok třeba zákazníkům tooverify hello následující položky:

* Hello SAP podporované typy virtuálních počítačů Azure
* Hello SAP podporované produkty/verze v Azure
* Hello podporované verze operačního systému a databázového systému pro hello konkrétní SAP uvolní v Azure
* Protokoly SAP propustnost poskytované různé identifikátory SKU Azure

Hello otázky toothese odpovědi lze číst v Poznámka SAP [1928533].

V druhém kroku musí Azure omezení prostředků a šířku pásma toobe porovnání tooactual spotřeby prostředků z místních systémů. Proto zákazníky nutné toobe obeznámeni s hello různé možnosti hello Azure typy podporované s SAP v oblasti hello:

* Prostředky procesoru a paměti různých typů virtuálních počítačů a
* Šířka pásma IOPS různé typy virtuálních počítačů a
* Možnosti sítě různých typů virtuálních počítačů.

Většina těchto dat lze nalézt [zde (Linux)] [ virtual-machines-sizes-linux] a [zde (Windows)][virtual-machines-sizes-windows].

Mějte na paměti, která hello omezení uvedené v výše uvedený odkaz hello jsou horní meze. Neznamená to, že hello limity pro kterýkoli z hello prostředků, například IOPS lze zadat za všech okolností. Hello výjimky, když jsou prostředky procesoru a paměti hello typu zvolené virtuálních počítačů. Pro typy virtuálních počítačů hello nepodporuje SAP hello prostředků procesoru a paměti jsou vyhrazeny a jako takový k dispozici v libovolném bodě v čase pro používání v rámci hello virtuálních počítačů.

Platforma Microsoft Azure Hello jako jiné platformy IaaS je platforma pro více klientů. To znamená, že jsou mezi klienty sdílené úložiště, sítě a dalším prostředkům. Inteligentní omezení a kvóty logiku je klient tooprevent použít jeden z závažný způsobem, což ovlivňuje výkon hello jiného klienta (aktivní sousedním). Přestože logiku v Azure pokusí odchylky tookeep šířky pásma došlo malé, vysoce sdílené platformy zpravidla toointroduce větší odchylky v dostupnosti prostředků nebo šířky pásma než mnoho zákazníků použité tooin jejich místní nasazení. V důsledku toho může dojít k různých úrovních šířky pásma týkající se sítěmi nebo úložištěm vstupně-výstupní operace (hello svazku a také latence) z toominute minutu. Hello pravděpodobnost, že systému SAP v Azure může na základě zkušeností uživatelů odchylky větší než v místním systému musí toobe vzít v úvahu.

Posledním krokem je tooevaluate dostupnosti požadavky. K tomu může dojít, že hello základní infrastrukturu Azure potřebuje tooget aktualizovat a vyžaduje hello hostitele se systémem toobe virtuální počítače restartovat. V těchto případech by se vypínají a restartují i virtuálních počítačů spuštěných na těchto hostitelích. Načasování Hello takové údržby se provádí v pracovní době vedlejší pro konkrétní oblasti, ale je relativně široké hello potenciální okno několik hodin, během které dojde k restartování. Existují různé technologie v rámci hello platformy Azure, kterou lze nakonfigurovat toomitigate některé nebo všechny hello vliv těchto aktualizací. Budoucí vylepšení z hello platformy Azure, databázového systému, a aplikace SAP jsou navržené toominimize hello dopad takové restartování.

V pořadí nasazení toosuccessfully systému SAP do Azure, hello místní SAP systémy operační systém, databáze, a na hello SAP Azure můžete zadat matici podpory, nevejde se do hello hello prostředky infrastruktury Azure a které musí být aplikace SAP můžete pracovat s hello dostupnost SLA Microsoft Azure nabízí. Jak se identifikují ty systémy, musíte toodecide na jednom z hello následující dva scénáře nasazení.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Jenom pro cloud - nasazení virtuálních počítačů do Azure bez závislosti na hello místní síti zákazníka
![Jeden virtuální počítač s ukázkovou SAP nebo scénáře školení nasazené v Azure][planning-guide-figure-100]

Tento scénář je typické pro školení nebo ukázku systémy, kde jsou nainstalovány všechny součásti hello SAP a softwaru jiných SAP v rámci jednoho virtuálního počítače. Produkční SAP systémy nejsou podporovány v tomto scénáři nasazení. Obecně platí tento scénář splňuje hello následující požadavky:

* vlastních virtuálních počítačů Hello jsou přístupné přes veřejnou síť hello. Přímé připojení hello aplikací spuštěných v rámci virtuálních počítačů toohello hello místní síť společnosti buď hello vlastnící hello demoaplikace nebo školení obsah nebo hello zákazníka není nutné.
* V případě několika virtuálních počítačů představující hello školení nebo ukázkový scénář musí síťové komunikace a překladu toowork mezi virtuálními počítači hello. Ale komunikace mezi hello sadu virtuálních počítačů potřebovat toobe izolované tak, aby několik sady virtuálních počítačů můžete nasadit vedle sebe bez narušení.  
* Připojení k Internetu je vyžadována pro hello koncový uživatel tooremote přihlášení do toohello, které virtuální počítače hostované v Azure. V závislosti na hello hostovaný operační systém, je použít Terminálové služby a služby RDS nebo VNC na ssh tooaccess hello virtuálních počítačů tooeither splnění hello školení úlohy nebo provádět hello ukázky. Pokud SAP porty například 3200, 3300 & 3600 může také zpřístupnit instanci aplikace SAP hello je přístupná na jakémkoli počítači připojené Internetu.
* Hello SAP systémy (a VM(s)) představují samostatné scénář v Azure, což pouze vyžaduje veřejné připojení k Internetu pro přístup koncových uživatelů a nevyžaduje tooother připojení virtuálních počítačů v Azure.
* SAPGUI a prohlížeče jsou nainstalovat a spustit přímo na hello virtuálních počítačů.
* Rychlé obnovení původního stavu toohello virtuálních počítačů a znovu nové nasazení tohoto původního stavu je vyžadován.
* V případě hello demo a scénáře školení, které jsou v několika virtuálních počítačů, služby Active Directory realizovány / OpenLDAP nebo DNS služby je zapotřebí pro každou sadu virtuálních počítačů.

![Skupinu Virtuálního počítače představující jednu ukázku nebo školení scénář v cloudové službě Azure][planning-guide-figure-200]

Je důležité, že tookeep si, že hello virtuální počítače v jednotlivých hello nastaví toobe nutné nasadit souběžně, kde jsou hello názvy virtuálních počítačů v každé sady hello hello stejné.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>Mezi různými místy - nasazení jedné nebo více virtuálních počítačů SAP do Azure s hello požadavek se plně integrován do hello do místní sítě
![Síť VPN se připojení Site-To-Site (mezi různými místy)][planning-guide-figure-300]

Tento scénář je mezi různými místy scénář s mnoha schémat možné nasazení. Může být jako jednoduše popsané jako spuštěné některé části hello SAP šířku místní a dalšími částmi sady hello SAP šířku v Azure. Všechny aspekty hello skutečnost, že součástí hello SAP komponent jsou spuštěné v Azure by měl být transparentní pro koncové uživatele. Proto hello SAP přenosu oprava systému (moduly STM), RFC komunikaci, tisk, zabezpečení (jako je jednotné přihlašování) atd fungovat bez problémů pro systémy SAP hello spuštěné v Azure. Ale hello mezi různými místy scénář také popisuje scénář, kde hello dokončení SAP šířku spuštění v Azure s doménou hello zákazníka a DNS rozšířit do Azure.

> [!NOTE]
> Toto je hello scénář nasazení, který je podporován pro produktivní systémem SAP.
>
>

Čtení [v tomto článku] [ vpn-gateway-create-site-to-site-rm-powershell] Další informace o tom, tooconnect místní sítě tooMicrosoft Azure

> [!IMPORTANT]
> Když jsme mluvíme o scénářích mezi různými místy mezi Azure a místními zákaznických nasazení, jsme před sebou hello členitost celou systémy SAP. Scénáře, které jsou *nepodporuje* pro mezi různými místy scénáře jsou:
>
> * Spuštění různých úrovní aplikace SAP v různých metod nasazení. Například spuštění hello databázového systému vrstvy v místě, ale hello SAP aplikační vrstvu ve virtuálních počítačích nasazených ve virtuálních počítačích Azure a naopak.
> * Některé součásti SAP vrstvy v Azure a některé na místě. Příklad rozdělení instance hello SAP aplikační vrstvu mezi místní a virtuální počítače Azure.
> * Není dostupná podpora distribuce virtuálních počítačů spuštěné instance SAP jeden systém nad několika oblastmi Azure.
>
> Hello důvod pro tato omezení je hello požadavek pro vysoce výkonné síť s velmi nízkou latencí v rámci jednoho systému SAP, zejména mezi instancemi aplikace hello a hello databázového systému vrstvy systému SAP.
>
>

### <a name="supported-os-and-database-releases"></a>Podporované operační systém a verze databáze
* Podporované služby virtuálního počítače Azure je uvedené v tomto článku serverového softwaru společnosti Microsoft: <http://support.microsoft.com/kb/2721672>.
* Podporované operační systém verze, vydání systému databáze podporované na služby virtuálního počítače Azure ve spojení s SAP softwaru jsou popsané v Poznámka SAP [1928533].
* Aplikace SAP a verzí podporované na virtuální počítač služby Azure jsou dokumentovány v článku Poznámka SAP [1928533].
* Pouze 64bitové obrázky nejsou podporované toorun jako virtuální počítače hostované v Azure pro SAP scénáře. Také to znamená, že jsou podporovány pouze 64bitové aplikace SAP a databází.

## <a name="microsoft-azure-virtual-machine-services"></a>Služby virtuálního počítače Microsoft Azure
Platforma Microsoft Azure Hello je internetových cloudové platformy služby hostované a provozována centrech dat společnosti Microsoft. Platforma Hello obsahuje služeb virtuálního počítače Microsoft Azure hello (infrastruktura jako služba nebo IaaS) a sadu bohaté platforma jako služba (PaaS) možnosti.

Hello platformy Azure omezuje hello nutnost počáteční technologie a nákupech infrastruktury. Zjednodušuje údržbu a provoz aplikace tím, že poskytuje na vyžádání toohost výpočetního prostředí a úložiště, škálování a správu webové aplikace a propojených aplikací. Správu infrastruktury je automatické s platformu, která je určená pro vysokou dostupnost a dynamické škálování potřeby využití toomatch s hello možnost průběžnými platbami cenový model.

![Umístění služby virtuálního počítače Microsoft Azure][planning-guide-figure-400]

Pomocí služby virtuálního počítače Azure Microsoft je umožňuje tooAzure bitové kopie toodeploy vlastní server jako instance IaaS (viz obrázek 4). Hello virtuálních počítačů v Azure jsou založené na technologii Hyper-V virtuální pevné disky (VHD) a jsou možné toorun různé operační systémy jako hostovaného operačního systému.

Z provozního hlediska vyskytne hello, které služba Azure Virtual Machine poskytuje podobné jako virtuální počítače nasazené místně. Má však hello významné výhody, které nepotřebujete tooprocure, správu a správě infrastruktury hello. Vývojáři a správci mít plnou kontrolu nad hello bitovou kopii operačního systému v rámci těchto virtuálních počítačů. Správci mohou přihlásit vzdáleně v toothose virtuální počítače tooperform údržby a řešení potíží s úlohy a také úlohy nasazení softwaru. V ohledem toodeployment jsou hello pouze omezení velikosti hello a možnosti virtuálních počítačích Azure. Nemusí se jednat o jako jemné granulární v konfiguraci, jako tomu lze místně. Při volbě typů virtuálních počítačů, které představují kombinaci:

* Počet Vcpu,
* Paměť
* Počet virtuálních pevných disků, které je možné připojit,
* Šířek pásma sítě a úložiště.

velikost Hello a omezení velikostí různé virtuální počítače různých nabízí, můžete zobrazit v tabulce v [v tomto článku (Linux)] [ virtual-machines-sizes-linux] a [v tomto článku (Windows)] [ virtual-machines-sizes-windows].

Jak si myslíte, existují různé rodiny nebo řadu virtuálních počítačů. Možné rozlišit hello následující rodiny virtuálních počítačů:

* Virtuální počítač a0 A7 typy: Ne všechny z nich jsou certifikovány pro SAP. První virtuální počítač série, která získali zavedené Azure IaaS.
* Virtuální počítač a8-A11 typy: instance s vysokým výkonem. Běžet na různých lepší provádění výpočetní hostitelů než ostatní virtuální počítače A-series.
* Počítač D/Dv2-Series typy: lépe než A0 A7 provádění. Ne všechny typy hello virtuálních počítačů jsou certifikované s SAP.
* Typy virtuálních počítačů služby DS, DSv2-Series: podobné tooD/Dv2-series, ale jsou možné tooconnect tooAzure Storage úrovně Premium (naleznete v kapitole [Azure Premium Storage] [ planning-guide-3.3.2] tohoto dokumentu). Znovu ne všechny typy virtuálních počítačů, které jsou certifikované s SAP.
* Virtuální počítač G-Series typy: typy vysokého využití paměti virtuálního počítače.
* Virtuální počítač GS-Series typy: jako G-Series, ale včetně hello možnost toouse Storage úrovně Premium (naleznete v kapitole [Azure Premium Storage] [ planning-guide-3.3.2] tohoto dokumentu). Při použití GS-Series virtuální počítače jako databázové servery, je povinné toouse Storage úrovně Premium pro soubory protokolů databáze dat a transakce

Stejné konfigurace procesoru a paměti může najde hello v řadě různých virtuálních počítačů. Nicméně když hledáte hello propustnost těchto virtuálních počítačů z řady různých hello se mohou výrazně lišit. Přes hello stejné konfigurace procesoru a paměti. Důvodem je, že hello základní hostitele serverový hardware v hello Úvod hello různých typů virtuálních počítačů mají různé propustnost vlastnosti.  Obvykle hello rozdíl ukazuje propustnost je odráží rovněž v hello cena hello různé virtuální počítače.

Ne všechny řady různých virtuálních počítačů může nabízena v každé z nich hello oblasti Azure (pro oblasti Azure naleznete v kapitole Další). Nezapomínejte, že ne všechny virtuální počítače nebo řadu virtuálních počítačů jsou certifikovány pro SAP.

> [!IMPORTANT]
> Pro použití hello SAP NetWeaver na základě aplikací, jenom podmnožina hello typů virtuálních počítačů a konfigurace uvedené v Poznámka SAP [1928533] jsou podporovány.
>
>

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Oblasti Azure
Microsoft umožňuje toodeploy virtuálních počítačů do tak volané Azure oblasti. Oblast Azure může být jeden nebo více datových center, které se nacházejí v těsné blízkosti. Pro většinu hello geopolitické oblasti v hello, world Microsoft má alespoň dva oblasti Azure. Například v Evropě není oblast Azure, Severní Evropa, a jednu z "Západní Evropa". Tyto dvě Azure oblastem v geopolitické oblasti jsou odděleny dostatečně velkou vzdálenost tak, aby přírodních nebo technických havárie neovlivní obou oblastí Azure v hello stejné geopolitické oblasti. Vzhledem k tomu, že Microsoft vytrvale vytváří se nové oblasti Azure v různých oblastech geopolitické globálně, hello počet tyto oblasti vytrvale roste a od prosince 2015 dosaženo hello počtu 20 oblasti Azure s další oblasti oznámeno již. Systémy SAP vy jako zákazník můžete nasadit do všech těchto oblastech, včetně hello dvou oblastech Azure v Číně. Aktuální aktuální informace o oblastech Azure najdete v tomto webu: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>Hello koncept virtuálního počítače Microsoft Azure
Microsoft Azure nabízí infrastruktury jako toohost řešení služby (IaaS) virtuálních počítačů s podobné funkce jako místní řešení virtualizace. Se může toocreate virtuální počítače v rámci hello portál Azure, PowerShell nebo rozhraní příkazového řádku, které také nabízí nasazení a možnosti správy.

Azure Resource Manager můžete tooprovision vaší aplikace s použitím deklarativní šablony. S jednou šablonou můžete nasadit několik služeb společně s jejich závislostmi. Použít hello stejné šablony toorepeatedly nasazení aplikace během každé fáze životního cyklu aplikace hello.

Další informace o používání šablon Resource Manageru naleznete zde:

* [Nasadit a spravovat virtuální počítače pomocí šablony Azure Resource Manager a hello rozhraní příkazového řádku Azure] [.. /.. / linux/create-ssh-secured-vm-from-template.md]
* [Správa virtuálních počítačů pomocí Azure Resource Manageru a prostředí PowerShell][virtual-machines-deploy-rmtemplates-powershell]
* <https://Azure.microsoft.com/documentation/Templates/>

Další zajímavých funkcí je hello možnost toocreate bitové kopie z virtuálních počítačů, které vám umožní tooprepare určité úložiště, ze kterých se může tooquickly nasazení instance virtuálních počítačů, které odpovídají vašim požadavkům.

Další informace o vytváření bitové kopie z virtuálních počítačů naleznete v [v tomto článku (Linux)] [ virtual-machines-linux-capture-image-resource-manager] a [v tomto článku (Windows)][virtual-machines-windows-capture-image-resource-manager].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Domén selhání
Fyzická jednotka nezdaří, velmi úzce související toohello fyzické infrastruktuře obsažené v datových centrech, a když fyzické okno nebo rack lze považovat za domény selhání představují domén selhání, neexistuje žádné přímé mapování 1: 1 mezi hello dva.

Při nasazení více virtuálních počítačů v rámci jednoho systému SAP v služeb virtuálního počítače Microsoft Azure, můžete ovlivnit hello Kontroleru prostředků infrastruktury Azure toodeploy vaší aplikace do různých domén selhání, a tím splňuje požadavky hello hello Microsoft Azure SLA. Však hello distribuce domén selhání přes jednotky škálování služby Azure (shromažďování stovky výpočetních uzlů nebo uzlů úložiště a sítě) nebo hello přiřazení virtuální počítače tooa konkrétní doméně selhání je něco přes které nemáte přímé ovládacího prvku. V pořadí toodirect hello prostředků infrastruktury Azure řadiče toodeploy sadu virtuálních počítačů v různých doménách selhání musíte v době nasazení tooassign toohello Azure sadu dostupnosti virtuálních počítačů. Další informace o Azure skupiny dostupnosti, naleznete v kapitole [skupiny dostupnosti Azure] [ planning-guide-3.2.3] v tomto dokumentu.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Domén upgradu
Domén upgradu představují logické jednotce, která pomáhají toodetermine, jak se virtuální počítač v rámci systému SAP, která se skládá z instancí SAP spuštěných v několika virtuálními počítači, aktualizuje. Když dojde k upgradu, Microsoft Azure projde hello proces aktualizace těchto domén upgradu po jednom. Tak, že se virtuální počítače v době nasazení v různých doménách upgradu, můžete chránit váš systém SAP částečně z potenciální výpadek. V pořadí tooforce Azure toodeploy hello virtuální počítače v různých doménách Upgrade systému SAP, musíte tooset konkrétní atribut v době nasazení každého virtuálního počítače. Podobně jako tooFault domén, představuje jednotku škálování Azure je rozdělené do několika domén upgradu. Pořadí toodirect hello prostředků infrastruktury Azure řadiče toodeploy sadu virtuálních počítačů v různých doménách upgradovat je nutné tooassign toohello Azure sadu dostupnosti virtuálních počítačů v době nasazení. Další informace o Azure skupiny dostupnosti, naleznete v kapitole [skupiny dostupnosti Azure] [ planning-guide-3.2.3] níže.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Skupiny dostupnosti Azure
Virtuální počítače Azure v rámci jedné skupiny dostupnosti Azure rozdělené podle hello Kontroleru prostředků infrastruktury Azure přes různé selhání a upgradu domény. účelem Hello rozdělení hello přes různé selhání a upgradu domény je tooprevent všech virtuálních počítačů systému SAP nebudou vypnut v případě hello údržbu infrastruktury nebo selhání v rámci jedné domény selhání. Ve výchozím nastavení virtuální počítače nejsou součástí skupiny dostupnosti. účast Hello virtuálního počítače ve skupině dostupnosti je definována v době nasazení nebo později tak, že změna konfigurace a opětovné nasazení virtuálního počítače.

Číst koncept hello toounderstand skupinami dostupnosti služby Azure a hello způsob skupiny dostupnosti se týkají tooFault a upgradu domény [v tomto článku][virtual-machines-manage-availability]

skupiny dostupnosti toodefine pro ARM prostřednictvím šablonu json najdete v části [hello specifikace rozhraní rest api](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) a vyhledejte "dostupnosti".

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Úložiště: Microsoft Azure Storage a datové disky
Microsoft Azure virtuální počítače využívat různé typy úložišť. Při implementaci SAP na služby virtuálního počítače Azure, je důležité toounderstand hello rozdíly mezi tyto dva hlavní typy úložiště:

* Bez trvalé, volatile úložiště.
* Trvalé úložiště.

Hello dočasnou úložiště je přímo připojené toohello běžící virtuální počítače a se nachází na výpočetních uzlech hello sami – hello místní instance úložiště (dočasné úložiště). velikost Hello závisí na velikosti hello hello vybrali při spuštění hello nasazení virtuálního počítače. Tento typ úložiště je volatile a proto hello disk inicializován při restartování instanci virtuálního počítače. Hello stránkovacího souboru pro operační systém hello se obvykle nachází na toto dočasný disku.

- - -
> ![Windows][Logo_Windows] Windows
>
> Na virtuálních počítačích Windows hello dočasného jednotka připojena jako jednotku D:\ v nasazených virtuálních počítačů.
>
> ![Linux][Logo_Linux] Linux
>
> Na virtuální počítače s Linuxem je připojené jako /mnt/resource nebo /mnt. Naleznete zde podrobnosti:
>
> * [Jak tooAttach datový Disk tooa Linux virtuálního počítače][virtual-machines-linux-how-to-attach-disk]
> * <https://docs.microsoft.com/Azure/Storage/Storage-About-Disks-and-vhds-Linux#Temporary-disk>
>
>

- - -
skutečné jednotka Hello je volatile, protože je získávání uložený na samotném serveru hello hostitele. Pokud se přesune hello virtuálních počítačů v opětovné nasazení (například kvůli toomaintenance v hostiteli hello nebo vypnutí a restartování) dojde ke ztrátě hello obsah hello jednotky. Proto není toostore možnost žádná důležitá data pro tuto jednotku. mezi jinou sérii virtuálních počítačů se příliš neliší výkonové charakteristiky, které od června 2015 vypadat liší Hello typu média použitého pro tento typ úložiště:

* A5-A7: Velmi omezená výkonu. Není doporučeno pro všechno, co je nad rámec stránkovacího souboru
* A8-A11: Velmi dobré výkonové charakteristiky se některé deset tisíc IOPS a > propustnost 1GB/s.
* D-Series: Velmi dobré výkonové charakteristiky se některé pak tisíců IOPS a > propustnost 1GB/s.
* DS-Series: Velmi dobré výkonové charakteristiky se některé deset tisíc IOPS a > propustnost 1GB/s.
* G-Series: Velmi dobré výkonové charakteristiky se některé deset tisíc IOPS a > propustnost 1GB/s.
* GS-Series: Velmi dobré výkonové charakteristiky se některé deset tisíc IOPS a > propustnost 1GB/s.

Výše uvedené příkazy se má použít toohello typy virtuálních počítačů, které jsou certifikované s SAP. Hello řady virtuálních počítačů s vynikající IOPS a propustnost kvalifikaci pro využívání některé funkce databázového systému. Další informace najdete v tématu hello [Průvodce nasazením databázového systému][dbms-guide].

Microsoft Azure Storage poskytuje trvalou úložiště a hello typické úrovně ochrany a redundance jsme si ukazovali tady úložiště SAN. Disky založené na Azure Storage jsou virtuální pevný disk (VHD) v hello služby úložiště Azure. Hello místní Disk operačního systému (Windows C:\, Linux/dev/sda1) je uložen na hello Azure Storage a další svazky nebo disky připojené toohello virtuální počítač získat v ní uloženy, příliš.

Je možné tooupload existujícího virtuálního pevného disku z místní nebo vytvořit prázdných z v rámci Azure a připojte tyto virtuální počítače toodeployed.

Po vytvoření nebo odesílání virtuálního pevného disku do úložiště Azure, je možné toomount a připojte tyto tooan existující virtuální počítač a toocopy existující virtuální pevný disk (nepřipojené).

Tyto virtuální pevné disky jsou nastavené jako trvalé, data a změny v těch, které jsou bezpečné při restartování a znovu vytvořit instanci virtuálního počítače. I když je odstraněna instance, tyto virtuální pevné disky zůstal v bezpečí a můžete znovu nasadit nebo v případě disky bez operačního systému, může být připojené tooother virtuální počítače.

V rámci hello mohou být konfigurovány síťové redundance různé úrovně úložiště Azure:

* Minimální úroveň, kterou je možné vybrat je "místní redundanci', která je ekvivalentní toothree repliky hello dat v rámci hello stejném datovém centru oblasti Azure (naleznete v kapitole [oblasti Azure][planning-guide-3.1]).
* Zóny redundantní úložiště, které šíří hello tři Image přes různých datových střediscích v rámci hello stejné oblasti Azure.
* Výchozí úroveň redundance je geografická redundance, která asynchronně replikuje hello obsahu do jiného tři bitové kopie hello data do jiné oblasti Azure, který je hostován v hello stejné geopolitické oblasti.

Informace v tématu hello tabulky v tomto článku týkající se možnosti různých redundance hello: <https://azure.microsoft.com/pricing/details/storage/>

Další informace o službě Azure Storage naleznete zde:

* <https://Azure.microsoft.com/documentation/Services/Storage/>
* <https://Azure.microsoft.com/Services/Site-Recovery>
* <https://docs.microsoft.com/REST/API/storageservices/Understanding-Block-BLOBS--append-BLOBS--and-Page-BLOBS>
* <https://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/Azure-disk-Encryption-for-Linux-and-Windows-Virtual-Machines-Public-Preview.aspx>

#### <a name="azure-standard-storage"></a>Úložiště Azure úrovně Standard
Úložiště Azure úrovně Standard byl hello typu úložiště k dispozici, pokud byl vydán Azure IaaS. Nebyly IOPS kvóty na jednom disku. Latence došlo nebyla ve hello stejné třídy jako zařízení sítě SAN nebo NAS obvykle nasazují systémů SAP vyšší kategorie hostované na místě. Nicméně hello Azure Standard Storage prokázat dostatečná pro mnoho stovky systémy SAP mezitím nasazené v Azure.

Disky, které jsou uložené na standardních účtech úložiště Azure budou účtovat podle hello skutečná data, která je uložena, objem hello transakce úložiště, odchozí přenosy dat a zvolená možnost redundance. Mnoho disky se dají vytvořit v hello maximálně 1 TB, velikost, ale také ty zůstat prázdné je bezplatná. Pokud pak zadejte jeden virtuální pevný disk s 100GB, budou se účtovat pro ukládání 100GB a ne pro hello hello nominální velikost virtuálního pevného disku nebyl vytvořen s.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Azure Premium Storage
V duben 2015 zavedl Microsoft Azure Premium Storage. Storage úrovně Premium získali zavedl s cílem tooprovide hello:

* Lepší latence vstupně-výstupní operace.
* Lepší propustnost.
* Menší je jejich variabilita latence vstupně-výstupní operace.

K tomuto účelu byly zavedeny mnoho změn, které hello dva nejvýznamnějších jsou:

* Využití disků SSD ve hello uzlů úložiště Azure
* Nové mezipaměti pro čtení, kterou je zajištěna hello místní SSD Azure výpočetního uzlu

V opačném tooStandard úložiště, kde možnosti nezměnila závisí na velikosti hello hello disku (nebo VHD), Storage úrovně Premium aktuálně má tří kategorií jiný disk, které jsou uvedeny v tomto článku: <https://azure.microsoft.com/ ceny/podrobnosti / / nespravované disky úložiště nebo>

Uvidíte, zda jsou závislé na kategorie hello velikost disků hello nebo disku a disku propustnost nebo disku

Základní náklady v případě hello úložiště Premium Storage není svazek hello skutečná data uložená v takové disky, ale velikost kategorie hello takové disku, nezávisle na hello množství hello data, která je uložena v hello disku.

Také můžete vytvořit disky na Storage úrovně Premium, které nejsou přímo mapování do kategorií velikost hello vidět. To může být případ hello, zejména v případě, že kopírování disků ze standardního úložiště do úložiště úrovně Premium. V takových případech se provádí disku řešením mapování toohello další největší úložiště Premium.

Upozorňujeme, že pouze určité řadu virtuálních počítačů mohou těžit z výhod hello Azure Premium Storage. Od prosince 2015 jedná se o hello DS - a GS-series. Hello DS-series je v podstatě hello stejné jako D-series s výjimkou hello že DS-series má možnost toomount hello Storage úrovně Premium založené na virtuálních počítačích kromě toodisks, které jsou hostované v Azure Standard Storage. Samé platí pro porovnání G-series tooGS-series.

Pokud se odhlašuje hello součástí hello DS-series virtuálních počítačů v [v tomto článku (Linux)] [ virtual-machines-sizes-linux] a [v tomto článku (Windows)][virtual-machines-sizes-windows], je potřeba si uvědomit, který nejsou data svazku omezení tooPremium úložiště disky na hello členitost hello úroveň virtuálního počítače. Různé DS-series nebo GS-series virtuálních počítačů také mít různá omezení v namapoval toohello počet datových disků, které může být připojen. Tato omezení jsou popsané v článku hello zmíněné také. Ale v podstatě znamená, že pokud, například připojíte 32 tooa disky x P30 jeden virtuální počítač DS14 není získáte 32 x hello maximální propustnost P30 disku. Místo toho hello maximální propustnost na úrovni virtuálních počítačů, jak je uvedeno v propustnost dat omezení článku hello.

Další informace o Storage úrovně Premium naleznete zde: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="c55b2c6e-3ca1-4476-be16-16c81927550f"></a>Spravované disky
Spravované disky jsou nového typu prostředku v Azure Resource Manager, který lze použít místo virtuální pevné disky, které jsou uložené v účtech úložiště Azure. Spravované disky automaticky přiblížili hello sadu dostupnosti hello virtuálního počítače, které jsou připojené tooand proto zvýšení dostupnosti hello virtuálního počítače a služby hello, které jsou spuštěny na virtuálním počítači hello. Další informace najdete v tématu hello [článek s přehledem](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

Doporučujeme použít tooyou spravované disk, protože se zjednodušit hello nasazení a správu virtuálních počítačů.
SAP aktuálně podporuje jenom disky spravované Premium. Další informace najdete v tématu Poznámka SAP [1928533].

#### <a name="azure-storage-accounts"></a>Účty služby Azure Storage
Při nasazování služeb nebo virtuálních počítačů v Azure, mohou být uspořádány nasazení virtuální pevné disky a Image virtuálních počítačů v jednotek nazývaných účtech úložiště Azure. Při plánování nasazení služby Azure, budete potřebovat toocarefully zvažte omezení hello Azure. Na jedné straně hello je omezený počet účtů úložiště podle předplatného Azure. I když každý účet úložiště Azure pojme velký počet soubory virtuálního pevného disku, je na hello pevný limit celkový počet IOPS na účet úložiště. Při nasazování stovky SAP virtuálních počítačů se systémy databázového systému vytváření významné volání vstupně-výstupní operace, je doporučeno toodistribute vysoké databázového systému virtuální počítače IOPS mezi více účtů úložiště Azure. Musí být pozor není tooexceed hello aktuální limit účtů úložiště Azure za předplatné. Vzhledem k tomu, že úložiště je sice podstatná součást hello nasazení databáze systému SAP, tento koncept je podrobněji popsána v hello již odkazuje [Průvodce nasazením databázového systému][dbms-guide].

Další informace o účtech Azure Storage najdete v [v tomto článku][storage-scalability-targets]. Přečtení tohoto článku, zjistíte, že nejsou v omezení hello rozdíly mezi standardních účtech úložiště Azure a účty úložiště Premium. Hlavní rozdíly mezi nimi jsou hello objem dat, které můžou být uložené v rámci účtu úložiště. V standardní úložiště hello svazek je větší než Storage úrovně Premium rozsahem. Na hello druhé straně hello standardní účet úložiště je vážně omezená IOPS (viz sloupec 'Celkový počet požadavků'), zatímco hello prémiový účet úložiště Azure má žádné takové omezení. Se budeme zabývat podrobnosti a výsledky tyto rozdíly když hovoříte o hello nasazení SAP systémů, hlavně servery hello databázového systému.

V rámci účtu úložiště máte různé kontejnery toocreate hello možnost hello za účelem uspořádání a kategorizaci jiný virtuální pevné disky. Tyto kontejnery se obvykle používají k například samostatné virtuální pevné disky různé virtuální počítače. Neexistují žádné ovlivnit výkon při použití kontejner jenom jeden nebo více kontejnerů pod jeden účet úložiště Azure.

V rámci Azure následuje název disku VHD hello následující pojmenování připojení, které je tooprovide jedinečný název pro hello virtuálního pevného disku v rámci Azure:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Jako výše uvedených hello řetězec potřebuje toouniquely identifikovat hello virtuálního pevného disku, který je uložen v úložišti Azure.

### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Sítě Microsoft Azure
Microsoft Azure poskytuje síťovou infrastrukturu, která umožňuje mapování hello všechny scénáře, které chceme toorealize softwarem SAP. Hello možnosti jsou:

* Přístup z hello mimo přímo toohello virtuálních počítačů prostřednictvím Terminálová služba systému Windows nebo ssh/VNC
* Tooservices přístup a určité porty používané aplikacemi v rámci hello virtuální počítače
* Interní komunikaci a překlad mezi skupinou virtuální počítače nasazené jako virtuální počítače Azure
* Připojení mezi různými místy mezi místní sítí a hello síť Azure zákazníka
* Křížové oblast Azure nebo datových center připojení mezi sítěmi Azure

Další informace naleznete zde: <https://azure.microsoft.com/documentation/services/virtual-network/>

Existuje mnoho různých možnosti tooconfigure název a IP řešení v Azure. V tomto dokumentu jenom pro Cloud scénáře spoléhají na výchozí hello pomocí Azure DNS (v kontrastu toodefining služby vlastní DNS). Je také novou službu Azure DNS, který může být použit místo nastavení serveru DNS. Další informace naleznete v [v tomto článku] [ virtual-networks-manage-dns-in-vnet] a na [tuto stránku](https://azure.microsoft.com/services/dns/).

Pro scénáře mezi různými místy, jsme se spoléhat na hello fakt této hello místní AD a OpenLDAP/DNS bylo rozšířeno prostřednictvím sítě VPN nebo tooAzure privátní připojení. Pro určité scénáře podle postupu uvedeného v tomto poli může být nutné toohave repliku AD nebo OpenLDAP nainstalován v Azure.

Protože sítě a překladu je sice podstatná součást hello nasazení databáze systému SAP, tento koncept je podrobněji popsána v hello [Průvodce nasazením databázového systému][dbms-guide].

##### <a name="azure-virtual-networks"></a>Virtuální sítě Azure
Sestavením virtuální síť Azure, můžete definovat rozsah adres hello hello soukromé IP adresy přidělené funkce Azure protokolu DHCP. Ve scénářích mezi různými místy rozsah IP adres hello definované je pořád ještě přidělená použití protokolu DHCP v Azure. Ale překlad názvu domény se provádí místně (za předpokladu, že hello virtuální počítače jsou součástí místní domény) a proto může překládat adresy mimo jiné cloudové služby Azure.

Každý virtuální počítač v Azure potřebám toobe připojen tooa virtuální sítě.

Další podrobnosti naleznete v [v tomto článku] [ resource-groups-networking] a na [tuto stránku](https://azure.microsoft.com/documentation/services/virtual-network/).

[comment]: <> (MShermannd TODO nelze nalézt článek, který obsahuje téma OpenLDAP hello + ARM;)
[comment]: <> (MSSedusch < https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [!NOTE]
> Ve výchozím nastavení Jakmile je nasazen virtuální počítač nelze změnit hello konfigurace virtuální sítě. nastavení protokolu TCP/IP Hello musí být ponechány toohello Azure DHCP server. Výchozí chování je přiřazování dynamické IP adres.
>
>

může změnit Hello adresa MAC hello virtuální síťové karty, například v po změny velikosti a hello Windows nebo Linux hostovaný operační systém převezme hello nové síťové karty a automaticky používá DHCP tooassign hello služby DNS a IP adresy v tomto případě.

##### <a name="static-ip-assignment"></a>Přiřazování statických IP adres
Je možné tooassign pevné nebo vyhrazené IP adresy tooVMs ve virtuální síti Azure. Spuštěných virtuálních počítačů hello ve virtuální síti Azure otevře tooleverage skvělé možnost tuto funkci, pokud potřebné nebo požadované pro některé scénáře. přiřazení IP Hello zůstává v platnosti po hello existenci hello virtuální počítač, nezávisle na tom, zda je spuštěna hello virtuálního počítače nebo vypnutí. V důsledku toho je nutné tootake hello celkový počet virtuálních počítačů (virtuálních počítačů spuštěných a zastavených) v úvahu při definování hello rozsah IP adres pro hello virtuální sítě. Hello IP adresa zůstane přiřazená až do odstranění hello virtuálního počítače a jeho síťové rozhraní, nebo dokud hello přiřazená IP adresa získá zrušte znovu. Další informace najdete v tématu [v tomto článku][virtual-networks-static-private-ip-arm-pportal].

##### <a name="multiple-nics-per-vm"></a>Několik síťových adaptérů na virtuální počítač
Můžete definovat několik virtuálních síťových karet (vNIC) pro virtuální počítač Azure. Možnost toohave hello více vNICs můžete spouštět tooset až oddělení provoz sítě, kde, například přenosy klientů je směrován přes jednu vNIC a back-end provoz se směruje pomocí druhého vNIC. Závisí na typu hello existují různá omezení v virtuálního počítače, pokud o toohello počet vNICs. Přesné podrobnosti, funkce a omezení naleznete v těchto článcích:

* [Vytvoření virtuálního počítače s Windows s více síťovými kartami][virtual-networks-multiple-nics-windows]
* [Vytvoření virtuálního počítače s Linuxem s více síťovými kartami][virtual-networks-multiple-nics-linux]
* [Nasazení více seskupování virtuálních počítačů pomocí šablony][virtual-network-deploy-multinic-arm-template]
* [Nasazení více seskupování virtuálních počítačů pomocí prostředí PowerShell][virtual-network-deploy-multinic-arm-ps]
* [Nasazení více seskupování virtuálních počítačů pomocí hello rozhraní příkazového řádku Azure][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Připojení Site-to-Site
Mezi různými místy je virtuálních počítačích Azure a místními propojené s transparentní, trvalé připojení VPN. Je očekávané toobecome hello nejběžnější SAP vzor nasazení v Azure. Hello předpokladem je, že by měl transparentně fungovat provozních postupů a procesů pomocí instance SAP v Azure. To znamená, že jste by měl být schopný tooprint mimo tyto systémy a také použít hello tootransport SAP přenosu správu systému (TMS) se změní z vývojového systému v Azure tooa testovací systém, který je nasazený na místě. Další dokumentaci ohledně site-to-site lze nalézt v [v tomto článku][vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>Zařízení tunelové propojení VPN
V pořadí toocreate připojení site-to-site (místní data center tooAzure datového centra), musíte tooeither získat a nakonfigurovat zařízení VPN nebo použít směrování a vzdálený přístup (RRAS) zavedenou jako součást softwaru se systémem Windows Server 2012.

* [Vytvoření virtuální sítě s připojením VPN typu site-to-site pomocí prostředí PowerShell][vpn-gateway-create-site-to-site-rm-powershell]
* [Informace o zařízeních VPN pro připojení Site-to-Site VPN Gateway][vpn-gateway-about-vpn-devices]
* [Nejčastější dotazy k branám VPN][vpn-gateway-vpn-faq]

![Připojení Site-to-site mezi místními a Azure][planning-guide-figure-600]

Hello předchozí obrázek ukazuje, že dvě předplatná Azure podrozsahů na adresu IP, které jsou vyhrazené pro použití ve virtuálních sítích v Azure. připojení k Hello z hello místní, že je vytvořeno tooAzure sítě prostřednictvím sítě VPN.

#### <a name="point-to-site-vpn"></a>Point-to-Site VPN
Point-to-site VPN vyžaduje každý počítač tooconnect klienta s vlastním VPN do Azure. Pro scénáře SAP hello Těšíme se na není praktické připojení point-to-site. Proto žádné další odkazy jsou uvedeny připojení toopoint-to-site VPN.

Další informace naleznete zde
* [Konfigurace tooa připojení Point-to-Site virtuální sítě pomocí portálu Azure hello](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [Konfigurace tooa připojení Point-to-Site virtuální sítě pomocí prostředí PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

#### <a name="multi-site-vpn"></a>Více servery VPN
Azure nabízí také v současné době hello možnost toocreate připojení k VPN více lokalit pro jedno předplatné. V rámci jednoho předplatného byla dřív připojení omezené tooone site-to-site VPN. Toto omezení se rychle s připojeními VPN typu více lokalit pro v rámci jednoho předplatného. Díky tomu je možné tooleverage více než jedné oblasti Azure pro konkrétní předplatné prostřednictvím konfigurace mezi různými místy.

Další dokumentaci, najdete v tématu [v tomto článku][vpn-gateway-create-site-to-site-rm-powershell]

[comment]: <> (MShermannd TODO nalezen žádný odkaz doku ARM)

#### <a name="vnet-toovnet-connection"></a>Virtuální síť tooVNet připojení
Prostřednictvím sítě VPN více lokalit, je nutné tooconfigure samostatné virtuální sítě Azure v jednotlivých oblastech hello. Máte ale velmi často hello požadavek, který by měl hello softwarové součásti v různých oblastech hello vzájemně komunikovat. V ideálním případě by neměl tato komunikace směrovat z jedné oblasti Azure tooon místní a že toohello jiné oblasti Azure. tooshortcut, Azure nabízí možnost tooconfigure hello připojení z jedné virtuální sítě Azure v jedné oblasti tooanother, které Azure Virtual Network hostované v jiné oblasti. Tato funkce je volána připojení VNet-to-VNet. Další podrobnosti o této funkci naleznete zde: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-tooazure--expressroute"></a>Privátní připojení tooAzure – ExpressRoute
Microsoft Azure ExpressRoute umožňuje vytvoření hello privátní připojení mezi datových center Azure a místní infrastruktury zákazníka buď hello nebo v prostředí ve společném umístění. ExpressRoute nabízí různé MPLS poskytovatelé sítě VPN (přepínáním paketů) nebo jiní poskytovatelé služeb sítě. Připojení ExpressRoute se nepřenášejí prostřednictvím hello veřejného Internetu. Připojení ExpressRoute nabízejí vyšší zabezpečení, spolehlivost další prostřednictvím více paralelních okruhů, vyšší rychlost a nižší latenci než Typická připojení přes hello Internet.

Najdete další informace o Azure ExpressRoute a nabídky tady:

* <https://Azure.microsoft.com/documentation/Services/expressroute/>
* <https://Azure.microsoft.com/pricing/details/expressroute/>
* <https://Azure.microsoft.com/documentation/articles/expressroute-faqs/>

Express Route umožňuje víc předplatných Azure prostřednictvím jeden okruh ExpressRoute, jak je uvedeno v tomto poli

* <https://Azure.microsoft.com/documentation/articles/expressroute-howto-linkvnet-arm/>
* <https://Azure.microsoft.com/documentation/articles/expressroute-howto-Circuit-arm/>

#### <a name="forced-tunneling-in-case-of-cross-premises"></a>Vynucené tunelování v případě mezi různými místy
Pro virtuální počítače připojení k doménám místní prostřednictvím site-to-site, point-to-site nebo ExpressRoute budete potřebovat toomake hello nastavení internetového proxy serveru jsou získávání nasazení pro všechny uživatele hello ve také tyto virtuální počítače. Ve výchozím nastavení, software na těchto virtuálních počítačů nebo uživatelů používajících tooaccess hello prohlížeče, nebude projít hello společnosti proxy Internetu, ale by připojení přímo přes Azure toohello Internetu. Ale i hello nastavení proxy serveru není přenosem hello toodirect 100 % řešení prostřednictvím proxy serveru společnosti hello vzhledem k tomu, že odpovídá toocheck softwaru a služeb pro proxy server hello. Pokud software na hello virtuálního počítače není učinit nebo správce manipuluje hello nastavení, můžete toohello provoz sítě Internet znovu detoured přímo přes Azure toohello Internetu.

V pořadí tooavoid to, můžete konfigurovat vynucené tunelování s připojením site-to-site mezi místními a Azure. Hello podrobný popis funkce vynucené tunelování hello je publikována zde <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/>

Vynucené tunelové připojení s ExpressRoute je povoleno zákazníci inzeruje výchozí trasu přes hello ExpressRoute BGP pro relace partnerského vztahu.

#### <a name="summary-of-azure-networking"></a>Shrnutí sítě Azure
Tato kapitola obsahuje mnoho důležitých bodů o sítí Azure. Zde je souhrn hello hlavní body:

* Virtuální sítě Azure umožňuje tooset hello sítě podle vlastních potřeb tooyour
* Virtuální sítě Azure lze využít tooassign IP adresy rozsahů tooVMs nebo přiřadit pevné IP adresy tooVMs
* tooset Site-To-Site nebo Point-To-Site připojení, je nejprve nutné toocreate virtuální síť Azure
* Po nasazení virtuálního počítače již není možné toochange hello, virtuální sítě přiřadit toohello virtuálních počítačů

### <a name="quotas-in-azure-virtual-machine-services"></a>Kvóty pro služby Azure virtuálního počítače
Budeme potřebovat toobe vymazat o hello fakt této hello úložiště a síťové infrastruktury jsou sdílena mezi virtuální počítače se systémem řady služeb v hello infrastruktury Azure. A jako v případě hello zákazníka vlastní datových centrech předimenzování některých prostředků infrastruktury hello trvá stupeň tooa místní. Hello platforma Microsoft Azure používá disk, procesoru, sítě a další spotřeby prostředků hello toolimit kvóty a výkonu toopreserve konzistentní a deterministický.  Hello různé typy virtuálních počítačů (A5, A6 atd.) mají různé kvóty pro hello počet disků, procesoru, paměti RAM a sítě.

> [!NOTE]
> Prostředky procesoru a paměti hello typů virtuálních počítačů nepodporuje SAP jsou předběžně přidělených na uzly hostitele hello. To znamená, že po nasazení virtuálního počítače hello hello prostředky na hostiteli hello jsou k dispozici podle definice hello typ virtuálního počítače.
>
>

Je třeba zvážit při plánování a změna velikosti SAP na řešení Azure hello kvóty pro každou velikost virtuálního počítače. kvóty Hello virtuálních počítačů jsou popsány [zde (Linux)] [ virtual-machines-sizes-linux] a [zde (Windows)][virtual-machines-sizes-windows].

Hello kvót popsané představují hello teoretický maximální hodnoty.  Hello limit IOPS na jeden disk může dosáhnout s malé IOs (8kb), ale pravděpodobně nemusí být dosaženo s velké IOs (1 Mb).  Hello IOPS limit se vynucuje u hello členitost jediný disk.

Jako toodecide hrubý rozhodovací strom zda se systému SAP zapadá do služby Azure virtuálního počítače a jeho funkce nebo zda stávajícího systému potřebuje toobe nakonfigurována jinak než v systému hello toodeploy pořadí v Azure, hello rozhodovací strom níže lze použít:

![Rozhodovací strom toodecide možnost toodeploy SAP v Azure][planning-guide-figure-700]

**Krok 1**: hello nejdůležitější informace toostart s je hello protokoly SAP požadavkem pro daný systém SAP. Hello protokoly SAP požadavky nutné toobe oddělené do části databázového systému hello a část aplikace hello SAP, i když hello SAP systému je již nasazen na místě v konfiguraci vrstvy 2. Pro existující systémy hello protokoly SAP související toohello hardwaru používá často lze určit nebo odhadované založené na existující SAP srovnávacích testů. výsledky Hello naleznete zde: <http://global.sap.com/campaigns/benchmark/index.epx>.
Pro nově nasazené systémy SAP by měl prošli nastavení velikosti cvičení, která by měla určí požadavky protokoly SAP hello hello systému.
Viz také tomto blogu a přiložený dokument pro SAP velikosti v Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**Krok 2**: stávajících systémů hello svazku vstupně-výstupních operací a by se mělo měřit vstupně-výstupních operací za sekundu na hello databázového systému serveru. Pro nově plánované systémy hello dimenzování cvičení pro nový systém hello také získat hrubý nápady hello vstupně-výstupní požadavky hello straně databázového systému. Pokud si jisti, musíte nakonec tooconduct testování konceptu.

**Krok 3**: porovnání hello protokoly SAP požadavek hello databázového systému server s protokoly SAP hello hello jiný virtuální počítač typy Azure poskytuje. Hello informace o protokoly SAP hello různých typů virtuální počítač Azure je popsána v Poznámka SAP [1928533]. fokus Hello by měla být na hello databázového systému virtuální počítač nejdřív vzhledem k tomu, že vrstva databáze hello je vrstva hello SAP NetWeaver systému, které není horizontální rozšíření kapacity v hello většinu nasazení. Naproti tomu hello SAP aplikační vrstvu lze škálovat. Pokud žádná z hello SAP podporované typy virtuálního počítače Azure můžete poskytovat hello požadované protokoly SAP, hello zatížení systému SAP hello plánované nelze spustit v Azure. Buď musíte toodeploy hello systému místní nebo potřebujete toochange hello zatížení svazku pro systém hello.

**Krok 4**: jak se píše [zde (Linux)] [ virtual-machines-sizes-linux] a [zde (Windows)][virtual-machines-sizes-windows], Azure vynucuje kvóty IOPS na disk nezávislé zda používat úložiště úrovně Standard nebo Premium Storage. Závisí na hello typ virtuálního počítače, hello počet datových disků, které může být připojen se liší. V důsledku toho můžete vypočítat maximální IOPS číslo, které lze dosáhnout s jednotlivými hello různé typy virtuálních počítačů. Závisí na hello rozložení souboru databáze, vám může rozkládají disky toobecome jeden svazek v hello hostovaný operační systém. Ale pokud hello aktuální IOPS svazku nasazené systému SAP překračuje omezení hello vypočítat hello největší typ virtuálního počítače Azure a pokud neexistuje žádné prvního toocompensate s více paměti, hello zatížení hello systému SAP ovlivněné můžou být vážně. V takových případech můžete dosáhl bodu, kde není vhodné nasazovat hello systému v Azure.

**Krok 5**: hlavně v systémech SAP, které jsou nasazené na místě v konfiguracích vrstvě 2, hello pravděpodobné že hello systému může být nutné toobe nakonfigurovaný v Azure v konfiguraci vrstvy 3. V tomto kroku je nutné toocheck, zda je součást v hello SAP aplikační vrstvu, která nelze škálovat na více systémů a který by začlenit do hello procesoru a paměti, že prostředky hello různé typy nabídka virtuálního počítače Azure. Pokud skutečně existuje takový komponentu, hello SAP systému a jeho úlohy nelze nasadit do Azure. Ale pokud můžete škálovat součásti aplikace hello SAP do více virtuálních počítačů Azure, hello systém lze nasadit do Azure.

**Krok 6**: Pokud hello databázového systému a součásti vrstvy aplikace SAP lze spustit ve virtuálních počítačích Azure, je hello konfiguraci toobe definované s ohledem na:

* Počet virtuálních počítačů Azure
* Typy virtuálních počítačů pro jednotlivé součásti hello
* Počet virtuálních pevných disků v tooprovide databázového systému virtuálního počítače dostatek IOPS

## <a name="managing-azure-assets"></a>Správa prostředků Azure
### <a name="azure-portal"></a>Azure Portal
Hello portál Azure je jedním ze tří rozhraní toomanage Azure nasazení virtuálních počítačů. úlohy Hello základní správy, jako je nasazení virtuálních počítačů z bitové kopie, lze provést prostřednictvím hello portálu Azure. Kromě toho hello vytvoření účtů úložiště, virtuální sítě, a další Azure součásti jsou také úlohy hello portálu Azure může zpracovat velmi dobře. Funkce jako nahrávat virtuální pevné disky z místní tooAzure nebo kopírování virtuálního pevného disku v rámci Azure jsou však úlohy, které vyžadují nástroje třetích stran nebo správu pomocí prostředí PowerShell nebo rozhraní příkazového řádku.

![Portál Microsoft Azure – Přehled virtuálních počítačů][planning-guide-figure-800]

[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Úlohy správy a konfigurace pro instanci virtuálního počítače hello je možné z v rámci hello portálu Azure.

Kromě toho, restartování a vypínání virtuálního počítače můžete také připojit, odpojit a vytvořit datových disků pro hello instanci virtuálního počítače, instanci hello toocapture pro přípravy bitové kopie a nakonfigurujte hello velikost hello instanci virtuálního počítače.

Hello portál Azure poskytuje základní funkce toodeploy a konfigurace virtuálních počítačů a mnoha dalším službám Azure. Ale ne všechny dostupné funkce je předmětem hello portálu Azure. Hello portálu Azure není možné tooperform úkoly, jako je:

* Odesílání tooAzure virtuálních pevných disků
* Kopírování virtuálních počítačů

[comment]: <> (MShermannd TODO, co o automatizaci služby pro SAP virtuální počítače?)
[comment]: <> (MSSedusch nasazení více virtuálních počítačů os mezitím možné)
[comment]: <> (Jakýkoli typ automatizace týkající se nasazení také MSSedusch není možné pomocí hello portálu Azure. Úlohy, jako je skriptované nasazení více virtuálních počítačů není možná prostřednictvím hello portálu Azure.)

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>Správa pomocí rutin prostředí PowerShell Microsoft Azure
Prostředí Windows PowerShell je výkonný a rozšiřitelný rámec, který má byla široce přijat zákazníky, kteří instalují velký počet systémů v Azure. Po instalaci hello rutin prostředí PowerShell na plochu, přenosných počítačů nebo vyhrazené správy stanice můžete hello rutiny prostředí PowerShell spustit vzdáleně.

Hello proces tooenable místní desktop nebo přenosný počítač pro hello použití rutin prostředí Azure PowerShell a jak tooconfigure pro použití s hello hello Azure odběry je popsáno v [v tomto článku][powershell-install-configure].

Další podrobné kroky na tom, jak tooinstall, aktualizovat a nakonfigurovat rutin prostředí Azure PowerShell hello najdete také v [tato kapitola hello Průvodce nasazením][deployment-guide-4.1].

Zkušeností zákazníků, pokud byl, prostředí PowerShell (PS) je určitě hello výkonnější nástroj toodeploy virtuální počítače a toocreate vlastní kroky v hello nasazení virtuálních počítačů. Všechny spuštěné instance SAP v Azure zákazníků hello používají PS rutiny toosupplement úlohy správy nemají v hello portál Azure nebo jsou i pomocí rutin PS výhradně toomanage jejich nasazeních v Azure. Vzhledem k tomu, že sdílené složky rutiny specifické pro Azure hello hello stejné zásady vytváření názvů jako hello více než 2000 rutiny související s Windows, je snadno úloha pro Windows správci tooleverage tyto rutiny.

Podívejte se sem příklad: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[comment]: <> (MShermannd TODO popisují nový příkaz rozhraní příkazového řádku při testování)
Nasazení hello rozšíření monitorování Azure pro SAP (naleznete v kapitole [řešením pro monitorování Azure pro SAP] [ planning-guide-9.1] v tomto dokumentu) je možné pouze prostřednictvím prostředí PowerShell nebo rozhraní příkazového řádku. Proto je povinné tooset provozu a konfigurace prostředí PowerShell nebo rozhraní příkazového řádku pro nasazování nebo správu systém SAP NetWeaver v Azure.  

Jak Azure poskytuje další funkce, budou nové rutiny PS toobe přidat, který vyžaduje aktualizace rutin hello. Proto má smysl toocheck hello stáhnout Azure lokality alespoň jednou hello měsíc <https://azure.microsoft.com/downloads/> pro novou verzi hello rutiny. nainstaluje se nová verze Hello nad hello starší verze.

Obecnější seznam souvisejících s Azure PowerShell příkazy najdete tady: <https://docs.microsoft.com/powershell/azure/overview>.

### <a name="management-via-microsoft-azure-cli-commands"></a>Správu prostřednictvím Microsoft rozhraní příkazového řádku Azure
Pro zákazníky, kteří se používá Linux a chcete toomanage Azure nemusí být prostředků Powershell možnost. Společnost Microsoft nabízí jako alternativu rozhraní příkazového řádku Azure.
Hello rozhraní příkazového řádku Azure poskytuje sadu softwaru open source, příkazy a platformy pro práci s hello platformě Azure. Hello rozhraní příkazového řádku Azure poskytuje mnohem hello stejné funkce najít v hello portálu Azure.

Informace o instalaci, konfiguraci a jak toouse rozhraní příkazového řádku příkazy tooaccomplish Azure úlohy v tématu

* [Nainstalujte hello rozhraní příkazového řádku Azure][xplat-cli]
* [Nasadit a spravovat virtuální počítače pomocí šablony Azure Resource Manager a hello rozhraní příkazového řádku Azure] [.. /.. / linux/create-ssh-secured-vm-from-template.md]
* [Hello použití Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru][xplat-cli-azure-resource-manager]

Také číst kapitoly [rozhraní příkazového řádku Azure pro virtuální počítače s Linuxem] [ deployment-guide-4.5.2] v hello [Průvodce nasazením] [ planning-guide] na tom, jak toouse rozhraní příkazového řádku Azure toodeploy hello Azure Monitoring Rozšíření pro SAP.

## <a name="different-ways-toodeploy-vms-for-sap-in-azure"></a>Různé způsoby toodeploy virtuálních počítačů pro SAP v Azure
V této kapitoly zjistíte hello různé způsoby toodeploy virtuálního počítače v Azure. V této kapitole jsou popsané postupy další přípravy, jakož i zpracování virtuální pevné disky a virtuální počítače v Azure.

### <a name="deployment-of-vms-for-sap"></a>Nasazení virtuálních počítačů pro SAP
Microsoft Azure nabízí několik způsobů toodeploy virtuální počítače a přidružené disky. Proto je velmi důležité toounderstand hello rozdíly, od přípravy hello virtuálních počítačů se můžou lišit v závislosti na metodě hello nasazení. Obecně platí jsme si prohlédněte hello následující scénáře:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Přesunutí virtuálního počítače z místní tooAzure s diskem zobecněn
Máte v plánu toomove konkrétního systému SAP z místní tooAzure. To lze provést tím, že nahrajete hello virtuálního pevného disku, který obsahuje hello operačního systému, hello SAP binární soubory a binární soubory databázového systému plus hello virtuální pevné disky s hello protokolu a data souborů tooAzure hello databázového systému. Na rozdíl od příliš[scénář #2 níže][planning-guide-5.1.2], zachovat hello název hostitele, identifikátor SID SAP, a SAP uživatelských účtů v hello virtuálního počítače Azure, jak byla nakonfigurována v místním prostředí hello. Proto generalizací hello bitové kopie není nutné. Najdete v kapitolách [přípravy pro přesun virtuálního počítače z místní tooAzure s diskem zobecněn] [ planning-guide-5.2.1] tohoto dokumentu pro místní přípravné kroky a odesílání tooAzure zobecněný virtuální počítače nebo virtuální pevné disky. Čtení kapitoly [scénář 3: přesunutí virtuálního počítače z místního virtuálního pevného disku není zobecněný Azure pomocí SAP] [ deployment-guide-3.4] v hello [Průvodce nasazením] [ deployment-guide] podrobný postup nasazení takové bitovou kopii v Azure.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Nasazení virtuálního počítače s bitovou kopii zákaznické
Z důvodu toospecific oprava požadavkům vaší verzí operačního systému nebo databázového systému nemusí hello zadané bitové kopie v Azure Marketplace hello podle vašich potřeb. Proto může být nutné toocreate virtuálního počítače pomocí vlastní "privátní" image operačního systému nebo databázového systému virtuálního počítače, které mohou být nasazeny několikrát později. tooprepare "privátní" image duplikovaná hello následující položky mají toobe považována za:

- - -
> ![Windows][Logo_Windows] Windows
>
> Zobrazte další podrobnosti zde: <https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed> nastavení systému Windows hello (jako jsou SID systému Windows a název hostitele) musí být zobecněn místními hello jejich abstrahované Virtuální počítač pomocí příkazu sysprep hello.
>
>
> ![Linux][Logo_Linux] Linux
>
> Postupujte podle kroků hello popsaných v těchto článcích pro [SUSE][virtual-machines-linux-create-upload-vhd-suse], [Red Hat][virtual-machines-linux-redhat-create-upload-vhd], nebo [Oracle Linux] [ virtual-machines-linux-create-upload-vhd-oracle], tooAzure nahrán tooprepare toobe virtuálního pevného disku.
>
>

- - -
Pokud jste již nainstalovali SAP obsah v místní virtuální počítač (hlavně u systémy vrstvě 2), můžete upravit nastavení systému SAP hello po nasazení hello hello virtuálního počítače Azure pomocí hello instance přejmenovat postup nepodporuje hello zřizování softwaru SAP Správce (Poznámka SAP [1619720]). Najdete v kapitolách [přípravy pro nasazení virtuálního počítače s bitovou kopii zákaznické pro SAP] [ planning-guide-5.2.2] a [odesílání virtuálního pevného disku z místní tooAzure] [ planning-guide-5.3.2]tohoto dokumentu pro místní přípravné kroky a odesílání zobecněný tooAzure virtuálních počítačů. Čtení kapitoly [scénář 2: nasazení virtuálního počítače s vlastní image pro SAP] [ deployment-guide-3.3] v hello [Průvodce nasazením] [ deployment-guide] podrobné kroky nasazení takové bitovou kopii v Azure.

#### <a name="deploying-a-vm-out-of-hello-azure-marketplace"></a>Nasazení virtuálních počítačů mimo hello Azure Marketplace
Chcete toouse společnosti Microsoft nebo třetích stran zadaná image virtuálního počítače z Azure Marketplace toodeploy hello virtuálního počítače. Po nasazení virtuálního počítače v Azure provedením hello stejné pokyny a nástroje pro tooinstall hello SAP softwaru nebo databázového systému uvnitř virtuálního počítače jako byste to udělali v místním prostředí. Podrobnější popis nasazení, získáte v kapitole [scénář 1: nasazení virtuálního počítače mimo hello Azure Marketplace pro SAP] [ deployment-guide-3.2] v hello [Průvodce nasazením] [ deployment-guide].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Příprava na Azure virtuálních počítačů s SAP
Před nahráním virtuálních počítačů do Azure je nutné toomake se, že hello virtuálních počítačů a virtuálních pevných disků splnit určité požadavky. Existují malé rozdíly v závislosti na metodě nasazení hello, který se používá.

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Příprava pro přesun virtuálního počítače z místní tooAzure s diskem zobecněn
Běžnou metodu nasazení je toomove existující virtuální počítač, na kterém běží systém SAP z místní tooAzure. Virtuální počítač a hello systému SAP v hello virtuální počítač by měl stačí spustit v Azure pomocí hello stejný název hostitele a velmi pravděpodobně hello stejný identifikátor SID SAP. V takovém případě by neměl být zobecněn hello hostovaného operačního systému virtuálního počítače pro více nasazení. Pokud tu rozšířeno hello do místní sítě do Azure (naleznete v kapitole [mezi různými místy - nasazení jedné nebo více virtuálních počítačů SAP do Azure s hello požadavek se plně integrován do místní sítě hello] [ planning-guide-2.2] v tomto dokumentu), pak i hello stejné účty domény může být použit hello virtuálního počítače jako těch, které byly používány před místně.

Požadavky při přípravě vlastní disku virtuálního počítače Azure jsou:

* Původně hello virtuálního pevného disku obsahující hello operačního systému může mít maximální velikost 127GB jenom. Toto omezení získali odstraňují u hello konci března 2015. Nyní hello virtuální pevný disk obsahující hello operačního systému může být až too1TB velikost jako jakékoli jiné úložiště Azure také hostované virtuální pevný disk.
* Musí být toobe v hello pevné formátu virtuálního pevného disku. Dynamické virtuální pevné disky nebo virtuální pevné disky ve formátu VHDx se ještě nepodporují v Azure. Dynamické virtuální pevné disky bude převedený toostatic virtuálních pevných disků při nahrávání hello virtuální pevný disk pomocí rutiny prostředí PowerShell nebo rozhraní příkazového řádku
* Virtuální pevné disky, které jsou připojené toohello virtuálních počítačů a by měl být připojené znovu toobe nutné virtuální počítač Azure toohello v také pevném formátu virtuálního pevného disku. Přečtěte si prosím [v tomto článku (Linux)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) a [v tomto článku (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) pro omezení velikosti datových disků. Dynamické virtuální pevné disky bude převedený toostatic virtuálních pevných disků při nahrávání hello virtuální pevný disk pomocí rutiny prostředí PowerShell nebo rozhraní příkazového řádku
* Přidáte, může být použit jiný místní účet s oprávněními správce, které mohou být využívána podpory společnosti Microsoft nebo které lze přiřadit jako kontext pro služby a aplikace toorun v dokud hello, který je nasazen virtuální počítač a více příslušné uživatele.
* Pro případ hello použití scénáři jenom pro Cloud nasazení (naleznete v kapitole [jenom pro Cloud - nasazení virtuálních počítačů do Azure bez závislosti na hello místní síti zákazníka] [ planning-guide-2.1] tohoto zdokumentujte) v kombinaci s Tato metoda nasazení, domény, které účty nemusí fungovat po hello disku Azure je nasazené v Azure. To platí hlavně pro účty, které jsou používané toorun služby, jako je aplikace hello databázového systému nebo SAP. Proto nutné tooreplace tyto účty domény s účty místní počítač a odstraňte hello místní doménové účty v hello virtuálních počítačů. Zachování místní domény uživatele v hello image virtuálního počítače se nejedná o problém při hello virtuální počítač nasazen v hello mezi různými místy scénář popsaný v kapitole [mezi různými místy - nasazení jedné nebo více virtuálních počítačů SAP do Azure s hello požadavek se plně integrován do místní sítě hello] [ planning-guide-2.2] v tomto dokumentu.
* Pokud byly použity účty domény jako databázového systému přihlášení nebo uživatelé při spuštění hello systému místně a tyto virtuální počítače mají toobe nasazené v čistě cloudové scénáře, uživatelé domény hello musí toobe odstranit. Je nutné toomake ujistili, že hello místního správce a jiný virtuální počítač místní uživatel je přidán jako přihlášení uživatele do hello databázového systému jako správci.
* Přidejte další místní účty, jak těch, které mohou být potřebné pro konkrétní nasazení scénář hello.

- - -
> ![Windows][Logo_Windows] Windows
>
> V tomto scénáři žádné generalizace (sysprep) hello virtuálního počítače je požadovaná tooupload a nasaďte hello virtuálního počítače na platformě Azure.
> Ujistěte se, že jednotku, kterou nepoužívá D:\.
> Nastavení automatického připojení disku pro připojené disky, jak je popsáno v kapitole [nastavení automatického připojení pro připojené disky] [ planning-guide-5.5.3] v tomto dokumentu.
>
> ![Linux][Logo_Linux] Linux
>
> V tomto scénáři žádné generalizace (příkaz waagent-deprovision) hello virtuálního počítače je požadovaná tooupload a nasadit hello virtuálního počítače na platformě Azure.
> Ujistěte se, zda se nepoužívá/mnt nebo prostředků a že jsou všechny disky připojené přes uuid. Pro disk hello operačního systému ověřte, že hello zaváděcí program pro spouštění položku také změna připojení na základě uuid hello.
>
>

- - -
#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>Příprava pro nasazení virtuálního počítače s bitovou kopii zákaznické pro SAP
Soubory virtuálního pevného disku, které obsahují zobecněný operačního systému jsou uloženy v kontejnery v účtech úložiště Azure nebo jako spravované diskové Image. Můžete nasadit nový virtuální počítač z takových bitové kopie odkazující na hello virtuálního pevného disku nebo spravovat bitové kopie disku jako zdroj v souborech šablony nasazení, jak je popsáno v kapitole [scénář 2: nasazení virtuálního počítače s vlastní image pro SAP] [ deployment-guide-3.3]z hello [Průvodce nasazením][deployment-guide].

Požadavky při přípravě vlastní Image virtuálního počítače Azure jsou:

* Původně hello virtuálního pevného disku obsahující hello operačního systému může mít maximální velikost 127GB jenom. Toto omezení získali odstraňují u hello konci března 2015. Nyní hello virtuální pevný disk obsahující hello operačního systému může být až too1TB velikost jako jakékoli jiné úložiště Azure také hostované virtuální pevný disk.
* Musí být toobe v hello pevné formátu virtuálního pevného disku. Dynamické virtuální pevné disky nebo virtuální pevné disky ve formátu VHDx se ještě nepodporují v Azure. Dynamické virtuální pevné disky bude převedený toostatic virtuálních pevných disků při nahrávání hello virtuální pevný disk pomocí rutiny prostředí PowerShell nebo rozhraní příkazového řádku
* Virtuální pevné disky, které jsou připojené toohello virtuálních počítačů a by měl být připojené znovu toobe nutné virtuální počítač Azure toohello v také pevném formátu virtuálního pevného disku. Přečtěte si prosím [v tomto článku (Linux)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) a [v tomto článku (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) pro omezení velikosti datových disků. Dynamické virtuální pevné disky bude převedený toostatic virtuálních pevných disků při nahrávání hello virtuální pevný disk pomocí rutiny prostředí PowerShell nebo rozhraní příkazového řádku
* Vzhledem k tomu, že všechny uživatele domény registrovaný jako uživatelé v hello virtuální počítač nebude existovat ve scénáři jenom pro Cloud hello (naleznete v kapitole [jenom pro Cloud - nasazení virtuálních počítačů do Azure bez závislosti na hello místní síti zákazníka] [ planning-guide-2.1] tohoto dokumentu), služby pomocí takové domény účty nemusí fungovat po hello obrázku je nasazena v Azure. To platí hlavně pro účty, které jsou používané toorun služby, jako je databázového systému nebo SAP aplikací. Proto nutné tooreplace tyto účty domény s účty místní počítač a odstraňte hello místní doménové účty v hello virtuálních počítačů. Zachování místní domény uživatele v hello image virtuálního počítače nemusí být problém, když virtuální počítač nasazen v hello hello mezi různými místy scénář popsaný v kapitole [mezi různými místy - nasazení jedné nebo více virtuálních počítačů SAP do Azure s hello požadavek Probíhá plně integrován do místní sítě hello] [ planning-guide-2.2] v tomto dokumentu.
* Přidáte, může být použit jiný místní účet s oprávněními správce, které mohou být využívána podpory společnosti Microsoft v vyšetřování problém nebo které lze přiřadit jako kontext pro služby a aplikace toorun v dokud hello, který je nasazen virtuální počítač a více příslušné uživatele.
* V nasazení jenom pro Cloud a kde účty domény byly použity jako databázového systému přihlášení nebo hello uživatelům při spuštění systému na místě měli byste odstranit hello uživatele domény. Je nutné toomake jistotu, že hello místního správce a jiný virtuální počítač místní uživatel je přidán jako přihlášení nebo uživatel hello databázového systému jako správci.
* Přidejte další místní účty, jak těch, které mohou být potřebné pro konkrétní nasazení scénář hello.
* Pokud hello bitová kopie obsahuje instalace SAP NetWeaver a přejmenování hello název hostitele z původní název hello v hello bod hello nasazení Azure je pravděpodobné, je doporučené toocopy hello nejnovější verze DVD Manager hello zřizování softwaru SAP do Šablona Hello. To vám umožní tooeasily použití hello SAP zadaný přejmenování funkce tooadapt hello změnil název hostitele nebo změnu hello SID hello systému SAP v rámci hello nasazení image virtuálního počítače, jakmile se spustí novou kopii.

- - -
> ![Windows][Logo_Windows] Windows
>
> Ujistěte se, že jednotku D:\ není použít automatického připojení disku sady pro připojené disky, jak je popsáno v kapitole [nastavení automatického připojení pro připojené disky] [ planning-guide-5.5.3] v tomto dokumentu.
>
> ![Linux][Logo_Linux] Linux
>
> Ujistěte se, zda se nepoužívá/mnt nebo prostředků a že jsou všechny disky připojené přes uuid. Pro disk hello operačního systému zkontrolujte, zda položka zaváděcí program pro spouštění hello také odráží připojení na základě uuid hello.
>
>

- - -
* SAP grafickým uživatelským rozhraním (pro správu a instalační program pro účely) může být předem nainstalován v takové šablonu.
* Další software hello nezbytné toorun, virtuální počítače úspěšně ve scénářích mezi různými místy lze nainstalovat, dokud tento software může spolupracovat s hello přejmenovat z hello virtuálních počítačů.

Pokud hello virtuálního počítače je připravená dostatečně toobe obecné a nakonec nezávisle účtů nebo uživatele není k dispozici v hello cíleného scénář nasazení Azure, se provádí hello poslední přípravy krok generalizací takové image.

##### <a name="generalizing-a-vm"></a>Generalizací virtuálního počítače
- - -
> ![Windows][Logo_Windows] Windows
>
> posledním krokem Hello je toolog v tooa virtuálních počítačů s účtem správce. Otevřete příkazové okno Windows jako 'správce. Přejděte too%windir%\windows\system32\sysprep a provést sysprep.exe.
> Zobrazí se okno malé. Je důležité toocheck hello 'Generalizace' možnost (výchozí hello je vypnuta) a změnit hello možnost vypnutí z výchozí hodnoty "Restartovat" too'Shutdown'. Tento postup předpokládá, že procesu sysprep hello je spustit místně v hello hostovaného operačního systému virtuálního počítače.
> Pokud chcete tooperform hello postup v případě virtuálních počítačů už běží v Azure, postupujte podle kroků hello popsaných v [v tomto článku](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource).
>
> ![Linux][Logo_Linux] Linux
>
> [Jak toocapture toouse virtuálního počítače Linux jako šablony Resource Manageru][capture-image-linux-step-2-create-vm-image]
>
>

- - -
### <a name="transferring-vms-and-vhds-between-on-premises-tooazure"></a>Přenos mezi místními tooAzure virtuálních počítačů a virtuálních pevných disků
Vzhledem k tomu, že odesílání tooAzure Image a disky virtuálního počítače není možné prostřednictvím hello portálu Azure, musíte toouse rutin prostředí Azure PowerShell nebo rozhraní příkazového řádku. Další možností je použití hello nástroje hello 'AzCopy'. Nástroj Hello můžete zkopírovat virtuální pevné disky mezi místními a Azure (v obou směrech). Je taky zkopírovat virtuální pevné disky mezi oblastmi Azure. Přečtěte si [této dokumentace] [ storage-use-azcopy] pro stažení a použití nástroje AzCopy.

Třetí alternativní by toouse různé orientované na grafickém uživatelském rozhraní nástroje třetích stran. Ale ujistěte, že tyto nástroje jsou podpora objekty BLOB stránky Azure. Pro naše účely potřebujeme toouse objekt Blob stránky Azure ukládání (hello rozdíly jsou popsány zde: <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>). Hello nástroje poskytované subsystémem pro Azure jsou také velmi efektivní v kompresi hello virtuálních počítačů a virtuálních pevných disků, které vyžadují toobe nahrát. To je důležité, protože této efektivity v komprese snižuje dobu hello nahrávání, (který se liší přesto podle hello nahrávání odkaz toohello cílem Internetu hello místní zařízením a hello nasazení Azure oblast). Je správného předpokládá, že nahrávat virtuálního počítače nebo virtuální pevný disk z Evropského umístění toohello Azure dat na základě USA, Center bude trvat déle než odesílání hello virtuální počítače nebo virtuální pevné disky toohello Evropského Azure datových center.

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Nahrání virtuálního pevného disku z místní tooAzure
tooupload existující virtuální počítač nebo virtuální pevný disk z hello místní sítí virtuálních počítačů nebo virtuální pevný disk musí toomeet hello požadavky, jak je uvedeno v kapitole [přípravy pro přesun virtuálního počítače z místní tooAzure s diskem zobecněn] [ planning-guide-5.2.1] tohoto dokumentu.

Tyto virtuální počítač nemusí toobe zobecněn a v hello stavu a tvar, který se má po vypnutí hello místní straně je možné uložit. Hello totéž platí pro další virtuální pevné disky, které neobsahují žádný operační systém.

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Nahrání virtuálního pevného disku a jejich zpřístupnění diskem Azure
V takovém případě jsme má tooupload disku VHD, s nebo bez operační systém v něm a ji tooa virtuální počítač jako datový disk připojit nebo ho použít jako disk s operačním systémem. Jedná se o vícekrokový proces

**Powershell**

* Přihlaste se tooyour předplatné s *Login-AzureRmAccount*
* Nastavte předplatné hello váš kontext s *Set-AzureRmContext* a parametr ID předplatného nebo Název_předplatného - <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* Nahrát hello virtuálního pevného disku s *přidat AzureRmVhd* najdete v části tooan účet úložiště Azure - <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (Volitelné) Vytvoření disku spravované ze hello virtuálního pevného disku s *New-AzureRmDisk* -najdete v části <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk>
* Nastavte hello operačního systému disku toohello nové konfigurace virtuálních počítačů virtuálního pevného disku nebo Disk spravované s *Set-AzureRmVMOSDisk* -najdete v části <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
* Vytvoření nového virtuálního počítače z konfigurace virtuálního počítače hello s *New-AzureRmVM* -najdete v části <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>
* Přidat datového disku tooa nový virtuální počítač s *přidat AzureRmVMDataDisk* -najdete v části <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk>

**Azure CLI 2.0**

* Přihlaste se tooyour předplatné s *az přihlášení*
* Vyberte předplatné s *nastaven účet az--předplatného`<subscription name or id`>*
* Nahrát hello virtuálního pevného disku s *az úložiště objektů blob nahrávání* -najdete v části [hello pomocí rozhraní příkazového řádku Azure s Azure Storage][storage-azure-cli]
* (Volitelné) Vytvoření disku spravované ze hello virtuálního pevného disku s *vytvoření disku az* -najdete v části https://docs.microsoft.com/cli/azure/disk#create
* Vytvoření nového virtuálního počítače zadání hello odesílané virtuálního pevného disku nebo spravované Disk jako disk operačního systému s *vytvořit virtuální počítač az* a parametr *– připojit disk operačního systému*
* Přidat tooa disku data nový virtuální počítač s *připojit disk virtuálního počítače az* a parametr *– nový*

**Šablona**

* Nahrát hello virtuálního pevného disku pomocí prostředí Powershell nebo rozhraní příkazového řádku Azure
* (Volitelné) Vytvoření disku spravované ze hello virtuálního pevného disku pomocí prostředí Powershell, rozhraní příkazového řádku Azure nebo hello portálu Azure
* Nasazení hello virtuálních počítačů pomocí JSON šablony odkazující na hello virtuálního pevného disku, jak je znázorněno v [této šablony JSON příklad](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json) nebo pomocí spravovaných disků, jak je znázorněno v [této šablony JSON příklad](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sap-2-tier-user-disk-md/azuredeploy.json).

#### <a name="deployment-of-a-vm-image"></a>Nasazení Image virtuálního počítače
tooupload existující virtuální počítač nebo virtuální pevný disk z hello místní sítě v pořadí toouse ji jako bitovou kopii virtuálního počítače Azure virtuálních počítačů nebo virtuální pevný disk potřebujete toomeet hello požadavky uvedené v kapitole [přípravy pro nasazení virtuálního počítače s bitovou kopii zákaznické pro SAP] [ planning-guide-5.2.2] tohoto dokumentu.

* Použití *sysprep* v systému Windows nebo *příkaz waagent-deprovision* na Linux toogeneralize virtuálního počítače – najdete v části [technické informace o nástroji Sysprep](https://technet.microsoft.com/library/cc766049.aspx) pro systém Windows nebo [jak toocapture Virtuální počítač toouse Linux jako šablony Resource Manageru] [ capture-image-linux-step-2-create-vm-image] pro Linux
* Přihlaste se tooyour předplatné s *Login-AzureRmAccount*
* Nastavte předplatné hello váš kontext s *Set-AzureRmContext* a parametr ID předplatného nebo Název_předplatného - <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* Nahrát hello virtuálního pevného disku s *přidat AzureRmVhd* najdete v části tooan účet úložiště Azure - <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (Volitelné) Vytvoření bitové kopie disku spravované z hello virtuálního pevného disku s *New-AzureRmImage* -najdete v části <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermimage>
* Nastavení disku operačního systému hello nové toothe konfigurace virtuálního počítače
  * Virtuální pevný disk s *Set AzureRmVMOSDisk - SourceImageUri - CreateOption fromImage* -najdete v části <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
  * Spravované bitové kopie disku *Set-AzureRmVMSourceImage* -najdete v části <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage>
* Vytvoření nového virtuálního počítače z konfigurace virtuálního počítače hello s *New-AzureRmVM* -najdete v části <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>

**Azure CLI 2.0**

* Použití *sysprep* v systému Windows nebo *příkaz waagent-deprovision* na Linux toogeneralize virtuálního počítače – najdete v části [technické informace o nástroji Sysprep](https://technet.microsoft.com/library/cc766049.aspx) pro systém Windows nebo [jak toocapture Virtuální počítač toouse Linux jako šablony Resource Manageru] [ capture-image-linux-step-2-create-vm-image] pro Linux
* Přihlaste se tooyour předplatné s *az přihlášení*
* Vyberte předplatné s *nastaven účet az--předplatného`<subscription name or id`>*
* Nahrát hello virtuálního pevného disku s *az úložiště objektů blob nahrávání* -najdete v části [hello pomocí rozhraní příkazového řádku Azure s Azure Storage][storage-azure-cli]
* (Volitelné) Vytvoření bitové kopie disku spravované z hello virtuálního pevného disku s *vytvoření bitové kopie az* -najdete v části https://docs.microsoft.com/cli/azure/image#create
* Vytvoření nového virtuálního počítače zadání hello odesílané virtuálního pevného disku nebo Image disku spravované jako disk operačního systému s *vytvořit virtuální počítač az* a parametr *– bitové kopie*

**Šablona**

* Použití *sysprep* v systému Windows nebo *příkaz waagent-deprovision* na Linux toogeneralize virtuálního počítače – najdete v části [technické informace o nástroji Sysprep](https://technet.microsoft.com/library/cc766049.aspx) pro systém Windows nebo [jak toocapture Virtuální počítač toouse Linux jako šablony Resource Manageru] [ capture-image-linux-step-2-create-vm-image] pro Linux
* Nahrát hello virtuálního pevného disku pomocí prostředí Powershell nebo rozhraní příkazového řádku Azure
* (Volitelné) Vytvoření bitové kopie disku spravované z hello virtuálního pevného disku pomocí prostředí Powershell, rozhraní příkazového řádku Azure nebo hello portálu Azure
* Nasazení hello virtuálních počítačů pomocí JSON šablony odkazující na hello image virtuálního pevného disku, jak je znázorněno v [této šablony JSON příklad](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sap-2-tier-user-image/azuredeploy.json) nebo pomocí hello spravované Image disku, jak je znázorněno v [této šablony JSON příklad](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).

#### <a name="downloading-vhds-or-managed-disks-tooon-premises"></a>Stahování virtuálních pevných disků nebo disků spravované tooon místní
Azure infrastruktura jako služba není jednosměrný ulici pouze způsobená možné tooupload virtuální pevné disky a systémy SAP. Můžete přesunout SAP systémy z Azure zpět do hello, world místní i.

Během hello čas hello nemůže být stažení hello virtuálních pevných disků nebo disků spravované aktivní. I když stahování disky, které jsou připojené tooVMs, hello toobe potřeby virtuální počítač vypnout a navrácena. Pokud chcete pouze obsah, pak by se použít tooset do nového systému místní a pokud je možné, že během hello čas hello stáhnout a hello instalace hello nový systém, který hello systému v Azure může být stále provozní databáze hello toodownload , můžete dlouho výpadky provedením komprimované databáze zálohování na disk a stáhnout pouze tento disk místo také stahování hello základní virtuální počítač operačního systému.

#### <a name="powershell"></a>PowerShell

  * Stahování se spravovaným diskem  
  Je nutné nejprve toohello přístup tooget základní blob hello spravované disku. Pak můžete zkopírovat hello základní tooa nového účtu úložiště blob a stažení z tohoto účtu úložiště objektů blob hello.

  ```powershell
  $access = Grant-AzureRmDiskAccess -ResourceGroupName <resource group> -DiskName <disk name> -Access Read -DurationInSecond 3600
  $key = (Get-AzureRmStorageAccountKey -ResourceGroupName <resource group> -Name <storage account name>)[0].Value
  $destContext = (New-AzureStorageContext -StorageAccountName <storage account name -StorageAccountKey $key)
  Start-AzureStorageBlobCopy -AbsoluteUri $access.AccessSAS -DestContainer <container name> -DestBlob <blob name> -DestContext $destContext
  # Wait for blob copy toofinish
  Get-AzureStorageBlobCopyState -Container <container name> -Blob <blob name> -Context $destContext
  Save-AzureRmVhd -SourceUri <blob in new storage account> -LocalFilePath <local file path> -StorageKey $key
  # Wait for download toofinish
  Revoke-AzureRmDiskAccess -ResourceGroupName <resource group> -DiskName <disk name>
  ```

  * Stahování virtuální pevný disk  
  Jakmile je zastavena hello systému SAP a hello virtuální počítač je vypnutý, můžete použít rutiny prostředí PowerShell hello AzureRmVhd uložit na hello místního disků VHD hello toodownload target zpět toohello místní world. V pořadí toodo, je nutné hello URL hello virtuálního pevného disku, které můžete najít v hello "Úložiště v tématu" Dobrý den portálu Azure (třeba toonavigate toohello účet úložiště a hello kontejner úložiště kde hello VHD se vytvořil) a je nutné tooknow kde hello virtuálního pevného disku by měla být zkopírovat do.

  Potom můžete využít hello příkaz tak, že jednoduše definujete hello parametr SourceUri jako adresa URL hello toodownload hello virtuálního pevného disku a hello LocalFilePath jako hello fyzické umístění hello virtuálního pevného disku (včetně jeho název). příkaz Hello může vypadat podobně jako:

  ```powerhell
  Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
  ```

  Další podrobnosti hello uložit AzureRmVhd rutiny, zkontrolujte, zde <https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvhd>.

#### <a name="cli-20"></a>CLI 2.0
  * Stahování se spravovaným diskem  
  Je nutné nejprve toohello přístup tooget základní blob hello spravované disku. Pak můžete zkopírovat hello základní tooa nového účtu úložiště blob a stažení z tohoto účtu úložiště objektů blob hello.
  ```
  az disk grant-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --duration-in-seconds 3600
  az storage blob download --sas-token "<sas token>" --account-name <account name> --container-name <container name> --name <blob name> --file <local file>
  az disk revoke-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>"
  ```

  * Stahování virtuální pevný disk   
  Jakmile je zastavena hello systému SAP a hello virtuální počítač je vypnutý, můžete použít příkaz příkazového řádku Azure CLI hello _stažení objektu blob úložiště azure_ na cíli místní hello toodownload hello virtuálního pevného disku disky back toohello místní world. V pořadí toodo, že budete potřebovat hello název a hello kontejner hello virtuálního pevného disku, které můžete najít v hello "Úložiště v tématu" Dobrý den portálu Azure (třeba toonavigate toohello účet úložiště a hello kontejner úložiště kde hello VHD se vytvořil) a je nutné tooknow kde Hello virtuálního pevného disku by se měl zkopírovat do.

  Potom můžete využít hello příkaz definováním jednoduše hello parametry blob a kontejner toodownload hello virtuálního pevného disku a cílové hello jako hello fyzické cílové umístění hello virtuálního pevného disku (včetně jeho název). příkaz Hello může vypadat podobně jako:

  ```
  az storage blob download --name <name of hello VHD toodownload> --container-name <container of hello VHD toodownload> --account-name <storage account name of hello VHD toodownload> --account-key <storage account key> --file <destination of hello VHD toodownload>
  ```

### <a name="transferring-vms-and-disks-within-azure"></a>Přenášení virtuální počítače a disky v rámci Azure
#### <a name="copying-sap-systems-within-azure"></a>Kopírování SAP systémy v rámci Azure
Systému SAP nebo i vyhrazený server databázového systému podpora aplikační vrstva SAP pravděpodobně sestávají z několik disků, které obsahují buď hello operačního systému s binárními soubory hello nebo hello data a soubory databáze SAP hello protokolu. Hello Azure funkce kopírování disků ani hello Azure funkce ukládání disků tooa místního disku má synchronizační mechanismus, který by snímky více disků synchronně. Proto stav hello hello zkopírovat nebo uložit disků i v případě, že těch, které jsou připojené proti hello stejného virtuálního počítače by být odlišné. To znamená, že v případě konkrétní hello toho, že různé datové a logfile(s) obsažené v různých disků hello hello databáze v hello end by být nekonzistentní.

**Uzavření: V pořadí toocopy nebo uložit disky, které jsou součástí konfigurace aplikace SAP systému musíte toostop hello SAP systému a také musí nasadit tooshut dolů hello virtuálních počítačů. Pouze pak můžete zkopírovat nebo stáhnout hello sadu disků tooeither vytvořte kopii hello systému SAP v Azure nebo místně.**

Datové disky mohou být uloženy jako soubory virtuálního pevného disku v účtu úložiště Azure a může být přímo připojené tooa virtuálního počítače nebo použít jako obrázek. V takovém případě hello virtuálního pevného disku je umístění zkopírovaného tooanother teprve pak ji bude připojené toohello virtuálního počítače. Hello úplný název souboru virtuálního pevného disku hello v Azure musí být jedinečný v rámci Azure. Jak už bylo zmíněno dříve již, název hello je druh třemi částmi název, který vypadá takto:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Datových disků může být také spravovat disky. V takovém případě hello spravované disku je použité toocreate nové spravované disku teprve pak ji bude připojené toohello virtuálního počítače. Název Hello hello spravované disku musí být jedinečný v rámci skupiny prostředků.

##### <a name="powershell"></a>PowerShell
Můžete použít toocopy rutin prostředí Azure PowerShell virtuální pevný disk, jak je znázorněno v [v tomto článku][storage-powershell-guide-full-copy-vhd]. toocreate nový Disk spravované, pomocí New-AzureRmDiskConfig a New-AzureRmDisk, jak ukazuje následující příklad hello.

```powershell
$config = New-AzureRmDiskConfig -CreateOption Copy -SourceUri "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" -Location <location>
New-AzureRmDisk -ResourceGroupName <resource group name> -DiskName <disk name> -Disk $config
```

##### <a name="cli-20"></a>CLI 2.0
Můžete použít rozhraní příkazového řádku Azure toocopy virtuální pevný disk, jak je znázorněno v [v tomto článku][storage-azure-cli-copy-blobs]. toocreate nový Disk spravované, pomocí *vytvoření disku az* jak ukazuje následující příklad hello.

```
az disk create --source "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --name <disk name> --resource-group <resource group name> --location <location>
```

##### <a name="azure-storage-tools"></a>Azure Storage tools
* <http://storageexplorer.com/>

Profesionální edicí Průzkumníci úložiště Azure naleznete zde:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer>

Hello kopii samotného virtuálního pevného disku v rámci účtu úložiště je proces, který trvá jenom pár sekund (podobně jako tooSAN hardwaru vytváření snímků s opožděné a kopii na zápis). Až budete mít kopii souboru virtuálního pevného disku hello můžete připojit tooa virtuálního počítače nebo použijte jako tooattach image kopíruje hello virtuálního pevného disku toovirtual počítačů.

##### <a name="powershell"></a>PowerShell
```powershell
# attach a vhd tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path toovhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a managed disk tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -ManagedDiskId <managed disk id> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of hello vhd tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name <disk name> -VhdUri <new path of vhd> -SourceImageUri <path tooimage vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption fromImage
$vm | Update-AzureRmVM

# attach a copy of hello managed disk tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$diskConfig = New-AzureRmDiskConfig -Location $vm.Location -CreateOption Copy -SourceUri <source managed disk id>
$disk = New-AzureRmDisk -DiskName <disk name> -Disk $diskConfig -ResourceGroupName <resource group name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Caching <caching option> -Lun <lun, for example 0> -CreateOption attach -ManagedDiskId $disk.Id
$vm | Update-AzureRmVM
```
##### <a name="cli-20"></a>CLI 2.0
```

# attach a vhd tooa vm
az vm unmanaged-disk attach --resource-group <resource group name> --vm-name <vm name> --vhd-uri <path toovhd>

# attach a managed disk tooa vm
az vm disk attach --resource-group <resource group name> --vm-name <vm name> --disk <managed disk id>

# attach a copy of hello vhd tooa vm
# this scenario is currently not possible with Azure CLI. A workaround is toomanually copy hello vhd toohello destination.

# attach a copy of a managed disk tooa vm
az disk create --name <new disk name> --resource-group <resource group name> --location <location of target virtual machine> --source <source managed disk id>
az vm disk attach --disk <new disk name or managed disk id> --resource-group <resource group name> --vm-name <vm name> --caching <caching option> --lun <lun, for example 0>
```

#### <a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Kopírování disků mezi účty úložiště Azure
Tuto úlohu nelze provést na hello portálu Azure. Můžete použít rutiny prostředí Azure PowerShell, rozhraní příkazového řádku Azure nebo prohlížeč úložiště jiných výrobců. Hello rutiny prostředí PowerShell nebo rozhraní příkazového řádku můžete vytvářet a spravovat objekty BLOB, mezi které patří objekty BLOB hello možnost tooasynchronously kopírování mezi různými účty úložiště a v oblastech v rámci hello předplatného Azure.

##### <a name="powershell"></a>PowerShell
Můžete také zkopírovat virtuální pevné disky, mezi předplatnými. Další informace najdete v tématu [v tomto článku][storage-powershell-guide-full-copy-vhd].

základní tok Hello hello PS rutiny logiku vypadá takto:

* Vytvoření kontextu účtu úložiště pro hello **zdroj** účet úložiště s *New-AzureStorageContext* -najdete v části <https://msdn.microsoft.com/library/dn806380.aspx>
* Vytvoření kontextu účtu úložiště pro hello **cíl** účet úložiště s *New-AzureStorageContext* -najdete v části <https://msdn.microsoft.com/library/dn806380.aspx>
* Spustit hello kopírování pomocí

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Zkontrolujte stav hello kopie hello ve smyčce s

```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Připojte hello nový virtuální pevný disk tooa virtuální počítač, jak je popsáno výše.

Příklady najdete v tématu [v tomto článku][storage-powershell-guide-full-copy-vhd].

##### <a name="cli-20"></a>CLI 2.0
* Spustit hello kopírování pomocí

```
az storage blob copy start --source-blob <source blob name> --source-container <source container name> --source-account-name <source storage account name> --source-account-key <source storage account key> --destination-container <target container name> --destination-blob <target blob name> --account-name <target storage account name> --account-key <target storage account name>
```

* Zkontrolujte stav hello, pokud hello kopie ve smyčce s

```
az storage blob show --name <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```

* Připojte hello nový virtuální pevný disk tooa virtuální počítač, jak je popsáno výše.

Příklady najdete v tématu [v tomto článku][storage-azure-cli-copy-blobs].

### <a name="disk-handling"></a>Zpracování disku
#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>Virtuální počítač nebo disk strukturu pro nasazení SAP
V ideálním případě hello zpracování struktury hello virtuálního počítače a hello přidružené disky by měly být velmi jednoduché. V místní instalace vyvinuté zákazníkům mnoho způsobů strukturování instalaci serveru.

* Jeden základní disk, který obsahuje hello operačního systému a všechny binární soubory hello z hello databázového systému nebo SAP. Od března 2015, tento disk může být až too1TB velikost místo starší omezení, která je omezená too127GB.
* Jeden nebo více discích, které obsahuje hello databázového systému soubor protokolu databáze SAP hello soubor protokolu hello hello oblast dočasného úložiště databázového systému (Pokud hello databázového systému podporuje to). Pokud jsou vysoké hello databáze protokolu IOPS požadavky, musíte toostripe více disků v pořadí tooreach hello IOPS svazek potřebný.
* Počet disků obsahující jeden nebo dva soubory databáze z databáze SAP hello a hello databázového systému dočasná data i soubory (Pokud hello databázového systému podporuje to).

![Konfigurace referenčního virtuálního počítače Azure IaaS pro SAP][planning-guide-figure-1300]

[comment]: <> (Struktura Linux popisují MShermannd TODO)

- - -
> ![Windows][Logo_Windows] Windows
>
> U mnoha zákazníků jsme viděli konfigurace, například SAP a databázového systému binární soubory nebyly nainstalovanou na jednotce c:\ hello kde hello operačního systému byla nainstalována. Existují různé důvody pro to, ale když jsme se zpět toohello kořenové, obvykle byl že hello jednotky byly malé a upgrady operačního systému před 10 až 15 let potřeba další místo. Obě podmínky dní příliš často už neplatí. Dnes jednotku c:\ hello lze mapovat na velkého objemu disků nebo virtuálních počítačů. V nasazeních pořadí tookeep jednoduché jejich struktury, je doporučeno toofollow následující vzor nasazení pro SAP NetWeaver systémy v Azure
>
> stránkovací soubor operačního systému Windows Hello by měla být na hello D: jednotky (dočasnou disk)
>
> ![Linux][Logo_Linux] Linux
>
> Umístěte hello Linux swapfile pod /mnt/mnt nebo prostředků v systému Linux, jak je popsáno v [v tomto článku][virtual-machines-linux-agent-user-guide]. Hello odkládací soubor lze nakonfigurovat v souboru konfigurace hello hello /etc/waagent.conf agenta systému Linux. Přidat nebo změnit hello následující nastavení:
>
>

```
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

tooactivate hello změny, je nutné toorestart hello agenta systému Linux s

```
sudo service waagent restart
```

Přečtěte si prosím Poznámka SAP [1597355] další podrobnosti o hello doporučené velikosti odkládacího souboru

- - -
hello IOPS požadavků a hello latence požadované by měl určit Hello počet disků použitých jako hello databázového systému datové soubory a typ hello tyto disky jsou hostované v Azure Storage. Přesný kvóty jsou popsané v [v tomto článku (Linux)] [ virtual-machines-sizes-linux] a [v tomto článku (Windows)][virtual-machines-sizes-windows].

Prostředí SAP nasazení přes hello poslední 2 roky nám výukové některé lekce, které jde vyhodnotit jako:

* IOPS provoz toodifferent datových souborů není vždy hello stejné od stávajících systémů zákazníků může mít jinak velikost dat souborů představující jejich SAP databází. V důsledku zapnuta out toobe více disků tooplace hello datové soubory, které logické jednotky LUN carved mimo těch, které lépe pomocí konfiguraci RAID. Nebyly situacích, zejména s Azure Standard Storage, kde IOPS míra dosáhl kvóty hello jednoho disku proti hello databázového systému transakčního protokolu. V takových situacích se doporučuje použít hello úložiště Premium Storage nebo případně agregování více standardního úložiště disků se softwaru RAID.

- - -
> ![Windows][Logo_Windows] Windows
>
> * [Osvědčené postupy z hlediska výkonu pro SQL Server v Azure Virtual Machines][virtual-machines-sql-server-performance-best-practices]
>
> ![Linux][Logo_Linux] Linux
>
> * [Konfigurace softwaru diskového pole RAID v systému Linux][virtual-machines-linux-configure-raid]
> * [Konfigurace LVM na virtuální počítač s Linuxem v Azure][virtual-machines-linux-configure-lvm]
> * [Azure tajných klíčů úložiště a vstupně-výstupních operací Linux optimalizace](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)
>
>

- - -
* Storage úrovně Premium se zobrazuje důležité lepší výkon, hlavně pro kritické transakce protokolu zápisy. Pro scénáře SAP, které jsou očekávané toodeliver produkční, jako je třeba výkon důrazně doporučujeme toouse VM-Series, které můžete využít Azure Premium Storage.

Mějte na paměti této hello disku, který obsahuje hello operačního systému, a jako doporučujeme, hello binární soubory databáze SAP a hello (základní virtuální počítač) taky není už omezený too127GB. Teď může mít až too1TB velikost. To by měl být dost místa tookeep všechny hello včetně nezbytné souboru, například, SAP batch v protokolech úloh.

Pro další návrhy a další podrobnosti, konkrétně pro virtuální počítače databázového systému, přečtěte si hello [Průvodce nasazením databázového systému][dbms-guide]

#### <a name="disk-handling"></a>Zpracování disku
Ve většině případů potřebujete další disky toocreate v databázi SAP hello toodeploy pořadí do hello virtuálních počítačů. Už jsme mluvili o aspektech hello na počet disků v kapitole [virtuální počítač nebo disk strukturu pro nasazení SAP] [ planning-guide-5.5.1] tohoto dokumentu. Hello portál Azure umožňuje tooattach a odpojit disky po základní virtuální počítač nasazen. Hello disků může být připojit nebo odpojit po hello virtuálních počítačů a spuštěných i když je zastavena. Při připojení disku, hello portál Azure poskytuje tooattach prázdný disk nebo stávající disk, který v tuto chvíli není připojen tooanother virtuálních počítačů.

**Poznámka:**: disky lze pouze připojené tooone virtuálních počítačů v každém okamžiku.

![Připojit / Odpojit disky s Azure Standard Storage][planning-guide-figure-1400]

Během hello nasazení nového virtuálního počítače můžete rozhodnout, zda mají být toouse spravované disky nebo umístit vaše disky na účtech úložiště Azure. Pokud chcete toouse Storage úrovně Premium, doporučujeme používat spravované disky.

Dále je nutné toodecide, zda chcete, aby toocreate nové a prázdný disk nebo jestli chcete tooselect existujícího disku, který se předtím nahrála a měl by být nyní připojen toohello virtuálních počítačů.

**Důležité**: můžete **nesmí** má toouse ukládání do mezipaměti hostitele s Azure Standard Storage. Nechat hello mezipaměti hostitele předvoleb na výchozí hello none. S Azure Premium Storage byste měli povolit ukládání do mezipaměti pro čtení vlastnosti hello vstupně-výstupních operací je většinou přečtení jako typický vstupně-výstupní provoz proti datových souborů databáze. Pokud se soubor protokolu transakcí databáze je doporučeno bez ukládání do mezipaměti.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Jak tooattach dat na disku v hello portálu Azure][virtual-machines-linux-attach-disk-portal]
>
> Pokud jsou připojené disky, musíte toolog v toohello virtuálních počítačů tooopen hello Správce disků systému Windows. Pokud automatického připojení není povolena podle doporučení v kapitole [nastavení automatického připojení pro připojené disky][planning-guide-5.5.3], hello nově připojená svazku musí toobe provést online a inicializován.
>
> ![Linux][Logo_Linux] Linux
>
> Pokud jsou připojené disky, potřebujete toolog v toohello virtuálního počítače a inicializace hello disků, jak je popsáno v [v tomto článku][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]
>
>

- - -
Pokud nový disk hello je prázdný disk, musíte také tooformat hello disku. Pro formátování, hlavně pro databázového systému hello dat a souborů protokolu stejná doporučení pro úplné nasazení hello platí databázového systému.

Jak již bylo uvedeno v kapitole [hello koncept virtuálního počítače Microsoft Azure][planning-guide-3.2], účet úložiště Azure neposkytuje nekonečné prostředky z hlediska vstupně-výstupních operací svazek, svazek IOPS a data. Obvykle databázového systému virtuální počítače jsou nejvíce ovlivněno. Pokud máte několik vysoké toodeploy virtuální počítače svazku vstupně-výstupních operací v pořadí toostay v hello limitu hello účet úložiště Azure svazek může být nejlepší toouse samostatný účet úložiště pro každý virtuální počítač. Jinak je nutné toosee jak můžou vyrovnávat těchto virtuálních počítačů mezi různými účty úložiště bez nedosáhli limitu hello každý jeden účet úložiště. Další podrobnosti, které jsou popsané v hello [Průvodce nasazením databázového systému][dbms-guide]. Tato omezení měli také mít na paměti pro čistá aplikace SAP virtuálních počítačů serveru nebo ostatních virtuálních počítačů, které nakonec může vyžadovat další virtuální pevné disky. Tato omezení neplatí, pokud používáte spravované disku. Pokud máte v plánu toouse Storage úrovně Premium, doporučujeme používat spravované disku.

Jiné téma, které jsou důležité pro účty úložiště se, zda jsou hello virtuálních pevných disků v účtu úložiště, získávání geograficky replikované. Geografická replikace povolená nebo zakázaná na hello úrovni účtu úložiště a ne na hello úroveň virtuálního počítače. Pokud je povoleno geografická replikace, hello hello virtuálních pevných disků v rámci hello účet úložiště se replikuje do jiného datového centra Azure v rámci stejné oblasti. Než se rozhodnete, že na tomto, měli zamyslet hello následující omezení:

Azure geografická replikace nereplikuje hello IOs v chronologickém pořadí napříč více virtuálních pevných disků ve virtuálním počítači a funguje místně na každý virtuální pevný disk ve virtuálním počítači. Proto se replikují hello virtuální pevný disk, zda text hello představuje základní virtuálních počítačů a také všechny další toohello připojit virtuální pevné disky virtuálního počítače nezávisle na sobě navzájem. To znamená, že neexistuje žádná synchronizace mezi hello změny v hello jiný virtuální pevné disky. Hello skutečnost, že hello IOs se replikují nezávisle na hello pořadí, ve které jsou zapsané znamená geografická replikace není hodnoty pro databázové servery, které mají své databáze distribuovány na více virtuálních pevných disků. V přidání toohello databázového systému také mohou existovat další aplikace, které procesy zápisu nebo pracovat s daty v různých virtuálních pevných disků, a tam, kde je důležité tookeep hello pořadí změny. Pokud se požadavek, by nemělo být povoleno geografická replikace v Azure. Závisí na tom, jestli potřebujete nebo chcete geografická replikace pro sadu virtuálních počítačů, ale ne pro jinou sadu, můžete již zařadit virtuálních počítačů a jejich související virtuální pevné disky do jiné účty úložiště, které mají geografická replikace povolená nebo zakázaná.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Nastavení automatického připojení pro připojené disky
- - -
> ![Windows][Logo_Windows] Windows
>
> Pro virtuální počítače, které jsou vytvořené pomocí vlastní Image nebo disky, je nutné toocheck a nastavte parametr hello automatického připojení, které by mohly mít. Nastavení tohoto parametru vám umožní hello virtuálních počítačů po restartování nebo opětovné nasazení v Azure toomount hello připojen nebo připojené jednotky automaticky znovu.
> Parametr Hello je nastaven pro bitové kopie hello od společnosti Microsoft v hello Azure Marketplace.
>
> V pořadí tooset hello automatického připojení Zkontrolujte prosím hello dokumentaci hello příkazového řádku spustitelného souboru diskpart.exe tady:
>
> * [Možnosti příkazového řádku DiskPart](https://technet.microsoft.com/library/bb490893.aspx)
> * [Automatického připojení](http://technet.microsoft.com/library/cc753703.aspx)
>
> okno příkazového řádku systému Windows Hello musí být otevřen jako správce.
>
> Pokud jsou připojené disky, musíte toolog v toohello virtuálních počítačů tooopen hello Správce disků systému Windows. Pokud automatického připojení není povolena podle doporučení v kapitole [nastavení automatického připojení pro připojené disky][planning-guide-5.5.3], hello nově připojený svazek > potřebuje toobe provést online a inicializován.
>
> ![Linux][Logo_Linux] Linux
>
> Je třeba tooinitialize nově připojená prázdný disk, jak je popsáno v [v tomto článku][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> Budete také potřebovat tooadd /etc/fstab toohello nové disky.
>
>

- - -
### <a name="final-deployment"></a>Poslední nasazení
Hello konečné nasazení a přesné kroky, zejména s namapoval toohello nasazení SAP rozšířené monitorování, naleznete toohello [Průvodce nasazením][deployment-guide].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Přístup k SAP systémy s operačním systémem v rámci virtuálních počítačů Azure
Pro scénáře jenom pro Cloud, můžete tooconnect toothose SAP systémy přes hello veřejného Internetu pomocí grafického uživatelského rozhraní SAP. U těchto případech hello následující postupy potřebovat toobe použít.

Později v hello dokument, který se budeme zabývat hello jiných hlavní scénáře, připojování tooSAP systémy v nasazeních mezi různými místy, které mají připojení site-to-site (tunelového připojení sítě VPN) nebo Azure ExpressRoute připojení mezi místní hello a Azure systémy.

### <a name="remote-access-toosap-systems"></a>Systémy tooSAP vzdáleného přístupu
Pomocí Azure Resource Manageru neobsahuje žádné výchozí koncové body už jako hello bývalé klasického modelu. Všechny porty virtuálního počítače Azure ARM jsou otevřené, dokud:

1. Žádné skupiny zabezpečení sítě je definována pro podsíť hello nebo hello síťové rozhraní. TooAzure provoz sítě virtuálních počítačů může být zabezpečené pomocí takzvané "skupin zabezpečení sítě". Další informace najdete v části [co je skupina zabezpečení sítě (NSG)?][virtual-networks-nsg]
2. Pro síťové rozhraní hello není definována žádná služba Vyrovnávání zatížení Azure   

V tématu hello architektura rozdíl mezi klasického modelu a ARM, jak je popsáno v [v tomto článku][virtual-machines-azure-resource-manager-architecture].

#### <a name="configuration-of-hello-sap-system-and-sap-gui-connectivity-for-cloud-only-scenario"></a>Konfigurace hello připojení k systému SAP a SAP grafického uživatelského rozhraní pro scénář jenom pro Cloud
Podrobnosti najdete v tomto článku, který popisuje téma toothis podrobnosti: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx>

#### <a name="changing-firewall-settings-within-vm"></a>Změna nastavení brány Firewall v rámci virtuálního počítače
Může být nutné tooconfigure hello brány firewall na vaše virtuální počítače tooallow příchozí provoz tooyour SAP systému.

- - -
> ![Windows][Logo_Windows] Windows
>
> Hello brány Windows Firewall v rámci Azure nasazený virtuální počítač je ve výchozím nastavení zapnuta. Teď musíte tooallow hello SAP Port toobe otevřít, jinak hello SAP grafickým uživatelským rozhraním nebudou moct tooconnect.
> toodo toto:
>
> * Otevřete ovládací panely\systém a zabezpečení\Brána Windows Firewall too'Advanced nastavení.
> * Nyní klikněte pravým tlačítkem na příchozí pravidla a pokusit 'Nové pravidlo'.
> * V hello následující průvodce zvolili toocreate nové pravidlo 'Port'.
> * V dalším kroku hello hello Průvodce ponechte nastavení hello na TCP a typ v požadované číslo portu hello tooopen. Vzhledem k tomu, že je naše ID instance SAP 00, vzali jsme 3200. Pokud je vaše instance číslo jinou instanci, musí být otevřen hello port, který jste definovali dříve podle čísla instance hello.
> * V další části hello hello průvodce musíte tooleave hello položku "Povolit připojení, zaškrtnutí.
> * V dalším kroku průvodce hello hello je nutné toodefine, zda text hello pravidlo pro doménu, privátní a veřejné sítě. Pokud potřebuje tooyour potřeby ho upravte. Ale připojení s grafickým uživatelským rozhraním SAP z hello mimo prostřednictvím hello veřejnou síť, musíte toohave hello pravidlo toohello veřejné síti.
> * V posledním kroku hello hello průvodce název hello pravidla a uložte stisknutím kombinace kláves 'Dokončit'
>
> pravidlo Hello začne okamžitě platit.
>
> ![Definice pravidla portu][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> Hello Linux obrázků v hello Azure Marketplace nepovolujte hello iptables brány firewall ve výchozím nastavení a hello připojení tooyour SAP systému, by měly fungovat. Pokud jste povolili iptables nebo jiná brána firewall, naleznete v dokumentaci toohello iptables nebo hello používá Brána firewall tooallow příliš příchozí přenosy tcp port 32xx (kde xx je číslo systému hello systému SAP).
>
>

- - -
#### <a name="security-recommendations"></a>Doporučení zabezpečení
Hello grafickým uživatelským rozhraním SAP nepřipojí okamžitě tooany instance SAP hello (port 32xx), které jsou spuštěny, ale poprvé připojí přes SAP toohello otevřený port hello zpráva procesu serveru (port 36xx). V hello za velmi stejný port hello použilo zprávu server hello hello interní komunikaci toohello aplikace instancí. tooprevent místní aplikační servery v nechtěně komunikaci s server zpráv v Azure hello interní komunikační porty může být změněny. Důrazně doporučujeme toochange hello interní komunikaci mezi hello SAP zpráv serveru a jeho aplikace instancí tooa jiné číslo portu v systémech, které byly klonovat z místních systémů, jako je například klon vývoje pro testování projektu atd. To lze provést pomocí hello výchozí profil parametru:

> rdisp/msserv_internal
>
>

jak je uvedeno v [nastavení zabezpečení pro SAP zprávu Server hello](https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm)

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>Koncepty čistě cloudové nasazení SAP instancí
### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Jeden virtuální počítač s SAP NetWeaver demo a školení scénář
![S jeden systémy ukázku SAP virtuálních počítačů s hello stejné názvy virtuálních počítačů, izolované ve službě Azure Cloud Services][planning-guide-figure-1700]

V tomto scénáři (naleznete v kapitole [jenom pro Cloud] [ planning-guide-2.1] tohoto dokumentu) jsme implementujete typické školení nebo ukázkový scénář systému kde obsaženy hello dokončení školení nebo ukázkový scénář v jeden virtuální počítač. Předpokládáme, že nasazení hello se provádí prostřednictvím image šablony virtuálních počítačů. Také předpokládáme, že více tyto ukázkové/školení toobe potřeba virtuální počítače nasazené s hello virtuálních počítačů s hello stejný název.

Hello předpokladem je, který jste vytvořili Image virtuálního počítače, jak je popsáno v některých částech kapitoly [přípravy virtuálních počítačů s SAP pro Azure] [ planning-guide-5.2] v tomto dokumentu.

Hello pořadí událostí tooimplement hello scénáře vypadá takto:

##### <a name="powershell"></a>PowerShell
* Vytvořit novou skupinu prostředků pro každých šířku školení nebo demo

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```
* Vytvořit nový účet úložiště, pokud nechcete, aby toouse spravované disky

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Vytvořit novou virtuální síť pro každý školení nebo ukázku šířku tooenable hello využití hello stejný název hostitele a IP adresy. Skupinu zabezpečení sítě, který povoluje přístup ke vzdálené ploše 3389 tooenable tooport provozu a port 22 pro SSH je chráněn Hello virtuální sítě.

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Vytvořit novou veřejnou IP adresu, která lze použít tooaccess hello virtuálnímu počítači z hello Internetu

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Vytvoření nového síťového rozhraní pro virtuální počítač hello

```powershell
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip
```

* Vytvoření virtuálního počítače. Pro scénář jenom pro Cloud hello bude mít každý virtuální počítač hello stejný název. Hello SAP SID hello SAP NetWeaver instance v těchto virtuálních počítačů bude stejné hello také. V rámci hello skupiny prostředků Azure, hello název hello virtuálního počítače potřebuje toobe jedinečné, ale v různých skupinách prostředků Azure můžete spustit virtuální počítače s hello stejný název. Výchozí účet 'správce Windows Hello nebo "kořenový" pro Linux nejsou platné. Proto nové uživatelské jméno správce musí toobe definované společně s heslem. velikost Hello hello virtuálního počítače musí také toobe definované.

```powershell
#####
# Create a new virtual machine with an official image from hello Azure Marketplace
#####
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES-SAP" -Skus "12-SP1" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "Oracle" -Offer "Oracle-Linux" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains hello private image that you want toouse
#####
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path tooVHD that contains hello OS image> -Windows
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path tooVHD that contains hello OS image> -Linux
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a Managed Disk Image
#####
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -Id <Id of Managed Disk Image>
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* Volitelně můžete přidat další disky a potřeby obsah obnovit. Upozorňujeme, že názvy všech objektů blob (toohello adresy URL objektů BLOB), musí být jedinečný v rámci Azure.

```powershell
# Optional: Attach additional VHD data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM

# Optional: Attach additional Managed Disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -DiskSizeInGB 1023 -CreateOption empty -Lun 0 | Update-AzureRmVM
```

##### <a name="cli"></a>Rozhraní příkazového řádku
Následující příklad kódu Hello lze v systému Linux. Pro systém Windows, prosím buď pomocí prostředí PowerShell, jak je popsáno výše nebo přizpůsobit hello příklad toouse % rgName % místo $rgName a nastavte proměnnou prostředí hello pomocí příkazu Windows hello *nastavit*.

* Vytvořit novou skupinu prostředků pro každých šířku školení nebo demo

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
az group create --name $rgName --location "North Europe"
```

* Vytvořit nový účet úložiště

```
az storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku Standard_LRS --name $rgNameLower
```

* Vytvořit novou virtuální síť pro každý školení nebo ukázku šířku tooenable hello využití hello stejný název hostitele a IP adresy. Skupinu zabezpečení sítě, který povoluje přístup ke vzdálené ploše 3389 tooenable tooport provozu a port 22 pro SSH je chráněn Hello virtuální sítě.

```
az network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

az network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
az network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group SAPERPDemoNSG
```

* Vytvořit novou veřejnou IP adresu, která lze použít tooaccess hello virtuálnímu počítači z hello Internetu

```
az network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --dns-name $rgNameLower --allocation-method Dynamic
```

* Vytvoření nového síťového rozhraní pro virtuální počítač hello

```
az network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-address SAPERPDemoPIP --subnet Subnet1 --vnet-name SAPERPDemoVNet
```

* Vytvoření virtuálního počítače. Pro scénář jenom pro Cloud hello bude mít každý virtuální počítač hello stejný název. Hello SAP SID hello SAP NetWeaver instance v těchto virtuálních počítačů bude stejné hello také. V rámci hello skupiny prostředků Azure, hello název hello virtuálního počítače potřebuje toobe jedinečné, ale v různých skupinách prostředků Azure můžete spustit virtuální počítače s hello stejný název. Výchozí účet 'správce Windows Hello nebo "kořenový" pro Linux nejsou platné. Proto nové uživatelské jméno správce musí toobe definované společně s heslem. velikost Hello hello virtuálního počítače musí také toobe definované.

```
#####
# Create virtual machines using storage accounts
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password

#####
# Create virtual machines using Managed Disks
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
```

```
#####
# Create a new virtual machine with a VHD that contains hello private image that you want toouse
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path tooimage vhd>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path tooimage vhd> --authentication-type password

#####
# Create a new virtual machine with a Managed Disk Image
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id> --authentication-type password
```

* Volitelně můžete přidat další disky a potřeby obsah obnovit. Upozorňujeme, že názvy všech objektů blob (toohello adresy URL objektů BLOB), musí být jedinečný v rámci Azure.

```
# Optional: Attach additional VHD data disks
az vm unmanaged-disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --vhd-uri https://$rgNameLower.blob.core.windows.net/vhds/data.vhd  --new

# Optional: Attach additional Managed Disks
az vm disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --disk datadisk --new
```

##### <a name="template"></a>Šablona
Můžete vytvořit šablony ukázka hello hello šablon azure rychlý start úložišti na githubu.

* [Jednoduchý virtuální počítač s Linuxem](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Jednoduchý virtuální počítač s Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [Virtuální počítač z bitové kopie](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-which-need-toocommunicate-within-azure"></a>Implementovat sadu virtuálních počítačů, které vyžadují toocommunicate v rámci Azure
Tento scénář jenom pro Cloud je typický scénář pro účely školení a ukázku, kde hello software představující hello demo a školení scénář rozložené v několika virtuálními počítači. Hello různé součásti nainstalované v hello jiné virtuální počítače nutné toocommunicate mezi sebou. Znovu v tomto scénáři nemáte žádnou místní síťová komunikace nebo mezi různými místy scénáři je potřeba.

Tento scénář je rozšíření hello instalace popsané v kapitole [jeden virtuální počítač s SAP NetWeaver demo a školení scénář] [ planning-guide-7.1] tohoto dokumentu. Další virtuální počítače v tomto případě bude přidán tooan existující skupinu prostředků. Následující příklad hello školení na šířku se skládá z SAP ASC nebo SCS virtuálních počítačů, virtuální počítač spuštěn databázového systému a aplikačního serveru SAP instance virtuálních počítačů v hello.

Než vytvoříte v tomto scénáři musíte toothink o základní nastavení už provést v případě hello před.

#### <a name="resource-group-and-virtual-machine-naming"></a>Skupina prostředků a pojmenování virtuálního počítače
Všechny názvy skupin prostředků musí být jedinečný. Vývoj vlastní schéma pojmenování prostředků, jako například `<rg-name`>-příponu.

název virtuálního počítače Hello má toobe jedinečný v rámci skupiny prostředků hello.

#### <a name="set-up-network-for-communication-between-hello-different-vms"></a>Nastavení sítě pro komunikaci mezi hello různé virtuální počítače
![Sadu virtuálních počítačů v rámci virtuální sítě Azure][planning-guide-figure-1900]

tooprevent pojmenování kolizí s klony hello stejné krajiny školení nebo ukázku, musíte pro každou šířku toocreate virtuální síť Azure. Překlad názvů DNS poskytovaný Azure nebo můžete nakonfigurovat vlastní DNS server mimo Azure (ne toobe další zde popsané). V tomto scénáři jsme nekonfigurujte vlastní DNS. Pro všechny virtuální počítače uvnitř jednu virtuální síť Azure bude povolena komunikace přes názvy hostitelů.

Hello důvodů, proč tooseparate školení nebo ukázku krajiny tak, že virtuální sítě a ne jenom prostředků, které skupiny může být:

* Hello SAP šířku podle potřeby vlastní AD OpenLDAP a součástí toobe Server domény potřebám jednotlivých krajiny hello.  
* na šířku SAP Hello nastavení má součásti tohoto potřeba toowork s pevné IP adresy.

Další informace o virtuálních sítí Azure a jak toodefine je možné najít v [v tomto článku][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>Nasazení SAP virtuálních počítačů s připojením k podnikové síti (mezi různými místy)
Spuštění SAP šířku a chcete nasazení hello toodivide mezi úplné obnovení vyšší kategorie servery databázového systému, místní virtualizované prostředí pro aplikaci vrstvy a menší vrstvě 2 nakonfigurovali SAP systémy a Azure IaaS. Hello základní předpokladem je, že systémy SAP v rámci jednoho šířku SAP nutné toocommunicate mezi sebou a s mnoha další softwarové součásti nasazení ve společnosti hello, nezávisle na jejich nasazení formuláře. Taky by měl existovat žádné rozdíly zaváděné hello nasazení formuláře pro připojení s grafickým uživatelským rozhraním SAP nebo dalších rozhraní hello koncového uživatele. Tyto podmínky lze splnit jenom, když máme hello místní Active Directory nebo OpenLDAP a služby DNS rozšířené toohello Azure systémy prostřednictvím připojení site na lokality nebo více-místě nebo privátní připojení, jako jsou Azure ExpressRoute.

V pořadí tooget informace na pozadí na podrobnosti implementace hello SAP v Azure, doporučujeme vám kapitoly tooread [koncepty Cloud-Only nasazení SAP instancí] [ planning-guide-7] tohoto dokumentu, která vysvětluje, Některé hello základy vytvoří Azure a jak tyto musí být použit s SAP aplikací v Azure.

### <a name="scenario-of-an-sap-landscape"></a>Scénář šířku SAP
Hello napříč místním a externím může být zhruba podle jako hello grafiky níže:

![Připojení Site-to-Site mezi místními a prostředky Azure][planning-guide-figure-2100]

scénář Hello uvedené výše popisuje scénář, kde hello místní AD nebo OpenLDAP a DNS jsou rozšířené tooAzure. Na straně místní hello je vyhrazený určitý rozsah IP adres podle předplatného Azure. Hello rozsah IP adres se přiřadí tooan Azure Virtual Network na hello Azure straně.

#### <a name="security-considerations"></a>Aspekty zabezpečení
Hello minimální požadavek je použití hello zabezpečený komunikační protokoly, jako je protokol SSL/TLS pro přístup z prohlížeče nebo připojení VPN pro systému získat přístup k toohello Azure services. Hello předpokladem je, společností velmi jinak zpracování hello připojení VPN mezi jejich podnikovou sítí a Azure. Některé společnosti mohou blankly otevřete všechny porty hello. Některé jiné společnosti může být vhodné toobe velmi přesné v porty, které potřebují tooopen atd.

V následující typické SAP hello tabulce jsou uvedeny komunikační porty. V podstatě je dostatečná tooopen hello portu brány SAP.

| Služba | Název portu | Příklad `<nn`> = 01 | Výchozí rozsah (min-max) | Komentář |
| --- | --- | --- | --- | --- |
| Dispečer |sapdp`<nn>` najdete v části * |3201 |3200 – 3299 |Dispečer SAP, používá SAP grafického uživatelského rozhraní pro systém Windows a Java |
| Server zpráv |sapms`<sid`> najdete v části ** |3600 |volné sapms`<anySID`> |identifikátor SID = ID systému SAP |
| brána |sapgw`<nn`> najdete v části * |3301 |Volné |Bráně SAP, používá pro komunikaci CPIC a RFC |
| Směrovač SAP |sapdp99 |3299 |Volné |Pouze názvy služby CI (centrální instance) můžete přiřadit v libovolné hodnotě /etc/services tooan po instalaci. |

*) nn = čísla Instance SAP

*) sid = ID systému SAP

Podrobnější informace o porty vyžadované pro různé SAP produkty nebo služby, na základě produktů SAP naleznete zde <http://scn.sap.com/docs/DOC-17124>.
Tento dokument je třeba možné tooopen vyhrazené porty potřebné pro scénáře a konkrétní produkty SAP zařízení VPN hello.

Další bezpečnostní opatření při nasazení virtuálních počítačů v takovém scénáři může být toocreate [skupinu zabezpečení sítě] [ virtual-networks-nsg] toodefine pravidla přístupu.

### <a name="dealing-with-different-virtual-machine-series"></a>Práci s jinou sérii virtuálního počítače
V průběhu posledních 12 měsíců hello Microsoft přidat mnoho další typy virtuálních počítačů, které se liší buď v počet Vcpu, paměti nebo důležitější na hardwaru, je spuštěn na. Ne všechny tyto virtuální počítače jsou podporovány v systému SAP (viz podporované typy virtuálních počítačů v Poznámka SAP [1928533]). Některé z těchto virtuálních počítačů na jiný hostitelský hardware generace spustit. V hello členitosti z Azure – jednotka škálování získávání nasazených generace hardwaru tyto hostitele. Mohou nastat znamená případy, kdy hello různých zvolíte nelze spustit na velikosti virtuálních počítačů hello stejné jednotky škálování. Skupiny dostupnosti je omezená toospan hello možnosti, které jednotek škálování závislosti na jiný hardware.  Pro příklad, pokud chcete toorun hello databázového systému na virtuálních počítačích A5 A11 a hello SAP aplikační vrstvu na virtuálních počítačích G-Series, by byl vynutit toodeploy jednoho systému SAP nebo jiné systémy SAP v rámci jiné skupiny dostupnosti.

#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>Tisk na místní síťové tiskárny z instance SAP v Azure
##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>Tisk přes TCP/IP v napříč místním a externím
Nastavení místní síťové tiskárny TCP/IP na základě virtuální počítač Azure se celkové hello stejné jako vaší podnikové síti, a to za předpokladu, že budete mít tunelové propojení VPN typu Site-To-Site nebo připojením ExpressRoute vytvořeno.

- - -
> ![Windows][Logo_Windows] Windows
>
> toodo toto:
>
> * Některé síťové tiskárny se dodávají s Průvodce konfigurací, takže je easy tooset vaši tiskárnu ve virtuálním počítači Azure. Pokud žádný takový software Průvodce byl distribuován se tiskárny hello "Ruční" způsobem tooset tiskárnu hello je toocreate nový port tiskárny TCP/IP.
> * Otevřete ovládací panely -> zařízení a tiskárny -> Přidat tiskárnu
> * Zvolte možnost Přidat tiskárnu pomocí adresy protokolu TCP/IP nebo název hostitele
> * Zadejte IP adresu hello tiskárny hello
> * Port tiskárny standardní 9100
> * V případě potřeby nainstalujte ručně hello příslušný ovladač tiskárny.
>
> ![Linux][Logo_Linux] Linux
>
> * jako k Windows právě podle hello standardní postup tooinstall síťové tiskárny
> * postupujte podle hello veřejné Linux příručky pro [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) nebo [Red Hat a Oracle Linux](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) o tooadd tiskárnu.
>
>

- - -
![Tisk v síti][planning-guide-figure-2200]

##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Na hostiteli tiskárnu přes protokol SMB (sdílené tiskárny) mezi různými místy scénář
Tiskárny na hostiteli nejsou kompatibilní sítě záměrné. Ale tiskárnu založené na hostiteli může být sdílen počítače v síti, dokud tiskárny hello je připojený tooa používá technologii na počítač. Připojení k vaší podnikové sítě Site-To-Site nebo ExpressRoute a sdílet místní tiskárny. Hello protokolu SMB pro rozhraní NetBIOS místo DNS používá jako název služby. název hostitele rozhraní NetBIOS Hello se může lišit od název hostitele DNS hello. Hello standardní případem je, že jsou identické hello název hostitele rozhraní NetBIOS a název hostitele DNS hello. Doména DNS Hello nemá smysl v oboru názvů pro rozhraní NetBIOS hello. Podle toho hello plně kvalifikovaný název hostitele DNS skládající se z názvu hostitele DNS hello a doména DNS nesmí se používat v oboru názvů pro rozhraní NetBIOS hello.

sdílené tiskárny Hello je identifikována jedinečný název v síti hello:

* Název hostitele SMB hello (vždycky potřeboval).
* Název sdílené složky hello (vždycky potřeboval).
* Název domény hello, pokud není sdílená složka tiskárny v hello stejné doméně jako SAP systému.
* Uživatelské jméno a heslo navíc může být sdílené tiskárny požadované tooaccess hello.

Postup:

- - -
> ![Windows][Logo_Windows] Windows
>
> Sdílet místní tiskárny.
> V hello virtuálního počítače Azure otevřete Průzkumníka Windows hello a zadejte název sdílené složky hello hello tiskárny.
> Průvodce instalací tiskárny vás provede hello procesu instalace.
>
> ![Linux][Logo_Linux] Linux
>
> Zde jsou některé příklady dokumentaci o konfiguraci síťové tiskárny v systému Linux nebo včetně kapitola ohledně tisku v systému Linux. Hello bude pracovat stejně jako virtuální počítač Azure Linux, pokud hello virtuální počítač je součástí sítě VPN:
>
> * SLES <_Share_or_Windows_Share https://en.opensuse.org/SDB:Printing_via_SMB_ (Samba)>
> * RHEL nebo Oracle Linux <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sec-Printer_Configuration.html#s1-printing-smb-printer>
>
>

- - -
##### <a name="usb-printer-printer-forwarding"></a>Tiskárny USB (přesměrování tiskárny)
V Azure hello schopnost hello služby Vzdálená plocha tooprovide uživatelům hello přístup zařízení místní tiskárnu tootheir ve vzdálené relaci není k dispozici.

- - -
> ![Windows][Logo_Windows] Windows
>
> Další informace o tisk se systémem Windows naleznete zde: <http://technet.microsoft.com/library/jj590748.aspx>.
>
>

- - -
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>Integrace SAP Azure systémů do oprava a systému pro přenos (TMS) v mezi různými místy
Hello SAP změn a přenosu systému (TMS) tooexport toobe nakonfigurované potřeb a importovat žádost o přenos mezi systémy v hello na šířku. Předpokládáme, že instance hello vývoj systému SAP (vývoj) nacházejí v Azure, zatímco hello kvality zajištění kvality a produktivitu systémy (PRD) jsou místní. Kromě toho předpokládáme, že existuje adresář centrální přenosu.

##### <a name="configuring-hello-transport-domain"></a>Konfigurace hello přenosu domény
Nakonfigurujte doménu přenosu systému hello jste určili jako hello přenosu řadič domény, jak je popsáno v [hello konfigurace řadiče domény přenos](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm). Systémový uživatel, bude vytvořen TMSADM a hello požadované cílové RFC se budou generovat. Tato připojení RFC pomocí hello transakce SM59 může zkontrolovat. Rozlišení názvů hostitelů musí být povolena napříč doménu přenosu.

Postup:

* V tomto scénáři jsme se rozhodli hello místního systému QAS bude řadič domény CTS hello. Volání transakce moduly STM. Zobrazí se dialogové okno TMS Hello. Zobrazí se dialogové okno Konfigurace domény přenosu. (Toto dialogové okno se zobrazí pouze pokud jste ještě nenakonfigurovali přenosu domény.)
* Ujistěte se, tento uživatel hello automaticky vytvoří TMSADM oprávněný (SM59 -> ABAP připojení -> TMSADM@E61.DOMAIN_E61 -> Podrobnosti o -> Utilities(M) -> autorizace Test). úvodní obrazovka Hello transakce moduly STM by měl zobrazit, že tento systém SAP funguje teď jako hello řadiče domény hello přenosu, jak je vidět tady:

![Úvodní obrazovka transakce moduly STM na řadiči domény hello][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-hello-transport-domain"></a>Včetně SAP systémy v hello přenosu domény
posloupnost Hello včetně systému SAP v doméně přenosu vypadá takto:

* Na hello DEV systému v Azure přejděte systému pro přenos toohello (klient 000) a volání transakce moduly STM. Vyberte další konfiguraci z dialogového okna hello a pokračujte zahrnují systému v doméně. Zadejte hello řadiče domény jako cílový hostitel ([včetně SAP systémy v hello přenosu domény](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). Hello systému je nyní součástí domény přenosu hello toobe čekání.
* Z bezpečnostních důvodů budete mít tooconfirm řadič domény back toohello toogo vaši žádost. Zvolte Přehled systému a schválit hello čekání systému. Potvrďte, že konfigurace řádku a hello hello budou distribuována.

Tento systém SAP nyní obsahuje hello nezbytné informace o všech hello jiných systémů SAP v doméně přenosu hello. V hello stejný čas, hello adresu, kterou odešle data hello nového systému SAP tooall hello jiných systémů SAP a hello systému SAP je zadána v profilu přenosu hello hello přenos řízení programu. Zkontrolujte, zda dokumenty RFC a přístup k adresáři přenosu toohello hello domény fungovat.

Pokračovat v konfiguraci hello přenosu systému obvykle popsaného v dokumentaci k hello [změn a systému pro přenos](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm).

Postup:

* Ujistěte se, zda že je správně nakonfigurována vaše moduly STM místně.
* Ujistěte se, že název hostitele hello hello přenosu řadič domény se dají vyřešit virtuálního počítače v Azure a naopak karet visa.
* Volání transakce moduly STM -> Další konfigurace -> zahrnují systému v doméně.
* Zkontrolujte připojení hello v hello na místním TMS systému.
* Nakonfigurujte směrování přenosu, skupiny a vrstvy jako obvykle.

V připojených site-to-site mezi různými místy scénáře, hello latenci mezi místními a Azure stále může být výrazně. Pokud jsme postupujte podle hello posloupnost převod objekty prostřednictvím vývoj a testování tooproduction systémy nebo myslíte o použití přenosy nebo podporu balíčky toohello různými systémy, si myslíte, závisí na umístění hello hello centrální přenosu adresář, některé systémy hello se setkají vysokou latencí čtení nebo zápis dat v adresáři centrální přenosu hello. Hello situace je podobné tooSAP na šířku konfigurace, kde se šíří různými systémy hello prostřednictvím různých datových střediscích s dlouhou vzdálenost mezi datovými centry hello.

V pořadí toowork kolem takové latence a mají hello systémy fungovat Rychlé čtení nebo zápis tooor z adresáře hello přenosu, můžete nastavit dvěma doménami přenosu moduly STM (jeden pro místní. a jednu s hello systémy v Azure a odkaz doménách přenosu hello Zkontrolujte prosím tuto dokumentaci, která vysvětluje principy hello za tento koncept v hello SAP TMS: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/ 38dd924eb711d182bf0000e829fbfe/Frameset.htm>.

Postup:

* Nastavení domény přenosu v každém umístění (místní a Azure) pomocí transakce moduly STM <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Domény hello propojení s doménou propojit a potvrďte hello propojení mezi dvěma doménami hello.
  <http://help.SAP.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/Content.htm>
* Distribuujte hello konfigurace toohello propojené systému.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>RFC provoz mezi instancemi SAP, které jsou umístěné v Azure a místními (mezi různými místy)
RFC provoz mezi systémy, které jsou místně a v Azure potřebuje toowork. tooset připojení volání transakce SM59 ve zdrojovém systému kde je nutné toodefine připojení k RFC směrem hello cílového systému. Konfigurace Hello je podobné toohello standardní nastavení připojení k RFC.

Předpokládáme, v případě mezi různými místy hello hello hello virtuálních počítačů, které spuštění SAP systémy, které je třeba toocommunicate mezi sebou jsou ve stejné doméně. Proto hello nastavení připojení k RFC mezi systémy SAP neliší od hello kroky instalace a vstupy ve scénářích místně.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Přístupem 'local' fileshares z instance SAP umístěná v Azure nebo naopak
Instance SAP umístěný v Azure potřebovat tooaccess sdílené složky, které jsou v rámci podnikové místní hello. Kromě toho místní instance SAP třeba tooaccess sdílené složky, které jsou umístěny v Azure. tooenable hello sdílených složek je třeba nakonfigurovat hello oprávnění a sdílení možnosti na místní systém hello. Ujistěte se, že porty hello tooopen na hello VPN nebo ExpressRoute připojení mezi Azure a vaším datacentrem.

## <a name="supportability"></a>Možnosti podpory
### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Azure monitorování řešení pro SAP
V pořadí tooenable hello monitorování zvláště důležité systémy SAP na Azure hello SAP nástroje monitorování SAPOSCOL nebo Agent hostitele SAP získat data z hostitele virtuálního počítače služby Azure hello prostřednictvím rozšíření monitorování Azure pro SAP. Vzhledem k tomu, že požadavky hello podle SAP měla velmi konkrétní tooSAP aplikace, Microsoft rozhodli není toogenerically implementace hello požadované funkce do Azure, ale ponechte pro zákazníky toodeploy hello potřeby monitorování součástí a konfigurace tootheir Virtuální počítače běžící v Azure. Nasazení a životního cyklu správu hello monitorování součásti však bude možné většinou automatizovat, Azure.

#### <a name="solution-design"></a>Návrh řešení
řešení Hello vyvinuté tooenable SAP monitorování je založen na architektuře hello agenta virtuálního počítače Azure a rámce pro rozšíření. Rada Hello hello agenta virtuálního počítače Azure a rozšíření prostředí je tooallow instalaci softwaru aplikace k dispozici v galerii hello rozšíření virtuálního počítače Azure v rámci virtuálního počítače. Dobrý den zásadou, že je vhodné za tento koncept je tooallow (v případech, jako je hello rozšíření monitorování Azure pro SAP), hello nasazení speciální funkce do virtuálních počítačů a hello konfigurací takový software v době nasazení.

Hello 'Agent virtuálního počítače Azure, umožňující zpracování konkrétní rozšíření virtuálního počítače Azure v rámci hello virtuálního počítače je vloženy do virtuálních počítačů Windows ve výchozím nastavení při vytváření virtuálních počítačů v hello portálu Azure. V případě SUSE, Red Hat nebo Oracle Linux hello virtuálního počítače agent je již součástí bitové kopie Azure Marketplace. V případě, že jeden by nahrát virtuální počítač s Linuxem z agenta virtuálního počítače hello tooAzure místní má toobe nainstalován ručně.

Hello základní stavební bloky hello monitorování řešení v Azure pro SAP vypadá takto:

![Součástí rozšíření Microsoft Azure][planning-guide-figure-2400]

Jak je vidět v diagramu bloku hello výše, je jednou ze součástí sady řešení monitorování pro SAP hello hostitelem hello Image virtuálního počítače Azure a Azure Galerie rozšíření, která je globálně replikované úložiště, který je spravován nástrojem Azure Operations. Je zodpovědností hello hello společné práci na hello Azure implementace SAP toowork s Azure Operations toopublish nové verze hello rozšíření monitorování Azure pro SAP týmu SAP/MS.

Při nasazování nového virtuálního počítače Windows hello Agent virtuálního počítače Azure se automaticky přidá do hello virtuálních počítačů. Funkce Hello tohoto agenta je toocoordinate hello načítání a konfiguraci hello rozšíření Azure pro sledování systému SAP NetWeaver. Pro virtuální počítače s Linuxem hello agenta virtuálního počítače Azure už je součástí hello Azure Marketplace bitové kopie operačního systému.

Je však krok, který je pořád toobe provedený hello zákazníka. Toto je hello povolení a konfigurace hello výkonu kolekce. toohello konfigurace je automatizováno pomocí skriptu prostředí PowerShell nebo rozhraní příkazového řádku příkaz související s procesem Hello. Hello skript prostředí PowerShell lze stáhnout v hello Microsoft Azure Script Center, jak je popsáno v hello [Průvodce nasazením][deployment-guide].

Hello přehled architektury hello Azure monitorování řešení pro SAP vypadá takto:

![Řešení monitorování pro SAP NetWeaver Azure][planning-guide-figure-2500]

**Hello přesný postupy tooand podrobný postup pomocí těchto rutin prostředí PowerShell nebo rozhraní příkazového řádku příkaz během nasazení, postupujte podle pokynů hello v hello [Průvodce nasazením][deployment-guide].**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Integrace Azure nachází instance SAP do SAProuter
Instance SAP běžící v Azure potřebovat toobe přístupné z SAProuter také.

![Připojení k síti SAP-směrovač][planning-guide-figure-2600]

SAProuter umožňuje hello TCP/IP komunikaci mezi zúčastněné systémy, pokud neexistuje žádné přímé připojení IP. To poskytuje hello výhodu, že žádné připojení klient server mezi partnery komunikace hello je nutné na úrovni sítě. Hello SAProuter naslouchá na portu 3299 ve výchozím nastavení.
tooconnect SAP instance prostřednictvím SAProuter budete potřebovat toogive hello SAProuter řetězec a název hostitele s žádné tooconnect pokus.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver AS Java
Dosavadní hello fokus hello dokument byl SAP NetWeaver obecně nebo hello SAP NetWeaver ABAP zásobníku. V této části malé jsou uvedeny konkrétní aspekty hello SAP Java zásobníku. Jedním z nejdůležitějších SAP NetWeaver Java založený výhradně na aplikace hello je hello SAP podnikový portál. Další SAP NetWeaver na základě aplikací, jako SAP platformy a správce řešení SAP použít hello SAP NetWeaver ABAP i zásobníky Java. Proto určitě není nutné tooconsider specifické aspekty související toohello SAP NetWeaver Java zásobník také.

### <a name="sap-enterprise-portal"></a>SAP podnikového portálu
Instalační program Hello SAP portálu v virtuální počítač Azure se neliší od na místní instalace, pokud nasazujete ve scénářích mezi různými místy. Od hello DNS se provádí na místě lze provést nastavení portu hello jednotlivých instancí hello jako nakonfigurované na místě. Hello doporučení a omezení popsaná v tomto dokumentu, pokud platí pro aplikaci jako SAP podnikový portál nebo hello SAP NetWeaver Java zásobníku obecně.

![Portál zveřejněné SAP][planning-guide-figure-2700]

Scénář speciální nasazení podle někteří zákazníci je hello přímé expozice hello SAP podnikový portál toohello Internet, zatímco hello hostitele virtuálního počítače je připojený toohello podnikové síti pomocí tunelového připojení sítě VPN nebo ExpressRoute site-to-site. Pro tento případ máte toomake se, že určité porty jsou otevřené a není blokován bránou firewall nebo skupiny zabezpečení. Hello stejné mechanismy potřebovat toobe použijí, když chcete tooconnect tooan SAP Java instance z místní ve scénáři jenom pro Cloud.

Počáteční portál Hello identifikátor URI je http (s):`<Portalserver`>: 5XX00/irj, kde je hello port tvořen 50000 plus (Systemnumber x 100). Hello výchozí portálu URI SAP systém 00 je `<dns name`>.`<azure region` >.Cloudapp.azure.com:PublicPort/irj. Další informace, podívejte se na <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>.

![Konfigurace koncového bodu][planning-guide-figure-2800]

Pokud chcete toocustomize hello URL nebo porty SAP podnikového portálu, zkontrolujte Tato dokumentace:

* [Změnit adresu URL portálu](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Změňte výchozí čísla portů, čísla portů portálu](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)

## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Vysoké dostupnosti (HA) a obnovení po havárii (DR) pro SAP NetWeaver spuštěna ve virtuálních počítačích Azure
### <a name="definition-of-terminologies"></a>Definice terminologie jsou
termín Hello **vysokou dostupnost (HA)** je obecně související tooa sady technologií, minimalizuje narušení IT díky kontinuity podnikových procesů IT služeb prostřednictvím redundantní, odolný proti chybám nebo převzetí služeb při selhání chráněné komponenty uvnitř hello **stejné** datového centra. V našem případě v rámci jedné oblasti Azure.

**Zotavení po havárii (DR)** také cílí minimalizovat narušení služeb IT a jejich obnovení ale napříč **různých** datových center, které jsou obvykle umístěné stovky kilometrů rychle. V našem případě obvykle mezi různých oblastech Azure v rámci hello stejné geopolitické oblasti nebo jako navázat vy jako zákazník.

### <a name="overview-of-high-availability"></a>Přehled vysokou dostupnost
Jsme můžete oddělit hello diskuzi o SAP vysoké dostupnosti v Azure do dvou částí:

* **Vysoká dostupnost infrastruktury Azure**, například HA výpočetní (VM), sítě, úložiště atd. a jeho výhody pro zvýšení dostupnosti aplikací SAP.
* **Vysoká dostupnost aplikace SAP**, například HA systému SAP softwarové součásti:
  * SAP aplikační servery
  * Instance SAP ASC nebo SCS
  * Databázového serveru

a jak je možné kombinovat s infrastrukturu Azure HA.

SAP vysoké dostupnosti v Azure má některé rozdíly ve srovnání tooSAP vysoké dostupnosti v místním fyzickém nebo virtuálním prostředí. Hello v SAP následující dokumentu popisuje standardní konfigurace s vysokou dostupností SAP ve virtualizovaném prostředí v systému Windows: <http://scn.sap.com/docs/DOC-44415>. Neexistuje žádné integrované sapinst SAP-HA konfigurace pro Linux jako existuje pro systém Windows. O SAP HA místní pro Linux najít další informace: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Vysoká dostupnost infrastruktury Azure
Není aktuálně single-VM SLA 99,9 %. tooget představu, jak hello dostupnost jeden virtuální počítač může vypadat jako můžete jednoduše vytvořit hello produkt hello různé dostupné Azure SLA: <https://azure.microsoft.com/support/legal/sla/>.

Hello základ pro výpočet hello je 30 dní, měsíčně nebo 43 200 minut. Proto hodnotu 0,05 % výpadek odpovídá too21.6 minut. Dostupnost hello hello různé služby bude obvyklým způsobem, vynásobte v hello následujícím způsobem:

(Služba dostupnosti č. 1/100) * (služba dostupnosti č. 2/100) * (služba dostupnosti č. 3/100) *...

Například:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 nebo celkové dostupnosti 99.75 %.

#### <a name="virtual-machine-vm-high-availability"></a>Vysoká dostupnost virtuálního počítače (VM)
Existují dva typy událostí platformy Azure, které můžou ovlivnit hello dostupnosti virtuálních počítačů: plánované údržby a neplánovaná Údržba.

* Události plánované údržby jsou pravidelné aktualizace od společnosti Microsoft toohello základní platformu Azure tooimprove celkové spolehlivosti, výkonu a zabezpečení infrastruktury hello platformy, které virtuální počítače spustit na.
* Neplánovaná Údržba události dojít, když hello hardware nebo fyzické infrastruktuře základní virtuálního počítače došlo k chybě nějakým způsobem. To může zahrnovat selhání místní sítě, selhání místního disku nebo další selhání na úrovni racku. Když se taková selhání detekuje, hello platformy Azure automaticky migruje virtuální počítač z hello není v pořádku fyzického serveru hostování fyzického serveru virtuálního počítače tooa v pořádku. Tyto události vyskytují jen vzácně, ale může také způsobit vaší tooreboot virtuálního počítače.

Další podrobnosti najdete v této dokumentaci: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Redundance úložiště Azure
Hello data ve vašem účtu úložiště Microsoft Azure je vždy replikované tooensure odolnost a vysokou dostupnost, splňuje hello smlouvy SLA pro úložiště Azure i v hello vzhled selhání hardwaru.

Vzhledem k tomu, že Azure Storage je ve výchozím nastavení uchovávání tři Image hello dat, RAID5 nebo RAID1 na více disků Azure nejsou potřebné.

Další podrobnosti najdete v tomto článku: <http://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### <a name="utilizing-azure-infrastructure-vm-restart-tooachieve-higher-availability-of-sap-applications"></a>Využitím Azure infrastruktury virtuální počítač restartovat tooAchieve "Vyšší dostupnost" aplikací SAP
Pokud se rozhodnete není toouse funkce jako je Windows Server Failover Clustering (WSFC) nebo kardiostimulátor v systému Linux (momentálně podporována pouze pro SLES 12 a vyšší), restartovat virtuální počítač Azure je využívané tooprotect SAP systém proti plánovaných a neplánovaných výpadků hello Azure fyzický server infrastruktury a celkové základní platformy Azure.

> [!NOTE]
> Je důležité toomention, restartovat virtuální počítač Azure především chrání virtuální počítače a ne aplikace. Virtuální počítač restartovat nenabízí vysokou dostupnost pro aplikace SAP, ale nabízí systém určitá úroveň dostupnost infrastruktury a proto nepřímo "vyšší dostupnosti" systémů SAP. Je zde také žádné SLA hello dobu bude trvat toorestart virtuální počítač po plánovaném nebo neplánovaném hostitele výpadku. Tuto metodu, vysokou dostupnost, není proto vhodný pro důležité součásti systému SAP jako (A) SCS nebo databázového systému.
>
>

Jiný element důležité infrastruktury pro zajištění vysoké dostupnosti je úložiště. Smlouvy SLA pro úložiště Azure je třeba 99,9 % dostupnost. Pokud nasadí do jeden účet Azure Storage, potenciální Azure Storage nedostupnosti způsobí, že nejsou dostupné všechny virtuální počítače, které jsou umístěny v daném účtu úložiště Azure a spuštěna v rámci těchto virtuálních počítačů také všechny součásti SAP všech virtuálních počítačů s jeho disky.  

Místo uvedení všechny virtuální počítače do jedné jeden účet úložiště Azure, můžete použít také vyhrazeného úložiště účtů pro každý virtuální počítač a tímto způsobem zvýšit celkový dostupnosti virtuálních počítačů a SAP aplikace s použitím více nezávislých účtů úložiště Azure.

Disky systému Azure spravované jsou automaticky umístěny v hello doména selhání hello virtuálního počítače, které jsou připojené k. Zadáte-li dva virtuální počítače ve skupině dostupnosti nastavit a spravovat disky používat, platformu hello se postará o distribuci hello spravované disky do různých domén selhání také. Pokud máte v plánu toouse Storage úrovně Premium, důrazně doporučujeme také pomocí správy disků.

Ukázková architektura systému SAP NetWeaver, který používá infrastrukturu Azure HA a úložiště účtů může vypadat například takto:

![Využívá infrastrukturu Azure "vyšší" dostupnosti HA tooachieve SAP aplikace][planning-guide-figure-2900]

Ukázková architektura systému SAP NetWeaver používá infrastrukturu Azure HA a spravované disků může vypadat například takto:

![Využívá infrastrukturu Azure "vyšší" dostupnosti HA tooachieve SAP aplikace][planning-guide-figure-2901]

Pro důležité součásti SAP jsme dosáhnout následujících hello, pokud:

* Vysoká dostupnost SAP aplikační servery (AS)

  Instance serveru SAP aplikace jsou redundantní komponenty. Každý SAP jako instance je nasazen na svůj vlastní virtuální počítač, který běží v jiné Azure selhání a upgradu domény (najdete v kapitolách [domén selhání] [ planning-guide-3.2.1] a [upgradu domény][planning-guide-3.2.2]). To je zajištěno pomocí sad dostupnosti Azure (naleznete v kapitole [skupiny dostupnosti Azure][planning-guide-3.2.3]). Potenciální nedostupnosti plánovaném nebo neplánovaném selhání Azure nebo doména Upgrade způsobí nedostupnosti omezenému počtu virtuálních počítačů s jejich SAP AS instancí.

  Každý SAP jako instance je umístěn v svůj vlastní účet služby Azure Storage – potenciální nedostupnosti jeden účet úložiště Azure způsobí, že nejsou dostupné jenom jeden virtuální počítač s jeho SAP AS instance. Však Upozorňujeme, že je maximální počet účtů úložiště Azure v rámci jednoho předplatného Azure. Automatické spuštění tooensure (A) instance SCS po restartování počítače hello virtuálních počítačů, ujistěte se, profil popsané v kapitole spuštění tooset hello automatické spuštění parametr (A) instance SCS [pomocí automatické spuštění pro instance SAP] [ planning-guide-11.5].
  Přečtěte si také kapitoly [vysoká dostupnost pro SAP aplikační servery] [ planning-guide-11.4.1] další podrobnosti.

  I když používáte spravované disky, tyto disky jsou také uloženy v účtu úložiště Azure a může být k dispozici v případě výpadku úložiště.

* *Vyšší* instance SCS dostupnosti SAP (A)

  Zde jsme využívat restartovat virtuální počítač Azure tooprotect hello virtuálního počítače s nainstalovanou instanci SCS SAP (A). V servery technologie hello případě plánované, nebo neplánované výpadky Azure, restartuje virtuální počítače na jiný server k dispozici. Jak už bylo zmíněno dříve, restartovat virtuální počítač Azure především chrání virtuální počítače a ne aplikace, v tomto případě hello (A) SCS instance. Prostřednictvím hello restartovat virtuální počítač je budete spojit nepřímo "vyšší dostupnosti" instance SCS SAP (A). Automatické spuštění tooinsure (A) instance SCS po restartování počítače hello virtuálních počítačů, ujistěte se, profil popsané v kapitole spuštění tooset parametr automatické spuštění v instanci (A) SCS [pomocí automatické spuštění pro instance SAP][planning-guide-11.5]. To znamená, že hello (A) SCS jako jeden bod z selhání (SPOF) spuštěné v jeden virtuální počítač bude instance hello určujícího Multi-Factor hello dostupnost celou šířku SAP hello.

* *Vyšší* dostupnost serveru databázového systému

  Zde případ použití podobné instance toohello SCS SAP (A), jsme využití restartovat virtuální počítač Azure tooprotect hello virtuálních počítačů s nainstalovaným softwarem databázového systému a jsme dosáhnout "vyšší dostupnosti" databázového systému softwaru prostřednictvím restartování virtuálního počítače.
  Databázového systému spuštěné v jeden virtuální počítač je také SPOF a je hello určujícího Multi-Factor hello dostupnost celou šířku SAP hello.

### <a name="sap-application-high-availability-on-azure-iaas"></a>SAP vysoké dostupnosti aplikace na Azure IaaS
tooachieve úplné SAP systému vysokou dostupnost, potřebujeme tooprotect všechny důležité součásti systému SAP, pro příklad redundantní SAP aplikační servery a jedinečný součásti (například jediné místo poruchy) jako instance SCS SAP (A) a databázového systému.

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>Vysoká dostupnost pro SAP aplikační servery
Pro instancemi serverů nebo dialogové okno aplikace SAP hello není nezbytné toothink o řešení konkrétní vysokou dostupnost. Redundance a tím jednoduše dosáhnout vysoké dostupnosti s dostatek z nich v různých virtuálních počítačů. Budou všechny umístit do stejné skupiny dostupnosti Azure tooavoid, který hello virtuální počítače mohou být aktualizovány na hello hello současně během plánované údržby. výpadků. Hello základních funkcí, který je založen na jiné upgradu a domén selhání v rámci jednotky škálování Azure již byla zavedena v kapitole [upgradu domény][planning-guide-3.2.2]. Azure skupiny dostupnosti prezentovaly v kapitole [skupiny dostupnosti Azure] [ planning-guide-3.2.3] tohoto dokumentu.

Neexistuje žádné nekonečné počet selhání a upgradu domén, které je možné podle skupiny dostupnosti Azure, v rámci jednotky škálování služby Azure. To znamená, že uvedení počet virtuálních počítačů do jedné skupiny dostupnosti, dřív nebo později více než jeden virtuální počítač skončilo v hello stejné selhání nebo upgradu domény.

Nasazení několika aplikačního serveru SAP instancí v jejich vyhrazených virtuálních počítačích a za předpokladu, že My pět upgradu domény, na konci hello ukáže hello následující obrázek. Hello skutečný maximální počet domén selhání a aktualizace v rámci skupiny dostupnosti mohou změnit v budoucnu hello:

![HA SAP aplikačních serverů v Azure][planning-guide-figure-3000]

Další podrobnosti najdete v této dokumentaci: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="high-availability-for-hello-sap-ascs-instance-on-windows"></a>Vysoká dostupnost pro instance hello SCS SAP (A) v systému Windows
Windows Server Failover Cluster (WSFC) je často používané řešení tooprotect hello SCS SAP (A) instance. Také je integrovaný do sapinst v podobě "HA instalace". V tuto chvíli hello infrastrukturu Azure není možné tooprovide hello funkce tooset až hello hello požadované clusteru převzetí služeb při selhání systému Windows Server stejným způsobem, jak se provádí na místě.

Od ledna 2016 hello Azure Cloudová platforma operačním systémem Windows hello neposkytuje hello možnost použití sdíleného svazku clusteru na disku sdílena mezi dva virtuální počítače Azure.

Platným řešením, když je využití hello 3. stran softwaru, který poskytuje sdíleného svazku pomocí replikace synchronní a transparentní disku, který lze integrovat do služby WSFC. Tento postup znamená, že pouze hello aktivním uzlu clusteru je možné tooaccess jeden hello disk zkopíruje na bod v čase. Od ledna 2016 tento HA konfigurace je podporované tooprotect hello SAP (A) SCS instanci na Windows hostovaný operační systém na virtuálních počítačích Azure v kombinaci s 3rd softwarem s DataKeeper.

Hello s DataKeeper řešení poskytuje sdílený disk clusteru prostředků tooWindows clustery převzetí služeb při selhání tak, že:

* Další Azure virtuální pevný disk připojený tooeach hello virtuálních počítačů (VM), které jsou v konfiguraci clusteru se systémem Windows
* Spuštěná na obou uzlech virtuálních počítačů s DataKeeper Cluster Edition
* S s DataKeeper Cluster Edition nakonfigurovaný tak, že synchronně zrcadlí hello obsah hello další virtuální pevný disk připojený svazek ze zdrojové virtuální počítače tooadditional virtuální pevný disk připojený svazku cílovém virtuálním počítači.
* S DataKeeper je poskytuje abstrakci hello zdrojové a cílové místní svazky a jejich prezentace tooWindows clusteru převzetí služeb při selhání jako jeden sdílený disk.

Můžete najít všechny podrobnosti o tom, tooinstall clusteru s podporou převzetí služeb při selhání systému Windows se s DataKeeper a SAP v hello [Clustering SAP ASC instanci pomocí clusteru převzetí služeb při selhání systému Windows Server v Azure s DataKeeper s] [ ha-guide-classic]dokumentu white paper.

#### <a name="high-availability-for-hello-sap-ascs-instance-on-linux"></a>Vysoká dostupnost pro instance hello SCS SAP (A) v systému Linux
Od prosince 2015 také není žádný disk ekvivalentní tooshared služby WSFC pro virtuální počítače s Linuxem v Azure. Alternativní řešení pomocí softwaru 3rdstrany jako s pro systém Windows ještě se neověřuje SAP systémem Linux v Azure.

#### <a name="high-availability-for-hello-sap-database-instance"></a>Vysoká dostupnost pro instanci databáze SAP hello
Hello typické SAP databázového systému HA instalace je založena na dva databázového systému virtuální počítače, kde se používá funkce vysoké dostupnosti databázového systému tooreplicate data z hello active databázového systému instance toohello druhé virtuálních počítačů do pasivní instance databázového systému.

Vysoká dostupnost a po havárii funkci obnovení pro databázového systému v obecné, jakož i konkrétní databázového systému jsou popsané v hello [Průvodce nasazením databázového systému][dbms-guide].

#### <a name="end-to-end-high-availability-for-hello-complete-sap-system"></a>Hello začátku do konce vysoká dostupnost pro celý systém SAP
Následují dva příklady architektury dokončení SAP NetWeaver HA v Azure – jeden pro Windows a jeden pro Linux.

Nespravované jen disky: hello koncepty, jak je popsáno níže může být nutné toobe ohrožené trochu při nasazování mnoha systémů SAP a hello počet nasazených virtuálních počítačů jsou vyšší než maximální limit hello účtů úložiště na jedno předplatné. V takových případech virtuální pevné disky virtuálních počítačů potřebovat toobe kombinaci v rámci jednoho účtu úložiště. Obvykle lze provést kombinací virtuální pevné disky SAP aplikační vrstvu virtuální počítače různých systémů SAP.  Také jsme kombinaci různých virtuální pevné disky různé databázového systému virtuální počítače různých systémů SAP v jednom účtu úložiště Azure. Tím uchování hello IOPS omezení účtů úložiště Azure v paměti (<https://azure.microsoft.com/documentation/articles/storage-scalability-targets>)


##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] HA v systému Windows
![Architektura HA aplikace SAP NetWeaver s SQL serverem v Azure IaaS][planning-guide-figure-3200]

Následující Azure konstrukce Hello se používají pro hello SAP NetWeaver systém toominimize dopad na problémy infrastruktury a hostitele opravy:

* celý systém Hello je nasazen na Azure (požadováno – databázového systému vrstvy, (A) SCS instanci a kompletní aplikaci vrstvy nutné toorun v hello stejné umístění).
* celý systém Hello běží v rámci jednoho předplatného Azure (povinné).
* celý systém Hello běží v rámci jedné virtuální sítě Azure (povinné).
* Hello oddělení hello virtuální počítače jednoho systému SAP do tří skupiny dostupnosti je možné, i když všechny virtuální počítače hello patřící toohello stejné virtuální síti.
* Všechny virtuální počítače spuštěné instance databázového systému SAP systémů jsou v jedné skupině dostupnosti. Předpokládáme, že existuje více než jeden virtuální počítač spuštěné instance databázového systému na systém od nativní vysokou dostupnost databázového systému, které se používají funkce, jako je SQL Server AlwaysOn nebo Oracle Data Guard.
* Všechny virtuální počítače spuštěné instance databázového systému použít vlastní účet úložiště. Databázového systému souborů protokolu a data se replikují z jeden účet tooanother úložiště účet úložiště pomocí funkcí databázového systému vysoké dostupnosti, které synchronizují hello data. Nedostupnosti jeden účet úložiště způsobí, že nedostupnosti jednoho uzlu clusteru serveru SQL Windows, ale není hello celou služby SQL Server.
* Všechny virtuální počítače spuštěné instance (A) SCS jednoho systému SAP se nacházejí v jedné skupině dostupnosti. Uvnitř těchto virtuálních počítačů tooprotect hello (A) SCS instance je nakonfigurován Windows Server Failover Cluster (WSFC).
* Všechny virtuální počítače spuštěné instance (A) SCS použít vlastní účet úložiště. (A) SCS instance soubory a složku globální SAP se replikují z jednoho účtu tooanother úložiště účtu úložiště DataKeeper s replikací. Nedostupnosti jeden účet úložiště může způsobit nedostupnost jednoho (A) SCS Windows uzlu clusteru, ale není hello celý (A) SCS služby.
* VŠECHNY virtuální počítače hello představující vrstva hello SAP aplikačního serveru jsou ve třetí skupině dostupnosti.
* VŠECHNY virtuální počítače hello spuštěné SAP aplikační servery pomocí účtu úložiště. Nedostupnosti jeden SAP aplikačního serveru, kde další SAP AS pokračovat toorun způsobí, že jeden účet úložiště nejsou dostupné.

Hello následující obrázek ilustrované hello stejné šířku, používání spravované disků.

![Architektura HA aplikace SAP NetWeaver s SQL serverem v Azure IaaS][planning-guide-figure-3201]

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] HA v systému Linux
Hello architektura pro SAP HA v systému Linux v Azure je v podstatě stejný jako u systému Windows jako výše popsané hello. Od ledna 2016 neexistuje SAP (A) SCS HA řešení zatím nepodporuje v systému Linux v Azure

V důsledku jako leden 2016 nelze dosáhnout systému SAP. Linux Azure hello stejné dostupnosti jako systém SAP. Windows Azure z důvodu chybějících HA pro instanci SCS hello (A) a hello databázi SAP App Service Environment jedné instance.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Pomocí automatické spuštění pro instance SAP
SAP nabízí funkce hello instance SAP toostart okamžitě po spuštění hello hello operačního systému v rámci hello virtuálních počítačů. přesný postup Hello byly popsané v článku znalostní báze SAP [1909114]. Ale SAP není doporučujeme toouse hello nastavení už protože neexistuje ovládací prvek v pořadí hello restartování instance, za předpokladu, že tu vliv více než jeden virtuální počítač nebo více instancí spustili na virtuální počítač. Za předpokladu, že Azure Typický scénář v případě virtuálních počítačů a hello jednoho virtuálního počítače, nakonec získávání restartovat jedné instance serveru aplikace SAP, hello automatické spuštění není skutečně kritické a lze je povolit přidáním tento parametr:

    Autostart = 1

Do hello spustit profil hello instance SAP ABAP nebo Java.

> [!NOTE]
> Parametr automatické spuštění Hello může mít také některé downfalls. Podrobněji aktivační události parametr hello hello spuštění instance SAP ABAP nebo Java při spuštění služby Windows nebo Linuxem hello instance související s hello. Že se nestane hello jistě, pokud se spustí hello operačního systému. Ale restartování služby SAP jsou také běžné věcí, kterou funkce správy životního cyklu softwaru SAP jako součet nebo jiné aktualizace nebo aktualizace. Tyto funkce nejsou byla očekávána instanci toobe na všechny automaticky restartuje. Proto by mělo být zakázáno automatické spuštění parametr hello před spuštěním takových úloh. Parametr automatické spuštění Hello nesmí použít také pro instance SAP, které jsou Clusterované, jako jsou ASC/SCS/CI.
>
>

Zjistit další informace o automatické spuštění pro SAP instance tady:

* [Spuštění a zastavení SAP společně s vaší spuštění a zastavení serveru systému Unix](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Spuštění a zastavení SAP NetWeaver správy agentů](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Jak tooenable automatické spuštění HANA databáze](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)

### <a name="larger-3-tier-sap-systems"></a>Větší systémy SAP vrstvy 3
Aspekty vysoké dostupnosti 3vrstvé SAP konfigurace neobdrželo již popsané v předchozích částech. Ale co o systémech, kde jsou příliš velké požadavky na server hello databázového systému toohave umístěná v Azure, ale může být nasazený aplikační vrstvu hello SAP do Azure?

#### <a name="location-of-3-tier-sap-configurations"></a>Umístění konfigurace SAP vrstvy 3
Není podporované toosplit hello aplikační vrstvě sám sebe nebo aplikace hello a úroveň databázového systému mezi místními a Azure. Systému SAP je buď úplně nasadit místně nebo v Azure. Je také není podporované toohave některé hello aplikačních serverů spustit místně a jiná v Azure. To je výchozí bod hello diskusní hello. Můžeme také nejsou podpora toohave hello databázového systému součásti systému SAP a hello SAP aplikační server vrstvu nasazené ve dvou různých oblastech Azure. Například databázového systému v západní USA a SAP aplikační vrstvu střed USA. Důvodem nejsou podporovány tyto konfigurace je hello latence citlivosti hello SAP NetWeaver architektura.

Průběhu hello data poslední rok vyvinuté partnery center společné umístění tooAzure oblasti. Tyto společné umístění často se v velmi těsné blízkosti toohello fyzické dat Azure centrech v rámci oblast Azure. Čekací doba, která je menší než 2 MS může způsobit krátkou vzdálenost Hello a připojení prostředků ve společném umístění hello prostřednictvím ExpressRoute do Azure. V takových případech je možné toolocate hello databázového systému vrstvy (včetně úložiště SAN nebo NAS) na společném umístění a hello SAP aplikační vrstvu v Azure. Od prosince 2015 nemáme všechna nasazení jako je například. Ale různých zákazníků, jejichž nasazení aplikace bez SAP již používáte takové přístupy.

### <a name="offline-backup-of-sap-systems"></a>Offline systémy zálohování SAP
Závisí na hello SAP konfigurace vybrali (vrstvy 2 nebo 3 úrovně) existuje může být nutné tooback nahoru. obsah Hello hello virtuální počítač a toohave zálohu databáze hello. Hello související databázového systému zálohy jsou očekávané toobe provádět pomocí metod databáze. Podrobný popis pro hello různých databází, najdete v [databázového systému průvodce][dbms-guide]. Na dobrý den druhé straně, hello SAP data lze zálohovat offline způsobem (včetně hello databáze také obsah) jak je popsáno v této části nebo online jak je popsáno v další části hello.

zálohování offline Hello by vyžadovaly v podstatě vypnutí hello virtuálních počítačů prostřednictvím hello portál Azure a kopii hello základní disk virtuálního počítače a všechny připojené disky toohello virtuálních počítačů. To by zachovávají bod v bitové kopii čas hello virtuálního počítače a jeho přidružené disk. Toocopy hello "zálohování" se doporučuje na jiný účet úložiště Azure. Proto hello postup popsaný v kapitole [kopírování disků mezi účty úložiště Azure] [ planning-guide-5.4.2] by použití tohoto dokumentu.
Kromě hello vypnutí hello Azure pomocí portálu, jeden také provést pomocí prostředí Powershell nebo rozhraní příkazového řádku podle postupu popsaného tady: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Obnovení tohoto stavu by obsahovat odstranění hello základní virtuální počítač také jako hello původní disky hello základní virtuální počítač a připojené disky, kopírování zpět hello uložené disky toohello původní účet úložiště nebo skupinu prostředků, pro spravované disky a pak opětovného nasazení hello systému.
Tento článek ukazuje příklad, jak tooscript toto zpracování v prostředí Powershell: <http://www.westerndevs.com/azure-snapshots/>

Zkontrolujte, zda tooinstall novou licenci SAP vzhledem k tomu, že obnovování zálohování virtuálních počítačů, jak je popsáno výše vytvoří nový klíč hardwaru.

### <a name="online-backup-of-an-sap-system"></a>Online zálohování systému SAP
Zálohování hello databázového systému se provádí pomocí metody specifické databázového systému, jak je popsáno v hello [databázového systému průvodce][dbms-guide].

Ostatní virtuální počítače v rámci hello systému SAP lze zálohovat pomocí funkce zálohování virtuálních počítačů Azure. Zálohování virtuálního počítače Azure získali zavedená v rané fázi 2015 a mezitím je standardní metoda tooback až dokončení virtuálního počítače v Azure. Zálohování Azure ukládá hello záloh v Azure a umožňuje obnovení virtuálního počítače znovu.

> [!NOTE]
> Od prosince 2015 pomocí zálohování virtuálních počítačů by neměly být hello jedinečné ID virtuálního počítače, který se používá pro SAP licencování. To znamená, že obnovení ze zálohy virtuálního počítače vyžaduje instalaci nový licenční klíč SAP hello obnovit virtuální počítač se považuje toobe nového virtuálního počítače a není to náhrada bývalé ta, která byla uložena.
>
> ![Windows][Logo_Windows] Windows
>
> Teoreticky, virtuálních počítačů, které spuštění databáze lze zálohovat konzistentním způsobem také pokud hello databázového systému systém podporuje hello systému Windows VSS (služby Stínová kopie svazku <https://msdn.microsoft.com/library/windows/desktop/bb968832 (v=vs.85).aspx >) jako, například SQL Server neexistuje.
> Upozorňujeme však, založený na zálohování virtuálního počítače Azure, které v daném okamžiku obnoví databází nejsou možné. Proto doporučujeme zálohování tooperform databází s funkcemi databázového systému, aniž byste museli spoléhat na zálohování virtuálních počítačů Azure.
>
> tooget obeznámeni s zálohování virtuálních počítačů Azure, spusťte zde: <https://docs.microsoft.com/azure/backup/backup-azure-vms>.
>
> Další možnosti jsou toouse kombinaci aplikace Microsoft Data Protection Manager nainstalovat do virtuálního počítače Azure a Azure Backup k zálohování a obnovení databáze. Další informace naleznete zde: <https://docs.microsoft.com/azure/backup/backup-azure-dpm-introduction>.  
>
> ![Linux][Logo_Linux] Linux
>
> Není žádná ekvivalentní tooWindows služby Stínová kopie svazku v systému Linux. Pouze konzistentními soubory zálohy jsou proto možná, ale není konzistentní s aplikací zálohování. Hello databázového systému SAP zálohování by mělo být provedeno pomocí funkce databázového systému. Hello systému souborů, které zahrnují data související s SAP hello lze uložit, například pomocí vkládání popsaný tady: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>
>
>

### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Azure jako web zotavení po Havárii pro produkční prostředí SAP krajiny
Od Mid 2014 rozšíření toovarious součásti kolem technologie Hyper-V, System Center a Azure hello využití povolit ve službě Azure jako web zotavení po Havárii pro virtuální počítače spuštěné místní založené na technologii Hyper-V.

Blog s podrobnostmi o tom, jak toodeploy toto řešení je tady popisujeme: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>.

## <a name="summary"></a>Souhrn
Hello klíčové body vysoké dostupnosti pro SAP systémy v Azure jsou:

* V tuto chvíli, hello SAP jediný bod selhání nelze zabezpečit přesně hello stejný způsobem, jak je možné ji provést v místním nasazení. Hello důvodem je, že sdílené diskové clustery nelze ještě být vytvořené v Azure bez použití hello 3. stran softwaru.
* Pro vrstvu databázového systému hello musíte funkce toouse databázového systému, který není závislý na sdílený disk clusteru technologie. Podrobnosti jsou popsané v hello [databázového systému průvodce][dbms-guide].
* dopad hello toominimize problémy v rámci domén selhání v hello Azure infrastruktury nebo hostitele údržby, byste měli používat skupiny dostupnosti Azure:
  * Je doporučeno toohave, jednu sadu dostupnosti pro hello SAP aplikační vrstvu.
  * Doporučujeme toohave sadu samostatné dostupnosti pro vrstvu databázového systému SAP hello.
  * Doporučujeme není hello tooapply sadu stejné dostupnosti pro virtuální počítače různých systémů SAP.
  * Doporučujeme disky spravované toouse Premium.
* Pro účely zálohování hello databázového systému SAP vrstvy, zkontrolujte, zda text hello [databázového systému průvodce][dbms-guide].
* Zálohování instance SAP dialogové okno má málo smysl, protože je to obvykle rychlejší instancí tooredeploy jednoduché dialogové okno.
* Zálohování virtuálních počítačů, které obsahuje globální adresář hello hello systému SAP a s ním hello všechny profily hello různých instancí hello dává smysl a opakuje zálohování nebo, například funkce vkládání v systému Linux. Vzhledem k tomu, že jsou rozdíly mezi Windows Server 2008 (R2) a Windows Server 2012 (R2), které umožňují snadnější tooback díky hello uvolní novější systému Windows Server, doporučujeme toorun systému Windows Server 2012 (R2) jako Windows hostovaného operačního systému.
