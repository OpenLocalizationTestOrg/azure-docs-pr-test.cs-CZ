---
title: "Začínáme s SAP na virtuálních počítačích Windows v Azure | Microsoft Docs"
description: "Další informace o řešení SAP běžících na virtuálních počítačích (VM) v Microsoft Azure"
services: virtual-machines-windows
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e714c47-add3-44c5-a6c8-9d26472ddcc4
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/08/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ebe420ea5105fb15e42ff32ad7e8d9d8b27d8b06
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-sap-on-azure-windows-virtual-machines-vms"></a><span data-ttu-id="67df4-103">Pomocí SAP na virtuálních počítačích s Azure Windows (VM)</span><span class="sxs-lookup"><span data-stu-id="67df4-103">Using SAP on Azure Windows Virtual Machines (VMs)</span></span>
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

[azure-cli]:../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../azure-subscription-service-limits.md#subscription

[dbms-guide]:../virtual-machines-windows-sap-dbms-guide.md 
[dbms-guide-2.1]:../virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f 
[dbms-guide-2.2]:../virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:../virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 
[dbms-guide-2]:../virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 
[dbms-guide-3]:../virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 
[dbms-guide-5.5.1]:../virtual-machines-windows-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 
[dbms-guide-5.5.2]:../virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:../virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:../virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:../virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:../virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 
[dbms-guide-8.4.2]:../virtual-machines-windows-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:../virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:../virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
[dbms-guide-900-sap-cache-server-on-premises]:../virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:sap-deployment-guide.md  
[deployment-guide-2.2]:sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 
[deployment-guide-3.1.2]:sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc 
[deployment-guide-4.4.2]:sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 
[deployment-guide-4.4]:sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d 
[deployment-guide-4.5.1]:sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 
[deployment-guide-4.5.2]:sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f 
[deployment-guide-4.5]:sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca 
[deployment-guide-5.1]:sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b 
[deployment-guide-configure-proxy]:sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d 
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b 

[deploy-template-cli]:../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-windows-../linux/classic/sap-get-started.md
[getting-started-dbms]:virtual-machines-windows-../linux/classic/sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-windows-../linux/classic/sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-windows-../linux/classic/sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-../linux/classic/sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-../linux/classic/sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-../linux/classic/sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-../linux/classic/sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-../linux/classic/sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-../linux/classic/sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:sap-high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:sap-planning-guide.md  
[planning-guide-1.2]:sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11.4.1]:sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
[planning-guide-2.1]:sap-planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10
[planning-guide-3.1]:sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a 
[planning-guide-3.2.1]:sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 
[planning-guide-3.2.2]:sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 
[planning-guide-3.2.3]:sap-planning-guide.md#18810088-f9be-4c97-958a-27996255c665
[planning-guide-3.2]:sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77
[planning-guide-3.3.2]:sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92
[planning-guide-5.1.1]:sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53
[planning-guide-5.1.2]:sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c
[planning-guide-5.2.1]:sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef
[planning-guide-5.2.2]:sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3
[planning-guide-5.2]:sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7
[planning-guide-5.3.1]:sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 
[planning-guide-5.3.2]:sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a
[planning-guide-5.4.2]:sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 
[planning-guide-5.5.1]:sap-planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646
[planning-guide-5.5.3]:sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d 
[planning-guide-7.1]:sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30
[planning-guide-7]:sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14
[planning-guide-9.1]:sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3
[planning-guide-azure-premium-storage]:sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92

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
[planning-guide-microsoft-azure-networking]:sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd 
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[powershell-install-configure]:/powershell/azureps-cmdlets-docs
[resource-group-authoring-templates]:../../resource-group-authoring-templates.md
[resource-group-overview]:../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam 
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
[virtual-machines-linux-cli-deploy-templates]:../linux/cli-deploy-templates.md 
[virtual-machines-deploy-rmtemplates-powershell]:../virtual-machines-windows-ps-manage.md 
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

<span data-ttu-id="67df4-104">Výběrem Microsoft Azure jako váš partner připravené cloudu SAP, bude možné spolehlivě spuštění kritické SAP úlohy na scaaleable předpisy a platformy prokázanou enterprise.</span><span class="sxs-lookup"><span data-stu-id="67df4-104">By choosing Microsoft Azure as your SAP ready cloud partner, you will be able to reliably run your mission critical SAP workloads on a scaaleable, compliant, and enterprise-proven platform.</span></span>  <span data-ttu-id="67df4-105">Získejte scaleability, flexability a finanční úspoře Azure.</span><span class="sxs-lookup"><span data-stu-id="67df4-105">Get the scaleability, flexability, and cost savings of Azure.</span></span> <span data-ttu-id="67df4-106">S rozšířené spolupráci mezi společností Microsoft a SAP můžete spustit aplikace SAP napříč scénáře vývoje/testování a provozním ve službě azure - a plně podporovat.</span><span class="sxs-lookup"><span data-stu-id="67df4-106">With the expanded partnership between Microsoft and SAP, you can run SAP applications across dev/test and production scenarios in azure - and be fully supported.</span></span> <span data-ttu-id="67df4-107">Máme všechno, co potřebujete, od SAP NetWeaveru po SAP S4/HANA, od Linuxu po Windows, od SAP HANA po SQL.</span><span class="sxs-lookup"><span data-stu-id="67df4-107">From SAP NetWeaver to SAP S4/HANA, Linux to Windows, SAP HANA to SQL, we have you covered.</span></span> 

<span data-ttu-id="67df4-108">Služby virtuálního počítače Microsoft Azure, a SAP HANA s instancemi Azure velký společnost Microsoft nabízí komplexní platformu infrastruktura jako služba (IaaS).</span><span class="sxs-lookup"><span data-stu-id="67df4-108">With Microsoft Azure virtual machine services, and SAP HANA on Azure large instances, Microsoft offers a comprehensive Infrastructure As A Service (IaaS) platform.</span></span> <span data-ttu-id="67df4-109">Protože řešení široké rozsahu SAP jsou podporovány v Azure, a to "získávání Začínáme dokumentu bude sloužit jako obsah pro naše aktuální sadu SAP dokumenty.</span><span class="sxs-lookup"><span data-stu-id="67df4-109">Because a wide range of SAP solutions are supported on Azure, this "getting started document will serve as a Table of Contents for our current set of SAP documents.</span></span> <span data-ttu-id="67df4-110">Jako další produkty se přidají do našich knihovny dokumentů - zobrazí se zde přidat.</span><span class="sxs-lookup"><span data-stu-id="67df4-110">As more titles get added to our document library - you will see them added here.</span></span> 

## <a name="sap-hana-certifications-on-microsoft-azure"></a><span data-ttu-id="67df4-111">SAP HANA certifikátů v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="67df4-111">SAP HANA Certifications on Microsoft Azure</span></span>
| <span data-ttu-id="67df4-112">Produkt SAP</span><span class="sxs-lookup"><span data-stu-id="67df4-112">SAP Product</span></span> | <span data-ttu-id="67df4-113">Podporovaný operační systém</span><span class="sxs-lookup"><span data-stu-id="67df4-113">Supported OS</span></span> | <span data-ttu-id="67df4-114">Nabídky Azure</span><span class="sxs-lookup"><span data-stu-id="67df4-114">Azure Offerings</span></span> |
| --- | --- | --- |
| <span data-ttu-id="67df4-115">SAP HANA Developer Edition (včetně klientský software HANA skládá z SQLODBC, pouze ODBO – Windows, rozhraní ODBC, ovladače JDBC, HANA studio a HANA databáze)</span><span class="sxs-lookup"><span data-stu-id="67df4-115">SAP HANA Developer Edition (including the HANA client software comprised of SQLODBC, ODBO-Windows only, ODBC, JDBC drivers, HANA studio, and HANA database)</span></span> |<span data-ttu-id="67df4-116">Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise</span><span class="sxs-lookup"><span data-stu-id="67df4-116">Red Hat Enterprise Linux, SUSE Linux Enterprise</span></span> |<span data-ttu-id="67df4-117">A7, A8</span><span class="sxs-lookup"><span data-stu-id="67df4-117">A7, A8</span></span> |
| <span data-ttu-id="67df4-118">MHANA jeden</span><span class="sxs-lookup"><span data-stu-id="67df4-118">MHANA One</span></span> |<span data-ttu-id="67df4-119">Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise</span><span class="sxs-lookup"><span data-stu-id="67df4-119">Red Hat Enterprise Linux, SUSE Linux Enterprise</span></span> |<span data-ttu-id="67df4-120">DS14_v2 (při všeobecné dostupnosti)</span><span class="sxs-lookup"><span data-stu-id="67df4-120">DS14_v2 (upon general availability)</span></span> |
| <span data-ttu-id="67df4-121">SAP S/4HANA</span><span class="sxs-lookup"><span data-stu-id="67df4-121">SAP S/4HANA</span></span> |<span data-ttu-id="67df4-122">Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise</span><span class="sxs-lookup"><span data-stu-id="67df4-122">Red Hat Enterprise Linux, SUSE Linux Enterprise</span></span> |<span data-ttu-id="67df4-123">Řízené dostupnost pro GS5, SAP HANA v Azure (velké instance)</span><span class="sxs-lookup"><span data-stu-id="67df4-123">Controlled Availability for GS5, SAP HANA on Azure (Large instances)</span></span> |
| <span data-ttu-id="67df4-124">Suite on HANA, OLTP</span><span class="sxs-lookup"><span data-stu-id="67df4-124">Suite on HANA, OLTP</span></span> |<span data-ttu-id="67df4-125">Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise</span><span class="sxs-lookup"><span data-stu-id="67df4-125">Red Hat Enterprise Linux, SUSE Linux Enterprise</span></span> |<span data-ttu-id="67df4-126">SAP HANA v Azure (velké instance)</span><span class="sxs-lookup"><span data-stu-id="67df4-126">SAP HANA on Azure (Large instances)</span></span> |
| <span data-ttu-id="67df4-127">HANA Enterprise for BW, OLAP</span><span class="sxs-lookup"><span data-stu-id="67df4-127">HANA Enterprise for BW, OLAP</span></span> |<span data-ttu-id="67df4-128">Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise</span><span class="sxs-lookup"><span data-stu-id="67df4-128">Red Hat Enterprise Linux, SUSE Linux Enterprise</span></span> |<span data-ttu-id="67df4-129">GS5 pro jeden uzel nasazení SAP HANA v Azure (velké instance)</span><span class="sxs-lookup"><span data-stu-id="67df4-129">GS5 for single node deployments, SAP HANA on Azure (Large instances)</span></span> |
| <span data-ttu-id="67df4-130">SAP BW/4HANA</span><span class="sxs-lookup"><span data-stu-id="67df4-130">SAP BW/4HANA</span></span> |<span data-ttu-id="67df4-131">Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise</span><span class="sxs-lookup"><span data-stu-id="67df4-131">Red Hat Enterprise Linux, SUSE Linux Enterprise</span></span> |<span data-ttu-id="67df4-132">GS5 pro jeden uzel nasazení SAP HANA v Azure (velké instance)</span><span class="sxs-lookup"><span data-stu-id="67df4-132">GS5 for single node deployments, SAP HANA on Azure (Large instances)</span></span> |

## <a name="sap-netweaver-certifications"></a><span data-ttu-id="67df4-133">SAP NetWeaver certifikace</span><span class="sxs-lookup"><span data-stu-id="67df4-133">SAP NetWeaver Certifications</span></span>
<span data-ttu-id="67df4-134">Microsoft Azure má certifikaci pro následující produkty SAP, se zárukou plné podpory od Microsoftu i SAPu.</span><span class="sxs-lookup"><span data-stu-id="67df4-134">Microsoft Azure is certified for the following SAP products, with full support from Microsoft and SAP.</span></span>

| <span data-ttu-id="67df4-135">Produkt SAP</span><span class="sxs-lookup"><span data-stu-id="67df4-135">SAP Product</span></span> | <span data-ttu-id="67df4-136">Hostovaného operačního systému</span><span class="sxs-lookup"><span data-stu-id="67df4-136">Guest OS</span></span> | <span data-ttu-id="67df4-137">RDBMS</span><span class="sxs-lookup"><span data-stu-id="67df4-137">RDBMS</span></span> | <span data-ttu-id="67df4-138">Typy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="67df4-138">Virtual Machine Types</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="67df4-139">SAP Business Suite Software</span><span class="sxs-lookup"><span data-stu-id="67df4-139">SAP Business Suite Software</span></span> |<span data-ttu-id="67df4-140">Windows, operačního systému SUSE Linux Enterprise, Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="67df4-140">Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux</span></span> |<span data-ttu-id="67df4-141">SQL Server, Oracle (jenom Windows), DB2, SAP App Service Environment</span><span class="sxs-lookup"><span data-stu-id="67df4-141">SQL Server, Oracle (Windows only), DB2, SAP ASE</span></span> |<span data-ttu-id="67df4-142">A5 až A11, D11 až D14, DS11 až DS14, GS1 až GS5</span><span class="sxs-lookup"><span data-stu-id="67df4-142">A5 to A11, D11 to D14, DS11 to DS14, GS1 to GS5</span></span> |
| <span data-ttu-id="67df4-143">SAP Business All-in-One</span><span class="sxs-lookup"><span data-stu-id="67df4-143">SAP Business All-in-One</span></span> |<span data-ttu-id="67df4-144">Windows, operačního systému SUSE Linux Enterprise, Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="67df4-144">Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux</span></span> |<span data-ttu-id="67df4-145">SQL Server, Oracle (jenom Windows), DB2, SAP App Service Environment</span><span class="sxs-lookup"><span data-stu-id="67df4-145">SQL Server, Oracle (Windows only), DB2, SAP ASE</span></span> |<span data-ttu-id="67df4-146">A5 až A11, D11 až D14, DS11 až DS14, GS1 až GS5</span><span class="sxs-lookup"><span data-stu-id="67df4-146">A5 to A11, D11 to D14, DS11 to DS14, GS1 to GS5</span></span> |
| <span data-ttu-id="67df4-147">SAP BusinessObjects BI</span><span class="sxs-lookup"><span data-stu-id="67df4-147">SAP BusinessObjects BI</span></span> |<span data-ttu-id="67df4-148">Windows</span><span class="sxs-lookup"><span data-stu-id="67df4-148">Windows</span></span> |<span data-ttu-id="67df4-149">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="67df4-149">N/A</span></span> |<span data-ttu-id="67df4-150">A5 až A11, D11 až D14, DS11 až DS14, GS1 až GS5</span><span class="sxs-lookup"><span data-stu-id="67df4-150">A5 to A11, D11 to D14, DS11 to DS14, GS1 to GS5</span></span> |
| <span data-ttu-id="67df4-151">SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="67df4-151">SAP NetWeaver</span></span> |<span data-ttu-id="67df4-152">Windows, operačního systému SUSE Linux Enterprise, Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="67df4-152">Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux</span></span> |<span data-ttu-id="67df4-153">SQL Server, Oracle (jenom Windows), DB2, SAP App Service Environment</span><span class="sxs-lookup"><span data-stu-id="67df4-153">SQL Server, Oracle (Windows only), DB2, SAP ASE</span></span> |<span data-ttu-id="67df4-154">A5 až A11, D11 až D14, DS11 až DS14, GS1 až GS5</span><span class="sxs-lookup"><span data-stu-id="67df4-154">A5 to A11, D11 to D14, DS11 to DS14, GS1 to GS5</span></span> |

## <a name="getting-started-with-sap-hana-on-azure"></a><span data-ttu-id="67df4-155">Začínáme s SAP HANA v Azure</span><span class="sxs-lookup"><span data-stu-id="67df4-155">Getting Started with SAP HANA on Azure</span></span>
<span data-ttu-id="67df4-156">Title: Průvodce rychlým zahájením pro ruční instalaci sady SAP HANA na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="67df4-156">Title: Quickstart guide for manual installation of SAP HANA on Azure VMs</span></span>

<span data-ttu-id="67df4-157">Souhrn: Tento průvodce rychlým zahájením pomůže vytvořit jednoduchou SAP HANA prototypu nebo ukázku systém na virtuálních počítačích Azure při ruční instalaci SAP NetWeaver 7.5 a SAP HANA SP12.</span><span class="sxs-lookup"><span data-stu-id="67df4-157">Summary: This quickstart guide will help to set up a single-instance SAP HANA prototype/demo system on Azure VMs by a manual installation of SAP NetWeaver 7.5 and SAP HANA SP12.</span></span> <span data-ttu-id="67df4-158">V Průvodci se předpokládá, že je čtečka seznámili se základy Azure IaaS, jako je nasazení virtuálních počítačů nebo virtuální sítě prostřednictvím portálu Azure nebo Powershell nebo rozhraní příkazového řádku včetně možnost použít šablony json.</span><span class="sxs-lookup"><span data-stu-id="67df4-158">The guide presumes that the reader is familiar with Azure IaaS basics like how to deploy virtual machines or virtual networks either via the Azure portal or Powershell/CLI including the option to use json templates.</span></span> <span data-ttu-id="67df4-159">Kromě toho se očekává, že čtečka je obeznámeni s SAP HANA, SAP NetWeaver a jak ji nainstalovat místně.</span><span class="sxs-lookup"><span data-stu-id="67df4-159">Furthermore it's expected that the reader is familiar with SAP HANA, SAP NetWeaver and how to install it on-premises.</span></span>

<span data-ttu-id="67df4-160">Aktualizované: Září 2016</span><span class="sxs-lookup"><span data-stu-id="67df4-160">Updated: September 2016</span></span>

[<span data-ttu-id="67df4-161">Tato příručka naleznete zde</span><span class="sxs-lookup"><span data-stu-id="67df4-161">This guide can be found here</span></span>](../virtual-machines-linux-sap-hana-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quickstart-guide-for-netweaver-on-suse-linux-on-azure"></a><span data-ttu-id="67df4-162">Průvodce rychlým zahájením pro NetWeaver na SUSE Linux na Azure</span><span class="sxs-lookup"><span data-stu-id="67df4-162">Quickstart Guide for NetWeaver on SUSE Linux on Azure</span></span>
<span data-ttu-id="67df4-163">Title: Testování SAP NetWeaver na virtuálních počítačích Microsoft Azure SUSE Linux</span><span class="sxs-lookup"><span data-stu-id="67df4-163">Title: Testing SAP NetWeaver on Microsoft Azure SUSE Linux VMs</span></span> 

<span data-ttu-id="67df4-164">Souhrn: Tento článek popisuje různé co je třeba zvážit při spuštěné SAP NetWeaver v Microsoft Azure SUSE Linux virtuální počítače (VM).</span><span class="sxs-lookup"><span data-stu-id="67df4-164">Summary: This article describes various things to consider when you're running SAP NetWeaver on Microsoft Azure SUSE Linux virtual machines (VMs).</span></span> <span data-ttu-id="67df4-165">Od 19 2016 může SAP NetWeaver oficiálně podporované v SUSE virtuální počítače s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="67df4-165">As of May 19 2016 SAP NetWeaver is officially supported on SUSE Linux VMs on Azure.</span></span> <span data-ttu-id="67df4-166">Všechny podrobnosti týkající se verze Linux, verze SAP jádra a tak dále najdete v 1928533 Poznámka SAP "SAP aplikace v Azure: podporované produkty a typy virtuálních počítačů Azure".</span><span class="sxs-lookup"><span data-stu-id="67df4-166">All details regarding Linux versions, SAP kernel versions and so on can be found in SAP Note 1928533 "SAP Applications on Azure: Supported Products and Azure VM types".</span></span>

<span data-ttu-id="67df4-167">Aktualizované: Září 2016</span><span class="sxs-lookup"><span data-stu-id="67df4-167">Updated: September 2016</span></span>

[<span data-ttu-id="67df4-168">Tato příručka naleznete zde</span><span class="sxs-lookup"><span data-stu-id="67df4-168">This guide can be found here</span></span>](../virtual-machines-linux-sap-on-suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="deploying-sap-ides-ehp7-sp3-for-sap-erp-60-on-microsoft-azure"></a><span data-ttu-id="67df4-169">Nasazení SAP integrovaného vývojového prostředí EHP7 SP3 pro SAP ERP 6.0 na platformě Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="67df4-169">Deploying SAP IDES EHP7 SP3 for SAP ERP 6.0 on Microsoft Azure</span></span>
<span data-ttu-id="67df4-170">Title: Průvodce rychlým zahájením pro ruční instalaci sady SAP HANA na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="67df4-170">Title: Quickstart guide for manual installation of SAP HANA on Azure VMs</span></span>

<span data-ttu-id="67df4-171">Souhrn: Tento článek popisuje postup nasazení SAP integrovaného vývojového prostředí s SQL Server a operačního systému Windows v Microsoft Azure prostřednictvím SAP cloudu zařízení knihovna 3.0.</span><span class="sxs-lookup"><span data-stu-id="67df4-171">Summary: This article describes how to deploy SAP IDES running with SQL Server and Windows OS on Microsoft Azure via SAP Cloud Appliance Library 3.0.</span></span> 

<span data-ttu-id="67df4-172">Aktualizované: Září 2016</span><span class="sxs-lookup"><span data-stu-id="67df4-172">Updated: September 2016</span></span>

[<span data-ttu-id="67df4-173">Tato příručka naleznete zde</span><span class="sxs-lookup"><span data-stu-id="67df4-173">This guide can be found here</span></span>](sap-cal-ides-erp6-ehp7-sp3-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <span data-ttu-id="67df4-174"><a name="3da0389e-708b-4e82-b2a2-e92f132df89c"></a>Plánování a implementace</span><span class="sxs-lookup"><span data-stu-id="67df4-174"><a name="3da0389e-708b-4e82-b2a2-e92f132df89c"></a>Planning and Implementation</span></span>
<span data-ttu-id="67df4-175">Title: SAP NetWeaver na virtuálních počítačích Azure (VM) – plánování a implementace Průvodce</span><span class="sxs-lookup"><span data-stu-id="67df4-175">Title: SAP NetWeaver on Azure Virtual Machines (VMs) – Planning and Implementation Guide</span></span>

<span data-ttu-id="67df4-176">Shrnutí: Toto je první dokument, po kterém byste měli sáhnout, pokud zvažujete používání systému SAP NetWeaver ve službě Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="67df4-176">Summary: This is the paper to start with if you are thinking about running SAP NetWeaver in Azure Virtual Machines.</span></span> <span data-ttu-id="67df4-177">Tato příručka pro plánování a implementaci vám pomůže vyhodnotit, jestli je možné existující nebo plánovaný systém SAP NetWeaver nasadit v prostředí Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="67df4-177">This planning and implementation guide will help you evaluate whether an existing or planned SAP NetWeaver-based system can be deployed to an Azure Virtual Machines environment.</span></span> <span data-ttu-id="67df4-178">Popisuje různé scénáře nasazení systému SAP NetWeaver a zahrnuje konfigurace SAP specifické pro Azure.</span><span class="sxs-lookup"><span data-stu-id="67df4-178">It covers multiple SAP NetWeaver deployment scenarios, and includes SAP configurations that are specific to Azure.</span></span> <span data-ttu-id="67df4-179">Dokument uvádí a popisuje všechny informace související s konfigurací, které potřebujete na straně SAP/Azure k vytvoření hybridního prostředí SAP.</span><span class="sxs-lookup"><span data-stu-id="67df4-179">The paper lists and describes all the necessary configuration information you’ll need on the SAP/Azure side to run a hybrid SAP landscape.</span></span> <span data-ttu-id="67df4-180">Popsána jsou také opatření, které můžete provést k zajištění vysoké dostupnosti systému SAP NetWeaver na IaaS.</span><span class="sxs-lookup"><span data-stu-id="67df4-180">Measures you can take to ensure high availability of SAP NetWeaver-based systems on IaaS are also covered.</span></span>

<span data-ttu-id="67df4-181">Aktualizované: Srpna 2016</span><span class="sxs-lookup"><span data-stu-id="67df4-181">Updated: August 2016</span></span>

<span data-ttu-id="67df4-182">[Tato příručka naleznete zde][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="67df4-182">[This guide can be found here][planning-guide]</span></span>

## <span data-ttu-id="67df4-183"><a name="6aadadd2-76b5-46d8-8713-e8d63630e955"></a>Nasazení</span><span class="sxs-lookup"><span data-stu-id="67df4-183"><a name="6aadadd2-76b5-46d8-8713-e8d63630e955"></a>Deployment</span></span>
<span data-ttu-id="67df4-184">Title: SAP NetWeaver na virtuálních počítačích Azure (VM) – Příručka pro nasazení</span><span class="sxs-lookup"><span data-stu-id="67df4-184">Title: SAP NetWeaver on Azure Virtual Machines (VMs) – Deployment Guide</span></span>

<span data-ttu-id="67df4-185">Shrnutí: Tento dokument poskytuje postupy a pokyny pro nasazení softwaru SAP NetWeaver na virtuální počítače v prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="67df4-185">Summary: This document provides procedural guidance for deploying SAP NetWeaver software to virtual machines in Azure.</span></span> <span data-ttu-id="67df4-186">Zaměřuje se na tři konkrétní scénáře nasazení s důrazem na použití rozšíření Azure Monitoring Extensions pro SAP, včetně doporučení pro řešení potíží s tímto rozšířením.</span><span class="sxs-lookup"><span data-stu-id="67df4-186">This paper focuses on three specific deployment scenarios, with an emphasis on enabling the Azure Monitoring Extensions for SAP, including troubleshooting recommendations for the Azure Monitoring Extensions for SAP.</span></span> <span data-ttu-id="67df4-187">Tento dokument předpokládá, že jste se seznámili s Příručkou pro plánování a implementaci.</span><span class="sxs-lookup"><span data-stu-id="67df4-187">This paper assumes that you’ve read the planning and implementation guide.</span></span>

<span data-ttu-id="67df4-188">Aktualizované: Prosinec 2016</span><span class="sxs-lookup"><span data-stu-id="67df4-188">Updated: December 2016</span></span>

<span data-ttu-id="67df4-189">[Tato příručka naleznete zde][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="67df4-189">[This guide can be found here][deployment-guide]</span></span>

## <span data-ttu-id="67df4-190"><a name="1343ffe1-8021-4ce6-a08d-3a1553a4db82"></a>Průvodce nasazením databázového systému</span><span class="sxs-lookup"><span data-stu-id="67df4-190"><a name="1343ffe1-8021-4ce6-a08d-3a1553a4db82"></a>DBMS Deployment Guide</span></span>
<span data-ttu-id="67df4-191">Title: SAP NetWeaver na virtuálních počítačích Azure (VM) – Průvodce nasazením databázového systému</span><span class="sxs-lookup"><span data-stu-id="67df4-191">Title: SAP NetWeaver on Azure Virtual Machines (VMs) – DBMS Deployment Guide</span></span>

<span data-ttu-id="67df4-192">Shrnutí: Tento dokument popisuje důležité aspekty plánování a implementace systémů DBMS, které by měly běžet spolu se systémem SAP.</span><span class="sxs-lookup"><span data-stu-id="67df4-192">Summary: This paper covers planning and implementation considerations for the DBMS systems that should run in conjunction with SAP.</span></span> <span data-ttu-id="67df4-193">V první části jsou uvedeny obecné pokyny.</span><span class="sxs-lookup"><span data-stu-id="67df4-193">In the first part, general considerations are listed and presented.</span></span> <span data-ttu-id="67df4-194">Další části se pak týkají nasazení různých DBMS v Azure, které SAP podporuje.</span><span class="sxs-lookup"><span data-stu-id="67df4-194">The following parts of the paper relate to deployments of different DBMS in Azure that are supported by SAP.</span></span> <span data-ttu-id="67df4-195">Dostupné systémy DBMS jsou SQL Server, SAP ASE a Oracle.</span><span class="sxs-lookup"><span data-stu-id="67df4-195">Different DBMS presented are SQL Server, SAP ASE and Oracle.</span></span> <span data-ttu-id="67df4-196">V těchto specifických částech jsou probrány aspekty, které byste měli vzít v úvahu při použití systému SAP v prostředí Azure spolu s jednotlivými systémy DBMS.</span><span class="sxs-lookup"><span data-stu-id="67df4-196">In those specific parts considerations you have to account for when you are running SAP systems on Azure in conjunction with those DBMS are discussed.</span></span> <span data-ttu-id="67df4-197">Jsou popsány oblasti jako metody zálohování nebo zvýšení dostupnosti, podporované jednotlivými systémy DBMS v Azure, které je možné s aplikacemi SAP použít.</span><span class="sxs-lookup"><span data-stu-id="67df4-197">Subjects like backup and high availability methods that are supported by the different DBMS on Azure are presented for the usage with SAP applications.</span></span>

<span data-ttu-id="67df4-198">Aktualizované: Srpna 2016</span><span class="sxs-lookup"><span data-stu-id="67df4-198">Updated: August 2016</span></span>

<span data-ttu-id="67df4-199">[Tato příručka naleznete zde][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="67df4-199">[This guide can be found here][dbms-guide]</span></span>

## <span data-ttu-id="67df4-200"><a name="63dab028-2c4f-4636-8f99-90bbb264eaba"></a>Příručka pro nasazení vysoké dostupnosti</span><span class="sxs-lookup"><span data-stu-id="67df4-200"><a name="63dab028-2c4f-4636-8f99-90bbb264eaba"></a>High Availability Deployment Guide</span></span>
<span data-ttu-id="67df4-201">Title: SAP NetWeaver na virtuálních počítačích Azure (VM) – Příručka pro nasazení vysoké dostupnosti</span><span class="sxs-lookup"><span data-stu-id="67df4-201">Title: SAP NetWeaver on Azure Virtual Machines (VMs) – High Availability Deployment Guide</span></span>

<span data-ttu-id="67df4-202">Souhrn: Tento dokument popisuje, jak se dají jediný bod selhání součásti jako SAP ASC nebo SCS a databázového systému SAP chránit v Azure.</span><span class="sxs-lookup"><span data-stu-id="67df4-202">Summary: This document describes how SAP single point of failure components like the SAP ASCS/SCS and DBMS can be protected in Azure.</span></span> <span data-ttu-id="67df4-203">Součásti SAP ASC nebo SCS, databázového systému a aplikací je nezbytné pro funkci SAP NetWeaver systémů, například SAP NetWeaver ABAP, SAP NetWeaver Java, SAP NetWeaver ABAP + Java systémy serverů.</span><span class="sxs-lookup"><span data-stu-id="67df4-203">Components of the SAP ASCS/SCS, DBMS and application servers are essential for the functionality of SAP NetWeaver systems, e.g. SAP NetWeaver ABAP, SAP NetWeaver Java, SAP NetWeaver ABAP+Java systems.</span></span> <span data-ttu-id="67df4-204">Proto musí funkce vysoké dostupnosti zavede a ujistěte se, že tyto součásti tolerovat selhání serveru nebo na virtuální počítač, protože dokončení konfigurace clusteru se systémem Windows pro úplné obnovení a prostředí Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="67df4-204">Therefore, high-availability functionality needs to be put in place to make sure that those components can sustain a failure of a server or a VM as done with Windows Cluster configurations for bare-metal and Hyper-V environments.</span></span>

<span data-ttu-id="67df4-205">Aktualizované: Prosinec 2016</span><span class="sxs-lookup"><span data-stu-id="67df4-205">Updated: December 2016</span></span>

<span data-ttu-id="67df4-206">[Tato příručka naleznete zde][ha-guide]</span><span class="sxs-lookup"><span data-stu-id="67df4-206">[This guide can be found here][ha-guide]</span></span>

