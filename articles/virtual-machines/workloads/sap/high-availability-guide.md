---
title: "aaaAzure vysoké dostupnosti virtuálních počítačů pro SAP NetWeaver | Microsoft Docs"
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
ms.openlocfilehash: 927409830065573248a43427eb382448ca07b6e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms"></a>Vysoká dostupnost pro SAP NetWeaver na virtuálních počítačích Azure

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


Virtuální počítače Azure je hello řešení pro organizace, které potřebují výpočty, úložiště a síťové prostředky ve minimálního čas a bez zdlouhavé nákup cykly. Můžete vytvořit virtuální počítače Azure toodeploy classic aplikace jako na základě SAP NetWeaver ABAP, Java a ABAP + Java zásobníku. Rozšiřte spolehlivosti a dostupnosti bez dalších místních prostředků. Virtuální počítače Azure podporuje připojení mezi různými místy, takže virtuální počítače Azure můžete integrovat do místní domény vaší organizace, privátních cloudů a šířku systému SAP.

V tomto článku jsme zahrnují hello kroky můžete podniknout toodeploy SAP systémů s vysokou dostupností v Azure pomocí modelu nasazení Azure Resource Manager hello. Můžeme vás provede procesem tyto hlavní úlohy:

* Najde hello správné příručky SAP poznámky a instalace, uvedené v hello [prostředky] [ sap-ha-guide-2] části. Tento článek doplňuje SAP instalace dokumentace a poznámky k SAP, které jsou hello primární prostředky, které vám mohou pomoci instalace a nasazení SAP software na specifické platformy.
* Přečtěte si hello rozdíly mezi hello modelu nasazení Azure Resource Manager a modelu nasazení Azure classic hello.
* Další informace o Windows Server Failover Clustering režimů kvora, takže můžete vybrat hello model, který je nejvhodnější pro vaše nasazení Azure.
* Další informace o Windows Server Failover Clustering sdíleného úložiště do služby Azure.
* Informace o ochraně toohelp jeden bod o selhání součásti jako Advanced obchodní aplikace programování (ABAP) SAP centrální služby (ASC) / SAP centrální služby (SCS) a databázové systémy (databázového systému) a redundantní komponenty, například SAP Aplikační Server v Azure.
* Postupujte podle podrobný příklad k instalaci a konfiguraci systému SAP vysoké dostupnosti v clusteru Windows Server Failover Clustering v Azure pomocí Azure Resource Manager.
* Další informace o další kroky požadované toouse Windows Server Failover Clustering v Azure, ale které nejsou potřebné v místním nasazení.

toosimplify nasazení a konfigurace, v tomto článku používáme hello SAP třívrstvá vysoké dostupnosti správce prostředků šablony. šablony Hello automatizovat nasazení hello celé infrastruktury, které potřebujete pro SAP systém vysoké dostupnosti. Infrastruktura Hello také podporuje nastavení velikosti SAP aplikace výkonu standardní (protokoly SAP) systému SAP.

## <a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Požadavky
Než začnete, ujistěte se, že splňujete požadavky hello, které jsou popsány v následující části hello. Být také, zda toocheck všechny prostředky uvedené v hello [prostředky] [ sap-ha-guide-2] části.

V tomto článku používáme šablon Azure Resource Manageru pro [třívrstvá SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/). Užitečné Přehled šablon naleznete v tématu [šablon SAP Azure Resource Manageru](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).

## <a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Prostředky
Tyto články popisuje nasazení SAP v Azure:

* [Azure virtuálních počítačů, plánování a implementace pro SAP NetWeaver][planning-guide]
* [Nasazení virtuálních počítačů Azure pro SAP NetWeaver][deployment-guide]
* [Nasazení virtuálních počítačů databázového systému Azure pro SAP NetWeaver][dbms-guide]
* [Azure vysoké dostupnosti virtuálních počítačů pro SAP NetWeaver (Tato příručka)][sap-ha-guide]

> [!NOTE]
> Kdykoli je to možné, jsme získáte na odkazu toohello odkazující Instalační příručka (viz hello [SAP instalace příručky][sap-installation-guides]). Požadavky a informace o procesu instalace hello, jeho, které provede instalaci s vhodné hello tooread SAP NetWeaver pečlivě. Tento článek se týká pouze konkrétní úlohy pro počítače se systémem SAP NetWeaver, které můžete použít s virtuálními počítači Azure.
>
>

Tyto poznámky SAP jsou související toohello tématem SAP v Azure:

| Poznámka: číslo | Název |
| --- | --- |
| [1928533] |Aplikace SAP v Azure: podporované produkty a velikosti |
| [2015553] |SAP na platformě Microsoft Azure: podporovat požadavky |
| [1999351] |Rozšířené monitorování Azure pro SAP |
| [2178632] |Klíč monitorování metriky pro SAP na platformě Microsoft Azure |
| [1999351] |Virtualizace v systému Windows: rozšířené monitorování |
| [2243692] |Používání úložiště Azure Premium SSD pro instanci databázového systému SAP |

Další informace o hello [omezení předplatná Azure][azure-subscription-service-limits-subscription], včetně obecné výchozí omezení a maximální omezení.

## <a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Vysoká dostupnost SAP s Azure Resource Manager a modelu nasazení Azure classic hello
Hello Azure Resource Manager a modelech nasazení Azure classic se liší v hello následující oblasti:

- Skupiny prostředků
- Azure interní načíst vyrovnávání závislost na skupinu prostředků Azure hello
- Podpora pro scénáře více SID SAP

### <a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Skupiny prostředků
V Azure Resource Manager, můžete použít toomanage skupiny prostředků všechny prostředky aplikace hello ve vašem předplatném Azure. Integrovaný přístup, ve skupině prostředků, všechny prostředky mají hello stejný životní cyklus. Například všechny prostředky jsou vytvořené na hello stejný čas a že jsou při hello odstraněny stejnou dobu. Další informace o [skupinách prostředků](../../../azure-resource-manager/resource-group-overview.md#resource-groups).

### <a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Azure interní načíst vyrovnávání závislost na skupinu prostředků Azure hello

V modelu nasazení Azure classic hello je závislost mezi hello Azure interní nástroj pro vyrovnávání zatížení (služba Vyrovnávání zatížení Azure) a skupinou služeb cloudu hello. Každý nástroj pro vyrovnávání zatížení interní vyžaduje jednu skupinu cloudové služby.

V Azure Resource Manager, nepotřebujete toouse skupiny prostředků Azure pro vyrovnávání zatížení Azure. Hello prostředí je jednodušší a flexibilnější.

### <a name="support-for-sap-multi-sid-scenarios"></a>Podpora pro scénáře více SID SAP

V Azure Resource Manager, můžete nainstalovat víc SAP systému identifikátor (SID) ASC nebo SCS instancí v jednom clusteru. SID více instancí je možné, protože obsahuje podporu pro více IP adres pro každý nástroj pro vyrovnávání zatížení Azure interní.

toouse hello modelu nasazení Azure classic, postupujte podle hello postupů popsaných v [SAP NetWeaver v Azure: instancí clusteringu ASC nebo SCS SAP pomocí služby Windows Server Failover Clustering v Azure pomocí s DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).

> [!IMPORTANT]
> Důrazně doporučujeme, abyste používali hello modelu nasazení Azure Resource Manager pro SAP instalací. Nabízí spoustu výhod, které nejsou k dispozici v modelu nasazení classic hello. Další informace o Azure [modely nasazení][virtual-machines-azure-resource-manager-architecture-benefits-arm].   
>
>

## <a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server Failover Clustering
Windows Server Failover Clustering je hello foundation instalace SAP ASC nebo SCS vysokou dostupnost a databázového systému v systému Windows.

Cluster s podporou převzetí služeb při selhání je skupina 1 + n nezávislých serverů (uzlů) které tooincrease hello dostupnosti aplikací a služeb vzájemně spolupracují. Pokud dojde k selhání uzlu, Windows Server Failover Clustering vypočítá hello počet chyb, které můžou nastat při zachování pořádku clusteru tooprovide aplikací a služeb. Můžete se z různých kvora režimy tooachieve převzetí služeb clusteringu.

### <a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Režim kvora
Při použití služby Windows Server Failover Clustering, můžete vybrat ze čtyř režimů kvora:

* **Většina uzlů**. Každý uzel clusteru hello hlasovat. Hello clusteru funguje jenom s většinou hlasů, který je s víc než poloviny hello hlasy. Doporučujeme tuto možnost pro clustery, které mají lichý počet uzlů. Například může selhat tři uzly v clusteru s podporou sedmi a hello clusteru nepřesahuje dosahuje většině a pokračuje toorun.  
* **Většina uzlů a disků**. Každý uzel a určený disk (disk s kopií clusteru) v úložišti clusteru hello hlasovat když jsou k dispozici a komunikace. Hello clusteru funguje jenom s většinou hello hlasů, tedy s víc než poloviny hello hlasy. Tento režim má smysl v prostředí clusteru s sudé číslo uzlů. Pokud poloviční hello uzlů a disků hello jsou online, hello cluster zůstane v dobrém stavu.
* **Uzel a sdílených složek většina**. Každý uzel, plus určené sdílené složky (soubor určující sdílenou složku), který hello správce vytvoří hlasovat, bez ohledu na to, jestli jsou k dispozici hello uzlů a sdílení souborů a v komunikaci. Hello clusteru funguje jenom s většinou hello hlasů, tedy s víc než poloviny hello hlasy. Tento režim má smysl v prostředí clusteru s sudé číslo uzlů. Je podobné toohello uzlu a většina disku režim, ale používá určující sdílené složky namísto disku s kopií clusteru. Tento režim je snadno tooimplement, ale pokud sdílená složka hello samotné není vysoce dostupný, může být jediný bod selhání.
* **Bez většiny: Na disku jenom**. Hello cluster má kvorum, pokud je k dispozici a komunikace s konkrétním diskem v úložišti clusteru hello jeden uzel. Pouze hello uzlů, které jsou také v komunikaci s tento disk může připojit hello cluster. Doporučujeme vám, nepoužívejte tento režim.
 

## <a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server převzetí služeb při selhání Clustering na místě
Obrázek 1 ukazuje dva uzly clusteru. Pokud hello síťové připojení mezi uzly hello selže a oba uzly zůstat nahoru a spuštěna, kvora disku nebo sdílené složky Určuje, který uzel bude pokračovat, aplikacím a službám tooprovide hello clusteru. Hello uzlu, který má přístup k disku kvora toohello nebo sdílených složek je hello uzlu, který zajistí, že služby pokračovat.

Protože tento příklad používá dva uzly clusteru, použijeme hello uzlu a režim kvora Majority sdílené složky souborů. Většina disku a Hello uzlu je taky platná možnost. V produkčním prostředí doporučujeme použít disk kvora. Můžete použít sítě a úložiště toomake technologie systému je vysoce dostupný.

![Obrázek 1: Příklad konfigurace služby systému Windows Server Failover Clustering pro SAP ASC nebo SCS v Azure][sap-ha-guide-figure-1000]

_**Obrázek 1:** příklad konfigurace služby systému Windows Server Failover Clustering pro SAP ASC nebo SCS v Azure_

### <a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Sdílené úložiště
Obrázek 1 zobrazuje i sdílené úložiště dvěma uzly clusteru. V clusteru služby místní sdílené úložiště rozpoznat všechny uzly v clusteru hello sdílené úložiště. Mechanismus uzamčení chrání hello data před poškozením. Všechny uzly, můžete zjistit, pokud jiný uzel selže. Pokud selže jeden uzel, zbývající uzel hello vlastníkem hello prostředků úložiště a zajišťuje hello dostupnost služeb.

> [!NOTE]
> Nepotřebujete sdílené disky pro zajištění vysoké dostupnosti s některými aplikacemi databázového systému, třeba s SQL serverem. SQL serveru Always On replikuje databázového systému souborů protokolu a data z místního disku hello jeden cluster uzlu toohello místní disk jiným uzlem clusteru. V takovém případě konfigurace clusteru Windows hello nepotřebuje sdíleného disku.
>
>

### <a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Sítě a překladu
Klientské počítače přístup hello clusteru přes virtuální IP adresu a název virtuálního hostitele tohoto hello, které poskytuje DNS server. Hello místní uzly a hello DNS server může zpracovat více IP adres.

V typické instalace můžete používat dva nebo více připojení k síti:

* Vyhrazené připojení toohello úložiště
* Cluster interní síťové připojení pro prezenčního signálu hello
* Veřejné síti, které využívají klienti tooconnect toohello clusteru

## <a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Server Failover Clustering v Azure
Porovnání toobare systému nebo privátního cloudu nasazení virtuálních počítačů Azure vyžaduje další kroky tooconfigure Windows Server Failover Clustering. Při sestavování sdíleném disku clusteru, musíte pro instance SAP ASC nebo SCS hello tooset několik IP adres a virtuální hostitel názvy.

V tomto článku jsme popisují klíčové koncepty a hello další kroky požadované toobuild clusteru služby SAP centrální služby vysoké dostupnosti v Azure. Můžeme vám ukážou, jak tooset až nástroj třetí strany hello s DataKeeper a jak tooconfigure hello Azure interní nástroj pro vyrovnávání zatížení. Tyto nástroje toocreate clusteru s podporou převzetí služeb při selhání systému Windows můžete použít s určující sdílené složce v Azure.

![Obrázek 2: Windows Server Failover Clustering konfiguraci v Azure bez sdíleného disku][sap-ha-guide-figure-1001]

_**Obrázek 2:** konfiguraci služby Windows Server Failover Clustering v Azure bez sdíleného disku_

### <a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>Sdílený disk v Azure s DataKeeper s
Třeba clusteru sdíleného úložiště pro instanci SAP ASC nebo SCS vysokou dostupnost. Jako září 2016 Azure nenabízí sdílené úložiště, můžete použít toocreate clusteru sdíleného úložiště. Pomocí softwaru třetí strany s DataKeeper Cluster Edition toocreate zrcadlené úložiště, která simuluje sdílené úložiště clusteru. Hello SIOS řešení poskytuje data v reálném čase synchronní replikace. Toto je, jak můžete vytvořit prostředek sdíleného disku pro cluster:

1. Připojte další Azure virtuální pevný disk (VHD) tooeach hello virtuálních počítačů (VM) v konfiguraci clusteru systému Windows.
2. V obou uzlech virtuální počítač spusťte s DataKeeper Cluster Edition.
3. Nakonfigurujte s DataKeeper Cluster Edition, aby ho zrcadlí hello obsah hello další virtuální pevný disk připojený svazek z hello zdrojový virtuální počítač toohello další virtuální pevný disk připojený svazek hello cílového virtuálního počítače. S DataKeeper abstrahuje hello zdrojové a cílové místní svazky a uvede je tooWindows Server Clustering převzetí služeb při selhání jako jeden sdílený disk.

Přečtěte si další informace o [s DataKeeper](http://us.sios.com/products/datakeeper-cluster/).

![Obrázek 3: Konfigurace služby systému Windows Server Failover Clustering v Azure pomocí DataKeeper s][sap-ha-guide-figure-1002]

_**Obrázek 3:** konfigurace služby systému Windows Server Failover Clustering v Azure pomocí DataKeeper s_

> [!NOTE]
> Pro zajištění vysoké dostupnosti se některé produkty, databázového systému, jako je SQL Server není třeba sdílené disky. SQL serveru Always On replikuje databázového systému souborů protokolu a data z místního disku hello jeden cluster uzlu toohello místní disk jiným uzlem clusteru. V takovém případě konfigurace clusteru Windows hello nepotřebuje sdíleného disku.
>
>

### <a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>Rozpoznání názvu v Azure
Hello Azure Cloudová platforma nenabízí hello možnost tooconfigure virtuální IP adresy, například plovoucí IP adresy. Budete potřebovat tooset alternativní řešení se virtuální IP adresy tooreach hello prostředku clusteru v cloudu hello.
Azure má interní nástroj v hello služby Vyrovnávání zatížení Azure. Pomocí nástroje pro vyrovnávání zatížení pro vnitřní hello klienti moci hello clusteru připojit přes virtuální IP adresu clusteru hello.
Je nutné toodeploy hello interní vyrovnávání zátěže ve skupině prostředků hello, který obsahuje hello uzly clusteru. Potom nakonfigurujte všechny nezbytné port předávání pravidla s hello testu porty nástroje pro vyrovnávání zatížení pro vnitřní hello.
Hello klienti mohou připojit prostřednictvím hello název virtuálního hostitele. Hello DNS server přeloží IP adresu clusteru hello a hello interní služby load vyrovnávání popisovače port předávání toohello aktivní uzel clusteru hello.

## <a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>SAP NetWeaver vysoké dostupnosti v Azure infrastruktury jako služba (IaaS)
tooachieve SAP vysokou dostupnost aplikace, například pro SAP softwarové komponenty, je třeba tooprotect hello následující součásti:

* Instance aplikačního serveru SAP
* Instance SAP ASC nebo SCS
* Server databázového systému

Další informace o ochraně SAP součásti ve scénářích s vysokou dostupností najdete v tématu [virtuální počítače Azure plánování a implementace pro SAP NetWeaver](planning-guide.md).

### <a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Vysoká dostupnost SAP aplikační Server
Obvykle nepotřebujete specifického řešení vysoké dostupnosti pro instance aplikačního serveru SAP a dialogové okno hello. Dosažení vysoké dostupnosti pomocí redundance a nakonfigurujete více instancí dialogové okno v různých instancí virtuálních počítačů Azure. Měli byste mít aspoň dvě instance SAP aplikace nainstalované v dvě instance virtuálních počítačů Azure.

![Obrázek 4: Vysoké dostupnosti SAP aplikační Server][sap-ha-guide-figure-2000]

_**Obrázek 4:** vysokou dostupnost SAP aplikační Server_

Je nutné umístit všechny virtuální počítače, že hello instance aplikačního serveru SAP hostitele ve stejné skupině dostupnosti Azure. Nastavení Azure dostupnosti zajišťuje, že:

* Všechny virtuální počítače jsou součástí hello stejné domény upgradu. Upgradovací doméně, například zajišťuje, že hello virtuální počítače se neaktualizují v hello současně během plánované údržby. výpadků.
* Všechny virtuální počítače jsou součástí hello stejné domény selhání. Domény selhání, například zajišťuje nasazení virtuálních počítačů, tak, aby žádný jediný bod selhání ovlivňuje hello dostupnosti všech virtuálních počítačů.

Další informace o příliš[spravovat hello dostupnosti virtuálních počítačů][virtual-machines-manage-availability].

Hello účtu úložiště Azure je potenciální jediný bod selhání, a proto je důležité toohave alespoň dva účty úložiště Azure, ve kterých jsou distribuovány aspoň dva virtuální počítače. V ideální instalační program by se nasadí hello disky každého virtuálního počítače, na kterém běží instance SAP dialogové okno jiný účet úložiště.

### <a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Instance SAP ASC nebo SCS vysokou dostupnost
Obrázek 5 je příklad instance SAP ASC nebo SCS vysokou dostupnost.

![Obrázek 5: Vysoká dostupnost SAP ASC nebo SCS instance][sap-ha-guide-figure-2001]

_**Obrázek 5:** vysokou dostupnost SAP ASC nebo SCS instance_

#### <a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>SAP ASC nebo SCS instance vysoká dostupnost s Windows Server Failover Clustering v Azure
Porovnání toobare systému nebo privátního cloudu nasazení virtuálních počítačů Azure vyžaduje další kroky tooconfigure Windows Server Failover Clustering. toobuild clusteru převzetí služeb při selhání se systémem Windows, budete potřebovat sdíleném disku clusteru, několik IP adres, několik názvy virtuálních hostitelů a k nástroji pro vyrovnávání zatížení Azure interní pro clustering SAP ASC nebo SCS instance. To probereme podrobněji dále v článku hello.

![Obrázek 6: Windows Server Failover Clustering pro konfiguraci SAP ASC nebo SCS v Azure pomocí DataKeeper s][sap-ha-guide-figure-1002]

_**Obrázek 6:** Windows Server Failover Clustering SAP ASC nebo SCS konfigurace v Azure pomocí DataKeeper s_

### <a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Instance databázového systému vysokou dostupnost
Hello databázového systému je také jeden kontaktní bod v systému SAP. Je třeba tooprotect ho pomocí řešení vysoké dostupnosti. Obrázek 7 znázorňuje řešení vysoké dostupnosti SQL serveru Always On v Azure, Windows Server Failover Clustering a hello Azure interní nástroj pro vyrovnávání zatížení. SQL serveru Always On replikuje pomocí replikace vlastní databázového systému databázového systému data a soubory protokolu. V takovém případě můžete nebudete potřebovat clusteru sdílené disky, což zjednodušuje nastavení celý hello.

![Obrázek 7: Příklad databázového systému vysoké dostupnosti SAP, s SQL serveru Always On][sap-ha-guide-figure-2003]

_**Obrázek 7:** příklad databázového systému vysoké dostupnosti SAP, s SQL serveru Always On_

Další informace o clusteringu SQL Server v Azure pomocí modelu nasazení Azure Resource Manager hello najdete v těchto článcích:

* [Konfigurace skupiny dostupnosti Always On v Azure Virtual Machines ručně pomocí Resource Manageru][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]
* [Konfigurace k nástroji pro vyrovnávání zatížení Azure interní pro skupiny dostupnosti Always On v Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]

## <a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>Scénáře nasazení vysoké dostupnosti začátku do konce

### <a name="deployment-scenario-using-architectural-template-1"></a>Scénář nasazení pomocí architektury šablony 1

Obrázek 8 ukazuje příklad architektury SAP NetWeaver vysoké dostupnosti v Azure pro **jeden** systému SAP. Tento scénář je nastavit takto:

- Jeden vyhrazený cluster se používá pro instance SAP ASC nebo SCS hello.
- Jeden vyhrazený cluster se používá pro instanci databázového systému hello.
- Instance aplikačního serveru SAP nasazených ve svých vlastních vyhrazených virtuálních počítačích.

![Obrázek 8: SAP vysokou dostupnost architektury šablony 1 vyhrazeném clusteru pro ASC nebo SCS a databázového systému][sap-ha-guide-figure-2004]

_**Obrázek 8:** SAP architektury 1 šablony vysokou dostupnost, vyhrazené clustery pro ASC nebo SCS a databázového systému_

### <a name="deployment-scenario-using-architectural-template-2"></a>Scénář nasazení pomocí architektury šablony 2

Obrázek 9 ukazuje příklad architektury SAP NetWeaver vysoké dostupnosti v Azure pro **jeden** systému SAP. Tento scénář je nastavit takto:

- Jeden vyhrazený cluster se používá pro **i** hello SAP ASC nebo SCS instance a hello databázového systému.
- Instance aplikačního serveru SAP jsou nasazeny v vlastní vyhrazených virtuálních počítačích.

![Obrázek 9: SAP vysokou dostupnost architektury šablony 2, přičemž vyhrazeném clusteru pro ASC nebo SCS a vyhrazeném clusteru pro databázového systému][sap-ha-guide-figure-2005]

_**Obrázek 9:** SAP vysokou dostupnost architektury šablony 2, přičemž vyhrazeném clusteru pro ASC nebo SCS a vyhrazeném clusteru pro databázového systému_

### <a name="deployment-scenario-using-architectural-template-3"></a>Scénář nasazení pomocí architektury 3 šablony

Obrázek 10 ukazuje příklad architektury SAP NetWeaver vysoké dostupnosti v Azure pro **dva** SAP systémy, s &lt;SID1&gt; a &lt;SID2&gt;. Tento scénář je nastavit takto:

- Jeden vyhrazený cluster se používá pro **i** instance hello SAP ASC nebo SCS SID1 *a* hello SAP ASC nebo SCS SID2 instance (jeden cluster).
- Jeden vyhrazený cluster se používá pro SID1 databázového systému a jiné vyhrazeném clusteru se používá pro SID2 databázového systému (dva clustery).
- Instance aplikačního serveru SAP pro hello systému SAP SID1 mají své vlastní vyhrazených virtuálních počítačích.
- Instance aplikačního serveru SAP pro hello systému SAP SID2 mají své vlastní vyhrazených virtuálních počítačích.

![Obrázek 10: Vysoká dostupnost architektury šablony 3, s clusterem vyhrazené pro různé ASC nebo SCS instance SAP][sap-ha-guide-figure-6003]

_**Obrázek 10:** SAP vysokou dostupnost architektury šablony 3, s clusterem vyhrazené pro různé instance ASC nebo SCS_

## <a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>Příprava infrastruktury hello

### <a name="prepare-hello-infrastructure-for-architectural-template-1"></a>Příprava hello infrastruktury pro architektury šablony 1
Šablony Azure Resource Manageru pro SAP zjednodušit nasazení požadované prostředky.

Hello třívrstvá šablony ve službě Správce prostředků Azure také podporují scénáře vysoké dostupnosti, například architektury 1 šablony, která má dva clustery. Každý cluster je SAP jediný bod selhání pro SAP ASC nebo SCS a databázového systému.

Zde je, kde můžete získat šablon Azure Resource Manageru hello ukázkový scénář, který jsme popisují v tomto článku:

* [Obrázek pro Azure Marketplace.](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Vlastní image](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

tooprepare hello infrastruktury pro architektury šablona 1:

- V portálu Azure, na hello hello **parametry** okno, v hello **SYSTEMAVAILABILITY** vyberte **HA**.

  ![Obrázek 11: Nastavit SAP vysokou dostupnost Azure Resource Manageru parametry][sap-ha-guide-figure-3000]

_**Obrázek 11:** nastavit parametry SAP vysokou dostupnost Azure Resource Manager_


  Vytvoření šablony Hello:

  * **Virtuální počítače**:
    * Virtuální počítače aplikačního serveru SAP: <*SAPSystemSID*> - di - <*číslo*>
    * ASC nebo SCS clusteru virtuálních počítačů: <*SAPSystemSID*> - ASC - <*číslo*>
    * Cluster databázového systému: <*SAPSystemSID*> - db - <*číslo*>

  * **Síťové karty pro všechny virtuální počítače s přidružené IP adresy**:
    * <*SAPSystemSID*> - nic - di - <*číslo*>
    * <*SAPSystemSID*> - nic - ASC - <*číslo*>
    * <*SAPSystemSID*> - nic - db - <*číslo*>

  * **Účty úložiště Azure**

  * **Skupiny dostupnosti** pro:
    * Virtuální počítače aplikačního serveru SAP: <*SAPSystemSID*> - avset - di
    * SAP ASC nebo SCS clusteru virtuálních počítačů: <*SAPSystemSID*> - avset - ASC
    * Virtuální počítače clusteru databázového systému: <*SAPSystemSID*> - avset - db

  * **Nástroje pro vyrovnávání zatížení Azure interní**:
    * S všechny porty pro instanci ASC nebo SCS hello a IP adresu <*SAPSystemSID*> - lb - ASC
    * S všechny porty pro hello databázového systému SQL Server a IP adresu <*SAPSystemSID*> - lb - db

  * **Skupina zabezpečení sítě**: <*SAPSystemSID*> - nsg - ASC-0  
    * Pomocí otevřete externí toohello portu protokolu RDP (Remote Desktop) <*SAPSystemSID*> - ASC - 0 virtuálního počítače

> [!NOTE]
> Jsou všechny IP adresy hello síťové karty a nástroje pro vyrovnávání zatížení Azure interní **dynamické** ve výchozím nastavení. Změna je příliš**statické** IP adresy. Jsme popisují, jak toodo to později v článku hello.
>
>

### <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Nasadit virtuální počítače s toouse (mezi různými místy) připojení k podnikové síti v produkčním prostředí
Pro produkční systémy SAP, nasaďte virtuální počítače Azure s [připojení k podnikové síti (mezi různými místy)] [ planning-guide-2.2] pomocí Azure Site-to-Site VPN nebo Azure ExpressRoute.

> [!NOTE]
> Můžete použít instanci Azure Virtual Network. Hello virtuální síť a podsíť již byly vytvořeny a připravený.
>
>

1.  V portálu Azure, na hello hello **parametry** okno, v hello **NEWOREXISTINGSUBNET** vyberte **existující**.
2.  V hello **SUBNETID** pole, přidejte hello úplný řetězec připravené Azure sítě SubnetID, kam budete toodeploy, virtuální počítače Azure.
3.  tooget seznam všech podsítí síť Azure, spusťte tento příkaz prostředí PowerShell:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  Hello **ID** poli se zobrazí hello **SUBNETID**.
4. seznam všech tooget **SUBNETID** hodnoty, spusťte tento příkaz prostředí PowerShell:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  Hello **SUBNETID** vypadá podobně jako tento:

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

### <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Nasadit jenom pro cloud instance SAP pro testovací a demo
Můžete nasadit systém SAP vysoké dostupnosti v modelu nasazení jenom cloudu. Tento typ nasazení je primárně užitečné pro ukázku a testovací případy použití. Ji není vhodná pro produkční případy použití.

- V portálu Azure, na hello hello **parametry** okno, v hello **NEWOREXISTINGSUBNET** vyberte **nové**. Nechte hello **SUBNETID** pole prázdná.

  Hello SAP Azure Resource Manager automaticky vytvoří šablona hello virtuální síť Azure a podsíť.

> [!NOTE]
> Musíte taky toodeploy virtuální počítač alespoň jeden vyhrazený pro službu Active Directory a DNS v hello stejnou instanci Azure Virtual Network. Šablona Hello nepodporuje vytvoření těchto virtuálních počítačů.
>
>


### <a name="prepare-hello-infrastructure-for-architectural-template-2"></a>Příprava infrastruktury hello architektury šablony 2

Tato šablona Azure Resource Manager můžete použít pro SAP toohelp zjednodušit nasazování požadované infrastruktury prostředků pro SAP architektury šablony 2.

Zde je, kde můžete získat šablon Azure Resource Manageru pro tento scénář nasazení:

* [Obrázek pro Azure Marketplace.](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Vlastní image](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)


### <a name="prepare-hello-infrastructure-for-architectural-template-3"></a>Příprava hello infrastruktury pro architektury 3 šablony

Můžete připravit hello infrastruktury a nakonfigurovat SAP pro **více SID**. Například můžete přidat další instance SAP ASC nebo SCS do *existující* konfigurace clusteru. Další informace najdete v tématu [nakonfigurovat další instance SAP ASC nebo SCS do existující toocreate konfigurace clusteru více SID konfigurace aplikace SAP ve službě Správce prostředků Azure][sap-ha-multi-sid-guide].

Pokud chcete, aby toocreate do nového clusteru více SID, můžete použít více hello-identifikátor SID [šablony rychlý start na Githubu](https://github.com/Azure/azure-quickstart-templates).
toocreate do nového clusteru více SID, je třeba toodeploy hello následující tři šablony:

* [ASC nebo SCS šablony](#ASCS-SCS-template)
* [Šablona databáze](#database-template)
* [Šablona servery aplikací](#application-servers-template)

Hello následující sekce obsahují další podrobnosti o hello šablony a parametry hello potřebujete tooprovide v šablonách hello.

#### <a name="ASCS-SCS-template"></a>ASC nebo SCS šablony

Šablona ASC nebo SCS Hello nasadí dva virtuální počítače, které můžete použít toocreate cluster převzetí služeb při selhání systému Windows Server, který je hostitelem více instancí ASC nebo SCS.

tooset až hello ASC nebo SCS více SID šabloně hello [ASC nebo SCS více SID šablony][sap-templates-3-tier-multisid-xscs-marketplace-image], zadejte hodnoty pro hello následující parametry:

  - **Předpona prostředků**.  Nastavte předpona hello prostředek, což je použité tooprefix všechny prostředky, které jsou vytvořeny při nasazení hello. Protože hello prostředky nepatří tooonly jednoho systému SAP, předpona hello hello prostředku není hello SID systému SAP jeden.  Předpona Hello musí být mezi **tři až šest znaků**.
  - **Stack – typ**. Vyberte typ zásobníku hello hello systému SAP. V závislosti na typu hello zásobníku nástroj pro vyrovnávání zatížení Azure má jeden (ABAP nebo Java pouze) nebo dvě (ABAP + Java) privátní IP adresy na systému SAP.
  -  **Typ operačního systému**. Vyberte operační systém hello hello virtuálních počítačů.
  -  **Počet systému SAP**. Vyberte číslo hello systémů SAP chcete tooinstall v tomto clusteru.
  -  **Dostupnost systému**. Vyberte **HA**.
  -  **Uživatelské jméno správce a heslo správce**. Vytvořte nového uživatele, který lze použít toosign toohello počítači.
  -  **Nový nebo existující podsíť**. Nastavte, jestli mají být vytvořeny nové virtuální sítě a podsítě, nebo se použije existující podsítí. Pokud již máte virtuální síť, která je připojená tooyour do místní sítě, vyberte **existující**.
  -  **Id podsítě**. ID sady hello hello podsíť toowhich hello virtuálních počítačů by měly být připojené. Vyberte podsíť hello virtuální privátní sítě (VPN) nebo ExpressRoute virtuální sítě tooconnect hello virtuálního počítače tooyour do místní sítě. Hello ID obvykle vypadá takto:

   /subscriptions/ <*id předplatného*> /resourceGroups/ <*název skupiny prostředků*> /providers/Microsoft.Network/virtualNetworks/ <*název virtuální sítě*> /subnets/ <*název podsítě.*>

Šablona Hello nasadí jednu instanci nástroj pro vyrovnávání zatížení Azure, která podporuje více systémů SAP.

- Hello ASC instance jsou nakonfigurované pro instance číslo 00, 10, 20...
- Hello SCS instance jsou nakonfigurované pro instance číslo 01, 11, 21...
- instance Hello ASC zařazování replikace serveru (YBRAT) (pouze Linux) jsou nakonfigurované pro čísla instance 02, 12, 22...
- instance Hello SCS YBRAT (pouze Linux) jsou nakonfigurované pro instance číslo 03, 13, 23...

Hello nástroj pro vyrovnávání zatížení obsahuje 1 (2 pro Linux) VIP(s), 1 x virtuální IP adresa pro ASC nebo SCS a 1 x virtuální IP adresa pro YBRAT (pouze Linux).

Hello následující seznam obsahuje všechny pravidla (kde x je číslo hello hello SAP systému, například 1, 2, 3...) pro vyrovnávání zatížení:
- Porty specifické pro systém Windows pro každý systém SAP: 445, 5985
- Porty ASC (čísla instance x0): 32 × 0, 36 x 0, 39 x 0, 81 x 0, 5 x 013, 5 x 014, 5 x 016
- Porty SCS (čísla instance x1): 32 x 1, 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116
- ASC YBRAT portů v systému Linux (čísla instance x2): 33 x 2, 5 x 213, 5 x 214, 5 x 216
- SCS YBRAT portů v systému Linux (čísla instance x3): 33 x 3, 5 x 313, 5 x 314, 5 x 316

Nástroj pro vyrovnávání zatížení Hello je nakonfigurované toouse hello následující porty testu (kde x je číslo hello hello SAP systému, například 1, 2, 3...):
- Port testu nástroje pro vyrovnávání zatížení ASC nebo SCS interní: 620 x 0
- Interní YBRAT načíst port testu vyrovnávání (pouze Linux): 621 x 2

#### <a name="database-template"></a>Šablona databáze

Šablona databáze Hello nasadí jeden nebo dva virtuální počítače, které můžete použít tooinstall hello systému správy relačních databází (RDBMS) pro systém SAP. Například pokud nasadíte šablonu ASC nebo SCS pro pět systémy SAP, je třeba toodeploy této šablony pětkrát.

tooset až hello databáze více SID šablony, v hello [databáze více SID šablony][sap-templates-3-tier-multisid-db-marketplace-image], zadejte hodnoty pro hello následující parametry:

  -  **Id systému SAP**. Zadejte ID systému SAP hello hello chcete tooinstall systému SAP. Hello ID se použije jako předpona pro hello prostředky, které jsou nasazeny.
  -  **Typ operačního systému**. Vyberte operační systém hello hello virtuálních počítačů.
  -  **Hodnota DbType**. Vyberte typ hello hello databáze, kterou chcete tooinstall v clusteru hello. Vyberte **SQL** Pokud chcete, aby tooinstall Microsoft SQL Server. Vyberte **HANA** Pokud máte v plánu tooinstall SAP HANA hello virtuálními počítači. Zajistěte, aby tooselect hello správný typ operačního systému: vyberte **Windows** pro SQL a vyberte distribuční Linux pro HANA. Hello Vyrovnávání zatížení Azure, která je připojená toohello, které virtuální počítače budou nakonfigurované toosupport hello vybrané databáze typu:
    * **SQL**. Nástroj pro vyrovnávání zatížení Hello bude Vyrovnávání zatížení port 1433. Ujistěte se, že toouse tento port pro váš instalační program SQL serveru Always On.
    * **HANA**. Nástroj pro vyrovnávání zatížení Hello bude porty 35015 a 35017 Vyrovnávání zatížení. Ujistěte se, že tooinstall SAP HANA číslem instance **50**.
    Nástroj pro vyrovnávání zatížení Hello použije port testu 62550.
  -  **Velikost systému SAP**. Poskytne sadu hello počet protokoly SAP hello nový systém. Pokud si nejste jisti, kolik systému hello protokoly SAP bude vyžadovat, požádejte SAP technologie partnera nebo systémový integrátor.
  -  **Dostupnost systému**. Vyberte **HA**.
  -  **Uživatelské jméno správce a heslo správce**. Vytvořte nového uživatele, který lze použít toosign toohello počítači.
  -  **Id podsítě**. Zadejte ID hello hello podsítě, který jste použili při nasazení hello hello ASC nebo SCS šablony nebo ID hello hello podsítě, který byl vytvořen jako součást hello ASC nebo SCS šablonu nasazení.

#### <a name="application-servers-template"></a>Šablona servery aplikací

šablony servery aplikace Hello nasadí dvě nebo více virtuálních počítačů, které lze použít jako instance aplikačního serveru SAP jeden systému SAP. Například pokud nasadíte šablonu ASC nebo SCS pro pět systémy SAP, je třeba toodeploy této šablony pětkrát.

tooset až hello servery více SID šablona aplikací v hello [šablony SID více serverů aplikace][sap-templates-3-tier-multisid-apps-marketplace-image], zadejte hodnoty pro hello následující parametry:

  -  **Id systému SAP**. Zadejte ID systému SAP hello hello chcete tooinstall systému SAP. Hello ID se použije jako předpona pro hello prostředky, které jsou nasazeny.
  -  **Typ operačního systému**. Vyberte operační systém hello hello virtuálních počítačů.
  -  **Velikost systému SAP**. poskytne Hello počet protokoly SAP hello nový systém. Pokud si nejste jisti, kolik systému hello protokoly SAP bude vyžadovat, požádejte SAP technologie partnera nebo systémový integrátor.
  -  **Dostupnost systému**. Vyberte **HA**.
  -  **Uživatelské jméno správce a heslo správce**. Vytvořte nového uživatele, který lze použít toosign toohello počítači.
  -  **Id podsítě**. Zadejte ID hello hello podsítě, který jste použili při nasazení hello hello ASC nebo SCS šablony nebo ID hello hello podsítě, který byl vytvořen jako součást hello ASC nebo SCS šablonu nasazení.


### <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Virtuální síť Azure
V našem příkladu hello adresním prostoru hello virtuální síť Azure je 10.0.0.0/16. Existuje jedna podsíť s názvem **podsíť**, rozsah adres 10.0.0.0/24. Všechny virtuální počítače a interní služby load balancer nasazených v této virtuální síti.

> [!IMPORTANT]
> Nemáte nic měnit nastavení sítě toohello uvnitř hello hostovaného operačního systému. To zahrnuje IP adresy, servery DNS a podsítě. Nakonfigurujte všechna nastavení sítě v Azure. Hello služby Dynamic Host Configuration Protocol (DHCP) rozšíří nastavení.
>
>

### <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS IP adresy

tooset hello požadované že DNS IP adresy, hello následující kroky.

1.  V portálu Azure, na hello hello **servery DNS** okno, ujistěte se, že virtuální sítě **servery DNS** je možnost nastavena příliš**vlastní DNS**.
2.  Vyberte nastavení na základě typu hello sítě, které máte. Další informace najdete v tématu hello následující prostředky:
    * [Připojení k podnikové síti (mezi různými místy)][planning-guide-2.2]: Přidat hello IP adresy serverů DNS místní hello.  
    Můžete rozšířit místní DNS servery toohello virtuálních počítačů, které jsou spuštěné v Azure. V tomto scénáři, můžete přidat hello IP adresy hello Azure virtuální počítače, na které spouštíte služba DNS hello.
    * [Čistě cloudové nasazení][planning-guide-2.1]: nasadit další virtuální počítač, pomocí něhož v hello stejnou instanci virtuální sítě, která slouží jako DNS server. Přidejte hello Azure hello IP adresy virtuálních počítačů, které jste nastavili toorun služba DNS.

    ![Obrázek 12: Servery DNS nakonfigurujte pro virtuální síť Azure][sap-ha-guide-figure-3001]

    _**Obrázek 12:** konfigurace DNS serverů pro virtuální síť Azure_

  > [!NOTE]
  > Pokud změníte hello IP adresy serverů DNS hello, je třeba toorestart hello virtuální počítače Azure tooapply hello změn a šířit hello nových serverů DNS.
  >
  >

V našem příkladu hello služba DNS je nainstalovaný a nakonfigurovaný na těchto virtuálních počítačích Windows:

| Role virtuálního počítače | Název hostitele virtuálního počítače | Název síťové karty | Statická IP adresa |
| --- | --- | --- | --- |
| První server DNS |domcontr-0 |PR1-seskupování domcontr-0 |10.0.0.10 |
| Druhý server DNS |domcontr-1 |PR1-seskupování domcontr-1 |10.0.0.11 |

### <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Názvy hostitelů a statické IP adresy pro clusterové instance SAP ASC nebo SCS hello a Clusterové instance databázového systému

Pro místní nasazení je třeba tyto názvy vyhrazené hostitele a IP adresy:

| Název role virtuálních hostitelů | Název virtuálního hostitele | Virtuální statickou IP adresu |
| --- | --- | --- |
| SAP ASC nebo SCS první clusteru virtuální hostitel název (pro správu clusteru) |PR1. ASC vir |10.0.0.42 |
| Název virtuálního hostitele instance SAP ASC nebo SCS |PR1. ASC sap |10.0.0.43 |
| Databázového systému SAP druhý cluster virtuálního hostitele název (Správa clusteru) |PR1. databázového systému vir |10.0.0.32 |

Při vytváření clusteru hello vytvořit hello názvy virtuálních hostitelů **pr1. ASC vir** a **pr1. databázového systému vir** a hello přidružené IP adresy, které spravují samotného clusteru hello. Informace o toodo, najdete v tématu [shromažďovat uzly clusteru v konfiguraci clusteru][sap-ha-guide-8.12.1].

Můžete ručně vytvořit hello další dva názvy virtuálních hostitelů **pr1. ASC sap** a **pr1. databázového systému sap**, a hello přidružené IP adresy, na serveru DNS hello. Hello Clusterové instance SAP ASC nebo SCS a instance databázového systému hello v clusteru používat tyto prostředky. Informace o toodo, najdete v tématu [vytvořte název virtuálního hostitele pro clusterovou instanci SAP ASC nebo SCS][sap-ha-guide-9.1.1].

### <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Nastavení statické IP adresy pro virtuální počítače, hello SAP
Poté, co nasadíte toouse hello virtuální počítače v clusteru, musíte tooset statické IP adresy pro všechny virtuální počítače. To udělat v hello konfigurace Azure Virtual Network a není v hello hostovaného operačního systému.

1.  V hello portálu Azure, vyberte **skupiny prostředků** > **síťové karty** > **nastavení** > **IP adresa** .
2.  Na hello **IP adresy** okno, v části **přiřazení**, vyberte **statické**. V hello **IP adresu** zadejte hello IP adresu, které chcete toouse.

  > [!NOTE]
  > Pokud změníte hello IP adresu hello síťové karty, je třeba toorestart hello virtuální počítače Azure tooapply hello změnu.  
  >
  >

  ![Obrázek 13: Nastavení statické IP adresy pro síťové karty hello každého virtuálního počítače][sap-ha-guide-figure-3002]

  _**Obrázek 13:** nastavení statické IP adresy pro síťové karty hello každého virtuálního počítače_

  Opakujte tento krok pro všechna síťová rozhraní, který je pro všechny virtuální počítače, včetně virtuálních počítačů má toouse služby Active Directory a DNS.

V našem příkladu máme tyto virtuální počítače a statické IP adresy:

| Role virtuálního počítače | Název hostitele virtuálního počítače | Název síťové karty | Statická IP adresa |
| --- | --- | --- | --- |
| První instance aplikačního serveru SAP |PR1-di-0 |PR1-seskupování di-0 |10.0.0.50 |
| Druhá instance aplikačního serveru SAP |PR1-di-1 |PR1-seskupování di-1 |10.0.0.51 |
| Tlačítka ... |Tlačítka ... |Tlačítka ... |Tlačítka ... |
| Poslední instance aplikačního serveru SAP |PR1-di-5 |PR1 seskupování di 5 |10.0.0.55 |
| Prvním uzlu clusteru pro instanci ASC nebo SCS |PR1-ASC-0 |PR1-seskupování ASC-0 |10.0.0.40 |
| Druhý uzel clusteru pro instanci ASC nebo SCS |PR1-ASC-1 |PR1-seskupování ASC-1 |10.0.0.41 |
| Prvním uzlu clusteru pro instanci databázového systému |PR1-db-0 |PR1-seskupování db-0 |10.0.0.30 |
| Druhý uzel clusteru pro instanci databázového systému |PR1-db-1 |PR1-seskupování db-1 |10.0.0.31 |

### <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Nastavení statické IP adresy pro vyrovnávání zatížení Azure interní hello

Vytvoří Hello SAP Azure Resource Manager šablona pro vyrovnávání zatížení Azure interní, který se používá pro hello SAP ASC nebo SCS instance clusteru a clusteru hello databázového systému.

> [!IMPORTANT]
> Hello IP adresu název virtuálního hostitele hello hello je SAP ASC nebo SCS hello stejné jako IP adresa hello hello SAP ASC nebo SCS interní vyrovnávání zátěže: **pr1-lb Asc**.
> Hello IP adresu hello virtuální název tohoto hello databázového systému je hello stejné jako IP adresa hello hello databázového systému interní vyrovnávání zátěže: **databázového systému pr1 lb**.
>
>

tooset statickou IP adresu pro hello Azure interní nástroj pro vyrovnávání zatížení:

1.  Hello počáteční nasazení nastaví IP adresa služby Vyrovnávání zatížení pro vnitřní hello příliš**dynamické**. V portálu Azure, na hello hello **IP adresy** okno, v části **přiřazení**, vyberte **statické**.
2.  Nastavení hello IP adresy služby Vyrovnávání zatížení interní hello **pr1-lb Asc** toohello IP adresu název virtuálního hostitele hello instance SAP ASC nebo SCS hello.
3.  Nastavení hello IP adresy služby Vyrovnávání zatížení interní hello **databázového systému pr1 lb** toohello IP adresu název virtuálního hostitele hello hello instance databázového systému.

  ![Obrázek 14: Nastavení statické IP adresy pro službu Vyrovnávání zatížení interní hello instance SAP ASC nebo SCS hello][sap-ha-guide-figure-3003]

  _**Obrázek 14:** nastavení statické IP adresy pro službu Vyrovnávání zatížení interní hello instance SAP ASC nebo SCS hello_

V našem příkladu máme dvě Azure interní služby load balancer, které mají tyto statické IP adresy:

| Role služby Vyrovnávání zatížení Azure interní | Název nástroje pro vyrovnávání zatížení Azure interní | Statická IP adresa |
| --- | --- | --- |
| SAP ASC nebo SCS instanci interní zátěže. |PR1-lb ASC |10.0.0.43 |
| Databázového systému SAP interní nástroj pro vyrovnávání zatížení |PR1-lb-databázového systému |10.0.0.33 |


### <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Výchozí pravidla pro vyrovnávání zatížení Azure interní hello Vyrovnávání zatížení ASC nebo SCS

Hello SAP Azure Resource Manager šablona vytváří hello porty, které budete potřebovat:
* Instance ABAP ASC s hello výchozí instance číslo **00**
* Instance Java SCS, s hello výchozí instance číslo **01**

Pokud nainstalujete instanci SAP ASC nebo SCS, je nutné použít hello výchozí instance číslo **00** pro vaše číslo ABAP ASC instance a hello výchozí instance **01** instance Java SCS.

Dále vytvořte požadované interní koncové body pro hello SAP NetWeaver porty pro vyrovnávání zatížení.

toocreate požadované interní služby load vyrovnávání koncových bodů, nejprve vytvořte tyto koncové body pro hello SAP NetWeaver ABAP ASC porty pro vyrovnávání zatížení:

| Název pravidla vyrovnávání služby nebo zatížení | Výchozí čísla portů | Konkrétní porty (ASC instance číslem instance 00) (YBRAT s 10) |
| --- | --- | --- |
| Zařadit Server / *lbrule3200* |32 <*InstanceNumber*> |3200 |
| Server zpráv ABAP / *lbrule3600* |36 <*InstanceNumber*> |3600 |
| Zpráva Vnitřní ABAP / *lbrule3900* |39 <*InstanceNumber*> |3900 |
| Server HTTP zpráv nebo *Lbrule8100* |81 <*InstanceNumber*> |8100 |
| SAP spuštění služby ASC HTTP / *Lbrule50013* |5 <*InstanceNumber*> 13 |50013 |
| SAP spuštění služby ASC HTTPS nebo *Lbrule50014* |5 <*InstanceNumber*> 14 |50014 |
| Zařazování replikace nebo *Lbrule50016* |5 <*InstanceNumber*> 16 |50016 |
| SAP spuštění služby YBRAT HTTP *Lbrule51013* |5 <*InstanceNumber*> 13 |51013 |
| SAP spuštění služby YBRAT HTTP *Lbrule51014* |5 <*InstanceNumber*> 14 |51014 |
| Win RM *Lbrule5985* | |5985 |
| Sdílení souborů *Lbrule445* | |445 |

_**Tabulka 1:** portu čísla instance SAP NetWeaver ABAP ASC hello_

Pak vytvořte tyto koncové body pro hello SAP NetWeaver Java SCS porty pro vyrovnávání zatížení:

| Název pravidla vyrovnávání služby nebo zatížení | Výchozí čísla portů | Konkrétní porty (SCS instance číslem instance 01) (YBRAT s 11) |
| --- | --- | --- |
| Zařadit Server / *lbrule3201* |32 <*InstanceNumber*> |3201 |
| Server brány nebo *lbrule3301* |33 <*InstanceNumber*> |3301 |
| Server zpráv Java / *lbrule3900* |39 <*InstanceNumber*> |3901 |
| Server HTTP zpráv nebo *Lbrule8101* |81 <*InstanceNumber*> |8101 |
| SAP spuštění služby SCS HTTP / *Lbrule50113* |5 <*InstanceNumber*> 13 |50113 |
| SAP spuštění služby SCS HTTPS nebo *Lbrule50114* |5 <*InstanceNumber*> 14 |50114 |
| Zařazování replikace nebo *Lbrule50116* |5 <*InstanceNumber*> 16 |50116 |
| SAP spuštění služby YBRAT HTTP *Lbrule51113* |5 <*InstanceNumber*> 13 |51113 |
| SAP spuštění služby YBRAT HTTP *Lbrule51114* |5 <*InstanceNumber*> 14 |51114 |
| Win RM *Lbrule5985* | |5985 |
| Sdílení souborů *Lbrule445* | |445 |

_**Tabulka 2:** portu čísla instance SAP NetWeaver Java SCS hello_

![Obrázek 15: Pravidla pro vyrovnávání zatížení Azure interní hello Vyrovnávání zatížení výchozí ASC nebo SCS][sap-ha-guide-figure-3004]

_**Obrázek 15:** ASC nebo SCS výchozí pravidla pro vyrovnávání zatížení Azure interní hello Vyrovnávání zatížení_

Nastavení hello IP adresy služby Vyrovnávání zatížení hello **databázového systému pr1 lb** toohello IP adresu název virtuálního hostitele hello hello instance databázového systému.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Změňte pravidla pro vyrovnávání zatížení Azure interní hello Vyrovnávání zatížení pro výchozí hello ASC nebo SCS

Pokud chcete pro hello SAP ASC nebo instance SCS toouse různá čísla, je třeba změnit hello názvy a hodnoty jejich porty z výchozí hodnoty.

1.  V hello portálu Azure, vyberte  **<* SID*> - lb - ASC načíst vyrovnávání ** > **pravidla Vyrovnávání zatížení**.
2.  Pro všechna pravidla, které patří toohello SAP ASC nebo SCS instance Vyrovnávání zatížení změňte tyto hodnoty:

  * Name (Název)
  * Port
  * Back-end port

  Například pokud chcete, aby toochange hello výchozí ASC instance číslo od 00 too31, musíte změny hello toomake pro všechny porty uvedené v tabulce 1.

  Tady je příklad aktualizace pro port *lbrule3200*.

  ![Obrázek 16: Pravidla pro vyrovnávání zatížení Azure interní hello Vyrovnávání zatížení pro výchozí hello ASC nebo SCS změňte][sap-ha-guide-figure-3005]

  _**Obrázek 16:** změnu hello ASC nebo SCS výchozí Vyrovnávání zatížení, pravidla pro vyrovnávání zatížení Azure interní hello_

### <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Přidání domény toohello virtuální počítače Windows

Po přiřazení počítačů toohello virtuální sady statických IP adres, přidejte domény toohello hello virtuálních počítačů.

![Obrázek 17: Přidání domény tooa virtuálního počítače][sap-ha-guide-figure-3006]

_**Obrázek 17:** přidat doménu tooa virtuálního počítače_

### <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Přidat položky registru na obou uzlů clusteru instance SAP ASC nebo SCS hello

Azure Vyrovnávání zatížení má interní nástroj, zavře připojení při hello připojení jsou nastavte dobu nečinnosti, po času (časový limit nečinnosti). SAP pracovních procesů v dialogovém okně instancí otevřená připojení toohello SAP zařazování zpracovat co nejrychleji hello první zařazování nebo vyřazení z fronty požadavků toobe potřeby odeslat. Tato připojení obvykle zůstat zavedené do hello pracovní proces nebo hello zařazování proces restartuje. Je-li připojení hello na nastavenou dobu nečinnosti, ale hello připojení hello zavře nástroj pro vyrovnávání zatížení Azure interní. To není problém, protože hello SAP pracovní proces obnoví proces zařazování toohello hello připojení, pokud už existuje. Tyto aktivity jsou popsané v trasování vývojáře hello procesů SAP, ale uživatel vytvořit velké množství další obsah v těchto trasování. Je vhodné toochange hello TCP/IP `KeepAliveTime` a `KeepAliveInterval` na obou uzlů clusteru. Kombinací těchto změnách ve parametry protokolu TCP/IP hello s parametry profil SAP, popsány dále v článku hello.

položky registru tooadd na obou uzlů clusteru instance SAP ASC nebo SCS hello, nejprve přidejte tyto položky registru systému Windows na obou uzlů clusteru systému Windows pro SAP ASC nebo SCS:

| Cesta | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Název proměnné |`KeepAliveTime` |
| Typ proměnné |REG_DWORD (decimální): |
| Hodnota |120000 |
| Odkaz toodocumentation |[https://technet.microsoft.com/en-us/library/cc957549.aspx](https://technet.microsoft.com/en-us/library/cc957549.aspx) |

_**Tabulka 3:** změnu hello první parametr TCP/IP_

Pak přidejte této položky registru systému Windows na obou uzlů clusteru systému Windows pro SAP ASC nebo SCS:

| Cesta | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Název proměnné |`KeepAliveInterval` |
| Typ proměnné |REG_DWORD (decimální): |
| Hodnota |120000 |
| Odkaz toodocumentation |[https://technet.microsoft.com/en-us/library/cc957548.aspx](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

_**Tabulka 4:** změnu hello druhý parametr TCP/IP_

**změny hello tooapply, restartujte obou uzlů clusteru**.

### <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Nastavení clusteru Windows Server Failover Clustering pro instance SAP ASC nebo SCS

Nastavení clusteru s podporou služby Windows Server Failover Clustering pro instance SAP ASC nebo SCS zahrnuje tyto úlohy:

- Shromažďování hello uzly clusteru v konfiguraci clusteru
- Konfigurace určující sdílenou složku clusteru

#### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Shromažďovat hello uzly clusteru v konfiguraci clusteru

1.  Hello přidat Role a funkce průvodce přidejte clustering tooboth uzly clusteru převzetí služeb při selhání.
2.  Nastavení clusteru převzetí služeb při selhání hello pomocí Správce clusteru převzetí služeb při selhání. Ve Správci clusteru převzetí služeb při selhání vyberte **vytvořením clusteru**a poté přidejte pouze hello název hello první clusteru, uzel A. Nepřidávejte druhého uzlu hello ještě; přidáte hello druhého uzlu do pozdějšího kroku.

  ![Obrázek 18: Přidání hello serveru nebo název virtuálního počítače hello prvního uzlu clusteru][sap-ha-guide-figure-3007]

  _**Obrázek 18:** hello přidat server nebo virtuální počítač název prvního uzlu clusteru hello_

3.  Zadejte název sítě hello (název hostitele virtuálního) clusteru hello.

  ![Obrázek 19: Zadejte název clusteru hello][sap-ha-guide-figure-3008]

  _**Obrázek 19:** zadejte název clusteru hello_

4.  Po vytvoření clusteru hello, spusťte test ověření clusteru.

  ![Obrázek 20: Spustit kontrolu ověření clusteru hello][sap-ha-guide-figure-3009]

  _**Obrázek 20:** spustit kontrolu pro ověření clusteru hello_

  Můžete ignorovat jakékoli upozornění o discích v tomto okamžiku v procesu hello. Přidáte, že určující sdílenou složku a hello SIOS sdílené disky později. V této fázi není nutný tooworry o jako kvorum.

  ![Obrázek 21: Nenajde žádný disk kvora][sap-ha-guide-figure-3010]

  _**Obrázek 21:** nenajde žádný disk kvora_

  ![Obrázek 22: Prostředek clusteru jádra potřebuje novou IP adresu][sap-ha-guide-figure-3011]

  _**Obrázek 22:** prostředku clusteru jádra potřebuje novou IP adresu_

5.  Změna IP adresy hello hello základní Clusterové služby. Hello clusteru nelze spustit, dokud změnit IP adresu hello hello základní clusteru služby, protože hello IP adresa serveru hello body tooone uzly hello virtuálního počítače. To udělat na hello **vlastnosti** stránku hello základní Clusterovou službu na IP prostředku.

  Například potřebujeme tooassign IP adresu (v našem příkladu **10.0.0.42**) pro název virtuálního hostitele clusteru hello **pr1. ASC vir**.

  ![Obrázek 23: V dialogové okno Vlastnosti hello, změna hello IP adresy][sap-ha-guide-figure-3012]

  _**Obrázek 23:** v hello **vlastnosti** dialogové okno, změna hello IP adresu_

  ![Obrázek 24: Přiřadit hello IP adresu, která je vyhrazena pro hello cluster][sap-ha-guide-figure-3013]

  _**Obrázek 24:** přiřadit hello IP adresu, která je vyhrazena pro hello cluster_

6.  Přepněte hello virtuální hostitel název clusteru online.

  ![Obrázek 25: Základní služby clusteru je zapnutý a běží a s hello opravte IP adresu][sap-ha-guide-figure-3014]

  _**Obrázek 25:** je základní služba clusteru a spuštěný a s hello opravte IP adresu_

7.  Přidání druhého uzlu clusteru hello.

  Nyní když hello základní Clusterová služba spuštěná, můžete přidat hello druhého uzlu clusteru.

  ![Obrázek 26: Přidání druhého uzlu clusteru hello][sap-ha-guide-figure-3015]

  _**Obrázek 26:** druhého uzlu clusteru přidat hello_

8.  Zadejte název pro hostitele na druhého uzlu clusteru hello.

  ![Obrázek 27: Zadejte název hostitele uzlu druhý clusteru hello][sap-ha-guide-figure-3016]

  _**Obrázek 27:** Enter hello druhý název uzlu clusteru hostitele_

  > [!IMPORTANT]
  > Ujistěte se, že hello **přidejte všechna vhodná úložiště toohello cluster** zaškrtávací políčko je **není** vybrané.  
  >
  >

  ![Obrázek 28: Nezaškrtávejte políčko hello][sap-ha-guide-figure-3017]

  _**Obrázek 28:** provést **není** hello vyberte zaškrtávací políčko_

  Můžete ignorovat upozornění o kvora a disky. Budete nastavit hello disk kvora a sdílenou složku hello později, jak je popsáno v [instalace s DataKeeper Cluster Edition u disku clusteru sdílení, SAP ASC nebo SCS][sap-ha-guide-8.12.3].

  ![Obrázek 29: Ignorujte upozornění o hello disku kvora][sap-ha-guide-figure-3018]

  _**Obrázek 29:** ignorovat upozornění o hello disku kvora_


#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Konfigurovat určující sdílenou složku clusteru

Konfigurace určující sdílenou složku clusteru zahrnuje tyto úlohy:

- Vytvoření sdílené složky
- Nastavení hello soubor sdílené složky s kopií clusteru kvora ve Správci clusteru převzetí služeb při selhání

##### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Vytvoření sdílené složky

1.  Vyberte složku s kopií místo disk kvora. Tuto volbu podporuje DataKeeper s.

  V hello příklady v tomto článku je hello určující sdílenou složku na serveru hello Active Directory a DNS, který běží v Azure. Hello určující sdílená složka se označuje jako **domcontr-0**. Vzhledem k tomu, že byste nakonfigurovali tooAzure připojení VPN (prostřednictvím sítě Site-to-Site VPN nebo Azure ExpressRoute), vaše/DNS služby Active Directory service je na místě a není vhodný toorun soubor sdílet s kopií clusteru.

  > [!NOTE]
  > Pokud služby Active Directory a DNS používá jenom v místě, není konfigurace vaší určující sdílenou složku na hello Active Directory a DNS Windows operační systém, který běží na místě. Latence sítě mezi uzly clusteru, které jsou spuštěné v Azure a Active Directory a DNS v místním může být příliš velký a způsobit problémy s připojením. Být jisti tooconfigure hello určující sdílenou složku na virtuální počítač Azure se systémem zavřít toohello uzlu clusteru.  
  >
  >

  disk kvora Hello potřebuje alespoň 1 024 MB volného místa. Doporučujeme, abyste hodnotu 2 048 MB volného místa pro disk kvora hello.

2.  Přidejte objekt názvu clusteru hello.

  ![Obrázek 30: Přiřadit oprávnění hello hello sdílené složky pro objekt názvu clusteru hello][sap-ha-guide-figure-3019]

  _**Obrázek 30:** přiřadit oprávnění hello hello sdílené složky pro objekt názvu clusteru hello_

  Ujistěte se, že oprávnění hello zahrnout hello autority toochange data hello sdílené složky pro objekt názvu clusteru hello (v našem příkladu **pr1. ASC vir$**).

3.  tooadd hello clusteru název toohello seznam objektů, vyberte **přidat**. Změnit hello toocheck filtru pro počítačových objektů ve toothose přidání znázorňuje obrázek 31.

  ![Obrázek 31: Změna počítače tooinclude hello typy objektů][sap-ha-guide-figure-3020]

  _**Obrázek 31:** změnit hello typy objektů tooinclude počítače_

  ![Obrázek 32: Políčko hello počítače][sap-ha-guide-figure-3021]

  _**Obrázek 32:** vyberte hello **počítače** zaškrtávací políčko_

4.  Zadejte objekt názvu clusteru hello jak ukazuje obrázek 31. Protože byl vytvořen záznam hello, můžete změnit hello oprávnění, jak ukazuje obrázek 30.

5.  Vyberte hello **zabezpečení** podrobnější hello sdílenou složku a potom nastavte oprávnění pro objekt názvu clusteru hello.

  ![Obrázek 33: Nastavení atributů hello zabezpečení pro objekt názvu clusteru hello hello souboru sdílenou složku kvora][sap-ha-guide-figure-3022]

  _**Obrázek 33:** nastavit atributy hello zabezpečení pro objekt názvu clusteru hello na hello souboru sdílenou složku kvora_

##### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Nastavení hello soubor sdílené složky s kopií clusteru kvora ve Správci clusteru převzetí služeb při selhání

1.  Otevřete hello Průvodce nastavení konfigurace kvora.

  ![Obrázek 34: Spuštění hello konfigurace Průvodce nastavení kvora clusteru][sap-ha-guide-figure-3023]

  _**Obrázek 34:** počáteční hello Průvodce nastavení konfigurace kvora clusteru_

2.  Na hello **vyberte konfiguraci kvora** vyberte **vybrat určující disk kvora hello**.

  ![Obrázek 35: Konfigurací kvora, které můžete vybrat z][sap-ha-guide-figure-3024]

  _**Obrázek 35:** konfigurací kvora, můžete si vybrat z_

3.  Na hello **vybrat určující disk kvora** vyberte **nakonfigurovat určující sdílenou složku**.

  ![36 obrázek: Určující sdílená složka vyberte hello][sap-ha-guide-figure-3025]

  _**Obrázek 36:** vybrat určující sdílenou složku hello_

4.  Zadejte hello UNC cestu toohello sdílené složky (v našem příkladu \\domcontr 0\FSW). toosee seznam změn hello můžete provést, vyberte **Další**.

  ![Obrázek 37: Definování hello umístění sdílené složky pro sdílenou složku určující hello][sap-ha-guide-figure-3026]

  _**Obrázek 37:** definovat hello umístění sdílené složky pro sdílenou složku určující hello_

5.  Vyberte hello změny a pak vyberte **Další**. Je třeba toosuccessfully překonfigurovat hello konfiguraci clusteru, jak ukazuje obrázek 38.  

  ![Obrázek 38: Potvrzení, že jste překonfigurovat hello clusteru][sap-ha-guide-figure-3027]

  _**Obrázek 38:** potvrzení, že jste překonfigurovat hello clusteru_

Po úspěšném nainstalování hello clusteru převzetí služeb při selhání se systémem Windows, třeba změny toobe provedené toosome prahové hodnoty tooadapt převzetí služeb při selhání detekce tooconditions v Azure. Hello změnit toobe parametry jsou popsané v tomto blogu: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/. Za předpokladu, že dva virtuální počítače, který sestavení hello konfigurace clusteru systému Windows pro ASC nebo SCS jsou v hello stejné podsíti, hello následující parametry potřebovat změnit toobe toothese hodnoty:
- SameSubNetDelay = 2
- SameSubNetThreshold = 15

Tato nastavení byly testovány s zákazníků a k dispozici dobrý ohrožení toobe dostatečně odolný na jedné straně hello. Na hello druhé straně tato nastavení se poskytuje rychlé dostatek převzetí služeb při selhání ve skutečné chybové stavy v SAP selhání uzlu nebo virtuálního počítače nebo softwaru. 

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>Instalace s DataKeeper Cluster Edition pro disk sdílené složky clusteru SAP ASC nebo SCS hello

Teď máte fungující konfiguraci služby Windows Server Failover Clustering v Azure. Ale tooinstall instance SAP ASC nebo SCS, budete potřebovat prostředek sdíleného disku. Nelze vytvořit hello sdílené diskové prostředky, které potřebujete v Azure. S DataKeeper Cluster Edition je řešení třetí strany, můžete použít toocreate sdílené diskové prostředky.

Instalace s DataKeeper Cluster Edition pro hello SAP ASC nebo SCS disk sdílené složky clusteru zahrnuje tyto úlohy:

- Přidání hello rozhraní .NET Framework 3.5
- Instalace SIOS DataKeeper
- Nastavení služby s DataKeeper

#### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>Přidat hello rozhraní .NET Framework 3.5
Hello rozhraní Microsoft .NET Framework 3.5 se automaticky aktivovat nebo není nainstalovaná v systému Windows Server 2012 R2. Protože DataKeeper s vyžaduje rozhraní .NET Framework toobe hello na všech uzlech, které nainstalujete DataKeeper na, je nutné nainstalovat na hello hostovaný operační systém všech virtuálních počítačů v clusteru hello hello rozhraní .NET Framework 3.5.

Existují dva způsoby tooadd hello rozhraní .NET Framework 3.5:

- Použití Průvodce hello přidání rolí a funkcí v systému Windows, jak ukazuje obrázek 39.

  ![Obrázek 39: Instalace hello rozhraní .NET Framework 3.5 pomocí Průvodce hello přidání rolí a funkcí][sap-ha-guide-figure-3028]

  _**Obrázek 39:** hello instalace rozhraní .NET Framework 3.5 pomocí Průvodce hello přidání rolí a funkcí_

  ![Obrázek 40: Průběh instalace panelu při instalaci hello rozhraní .NET Framework 3.5 pomocí Průvodce hello přidání rolí a funkcí][sap-ha-guide-figure-3029]

  _**Obrázek 40:** průběh instalace panelu při instalaci hello rozhraní .NET Framework 3.5 pomocí Průvodce hello přidání rolí a funkcí_

- Použijte nástroj příkazového řádku nástroje dism.exe hello. Pro tento typ instalace musíte tooaccess hello SxS adresáře na instalačním médiu systému Windows hello. Na příkazovém řádku se zvýšenými oprávněními zadejte:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <a name="dd41d5a2-8083-415b-9878-839652812102"></a>Nainstalujte SIOS DataKeeper

Nainstalujte na každém uzlu v clusteru hello s DataKeeper Cluster Edition. toocreate virtuální sdílené úložiště s s DataKeeper, vytvořte synchronizoval zrcadlení a pak simulovat sdílené úložiště clusteru.

Před instalací softwaru SIOS hello vytvořit uživatele domény hello **DataKeeperSvc**.

> [!NOTE]
> Přidat hello **DataKeeperSvc** uživatele toohello **místního správce** v obou uzlů clusteru.
>
>

tooinstall s DataKeeper:

1.  Nainstalujte hello SIOS software do obou uzlů clusteru.

  ![Instalační program SIOS][sap-ha-guide-figure-3030]

  ![Obrázek 41: První stránku hello s DataKeeper instalace][sap-ha-guide-figure-3031]

  _**Obrázek 41:** první stránku hello s DataKeeper instalace_

2.  V dialogu vyberte hello znázorňuje obrázek 42 vyberte **Ano**.

  ![Obrázek 42: DataKeeper vás upozorní, že služba bude zakázán][sap-ha-guide-figure-3032]

  _**Obrázek 42:** DataKeeper informuje, že služba bude zakázán_

3.  V hello dialogové okno zobrazí obrázek 43, doporučujeme, abyste vybrali **účet domény nebo serveru**.

  ![Obrázek 43: Výběr uživatele s DataKeeper][sap-ha-guide-figure-3033]

  _**Obrázek 43:** výběr uživatele s DataKeeper_

4.  Zadejte uživatelské jméno hello domény účtu a hesla, které jste vytvořili pro DataKeeper s.

  ![Obrázek 44: Zadejte hello domény uživatelské jméno a heslo pro hello s DataKeeper instalace][sap-ha-guide-figure-3034]

  _**Obrázek 44:** zadejte hello domény uživatelské jméno a heslo pro hello s DataKeeper instalace_

5.  Nainstalujte klíč hello licencí pro vaše instance s DataKeeper, jak ukazuje obrázek 45.

  ![45 obrázek: Zadejte klíč licence s DataKeeper][sap-ha-guide-figure-3035]

  _**Obrázek 45:** zadejte klíč DataKeeper s licencí_

6.  Po zobrazení výzvy restartujte hello virtuálního počítače.

#### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>Nastavení pro zařízení s DataKeeper

Po instalaci s DataKeeper oba uzly, je třeba toostart hello konfigurace. cílem Hello hello konfigurace je toohave synchronní data replikace mezi hello další virtuální pevné disky připojené tooeach hello virtuálních počítačů.

1.  Spusťte konfigurační nástroj a hello DataKeeper správy a potom vyberte **připojit Server**. (V obrázku 46, tato možnost je v kroužku červeně.)

  ![46 obrázek: Nástroj Konfigurace a SIOS DataKeeper správy][sap-ha-guide-figure-3036]

  _**Obrázek 46:** s DataKeeper správu a konfiguraci nástroje_

2.  Zadejte hello název nebo adresa TCP/IP hello první uzel hello správu a konfiguraci nástroje by se měly připojit k a v druhém kroku, hello druhého uzlu.

  ![Obrázek 47: Vložení hello název nebo adresa TCP/IP hello první uzel hello správu a konfiguraci nástroje by měl připojit a v druhém kroku, druhý uzel hello][sap-ha-guide-figure-3037]

  _**Obrázek 47:** vložení hello název nebo adresa TCP/IP hello první uzel hello správu a konfiguraci nástroje by měl připojit a v druhém kroku, druhý uzel hello_

3.  Vytvořte úlohu hello replikace mezi dvěma uzly hello.

  ![Obrázek 48: Vytvoření úlohy replikace][sap-ha-guide-figure-3038]

  _**Obrázek 48:** vytvořit úlohu replikace_

  Průvodce vás provede procesem vytvoření úlohy replikace hello.
4.  Zadejte název hello, adresa TCP/IP a svazku hello zdrojový uzel.

  ![Obrázek 49: Definujte hello název úlohy replikace hello][sap-ha-guide-figure-3039]

  _**Obrázek 49:** definovat hello název úlohy replikace hello_

  ![Obrázek 50: Definování hello základní data pro uzel hello, které by se měly hello aktuální zdrojový uzel][sap-ha-guide-figure-3040]

  _**Obrázek 50:** zadat hello základní data pro uzel hello, které by se měly hello aktuální zdrojový uzel_

5.  Zadejte název hello, adresa TCP/IP a svazku hello cílový uzel.

  ![Obrázek 51: Definování hello základní data pro uzel hello, které by měly být cílový uzel aktuální hello][sap-ha-guide-figure-3041]

  _**Obrázek 51:** zadat hello základní data pro uzel hello, které by měly být cílový uzel aktuální hello_

6.  Definujte algoritmy komprese hello. V našem příkladu doporučujeme kompresi hello replikačního streamu. Hlavně v situacích, opakované synchronizace hello komprese hello replikačního streamu výrazně snižuje dobu Opakovaná synchronizace. Všimněte si, že komprese používá prostředky procesoru a paměti RAM hello virtuálního počítače. Jak zvyšuje rychlost komprese hello, takže hello objem prostředků procesoru, které používá. Můžete také upravit toto nastavení později.

7.  Jiné nastavení budete potřebovat toocheck se, zda text hello replikace probíhá synchronně nebo asynchronně. *Pokud budete chránit SAP ASC nebo SCS konfigurace, je nutné použít synchronní replikace*.  

  ![Obrázek 52: Definování podrobnosti k replikaci][sap-ha-guide-figure-3042]

  _**Obrázek 52:** definovat podrobnosti k replikaci_

8.  Zadejte, zda hello svazek, který se replikují úlohou hello replikace by měla být reprezentována tooa konfigurace clusteru Windows Server Failover Clustering jako sdílený disk. Pro konfiguraci SAP ASC nebo SCS hello, vyberte **Ano** tak, aby Windows hello clusteru uvidí hello replikované svazek jako sdílený disk, který můžete použít jako svazek clusteru.

  ![Obrázek 53: Vyberte Ano tooset hello replikovat svazek jako svazek clusteru][sap-ha-guide-figure-3043]

  _**Obrázek 53:** vyberte **Ano** tooset hello replikovat svazek jako svazek clusteru_

  Po vytvoření svazku hello hello DataKeeper správu a konfiguraci nástroje ukazuje, že tuto úlohu hello replikace aktivní.

  ![Obrázek 54: DataKeeper synchronní zrcadlení pro hello SAP ASC nebo SCS sdílené složky disku je aktivní][sap-ha-guide-figure-3044]

  _**Obrázek 54:** DataKeeper synchronní zrcadlení pro hello SAP ASC nebo SCS sdílet disk je aktivní_

  Správce clusteru převzetí služeb při selhání teď hello disk jako disk s DataKeeper ukazuje, jak ukazuje obrázek 55.

  ![Obrázek 55: Správce clusteru převzetí služeb při selhání zobrazuje hello disk, který DataKeeper replikovat][sap-ha-guide-figure-3045]

  _**Obrázek 55:** Správce clusteru převzetí služeb při selhání zobrazuje hello disku tohoto DataKeeper replikovat_

## <a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Instalace systému SAP NetWeaver hello

Instalace databázového systému hello jsme nebude popisují nastavení lišit v závislosti na hello databázového systému systému, které používáte. Ale předpokládáme, že jsou aspekty vysoké dostupnosti s hello databázového systému řešit pomocí hello funkce, které hello různých výrobců databázového systému podpora pro Azure. Například Always On nebo zrcadlení databáze systému SQL Server a Oracle Data Guard pro databáze Oracle. V případě hello, které používáme v tomto článku jsme nepřidali další ochrany toohello databázového systému.

Když různé služby databázového systému interakci s tímto typem Clusterované konfigurace SAP ASC nebo SCS v Azure neexistují žádné zvláštní požadavky.

> [!NOTE]
> postupy instalace Hello SAP NetWeaver ABAP systémů, Java systémy a systémy ABAP + Java jsou téměř shodné. Hello nejdůležitější rozdíl je, že systému SAP ABAP má jednu instanci ASC. Hello systému SAP Java má jednu instanci SCS. Hello systému SAP ABAP + Java má jedna instance ASC a jedna instance SCS spuštěné v hello stejné skupiny clusteru převzetí služeb při selhání Microsoft. Případné rozdíly instalace pro každou SAP NetWeaver instalace zásobníku se výslovně uveden. Můžete předpokládat, že jsou všechny ostatní části hello stejné.  
>
>

### <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Instalace s vysokou dostupností ASC nebo SCS instance SAP

> [!IMPORTANT]
> Ujistěte se, není tooplace stránku souborů na DataKeeper zrcadleny svazky. DataKeeper nepodporuje zrcadlové svazky. Můžete ponechat stránkovacím souboru na dočasné jednotce hello D Azure virtuálního počítače, což je výchozí hello. Pokud není již existuje, přesuňte hello Windows stránky souboru toodrive D virtuálního počítače Azure.
>
>

Instalace s vysokou dostupností ASC nebo SCS instance SAP zahrnuje tyto úlohy:

- Vytváření název virtuálního hostitele pro instance SAP ASC nebo SCS hello v clusteru
- Instalace hello SAP prvním uzlu clusteru
- Úprava profilu SAP hello hello ASC nebo SCS instance
- Přidání port testu
- Otevřete port testu brány firewall systému Windows hello

#### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Vytvořte název virtuálního hostitele pro instance SAP ASC nebo SCS hello v clusteru

1.  Ve správci Windows DNS hello vytvořte záznam DNS pro název virtuálního hostitele hello hello ASC nebo SCS instance.

  > [!IMPORTANT]
  > Hello IP adresu, která přiřadíte název virtuálního hostitele toohello hello ASC nebo SCS instance musí být hello stejné jako hello IP adresa, který jste přiřadili tooAzure nástroj pro vyrovnávání zatížení (**<*SID*> - lb - ASC **).  
  >
  >

  Hello IP adresu hello virtuální SAP ASC nebo SCS název hostitele (**pr1. ASC sap**) hello stejné jako hello IP adresa služby Vyrovnávání zatížení Azure (**pr1-lb Asc**).

  ![Obrázek 56: Definování hello položky DNS pro virtuální název clusteru hello SAP ASC nebo SCS a adresa TCP/IP][sap-ha-guide-figure-3046]

  _**Obrázek 56:** definovat hello položku DNS pro virtuální název clusteru hello SAP ASC nebo SCS adresa TCP/IP_

2.  toodefine hello IP adresu přiřazenou toohello název virtuálního hostitele, vyberte **Správce DNS** > **domény**.

  ![Obrázek 57: Nový virtuální název a adresu TCP/IP pro konfiguraci clusteru SAP ASC nebo SCS][sap-ha-guide-figure-3047]

  _**Obrázek 57:** nový virtuální název a TCP/IP adres pro konfiguraci clusteru SAP ASC nebo SCS_

#### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Nainstalujte hello SAP prvním uzlu clusteru

1.  Spuštění hello první clusteru uzlu možnost na uzlu clusteru A. Například na hello **pr1-ASC-0** hostitele.
2.  tookeep hello výchozí porty pro hello Azure interní nástroj pro vyrovnávání zatížení, vyberte:

  * **Systém ABAP**: **Asc** instance číslo **00**
  * **Java systému**: **SCS** instance číslo **01**
  * **ABAP + Java systému**: **Asc** instance číslo **00** a **SCS** instance číslo **01**

  toouse instance čísla, jiné než 00 pro hello ABAP ASC instance a 01 hello Java SCS instanci, je třeba nejprve toochange hello Azure interní výchozí Vyrovnávání zatížení pravidel, popsaných v [změnu hello ASC nebo SCS výchozí zatížení pravidla pro vyrovnávání zatížení Azure interní hello vyrovnávání][sap-ha-guide-8.9].

Hello další několik úloh nejsou popsané v dokumentaci hello standardní SAP instalace.

> [!NOTE]
> Hello SAP instalace dokumentace popisuje, jak tooinstall hello prvního uzlu clusteru ASC nebo SCS.
>
>

#### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Upravit profil SAP hello hello ASC nebo SCS instance

Je nutné tooadd nový parametr profilu. Parametr profil Hello zabrání připojení mezi SAP pracovní procesy a server hello zařazování zavření po nečinnosti příliš dlouho. Jsme uvedených hello scénář v [přidat položky registru na obou uzlů clusteru instance SAP ASC nebo SCS hello][sap-ha-guide-8.11]. V této části zavedli jsme taky dvě změny toosome základní parametry protokolu TCP/IP připojení. V druhém kroku, je nutné tooset hello zařadit server toosend `keep_alive` signál, aby hello připojení není dosáhl limit nečinnosti Vyrovnávání zatížení Azure interní hello.

toomodify hello SAP profil instance ASC nebo SCS hello:

1.  Přidejte tento profil parametr toohello SAP ASC nebo SCS instance profil:

  ```
  enque/encni/set_so_keepalive = true
  ```
  V našem příkladu hello cesta je:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  Například profil instance SAP SCS toohello a odpovídající cesta:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  změny hello tooapply, restartujte instanci SAP ASC /SCS hello.

#### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Přidat port testu

Vyrovnávání zatížení pro vnitřní hello testu funkce toomake hello celý cluster konfigurace pracovní pomocí vyrovnávání zatížení Azure. Nástroje pro vyrovnávání zatížení Azure interní Hello obvykle distribuuje hello příchozí zatížení rovnoměrně mezi zúčastněných virtuálních počítačů. Ale tato funkce nebude pracovat v některé konfigurace clusteru vzhledem k tomu, že pouze jedna instance je aktivní. Hello další instance je pasivní a nemůže přijímat hello zatížení. Funkce kontroly pomáhá při přiřadí nástroje pro vyrovnávání zatížení Azure interní hello fungovat pouze tooan aktivní instance. Pomocí funkce testu hello hello interní vyrovnávání zátěže může zjistit, které instance jsou aktivní a pak cíle pouze hello instance hello zatížení.

tooadd port testu:

1.  Zkontrolujte aktuální hello **ProbePort** nastavení tak, že spustíte následující příkaz prostředí PowerShell hello. Spouštění ho do jedné hello virtuálních počítačů v konfiguraci clusteru hello.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  Definujte port testu. Hello výchozí číslo portu testu je **0**. V našem příkladu používáme port testu **62000**.

  ![Obrázek 58: port testu konfigurace clusteru hello je 0. ve výchozím nastavení][sap-ha-guide-figure-3048]

  _**Obrázek 58:** hello výchozí clusteru Konfigurace testu port je 0._

  číslo portu Hello je definována v šablonách SAP Azure Resource Manager. Můžete přiřadit číslo portu hello v prostředí PowerShell.

  tooset novou hodnotu ProbePort hello  **SAP <*SID*> IP ** prostředek clusteru, spusťte následující skript prostředí PowerShell hello. Aktualizujte hello proměnné prostředí PowerShell pro vaše prostředí. Po spuštění skriptu hello budete výzvami toorestart hello SAP clusteru skupiny tooactivate hello změny.

  ```PowerShell
  $SAPSID = "PR1"      # SAP <SID>
  $ProbePort = 62000   # ProbePort of hello Azure Internal Load Balancer

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
  Write-Host "Current probe port property of hello SAP cluster resource '$SAPIPresourceName' is '$OldProbePort'." -ForegroundColor Cyan
  Write-Host
  Write-Host "Setting hello new probe port property of hello SAP cluster resource '$SAPIPresourceName' too'$ProbePort' ..." -ForegroundColor Cyan
  Write-Host

  $var | Set-ClusterParameter -Multiple @{"Address"=$IPAddress;"ProbePort"=$ProbePort;"Subnetmask"=$SubnetMask;"Network"=$NetworkName;"OverrideAddressMatch"=$OverrideAddressMatch;"EnableDhcp"=$EnableDhcp}

  Write-Host

  $ActivateChanges = Read-Host "Do you want tootake restart SAP cluster role '$SAPClusterRoleName', tooactivate hello changes (yes/no)?"

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

  Po přepnutí hello  **SAP <*SID*> ** clusteru role online, ověřte, že **ProbePort** nastavena toohello novou hodnotu.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![Obrázek 59: Probe hello clusteru port po nastavení novou hodnotu hello][sap-ha-guide-figure-3049]

  _**Obrázek 59:** testu hello clusteru port po nastavení novou hodnotu hello_

#### <a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Otevřete port testu brány firewall systému Windows hello

Je nutné tooopen testu port brány firewall systému Windows na obou uzlů clusteru. Použijte následující tooopen skript port testu brány firewall systému Windows hello. Aktualizujte hello proměnné prostředí PowerShell pro vaše prostředí.

  ```PowerShell
  $ProbePort = 62000   # ProbePort of hello Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

Hello **ProbePort** je nastaven příliš**62000**. Teď můžete přístup hello sdílené složky  **\\\ascsha-clsap\sapmnt** z jiných hostitelů, například jako z **ascsha dbas**.

### <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Instalovat instanci databáze hello

databáze hello tooinstall instance, postupujte podle procesu hello popsané v dokumentaci k instalaci SAP hello.

### <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Nainstalujte hello druhého uzlu clusteru

tooinstall hello druhý cluster, postupujte podle kroků hello v hello Instalační příručka.

### <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Změňte typ spouštění hello instance služby SAP YBRAT Windows hello

Změňte typ spouštění hello hello služba systému Windows YBRAT SAP příliš**automaticky (zpožděné spuštění)** na obou uzlů clusteru.

![Obrázek 60: Změnit typ hello service pro automatické toodelayed instance SAP YBRAT hello][sap-ha-guide-figure-3050]

_**Obrázek 60:** změnit typ hello service pro automatické toodelayed instance SAP YBRAT hello_

### <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>Nainstalujte hello primární Server aplikace SAP

Instalovat instanci serveru primární aplikace (Pa) hello <*SID*> - di - 0 hello virtuálního počítače, který jste určili toohost hello Pa adresy. Neexistují žádné závislosti na Azure nebo DataKeeper specifická nastavení.

### <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>Nainstalujte hello SAP další aplikační Server

Nainstalujte SAP serveru pro další aplikace (AAS) na všechny hello virtuální počítače, které jste označili toohost instance aplikačního serveru SAP. Například na <*SID*> - di - 1 příliš <*SID*> - di -&lt;n&gt;.

> [!NOTE]
> To dokončí instalaci hello systému SAP NetWeaver vysokou dostupnost. Pokračujte dále testování převzetí služeb při selhání.
>


## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Testovací převzetí služeb při selhání hello SAP ASC nebo SCS instance a SIOS replikace
Je snadno tootest a monitorování SAP ASC nebo SCS instance převzetí služeb při selhání a SIOS replikaci disku pomocí Správce clusteru převzetí služeb při selhání a s DataKeeper správu a konfiguraci nástroje hello.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>V uzlu clusteru A je spuštěna instance SAP ASC nebo SCS

Hello **SAP PR1** skupina clusteru běží na uzlu clusteru A. Například na **pr1-ASC-0**. Přiřadit hello sdíleného disku S, který je součástí hello **SAP PR1** skupiny, a kterou instanci ASC nebo SCS hello používá, A. toocluster uzlu clusteru

![Obrázek 61: Správce clusteru převzetí služeb při selhání: Skupina clusteru SAP < SID > hello běží na uzlu clusteru A][sap-ha-guide-figure-5000]

_**Obrázek 61:** Správce clusteru převzetí služeb při selhání: hello SAP <*SID*> Skupina clusteru běží na uzlu clusteru A_

V hello s DataKeeper správu a konfigurační nástroj se zobrazí tento hello sdílený disk, který synchronně replikace dat z hello zdrojového svazku jednotky S na uzlu clusteru toohello cílového svazku jednotky S na uzlu clusteru B. Například se replikují z **pr1-ASC-0 [10.0.0.40]** příliš**pr1-ASC-1 [10.0.0.41]**.

![Obrázek 62: V DataKeeper s replikovat místní svazek hello z uzlu clusteru toocluster uzlu B][sap-ha-guide-figure-5001]

_**Obrázek 62:** v DataKeeper s replikovat místní svazek hello z uzlu clusteru toocluster uzlu B_

### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Převzetí služeb při selhání uzlu A toonode B

1.  Vyberte jednu z těchto možností tooinitiate převzetí služeb při selhání hello SAP <*SID*> Skupina clusteru z uzlu clusteru uzel A toocluster B:
  - Pomocí Správce clusteru převzetí služeb při selhání  
  - Pomocí prostředí PowerShell pro Cluster převzetí služeb při selhání

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  Restartovat A uzlu clusteru v rámci operačního systému hosta Windows hello (tím se vyvolá automatické převzetí služeb při selhání systému hello SAP <*SID*> Skupina clusteru z uzlu A toonode B).  
3.  Restartovat A uzel clusteru z hello portálu Azure (tím se vyvolá automatické převzetí služeb při selhání systému hello SAP <*SID*> Skupina clusteru z uzlu A toonode B).  
4.  Restartujte počítač A uzlu clusteru pomocí prostředí Azure PowerShell (tím se vyvolá automatické převzetí služeb při selhání systému hello SAP <*SID*> Skupina clusteru z uzlu A toonode B).

  Po převzetí služeb při selhání, hello SAP <*SID*> Skupina clusteru běží na uzlu clusteru B. Například je spuštěn na **pr1-ASC-1**.

  ![Obrázek 63: Ve Správci clusteru převzetí služeb při selhání skupiny clusteru SAP < SID > hello běží na uzlu clusteru B][sap-ha-guide-figure-5002]

  _**Obrázek 63**: V převzetí služeb při selhání modulu Správce clusteru hello SAP <*SID*> Skupina clusteru běží na uzlu clusteru B_

  Hello sdílený disk je teď připojený v clusteru je uzel B. s DataKeeper replikaci dat ze zdrojového svazku jednotky S v clusteru uzlu B tootarget svazku jednotky S na uzlu clusteru A. Například je replikace z **pr1-ASC-1 [10.0.0.41]** příliš**pr1-ASC-0 [10.0.0.40]**.

  ![Obrázek 64: SIOS DataKeeper replikuje hello místního svazku z clusteru uzlu B toocluster uzlu A][sap-ha-guide-figure-5003]

  _**Obrázek 64:** s DataKeeper replikuje hello místního svazku z uzlu clusteru, toocluster uzlu B A_
