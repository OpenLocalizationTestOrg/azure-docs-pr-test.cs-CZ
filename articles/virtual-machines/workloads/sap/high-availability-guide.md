---
title: "Azure vysoké dostupnosti virtuálních počítačů pro SAP NetWeaver | Microsoft Docs"
description: "Průvodce vysokou dostupností pro SAP NetWeaver ve virtuálních počítačích Azure"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/07/2016
ms.author: goraco
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 65236f527b62b4990b062fb6a54ce13b3c182e93
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms"></a><span data-ttu-id="6e167-103">Vysoká dostupnost pro SAP NetWeaver na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="6e167-103">High availability for SAP NetWeaver on Azure VMs</span></span>

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
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:https://docs.microsoft.com/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md
[dbms-guide-2.1]:../../virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:../../virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:../../virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:../../virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:../../virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:../../virtual-machines-windows-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:../../virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:../../virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:../../virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:../../virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:../../virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:../../virtual-machines-windows-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:../../virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:../../virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
[dbms-guide-900-sap-cache-server-on-premises]:../../virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:../../virtual-machines-windows-sap-deployment-guide.md
[deployment-guide-2.2]:../../virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:../../virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:../../virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:../../virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:../../virtual-machines-windows-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:../../virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:../../virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:../../virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:../../virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:../../virtual-machines-windows-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:../../virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:../../virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:../../virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:../../virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:../../virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:../../virtual-machines-windows-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:../../virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:../../virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:../../virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:../../virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:../../virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:../../virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:../../virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:../../virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:../../virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:../../virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:../../virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:../../virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../../../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../../../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:../../virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:../../virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:../../virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:../../virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:high-availability-guide.md

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
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:high-availability-guide.md
[sap-ha-guide-1]:high-availability-guide.md#217c5479-5595-4cd8-870d-15ab00d4f84c
[sap-ha-guide-2]:high-availability-guide.md#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-3]:high-availability-guide.md#42156640c6-01cf-45a9-b225-4baa678b24f1
[sap-ha-guide-3.1]:high-availability-guide.md#f76af273-1993-4d83-b12d-65deeae23686
[sap-ha-guide-3.2]:high-availability-guide.md#3e85fbe0-84b1-4892-87af-d9b65ff91860
[sap-ha-guide-4]:high-availability-guide.md#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-4.1]:high-availability-guide.md#1a3c5408-b168-46d6-99f5-4219ad1b1ff2
[sap-ha-guide-5]:high-availability-guide.md#fdfee875-6e66-483a-a343-14bbaee33275
[sap-ha-guide-5.1]:high-availability-guide.md#be21cf3e-fb01-402b-9955-54fbecf66592
[sap-ha-guide-5.2]:high-availability-guide.md#ff7a9a06-2bc5-4b20-860a-46cdb44669cd
[sap-ha-guide-6]:high-availability-guide.md#2ddba413-a7f5-4e4e-9a51-87908879c10a
[sap-ha-guide-6.1]:high-availability-guide.md#1a464091-922b-48d7-9d08-7cecf757f341
[sap-ha-guide-6.2]:high-availability-guide.md#44641e18-a94e-431f-95ff-303ab65e0bcb
[sap-ha-guide-7]:high-availability-guide.md#2e3fec50-241e-441b-8708-0b1864f66dfa
[sap-ha-guide-7.1]:high-availability-guide.md#93faa747-907e-440a-b00a-1ae0a89b1c0e
[sap-ha-guide-7.2]:high-availability-guide.md#f559c285-ee68-4eec-add1-f60fe7b978db
[sap-ha-guide-7.2.1]:high-availability-guide.md#b5b1fd0b-1db4-4d49-9162-de07a0132a51
[sap-ha-guide-7.3]:high-availability-guide.md#ddd878a0-9c2f-4b8e-8968-26ce60be1027
[sap-ha-guide-7.4]:high-availability-guide.md#045252ed-0277-4fc8-8f46-c5a29694a816
[sap-ha-guide-8]:high-availability-guide.md#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:high-availability-guide.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.2]:high-availability-guide.md#7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310
[sap-ha-guide-8.3]:high-availability-guide.md#47d5300a-a830-41d4-83dd-1a0d1ffdbe6a
[sap-ha-guide-8.4]:high-availability-guide.md#b22d7b3b-4343-40ff-a319-097e13f62f9e
[sap-ha-guide-8.5]:high-availability-guide.md#9fbd43c0-5850-4965-9726-2a921d85d73f
[sap-ha-guide-8.6]:high-availability-guide.md#84c019fe-8c58-4dac-9e54-173efd4b2c30
[sap-ha-guide-8.7]:high-availability-guide.md#7a8f3e9b-0624-4051-9e41-b73fff816a9e
[sap-ha-guide-8.8]:high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.9]:high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.10]:high-availability-guide.md#e69e9a34-4601-47a3-a41c-d2e11c626c0c
[sap-ha-guide-8.11]:high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:high-availability-guide.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:high-availability-guide.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.2]:high-availability-guide.md#e49a4529-50c9-4dcf-bde7-15a0c21d21ca
[sap-ha-guide-8.12.2.1]:high-availability-guide.md#06260b30-d697-4c4d-b1c9-d22c0bd64855
[sap-ha-guide-8.12.2.2]:high-availability-guide.md#4c08c387-78a0-46b1-9d27-b497b08cac3d
[sap-ha-guide-8.12.3]:high-availability-guide.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:high-availability-guide.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:high-availability-guide.md#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-ha-guide-9.1.2]:high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0
[sap-ha-guide-9.1.3]:high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556
[sap-ha-guide-9.1.4]:high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761
[sap-ha-guide-9.1.5]:high-availability-guide.md#4498c707-86c0-4cde-9c69-058a7ab8c3ac
[sap-ha-guide-9.2]:high-availability-guide.md#85d78414-b21d-4097-92b6-34d8bcb724b7
[sap-ha-guide-9.3]:high-availability-guide.md#8a276e16-f507-4071-b829-cdc0a4d36748
[sap-ha-guide-9.4]:high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a
[sap-ha-guide-9.5]:high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5
[sap-ha-guide-9.6]:high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772
[sap-ha-guide-10]:high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9
[sap-ha-guide-10.1]:high-availability-guide.md#65fdef0f-9f94-41f9-b314-ea45bbfea445
[sap-ha-guide-10.2]:high-availability-guide.md#5e959fa9-8fcd-49e5-a12c-37f6ba07b916
[sap-ha-guide-10.3]:high-availability-guide.md#755a6b93-0099-4533-9f6d-5c9a613878b5

[sap-ha-multi-sid-guide]:high-availability-multi-sid.md (SAP multi-SID high-availability configuration)


[sap-ha-guide-figure-1000]:media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
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
[virtual-machines-windows-attach-disk-portal]:../../virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../azure-resource-manager/resource-group-overview.md
[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-capture]:../../linux/capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:../../virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:../../virtual-machines-windows-sizes.md
[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md
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
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md


<span data-ttu-id="6e167-109">Virtuální počítače Azure je řešení pro organizace, které potřebují výpočty, úložiště a síťové prostředky ve minimálního čas a bez zdlouhavé nákup cykly.</span><span class="sxs-lookup"><span data-stu-id="6e167-109">Azure Virtual Machines is the solution for organizations that need compute, storage, and network resources, in minimal time, and without lengthy procurement cycles.</span></span> <span data-ttu-id="6e167-110">Virtuální počítače Azure můžete použít k nasazení classic aplikace jako na základě SAP NetWeaver ABAP, Java a ABAP + Java zásobníku.</span><span class="sxs-lookup"><span data-stu-id="6e167-110">You can use Azure Virtual Machines to deploy classic applications like SAP NetWeaver-based ABAP, Java, and an ABAP+Java stack.</span></span> <span data-ttu-id="6e167-111">Rozšiřte spolehlivosti a dostupnosti bez dalších místních prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e167-111">Extend reliability and availability without additional on-premises resources.</span></span> <span data-ttu-id="6e167-112">Virtuální počítače Azure podporuje připojení mezi různými místy, takže virtuální počítače Azure můžete integrovat do místní domény vaší organizace, privátních cloudů a šířku systému SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-112">Azure Virtual Machines supports cross-premises connectivity, so you can integrate Azure Virtual Machines into your organization's on-premises domains, private clouds, and SAP system landscape.</span></span>

<span data-ttu-id="6e167-113">V tomto článku jsme zahrnují kroky, které můžete provést nasazení SAP systémů s vysokou dostupností v Azure pomocí modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6e167-113">In this article, we cover the steps that you can take to deploy high-availability SAP systems in Azure by using the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="6e167-114">Můžeme vás provede procesem tyto hlavní úlohy:</span><span class="sxs-lookup"><span data-stu-id="6e167-114">We walk you through these major tasks:</span></span>

* <span data-ttu-id="6e167-115">Nalezení správné SAP poznámky a příručky pro instalaci, uvedené v [prostředky] [ sap-ha-guide-2] části.</span><span class="sxs-lookup"><span data-stu-id="6e167-115">Find the right SAP Notes and installation guides, listed in the [Resources][sap-ha-guide-2] section.</span></span> <span data-ttu-id="6e167-116">Tento článek doplňuje SAP instalace dokumentace a poznámky k SAP, což jsou primární prostředky, které vám mohou pomoci instalace a nasazení SAP software na specifické platformy.</span><span class="sxs-lookup"><span data-stu-id="6e167-116">This article complements SAP installation documentation and SAP Notes, which are the primary resources that can help you install and deploy SAP software on specific platforms.</span></span>
* <span data-ttu-id="6e167-117">Přečtěte si rozdíly mezi modelem nasazení Azure Resource Manager a modelu nasazení Azure classic.</span><span class="sxs-lookup"><span data-stu-id="6e167-117">Learn the differences between the Azure Resource Manager deployment model and the Azure classic deployment model.</span></span>
* <span data-ttu-id="6e167-118">Další informace o Windows Server Failover Clustering režimů kvora, takže můžete vybrat model, který je nejvhodnější pro vaše nasazení Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-118">Learn about Windows Server Failover Clustering quorum modes, so you can select the model that is right for your Azure deployment.</span></span>
* <span data-ttu-id="6e167-119">Další informace o Windows Server Failover Clustering sdíleného úložiště do služby Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-119">Learn about Windows Server Failover Clustering shared storage in Azure services.</span></span>
* <span data-ttu-id="6e167-120">Zjistěte, jak pomoci chránit jeden bod o selhání součásti jako Advanced obchodní aplikace programování (ABAP) SAP centrální služby (ASC) / SAP centrální služby (SCS) a databázové systémy (databázového systému) a redundantní komponenty, například aplikace SAP Server v Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-120">Learn how to help protect single-point-of-failure components like Advanced Business Application Programming (ABAP) SAP Central Services (ASCS)/SAP Central Services (SCS) and database management systems (DBMS), and redundant components like SAP Application Server, in Azure.</span></span>
* <span data-ttu-id="6e167-121">Postupujte podle podrobný příklad k instalaci a konfiguraci systému SAP vysoké dostupnosti v clusteru Windows Server Failover Clustering v Azure pomocí Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6e167-121">Follow a step-by-step example of an installation and configuration of a high-availability SAP system in a Windows Server Failover Clustering cluster in Azure by using Azure Resource Manager.</span></span>
* <span data-ttu-id="6e167-122">Další informace o dalších krocích, které jsou potřeba použít Windows Server Failover Clustering v Azure, ale které nejsou potřebné v místním nasazení.</span><span class="sxs-lookup"><span data-stu-id="6e167-122">Learn about additional steps required to use Windows Server Failover Clustering in Azure, but which are not needed in an on-premises deployment.</span></span>

<span data-ttu-id="6e167-123">Zjednodušit nasazení a konfigurace v tomto článku používáme šablony Resource Manageru SAP třívrstvá vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="6e167-123">To simplify deployment and configuration, in this article, we use the SAP three-tier high-availability Resource Manager templates.</span></span> <span data-ttu-id="6e167-124">Šablony automatizovat nasazení celé infrastruktury, které potřebujete pro SAP systém vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="6e167-124">The templates automate deployment of the entire infrastructure that you need for a high-availability SAP system.</span></span> <span data-ttu-id="6e167-125">Infrastruktura podporuje také SAP aplikace výkonu standardní (protokoly SAP) velikosti vašeho systému SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-125">The infrastructure also supports SAP Application Performance Standard (SAPS) sizing of your SAP system.</span></span>

## <span data-ttu-id="6e167-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="6e167-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a> Prerequisites</span></span>
<span data-ttu-id="6e167-127">Než začnete, ujistěte se, že splňujete požadavky, které jsou popsané v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="6e167-127">Before you start, make sure that you meet the prerequisites that are described in the following sections.</span></span> <span data-ttu-id="6e167-128">Také, nezapomeňte zkontrolovat všechny prostředky uvedené v [prostředky] [ sap-ha-guide-2] části.</span><span class="sxs-lookup"><span data-stu-id="6e167-128">Also, be sure to check all resources listed in the [Resources][sap-ha-guide-2] section.</span></span>

<span data-ttu-id="6e167-129">V tomto článku používáme šablon Azure Resource Manageru pro [třívrstvá SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/).</span><span class="sxs-lookup"><span data-stu-id="6e167-129">In this article, we use Azure Resource Manager templates for [three-tier SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/).</span></span> <span data-ttu-id="6e167-130">Užitečné Přehled šablon naleznete v tématu [šablon SAP Azure Resource Manageru](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span><span class="sxs-lookup"><span data-stu-id="6e167-130">For a helpful overview of templates, see [SAP Azure Resource Manager templates](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span></span>

## <span data-ttu-id="6e167-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Prostředky</span><span class="sxs-lookup"><span data-stu-id="6e167-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a> Resources</span></span>
<span data-ttu-id="6e167-132">Tyto články popisuje nasazení SAP v Azure:</span><span class="sxs-lookup"><span data-stu-id="6e167-132">These articles cover SAP deployments in Azure:</span></span>

* <span data-ttu-id="6e167-133">[Azure virtuálních počítačů, plánování a implementace pro SAP NetWeaver][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="6e167-133">[Azure Virtual Machines planning and implementation for SAP NetWeaver][planning-guide]</span></span>
* <span data-ttu-id="6e167-134">[Nasazení virtuálních počítačů Azure pro SAP NetWeaver][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="6e167-134">[Azure Virtual Machines deployment for SAP NetWeaver][deployment-guide]</span></span>
* <span data-ttu-id="6e167-135">[Nasazení virtuálních počítačů databázového systému Azure pro SAP NetWeaver][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="6e167-135">[Azure Virtual Machines DBMS deployment for SAP NetWeaver][dbms-guide]</span></span>
* <span data-ttu-id="6e167-136">[Azure vysoké dostupnosti virtuálních počítačů pro SAP NetWeaver (Tato příručka)][sap-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="6e167-136">[Azure Virtual Machines high availability for SAP NetWeaver (this guide)][sap-ha-guide]</span></span>

> [!NOTE]
> <span data-ttu-id="6e167-137">Kdykoli je to možné, můžeme vám dát odkaz odkazující Průvodce instalací SAP (najdete v článku [SAP instalace příručky][sap-installation-guides]).</span><span class="sxs-lookup"><span data-stu-id="6e167-137">Whenever possible, we give you a link to the referring SAP installation guide (see the [SAP installation guides][sap-installation-guides]).</span></span> <span data-ttu-id="6e167-138">Informace o procesu instalace a požadavky je vhodné si pozorně přečtěte příručky instalace SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="6e167-138">For prerequisites and information about the installation process, it's a good idea to read the SAP NetWeaver installation guides carefully.</span></span> <span data-ttu-id="6e167-139">Tento článek se týká pouze konkrétní úlohy pro počítače se systémem SAP NetWeaver, které můžete použít s virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-139">This article covers only specific tasks for SAP NetWeaver-based systems that you can use with Azure Virtual Machines.</span></span>
>
>

<span data-ttu-id="6e167-140">Tyto poznámky SAP souvisí s tématem SAP v Azure:</span><span class="sxs-lookup"><span data-stu-id="6e167-140">These SAP Notes are related to the topic of SAP in Azure:</span></span>

| <span data-ttu-id="6e167-141">Poznámka: číslo</span><span class="sxs-lookup"><span data-stu-id="6e167-141">Note number</span></span> | <span data-ttu-id="6e167-142">Název</span><span class="sxs-lookup"><span data-stu-id="6e167-142">Title</span></span> |
| --- | --- |
| <span data-ttu-id="6e167-143">[1928533]</span><span class="sxs-lookup"><span data-stu-id="6e167-143">[1928533]</span></span> |<span data-ttu-id="6e167-144">Aplikace SAP v Azure: podporované produkty a velikosti</span><span class="sxs-lookup"><span data-stu-id="6e167-144">SAP Applications on Azure: Supported Products and Sizing</span></span> |
| <span data-ttu-id="6e167-145">[2015553]</span><span class="sxs-lookup"><span data-stu-id="6e167-145">[2015553]</span></span> |<span data-ttu-id="6e167-146">SAP na platformě Microsoft Azure: podporovat požadavky</span><span class="sxs-lookup"><span data-stu-id="6e167-146">SAP on Microsoft Azure: Support Prerequisites</span></span> |
| <span data-ttu-id="6e167-147">[1999351]</span><span class="sxs-lookup"><span data-stu-id="6e167-147">[1999351]</span></span> |<span data-ttu-id="6e167-148">Rozšířené monitorování Azure pro SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-148">Enhanced Azure Monitoring for SAP</span></span> |
| <span data-ttu-id="6e167-149">[2178632]</span><span class="sxs-lookup"><span data-stu-id="6e167-149">[2178632]</span></span> |<span data-ttu-id="6e167-150">Klíč monitorování metriky pro SAP na platformě Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="6e167-150">Key Monitoring Metrics for SAP on Microsoft Azure</span></span> |
| <span data-ttu-id="6e167-151">[1999351]</span><span class="sxs-lookup"><span data-stu-id="6e167-151">[1999351]</span></span> |<span data-ttu-id="6e167-152">Virtualizace v systému Windows: rozšířené monitorování</span><span class="sxs-lookup"><span data-stu-id="6e167-152">Virtualization on Windows: Enhanced Monitoring</span></span> |
| <span data-ttu-id="6e167-153">[2243692]</span><span class="sxs-lookup"><span data-stu-id="6e167-153">[2243692]</span></span> |<span data-ttu-id="6e167-154">Používání úložiště Azure Premium SSD pro instanci databázového systému SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-154">Use of Azure Premium SSD Storage for SAP DBMS Instance</span></span> |

<span data-ttu-id="6e167-155">Další informace o [omezení předplatná Azure][azure-subscription-service-limits-subscription], včetně obecné výchozí omezení a maximální omezení.</span><span class="sxs-lookup"><span data-stu-id="6e167-155">Learn more about the [limitations of Azure subscriptions][azure-subscription-service-limits-subscription], including general default limitations and maximum limitations.</span></span>

## <span data-ttu-id="6e167-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Vysoká dostupnost SAP s Azure Resource Manager a modelu nasazení Azure classic</span><span class="sxs-lookup"><span data-stu-id="6e167-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>High-availability SAP with Azure Resource Manager vs. the Azure classic deployment model</span></span>
<span data-ttu-id="6e167-157">Azure Resource Manager a modelech nasazení Azure classic se liší v těchto oblastech:</span><span class="sxs-lookup"><span data-stu-id="6e167-157">The Azure Resource Manager and Azure classic deployment models are different in the following areas:</span></span>

- <span data-ttu-id="6e167-158">Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6e167-158">Resource groups</span></span>
- <span data-ttu-id="6e167-159">Závislost nástroje pro vyrovnávání zatížení Azure interní na skupina prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="6e167-159">Azure internal load balancer dependency on the Azure resource group</span></span>
- <span data-ttu-id="6e167-160">Podpora pro scénáře více SID SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-160">Support for SAP multi-SID scenarios</span></span>

### <span data-ttu-id="6e167-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6e167-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a> Resource groups</span></span>
<span data-ttu-id="6e167-162">V Azure Resource Manager, můžete použít skupin prostředků ke správě všechny prostředky aplikace ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-162">In Azure Resource Manager, you can use resource groups to manage all the application resources in your Azure subscription.</span></span> <span data-ttu-id="6e167-163">Integrovaný přístup, všechny prostředky mají stejný životní cyklus za ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e167-163">An integrated approach, in a resource group, all resources have the same life cycle.</span></span> <span data-ttu-id="6e167-164">Například všechny prostředky jsou vytvořeny ve stejnou dobu a jsou odstraněny ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="6e167-164">For example, all resources are created at the same time and they are deleted at the same time.</span></span> <span data-ttu-id="6e167-165">Další informace o [skupinách prostředků](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="6e167-165">Learn more about [resource groups](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>

### <span data-ttu-id="6e167-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Závislost nástroje pro vyrovnávání zatížení Azure interní na skupina prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="6e167-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a> Azure internal load balancer dependency on the Azure resource group</span></span>

<span data-ttu-id="6e167-167">V modelu nasazení Azure classic je závislost mezi nástroje pro vyrovnávání zatížení Azure interní (služba Vyrovnávání zatížení Azure) a skupinou služeb cloudu.</span><span class="sxs-lookup"><span data-stu-id="6e167-167">In the Azure classic deployment model, there is a dependency between the Azure internal load balancer (Azure Load Balancer service) and the cloud service group.</span></span> <span data-ttu-id="6e167-168">Každý nástroj pro vyrovnávání zatížení interní vyžaduje jednu skupinu cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6e167-168">Every internal load balancer needs one cloud service group.</span></span>

<span data-ttu-id="6e167-169">V Azure Resource Manager, není třeba použít nástroj pro vyrovnávání zatížení Azure skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-169">In Azure Resource Manager, you don't need an Azure resource group to use Azure Load Balancer.</span></span> <span data-ttu-id="6e167-170">Prostředí je jednodušší a flexibilnější.</span><span class="sxs-lookup"><span data-stu-id="6e167-170">The environment is simpler and more flexible.</span></span>

### <a name="support-for-sap-multi-sid-scenarios"></a><span data-ttu-id="6e167-171">Podpora pro scénáře více SID SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-171">Support for SAP multi-SID scenarios</span></span>

<span data-ttu-id="6e167-172">V Azure Resource Manager, můžete nainstalovat víc SAP systému identifikátor (SID) ASC nebo SCS instancí v jednom clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-172">In Azure Resource Manager, you can install multiple SAP system identifier (SID) ASCS/SCS instances in one cluster.</span></span> <span data-ttu-id="6e167-173">SID více instancí je možné, protože obsahuje podporu pro více IP adres pro každý nástroj pro vyrovnávání zatížení Azure interní.</span><span class="sxs-lookup"><span data-stu-id="6e167-173">Multi-SID instances are possible because of support for multiple IP addresses for each Azure internal load balancer.</span></span>

<span data-ttu-id="6e167-174">Model nasazení Azure classic, postupujte podle postupů popsaných v [SAP NetWeaver v Azure: instancí clusteringu ASC nebo SCS SAP pomocí služby Windows Server Failover Clustering v Azure pomocí s DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).</span><span class="sxs-lookup"><span data-stu-id="6e167-174">To use the Azure classic deployment model, follow the procedures described in [SAP NetWeaver in Azure: Clustering SAP ASCS/SCS instances by using Windows Server Failover Clustering in Azure with SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6e167-175">Důrazně doporučujeme použít model nasazení Azure Resource Manageru pro SAP instalací.</span><span class="sxs-lookup"><span data-stu-id="6e167-175">We strongly recommend that you use the Azure Resource Manager deployment model for your SAP installations.</span></span> <span data-ttu-id="6e167-176">Nabízí spoustu výhod, které nejsou k dispozici v modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="6e167-176">It offers many benefits that are not available in the classic deployment model.</span></span> <span data-ttu-id="6e167-177">Další informace o Azure [modely nasazení][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span><span class="sxs-lookup"><span data-stu-id="6e167-177">Learn more about Azure [deployment models][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span></span>   
>
>

## <span data-ttu-id="6e167-178"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server Failover Clustering</span><span class="sxs-lookup"><span data-stu-id="6e167-178"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a> Windows Server Failover Clustering</span></span>
<span data-ttu-id="6e167-179">Windows Server Failover Clustering je základem SAP ASC nebo SCS instalace vysokou dostupnost a databázového systému v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="6e167-179">Windows Server Failover Clustering is the foundation of a high-availability SAP ASCS/SCS installation and DBMS in Windows.</span></span>

<span data-ttu-id="6e167-180">Cluster s podporou převzetí služeb při selhání je skupina 1 + n nezávislých serverů (uzlů) které vzájemně spolupracují a zvyšují tak dostupnost aplikací a služeb.</span><span class="sxs-lookup"><span data-stu-id="6e167-180">A failover cluster is a group of 1+n independent servers (nodes) that work together to increase the availability of applications and services.</span></span> <span data-ttu-id="6e167-181">Pokud dojde k selhání uzlu, Windows Server Failover Clustering vypočítá počet chyb, ke kterým dochází při zachování pořádku cluster, který poskytuje aplikacím a službám.</span><span class="sxs-lookup"><span data-stu-id="6e167-181">If a node failure occurs, Windows Server Failover Clustering calculates the number of failures that can occur while maintaining a healthy cluster to provide applications and services.</span></span> <span data-ttu-id="6e167-182">Můžete z různých kvora režimy k dosažení clustering převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6e167-182">You can choose from different quorum modes to achieve failover clustering.</span></span>

### <span data-ttu-id="6e167-183"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Režim kvora</span><span class="sxs-lookup"><span data-stu-id="6e167-183"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a> Quorum modes</span></span>
<span data-ttu-id="6e167-184">Při použití služby Windows Server Failover Clustering, můžete vybrat ze čtyř režimů kvora:</span><span class="sxs-lookup"><span data-stu-id="6e167-184">You can choose from four quorum modes when you use Windows Server Failover Clustering:</span></span>

* <span data-ttu-id="6e167-185">**Většina uzlů**.</span><span class="sxs-lookup"><span data-stu-id="6e167-185">**Node Majority**.</span></span> <span data-ttu-id="6e167-186">Každý uzel clusteru hlasovat.</span><span class="sxs-lookup"><span data-stu-id="6e167-186">Each node of the cluster can vote.</span></span> <span data-ttu-id="6e167-187">Cluster funguje pouze s většinou hlasů, který je s víc než poloviny hlasy.</span><span class="sxs-lookup"><span data-stu-id="6e167-187">The cluster functions only with a majority of votes, that is, with more than half the votes.</span></span> <span data-ttu-id="6e167-188">Doporučujeme tuto možnost pro clustery, které mají lichý počet uzlů.</span><span class="sxs-lookup"><span data-stu-id="6e167-188">We recommend this option for clusters that have an uneven number of nodes.</span></span> <span data-ttu-id="6e167-189">Například může selhat tři uzly v clusteru s podporou sedmi a nepřesahuje clusteru dosahuje většině a pokračuje v činnosti.</span><span class="sxs-lookup"><span data-stu-id="6e167-189">For example, three nodes in a seven-node cluster can fail, and the cluster stills achieves a majority and continues to run.</span></span>  
* <span data-ttu-id="6e167-190">**Většina uzlů a disků**.</span><span class="sxs-lookup"><span data-stu-id="6e167-190">**Node and Disk Majority**.</span></span> <span data-ttu-id="6e167-191">Každý uzel a určený disk (disk s kopií clusteru) v úložišti clusteru můžete hlasovat, když jsou k dispozici a komunikace.</span><span class="sxs-lookup"><span data-stu-id="6e167-191">Each node and a designated disk (a disk witness) in the cluster storage can vote when they are available and in communication.</span></span> <span data-ttu-id="6e167-192">Cluster funguje pouze s většinou hlasů, který je s víc než poloviny hlasy.</span><span class="sxs-lookup"><span data-stu-id="6e167-192">The cluster functions only with a majority of the votes, that is, with more than half the votes.</span></span> <span data-ttu-id="6e167-193">Tento režim má smysl v prostředí clusteru s sudé číslo uzlů.</span><span class="sxs-lookup"><span data-stu-id="6e167-193">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="6e167-194">Pokud se polovina uzlů a disku jsou online, cluster zůstane v dobrém stavu.</span><span class="sxs-lookup"><span data-stu-id="6e167-194">If half the nodes and the disk are online, the cluster remains in a healthy state.</span></span>
* <span data-ttu-id="6e167-195">**Uzel a sdílených složek většina**.</span><span class="sxs-lookup"><span data-stu-id="6e167-195">**Node and File Share Majority**.</span></span> <span data-ttu-id="6e167-196">Každý uzel plus určené sdílené složky (soubor určující sdílenou složku), kterou vytvoří hlasovat, bez ohledu na to, jestli jsou k dispozici uzlů a sdílení souborů a v komunikaci.</span><span class="sxs-lookup"><span data-stu-id="6e167-196">Each node plus a designated file share (a file share witness) that the administrator creates can vote, regardless of whether the nodes and file share are available and in communication.</span></span> <span data-ttu-id="6e167-197">Cluster funguje pouze s většinou hlasů, který je s víc než poloviny hlasy.</span><span class="sxs-lookup"><span data-stu-id="6e167-197">The cluster functions only with a majority of the votes, that is, with more than half the votes.</span></span> <span data-ttu-id="6e167-198">Tento režim má smysl v prostředí clusteru s sudé číslo uzlů.</span><span class="sxs-lookup"><span data-stu-id="6e167-198">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="6e167-199">Je podobná režimu Většina uzlů a disků, ale používá určující sdílené složky namísto disku s kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-199">It's similar to the Node and Disk Majority mode, but it uses a witness file share instead of a witness disk.</span></span> <span data-ttu-id="6e167-200">Tento režim je snadno implementovat, ale pokud sdílená samotné není vysoce dostupný, může být jediný bod selhání.</span><span class="sxs-lookup"><span data-stu-id="6e167-200">This mode is easy to implement, but if the file share itself is not highly available, it might become a single point of failure.</span></span>
* <span data-ttu-id="6e167-201">**Bez většiny: Na disku jenom**.</span><span class="sxs-lookup"><span data-stu-id="6e167-201">**No Majority: Disk Only**.</span></span> <span data-ttu-id="6e167-202">Cluster má kvorum, pokud jeden uzel je k dispozici a komunikace s konkrétním diskem v úložišti clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-202">The cluster has a quorum if one node is available and in communication with a specific disk in the cluster storage.</span></span> <span data-ttu-id="6e167-203">Pouze uzly, které jsou také v komunikaci s tento disk může připojit ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-203">Only the nodes that also are in communication with that disk can join the cluster.</span></span> <span data-ttu-id="6e167-204">Doporučujeme vám, nepoužívejte tento režim.</span><span class="sxs-lookup"><span data-stu-id="6e167-204">We recommend that you do not use this mode.</span></span>
 

## <span data-ttu-id="6e167-205"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server převzetí služeb při selhání Clustering na místě</span><span class="sxs-lookup"><span data-stu-id="6e167-205"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a> Windows Server Failover Clustering on-premises</span></span>
<span data-ttu-id="6e167-206">Obrázek 1 ukazuje dva uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-206">Figure 1 shows a cluster of two nodes.</span></span> <span data-ttu-id="6e167-207">Pokud síťové připojení mezi uzly selže a jak zůstat uzly nahoru a spuštěna, disky kvora nebo soubor sdílet Určuje, který uzel bude pokračovat v poskytování aplikace a služby clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-207">If the network connection between the nodes fails and both nodes stay up and running, a quorum disk or file share determines which node will continue to provide the cluster's applications and services.</span></span> <span data-ttu-id="6e167-208">Uzel, který má přístup k disku nebo sdílené složky kvora je uzlu, který zajistí, že služby pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6e167-208">The node that has access to the quorum disk or file share is the node that ensures that services continue.</span></span>

<span data-ttu-id="6e167-209">Protože tento příklad používá dva uzly clusteru, použijeme Režim kvora Majority sdílené složky souborů a uzlu.</span><span class="sxs-lookup"><span data-stu-id="6e167-209">Because this example uses a two-node cluster, we use the Node and File Share Majority quorum mode.</span></span> <span data-ttu-id="6e167-210">Většina uzlů a disků je taky platná možnost.</span><span class="sxs-lookup"><span data-stu-id="6e167-210">The Node and Disk Majority also is a valid option.</span></span> <span data-ttu-id="6e167-211">V produkčním prostředí doporučujeme použít disk kvora.</span><span class="sxs-lookup"><span data-stu-id="6e167-211">In a production environment, we recommend that you use a quorum disk.</span></span> <span data-ttu-id="6e167-212">Technologie systému sítě a úložiště můžete nastavit jej jako vysoce dostupný.</span><span class="sxs-lookup"><span data-stu-id="6e167-212">You can use network and storage system technology to make it highly available.</span></span>

![Obrázek 1: Příklad konfigurace služby systému Windows Server Failover Clustering pro SAP ASC nebo SCS v Azure][sap-ha-guide-figure-1000]

<span data-ttu-id="6e167-214">_**Obrázek 1:** příklad konfigurace služby systému Windows Server Failover Clustering pro SAP ASC nebo SCS v Azure_</span><span class="sxs-lookup"><span data-stu-id="6e167-214">_**Figure 1:** Example of a Windows Server Failover Clustering configuration for SAP ASCS/SCS in Azure_</span></span>

### <span data-ttu-id="6e167-215"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Sdílené úložiště</span><span class="sxs-lookup"><span data-stu-id="6e167-215"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a> Shared storage</span></span>
<span data-ttu-id="6e167-216">Obrázek 1 zobrazuje i sdílené úložiště dvěma uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-216">Figure 1 also shows a two-node shared storage cluster.</span></span> <span data-ttu-id="6e167-217">V clusteru služby místní sdílené úložiště rozpoznat všechny uzly v clusteru sdíleného úložiště.</span><span class="sxs-lookup"><span data-stu-id="6e167-217">In an on-premises shared storage cluster, all nodes in the cluster detect shared storage.</span></span> <span data-ttu-id="6e167-218">Mechanismus uzamčení chrání data před poškozením.</span><span class="sxs-lookup"><span data-stu-id="6e167-218">A locking mechanism protects the data from corruption.</span></span> <span data-ttu-id="6e167-219">Všechny uzly, můžete zjistit, pokud jiný uzel selže.</span><span class="sxs-lookup"><span data-stu-id="6e167-219">All nodes can detect if another node fails.</span></span> <span data-ttu-id="6e167-220">V případě selhání jednoho uzlu na zbývající uzel převezme vlastnictví prostředků úložiště a zajišťuje dostupnost služby.</span><span class="sxs-lookup"><span data-stu-id="6e167-220">If one node fails, the remaining node takes ownership of the storage resources and ensures the availability of services.</span></span>

> [!NOTE]
> <span data-ttu-id="6e167-221">Nepotřebujete sdílené disky pro zajištění vysoké dostupnosti s některými aplikacemi databázového systému, třeba s SQL serverem.</span><span class="sxs-lookup"><span data-stu-id="6e167-221">You don't need shared disks for high availability with some DBMS applications, like with SQL Server.</span></span> <span data-ttu-id="6e167-222">SQL serveru Always On replikuje databázového systému souborů protokolu a data z místního disku jednoho uzlu na místní disk jiným uzlem clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-222">SQL Server Always On replicates DBMS data and log files from the local disk of one cluster node to the local disk of another cluster node.</span></span> <span data-ttu-id="6e167-223">Konfigurace clusteru systému Windows v takovém případě nemusí sdíleného disku.</span><span class="sxs-lookup"><span data-stu-id="6e167-223">In that case, the Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="6e167-224"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Sítě a překladu</span><span class="sxs-lookup"><span data-stu-id="6e167-224"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a> Networking and name resolution</span></span>
<span data-ttu-id="6e167-225">Klientské počítače přístup clusteru přes virtuální IP adresu a název virtuálního hostitele, který poskytuje DNS server.</span><span class="sxs-lookup"><span data-stu-id="6e167-225">Client computers reach the cluster over a virtual IP address and a virtual host name that the DNS server provides.</span></span> <span data-ttu-id="6e167-226">Uzly místně a DNS server může zpracovat více IP adres.</span><span class="sxs-lookup"><span data-stu-id="6e167-226">The on-premises nodes and the DNS server can handle multiple IP addresses.</span></span>

<span data-ttu-id="6e167-227">V typické instalace můžete používat dva nebo více připojení k síti:</span><span class="sxs-lookup"><span data-stu-id="6e167-227">In a typical setup, you use two or more network connections:</span></span>

* <span data-ttu-id="6e167-228">Vyhrazené připojení k úložišti</span><span class="sxs-lookup"><span data-stu-id="6e167-228">A dedicated connection to the storage</span></span>
* <span data-ttu-id="6e167-229">Cluster interní síťové připojení pro prezenční signál</span><span class="sxs-lookup"><span data-stu-id="6e167-229">A cluster-internal network connection for the heartbeat</span></span>
* <span data-ttu-id="6e167-230">Veřejné síti, který klienti používat pro připojení ke clusteru</span><span class="sxs-lookup"><span data-stu-id="6e167-230">A public network that clients use to connect to the cluster</span></span>

## <span data-ttu-id="6e167-231"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Server Failover Clustering v Azure</span><span class="sxs-lookup"><span data-stu-id="6e167-231"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a> Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="6e167-232">Porovnání s úplné nasazení systému nebo privátního cloudu, virtuálních počítačů Azure vyžaduje další kroky konfigurace, Windows Server Failover Clustering.</span><span class="sxs-lookup"><span data-stu-id="6e167-232">Compared to bare metal or private cloud deployments, Azure Virtual Machines requires additional steps to configure Windows Server Failover Clustering.</span></span> <span data-ttu-id="6e167-233">Při sestavování sdíleném disku clusteru, budete muset nastavit několik IP adresy a názvy virtuálních hostitelů pro SAP ASC nebo SCS instanci.</span><span class="sxs-lookup"><span data-stu-id="6e167-233">When you build a shared cluster disk, you need to set several IP addresses and virtual host names for the SAP ASCS/SCS instance.</span></span>

<span data-ttu-id="6e167-234">V tomto článku probereme klíčové koncepty a další kroky potřebné k vytvoření clusteru služby SAP centrální služby vysoké dostupnosti v Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-234">In this article, we discuss key concepts and the additional steps required to build an SAP high-availability central services cluster in Azure.</span></span> <span data-ttu-id="6e167-235">Ukážeme vám, jak nastavit nástroj třetí strany s DataKeeper a konfiguraci nástroje pro vyrovnávání zatížení Azure interní.</span><span class="sxs-lookup"><span data-stu-id="6e167-235">We show you how to set up the third-party tool SIOS DataKeeper, and how to configure the Azure internal load balancer.</span></span> <span data-ttu-id="6e167-236">Tyto nástroje slouží k vytvoření clusteru s podporou převzetí služeb při selhání systému Windows s určující sdílené složce v Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-236">You can use these tools to create a Windows failover cluster with a file share witness in Azure.</span></span>

![Obrázek 2: Windows Server Failover Clustering konfiguraci v Azure bez sdíleného disku][sap-ha-guide-figure-1001]

<span data-ttu-id="6e167-238">_**Obrázek 2:** konfiguraci služby Windows Server Failover Clustering v Azure bez sdíleného disku_</span><span class="sxs-lookup"><span data-stu-id="6e167-238">_**Figure 2:** Windows Server Failover Clustering configuration in Azure without a shared disk_</span></span>

### <span data-ttu-id="6e167-239"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>Sdílený disk v Azure s DataKeeper s</span><span class="sxs-lookup"><span data-stu-id="6e167-239"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a> Shared disk in Azure with SIOS DataKeeper</span></span>
<span data-ttu-id="6e167-240">Třeba clusteru sdíleného úložiště pro instanci SAP ASC nebo SCS vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="6e167-240">You need cluster shared storage for a high-availability SAP ASCS/SCS instance.</span></span> <span data-ttu-id="6e167-241">Od září 2016 není Azure nabízí sdílené úložiště, které můžete použít k vytvoření clusteru sdíleného úložiště.</span><span class="sxs-lookup"><span data-stu-id="6e167-241">As of September 2016, Azure doesn't offer shared storage that you can use to create a shared storage cluster.</span></span> <span data-ttu-id="6e167-242">Software třetích stran s DataKeeper Cluster Edition slouží k vytvoření zrcadlené úložiště, která simuluje sdílené úložiště clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-242">You can use third-party software SIOS DataKeeper Cluster Edition to create a mirrored storage that simulates cluster shared storage.</span></span> <span data-ttu-id="6e167-243">Řešení SIOS poskytuje data v reálném čase synchronní replikace.</span><span class="sxs-lookup"><span data-stu-id="6e167-243">The SIOS solution provides real-time synchronous data replication.</span></span> <span data-ttu-id="6e167-244">Toto je, jak můžete vytvořit prostředek sdíleného disku pro cluster:</span><span class="sxs-lookup"><span data-stu-id="6e167-244">This is how you can create a shared disk resource for a cluster:</span></span>

1. <span data-ttu-id="6e167-245">Připojte další Azure virtuální pevný disk (VHD) pro každý z virtuálních počítačů (VM) v konfiguraci clusteru systému Windows.</span><span class="sxs-lookup"><span data-stu-id="6e167-245">Attach an additional Azure virtual hard disk (VHD) to each of the virtual machines (VMs) in a Windows cluster configuration.</span></span>
2. <span data-ttu-id="6e167-246">V obou uzlech virtuální počítač spusťte s DataKeeper Cluster Edition.</span><span class="sxs-lookup"><span data-stu-id="6e167-246">Run SIOS DataKeeper Cluster Edition on both virtual machine nodes.</span></span>
3. <span data-ttu-id="6e167-247">Nakonfigurujte s DataKeeper Cluster Edition, aby ho zrcadlí obsah další virtuální pevný disk připojený svazek ze zdrojového virtuálního počítače na další svazek připojen virtuální pevný disk cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6e167-247">Configure SIOS DataKeeper Cluster Edition so that it mirrors the content of the additional VHD attached volume from the source virtual machine to the additional VHD attached volume of the target virtual machine.</span></span> <span data-ttu-id="6e167-248">S DataKeeper abstrahuje zdrojové a cílové místní svazky a k jejich zobrazení na Windows Server Failover Clustering jako jeden sdílený disk.</span><span class="sxs-lookup"><span data-stu-id="6e167-248">SIOS DataKeeper abstracts the source and target local volumes, and then presents them to Windows Server Failover Clustering as one shared disk.</span></span>

<span data-ttu-id="6e167-249">Přečtěte si další informace o [s DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span><span class="sxs-lookup"><span data-stu-id="6e167-249">Get more information about [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span></span>

![Obrázek 3: Konfigurace služby systému Windows Server Failover Clustering v Azure pomocí DataKeeper s][sap-ha-guide-figure-1002]

<span data-ttu-id="6e167-251">_**Obrázek 3:** konfigurace služby systému Windows Server Failover Clustering v Azure pomocí DataKeeper s_</span><span class="sxs-lookup"><span data-stu-id="6e167-251">_**Figure 3:** Windows Server Failover Clustering configuration in Azure with SIOS DataKeeper_</span></span>

> [!NOTE]
> <span data-ttu-id="6e167-252">Pro zajištění vysoké dostupnosti se některé produkty, databázového systému, jako je SQL Server není třeba sdílené disky.</span><span class="sxs-lookup"><span data-stu-id="6e167-252">You don't need shared disks for high availability with some DBMS products, like SQL Server.</span></span> <span data-ttu-id="6e167-253">SQL serveru Always On replikuje databázového systému souborů protokolu a data z místního disku jednoho uzlu na místní disk jiným uzlem clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-253">SQL Server Always On replicates DBMS data and log files from the local disk of one cluster node to the local disk of another cluster node.</span></span> <span data-ttu-id="6e167-254">Konfigurace clusteru systému Windows v takovém případě nemusí sdíleného disku.</span><span class="sxs-lookup"><span data-stu-id="6e167-254">In this case, the Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="6e167-255"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>Rozpoznání názvu v Azure</span><span class="sxs-lookup"><span data-stu-id="6e167-255"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a> Name resolution in Azure</span></span>
<span data-ttu-id="6e167-256">Azure Cloudová platforma nenabízí možnost konfigurovat virtuální IP adresy, například plovoucí IP adresy.</span><span class="sxs-lookup"><span data-stu-id="6e167-256">The Azure cloud platform doesn't offer the option to configure virtual IP addresses, such as floating IP addresses.</span></span> <span data-ttu-id="6e167-257">Je třeba nastavit virtuální IP adresy k dosažení prostředek clusteru v cloudu alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="6e167-257">You need an alternative solution to set up a virtual IP address to reach the cluster resource in the cloud.</span></span>
<span data-ttu-id="6e167-258">Azure má k nástroji pro vyrovnávání zatížení pro vnitřní ve službě Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-258">Azure has an internal load balancer in the Azure Load Balancer service.</span></span> <span data-ttu-id="6e167-259">S nástrojem pro vyrovnávání zatížení pro vnitřní klienti moci clusteru připojit přes virtuální IP adresu clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-259">With the internal load balancer, clients reach the cluster over the cluster virtual IP address.</span></span>
<span data-ttu-id="6e167-260">Je třeba nasadit nástroj pro vyrovnávání zatížení interní ve skupině prostředků, který obsahuje uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-260">You need to deploy the internal load balancer in the resource group that contains the cluster nodes.</span></span> <span data-ttu-id="6e167-261">Potom nakonfigurujte všechny nezbytné port předávání pravidla s sondy porty nástroje pro vyrovnávání zatížení interní.</span><span class="sxs-lookup"><span data-stu-id="6e167-261">Then, configure all necessary port forwarding rules with the probe ports of the internal load balancer.</span></span>
<span data-ttu-id="6e167-262">Můžete se klienti připojují prostřednictvím název virtuálního hostitele.</span><span class="sxs-lookup"><span data-stu-id="6e167-262">The clients can connect via the virtual host name.</span></span> <span data-ttu-id="6e167-263">DNS server přeloží IP adresu clusteru a port obslužných rutin vyrovnávání interní služby load předávání do aktivního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-263">The DNS server resolves the cluster IP address, and the internal load balancer handles port forwarding to the active node of the cluster.</span></span>

## <span data-ttu-id="6e167-264"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>SAP NetWeaver vysoké dostupnosti v Azure infrastruktury jako služba (IaaS)</span><span class="sxs-lookup"><span data-stu-id="6e167-264"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a> SAP NetWeaver high availability in Azure Infrastructure-as-a-Service (IaaS)</span></span>
<span data-ttu-id="6e167-265">K dosažení vysoké dostupnosti aplikace SAP pro SAP softwarové komponenty, jako je třeba chránit následující součásti:</span><span class="sxs-lookup"><span data-stu-id="6e167-265">To achieve SAP application high availability, such as for SAP software components, you need to protect the following components:</span></span>

* <span data-ttu-id="6e167-266">Instance aplikačního serveru SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-266">SAP Application Server instance</span></span>
* <span data-ttu-id="6e167-267">Instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="6e167-267">SAP ASCS/SCS instance</span></span>
* <span data-ttu-id="6e167-268">Server databázového systému</span><span class="sxs-lookup"><span data-stu-id="6e167-268">DBMS server</span></span>

<span data-ttu-id="6e167-269">Další informace o ochraně SAP součásti ve scénářích s vysokou dostupností najdete v tématu [virtuální počítače Azure plánování a implementace pro SAP NetWeaver](planning-guide.md).</span><span class="sxs-lookup"><span data-stu-id="6e167-269">For more information about protecting SAP components in high-availability scenarios, see [Azure Virtual Machines planning and implementation for SAP NetWeaver](planning-guide.md).</span></span>

### <span data-ttu-id="6e167-270"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Vysoká dostupnost SAP aplikační Server</span><span class="sxs-lookup"><span data-stu-id="6e167-270"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a> High-availability SAP Application Server</span></span>
<span data-ttu-id="6e167-271">Obvykle nepotřebujete specifického řešení vysoké dostupnosti pro instance aplikačního serveru SAP a dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e167-271">You usually don't need a specific high-availability solution for the SAP Application Server and dialog instances.</span></span> <span data-ttu-id="6e167-272">Dosažení vysoké dostupnosti pomocí redundance a nakonfigurujete více instancí dialogové okno v různých instancí virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-272">You achieve high availability by redundancy, and you'll configure multiple dialog instances in different instances of Azure Virtual Machines.</span></span> <span data-ttu-id="6e167-273">Měli byste mít aspoň dvě instance SAP aplikace nainstalované v dvě instance virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-273">You should have at least two SAP application instances installed in two instances of Azure Virtual Machines.</span></span>

![Obrázek 4: Vysoké dostupnosti SAP aplikační Server][sap-ha-guide-figure-2000]

<span data-ttu-id="6e167-275">_**Obrázek 4:** vysokou dostupnost SAP aplikační Server_</span><span class="sxs-lookup"><span data-stu-id="6e167-275">_**Figure 4:** High-availability SAP Application Server_</span></span>

<span data-ttu-id="6e167-276">Je nutné umístit všechny virtuální počítače, které instance aplikačního serveru SAP hostitele ve stejném Azure dostupnosti nastavit.</span><span class="sxs-lookup"><span data-stu-id="6e167-276">You must place all virtual machines that host SAP Application Server instances in the same Azure availability set.</span></span> <span data-ttu-id="6e167-277">Nastavení Azure dostupnosti zajišťuje, že:</span><span class="sxs-lookup"><span data-stu-id="6e167-277">An Azure availability set ensures that:</span></span>

* <span data-ttu-id="6e167-278">Všechny virtuální počítače jsou součástí stejné domény upgradu.</span><span class="sxs-lookup"><span data-stu-id="6e167-278">All virtual machines are part of the same upgrade domain.</span></span> <span data-ttu-id="6e167-279">Upgradovací doméně, například zajišťuje, že virtuální počítače se neaktualizují ve stejnou dobu během plánované údržby. výpadků.</span><span class="sxs-lookup"><span data-stu-id="6e167-279">An upgrade domain, for example, makes sure that the virtual machines aren't updated at the same time during planned maintenance downtime.</span></span>
* <span data-ttu-id="6e167-280">Všechny virtuální počítače jsou součástí stejné domény selhání.</span><span class="sxs-lookup"><span data-stu-id="6e167-280">All virtual machines are part of the same fault domain.</span></span> <span data-ttu-id="6e167-281">Domény selhání, například zajišťuje nasazení virtuálních počítačů, tak, aby žádný jediný bod selhání má vliv na dostupnost všech virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e167-281">A fault domain, for example, makes sure that virtual machines are deployed so that no single point of failure affects the availability of all virtual machines.</span></span>

<span data-ttu-id="6e167-282">Další informace o tom, jak [Správa dostupnosti virtuálních počítačů][virtual-machines-manage-availability].</span><span class="sxs-lookup"><span data-stu-id="6e167-282">Learn more about how to [manage the availability of virtual machines][virtual-machines-manage-availability].</span></span>

<span data-ttu-id="6e167-283">Účet úložiště Azure je potenciální jediný bod selhání, je důležité mít alespoň dva účty úložiště Azure, ve kterých jsou distribuovány aspoň dva virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="6e167-283">Because the Azure storage account is a potential single point of failure, it's important to have at least two Azure storage accounts, in which at least two virtual machines are distributed.</span></span> <span data-ttu-id="6e167-284">V ideální instalační program by se nasadí disky každého virtuálního počítače, na kterém běží instance SAP dialogové okno jiný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6e167-284">In an ideal setup, the disks of each virtual machine that is running an SAP dialog instance would be deployed in a different storage account.</span></span>

### <span data-ttu-id="6e167-285"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Instance SAP ASC nebo SCS vysokou dostupnost</span><span class="sxs-lookup"><span data-stu-id="6e167-285"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a> High-availability SAP ASCS/SCS instance</span></span>
<span data-ttu-id="6e167-286">Obrázek 5 je příklad instance SAP ASC nebo SCS vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="6e167-286">Figure 5 is an example of a high-availability SAP ASCS/SCS instance.</span></span>

![Obrázek 5: Vysoká dostupnost SAP ASC nebo SCS instance][sap-ha-guide-figure-2001]

<span data-ttu-id="6e167-288">_**Obrázek 5:** vysokou dostupnost SAP ASC nebo SCS instance_</span><span class="sxs-lookup"><span data-stu-id="6e167-288">_**Figure 5:** High-availability SAP ASCS/SCS instance_</span></span>

#### <span data-ttu-id="6e167-289"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>SAP ASC nebo SCS instance vysoká dostupnost s Windows Server Failover Clustering v Azure</span><span class="sxs-lookup"><span data-stu-id="6e167-289"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a> SAP ASCS/SCS instance high availability with Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="6e167-290">Porovnání s úplné nasazení systému nebo privátního cloudu, virtuálních počítačů Azure vyžaduje další kroky konfigurace, Windows Server Failover Clustering.</span><span class="sxs-lookup"><span data-stu-id="6e167-290">Compared to bare metal or private cloud deployments, Azure Virtual Machines requires additional steps to configure Windows Server Failover Clustering.</span></span> <span data-ttu-id="6e167-291">K vytvoření clusteru s podporou převzetí služeb při selhání systému Windows, musíte pro clustering instance SAP ASC nebo SCS sdíleném disku clusteru, několik IP adres, několik názvy virtuálních hostitelů a k nástroji pro vyrovnávání zatížení Azure interní.</span><span class="sxs-lookup"><span data-stu-id="6e167-291">To build a Windows failover cluster, you need a shared cluster disk, several IP addresses, several virtual host names, and an Azure internal load balancer for clustering an SAP ASCS/SCS instance.</span></span> <span data-ttu-id="6e167-292">To probereme podrobněji později v článku.</span><span class="sxs-lookup"><span data-stu-id="6e167-292">We discuss this in more detail later in the article.</span></span>

![Obrázek 6: Windows Server Failover Clustering pro konfiguraci SAP ASC nebo SCS v Azure pomocí DataKeeper s][sap-ha-guide-figure-1002]

<span data-ttu-id="6e167-294">_**Obrázek 6:** Windows Server Failover Clustering SAP ASC nebo SCS konfigurace v Azure pomocí DataKeeper s_</span><span class="sxs-lookup"><span data-stu-id="6e167-294">_**Figure 6:** Windows Server Failover Clustering for an SAP ASCS/SCS configuration in Azure with SIOS DataKeeper_</span></span>

### <span data-ttu-id="6e167-295"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Instance databázového systému vysokou dostupnost</span><span class="sxs-lookup"><span data-stu-id="6e167-295"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>High-availability DBMS instance</span></span>
<span data-ttu-id="6e167-296">Systému správy databáze je také jeden kontaktní bod v systému SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-296">The DBMS also is a single point of contact in an SAP system.</span></span> <span data-ttu-id="6e167-297">Potřebujete chránit pomocí řešení vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="6e167-297">You need to protect it by using a high-availability solution.</span></span> <span data-ttu-id="6e167-298">Obrázek 7 znázorňuje řešení vysoké dostupnosti SQL serveru Always On v Azure, Windows Server Failover Clustering a Azure interní nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6e167-298">Figure 7 shows a SQL Server Always On high-availability solution in Azure, with Windows Server Failover Clustering and the Azure internal load balancer.</span></span> <span data-ttu-id="6e167-299">SQL serveru Always On replikuje pomocí replikace vlastní databázového systému databázového systému data a soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="6e167-299">SQL Server Always On replicates DBMS data and log files by using its own DBMS replication.</span></span> <span data-ttu-id="6e167-300">V takovém případě můžete nebudete potřebovat clusteru sdílené disky, což zjednodušuje celý nastavení.</span><span class="sxs-lookup"><span data-stu-id="6e167-300">In this case, you don't need cluster shared disks, which simplifies the entire setup.</span></span>

![Obrázek 7: Příklad databázového systému vysoké dostupnosti SAP, s SQL serveru Always On][sap-ha-guide-figure-2003]

<span data-ttu-id="6e167-302">_**Obrázek 7:** příklad databázového systému vysoké dostupnosti SAP, s SQL serveru Always On_</span><span class="sxs-lookup"><span data-stu-id="6e167-302">_**Figure 7:** Example of a high-availability SAP DBMS, with SQL Server Always On_</span></span>

<span data-ttu-id="6e167-303">Další informace o clusteringu SQL Server v Azure pomocí modelu nasazení Azure Resource Manager najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="6e167-303">For more information about clustering SQL Server in Azure by using the Azure Resource Manager deployment model, see these articles:</span></span>

* <span data-ttu-id="6e167-304">[Konfigurace skupiny dostupnosti Always On v Azure Virtual Machines ručně pomocí Resource Manageru][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span><span class="sxs-lookup"><span data-stu-id="6e167-304">[Configure Always On availability group in Azure Virtual Machines manually by using Resource Manager][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span></span>
* <span data-ttu-id="6e167-305">[Konfigurace k nástroji pro vyrovnávání zatížení Azure interní pro skupiny dostupnosti Always On v Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]</span><span class="sxs-lookup"><span data-stu-id="6e167-305">[Configure an Azure internal load balancer for an Always On availability group in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]</span></span>

## <span data-ttu-id="6e167-306"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>Scénáře nasazení vysoké dostupnosti začátku do konce</span><span class="sxs-lookup"><span data-stu-id="6e167-306"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a> End-to-end high-availability deployment scenarios</span></span>

### <a name="deployment-scenario-using-architectural-template-1"></a><span data-ttu-id="6e167-307">Scénář nasazení pomocí architektury šablony 1</span><span class="sxs-lookup"><span data-stu-id="6e167-307">Deployment scenario using Architectural Template 1</span></span>

<span data-ttu-id="6e167-308">Obrázek 8 ukazuje příklad architektury SAP NetWeaver vysoké dostupnosti v Azure pro **jeden** systému SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-308">Figure 8 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="6e167-309">Tento scénář je nastavit takto:</span><span class="sxs-lookup"><span data-stu-id="6e167-309">This scenario is set up as follows:</span></span>

- <span data-ttu-id="6e167-310">Jeden vyhrazený cluster se používá pro SAP ASC nebo SCS instanci.</span><span class="sxs-lookup"><span data-stu-id="6e167-310">One dedicated cluster is used for the SAP ASCS/SCS instance.</span></span>
- <span data-ttu-id="6e167-311">Jeden vyhrazený cluster se používá pro instanci databázového systému.</span><span class="sxs-lookup"><span data-stu-id="6e167-311">One dedicated cluster is used for the DBMS instance.</span></span>
- <span data-ttu-id="6e167-312">Instance aplikačního serveru SAP nasazených ve svých vlastních vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="6e167-312">SAP Application Server instances are deployed in their own dedicated VMs.</span></span>

![Obrázek 8: SAP vysokou dostupnost architektury šablony 1 vyhrazeném clusteru pro ASC nebo SCS a databázového systému][sap-ha-guide-figure-2004]

<span data-ttu-id="6e167-314">_**Obrázek 8:** SAP architektury 1 šablony vysokou dostupnost, vyhrazené clustery pro ASC nebo SCS a databázového systému_</span><span class="sxs-lookup"><span data-stu-id="6e167-314">_**Figure 8:** SAP high-availability Architectural Template 1, dedicated clusters for ASCS/SCS and DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-2"></a><span data-ttu-id="6e167-315">Scénář nasazení pomocí architektury šablony 2</span><span class="sxs-lookup"><span data-stu-id="6e167-315">Deployment scenario using Architectural Template 2</span></span>

<span data-ttu-id="6e167-316">Obrázek 9 ukazuje příklad architektury SAP NetWeaver vysoké dostupnosti v Azure pro **jeden** systému SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-316">Figure 9 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="6e167-317">Tento scénář je nastavit takto:</span><span class="sxs-lookup"><span data-stu-id="6e167-317">This scenario is set up as follows:</span></span>

- <span data-ttu-id="6e167-318">Jeden vyhrazený cluster se používá pro **i** instance SAP ASC nebo SCS a databázového systému.</span><span class="sxs-lookup"><span data-stu-id="6e167-318">One dedicated cluster is used for **both** the SAP ASCS/SCS instance and the DBMS.</span></span>
- <span data-ttu-id="6e167-319">Instance aplikačního serveru SAP jsou nasazeny v vlastní vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="6e167-319">SAP Application Server instances are deployed in own dedicated VMs.</span></span>

![Obrázek 9: SAP vysokou dostupnost architektury šablony 2, přičemž vyhrazeném clusteru pro ASC nebo SCS a vyhrazeném clusteru pro databázového systému][sap-ha-guide-figure-2005]

<span data-ttu-id="6e167-321">_**Obrázek 9:** SAP vysokou dostupnost architektury šablony 2, přičemž vyhrazeném clusteru pro ASC nebo SCS a vyhrazeném clusteru pro databázového systému_</span><span class="sxs-lookup"><span data-stu-id="6e167-321">_**Figure 9:** SAP high-availability Architectural Template 2, with a dedicated cluster for ASCS/SCS and a dedicated cluster for DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-3"></a><span data-ttu-id="6e167-322">Scénář nasazení pomocí architektury 3 šablony</span><span class="sxs-lookup"><span data-stu-id="6e167-322">Deployment scenario using Architectural Template 3</span></span>

<span data-ttu-id="6e167-323">Obrázek 10 ukazuje příklad architektury SAP NetWeaver vysoké dostupnosti v Azure pro **dva** SAP systémy, s &lt;SID1&gt; a &lt;SID2&gt;.</span><span class="sxs-lookup"><span data-stu-id="6e167-323">Figure 10 shows an example of an SAP NetWeaver high-availability architecture in Azure for **two** SAP systems, with &lt;SID1&gt; and &lt;SID2&gt;.</span></span> <span data-ttu-id="6e167-324">Tento scénář je nastavit takto:</span><span class="sxs-lookup"><span data-stu-id="6e167-324">This scenario is set up as follows:</span></span>

- <span data-ttu-id="6e167-325">Jeden vyhrazený cluster se používá pro **i** instance SAP ASC nebo SCS SID1 *a* instance SAP ASC nebo SCS SID2 (jeden cluster).</span><span class="sxs-lookup"><span data-stu-id="6e167-325">One dedicated cluster is used for **both** the SAP ASCS/SCS SID1 instance *and* the SAP ASCS/SCS SID2 instance (one cluster).</span></span>
- <span data-ttu-id="6e167-326">Jeden vyhrazený cluster se používá pro SID1 databázového systému a jiné vyhrazeném clusteru se používá pro SID2 databázového systému (dva clustery).</span><span class="sxs-lookup"><span data-stu-id="6e167-326">One dedicated cluster is used for DBMS SID1, and another dedicated cluster is used for DBMS SID2 (two clusters).</span></span>
- <span data-ttu-id="6e167-327">Instance aplikačního serveru SAP systému SAP SID1 mít vlastní vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="6e167-327">SAP Application Server instances for the SAP system SID1 have their own dedicated VMs.</span></span>
- <span data-ttu-id="6e167-328">Instance aplikačního serveru SAP systému SAP SID2 mít vlastní vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="6e167-328">SAP Application Server instances for the SAP system SID2 have their own dedicated VMs.</span></span>

![Obrázek 10: Vysoká dostupnost architektury šablony 3, s clusterem vyhrazené pro různé ASC nebo SCS instance SAP][sap-ha-guide-figure-6003]

<span data-ttu-id="6e167-330">_**Obrázek 10:** SAP vysokou dostupnost architektury šablony 3, s clusterem vyhrazené pro různé instance ASC nebo SCS_</span><span class="sxs-lookup"><span data-stu-id="6e167-330">_**Figure 10:** SAP high-availability Architectural Template 3, with a dedicated cluster for different ASCS/SCS instances_</span></span>

## <span data-ttu-id="6e167-331"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>Příprava infrastruktury</span><span class="sxs-lookup"><span data-stu-id="6e167-331"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a> Prepare the infrastructure</span></span>

### <a name="prepare-the-infrastructure-for-architectural-template-1"></a><span data-ttu-id="6e167-332">Příprava infrastruktury pro architektury šablony 1</span><span class="sxs-lookup"><span data-stu-id="6e167-332">Prepare the infrastructure for Architectural Template 1</span></span>
<span data-ttu-id="6e167-333">Šablony Azure Resource Manageru pro SAP zjednodušit nasazení požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="6e167-333">Azure Resource Manager templates for SAP help simplify deployment of required resources.</span></span>

<span data-ttu-id="6e167-334">Třívrstvá šablony ve službě Správce prostředků Azure také podporují scénáře vysoké dostupnosti, například architektury 1 šablony, která má dva clustery.</span><span class="sxs-lookup"><span data-stu-id="6e167-334">The three-tier templates in Azure Resource Manager also support high-availability scenarios, such as in Architectural Template 1, which has two clusters.</span></span> <span data-ttu-id="6e167-335">Každý cluster je SAP jediný bod selhání pro SAP ASC nebo SCS a databázového systému.</span><span class="sxs-lookup"><span data-stu-id="6e167-335">Each cluster is an SAP single point of failure for SAP ASCS/SCS and DBMS.</span></span>

<span data-ttu-id="6e167-336">Zde je, kde můžete získat šablon Azure Resource Manageru ukázkový scénář, který jsme popisují v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="6e167-336">Here's where you can get Azure Resource Manager templates for the example scenario we describe in this article:</span></span>

* [<span data-ttu-id="6e167-337">Obrázek pro Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6e167-337">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [<span data-ttu-id="6e167-338">Vlastní image</span><span class="sxs-lookup"><span data-stu-id="6e167-338">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

<span data-ttu-id="6e167-339">Příprava infrastruktury na architektury šablona 1:</span><span class="sxs-lookup"><span data-stu-id="6e167-339">To prepare the infrastructure for Architectural Template 1:</span></span>

- <span data-ttu-id="6e167-340">Na portálu Azure na **parametry** okno v **SYSTEMAVAILABILITY** vyberte **HA**.</span><span class="sxs-lookup"><span data-stu-id="6e167-340">In the Azure portal, on the **Parameters** blade, in the **SYSTEMAVAILABILITY** box, select **HA**.</span></span>

  ![Obrázek 11: Nastavit SAP vysokou dostupnost Azure Resource Manageru parametry][sap-ha-guide-figure-3000]

<span data-ttu-id="6e167-342">_**Obrázek 11:** nastavit parametry SAP vysokou dostupnost Azure Resource Manager_</span><span class="sxs-lookup"><span data-stu-id="6e167-342">_**Figure 11:** Set SAP high-availability Azure Resource Manager parameters_</span></span>


  <span data-ttu-id="6e167-343">Vytvoření šablony:</span><span class="sxs-lookup"><span data-stu-id="6e167-343">The templates create:</span></span>

  * <span data-ttu-id="6e167-344">**Virtuální počítače**:</span><span class="sxs-lookup"><span data-stu-id="6e167-344">**Virtual machines**:</span></span>
    * <span data-ttu-id="6e167-345">Virtuální počítače aplikačního serveru SAP: <*SAPSystemSID*> - di - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="6e167-345">SAP Application Server virtual machines: <*SAPSystemSID*>-di-<*Number*></span></span>
    * <span data-ttu-id="6e167-346">ASC nebo SCS clusteru virtuálních počítačů: <*SAPSystemSID*> - ASC - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="6e167-346">ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-ascs-<*Number*></span></span>
    * <span data-ttu-id="6e167-347">Cluster databázového systému: <*SAPSystemSID*> - db - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="6e167-347">DBMS cluster: <*SAPSystemSID*>-db-<*Number*></span></span>

  * <span data-ttu-id="6e167-348">**Síťové karty pro všechny virtuální počítače s přidružené IP adresy**:</span><span class="sxs-lookup"><span data-stu-id="6e167-348">**Network cards for all virtual machines, with associated IP addresses**:</span></span>
    * <span data-ttu-id="6e167-349"><*SAPSystemSID*> - nic - di - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="6e167-349"><*SAPSystemSID*>-nic-di-<*Number*></span></span>
    * <span data-ttu-id="6e167-350"><*SAPSystemSID*> - nic - ASC - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="6e167-350"><*SAPSystemSID*>-nic-ascs-<*Number*></span></span>
    * <span data-ttu-id="6e167-351"><*SAPSystemSID*> - nic - db - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="6e167-351"><*SAPSystemSID*>-nic-db-<*Number*></span></span>

  * <span data-ttu-id="6e167-352">**Účty úložiště Azure**</span><span class="sxs-lookup"><span data-stu-id="6e167-352">**Azure storage accounts**</span></span>

  * <span data-ttu-id="6e167-353">**Skupiny dostupnosti** pro:</span><span class="sxs-lookup"><span data-stu-id="6e167-353">**Availability groups** for:</span></span>
    * <span data-ttu-id="6e167-354">Virtuální počítače aplikačního serveru SAP: <*SAPSystemSID*> - avset - di</span><span class="sxs-lookup"><span data-stu-id="6e167-354">SAP Application Server virtual machines: <*SAPSystemSID*>-avset-di</span></span>
    * <span data-ttu-id="6e167-355">SAP ASC nebo SCS clusteru virtuálních počítačů: <*SAPSystemSID*> - avset - ASC</span><span class="sxs-lookup"><span data-stu-id="6e167-355">SAP ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-avset-ascs</span></span>
    * <span data-ttu-id="6e167-356">Virtuální počítače clusteru databázového systému: <*SAPSystemSID*> - avset - db</span><span class="sxs-lookup"><span data-stu-id="6e167-356">DBMS cluster virtual machines: <*SAPSystemSID*>-avset-db</span></span>

  * <span data-ttu-id="6e167-357">**Nástroje pro vyrovnávání zatížení Azure interní**:</span><span class="sxs-lookup"><span data-stu-id="6e167-357">**Azure internal load balancer**:</span></span>
    * <span data-ttu-id="6e167-358">S všechny porty pro instanci ASC nebo SCS a IP adresu <*SAPSystemSID*> - lb - ASC</span><span class="sxs-lookup"><span data-stu-id="6e167-358">With all ports for the ASCS/SCS instance and IP address <*SAPSystemSID*>-lb-ascs</span></span>
    * <span data-ttu-id="6e167-359">S všechny porty pro SQL Server databázového systému a IP adresu <*SAPSystemSID*> - lb - db</span><span class="sxs-lookup"><span data-stu-id="6e167-359">With all ports for the SQL Server DBMS and IP address <*SAPSystemSID*>-lb-db</span></span>

  * <span data-ttu-id="6e167-360">**Skupina zabezpečení sítě**: <*SAPSystemSID*> - nsg - ASC-0</span><span class="sxs-lookup"><span data-stu-id="6e167-360">**Network security group**: <*SAPSystemSID*>-nsg-ascs-0</span></span>  
    * <span data-ttu-id="6e167-361">S k externí protokol RDP (Remote Desktop) portu k <*SAPSystemSID*> - ASC - 0 virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6e167-361">With an open external Remote Desktop Protocol (RDP) port to the <*SAPSystemSID*>-ascs-0 virtual machine</span></span>

> [!NOTE]
> <span data-ttu-id="6e167-362">Jsou všechny IP adresy síťové karty a nástroje pro vyrovnávání zatížení Azure interní **dynamické** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6e167-362">All IP addresses of the network cards and Azure internal load balancers are **dynamic** by default.</span></span> <span data-ttu-id="6e167-363">Změnit tak, aby **statické** IP adresy.</span><span class="sxs-lookup"><span data-stu-id="6e167-363">Change them to **static** IP addresses.</span></span> <span data-ttu-id="6e167-364">Jsme popisují, jak to udělat později v článku.</span><span class="sxs-lookup"><span data-stu-id="6e167-364">We describe how to do this later in the article.</span></span>
>
>

### <span data-ttu-id="6e167-365"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Nasazení virtuálních počítačů s připojením k podnikové síti (mezi různými místy) používat v produkčním prostředí</span><span class="sxs-lookup"><span data-stu-id="6e167-365"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Deploy virtual machines with corporate network connectivity (cross-premises) to use in production</span></span>
<span data-ttu-id="6e167-366">Pro produkční systémy SAP, nasaďte virtuální počítače Azure s [připojení k podnikové síti (mezi různými místy)] [ planning-guide-2.2] pomocí Azure Site-to-Site VPN nebo Azure ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6e167-366">For production SAP systems, deploy Azure virtual machines with [corporate network connectivity (cross-premises)][planning-guide-2.2] by using Azure Site-to-Site VPN or Azure ExpressRoute.</span></span>

> [!NOTE]
> <span data-ttu-id="6e167-367">Můžete použít instanci Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="6e167-367">You can use your Azure Virtual Network instance.</span></span> <span data-ttu-id="6e167-368">Virtuální síť a podsíť již byly vytvořeny a připravený.</span><span class="sxs-lookup"><span data-stu-id="6e167-368">The virtual network and subnet have already been created and prepared.</span></span>
>
>

1.  <span data-ttu-id="6e167-369">Na portálu Azure na **parametry** okno v **NEWOREXISTINGSUBNET** vyberte **existující**.</span><span class="sxs-lookup"><span data-stu-id="6e167-369">In the Azure portal, on the **Parameters** blade, in the **NEWOREXISTINGSUBNET** box, select **existing**.</span></span>
2.  <span data-ttu-id="6e167-370">V **SUBNETID** pole, přidejte úplný řetězec připravené Azure sítě SubnetID, kam je plánujete nasadit virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-370">In the **SUBNETID** box, add the full string of your prepared Azure network SubnetID where you plan to deploy your Azure virtual machines.</span></span>
3.  <span data-ttu-id="6e167-371">Chcete-li získat seznam všech podsítí síť Azure, spusťte tento příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6e167-371">To get a list of all Azure network subnets, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  <span data-ttu-id="6e167-372">**ID** pole ukazuje **SUBNETID**.</span><span class="sxs-lookup"><span data-stu-id="6e167-372">The **ID** field shows the **SUBNETID**.</span></span>
4. <span data-ttu-id="6e167-373">Chcete-li získat seznam všech **SUBNETID** hodnoty, spusťte tento příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6e167-373">To get a list of all **SUBNETID** values, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  <span data-ttu-id="6e167-374">**SUBNETID** vypadá podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="6e167-374">The **SUBNETID** looks like this:</span></span>

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

### <span data-ttu-id="6e167-375"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Nasadit jenom pro cloud instance SAP pro testovací a demo</span><span class="sxs-lookup"><span data-stu-id="6e167-375"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Deploy cloud-only SAP instances for test and demo</span></span>
<span data-ttu-id="6e167-376">Můžete nasadit systém SAP vysoké dostupnosti v modelu nasazení jenom cloudu.</span><span class="sxs-lookup"><span data-stu-id="6e167-376">You can deploy your high-availability SAP system in a cloud-only deployment model.</span></span> <span data-ttu-id="6e167-377">Tento typ nasazení je primárně užitečné pro ukázku a testovací případy použití.</span><span class="sxs-lookup"><span data-stu-id="6e167-377">This kind of deployment primarily is useful for demo and test use cases.</span></span> <span data-ttu-id="6e167-378">Ji není vhodná pro produkční případy použití.</span><span class="sxs-lookup"><span data-stu-id="6e167-378">It's not suited for production use cases.</span></span>

- <span data-ttu-id="6e167-379">Na portálu Azure na **parametry** okno v **NEWOREXISTINGSUBNET** vyberte **nové**.</span><span class="sxs-lookup"><span data-stu-id="6e167-379">In the Azure portal, on the **Parameters** blade, in the **NEWOREXISTINGSUBNET** box, select **new**.</span></span> <span data-ttu-id="6e167-380">Ponechte **SUBNETID** pole prázdná.</span><span class="sxs-lookup"><span data-stu-id="6e167-380">Leave the **SUBNETID** field empty.</span></span>

  <span data-ttu-id="6e167-381">Šablona SAP Azure Resource Manager automaticky vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="6e167-381">The SAP Azure Resource Manager template automatically creates the Azure virtual network and subnet.</span></span>

> [!NOTE]
> <span data-ttu-id="6e167-382">Musíte také nasadit alespoň jeden vyhrazený virtuální počítač pro Active Directory a DNS ve stejné instanci Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="6e167-382">You also need to deploy at least one dedicated virtual machine for Active Directory and DNS in the same Azure Virtual Network instance.</span></span> <span data-ttu-id="6e167-383">Šablona nepodporuje vytvoření těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e167-383">The template doesn't create these virtual machines.</span></span>
>
>


### <a name="prepare-the-infrastructure-for-architectural-template-2"></a><span data-ttu-id="6e167-384">Příprava infrastruktury pro architektury šablony 2</span><span class="sxs-lookup"><span data-stu-id="6e167-384">Prepare the infrastructure for Architectural Template 2</span></span>

<span data-ttu-id="6e167-385">Tato šablona Azure Resource Manageru pro SAP můžete zjednodušit nasazování požadované infrastruktury prostředků pro SAP architektury šablony 2.</span><span class="sxs-lookup"><span data-stu-id="6e167-385">You can use this Azure Resource Manager template for SAP to help simplify deployment of required infrastructure resources for SAP Architectural Template 2.</span></span>

<span data-ttu-id="6e167-386">Zde je, kde můžete získat šablon Azure Resource Manageru pro tento scénář nasazení:</span><span class="sxs-lookup"><span data-stu-id="6e167-386">Here's where you can get Azure Resource Manager templates for this deployment scenario:</span></span>

* [<span data-ttu-id="6e167-387">Obrázek pro Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6e167-387">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [<span data-ttu-id="6e167-388">Vlastní image</span><span class="sxs-lookup"><span data-stu-id="6e167-388">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)


### <a name="prepare-the-infrastructure-for-architectural-template-3"></a><span data-ttu-id="6e167-389">Příprava infrastruktury pro architektury 3 šablony</span><span class="sxs-lookup"><span data-stu-id="6e167-389">Prepare the infrastructure for Architectural Template 3</span></span>

<span data-ttu-id="6e167-390">Můžete Příprava infrastruktury a nakonfigurovat SAP pro **více SID**.</span><span class="sxs-lookup"><span data-stu-id="6e167-390">You can prepare the infrastructure and configure SAP for **multi-SID**.</span></span> <span data-ttu-id="6e167-391">Například můžete přidat další instance SAP ASC nebo SCS do *existující* konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-391">For example, you can add an additional SAP ASCS/SCS instance into an *existing* cluster configuration.</span></span> <span data-ttu-id="6e167-392">Další informace najdete v tématu [nakonfigurovat další instance SAP ASC nebo SCS do stávající konfigurace clusteru pro vytvoření konfigurace aplikace SAP více SID ve službě Správce prostředků Azure][sap-ha-multi-sid-guide].</span><span class="sxs-lookup"><span data-stu-id="6e167-392">For more information, see [Configure an additional SAP ASCS/SCS instance into an existing cluster configuration to create an SAP multi-SID configuration in Azure Resource Manager][sap-ha-multi-sid-guide].</span></span>

<span data-ttu-id="6e167-393">Pokud chcete vytvořit nový cluster více SID, můžete použít více SID [šablony rychlý start na Githubu](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="6e167-393">If you want to create a new multi-SID cluster, you can use the multi-SID [quickstart templates on GitHub](https://github.com/Azure/azure-quickstart-templates).</span></span>
<span data-ttu-id="6e167-394">K vytvoření nového clusteru více SID, musíte nasadit následující tři šablony:</span><span class="sxs-lookup"><span data-stu-id="6e167-394">To create a new multi-SID cluster, you need to deploy the following three templates:</span></span>

* [<span data-ttu-id="6e167-395">ASC nebo SCS šablony</span><span class="sxs-lookup"><span data-stu-id="6e167-395">ASCS/SCS template</span></span>](#ASCS-SCS-template)
* [<span data-ttu-id="6e167-396">Šablona databáze</span><span class="sxs-lookup"><span data-stu-id="6e167-396">Database template</span></span>](#database-template)
* [<span data-ttu-id="6e167-397">Šablona servery aplikací</span><span class="sxs-lookup"><span data-stu-id="6e167-397">Application servers template</span></span>](#application-servers-template)

<span data-ttu-id="6e167-398">Následující sekce obsahují další informace o šablonách a parametry, které je třeba zadat v šablonách.</span><span class="sxs-lookup"><span data-stu-id="6e167-398">The following sections have more details about the templates and the parameters you need to provide in the templates.</span></span>

#### <span data-ttu-id="6e167-399"><a name="ASCS-SCS-template"></a>ASC nebo SCS šablony</span><span class="sxs-lookup"><span data-stu-id="6e167-399"><a name="ASCS-SCS-template"></a> ASCS/SCS template</span></span>

<span data-ttu-id="6e167-400">Šablona ASC nebo SCS nasadí dva virtuální počítače, které můžete použít k vytvoření clusteru převzetí služeb při selhání systému Windows Server, který je hostitelem více instancí ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="6e167-400">The ASCS/SCS template deploys two virtual machines that you can use to create a Windows Server failover cluster that hosts multiple ASCS/SCS instances.</span></span>

<span data-ttu-id="6e167-401">Nastavení v šabloně ASC nebo SCS více SID [ASC nebo SCS více SID šablony][sap-templates-3-tier-multisid-xscs-marketplace-image], zadejte hodnoty následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="6e167-401">To set up the ASCS/SCS multi-SID template, in the [ASCS/SCS multi-SID template][sap-templates-3-tier-multisid-xscs-marketplace-image], enter values for the following parameters:</span></span>

  - <span data-ttu-id="6e167-402">**Předpona prostředků**.</span><span class="sxs-lookup"><span data-stu-id="6e167-402">**Resource Prefix**.</span></span>  <span data-ttu-id="6e167-403">Nastaví předponu prostředku, který se používá k předpony všechny prostředky, které jsou vytvořené během nasazení.</span><span class="sxs-lookup"><span data-stu-id="6e167-403">Set the resource prefix, which is used to prefix all resources that are created during the deployment.</span></span> <span data-ttu-id="6e167-404">Protože prostředky nepatří do systému jenom jeden SAP, není SID systému SAP jeden předponu prostředku.</span><span class="sxs-lookup"><span data-stu-id="6e167-404">Because the resources do not belong to only one SAP system, the prefix of the resource is not the SID of one SAP system.</span></span>  <span data-ttu-id="6e167-405">Předpona, která musí být v rozmezí **tři až šest znaků**.</span><span class="sxs-lookup"><span data-stu-id="6e167-405">The prefix must be between **three and six characters**.</span></span>
  - <span data-ttu-id="6e167-406">**Stack – typ**.</span><span class="sxs-lookup"><span data-stu-id="6e167-406">**Stack Type**.</span></span> <span data-ttu-id="6e167-407">Vyberte typ zásobníku systému SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-407">Select the stack type of the SAP system.</span></span> <span data-ttu-id="6e167-408">V závislosti na typu zásobníku nástroj pro vyrovnávání zatížení Azure má jeden (ABAP nebo Java pouze) nebo dvě (ABAP + Java) privátní IP adresy na systému SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-408">Depending on the stack type, Azure Load Balancer has one (ABAP or Java only) or two (ABAP+Java) private IP addresses per SAP system.</span></span>
  -  <span data-ttu-id="6e167-409">**Typ operačního systému**.</span><span class="sxs-lookup"><span data-stu-id="6e167-409">**OS Type**.</span></span> <span data-ttu-id="6e167-410">Vyberte verzi operačního systému z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e167-410">Select the operating system of the virtual machines.</span></span>
  -  <span data-ttu-id="6e167-411">**Počet systému SAP**.</span><span class="sxs-lookup"><span data-stu-id="6e167-411">**SAP System Count**.</span></span> <span data-ttu-id="6e167-412">Vyberte počet SAP systémy, které chcete nainstalovat v tomto clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-412">Select the number of SAP systems you want to install in this cluster.</span></span>
  -  <span data-ttu-id="6e167-413">**Dostupnost systému**.</span><span class="sxs-lookup"><span data-stu-id="6e167-413">**System Availability**.</span></span> <span data-ttu-id="6e167-414">Vyberte **HA**.</span><span class="sxs-lookup"><span data-stu-id="6e167-414">Select **HA**.</span></span>
  -  <span data-ttu-id="6e167-415">**Uživatelské jméno správce a heslo správce**.</span><span class="sxs-lookup"><span data-stu-id="6e167-415">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="6e167-416">Vytvořte nového uživatele, který lze použít k přihlášení k počítači.</span><span class="sxs-lookup"><span data-stu-id="6e167-416">Create a new user that can be used to sign in to the machine.</span></span>
  -  <span data-ttu-id="6e167-417">**Nový nebo existující podsíť**.</span><span class="sxs-lookup"><span data-stu-id="6e167-417">**New Or Existing Subnet**.</span></span> <span data-ttu-id="6e167-418">Nastavte, jestli mají být vytvořeny nové virtuální sítě a podsítě, nebo se použije existující podsítí.</span><span class="sxs-lookup"><span data-stu-id="6e167-418">Set whether a new virtual network and subnet should be created, or an existing subnet should be used.</span></span> <span data-ttu-id="6e167-419">Pokud již máte virtuální síť, která je připojen k síti na pracovišti, vyberte **existující**.</span><span class="sxs-lookup"><span data-stu-id="6e167-419">If you already have a virtual network that is connected to your on-premises network, select **existing**.</span></span>
  -  <span data-ttu-id="6e167-420">**Id podsítě**. Nastavte ID podsítě, ke kterému má být připojen virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="6e167-420">**Subnet Id**. Set the ID of the subnet to which the virtual machines should be connected.</span></span> <span data-ttu-id="6e167-421">Vyberte podsíť virtuální privátní sítě (VPN) nebo ExpressRoute virtuální sítě pro virtuální počítač připojit k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="6e167-421">Select the subnet of your virtual private network (VPN) or ExpressRoute virtual network to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="6e167-422">ID obvykle vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6e167-422">The ID usually looks like this:</span></span>

   <span data-ttu-id="6e167-423">/subscriptions/ <*id předplatného*> /resourceGroups/ <*název skupiny prostředků*> /providers/Microsoft.Network/virtualNetworks/ <*název virtuální sítě*> /subnets/ <*název podsítě.*></span><span class="sxs-lookup"><span data-stu-id="6e167-423">/subscriptions/<*subscription id*>/resourceGroups/<*resource group name*>/providers/Microsoft.Network/virtualNetworks/<*virtual network name*>/subnets/<*subnet name*></span></span>

<span data-ttu-id="6e167-424">Šablona nasadí jednu instanci nástroj pro vyrovnávání zatížení Azure, která podporuje více systémů SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-424">The template deploys one Azure Load Balancer instance, which supports multiple SAP systems.</span></span>

- <span data-ttu-id="6e167-425">Instance ASC jsou nakonfigurované pro instance číslo 00, 10, 20...</span><span class="sxs-lookup"><span data-stu-id="6e167-425">The ASCS instances are configured for instance number 00, 10, 20...</span></span>
- <span data-ttu-id="6e167-426">Instance SCS jsou nakonfigurované pro instance číslo 01, 11, 21...</span><span class="sxs-lookup"><span data-stu-id="6e167-426">The SCS instances are configured for instance number 01, 11, 21...</span></span>
- <span data-ttu-id="6e167-427">Instance ASC zařazování replikace serveru (YBRAT) (pouze Linux) jsou nakonfigurované pro čísla instance 02, 12, 22...</span><span class="sxs-lookup"><span data-stu-id="6e167-427">The ASCS Enqueue Replication Server (ERS) (Linux only) instances are configured for instance number 02, 12, 22...</span></span>
- <span data-ttu-id="6e167-428">Instance SCS YBRAT (pouze Linux) jsou nakonfigurované pro instance číslo 03, 13, 23...</span><span class="sxs-lookup"><span data-stu-id="6e167-428">The SCS ERS (Linux only) instances are configured for instance number 03, 13, 23...</span></span>

<span data-ttu-id="6e167-429">Nástroje pro vyrovnávání zatížení obsahuje 1 (2 pro Linux) VIP(s), 1 x virtuální IP adresa pro ASC nebo SCS a 1 x virtuální IP adresa pro YBRAT (pouze Linux).</span><span class="sxs-lookup"><span data-stu-id="6e167-429">The load balancer contains 1 (2 for Linux) VIP(s), 1x VIP for ASCS/SCS and 1x VIP for ERS (Linux only).</span></span>

<span data-ttu-id="6e167-430">Následující seznam obsahuje všechny pravidla (kde x je číslo systému SAP, například 1, 2, 3...) pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="6e167-430">The following list contains all load balancing rules (where x is the number of the SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="6e167-431">Porty specifické pro systém Windows pro každý systém SAP: 445, 5985</span><span class="sxs-lookup"><span data-stu-id="6e167-431">Windows-specific ports for every SAP system: 445, 5985</span></span>
- <span data-ttu-id="6e167-432">Porty ASC (čísla instance x0): 32 × 0, 36 x 0, 39 x 0, 81 x 0, 5 x 013, 5 x 014, 5 x 016</span><span class="sxs-lookup"><span data-stu-id="6e167-432">ASCS ports (instance number x0): 32x0, 36x0, 39x0, 81x0, 5x013, 5x014, 5x016</span></span>
- <span data-ttu-id="6e167-433">Porty SCS (čísla instance x1): 32 x 1, 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116</span><span class="sxs-lookup"><span data-stu-id="6e167-433">SCS ports (instance number x1): 32x1, 33x1, 39x1, 81x1, 5x113, 5x114, 5x116</span></span>
- <span data-ttu-id="6e167-434">ASC YBRAT portů v systému Linux (čísla instance x2): 33 x 2, 5 x 213, 5 x 214, 5 x 216</span><span class="sxs-lookup"><span data-stu-id="6e167-434">ASCS ERS ports on Linux (instance number x2): 33x2, 5x213, 5x214, 5x216</span></span>
- <span data-ttu-id="6e167-435">SCS YBRAT portů v systému Linux (čísla instance x3): 33 x 3, 5 x 313, 5 x 314, 5 x 316</span><span class="sxs-lookup"><span data-stu-id="6e167-435">SCS ERS ports on Linux (instance number x3): 33x3, 5x313, 5x314, 5x316</span></span>

<span data-ttu-id="6e167-436">Nástroje pro vyrovnávání zatížení je nakonfigurovaný na použití následující porty testu (kde x je číslo systému SAP, například 1, 2, 3...):</span><span class="sxs-lookup"><span data-stu-id="6e167-436">The load balancer is configured to use the following probe ports (where x is the number of the SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="6e167-437">Port testu nástroje pro vyrovnávání zatížení ASC nebo SCS interní: 620 x 0</span><span class="sxs-lookup"><span data-stu-id="6e167-437">ASCS/SCS internal load balancer probe port: 620x0</span></span>
- <span data-ttu-id="6e167-438">Interní YBRAT načíst port testu vyrovnávání (pouze Linux): 621 x 2</span><span class="sxs-lookup"><span data-stu-id="6e167-438">ERS internal load balancer probe port (Linux only): 621x2</span></span>

#### <span data-ttu-id="6e167-439"><a name="database-template"></a>Šablona databáze</span><span class="sxs-lookup"><span data-stu-id="6e167-439"><a name="database-template"></a> Database template</span></span>

<span data-ttu-id="6e167-440">Šablona databáze nasadí jednu nebo dvě virtuální počítače, které můžete použít k instalaci systému správy relačních databází (RDBMS) pro systém SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-440">The database template deploys one or two virtual machines that you can use to install the relational database management system (RDBMS) for one SAP system.</span></span> <span data-ttu-id="6e167-441">Například pokud nasadíte šablonu ASC nebo SCS pro pět systémy SAP, budete muset nasadit pětkrát této šablony.</span><span class="sxs-lookup"><span data-stu-id="6e167-441">For example, if you deploy an ASCS/SCS template for five SAP systems, you need to deploy this template five times.</span></span>

<span data-ttu-id="6e167-442">Nastavení v šabloně více SID databáze [databáze více SID šablony][sap-templates-3-tier-multisid-db-marketplace-image], zadejte hodnoty následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="6e167-442">To set up the database multi-SID template, in the [database multi-SID template][sap-templates-3-tier-multisid-db-marketplace-image], enter values for the following parameters:</span></span>

  -  <span data-ttu-id="6e167-443">**Id systému SAP**. Zadejte ID systému SAP systému SAP, který chcete nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="6e167-443">**Sap System Id**. Enter the SAP system ID of the SAP system you want to install.</span></span> <span data-ttu-id="6e167-444">Identifikátor se použije jako předpona pro prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="6e167-444">The ID will be used as a prefix for the resources that are deployed.</span></span>
  -  <span data-ttu-id="6e167-445">**Typ operačního systému**.</span><span class="sxs-lookup"><span data-stu-id="6e167-445">**Os Type**.</span></span> <span data-ttu-id="6e167-446">Vyberte verzi operačního systému z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e167-446">Select the operating system of the virtual machines.</span></span>
  -  <span data-ttu-id="6e167-447">**Hodnota DbType**.</span><span class="sxs-lookup"><span data-stu-id="6e167-447">**Dbtype**.</span></span> <span data-ttu-id="6e167-448">Vyberte typ databáze, kterou chcete nainstalovat na clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-448">Select the type of the database you want to install on the cluster.</span></span> <span data-ttu-id="6e167-449">Vyberte **SQL** Pokud chcete nainstalovat Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6e167-449">Select **SQL** if you want to install Microsoft SQL Server.</span></span> <span data-ttu-id="6e167-450">Vyberte **HANA** Pokud budete chtít na virtuální počítače nainstalovat SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="6e167-450">Select **HANA** if you plan to install SAP HANA on the virtual machines.</span></span> <span data-ttu-id="6e167-451">Je nutné vybrat typ správný operační systém: vyberte **Windows** pro SQL a vyberte distribuční Linux pro HANA.</span><span class="sxs-lookup"><span data-stu-id="6e167-451">Make sure to select the correct operating system type: select **Windows** for SQL, and select a Linux distribution for HANA.</span></span> <span data-ttu-id="6e167-452">Vyrovnávání zatížení Azure, která je připojena k virtuálním počítačům, bude nakonfigurován pro podporu typ vybrané databáze:</span><span class="sxs-lookup"><span data-stu-id="6e167-452">The Azure Load Balancer that is connected to the virtual machines will be configured to support the selected database type:</span></span>
    * <span data-ttu-id="6e167-453">**SQL**.</span><span class="sxs-lookup"><span data-stu-id="6e167-453">**SQL**.</span></span> <span data-ttu-id="6e167-454">Nástroje pro vyrovnávání zatížení bude Vyrovnávání zatížení port 1433.</span><span class="sxs-lookup"><span data-stu-id="6e167-454">The load balancer will load-balance port 1433.</span></span> <span data-ttu-id="6e167-455">Ujistěte se, že tento port použít pro vaše instalační program SQL serveru Always On.</span><span class="sxs-lookup"><span data-stu-id="6e167-455">Make sure to use this port for your SQL Server Always On setup.</span></span>
    * <span data-ttu-id="6e167-456">**HANA**.</span><span class="sxs-lookup"><span data-stu-id="6e167-456">**HANA**.</span></span> <span data-ttu-id="6e167-457">Nástroje pro vyrovnávání zatížení bude porty 35015 a 35017 Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6e167-457">The load balancer will load-balance ports 35015 and 35017.</span></span> <span data-ttu-id="6e167-458">Nainstalujte SAP HANA číslem instance **50**.</span><span class="sxs-lookup"><span data-stu-id="6e167-458">Make sure to install SAP HANA with instance number **50**.</span></span>
    <span data-ttu-id="6e167-459">Nástroje pro vyrovnávání zatížení bude používat port testu 62550.</span><span class="sxs-lookup"><span data-stu-id="6e167-459">The load balancer will use probe port 62550.</span></span>
  -  <span data-ttu-id="6e167-460">**Velikost systému SAP**.</span><span class="sxs-lookup"><span data-stu-id="6e167-460">**Sap System Size**.</span></span> <span data-ttu-id="6e167-461">Nastavte počet protokoly SAP bude poskytovat nový systém.</span><span class="sxs-lookup"><span data-stu-id="6e167-461">Set the number of SAPS the new system will provide.</span></span> <span data-ttu-id="6e167-462">Pokud si nejste jisti kolik protokoly SAP, systém bude vyžadovat, požádejte SAP technologie partnera nebo systémový integrátor.</span><span class="sxs-lookup"><span data-stu-id="6e167-462">If you are not sure how many SAPS the system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="6e167-463">**Dostupnost systému**.</span><span class="sxs-lookup"><span data-stu-id="6e167-463">**System Availability**.</span></span> <span data-ttu-id="6e167-464">Vyberte **HA**.</span><span class="sxs-lookup"><span data-stu-id="6e167-464">Select **HA**.</span></span>
  -  <span data-ttu-id="6e167-465">**Uživatelské jméno správce a heslo správce**.</span><span class="sxs-lookup"><span data-stu-id="6e167-465">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="6e167-466">Vytvořte nového uživatele, který lze použít k přihlášení k počítači.</span><span class="sxs-lookup"><span data-stu-id="6e167-466">Create a new user that can be used to sign in to the machine.</span></span>
  -  <span data-ttu-id="6e167-467">**Id podsítě**. Zadejte ID podsítě, která jste použili při nasazení ASC nebo SCS šablony nebo ID podsítě, která byla vytvořena jako součást nasazení ASC nebo SCS šablony.</span><span class="sxs-lookup"><span data-stu-id="6e167-467">**Subnet Id**. Enter the ID of the subnet that you used during the deployment of the ASCS/SCS template, or the ID of the subnet that was created as part of the ASCS/SCS template deployment.</span></span>

#### <span data-ttu-id="6e167-468"><a name="application-servers-template"></a>Šablona servery aplikací</span><span class="sxs-lookup"><span data-stu-id="6e167-468"><a name="application-servers-template"></a> Application servers template</span></span>

<span data-ttu-id="6e167-469">Šablony servery aplikace nasadí dvě nebo více virtuálních počítačů, které lze použít jako instance aplikačního serveru SAP jeden systému SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-469">The application servers template deploys two or more virtual machines that can be used as SAP Application Server instances for one SAP system.</span></span> <span data-ttu-id="6e167-470">Například pokud nasadíte šablonu ASC nebo SCS pro pět systémy SAP, budete muset nasadit pětkrát této šablony.</span><span class="sxs-lookup"><span data-stu-id="6e167-470">For example, if you deploy an ASCS/SCS template for five SAP systems, you need to deploy this template five times.</span></span>

<span data-ttu-id="6e167-471">Nastavení v šabloně SID více serverů aplikace [šablony SID více serverů aplikace][sap-templates-3-tier-multisid-apps-marketplace-image], zadejte hodnoty následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="6e167-471">To set up the application servers multi-SID template, in the [application servers multi-SID template][sap-templates-3-tier-multisid-apps-marketplace-image], enter values for the following parameters:</span></span>

  -  <span data-ttu-id="6e167-472">**Id systému SAP**. Zadejte ID systému SAP systému SAP, který chcete nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="6e167-472">**Sap System Id**. Enter the SAP system ID of the SAP system you want to install.</span></span> <span data-ttu-id="6e167-473">Identifikátor se použije jako předpona pro prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="6e167-473">The ID will be used as a prefix for the resources that are deployed.</span></span>
  -  <span data-ttu-id="6e167-474">**Typ operačního systému**.</span><span class="sxs-lookup"><span data-stu-id="6e167-474">**Os Type**.</span></span> <span data-ttu-id="6e167-475">Vyberte verzi operačního systému z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e167-475">Select the operating system of the virtual machines.</span></span>
  -  <span data-ttu-id="6e167-476">**Velikost systému SAP**.</span><span class="sxs-lookup"><span data-stu-id="6e167-476">**Sap System Size**.</span></span> <span data-ttu-id="6e167-477">Počet je nový systém bude poskytovat protokoly SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-477">The number of SAPS the new system will provide.</span></span> <span data-ttu-id="6e167-478">Pokud si nejste jisti kolik protokoly SAP, systém bude vyžadovat, požádejte SAP technologie partnera nebo systémový integrátor.</span><span class="sxs-lookup"><span data-stu-id="6e167-478">If you are not sure how many SAPS the system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="6e167-479">**Dostupnost systému**.</span><span class="sxs-lookup"><span data-stu-id="6e167-479">**System Availability**.</span></span> <span data-ttu-id="6e167-480">Vyberte **HA**.</span><span class="sxs-lookup"><span data-stu-id="6e167-480">Select **HA**.</span></span>
  -  <span data-ttu-id="6e167-481">**Uživatelské jméno správce a heslo správce**.</span><span class="sxs-lookup"><span data-stu-id="6e167-481">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="6e167-482">Vytvořte nového uživatele, který lze použít k přihlášení k počítači.</span><span class="sxs-lookup"><span data-stu-id="6e167-482">Create a new user that can be used to sign in to the machine.</span></span>
  -  <span data-ttu-id="6e167-483">**Id podsítě**. Zadejte ID podsítě, která jste použili při nasazení ASC nebo SCS šablony nebo ID podsítě, která byla vytvořena jako součást nasazení ASC nebo SCS šablony.</span><span class="sxs-lookup"><span data-stu-id="6e167-483">**Subnet Id**. Enter the ID of the subnet that you used during the deployment of the ASCS/SCS template, or the ID of the subnet that was created as part of the ASCS/SCS template deployment.</span></span>


### <span data-ttu-id="6e167-484"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="6e167-484"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure virtual network</span></span>
<span data-ttu-id="6e167-485">V našem příkladu adresní prostor virtuální sítě Azure je 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="6e167-485">In our example, the address space of the Azure virtual network is 10.0.0.0/16.</span></span> <span data-ttu-id="6e167-486">Existuje jedna podsíť s názvem **podsíť**, rozsah adres 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="6e167-486">There is one subnet called **Subnet**, with an address range of 10.0.0.0/24.</span></span> <span data-ttu-id="6e167-487">Všechny virtuální počítače a interní služby load balancer nasazených v této virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="6e167-487">All virtual machines and internal load balancers are deployed in this virtual network.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6e167-488">Nemáte žádné změny nastavení sítě v hostovaném operačním systému.</span><span class="sxs-lookup"><span data-stu-id="6e167-488">Don't make any changes to the network settings inside the guest operating system.</span></span> <span data-ttu-id="6e167-489">To zahrnuje IP adresy, servery DNS a podsítě.</span><span class="sxs-lookup"><span data-stu-id="6e167-489">This includes IP addresses, DNS servers, and subnet.</span></span> <span data-ttu-id="6e167-490">Nakonfigurujte všechna nastavení sítě v Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-490">Configure all your network settings in Azure.</span></span> <span data-ttu-id="6e167-491">Službu Dynamic Host Configuration Protocol (DHCP) rozšíří nastavení.</span><span class="sxs-lookup"><span data-stu-id="6e167-491">The Dynamic Host Configuration Protocol (DHCP) service propagates your settings.</span></span>
>
>

### <span data-ttu-id="6e167-492"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS IP adresy</span><span class="sxs-lookup"><span data-stu-id="6e167-492"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS IP addresses</span></span>

<span data-ttu-id="6e167-493">Pokud chcete nastavit požadované DNS IP adresy, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="6e167-493">To set the required DNS IP addresses, do the following steps.</span></span>

1.  <span data-ttu-id="6e167-494">Na portálu Azure na **servery DNS** okno, ujistěte se, že virtuální sítě **servery DNS** je možnost nastavena na **vlastní DNS**.</span><span class="sxs-lookup"><span data-stu-id="6e167-494">In the Azure portal, on the **DNS servers** blade, make sure that your virtual network **DNS servers** option is set to **Custom DNS**.</span></span>
2.  <span data-ttu-id="6e167-495">Vyberte nastavení na základě typu sítě, které máte.</span><span class="sxs-lookup"><span data-stu-id="6e167-495">Select your settings based on the type of network you have.</span></span> <span data-ttu-id="6e167-496">Další informace najdete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="6e167-496">For more information, see the following resources:</span></span>
    * <span data-ttu-id="6e167-497">[Připojení k podnikové síti (mezi různými místy)][planning-guide-2.2]: Přidejte IP adresy na místní servery DNS.</span><span class="sxs-lookup"><span data-stu-id="6e167-497">[Corporate network connectivity (cross-premises)][planning-guide-2.2]: Add the IP addresses of the on-premises DNS servers.</span></span>  
    <span data-ttu-id="6e167-498">Můžete rozšířit na místní servery DNS pro virtuální počítače, které jsou spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-498">You can extend on-premises DNS servers to the virtual machines that are running in Azure.</span></span> <span data-ttu-id="6e167-499">V tomto scénáři můžete přidat IP adresy Azure virtuální počítače, na které spouštíte služby DNS.</span><span class="sxs-lookup"><span data-stu-id="6e167-499">In that scenario, you can add the IP addresses of the Azure virtual machines on which you run the DNS service.</span></span>
    * <span data-ttu-id="6e167-500">[Čistě cloudové nasazení][planning-guide-2.1]: nasazení se další virtuální počítač ve stejné instanci virtuální sítě, která slouží jako DNS server.</span><span class="sxs-lookup"><span data-stu-id="6e167-500">[Cloud-only deployment][planning-guide-2.1]: Deploy an additional virtual machine in the same Virtual Network instance that serves as a DNS server.</span></span> <span data-ttu-id="6e167-501">Přidání IP adres virtuální počítače Azure, které jste nastavili ke spouštění služby DNS.</span><span class="sxs-lookup"><span data-stu-id="6e167-501">Add the IP addresses of the Azure virtual machines that you've set up to run DNS service.</span></span>

    ![Obrázek 12: Servery DNS nakonfigurujte pro virtuální síť Azure][sap-ha-guide-figure-3001]

    <span data-ttu-id="6e167-503">_**Obrázek 12:** konfigurace DNS serverů pro virtuální síť Azure_</span><span class="sxs-lookup"><span data-stu-id="6e167-503">_**Figure 12:** Configure DNS servers for Azure Virtual Network_</span></span>

  > [!NOTE]
  > <span data-ttu-id="6e167-504">Pokud změníte IP adresy serverů DNS, musíte restartovat virtuální počítače Azure k použití změny a šířit nové servery DNS.</span><span class="sxs-lookup"><span data-stu-id="6e167-504">If you change the IP addresses of the DNS servers, you need to restart the Azure virtual machines to apply the change and propagate the new DNS servers.</span></span>
  >
  >

<span data-ttu-id="6e167-505">V našem příkladu služba DNS nainstalovaná a nakonfigurovaná na těchto virtuálních počítačích Windows:</span><span class="sxs-lookup"><span data-stu-id="6e167-505">In our example, the DNS service is installed and configured on these Windows virtual machines:</span></span>

| <span data-ttu-id="6e167-506">Role virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6e167-506">Virtual machine role</span></span> | <span data-ttu-id="6e167-507">Název hostitele virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6e167-507">Virtual machine host name</span></span> | <span data-ttu-id="6e167-508">Název síťové karty</span><span class="sxs-lookup"><span data-stu-id="6e167-508">Network card name</span></span> | <span data-ttu-id="6e167-509">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="6e167-509">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6e167-510">První server DNS</span><span class="sxs-lookup"><span data-stu-id="6e167-510">First DNS server</span></span> |<span data-ttu-id="6e167-511">domcontr-0</span><span class="sxs-lookup"><span data-stu-id="6e167-511">domcontr-0</span></span> |<span data-ttu-id="6e167-512">PR1-seskupování domcontr-0</span><span class="sxs-lookup"><span data-stu-id="6e167-512">pr1-nic-domcontr-0</span></span> |<span data-ttu-id="6e167-513">10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="6e167-513">10.0.0.10</span></span> |
| <span data-ttu-id="6e167-514">Druhý server DNS</span><span class="sxs-lookup"><span data-stu-id="6e167-514">Second DNS server</span></span> |<span data-ttu-id="6e167-515">domcontr-1</span><span class="sxs-lookup"><span data-stu-id="6e167-515">domcontr-1</span></span> |<span data-ttu-id="6e167-516">PR1-seskupování domcontr-1</span><span class="sxs-lookup"><span data-stu-id="6e167-516">pr1-nic-domcontr-1</span></span> |<span data-ttu-id="6e167-517">10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="6e167-517">10.0.0.11</span></span> |

### <span data-ttu-id="6e167-518"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Názvy hostitelů a statické IP adresy pro clusterové instance SAP ASC nebo SCS a Clusterové instance databázového systému</span><span class="sxs-lookup"><span data-stu-id="6e167-518"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Host names and static IP addresses for the SAP ASCS/SCS clustered instance and DBMS clustered instance</span></span>

<span data-ttu-id="6e167-519">Pro místní nasazení je třeba tyto názvy vyhrazené hostitele a IP adresy:</span><span class="sxs-lookup"><span data-stu-id="6e167-519">For on-premises deployment, you need these reserved host names and IP addresses:</span></span>

| <span data-ttu-id="6e167-520">Název role virtuálních hostitelů</span><span class="sxs-lookup"><span data-stu-id="6e167-520">Virtual host name role</span></span> | <span data-ttu-id="6e167-521">Název virtuálního hostitele</span><span class="sxs-lookup"><span data-stu-id="6e167-521">Virtual host name</span></span> | <span data-ttu-id="6e167-522">Virtuální statickou IP adresu</span><span class="sxs-lookup"><span data-stu-id="6e167-522">Virtual static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6e167-523">SAP ASC nebo SCS první clusteru virtuální hostitel název (pro správu clusteru)</span><span class="sxs-lookup"><span data-stu-id="6e167-523">SAP ASCS/SCS first cluster virtual host name (for cluster management)</span></span> |<span data-ttu-id="6e167-524">PR1. ASC vir</span><span class="sxs-lookup"><span data-stu-id="6e167-524">pr1-ascs-vir</span></span> |<span data-ttu-id="6e167-525">10.0.0.42</span><span class="sxs-lookup"><span data-stu-id="6e167-525">10.0.0.42</span></span> |
| <span data-ttu-id="6e167-526">Název virtuálního hostitele instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="6e167-526">SAP ASCS/SCS instance virtual host name</span></span> |<span data-ttu-id="6e167-527">PR1. ASC sap</span><span class="sxs-lookup"><span data-stu-id="6e167-527">pr1-ascs-sap</span></span> |<span data-ttu-id="6e167-528">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="6e167-528">10.0.0.43</span></span> |
| <span data-ttu-id="6e167-529">Databázového systému SAP druhý cluster virtuálního hostitele název (Správa clusteru)</span><span class="sxs-lookup"><span data-stu-id="6e167-529">SAP DBMS second cluster virtual host name (cluster management)</span></span> |<span data-ttu-id="6e167-530">PR1. databázového systému vir</span><span class="sxs-lookup"><span data-stu-id="6e167-530">pr1-dbms-vir</span></span> |<span data-ttu-id="6e167-531">10.0.0.32</span><span class="sxs-lookup"><span data-stu-id="6e167-531">10.0.0.32</span></span> |

<span data-ttu-id="6e167-532">Při vytváření clusteru, vytvořit názvy virtuálních hostitelů **pr1. ASC vir** a **pr1. databázového systému vir** a přidružené IP adresy, které spravují samotného clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-532">When you create the cluster, create the virtual host names **pr1-ascs-vir** and **pr1-dbms-vir** and the associated IP addresses that manage the cluster itself.</span></span> <span data-ttu-id="6e167-533">Informace o tom, jak to udělat najdete v tématu [shromažďovat uzly clusteru v konfiguraci clusteru][sap-ha-guide-8.12.1].</span><span class="sxs-lookup"><span data-stu-id="6e167-533">For information about how to do this, see [Collect cluster nodes in a cluster configuration][sap-ha-guide-8.12.1].</span></span>

<span data-ttu-id="6e167-534">Můžete ručně vytvořit další dva názvy virtuálních hostitelů, **pr1. ASC sap** a **pr1. databázového systému sap**a přidružené IP adresy na serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="6e167-534">You can manually create the other two virtual host names, **pr1-ascs-sap** and **pr1-dbms-sap**, and the associated IP addresses, on the DNS server.</span></span> <span data-ttu-id="6e167-535">SAP ASC nebo SCS skupinu prostředků clusteru a skupinu prostředků clusteru databázového systému používají tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="6e167-535">The clustered SAP ASCS/SCS instance and the clustered DBMS instance use these resources.</span></span> <span data-ttu-id="6e167-536">Informace o tom, jak to udělat najdete v tématu [vytvořte název virtuálního hostitele pro clusterovou instanci SAP ASC nebo SCS][sap-ha-guide-9.1.1].</span><span class="sxs-lookup"><span data-stu-id="6e167-536">For information about how to do this, see [Create a virtual host name for a clustered SAP ASCS/SCS instance][sap-ha-guide-9.1.1].</span></span>

### <span data-ttu-id="6e167-537"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Nastavení statické IP adresy pro virtuální počítače SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-537"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> Set static IP addresses for the SAP virtual machines</span></span>
<span data-ttu-id="6e167-538">Poté, co nasadíte virtuální počítače pro použití v clusteru, musíte nastavit statické IP adresy pro všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="6e167-538">After you deploy the virtual machines to use in your cluster, you need to set static IP addresses for all virtual machines.</span></span> <span data-ttu-id="6e167-539">To udělat v konfiguraci virtuální sítě Azure a není v hostovaném operačním systému.</span><span class="sxs-lookup"><span data-stu-id="6e167-539">Do this in the Azure Virtual Network configuration, and not in the guest operating system.</span></span>

1.  <span data-ttu-id="6e167-540">Na portálu Azure vyberte **skupiny prostředků** > **síťové karty** > **nastavení** > **IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="6e167-540">In the Azure portal, select **Resource Group** > **Network Card** > **Settings** > **IP Address**.</span></span>
2.  <span data-ttu-id="6e167-541">Na **IP adresy** okno, v části **přiřazení**, vyberte **statické**.</span><span class="sxs-lookup"><span data-stu-id="6e167-541">On the **IP addresses** blade, under **Assignment**, select **Static**.</span></span> <span data-ttu-id="6e167-542">V **IP adresu** zadejte IP adresu, která chcete použít.</span><span class="sxs-lookup"><span data-stu-id="6e167-542">In the **IP address** box, enter the IP address that you want to use.</span></span>

  > [!NOTE]
  > <span data-ttu-id="6e167-543">Pokud změníte IP adresa síťové karty, budete muset restartovat virtuální počítače Azure na použití změny.</span><span class="sxs-lookup"><span data-stu-id="6e167-543">If you change the IP address of the network card, you need to restart the Azure virtual machines to apply the change.</span></span>  
  >
  >

  ![Obrázek 13: Nastavení statické IP adresy pro síťové karty každého virtuálního počítače][sap-ha-guide-figure-3002]

  <span data-ttu-id="6e167-545">_**Obrázek 13:** nastavení statické IP adresy pro síťové karty každého virtuálního počítače_</span><span class="sxs-lookup"><span data-stu-id="6e167-545">_**Figure 13:** Set static IP addresses for the network card of each virtual machine_</span></span>

  <span data-ttu-id="6e167-546">Tento krok opakujte pro všechna síťová rozhraní, které se pro všechny virtuální počítače, včetně virtuálních počítačů, které chcete použít pro vaši službu Active Directory a DNS.</span><span class="sxs-lookup"><span data-stu-id="6e167-546">Repeat this step for all network interfaces, that is, for all virtual machines, including virtual machines that you want to use for your Active Directory/DNS service.</span></span>

<span data-ttu-id="6e167-547">V našem příkladu máme tyto virtuální počítače a statické IP adresy:</span><span class="sxs-lookup"><span data-stu-id="6e167-547">In our example, we have these virtual machines and static IP addresses:</span></span>

| <span data-ttu-id="6e167-548">Role virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6e167-548">Virtual machine role</span></span> | <span data-ttu-id="6e167-549">Název hostitele virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6e167-549">Virtual machine host name</span></span> | <span data-ttu-id="6e167-550">Název síťové karty</span><span class="sxs-lookup"><span data-stu-id="6e167-550">Network card name</span></span> | <span data-ttu-id="6e167-551">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="6e167-551">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6e167-552">První instance aplikačního serveru SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-552">First SAP Application Server instance</span></span> |<span data-ttu-id="6e167-553">PR1-di-0</span><span class="sxs-lookup"><span data-stu-id="6e167-553">pr1-di-0</span></span> |<span data-ttu-id="6e167-554">PR1-seskupování di-0</span><span class="sxs-lookup"><span data-stu-id="6e167-554">pr1-nic-di-0</span></span> |<span data-ttu-id="6e167-555">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="6e167-555">10.0.0.50</span></span> |
| <span data-ttu-id="6e167-556">Druhá instance aplikačního serveru SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-556">Second SAP Application Server instance</span></span> |<span data-ttu-id="6e167-557">PR1-di-1</span><span class="sxs-lookup"><span data-stu-id="6e167-557">pr1-di-1</span></span> |<span data-ttu-id="6e167-558">PR1-seskupování di-1</span><span class="sxs-lookup"><span data-stu-id="6e167-558">pr1-nic-di-1</span></span> |<span data-ttu-id="6e167-559">10.0.0.51</span><span class="sxs-lookup"><span data-stu-id="6e167-559">10.0.0.51</span></span> |
| <span data-ttu-id="6e167-560">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="6e167-560">...</span></span> |<span data-ttu-id="6e167-561">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="6e167-561">...</span></span> |<span data-ttu-id="6e167-562">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="6e167-562">...</span></span> |<span data-ttu-id="6e167-563">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="6e167-563">...</span></span> |
| <span data-ttu-id="6e167-564">Poslední instance aplikačního serveru SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-564">Last SAP Application Server instance</span></span> |<span data-ttu-id="6e167-565">PR1-di-5</span><span class="sxs-lookup"><span data-stu-id="6e167-565">pr1-di-5</span></span> |<span data-ttu-id="6e167-566">PR1 seskupování di 5</span><span class="sxs-lookup"><span data-stu-id="6e167-566">pr1-nic-di-5</span></span> |<span data-ttu-id="6e167-567">10.0.0.55</span><span class="sxs-lookup"><span data-stu-id="6e167-567">10.0.0.55</span></span> |
| <span data-ttu-id="6e167-568">Prvním uzlu clusteru pro instanci ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="6e167-568">First cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="6e167-569">PR1-ASC-0</span><span class="sxs-lookup"><span data-stu-id="6e167-569">pr1-ascs-0</span></span> |<span data-ttu-id="6e167-570">PR1-seskupování ASC-0</span><span class="sxs-lookup"><span data-stu-id="6e167-570">pr1-nic-ascs-0</span></span> |<span data-ttu-id="6e167-571">10.0.0.40</span><span class="sxs-lookup"><span data-stu-id="6e167-571">10.0.0.40</span></span> |
| <span data-ttu-id="6e167-572">Druhý uzel clusteru pro instanci ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="6e167-572">Second cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="6e167-573">PR1-ASC-1</span><span class="sxs-lookup"><span data-stu-id="6e167-573">pr1-ascs-1</span></span> |<span data-ttu-id="6e167-574">PR1-seskupování ASC-1</span><span class="sxs-lookup"><span data-stu-id="6e167-574">pr1-nic-ascs-1</span></span> |<span data-ttu-id="6e167-575">10.0.0.41</span><span class="sxs-lookup"><span data-stu-id="6e167-575">10.0.0.41</span></span> |
| <span data-ttu-id="6e167-576">Prvním uzlu clusteru pro instanci databázového systému</span><span class="sxs-lookup"><span data-stu-id="6e167-576">First cluster node for DBMS instance</span></span> |<span data-ttu-id="6e167-577">PR1-db-0</span><span class="sxs-lookup"><span data-stu-id="6e167-577">pr1-db-0</span></span> |<span data-ttu-id="6e167-578">PR1-seskupování db-0</span><span class="sxs-lookup"><span data-stu-id="6e167-578">pr1-nic-db-0</span></span> |<span data-ttu-id="6e167-579">10.0.0.30</span><span class="sxs-lookup"><span data-stu-id="6e167-579">10.0.0.30</span></span> |
| <span data-ttu-id="6e167-580">Druhý uzel clusteru pro instanci databázového systému</span><span class="sxs-lookup"><span data-stu-id="6e167-580">Second cluster node for DBMS instance</span></span> |<span data-ttu-id="6e167-581">PR1-db-1</span><span class="sxs-lookup"><span data-stu-id="6e167-581">pr1-db-1</span></span> |<span data-ttu-id="6e167-582">PR1-seskupování db-1</span><span class="sxs-lookup"><span data-stu-id="6e167-582">pr1-nic-db-1</span></span> |<span data-ttu-id="6e167-583">10.0.0.31</span><span class="sxs-lookup"><span data-stu-id="6e167-583">10.0.0.31</span></span> |

### <span data-ttu-id="6e167-584"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Nastavit statickou IP adresu pro nástroj pro vyrovnávání zatížení Azure interní</span><span class="sxs-lookup"><span data-stu-id="6e167-584"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Set a static IP address for the Azure internal load balancer</span></span>

<span data-ttu-id="6e167-585">Šablona SAP Azure Resource Manager vytvoří pro vyrovnávání zatížení Azure interní, který se používá pro SAP ASC nebo SCS instance clusteru a clusteru databázového systému.</span><span class="sxs-lookup"><span data-stu-id="6e167-585">The SAP Azure Resource Manager template creates an Azure internal load balancer that is used for the SAP ASCS/SCS instance cluster and the DBMS cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6e167-586">IP adresa název virtuálního hostitele SAP ASC nebo SCS je stejný jako IP adresu služby Vyrovnávání zatížení interní SAP ASC nebo SCS: **pr1-lb Asc**.</span><span class="sxs-lookup"><span data-stu-id="6e167-586">The IP address of the virtual host name of the SAP ASCS/SCS is the same as the IP address of the SAP ASCS/SCS internal load balancer: **pr1-lb-ascs**.</span></span>
> <span data-ttu-id="6e167-587">IP adresa virtuální název tohoto databázového systému je stejné jako IP adresa služby Vyrovnávání zatížení interní databázového systému: **databázového systému pr1 lb**.</span><span class="sxs-lookup"><span data-stu-id="6e167-587">The IP address of the virtual name of the DBMS is the same as the IP address of the DBMS internal load balancer: **pr1-lb-dbms**.</span></span>
>
>

<span data-ttu-id="6e167-588">Chcete-li nastavit statickou IP adresu pro nástroj pro vyrovnávání zatížení Azure interní:</span><span class="sxs-lookup"><span data-stu-id="6e167-588">To set a static IP address for the Azure internal load balancer:</span></span>

1.  <span data-ttu-id="6e167-589">Počáteční nasazení nastaví IP adresa služby Vyrovnávání zatížení pro vnitřní **dynamické**.</span><span class="sxs-lookup"><span data-stu-id="6e167-589">The initial deployment sets the internal load balancer IP address to **Dynamic**.</span></span> <span data-ttu-id="6e167-590">Na portálu Azure na **IP adresy** okno, v části **přiřazení**, vyberte **statické**.</span><span class="sxs-lookup"><span data-stu-id="6e167-590">In the Azure portal, on the **IP addresses** blade, under **Assignment**, select **Static**.</span></span>
2.  <span data-ttu-id="6e167-591">Nastavení IP adresy služby Vyrovnávání zatížení pro vnitřní **pr1-lb Asc** na název virtuálního hostitele instance SAP ASC nebo SCS adresu IP.</span><span class="sxs-lookup"><span data-stu-id="6e167-591">Set the IP address of the internal load balancer **pr1-lb-ascs** to the IP address of the virtual host name of the SAP ASCS/SCS instance.</span></span>
3.  <span data-ttu-id="6e167-592">Nastavení IP adresy služby Vyrovnávání zatížení pro vnitřní **databázového systému pr1 lb** na IP adresu název virtuálního hostitele instance databázového systému.</span><span class="sxs-lookup"><span data-stu-id="6e167-592">Set the IP address of the internal load balancer **pr1-lb-dbms** to the IP address of the virtual host name of the DBMS instance.</span></span>

  ![Obrázek 14: Nastavení statické IP adresy pro vyrovnávání zatížení pro vnitřní pro instance SAP ASC nebo SCS][sap-ha-guide-figure-3003]

  <span data-ttu-id="6e167-594">_**Obrázek 14:** nastavení statické IP adresy pro vyrovnávání zatížení pro vnitřní pro instance SAP ASC nebo SCS_</span><span class="sxs-lookup"><span data-stu-id="6e167-594">_**Figure 14:** Set static IP addresses for the internal load balancer for the SAP ASCS/SCS instance_</span></span>

<span data-ttu-id="6e167-595">V našem příkladu máme dvě Azure interní služby load balancer, které mají tyto statické IP adresy:</span><span class="sxs-lookup"><span data-stu-id="6e167-595">In our example, we have two Azure internal load balancers that have these static IP addresses:</span></span>

| <span data-ttu-id="6e167-596">Role služby Vyrovnávání zatížení Azure interní</span><span class="sxs-lookup"><span data-stu-id="6e167-596">Azure internal load balancer role</span></span> | <span data-ttu-id="6e167-597">Název nástroje pro vyrovnávání zatížení Azure interní</span><span class="sxs-lookup"><span data-stu-id="6e167-597">Azure internal load balancer name</span></span> | <span data-ttu-id="6e167-598">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="6e167-598">Static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6e167-599">SAP ASC nebo SCS instanci interní zátěže.</span><span class="sxs-lookup"><span data-stu-id="6e167-599">SAP ASCS/SCS instance internal load balancer</span></span> |<span data-ttu-id="6e167-600">PR1-lb ASC</span><span class="sxs-lookup"><span data-stu-id="6e167-600">pr1-lb-ascs</span></span> |<span data-ttu-id="6e167-601">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="6e167-601">10.0.0.43</span></span> |
| <span data-ttu-id="6e167-602">Databázového systému SAP interní nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="6e167-602">SAP DBMS internal load balancer</span></span> |<span data-ttu-id="6e167-603">PR1-lb-databázového systému</span><span class="sxs-lookup"><span data-stu-id="6e167-603">pr1-lb-dbms</span></span> |<span data-ttu-id="6e167-604">10.0.0.33</span><span class="sxs-lookup"><span data-stu-id="6e167-604">10.0.0.33</span></span> |


### <span data-ttu-id="6e167-605"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Výchozí pravidla pro vyrovnávání zatížení Azure interní Vyrovnávání zatížení ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="6e167-605"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Default ASCS/SCS load balancing rules for the Azure internal load balancer</span></span>

<span data-ttu-id="6e167-606">Šablona SAP Azure Resource Manager vytvoří porty, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="6e167-606">The SAP Azure Resource Manager template creates the ports you need:</span></span>
* <span data-ttu-id="6e167-607">Instance ABAP ASC pomocí výchozí instance číslo **00**</span><span class="sxs-lookup"><span data-stu-id="6e167-607">An ABAP ASCS instance, with the default instance number **00**</span></span>
* <span data-ttu-id="6e167-608">Instance Java SCS, s výchozí instance číslo **01**</span><span class="sxs-lookup"><span data-stu-id="6e167-608">A Java SCS instance, with the default instance number **01**</span></span>

<span data-ttu-id="6e167-609">Při instalaci instance SAP ASC nebo SCS, musíte použít výchozí instanci číslo **00** pro vaše ABAP ASC instance a číslo instance výchozí **01** instance Java SCS.</span><span class="sxs-lookup"><span data-stu-id="6e167-609">When you install your SAP ASCS/SCS instance, you must use the default instance number **00** for your ABAP ASCS instance and the default instance number **01** for your Java SCS instance.</span></span>

<span data-ttu-id="6e167-610">Dále vytvořte požadované interní koncové body pro SAP NetWeaver porty pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6e167-610">Next, create required internal load balancing endpoints for the SAP NetWeaver ports.</span></span>

<span data-ttu-id="6e167-611">Pokud chcete vytvořit požadované interní služby load vyrovnávání koncové body, nejdřív vytvořte tyto koncové body pro SAP NetWeaver ABAP ASC porty pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="6e167-611">To create required internal load balancing endpoints, first, create these load balancing endpoints for the SAP NetWeaver ABAP ASCS ports:</span></span>

| <span data-ttu-id="6e167-612">Název pravidla vyrovnávání služby nebo zatížení</span><span class="sxs-lookup"><span data-stu-id="6e167-612">Service/load balancing rule name</span></span> | <span data-ttu-id="6e167-613">Výchozí čísla portů</span><span class="sxs-lookup"><span data-stu-id="6e167-613">Default port numbers</span></span> | <span data-ttu-id="6e167-614">Konkrétní porty (ASC instance číslem instance 00) (YBRAT s 10)</span><span class="sxs-lookup"><span data-stu-id="6e167-614">Concrete ports for (ASCS instance with instance number 00) (ERS with 10)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6e167-615">Zařadit Server / *lbrule3200*</span><span class="sxs-lookup"><span data-stu-id="6e167-615">Enqueue Server / *lbrule3200*</span></span> |<span data-ttu-id="6e167-616">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="6e167-616">32<*InstanceNumber*></span></span> |<span data-ttu-id="6e167-617">3200</span><span class="sxs-lookup"><span data-stu-id="6e167-617">3200</span></span> |
| <span data-ttu-id="6e167-618">Server zpráv ABAP / *lbrule3600*</span><span class="sxs-lookup"><span data-stu-id="6e167-618">ABAP Message Server / *lbrule3600*</span></span> |<span data-ttu-id="6e167-619">36 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="6e167-619">36<*InstanceNumber*></span></span> |<span data-ttu-id="6e167-620">3600</span><span class="sxs-lookup"><span data-stu-id="6e167-620">3600</span></span> |
| <span data-ttu-id="6e167-621">Zpráva Vnitřní ABAP / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="6e167-621">Internal ABAP Message / *lbrule3900*</span></span> |<span data-ttu-id="6e167-622">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="6e167-622">39<*InstanceNumber*></span></span> |<span data-ttu-id="6e167-623">3900</span><span class="sxs-lookup"><span data-stu-id="6e167-623">3900</span></span> |
| <span data-ttu-id="6e167-624">Server HTTP zpráv nebo *Lbrule8100*</span><span class="sxs-lookup"><span data-stu-id="6e167-624">Message Server HTTP / *Lbrule8100*</span></span> |<span data-ttu-id="6e167-625">81 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="6e167-625">81<*InstanceNumber*></span></span> |<span data-ttu-id="6e167-626">8100</span><span class="sxs-lookup"><span data-stu-id="6e167-626">8100</span></span> |
| <span data-ttu-id="6e167-627">SAP spuštění služby ASC HTTP / *Lbrule50013*</span><span class="sxs-lookup"><span data-stu-id="6e167-627">SAP Start Service ASCS HTTP / *Lbrule50013*</span></span> |<span data-ttu-id="6e167-628">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="6e167-628">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="6e167-629">50013</span><span class="sxs-lookup"><span data-stu-id="6e167-629">50013</span></span> |
| <span data-ttu-id="6e167-630">SAP spuštění služby ASC HTTPS nebo *Lbrule50014*</span><span class="sxs-lookup"><span data-stu-id="6e167-630">SAP Start Service ASCS HTTPS / *Lbrule50014*</span></span> |<span data-ttu-id="6e167-631">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="6e167-631">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="6e167-632">50014</span><span class="sxs-lookup"><span data-stu-id="6e167-632">50014</span></span> |
| <span data-ttu-id="6e167-633">Zařazování replikace nebo *Lbrule50016*</span><span class="sxs-lookup"><span data-stu-id="6e167-633">Enqueue Replication / *Lbrule50016*</span></span> |<span data-ttu-id="6e167-634">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="6e167-634">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="6e167-635">50016</span><span class="sxs-lookup"><span data-stu-id="6e167-635">50016</span></span> |
| <span data-ttu-id="6e167-636">SAP spuštění služby YBRAT HTTP *Lbrule51013*</span><span class="sxs-lookup"><span data-stu-id="6e167-636">SAP Start Service ERS HTTP *Lbrule51013*</span></span> |<span data-ttu-id="6e167-637">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="6e167-637">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="6e167-638">51013</span><span class="sxs-lookup"><span data-stu-id="6e167-638">51013</span></span> |
| <span data-ttu-id="6e167-639">SAP spuštění služby YBRAT HTTP *Lbrule51014*</span><span class="sxs-lookup"><span data-stu-id="6e167-639">SAP Start Service ERS HTTP *Lbrule51014*</span></span> |<span data-ttu-id="6e167-640">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="6e167-640">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="6e167-641">51014</span><span class="sxs-lookup"><span data-stu-id="6e167-641">51014</span></span> |
| <span data-ttu-id="6e167-642">Win RM *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="6e167-642">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="6e167-643">5985</span><span class="sxs-lookup"><span data-stu-id="6e167-643">5985</span></span> |
| <span data-ttu-id="6e167-644">Sdílení souborů *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="6e167-644">File Share *Lbrule445*</span></span> | |<span data-ttu-id="6e167-645">445</span><span class="sxs-lookup"><span data-stu-id="6e167-645">445</span></span> |

<span data-ttu-id="6e167-646">_**Tabulka 1:** portu čísla instance SAP NetWeaver ABAP ASC_</span><span class="sxs-lookup"><span data-stu-id="6e167-646">_**Table 1:** Port numbers of the SAP NetWeaver ABAP ASCS instances_</span></span>

<span data-ttu-id="6e167-647">Pak vytvořte tyto koncové body pro SAP NetWeaver Java SCS porty pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="6e167-647">Then, create these load balancing endpoints for the SAP NetWeaver Java SCS ports:</span></span>

| <span data-ttu-id="6e167-648">Název pravidla vyrovnávání služby nebo zatížení</span><span class="sxs-lookup"><span data-stu-id="6e167-648">Service/load balancing rule name</span></span> | <span data-ttu-id="6e167-649">Výchozí čísla portů</span><span class="sxs-lookup"><span data-stu-id="6e167-649">Default port numbers</span></span> | <span data-ttu-id="6e167-650">Konkrétní porty (SCS instance číslem instance 01) (YBRAT s 11)</span><span class="sxs-lookup"><span data-stu-id="6e167-650">Concrete ports for (SCS instance with instance number 01) (ERS with 11)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6e167-651">Zařadit Server / *lbrule3201*</span><span class="sxs-lookup"><span data-stu-id="6e167-651">Enqueue Server / *lbrule3201*</span></span> |<span data-ttu-id="6e167-652">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="6e167-652">32<*InstanceNumber*></span></span> |<span data-ttu-id="6e167-653">3201</span><span class="sxs-lookup"><span data-stu-id="6e167-653">3201</span></span> |
| <span data-ttu-id="6e167-654">Server brány nebo *lbrule3301*</span><span class="sxs-lookup"><span data-stu-id="6e167-654">Gateway Server / *lbrule3301*</span></span> |<span data-ttu-id="6e167-655">33 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="6e167-655">33<*InstanceNumber*></span></span> |<span data-ttu-id="6e167-656">3301</span><span class="sxs-lookup"><span data-stu-id="6e167-656">3301</span></span> |
| <span data-ttu-id="6e167-657">Server zpráv Java / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="6e167-657">Java Message Server / *lbrule3900*</span></span> |<span data-ttu-id="6e167-658">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="6e167-658">39<*InstanceNumber*></span></span> |<span data-ttu-id="6e167-659">3901</span><span class="sxs-lookup"><span data-stu-id="6e167-659">3901</span></span> |
| <span data-ttu-id="6e167-660">Server HTTP zpráv nebo *Lbrule8101*</span><span class="sxs-lookup"><span data-stu-id="6e167-660">Message Server HTTP / *Lbrule8101*</span></span> |<span data-ttu-id="6e167-661">81 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="6e167-661">81<*InstanceNumber*></span></span> |<span data-ttu-id="6e167-662">8101</span><span class="sxs-lookup"><span data-stu-id="6e167-662">8101</span></span> |
| <span data-ttu-id="6e167-663">SAP spuštění služby SCS HTTP / *Lbrule50113*</span><span class="sxs-lookup"><span data-stu-id="6e167-663">SAP Start Service SCS HTTP / *Lbrule50113*</span></span> |<span data-ttu-id="6e167-664">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="6e167-664">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="6e167-665">50113</span><span class="sxs-lookup"><span data-stu-id="6e167-665">50113</span></span> |
| <span data-ttu-id="6e167-666">SAP spuštění služby SCS HTTPS nebo *Lbrule50114*</span><span class="sxs-lookup"><span data-stu-id="6e167-666">SAP Start Service SCS HTTPS / *Lbrule50114*</span></span> |<span data-ttu-id="6e167-667">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="6e167-667">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="6e167-668">50114</span><span class="sxs-lookup"><span data-stu-id="6e167-668">50114</span></span> |
| <span data-ttu-id="6e167-669">Zařazování replikace nebo *Lbrule50116*</span><span class="sxs-lookup"><span data-stu-id="6e167-669">Enqueue Replication / *Lbrule50116*</span></span> |<span data-ttu-id="6e167-670">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="6e167-670">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="6e167-671">50116</span><span class="sxs-lookup"><span data-stu-id="6e167-671">50116</span></span> |
| <span data-ttu-id="6e167-672">SAP spuštění služby YBRAT HTTP *Lbrule51113*</span><span class="sxs-lookup"><span data-stu-id="6e167-672">SAP Start Service ERS HTTP *Lbrule51113*</span></span> |<span data-ttu-id="6e167-673">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="6e167-673">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="6e167-674">51113</span><span class="sxs-lookup"><span data-stu-id="6e167-674">51113</span></span> |
| <span data-ttu-id="6e167-675">SAP spuštění služby YBRAT HTTP *Lbrule51114*</span><span class="sxs-lookup"><span data-stu-id="6e167-675">SAP Start Service ERS HTTP *Lbrule51114*</span></span> |<span data-ttu-id="6e167-676">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="6e167-676">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="6e167-677">51114</span><span class="sxs-lookup"><span data-stu-id="6e167-677">51114</span></span> |
| <span data-ttu-id="6e167-678">Win RM *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="6e167-678">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="6e167-679">5985</span><span class="sxs-lookup"><span data-stu-id="6e167-679">5985</span></span> |
| <span data-ttu-id="6e167-680">Sdílení souborů *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="6e167-680">File Share *Lbrule445*</span></span> | |<span data-ttu-id="6e167-681">445</span><span class="sxs-lookup"><span data-stu-id="6e167-681">445</span></span> |

<span data-ttu-id="6e167-682">_**Tabulka 2:** portu čísla instance SAP NetWeaver Java SCS_</span><span class="sxs-lookup"><span data-stu-id="6e167-682">_**Table 2:** Port numbers of the SAP NetWeaver Java SCS instances_</span></span>

![Obrázek 15: Pravidla pro vyrovnávání zatížení Azure interní Vyrovnávání zatížení výchozí ASC nebo SCS][sap-ha-guide-figure-3004]

<span data-ttu-id="6e167-684">_**Obrázek 15:** ASC nebo SCS výchozí pravidla pro vyrovnávání zatížení Azure interní Vyrovnávání zatížení_</span><span class="sxs-lookup"><span data-stu-id="6e167-684">_**Figure 15:** Default ASCS/SCS load balancing rules for the Azure internal load balancer_</span></span>

<span data-ttu-id="6e167-685">Nastavení IP adresy služby Vyrovnávání zatížení **databázového systému pr1 lb** na IP adresu název virtuálního hostitele instance databázového systému.</span><span class="sxs-lookup"><span data-stu-id="6e167-685">Set the IP address of the load balancer **pr1-lb-dbms** to the IP address of the virtual host name of the DBMS instance.</span></span>

### <span data-ttu-id="6e167-686"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Změňte pravidla pro vyrovnávání zatížení Azure interní Vyrovnávání zatížení výchozí ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="6e167-686"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> Change the ASCS/SCS default load balancing rules for the Azure internal load balancer</span></span>

<span data-ttu-id="6e167-687">Pokud chcete používat jiný čísla pro SAP ASC nebo SCS instancí, je třeba změnit názvy a hodnoty jejich porty z výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6e167-687">If you want to use different numbers for the SAP ASCS or SCS instances, you must change the names and values of their ports from default values.</span></span>

1.  <span data-ttu-id="6e167-688">Na portálu Azure vyberte  **<* SID*> - lb - ASC načíst vyrovnávání ** > **pravidla Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="6e167-688">In the Azure portal, select **<*SID*>-lb-ascs load balancer** > **Load Balancing Rules**.</span></span>
2.  <span data-ttu-id="6e167-689">Pro všechna pravidla, které náleží do instance SAP ASC nebo SCS Vyrovnávání zatížení změňte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="6e167-689">For all load balancing rules that belong to the SAP ASCS or SCS instance, change these values:</span></span>

  * <span data-ttu-id="6e167-690">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="6e167-690">Name</span></span>
  * <span data-ttu-id="6e167-691">Port</span><span class="sxs-lookup"><span data-stu-id="6e167-691">Port</span></span>
  * <span data-ttu-id="6e167-692">Back-end port</span><span class="sxs-lookup"><span data-stu-id="6e167-692">Back-end port</span></span>

  <span data-ttu-id="6e167-693">Například pokud chcete změnit výchozí číslo instance ASC od 00 do 31, budete muset provést změny pro všechny porty uvedené v tabulce 1.</span><span class="sxs-lookup"><span data-stu-id="6e167-693">For example, if you want to change the default ASCS instance number from 00 to 31, you need to make the changes for all ports listed in Table 1.</span></span>

  <span data-ttu-id="6e167-694">Tady je příklad aktualizace pro port *lbrule3200*.</span><span class="sxs-lookup"><span data-stu-id="6e167-694">Here's an example of an update for port *lbrule3200*.</span></span>

  ![Obrázek 16: Změňte pravidla pro vyrovnávání zatížení Azure interní Vyrovnávání zatížení výchozí ASC nebo SCS][sap-ha-guide-figure-3005]

  <span data-ttu-id="6e167-696">_**Obrázek 16:** změňte pravidla pro vyrovnávání zatížení Azure interní Vyrovnávání zatížení výchozí ASC nebo SCS_</span><span class="sxs-lookup"><span data-stu-id="6e167-696">_**Figure 16:** Change the ASCS/SCS default load balancing rules for the Azure internal load balancer_</span></span>

### <span data-ttu-id="6e167-697"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Přidejte virtuální počítače s Windows do domény</span><span class="sxs-lookup"><span data-stu-id="6e167-697"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Add Windows virtual machines to the domain</span></span>

<span data-ttu-id="6e167-698">Když přiřadíte statickou IP adresu pro virtuální počítače, přidejte virtuální počítače k doméně.</span><span class="sxs-lookup"><span data-stu-id="6e167-698">After you assign a static IP address to the virtual machines, add the virtual machines to the domain.</span></span>

![Obrázek 17: Přidáte virtuální počítač k doméně][sap-ha-guide-figure-3006]

<span data-ttu-id="6e167-700">_**Obrázek 17:** přidat virtuální počítač k doméně_</span><span class="sxs-lookup"><span data-stu-id="6e167-700">_**Figure 17:** Add a virtual machine to a domain_</span></span>

### <span data-ttu-id="6e167-701"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Přidat položky registru na obou uzlů clusteru instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="6e167-701"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> Add registry entries on both cluster nodes of the SAP ASCS/SCS instance</span></span>

<span data-ttu-id="6e167-702">Azure Vyrovnávání zatížení má interní nástroj, zavře připojení při připojení jsou nastavte dobu nečinnosti, čas (časový limit nečinnosti).</span><span class="sxs-lookup"><span data-stu-id="6e167-702">Azure Load Balancer has an internal load balancer that closes connections when the connections are idle for a set period of time (an idle timeout).</span></span> <span data-ttu-id="6e167-703">SAP pracovních procesů v dialogovém okně Otevřít připojení instance SAP zařadit do fronty zpracovat při první zařazování/dequeue požadavku musí být odeslána.</span><span class="sxs-lookup"><span data-stu-id="6e167-703">SAP work processes in dialog instances open connections to the SAP enqueue process as soon as the first enqueue/dequeue request needs to be sent.</span></span> <span data-ttu-id="6e167-704">Tato připojení obvykle zůstat zavedené do pracovní proces nebo proces zařazování restartuje.</span><span class="sxs-lookup"><span data-stu-id="6e167-704">These connections usually remain established until the work process or the enqueue process restarts.</span></span> <span data-ttu-id="6e167-705">Ale pokud se jedná o připojení na nastavenou dobu nečinnosti, nástroje pro vyrovnávání zatížení Azure interní zavře připojení.</span><span class="sxs-lookup"><span data-stu-id="6e167-705">However, if the connection is idle for a set period of time, the Azure internal load balancer closes the connections.</span></span> <span data-ttu-id="6e167-706">To není problém, protože pracovní proces SAP připojení do procesu zařazování obnoví, pokud už existuje.</span><span class="sxs-lookup"><span data-stu-id="6e167-706">This isn't a problem because the SAP work process reestablishes the connection to the enqueue process if it no longer exists.</span></span> <span data-ttu-id="6e167-707">Tyto aktivity jsou popsané v trasování vývojáře procesů SAP, ale uživatel vytvořit velké množství další obsah v těchto trasování.</span><span class="sxs-lookup"><span data-stu-id="6e167-707">These activities are documented in the developer traces of SAP processes, but they create a large amount of extra content in those traces.</span></span> <span data-ttu-id="6e167-708">Je vhodné změnit TCP/IP `KeepAliveTime` a `KeepAliveInterval` na obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-708">It's a good idea to change the TCP/IP `KeepAliveTime` and `KeepAliveInterval` on both cluster nodes.</span></span> <span data-ttu-id="6e167-709">Kombinací těchto změn v parametrech TCP/IP s parametry profil SAP, popsanou dále v článku.</span><span class="sxs-lookup"><span data-stu-id="6e167-709">Combine these changes in the TCP/IP parameters with SAP profile parameters, described later in the article.</span></span>

<span data-ttu-id="6e167-710">Chcete-li přidat položky registru na obou uzlů clusteru instance SAP ASC nebo SCS, nejprve přidejte tyto položky registru systému Windows na obou uzlů clusteru systému Windows pro SAP ASC nebo SCS:</span><span class="sxs-lookup"><span data-stu-id="6e167-710">To add registry entries on both cluster nodes of the SAP ASCS/SCS instance, first, add these Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="6e167-711">Cesta</span><span class="sxs-lookup"><span data-stu-id="6e167-711">Path</span></span> | <span data-ttu-id="6e167-712">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="6e167-712">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="6e167-713">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="6e167-713">Variable name</span></span> |`KeepAliveTime` |
| <span data-ttu-id="6e167-714">Typ proměnné</span><span class="sxs-lookup"><span data-stu-id="6e167-714">Variable type</span></span> |<span data-ttu-id="6e167-715">REG_DWORD (decimální):</span><span class="sxs-lookup"><span data-stu-id="6e167-715">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="6e167-716">Hodnota</span><span class="sxs-lookup"><span data-stu-id="6e167-716">Value</span></span> |<span data-ttu-id="6e167-717">120000</span><span class="sxs-lookup"><span data-stu-id="6e167-717">120000</span></span> |
| <span data-ttu-id="6e167-718">Odkaz na dokumentaci</span><span class="sxs-lookup"><span data-stu-id="6e167-718">Link to documentation</span></span> |[<span data-ttu-id="6e167-719">https://technet.microsoft.com/en-us/library/cc957549.aspx</span><span class="sxs-lookup"><span data-stu-id="6e167-719">https://technet.microsoft.com/en-us/library/cc957549.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957549.aspx) |

<span data-ttu-id="6e167-720">_**Tabulka 3:** změnit první parametr TCP/IP_</span><span class="sxs-lookup"><span data-stu-id="6e167-720">_**Table 3:** Change the first TCP/IP parameter_</span></span>

<span data-ttu-id="6e167-721">Pak přidejte této položky registru systému Windows na obou uzlů clusteru systému Windows pro SAP ASC nebo SCS:</span><span class="sxs-lookup"><span data-stu-id="6e167-721">Then, add this Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="6e167-722">Cesta</span><span class="sxs-lookup"><span data-stu-id="6e167-722">Path</span></span> | <span data-ttu-id="6e167-723">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="6e167-723">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="6e167-724">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="6e167-724">Variable name</span></span> |`KeepAliveInterval` |
| <span data-ttu-id="6e167-725">Typ proměnné</span><span class="sxs-lookup"><span data-stu-id="6e167-725">Variable type</span></span> |<span data-ttu-id="6e167-726">REG_DWORD (decimální):</span><span class="sxs-lookup"><span data-stu-id="6e167-726">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="6e167-727">Hodnota</span><span class="sxs-lookup"><span data-stu-id="6e167-727">Value</span></span> |<span data-ttu-id="6e167-728">120000</span><span class="sxs-lookup"><span data-stu-id="6e167-728">120000</span></span> |
| <span data-ttu-id="6e167-729">Odkaz na dokumentaci</span><span class="sxs-lookup"><span data-stu-id="6e167-729">Link to documentation</span></span> |[<span data-ttu-id="6e167-730">https://technet.microsoft.com/en-us/library/cc957548.aspx</span><span class="sxs-lookup"><span data-stu-id="6e167-730">https://technet.microsoft.com/en-us/library/cc957548.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

<span data-ttu-id="6e167-731">_**Tabulka 4:** změnit druhý parametr protokolu TCP/IP_</span><span class="sxs-lookup"><span data-stu-id="6e167-731">_**Table 4:** Change the second TCP/IP parameter_</span></span>

<span data-ttu-id="6e167-732">**Aby se změny projevily, restartujte obou uzlů clusteru**.</span><span class="sxs-lookup"><span data-stu-id="6e167-732">**To apply the changes, restart both cluster nodes**.</span></span>

### <span data-ttu-id="6e167-733"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Nastavení clusteru Windows Server Failover Clustering pro instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="6e167-733"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Set up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance</span></span>

<span data-ttu-id="6e167-734">Nastavení clusteru s podporou služby Windows Server Failover Clustering pro instance SAP ASC nebo SCS zahrnuje tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="6e167-734">Setting up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="6e167-735">Shromažďování uzly clusteru v konfiguraci clusteru</span><span class="sxs-lookup"><span data-stu-id="6e167-735">Collecting the cluster nodes in a cluster configuration</span></span>
- <span data-ttu-id="6e167-736">Konfigurace určující sdílenou složku clusteru</span><span class="sxs-lookup"><span data-stu-id="6e167-736">Configuring a cluster file share witness</span></span>

#### <span data-ttu-id="6e167-737"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Shromažďovat uzly clusteru v konfiguraci clusteru</span><span class="sxs-lookup"><span data-stu-id="6e167-737"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Collect the cluster nodes in a cluster configuration</span></span>

1.  <span data-ttu-id="6e167-738">Přidat Role a funkce průvodce přidejte clusteringu obou uzlů clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6e167-738">In the Add Role and Features Wizard, add failover clustering to both cluster nodes.</span></span>
2.  <span data-ttu-id="6e167-739">Nastavení clusteru převzetí služeb při selhání pomocí Správce clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6e167-739">Set up the failover cluster by using Failover Cluster Manager.</span></span> <span data-ttu-id="6e167-740">Ve Správci clusteru převzetí služeb při selhání vyberte **vytvořením clusteru**a poté přidejte pouze název první clusteru uzlu A. Nepřidávejte druhého uzlu ještě; v pozdější fázi budete přidání druhého uzlu.</span><span class="sxs-lookup"><span data-stu-id="6e167-740">In Failover Cluster Manager, select **Create Cluster**, and then add only the name of the first cluster, node A. Do not add the second node yet; you'll add the second node in a later step.</span></span>

  ![Obrázek 18: Přidejte název serveru nebo virtuálního počítače na prvním uzlu clusteru][sap-ha-guide-figure-3007]

  <span data-ttu-id="6e167-742">_**Obrázek 18:** přidejte název serveru nebo virtuálního počítače na prvním uzlu clusteru_</span><span class="sxs-lookup"><span data-stu-id="6e167-742">_**Figure 18:** Add the server or virtual machine name of the first cluster node_</span></span>

3.  <span data-ttu-id="6e167-743">Zadejte název sítě (název hostitele virtuálního) clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-743">Enter the network name (virtual host name) of the cluster.</span></span>

  ![Obrázek 19: Zadejte název clusteru][sap-ha-guide-figure-3008]

  <span data-ttu-id="6e167-745">_**Obrázek 19:** zadejte název clusteru_</span><span class="sxs-lookup"><span data-stu-id="6e167-745">_**Figure 19:** Enter the cluster name_</span></span>

4.  <span data-ttu-id="6e167-746">Po vytvoření clusteru, spusťte test ověření clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-746">After you've created the cluster, run a cluster validation test.</span></span>

  ![Obrázek 20: Spustit kontrolu ověření clusteru][sap-ha-guide-figure-3009]

  <span data-ttu-id="6e167-748">_**Obrázek 20:** spustit kontrolu ověření clusteru_</span><span class="sxs-lookup"><span data-stu-id="6e167-748">_**Figure 20:** Run the cluster validation check_</span></span>

  <span data-ttu-id="6e167-749">Můžete ignorovat jakékoli upozornění o discích v tomto okamžiku v procesu.</span><span class="sxs-lookup"><span data-stu-id="6e167-749">You can ignore any warnings about disks at this point in the process.</span></span> <span data-ttu-id="6e167-750">Přidáte, že určující sdílené složce a SIOS sdílené disky později.</span><span class="sxs-lookup"><span data-stu-id="6e167-750">You'll add a file share witness and the SIOS shared disks later.</span></span> <span data-ttu-id="6e167-751">V této fázi nemusíte obávat o kvora.</span><span class="sxs-lookup"><span data-stu-id="6e167-751">At this stage, you don't need to worry about having a quorum.</span></span>

  ![Obrázek 21: Nenajde žádný disk kvora][sap-ha-guide-figure-3010]

  <span data-ttu-id="6e167-753">_**Obrázek 21:** nenajde žádný disk kvora_</span><span class="sxs-lookup"><span data-stu-id="6e167-753">_**Figure 21:** No quorum disk is found_</span></span>

  ![Obrázek 22: Prostředek clusteru jádra potřebuje novou IP adresu][sap-ha-guide-figure-3011]

  <span data-ttu-id="6e167-755">_**Obrázek 22:** prostředku clusteru jádra potřebuje novou IP adresu_</span><span class="sxs-lookup"><span data-stu-id="6e167-755">_**Figure 22:** Core cluster resource needs a new IP address_</span></span>

5.  <span data-ttu-id="6e167-756">Změna IP adresy jádra Clusterové služby.</span><span class="sxs-lookup"><span data-stu-id="6e167-756">Change the IP address of the core cluster service.</span></span> <span data-ttu-id="6e167-757">Clusteru nelze spustit dokud změnit IP adresu clusteru služby jádra, protože IP adresa serveru odkazuje na jednom z uzlů virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6e167-757">The cluster can't start until you change the IP address of the core cluster service, because the IP address of the server points to one of the virtual machine nodes.</span></span> <span data-ttu-id="6e167-758">To udělat na **vlastnosti** stránky prostředek IP základní služby clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-758">Do this on the **Properties** page of the core cluster service's IP resource.</span></span>

  <span data-ttu-id="6e167-759">Například je potřeba přiřadit IP adresu (v našem příkladu **10.0.0.42**) pro název virtuálního hostitele clusteru **pr1. ASC vir**.</span><span class="sxs-lookup"><span data-stu-id="6e167-759">For example, we need to assign an IP address (in our example, **10.0.0.42**) for the cluster virtual host name **pr1-ascs-vir**.</span></span>

  ![Obrázek 23: V dialogovém okně vlastností změňte IP adresu][sap-ha-guide-figure-3012]

  <span data-ttu-id="6e167-761">_**Obrázek 23:** v **vlastnosti** dialogové okno pole, změna IP adresy_</span><span class="sxs-lookup"><span data-stu-id="6e167-761">_**Figure 23:** In the **Properties** dialog box, change the IP address_</span></span>

  ![Obrázek 24: Přiřadíte IP adresu, která je vyhrazena pro cluster][sap-ha-guide-figure-3013]

  <span data-ttu-id="6e167-763">_**Obrázek 24:** přiřadit IP adresu, která je vyhrazena pro cluster_</span><span class="sxs-lookup"><span data-stu-id="6e167-763">_**Figure 24:** Assign the IP address that is reserved for the cluster_</span></span>

6.  <span data-ttu-id="6e167-764">Uveďte název virtuálního hostitele clusteru online.</span><span class="sxs-lookup"><span data-stu-id="6e167-764">Bring the cluster virtual host name online.</span></span>

  ![Obrázek 25: Základní služby clusteru je zapnutý a běží a s správnou IP adres][sap-ha-guide-figure-3014]

  <span data-ttu-id="6e167-766">_**Obrázek 25:** základní služby clusteru je zapnutý a běží a s správnou IP adres_</span><span class="sxs-lookup"><span data-stu-id="6e167-766">_**Figure 25:** Cluster core service is up and running, and with the correct IP address_</span></span>

7.  <span data-ttu-id="6e167-767">Přidání druhého uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-767">Add the second cluster node.</span></span>

  <span data-ttu-id="6e167-768">Nyní když základní Clusterová služba spuštěná, můžete přidat druhý uzel clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-768">Now that the core cluster service is up and running, you can add the second cluster node.</span></span>

  ![Obrázek 26: Přidání druhého uzlu clusteru][sap-ha-guide-figure-3015]

  <span data-ttu-id="6e167-770">_**Obrázek 26:** přidání druhého uzlu clusteru_</span><span class="sxs-lookup"><span data-stu-id="6e167-770">_**Figure 26:** Add the second cluster node_</span></span>

8.  <span data-ttu-id="6e167-771">Zadejte název pro druhý hostitele uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-771">Enter a name for the second cluster node host.</span></span>

  ![Obrázek 27: Zadejte název hostitele druhého uzlu clusteru][sap-ha-guide-figure-3016]

  <span data-ttu-id="6e167-773">_**Obrázek 27:** zadejte název hostitele druhého uzlu clusteru_</span><span class="sxs-lookup"><span data-stu-id="6e167-773">_**Figure 27:** Enter the second cluster node host name_</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="6e167-774">Ujistěte se, který **přidat do clusteru veškeré oprávněné úložiště** zaškrtávací políčko je **není** vybrané.</span><span class="sxs-lookup"><span data-stu-id="6e167-774">Be sure that the **Add all eligible storage to the cluster** check box is **NOT** selected.</span></span>  
  >
  >

  ![Obrázek 28: Nevybírejte zaškrtávací políčko][sap-ha-guide-figure-3017]

  <span data-ttu-id="6e167-776">_**Obrázek 28:** provést **není** vyberte zaškrtávací pole_</span><span class="sxs-lookup"><span data-stu-id="6e167-776">_**Figure 28:** Do **not** select the check box_</span></span>

  <span data-ttu-id="6e167-777">Můžete ignorovat upozornění o kvora a disky.</span><span class="sxs-lookup"><span data-stu-id="6e167-777">You can ignore warnings about quorum and disks.</span></span> <span data-ttu-id="6e167-778">Můžete nastavit kvora a sdílet disk později, jak je popsáno v [instalace s DataKeeper Cluster Edition u disku clusteru sdílení, SAP ASC nebo SCS][sap-ha-guide-8.12.3].</span><span class="sxs-lookup"><span data-stu-id="6e167-778">You'll set the quorum and share the disk later, as described in [Installing SIOS DataKeeper Cluster Edition for SAP ASCS/SCS cluster share disk][sap-ha-guide-8.12.3].</span></span>

  ![Obrázek 29: Ignorujte upozornění o disku kvora][sap-ha-guide-figure-3018]

  <span data-ttu-id="6e167-780">_**Obrázek 29:** ignorovat upozornění o disku kvora_</span><span class="sxs-lookup"><span data-stu-id="6e167-780">_**Figure 29:** Ignore warnings about the disk quorum_</span></span>


#### <span data-ttu-id="6e167-781"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Konfigurovat určující sdílenou složku clusteru</span><span class="sxs-lookup"><span data-stu-id="6e167-781"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Configure a cluster file share witness</span></span>

<span data-ttu-id="6e167-782">Konfigurace určující sdílenou složku clusteru zahrnuje tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="6e167-782">Configuring a cluster file share witness involves these tasks:</span></span>

- <span data-ttu-id="6e167-783">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6e167-783">Creating a file share</span></span>
- <span data-ttu-id="6e167-784">Nastavení kvora určující sdílené složky souborů ve Správci clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="6e167-784">Setting the file share witness quorum in Failover Cluster Manager</span></span>

##### <span data-ttu-id="6e167-785"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6e167-785"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Create a file share</span></span>

1.  <span data-ttu-id="6e167-786">Vyberte složku s kopií místo disk kvora.</span><span class="sxs-lookup"><span data-stu-id="6e167-786">Select a file share witness instead of a quorum disk.</span></span> <span data-ttu-id="6e167-787">Tuto volbu podporuje DataKeeper s.</span><span class="sxs-lookup"><span data-stu-id="6e167-787">SIOS DataKeeper supports this option.</span></span>

  <span data-ttu-id="6e167-788">V příkladech v tomto článku je určující sdílenou složku na serveru Active Directory a DNS, který běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-788">In the examples in this article, the file share witness is on the Active Directory/DNS server that is running in Azure.</span></span> <span data-ttu-id="6e167-789">Určující sdílená složka se označuje jako **domcontr-0**.</span><span class="sxs-lookup"><span data-stu-id="6e167-789">The file share witness is called **domcontr-0**.</span></span> <span data-ttu-id="6e167-790">Vzhledem k tomu, že by jste nakonfigurovali připojení VPN do Azure (prostřednictvím sítě Site-to-Site VPN nebo Azure ExpressRoute), vaše/DNS služby Active Directory service je místní a není vhodné pro spuštění souboru sdílet s kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-790">Because you would have configured a VPN connection to Azure (via Site-to-Site VPN or Azure ExpressRoute), your Active Directory/DNS service is on-premises and isn't suitable to run a file share witness.</span></span>

  > [!NOTE]
  > <span data-ttu-id="6e167-791">Pokud služby Active Directory a DNS používá jenom v místě, není konfigurace vaší určující sdílenou složku v operačním systému Windows Active Directory a DNS, který běží na místě.</span><span class="sxs-lookup"><span data-stu-id="6e167-791">If your Active Directory/DNS service runs only on-premises, don't configure your file share witness on the Active Directory/DNS Windows operating system that is running on-premises.</span></span> <span data-ttu-id="6e167-792">Latence sítě mezi uzly clusteru, které jsou spuštěné v Azure a Active Directory a DNS v místním může být příliš velký a způsobit problémy s připojením.</span><span class="sxs-lookup"><span data-stu-id="6e167-792">Network latency between cluster nodes running in Azure and Active Directory/DNS on-premises might be too large and cause connectivity issues.</span></span> <span data-ttu-id="6e167-793">Nezapomeňte nakonfigurovat určující sdílenou složku na virtuální počítač Azure se blíží uzlu clusteru se systémem.</span><span class="sxs-lookup"><span data-stu-id="6e167-793">Be sure to configure the file share witness on an Azure virtual machine that is running close to the cluster node.</span></span>  
  >
  >

  <span data-ttu-id="6e167-794">Jednotka kvora vyžaduje alespoň 1 024 MB volného místa.</span><span class="sxs-lookup"><span data-stu-id="6e167-794">The quorum drive needs at least 1,024 MB of free space.</span></span> <span data-ttu-id="6e167-795">Doporučujeme, abyste hodnotu 2 048 MB volného místa pro disk kvora.</span><span class="sxs-lookup"><span data-stu-id="6e167-795">We recommend 2,048 MB of free space for the quorum drive.</span></span>

2.  <span data-ttu-id="6e167-796">Přidejte objekt názvu clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-796">Add the cluster name object.</span></span>

  ![Obrázek 30: Přiřadíte oprávnění pro sdílenou složku pro objekt názvu clusteru][sap-ha-guide-figure-3019]

  <span data-ttu-id="6e167-798">_**Obrázek 30:** přiřadit oprávnění pro sdílenou složku pro objekt názvu clusteru_</span><span class="sxs-lookup"><span data-stu-id="6e167-798">_**Figure 30:** Assign the permissions on the share for the cluster name object_</span></span>

  <span data-ttu-id="6e167-799">Ujistěte se, že oprávnění zahrnout oprávnění pro změnu data ve sdílené složce pro objekt názvu clusteru (v našem příkladu **pr1. ASC vir$**).</span><span class="sxs-lookup"><span data-stu-id="6e167-799">Be sure that the permissions include the authority to change data in the share for the cluster name object (in our example, **pr1-ascs-vir$**).</span></span>

3.  <span data-ttu-id="6e167-800">Chcete-li přidat objekt názvu clusteru do seznamu, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6e167-800">To add the cluster name object to the list, select **Add**.</span></span> <span data-ttu-id="6e167-801">Změna filtru k vyhledání počítačových objektů, kromě těch, které znázorňuje obrázek 31.</span><span class="sxs-lookup"><span data-stu-id="6e167-801">Change the filter to check for computer objects, in addition to those shown in Figure 31.</span></span>

  ![Obrázek 31: Změňte typy objektů pro zahrnutí počítačů][sap-ha-guide-figure-3020]

  <span data-ttu-id="6e167-803">_**Obrázek 31:** změnit typy objektů pro zahrnutí počítačů_</span><span class="sxs-lookup"><span data-stu-id="6e167-803">_**Figure 31:** Change the Object Types to include computers_</span></span>

  ![Obrázek 32: Zaškrtněte políčko počítače][sap-ha-guide-figure-3021]

  <span data-ttu-id="6e167-805">_**Obrázek 32:** vyberte **počítače** zaškrtávací políčko_</span><span class="sxs-lookup"><span data-stu-id="6e167-805">_**Figure 32:** Select the **Computers** check box_</span></span>

4.  <span data-ttu-id="6e167-806">Objekt názvu clusteru zadejte, jak ukazuje obrázek 31.</span><span class="sxs-lookup"><span data-stu-id="6e167-806">Enter the cluster name object as shown in Figure 31.</span></span> <span data-ttu-id="6e167-807">Protože byl vytvořen záznam, můžete změnit oprávnění, jak ukazuje obrázek 30.</span><span class="sxs-lookup"><span data-stu-id="6e167-807">Because the record has already been created, you can change the permissions, as shown in Figure 30.</span></span>

5.  <span data-ttu-id="6e167-808">Vyberte **zabezpečení** podrobnější kartě do sdílené složky a potom nastavte oprávnění pro objekt názvu clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-808">Select the **Security** tab of the share, and then set more detailed permissions for the cluster name object.</span></span>

  ![Obrázek 33: Nastavte atributy zabezpečení pro objekt názvu clusteru na sdílenou složku kvora souboru][sap-ha-guide-figure-3022]

  <span data-ttu-id="6e167-810">_**Obrázek 33:** nastavit atributy zabezpečení pro objekt názvu clusteru v souboru sdílenou složku kvora_</span><span class="sxs-lookup"><span data-stu-id="6e167-810">_**Figure 33:** Set the security attributes for the cluster name object on the file share quorum_</span></span>

##### <span data-ttu-id="6e167-811"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Nastavení kvora určující sdílené složky souborů ve Správci clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="6e167-811"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Set the file share witness quorum in Failover Cluster Manager</span></span>

1.  <span data-ttu-id="6e167-812">Otevřete kvora nastavení Průvodce konfigurací.</span><span class="sxs-lookup"><span data-stu-id="6e167-812">Open the Configure Quorum Setting Wizard.</span></span>

  ![Obrázek 34: Spuštění Průvodce konfigurace kvora clusteru nastavení][sap-ha-guide-figure-3023]

  <span data-ttu-id="6e167-814">_**Obrázek 34:** spustit Průvodce konfigurace kvora clusteru nastavení_</span><span class="sxs-lookup"><span data-stu-id="6e167-814">_**Figure 34:** Start the Configure Cluster Quorum Setting Wizard_</span></span>

2.  <span data-ttu-id="6e167-815">Na **vyberte konfiguraci kvora** vyberte **vybrat určující disk kvora**.</span><span class="sxs-lookup"><span data-stu-id="6e167-815">On the **Select Quorum Configuration** page, select **Select the quorum witness**.</span></span>

  ![Obrázek 35: Konfigurací kvora, které můžete vybrat z][sap-ha-guide-figure-3024]

  <span data-ttu-id="6e167-817">_**Obrázek 35:** konfigurací kvora, můžete si vybrat z_</span><span class="sxs-lookup"><span data-stu-id="6e167-817">_**Figure 35:** Quorum configurations you can choose from_</span></span>

3.  <span data-ttu-id="6e167-818">Na **vybrat určující disk kvora** vyberte **nakonfigurovat určující sdílenou složku**.</span><span class="sxs-lookup"><span data-stu-id="6e167-818">On the **Select Quorum Witness** page, select **Configure a file share witness**.</span></span>

  ![Obrázek 36: Vyberte sdílenou složku][sap-ha-guide-figure-3025]

  <span data-ttu-id="6e167-820">_**Obrázek 36:** vyberte sdílenou složku_</span><span class="sxs-lookup"><span data-stu-id="6e167-820">_**Figure 36:** Select the file share witness_</span></span>

4.  <span data-ttu-id="6e167-821">Zadejte cestu UNC ke sdílené složce (v našem příkladu \\domcontr 0\FSW).</span><span class="sxs-lookup"><span data-stu-id="6e167-821">Enter the UNC path to the file share (in our example, \\domcontr-0\FSW).</span></span> <span data-ttu-id="6e167-822">Pokud chcete zobrazit seznam změn, můžete provést, vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="6e167-822">To see a list of the changes you can make, select **Next**.</span></span>

  ![Obrázek 37: Zadejte umístění pro sdílení souborů pro sdílenou složku s kopií clusteru][sap-ha-guide-figure-3026]

  <span data-ttu-id="6e167-824">_**Obrázek 37:** zadejte umístění pro sdílení souborů pro sdílenou složku s kopií clusteru_</span><span class="sxs-lookup"><span data-stu-id="6e167-824">_**Figure 37:** Define the file share location for the witness share_</span></span>

5.  <span data-ttu-id="6e167-825">Vyberte požadované změny a pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="6e167-825">Select the changes you want, and then select **Next**.</span></span> <span data-ttu-id="6e167-826">Je třeba úspěšně znovu nakonfigurovat konfiguraci clusteru, jak ukazuje obrázek 38.</span><span class="sxs-lookup"><span data-stu-id="6e167-826">You need to successfully reconfigure the cluster configuration as shown in Figure 38.</span></span>  

  ![Obrázek 38: Potvrzení, že jste jste změnili konfiguraci clusteru][sap-ha-guide-figure-3027]

  <span data-ttu-id="6e167-828">_**Obrázek 38:** potvrzení, že jste jste změnili konfiguraci clusteru_</span><span class="sxs-lookup"><span data-stu-id="6e167-828">_**Figure 38:** Confirmation that you've reconfigured the cluster_</span></span>

<span data-ttu-id="6e167-829">Po úspěšném nainstalování clusteru převzetí služeb při selhání systému Windows, třeba změny provedené některé prahové hodnoty pro přizpůsobení převzetí služeb při selhání detekce podmínky v Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-829">After installing the Windows Failover Cluster successfully, changes need to be made to some thresholds to adapt failover detection to conditions in Azure.</span></span> <span data-ttu-id="6e167-830">Parametry, které chcete změnit, jsou popsané v tomto blogu: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/.</span><span class="sxs-lookup"><span data-stu-id="6e167-830">The parameters to be changed are documented in this blog: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/ .</span></span> <span data-ttu-id="6e167-831">Za předpokladu, že dva virtuální počítače, které sestavení konfigurace clusteru systému Windows pro ASC nebo SCS jsou ve stejné podsíti, je nutné změnit tak, aby tyto hodnoty následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="6e167-831">Assuming that your two VMs that build the Windows Cluster Configuration for ASCS/SCS are in the same SubNet, the following parameters need to be changed to these values:</span></span>
- <span data-ttu-id="6e167-832">SameSubNetDelay = 2</span><span class="sxs-lookup"><span data-stu-id="6e167-832">SameSubNetDelay = 2</span></span>
- <span data-ttu-id="6e167-833">SameSubNetThreshold = 15</span><span class="sxs-lookup"><span data-stu-id="6e167-833">SameSubNetThreshold = 15</span></span>

<span data-ttu-id="6e167-834">Tato nastavení byly testovány s zákazníků a poskytuje dobrý ohrožení chcete být odolní dostatečně na jedné straně.</span><span class="sxs-lookup"><span data-stu-id="6e167-834">These settings were tested with customers and provided a good compromise to be resilient enough on the one side.</span></span> <span data-ttu-id="6e167-835">Na druhé straně se tato nastavení fast poskytuje dostatek převzetí služeb při selhání ve skutečné chybové stavy na SAP selhání uzlu nebo virtuálního počítače nebo softwaru.</span><span class="sxs-lookup"><span data-stu-id="6e167-835">On the other hand those settings were providing fast enough failover in real error conditions on SAP software or node/VM failure.</span></span> 

### <span data-ttu-id="6e167-836"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>Instalace s DataKeeper Cluster Edition pro SAP ASC nebo SCS disk clusteru sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6e167-836"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> Install SIOS DataKeeper Cluster Edition for the SAP ASCS/SCS cluster share disk</span></span>

<span data-ttu-id="6e167-837">Teď máte fungující konfiguraci služby Windows Server Failover Clustering v Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-837">You now have a working Windows Server Failover Clustering configuration in Azure.</span></span> <span data-ttu-id="6e167-838">Ale instalaci instance SAP ASC nebo SCS, budete potřebovat prostředek sdíleného disku.</span><span class="sxs-lookup"><span data-stu-id="6e167-838">But, to install an SAP ASCS/SCS instance, you need a shared disk resource.</span></span> <span data-ttu-id="6e167-839">Nelze vytvořit sdílené síťové prostředky, které potřebujete v Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-839">You cannot create the shared disk resources you need in Azure.</span></span> <span data-ttu-id="6e167-840">S DataKeeper Cluster Edition je řešení třetí strany, které můžete použít k vytvoření sdílené síťové prostředky.</span><span class="sxs-lookup"><span data-stu-id="6e167-840">SIOS DataKeeper Cluster Edition is a third-party solution you can use to create shared disk resources.</span></span>

<span data-ttu-id="6e167-841">Instalace s DataKeeper Cluster Edition pro sdílenou složku disk clusteru SAP ASC nebo SCS zahrnuje tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="6e167-841">Installing SIOS DataKeeper Cluster Edition for the SAP ASCS/SCS cluster share disk involves these tasks:</span></span>

- <span data-ttu-id="6e167-842">Přidání rozhraní .NET Framework 3.5</span><span class="sxs-lookup"><span data-stu-id="6e167-842">Adding the .NET Framework 3.5</span></span>
- <span data-ttu-id="6e167-843">Instalace SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="6e167-843">Installing SIOS DataKeeper</span></span>
- <span data-ttu-id="6e167-844">Nastavení služby s DataKeeper</span><span class="sxs-lookup"><span data-stu-id="6e167-844">Setting up SIOS DataKeeper</span></span>

#### <span data-ttu-id="6e167-845"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>Přidání rozhraní .NET Framework 3.5</span><span class="sxs-lookup"><span data-stu-id="6e167-845"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> Add the .NET Framework 3.5</span></span>
<span data-ttu-id="6e167-846">Rozhraní Microsoft .NET Framework 3.5 se automaticky aktivovat nebo není nainstalovaná v systému Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="6e167-846">The Microsoft .NET Framework 3.5 isn't automatically activated or installed on Windows Server 2012 R2.</span></span> <span data-ttu-id="6e167-847">Protože DataKeeper s vyžaduje rozhraní .NET Framework na všech uzlech, jež nainstalujete DataKeeper, musíte nainstalovat rozhraní .NET Framework 3.5 v hostovaném operačním systému všech virtuálních počítačů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-847">Because SIOS DataKeeper requires the .NET Framework to be on all nodes that you install DataKeeper on, you must install the .NET Framework 3.5 on the guest operating system of all virtual machines in the cluster.</span></span>

<span data-ttu-id="6e167-848">Chcete-li přidat rozhraní .NET Framework 3.5 dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="6e167-848">There are two ways to add the .NET Framework 3.5:</span></span>

- <span data-ttu-id="6e167-849">Použití funkce Průvodce přidáním rolí a v systému Windows, jak ukazuje obrázek 39.</span><span class="sxs-lookup"><span data-stu-id="6e167-849">Use the Add Roles and Features Wizard in Windows as shown in Figure 39.</span></span>

  ![Obrázek 39: Instalace rozhraní .NET Framework 3.5 pomocí funkce Průvodce přidáním rolí a][sap-ha-guide-figure-3028]

  <span data-ttu-id="6e167-851">_**Obrázek 39:** instalace rozhraní .NET Framework 3.5 pomocí funkce Průvodce přidáním rolí a_</span><span class="sxs-lookup"><span data-stu-id="6e167-851">_**Figure 39:** Install the .NET Framework 3.5 by using the Add Roles and Features Wizard_</span></span>

  ![Obrázek 40: Průběh instalace panelu při instalaci rozhraní .NET Framework 3.5 pomocí Přidat role a funkce Průvodce][sap-ha-guide-figure-3029]

  <span data-ttu-id="6e167-853">_**Obrázek 40:** průběh instalace panelu při instalaci rozhraní .NET Framework 3.5 pomocí Přidat role a funkce Průvodce_</span><span class="sxs-lookup"><span data-stu-id="6e167-853">_**Figure 40:** Installation progress bar when you install the .NET Framework 3.5 by using the Add Roles and Features Wizard_</span></span>

- <span data-ttu-id="6e167-854">Použijte nástroj příkazového řádku nástroje dism.exe.</span><span class="sxs-lookup"><span data-stu-id="6e167-854">Use the command-line tool dism.exe.</span></span> <span data-ttu-id="6e167-855">Pro tento typ instalace musíte pro přístup k adresáři SxS na instalačním médiu systému Windows.</span><span class="sxs-lookup"><span data-stu-id="6e167-855">For this type of installation, you need to access the SxS directory on the Windows installation media.</span></span> <span data-ttu-id="6e167-856">Na příkazovém řádku se zvýšenými oprávněními zadejte:</span><span class="sxs-lookup"><span data-stu-id="6e167-856">At an elevated command prompt, type:</span></span>

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <span data-ttu-id="6e167-857"><a name="dd41d5a2-8083-415b-9878-839652812102"></a>Nainstalujte SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="6e167-857"><a name="dd41d5a2-8083-415b-9878-839652812102"></a> Install SIOS DataKeeper</span></span>

<span data-ttu-id="6e167-858">Nainstalujte na každém uzlu v clusteru s DataKeeper Cluster Edition.</span><span class="sxs-lookup"><span data-stu-id="6e167-858">Install SIOS DataKeeper Cluster Edition on each node in the cluster.</span></span> <span data-ttu-id="6e167-859">Pokud chcete vytvořit virtuální sdílené úložiště s s DataKeeper, vytvořte synchronizoval zrcadlení a pak simulovat sdílené úložiště clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-859">To create virtual shared storage with SIOS DataKeeper, create a synced mirror and then simulate cluster shared storage.</span></span>

<span data-ttu-id="6e167-860">Před instalací softwaru SIOS vytvořit uživatele domény **DataKeeperSvc**.</span><span class="sxs-lookup"><span data-stu-id="6e167-860">Before you install the SIOS software, create the domain user **DataKeeperSvc**.</span></span>

> [!NOTE]
> <span data-ttu-id="6e167-861">Přidat **DataKeeperSvc** uživateli **místního správce** v obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-861">Add the **DataKeeperSvc** user to the **Local Administrator** group on both cluster nodes.</span></span>
>
>

<span data-ttu-id="6e167-862">Instalace s DataKeeper:</span><span class="sxs-lookup"><span data-stu-id="6e167-862">To install SIOS DataKeeper:</span></span>

1.  <span data-ttu-id="6e167-863">Nainstalujte SIOS software do obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-863">Install the SIOS software on both cluster nodes.</span></span>

  ![Instalační program SIOS][sap-ha-guide-figure-3030]

  ![41 obrázek: První stránka instalace s DataKeeper][sap-ha-guide-figure-3031]

  <span data-ttu-id="6e167-866">_**Obrázek 41:** první stránka s DataKeeper instalace_</span><span class="sxs-lookup"><span data-stu-id="6e167-866">_**Figure 41:** First page of the SIOS DataKeeper installation_</span></span>

2.  <span data-ttu-id="6e167-867">V dialogovém okně znázorňuje obrázek 42 vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="6e167-867">In the dialog box shown in Figure 42, select **Yes**.</span></span>

  ![Obrázek 42: DataKeeper vás upozorní, že služba bude zakázán][sap-ha-guide-figure-3032]

  <span data-ttu-id="6e167-869">_**Obrázek 42:** DataKeeper informuje, že služba bude zakázán_</span><span class="sxs-lookup"><span data-stu-id="6e167-869">_**Figure 42:** DataKeeper informs you that a service will be disabled_</span></span>

3.  <span data-ttu-id="6e167-870">V dialogovém okně zobrazí obrázek 43 doporučujeme, abyste vybrali **účet domény nebo serveru**.</span><span class="sxs-lookup"><span data-stu-id="6e167-870">In the dialog box shown in Figure 43, we recommend that you select **Domain or Server account**.</span></span>

  ![Obrázek 43: Výběr uživatele s DataKeeper][sap-ha-guide-figure-3033]

  <span data-ttu-id="6e167-872">_**Obrázek 43:** výběr uživatele s DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="6e167-872">_**Figure 43:** User selection for SIOS DataKeeper_</span></span>

4.  <span data-ttu-id="6e167-873">Zadejte uživatelské jméno pro účet domény a hesla, které jste vytvořili pro DataKeeper s.</span><span class="sxs-lookup"><span data-stu-id="6e167-873">Enter the domain account user name and passwords that you created for SIOS DataKeeper.</span></span>

  ![Obrázek 44: Zadejte uživatelské jméno domény a heslo pro instalace s DataKeeper][sap-ha-guide-figure-3034]

  <span data-ttu-id="6e167-875">_**Obrázek 44:** zadejte uživatelské jméno domény a heslo pro instalaci s DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="6e167-875">_**Figure 44:** Enter the domain user name and password for the SIOS DataKeeper installation_</span></span>

5.  <span data-ttu-id="6e167-876">Licenční klíč pro instanci s DataKeeper nainstalujte, jak ukazuje obrázek 45.</span><span class="sxs-lookup"><span data-stu-id="6e167-876">Install the license key for your SIOS DataKeeper instance as shown in Figure 45.</span></span>

  ![45 obrázek: Zadejte klíč licence s DataKeeper][sap-ha-guide-figure-3035]

  <span data-ttu-id="6e167-878">_**Obrázek 45:** zadejte klíč DataKeeper s licencí_</span><span class="sxs-lookup"><span data-stu-id="6e167-878">_**Figure 45:** Enter your SIOS DataKeeper license key_</span></span>

6.  <span data-ttu-id="6e167-879">Po zobrazení výzvy restartujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6e167-879">When prompted, restart the virtual machine.</span></span>

#### <span data-ttu-id="6e167-880"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>Nastavení pro zařízení s DataKeeper</span><span class="sxs-lookup"><span data-stu-id="6e167-880"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> Set up SIOS DataKeeper</span></span>

<span data-ttu-id="6e167-881">Po instalaci s DataKeeper oba uzly, budete muset spustit konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="6e167-881">After you install SIOS DataKeeper on both nodes, you need to start the configuration.</span></span> <span data-ttu-id="6e167-882">Cílem konfigurace je tak, aby měl synchronní data replikace mezi další virtuální pevné disky připojené ke každému z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e167-882">The goal of the configuration is to have synchronous data replication between the additional VHDs attached to each of the virtual machines.</span></span>

1.  <span data-ttu-id="6e167-883">Spusťte nástroj Správa DataKeeper a konfigurace a potom vyberte **připojit Server**.</span><span class="sxs-lookup"><span data-stu-id="6e167-883">Start the DataKeeper Management and Configuration tool, and then select **Connect Server**.</span></span> <span data-ttu-id="6e167-884">(V obrázku 46, tato možnost je v kroužku červeně.)</span><span class="sxs-lookup"><span data-stu-id="6e167-884">(In Figure 46, this option is circled in red.)</span></span>

  ![46 obrázek: Nástroj Konfigurace a SIOS DataKeeper správy][sap-ha-guide-figure-3036]

  <span data-ttu-id="6e167-886">_**Obrázek 46:** s DataKeeper správu a konfiguraci nástroje_</span><span class="sxs-lookup"><span data-stu-id="6e167-886">_**Figure 46:** SIOS DataKeeper Management and Configuration tool_</span></span>

2.  <span data-ttu-id="6e167-887">Zadejte název nebo adresu TCP/IP prvního uzlu, který nástroj Správa a konfigurace by se měly připojit k a v druhém kroku, ve druhém uzlu.</span><span class="sxs-lookup"><span data-stu-id="6e167-887">Enter the name or TCP/IP address of the first node the Management and Configuration tool should connect to, and, in a second step, the second node.</span></span>

  ![Obrázek 47: Vložit název nebo adresu TCP/IP prvního uzlu pro správu a konfigurační nástroj by měl připojit a v druhém kroku, ve druhém uzlu][sap-ha-guide-figure-3037]

  <span data-ttu-id="6e167-889">_**Obrázek 47:** vložit název nebo adresu TCP/IP prvního uzlu nástroj pro správu a konfiguraci by měl připojit a v druhém kroku, ve druhém uzlu_</span><span class="sxs-lookup"><span data-stu-id="6e167-889">_**Figure 47:** Insert the name or TCP/IP address of the first node the Management and Configuration tool should connect to, and in a second step, the second node_</span></span>

3.  <span data-ttu-id="6e167-890">Vytvoření úlohy replikace mezi dvěma uzly.</span><span class="sxs-lookup"><span data-stu-id="6e167-890">Create the replication job between the two nodes.</span></span>

  ![Obrázek 48: Vytvoření úlohy replikace][sap-ha-guide-figure-3038]

  <span data-ttu-id="6e167-892">_**Obrázek 48:** vytvořit úlohu replikace_</span><span class="sxs-lookup"><span data-stu-id="6e167-892">_**Figure 48:** Create a replication job_</span></span>

  <span data-ttu-id="6e167-893">Průvodce vás provede procesem vytvoření úlohy replikace.</span><span class="sxs-lookup"><span data-stu-id="6e167-893">A wizard guides you through the process of creating a replication job.</span></span>
4.  <span data-ttu-id="6e167-894">Zadejte název, adresa TCP/IP a svazku zdrojový uzel.</span><span class="sxs-lookup"><span data-stu-id="6e167-894">Define the name, TCP/IP address, and disk volume of the source node.</span></span>

  ![Obrázek 49: Definujte název úlohy replikace][sap-ha-guide-figure-3039]

  <span data-ttu-id="6e167-896">_**Obrázek 49:** definujte název úlohy replikace_</span><span class="sxs-lookup"><span data-stu-id="6e167-896">_**Figure 49:** Define the name of the replication job_</span></span>

  ![Obrázek 50: Zadejte základní data pro uzel, které by měly být aktuální zdrojový uzel][sap-ha-guide-figure-3040]

  <span data-ttu-id="6e167-898">_**Obrázek 50:** zadat základní data pro uzel, které by měly být aktuální zdrojový uzel_</span><span class="sxs-lookup"><span data-stu-id="6e167-898">_**Figure 50:** Define the base data for the node, which should be the current source node_</span></span>

5.  <span data-ttu-id="6e167-899">Zadejte název, adresa TCP/IP a svazku cílový uzel.</span><span class="sxs-lookup"><span data-stu-id="6e167-899">Define the name, TCP/IP address, and disk volume of the target node.</span></span>

  ![Obrázek 51: Zadejte základní data pro uzel, které by měly být aktuální cílový uzel][sap-ha-guide-figure-3041]

  <span data-ttu-id="6e167-901">_**Obrázek 51:** zadat základní data pro uzel, které by měly být aktuální cílový uzel_</span><span class="sxs-lookup"><span data-stu-id="6e167-901">_**Figure 51:** Define the base data for the node, which should be the current target node_</span></span>

6.  <span data-ttu-id="6e167-902">Definujte algoritmy komprese.</span><span class="sxs-lookup"><span data-stu-id="6e167-902">Define the compression algorithms.</span></span> <span data-ttu-id="6e167-903">V našem příkladu doporučujeme kompresi datového proudu replikace.</span><span class="sxs-lookup"><span data-stu-id="6e167-903">In our example, we recommend that you compress the replication stream.</span></span> <span data-ttu-id="6e167-904">Hlavně v situacích, opakované synchronizace kompresi datového proudu replikace výrazně snižuje dobu Opakovaná synchronizace.</span><span class="sxs-lookup"><span data-stu-id="6e167-904">Especially in resynchronization situations, the compression of the replication stream dramatically reduces resynchronization time.</span></span> <span data-ttu-id="6e167-905">Všimněte si, že komprese používá prostředky procesoru a paměť RAM virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6e167-905">Note that compression uses the CPU and RAM resources of a virtual machine.</span></span> <span data-ttu-id="6e167-906">Jako hodnota se zvyšuje rychlost komprese, takže nemá objem prostředků procesoru, které používá.</span><span class="sxs-lookup"><span data-stu-id="6e167-906">As the compression rate increases, so does the volume of CPU resources used.</span></span> <span data-ttu-id="6e167-907">Můžete také upravit toto nastavení později.</span><span class="sxs-lookup"><span data-stu-id="6e167-907">You also can adjust this setting later.</span></span>

7.  <span data-ttu-id="6e167-908">Další nastavení, které je potřeba zkontrolovat se, zda dojde k replikaci synchronně nebo asynchronně.</span><span class="sxs-lookup"><span data-stu-id="6e167-908">Another setting you need to check is whether the replication occurs asynchronously or synchronously.</span></span> <span data-ttu-id="6e167-909">*Pokud budete chránit SAP ASC nebo SCS konfigurace, je nutné použít synchronní replikace*.</span><span class="sxs-lookup"><span data-stu-id="6e167-909">*When you protect SAP ASCS/SCS configurations, you must use synchronous replication*.</span></span>  

  ![Obrázek 52: Definování podrobnosti k replikaci][sap-ha-guide-figure-3042]

  <span data-ttu-id="6e167-911">_**Obrázek 52:** definovat podrobnosti k replikaci_</span><span class="sxs-lookup"><span data-stu-id="6e167-911">_**Figure 52:** Define replication details_</span></span>

8.  <span data-ttu-id="6e167-912">Zadejte, zda svazek, který se replikují úlohou replikace by měly být zastoupeny ke konfiguraci clusteru Windows Server Failover Clustering jako sdílený disk.</span><span class="sxs-lookup"><span data-stu-id="6e167-912">Define whether the volume that is replicated by the replication job should be represented to a Windows Server Failover Clustering cluster configuration as a shared disk.</span></span> <span data-ttu-id="6e167-913">Pro konfiguraci SAP ASC nebo SCS vyberte **Ano** tak, aby Windows cluster uvidí replikované svazek jako sdílený disk, který můžete použít jako svazek clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-913">For the SAP ASCS/SCS configuration, select **Yes** so that the Windows cluster sees the replicated volume as a shared disk that it can use as a cluster volume.</span></span>

  ![Obrázek 53: Vyberte možnost Ano nastavit replikované svazek jako svazek clusteru][sap-ha-guide-figure-3043]

  <span data-ttu-id="6e167-915">_**Obrázek 53:** vyberte **Ano** nastavit replikované svazek jako svazek clusteru_</span><span class="sxs-lookup"><span data-stu-id="6e167-915">_**Figure 53:** Select **Yes** to set the replicated volume as a cluster volume_</span></span>

  <span data-ttu-id="6e167-916">Po vytvoření svazku nástroj DataKeeper Správa a konfigurace ukazuje, že je úloha replikace aktivní.</span><span class="sxs-lookup"><span data-stu-id="6e167-916">After the volume is created, the DataKeeper Management and Configuration tool shows that the replication job is active.</span></span>

  ![Obrázek 54: DataKeeper synchronní zrcadlení pro sdílenou složku disk SAP ASC nebo SCS je aktivní][sap-ha-guide-figure-3044]

  <span data-ttu-id="6e167-918">_**Obrázek 54:** DataKeeper synchronní zrcadlení pro SAP ASC nebo SCS sdílet disk je aktivní_</span><span class="sxs-lookup"><span data-stu-id="6e167-918">_**Figure 54:** DataKeeper synchronous mirroring for the SAP ASCS/SCS share disk is active_</span></span>

  <span data-ttu-id="6e167-919">Správce clusteru převzetí služeb při selhání teď disk jako disk s DataKeeper ukazuje, jak ukazuje obrázek 55.</span><span class="sxs-lookup"><span data-stu-id="6e167-919">Failover Cluster Manager now shows the disk as a DataKeeper disk, as shown in Figure 55.</span></span>

  ![Obrázek 55: Správce clusteru převzetí služeb při selhání zobrazuje na disk, který DataKeeper replikovat][sap-ha-guide-figure-3045]

  <span data-ttu-id="6e167-921">_**Obrázek 55:** Správce clusteru převzetí služeb při selhání disku ukazuje, že DataKeeper replikovat_</span><span class="sxs-lookup"><span data-stu-id="6e167-921">_**Figure 55:** Failover Cluster Manager shows the disk that DataKeeper replicated_</span></span>

## <span data-ttu-id="6e167-922"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Instalace systému SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="6e167-922"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a> Install the SAP NetWeaver system</span></span>

<span data-ttu-id="6e167-923">Instalace databázového systému jsme nebude popisují nastavení lišit v závislosti na tom, které můžete použít systém databázového systému.</span><span class="sxs-lookup"><span data-stu-id="6e167-923">We won’t describe the DBMS setup because setups vary depending on the DBMS system you use.</span></span> <span data-ttu-id="6e167-924">Ale předpokládáme, že jsou aspekty vysoké dostupnosti s databázového systému řešit pomocí funkce, které různých výrobců databázového systému podpora pro Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-924">However, we assume that high-availability concerns with the DBMS are addressed with the functionalities the different DBMS vendors support for Azure.</span></span> <span data-ttu-id="6e167-925">Například Always On nebo zrcadlení databáze systému SQL Server a Oracle Data Guard pro databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="6e167-925">For example, Always On or database mirroring for SQL Server, and Oracle Data Guard for Oracle databases.</span></span> <span data-ttu-id="6e167-926">Ve scénáři, které používáme v tomto článku jsme nebyla přidat další ochranu do databázového systému.</span><span class="sxs-lookup"><span data-stu-id="6e167-926">In the scenario we use in this article, we didn't add more protection to the DBMS.</span></span>

<span data-ttu-id="6e167-927">Když různé služby databázového systému interakci s tímto typem Clusterované konfigurace SAP ASC nebo SCS v Azure neexistují žádné zvláštní požadavky.</span><span class="sxs-lookup"><span data-stu-id="6e167-927">There are no special considerations when different DBMS services interact with this kind of clustered SAP ASCS/SCS configuration in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="6e167-928">Postupy instalace systémů SAP NetWeaver ABAP, Java systémy a systémy ABAP + Java jsou téměř shodné.</span><span class="sxs-lookup"><span data-stu-id="6e167-928">The installation procedures of SAP NetWeaver ABAP systems, Java systems, and ABAP+Java systems are almost identical.</span></span> <span data-ttu-id="6e167-929">Nejdůležitější rozdíl je, že systému SAP ABAP má jednu instanci ASC.</span><span class="sxs-lookup"><span data-stu-id="6e167-929">The most significant difference is that an SAP ABAP system has one ASCS instance.</span></span> <span data-ttu-id="6e167-930">V systému SAP Java má jednu instanci SCS.</span><span class="sxs-lookup"><span data-stu-id="6e167-930">The SAP Java system has one SCS instance.</span></span> <span data-ttu-id="6e167-931">V systému SAP ABAP + Java obsahuje jednu instanci ASC a jednu instanci SCS spuštěné ve stejné skupině clusteru převzetí služeb při selhání Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6e167-931">The SAP ABAP+Java system has one ASCS instance and one SCS instance running in the same Microsoft failover cluster group.</span></span> <span data-ttu-id="6e167-932">Případné rozdíly instalace pro každou SAP NetWeaver instalace zásobníku se výslovně uveden.</span><span class="sxs-lookup"><span data-stu-id="6e167-932">Any installation differences for each SAP NetWeaver installation stack are explicitly mentioned.</span></span> <span data-ttu-id="6e167-933">Můžete předpokládat, že všechny ostatní části jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="6e167-933">You can assume that all other parts are the same.</span></span>  
>
>

### <span data-ttu-id="6e167-934"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Instalace s vysokou dostupností ASC nebo SCS instance SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-934"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a> Install SAP with a high-availability ASCS/SCS instance</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6e167-935">Nezapomeňte toto stránkovacím souboru na svazcích DataKeeper zrcadlení.</span><span class="sxs-lookup"><span data-stu-id="6e167-935">Be sure not to place your page file on DataKeeper mirrored volumes.</span></span> <span data-ttu-id="6e167-936">DataKeeper nepodporuje zrcadlové svazky.</span><span class="sxs-lookup"><span data-stu-id="6e167-936">DataKeeper does not support mirrored volumes.</span></span> <span data-ttu-id="6e167-937">Můžete ponechat stránkovacím souboru na dočasné jednotce D Azure virtuálního počítače, který je výchozí.</span><span class="sxs-lookup"><span data-stu-id="6e167-937">You can leave your page file on the temporary drive D of an Azure virtual machine, which is the default.</span></span> <span data-ttu-id="6e167-938">Pokud není již existuje, přesuňte stránkovací soubor Windows na jednotku D virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-938">If it's not already there, move the Windows page file to drive D of your Azure virtual machine.</span></span>
>
>

<span data-ttu-id="6e167-939">Instalace s vysokou dostupností ASC nebo SCS instance SAP zahrnuje tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="6e167-939">Installing SAP with a high-availability ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="6e167-940">Vytváření název virtuálního hostitele pro SAP ASC nebo SCS skupinu prostředků clusteru</span><span class="sxs-lookup"><span data-stu-id="6e167-940">Creating a virtual host name for the clustered SAP ASCS/SCS instance</span></span>
- <span data-ttu-id="6e167-941">Instalace prvního uzlu clusteru SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-941">Installing the SAP first cluster node</span></span>
- <span data-ttu-id="6e167-942">Úprava profilu SAP ASC nebo SCS instance</span><span class="sxs-lookup"><span data-stu-id="6e167-942">Modifying the SAP profile of the ASCS/SCS instance</span></span>
- <span data-ttu-id="6e167-943">Přidání port testu</span><span class="sxs-lookup"><span data-stu-id="6e167-943">Adding a probe port</span></span>
- <span data-ttu-id="6e167-944">Otevřete port testu brány firewall systému Windows</span><span class="sxs-lookup"><span data-stu-id="6e167-944">Opening the Windows firewall probe port</span></span>

#### <span data-ttu-id="6e167-945"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Vytvořte název virtuálního hostitele pro SAP ASC nebo SCS skupinu prostředků clusteru</span><span class="sxs-lookup"><span data-stu-id="6e167-945"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a> Create a virtual host name for the clustered SAP ASCS/SCS instance</span></span>

1.  <span data-ttu-id="6e167-946">Ve Správci DNS systému Windows vytvořte záznam DNS pro název virtuálního hostitele ASC nebo SCS instance.</span><span class="sxs-lookup"><span data-stu-id="6e167-946">In the Windows DNS manager, create a DNS entry for the virtual host name of the ASCS/SCS instance.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="6e167-947">IP adresa, která přiřadíte název virtuálního hostitele ASC nebo SCS instance musí být stejná jako adresa IP, který jste přiřadili k vyrovnávání zatížení Azure (**<*SID*> - lb - ASC **).</span><span class="sxs-lookup"><span data-stu-id="6e167-947">The IP address that you assign to the virtual host name of the ASCS/SCS instance must be the same as the IP address that you assigned to Azure Load Balancer (**<*SID*>-lb-ascs**).</span></span>  
  >
  >

  <span data-ttu-id="6e167-948">IP adresa virtuální název hostitele SAP ASC nebo SCS (**pr1. ASC sap**) je stejný jako IP adresu služby Vyrovnávání zatížení Azure (**pr1-lb Asc**).</span><span class="sxs-lookup"><span data-stu-id="6e167-948">The IP address of the virtual SAP ASCS/SCS host name (**pr1-ascs-sap**) is the same as the IP address of Azure Load Balancer (**pr1-lb-ascs**).</span></span>

  ![Obrázek 56: Definování položky DNS pro virtuální název clusteru SAP ASC nebo SCS a adresa TCP/IP][sap-ha-guide-figure-3046]

  <span data-ttu-id="6e167-950">_**Obrázek 56:** definovat položku DNS pro virtuální název clusteru SAP ASC nebo SCS a adresa TCP/IP_</span><span class="sxs-lookup"><span data-stu-id="6e167-950">_**Figure 56:** Define the DNS entry for the SAP ASCS/SCS cluster virtual name and TCP/IP address_</span></span>

2.  <span data-ttu-id="6e167-951">Chcete-li definovat přiřazené název virtuálního hostitele IP adresy, vyberte **Správce DNS** > **domény**.</span><span class="sxs-lookup"><span data-stu-id="6e167-951">To define the IP address assigned to the virtual host name, select **DNS Manager** > **Domain**.</span></span>

  ![Obrázek 57: Nový virtuální název a adresu TCP/IP pro konfiguraci clusteru SAP ASC nebo SCS][sap-ha-guide-figure-3047]

  <span data-ttu-id="6e167-953">_**Obrázek 57:** nový virtuální název a TCP/IP adres pro konfiguraci clusteru SAP ASC nebo SCS_</span><span class="sxs-lookup"><span data-stu-id="6e167-953">_**Figure 57:** New virtual name and TCP/IP address for SAP ASCS/SCS cluster configuration_</span></span>

#### <span data-ttu-id="6e167-954"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Instalace prvního uzlu clusteru SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-954"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> Install the SAP first cluster node</span></span>

1.  <span data-ttu-id="6e167-955">Provést první možnost uzlu clusteru na uzlu clusteru A. Například **pr1-ASC-0** hostitele.</span><span class="sxs-lookup"><span data-stu-id="6e167-955">Execute the first cluster node option on cluster node A. For example, on the **pr1-ascs-0** host.</span></span>
2.  <span data-ttu-id="6e167-956">Ponechat výchozí porty pro vyrovnávání zatížení Azure interní, vyberte:</span><span class="sxs-lookup"><span data-stu-id="6e167-956">To keep the default ports for the Azure internal load balancer, select:</span></span>

  * <span data-ttu-id="6e167-957">**Systém ABAP**: **Asc** instance číslo **00**</span><span class="sxs-lookup"><span data-stu-id="6e167-957">**ABAP system**: **ASCS** instance number **00**</span></span>
  * <span data-ttu-id="6e167-958">**Java systému**: **SCS** instance číslo **01**</span><span class="sxs-lookup"><span data-stu-id="6e167-958">**Java system**: **SCS** instance number **01**</span></span>
  * <span data-ttu-id="6e167-959">**ABAP + Java systému**: **Asc** instance číslo **00** a **SCS** instance číslo **01**</span><span class="sxs-lookup"><span data-stu-id="6e167-959">**ABAP+Java system**: **ASCS** instance number **00** and **SCS** instance number **01**</span></span>

  <span data-ttu-id="6e167-960">Pokud chcete použít instanci čísla než 00 pro instanci ABAP ASC a 01 pro instanci Java SCS, nejdřív je potřeba změnit Azure interní výchozí Vyrovnávání zatížení pravidel, popsaných v [změňte pravidla pro vyrovnávání zatížení výchozí ASC nebo SCS Nástroje pro vyrovnávání zatížení Azure interní][sap-ha-guide-8.9].</span><span class="sxs-lookup"><span data-stu-id="6e167-960">To use instance numbers other than 00 for the ABAP ASCS instance and 01 for the Java SCS instance, first you need to change the Azure internal load balancer default load balancing rules, described in [Change the ASCS/SCS default load balancing rules for the Azure internal load balancer][sap-ha-guide-8.9].</span></span>

<span data-ttu-id="6e167-961">Další několik úloh nejsou popsané v dokumentaci standardní instalace SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-961">The next few tasks aren't described in the standard SAP installation documentation.</span></span>

> [!NOTE]
> <span data-ttu-id="6e167-962">Dokumentaci k instalaci SAP popisuje postup instalace prvního uzlu clusteru ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="6e167-962">The SAP installation documentation describes how to install the first ASCS/SCS cluster node.</span></span>
>
>

#### <span data-ttu-id="6e167-963"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Upravit profil SAP ASC nebo SCS instance</span><span class="sxs-lookup"><span data-stu-id="6e167-963"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> Modify the SAP profile of the ASCS/SCS instance</span></span>

<span data-ttu-id="6e167-964">Je nutné přidat nový parametr profilu.</span><span class="sxs-lookup"><span data-stu-id="6e167-964">You need to add a new profile parameter.</span></span> <span data-ttu-id="6e167-965">Parametr profil zabrání připojení mezi SAP pracovní procesy a serverem zařazování zavření po nečinnosti příliš dlouho.</span><span class="sxs-lookup"><span data-stu-id="6e167-965">The profile parameter prevents connections between SAP work processes and the enqueue server from closing when they are idle for too long.</span></span> <span data-ttu-id="6e167-966">Jsme uvedených scénář v [přidat položky registru na obou uzlů clusteru instance SAP ASC nebo SCS][sap-ha-guide-8.11].</span><span class="sxs-lookup"><span data-stu-id="6e167-966">We mentioned the problem scenario in [Add registry entries on both cluster nodes of the SAP ASCS/SCS instance][sap-ha-guide-8.11].</span></span> <span data-ttu-id="6e167-967">V této části zavedli jsme taky dvě změny některé základní parametry připojení TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="6e167-967">In that section, we also introduced two changes to some basic TCP/IP connection parameters.</span></span> <span data-ttu-id="6e167-968">V druhém kroku, je nutné nastavit zařadit server k odeslání `keep_alive` signál, aby připojení není dosáhl nástroje pro vyrovnávání zatížení Azure vnitřní limit nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="6e167-968">In a second step, you need to set the enqueue server to send a `keep_alive` signal so that the connections don't hit the Azure internal load balancer's idle threshold.</span></span>

<span data-ttu-id="6e167-969">Úprava profilu SAP instance ASC nebo SCS:</span><span class="sxs-lookup"><span data-stu-id="6e167-969">To modify the SAP profile of the ASCS/SCS instance:</span></span>

1.  <span data-ttu-id="6e167-970">Tento parametr profil přidáte do profilu instance SAP ASC nebo SCS:</span><span class="sxs-lookup"><span data-stu-id="6e167-970">Add this profile parameter to the SAP ASCS/SCS instance profile:</span></span>

  ```
  enque/encni/set_so_keepalive = true
  ```
  <span data-ttu-id="6e167-971">V našem příkladu je cesta:</span><span class="sxs-lookup"><span data-stu-id="6e167-971">In our example, the path is:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  <span data-ttu-id="6e167-972">Například pro SAP SCS instance profilu a odpovídajících cesta:</span><span class="sxs-lookup"><span data-stu-id="6e167-972">For example, to the SAP SCS instance profile and corresponding path:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  <span data-ttu-id="6e167-973">Aby se změny projevily, restartujte instanci /SCS ASC SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-973">To apply the changes, restart the SAP ASCS /SCS instance.</span></span>

#### <span data-ttu-id="6e167-974"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Přidat port testu</span><span class="sxs-lookup"><span data-stu-id="6e167-974"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Add a probe port</span></span>

<span data-ttu-id="6e167-975">Pomocí nástroje pro vyrovnávání zatížení interní test funkce usnadňují konfiguraci celý cluster pracovat s Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="6e167-975">Use the internal load balancer's probe functionality to make the entire cluster configuration work with Azure Load Balancer.</span></span> <span data-ttu-id="6e167-976">Nástroje pro vyrovnávání zatížení Azure interní obvykle distribuuje příchozí zatížení rovnoměrně mezi zúčastněných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e167-976">The Azure internal load balancer usually distributes the incoming workload equally between participating virtual machines.</span></span> <span data-ttu-id="6e167-977">Ale tato funkce nebude pracovat v některé konfigurace clusteru vzhledem k tomu, že pouze jedna instance je aktivní.</span><span class="sxs-lookup"><span data-stu-id="6e167-977">However, this won't work in some cluster configurations because only one instance is active.</span></span> <span data-ttu-id="6e167-978">Druhá instance je pasivní a nemůže přijímat úlohy.</span><span class="sxs-lookup"><span data-stu-id="6e167-978">The other instance is passive and can’t accept any of the workload.</span></span> <span data-ttu-id="6e167-979">Funkce kontroly pomáhá při nástroje pro vyrovnávání zatížení Azure interní přiřadí pracovní jenom na aktivní instance.</span><span class="sxs-lookup"><span data-stu-id="6e167-979">A probe functionality helps when the Azure internal load balancer assigns work only to an active instance.</span></span> <span data-ttu-id="6e167-980">Pomocí funkce testu nástroje pro vyrovnávání zatížení pro vnitřní může zjistit, které instancí jsou aktivní a pak cíle pouze instance se zatížením.</span><span class="sxs-lookup"><span data-stu-id="6e167-980">With the probe functionality, the internal load balancer can detect which instances are active, and then target only the instance with the workload.</span></span>

<span data-ttu-id="6e167-981">Chcete-li přidat port testu:</span><span class="sxs-lookup"><span data-stu-id="6e167-981">To add a probe port:</span></span>

1.  <span data-ttu-id="6e167-982">Zkontrolujte aktuální **ProbePort** nastavení spuštěním následujícího příkazu Powershellu.</span><span class="sxs-lookup"><span data-stu-id="6e167-982">Check the current **ProbePort** setting by running the following PowerShell command.</span></span> <span data-ttu-id="6e167-983">Spouštění ho do jedné z virtuálních počítačů v konfiguraci clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-983">Execute it from within one of the virtual machines in the cluster configuration.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  <span data-ttu-id="6e167-984">Definujte port testu.</span><span class="sxs-lookup"><span data-stu-id="6e167-984">Define a probe port.</span></span> <span data-ttu-id="6e167-985">Výchozí číslo portu testu je **0**.</span><span class="sxs-lookup"><span data-stu-id="6e167-985">The default probe port number is **0**.</span></span> <span data-ttu-id="6e167-986">V našem příkladu používáme port testu **62000**.</span><span class="sxs-lookup"><span data-stu-id="6e167-986">In our example, we use probe port **62000**.</span></span>

  ![Obrázek 58: Port testu konfigurace clusteru je 0. ve výchozím nastavení][sap-ha-guide-figure-3048]

  <span data-ttu-id="6e167-988">_**Obrázek 58:** výchozí port test konfigurace clusteru je 0._</span><span class="sxs-lookup"><span data-stu-id="6e167-988">_**Figure 58:** The default cluster configuration probe port is 0_</span></span>

  <span data-ttu-id="6e167-989">Číslo portu je definována v šablonách SAP Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6e167-989">The port number is defined in SAP Azure Resource Manager templates.</span></span> <span data-ttu-id="6e167-990">Můžete přiřadit číslo portu v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6e167-990">You can assign the port number in PowerShell.</span></span>

  <span data-ttu-id="6e167-991">Chcete-li nastavit novou hodnotu ProbePort  **SAP <*SID*> IP ** prostředek clusteru, spusťte následující skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6e167-991">To set a new ProbePort value for the **SAP <*SID*> IP** cluster resource, run the following PowerShell script.</span></span> <span data-ttu-id="6e167-992">Aktualizujte proměnné prostředí PowerShell pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="6e167-992">Update the PowerShell variables for your environment.</span></span> <span data-ttu-id="6e167-993">Po spuštění skriptu budete vyzváni, restartujte skupina clusteru SAP k aktivaci změny.</span><span class="sxs-lookup"><span data-stu-id="6e167-993">After the script runs, you'll be prompted to restart the SAP cluster group to activate the changes.</span></span>

  ```PowerShell
  $SAPSID = "PR1"      # SAP <SID>
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  Clear-Host
  $SAPClusterRoleName = "SAP $SAPSID"
  $SAPIPresourceName = "SAP $SAPSID IP"
  $SAPIPResourceClusterParameters =  Get-ClusterResource $SAPIPresourceName | Get-ClusterParameter
  $IPAddress = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Address" }).Value
  $NetworkName = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Network" }).Value
  $SubnetMask = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "SubnetMask" }).Value
  $OverrideAddressMatch = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "OverrideAddressMatch" }).Value
  $EnableDhcp = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "EnableDhcp" }).Value
  $OldProbePort = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "ProbePort" }).Value

  $var = Get-ClusterResource | Where-Object {  $_.name -eq $SAPIPresourceName  }

  Write-Host "Current configuration parameters for SAP IP cluster resource '$SAPIPresourceName' are:" -ForegroundColor Cyan
  Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter

  Write-Host
  Write-Host "Current probe port property of the SAP cluster resource '$SAPIPresourceName' is '$OldProbePort'." -ForegroundColor Cyan
  Write-Host
  Write-Host "Setting the new probe port property of the SAP cluster resource '$SAPIPresourceName' to '$ProbePort' ..." -ForegroundColor Cyan
  Write-Host

  $var | Set-ClusterParameter -Multiple @{"Address"=$IPAddress;"ProbePort"=$ProbePort;"Subnetmask"=$SubnetMask;"Network"=$NetworkName;"OverrideAddressMatch"=$OverrideAddressMatch;"EnableDhcp"=$EnableDhcp}

  Write-Host

  $ActivateChanges = Read-Host "Do you want to take restart SAP cluster role '$SAPClusterRoleName', to activate the changes (yes/no)?"

  if($ActivateChanges -eq "yes"){
  Write-Host
  Write-Host "Activating changes..." -ForegroundColor Cyan

  Write-Host
  write-host "Taking SAP cluster IP resource '$SAPIPresourceName' offline ..." -ForegroundColor Cyan
  Stop-ClusterResource -Name $SAPIPresourceName
  sleep 5

  Write-Host "Starting SAP cluster role '$SAPClusterRoleName' ..." -ForegroundColor Cyan
  Start-ClusterGroup -Name $SAPClusterRoleName

  Write-Host "New ProbePort parameter is active." -ForegroundColor Green
  Write-Host

  Write-Host "New configuration parameters for SAP IP cluster resource '$SAPIPresourceName':" -ForegroundColor Cyan
  Write-Host
  Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter
  }else
  {
  Write-Host "Changes are not activated."
  }
  ```

  <span data-ttu-id="6e167-994">Po přepnutí  **SAP <*SID*> ** clusteru role online, ověřte, že **ProbePort** nastavena na novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6e167-994">After you bring the **SAP <*SID*>** cluster role online, verify that **ProbePort** is set to the new value.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![Obrázek 59: Probe port clusteru po nastavení nová hodnota][sap-ha-guide-figure-3049]

  <span data-ttu-id="6e167-996">_**Obrázek 59:** testu port clusteru po nastavení nová hodnota_</span><span class="sxs-lookup"><span data-stu-id="6e167-996">_**Figure 59:** Probe the cluster port after you set the new value_</span></span>

#### <span data-ttu-id="6e167-997"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Otevřete port testu brány firewall systému Windows</span><span class="sxs-lookup"><span data-stu-id="6e167-997"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a> Open the Windows firewall probe port</span></span>

<span data-ttu-id="6e167-998">Budete muset otevřít port testu brány firewall systému Windows na obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-998">You need to open a Windows firewall probe port on both cluster nodes.</span></span> <span data-ttu-id="6e167-999">Pomocí následujícího skriptu otevřít port testu brány firewall systému Windows.</span><span class="sxs-lookup"><span data-stu-id="6e167-999">Use the following script to open a Windows firewall probe port.</span></span> <span data-ttu-id="6e167-1000">Aktualizujte proměnné prostředí PowerShell pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="6e167-1000">Update the PowerShell variables for your environment.</span></span>

  ```PowerShell
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

<span data-ttu-id="6e167-1001">**ProbePort** je nastaven na **62000**.</span><span class="sxs-lookup"><span data-stu-id="6e167-1001">The **ProbePort** is set to **62000**.</span></span> <span data-ttu-id="6e167-1002">Teď můžete přístup ke sdílené složce  **\\\ascsha-clsap\sapmnt** z jiných hostitelů, například jako z **ascsha dbas**.</span><span class="sxs-lookup"><span data-stu-id="6e167-1002">Now you can access the file share **\\\ascsha-clsap\sapmnt** from other hosts, such as from **ascsha-dbas**.</span></span>

### <span data-ttu-id="6e167-1003"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Instalovat instanci databáze</span><span class="sxs-lookup"><span data-stu-id="6e167-1003"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Install the database instance</span></span>

<span data-ttu-id="6e167-1004">Chcete-li nainstalovat instanci databáze, postupujte podle procesu popsaného v dokumentaci k instalaci SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-1004">To install the database instance, follow the process described in the SAP installation documentation.</span></span>

### <span data-ttu-id="6e167-1005"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Instalaci druhého uzlu clusteru</span><span class="sxs-lookup"><span data-stu-id="6e167-1005"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> Install the second cluster node</span></span>

<span data-ttu-id="6e167-1006">K instalaci druhé clusteru, postupujte podle kroků v Průvodci instalací SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-1006">To install the second cluster, follow the steps in the SAP installation guide.</span></span>

### <span data-ttu-id="6e167-1007"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Změnit typ spuštění instance služby Windows YBRAT SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-1007"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> Change the start type of the SAP ERS Windows service instance</span></span>

<span data-ttu-id="6e167-1008">Změnit typ spuštění služby Windows YBRAT SAP **automaticky (zpožděné spuštění)** na obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e167-1008">Change the start type of the SAP ERS Windows service to **Automatic (Delayed Start)** on both cluster nodes.</span></span>

![Obrázek 60: Změňte typ služby pro instance SAP YBRAT na zpožděné automaticky][sap-ha-guide-figure-3050]

<span data-ttu-id="6e167-1010">_**Obrázek 60:** změnit typ služby pro instance SAP YBRAT pro odložené automatické_</span><span class="sxs-lookup"><span data-stu-id="6e167-1010">_**Figure 60:** Change the service type for the SAP ERS instance to delayed automatic_</span></span>

### <span data-ttu-id="6e167-1011"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>Instalace serveru primární aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="6e167-1011"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> Install the SAP Primary Application Server</span></span>

<span data-ttu-id="6e167-1012">Nainstalovat instanci serveru primární aplikace (Pa) <*SID*> - di - 0 na virtuálním počítači, který jste určili pro hostování Pa ADRESAMI.</span><span class="sxs-lookup"><span data-stu-id="6e167-1012">Install the Primary Application Server (PAS) instance <*SID*>-di-0 on the virtual machine that you've designated to host the PAS.</span></span> <span data-ttu-id="6e167-1013">Neexistují žádné závislosti na Azure nebo DataKeeper specifická nastavení.</span><span class="sxs-lookup"><span data-stu-id="6e167-1013">There are no dependencies on Azure or DataKeeper-specific settings.</span></span>

### <span data-ttu-id="6e167-1014"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>Instalace serveru SAP další aplikace</span><span class="sxs-lookup"><span data-stu-id="6e167-1014"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> Install the SAP Additional Application Server</span></span>

<span data-ttu-id="6e167-1015">Nainstalujte SAP serveru pro další aplikace (AAS) na všechny virtuální počítače, které jste označili k hostování instance aplikačního serveru SAP.</span><span class="sxs-lookup"><span data-stu-id="6e167-1015">Install an SAP Additional Application Server (AAS) on all the virtual machines that you've designated to host an SAP Application Server instance.</span></span> <span data-ttu-id="6e167-1016">Například na <*SID*> - di - 1 pro <*SID*> - di -&lt;n&gt;.</span><span class="sxs-lookup"><span data-stu-id="6e167-1016">For example, on <*SID*>-di-1 to <*SID*>-di-&lt;n&gt;.</span></span>

> [!NOTE]
> <span data-ttu-id="6e167-1017">To dokončí instalaci systému SAP NetWeaver vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="6e167-1017">This finishes the installation of a high-availability SAP NetWeaver system.</span></span> <span data-ttu-id="6e167-1018">Pokračujte dále testování převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6e167-1018">Next, proceed with failover testing.</span></span>
>


## <span data-ttu-id="6e167-1019"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Testovací převzetí služeb při selhání SAP ASC nebo SCS instance a SIOS replikace</span><span class="sxs-lookup"><span data-stu-id="6e167-1019"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> Test the SAP ASCS/SCS instance failover and SIOS replication</span></span>
<span data-ttu-id="6e167-1020">Je snadné pro testování a monitorování SAP ASC nebo SCS instance převzetí služeb při selhání a replikaci disku SIOS pomocí nástroje Správce clusteru převzetí služeb při selhání a s DataKeeper správy a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6e167-1020">It's easy to test and monitor an SAP ASCS/SCS instance failover and SIOS disk replication by using Failover Cluster Manager and the SIOS DataKeeper Management and Configuration tool.</span></span>

### <span data-ttu-id="6e167-1021"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>V uzlu clusteru A je spuštěna instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="6e167-1021"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> SAP ASCS/SCS instance is running on cluster node A</span></span>

<span data-ttu-id="6e167-1022">**SAP PR1** skupina clusteru běží na uzlu clusteru A. Například na **pr1-ASC-0**.</span><span class="sxs-lookup"><span data-stu-id="6e167-1022">The **SAP PR1** cluster group is running on cluster node A. For example, on **pr1-ascs-0**.</span></span> <span data-ttu-id="6e167-1023">Přiřadit sdílené diskové jednotce S, která je součástí systému **SAP PR1** skupina clusteru a který instance ASC nebo SCS používá A. uzlu v clusteru</span><span class="sxs-lookup"><span data-stu-id="6e167-1023">Assign the shared disk drive S, which is part of the **SAP PR1** cluster group, and which the ASCS/SCS instance uses, to cluster node A.</span></span>

![Obrázek 61: Správce clusteru převzetí služeb při selhání: Skupina clusteru SAP < SID > běží na uzlu clusteru A][sap-ha-guide-figure-5000]

<span data-ttu-id="6e167-1025">_**Obrázek 61:** Správce clusteru převzetí služeb při selhání: SAP <*SID*> Skupina clusteru běží na uzlu clusteru A_</span><span class="sxs-lookup"><span data-stu-id="6e167-1025">_**Figure 61:** Failover Cluster Manager: The SAP <*SID*> cluster group is running on cluster node A_</span></span>

<span data-ttu-id="6e167-1026">Nástroje pro správu s DataKeeper a konfiguraci uvidíte, že dat sdíleného disku je synchronně replikovat ze zdrojového svazku jednotky S na uzlu clusteru A jednotky cílový svazek S na uzlu clusteru B. Například se replikují z **pr1-ASC-0 [10.0.0.40]** k **pr1-ASC-1 [10.0.0.41]**.</span><span class="sxs-lookup"><span data-stu-id="6e167-1026">In the SIOS DataKeeper Management and Configuration tool, you can see that the shared disk data is synchronously replicated from the source volume drive S on cluster node A to the target volume drive S on cluster node B. For example, it's replicated from **pr1-ascs-0 [10.0.0.40]** to **pr1-ascs-1 [10.0.0.41]**.</span></span>

![Obrázek 62: V DataKeeper s replikovat místní svazek z uzlu clusteru A k uzlu clusteru B][sap-ha-guide-figure-5001]

<span data-ttu-id="6e167-1028">_**Obrázek 62:** v DataKeeper s replikovat místní svazek z uzlu clusteru A k uzlu clusteru B_</span><span class="sxs-lookup"><span data-stu-id="6e167-1028">_**Figure 62:** In SIOS DataKeeper, replicate the local volume from cluster node A to cluster node B_</span></span>

### <span data-ttu-id="6e167-1029"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Převzetí služeb při selhání z uzlu A k uzlu B</span><span class="sxs-lookup"><span data-stu-id="6e167-1029"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Failover from node A to node B</span></span>

1.  <span data-ttu-id="6e167-1030">Vyberte jednu z těchto možností k zahájení převzetí služeb při selhání SAP <*SID*> Skupina clusteru z uzlu clusteru A k uzlu clusteru B:</span><span class="sxs-lookup"><span data-stu-id="6e167-1030">Choose one of these options to initiate a failover of the SAP <*SID*> cluster group from cluster node A to cluster node B:</span></span>
  - <span data-ttu-id="6e167-1031">Pomocí Správce clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="6e167-1031">Use Failover Cluster Manager</span></span>  
  - <span data-ttu-id="6e167-1032">Pomocí prostředí PowerShell pro Cluster převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="6e167-1032">Use Failover Cluster PowerShell</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  <span data-ttu-id="6e167-1033">Restartovat A uzlu clusteru v rámci hostovaného operačního systému Windows (tím se vyvolá automatické převzetí služeb při selhání z SAP <*SID*> Skupina clusteru z uzlu A k uzlu B).</span><span class="sxs-lookup"><span data-stu-id="6e167-1033">Restart cluster node A within the Windows guest operating system (this initiates an automatic failover of the SAP <*SID*> cluster group from node A to node B).</span></span>  
3.  <span data-ttu-id="6e167-1034">Restartovat uzel A cluster z portálu Azure (tím se vyvolá automatické převzetí služeb při selhání z SAP <*SID*> Skupina clusteru z uzlu A k uzlu B).</span><span class="sxs-lookup"><span data-stu-id="6e167-1034">Restart cluster node A from the Azure portal (this initiates an automatic failover of the SAP <*SID*> cluster group from node A to node B).</span></span>  
4.  <span data-ttu-id="6e167-1035">Restartujte počítač A uzlu clusteru pomocí prostředí Azure PowerShell (tím se vyvolá automatické převzetí služeb při selhání z SAP <*SID*> Skupina clusteru z uzlu A k uzlu B).</span><span class="sxs-lookup"><span data-stu-id="6e167-1035">Restart cluster node A by using Azure PowerShell (this initiates an automatic failover of the SAP <*SID*> cluster group from node A to node B).</span></span>

  <span data-ttu-id="6e167-1036">Po převzetí služeb při selhání, SAP <*SID*> Skupina clusteru běží na uzlu clusteru B. Například je spuštěn na **pr1-ASC-1**.</span><span class="sxs-lookup"><span data-stu-id="6e167-1036">After failover, the SAP <*SID*> cluster group is running on cluster node B. For example, it's running on **pr1-ascs-1**.</span></span>

  ![Obrázek 63: Ve Správci clusteru převzetí služeb při selhání skupiny clusteru SAP < SID > běží na uzlu clusteru B][sap-ha-guide-figure-5002]

  <span data-ttu-id="6e167-1038">_**Obrázek 63**: ve Správci clusteru převzetí služeb při selhání, SAP <*SID*> Skupina clusteru běží na uzlu clusteru B_</span><span class="sxs-lookup"><span data-stu-id="6e167-1038">_**Figure 63**: In Failover Cluster Manager, the SAP <*SID*> cluster group is running on cluster node B_</span></span>

  <span data-ttu-id="6e167-1039">Sdílený disk je teď připojené v clusteru uzlu B. s DataKeeper se replikuje data ze zdrojového svazku jednotky S na uzlu clusteru B na cílový svazek jednotky S na uzlu clusteru A. Například je replikace z **pr1-ASC-1 [10.0.0.41]** k **pr1-ASC-0 [10.0.0.40]**.</span><span class="sxs-lookup"><span data-stu-id="6e167-1039">The shared disk is now mounted on cluster node B. SIOS DataKeeper is replicating data from source volume drive S on cluster node B to target volume drive S on cluster node A. For example, it's replicating from **pr1-ascs-1 [10.0.0.41]** to **pr1-ascs-0 [10.0.0.40]**.</span></span>

  ![Obrázek 64: SIOS DataKeeper z uzlu clusteru B A uzlu v clusteru se replikuje místního svazku][sap-ha-guide-figure-5003]

  <span data-ttu-id="6e167-1041">_**Obrázek 64:** s DataKeeper replikuje místní svazek z uzlu clusteru B A uzlu v clusteru_</span><span class="sxs-lookup"><span data-stu-id="6e167-1041">_**Figure 64:** SIOS DataKeeper replicates the local volume from cluster node B to cluster node A_</span></span>
