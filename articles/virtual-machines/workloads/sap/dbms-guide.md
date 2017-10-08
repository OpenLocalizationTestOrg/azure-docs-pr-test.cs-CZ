---
title: "aaaAzure nasazení databázového systému virtuálních počítačů pro SAP NetWeaver | Microsoft Docs"
description: "Nasazení virtuálních počítačů databázového systému Azure pro SAP NetWeaver"
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5654dac7-4204-4387-b312-3d8b2898eb3a
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 501f6fbc2baa379b706e95d2bfba377ac129b382
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-dbms-deployment-for-sap-netweaver"></a>Nasazení virtuálních počítačů databázového systému Azure pro SAP NetWeaver
[767598 ]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
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
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
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
[dbms-guide-managed-disks]:dbms-guide.md#f42c6cb5-d563-484d-9667-b07ae51bce29

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
[virtual-machines-azurerm-versus-azuresm]:../../../resource-manager-deployment-model.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md 
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md 
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
[virtual-machines-manage-availability-linux]:../../linux/manage-availability.md
[virtual-machines-manage-availability-windows]:../../windows/manage-availability.md
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

Tato příručka je součástí hello dokumentaci na implementaci a nasazení softwaru SAP hello v Microsoft Azure. Před čtením této příručky, přečtěte si hello [plánování a implementace průvodce][planning-guide]. Tento dokument popisuje nasazení hello různých systémů správy relační databáze (RDBMS) a souvisejících produktů v kombinaci s SAP na Microsoft Azure virtuální počítače (VM) pomocí hello infrastruktury Azure jako možnosti služby (IaaS).

zadané platformy Hello dokumentu doplňuje hello SAP instalace dokumentace a poznámky k SAP, které představují hello primární prostředky pro instalace a nasazení softwaru SAP na.

## <a name="general-considerations"></a>Obecné aspekty
V této kapitole vydávají aspektů s databázového systému SAP související systémy ve virtuálních počítačích Azure. Existuje několik odkazů na systémy toospecific databázového systému v této kapitole. Místo toho hello konkrétní databázového systému systémy jsou zpracovávány v tomto dokumentu po této kapitoly.

### <a name="definitions-upfront"></a>Definice předem
V dokumentu hello používáme hello následující podmínky:

* IaaS: Infrastruktura jako služba.
* PaaS: Platforma jako služba.
* SaaS: Software jako služba.
* Součást SAP: jednotlivých SAP aplikace například ECC, BW, správce řešení nebo podnikovém portálu.  SAP součástí může být založen na tradičních technologií ABAP nebo Java nebo jiných NetWeaver na základě aplikaci, například obchodních objektů.
* Prostředí SAP: jeden nebo více součástí SAP logicky seskupeny tooperform obchodní funkce jako je například vývoj, QAS, školení, zotavení po Havárii nebo produkční.
* SAP na šířku: Vztahuje toohello celý SAP prostředky v zákazníka na šířku IT. Hello SAP šířku zahrnuje všechny produkční a mimo provozní prostředí.
* Systém SAP: kombinace hello databázového systému vrstvu a aplikační vrstvu služby, například SAP ERP vývojového systému SAP BW testovací systém, produkční systému SAP CRM, atd. V Azure nasazení není podporované toodivide tyto dvě vrstvy mezi místními a Azure. To znamená, že systému SAP buď je nasazena místně nebo je nasazené v Azure. Můžete však nasadit hello různých systémech SAP šířku v Azure nebo místní. Například může nasadit systémy vývoj a testování SAP CRM hello v Azure, ale hello SAP CRM produkční systému místní.
* Nasazení jenom cloudu: nasazení, kde není hello předplatného Azure připojená prostřednictvím site-to-site nebo ExpressRoute připojení toohello místní síťové infrastruktuře. Společné dokumentace k Azure tyto typy nasazení jsou také popsány jako 'jenom pro Cloud, nasazení. Virtuální počítače nasazené pomocí této metody jsou přístupné prostřednictvím hello Internet a veřejné koncové body Internetu přiřazených toohello virtuálních počítačů v Azure. Hello místní služby Active Directory (AD) a DNS není rozšířeno tooAzure v těchto typů nasazení. Virtuální počítače hello proto nejsou součástí hello místní služby Active Directory. Poznámka: Jenom pro Cloud nasazení v tomto dokumentu jsou definovány jako dokončení krajiny SAP, které jsou spuštěny v Azure bez rozšíření služby Active Directory nebo překlad výhradně z místního do veřejného cloudu. Jenom pro cloud konfigurace nejsou podporovány pro produkční systémy SAP nebo konfigurace, kde moduly STM SAP nebo jiné místní prostředky musí toobe používá mezi systémy SAP hostované v Azure a prostředky, které se nacházejí v místě.
* Mezi různými místy: Popisuje scénář, kde jsou virtuální počítače nasazené tooan předplatné Azure, který má site-to-site, více lokalit nebo připojením ExpressRoute mezi místní hello datových centrech a Azure. Dokumentace k společné Azure, tyto typy nasazení jsou také popsány jako mezi různými místy scénáře. Hello důvod pro připojení hello je tooextend místní domény, místní služby Active Directory a DNS místně do Azure. Hello místně na šířku je rozšířené toohello Azure prostředky předplatného hello. S touto příponou, hello virtuálních počítačů může být součástí hello místní domény. Uživatelé domény hello místní domény může přistupovat k serverům hello a službu lze spouštět na ty virtuální počítače (např. služby databázového systému). Komunikace a název rozlišení mezi virtuálními počítači nasazen místní a virtuální počítače nasazené v Azure je možné. Očekáváme, že tento toobe hello nejběžnější scénáře pro nasazení SAP prostředky v Azure. Další informace najdete v tématu [v tomto článku] [ vpn-gateway-cross-premises-options] a [v tomto článku][vpn-gateway-site-to-site-create].

> [!NOTE]
> Mezi různými místy nasazení SAP systémy, kde Azure Virtual Machines s SAP systémy jsou členy místní domény jsou podporovány pro produkční systémy SAP. Konfigurace mezi různými místy jsou podporovány pro nasazení částí nebo dokončení krajiny SAP do Azure. Hello dokončení SAP na šířku i běžící v Azure vyžaduje, že tyto virtuální počítače byly součástí místní domény a služby Active Directory. V předchozí verze dokumentace hello už jsme mluvili o IT hybridní scénáře, kde se zobrazuje hello termín "Hybridní" v hello fakt, že existuje připojení mezi různými místy mezi místními a Azure. V tomto případě "Hybridní" také znamená, že hello virtuálních počítačů v Azure jsou součástí hello místní služby Active Directory.
> 
> 

Některé dokumentaci Microsoft popisuje scénáře mezi různými místy trochu jinak, zejména pro konfigurace HA databázového systému. V případě hello hello související SAP dokumentů hello mezi různými místy scénář právě boils dolů toohaving site-to-site nebo privátní (ExpressRoute) připojení a toohello faktu, hello SAP šířku rozděluje mezi místními a Azure.

### <a name="resources"></a>Zdroje
Hello následující příručky jsou k dispozici pro téma hello nasazení SAP v Azure:

* [Azure virtuálních počítačů, plánování a implementace pro SAP NetWeaver][planning-guide]
* [Nasazení virtuálních počítačů Azure pro SAP NetWeaver][deployment-guide]
* [Nasazení virtuálních počítačů databázového systému Azure pro SAP NetWeaver (Tento dokument)][dbms-guide]

Následující poznámky k SAP Hello jsou související toohello tématem SAP v Azure:

| Poznámka: číslo | Název |
| --- | --- |
| [1928533] |Aplikace SAP v Azure: typy podporovaných produktů a virtuální počítač Azure |
| [2015553] |SAP na platformě Microsoft Azure: podporovat požadavky |
| [1999351] |Řešení potíží s rozšířenou Azure monitorování pro SAP |
| [2178632] |Klíč monitorování metriky pro SAP na platformě Microsoft Azure |
| [1409604] |Virtualizace v systému Windows: rozšířené monitorování |
| [2191498] |SAP v systému Linux s Azure: rozšířené monitorování |
| [2039619] |SAP aplikací v Microsoft Azure pomocí hello databáze Oracle: podporované produkty a verze |
| [2233094] |DB6: Aplikace SAP v Azure pomocí IBM DB2 pro Linux, UNIX a Windows - Další informace |
| [2243692] |Linux na Microsoft Azure (IaaS) virtuálních počítačů: problémy licence SAP |
| [1984787] |Systému SUSE LINUX Enterprise Server 12: Poznámky k instalaci |
| [2002167] |Red Hat Enterprise Linux 7.x: instalace a Upgrade |
| [2069760] |Oracle Linux 7.x SAP instalace a Upgrade |
| [1597355] |Doporučení odkládacího prostoru pro Linux |
| [2171857] |Oracle Database 12c – podpora systému souborů v systému Linux |
| [1114181] |Databáze Oracle 11g – podpora systému souborů v systému Linux |


Hello si také přečíst [oznámení změny stavu Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) obsahující všechny SAP poznámky pro Linux.

Měli byste mít praktické znalosti o hello Architektura Microsoft Azure a jak jsou nasadit a provozovat virtuální počítače Microsoft Azure. Další informace najdete <https://azure.microsoft.com/documentation/>

> [!NOTE]
> Snažíme se **není** hovoříte o platforma Microsoft Azure jako služba (PaaS) nabídky Nástroje hello platforma Microsoft Azure. Tento dokument je o spuštění systému správy databáze (databázového systému) v Microsoft Azure Virtual Machines (IaaS), stejně jako hello databázového systému by byl spuštěn v místním prostředí. Možnosti databáze a funkce mezi tyto dvě nabídky se velmi liší a nesmí promíchala mezi sebou. Viz také: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Vzhledem k tomu, že budeme se zabývat IaaS, obecně hello Windows, Linux a databázového systému instalace a konfigurace jsou v podstatě hello stejné jako jakékoli virtuálního počítače nebo úplné systému počítače, které by nainstalujete místně. Existují však některé architektura a systém správy implementace rozhodnutí, která se liší, při použití IaaS. Hello účelem tohoto dokumentu je tooexplain hello konkrétní systému a architektuře správy rozdíly, musí být připravených pro při použití IaaS.

Obecně platí hello celkové oblasti rozdílem, že tento dokument popisuje jsou:

* Plánování hello správnou virtuální počítač nebo disk rozložení tooensure systémy SAP máte rozložení souboru hello správná data a můžete dosáhnout dostatek IOPS pro úlohy.
* Požadavky sítě při použití IaaS.
* Funkce toouse konkrétní databáze v pořadí toooptimize hello databáze rozložení.
* Aspekty zálohování a obnovení v IaaS.
* Využívá různé typy Image pro nasazení.
* Vysoké dostupnosti v Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Struktura RDBMS nasazení
V pořadí toofollow tato kapitola, je nutné toounderstand co se zobrazí v [to] [ deployment-guide-3] kapitoly hello [Průvodce nasazením] [ deployment-guide]. Znalost hello jinou sérii virtuálních počítačů a jejich rozdíly a rozdíly Azure Standard a Premium Storage by měl být rozumím jim a známé před čtením této kapitoly.

Dokud března 2015 disky, které obsahují operačního systému byly omezené too127 GB velikost. Toto omezení získali odvolat v března 2015 (Další informace o kontrole <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/>). Odtud na discích obsahující hello operačního systému může mít hello stejnou velikost jako jakákoli jiná disku. Nicméně nám stále přednost struktura nasazení, kde jsou oddělené od soubory databáze hello hello operační systém, databázového systému a případný SAP binární soubory. Proto Očekáváme, že SAP systémy s operačním systémem v Azure Virtual Machines hello základní virtuálního počítače (nebo disk) nainstalovat s hello operačního systému, spustitelné soubory systému správy databáze a SAP spustitelné soubory. Hello databázového systému data a soubory protokolu jsou uložené ve službě Azure Storage (Standard nebo Premium Storage) v různých discích a připojené jako logické disky toohello původní Azure bitovou kopii operačního systému virtuálního počítače. 

Závisí na využívání Azure Standard nebo Premium Storage (například pomocí hello DS-series nebo GS-series virtuálních počítačů) existuje jsou ostatní kvóty v Azure, které jsou popsány [zde (Linux)] [ virtual-machines-sizes-linux] a [zde (Windows)][virtual-machines-sizes-windows]. Při plánování vaší rozložení disku, je třeba toofind hello vyvážit hello kvót pro hello následující položky:

* Hello počet datových souborů.
* Hello počet disků, které obsahují soubory hello.
* kvóty IOPS Hello jednoho disku.
* Hello propustnost dat na disk.
* Hello počet dalších datových disků na jeden velikost virtuálního počítače.
* Hello celková propustnost úložiště virtuálního počítače může poskytnout.

Azure vynucuje kvóty IOPS podle datový disk. Tyto kvóty se liší pro disky hostované na úložiště Azure úrovně Standard a Premium Storage. Latence vstupně-výstupní operace jsou také velmi liší hello dva typy úložiště Storage úrovně Premium doručování faktory lepší latence vstupně-výstupní operace. Každý hello různých typů virtuálních počítačů má omezený počet datových disků, že budete mít tooattach. Další omezení je, že pouze určité typy virtuálních počítačů můžete využít Azure Premium Storage. To znamená, že hello rozhodnutí pro určitý typ virtuálního počítače nemusí pouze bude týkat hello procesoru a požadavky na paměť, ale i hello IOPS, latenci a disku propustnost požadavky, které obvykle jsou změněna pomocí hello počet disků nebo hello typ disky úložiště Premium. Zejména s Storage úrovně Premium hello velikost disku také může být závisí na hello počtu IOPS a propustnosti, vyžadující toobe dosáhnout každého disku.

Hello skutečnost, že hello celkovou rychlost IOPS, hello počet disky připojené a hello velikost hello virtuálních počítačů, jsou všechny svázané společně, může dojít Azure konfigurace systému SAP toobe liší od jeho místního nasazení. omezení Hello IOPS na logické jednotce se obvykle konfigurovat v místním nasazení. Vzhledem k tomu s Azure Storage jsou tyto limity pevný nebo jako úložiště Premium závisí na typu disku hello. Proto s místní nasazení vidíte zákazníka konfigurace databázové servery, které používají mnoho různých svazcích pro speciální spustitelné soubory jako SAP a hello databázového systému nebo speciální svazky pro dočasné databáze nebo tabulka prostory. Přesunutý tooAzure po v místním systému se může vést tooa odpady šířky pásma potenciální IOPS podle plýtvání disk pro spustitelné soubory nebo databáze, které neprovádějte žádné nebo není spoustu IOPS. Ve virtuálních počítačích Azure proto doporučujeme tuto hello databázového systému SAP spustitelné soubory a pokud je to možné nainstalovat na disk hello operačního systému.

Hello umístění hello databázové soubory a soubory protokolu a typ hello Azure Storage používá, nesmí být definován IOPS, latence a propustnosti požadavky. V pořadí toohave dostatek IOPS pro hello transakčního protokolu, je možné, vynucené tooleverage více disků pro hello transakčního protokolu souborů nebo použijte větší disk úložiště Premium. V tomto případě se jeden by sestavení RAID softwaru (například Windows úložiště fondu pro systém Windows nebo MDADM a LVM (Správce logických svazku) pro Linux) s hello disky, které obsahují hello transakčního protokolu.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Jednotka D:\ virtuální počítač Azure je jednotka netrvalý, který je zálohovaný díky některé místní disky na hello Azure výpočetním uzlu. Protože se jedná, netrvalý, to znamená, všechny změny provedené toohello obsahu na jednotku D:\ hello bude ztracena, jakmile hello virtuálních počítačů po restartu. "Jakékoliv změny" jsme znamená uložené soubory, adresáře vytvořené, nainstalované aplikace, atd.
> 
> ![Linux][Logo_Linux] Linux
> 
> Virtuální počítače Azure s Linuxem jednotku v /mnt/resource, který se nachází na dočasné jednotce založenou na místní disky na hello Azure výpočetním uzlu automaticky připojit. Protože se jedná, netrvalý, to znamená, že toocontent všechny změny provedené v /mnt/resource jsou ztraceny, když hello virtuálních počítačů po restartu. Jakékoliv změny jsme znamená soubory uložené, adresáře vytvořené, nainstalované aplikace, atd.
> 
> 

- - -
Závisí na hello Azure VM-series, hello místní disky na hello výpočetní výkon zobrazit jiný uzel, který může být klasifikovány jako:

* A0 A7: Velmi omezená výkonu. Nejde použít pro všechno, co je nad rámec stránkovací soubor windows
* A8-A11: Velmi dobré výkonové charakteristiky se některé deset tisíc IOPS a > propustnost 1GB/s
* D-Series: Velmi dobré výkonové charakteristiky se některé deset tisíc IOPS a > propustnost 1GB/s
* DS-Series: Velmi dobré výkonové charakteristiky se některé deset tisíc IOPS a > propustnost 1GB/s
* G-Series: Velmi dobré výkonové charakteristiky se některé deset tisíc IOPS a > propustnost 1GB/s
* GS-Series: Velmi dobré výkonové charakteristiky se některé deset tisíc IOPS a > propustnost 1GB/s

Výše uvedené příkazy se má použít toohello typy virtuálních počítačů, které jsou certifikované s SAP. Hello řady virtuálních počítačů s vynikající IOPS a propustnost kvalifikaci pro využívání některé funkce databázového systému, jako jsou databáze tempdb nebo místa na dočasné tabulky.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Ukládání do mezipaměti pro virtuální počítače a datové disky
Při vytváření datových disků prostřednictvím portálu hello nebo pokud jsme připojit tooVMs nahrané disky, jsme můžete vybrat, zda text hello vstupně-výstupní provoz mezi hello virtuálních počítačů a těchto disků umístěný v úložišti Azure jsou uloženy v mezipaměti. Azure Standard a Premium Storage použít dvě různé technologie pro tento typ mezipaměti. V obou případech by samotnou mezipamětí hello disku zálohovaný v hello používá stejné jednotky hello dočasné disku (v systému Windows D:\) nebo /mnt/resource v systému Linux hello virtuálních počítačů.

Pro Azure Standard Storage hello mezipaměti možné typy jsou:

* Žádné ukládání do mezipaměti
* Mezipaměti pro čtení
* Čtení a ukládání do mezipaměti

Konzistentní a deterministický výkonu tooget pořadí byste měli nastavit hello ukládání do mezipaměti na Azure Standard Storage pro všechny disky obsahující **data související s databázového systému souborů, souborů protokolu a tabulky místo too'NONE'**. Hello ukládání do mezipaměti hello virtuálních počítačů může zůstat výchozí hello.

Pro Storage úrovně Premium existovat hello následující možnosti ukládání do mezipaměti:

* Žádné ukládání do mezipaměti
* Mezipaměti pro čtení

Doporučení pro Storage úrovně Premium je tooleverage **číst ukládání do mezipaměti pro datové soubory** databázi SAP hello a pokusit **bez ukládání do mezipaměti pro disky hello soubory protokolu**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Softwaru diskového pole RAID
Jak již bylo uvedeno výše musíte toobalance hello počet IOPS potřebné pro soubory databáze hello napříč hello počet disků můžete nakonfigurovat a hello maximální IOPS virtuálního počítače Azure poskytuje na disku nebo disk typu Premium Storage. Nejjednodušší způsob, jak toodeal s hello IOPS načíst přes disků je toobuild softwaru diskového pole RAID přes hello různých disků. Počet datových souborů hello databázového systému SAP pak umístíte na hello logické jednotky LUN carved mimo hello softwaru diskového pole RAID. Závisí na hello požadavky, které že můžete tooconsider hello využití úložiště Premium Storage také od dva hello tři různé Storage úrovně Premium disky poskytují vyšší kvóty IOPS než disky podle standardního úložiště. Kromě hello významné lépe vstupně-výstupních operací latence poskytované Azure Premium Storage. 

Totéž platí i toohello transakčního protokolu hello různých systémů databázového systému. S mnohými z nich jen přidat další soubory protokolu nepomůže vzhledem k tomu, že systémy databázového systému hello zapisovat do jednoho ze souborů hello vždy pouze. Pokud jsou potřeba vyšší rychlosti IOPS než jednu standardní úložiště založené může poskytnout disk, můžete rozkládají přes víc disků Standard Storage nebo můžete použít větší typ disku Storage úrovně Premium, který kromě vyšší rychlosti IOPS také nabízí faktory menší latence pro zápis hello Vstupně-výstupních operací do hello transakčního protokolu.

Situace došlo v Azure nasazení, které by upřednostnit pomocí softwaru diskového pole RAID jsou:

* Transakční protokol/opakování protokol vyžadují více procesorů než Azure poskytuje pro jeden disk. Jak je uvedeno nahoře to bude možné vyřešit podle budovy logické jednotce LUN přes víc disků pomocí softwaru diskového pole RAID.
* Nevyrovnaná vstupně-výstupní úlohy distribuce hello různé datové soubory databáze SAP hello. V takových případech jeden prostředí jeden datový soubor místo často stiskne hello kvóty. Zatímco jiné datové soubory i nezobrazují zavřít toohello IOPS kvóty jeden disk. V takových případech hello Nejsnazším řešením je toobuild jednu logickou jednotku prostřednictvím více disků pomocí softwaru diskového pole RAID. 
* Nevíte, jaká hello přesný vstupně-výstupní úlohy na datovém souboru je a pouze zhruba vědět, co hello celkové zatížení IOPS proti hello databázového systému je. Nejjednodušší toodo je toobuild jedné logické jednotce s hello pomůže softwaru diskového pole RAID. Součet Hello kvóty více disků za tuto logickou jednotku by pak splnění hello známé IOPS rychlost.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Doporučujeme použít prostory úložiště ve Windows, pokud používáte v systému Windows Server 2012 nebo vyšší. Je efektivnější než proložení Windows starších verzí systému Windows. Pomocí příkazů prostředí PowerShell pravděpodobně potřebovat toocreate hello fondy úložiště systému Windows a prostory úložiště, pokud používáte Windows Server 2012 jako operační systém. Příkazy prostředí PowerShell Hello naleznete zde <https://technet.microsoft.com/library/jj851254.aspx>
> 
> ![Linux][Logo_Linux] Linux
> 
> Pouze MDADM a LVM (Správce logických svazku) jsou podporované toobuild softwaru diskového pole RAID v systému Linux. Další informace najdete v tématu hello následující články:
> 
> * [Konfigurace softwaru diskového pole RAID v systému Linux] [ virtual-machines-linux-configure-raid] (pro MDADM)
> * [Konfigurace LVM na virtuální počítač s Linuxem v Azure][virtual-machines-linux-configure-lvm]
> 
> 

- - -
Důležité informace pro využívání virtuálních počítačů series, které jsou možné toowork službou Azure Premium Storage obvykle jsou:

* Požadavky pro latenci vstupně-výstupních operací, které jsou zavřete toowhat SAN nebo NAS zařízení doručit.
* Vyžádání pro faktory lepší vstupně-výstupních operací latenci než může poskytnout Azure Standard Storage.
* Vyšší IOPS na virtuálních počítačů než co bylo možné dosáhnout s více disky standardní úložiště pro určitý typ virtuálního počítače.

Od hello základní Azure Storage replikuje jednotlivých uzlů úložiště disku tooat minimálně tři jednoduché RAID 0 proložení lze použít. Neexistuje žádné nutné tooimplement RAID5 nebo RAID1.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure Storage
Microsoft Azure Storage ukládá hello základní virtuální počítač (s operačním systémem) a disky nebo uzlů tooat minimálně tři samostatné úložiště objektů BLOB. Při vytváření účtu úložiště nebo spravovaných disků, při volbě ochrany, jak je vidět tady:

![Geografická replikace povolena pro účet úložiště Azure][dbms-guide-figure-100]

Místní účet replikace Azure Storage (místně redundantní) poskytuje úrovně ochrany proti ztrátě dat z důvodu selhání tooinfrastructure, že několik zákazníků může dovolit toodeploy. Jako v příkladu nahoře, že se s pátá se varianta mezi hello první tři čtyři různé možnosti. Vyhledávání blíže v nich jsme možné rozlišit:

* **Premium místně redundantní úložiště (LRS)**: Azure Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disku pro virtuální počítače spuštěné I náročnými úlohy. Existují tři repliky hello dat v rámci hello stejné datové centrum Azure z oblasti Azure. Hello kopie jsou v různých selhání a upgradu domén (koncepty, najdete v tématu [to] [ planning-guide-3.2] kapitoly v hello [Planning Guide][planning-guide]). V případě repliky hello dat přejdete mimo provoz z důvodu selhání uzlu úložiště tooa nebo selhání disku se automaticky vytvoří novou repliku.
* **Místně redundantní úložiště (LRS)**: V tomto případě existují tři repliky hello dat v rámci hello stejné datové centrum Azure z oblasti Azure. Hello kopie jsou v různých selhání a upgradu domén (koncepty, najdete v tématu [to] [ planning-guide-3.2] kapitoly v hello [Planning Guide][planning-guide]). V případě repliky hello dat přejdete mimo provoz z důvodu selhání uzlu úložiště tooa nebo selhání disku se automaticky vytvoří novou repliku. 
* **Geograficky redundantní úložiště (GRS)**: V tomto případě je asynchronní replikaci, která kanály další tři repliky hello dat v jiné oblasti Azure, který je ve většině případů hello v hello stejné zeměpisné oblasti (např. Severní Evropa a západní Evropa). Výsledkem tři další repliky, aby byly v součet šesti repliky. Varianta to je doplněk, kde hello data v hello geograficky replikované oblasti Azure lze použít pro čtení účely (Read-Access Geo-redundantní).
* **Zónu redundantní úložiště (ZRS)**: V tomto případě hello hello tři repliky hello data zůstat ve stejné oblasti Azure. Jak je popsáno v [to] [ planning-guide-3.1] kapitoly hello [Planning Guide] [ planning-guide] oblast Azure může být počet datových centrech v těsné blízkosti. V případě hello LRS by hello repliky distribuovány prostřednictvím různých datových centrech hello, který jedné oblasti Azure.

Další informace naleznete [sem][storage-redundancy].

> [!NOTE]
> Pro nasazení databázového systému se nedoporučuje hello využití geograficky redundantní úložiště
> 
> Azure geografická replikace úložiště je asynchronní. Replikaci jednotlivých disků připojených tooa jeden virtuální počítač nejsou synchronizované v kroku zámku. Proto není vhodné tooreplicate databázového systému souborů, které jsou distribuovány na jiné disky nebo nasadit proti softwaru založené na více disků RAID. Software databázového systému vyžaduje, aby hello trvalé diskového úložiště se synchronizuje přesněji v různých logických jednotek a základní disky nebo disky. Software databázového systému používá různé mechanismy toosequence vstupně-výstupní operace zápisu aktivity a databázového systému hlásí, že cílem replikace hello hello diskového úložiště je poškozená, pokud tyto i liší v závislosti na několik milisekund. Proto pokud jeden opravdu chce konfigurace databáze s databází na více disků geograficky replikované k roztažení, takové replikace musí toobe provést s prostředky databáze a funkce. Jeden neměli spoléhat na úložiště Azure geografická replikace tooperform tuto úlohu. 
> 
> problém Hello je nejjednodušší tooexplain příklad systém. Předpokládejme, že máte systému SAP nahraje do Azure, který má osm disky obsahující data souborů hello databázového systému plus jeden disk obsahující soubor protokolu transakcí hello. Každé z nich tyto devět disků mají data zapsat toothem v konzistentní způsob podle toohello databázového systému, zda zapisuje hello data toohello dat nebo transakcí soubory protokolu.
> 
> V pořadí tooproperly geo replikovat hello dat a údržbu image konzistentní databáze, hello obsah všech devět disků by mít toobe geograficky replikované v hello přesný pořadí hello vstupně-výstupních operacích měla spustit pro hello devět různých disků. Geografická replikace Azure Storage, ale neumožňuje toodeclare závislosti mezi disky. To znamená, že Microsoft Azure Storage se geografická replikace není vědět o hello fakt, že hello obsah v těchto devět různých disků jsou související tooeach jiné a že jsou změny dat hello konzistentní pouze v případě, že replikace v hello pořadí hello vstupně-výstupních operací došlo k na všechny disky devět hello.
> 
> Kromě toho se vysoké, že hello geograficky replikované Image ve scénáři hello neposkytují image konzistentní databáze, také je snížení výkonu, který se zobrazí s geograficky redundantní úložiště, které může vážně šance dopad na výkon. V souhrnu nepoužívejte tento typ redundance úložiště pro úlohy typu databázového systému.
> 
> 

#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Mapování virtuálních pevných disků do účtů úložiště služby virtuální počítač Azure
Tato kapitola se vztahuje pouze na účty úložiště tooAzure. Pokud máte v úmyslu spravovat disky toouse, platí omezení hello výše v této kapitole se nevztahují. Další informace o discích spravovaných, najdete v kapitole [spravované disky] [ dbms-guide-managed-disks] tohoto průvodce.

Účet úložiště Azure je pouze pro správu konstrukce, ale také předmětem omezení. Zatímco hello omezení liší na tom, jestli mluvíme o standardní účet úložiště Azure nebo prémiový účet úložiště Azure. Hello přesný možnosti a omezení jsou uvedeny [sem][storage-scalability-targets]

Proto pro Azure Standard Storage je důležité toonote je omezena na hello IOPS na účet úložiště (řádek obsahující 'celkový počet požadavků, v [hello článku][storage-scalability-targets]). Kromě toho existuje počáteční maximální 100 účtů úložiště za předplatné Azure (k červenci 2015). Proto se doporučuje toobalance IOPS z virtuálních počítačů mezi více účtů úložiště při použití Azure Standard Storage. Zatímco jeden virtuální počítač v ideálním případě pokud je to možné používá jeden účet úložiště. Takže pokud mluvíme o databázového systému nasazení, kde může každý virtuální pevný disk, který je hostován na Azure Standard Storage dosažení limitu kvóty, měli byste pouze nasadit 30-40 virtuální pevné disky na účet úložiště Azure, která používá Azure Standard Storage. Na hello druhé straně, pokud využít Azure Premium Storage a chcete toostore svazky velké databáze, je dobře z hlediska IOPS. Ale prémiový účet úložiště Azure je ve svazku data způsobem více omezující než standardní účet úložiště Azure. V důsledku toho lze nasadit pouze omezený počet virtuálních pevných disků v rámci účtu Azure Premium Storage před stiskne limit hello dat svazku. Na konci hello představit jako "Virtuální síť SAN", který má omezené možnosti ve IOPS a kapacity účtu úložiště Azure. Hello úkol zůstane v důsledku toho jako místní nasazení, toodefine hello rozložení hello virtuální pevné disky hello různé SAP systémy přes hello různých 'pomyslná zařízení sítě SAN, nebo účty úložiště Azure.

Pro Azure Standard Storage, není doporučeno toopresent úložiště z jiného úložiště účtů tooa jeden virtuální počítač, pokud je to možné.

Pokud používáte hello DS nebo GS-series virtuálních počítačů Azure, je možné toomount virtuální pevné disky mimo standardní účty úložiště Azure a účty úložiště Premium. Případy použití, jako jsou zápisu do standardního úložiště zálohování zálohuje virtuální pevné disky a že máte data databázového systému a soubory protokolu na Storage úrovně Premium se toomind, kde může využít takové heterogenní úložiště. 

Na základě zákaznických nasazení a testování přibližně 30 too40, které virtuální pevné disky obsahující data soubory databáze a soubory protokolu se dá zřídit v jednom Azure standardní účet úložiště s přijatelný výkon. Jak už bylo zmíněno dříve, hello omezení prémiový účet úložiště Azure je pravděpodobně toobe hello data kapacitu, kterou lze nainstalovat a není IOPS.

Jako síť SAN zařízení místní, sdílení vyžaduje některá monitorování v pořadí tooeventually detekovat kritická místa na účet úložiště Azure. Hello rozšíření monitorování Azure pro SAP a hello portálu Azure jsou nástroje, které se dají použít toodetect zaneprázdněných účty úložiště Azure, který může být doručování zhoršené výkon vstupně-výstupní operace.  Pokud se v této situaci detekuje, se doporučuje toomove zaneprázdněn virtuální počítače tooanother účet úložiště Azure. Odkazovat toohello [Průvodce nasazením] [ deployment-guide] podrobnosti o tom, jak tooactivate hello SAP hostitele možnosti monitorování.

Jiný článek shrnutí osvědčené postupy v oblasti Azure Standard Storage a standardních účtech úložiště Azure je zde uveden <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>

#### <a name="f42c6cb5-d563-484d-9667-b07ae51bce29"></a>Spravované disky
Spravované disky jsou nového typu prostředku v Azure Resource Manager, který lze použít místo virtuální pevné disky, které jsou uložené v účtech úložiště Azure. Spravované disky automaticky přiblížili hello sadu dostupnosti hello virtuálního počítače, které jsou připojené tooand proto zvýšení dostupnosti hello virtuálního počítače a služby hello, které jsou spuštěny na virtuálním počítači hello. toolearn víc, přečtěte si hello [článek s přehledem](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

SAP aktuálně podporuje jenom disky spravované Premium. Poznámka SAP čtení [1928533] další podrobnosti.

#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-tooazure-premium-storage"></a>Přesunutí nasadit databázového systému virtuálních počítačů z Azure Standard Storage tooAzure Storage úrovně Premium
Jsme setkají poměrně některých scénářích, kde se jako zákazník má toomove nasazený virtuální počítač z Azure Standard Storage do Azure Premium Storage. Pokud vaše disky jsou uložené v účtech úložiště Azure, to není možné bez fyzicky přesouvání dat hello. Existuje několik způsobů tooachieve hello cíle:

* Všechny virtuální pevné disky, základní virtuální pevný disk a také data virtuálních pevných disků může jednoduše zkopírovat do nového účtu úložiště Azure Premium. Často zvolili hello počet virtuálních pevných disků v Azure Standard Storage není z důvodu hello fakt, že je potřeba zavést hello datový svazek. Ale potřebné tolika virtuálních pevných disků kvůli hello IOPS. Teď, když přesouváte tooAzure Storage úrovně Premium může přejít způsob méně tooachieve virtuální pevné disky hello stejné propustnosti IOPS. Vzhledem tomu hello, že v Azure Standard Storage platíte za hello používá data a není velikost hello nominální disku, hello počet virtuálních pevných disků, není podstatné skutečně z hlediska nákladů. S Azure Premium Storage, však by platila pro velikost disku nominální hello. Většina zákazníků hello proto zkuste tookeep hello počet virtuálních pevných discích Azure ve Storage úrovně Premium na hello číslo potřebné tooachieve hello IOPS propustnost nezbytné. Ano většina zákazníků rozhodnout proti hello způsob jednoduchou 1:1 kopie.
* Pokud není dosud připojen, připojte se jeden virtuální pevný disk obsahující zálohu databáze z databáze SAP. Po dokončení zálohování hello odpojte všechny virtuální pevné disky, včetně hello virtuálního pevného disku obsahující hello zálohování a kopírování hello základní virtuální pevný disk a hello virtuálního pevného disku s hello zálohování do účtu Azure Premium Storage. Při nasazení pak hello virtuálních počítačů v závislosti na hello základní virtuální pevný disk a připojení hello virtuálního pevného disku s hello zálohování. Nyní můžete vytvořit další prázdný disky úložiště Premium pro hello virtuálních počítačů, které jsou používané toorestore hello databáze do. Předpokladem je, že tento hello databázového systému můžete toochange cesty toohello dat a souborů protokolu v rámci procesu obnovení hello.
* Další možností je varianta hello bývalé procesu, kde právě zkopírujte hello zálohování virtuálního pevného disku do Azure Premium Storage a připojte ji na virtuální počítač, který nově nasazení a instalaci.
* čtvrtý možnost Hello je by zvolte, pokud potřebují toochange hello počtu datových souborů databáze. V takovém případě můžete provést kopie homogenního systému SAP pomocí exportu/importu. Vložení ty exportovat soubory do virtuálního pevného disku, která se zkopírují do prémiový účet úložiště Azure a jeho připojení tooa virtuálních počítačů, že používáte toorun hello import procesy. Zákazníci využít tuto možnost, hlavně v případě, že chtějí toodecrease hello počtu datových souborů.

Pokud používáte spravované disky, můžete migrovat tooPremium úložiště podle:

1. Zrušit přidělení hello virtuálního počítače
2. V případě potřeby změnit velikost hello tooa velikost virtuálního počítače, který podporuje službu Premium Storage (například DS nebo GS)
3. Změnit hello disku spravovaný účet typu tooPremium (SSD)
4. Spustit virtuální počítač

### <a name="deployment-of-vms-for-sap-in-azure"></a>Nasazení virtuálních počítačů pro SAP v Azure
Microsoft Azure nabízí několik způsobů toodeploy virtuální počítače a přidružené disky. Tím je důležité toounderstand hello rozdíly, od přípravy hello virtuálních počítačů se můžou lišit závisí na hello způsob nasazení. Obecně platí podíváme do scénáře hello popsané v následujících kapitolách hello.

#### <a name="deploying-a-vm-from-hello-azure-marketplace"></a>Nasazení virtuálního počítače z hello Azure Marketplace
Jako tootake od společnosti Microsoft nebo třetích stran, pokud bitovou kopii z Azure Marketplace toodeploy hello virtuálního počítače. Po nasazení virtuálního počítače v Azure provedením hello stejné pokyny a nástroje pro tooinstall hello SAP softwaru uvnitř virtuálního počítače, jako byste to udělali v místním prostředí. Pro instalaci softwaru SAP hello uvnitř hello virtuálního počítače Azure, SAP a Microsoft doporučujeme odesílání a uložte hello SAP instalačním médiu disky nebo toocreate virtuální počítač Azure funguje jako "souborový server", který obsahuje všechny hello nezbytné SAP instalačního média.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Nasazení virtuálního počítače pomocí bitové kopie zobecněný zákaznické
Z důvodu toospecific oprava požadavky týkající se vaší verzí operačního systému nebo databázového systému nemusí hello zadané bitové kopie v Azure Marketplace hello podle vašich potřeb. Proto může být nutné toocreate virtuálního počítače pomocí vlastní "privátní" image operačního systému nebo databázového systému virtuálního počítače, které mohou být nasazeny několikrát později. tooprepare "privátní" image duplikovaná hello operačního systému musí být zobecněn na hello místní počítač. Odkazovat toohello [Průvodce nasazením] [ deployment-guide] podrobnosti o tom toogeneralize virtuálního počítače.

Pokud jste již nainstalovali SAP obsah v místní virtuální počítač (hlavně u systémy vrstvě 2), můžete upravit nastavení systému SAP hello po nasazení hello hello virtuálního počítače Azure pomocí hello instance přejmenovat postup nepodporuje hello zřizování softwaru SAP Správce (Poznámka SAP [1619720]). Jinak můžete nainstalovat hello SAP software později po nasazení hello hello virtuálního počítače Azure.

Od verze obsahu databáze hello používá hello aplikace SAP hello obsahu může generovat čerstvě instalací SAP nebo svůj obsah můžete importovat do Azure pomocí virtuální pevný disk s zálohu databáze databázového systému nebo využití funkcí toodirectly databázového systému hello zálohování do služby Microsoft Azure Storage. V takovém případě může také připravit virtuální pevné disky s hello databázového systému dat a protokolovat soubory na místě a importovat tyto disky do Azure. Ale hello přenosu dat databázového systému, který je načítán z místní tooAzure by fungovat přes virtuální pevný disk disky, které potřebují toobe připravené na místě.

#### <a name="moving-a-vm-from-on-premises-tooazure-with-a-non-generalized-disk"></a>Přesunutí virtuálního počítače z místní tooAzure s diskem zobecněn
Máte v plánu toomove konkrétního systému SAP z místní tooAzure (navýšení a shift). To lze provést tím, že nahrajete hello disk, který obsahuje hello operačního systému, hello SAP binární soubory a případné databázového systému binární soubory a disky hello s hello data a soubory hello databázového systému tooAzure protokolu. V opačném tooscenario #2 výše necháte hello název hostitele, identifikátor SID SAP a SAP uživatelské účty v hello virtuálního počítače Azure byly nakonfigurované v prostředí místní hello. Proto generalizací hello bitové kopie není nutné. Tento případ se většinou platí pro scénáře mezi různými místy, kde je součástí hello SAP šířku spouštět místně a částí v Azure.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Vysoká dostupnost a zotavení po havárii s virtuálními počítači Azure
Azure nabízí následující funkce vysoké dostupnosti (HA) a obnovení po havárii (DR), které se vztahují toodifferent součásti, které jsme byste použili pro nasazení SAP a databázového systému hello

### <a name="vms-deployed-on-azure-nodes"></a>Virtuální počítače nasazené na uzly Azure
Hello platformě Azure nenabízí funkce, jako je migrace za provozu pro nasazené virtuální počítače. To znamená, zda v clusteru serveru, na kterém je nasazený virtuální počítač je nutné údržby, hello virtuálních počítačů není nutné tooget zastavena a restartována. Údržby v Azure se provádí pomocí tak názvem upgradu domén v rámci clusterů serverů. Pouze jeden upgradu domény současně je neudržují. Během restartování dojde přerušení služby během hello virtuální počítač je vypnutý, provádění údržby a restartování virtuálního počítače. Většina dodavatelů databázového systému ale poskytovat vysokou dostupnost a zotavení po havárii funkce, které rychle restartuje služby databázového systému hello v jiném uzlu, pokud není k dispozici hello primárního uzlu. Hello platformě Azure nabízí funkce toodistribute virtuálních počítačů, úložiště a dalším službám Azure napříč tooensure upgradu domény, který plánované údržby nebo infrastruktury selhání by ovlivnit pouze malou část virtuálních počítačů nebo služeb.  S pečlivé plánování, je možné tooachieve dostupnosti úrovně porovnatelný z hlediska tooon místní infrastruktury.

Skupiny dostupnosti Microsoft Azure jsou logické seskupení virtuální počítače nebo služby, které zajišťuje virtuálním počítačům a dalším službám distribuované toodifferent selhání a upgradu domén v rámci clusteru tak, že by existovat pouze jedna vypnutí uzlu v daném v čase (přečíst [této (Linux)] [ virtual-machines-manage-availability-linux] nebo [této (Windows)] [ virtual-machines-manage-availability-windows] další podrobnosti najdete v článku).

Je nutné toobe nakonfiguroval účel při zavádění virtuálních počítačů, jak je vidět tady:

![Definice sadu dostupnosti pro HA databázového systému konfigurace][dbms-guide-figure-200]

Pokud chceme toocreate vysoce dostupné konfigurace databázového systému nasazení (nezávisle na hello jednotlivých HA databázového systému funkce použité), by třeba hello databázového systému virtuální počítače:

* Přidat virtuální počítače toohello hello stejné virtuální síti Azure (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* v hello navíc by měl mít Hello virtuální počítače hello HA konfigurace stejné podsíti. Překlad mezi různých podsítích hello není možné v čistě cloudové nasazení, pouze funguje překlad IP. Pomocí site-to-site nebo připojením ExpressRoute pro nasazení mezi různými místy, síť se alespoň jednu podsíť je už vytvořené. Provádí překlad podle toohello místní AD zásady a síťové infrastruktury. 

[comment]: <> (Test TODO MSSedusch Pokud stále true v ARM)

#### <a name="ip-addresses"></a>IP adresy
Důrazně doporučujeme toosetup hello virtuálních počítačů pro konfigurace HA odolným způsobem. Spoléhat na IP adresy tooaddress hello HA partnerů v rámci konfigurace HA hello není spolehlivá v Azure, pokud se používají statické IP adresy. V Azure existují dva koncepty "Vypnout":

* Vypnutí dolů prostřednictvím portálu Azure nebo Azure PowerShell rutinu Stop-AzureRmVM: V tomto případě hello virtuální počítač získá vypnutí a zrušte přiřazený. Účtu Azure je už účtovat pro tento virtuální počítač, takže hello pouze poplatky, které dojít k za využívání úložiště hello. Ale pokud hello privátní IP adresu síťového rozhraní hello nebyla statické, hello IP adresa se neuvolní a není zaručeno, že rozhraní sítě, hello získá hello starou znovu po restartování hello virtuálních počítačů přiřazen adresu IP. Provádění hello vypnout prostřednictvím hello portál Azure nebo voláním Stop-AzureRmVM způsobí, že deaktivace přidělení. Pokud nechcete, aby počítač hello toodeallocate použijte Stop-AzureRmVM - StayProvisioned 
* Pokud vypnete hello virtuální počítač z úroveň operačního systému, získá hello virtuální počítač vypnout a není zrušte přiřazený. Ale v takovém případě účtu Azure je stále účtovat hello virtuální počítač, i přes hello fakt, že se jedná o vypnutí. V takovém případě hello přiřazení hello IP adresu tooa zastavit virtuální počítač zůstane beze změn. Vypínání hello virtuálních počítačů v rámci nevynutí automaticky deaktivace přidělení.

I pro scénáře mezi různými místy ve výchozím nastavení vypnutí a deaktivace přidělení znamená deaktivace přiřazení IP adresy, hello hello virtuální počítač, i když místní zásady v nastavení protokolu DHCP se liší. 

* Hello výjimka je pokud jeden přiřadí statické IP adresy tooa rozhraní sítě jako popsaný [sem][virtual-networks-reserved-private-ip].
* V takovém případě zůstává pevná hello IP adresu, tak dlouho, dokud se neodstraní hello síťové rozhraní.

> [!IMPORTANT]
> V pořadí tookeep hello celého nasazení jednoduché a spravovat hello hello jasné, že doporučení je toosetup partnerství společností v konfiguraci s HA databázového systému nebo zotavení po Havárii v rámci Azure tak, že je funkční překlad mezi hello, které se podílejí různé virtuální počítače virtuální počítače.
> 
> 

## <a name="deployment-of-host-monitoring"></a>Nasazení hostitele monitorování
Pro produktivní využití SAP aplikací v Azure Virtual Machines vyžaduje SAP hello možnost tooget hostitele dat monitorování od hello fyzických hostitelů se spuštěnými hello virtuálních počítačích Azure. Konkrétní úroveň oprav SAP Agent hostitele není třeba umožňující tuto funkci SAPOSCOL a Agent hostitele SAP. úroveň oprav přesný Hello je popsána v Poznámka SAP [1409604].

Hello podrobnosti týkající se nasazení komponent, které poskytovat tooSAPOSCOL dat hostitele a Agent hostitele SAP a správa životního cyklu hello těchto součástí, najdete v části toohello [Příručka pro nasazení][deployment-guide]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Specifika tooMicrosoft systému SQL Server
### <a name="sql-server-iaas"></a>SQL Server IaaS
Od verze Microsoft Azure, můžete snadno migrovat existující systém SQL Server aplikace založená na Windows Server platforma tooAzure virtuálních počítačů. SQL Server ve virtuálním počítači můžete tooreduce hello celkové náklady na vlastnictví nasazení, správu a údržbu enterprise spektra aplikací umožňuje snadno migrací tooMicrosoft tyto aplikace Azure. Se systémem SQL Server v virtuální počítač Azure můžete správci a vývojáři dál používat hello stejných nástrojů vývoj a správu, které jsou k dispozici místně. 

> [!IMPORTANT]
> Jsme nejsou hovoříte o Microsoft Azure SQL Database, který je platforma jako služba nabídka Dobrý den platforma Microsoft Azure. Hello informace v tomto dokumentu jsou o spuštění hello produktu SQL Server, protože je znám pro místní nasazení v Azure Virtual Machines, využívání hello infrastruktury jako funkce služby Azure. Možnosti databáze a funkce mezi tyto dvě nabídky se liší a nesmí promíchala mezi sebou. Viz také: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Důrazně doporučujeme tooreview [to] [ virtual-machines-sql-server-infrastructure-services] dokumentace než budete pokračovat.

Následující části částí hello dokumentace v části výše uvedený odkaz hello jsou v hello agregovat a uvedených. Specifika kolem SAP jsou také uvedené a některé pojmy jsou popsány podrobněji. Důrazně ale toowork prostřednictvím hello dokumentace výše první než si přečtete konkrétní dokumentaci k systému SQL Server hello.

V IaaS konkrétní informace, které byste měli vědět před pokračováním je některé systému SQL Server:

* **Virtuální počítač SLA**: je SLA pro virtuální počítače běžící v Azure, které naleznete zde: <https://azure.microsoft.com/support/legal/sla/>  
* **Podpora verzí SQL**: pro zákazníky, SAP, podporujeme SQL Server 2008 R2 a vyšší na virtuální počítač Microsoft Azure. Nejsou podporované starší verze. Zkontrolujte tato obecná [prohlášení o odborné pomoci](https://support.microsoft.com/kb/956893) další podrobnosti. Všimněte si, že obecně systému SQL Server 2008 je společnost Microsoft podporuje také. Ale kvůli toosignificant funkce pro SAP, která byla zavedena v systému SQL Server 2008 R2, SQL Server 2008 R2 je hello minimální verze pro SAP. Mějte na paměti, že SQL Server 2012 a 2014 získali rozšířené o hlubší integrace do hello scénář IaaS (např. zálohování přímo s Azure Storage). Proto jsme omezit tento dokument tooSQL Server 2012 a 2014 s jeho nejnovější úroveň oprav pro Azure.
* **Podpora funkce SQL**: funkce nejvíce systému SQL Server jsou podporovány ve virtuálních počítačích Microsoft Azure na několik výjimek. **SQL Server převzetí služeb při selhání pomocí sdílených disků není možné clusterování**.  Distribuované technologie jako databáze zrcadlení, skupiny dostupnosti AlwaysOn, replikace, přesouvání protokolu a služby Service Broker jsou podporovány v jedné oblasti Azure. SQL Server AlwaysOn taky je podporovaná mezi různých oblastech Azure, jak je uvedeno zde: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Zkontrolujte hello [prohlášení o odborné pomoci](https://support.microsoft.com/kb/956893) další podrobnosti. Příklad na tom, jak toodeploy konfigurace aplikace AlwaysOn se zobrazí v [to] [ virtual-machines-workload-template-sql-alwayson] článku. Zkontrolujte také, out hello osvědčené postupy popsané [sem][virtual-machines-sql-server-infrastructure-services] 
* **Výkon SQL**: jsme si jisti, že Microsoft Azure hostované virtuální počítače provést velmi dobře v porovnání tooother veřejného cloudu virtualizace nabídek, ale jednotlivé výsledky se může lišit. Podívejte se na [to] [ virtual-machines-sql-server-performance-best-practices] článku.
* **Pomocí bitové kopie z Azure Marketplace**: hello nejrychlejší způsob, jak toodeploy nový virtuální počítač Microsoft Azure je toouse bitové kopie z hello Azure Marketplace. Existují obrázků v hello Azure Marketplace, které obsahují systému SQL Server. Hello bitové kopie, kde je již nainstalován systém SQL Server nelze použít pro aplikace SAP NetWeaver okamžitě. Hello důvodem je, že kolace systému SQL Server hello výchozí je nainstalován v rámci těchto bitových kopií a není hello kolace vyžadovanou SAP NetWeaver systémy. V pořadí toouse takovými obrázky, zkontrolujte hello kroků popsaných v kapitole [pomocí bitové kopie systému SQL Server z webu Microsoft Azure Marketplace hello][dbms-guide-5.6]. 
* Podívejte se na [podrobnosti o cenách](https://azure.microsoft.com/pricing/) Další informace. Hello [SQL Server 2012 Licensing průvodce](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) a [SQL Server 2014 licencování průvodce](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) jsou také důležité prostředků.

### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>Pokyny pro konfigurace systému SQL Server pro instalace související SAP systému SQL Server ve virtuálních počítačích Azure
#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Doporučení pro virtuální počítač nebo virtuální pevný disk strukturu pro nasazení SAP související systému SQL Server
V souladu s hello obecný popis, by měla být spustitelné soubory systému SQL Server nachází nebo nainstalován do hello systémové jednotce hello Virtuálního počítače disk operačního systému (jednotka C:\).  Obvykle se většina databáze systému SQL Server hello nejsou využívaných na vysoké úrovni SAP NetWeaver zatížení. Proto hello systémové databáze systému SQL Server (hlavní, databázi msdb a modelu) může zůstat na hello také jednotku C:\. Výjimka může být databáze tempdb, v případě hello některé ERP SAP a všechny úlohy BW, může to vyžadovat vyšší datový svazek nebo vstupně-výstupních operací operations svazek, který se nevejde do hello původní virtuální počítač. Pro tyto systémy je možné provádět hello následující kroky:

* Přesunout hello primární databáze tempdb datové soubory toohello stejné logické jednotce jako primární datové soubory hello hello SAP databáze.
* Přidejte všechny další databáze tempdb data souborů tooeach Dobrý den jiné logické jednotky obsahující soubor dat z databáze uživatelů SAP hello.
* Přidejte hello databáze tempdb logfile toohello logické jednotky, obsahující soubor protokolu databáze hello uživatele.
* **Výhradně pro typů virtuálních počítačů, které používají místní SSD** v protokolu a hello výpočetní uzel databáze tempdb data může soubory umístit na jednotku D:\ hello. Nicméně, může to být doporučená toouse víc datových souborech databáze tempdb. Mějte na paměti, že svazky jednotce D:\ se liší podle hello typ virtuálního počítače.

Tyto konfigurace povolit databáze tempdb tooconsume více místa, než je možné tooprovide hello systémová jednotka. V pořadí toodetermine hello databáze tempdb správnou velikost jeden zkontrolujte velikost databáze tempdb hello na stávajících systémů, které se spustit místní. Kromě toho by taková konfigurace umožňuje IOPS čísla proti databázi tempdb nelze zadat s hello systémového disku. Systémy, které jsou místní znovu, může být použité toomonitor vstupně-výstupní úlohy proti databázi tempdb, tak, aby odvozujete čísla IOPS hello očekávat toosee na vaše databáze tempdb.

Konfigurace virtuálního počítače, který používá systém SQL Server s databázi SAP a umístění souboru protokolu databáze tempdb a databázi tempdb dat na jednotku D:\ hello by vypadat podobně jako:

![Konfigurace referenčního virtuálního počítače Azure IaaS pro SAP][dbms-guide-figure-300]

Mějte na paměti, že tento hello jednotku D:\ má různou velikost závisí na hello typ virtuálního počítače. Závisí na požadavek hello velikost databáze tempdb je vynucené toopair databáze tempdb data a soubory protokolu s hello SAP data databáze a soubory protokolů v případech, kdy jednotku D:\ je příliš malá.

#### <a name="formatting-hello-disks"></a>Formátování hello disky
Pro SQL Server hello velikost bloku systému souborů NTFS pro disky obsahující data systému SQL Server a protokolu musí být soubory 64 kB. Neexistuje žádné nutné tooformat hello jednotku D:\. Tato jednotka obsahuje předem formátovaný.

V pořadí toomake jistotu, že hello obnovení nebo vytváření databází není inicializace hello datové soubory podle vynulování hello obsah hello soubory, jeden měli ujistit, že je spuštěna služba SQL Server hello uživatele kontextu hello má určitá oprávnění. Uživatelé ve skupině pro správu služby Windows hello obvykle mají tato oprávnění. Pokud hello služby SQL Server běží v kontextu uživatele hello Windows uživatel není správcem, je třeba tooassign tohoto uživatele hello uživatelské právo, provádět úlohy údržby svazku'.  Zobrazit podrobnosti hello v tomto článku znalostní báze Microsoft: <https://support.microsoft.com/kb/2574695>

#### <a name="impact-of-database-compression"></a>Dopad komprese databáze
V konfiguracích, kde vstupně-výstupní šířky pásma může představovat problém mohou pomoci při každé míry, což snižuje IOPS toostretch hello úlohy, jež možné spouštět v případě pomocí IaaS, jako je například Azure. Proto pokud to ještě neudělali, použití SQL serveru stránky komprese důrazně doporučujeme SAP i Microsoft před nahráním existující databázi tooAzure SAP.

Hello doporučení tooperform komprese databáze před nahráním tooAzure je dán ze dvou důvodů:

* Hello množství dat toobe nahrán je nižší.
* Doba trvání Hello provádění komprese hello je kratší, za předpokladu, že jeden může používat silnější hardware s více procesorů nebo větší šířku pásma vstupně-výstupních operací nebo méně vstupně-výstupních operací latence místně.
* Menší velikosti databáze může vést tooless náklady pro přidělení disku

Komprese databáze pracuje také virtuálních počítačích Azure, jako místní. Další podrobnosti o tom toocompress existující databázi SQL serveru SAP, zkontrolujte, zde: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>

### <a name="sql-server-2014--storing-database-files-directly-on-azure-blob-storage"></a>SQL Server 2014 – ukládání souborů databáze přímo na Azure Blob Storage
SQL Server 2014 otevře hello možnost toostore databázových souborů přímo v úložišti objektů Blob Azure bez hello "obálku" VHD je obcházet. Zejména s použitím standardního úložiště Azure nebo menší typy virtuálních počítačů to umožňuje scénáře, kde lze překonat hello omezení IOPS, která vynucovaly omezený počet disků, které mohou být připojené toosome menší typy virtuálních počítačů. Tento postup funguje pro uživatelské databáze, ale ne pro systémové databáze systému SQL Server. Funguje i pro data a soubory protokolu serveru SQL Server. Pokud chcete toodeploy databáze systému SQL Server SAP tímto způsobem místo 'zabalení"jej do virtuální pevné disky, mějte hello následující skutečnosti:

* toobe potřebám Hello účet úložiště používané v hello stejné oblasti Azure jako ten, který je použité toodeploy hello virtuálního počítače SQL Server běží v hello.
* Aspekty, které jsou uvedené dříve týkající se distribučních hello virtuálních pevných disků v různých účtech úložiště Azure platí pro tuto metodu také nasazení. Znamená hello počet vstupně-výstupní operace proti hello omezení hello účet úložiště Azure.

[comment]: <> (MSSedusch TODO ale tímto dojde k použití šířky pásma sítě a není úložiště šířky pásma, nebude ho?)

Podrobnosti o tomto typu nasazení jsou zde uvedeny: <https://docs.microsoft.com/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure>

Pořadí toostore soubory systému SQL Server data přímo na Azure Premium Storage, je nutné vydání opravy toohave minimální SQL Server 2014, které jsou zde uvedeny: <https://support.microsoft.com/kb/3063054>. Ukládání souborů dat systému SQL Server v Azure Standard Storage funguje s hello vydaná verze systému SQL Server 2014. Velmi stejné opravy hello však obsahují další řadu opravy, které hello přímého použití Azure Blob Storage pro datové soubory SQL serveru a zálohy spolehlivější. Proto doporučujeme používat tyto opravy obecně.

### <a name="sql-server-2014-buffer-pool-extension"></a>Rozšíření fondu vyrovnávací paměti systému SQL Server 2014
SQL Server 2014 zavedly novou funkci, která se nazývá rozšíření fondu vyrovnávací paměti. Tato funkce rozšiřuje hello fondu vyrovnávací paměti systému SQL Server, který je uložen v paměti s druhou úroveň mezipaměti, kterou je zajištěna místní SSD server nebo virtuální počítač. To umožňuje tookeep větší pracovní sady dat "v paměti'. Porovnání tooaccessing Azure Standard Storage hello přístup do hello rozšíření fondu vyrovnávací paměti hello, který je uložený na místní SSD virtuální počítač Azure je rychlejší mnoha faktorech.  Využití hello místní jednotku D:\ hello typů virtuálních počítačů, které mají vynikající IOPS a propustnost proto může být tooreduce hello velmi rozumný způsob, IOPS načíst Azure Storage a výrazně zlepšit dobu odezvy dotazů. To platí hlavně v případě, že není použití služby Premium Storage. V případě a hello využití na výpočetním uzlu hello hello mezipaměti pro čtení Azure Premium Storage úrovně Premium jsou doporučené pro datové soubory, očekávané žádné významné rozdíly. Důvodem je to, jak mezipaměti (rozšíření fondu vyrovnávací paměti systému SQL Server a mezipaměti pro čtení úložiště Premium) používáte místní disky hello hello výpočetních uzlů.
Další podrobnosti o této funkci, podívejte se do této dokumentace: <https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension> 

### <a name="backuprecovery-considerations-for-sql-server"></a>Důležité informace o zálohování nebo obnovení pro SQL Server
Při nasazování systému SQL Server do Azure musí být zkontrolovány vaše zálohování metodika. I když hello systému není systémem produktivní, databázi SAP hello hostitelem SQL Server je nutné zálohovat pravidelně. Vzhledem k tomu, že Azure úložiště udržuje tři bitové kopie, je nyní méně důležité v ohledem toocompensating havárie úložiště zálohy. Důvod priority Hello k zachování správné plán zálohování a obnovení je informace, které můžete kompenzovat chyby logické nebo ruční tím, že poskytuje bod v možnosti v době obnovení. Tak, aby hello cílem tooeither použití zálohy toorestore hello zálohování databáze tooa určité bodu v čase nebo toouse hello záloh v Azure tooseed jiného systému zkopírováním hello existující databázi. Například je může přenést z vrstvě 2 SAP konfigurace tooa 3vrstvé systému nastavení hello stejné systému obnovení ze zálohy.

Existují tři různé způsoby toobackup systému SQL Server tooAzure úložiště:

1. SQL Server 2012 CU4 a vyšší mohou nativně zálohování databází tooa adresy URL. To je podrobně popsán v blogu hello [novou funkčnost systému SQL Server 2014 – část 5 – zálohování a obnovení vylepšení](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Naleznete v kapitole [SQL Server 2012 SP1 CU4 nebo novější][dbms-guide-5.5.1].
2. Předchozí tooSQL verzích systému SQL Server 2012 CU4 používat tooa toobackup funkce přesměrování virtuálního pevného disku a v podstatě přesunout hello zápisu datového proudu směrem umístění úložiště Azure, který byl nakonfigurován. Naleznete v kapitole [SQL Server 2012 SP1 CU3 a starších verzích][dbms-guide-5.5.2].
3. poslední metodu Hello je tooperform konvenční příkaz toodisk zálohování systému SQL Server na disk zařízení. Toto je identické toohello místní nasazení vzor a není podrobněji v tomto dokumentu.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 nebo novější
Tato funkce umožňuje úložiště objektů BLOB zálohy tooAzure toodirectly. Bez této metody musíte zálohovat tooother disky, které by využívat disku a kapacity IOPS. Rada Hello je v podstatě to:

 ![Pomocí zálohování systému SQL Server 2012 tooMicrosoft Azure Storage BLOB][dbms-guide-figure-400]

Hello využít v tomto případě je, že jeden nepotřebuje záloh systému SQL Server toostore toospend disky na. Proto má méně disků, které jsou přidělené a hello celou šířku pásma disku IOPS lze použít pro soubory protokolu a data. Všimněte si, že hello maximální velikost zálohy je omezené tooa maximálně 1 TB, jak je uvedeno v části hello "Meze" v tomto článku: <https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url#limitations>. Pokud velikost zálohování hello navzdory pomocí zálohování serveru SQL komprese překročí velikost 1 TB, hello funkce popsané v kapitole [SQL Server 2012 SP1 CU3 a starších verzích] [ dbms-guide-5.5.2] musí v tomto dokumentu toobe použít.

[Související dokumentaci](https://docs.microsoft.com/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure) popisující hello obnovení databáze ze zálohy na úložišti objektů Blob Azure doporučujeme není toorestore přímo z úložiště objektů BLOB v Azure, pokud je záloha hello > 25 GB. Hello doporučení v tomto článku je jednoduše založenou na důležité informace o výkonu a ne z důvodu omezení toofunctional. Proto různých podmínkách uplatnit na případ od případu.

Dokumentaci o tom, jak je tento typ zálohy nastavit a využít lze nalézt v [to](https://docs.microsoft.com/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016) kurzu

Příkladem hello pořadí kroků, mohou být čteny v [zde](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

Automatizace zálohování, je nejvyšší důležitosti toomake jistotu, že jsou objekty BLOB hello pro každé zálohování jiný název. V opačném případě budou přepsána a je porušený řetězec obnovení hello.

V pořadí není toomix až věcí mezi hello tři různé typy zálohování je vhodné toocreate různé kontejnery pod hello účet úložiště používané pro zálohování. kontejnery Hello může být pouze virtuální počítač nebo podle typu virtuálního počítače a zálohování. schéma Hello může vypadat podobně jako:

 ![Pomocí zálohování systému SQL Server 2012 tooMicrosoft Azure Storage BLOB – různé kontejnery v části samostatný účet úložiště][dbms-guide-figure-500]

V příkladu hello výše hello, že zálohování by nebyla provedena do hello účet stejné úložiště, kde hello nasazených virtuálních počítačů. Bude nový účet úložiště pro zálohy hello. V rámci hello účty úložiště by různé kontejnery, které jsou vytvořené pomocí matice hello typu zálohování a hello název virtuálního počítače. Takové segmentace umožňuje snazší zálohy tooadministrate hello hello různé virtuální počítače.

objekty BLOB Hello jeden přímo zapíše hello zálohy, nejsou přidání toohello počet hello datových disků virtuálního počítače. Proto může jeden maximalizovat hello maximální počet datových disků připojit hello specifické SKU virtuálních počítačů pro hello data a soubor protokolu transakcí a spustit zálohu na kontejner úložiště. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 a starších verzích
Hello prvním krokem je nutné provést v pořadí tooachieve zálohování přímo s Azure Storage bude toodownload hello msi, který je propojený příliš[to](https://www.microsoft.com/download/details.aspx?id=40740) KBA článku.

Stáhněte si instalační soubor hello x64 a dokumentaci hello. soubor Hello nainstaluje program s názvem: "Zálohování systému Microsoft SQL Server tooMicrosoft nástroj Azure". Přečtěte si dokumentaci hello produktu hello důkladně.  Nástroj Hello v podstatě funguje v hello následujícím způsobem:

* Z hello straně systému SQL Server, je definována umístění na disku pro zálohování serveru SQL Server hello (nepoužívejte jednotku D:\ hello to).
* Hello nástroj umožňuje toodefine pravidla, které můžou být použité toodirect různé typy záloh toodifferent Azure Storage kontejnerů.
* Jakmile hello pravidla jsou na místě, přesměruje hello nástroj hello zápisu datového proudu hello zálohování tooone z virtuálních pevných disků nebo disků toohello hello umístění úložiště Azure, který byl dříve definován.
* Nástroj Hello ponechá malé se zakázaným inzerováním soubor několik velikosti KB na hello virtuálního pevného disku nebo Disk, který byl definován pro hello systému SQL Server zálohování. **Tento soubor by měl být ponecháno na umístění úložiště hello vzhledem k tomu, že je požadovaná toorestore znovu ze služby Azure Storage.**
  * Pokud jste ztratili hello se zakázaným inzerováním souboru (například prostřednictvím ztrátě hello úložná média, která obsahovala hello se zakázaným inzerováním souboru) a vybrali jste možnost hello zálohování tooa účet služby Microsoft Azure Storage, obnovíte hello se zakázaným inzerováním soubor prostřednictvím služby Microsoft Azure Storage podle stáhnout z kontejneru hello úložiště, ve kterém je umístěn. Souboru se zakázaným inzerováním hello by pak umístit do složky v místním počítači hello, kde hello nástroj je nakonfigurované toohello toodetect a nahrání stejný kontejner s hello stejné heslo šifrování, pokud šifrování byl použit s původní pravidlo hello. 

To znamená, že schéma hello jak bylo popsáno výše pro novější verze systému SQL Server, můžou být přepnuté zavedené i pro verze systému SQL Server, které nejsou povolení přímé adresu umístění úložiště Azure.

Tato metoda by neměl být používá s novější verze systému SQL Server, které podporují základní až nativně Azure Storage. Výjimky jsou, kde se omezeních hello nativního zálohování do Azure blokování nativní zálohování provádění do Azure.

#### <a name="other-possibilities-toobackup-sql-server-databases"></a>Databáze systému SQL Server toobackup další možnosti.
Další možnosti toobackup databáze je tooattach další datové disky tooa virtuální počítač, který používáte toostore zálohy na. V takovém případě bude třeba toomake se, že hello disky nejsou spuštěna úplná. Pokud, je případ hello, potřebovali byste toounmount hello disky a proto toospeak 'archivu"jej a nahraďte ji metodou nového prázdného disku. Pokud přejdete dolů této cestě, chcete tookeep tyto virtuální pevné disky v samostatných účtech úložiště Azure z hello ty, které hello virtuální pevné disky s hello soubory databáze.

Druhou možností je toouse velký virtuální počítač, který může mít mnoho disky připojené, například D14 s 32VHDs. Použijte prostory úložiště toobuild flexibilní prostředí, kde je sestavení sdílených složek, jsou použity pak jako cíle zálohování pro různé servery databázového systému hello.

Některé z osvědčených postupů tu popsané [zde](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) také. 

#### <a name="performance-considerations-for-backupsrestores"></a>Důležité informace o výkonu pro zálohování a obnovování
Stejně jako u nasazení úplné obnovení je závislá na tom, kolik svazky lze číst souběžně a může být co hello propustnost těchto svazků výkonu zálohování a obnovení. Kromě toho může hello používá kompresi zálohy spotřeby procesoru přehrát významnou roli na virtuálních počítačích s právě až tooeight procesoru vláken. Proto můžete předpokládat jeden:

* Hello méně hello počet disků použitých toostore hello data souborů, hello menší hello celkovou propustnost v režimu čtení.
* Dobrý den menší hello počet vláken procesoru v hello virtuálních počítačů, hello závažnější hello dopad kompresi zálohy.
* Hello méně cílů (objekty BLOB, virtuálních pevných disků nebo disků) toowrite hello zálohování na, hello nižší úrovně propustnosti hello.
* Hello menší hello velikost virtuálního počítače, hello menší hello propustnost kvótu úložiště zápis a čtení ze služby Azure Storage. Nezávisle na tom, jestli jsou hello zálohy přímo uložené v Azure Blob, nebo zda jsou uloženy na virtuální pevné disky, které znovu ukládají do objektů BLOB Azure.

Při použití Microsoft Azure Storage BLOB jako cíl zálohování hello v novějších verzích, jste s omezeným přístupem toodesignating pouze jeden cíl adresy URL pro každé konkrétní zálohování.

Ale při použití hello "zálohování systému Microsoft SQL Server tooMicrosoft nástroj Azure" starší verze, můžete definovat více než jeden soubor cíl. S více než jeden cíl zálohování hello můžete škálovat a hello propustnost hello zálohy je vyšší. Výsledkem by pak více souborů také v hello účet úložiště Azure. V našich testech pomocí více cílů souboru jeden výborný můžete dosáhnout hello propustnosti, která může dosáhnout s příponami zálohování hello implementuje v z SQL serveru 2012 SP1 CU4 na. Můžete také nejsou blokována bránou limit 1 TB hello jako hello nativního zálohování do Azure.

Nicméně, mějte na paměti, hello propustnost je také závisí na umístění hello hello účet úložiště Azure můžete použít k zálohování hello. Účet úložiště hello toolocate v jiné oblasti než hello virtuální počítače jsou spuštěné v může být představu. Například by spustit hello konfigurace virtuálního počítače v oblasti západní Evropa, ale put hello účtu úložiště používat tooback nahoru vůči v severní Evropě. Který určitě má dopad na propustnost zálohování hello a je nepravděpodobné toogenerate propustnost 150MB/s, protože se zdá být toobe v případech, kde hello cílového úložiště a virtuální počítače hello běží v hello možné stejného místního datového centra.

#### <a name="managing-backup-blobs"></a>Správa objektů BLOB zálohy
Není zálohování hello toomanage požadavek na vlastní. Vzhledem k tomu, že hello očekává se, že mnoho objektů BLOB jsou vytvořeny tak, že spustíte zálohování časté transakčního protokolu, správu těchto objektů BLOB snadno může přetížit hello portálu Azure. Proto je recommendable tooleverage služby Azure storage Exploreru. Existuje několik dobrý ty, které jsou k dispozici, které může pomoci toomanage účet úložiště Azure

* Microsoft Visual Studio sadou Azure SDK nainstalovaný (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure Storage Explorer (<https://azure.microsoft.com/downloads/>)
* Nástroje třetích stran

Podrobnější diskuzi o zálohování a SAP v Azure, najdete v části příliš[hello SAP zálohování průvodce](sap-hana-backup-guide.md) Další informace.

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Pomocí bitové kopie systému SQL Server mimo hello Microsoft Azure Marketplace
Společnost Microsoft poskytuje virtuální počítače v hello Azure Marketplace, které již obsahují verze systému SQL Server. Pro SAP zákazníky, kteří požadují licence pro SQL Server a Windows může se jednat možnost toobasically titulní hello potřebu licencí podle roztočený až virtuální počítače se systémem SQL Server již nainstalován. Pořadí toouse takovými obrázky pro SAP, hello následující aspekty třeba toobe provedené:

* Hello systému SQL Server není zkušební verze získat vyšší náklady než právě "Pouze pro systém Windows" virtuální počítač nasadit z Azure Marketplace. Najdete v těchto článcích toocompare ceny: <https://azure.microsoft.com/pricing/details/virtual-machines/windows/> a <https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/>. 
* Můžete použít jenom verze systému SQL Server, které jsou podporovány produktem SAP, jako je SQL Server 2012.
* kolace Hello hello instance systému SQL Server, který je nainstalován ve virtuálních počítačích hello nenabízí hello Azure Marketplace není hello kolace SAP NetWeaver vyžaduje toorun instance systému SQL Server hello. I když s hello pokynů v následující části hello, můžete změnit kolaci hello.

#### <a name="changing-hello-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Změna kolace systému SQL Server z virtuálního počítače s Microsoft Windows nebo SQL Server hello
Vzhledem k tomu, že hello bitové kopie systému SQL Server v Azure Marketplace hello nejsou nastavit toouse hello kolace, který je požadován SAP NetWeaver aplikace, musí toobe okamžitě po nasazení hello změnit. Pro SQL Server 2012, to lze provést pomocí hello následující kroky při hello virtuálního počítače byla nasazena a správce může toolog do hello nasazení virtuálních počítačů:

* 'Jako správce, otevřete okno příkazu systému Windows.
* Změňte hello directory tooC:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* Provedení příkazu hello: Setup.exe/quiet nezobrazí /ACTION = REBUILDDATABASE InstanceName = MSSQLSERVER /SQLSYSADMINACCOUNTS =`<local_admin_account_name`> /SQLCOLLATION = SQL_Latin1_General_Cp850_BIN2   
  * `<local_admin_account_name`> je hello účet, který byl definován jako účet správce hello při nasazování hello virtuálních počítačů pro hello poprvé prostřednictvím Galerie hello.

proces Hello by měla pouze trvat několik minut. V pořadí toomake se, zda hello krok skončila s hello správný výsledek, proveďte hello následující kroky:

* Otevřete SQL Server Management Studio.
* Otevřete okno dotazu.
* Spustíte příkaz sp_helpsort hello v hlavní databázi systému SQL Server hello.

výsledek Hello potřeby by měl vypadat podobně jako:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Pokud není výsledek hello, ZASTAVTE nasazení SAP a zjistěte, proč hello instalačního příkazu nefunguje podle očekávání. Nasazení aplikací SAP NetWeaver do instance systému SQL Server s jinou kódové stránky systému SQL Server, než jeden zmíněné hello je **není** podporována.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server vysoká dostupnost pro SAP v Azure
Jak je uvedeno výše v tomto dokumentu, neexistuje žádné možnost toocreate sdílené úložiště, který je potřebný pro použití hello hello funkci vysoké dostupnosti nejstarší systému SQL Server. Tato funkce by nainstalujte dva nebo více instancí systému SQL Server v Windows Server Failover Cluster (WSFC) pomocí sdíleného disku pro hello uživatelských databází (a nakonec databáze tempdb). Toto je hello dlouhou dobu standardní vysoké dostupnosti metoda, kterou také podporuje SAP. Protože Azure nepodporuje sdílené úložiště, nemůže být dosaženo konfigurace systému SQL Server s vysokou dostupností s konfigurací sdíleného disku clusteru. Ale jiné metody vysoké dostupnosti jsou stále možné a jsou popsané v následující části hello.

#### <a name="sql-server-log-shipping"></a>Přesouvání protokolu systému SQL Server
Jednu z metod hello vysoké dostupnosti (HA) je přesouvání protokolu SQL serveru. Pokud virtuální počítače hello účastní hello HA konfigurace funguje překlad, žádný problém a hello instalační program v Azure se neliší od žádné nastavení, která se provádí na místě. Není doporučeno toorely na pouze IP řešení. S namapoval toosetting přesouvání protokolu a zásady hello kolem přesouvání protokolu zkontrolujte Tato dokumentace:

<https://docs.microsoft.com/SQL/Database-Engine/log-Shipping/About-log-Shipping-SQL-Server>

V pořadí tooreally dosáhnout vysoké dostupnosti proveďte, jeden musí toodeploy hello virtuálních počítačů, které jsou v rámci takové přesouvání protokolu configuration toobe v rámci hello stejné skupiny dostupnosti Azure.

#### <a name="database-mirroring"></a>Zrcadlení databáze
Zrcadlení databáze, podporuje SAP (viz poznámka SAP [965908]) využívá k definování partnera převzetí služeb při selhání v hello SAP připojovací řetězec. V případech hello mezi různými místy, předpokládáme, že hello dva virtuální počítače jsou v hello stejné doméně a tomto kontextu uživatele hello hello dva uživatele domény, který je také nejsou spuštěny instance systému SQL Server a mít dostatečná oprávnění v instance systému SQL Server hello dva spojené. Proto mezi typické místní instalace nebo konfigurační liší hello nastavení zrcadlení databáze v Azure.

Jako čistě cloudové nasazení, je nejsnazší hello toohave jinou doménu instalační program v Azure toohave tyto virtuální počítače databázového systému (a ideálně vyhrazených virtuálních počítačích SAP) v rámci jedné domény.

Pokud domény není možné, jeden můžete také použít certifikáty pro hello databázi zrcadlení koncových bodů podle postupu popsaného tady: <https://docs.microsoft.com/sql/database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql>

Kurz tooset až zrcadlení databáze v Azure naleznete zde: <https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server> 

#### <a name="sql-server-always-on"></a>Always On systému SQL Server
Always On podporuje pro SAP místní (viz poznámka SAP [1772688]), je podporované toobe použít v kombinaci s SAP v Azure. Hello fakt, že zatím nejste možné toocreate sdílené disky v Azure neznamená, že jeden nejde vytvořit konfiguraci vždy na Windows Server Failover Cluster (WSFC) mezi různé virtuální počítače. Pouze znamená, že nemáte možnost toouse hello sdíleného disku jako kvora v clusteru s hello. Proto můžete sestavit konfigurace aplikace vždy na WSFC v Azure a jednoduše nevybírejte hello kvora typ, který využívá sdílený disk. Hello prostředí Azure těchto virtuálních počítačů nasazených v měli vyřešit hello virtuální počítače podle názvu a hello virtuální počítače by měla být v hello stejné domény. To platí pro pouze Azure a mezi různými místy nasazení. Existují některé důležité informace týkající se nasazení hello SQL serveru naslouchací proces skupiny dostupnosti (ne toobe zaměňovat s hello sadu dostupnosti Azure) vzhledem k tomu, že Azure v tuto chvíli není povolena toosimply vytvoření objektu služby AD a DNS, protože je možné, místní. Proto některé jiné instalace kroky jsou nezbytné tooovercome hello konkrétní chování Azure.

Některé aspekty pomocí naslouchací proces skupiny dostupnosti jsou:

* Pomocí naslouchací proces skupiny dostupnosti je možné pouze v systému Windows Server 2012 nebo vyšší jako hostovaný operační systém hello virtuálních počítačů. Pro Windows Server 2012 je nutné toomake jistotu, že tato oprava platí: <https://support.microsoft.com/kb/2854082> 
* Pro Windows Server 2008 R2 tato oprava neexistuje a Always On potřebovat toobe používán hello stejný způsobem jako zrcadlení databáze zadáním partnera převzetí služeb při selhání v řetězci připojení hello (prostřednictvím hello SAP default.pfl parametr databáze a mss nebo serveru – viz poznámka SAP [965908]).
* Pokud pomocí naslouchací proces skupiny dostupnosti, virtuální počítače hello databáze potřebovat toobe připojené tooa vyhrazené pro vyrovnávání zatížení. Název řešení v čistě cloudové nasazení by buď vyžadují všechny virtuální počítače SAP systému (aplikační servery, databázového systému server a server (A) SCS) jsou ve stejné virtuální síti hello nebo by vyžadovaly ze SAP aplikace vrstvy hello údržby hello etc\host souboru v pořadí tooget hello virtuálních počítačů názvy hello SQL serveru, virtuálních počítačů přeložit. V pořadí tooavoid, že Azure je přiřazení nové IP adresy v případech, kdy oba virtuální počítače jsou náhodně vypnutí jeden měli přiřadit statické IP adresy síťových rozhraní toohello těchto virtuálních počítačů v hello vždy v konfiguraci (definování statickou IP adresu je popsaný v tématu [to] [ virtual-networks-reserved-private-ip] článek)

[comment]: <> (Původní blogy)
[comment]: <> (< https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx>, < https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Zvláštní kroky potřebné při vytváření konfigurace clusteru služby WSFC hello kdy hello clusteru je speciální IP adresu přiřazen, protože Azure s jeho stávající funkčnost bude přiřazen název clusteru hello hello stejnou IP adresu jako cluster hello uzlu hello je vytvořen v. To znamená, že provedení ručního kroku musí být provádět tooassign jiného clusteru toohello IP adresu.
* Hello naslouchací proces skupiny dostupnosti přechází toobe vytvoří v Azure pomocí koncových bodů protokolu TCP/IP, které jsou přiřazeny toohello virtuální počítače se systémem hello primární a sekundární repliky skupiny dostupnosti hello.
* Může být nutné toosecure tyto koncové body pomocí seznamů řízení přístupu.

[comment]: <> (Blog staré TODO)
[comment]: <> (Hello podrobné kroky a životní potřeby instalace konfigurace aplikace AlwaysOn v Azure jsou nejpohodlnější, když s návodem hello kurz k dispozici [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[comment]: <> (Předkonfigurované nastavení AlwaysOn prostřednictvím hello Azure galerii < https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[comment]: <> (Vytváření naslouchací proces skupiny dostupnosti se nejlépe popisuje v kurzu [this][virtual-machines-windows-classic-ps-sql-int-listener])
[comment]: <> (Zabezpečení koncových bodů sítě s seznamy ACL vysvětlení najdete nejlépe tady:)
[comment]: <> (* < https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[comment]: <> (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>)
[comment]: <> (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[comment]: <> (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Je možné toodeploy SQL Server vždy na skupině dostupnosti v různých oblastech Azure také. Tato funkce využívá hello Azure VNet-to-Vnet připojení ([podrobnosti][virtual-networks-configure-vnet-to-vnet-connection]).

[comment]: <> (Blog staré TODO)
[comment]: <> (Instalační program Hello skupin dostupnosti AlwaysOn serveru SQL v takové situaci je zde popsán: < https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>Souhrn na vysoké dostupnosti SQL serveru v Azure
Vzhledem tomu hello, že Azure Storage chrání obsah hello, na bitovou kopii hot standby je jeden menší tooinsist důvod. To znamená, že váš scénář vysoké dostupnosti musí tooonly ochranu proti hello následujících případech:

* Nedostupnosti hello virtuálního počítače jako celek kvůli toomaintenance v clusteru serveru hello v Azure nebo z jiných důvodů
* Problémů se softwarem v instanci systému SQL Server hello
* Ochrana proti ruční chyba, kde se odstranila data a je potřeba obnovení bodu v čase

Prohlížení odpovídající technologií, pomocí kterých jeden můžete uvádějí, že první dva případy hello se dá pokrýt komponentami zrcadlení databáze nebo Always On, zatímco třetí případ hello pouze se dá pokrýt komponentami přesouvání protokolu.

Je třeba toobalance hello složitější nastavení Always On, porovnání tooDatabase zrcadlení s hello výhody Always On. Tyto výhody může být uvedený jako:

* Čitelných místních replikách.
* Zálohování z sekundární repliky.
* Lepší škálovatelnost.
* Více než jednu sekundární repliky.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Obecné SQL Server pro SAP v Azure souhrnu
Existuje mnoho doporučení v tomto průvodci a doporučujeme přečtěte si ho více než jednou před plánování vašeho nasazení Azure. Obecně platí ale být jisti toofollow hello top deset obecné databázového systému na Azure konkrétní body:

[comment]: <> (2.3 vyšší propustnost než co? Než jeden virtuální pevný disk?)
1. Použijte hello nejnovější databázového systému vydání, jako je SQL Server 2014, která má většina výhod hello v Azure. Pro systém SQL Server to je SQL Server 2012 SP1 CU4, která bude zahrnovat hello funkce zálohování zobrazení Azure Storage. Ve spojení s SAP by doporučujeme však minimálně SQL Server 2014 SP1 CU1 nebo SQL Server 2012 SP2 a hello nejnovější CU.
2. Pečlivě naplánujte vašeho systému SAP na šířku v Azure omezení a rozložení Azure toobalance hello datového souboru:
   * Není k dispozici příliš mnoho disků, ale mají dostatek tooensure nedostanete vaší požadované IOPS.
   * Pokud nepoužijete spravované disky, mějte na paměti, že IOPS jsou také omezené na účet úložiště Azure a omezeny účty úložiště v rámci každé předplatné Azure ([podrobnosti][azure-subscription-service-limits]). 
   * Pouze stripe na discích, pokud potřebujete tooachieve vyšší propustnost.
3. Software nemá nikdy instalovat nebo blokovat všechny soubory, které vyžadují trvalost na hello jednotku D:\, jako je jiný trvalé a nic na této jednotce dojde ke ztrátě při restartování systému Windows.
4. Nepoužívejte ukládání do mezipaměti na disku pro Azure Standard Storage.
5. Nepoužívejte Azure geograficky replikované úložiště účtů.  Místně redundantní použijte pro úlohy databázového systému.
6. Použijte data databáze tooreplicate řešení HA/DR od dodavatele databázového systému.
7. Vždy můžete použít překlad, nespoléhejte na IP adresy.
8. Použijte hello nejvyšší databáze komprese možné. Pro systém SQL Server Toto je stránka komprese.
9. Dávejte pozor, pomocí bitové kopie systému SQL Server z hello Azure Marketplace. Pokud používáte hello systému SQL Server, jednu, musíte změnit kompletování instance hello před instalací jakékoli systému SAP NetWeaver na něm.
10. Instalace a konfigurace hello SAP hostitele monitorování pro Azure, jak je popsáno v [Průvodce nasazením][deployment-guide].

## <a name="specifics-toosap-ase-on-windows"></a>Specifika tooSAP App Service Environment v systému Windows
Od verze Microsoft Azure, můžete snadno migrovat vaší existující tooAzure aplikace app Service Environment SAP virtuálních počítačů. SAP App Service Environment ve virtuálním počítači můžete tooreduce hello celkové náklady na vlastnictví nasazení, správu a údržbu enterprise spektra aplikací umožňuje snadno migrací tooMicrosoft tyto aplikace Azure. SAP App Service Environment ve virtuální počítač Azure správci a vývojáři stále pomocí hello stejných nástrojů vývoj a správu, které jsou k dispozici místně.

Je SLA pro hello virtuální počítače Azure, které naleznete zde: <https://azure.microsoft.com/support/legal/sla/virtual-machines>

Jsme si jisti, že Microsoft Azure hostované virtuální počítače provede velmi dobře v porovnání tooother veřejného cloudu virtualizace nabídek, ale jednotlivé výsledky se může lišit. Změna velikosti protokoly SAP počty hello různých SAP certifikované SKU virtuálního počítače je k dispozici v samostatné Poznámka SAP SAP [1928533].

Příkazy a doporučení v ohledem toohello využití Azure Storage, nasazení SAP prostředků virtuálních počítačů nebo monitorování SAP použít toodeployments SAP App Service Environment ve spojení s aplikací SAP, jak je uvedeno v rámci hello první čtyři kapitol tohoto dokumentu.

### <a name="sap-ase-version-support"></a>Podpora verzí App Service Environment SAP
SAP aktuálně podporuje SAP App Service Environment verze 16.0 pro použití s produkty SAP Business Suite. Všechny aktualizace pro server App Service Environment SAP nebo JDBC a rozhraní ODBC toobe ovladače použít s produkty jsou poskytovány výhradně prostřednictvím SAP Business Suite hello Marketplace SAP Service na: <https://support.sap.com/swdc>.

Jako u instalací na místních počítačích nestahovat přímo z databáze Sybase webů aktualizací pro server hello SAP App Service Environment, nebo hello JDBC a ovladačů ODBC. Podrobné informace o opravy, které jsou podporovány pro použití s SAP Business Suite produkty místně a v Azure Virtual Machines najdete v části hello následující SAP poznámky:

* [1590719]
* [1973241]

Obecné informace o spouštění SAP Business Suite v App Service Environment SAP naleznete v hello [oznámení změny stavu](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Pokyny pro konfigurace SAP App Service Environment pro SAP související SAP instalace App Service Environment ve virtuálních počítačích Azure
#### <a name="structure-of-hello-sap-ase-deployment"></a>Struktura hello nasazení SAP App Service Environment
V souladu s hello obecný popis, by měla být App Service Environment SAP spustitelné soubory umístěné nebo nainstalován do hello systémové jednotce hello Virtuálního počítače disk operačního systému (jednotka c:\). Obvykle většinu hello databáze systému a nástroje pro SAP App Service Environment nejsou využít skutečně pevného SAP NetWeaver zatížení. Proto hello systému a nástroje pro databáze (master, model, saptools, sybmgmtdb, sybsystemdb) může zůstat na jednotce C:\ hello také. 

Výjimka může být hello dočasné databáze obsahující všechny pracovní tabulky a vytvořit pomocným SAP, který v případě některých ERP SAP a všechny úlohy BW může vyžadovat vyšší datový svazek nebo vstupně-výstupní operace svazek, který se nemůže vejít do hello původní dočasných tabulek Disk s operačním systémem Virtuálního počítače (jednotka c:\).

V závislosti na hello používá verzi SAPInst nebo SWPM tooinstall hello systému, může obsahovat hello databáze:

* Jeden tempdb SAP App Service Environment, která je vytvořena při instalaci SAP App Service Environment
* App Service Environment SAP tempdb vytvořené instalace SAP App Service Environment a další saptempdb vytvořené hello SAP instalačního programu
* App Service Environment SAP tempdb vytvořené instalace SAP App Service Environment a další databáze tempdb, který byl vytvořen ručně (například následující poznámka SAP [1752266]) toomeet ERP/BW konkrétní databázi tempdb požadavky

V případě konkrétní ERP nebo všechny úlohy BW má smysl, v ohledem tooperformance tookeep hello databáze tempdb zařízení hello kromě vytvoření databáze tempdb (SWPM nebo ručně) na jiné jednotce než C:\. Pokud žádné další databáze tempdb existuje, je vhodné toocreate jeden (Poznámka SAP [1752266]).

Pro tyto systémy hello by měla pro hello kromě vytvořit databázi tempdb provést následující kroky:

* Přesunout hello první databáze tempdb zařízení toohello první zařízení databáze SAP hello
* Přidání databáze tempdb zařízení tooeach Dobrý den virtuální pevné disky obsahující zařízení databáze SAP hello

Tato konfigurace umožňuje databáze tempdb tooeither využívat více místa, než je možné tooprovide hello systémová jednotka. Jako referenci jeden můžete zkontrolovat hello databáze tempdb zařízení velikosti na stávajících systémů, které spustit místně. Nebo taková konfigurace by povolte IOPS čísla proti databázi tempdb nelze zadat s hello systémová jednotka. Systémy, které jsou místní může být znovu použít toomonitor vstupně-výstupní úlohy proti databázi tempdb.

Nikdy uveďte veškerá zařízení, která App Service Environment SAP do hello jednotce D:\ hello virtuálních počítačů. To platí toohello tempdb, i v případě hello objekty zachovány v databázi tempdb hello jsou pouze dočasné.

#### <a name="impact-of-database-compression"></a>Dopad komprese databáze
V konfiguracích, kde vstupně-výstupní šířky pásma může představovat problém mohou pomoci při každé míry, což snižuje IOPS toostretch hello úlohy, jež možné spouštět v případě pomocí IaaS, jako je například Azure. Proto důrazně doporučujeme toomake jistotu, že se používá komprese App Service Environment SAP před nahráním existující databázi tooAzure SAP.

komprese tooperform doporučení Hello před nahráním tooAzure, pokud již není implementována je dán z několika důvodů:

* je menší množství Hello tooAzure toobe nahrát data
* Doba trvání Hello provádění komprese hello je kratší, za předpokladu, že jeden může používat silnější hardware s více procesorů nebo větší šířku pásma vstupně-výstupních operací nebo méně vstupně-výstupních operací latence místně
* Menší velikosti databáze může vést tooless náklady pro přidělení disku

Komprese dat a obchodní fungovat na virtuální počítač hostovaný v Azure Virtual Machines, stejně jako místní. Další informace o tom, jak toocheck Pokud komprese je již používán v existující databázi SAP App Service Environment, zkontrolujte Poznámka SAP [1750510].

#### <a name="using-dbacockpit-toomonitor-database-instances"></a>Pomocí DBACockpit toomonitor databáze instancí
Pro systémy, SAP, které používají jako platformu databázi SAP App Service Environment, je dostupné jako windows embedded prohlížeče v transakci DBACockpit nebo Webdynpro hello DBACockpit. Ale hello úplné funkce pro monitorování a správa hello databáze je k dispozici v implementaci Webdynpro hello hello DBACockpit pouze.

Jako místní systémy, které jsou několik kroků vyžadován tooenable všechny funkce SAP NetWeaver používané hello Webdynpro provádění hello DBACockpit. Postupujte podle Poznámka SAP [1245200] tooenable hello využití webdynpros a generovat hello těch, které jsou potřeba. Pokud postupovat hello pokynů hello výše poznámky, můžete také nakonfigurovat hello Správce internetové komunikace (icm) společně s toobe porty hello používá pro připojení http a https. Hello výchozí nastavení pro protokol http vypadá takto:

> ICM/server_port_0 = ochranu = HTTP, PORT = 8000 PROCTIMEOUT = 600, vypršení časového LIMITU = 600
> 
> ICM/server_port_1 = ochranu = protokolu HTTPS, PORT = 443$ $, PROCTIMEOUT = 600, vypršení časového LIMITU = 600
> 
> 

a odkazy hello vygenerované v transakci DBACockpit bude vypadat podobně jako toothis:

> https://`<fullyqualifiedhostname`>: 44300/sap nebo bc/webdynpro/sap nebo dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/sap nebo bc/webdynpro/sap nebo dba_cockpit
> 
> 

V závislosti na zda a jak hello virtuálního počítače Azure hostování hello systému SAP je připojená přes site-to-site, více lokalit nebo ExpressRoute (mezi různými místy nasazení), je nutné toomake se, že ICM používá plně kvalifikovaný název hostitele, který lze převést na hello počítače, které se pokoušíte tooopen hello DBACockpit z. Viz poznámka SAP [773830] toounderstand Určuje, jak ICM hello plně kvalifikovaný název hostitele v závislosti na parametry profil a sadu parametr icm/host_name_full explicitně v případě potřeby.

Pokud jste nasadili hello virtuálních počítačů ve scénáři jenom pro Cloud bez připojení mezi různými místy mezi místními a Azure, budete potřebovat toodefine veřejnou IP adresu a domainlabel. Formát Hello hello veřejného názvu DNS hello virtuálních počítačů vypadá takto:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com
> 
> 

Další podrobnosti související naleznete název DNS toohello [sem][virtual-machines-azurerm-versus-azuresm].

Nastavení hello SAP profil parametr icm/host_name_full toohello, který může vypadat podobně jako název DNS hello virtuálního počítače Azure hello odkaz:

> https://mydomainlabel.westeurope.cloudapp.NET:44300/sap nebo bc/webdynpro/sap nebo dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.NET:8000/sap nebo bc/webdynpro/sap nebo dba_cockpit
> 
> 

V takovém případě je třeba toomake nezapomeňte:

* Přidání toohello příchozích pravidel skupiny zabezpečení sítě v hello portál Azure pro porty používané toocommunicate hello TCP/IP s ICM
* Přidat konfiguraci brány Windows Firewall toohello příchozích pravidel pro toocommunicate používané porty TCP/IP hello s hello ICM

Automatickou importované všechny opravy, které jsou k dispozici, je doporučeno použít tooperiodically hello oprava kolekce Poznámka SAP použít tooyour SAP verze:

* [1558958]
* [1619967]
* [1882376]

Další informace o DBA řídící panel pro App Service Environment SAP naleznete v následující poznámky k SAP hello:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Důležité informace o zálohování nebo obnovení pro SAP App Service Environment
Při nasazování App Service Environment SAP do Azure musí být zkontrolovány vaše zálohování metodika. I v případě, že systém hello není systémem produktivní, hello SAP databázi hostované SAP App Service Environment musí být zálohovat pravidelně. Vzhledem k tomu, že Azure úložiště udržuje tři bitové kopie, je nyní méně důležité v ohledem toocompensating havárie úložiště zálohy. Hello hlavním důvodem pro údržbu správné plán zálohování a obnovení je větší, můžete kompenzovat chyby logické nebo ruční tím, že poskytuje bod v možnosti v době obnovení. Tak, aby hello cílem tooeither použití zálohy toorestore hello zálohování databáze tooa určité bodu v čase nebo toouse hello záloh v Azure tooseed jiného systému zkopírováním hello existující databázi. Například je může přenést z vrstvě 2 SAP konfigurace tooa 3vrstvé systému nastavení hello stejné systému obnovení ze zálohy.

Zálohování a obnovení databáze v Azure funguje hello stejný způsobem jako místní. Naleznete v poznámkách k SAP:

* [1588316]
* [1585981]

Podrobné informace o vytváření výpis konfigurace a plánování zálohování. V závislosti na vaše požadavky, které můžete konfigurovat a strategie databáze a protokolu výpisy toodisk na jednom z existujících disků hello nebo přidejte další disk pro zálohování hello. tooreduce hello nebezpečí dojít ke ztrátě dat v případě chyby, je doporučeno toouse disku, kde se nachází žádné databáze zařízení.

Kromě dat a obchodní komprese App Service Environment SAP také nabízí kompresi zálohy. Vypíše toooccupy méně místa s hello databáze a protokolu se doporučuje toouse kompresi zálohy. Další informace viz poznámka SAP [1588316]. Komprese zálohy hello je také velmi důležitý tooreduce hello množství dat toobe přenést, pokud máte v plánu zálohování toodownload nebo virtuální pevné disky obsahující zálohování výpisy z hello tooon místní virtuální počítač Azure.

Nepoužívejte jednotce D:\ jako cíl výpisu databázi nebo protokolu.

#### <a name="performance-considerations-for-backupsrestores"></a>Důležité informace o výkonu pro zálohování a obnovování
Stejně jako u nasazení úplné obnovení je závislá na tom, kolik svazky lze číst souběžně a může být co hello propustnost těchto svazků výkonu zálohování a obnovení. Kromě toho může hello používá kompresi zálohy spotřeby procesoru přehrát významnou roli na virtuálních počítačích s právě až tooeight procesoru vláken. Proto můžete předpokládat jeden:

* Hello méně hello počtu toostore disky použité hello zařízení databáze, hello menší hello celková propustnost v čtení
* Dobrý den menší hello počet vláken procesoru v hello virtuálních počítačů, hello závažnější hello dopad kompresi zálohy
* Dobrý den méně cílů (Stripe adresáře, disků) toowrite hello zálohování, hello nižší úrovně propustnosti hello

tooincrease hello počet toothere toowrite cíle jsou dvě možnosti, které lze použít nebo kombinaci v závislosti na vašich potřeb:

* Prokládání hello zálohování cílový svazek přes více připojené disky v pořadí tooimprove hello IOPS propustnosti na tomto svazku prokládané
* Vytvoření výpisu konfigurace na úrovni SAP App Service Environment, který používá více než jeden cílový adresář toowrite hello výpis do

Prokládání svazek přes více připojené disky má popsané výše v této příručce. Další informace o používání více adresářů v konfiguraci výpisu hello SAP App Service Environment, naleznete v dokumentaci toohello na sp_config_dump uloženou proceduru, což je použité toocreate hello výpis konfigurace na hello [informační Sybase středisko](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Zotavení po havárii s virtuálními počítači Azure
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replikace dat s SAP Sybase replikace Server
S hello App Service Environment SAP SAP Sybase replikace serveru (SRS) poskytuje na záložním pohotovostním řešení tootransfer databáze transakce tooa vzdálené umístění asynchronně. 

instalace Hello a operace SRS funguje i funkčně ve virtuálním počítači, který je hostitelem služby virtuálního počítače Azure stejně jako místní.

App Service Environment HADR prostřednictvím serveru SAP replikace je plánované v budoucí verzi. Bude testovány s a vydání pro platformy Microsoft Azure, jakmile je k dispozici.

## <a name="specifics-toosap-ase-on-linux"></a>Specifika tooSAP App Service Environment v systému Linux
Od verze Microsoft Azure, můžete snadno migrovat vaší existující tooAzure aplikace app Service Environment SAP virtuálních počítačů. SAP App Service Environment ve virtuálním počítači můžete tooreduce hello celkové náklady na vlastnictví nasazení, správu a údržbu enterprise spektra aplikací umožňuje snadno migrací tooMicrosoft tyto aplikace Azure. SAP App Service Environment ve virtuální počítač Azure správci a vývojáři stále pomocí hello stejných nástrojů vývoj a správu, které jsou k dispozici místně.

Pro nasazení virtuálních počítačů Azure důležité tooknow hello oficiální SLA, které naleznete zde: <https://azure.microsoft.com/support/legal/sla>

Informace o nastavení velikosti SAP a seznam SAP certifikované SKU virtuální počítač je součástí Poznámka SAP [1928533]. Změna velikosti dokumentů pro virtuálními počítači Azure je zde uveden další SAP <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> a zde <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Příkazy a doporučení v ohledem toohello využití Azure Storage, nasazení SAP prostředků virtuálních počítačů nebo monitorování SAP použít toodeployments SAP App Service Environment ve spojení s aplikací SAP, jak je uvedeno v rámci hello první čtyři kapitol tohoto dokumentu.

Hello následující dvě poznámky SAP zahrnout obecné informace o App Service Environment na Linuxu a App Service Environment hello cloudu:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>Podpora verzí App Service Environment SAP
SAP aktuálně podporuje SAP App Service Environment verze 16.0 pro použití s produkty SAP Business Suite. Všechny aktualizace pro server App Service Environment SAP nebo JDBC a rozhraní ODBC toobe ovladače použít s produkty jsou poskytovány výhradně prostřednictvím SAP Business Suite hello Marketplace SAP Service na: <https://support.sap.com/swdc>.

Jako u instalací na místních počítačích nestahovat přímo z databáze Sybase webů aktualizací pro server hello SAP App Service Environment, nebo hello JDBC a ovladačů ODBC. Podrobné informace o opravy, které jsou podporovány pro použití s SAP Business Suite produkty místně a v Azure Virtual Machines najdete v části hello následující SAP poznámky:

* [1590719]
* [1973241]

Obecné informace o spouštění SAP Business Suite v App Service Environment SAP naleznete v hello [oznámení změny stavu](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Pokyny pro konfigurace SAP App Service Environment pro SAP související SAP instalace App Service Environment ve virtuálních počítačích Azure
#### <a name="structure-of-hello-sap-ase-deployment"></a>Struktura hello nasazení SAP App Service Environment
V souladu s hello obecný popis by měla být App Service Environment SAP spustitelné soubory umístěné nebo nainstalován do systému souborů kořenové hello hello virtuálních počítačů (/sybase). Obvykle většinu hello databáze systému a nástroje pro SAP App Service Environment nejsou využít skutečně pevného SAP NetWeaver zatížení. Proto hello systému a nástroje pro databáze (master, model, saptools, sybmgmtdb, sybsystemdb) může zůstat v hello kořenové systému souborů i. 

Výjimka může být hello dočasné databáze obsahující všechny pracovní tabulky a vytvořit pomocným SAP, v případě některých ERP SAP a všechny úlohy BW může to vyžadovat vyšší datový svazek nebo vstupně-výstupních operací svazek, který se nevejde do hello původní dočasných tabulek Disk s operačním systémem Virtuálního počítače.

V závislosti na hello používá verzi SAPInst nebo SWPM tooinstall hello systému, může obsahovat hello databáze:

* Jeden tempdb SAP App Service Environment, která je vytvořena při instalaci SAP App Service Environment
* App Service Environment SAP tempdb vytvořené instalace SAP App Service Environment a další saptempdb vytvořené hello SAP instalačního programu
* App Service Environment SAP tempdb vytvořené instalace SAP App Service Environment a další databáze tempdb, který byl vytvořen ručně (například následující poznámka SAP [1752266]) toomeet ERP/BW konkrétní databázi tempdb požadavky

V případě konkrétní ERP nebo všechny úlohy BW má smysl, v ohledem tooperformance tookeep hello databáze tempdb zařízení tempdb hello kromě vytvořili (SWPM nebo ručně) v systému samostatného souboru, který může být reprezentovaný jednoho Azure datový disk nebo Linux RAID práci s více disky dat Azure. Pokud žádné další databáze tempdb existuje, je vhodné toocreate jeden (Poznámka SAP [1752266]).

Pro tyto systémy hello by měla pro hello kromě vytvořit databázi tempdb provést následující kroky:

* Přesunout hello první databáze tempdb directory toohello první systém souborů databáze SAP hello
* Přidání databáze tempdb adresáře tooeach hello disků obsahující systému souborů databáze SAP hello

Tato konfigurace umožňuje databáze tempdb tooeither využívat více místa, než je možné tooprovide hello systémová jednotka. Jako referenci jeden můžete zkontrolovat velikosti adresář databáze tempdb hello na stávajících systémů, které spustit místně. Nebo taková konfigurace by povolte IOPS čísla proti databázi tempdb nelze zadat s hello systémová jednotka. Systémy, které jsou místní může být znovu použít toomonitor vstupně-výstupní úlohy proti databázi tempdb.

Nikdy uveďte všechny adresáře App Service Environment SAP do /mnt nebo /mnt/resource hello virtuálních počítačů. To platí toohello tempdb, i v případě hello objekty zachovány v databázi tempdb hello jsou pouze dočasné, protože /mnt nebo /mnt/resource je k výchozí virtuální počítač Azure dočasného prostoru, což není trvalý. Další podrobnosti o hello dočasného prostoru virtuálního počítače Azure najdete v [v tomto článku][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Dopad komprese databáze
V konfiguracích, kde vstupně-výstupní šířky pásma může představovat problém mohou pomoci při každé míry, což snižuje IOPS toostretch hello úlohy, jež možné spouštět v případě pomocí IaaS, jako je například Azure. Proto důrazně doporučujeme toomake jistotu, že se používá komprese App Service Environment SAP před nahráním existující databázi tooAzure SAP.

komprese tooperform doporučení Hello před nahráním tooAzure, pokud již není implementována je dán z několika důvodů:

* je menší množství Hello tooAzure toobe nahrát data
* Doba trvání Hello provádění komprese hello je kratší, za předpokladu, že jeden může používat silnější hardware s více procesorů nebo větší šířku pásma vstupně-výstupních operací nebo méně vstupně-výstupních operací latence místně
* Menší velikosti databáze může vést tooless náklady pro přidělení disku

Komprese dat a obchodní fungovat na virtuální počítač hostovaný v Azure Virtual Machines, stejně jako místní. Další informace o tom, jak toocheck Pokud komprese je již používán v existující databázi SAP App Service Environment, zkontrolujte Poznámka SAP [1750510]. Další informace týkající se databáze komprese, viz poznámka SAP [2121797].

#### <a name="using-dbacockpit-toomonitor-database-instances"></a>Pomocí DBACockpit toomonitor databáze instancí
Pro systémy, SAP, které používají jako platformu databázi SAP App Service Environment, je dostupné jako windows embedded prohlížeče v transakci DBACockpit nebo Webdynpro hello DBACockpit. Ale hello úplné funkce pro monitorování a správa hello databáze je k dispozici v implementaci Webdynpro hello hello DBACockpit pouze.

Jako místní systémy, které jsou několik kroků vyžadován tooenable všechny funkce SAP NetWeaver používané hello Webdynpro provádění hello DBACockpit. Postupujte podle Poznámka SAP [1245200] tooenable hello využití webdynpros a generovat hello těch, které jsou potřeba. Pokud postupovat hello pokynů hello výše poznámky, můžete také nakonfigurovat hello Správce internetové komunikace (icm) společně s toobe porty hello používá pro připojení http a https. Hello výchozí nastavení pro protokol http vypadá takto:

> ICM/server_port_0 = ochranu = HTTP, PORT = 8000 PROCTIMEOUT = 600, vypršení časového LIMITU = 600
> 
> ICM/server_port_1 = ochranu = protokolu HTTPS, PORT = 443$ $, PROCTIMEOUT = 600, vypršení časového LIMITU = 600
> 
> 

a odkazy hello vygenerované v transakci DBACockpit bude vypadat podobně jako toothis:

> https://`<fullyqualifiedhostname`>: 44300/sap nebo bc/webdynpro/sap nebo dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/sap nebo bc/webdynpro/sap nebo dba_cockpit
> 
> 

V závislosti na zda a jak hello virtuálního počítače Azure hostování hello systému SAP je připojená přes site-to-site, více lokalit nebo ExpressRoute (mezi různými místy nasazení), je nutné toomake se, že ICM používá plně kvalifikovaný název hostitele, který lze převést na hello počítače, které se pokoušíte tooopen hello DBACockpit z. Viz poznámka SAP [773830] toounderstand Určuje, jak ICM hello plně kvalifikovaný název hostitele v závislosti na parametry profil a sadu parametr icm/host_name_full explicitně v případě potřeby.

Pokud jste nasadili hello virtuálních počítačů ve scénáři jenom pro Cloud bez připojení mezi různými místy mezi místními a Azure, budete potřebovat toodefine veřejnou IP adresu a domainlabel. Formát Hello hello veřejného názvu DNS hello virtuálních počítačů vypadá takto:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com
> 
> 

Další podrobnosti související naleznete název DNS toohello [sem][virtual-machines-azurerm-versus-azuresm].

Nastavení hello SAP profil parametr icm/host_name_full toohello, který může vypadat podobně jako název DNS hello virtuálního počítače Azure hello odkaz:

> https://mydomainlabel.westeurope.cloudapp.NET:44300/sap nebo bc/webdynpro/sap nebo dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.NET:8000/sap nebo bc/webdynpro/sap nebo dba_cockpit
> 
> 

V takovém případě je třeba toomake nezapomeňte:

* Přidání toohello příchozích pravidel skupiny zabezpečení sítě v hello portál Azure pro porty používané toocommunicate hello TCP/IP s ICM
* Přidat konfiguraci brány Windows Firewall toohello příchozích pravidel pro toocommunicate používané porty TCP/IP hello s hello ICM

Automatickou importované všechny opravy, které jsou k dispozici, je doporučeno použít tooperiodically hello oprava kolekce Poznámka SAP použít tooyour SAP verze:

* [1558958]
* [1619967]
* [1882376]

Další informace o DBA řídící panel pro App Service Environment SAP naleznete v následující poznámky k SAP hello:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Důležité informace o zálohování nebo obnovení pro SAP App Service Environment
Při nasazování App Service Environment SAP do Azure musí být zkontrolovány vaše zálohování metodika. I v případě, že systém hello není systémem produktivní, hello SAP databázi hostované SAP App Service Environment musí být zálohovat pravidelně. Vzhledem k tomu, že Azure úložiště udržuje tři bitové kopie, je nyní méně důležité v ohledem toocompensating havárie úložiště zálohy. Hello hlavním důvodem pro údržbu správné plán zálohování a obnovení je větší, můžete kompenzovat chyby logické nebo ruční tím, že poskytuje bod v možnosti v době obnovení. Tak, aby hello cílem tooeither použití zálohy toorestore hello zálohování databáze tooa určité bodu v čase nebo toouse hello záloh v Azure tooseed jiného systému zkopírováním hello existující databázi. Například je může přenést z vrstvě 2 SAP konfigurace tooa 3vrstvé systému nastavení hello stejné systému obnovení ze zálohy.

Zálohování a obnovení databáze v Azure funguje hello stejný způsobem jako místní. Naleznete v poznámkách k SAP:

* [1588316]
* [1585981]

Podrobné informace o vytváření výpis konfigurace a plánování zálohování. V závislosti na vaše požadavky, které můžete konfigurovat a strategie databáze a protokolu výpisy toodisk na jednom z existujících disků hello nebo přidejte další disk pro zálohování hello. tooreduce hello nebezpečí dojít ke ztrátě dat v případě chyby je doporučeno toouse disku, kde je umístěn žádný adresář nebo soubor databáze.

Kromě dat a obchodní komprese App Service Environment SAP také nabízí kompresi zálohy. Vypíše toooccupy méně místa s hello databáze a protokolu se doporučuje toouse kompresi zálohy. Další informace viz poznámka SAP [1588316]. Komprese zálohy hello je také velmi důležitý tooreduce hello množství dat toobe přenést, pokud máte v plánu zálohování toodownload nebo virtuální pevné disky obsahující zálohování výpisy z hello tooon místní virtuální počítač Azure.

Nepoužívejte hello virtuálního počítače Azure dočasného prostoru /mnt nebo /mnt/resource jako cíl výpisu databázi nebo protokolu.

#### <a name="performance-considerations-for-backupsrestores"></a>Důležité informace o výkonu pro zálohování a obnovování
Stejně jako u nasazení úplné obnovení je závislá na tom, kolik svazky lze číst souběžně a může být co hello propustnost těchto svazků výkonu zálohování a obnovení. Kromě toho může hello používá kompresi zálohy spotřeby procesoru přehrát významnou roli na virtuálních počítačích s právě až tooeight procesoru vláken. Proto můžete předpokládat jeden:

* Hello méně hello počtu toostore disky použité hello zařízení databáze, hello menší hello celková propustnost v čtení
* Dobrý den menší hello počet vláken procesoru v hello virtuálních počítačů, hello závažnější hello dopad kompresi zálohy
* Hello méně cílů (Linux softwaru diskového pole RAID, disků) toowrite hello zálohování na, hello nižší úrovně propustnosti hello

tooincrease hello počet toothere toowrite cíle jsou dvě možnosti, které lze použít nebo kombinaci v závislosti na vašich potřeb:

* Prokládání hello zálohování cílový svazek přes více připojené disky v pořadí tooimprove hello IOPS propustnosti na tomto svazku prokládané
* Vytvoření výpisu konfigurace na úrovni SAP App Service Environment, který používá více než jeden cílový adresář toowrite hello výpis do

Prokládání svazek přes více připojené disky má popsané výše v této příručce. Další informace o používání více adresářů v konfiguraci výpisu hello SAP App Service Environment, naleznete v dokumentaci toohello na sp_config_dump uloženou proceduru, což je použité toocreate hello výpis konfigurace na hello [informační Sybase středisko](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Zotavení po havárii s virtuálními počítači Azure
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replikace dat s SAP Sybase replikace Server
S hello App Service Environment SAP SAP Sybase replikace serveru (SRS) poskytuje na záložním pohotovostním řešení tootransfer databáze transakce tooa vzdálené umístění asynchronně. 

instalace Hello a operace SRS funguje i funkčně ve virtuálním počítači, který je hostitelem služby virtuálního počítače Azure stejně jako místní.

App Service Environment HADR prostřednictvím serveru SAP replikace není podporována v daném okamžiku. Může být testovány s a vydání pro platformy Microsoft Azure v budoucnu hello.

## <a name="specifics-toooracle-database-on-windows"></a>Specifika tooOracle databáze v systému Windows
Oracle softwaru podporuje toorun Oracle na Microsoft Windows Hyper-V a Azure. Podrobnosti o hello obecné podpoře Windows Hyper-V a Azure, zkontrolujte: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Následující obecné podporu hello je také podporována hello konkrétní scénář aplikací SAP, Oracle – databáze využití. Podrobnosti jsou pojmenované v této části dokumentu hello.

### <a name="oracle-version-support"></a>Podpora verzí Oracle
Verze Oracle a odpovídající verze operačního systému, které jsou podporovány pro SAP systémem Oracle na virtuálních počítačích Azure lze nalézt v Poznámka SAP [2039619].

Obecné informace o spuštění SAP Business Suite na Oracle naleznete v 1DX: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Pokyny pro konfigurace Oracle pro SAP instalace ve virtuálních počítačích Azure
#### <a name="storage-configuration"></a>Konfigurace úložiště
Pouze jednu instanci Oracle pomocí NTFS naformátovaný disků je podporována. Všechny soubory databáze musí být uložené na hello založené na virtuálních pevných disků nebo disků spravované systému souborů NTFS. Tyto disky připojené toohello virtuální počítač Azure a jsou založené na úložiště objektů BLOB stránky Azure (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) nebo spravované disky (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Jakýkoli druh síťové jednotky nebo vzdálených sdílených složkách, jako je Azure souborových služeb:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

jsou **není** podporované pro soubory databáze Oracle!

Pomocí disků na základě úložiště objektů BLOB stránky Azure nebo spravovat disky, hello příkazy provedené v tomto dokumentu v kapitole [ukládání do mezipaměti pro virtuální počítače a datové disky] [ dbms-guide-2.1] a [Microsoft Azure Storage] [ dbms-guide-2.3] použít toodeployments s také hello databáze Oracle.

Jak je popsáno výše v části Obecné hello hello dokumentu, existují kvóty na propustnost IOPS pro disky systému Azure. přesný kvóty Hello se v závislosti na typu hello virtuálních počítačů používají. Seznam typů virtuálních počítačů s jejich kvóty najdete [zde (Linux)] [ virtual-machines-sizes-linux] a [zde (Windows)][virtual-machines-sizes-windows].

tooidentify hello podporované typy virtuálního počítače Azure, získáte Poznámka tooSAP [1928533].

Tak dlouho, dokud hello aktuální kvóty IOPS na disk splňuje požadavky hello, je možné toostore všechny soubory databáze na jednom disku jedné připojené hello. 

Pokud jsou vyžadovány další IOPS, důrazně doporučujeme toouse okno fondy úložiště (pouze k dispozici v systému Windows Server 2012 a vyšší) nebo Windows pro systém Windows 2008 R2 toocreate prokládání jedno velké logické zařízení přes více připojené disky (viz také kapitoly [Softwaru diskového pole RAID] [ dbms-guide-2.2] tohoto dokumentu). Tento přístup zjednodušuje hello správy režijní toomanage hello místa a zabraňuje hello úsilí toomanually distribuovat soubory do více připojené disky.

#### <a name="backup--restore"></a>Backup / obnovení
Pro zálohování / obnovit funkčnost, dobrý den SAP BR * nástroje pro Oracle jsou podporovány ve stejné hello způsobem jako na standardní operační systémy Windows Server a Hyper-V. Správce obnovení Oracle (RMAN) je také podporována pro toodisk zálohování a obnovení z disku.

#### <a name="high-availability"></a>Vysoká dostupnost
Oracle Data Guard je podporována pro vysokou dostupnost a zotavení po havárii pro účely. Podrobnosti najdete v [to] [ virtual-machines-windows-classic-configure-oracle-data-guard] dokumentaci.

#### <a name="other"></a>Ostatní
Další obecné témata jako skupiny dostupnosti Azure nebo SAP monitorování platí jak je popsáno v hello první tři kapitol tohoto dokumentu pro nasazení virtuálních počítačů s také hello databáze Oracle.

## <a name="specifics-toooracle-database-on-oracle-linux"></a>Specifika tooOracle databáze na Oracle Linux
Oracle softwaru podporuje toorun Oracle na Microsoft Windows Hyper-V a Azure. Podrobnosti o hello obecné podpoře Windows Hyper-V a Azure, zkontrolujte: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Následující obecné podporu hello je také podporována hello konkrétní scénář aplikací SAP, Oracle – databáze využití. Podrobnosti jsou pojmenované v této části dokumentu hello.

### <a name="oracle-version-support"></a>Podpora verzí Oracle
Verze Oracle a odpovídající verze operačního systému, které jsou podporovány pro SAP systémem Oracle na virtuálních počítačích Azure lze nalézt v Poznámka SAP [2039619].

Obecné informace o spuštění SAP Business Suite na Oracle naleznete v 1DX: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Pokyny pro konfigurace Oracle pro SAP instalace ve virtuálních počítačích Azure
#### <a name="storage-configuration"></a>Konfigurace úložiště
Je podporován pouze jednu instanci Oracle pomocí ext3, ext4 a xfs formátovány disky. Všechny soubory databáze musí být uložen v těchto systémech souborů na základě virtuálních pevných disků nebo disků spravované. Tyto disky připojené toohello virtuální počítač Azure a jsou založené na úložiště objektů BLOB stránky Azure (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) nebo spravované disky (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Jakýkoli druh síťové jednotky nebo vzdálených sdílených složkách, jako je Azure souborových služeb:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

jsou **není** podporované pro soubory databáze Oracle!

Pomocí disků na základě úložiště objektů BLOB stránky Azure nebo spravovat disky, hello příkazy provedené v tomto dokumentu v kapitole [ukládání do mezipaměti pro virtuální počítače a datové disky] [ dbms-guide-2.1] a [Microsoft Azure Storage] [ dbms-guide-2.3] použít toodeployments s také hello databáze Oracle.

Jak je popsáno výše v části Obecné hello hello dokumentu, existují kvóty na propustnost IOPS pro disky systému Azure. přesný kvóty Hello se v závislosti na typu hello virtuálních počítačů používají. Seznam typů virtuálních počítačů s jejich kvóty najdete [zde (Linux)] [ virtual-machines-sizes-linux] a [zde (Windows)][virtual-machines-sizes-windows].

tooidentify hello podporované typy virtuálního počítače Azure, získáte Poznámka tooSAP [1928533]

Tak dlouho, dokud hello aktuální kvóty IOPS na disk splňuje požadavky hello, je možné toostore všechny soubory databáze na jednom disku jedné připojené hello. 

Pokud jsou vyžadovány další IOPS, důrazně doporučujeme toouse LVM (Správce logických svazku) nebo MDADM toocreate jedné logické značný přes více připojené disky. Viz také kapitoly [softwaru diskového pole RAID] [ dbms-guide-2.2] tohoto dokumentu. Tento přístup zjednodušuje hello správy režijní toomanage hello místa a zabraňuje hello úsilí toomanually distribuovat soubory do více připojené disky.

#### <a name="backup--restore"></a>Backup / obnovení
Pro zálohování / obnovit funkčnost, dobrý den SAP BR * nástroje pro Oracle jsou podporovány ve stejné hello způsobem jako na holý počítač a Hyper-V. Správce obnovení Oracle (RMAN) je také podporována pro toodisk zálohování a obnovení z disku.

#### <a name="high-availability"></a>Vysoká dostupnost
Oracle Data Guard je podporována pro vysokou dostupnost a zotavení po havárii pro účely. Podrobnosti najdete v [to] [ virtual-machines-windows-classic-configure-oracle-data-guard] dokumentaci.

#### <a name="other"></a>Ostatní
Další obecné témata jako skupiny dostupnosti Azure nebo SAP monitorování platí jak je popsáno v hello první tři kapitol tohoto dokumentu pro nasazení virtuálních počítačů s také hello databáze Oracle.

## <a name="specifics-for-hello-sap-maxdb-database-on-windows"></a>Specifika pro hello SAP MaxDB databáze v systému Windows
### <a name="sap-maxdb-version-support"></a>Podpora verzí MaxDB SAP
SAP aktuálně podporuje SAP MaxDB verze 7.9 pro použití s produkty na základě SAP NetWeaver v Azure. Všechny aktualizace pro SAP MaxDB server nebo JDBC a rozhraní ODBC toobe ovladače použít s produkty na základě SAP NetWeaver jsou k dispozici výhradně prostřednictvím hello SAP služby Marketplace na <https://support.sap.com/swdc>.
Obecné informace o spouštění SAP NetWeaver na SAP MaxDB lze najít na <https://www.sap.com/community/topic/maxdb.html>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Podporované typy verze Microsoft Windows a virtuálních počítačů Azure pro SAP MaxDB databázového systému
verze Microsoft Windows hello podporované toofind pro SAP MaxDB databázového systému v Azure, najdete v části:

* [SAP produktu dostupnosti matice (PAM)][sap-pam]
* Poznámka SAP [1928533]

Důrazně doporučujeme toouse hello nejnovější verzi hello operačního systému Microsoft Windows, což je Microsoft Windows 2012 R2.

### <a name="available-sap-maxdb-documentation"></a>K dispozici SAP MaxDB dokumentace
Hello aktualizovat seznam SAP MaxDB dokumentaci můžete najít v následujících Poznámka SAP hello [767598 ]

### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Pokyny pro konfigurace MaxDB SAP pro SAP instalace ve virtuálních počítačích Azure
#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Konfigurace úložiště
Úložiště Azure osvědčené postupy pro SAP MaxDB podle hello obecná doporučení uvedená v kapitole [struktura nasazení RDBMS][dbms-guide-2].

> [!IMPORTANT]
> Stejně jako jiné databáze SAP MaxDB má také dat a souborů protokolu. V terminologii SAP MaxDB je hello správné termín "svazek" (ne "soubor"). Například existují SAP MaxDB datové svazky a svazky protokolu. Nezaměňujte tato nastavení u diskové svazky operačního systému. 
> 
> 

Stručně řečeno budete muset:

* Pokud používáte účty Azure Storage, nastavte hello účtu úložiště Azure, který obsahuje příliš hello SAP MaxDB protokolu a data svazky (tj. soubory)**místní redundantní úložiště (LRS)** uvedené v kapitole [Microsoft Azure Storage] [dbms-guide-2.3].
* Samostatné hello cesta vstupně-výstupní operace pro SAP MaxDB datové svazky (tj. soubory) z cesty hello vstupně-výstupní operace pro svazky protokolu (tj. soubory). To znamená, že SAP MaxDB datové svazky (tj. soubory) mají toobe nainstalovaná na jedné logické jednotce a svazky protokolu SAP MaxDB (tj. soubory) mají toobe nainstalován na jiné logické jednotce.
* Sada hello správné ukládání do mezipaměti typu pro každý disk, v závislosti na tom, jestli ho použít pro SAP MaxDB dat či protokolu svazky (tj. soubory) a zda používáte standardní Azure nebo Azure Premium Storage, jak je popsáno v kapitole [ukládání do mezipaměti pro virtuální počítače a datové disky][dbms-guide-2.1].
* Tak dlouho, dokud hello aktuální kvóty IOPS na disk splňuje požadavky hello, je možné toostore všechny datové svazky hello na jednom disku připojené a také uložení všechny svazky protokolu databáze na jiný disk jedné připojené.
* Pokud jsou vyžadovány další IOPS nebo místa, důrazně doporučujeme toouse Microsoft okno fondy úložiště (pouze k dispozici v systému Microsoft Windows Server 2012 a vyšší) nebo Microsoft Windows prokládání pro Microsoft Windows 2008 R2 toocreate jedno velké logické zařízení přes více připojené disky. Viz také kapitoly [softwaru diskového pole RAID] [ dbms-guide-2.2] tohoto dokumentu. Tento přístup zjednodušuje hello správy režijní toomanage hello místa a zabraňuje hello úsilí ručně distribuci souborů mezi více připojené disky.
* Hello požadavky na nejvyšší IOPS můžete použít Azure Premium Storage, který je k dispozici na DS-series a GS-series virtuálních počítačů.

![Konfigurace referenčního virtuálního počítače Azure IaaS pro SAP MaxDB databázového systému][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Zálohování a obnovení
Při nasazování SAP MaxDB do Azure, je nutné si vaše zálohování metody. I když hello systému není systémem produktivní, databázi SAP hello hostované SAP MaxDB je nutné zálohovat pravidelně. Vzhledem k tomu, že Azure úložiště udržuje tři bitové kopie, je nyní zálohu méně důležité z hlediska ochrany proti selhání úložiště a důležitější selhání provozní nebo správce systému. Hello hlavním důvodem pro údržbu správné zálohování a obnovení plán je, aby můžete kompenzovat chyby logické nebo ruční tím, že poskytuje možnosti obnovení bodu v čase. Tak hello cílem je tooeither použití zálohy toorestore hello databáze tooa určité bodu v čase nebo toouse hello záloh v Azure tooseed jiného systému zkopírováním hello existující databázi. Například je může přenést z vrstvě 2 SAP konfigurace tooa 3vrstvé systému nastavení hello stejné systému obnovení ze zálohy.

Zálohování a obnovení databáze v Azure funguje hello stejným způsobem, jak aplikace místních systémů, abyste mohli používat standardní MaxDB SAP provede zálohování a obnovení nástroje, které jsou popsány v jednom z hello SAP MaxDB dokumentace dokumenty, které jsou uvedené v Poznámka SAP [767598 ]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Důležité informace o výkonu pro zálohování a obnovení
Stejně jako u úplné nasazení je závislá na tom, kolik svazky lze číst v paralelní a hello propustnost těchto svazků výkonu zálohování a obnovení. Kromě toho můžete hello používá kompresi zálohy spotřeby procesoru přehrát významnou roli na virtuálních počítačích s až tooeight procesoru vláken. Proto můžete předpokládat jeden:

* Dobrý den méně hello počet disky použité toostore hello databáze zařízení, hello celkové nižší hello číst propustnost
* Dobrý den menší hello počet vláken procesoru v hello virtuálních počítačů, hello závažnější hello dopad kompresi zálohy
* Hello méně cílů (Stripe adresáře, disků) toowrite hello zálohování, propustnost nižší hello hello

počet hello tooincrease cílem toowrite k, existují dvě možnosti, které můžete použít, pravděpodobně v kombinaci, v závislosti na vašich potřeb:

* Vyhradit samostatných svazcích pro zálohování
* Prokládání hello zálohování cílový svazek přes více připojené disky v pořadí tooimprove hello IOPS propustnosti na tomto svazku prokládané disku
* S zařízení, samostatné vyhrazené logického disku:
  * SAP MaxDB záložní svazky (tj. soubory)
  * SAP MaxDB datové svazky (tj. soubory)
  * SAP MaxDB protokolu svazky (tj. soubory)

Prokládání svazek přes více připojené disky popsané dříve v kapitole [softwaru diskového pole RAID] [ dbms-guide-2.2] tohoto dokumentu. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Další
Další obecné témata jako jsou skupiny dostupnosti Azure nebo SAP monitorování platí také, jak je popsáno v hello první tři kapitol tohoto dokumentu pro nasazení virtuálních počítačů s databázi SAP MaxDB hello.
Další nastavení MaxDB specifické pro SAP, jsou virtuální počítače transparentní tooAzure a jsou popsané v různé dokumenty, které jsou uvedené v Poznámka SAP [767598 ] a v těchto poznámkách k SAP:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Specifika pro SAP liveCache v systému Windows
### <a name="sap-livecache-version-support"></a>SAP liveCache podpora verzí
Minimální verze SAP liveCache podporované v Azure Virtual Machines je **SAP LC/LCAPPS 10.0 SP 25** včetně **liveCache 7.9.08.31** a **LCA sestavení 25**, vydaná pro **EhP 2 pro SAP SCM 7.0** a vyšší.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Podporované typy verze Microsoft Windows a virtuálních počítačů Azure pro SAP liveCache databázového systému
verze Microsoft Windows hello podporované toofind pro SAP liveCache v Azure, najdete v části:

* [SAP produktu dostupnosti matice (PAM)][sap-pam]
* Poznámka SAP [1928533]

Důrazně doporučujeme toouse hello nejnovější verzi hello operačního systému Microsoft Windows Server. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache pokyny pro konfigurace pro SAP instalace ve virtuálních počítačích Azure
#### <a name="recommended-azure-vm-types"></a>Doporučená typy virtuálních počítačů Azure
SAP liveCache je aplikace, která provede výpočty velký, má hlavní vliv na výkon liveCache SAP hello velikost a rychlost paměti RAM a procesoru. 

Pro hello virtuálního počítače Azure typy podporované systémem SAP (Poznámka SAP [1928533]), všechny virtuální prostředky procesoru přidělené toohello virtuálních počítačů jsou zajišťované vyhrazené fyzické prostředky procesoru hello hypervisoru. Žádné předimenzování (a proto žádné soutěž o prostředky procesoru) probíhá.

Podobně pro všechny virtuální počítač Azure instance typy podporované systémem SAP, hello paměť virtuálního počítače je 100 % namapované toohello fyzické paměti – předimenzování (většího celkového), například nepoužívá.

Z tohoto hlediska důrazně doporučujeme toouse hello nového virtuálního počítače Azure DS-series (v kombinaci s Azure Premium Storage) nebo D-series typu, jako mají 60 % rychlejší procesory než hello A-series. Pro hello nejvyšší paměti RAM a zatížení procesoru, můžete použít G-series a GS-series (v kombinaci s Azure Premium Storage) virtuální počítače s procesorem Intel Xeon® nejnovější hello E5 v3 rodiny, které mají dvakrát hello paměti a čtyřikrát hello SSD disk úložiště (SSD) hello D / DS-series.

#### <a name="storage-configuration"></a>Konfigurace úložiště
Jako SAP liveCache je založena na technologii SAP MaxDB, všechny hello úložiště Azure z doporučených osvědčených postupů uvedených v kapitole pro SAP MaxDB [konfigurace úložiště] [ dbms-guide-8.4.1] platí také pro SAP liveCache. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Pro liveCache vyhrazený virtuální počítač Azure
Jako SAP liveCache intenzivně využívá výpočetní výkon, produktivitu využití důrazně doporučujeme toodeploy na vyhrazený virtuální počítač Azure. 

![Vyhrazený virtuální počítač Azure pro liveCache pro případ použití produktivitu][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Zálohování a obnovení
Zálohování a obnovení, včetně faktory ovlivňující výkon, jsou již popsané v hello odpovídající SAP MaxDB kapitoly [zálohování a obnovení] [ dbms-guide-8.4.2] a [důležité informace o výkonu pro zálohování a obnovení][dbms-guide-8.4.3]. 

#### <a name="other"></a>Ostatní
Další obecné témata jsou již popsané v hello relevantní SAP MaxDB [to] [ dbms-guide-8.4.4] kapitoly. 

## <a name="specifics-for-hello-sap-content-server-on-windows"></a>Specifika pro hello SAP Server obsahu v systému Windows
Hello serveru SAP obsah je obsahem toostore samostatný, serverových součástí například elektronických dokumentů v různých formátech. Hello obsahu serveru SAP zajišťuje vývoj technologie a je mezi aplikacemi toobe použít pro všechny aplikace SAP. Je nainstalovaná v samostatném systému. Typické obsah je cvičení materiálu a dokumentace z skladu znalostní báze nebo technické výkresy pocházející z hello mySAP PLM systém správy dokumentů. 

### <a name="sap-content-server-version-support"></a>Podpora verze obsahu serveru SAP
SAP aktuálně podporuje:

* **Server obsahu SAP** s verzí **6.50 (a vyšší)**
* **SAP MaxDB verze 7.9**
* **Microsoft IIS (Internet Information Server) verze 8.0 (a vyšší)**

Důrazně doporučujeme toouse hello nejnovější verzi obsahu serveru SAP, což v době psaní tohoto dokumentu hello je **6.50 SP4**a nejnovější verze hello **Microsoft IIS 8.5**. 

Zkontrolujte hello nejnovější podporované verze obsahu serveru SAP a Microsoft IIS v hello [SAP produktu dostupnosti matice (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Podporované typy Microsoft Windows a virtuálních počítačů Azure pro Server obsahu SAP
toofind na podporovanou verzi systému Windows pro SAP Server obsahu v Azure, najdete v části:

* [SAP produktu dostupnosti matice (PAM)][sap-pam]
* Poznámka SAP [1928533]

Důrazně doporučujeme toouse hello nejnovější verzi systému Microsoft Windows Server.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Pokyny ke konfiguraci serveru obsahu SAP pro SAP instalace ve virtuálních počítačích Azure
#### <a name="storage-configuration"></a>Konfigurace úložiště
Pokud nakonfigurujete Server obsahu SAP toostore soubory v databázi SAP MaxDB hello, všechny úložiště Azure osvědčených postupů doporučení uvedená v kapitole pro SAP MaxDB [konfigurace úložiště] [ dbms-guide-8.4.1] jsou také platné pro scénář obsahu serveru SAP hello. 

Pokud nakonfigurujete Server obsahu SAP toostore soubory v systému souborů hello, je doporučeno toouse vyhrazené logické jednotky. Použití prostorů úložiště systému Windows můžete tooalso zvýšení logického disku velikosti a IOPS propustnosti, jak je popsáno v kapitole [softwaru diskového pole RAID][dbms-guide-2.2]. 

#### <a name="sap-content-server-location"></a>Umístění obsahu serveru SAP
Server obsahu SAP má toobe nasazené v hello stejné oblasti Azure a virtuální síť Azure, kde je nasazená hello systému SAP. Jestli chcete, že součásti serveru SAP obsahu toodeploy na vyhrazených virtuálních počítačů Azure nebo na hello stejného virtuálního počítače se spuštěným systémem hello systému SAP jste volné toodecide. 

![Vyhrazený virtuální počítač Azure pro Server obsahu SAP][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>Umístění mezipaměti serveru SAP
Hello serveru SAP mezipaměti je další serverové součásti tooprovide přístup too(cached) dokumenty místně. Hello serveru SAP mezipaměti ukládá do mezipaměti hello dokumenty obsahu serveru SAP. Toto je toooptimize síťový provoz, pokud dokumenty toobe více než jednou načteny z různých umístění. Hello obecně platí, že tento Server mezipaměti SAP hello má toobe fyzicky zavřít toohello klienta, který přistupuje k hello mezipaměti serveru SAP. 

Zde máte dvě možnosti:

1. **Klient je systém SAP back-end** Pokud systému SAP back-end je nakonfigurované tooaccess obsahu serveru SAP, že systému SAP je klient. Při nasazování systému SAP a obsahu serveru SAP v hello stejné oblasti Azure – v hello stejné datové centrum Azure – jsou fyzicky zavřít tooeach jiné. Je proto bez nutnosti toohave vyhrazený Server mezipaměti SAP. Hello přístup klientů (SAP grafického uživatelského rozhraní nebo webový prohlížeč) SAP uživatelského rozhraní systému SAP přímo a hello SAP systému načte dokumenty z hello obsahu serveru SAP.
2. **Klient je webový prohlížeč místně** hello serveru SAP obsahu může být nakonfigurované toobe přistupovat přímo pomocí hello webového prohlížeče. V takovém případě je webový prohlížeč s místní – klienta hello obsahu serveru SAP. Místního datového centra a datové centrum Azure jsou umístěny v různých fyzických lokacích (v ideálním případě zavřít tooeach Další). Vaše místního datového centra je připojený tooAzure přes Azure Site-to-Site VPN nebo ExpressRoute. I když obě možnosti nabízí zabezpečené tooAzure připojení sítě VPN, site-to-site síťové připojení nenabízí SLA síťové šířky pásma a čekací doba mezi hello místního datového centra a hello datového centra Azure. toospeed až toodocuments přístup, můžete provést jednu z následujících akcí hello:
   1. Instalace serveru SAP mezipaměti místně, zavřete toohello místní webový prohlížeč (možnost [to] [ dbms-guide-900-sap-cache-server-on-premises] obrázek)
   2. Konfigurace Azure ExpressRoute, který nabízí vysokorychlostní a nízkou latencí vyhrazené síťové připojení mezi místního datového centra a datového centra Azure.

![Možnost tooinstall SAP mezipaměti místní Server][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Backup / obnovení
Pokud nakonfigurujete hello obsahu serveru SAP toostore soubory v databázi SAP MaxDB hello, hello zálohování nebo obnovení postupu a výkonové požadavky jsou již popsané v kapitole SAP MaxDB [zálohování a obnovení] [ dbms-guide-8.4.2] a kapitoly [otázky výkonu při zálohování a obnovení][dbms-guide-8.4.3]. 

Pokud nakonfigurujete hello obsahu serveru SAP toostore soubory v systému souborů hello, jednou z možností je tooexecute ruční zálohování nebo obnovení hello celý soubor struktury a kde se nachází hello dokumenty. Podobně jako tooSAP MaxDB zálohování a obnovení, je doporučeno toohave vyhrazené diskový svazek pro zálohování účel. 

#### <a name="other"></a>Ostatní
Další konkrétní nastavení serveru SAP obsahu je transparentní tooAzure virtuálních počítačů a jsou popsané v různých dokumenty a SAP poznámky:

* <https://Service.SAP.com/contentserver> 
* Poznámka SAP [1619726]  

## <a name="specifics-tooibm-db2-for-luw-on-windows"></a>Specifika tooIBM DB2 pro LUW v systému Windows
S Microsoft Azure můžete snadno migrovat stávající aplikaci SAP systémem IBM DB2 pro Linux, UNIX a systému Windows (LUW) tooAzure virtuální počítače. SAP na IBM DB2 pro LUW správci a vývojáři stále pomocí hello stejných nástrojů vývoj a správu, které jsou k dispozici místně.
Obecné informace o spuštění SAP Business Suite na IBM DB2 pro LUW lze nalézt v hello SAP komunity sítě (oznámení změny stavu) na <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Další informace a aktualizace o SAP v DB2 pro LUW v Azure, viz poznámka SAP [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 pro Linux, UNIX a podpora verzí systému Windows
SAP na IBM DB2 pro LUW na služby Microsoft Azure virtuálního počítače je podporováno od verze DB2 10.5.

Informace o podporovaných produktech SAP a typy virtuálního počítače Azure, najdete v části tooSAP Poznámka [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 pro Linux, UNIX a pokyny pro konfigurace systému Windows pro SAP instalace ve virtuálních počítačích Azure
#### <a name="storage-configuration"></a>Konfigurace úložiště
Všechny soubory databáze musí být uložen v systému souborů NTFS hello podle přímo připojených disků. Tyto disky připojené toohello virtuální počítač Azure a jsou založené na Azure úložiště objektů BLOB stránky (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) nebo spravované disky (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Jakýkoli druh síťové jednotky nebo vzdálených sdílených složkách, jako jsou následující Azure souborové služby hello **není** podporované pro soubory databáze: 

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

Pokud používáte disky na základě úložiště objektů BLOB stránky Azure nebo spravovat disky, hello příkazy provedené v tomto dokumentu v kapitole [struktura nasazení RDBMS] [ dbms-guide-2] platí také toodeployments s hello IBM DB2 pro databázi LUW. 

Jak je popsáno výše v části Obecné hello hello dokumentu, existují kvóty na propustnost IOPS pro disky. přesný kvóty Hello závisí na typu hello virtuálních počítačů použít. Seznam typů virtuálních počítačů s jejich kvóty najdete [zde (Linux)] [ virtual-machines-sizes-linux] a [zde (Windows)][virtual-machines-sizes-windows].

Tak dlouho, dokud hello aktuální kvóty IOPS na disk je dostačující, že je možné toostore hello všechny soubory databáze na jednom disku jedné připojené. 

Aspekty výkonu se také podívat toochapter "datové zabezpečení a výkonu důležité informace pro databázi adresáře" v příručkách instalace SAP.

Alternativně můžete použít fondy úložiště systému Windows (pouze k dispozici v systému Windows Server 2012 a vyšší) nebo proložení Windows pro systém Windows 2008 R2 jako popsané v kapitole [softwaru diskového pole RAID] [ dbms-guide-2.2] tohoto dokumentu toocreate jedno velké logické zařízení přes několik disků.
Pro disky hello obsahující hello DB2 cesty úložiště pro vaše sapdata a saptmp adresáře musíte zadat velikost sektoru fyzického disku 512 KB. Pokud používáte fondy úložišť systému Windows, musíte vytvořit hello fondy úložiště ručně pomocí rozhraní příkazového řádku pomocí parametru hello "-LogicalSectorSizeDefault". Další informace najdete v tématu <https://technet.microsoft.com/itpro/powershell/windows/storage/new-storagepool>.

#### <a name="backuprestore"></a>Zálohování a obnovení
Hello funkce zálohování a obnovení pro IBM DB2 LUW je podporováno v hello stejný způsobem jako na standardní operační systémy Windows Server a Hyper-V.

Musí se ujistěte, že máte zavedenou strategie zálohování platnou databázi. 

Jako úplné nasazení výkonu zálohování a obnovení závisí na kolik svazky lze číst paralelně a může být co hello propustnost těchto svazků. Kromě toho může hello používá kompresi zálohy spotřeby procesoru přehrát významnou roli na virtuálních počítačích s právě až tooeight procesoru vláken. Proto můžete předpokládat jeden:

* Hello méně hello počtu toostore disky použité hello zařízení databáze, hello menší hello celková propustnost v čtení
* Dobrý den menší hello počet vláken procesoru v hello virtuálních počítačů, hello závažnější hello dopad kompresi zálohy
* Hello méně cílů (Stripe adresáře, disků) toowrite hello zálohování, propustnost nižší hello hello

tooincrease hello počet toowrite cíle k, dvě možnosti může být použit nebo kombinaci podle potřeby:

* Prokládání hello zálohování cílový svazek přes víc disků ve pořadí tooimprove hello IOPS propustnosti na tomto svazku prokládané
* Použití více než jeden cílový adresář toowrite hello zálohování

#### <a name="high-availability-and-disaster-recovery"></a>Vysoká dostupnost a zotavení po havárii
Microsoft Cluster Server (MSCS) není podporována.

Zotavení po havárii DB2 vysokou dostupnost (HADR) je podporováno. Pokud virtuální počítače hello hello HA konfigurace funguje překlad, instalační program hello v Azure se neliší od žádné nastavení, která se provádí na místě. Není doporučeno toorely na pouze IP řešení.

Nepoužívejte geografická replikace pro účty úložiště hello, které ukládají databáze disky hello. Další informace najdete v části toochapter [Microsoft Azure Storage] [ dbms-guide-2.3] a kapitoly [vysokou dostupnost a zotavení po havárii s virtuálními počítači Azure] [ dbms-guide-3].

#### <a name="other"></a>Ostatní
Jak je popsáno v hello první tři kapitol tohoto dokumentu pro nasazení virtuálních počítačů s IBM DB2 pro LUW také použít všechny ostatní obecné témata jako skupiny dostupnosti Azure nebo SAP monitorování. 

Najdete také toochapter [obecné SQL Server pro SAP v Azure souhrnu][dbms-guide-5.8].

## <a name="specifics-tooibm-db2-for-luw-on-linux"></a>Specifika tooIBM DB2 pro LUW v systému Linux
S Microsoft Azure můžete snadno migrovat stávající aplikaci SAP systémem IBM DB2 pro Linux, UNIX a systému Windows (LUW) tooAzure virtuální počítače. SAP na IBM DB2 pro LUW správci a vývojáři stále pomocí hello stejných nástrojů vývoj a správu, které jsou k dispozici místně. Obecné informace o spuštění SAP Business Suite na IBM DB2 pro LUW lze nalézt v hello SAP komunity sítě (oznámení změny stavu) na <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Další informace a aktualizace o SAP v DB2 pro LUW v Azure, viz poznámka SAP [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 pro Linux, UNIX a podpora verzí systému Windows
SAP na IBM DB2 pro LUW na služby Microsoft Azure virtuálního počítače je podporováno od verze DB2 10.5.

Informace o podporovaných produktech SAP a typy virtuálního počítače Azure, najdete v části tooSAP Poznámka [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 pro Linux, UNIX a pokyny pro konfigurace systému Windows pro SAP instalace ve virtuálních počítačích Azure
#### <a name="storage-configuration"></a>Konfigurace úložiště
Všechny soubory databáze musí být uložen v systému souborů podle přímo připojených disků. Tyto disky připojené toohello virtuální počítač Azure a jsou založené na Azure úložiště objektů BLOB stránky (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) nebo spravované disky (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Jakýkoli druh síťové jednotky nebo vzdálených sdílených složkách, jako jsou následující Azure souborové služby hello **není** podporované pro soubory databáze:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

Pokud používáte disky založené na úložiště objektů BLOB stránky Azure, hello příkazy provedené v tomto dokumentu v kapitole [struktura nasazení RDBMS] [ dbms-guide-2] platí také pro databázi LUW toodeployments s hello IBM DB2.

Jak je popsáno výše v části Obecné hello hello dokumentu, existují kvóty na propustnost IOPS pro disky. přesný kvóty Hello závisí na typu hello virtuálních počítačů použít. Seznam typů virtuálních počítačů s jejich kvóty najdete [zde (Linux)] [ virtual-machines-sizes-linux] a [zde (Windows)][virtual-machines-sizes-windows].

Tak dlouho, dokud hello aktuální kvóty IOPS na disk je dostačující, že je možné toostore hello všechny soubory databáze na jeden jediný disk.

Aspekty výkonu se také podívat toochapter "datové zabezpečení a výkonu důležité informace pro databázi adresáře" v příručkách instalace SAP.

Alternativně můžete použít LVM (Správce logických svazku) nebo MDADM jak je popsáno v kapitole [softwaru diskového pole RAID] [ dbms-guide-2.2] tohoto dokumentu toocreate jeden velké logické zařízení přes několik disků.
Pro disky hello obsahující hello DB2 cesty úložiště pro vaše sapdata a saptmp adresáře musíte zadat velikost sektoru fyzického disku 512 KB.

#### <a name="backuprestore"></a>Zálohování a obnovení
Hello funkce zálohování a obnovení pro IBM DB2 LUW je podporováno v hello stejné způsobem jako u standardní Linux instalace místně.

Musí se ujistěte, že máte zavedenou strategie zálohování platnou databázi.

Jako úplné nasazení výkonu zálohování a obnovení závisí na kolik svazky lze číst paralelně a může být co hello propustnost těchto svazků. Kromě toho může hello používá kompresi zálohy spotřeby procesoru přehrát významnou roli na virtuálních počítačích s právě až tooeight procesoru vláken. Proto můžete předpokládat jeden:

* Hello méně hello počtu toostore disky použité hello zařízení databáze, hello menší hello celková propustnost v čtení
* Dobrý den menší hello počet vláken procesoru v hello virtuálních počítačů, hello závažnější hello dopad kompresi zálohy
* Hello méně cílů (Stripe adresáře, disků) toowrite hello zálohování, propustnost nižší hello hello

tooincrease hello počet toowrite cíle k, dvě možnosti může být použit nebo kombinaci podle potřeby:

* Prokládání hello zálohování cílový svazek přes víc disků ve pořadí tooimprove hello IOPS propustnosti na tomto svazku prokládané
* Použití více než jeden cílový adresář toowrite hello zálohování

#### <a name="high-availability-and-disaster-recovery"></a>Vysoká dostupnost a zotavení po havárii
Zotavení po havárii DB2 vysokou dostupnost (HADR) je podporováno. Pokud virtuální počítače hello hello HA konfigurace funguje překlad, instalační program hello v Azure se neliší od žádné nastavení, která se provádí na místě. Není doporučeno toorely na pouze IP řešení.

Nepoužívejte geografická replikace pro účty úložiště hello, které ukládají databáze disky hello. Další informace najdete v části toochapter [Microsoft Azure Storage] [ dbms-guide-2.3] a kapitoly [vysokou dostupnost a zotavení po havárii s virtuálními počítači Azure] [ dbms-guide-3].

#### <a name="other"></a>Ostatní
Jak je popsáno v hello první tři kapitol tohoto dokumentu pro nasazení virtuálních počítačů s IBM DB2 pro LUW také použít všechny ostatní obecné témata jako skupiny dostupnosti Azure nebo SAP monitorování.

Najdete také toochapter [obecné SQL Server pro SAP v Azure souhrnu][dbms-guide-5.8].

