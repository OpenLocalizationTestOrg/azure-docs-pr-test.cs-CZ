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
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d00db895ffcf9ba9a51e3df2dae5d33c0277dd6f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-virtual-machines-high-availability-for-sap-netweaver"></a><span data-ttu-id="76079-103">Azure vysoké dostupnosti virtuálních počítačů pro SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="76079-103">Azure Virtual Machines high availability for SAP NetWeaver</span></span>

<span data-ttu-id="76079-104">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span><span class="sxs-lookup"><span data-stu-id="76079-104">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span></span>
<span data-ttu-id="76079-105">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span><span class="sxs-lookup"><span data-stu-id="76079-105">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span></span>
<span data-ttu-id="76079-106">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span><span class="sxs-lookup"><span data-stu-id="76079-106">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span></span>
<span data-ttu-id="76079-107">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span><span class="sxs-lookup"><span data-stu-id="76079-107">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span></span>
<span data-ttu-id="76079-108">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span><span class="sxs-lookup"><span data-stu-id="76079-108">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span></span>

[sap-installation-guides]:http://service.sap.com/instguides

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:sap-high-availability-guide.md
[sap-ha-guide-2]:#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-4]:#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-8]:#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.9]:#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.3]:#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:#a97ad604-9094-44fe-a364-f89cb39bf097

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID high-availability configuration)


[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:./media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md



<span data-ttu-id="76079-109">Virtuální počítače Azure je řešení pro organizace, které potřebují výpočty, úložiště a síťové prostředky ve minimálního čas a bez zdlouhavé nákup cykly.</span><span class="sxs-lookup"><span data-stu-id="76079-109">Azure Virtual Machines is the solution for organizations that need compute, storage, and network resources, in minimal time, and without lengthy procurement cycles.</span></span> <span data-ttu-id="76079-110">Virtuální počítače Azure můžete použít k nasazení classic aplikace jako na základě SAP NetWeaver ABAP, Java a ABAP + Java zásobníku.</span><span class="sxs-lookup"><span data-stu-id="76079-110">You can use Azure Virtual Machines to deploy classic applications like SAP NetWeaver-based ABAP, Java, and an ABAP+Java stack.</span></span> <span data-ttu-id="76079-111">Rozšiřte spolehlivosti a dostupnosti bez dalších místních prostředků.</span><span class="sxs-lookup"><span data-stu-id="76079-111">Extend reliability and availability without additional on-premises resources.</span></span> <span data-ttu-id="76079-112">Virtuální počítače Azure podporuje připojení mezi různými místy, takže virtuální počítače Azure můžete integrovat do místní domény vaší organizace, privátních cloudů a šířku systému SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-112">Azure Virtual Machines supports cross-premises connectivity, so you can integrate Azure Virtual Machines into your organization's on-premises domains, private clouds, and SAP system landscape.</span></span>

<span data-ttu-id="76079-113">V tomto článku jsme zahrnují kroky, které můžete provést nasazení SAP systémů s vysokou dostupností v Azure pomocí modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="76079-113">In this article, we cover the steps that you can take to deploy high-availability SAP systems in Azure by using the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="76079-114">Můžeme vás provede procesem tyto hlavní úlohy:</span><span class="sxs-lookup"><span data-stu-id="76079-114">We walk you through these major tasks:</span></span>

* <span data-ttu-id="76079-115">Nalezení správné SAP poznámky a příručky pro instalaci, uvedené v [prostředky] [ sap-ha-guide-2] části.</span><span class="sxs-lookup"><span data-stu-id="76079-115">Find the right SAP Notes and installation guides, listed in the [Resources][sap-ha-guide-2] section.</span></span> <span data-ttu-id="76079-116">Tento článek doplňuje SAP instalace dokumentace a poznámky k SAP, což jsou primární prostředky, které vám mohou pomoci instalace a nasazení SAP software na specifické platformy.</span><span class="sxs-lookup"><span data-stu-id="76079-116">This article complements SAP installation documentation and SAP Notes, which are the primary resources that can help you install and deploy SAP software on specific platforms.</span></span>
* <span data-ttu-id="76079-117">Přečtěte si rozdíly mezi modelem nasazení Azure Resource Manager a modelu nasazení Azure classic.</span><span class="sxs-lookup"><span data-stu-id="76079-117">Learn the differences between the Azure Resource Manager deployment model and the Azure classic deployment model.</span></span>
* <span data-ttu-id="76079-118">Další informace o Windows Server Failover Clustering režimů kvora, takže můžete vybrat model, který je nejvhodnější pro vaše nasazení Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-118">Learn about Windows Server Failover Clustering quorum modes, so you can select the model that is right for your Azure deployment.</span></span>
* <span data-ttu-id="76079-119">Další informace o Windows Server Failover Clustering sdíleného úložiště do služby Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-119">Learn about Windows Server Failover Clustering shared storage in Azure services.</span></span>
* <span data-ttu-id="76079-120">Zjistěte, jak pomoci chránit jeden bod o selhání součásti jako Advanced obchodní aplikace programování (ABAP) SAP centrální služby (ASC) / SAP centrální služby (SCS) a databázové systémy (databázového systému) a redundantní komponenty, například aplikace SAP Server v Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-120">Learn how to help protect single-point-of-failure components like Advanced Business Application Programming (ABAP) SAP Central Services (ASCS)/SAP Central Services (SCS) and database management systems (DBMS), and redundant components like SAP Application Server, in Azure.</span></span>
* <span data-ttu-id="76079-121">Postupujte podle podrobný příklad k instalaci a konfiguraci systému SAP vysoké dostupnosti v clusteru Windows Server Failover Clustering v Azure pomocí Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="76079-121">Follow a step-by-step example of an installation and configuration of a high-availability SAP system in a Windows Server Failover Clustering cluster in Azure by using Azure Resource Manager.</span></span>
* <span data-ttu-id="76079-122">Další informace o dalších krocích, které jsou potřeba použít Windows Server Failover Clustering v Azure, ale které nejsou potřebné v místním nasazení.</span><span class="sxs-lookup"><span data-stu-id="76079-122">Learn about additional steps required to use Windows Server Failover Clustering in Azure, but which are not needed in an on-premises deployment.</span></span>

<span data-ttu-id="76079-123">Zjednodušit nasazení a konfigurace v tomto článku používáme šablony Resource Manageru SAP třívrstvá vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="76079-123">To simplify deployment and configuration, in this article, we use the SAP three-tier high-availability Resource Manager templates.</span></span> <span data-ttu-id="76079-124">Šablony automatizovat nasazení celé infrastruktury, které potřebujete pro SAP systém vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="76079-124">The templates automate deployment of the entire infrastructure that you need for a high-availability SAP system.</span></span> <span data-ttu-id="76079-125">Infrastruktura podporuje také SAP aplikace výkonu standardní (protokoly SAP) velikosti vašeho systému SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-125">The infrastructure also supports SAP Application Performance Standard (SAPS) sizing of your SAP system.</span></span>

## <span data-ttu-id="76079-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="76079-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a> Prerequisites</span></span>
<span data-ttu-id="76079-127">Než začnete, ujistěte se, že splňujete požadavky, které jsou popsané v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="76079-127">Before you start, make sure that you meet the prerequisites that are described in the following sections.</span></span> <span data-ttu-id="76079-128">Také, nezapomeňte zkontrolovat všechny prostředky uvedené v [prostředky] [ sap-ha-guide-2] části.</span><span class="sxs-lookup"><span data-stu-id="76079-128">Also, be sure to check all resources listed in the [Resources][sap-ha-guide-2] section.</span></span>

<span data-ttu-id="76079-129">V tomto článku používáme šablon Azure Resource Manageru pro [třívrstvá SAP NetWeaver pomocí disků spravované](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md/).</span><span class="sxs-lookup"><span data-stu-id="76079-129">In this article, we use Azure Resource Manager templates for [three-tier SAP NetWeaver using Managed Disks](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md/).</span></span> <span data-ttu-id="76079-130">Užitečné Přehled šablon naleznete v tématu [šablon SAP Azure Resource Manageru](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span><span class="sxs-lookup"><span data-stu-id="76079-130">For a helpful overview of templates, see [SAP Azure Resource Manager templates](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span></span>

## <span data-ttu-id="76079-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Prostředky</span><span class="sxs-lookup"><span data-stu-id="76079-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a> Resources</span></span>
<span data-ttu-id="76079-132">Tyto články popisuje nasazení SAP v Azure:</span><span class="sxs-lookup"><span data-stu-id="76079-132">These articles cover SAP deployments in Azure:</span></span>

* <span data-ttu-id="76079-133">[Azure virtuálních počítačů, plánování a implementace pro SAP NetWeaver][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="76079-133">[Azure Virtual Machines planning and implementation for SAP NetWeaver][planning-guide]</span></span>
* <span data-ttu-id="76079-134">[Nasazení virtuálních počítačů Azure pro SAP NetWeaver][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="76079-134">[Azure Virtual Machines deployment for SAP NetWeaver][deployment-guide]</span></span>
* <span data-ttu-id="76079-135">[Nasazení virtuálních počítačů databázového systému Azure pro SAP NetWeaver][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="76079-135">[Azure Virtual Machines DBMS deployment for SAP NetWeaver][dbms-guide]</span></span>
* <span data-ttu-id="76079-136">[Azure vysoké dostupnosti virtuálních počítačů pro SAP NetWeaver (Tato příručka)][sap-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="76079-136">[Azure Virtual Machines high availability for SAP NetWeaver (this guide)][sap-ha-guide]</span></span>

> [!NOTE]
> <span data-ttu-id="76079-137">Kdykoli je to možné, můžeme vám dát odkaz odkazující Průvodce instalací SAP (najdete v článku [SAP instalace příručky][sap-installation-guides]).</span><span class="sxs-lookup"><span data-stu-id="76079-137">Whenever possible, we give you a link to the referring SAP installation guide (see the [SAP installation guides][sap-installation-guides]).</span></span> <span data-ttu-id="76079-138">Informace o procesu instalace a požadavky je vhodné si pozorně přečtěte příručky instalace SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="76079-138">For prerequisites and information about the installation process, it's a good idea to read the SAP NetWeaver installation guides carefully.</span></span> <span data-ttu-id="76079-139">Tento článek se týká pouze konkrétní úlohy pro počítače se systémem SAP NetWeaver, které můžete použít s virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-139">This article covers only specific tasks for SAP NetWeaver-based systems that you can use with Azure Virtual Machines.</span></span>
>
>

<span data-ttu-id="76079-140">Tyto poznámky SAP souvisí s tématem SAP v Azure:</span><span class="sxs-lookup"><span data-stu-id="76079-140">These SAP Notes are related to the topic of SAP in Azure:</span></span>

| <span data-ttu-id="76079-141">Poznámka: číslo</span><span class="sxs-lookup"><span data-stu-id="76079-141">Note number</span></span> | <span data-ttu-id="76079-142">Název</span><span class="sxs-lookup"><span data-stu-id="76079-142">Title</span></span> |
| --- | --- |
| <span data-ttu-id="76079-143">[1928533]</span><span class="sxs-lookup"><span data-stu-id="76079-143">[1928533]</span></span> |<span data-ttu-id="76079-144">Aplikace SAP v Azure: podporované produkty a velikosti</span><span class="sxs-lookup"><span data-stu-id="76079-144">SAP Applications on Azure: Supported Products and Sizing</span></span> |
| <span data-ttu-id="76079-145">[2015553]</span><span class="sxs-lookup"><span data-stu-id="76079-145">[2015553]</span></span> |<span data-ttu-id="76079-146">SAP na platformě Microsoft Azure: podporovat požadavky</span><span class="sxs-lookup"><span data-stu-id="76079-146">SAP on Microsoft Azure: Support Prerequisites</span></span> |
| <span data-ttu-id="76079-147">[1999351]</span><span class="sxs-lookup"><span data-stu-id="76079-147">[1999351]</span></span> |<span data-ttu-id="76079-148">Rozšířené monitorování Azure pro SAP</span><span class="sxs-lookup"><span data-stu-id="76079-148">Enhanced Azure Monitoring for SAP</span></span> |
| <span data-ttu-id="76079-149">[2178632]</span><span class="sxs-lookup"><span data-stu-id="76079-149">[2178632]</span></span> |<span data-ttu-id="76079-150">Klíč monitorování metriky pro SAP na platformě Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="76079-150">Key Monitoring Metrics for SAP on Microsoft Azure</span></span> |
| <span data-ttu-id="76079-151">[1999351]</span><span class="sxs-lookup"><span data-stu-id="76079-151">[1999351]</span></span> |<span data-ttu-id="76079-152">Virtualizace v systému Windows: rozšířené monitorování</span><span class="sxs-lookup"><span data-stu-id="76079-152">Virtualization on Windows: Enhanced Monitoring</span></span> |
| <span data-ttu-id="76079-153">[2243692]</span><span class="sxs-lookup"><span data-stu-id="76079-153">[2243692]</span></span> |<span data-ttu-id="76079-154">Používání úložiště Azure Premium SSD pro instanci databázového systému SAP</span><span class="sxs-lookup"><span data-stu-id="76079-154">Use of Azure Premium SSD Storage for SAP DBMS Instance</span></span> |

<span data-ttu-id="76079-155">Další informace o [omezení předplatná Azure][azure-subscription-service-limits-subscription], včetně obecné výchozí omezení a maximální omezení.</span><span class="sxs-lookup"><span data-stu-id="76079-155">Learn more about the [limitations of Azure subscriptions][azure-subscription-service-limits-subscription], including general default limitations and maximum limitations.</span></span>

## <span data-ttu-id="76079-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Vysoká dostupnost SAP s Azure Resource Manager a modelu nasazení Azure classic</span><span class="sxs-lookup"><span data-stu-id="76079-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>High-availability SAP with Azure Resource Manager vs. the Azure classic deployment model</span></span>
<span data-ttu-id="76079-157">Azure Resource Manager a modelech nasazení Azure classic se liší v těchto oblastech:</span><span class="sxs-lookup"><span data-stu-id="76079-157">The Azure Resource Manager and Azure classic deployment models are different in the following areas:</span></span>

- <span data-ttu-id="76079-158">Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="76079-158">Resource groups</span></span>
- <span data-ttu-id="76079-159">Závislost nástroje pro vyrovnávání zatížení Azure interní na skupina prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="76079-159">Azure internal load balancer dependency on the Azure resource group</span></span>
- <span data-ttu-id="76079-160">Podpora pro scénáře více SID SAP</span><span class="sxs-lookup"><span data-stu-id="76079-160">Support for SAP multi-SID scenarios</span></span>

### <span data-ttu-id="76079-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="76079-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a> Resource groups</span></span>
<span data-ttu-id="76079-162">V Azure Resource Manager, můžete použít skupin prostředků ke správě všechny prostředky aplikace ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-162">In Azure Resource Manager, you can use resource groups to manage all the application resources in your Azure subscription.</span></span> <span data-ttu-id="76079-163">Integrovaný přístup, všechny prostředky mají stejný životní cyklus za ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="76079-163">An integrated approach, in a resource group, all resources have the same life cycle.</span></span> <span data-ttu-id="76079-164">Například všechny prostředky jsou vytvořeny ve stejnou dobu a jsou odstraněny ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="76079-164">For example, all resources are created at the same time and they are deleted at the same time.</span></span> <span data-ttu-id="76079-165">Další informace o [skupinách prostředků](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="76079-165">Learn more about [resource groups](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>

### <span data-ttu-id="76079-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Závislost nástroje pro vyrovnávání zatížení Azure interní na skupina prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="76079-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a> Azure internal load balancer dependency on the Azure resource group</span></span>

<span data-ttu-id="76079-167">V modelu nasazení Azure classic je závislost mezi nástroje pro vyrovnávání zatížení Azure interní (služba Vyrovnávání zatížení Azure) a cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="76079-167">In the Azure classic deployment model, there is a dependency between the Azure internal load balancer (Azure Load Balancer service) and the cloud service.</span></span> <span data-ttu-id="76079-168">Každý nástroj pro vyrovnávání zatížení interní musí jeden cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="76079-168">Every internal load balancer needs one cloud service.</span></span>

<span data-ttu-id="76079-169">V Azure Resource Manager, každý prostředků Azure je nutné umístit do skupiny prostředků Azure, a to je platný pro vyrovnávání zatížení Azure také.</span><span class="sxs-lookup"><span data-stu-id="76079-169">In Azure Resource Manager, every Azure resource needs to be placed into an Azure resource group, and this is valid for Azure Load Balancer as well.</span></span> <span data-ttu-id="76079-170">Ale není nutné mít jednu skupinu prostředků Azure na nástroje pro vyrovnávání zatížení Azure, například jedna skupina prostředků Azure může obsahovat více Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-170">However, there is no need to have one Azure resource group per Azure load balancer, e.g. one Azure resource group can contain multiple Azure Load Balancers.</span></span> <span data-ttu-id="76079-171">Prostředí je jednodušší a flexibilnější.</span><span class="sxs-lookup"><span data-stu-id="76079-171">The environment is simpler and more flexible.</span></span> 

### <a name="support-for-sap-multi-sid-scenarios"></a><span data-ttu-id="76079-172">Podpora pro scénáře více SID SAP</span><span class="sxs-lookup"><span data-stu-id="76079-172">Support for SAP multi-SID scenarios</span></span>

<span data-ttu-id="76079-173">V Azure Resource Manager, můžete nainstalovat víc SAP systému identifikátor (SID) ASC nebo SCS instancí v jednom clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-173">In Azure Resource Manager, you can install multiple SAP system identifier (SID) ASCS/SCS instances in one cluster.</span></span> <span data-ttu-id="76079-174">SID více instancí je možné, protože obsahuje podporu pro více IP adres pro každý nástroj pro vyrovnávání zatížení Azure interní.</span><span class="sxs-lookup"><span data-stu-id="76079-174">Multi-SID instances are possible because of support for multiple IP addresses for each Azure internal load balancer.</span></span>

<span data-ttu-id="76079-175">Model nasazení Azure classic, postupujte podle postupů popsaných v [SAP NetWeaver v Azure: instancí clusteringu ASC nebo SCS SAP pomocí služby Windows Server Failover Clustering v Azure pomocí s DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).</span><span class="sxs-lookup"><span data-stu-id="76079-175">To use the Azure classic deployment model, follow the procedures described in [SAP NetWeaver in Azure: Clustering SAP ASCS/SCS instances by using Windows Server Failover Clustering in Azure with SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="76079-176">Důrazně doporučujeme použít model nasazení Azure Resource Manageru pro SAP instalací.</span><span class="sxs-lookup"><span data-stu-id="76079-176">We strongly recommend that you use the Azure Resource Manager deployment model for your SAP installations.</span></span> <span data-ttu-id="76079-177">Nabízí spoustu výhod, které nejsou k dispozici v modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="76079-177">It offers many benefits that are not available in the classic deployment model.</span></span> <span data-ttu-id="76079-178">Další informace o Azure [modely nasazení][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span><span class="sxs-lookup"><span data-stu-id="76079-178">Learn more about Azure [deployment models][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span></span>   
>
>

## <span data-ttu-id="76079-179"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server Failover Clustering</span><span class="sxs-lookup"><span data-stu-id="76079-179"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a> Windows Server Failover Clustering</span></span>
<span data-ttu-id="76079-180">Windows Server Failover Clustering je základem SAP ASC nebo SCS instalace vysokou dostupnost a databázového systému v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="76079-180">Windows Server Failover Clustering is the foundation of a high-availability SAP ASCS/SCS installation and DBMS in Windows.</span></span>

<span data-ttu-id="76079-181">Cluster s podporou převzetí služeb při selhání je skupina 1 + n nezávislých serverů (uzlů) které vzájemně spolupracují a zvyšují tak dostupnost aplikací a služeb.</span><span class="sxs-lookup"><span data-stu-id="76079-181">A failover cluster is a group of 1+n independent servers (nodes) that work together to increase the availability of applications and services.</span></span> <span data-ttu-id="76079-182">Pokud dojde k selhání uzlu, Windows Server Failover Clustering vypočítá počet chyb, ke kterým dochází při zachování pořádku cluster, který poskytuje aplikacím a službám.</span><span class="sxs-lookup"><span data-stu-id="76079-182">If a node failure occurs, Windows Server Failover Clustering calculates the number of failures that can occur while maintaining a healthy cluster to provide applications and services.</span></span> <span data-ttu-id="76079-183">Můžete z různých kvora režimy k dosažení clustering převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="76079-183">You can choose from different quorum modes to achieve failover clustering.</span></span>

### <span data-ttu-id="76079-184"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Režim kvora</span><span class="sxs-lookup"><span data-stu-id="76079-184"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a> Quorum modes</span></span>
<span data-ttu-id="76079-185">Při použití služby Windows Server Failover Clustering, můžete vybrat ze čtyř režimů kvora:</span><span class="sxs-lookup"><span data-stu-id="76079-185">You can choose from four quorum modes when you use Windows Server Failover Clustering:</span></span>

* <span data-ttu-id="76079-186">**Většina uzlů**.</span><span class="sxs-lookup"><span data-stu-id="76079-186">**Node Majority**.</span></span> <span data-ttu-id="76079-187">Každý uzel clusteru hlasovat.</span><span class="sxs-lookup"><span data-stu-id="76079-187">Each node of the cluster can vote.</span></span> <span data-ttu-id="76079-188">Cluster funguje pouze s většinou hlasů, který je s víc než poloviny hlasy.</span><span class="sxs-lookup"><span data-stu-id="76079-188">The cluster functions only with a majority of votes, that is, with more than half the votes.</span></span> <span data-ttu-id="76079-189">Doporučujeme tuto možnost pro clustery, které mají lichý počet uzlů.</span><span class="sxs-lookup"><span data-stu-id="76079-189">We recommend this option for clusters that have an uneven number of nodes.</span></span> <span data-ttu-id="76079-190">Například může selhat tři uzly v clusteru s podporou sedmi a nepřesahuje clusteru dosahuje většině a pokračuje v činnosti.</span><span class="sxs-lookup"><span data-stu-id="76079-190">For example, three nodes in a seven-node cluster can fail, and the cluster stills achieves a majority and continues to run.</span></span>  
* <span data-ttu-id="76079-191">**Většina uzlů a disků**.</span><span class="sxs-lookup"><span data-stu-id="76079-191">**Node and Disk Majority**.</span></span> <span data-ttu-id="76079-192">Každý uzel a určený disk (disk s kopií clusteru) v úložišti clusteru můžete hlasovat, když jsou k dispozici a komunikace.</span><span class="sxs-lookup"><span data-stu-id="76079-192">Each node and a designated disk (a disk witness) in the cluster storage can vote when they are available and in communication.</span></span> <span data-ttu-id="76079-193">Cluster funguje pouze s většinou hlasů, který je s víc než poloviny hlasy.</span><span class="sxs-lookup"><span data-stu-id="76079-193">The cluster functions only with a majority of the votes, that is, with more than half the votes.</span></span> <span data-ttu-id="76079-194">Tento režim má smysl v prostředí clusteru s sudé číslo uzlů.</span><span class="sxs-lookup"><span data-stu-id="76079-194">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="76079-195">Pokud se polovina uzlů a disku jsou online, cluster zůstane v dobrém stavu.</span><span class="sxs-lookup"><span data-stu-id="76079-195">If half the nodes and the disk are online, the cluster remains in a healthy state.</span></span>
* <span data-ttu-id="76079-196">**Uzel a sdílených složek většina**.</span><span class="sxs-lookup"><span data-stu-id="76079-196">**Node and File Share Majority**.</span></span> <span data-ttu-id="76079-197">Každý uzel plus určené sdílené složky (soubor určující sdílenou složku), kterou vytvoří hlasovat, bez ohledu na to, jestli jsou k dispozici uzlů a sdílení souborů a v komunikaci.</span><span class="sxs-lookup"><span data-stu-id="76079-197">Each node plus a designated file share (a file share witness) that the administrator creates can vote, regardless of whether the nodes and file share are available and in communication.</span></span> <span data-ttu-id="76079-198">Cluster funguje pouze s většinou hlasů, který je s víc než poloviny hlasy.</span><span class="sxs-lookup"><span data-stu-id="76079-198">The cluster functions only with a majority of the votes, that is, with more than half the votes.</span></span> <span data-ttu-id="76079-199">Tento režim má smysl v prostředí clusteru s sudé číslo uzlů.</span><span class="sxs-lookup"><span data-stu-id="76079-199">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="76079-200">Je podobná režimu Většina uzlů a disků, ale používá určující sdílené složky namísto disku s kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-200">It's similar to the Node and Disk Majority mode, but it uses a witness file share instead of a witness disk.</span></span> <span data-ttu-id="76079-201">Tento režim je snadno implementovat, ale pokud sdílená samotné není vysoce dostupný, může být jediný bod selhání.</span><span class="sxs-lookup"><span data-stu-id="76079-201">This mode is easy to implement, but if the file share itself is not highly available, it might become a single point of failure.</span></span>
* <span data-ttu-id="76079-202">**Bez většiny: Na disku jenom**.</span><span class="sxs-lookup"><span data-stu-id="76079-202">**No Majority: Disk Only**.</span></span> <span data-ttu-id="76079-203">Cluster má kvorum, pokud jeden uzel je k dispozici a komunikace s konkrétním diskem v úložišti clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-203">The cluster has a quorum if one node is available and in communication with a specific disk in the cluster storage.</span></span> <span data-ttu-id="76079-204">Pouze uzly, které jsou také v komunikaci s tento disk může připojit ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-204">Only the nodes that also are in communication with that disk can join the cluster.</span></span> <span data-ttu-id="76079-205">Doporučujeme vám, nepoužívejte tento režim.</span><span class="sxs-lookup"><span data-stu-id="76079-205">We recommend that you do not use this mode.</span></span>
 

## <span data-ttu-id="76079-206"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server převzetí služeb při selhání Clustering na místě</span><span class="sxs-lookup"><span data-stu-id="76079-206"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a> Windows Server Failover Clustering on-premises</span></span>
<span data-ttu-id="76079-207">Obrázek 1 ukazuje dva uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-207">Figure 1 shows a cluster of two nodes.</span></span> <span data-ttu-id="76079-208">Pokud síťové připojení mezi uzly selže a jak zůstat uzly nahoru a spuštěna, disky kvora nebo soubor sdílet Určuje, který uzel bude pokračovat v poskytování aplikace a služby clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-208">If the network connection between the nodes fails and both nodes stay up and running, a quorum disk or file share determines which node will continue to provide the cluster's applications and services.</span></span> <span data-ttu-id="76079-209">Uzel, který má přístup k disku nebo sdílené složky kvora je uzlu, který zajistí, že služby pokračovat.</span><span class="sxs-lookup"><span data-stu-id="76079-209">The node that has access to the quorum disk or file share is the node that ensures that services continue.</span></span>

<span data-ttu-id="76079-210">Protože tento příklad používá dva uzly clusteru, použijeme Režim kvora Majority sdílené složky souborů a uzlu.</span><span class="sxs-lookup"><span data-stu-id="76079-210">Because this example uses a two-node cluster, we use the Node and File Share Majority quorum mode.</span></span> <span data-ttu-id="76079-211">Většina uzlů a disků je taky platná možnost.</span><span class="sxs-lookup"><span data-stu-id="76079-211">The Node and Disk Majority also is a valid option.</span></span> <span data-ttu-id="76079-212">V produkčním prostředí doporučujeme použít disk kvora.</span><span class="sxs-lookup"><span data-stu-id="76079-212">In a production environment, we recommend that you use a quorum disk.</span></span> <span data-ttu-id="76079-213">Technologie systému sítě a úložiště můžete nastavit jej jako vysoce dostupný.</span><span class="sxs-lookup"><span data-stu-id="76079-213">You can use network and storage system technology to make it highly available.</span></span>

![Obrázek 1: Příklad konfigurace služby systému Windows Server Failover Clustering pro SAP ASC nebo SCS v Azure][sap-ha-guide-figure-1000]

<span data-ttu-id="76079-215">_**Obrázek 1:** příklad konfigurace služby systému Windows Server Failover Clustering pro SAP ASC nebo SCS v Azure_</span><span class="sxs-lookup"><span data-stu-id="76079-215">_**Figure 1:** Example of a Windows Server Failover Clustering configuration for SAP ASCS/SCS in Azure_</span></span>

### <span data-ttu-id="76079-216"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Sdílené úložiště</span><span class="sxs-lookup"><span data-stu-id="76079-216"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a> Shared storage</span></span>
<span data-ttu-id="76079-217">Obrázek 1 zobrazuje i sdílené úložiště dvěma uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-217">Figure 1 also shows a two-node shared storage cluster.</span></span> <span data-ttu-id="76079-218">V clusteru služby místní sdílené úložiště rozpoznat všechny uzly v clusteru sdíleného úložiště.</span><span class="sxs-lookup"><span data-stu-id="76079-218">In an on-premises shared storage cluster, all nodes in the cluster detect shared storage.</span></span> <span data-ttu-id="76079-219">Mechanismus uzamčení chrání data před poškozením.</span><span class="sxs-lookup"><span data-stu-id="76079-219">A locking mechanism protects the data from corruption.</span></span> <span data-ttu-id="76079-220">Všechny uzly, můžete zjistit, pokud jiný uzel selže.</span><span class="sxs-lookup"><span data-stu-id="76079-220">All nodes can detect if another node fails.</span></span> <span data-ttu-id="76079-221">V případě selhání jednoho uzlu na zbývající uzel převezme vlastnictví prostředků úložiště a zajišťuje dostupnost služby.</span><span class="sxs-lookup"><span data-stu-id="76079-221">If one node fails, the remaining node takes ownership of the storage resources and ensures the availability of services.</span></span>

> [!NOTE]
> <span data-ttu-id="76079-222">Nepotřebujete sdílené disky pro zajištění vysoké dostupnosti s některými aplikacemi databázového systému, třeba s SQL serverem.</span><span class="sxs-lookup"><span data-stu-id="76079-222">You don't need shared disks for high availability with some DBMS applications, like with SQL Server.</span></span> <span data-ttu-id="76079-223">SQL serveru Always On replikuje databázového systému souborů protokolu a data z místního disku jednoho uzlu na místní disk jiným uzlem clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-223">SQL Server Always On replicates DBMS data and log files from the local disk of one cluster node to the local disk of another cluster node.</span></span> <span data-ttu-id="76079-224">Konfigurace clusteru systému Windows v takovém případě nemusí sdíleného disku.</span><span class="sxs-lookup"><span data-stu-id="76079-224">In that case, the Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="76079-225"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Sítě a překladu</span><span class="sxs-lookup"><span data-stu-id="76079-225"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a> Networking and name resolution</span></span>
<span data-ttu-id="76079-226">Klientské počítače přístup clusteru přes virtuální IP adresu a název virtuálního hostitele, který poskytuje DNS server.</span><span class="sxs-lookup"><span data-stu-id="76079-226">Client computers reach the cluster over a virtual IP address and a virtual host name that the DNS server provides.</span></span> <span data-ttu-id="76079-227">Uzly místně a DNS server může zpracovat více IP adres.</span><span class="sxs-lookup"><span data-stu-id="76079-227">The on-premises nodes and the DNS server can handle multiple IP addresses.</span></span>

<span data-ttu-id="76079-228">V typické instalace můžete používat dva nebo více připojení k síti:</span><span class="sxs-lookup"><span data-stu-id="76079-228">In a typical setup, you use two or more network connections:</span></span>

* <span data-ttu-id="76079-229">Vyhrazené připojení k úložišti</span><span class="sxs-lookup"><span data-stu-id="76079-229">A dedicated connection to the storage</span></span>
* <span data-ttu-id="76079-230">Cluster interní síťové připojení pro prezenční signál</span><span class="sxs-lookup"><span data-stu-id="76079-230">A cluster-internal network connection for the heartbeat</span></span>
* <span data-ttu-id="76079-231">Veřejné síti, který klienti používat pro připojení ke clusteru</span><span class="sxs-lookup"><span data-stu-id="76079-231">A public network that clients use to connect to the cluster</span></span>

## <span data-ttu-id="76079-232"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Server Failover Clustering v Azure</span><span class="sxs-lookup"><span data-stu-id="76079-232"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a> Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="76079-233">Porovnání s úplné nasazení systému nebo privátního cloudu, virtuálních počítačů Azure vyžaduje další kroky konfigurace, Windows Server Failover Clustering.</span><span class="sxs-lookup"><span data-stu-id="76079-233">Compared to bare metal or private cloud deployments, Azure Virtual Machines requires additional steps to configure Windows Server Failover Clustering.</span></span> <span data-ttu-id="76079-234">Při sestavování sdíleném disku clusteru, budete muset nastavit několik IP adresy a názvy virtuálních hostitelů pro SAP ASC nebo SCS instanci.</span><span class="sxs-lookup"><span data-stu-id="76079-234">When you build a shared cluster disk, you need to set several IP addresses and virtual host names for the SAP ASCS/SCS instance.</span></span>

<span data-ttu-id="76079-235">V tomto článku probereme klíčové koncepty a další kroky potřebné k vytvoření clusteru služby SAP centrální služby vysoké dostupnosti v Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-235">In this article, we discuss key concepts and the additional steps required to build an SAP high-availability central services cluster in Azure.</span></span> <span data-ttu-id="76079-236">Ukážeme vám, jak nastavit nástroj třetí strany s DataKeeper a konfiguraci nástroje pro vyrovnávání zatížení Azure interní.</span><span class="sxs-lookup"><span data-stu-id="76079-236">We show you how to set up the third-party tool SIOS DataKeeper, and how to configure the Azure internal load balancer.</span></span> <span data-ttu-id="76079-237">Tyto nástroje slouží k vytvoření clusteru s podporou převzetí služeb při selhání systému Windows s určující sdílené složce v Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-237">You can use these tools to create a Windows failover cluster with a file share witness in Azure.</span></span>

![Obrázek 2: Windows Server Failover Clustering konfiguraci v Azure bez sdíleného disku][sap-ha-guide-figure-1001]

<span data-ttu-id="76079-239">_**Obrázek 2:** konfiguraci služby Windows Server Failover Clustering v Azure bez sdíleného disku_</span><span class="sxs-lookup"><span data-stu-id="76079-239">_**Figure 2:** Windows Server Failover Clustering configuration in Azure without a shared disk_</span></span>

### <span data-ttu-id="76079-240"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>Sdílený disk v Azure s DataKeeper s</span><span class="sxs-lookup"><span data-stu-id="76079-240"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a> Shared disk in Azure with SIOS DataKeeper</span></span>
<span data-ttu-id="76079-241">Třeba clusteru sdíleného úložiště pro instanci SAP ASC nebo SCS vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="76079-241">You need cluster shared storage for a high-availability SAP ASCS/SCS instance.</span></span> <span data-ttu-id="76079-242">Od září 2016 není Azure nabízí sdílené úložiště, které můžete použít k vytvoření clusteru sdíleného úložiště.</span><span class="sxs-lookup"><span data-stu-id="76079-242">As of September 2016, Azure doesn't offer shared storage that you can use to create a shared storage cluster.</span></span> <span data-ttu-id="76079-243">Software třetích stran s DataKeeper Cluster Edition slouží k vytvoření zrcadlené úložiště, která simuluje sdílené úložiště clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-243">You can use third-party software SIOS DataKeeper Cluster Edition to create a mirrored storage that simulates cluster shared storage.</span></span> <span data-ttu-id="76079-244">Řešení SIOS poskytuje data v reálném čase synchronní replikace.</span><span class="sxs-lookup"><span data-stu-id="76079-244">The SIOS solution provides real-time synchronous data replication.</span></span> <span data-ttu-id="76079-245">Toto je, jak můžete vytvořit prostředek sdíleného disku pro cluster:</span><span class="sxs-lookup"><span data-stu-id="76079-245">This is how you can create a shared disk resource for a cluster:</span></span>

1. <span data-ttu-id="76079-246">Připojte další disk pro každý z virtuálních počítačů (VM) v konfiguraci clusteru systému Windows.</span><span class="sxs-lookup"><span data-stu-id="76079-246">Attach an additional disk to each of the virtual machines (VMs) in a Windows cluster configuration.</span></span>
2. <span data-ttu-id="76079-247">V obou uzlech virtuální počítač spusťte s DataKeeper Cluster Edition.</span><span class="sxs-lookup"><span data-stu-id="76079-247">Run SIOS DataKeeper Cluster Edition on both virtual machine nodes.</span></span>
3. <span data-ttu-id="76079-248">Nakonfigurujte s DataKeeper Cluster Edition, aby ho zrcadlí obsah další disk připojený svazek ze zdrojového virtuálního počítače na další disk připojený objem cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="76079-248">Configure SIOS DataKeeper Cluster Edition so that it mirrors the content of the additional disk attached volume from the source virtual machine to the additional disk attached volume of the target virtual machine.</span></span> <span data-ttu-id="76079-249">S DataKeeper abstrahuje zdrojové a cílové místní svazky a k jejich zobrazení na Windows Server Failover Clustering jako jeden sdílený disk.</span><span class="sxs-lookup"><span data-stu-id="76079-249">SIOS DataKeeper abstracts the source and target local volumes, and then presents them to Windows Server Failover Clustering as one shared disk.</span></span>

<span data-ttu-id="76079-250">Přečtěte si další informace o [s DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span><span class="sxs-lookup"><span data-stu-id="76079-250">Get more information about [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span></span>

![Obrázek 3: Konfigurace služby systému Windows Server Failover Clustering v Azure pomocí DataKeeper s][sap-ha-guide-figure-1002]

<span data-ttu-id="76079-252">_**Obrázek 3:** konfigurace služby systému Windows Server Failover Clustering v Azure pomocí DataKeeper s_</span><span class="sxs-lookup"><span data-stu-id="76079-252">_**Figure 3:** Windows Server Failover Clustering configuration in Azure with SIOS DataKeeper_</span></span>

> [!NOTE]
> <span data-ttu-id="76079-253">Pro zajištění vysoké dostupnosti se některé produkty, databázového systému, jako je SQL Server není třeba sdílené disky.</span><span class="sxs-lookup"><span data-stu-id="76079-253">You don't need shared disks for high availability with some DBMS products, like SQL Server.</span></span> <span data-ttu-id="76079-254">SQL serveru Always On replikuje databázového systému souborů protokolu a data z místního disku jednoho uzlu na místní disk jiným uzlem clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-254">SQL Server Always On replicates DBMS data and log files from the local disk of one cluster node to the local disk of another cluster node.</span></span> <span data-ttu-id="76079-255">Konfigurace clusteru systému Windows v takovém případě nemusí sdíleného disku.</span><span class="sxs-lookup"><span data-stu-id="76079-255">In this case, the Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="76079-256"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>Rozpoznání názvu v Azure</span><span class="sxs-lookup"><span data-stu-id="76079-256"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a> Name resolution in Azure</span></span>
<span data-ttu-id="76079-257">Azure Cloudová platforma nenabízí možnost konfigurovat virtuální IP adresy, například plovoucí IP adresy.</span><span class="sxs-lookup"><span data-stu-id="76079-257">The Azure cloud platform doesn't offer the option to configure virtual IP addresses, such as floating IP addresses.</span></span> <span data-ttu-id="76079-258">Je třeba nastavit virtuální IP adresy k dosažení prostředek clusteru v cloudu alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="76079-258">You need an alternative solution to set up a virtual IP address to reach the cluster resource in the cloud.</span></span>
<span data-ttu-id="76079-259">Azure má k nástroji pro vyrovnávání zatížení pro vnitřní ve službě Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-259">Azure has an internal load balancer in the Azure Load Balancer service.</span></span> <span data-ttu-id="76079-260">S nástrojem pro vyrovnávání zatížení pro vnitřní klienti moci clusteru připojit přes virtuální IP adresu clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-260">With the internal load balancer, clients reach the cluster over the cluster virtual IP address.</span></span>
<span data-ttu-id="76079-261">Je třeba nasadit nástroj pro vyrovnávání zatížení interní ve skupině prostředků, který obsahuje uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-261">You need to deploy the internal load balancer in the resource group that contains the cluster nodes.</span></span> <span data-ttu-id="76079-262">Potom nakonfigurujte všechny nezbytné port předávání pravidla s sondy porty nástroje pro vyrovnávání zatížení interní.</span><span class="sxs-lookup"><span data-stu-id="76079-262">Then, configure all necessary port forwarding rules with the probe ports of the internal load balancer.</span></span>
<span data-ttu-id="76079-263">Můžete se klienti připojují prostřednictvím název virtuálního hostitele.</span><span class="sxs-lookup"><span data-stu-id="76079-263">The clients can connect via the virtual host name.</span></span> <span data-ttu-id="76079-264">DNS server přeloží IP adresu clusteru a port obslužných rutin vyrovnávání interní služby load předávání do aktivního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-264">The DNS server resolves the cluster IP address, and the internal load balancer handles port forwarding to the active node of the cluster.</span></span>

## <span data-ttu-id="76079-265"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>SAP NetWeaver vysoké dostupnosti v Azure infrastruktury jako služba (IaaS)</span><span class="sxs-lookup"><span data-stu-id="76079-265"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a> SAP NetWeaver high availability in Azure Infrastructure-as-a-Service (IaaS)</span></span>
<span data-ttu-id="76079-266">K dosažení vysoké dostupnosti aplikace SAP pro SAP softwarové komponenty, jako je třeba chránit následující součásti:</span><span class="sxs-lookup"><span data-stu-id="76079-266">To achieve SAP application high availability, such as for SAP software components, you need to protect the following components:</span></span>

* <span data-ttu-id="76079-267">Instance aplikačního serveru SAP</span><span class="sxs-lookup"><span data-stu-id="76079-267">SAP Application Server instance</span></span>
* <span data-ttu-id="76079-268">Instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="76079-268">SAP ASCS/SCS instance</span></span>
* <span data-ttu-id="76079-269">Server databázového systému</span><span class="sxs-lookup"><span data-stu-id="76079-269">DBMS server</span></span>

<span data-ttu-id="76079-270">Další informace o ochraně SAP součásti ve scénářích s vysokou dostupností najdete v tématu [virtuální počítače Azure plánování a implementace pro SAP NetWeaver][planning-guide-11].</span><span class="sxs-lookup"><span data-stu-id="76079-270">For more information about protecting SAP components in high-availability scenarios, see [Azure Virtual Machines planning and implementation for SAP NetWeaver][planning-guide-11].</span></span>

### <span data-ttu-id="76079-271"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Vysoká dostupnost SAP aplikační Server</span><span class="sxs-lookup"><span data-stu-id="76079-271"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a> High-availability SAP Application Server</span></span>
<span data-ttu-id="76079-272">Obvykle nepotřebujete specifického řešení vysoké dostupnosti pro instance aplikačního serveru SAP a dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="76079-272">You usually don't need a specific high-availability solution for the SAP Application Server and dialog instances.</span></span> <span data-ttu-id="76079-273">Dosažení vysoké dostupnosti pomocí redundance a nakonfigurujete více instancí dialogové okno v různých instancí virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-273">You achieve high availability by redundancy, and you'll configure multiple dialog instances in different instances of Azure Virtual Machines.</span></span> <span data-ttu-id="76079-274">Měli byste mít aspoň dvě instance SAP aplikace nainstalované v dvě instance virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-274">You should have at least two SAP application instances installed in two instances of Azure Virtual Machines.</span></span>

![Obrázek 4: Vysoké dostupnosti SAP aplikační Server][sap-ha-guide-figure-2000]

<span data-ttu-id="76079-276">_**Obrázek 4:** vysokou dostupnost SAP aplikační Server_</span><span class="sxs-lookup"><span data-stu-id="76079-276">_**Figure 4:** High-availability SAP Application Server_</span></span>

<span data-ttu-id="76079-277">Je nutné umístit všechny virtuální počítače, které instance aplikačního serveru SAP hostitele ve stejném Azure dostupnosti nastavit.</span><span class="sxs-lookup"><span data-stu-id="76079-277">You must place all virtual machines that host SAP Application Server instances in the same Azure availability set.</span></span> <span data-ttu-id="76079-278">Nastavení Azure dostupnosti zajišťuje, že:</span><span class="sxs-lookup"><span data-stu-id="76079-278">An Azure availability set ensures that:</span></span>

* <span data-ttu-id="76079-279">Všechny virtuální počítače jsou součástí stejné domény upgradu.</span><span class="sxs-lookup"><span data-stu-id="76079-279">All virtual machines are part of the same upgrade domain.</span></span> <span data-ttu-id="76079-280">Upgradovací doméně, například zajišťuje, že virtuální počítače se neaktualizují ve stejnou dobu během plánované údržby. výpadků.</span><span class="sxs-lookup"><span data-stu-id="76079-280">An upgrade domain, for example, makes sure that the virtual machines aren't updated at the same time during planned maintenance downtime.</span></span>
* <span data-ttu-id="76079-281">Všechny virtuální počítače jsou součástí stejné domény selhání.</span><span class="sxs-lookup"><span data-stu-id="76079-281">All virtual machines are part of the same fault domain.</span></span> <span data-ttu-id="76079-282">Domény selhání, například zajišťuje nasazení virtuálních počítačů, tak, aby žádný jediný bod selhání má vliv na dostupnost všech virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="76079-282">A fault domain, for example, makes sure that virtual machines are deployed so that no single point of failure affects the availability of all virtual machines.</span></span>

<span data-ttu-id="76079-283">Další informace o tom, jak [Správa dostupnosti virtuálních počítačů][virtual-machines-manage-availability].</span><span class="sxs-lookup"><span data-stu-id="76079-283">Learn more about how to [manage the availability of virtual machines][virtual-machines-manage-availability].</span></span>

<span data-ttu-id="76079-284">Pouze nespravovaná disk: účet úložiště Azure je potenciální jediný bod selhání, je důležité mít alespoň dva účty úložiště Azure, ve kterých jsou distribuovány aspoň dva virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="76079-284">Unmanaged disk only: Because the Azure storage account is a potential single point of failure, it's important to have at least two Azure storage accounts, in which at least two virtual machines are distributed.</span></span> <span data-ttu-id="76079-285">V ideální instalační program by se nasadí disky každého virtuálního počítače, na kterém běží instance SAP dialogové okno jiný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="76079-285">In an ideal setup, the disks of each virtual machine that is running an SAP dialog instance would be deployed in a different storage account.</span></span>

### <span data-ttu-id="76079-286"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Instance SAP ASC nebo SCS vysokou dostupnost</span><span class="sxs-lookup"><span data-stu-id="76079-286"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a> High-availability SAP ASCS/SCS instance</span></span>
<span data-ttu-id="76079-287">Obrázek 5 je příklad instance SAP ASC nebo SCS vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="76079-287">Figure 5 is an example of a high-availability SAP ASCS/SCS instance.</span></span>

![Obrázek 5: Vysoká dostupnost SAP ASC nebo SCS instance][sap-ha-guide-figure-2001]

<span data-ttu-id="76079-289">_**Obrázek 5:** vysokou dostupnost SAP ASC nebo SCS instance_</span><span class="sxs-lookup"><span data-stu-id="76079-289">_**Figure 5:** High-availability SAP ASCS/SCS instance_</span></span>

#### <span data-ttu-id="76079-290"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>SAP ASC nebo SCS instance vysoká dostupnost s Windows Server Failover Clustering v Azure</span><span class="sxs-lookup"><span data-stu-id="76079-290"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a> SAP ASCS/SCS instance high availability with Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="76079-291">Porovnání s úplné nasazení systému nebo privátního cloudu, virtuálních počítačů Azure vyžaduje další kroky konfigurace, Windows Server Failover Clustering.</span><span class="sxs-lookup"><span data-stu-id="76079-291">Compared to bare metal or private cloud deployments, Azure Virtual Machines requires additional steps to configure Windows Server Failover Clustering.</span></span> <span data-ttu-id="76079-292">K vytvoření clusteru s podporou převzetí služeb při selhání systému Windows, musíte pro clustering instance SAP ASC nebo SCS sdíleném disku clusteru, několik IP adres, několik názvy virtuálních hostitelů a k nástroji pro vyrovnávání zatížení Azure interní.</span><span class="sxs-lookup"><span data-stu-id="76079-292">To build a Windows failover cluster, you need a shared cluster disk, several IP addresses, several virtual host names, and an Azure internal load balancer for clustering an SAP ASCS/SCS instance.</span></span> <span data-ttu-id="76079-293">To probereme podrobněji později v článku.</span><span class="sxs-lookup"><span data-stu-id="76079-293">We discuss this in more detail later in the article.</span></span>

![Obrázek 6: Windows Server Failover Clustering pro konfiguraci SAP ASC nebo SCS v Azure pomocí DataKeeper s][sap-ha-guide-figure-1002]

<span data-ttu-id="76079-295">_**Obrázek 6:** Windows Server Failover Clustering SAP ASC nebo SCS konfigurace v Azure pomocí DataKeeper s_</span><span class="sxs-lookup"><span data-stu-id="76079-295">_**Figure 6:** Windows Server Failover Clustering for an SAP ASCS/SCS configuration in Azure with SIOS DataKeeper_</span></span>

### <span data-ttu-id="76079-296"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Instance databázového systému vysokou dostupnost</span><span class="sxs-lookup"><span data-stu-id="76079-296"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>High-availability DBMS instance</span></span>
<span data-ttu-id="76079-297">Systému správy databáze je také jeden kontaktní bod v systému SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-297">The DBMS also is a single point of contact in an SAP system.</span></span> <span data-ttu-id="76079-298">Potřebujete chránit pomocí řešení vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="76079-298">You need to protect it by using a high-availability solution.</span></span> <span data-ttu-id="76079-299">Obrázek 7 znázorňuje řešení vysoké dostupnosti SQL serveru Always On v Azure, Windows Server Failover Clustering a Azure interní nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="76079-299">Figure 7 shows a SQL Server Always On high-availability solution in Azure, with Windows Server Failover Clustering and the Azure internal load balancer.</span></span> <span data-ttu-id="76079-300">SQL serveru Always On replikuje pomocí replikace vlastní databázového systému databázového systému data a soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="76079-300">SQL Server Always On replicates DBMS data and log files by using its own DBMS replication.</span></span> <span data-ttu-id="76079-301">V takovém případě můžete nebudete potřebovat clusteru sdílené disky, což zjednodušuje celý nastavení.</span><span class="sxs-lookup"><span data-stu-id="76079-301">In this case, you don't need cluster shared disks, which simplifies the entire setup.</span></span>

![Obrázek 7: Příklad databázového systému vysoké dostupnosti SAP, s SQL serveru Always On][sap-ha-guide-figure-2003]

<span data-ttu-id="76079-303">_**Obrázek 7:** příklad databázového systému vysoké dostupnosti SAP, s SQL serveru Always On_</span><span class="sxs-lookup"><span data-stu-id="76079-303">_**Figure 7:** Example of a high-availability SAP DBMS, with SQL Server Always On_</span></span>

<span data-ttu-id="76079-304">Další informace o clusteringu SQL Server v Azure pomocí modelu nasazení Azure Resource Manager najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="76079-304">For more information about clustering SQL Server in Azure by using the Azure Resource Manager deployment model, see these articles:</span></span>

* <span data-ttu-id="76079-305">[Konfigurace skupiny dostupnosti Always On v Azure Virtual Machines ručně pomocí Resource Manager] [virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span><span class="sxs-lookup"><span data-stu-id="76079-305">[Configure Always On availability group in Azure Virtual Machines manually by using Resource Manager][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span></span>
* <span data-ttu-id="76079-306">[Konfigurace k nástroji pro vyrovnávání zatížení Azure interní pro skupiny dostupnosti Always On v Azure] [virtual-machines-windows-portal-sql-alwayson-int-listener]</span><span class="sxs-lookup"><span data-stu-id="76079-306">[Configure an Azure internal load balancer for an Always On availability group in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]</span></span>

## <span data-ttu-id="76079-307"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>Scénáře nasazení vysoké dostupnosti začátku do konce</span><span class="sxs-lookup"><span data-stu-id="76079-307"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a> End-to-end high-availability deployment scenarios</span></span>

### <a name="deployment-scenario-using-architectural-template-1"></a><span data-ttu-id="76079-308">Scénář nasazení pomocí architektury šablony 1</span><span class="sxs-lookup"><span data-stu-id="76079-308">Deployment scenario using Architectural Template 1</span></span>

<span data-ttu-id="76079-309">Obrázek 8 ukazuje příklad architektury SAP NetWeaver vysoké dostupnosti v Azure pro **jeden** systému SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-309">Figure 8 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="76079-310">Tento scénář je nastavit takto:</span><span class="sxs-lookup"><span data-stu-id="76079-310">This scenario is set up as follows:</span></span>

- <span data-ttu-id="76079-311">Jeden vyhrazený cluster se používá pro SAP ASC nebo SCS instanci.</span><span class="sxs-lookup"><span data-stu-id="76079-311">One dedicated cluster is used for the SAP ASCS/SCS instance.</span></span>
- <span data-ttu-id="76079-312">Jeden vyhrazený cluster se používá pro instanci databázového systému.</span><span class="sxs-lookup"><span data-stu-id="76079-312">One dedicated cluster is used for the DBMS instance.</span></span>
- <span data-ttu-id="76079-313">Instance aplikačního serveru SAP nasazených ve svých vlastních vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="76079-313">SAP Application Server instances are deployed in their own dedicated VMs.</span></span>

![Obrázek 8: SAP vysokou dostupnost architektury šablony 1 vyhrazeném clusteru pro ASC nebo SCS a databázového systému][sap-ha-guide-figure-2004]

<span data-ttu-id="76079-315">_**Obrázek 8:** SAP architektury 1 šablony vysokou dostupnost, vyhrazené clustery pro ASC nebo SCS a databázového systému_</span><span class="sxs-lookup"><span data-stu-id="76079-315">_**Figure 8:** SAP high-availability Architectural Template 1, dedicated clusters for ASCS/SCS and DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-2"></a><span data-ttu-id="76079-316">Scénář nasazení pomocí architektury šablony 2</span><span class="sxs-lookup"><span data-stu-id="76079-316">Deployment scenario using Architectural Template 2</span></span>

<span data-ttu-id="76079-317">Obrázek 9 ukazuje příklad architektury SAP NetWeaver vysoké dostupnosti v Azure pro **jeden** systému SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-317">Figure 9 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="76079-318">Tento scénář je nastavit takto:</span><span class="sxs-lookup"><span data-stu-id="76079-318">This scenario is set up as follows:</span></span>

- <span data-ttu-id="76079-319">Jeden vyhrazený cluster se používá pro **i** instance SAP ASC nebo SCS a databázového systému.</span><span class="sxs-lookup"><span data-stu-id="76079-319">One dedicated cluster is used for **both** the SAP ASCS/SCS instance and the DBMS.</span></span>
- <span data-ttu-id="76079-320">Instance aplikačního serveru SAP jsou nasazeny v vlastní vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="76079-320">SAP Application Server instances are deployed in own dedicated VMs.</span></span>

![Obrázek 9: SAP vysokou dostupnost architektury šablony 2, přičemž vyhrazeném clusteru pro ASC nebo SCS a vyhrazeném clusteru pro databázového systému][sap-ha-guide-figure-2005]

<span data-ttu-id="76079-322">_**Obrázek 9:** SAP vysokou dostupnost architektury šablony 2, přičemž vyhrazeném clusteru pro ASC nebo SCS a vyhrazeném clusteru pro databázového systému_</span><span class="sxs-lookup"><span data-stu-id="76079-322">_**Figure 9:** SAP high-availability Architectural Template 2, with a dedicated cluster for ASCS/SCS and a dedicated cluster for DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-3"></a><span data-ttu-id="76079-323">Scénář nasazení pomocí architektury 3 šablony</span><span class="sxs-lookup"><span data-stu-id="76079-323">Deployment scenario using Architectural Template 3</span></span>

<span data-ttu-id="76079-324">Obrázek 10 ukazuje příklad architektury SAP NetWeaver vysoké dostupnosti v Azure pro **dva** SAP systémy, s &lt;SID1&gt; a &lt;SID2&gt;.</span><span class="sxs-lookup"><span data-stu-id="76079-324">Figure 10 shows an example of an SAP NetWeaver high-availability architecture in Azure for **two** SAP systems, with &lt;SID1&gt; and &lt;SID2&gt;.</span></span> <span data-ttu-id="76079-325">Tento scénář je nastavit takto:</span><span class="sxs-lookup"><span data-stu-id="76079-325">This scenario is set up as follows:</span></span>

- <span data-ttu-id="76079-326">Jeden vyhrazený cluster se používá pro **i** instance SAP ASC nebo SCS SID1 *a* instance SAP ASC nebo SCS SID2 (jeden cluster).</span><span class="sxs-lookup"><span data-stu-id="76079-326">One dedicated cluster is used for **both** the SAP ASCS/SCS SID1 instance *and* the SAP ASCS/SCS SID2 instance (one cluster).</span></span>
- <span data-ttu-id="76079-327">Jeden vyhrazený cluster se používá pro SID1 databázového systému a jiné vyhrazeném clusteru se používá pro SID2 databázového systému (dva clustery).</span><span class="sxs-lookup"><span data-stu-id="76079-327">One dedicated cluster is used for DBMS SID1, and another dedicated cluster is used for DBMS SID2 (two clusters).</span></span>
- <span data-ttu-id="76079-328">Instance aplikačního serveru SAP systému SAP SID1 mít vlastní vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="76079-328">SAP Application Server instances for the SAP system SID1 have their own dedicated VMs.</span></span>
- <span data-ttu-id="76079-329">Instance aplikačního serveru SAP systému SAP SID2 mít vlastní vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="76079-329">SAP Application Server instances for the SAP system SID2 have their own dedicated VMs.</span></span>

![Obrázek 10: Vysoká dostupnost architektury šablony 3, s clusterem vyhrazené pro různé ASC nebo SCS instance SAP][sap-ha-guide-figure-6003]

<span data-ttu-id="76079-331">_**Obrázek 10:** SAP vysokou dostupnost architektury šablony 3, s clusterem vyhrazené pro různé instance ASC nebo SCS_</span><span class="sxs-lookup"><span data-stu-id="76079-331">_**Figure 10:** SAP high-availability Architectural Template 3, with a dedicated cluster for different ASCS/SCS instances_</span></span>

## <span data-ttu-id="76079-332"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>Příprava infrastruktury</span><span class="sxs-lookup"><span data-stu-id="76079-332"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a> Prepare the infrastructure</span></span>

### <a name="prepare-the-infrastructure-for-architectural-template-1"></a><span data-ttu-id="76079-333">Příprava infrastruktury pro architektury šablony 1</span><span class="sxs-lookup"><span data-stu-id="76079-333">Prepare the infrastructure for Architectural Template 1</span></span>
<span data-ttu-id="76079-334">Šablony Azure Resource Manageru pro SAP zjednodušit nasazení požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="76079-334">Azure Resource Manager templates for SAP help simplify deployment of required resources.</span></span>

<span data-ttu-id="76079-335">Třívrstvá šablony ve službě Správce prostředků Azure také podporují scénáře vysoké dostupnosti, například architektury 1 šablony, která má dva clustery.</span><span class="sxs-lookup"><span data-stu-id="76079-335">The three-tier templates in Azure Resource Manager also support high-availability scenarios, such as in Architectural Template 1, which has two clusters.</span></span> <span data-ttu-id="76079-336">Každý cluster je SAP jediný bod selhání pro SAP ASC nebo SCS a databázového systému.</span><span class="sxs-lookup"><span data-stu-id="76079-336">Each cluster is an SAP single point of failure for SAP ASCS/SCS and DBMS.</span></span>

<span data-ttu-id="76079-337">Zde je, kde můžete získat šablon Azure Resource Manageru ukázkový scénář, který jsme popisují v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="76079-337">Here's where you can get Azure Resource Manager templates for the example scenario we describe in this article:</span></span>

* [<span data-ttu-id="76079-338">Obrázek pro Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="76079-338">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [<span data-ttu-id="76079-339">Obrázek pro Azure Marketplace pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="76079-339">Azure Marketplace image using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md)  
* [<span data-ttu-id="76079-340">Vlastní image</span><span class="sxs-lookup"><span data-stu-id="76079-340">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)
* [<span data-ttu-id="76079-341">Vlastní image pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="76079-341">Custom image using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-md)

<span data-ttu-id="76079-342">Příprava infrastruktury na architektury šablona 1:</span><span class="sxs-lookup"><span data-stu-id="76079-342">To prepare the infrastructure for Architectural Template 1:</span></span>

- <span data-ttu-id="76079-343">Na portálu Azure na **parametry** okno v **SYSTEMAVAILABILITY** vyberte **HA**.</span><span class="sxs-lookup"><span data-stu-id="76079-343">In the Azure portal, on the **Parameters** blade, in the **SYSTEMAVAILABILITY** box, select **HA**.</span></span>

  ![Obrázek 11: Nastavit SAP vysokou dostupnost Azure Resource Manageru parametry][sap-ha-guide-figure-3000]

<span data-ttu-id="76079-345">_**Obrázek 11:** nastavit parametry SAP vysokou dostupnost Azure Resource Manager_</span><span class="sxs-lookup"><span data-stu-id="76079-345">_**Figure 11:** Set SAP high-availability Azure Resource Manager parameters_</span></span>


  <span data-ttu-id="76079-346">Vytvoření šablony:</span><span class="sxs-lookup"><span data-stu-id="76079-346">The templates create:</span></span>

  * <span data-ttu-id="76079-347">**Virtuální počítače**:</span><span class="sxs-lookup"><span data-stu-id="76079-347">**Virtual machines**:</span></span>
    * <span data-ttu-id="76079-348">Virtuální počítače aplikačního serveru SAP: <*SAPSystemSID*> - di - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="76079-348">SAP Application Server virtual machines: <*SAPSystemSID*>-di-<*Number*></span></span>
    * <span data-ttu-id="76079-349">ASC nebo SCS clusteru virtuálních počítačů: <*SAPSystemSID*> - ASC - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="76079-349">ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-ascs-<*Number*></span></span>
    * <span data-ttu-id="76079-350">Cluster databázového systému: <*SAPSystemSID*> - db - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="76079-350">DBMS cluster: <*SAPSystemSID*>-db-<*Number*></span></span>

  * <span data-ttu-id="76079-351">**Síťové karty pro všechny virtuální počítače s přidružené IP adresy**:</span><span class="sxs-lookup"><span data-stu-id="76079-351">**Network cards for all virtual machines, with associated IP addresses**:</span></span>
    * <span data-ttu-id="76079-352"><*SAPSystemSID*> - nic - di - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="76079-352"><*SAPSystemSID*>-nic-di-<*Number*></span></span>
    * <span data-ttu-id="76079-353"><*SAPSystemSID*> - nic - ASC - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="76079-353"><*SAPSystemSID*>-nic-ascs-<*Number*></span></span>
    * <span data-ttu-id="76079-354"><*SAPSystemSID*> - nic - db - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="76079-354"><*SAPSystemSID*>-nic-db-<*Number*></span></span>

  * <span data-ttu-id="76079-355">**Účty služby Azure storage (pouze nespravovaná disků)**</span><span class="sxs-lookup"><span data-stu-id="76079-355">**Azure storage accounts (unmanaged disks only)**</span></span>

  * <span data-ttu-id="76079-356">**Skupiny dostupnosti** pro:</span><span class="sxs-lookup"><span data-stu-id="76079-356">**Availability groups** for:</span></span>
    * <span data-ttu-id="76079-357">Virtuální počítače aplikačního serveru SAP: <*SAPSystemSID*> - avset - di</span><span class="sxs-lookup"><span data-stu-id="76079-357">SAP Application Server virtual machines: <*SAPSystemSID*>-avset-di</span></span>
    * <span data-ttu-id="76079-358">SAP ASC nebo SCS clusteru virtuálních počítačů: <*SAPSystemSID*> - avset - ASC</span><span class="sxs-lookup"><span data-stu-id="76079-358">SAP ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-avset-ascs</span></span>
    * <span data-ttu-id="76079-359">Virtuální počítače clusteru databázového systému: <*SAPSystemSID*> - avset - db</span><span class="sxs-lookup"><span data-stu-id="76079-359">DBMS cluster virtual machines: <*SAPSystemSID*>-avset-db</span></span>

  * <span data-ttu-id="76079-360">**Nástroje pro vyrovnávání zatížení Azure interní**:</span><span class="sxs-lookup"><span data-stu-id="76079-360">**Azure internal load balancer**:</span></span>
    * <span data-ttu-id="76079-361">S všechny porty pro instanci ASC nebo SCS a IP adresu <*SAPSystemSID*> - lb - ASC</span><span class="sxs-lookup"><span data-stu-id="76079-361">With all ports for the ASCS/SCS instance and IP address <*SAPSystemSID*>-lb-ascs</span></span>
    * <span data-ttu-id="76079-362">S všechny porty pro SQL Server databázového systému a IP adresu <*SAPSystemSID*> - lb - db</span><span class="sxs-lookup"><span data-stu-id="76079-362">With all ports for the SQL Server DBMS and IP address <*SAPSystemSID*>-lb-db</span></span>

  * <span data-ttu-id="76079-363">**Skupina zabezpečení sítě**: <*SAPSystemSID*> - nsg - ASC-0</span><span class="sxs-lookup"><span data-stu-id="76079-363">**Network security group**: <*SAPSystemSID*>-nsg-ascs-0</span></span>  
    * <span data-ttu-id="76079-364">S k externí protokol RDP (Remote Desktop) portu k <*SAPSystemSID*> - ASC - 0 virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="76079-364">With an open external Remote Desktop Protocol (RDP) port to the <*SAPSystemSID*>-ascs-0 virtual machine</span></span>

> [!NOTE]
> <span data-ttu-id="76079-365">Jsou všechny IP adresy síťové karty a nástroje pro vyrovnávání zatížení Azure interní **dynamické** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="76079-365">All IP addresses of the network cards and Azure internal load balancers are **dynamic** by default.</span></span> <span data-ttu-id="76079-366">Změnit tak, aby **statické** IP adresy.</span><span class="sxs-lookup"><span data-stu-id="76079-366">Change them to **static** IP addresses.</span></span> <span data-ttu-id="76079-367">Jsme popisují, jak to udělat později v článku.</span><span class="sxs-lookup"><span data-stu-id="76079-367">We describe how to do this later in the article.</span></span>
>
>

### <span data-ttu-id="76079-368"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Nasazení virtuálních počítačů s připojením k podnikové síti (mezi různými místy) používat v produkčním prostředí</span><span class="sxs-lookup"><span data-stu-id="76079-368"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Deploy virtual machines with corporate network connectivity (cross-premises) to use in production</span></span>
<span data-ttu-id="76079-369">Pro produkční systémy SAP, nasaďte virtuální počítače Azure s [připojení k podnikové síti (mezi různými místy)] [ planning-guide-2.2] pomocí Azure Site-to-Site VPN nebo Azure ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="76079-369">For production SAP systems, deploy Azure virtual machines with [corporate network connectivity (cross-premises)][planning-guide-2.2] by using Azure Site-to-Site VPN or Azure ExpressRoute.</span></span>

> [!NOTE]
> <span data-ttu-id="76079-370">Můžete použít instanci Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="76079-370">You can use your Azure Virtual Network instance.</span></span> <span data-ttu-id="76079-371">Virtuální síť a podsíť již byly vytvořeny a připravený.</span><span class="sxs-lookup"><span data-stu-id="76079-371">The virtual network and subnet have already been created and prepared.</span></span>
>
>

1.  <span data-ttu-id="76079-372">Na portálu Azure na **parametry** okno v **NEWOREXISTINGSUBNET** vyberte **existující**.</span><span class="sxs-lookup"><span data-stu-id="76079-372">In the Azure portal, on the **Parameters** blade, in the **NEWOREXISTINGSUBNET** box, select **existing**.</span></span>
2.  <span data-ttu-id="76079-373">V **SUBNETID** pole, přidejte úplný řetězec připravené Azure sítě SubnetID, kam je plánujete nasadit virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-373">In the **SUBNETID** box, add the full string of your prepared Azure network SubnetID where you plan to deploy your Azure virtual machines.</span></span>
3.  <span data-ttu-id="76079-374">Chcete-li získat seznam všech podsítí síť Azure, spusťte tento příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="76079-374">To get a list of all Azure network subnets, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  <span data-ttu-id="76079-375">**ID** pole ukazuje **SUBNETID**.</span><span class="sxs-lookup"><span data-stu-id="76079-375">The **ID** field shows the **SUBNETID**.</span></span>
4. <span data-ttu-id="76079-376">Chcete-li získat seznam všech **SUBNETID** hodnoty, spusťte tento příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="76079-376">To get a list of all **SUBNETID** values, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  <span data-ttu-id="76079-377">**SUBNETID** vypadá podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="76079-377">The **SUBNETID** looks like this:</span></span>

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

### <span data-ttu-id="76079-378"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Nasadit jenom pro cloud instance SAP pro testovací a demo</span><span class="sxs-lookup"><span data-stu-id="76079-378"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Deploy cloud-only SAP instances for test and demo</span></span>
<span data-ttu-id="76079-379">Můžete nasadit systém SAP vysoké dostupnosti v modelu nasazení jenom cloudu.</span><span class="sxs-lookup"><span data-stu-id="76079-379">You can deploy your high-availability SAP system in a cloud-only deployment model.</span></span> <span data-ttu-id="76079-380">Tento typ nasazení je primárně užitečné pro ukázku a testovací případy použití.</span><span class="sxs-lookup"><span data-stu-id="76079-380">This kind of deployment primarily is useful for demo and test use cases.</span></span> <span data-ttu-id="76079-381">Ji není vhodná pro produkční případy použití.</span><span class="sxs-lookup"><span data-stu-id="76079-381">It's not suited for production use cases.</span></span>

- <span data-ttu-id="76079-382">Na portálu Azure na **parametry** okno v **NEWOREXISTINGSUBNET** vyberte **nové**.</span><span class="sxs-lookup"><span data-stu-id="76079-382">In the Azure portal, on the **Parameters** blade, in the **NEWOREXISTINGSUBNET** box, select **new**.</span></span> <span data-ttu-id="76079-383">Ponechte **SUBNETID** pole prázdná.</span><span class="sxs-lookup"><span data-stu-id="76079-383">Leave the **SUBNETID** field empty.</span></span>

  <span data-ttu-id="76079-384">Šablona SAP Azure Resource Manager automaticky vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="76079-384">The SAP Azure Resource Manager template automatically creates the Azure virtual network and subnet.</span></span>

> [!NOTE]
> <span data-ttu-id="76079-385">Musíte také nasadit alespoň jeden vyhrazený virtuální počítač pro Active Directory a DNS ve stejné instanci Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="76079-385">You also need to deploy at least one dedicated virtual machine for Active Directory and DNS in the same Azure Virtual Network instance.</span></span> <span data-ttu-id="76079-386">Šablona nepodporuje vytvoření těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="76079-386">The template doesn't create these virtual machines.</span></span>
>
>


### <a name="prepare-the-infrastructure-for-architectural-template-2"></a><span data-ttu-id="76079-387">Příprava infrastruktury pro architektury šablony 2</span><span class="sxs-lookup"><span data-stu-id="76079-387">Prepare the infrastructure for Architectural Template 2</span></span>

<span data-ttu-id="76079-388">Tato šablona Azure Resource Manageru pro SAP můžete zjednodušit nasazování požadované infrastruktury prostředků pro SAP architektury šablony 2.</span><span class="sxs-lookup"><span data-stu-id="76079-388">You can use this Azure Resource Manager template for SAP to help simplify deployment of required infrastructure resources for SAP Architectural Template 2.</span></span>

<span data-ttu-id="76079-389">Zde je, kde můžete získat šablon Azure Resource Manageru pro tento scénář nasazení:</span><span class="sxs-lookup"><span data-stu-id="76079-389">Here's where you can get Azure Resource Manager templates for this deployment scenario:</span></span>

* [<span data-ttu-id="76079-390">Obrázek pro Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="76079-390">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [<span data-ttu-id="76079-391">Obrázek pro Azure Marketplace pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="76079-391">Azure Marketplace image using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged-md)  
* [<span data-ttu-id="76079-392">Vlastní image</span><span class="sxs-lookup"><span data-stu-id="76079-392">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)
* [<span data-ttu-id="76079-393">Vlastní image pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="76079-393">Custom image using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged-md)


### <a name="prepare-the-infrastructure-for-architectural-template-3"></a><span data-ttu-id="76079-394">Příprava infrastruktury pro architektury 3 šablony</span><span class="sxs-lookup"><span data-stu-id="76079-394">Prepare the infrastructure for Architectural Template 3</span></span>

<span data-ttu-id="76079-395">Můžete Příprava infrastruktury a nakonfigurovat SAP pro **více SID**.</span><span class="sxs-lookup"><span data-stu-id="76079-395">You can prepare the infrastructure and configure SAP for **multi-SID**.</span></span> <span data-ttu-id="76079-396">Například můžete přidat další instance SAP ASC nebo SCS do *existující* konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-396">For example, you can add an additional SAP ASCS/SCS instance into an *existing* cluster configuration.</span></span> <span data-ttu-id="76079-397">Další informace najdete v tématu [nakonfigurovat další instance SAP ASC nebo SCS do stávající konfigurace clusteru pro vytvoření konfigurace aplikace SAP více SID ve službě Správce prostředků Azure][sap-ha-multi-sid-guide].</span><span class="sxs-lookup"><span data-stu-id="76079-397">For more information, see [Configure an additional SAP ASCS/SCS instance into an existing cluster configuration to create an SAP multi-SID configuration in Azure Resource Manager][sap-ha-multi-sid-guide].</span></span>

<span data-ttu-id="76079-398">Pokud chcete vytvořit nový cluster více SID, můžete použít více SID [šablony rychlý start na Githubu](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="76079-398">If you want to create a new multi-SID cluster, you can use the multi-SID [quickstart templates on GitHub](https://github.com/Azure/azure-quickstart-templates).</span></span>
<span data-ttu-id="76079-399">K vytvoření nového clusteru více SID, musíte nasadit následující tři šablony:</span><span class="sxs-lookup"><span data-stu-id="76079-399">To create a new multi-SID cluster, you need to deploy the following three templates:</span></span>

* [<span data-ttu-id="76079-400">ASC nebo SCS šablony</span><span class="sxs-lookup"><span data-stu-id="76079-400">ASCS/SCS template</span></span>](#ASCS-SCS-template)
* [<span data-ttu-id="76079-401">Šablona databáze</span><span class="sxs-lookup"><span data-stu-id="76079-401">Database template</span></span>](#database-template)
* [<span data-ttu-id="76079-402">Šablona servery aplikací</span><span class="sxs-lookup"><span data-stu-id="76079-402">Application servers template</span></span>](#application-servers-template)

<span data-ttu-id="76079-403">Následující sekce obsahují další informace o šablonách a parametry, které je třeba zadat v šablonách.</span><span class="sxs-lookup"><span data-stu-id="76079-403">The following sections have more details about the templates and the parameters you need to provide in the templates.</span></span>

#### <span data-ttu-id="76079-404"><a name="ASCS-SCS-template"></a>ASC nebo SCS šablony</span><span class="sxs-lookup"><span data-stu-id="76079-404"><a name="ASCS-SCS-template"></a> ASCS/SCS template</span></span>

<span data-ttu-id="76079-405">Šablona ASC nebo SCS nasadí dva virtuální počítače, které můžete použít k vytvoření clusteru převzetí služeb při selhání systému Windows Server, který je hostitelem více instancí ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="76079-405">The ASCS/SCS template deploys two virtual machines that you can use to create a Windows Server failover cluster that hosts multiple ASCS/SCS instances.</span></span>

<span data-ttu-id="76079-406">Nastavení v šabloně více SID ASC nebo SCS [ASC nebo SCS více SID šablony] [ sap-templates-3-tier-multisid-xscs-marketplace-image] nebo [ASC nebo SCS více SID šablony pomocí disků spravované] [ sap-templates-3-tier-multisid-xscs-marketplace-image-md], zadejte hodnoty následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="76079-406">To set up the ASCS/SCS multi-SID template, in the [ASCS/SCS multi-SID template][sap-templates-3-tier-multisid-xscs-marketplace-image] or [ASCS/SCS multi-SID template using Managed Disks][sap-templates-3-tier-multisid-xscs-marketplace-image-md], enter values for the following parameters:</span></span>

  - <span data-ttu-id="76079-407">**Předpona prostředků**.</span><span class="sxs-lookup"><span data-stu-id="76079-407">**Resource Prefix**.</span></span>  <span data-ttu-id="76079-408">Nastaví předponu prostředku, který se používá k předpony všechny prostředky, které jsou vytvořené během nasazení.</span><span class="sxs-lookup"><span data-stu-id="76079-408">Set the resource prefix, which is used to prefix all resources that are created during the deployment.</span></span> <span data-ttu-id="76079-409">Protože prostředky nepatří do systému jenom jeden SAP, není SID systému SAP jeden předponu prostředku.</span><span class="sxs-lookup"><span data-stu-id="76079-409">Because the resources do not belong to only one SAP system, the prefix of the resource is not the SID of one SAP system.</span></span>  <span data-ttu-id="76079-410">Předpona, která musí být v rozmezí **tři až šest znaků**.</span><span class="sxs-lookup"><span data-stu-id="76079-410">The prefix must be between **three and six characters**.</span></span>
  - <span data-ttu-id="76079-411">**Stack – typ**.</span><span class="sxs-lookup"><span data-stu-id="76079-411">**Stack Type**.</span></span> <span data-ttu-id="76079-412">Vyberte typ zásobníku systému SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-412">Select the stack type of the SAP system.</span></span> <span data-ttu-id="76079-413">V závislosti na typu zásobníku nástroj pro vyrovnávání zatížení Azure má jeden (ABAP nebo Java pouze) nebo dvě (ABAP + Java) privátní IP adresy na systému SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-413">Depending on the stack type, Azure Load Balancer has one (ABAP or Java only) or two (ABAP+Java) private IP addresses per SAP system.</span></span>
  -  <span data-ttu-id="76079-414">**Typ operačního systému**.</span><span class="sxs-lookup"><span data-stu-id="76079-414">**OS Type**.</span></span> <span data-ttu-id="76079-415">Vyberte verzi operačního systému z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="76079-415">Select the operating system of the virtual machines.</span></span>
  -  <span data-ttu-id="76079-416">**Počet systému SAP**.</span><span class="sxs-lookup"><span data-stu-id="76079-416">**SAP System Count**.</span></span> <span data-ttu-id="76079-417">Vyberte počet SAP systémy, které chcete nainstalovat v tomto clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-417">Select the number of SAP systems you want to install in this cluster.</span></span>
  -  <span data-ttu-id="76079-418">**Dostupnost systému**.</span><span class="sxs-lookup"><span data-stu-id="76079-418">**System Availability**.</span></span> <span data-ttu-id="76079-419">Vyberte **HA**.</span><span class="sxs-lookup"><span data-stu-id="76079-419">Select **HA**.</span></span>
  -  <span data-ttu-id="76079-420">**Uživatelské jméno správce a heslo správce**.</span><span class="sxs-lookup"><span data-stu-id="76079-420">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="76079-421">Vytvořte nového uživatele, který lze použít k přihlášení k počítači.</span><span class="sxs-lookup"><span data-stu-id="76079-421">Create a new user that can be used to sign in to the machine.</span></span>
  -  <span data-ttu-id="76079-422">**Nový nebo existující podsíť**.</span><span class="sxs-lookup"><span data-stu-id="76079-422">**New Or Existing Subnet**.</span></span> <span data-ttu-id="76079-423">Nastavte, jestli mají být vytvořeny nové virtuální sítě a podsítě, nebo se použije existující podsítí.</span><span class="sxs-lookup"><span data-stu-id="76079-423">Set whether a new virtual network and subnet should be created, or an existing subnet should be used.</span></span> <span data-ttu-id="76079-424">Pokud již máte virtuální síť, která je připojen k síti na pracovišti, vyberte **existující**.</span><span class="sxs-lookup"><span data-stu-id="76079-424">If you already have a virtual network that is connected to your on-premises network, select **existing**.</span></span>
  -  <span data-ttu-id="76079-425">**Id podsítě**.</span><span class="sxs-lookup"><span data-stu-id="76079-425">**Subnet Id**.</span></span> <span data-ttu-id="76079-426">Nastavte ID podsítě, ke kterému má být připojen virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="76079-426">Set the ID of the subnet to which the virtual machines should be connected.</span></span> <span data-ttu-id="76079-427">Vyberte podsíť virtuální privátní sítě (VPN) nebo ExpressRoute virtuální sítě pro virtuální počítač připojit k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="76079-427">Select the subnet of your virtual private network (VPN) or ExpressRoute virtual network to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="76079-428">ID obvykle vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="76079-428">The ID usually looks like this:</span></span>

   <span data-ttu-id="76079-429">/subscriptions/ <*id předplatného*> /resourceGroups/ <*název skupiny prostředků*> /providers/Microsoft.Network/virtualNetworks/ <*název virtuální sítě*> /subnets/ <*název podsítě.*></span><span class="sxs-lookup"><span data-stu-id="76079-429">/subscriptions/<*subscription id*>/resourceGroups/<*resource group name*>/providers/Microsoft.Network/virtualNetworks/<*virtual network name*>/subnets/<*subnet name*></span></span>

<span data-ttu-id="76079-430">Šablona nasadí jednu instanci nástroj pro vyrovnávání zatížení Azure, která podporuje více systémů SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-430">The template deploys one Azure Load Balancer instance, which supports multiple SAP systems.</span></span>

- <span data-ttu-id="76079-431">Instance ASC jsou nakonfigurované pro instance číslo 00, 10, 20...</span><span class="sxs-lookup"><span data-stu-id="76079-431">The ASCS instances are configured for instance number 00, 10, 20...</span></span>
- <span data-ttu-id="76079-432">Instance SCS jsou nakonfigurované pro instance číslo 01, 11, 21...</span><span class="sxs-lookup"><span data-stu-id="76079-432">The SCS instances are configured for instance number 01, 11, 21...</span></span>
- <span data-ttu-id="76079-433">Instance ASC zařazování replikace serveru (YBRAT) (pouze Linux) jsou nakonfigurované pro čísla instance 02, 12, 22...</span><span class="sxs-lookup"><span data-stu-id="76079-433">The ASCS Enqueue Replication Server (ERS) (Linux only) instances are configured for instance number 02, 12, 22...</span></span>
- <span data-ttu-id="76079-434">Instance SCS YBRAT (pouze Linux) jsou nakonfigurované pro instance číslo 03, 13, 23...</span><span class="sxs-lookup"><span data-stu-id="76079-434">The SCS ERS (Linux only) instances are configured for instance number 03, 13, 23...</span></span>

<span data-ttu-id="76079-435">Nástroje pro vyrovnávání zatížení obsahuje 1 (2 pro Linux) VIP(s), 1 x virtuální IP adresa pro ASC nebo SCS a 1 x virtuální IP adresa pro YBRAT (pouze Linux).</span><span class="sxs-lookup"><span data-stu-id="76079-435">The load balancer contains 1 (2 for Linux) VIP(s), 1x VIP for ASCS/SCS and 1x VIP for ERS (Linux only).</span></span>

<span data-ttu-id="76079-436">Následující seznam obsahuje všechny pravidla (kde x je číslo systému SAP, například 1, 2, 3...) pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="76079-436">The following list contains all load balancing rules (where x is the number of the SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="76079-437">Porty specifické pro systém Windows pro každý systém SAP: 445, 5985</span><span class="sxs-lookup"><span data-stu-id="76079-437">Windows-specific ports for every SAP system: 445, 5985</span></span>
- <span data-ttu-id="76079-438">Porty ASC (čísla instance x0): 32 × 0, 36 x 0, 39 x 0, 81 x 0, 5 x 013, 5 x 014, 5 x 016</span><span class="sxs-lookup"><span data-stu-id="76079-438">ASCS ports (instance number x0): 32x0, 36x0, 39x0, 81x0, 5x013, 5x014, 5x016</span></span>
- <span data-ttu-id="76079-439">Porty SCS (čísla instance x1): 32 x 1, 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116</span><span class="sxs-lookup"><span data-stu-id="76079-439">SCS ports (instance number x1): 32x1, 33x1, 39x1, 81x1, 5x113, 5x114, 5x116</span></span>
- <span data-ttu-id="76079-440">ASC YBRAT portů v systému Linux (čísla instance x2): 33 x 2, 5 x 213, 5 x 214, 5 x 216</span><span class="sxs-lookup"><span data-stu-id="76079-440">ASCS ERS ports on Linux (instance number x2): 33x2, 5x213, 5x214, 5x216</span></span>
- <span data-ttu-id="76079-441">SCS YBRAT portů v systému Linux (čísla instance x3): 33 x 3, 5 x 313, 5 x 314, 5 x 316</span><span class="sxs-lookup"><span data-stu-id="76079-441">SCS ERS ports on Linux (instance number x3): 33x3, 5x313, 5x314, 5x316</span></span>

<span data-ttu-id="76079-442">Nástroje pro vyrovnávání zatížení je nakonfigurovaný na použití následující porty testu (kde x je číslo systému SAP, například 1, 2, 3...):</span><span class="sxs-lookup"><span data-stu-id="76079-442">The load balancer is configured to use the following probe ports (where x is the number of the SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="76079-443">Port testu nástroje pro vyrovnávání zatížení ASC nebo SCS interní: 620 x 0</span><span class="sxs-lookup"><span data-stu-id="76079-443">ASCS/SCS internal load balancer probe port: 620x0</span></span>
- <span data-ttu-id="76079-444">Interní YBRAT načíst port testu vyrovnávání (pouze Linux): 621 x 2</span><span class="sxs-lookup"><span data-stu-id="76079-444">ERS internal load balancer probe port (Linux only): 621x2</span></span>

#### <span data-ttu-id="76079-445"><a name="database-template"></a>Šablona databáze</span><span class="sxs-lookup"><span data-stu-id="76079-445"><a name="database-template"></a> Database template</span></span>

<span data-ttu-id="76079-446">Šablona databáze nasadí jednu nebo dvě virtuální počítače, které můžete použít k instalaci systému správy relačních databází (RDBMS) pro systém SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-446">The database template deploys one or two virtual machines that you can use to install the relational database management system (RDBMS) for one SAP system.</span></span> <span data-ttu-id="76079-447">Například pokud nasadíte šablonu ASC nebo SCS pro pět systémy SAP, budete muset nasadit pětkrát této šablony.</span><span class="sxs-lookup"><span data-stu-id="76079-447">For example, if you deploy an ASCS/SCS template for five SAP systems, you need to deploy this template five times.</span></span>

<span data-ttu-id="76079-448">Nastavení v šabloně více SID databáze [databáze více SID šablony] [ sap-templates-3-tier-multisid-db-marketplace-image] nebo [databáze více SID šablony pomocí disků spravované] [ sap-templates-3-tier-multisid-db-marketplace-image-md], zadejte hodnoty následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="76079-448">To set up the database multi-SID template, in the [database multi-SID template][sap-templates-3-tier-multisid-db-marketplace-image] or [database multi-SID template using Managed Disks][sap-templates-3-tier-multisid-db-marketplace-image-md], enter values for the following parameters:</span></span>

  -  <span data-ttu-id="76079-449">**Id systému SAP**.</span><span class="sxs-lookup"><span data-stu-id="76079-449">**Sap System Id**.</span></span> <span data-ttu-id="76079-450">Zadejte ID systému SAP systému SAP, který chcete nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="76079-450">Enter the SAP system ID of the SAP system you want to install.</span></span> <span data-ttu-id="76079-451">Identifikátor se použije jako předpona pro prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="76079-451">The ID will be used as a prefix for the resources that are deployed.</span></span>
  -  <span data-ttu-id="76079-452">**Typ operačního systému**.</span><span class="sxs-lookup"><span data-stu-id="76079-452">**Os Type**.</span></span> <span data-ttu-id="76079-453">Vyberte verzi operačního systému z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="76079-453">Select the operating system of the virtual machines.</span></span>
  -  <span data-ttu-id="76079-454">**Hodnota DbType**.</span><span class="sxs-lookup"><span data-stu-id="76079-454">**Dbtype**.</span></span> <span data-ttu-id="76079-455">Vyberte typ databáze, kterou chcete nainstalovat na clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-455">Select the type of the database you want to install on the cluster.</span></span> <span data-ttu-id="76079-456">Vyberte **SQL** Pokud chcete nainstalovat Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="76079-456">Select **SQL** if you want to install Microsoft SQL Server.</span></span> <span data-ttu-id="76079-457">Vyberte **HANA** Pokud budete chtít na virtuální počítače nainstalovat SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="76079-457">Select **HANA** if you plan to install SAP HANA on the virtual machines.</span></span> <span data-ttu-id="76079-458">Je nutné vybrat typ správný operační systém: vyberte **Windows** pro SQL a vyberte distribuční Linux pro HANA.</span><span class="sxs-lookup"><span data-stu-id="76079-458">Make sure to select the correct operating system type: select **Windows** for SQL, and select a Linux distribution for HANA.</span></span> <span data-ttu-id="76079-459">Vyrovnávání zatížení Azure, která je připojena k virtuálním počítačům, bude nakonfigurován pro podporu typ vybrané databáze:</span><span class="sxs-lookup"><span data-stu-id="76079-459">The Azure Load Balancer that is connected to the virtual machines will be configured to support the selected database type:</span></span>
    * <span data-ttu-id="76079-460">**SQL**.</span><span class="sxs-lookup"><span data-stu-id="76079-460">**SQL**.</span></span> <span data-ttu-id="76079-461">Nástroje pro vyrovnávání zatížení bude Vyrovnávání zatížení port 1433.</span><span class="sxs-lookup"><span data-stu-id="76079-461">The load balancer will load-balance port 1433.</span></span> <span data-ttu-id="76079-462">Ujistěte se, že tento port použít pro vaše instalační program SQL serveru Always On.</span><span class="sxs-lookup"><span data-stu-id="76079-462">Make sure to use this port for your SQL Server Always On setup.</span></span>
    * <span data-ttu-id="76079-463">**HANA**.</span><span class="sxs-lookup"><span data-stu-id="76079-463">**HANA**.</span></span> <span data-ttu-id="76079-464">Nástroje pro vyrovnávání zatížení bude porty 35015 a 35017 Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="76079-464">The load balancer will load-balance ports 35015 and 35017.</span></span> <span data-ttu-id="76079-465">Nainstalujte SAP HANA číslem instance **50**.</span><span class="sxs-lookup"><span data-stu-id="76079-465">Make sure to install SAP HANA with instance number **50**.</span></span>
    <span data-ttu-id="76079-466">Nástroje pro vyrovnávání zatížení bude používat port testu 62550.</span><span class="sxs-lookup"><span data-stu-id="76079-466">The load balancer will use probe port 62550.</span></span>
  -  <span data-ttu-id="76079-467">**Velikost systému SAP**.</span><span class="sxs-lookup"><span data-stu-id="76079-467">**Sap System Size**.</span></span> <span data-ttu-id="76079-468">Nastavte počet protokoly SAP bude poskytovat nový systém.</span><span class="sxs-lookup"><span data-stu-id="76079-468">Set the number of SAPS the new system will provide.</span></span> <span data-ttu-id="76079-469">Pokud si nejste jisti kolik protokoly SAP, systém bude vyžadovat, požádejte SAP technologie partnera nebo systémový integrátor.</span><span class="sxs-lookup"><span data-stu-id="76079-469">If you are not sure how many SAPS the system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="76079-470">**Dostupnost systému**.</span><span class="sxs-lookup"><span data-stu-id="76079-470">**System Availability**.</span></span> <span data-ttu-id="76079-471">Vyberte **HA**.</span><span class="sxs-lookup"><span data-stu-id="76079-471">Select **HA**.</span></span>
  -  <span data-ttu-id="76079-472">**Uživatelské jméno správce a heslo správce**.</span><span class="sxs-lookup"><span data-stu-id="76079-472">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="76079-473">Vytvořte nového uživatele, který lze použít k přihlášení k počítači.</span><span class="sxs-lookup"><span data-stu-id="76079-473">Create a new user that can be used to sign in to the machine.</span></span>
  -  <span data-ttu-id="76079-474">**Id podsítě**.</span><span class="sxs-lookup"><span data-stu-id="76079-474">**Subnet Id**.</span></span> <span data-ttu-id="76079-475">Zadejte ID podsítě, která jste použili při nasazení ASC nebo SCS šablony nebo ID podsítě, která byla vytvořena jako součást nasazení ASC nebo SCS šablony.</span><span class="sxs-lookup"><span data-stu-id="76079-475">Enter the ID of the subnet that you used during the deployment of the ASCS/SCS template, or the ID of the subnet that was created as part of the ASCS/SCS template deployment.</span></span>

#### <span data-ttu-id="76079-476"><a name="application-servers-template"></a>Šablona servery aplikací</span><span class="sxs-lookup"><span data-stu-id="76079-476"><a name="application-servers-template"></a> Application servers template</span></span>

<span data-ttu-id="76079-477">Šablony servery aplikace nasadí dvě nebo více virtuálních počítačů, které lze použít jako instance aplikačního serveru SAP jeden systému SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-477">The application servers template deploys two or more virtual machines that can be used as SAP Application Server instances for one SAP system.</span></span> <span data-ttu-id="76079-478">Například pokud nasadíte šablonu ASC nebo SCS pro pět systémy SAP, budete muset nasadit pětkrát této šablony.</span><span class="sxs-lookup"><span data-stu-id="76079-478">For example, if you deploy an ASCS/SCS template for five SAP systems, you need to deploy this template five times.</span></span>

<span data-ttu-id="76079-479">Nastavení v šabloně SID více serverů aplikace [šablony SID více serverů aplikace] [ sap-templates-3-tier-multisid-apps-marketplace-image] nebo [šablony SID více serverů aplikace pomocí spravovaných disků] [ sap-templates-3-tier-multisid-apps-marketplace-image-md], zadejte hodnoty následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="76079-479">To set up the application servers multi-SID template, in the [application servers multi-SID template][sap-templates-3-tier-multisid-apps-marketplace-image] or [application servers multi-SID template using Managed Disks][sap-templates-3-tier-multisid-apps-marketplace-image-md], enter values for the following parameters:</span></span>

  -  <span data-ttu-id="76079-480">**Id systému SAP**.</span><span class="sxs-lookup"><span data-stu-id="76079-480">**Sap System Id**.</span></span> <span data-ttu-id="76079-481">Zadejte ID systému SAP systému SAP, který chcete nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="76079-481">Enter the SAP system ID of the SAP system you want to install.</span></span> <span data-ttu-id="76079-482">Identifikátor se použije jako předpona pro prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="76079-482">The ID will be used as a prefix for the resources that are deployed.</span></span>
  -  <span data-ttu-id="76079-483">**Typ operačního systému**.</span><span class="sxs-lookup"><span data-stu-id="76079-483">**Os Type**.</span></span> <span data-ttu-id="76079-484">Vyberte verzi operačního systému z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="76079-484">Select the operating system of the virtual machines.</span></span>
  -  <span data-ttu-id="76079-485">**Velikost systému SAP**.</span><span class="sxs-lookup"><span data-stu-id="76079-485">**Sap System Size**.</span></span> <span data-ttu-id="76079-486">Počet je nový systém bude poskytovat protokoly SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-486">The number of SAPS the new system will provide.</span></span> <span data-ttu-id="76079-487">Pokud si nejste jisti kolik protokoly SAP, systém bude vyžadovat, požádejte SAP technologie partnera nebo systémový integrátor.</span><span class="sxs-lookup"><span data-stu-id="76079-487">If you are not sure how many SAPS the system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="76079-488">**Dostupnost systému**.</span><span class="sxs-lookup"><span data-stu-id="76079-488">**System Availability**.</span></span> <span data-ttu-id="76079-489">Vyberte **HA**.</span><span class="sxs-lookup"><span data-stu-id="76079-489">Select **HA**.</span></span>
  -  <span data-ttu-id="76079-490">**Uživatelské jméno správce a heslo správce**.</span><span class="sxs-lookup"><span data-stu-id="76079-490">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="76079-491">Vytvořte nového uživatele, který lze použít k přihlášení k počítači.</span><span class="sxs-lookup"><span data-stu-id="76079-491">Create a new user that can be used to sign in to the machine.</span></span>
  -  <span data-ttu-id="76079-492">**Id podsítě**.</span><span class="sxs-lookup"><span data-stu-id="76079-492">**Subnet Id**.</span></span> <span data-ttu-id="76079-493">Zadejte ID podsítě, která jste použili při nasazení ASC nebo SCS šablony nebo ID podsítě, která byla vytvořena jako součást nasazení ASC nebo SCS šablony.</span><span class="sxs-lookup"><span data-stu-id="76079-493">Enter the ID of the subnet that you used during the deployment of the ASCS/SCS template, or the ID of the subnet that was created as part of the ASCS/SCS template deployment.</span></span>


### <span data-ttu-id="76079-494"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="76079-494"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure virtual network</span></span>
<span data-ttu-id="76079-495">V našem příkladu adresní prostor virtuální sítě Azure je 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="76079-495">In our example, the address space of the Azure virtual network is 10.0.0.0/16.</span></span> <span data-ttu-id="76079-496">Existuje jedna podsíť s názvem **podsíť**, rozsah adres 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="76079-496">There is one subnet called **Subnet**, with an address range of 10.0.0.0/24.</span></span> <span data-ttu-id="76079-497">Všechny virtuální počítače a interní služby load balancer nasazených v této virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="76079-497">All virtual machines and internal load balancers are deployed in this virtual network.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="76079-498">Nemáte žádné změny nastavení sítě v hostovaném operačním systému.</span><span class="sxs-lookup"><span data-stu-id="76079-498">Don't make any changes to the network settings inside the guest operating system.</span></span> <span data-ttu-id="76079-499">To zahrnuje IP adresy, servery DNS a podsítě.</span><span class="sxs-lookup"><span data-stu-id="76079-499">This includes IP addresses, DNS servers, and subnet.</span></span> <span data-ttu-id="76079-500">Nakonfigurujte všechna nastavení sítě v Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-500">Configure all your network settings in Azure.</span></span> <span data-ttu-id="76079-501">Službu Dynamic Host Configuration Protocol (DHCP) rozšíří nastavení.</span><span class="sxs-lookup"><span data-stu-id="76079-501">The Dynamic Host Configuration Protocol (DHCP) service propagates your settings.</span></span>
>
>

### <span data-ttu-id="76079-502"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS IP adresy</span><span class="sxs-lookup"><span data-stu-id="76079-502"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS IP addresses</span></span>

<span data-ttu-id="76079-503">Pokud chcete nastavit požadované DNS IP adresy, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="76079-503">To set the required DNS IP addresses, do the following steps.</span></span>

1.  <span data-ttu-id="76079-504">Na portálu Azure na **servery DNS** okno, ujistěte se, že virtuální sítě **servery DNS** je možnost nastavena na **vlastní DNS**.</span><span class="sxs-lookup"><span data-stu-id="76079-504">In the Azure portal, on the **DNS servers** blade, make sure that your virtual network **DNS servers** option is set to **Custom DNS**.</span></span>
2.  <span data-ttu-id="76079-505">Vyberte nastavení na základě typu sítě, které máte.</span><span class="sxs-lookup"><span data-stu-id="76079-505">Select your settings based on the type of network you have.</span></span> <span data-ttu-id="76079-506">Další informace najdete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="76079-506">For more information, see the following resources:</span></span>
    * <span data-ttu-id="76079-507">[Připojení k podnikové síti (mezi různými místy)][planning-guide-2.2]: Přidejte IP adresy na místní servery DNS.</span><span class="sxs-lookup"><span data-stu-id="76079-507">[Corporate network connectivity (cross-premises)][planning-guide-2.2]: Add the IP addresses of the on-premises DNS servers.</span></span>  
    <span data-ttu-id="76079-508">Můžete rozšířit na místní servery DNS pro virtuální počítače, které jsou spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-508">You can extend on-premises DNS servers to the virtual machines that are running in Azure.</span></span> <span data-ttu-id="76079-509">V tomto scénáři můžete přidat IP adresy Azure virtuální počítače, na které spouštíte služby DNS.</span><span class="sxs-lookup"><span data-stu-id="76079-509">In that scenario, you can add the IP addresses of the Azure virtual machines on which you run the DNS service.</span></span>
    * <span data-ttu-id="76079-510">[Čistě cloudové nasazení][planning-guide-2.1]: nasazení se další virtuální počítač ve stejné instanci virtuální sítě, která slouží jako DNS server.</span><span class="sxs-lookup"><span data-stu-id="76079-510">[Cloud-only deployment][planning-guide-2.1]: Deploy an additional virtual machine in the same Virtual Network instance that serves as a DNS server.</span></span> <span data-ttu-id="76079-511">Přidání IP adres virtuální počítače Azure, které jste nastavili ke spouštění služby DNS.</span><span class="sxs-lookup"><span data-stu-id="76079-511">Add the IP addresses of the Azure virtual machines that you've set up to run DNS service.</span></span>

    ![Obrázek 12: Servery DNS nakonfigurujte pro virtuální síť Azure][sap-ha-guide-figure-3001]

    <span data-ttu-id="76079-513">_**Obrázek 12:** konfigurace DNS serverů pro virtuální síť Azure_</span><span class="sxs-lookup"><span data-stu-id="76079-513">_**Figure 12:** Configure DNS servers for Azure Virtual Network_</span></span>

  > [!NOTE]
  > <span data-ttu-id="76079-514">Pokud změníte IP adresy serverů DNS, musíte restartovat virtuální počítače Azure k použití změny a šířit nové servery DNS.</span><span class="sxs-lookup"><span data-stu-id="76079-514">If you change the IP addresses of the DNS servers, you need to restart the Azure virtual machines to apply the change and propagate the new DNS servers.</span></span>
  >
  >

<span data-ttu-id="76079-515">V našem příkladu služba DNS nainstalovaná a nakonfigurovaná na těchto virtuálních počítačích Windows:</span><span class="sxs-lookup"><span data-stu-id="76079-515">In our example, the DNS service is installed and configured on these Windows virtual machines:</span></span>

| <span data-ttu-id="76079-516">Role virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="76079-516">Virtual machine role</span></span> | <span data-ttu-id="76079-517">Název hostitele virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="76079-517">Virtual machine host name</span></span> | <span data-ttu-id="76079-518">Název síťové karty</span><span class="sxs-lookup"><span data-stu-id="76079-518">Network card name</span></span> | <span data-ttu-id="76079-519">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="76079-519">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="76079-520">První server DNS</span><span class="sxs-lookup"><span data-stu-id="76079-520">First DNS server</span></span> |<span data-ttu-id="76079-521">domcontr-0</span><span class="sxs-lookup"><span data-stu-id="76079-521">domcontr-0</span></span> |<span data-ttu-id="76079-522">PR1-seskupování domcontr-0</span><span class="sxs-lookup"><span data-stu-id="76079-522">pr1-nic-domcontr-0</span></span> |<span data-ttu-id="76079-523">10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="76079-523">10.0.0.10</span></span> |
| <span data-ttu-id="76079-524">Druhý server DNS</span><span class="sxs-lookup"><span data-stu-id="76079-524">Second DNS server</span></span> |<span data-ttu-id="76079-525">domcontr-1</span><span class="sxs-lookup"><span data-stu-id="76079-525">domcontr-1</span></span> |<span data-ttu-id="76079-526">PR1-seskupování domcontr-1</span><span class="sxs-lookup"><span data-stu-id="76079-526">pr1-nic-domcontr-1</span></span> |<span data-ttu-id="76079-527">10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="76079-527">10.0.0.11</span></span> |

### <span data-ttu-id="76079-528"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Názvy hostitelů a statické IP adresy pro clusterové instance SAP ASC nebo SCS a Clusterové instance databázového systému</span><span class="sxs-lookup"><span data-stu-id="76079-528"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Host names and static IP addresses for the SAP ASCS/SCS clustered instance and DBMS clustered instance</span></span>

<span data-ttu-id="76079-529">Pro místní nasazení je třeba tyto názvy vyhrazené hostitele a IP adresy:</span><span class="sxs-lookup"><span data-stu-id="76079-529">For on-premises deployment, you need these reserved host names and IP addresses:</span></span>

| <span data-ttu-id="76079-530">Název role virtuálních hostitelů</span><span class="sxs-lookup"><span data-stu-id="76079-530">Virtual host name role</span></span> | <span data-ttu-id="76079-531">Název virtuálního hostitele</span><span class="sxs-lookup"><span data-stu-id="76079-531">Virtual host name</span></span> | <span data-ttu-id="76079-532">Virtuální statickou IP adresu</span><span class="sxs-lookup"><span data-stu-id="76079-532">Virtual static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="76079-533">SAP ASC nebo SCS první clusteru virtuální hostitel název (pro správu clusteru)</span><span class="sxs-lookup"><span data-stu-id="76079-533">SAP ASCS/SCS first cluster virtual host name (for cluster management)</span></span> |<span data-ttu-id="76079-534">PR1. ASC vir</span><span class="sxs-lookup"><span data-stu-id="76079-534">pr1-ascs-vir</span></span> |<span data-ttu-id="76079-535">10.0.0.42</span><span class="sxs-lookup"><span data-stu-id="76079-535">10.0.0.42</span></span> |
| <span data-ttu-id="76079-536">Název virtuálního hostitele instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="76079-536">SAP ASCS/SCS instance virtual host name</span></span> |<span data-ttu-id="76079-537">PR1. ASC sap</span><span class="sxs-lookup"><span data-stu-id="76079-537">pr1-ascs-sap</span></span> |<span data-ttu-id="76079-538">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="76079-538">10.0.0.43</span></span> |
| <span data-ttu-id="76079-539">Databázového systému SAP druhý cluster virtuálního hostitele název (Správa clusteru)</span><span class="sxs-lookup"><span data-stu-id="76079-539">SAP DBMS second cluster virtual host name (cluster management)</span></span> |<span data-ttu-id="76079-540">PR1. databázového systému vir</span><span class="sxs-lookup"><span data-stu-id="76079-540">pr1-dbms-vir</span></span> |<span data-ttu-id="76079-541">10.0.0.32</span><span class="sxs-lookup"><span data-stu-id="76079-541">10.0.0.32</span></span> |

<span data-ttu-id="76079-542">Při vytváření clusteru, vytvořit názvy virtuálních hostitelů **pr1. ASC vir** a **pr1. databázového systému vir** a přidružené IP adresy, které spravují samotného clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-542">When you create the cluster, create the virtual host names **pr1-ascs-vir** and **pr1-dbms-vir** and the associated IP addresses that manage the cluster itself.</span></span> <span data-ttu-id="76079-543">Informace o tom, jak to udělat najdete v tématu [shromažďovat uzly clusteru v konfiguraci clusteru][sap-ha-guide-8.12.1].</span><span class="sxs-lookup"><span data-stu-id="76079-543">For information about how to do this, see [Collect cluster nodes in a cluster configuration][sap-ha-guide-8.12.1].</span></span>

<span data-ttu-id="76079-544">Můžete ručně vytvořit další dva názvy virtuálních hostitelů, **pr1. ASC sap** a **pr1. databázového systému sap**a přidružené IP adresy na serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="76079-544">You can manually create the other two virtual host names, **pr1-ascs-sap** and **pr1-dbms-sap**, and the associated IP addresses, on the DNS server.</span></span> <span data-ttu-id="76079-545">SAP ASC nebo SCS skupinu prostředků clusteru a skupinu prostředků clusteru databázového systému používají tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="76079-545">The clustered SAP ASCS/SCS instance and the clustered DBMS instance use these resources.</span></span> <span data-ttu-id="76079-546">Informace o tom, jak to udělat najdete v tématu [vytvořte název virtuálního hostitele pro clusterovou instanci SAP ASC nebo SCS][sap-ha-guide-9.1.1].</span><span class="sxs-lookup"><span data-stu-id="76079-546">For information about how to do this, see [Create a virtual host name for a clustered SAP ASCS/SCS instance][sap-ha-guide-9.1.1].</span></span>

### <span data-ttu-id="76079-547"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Nastavení statické IP adresy pro virtuální počítače SAP</span><span class="sxs-lookup"><span data-stu-id="76079-547"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> Set static IP addresses for the SAP virtual machines</span></span>
<span data-ttu-id="76079-548">Poté, co nasadíte virtuální počítače pro použití v clusteru, musíte nastavit statické IP adresy pro všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="76079-548">After you deploy the virtual machines to use in your cluster, you need to set static IP addresses for all virtual machines.</span></span> <span data-ttu-id="76079-549">To udělat v konfiguraci virtuální sítě Azure a není v hostovaném operačním systému.</span><span class="sxs-lookup"><span data-stu-id="76079-549">Do this in the Azure Virtual Network configuration, and not in the guest operating system.</span></span>

1.  <span data-ttu-id="76079-550">Na portálu Azure vyberte **skupiny prostředků** > **síťové karty** > **nastavení** > **IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="76079-550">In the Azure portal, select **Resource Group** > **Network Card** > **Settings** > **IP Address**.</span></span>
2.  <span data-ttu-id="76079-551">Na **IP adresy** okno, v části **přiřazení**, vyberte **statické**.</span><span class="sxs-lookup"><span data-stu-id="76079-551">On the **IP addresses** blade, under **Assignment**, select **Static**.</span></span> <span data-ttu-id="76079-552">V **IP adresu** zadejte IP adresu, která chcete použít.</span><span class="sxs-lookup"><span data-stu-id="76079-552">In the **IP address** box, enter the IP address that you want to use.</span></span>

  > [!NOTE]
  > <span data-ttu-id="76079-553">Pokud změníte IP adresa síťové karty, budete muset restartovat virtuální počítače Azure na použití změny.</span><span class="sxs-lookup"><span data-stu-id="76079-553">If you change the IP address of the network card, you need to restart the Azure virtual machines to apply the change.</span></span>  
  >
  >

  ![Obrázek 13: Nastavení statické IP adresy pro síťové karty každého virtuálního počítače][sap-ha-guide-figure-3002]

  <span data-ttu-id="76079-555">_**Obrázek 13:** nastavení statické IP adresy pro síťové karty každého virtuálního počítače_</span><span class="sxs-lookup"><span data-stu-id="76079-555">_**Figure 13:** Set static IP addresses for the network card of each virtual machine_</span></span>

  <span data-ttu-id="76079-556">Tento krok opakujte pro všechna síťová rozhraní, které se pro všechny virtuální počítače, včetně virtuálních počítačů, které chcete použít pro vaši službu Active Directory a DNS.</span><span class="sxs-lookup"><span data-stu-id="76079-556">Repeat this step for all network interfaces, that is, for all virtual machines, including virtual machines that you want to use for your Active Directory/DNS service.</span></span>

<span data-ttu-id="76079-557">V našem příkladu máme tyto virtuální počítače a statické IP adresy:</span><span class="sxs-lookup"><span data-stu-id="76079-557">In our example, we have these virtual machines and static IP addresses:</span></span>

| <span data-ttu-id="76079-558">Role virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="76079-558">Virtual machine role</span></span> | <span data-ttu-id="76079-559">Název hostitele virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="76079-559">Virtual machine host name</span></span> | <span data-ttu-id="76079-560">Název síťové karty</span><span class="sxs-lookup"><span data-stu-id="76079-560">Network card name</span></span> | <span data-ttu-id="76079-561">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="76079-561">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="76079-562">První instance aplikačního serveru SAP</span><span class="sxs-lookup"><span data-stu-id="76079-562">First SAP Application Server instance</span></span> |<span data-ttu-id="76079-563">PR1-di-0</span><span class="sxs-lookup"><span data-stu-id="76079-563">pr1-di-0</span></span> |<span data-ttu-id="76079-564">PR1-seskupování di-0</span><span class="sxs-lookup"><span data-stu-id="76079-564">pr1-nic-di-0</span></span> |<span data-ttu-id="76079-565">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="76079-565">10.0.0.50</span></span> |
| <span data-ttu-id="76079-566">Druhá instance aplikačního serveru SAP</span><span class="sxs-lookup"><span data-stu-id="76079-566">Second SAP Application Server instance</span></span> |<span data-ttu-id="76079-567">PR1-di-1</span><span class="sxs-lookup"><span data-stu-id="76079-567">pr1-di-1</span></span> |<span data-ttu-id="76079-568">PR1-seskupování di-1</span><span class="sxs-lookup"><span data-stu-id="76079-568">pr1-nic-di-1</span></span> |<span data-ttu-id="76079-569">10.0.0.51</span><span class="sxs-lookup"><span data-stu-id="76079-569">10.0.0.51</span></span> |
| <span data-ttu-id="76079-570">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="76079-570">...</span></span> |<span data-ttu-id="76079-571">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="76079-571">...</span></span> |<span data-ttu-id="76079-572">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="76079-572">...</span></span> |<span data-ttu-id="76079-573">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="76079-573">...</span></span> |
| <span data-ttu-id="76079-574">Poslední instance aplikačního serveru SAP</span><span class="sxs-lookup"><span data-stu-id="76079-574">Last SAP Application Server instance</span></span> |<span data-ttu-id="76079-575">PR1-di-5</span><span class="sxs-lookup"><span data-stu-id="76079-575">pr1-di-5</span></span> |<span data-ttu-id="76079-576">PR1 seskupování di 5</span><span class="sxs-lookup"><span data-stu-id="76079-576">pr1-nic-di-5</span></span> |<span data-ttu-id="76079-577">10.0.0.55</span><span class="sxs-lookup"><span data-stu-id="76079-577">10.0.0.55</span></span> |
| <span data-ttu-id="76079-578">Prvním uzlu clusteru pro instanci ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="76079-578">First cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="76079-579">PR1-ASC-0</span><span class="sxs-lookup"><span data-stu-id="76079-579">pr1-ascs-0</span></span> |<span data-ttu-id="76079-580">PR1-seskupování ASC-0</span><span class="sxs-lookup"><span data-stu-id="76079-580">pr1-nic-ascs-0</span></span> |<span data-ttu-id="76079-581">10.0.0.40</span><span class="sxs-lookup"><span data-stu-id="76079-581">10.0.0.40</span></span> |
| <span data-ttu-id="76079-582">Druhý uzel clusteru pro instanci ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="76079-582">Second cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="76079-583">PR1-ASC-1</span><span class="sxs-lookup"><span data-stu-id="76079-583">pr1-ascs-1</span></span> |<span data-ttu-id="76079-584">PR1-seskupování ASC-1</span><span class="sxs-lookup"><span data-stu-id="76079-584">pr1-nic-ascs-1</span></span> |<span data-ttu-id="76079-585">10.0.0.41</span><span class="sxs-lookup"><span data-stu-id="76079-585">10.0.0.41</span></span> |
| <span data-ttu-id="76079-586">Prvním uzlu clusteru pro instanci databázového systému</span><span class="sxs-lookup"><span data-stu-id="76079-586">First cluster node for DBMS instance</span></span> |<span data-ttu-id="76079-587">PR1-db-0</span><span class="sxs-lookup"><span data-stu-id="76079-587">pr1-db-0</span></span> |<span data-ttu-id="76079-588">PR1-seskupování db-0</span><span class="sxs-lookup"><span data-stu-id="76079-588">pr1-nic-db-0</span></span> |<span data-ttu-id="76079-589">10.0.0.30</span><span class="sxs-lookup"><span data-stu-id="76079-589">10.0.0.30</span></span> |
| <span data-ttu-id="76079-590">Druhý uzel clusteru pro instanci databázového systému</span><span class="sxs-lookup"><span data-stu-id="76079-590">Second cluster node for DBMS instance</span></span> |<span data-ttu-id="76079-591">PR1-db-1</span><span class="sxs-lookup"><span data-stu-id="76079-591">pr1-db-1</span></span> |<span data-ttu-id="76079-592">PR1-seskupování db-1</span><span class="sxs-lookup"><span data-stu-id="76079-592">pr1-nic-db-1</span></span> |<span data-ttu-id="76079-593">10.0.0.31</span><span class="sxs-lookup"><span data-stu-id="76079-593">10.0.0.31</span></span> |

### <span data-ttu-id="76079-594"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Nastavit statickou IP adresu pro nástroj pro vyrovnávání zatížení Azure interní</span><span class="sxs-lookup"><span data-stu-id="76079-594"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Set a static IP address for the Azure internal load balancer</span></span>

<span data-ttu-id="76079-595">Šablona SAP Azure Resource Manager vytvoří pro vyrovnávání zatížení Azure interní, který se používá pro SAP ASC nebo SCS instance clusteru a clusteru databázového systému.</span><span class="sxs-lookup"><span data-stu-id="76079-595">The SAP Azure Resource Manager template creates an Azure internal load balancer that is used for the SAP ASCS/SCS instance cluster and the DBMS cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="76079-596">IP adresa název virtuálního hostitele SAP ASC nebo SCS je stejný jako IP adresu služby Vyrovnávání zatížení interní SAP ASC nebo SCS: **pr1-lb Asc**.</span><span class="sxs-lookup"><span data-stu-id="76079-596">The IP address of the virtual host name of the SAP ASCS/SCS is the same as the IP address of the SAP ASCS/SCS internal load balancer: **pr1-lb-ascs**.</span></span>
> <span data-ttu-id="76079-597">IP adresa virtuální název tohoto databázového systému je stejné jako IP adresa služby Vyrovnávání zatížení interní databázového systému: **databázového systému pr1 lb**.</span><span class="sxs-lookup"><span data-stu-id="76079-597">The IP address of the virtual name of the DBMS is the same as the IP address of the DBMS internal load balancer: **pr1-lb-dbms**.</span></span>
>
>

<span data-ttu-id="76079-598">Chcete-li nastavit statickou IP adresu pro nástroj pro vyrovnávání zatížení Azure interní:</span><span class="sxs-lookup"><span data-stu-id="76079-598">To set a static IP address for the Azure internal load balancer:</span></span>

1.  <span data-ttu-id="76079-599">Počáteční nasazení nastaví IP adresa služby Vyrovnávání zatížení pro vnitřní **dynamické**.</span><span class="sxs-lookup"><span data-stu-id="76079-599">The initial deployment sets the internal load balancer IP address to **Dynamic**.</span></span> <span data-ttu-id="76079-600">Na portálu Azure na **IP adresy** okno, v části **přiřazení**, vyberte **statické**.</span><span class="sxs-lookup"><span data-stu-id="76079-600">In the Azure portal, on the **IP addresses** blade, under **Assignment**, select **Static**.</span></span>
2.  <span data-ttu-id="76079-601">Nastavení IP adresy služby Vyrovnávání zatížení pro vnitřní **pr1-lb Asc** na název virtuálního hostitele instance SAP ASC nebo SCS adresu IP.</span><span class="sxs-lookup"><span data-stu-id="76079-601">Set the IP address of the internal load balancer **pr1-lb-ascs** to the IP address of the virtual host name of the SAP ASCS/SCS instance.</span></span>
3.  <span data-ttu-id="76079-602">Nastavení IP adresy služby Vyrovnávání zatížení pro vnitřní **databázového systému pr1 lb** na IP adresu název virtuálního hostitele instance databázového systému.</span><span class="sxs-lookup"><span data-stu-id="76079-602">Set the IP address of the internal load balancer **pr1-lb-dbms** to the IP address of the virtual host name of the DBMS instance.</span></span>

  ![Obrázek 14: Nastavení statické IP adresy pro vyrovnávání zatížení pro vnitřní pro instance SAP ASC nebo SCS][sap-ha-guide-figure-3003]

  <span data-ttu-id="76079-604">_**Obrázek 14:** nastavení statické IP adresy pro vyrovnávání zatížení pro vnitřní pro instance SAP ASC nebo SCS_</span><span class="sxs-lookup"><span data-stu-id="76079-604">_**Figure 14:** Set static IP addresses for the internal load balancer for the SAP ASCS/SCS instance_</span></span>

<span data-ttu-id="76079-605">V našem příkladu máme dvě Azure interní služby load balancer, které mají tyto statické IP adresy:</span><span class="sxs-lookup"><span data-stu-id="76079-605">In our example, we have two Azure internal load balancers that have these static IP addresses:</span></span>

| <span data-ttu-id="76079-606">Role služby Vyrovnávání zatížení Azure interní</span><span class="sxs-lookup"><span data-stu-id="76079-606">Azure internal load balancer role</span></span> | <span data-ttu-id="76079-607">Název nástroje pro vyrovnávání zatížení Azure interní</span><span class="sxs-lookup"><span data-stu-id="76079-607">Azure internal load balancer name</span></span> | <span data-ttu-id="76079-608">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="76079-608">Static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="76079-609">SAP ASC nebo SCS instanci interní zátěže.</span><span class="sxs-lookup"><span data-stu-id="76079-609">SAP ASCS/SCS instance internal load balancer</span></span> |<span data-ttu-id="76079-610">PR1-lb ASC</span><span class="sxs-lookup"><span data-stu-id="76079-610">pr1-lb-ascs</span></span> |<span data-ttu-id="76079-611">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="76079-611">10.0.0.43</span></span> |
| <span data-ttu-id="76079-612">Databázového systému SAP interní nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="76079-612">SAP DBMS internal load balancer</span></span> |<span data-ttu-id="76079-613">PR1-lb-databázového systému</span><span class="sxs-lookup"><span data-stu-id="76079-613">pr1-lb-dbms</span></span> |<span data-ttu-id="76079-614">10.0.0.33</span><span class="sxs-lookup"><span data-stu-id="76079-614">10.0.0.33</span></span> |


### <span data-ttu-id="76079-615"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Výchozí pravidla pro vyrovnávání zatížení Azure interní Vyrovnávání zatížení ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="76079-615"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Default ASCS/SCS load balancing rules for the Azure internal load balancer</span></span>

<span data-ttu-id="76079-616">Šablona SAP Azure Resource Manager vytvoří porty, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="76079-616">The SAP Azure Resource Manager template creates the ports you need:</span></span>
* <span data-ttu-id="76079-617">Instance ABAP ASC pomocí výchozí instance číslo **00**</span><span class="sxs-lookup"><span data-stu-id="76079-617">An ABAP ASCS instance, with the default instance number **00**</span></span>
* <span data-ttu-id="76079-618">Instance Java SCS, s výchozí instance číslo **01**</span><span class="sxs-lookup"><span data-stu-id="76079-618">A Java SCS instance, with the default instance number **01**</span></span>

<span data-ttu-id="76079-619">Při instalaci instance SAP ASC nebo SCS, musíte použít výchozí instanci číslo **00** pro vaše ABAP ASC instance a číslo instance výchozí **01** instance Java SCS.</span><span class="sxs-lookup"><span data-stu-id="76079-619">When you install your SAP ASCS/SCS instance, you must use the default instance number **00** for your ABAP ASCS instance and the default instance number **01** for your Java SCS instance.</span></span>

<span data-ttu-id="76079-620">Dále vytvořte požadované interní koncové body pro SAP NetWeaver porty pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="76079-620">Next, create required internal load balancing endpoints for the SAP NetWeaver ports.</span></span>

<span data-ttu-id="76079-621">Pokud chcete vytvořit požadované interní služby load vyrovnávání koncové body, nejdřív vytvořte tyto koncové body pro SAP NetWeaver ABAP ASC porty pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="76079-621">To create required internal load balancing endpoints, first, create these load balancing endpoints for the SAP NetWeaver ABAP ASCS ports:</span></span>

| <span data-ttu-id="76079-622">Název pravidla vyrovnávání služby nebo zatížení</span><span class="sxs-lookup"><span data-stu-id="76079-622">Service/load balancing rule name</span></span> | <span data-ttu-id="76079-623">Výchozí čísla portů</span><span class="sxs-lookup"><span data-stu-id="76079-623">Default port numbers</span></span> | <span data-ttu-id="76079-624">Konkrétní porty (ASC instance číslem instance 00) (YBRAT s 10)</span><span class="sxs-lookup"><span data-stu-id="76079-624">Concrete ports for (ASCS instance with instance number 00) (ERS with 10)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="76079-625">Zařadit Server / *lbrule3200*</span><span class="sxs-lookup"><span data-stu-id="76079-625">Enqueue Server / *lbrule3200*</span></span> |<span data-ttu-id="76079-626">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="76079-626">32<*InstanceNumber*></span></span> |<span data-ttu-id="76079-627">3200</span><span class="sxs-lookup"><span data-stu-id="76079-627">3200</span></span> |
| <span data-ttu-id="76079-628">Server zpráv ABAP / *lbrule3600*</span><span class="sxs-lookup"><span data-stu-id="76079-628">ABAP Message Server / *lbrule3600*</span></span> |<span data-ttu-id="76079-629">36 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="76079-629">36<*InstanceNumber*></span></span> |<span data-ttu-id="76079-630">3600</span><span class="sxs-lookup"><span data-stu-id="76079-630">3600</span></span> |
| <span data-ttu-id="76079-631">Zpráva Vnitřní ABAP / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="76079-631">Internal ABAP Message / *lbrule3900*</span></span> |<span data-ttu-id="76079-632">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="76079-632">39<*InstanceNumber*></span></span> |<span data-ttu-id="76079-633">3900</span><span class="sxs-lookup"><span data-stu-id="76079-633">3900</span></span> |
| <span data-ttu-id="76079-634">Server HTTP zpráv nebo *Lbrule8100*</span><span class="sxs-lookup"><span data-stu-id="76079-634">Message Server HTTP / *Lbrule8100*</span></span> |<span data-ttu-id="76079-635">81 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="76079-635">81<*InstanceNumber*></span></span> |<span data-ttu-id="76079-636">8100</span><span class="sxs-lookup"><span data-stu-id="76079-636">8100</span></span> |
| <span data-ttu-id="76079-637">SAP spuštění služby ASC HTTP / *Lbrule50013*</span><span class="sxs-lookup"><span data-stu-id="76079-637">SAP Start Service ASCS HTTP / *Lbrule50013*</span></span> |<span data-ttu-id="76079-638">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="76079-638">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="76079-639">50013</span><span class="sxs-lookup"><span data-stu-id="76079-639">50013</span></span> |
| <span data-ttu-id="76079-640">SAP spuštění služby ASC HTTPS nebo *Lbrule50014*</span><span class="sxs-lookup"><span data-stu-id="76079-640">SAP Start Service ASCS HTTPS / *Lbrule50014*</span></span> |<span data-ttu-id="76079-641">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="76079-641">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="76079-642">50014</span><span class="sxs-lookup"><span data-stu-id="76079-642">50014</span></span> |
| <span data-ttu-id="76079-643">Zařazování replikace nebo *Lbrule50016*</span><span class="sxs-lookup"><span data-stu-id="76079-643">Enqueue Replication / *Lbrule50016*</span></span> |<span data-ttu-id="76079-644">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="76079-644">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="76079-645">50016</span><span class="sxs-lookup"><span data-stu-id="76079-645">50016</span></span> |
| <span data-ttu-id="76079-646">SAP spuštění služby YBRAT HTTP *Lbrule51013*</span><span class="sxs-lookup"><span data-stu-id="76079-646">SAP Start Service ERS HTTP *Lbrule51013*</span></span> |<span data-ttu-id="76079-647">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="76079-647">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="76079-648">51013</span><span class="sxs-lookup"><span data-stu-id="76079-648">51013</span></span> |
| <span data-ttu-id="76079-649">SAP spuštění služby YBRAT HTTP *Lbrule51014*</span><span class="sxs-lookup"><span data-stu-id="76079-649">SAP Start Service ERS HTTP *Lbrule51014*</span></span> |<span data-ttu-id="76079-650">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="76079-650">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="76079-651">51014</span><span class="sxs-lookup"><span data-stu-id="76079-651">51014</span></span> |
| <span data-ttu-id="76079-652">Win RM *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="76079-652">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="76079-653">5985</span><span class="sxs-lookup"><span data-stu-id="76079-653">5985</span></span> |
| <span data-ttu-id="76079-654">Sdílení souborů *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="76079-654">File Share *Lbrule445*</span></span> | |<span data-ttu-id="76079-655">445</span><span class="sxs-lookup"><span data-stu-id="76079-655">445</span></span> |

<span data-ttu-id="76079-656">_**Tabulka 1:** portu čísla instance SAP NetWeaver ABAP ASC_</span><span class="sxs-lookup"><span data-stu-id="76079-656">_**Table 1:** Port numbers of the SAP NetWeaver ABAP ASCS instances_</span></span>

<span data-ttu-id="76079-657">Pak vytvořte tyto koncové body pro SAP NetWeaver Java SCS porty pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="76079-657">Then, create these load balancing endpoints for the SAP NetWeaver Java SCS ports:</span></span>

| <span data-ttu-id="76079-658">Název pravidla vyrovnávání služby nebo zatížení</span><span class="sxs-lookup"><span data-stu-id="76079-658">Service/load balancing rule name</span></span> | <span data-ttu-id="76079-659">Výchozí čísla portů</span><span class="sxs-lookup"><span data-stu-id="76079-659">Default port numbers</span></span> | <span data-ttu-id="76079-660">Konkrétní porty (SCS instance číslem instance 01) (YBRAT s 11)</span><span class="sxs-lookup"><span data-stu-id="76079-660">Concrete ports for (SCS instance with instance number 01) (ERS with 11)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="76079-661">Zařadit Server / *lbrule3201*</span><span class="sxs-lookup"><span data-stu-id="76079-661">Enqueue Server / *lbrule3201*</span></span> |<span data-ttu-id="76079-662">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="76079-662">32<*InstanceNumber*></span></span> |<span data-ttu-id="76079-663">3201</span><span class="sxs-lookup"><span data-stu-id="76079-663">3201</span></span> |
| <span data-ttu-id="76079-664">Server brány nebo *lbrule3301*</span><span class="sxs-lookup"><span data-stu-id="76079-664">Gateway Server / *lbrule3301*</span></span> |<span data-ttu-id="76079-665">33 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="76079-665">33<*InstanceNumber*></span></span> |<span data-ttu-id="76079-666">3301</span><span class="sxs-lookup"><span data-stu-id="76079-666">3301</span></span> |
| <span data-ttu-id="76079-667">Server zpráv Java / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="76079-667">Java Message Server / *lbrule3900*</span></span> |<span data-ttu-id="76079-668">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="76079-668">39<*InstanceNumber*></span></span> |<span data-ttu-id="76079-669">3901</span><span class="sxs-lookup"><span data-stu-id="76079-669">3901</span></span> |
| <span data-ttu-id="76079-670">Server HTTP zpráv nebo *Lbrule8101*</span><span class="sxs-lookup"><span data-stu-id="76079-670">Message Server HTTP / *Lbrule8101*</span></span> |<span data-ttu-id="76079-671">81 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="76079-671">81<*InstanceNumber*></span></span> |<span data-ttu-id="76079-672">8101</span><span class="sxs-lookup"><span data-stu-id="76079-672">8101</span></span> |
| <span data-ttu-id="76079-673">SAP spuštění služby SCS HTTP / *Lbrule50113*</span><span class="sxs-lookup"><span data-stu-id="76079-673">SAP Start Service SCS HTTP / *Lbrule50113*</span></span> |<span data-ttu-id="76079-674">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="76079-674">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="76079-675">50113</span><span class="sxs-lookup"><span data-stu-id="76079-675">50113</span></span> |
| <span data-ttu-id="76079-676">SAP spuštění služby SCS HTTPS nebo *Lbrule50114*</span><span class="sxs-lookup"><span data-stu-id="76079-676">SAP Start Service SCS HTTPS / *Lbrule50114*</span></span> |<span data-ttu-id="76079-677">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="76079-677">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="76079-678">50114</span><span class="sxs-lookup"><span data-stu-id="76079-678">50114</span></span> |
| <span data-ttu-id="76079-679">Zařazování replikace nebo *Lbrule50116*</span><span class="sxs-lookup"><span data-stu-id="76079-679">Enqueue Replication / *Lbrule50116*</span></span> |<span data-ttu-id="76079-680">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="76079-680">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="76079-681">50116</span><span class="sxs-lookup"><span data-stu-id="76079-681">50116</span></span> |
| <span data-ttu-id="76079-682">SAP spuštění služby YBRAT HTTP *Lbrule51113*</span><span class="sxs-lookup"><span data-stu-id="76079-682">SAP Start Service ERS HTTP *Lbrule51113*</span></span> |<span data-ttu-id="76079-683">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="76079-683">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="76079-684">51113</span><span class="sxs-lookup"><span data-stu-id="76079-684">51113</span></span> |
| <span data-ttu-id="76079-685">SAP spuštění služby YBRAT HTTP *Lbrule51114*</span><span class="sxs-lookup"><span data-stu-id="76079-685">SAP Start Service ERS HTTP *Lbrule51114*</span></span> |<span data-ttu-id="76079-686">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="76079-686">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="76079-687">51114</span><span class="sxs-lookup"><span data-stu-id="76079-687">51114</span></span> |
| <span data-ttu-id="76079-688">Win RM *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="76079-688">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="76079-689">5985</span><span class="sxs-lookup"><span data-stu-id="76079-689">5985</span></span> |
| <span data-ttu-id="76079-690">Sdílení souborů *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="76079-690">File Share *Lbrule445*</span></span> | |<span data-ttu-id="76079-691">445</span><span class="sxs-lookup"><span data-stu-id="76079-691">445</span></span> |

<span data-ttu-id="76079-692">_**Tabulka 2:** portu čísla instance SAP NetWeaver Java SCS_</span><span class="sxs-lookup"><span data-stu-id="76079-692">_**Table 2:** Port numbers of the SAP NetWeaver Java SCS instances_</span></span>

![Obrázek 15: Pravidla pro vyrovnávání zatížení Azure interní Vyrovnávání zatížení výchozí ASC nebo SCS][sap-ha-guide-figure-3004]

<span data-ttu-id="76079-694">_**Obrázek 15:** ASC nebo SCS výchozí pravidla pro vyrovnávání zatížení Azure interní Vyrovnávání zatížení_</span><span class="sxs-lookup"><span data-stu-id="76079-694">_**Figure 15:** Default ASCS/SCS load balancing rules for the Azure internal load balancer_</span></span>

<span data-ttu-id="76079-695">Nastavení IP adresy služby Vyrovnávání zatížení **databázového systému pr1 lb** na IP adresu název virtuálního hostitele instance databázového systému.</span><span class="sxs-lookup"><span data-stu-id="76079-695">Set the IP address of the load balancer **pr1-lb-dbms** to the IP address of the virtual host name of the DBMS instance.</span></span>

### <span data-ttu-id="76079-696"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Změňte pravidla pro vyrovnávání zatížení Azure interní Vyrovnávání zatížení výchozí ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="76079-696"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> Change the ASCS/SCS default load balancing rules for the Azure internal load balancer</span></span>

<span data-ttu-id="76079-697">Pokud chcete používat jiný čísla pro SAP ASC nebo SCS instancí, je třeba změnit názvy a hodnoty jejich porty z výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="76079-697">If you want to use different numbers for the SAP ASCS or SCS instances, you must change the names and values of their ports from default values.</span></span>

1.  <span data-ttu-id="76079-698">Na portálu Azure vyberte  **<* SID*> - lb - ASC načíst vyrovnávání ** > **pravidla Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="76079-698">In the Azure portal, select **<*SID*>-lb-ascs load balancer** > **Load Balancing Rules**.</span></span>
2.  <span data-ttu-id="76079-699">Pro všechna pravidla, které náleží do instance SAP ASC nebo SCS Vyrovnávání zatížení změňte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="76079-699">For all load balancing rules that belong to the SAP ASCS or SCS instance, change these values:</span></span>

  * <span data-ttu-id="76079-700">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="76079-700">Name</span></span>
  * <span data-ttu-id="76079-701">Port</span><span class="sxs-lookup"><span data-stu-id="76079-701">Port</span></span>
  * <span data-ttu-id="76079-702">Back-end port</span><span class="sxs-lookup"><span data-stu-id="76079-702">Back-end port</span></span>

  <span data-ttu-id="76079-703">Například pokud chcete změnit výchozí číslo instance ASC od 00 do 31, budete muset provést změny pro všechny porty uvedené v tabulce 1.</span><span class="sxs-lookup"><span data-stu-id="76079-703">For example, if you want to change the default ASCS instance number from 00 to 31, you need to make the changes for all ports listed in Table 1.</span></span>

  <span data-ttu-id="76079-704">Tady je příklad aktualizace pro port *lbrule3200*.</span><span class="sxs-lookup"><span data-stu-id="76079-704">Here's an example of an update for port *lbrule3200*.</span></span>

  ![Obrázek 16: Změňte pravidla pro vyrovnávání zatížení Azure interní Vyrovnávání zatížení výchozí ASC nebo SCS][sap-ha-guide-figure-3005]

  <span data-ttu-id="76079-706">_**Obrázek 16:** změňte pravidla pro vyrovnávání zatížení Azure interní Vyrovnávání zatížení výchozí ASC nebo SCS_</span><span class="sxs-lookup"><span data-stu-id="76079-706">_**Figure 16:** Change the ASCS/SCS default load balancing rules for the Azure internal load balancer_</span></span>

### <span data-ttu-id="76079-707"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Přidejte virtuální počítače s Windows do domény</span><span class="sxs-lookup"><span data-stu-id="76079-707"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Add Windows virtual machines to the domain</span></span>

<span data-ttu-id="76079-708">Když přiřadíte statickou IP adresu pro virtuální počítače, přidejte virtuální počítače k doméně.</span><span class="sxs-lookup"><span data-stu-id="76079-708">After you assign a static IP address to the virtual machines, add the virtual machines to the domain.</span></span>

![Obrázek 17: Přidáte virtuální počítač k doméně][sap-ha-guide-figure-3006]

<span data-ttu-id="76079-710">_**Obrázek 17:** přidat virtuální počítač k doméně_</span><span class="sxs-lookup"><span data-stu-id="76079-710">_**Figure 17:** Add a virtual machine to a domain_</span></span>

### <span data-ttu-id="76079-711"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Přidat položky registru na obou uzlů clusteru instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="76079-711"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> Add registry entries on both cluster nodes of the SAP ASCS/SCS instance</span></span>

<span data-ttu-id="76079-712">Azure Vyrovnávání zatížení má interní nástroj, zavře připojení při připojení jsou nastavte dobu nečinnosti, čas (časový limit nečinnosti).</span><span class="sxs-lookup"><span data-stu-id="76079-712">Azure Load Balancer has an internal load balancer that closes connections when the connections are idle for a set period of time (an idle timeout).</span></span> <span data-ttu-id="76079-713">SAP pracovních procesů v dialogovém okně Otevřít připojení instance SAP zařadit do fronty zpracovat při první zařazování/dequeue požadavku musí být odeslána.</span><span class="sxs-lookup"><span data-stu-id="76079-713">SAP work processes in dialog instances open connections to the SAP enqueue process as soon as the first enqueue/dequeue request needs to be sent.</span></span> <span data-ttu-id="76079-714">Tato připojení obvykle zůstat zavedené do pracovní proces nebo proces zařazování restartuje.</span><span class="sxs-lookup"><span data-stu-id="76079-714">These connections usually remain established until the work process or the enqueue process restarts.</span></span> <span data-ttu-id="76079-715">Ale pokud se jedná o připojení na nastavenou dobu nečinnosti, nástroje pro vyrovnávání zatížení Azure interní zavře připojení.</span><span class="sxs-lookup"><span data-stu-id="76079-715">However, if the connection is idle for a set period of time, the Azure internal load balancer closes the connections.</span></span> <span data-ttu-id="76079-716">To není problém, protože pracovní proces SAP připojení do procesu zařazování obnoví, pokud už existuje.</span><span class="sxs-lookup"><span data-stu-id="76079-716">This isn't a problem because the SAP work process reestablishes the connection to the enqueue process if it no longer exists.</span></span> <span data-ttu-id="76079-717">Tyto aktivity jsou popsané v trasování vývojáře procesů SAP, ale uživatel vytvořit velké množství další obsah v těchto trasování.</span><span class="sxs-lookup"><span data-stu-id="76079-717">These activities are documented in the developer traces of SAP processes, but they create a large amount of extra content in those traces.</span></span> <span data-ttu-id="76079-718">Je vhodné změnit TCP/IP `KeepAliveTime` a `KeepAliveInterval` na obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-718">It's a good idea to change the TCP/IP `KeepAliveTime` and `KeepAliveInterval` on both cluster nodes.</span></span> <span data-ttu-id="76079-719">Kombinací těchto změn v parametrech TCP/IP s parametry profil SAP, popsanou dále v článku.</span><span class="sxs-lookup"><span data-stu-id="76079-719">Combine these changes in the TCP/IP parameters with SAP profile parameters, described later in the article.</span></span>

<span data-ttu-id="76079-720">Chcete-li přidat položky registru na obou uzlů clusteru instance SAP ASC nebo SCS, nejprve přidejte tyto položky registru systému Windows na obou uzlů clusteru systému Windows pro SAP ASC nebo SCS:</span><span class="sxs-lookup"><span data-stu-id="76079-720">To add registry entries on both cluster nodes of the SAP ASCS/SCS instance, first, add these Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="76079-721">Cesta</span><span class="sxs-lookup"><span data-stu-id="76079-721">Path</span></span> | <span data-ttu-id="76079-722">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="76079-722">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="76079-723">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="76079-723">Variable name</span></span> |`KeepAliveTime` |
| <span data-ttu-id="76079-724">Typ proměnné</span><span class="sxs-lookup"><span data-stu-id="76079-724">Variable type</span></span> |<span data-ttu-id="76079-725">REG_DWORD (decimální):</span><span class="sxs-lookup"><span data-stu-id="76079-725">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="76079-726">Hodnota</span><span class="sxs-lookup"><span data-stu-id="76079-726">Value</span></span> |<span data-ttu-id="76079-727">120000</span><span class="sxs-lookup"><span data-stu-id="76079-727">120000</span></span> |
| <span data-ttu-id="76079-728">Odkaz na dokumentaci</span><span class="sxs-lookup"><span data-stu-id="76079-728">Link to documentation</span></span> |[<span data-ttu-id="76079-729">https://technet.microsoft.com/en-us/library/cc957549.aspx</span><span class="sxs-lookup"><span data-stu-id="76079-729">https://technet.microsoft.com/en-us/library/cc957549.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957549.aspx) |

<span data-ttu-id="76079-730">_**Tabulka 3:** změnit první parametr TCP/IP_</span><span class="sxs-lookup"><span data-stu-id="76079-730">_**Table 3:** Change the first TCP/IP parameter_</span></span>

<span data-ttu-id="76079-731">Pak přidejte této položky registru systému Windows na obou uzlů clusteru systému Windows pro SAP ASC nebo SCS:</span><span class="sxs-lookup"><span data-stu-id="76079-731">Then, add this Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="76079-732">Cesta</span><span class="sxs-lookup"><span data-stu-id="76079-732">Path</span></span> | <span data-ttu-id="76079-733">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="76079-733">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="76079-734">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="76079-734">Variable name</span></span> |`KeepAliveInterval` |
| <span data-ttu-id="76079-735">Typ proměnné</span><span class="sxs-lookup"><span data-stu-id="76079-735">Variable type</span></span> |<span data-ttu-id="76079-736">REG_DWORD (decimální):</span><span class="sxs-lookup"><span data-stu-id="76079-736">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="76079-737">Hodnota</span><span class="sxs-lookup"><span data-stu-id="76079-737">Value</span></span> |<span data-ttu-id="76079-738">120000</span><span class="sxs-lookup"><span data-stu-id="76079-738">120000</span></span> |
| <span data-ttu-id="76079-739">Odkaz na dokumentaci</span><span class="sxs-lookup"><span data-stu-id="76079-739">Link to documentation</span></span> |[<span data-ttu-id="76079-740">https://technet.microsoft.com/en-us/library/cc957548.aspx</span><span class="sxs-lookup"><span data-stu-id="76079-740">https://technet.microsoft.com/en-us/library/cc957548.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

<span data-ttu-id="76079-741">_**Tabulka 4:** změnit druhý parametr protokolu TCP/IP_</span><span class="sxs-lookup"><span data-stu-id="76079-741">_**Table 4:** Change the second TCP/IP parameter_</span></span>

<span data-ttu-id="76079-742">**Aby se změny projevily, restartujte obou uzlů clusteru**.</span><span class="sxs-lookup"><span data-stu-id="76079-742">**To apply the changes, restart both cluster nodes**.</span></span>

### <span data-ttu-id="76079-743"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Nastavení clusteru Windows Server Failover Clustering pro instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="76079-743"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Set up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance</span></span>

<span data-ttu-id="76079-744">Nastavení clusteru s podporou služby Windows Server Failover Clustering pro instance SAP ASC nebo SCS zahrnuje tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="76079-744">Setting up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="76079-745">Shromažďování uzly clusteru v konfiguraci clusteru</span><span class="sxs-lookup"><span data-stu-id="76079-745">Collecting the cluster nodes in a cluster configuration</span></span>
- <span data-ttu-id="76079-746">Konfigurace určující sdílenou složku clusteru</span><span class="sxs-lookup"><span data-stu-id="76079-746">Configuring a cluster file share witness</span></span>

#### <span data-ttu-id="76079-747"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Shromažďovat uzly clusteru v konfiguraci clusteru</span><span class="sxs-lookup"><span data-stu-id="76079-747"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Collect the cluster nodes in a cluster configuration</span></span>

1.  <span data-ttu-id="76079-748">Přidat Role a funkce průvodce přidejte clusteringu obou uzlů clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="76079-748">In the Add Role and Features Wizard, add failover clustering to both cluster nodes.</span></span>
2.  <span data-ttu-id="76079-749">Nastavení clusteru převzetí služeb při selhání pomocí Správce clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="76079-749">Set up the failover cluster by using Failover Cluster Manager.</span></span> <span data-ttu-id="76079-750">Ve Správci clusteru převzetí služeb při selhání vyberte **vytvořením clusteru**a poté přidejte pouze název první clusteru uzlu A. Nepřidávejte druhého uzlu ještě; v pozdější fázi budete přidání druhého uzlu.</span><span class="sxs-lookup"><span data-stu-id="76079-750">In Failover Cluster Manager, select **Create Cluster**, and then add only the name of the first cluster, node A. Do not add the second node yet; you'll add the second node in a later step.</span></span>

  ![Obrázek 18: Přidejte název serveru nebo virtuálního počítače na prvním uzlu clusteru][sap-ha-guide-figure-3007]

  <span data-ttu-id="76079-752">_**Obrázek 18:** přidejte název serveru nebo virtuálního počítače na prvním uzlu clusteru_</span><span class="sxs-lookup"><span data-stu-id="76079-752">_**Figure 18:** Add the server or virtual machine name of the first cluster node_</span></span>

3.  <span data-ttu-id="76079-753">Zadejte název sítě (název hostitele virtuálního) clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-753">Enter the network name (virtual host name) of the cluster.</span></span>

  ![Obrázek 19: Zadejte název clusteru][sap-ha-guide-figure-3008]

  <span data-ttu-id="76079-755">_**Obrázek 19:** zadejte název clusteru_</span><span class="sxs-lookup"><span data-stu-id="76079-755">_**Figure 19:** Enter the cluster name_</span></span>

4.  <span data-ttu-id="76079-756">Po vytvoření clusteru, spusťte test ověření clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-756">After you've created the cluster, run a cluster validation test.</span></span>

  ![Obrázek 20: Spustit kontrolu ověření clusteru][sap-ha-guide-figure-3009]

  <span data-ttu-id="76079-758">_**Obrázek 20:** spustit kontrolu ověření clusteru_</span><span class="sxs-lookup"><span data-stu-id="76079-758">_**Figure 20:** Run the cluster validation check_</span></span>

  <span data-ttu-id="76079-759">Můžete ignorovat jakékoli upozornění o discích v tomto okamžiku v procesu.</span><span class="sxs-lookup"><span data-stu-id="76079-759">You can ignore any warnings about disks at this point in the process.</span></span> <span data-ttu-id="76079-760">Přidáte, že určující sdílené složce a SIOS sdílené disky později.</span><span class="sxs-lookup"><span data-stu-id="76079-760">You'll add a file share witness and the SIOS shared disks later.</span></span> <span data-ttu-id="76079-761">V této fázi nemusíte obávat o kvora.</span><span class="sxs-lookup"><span data-stu-id="76079-761">At this stage, you don't need to worry about having a quorum.</span></span>

  ![Obrázek 21: Nenajde žádný disk kvora][sap-ha-guide-figure-3010]

  <span data-ttu-id="76079-763">_**Obrázek 21:** nenajde žádný disk kvora_</span><span class="sxs-lookup"><span data-stu-id="76079-763">_**Figure 21:** No quorum disk is found_</span></span>

  ![Obrázek 22: Prostředek clusteru jádra potřebuje novou IP adresu][sap-ha-guide-figure-3011]

  <span data-ttu-id="76079-765">_**Obrázek 22:** prostředku clusteru jádra potřebuje novou IP adresu_</span><span class="sxs-lookup"><span data-stu-id="76079-765">_**Figure 22:** Core cluster resource needs a new IP address_</span></span>

5.  <span data-ttu-id="76079-766">Změna IP adresy jádra Clusterové služby.</span><span class="sxs-lookup"><span data-stu-id="76079-766">Change the IP address of the core cluster service.</span></span> <span data-ttu-id="76079-767">Clusteru nelze spustit dokud změnit IP adresu clusteru služby jádra, protože IP adresa serveru odkazuje na jednom z uzlů virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="76079-767">The cluster can't start until you change the IP address of the core cluster service, because the IP address of the server points to one of the virtual machine nodes.</span></span> <span data-ttu-id="76079-768">To udělat na **vlastnosti** stránky prostředek IP základní služby clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-768">Do this on the **Properties** page of the core cluster service's IP resource.</span></span>

  <span data-ttu-id="76079-769">Například je potřeba přiřadit IP adresu (v našem příkladu **10.0.0.42**) pro název virtuálního hostitele clusteru **pr1. ASC vir**.</span><span class="sxs-lookup"><span data-stu-id="76079-769">For example, we need to assign an IP address (in our example, **10.0.0.42**) for the cluster virtual host name **pr1-ascs-vir**.</span></span>

  ![Obrázek 23: V dialogovém okně vlastností změňte IP adresu][sap-ha-guide-figure-3012]

  <span data-ttu-id="76079-771">_**Obrázek 23:** v **vlastnosti** dialogové okno pole, změna IP adresy_</span><span class="sxs-lookup"><span data-stu-id="76079-771">_**Figure 23:** In the **Properties** dialog box, change the IP address_</span></span>

  ![Obrázek 24: Přiřadíte IP adresu, která je vyhrazena pro cluster][sap-ha-guide-figure-3013]

  <span data-ttu-id="76079-773">_**Obrázek 24:** přiřadit IP adresu, která je vyhrazena pro cluster_</span><span class="sxs-lookup"><span data-stu-id="76079-773">_**Figure 24:** Assign the IP address that is reserved for the cluster_</span></span>

6.  <span data-ttu-id="76079-774">Uveďte název virtuálního hostitele clusteru online.</span><span class="sxs-lookup"><span data-stu-id="76079-774">Bring the cluster virtual host name online.</span></span>

  ![Obrázek 25: Základní služby clusteru je zapnutý a běží a s správnou IP adres][sap-ha-guide-figure-3014]

  <span data-ttu-id="76079-776">_**Obrázek 25:** základní služby clusteru je zapnutý a běží a s správnou IP adres_</span><span class="sxs-lookup"><span data-stu-id="76079-776">_**Figure 25:** Cluster core service is up and running, and with the correct IP address_</span></span>

7.  <span data-ttu-id="76079-777">Přidání druhého uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-777">Add the second cluster node.</span></span>

  <span data-ttu-id="76079-778">Nyní když základní Clusterová služba spuštěná, můžete přidat druhý uzel clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-778">Now that the core cluster service is up and running, you can add the second cluster node.</span></span>

  ![Obrázek 26: Přidání druhého uzlu clusteru][sap-ha-guide-figure-3015]

  <span data-ttu-id="76079-780">_**Obrázek 26:** přidání druhého uzlu clusteru_</span><span class="sxs-lookup"><span data-stu-id="76079-780">_**Figure 26:** Add the second cluster node_</span></span>

8.  <span data-ttu-id="76079-781">Zadejte název pro druhý hostitele uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-781">Enter a name for the second cluster node host.</span></span>

  ![Obrázek 27: Zadejte název hostitele druhého uzlu clusteru][sap-ha-guide-figure-3016]

  <span data-ttu-id="76079-783">_**Obrázek 27:** zadejte název hostitele druhého uzlu clusteru_</span><span class="sxs-lookup"><span data-stu-id="76079-783">_**Figure 27:** Enter the second cluster node host name_</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="76079-784">Ujistěte se, který **přidat do clusteru veškeré oprávněné úložiště** zaškrtávací políčko je **není** vybrané.</span><span class="sxs-lookup"><span data-stu-id="76079-784">Be sure that the **Add all eligible storage to the cluster** check box is **NOT** selected.</span></span>  
  >
  >

  ![Obrázek 28: Nevybírejte zaškrtávací políčko][sap-ha-guide-figure-3017]

  <span data-ttu-id="76079-786">_**Obrázek 28:** provést **není** vyberte zaškrtávací pole_</span><span class="sxs-lookup"><span data-stu-id="76079-786">_**Figure 28:** Do **not** select the check box_</span></span>

  <span data-ttu-id="76079-787">Můžete ignorovat upozornění o kvora a disky.</span><span class="sxs-lookup"><span data-stu-id="76079-787">You can ignore warnings about quorum and disks.</span></span> <span data-ttu-id="76079-788">Můžete nastavit kvora a sdílet disk později, jak je popsáno v [instalace s DataKeeper Cluster Edition u disku clusteru sdílení, SAP ASC nebo SCS][sap-ha-guide-8.12.3].</span><span class="sxs-lookup"><span data-stu-id="76079-788">You'll set the quorum and share the disk later, as described in [Installing SIOS DataKeeper Cluster Edition for SAP ASCS/SCS cluster share disk][sap-ha-guide-8.12.3].</span></span>

  ![Obrázek 29: Ignorujte upozornění o disku kvora][sap-ha-guide-figure-3018]

  <span data-ttu-id="76079-790">_**Obrázek 29:** ignorovat upozornění o disku kvora_</span><span class="sxs-lookup"><span data-stu-id="76079-790">_**Figure 29:** Ignore warnings about the disk quorum_</span></span>


#### <span data-ttu-id="76079-791"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Konfigurovat určující sdílenou složku clusteru</span><span class="sxs-lookup"><span data-stu-id="76079-791"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Configure a cluster file share witness</span></span>

<span data-ttu-id="76079-792">Konfigurace určující sdílenou složku clusteru zahrnuje tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="76079-792">Configuring a cluster file share witness involves these tasks:</span></span>

- <span data-ttu-id="76079-793">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="76079-793">Creating a file share</span></span>
- <span data-ttu-id="76079-794">Nastavení kvora určující sdílené složky souborů ve Správci clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="76079-794">Setting the file share witness quorum in Failover Cluster Manager</span></span>

##### <span data-ttu-id="76079-795"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="76079-795"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Create a file share</span></span>

1.  <span data-ttu-id="76079-796">Vyberte složku s kopií místo disk kvora.</span><span class="sxs-lookup"><span data-stu-id="76079-796">Select a file share witness instead of a quorum disk.</span></span> <span data-ttu-id="76079-797">Tuto volbu podporuje DataKeeper s.</span><span class="sxs-lookup"><span data-stu-id="76079-797">SIOS DataKeeper supports this option.</span></span>

  <span data-ttu-id="76079-798">V příkladech v tomto článku je určující sdílenou složku na serveru Active Directory a DNS, který běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-798">In the examples in this article, the file share witness is on the Active Directory/DNS server that is running in Azure.</span></span> <span data-ttu-id="76079-799">Určující sdílená složka se označuje jako **domcontr-0**.</span><span class="sxs-lookup"><span data-stu-id="76079-799">The file share witness is called **domcontr-0**.</span></span> <span data-ttu-id="76079-800">Vzhledem k tomu, že by jste nakonfigurovali připojení VPN do Azure (prostřednictvím sítě Site-to-Site VPN nebo Azure ExpressRoute), vaše/DNS služby Active Directory service je místní a není vhodné pro spuštění souboru sdílet s kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-800">Because you would have configured a VPN connection to Azure (via Site-to-Site VPN or Azure ExpressRoute), your Active Directory/DNS service is on-premises and isn't suitable to run a file share witness.</span></span>

  > [!NOTE]
  > <span data-ttu-id="76079-801">Pokud služby Active Directory a DNS používá jenom v místě, není konfigurace vaší určující sdílenou složku v operačním systému Windows Active Directory a DNS, který běží na místě.</span><span class="sxs-lookup"><span data-stu-id="76079-801">If your Active Directory/DNS service runs only on-premises, don't configure your file share witness on the Active Directory/DNS Windows operating system that is running on-premises.</span></span> <span data-ttu-id="76079-802">Latence sítě mezi uzly clusteru, které jsou spuštěné v Azure a Active Directory a DNS v místním může být příliš velký a způsobit problémy s připojením.</span><span class="sxs-lookup"><span data-stu-id="76079-802">Network latency between cluster nodes running in Azure and Active Directory/DNS on-premises might be too large and cause connectivity issues.</span></span> <span data-ttu-id="76079-803">Nezapomeňte nakonfigurovat určující sdílenou složku na virtuální počítač Azure se blíží uzlu clusteru se systémem.</span><span class="sxs-lookup"><span data-stu-id="76079-803">Be sure to configure the file share witness on an Azure virtual machine that is running close to the cluster node.</span></span>  
  >
  >

  <span data-ttu-id="76079-804">Jednotka kvora vyžaduje alespoň 1 024 MB volného místa.</span><span class="sxs-lookup"><span data-stu-id="76079-804">The quorum drive needs at least 1,024 MB of free space.</span></span> <span data-ttu-id="76079-805">Doporučujeme, abyste hodnotu 2 048 MB volného místa pro disk kvora.</span><span class="sxs-lookup"><span data-stu-id="76079-805">We recommend 2,048 MB of free space for the quorum drive.</span></span>

2.  <span data-ttu-id="76079-806">Přidejte objekt názvu clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-806">Add the cluster name object.</span></span>

  ![Obrázek 30: Přiřadíte oprávnění pro sdílenou složku pro objekt názvu clusteru][sap-ha-guide-figure-3019]

  <span data-ttu-id="76079-808">_**Obrázek 30:** přiřadit oprávnění pro sdílenou složku pro objekt názvu clusteru_</span><span class="sxs-lookup"><span data-stu-id="76079-808">_**Figure 30:** Assign the permissions on the share for the cluster name object_</span></span>

  <span data-ttu-id="76079-809">Ujistěte se, že oprávnění zahrnout oprávnění pro změnu data ve sdílené složce pro objekt názvu clusteru (v našem příkladu **pr1. ASC vir$**).</span><span class="sxs-lookup"><span data-stu-id="76079-809">Be sure that the permissions include the authority to change data in the share for the cluster name object (in our example, **pr1-ascs-vir$**).</span></span>

3.  <span data-ttu-id="76079-810">Chcete-li přidat objekt názvu clusteru do seznamu, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="76079-810">To add the cluster name object to the list, select **Add**.</span></span> <span data-ttu-id="76079-811">Změna filtru k vyhledání počítačových objektů, kromě těch, které znázorňuje obrázek 31.</span><span class="sxs-lookup"><span data-stu-id="76079-811">Change the filter to check for computer objects, in addition to those shown in Figure 31.</span></span>

  ![Obrázek 31: Změňte typy objektů pro zahrnutí počítačů][sap-ha-guide-figure-3020]

  <span data-ttu-id="76079-813">_**Obrázek 31:** změnit typy objektů pro zahrnutí počítačů_</span><span class="sxs-lookup"><span data-stu-id="76079-813">_**Figure 31:** Change the Object Types to include computers_</span></span>

  ![Obrázek 32: Zaškrtněte políčko počítače][sap-ha-guide-figure-3021]

  <span data-ttu-id="76079-815">_**Obrázek 32:** vyberte **počítače** zaškrtávací políčko_</span><span class="sxs-lookup"><span data-stu-id="76079-815">_**Figure 32:** Select the **Computers** check box_</span></span>

4.  <span data-ttu-id="76079-816">Objekt názvu clusteru zadejte, jak ukazuje obrázek 31.</span><span class="sxs-lookup"><span data-stu-id="76079-816">Enter the cluster name object as shown in Figure 31.</span></span> <span data-ttu-id="76079-817">Protože byl vytvořen záznam, můžete změnit oprávnění, jak ukazuje obrázek 30.</span><span class="sxs-lookup"><span data-stu-id="76079-817">Because the record has already been created, you can change the permissions, as shown in Figure 30.</span></span>

5.  <span data-ttu-id="76079-818">Vyberte **zabezpečení** podrobnější kartě do sdílené složky a potom nastavte oprávnění pro objekt názvu clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-818">Select the **Security** tab of the share, and then set more detailed permissions for the cluster name object.</span></span>

  ![Obrázek 33: Nastavte atributy zabezpečení pro objekt názvu clusteru na sdílenou složku kvora souboru][sap-ha-guide-figure-3022]

  <span data-ttu-id="76079-820">_**Obrázek 33:** nastavit atributy zabezpečení pro objekt názvu clusteru v souboru sdílenou složku kvora_</span><span class="sxs-lookup"><span data-stu-id="76079-820">_**Figure 33:** Set the security attributes for the cluster name object on the file share quorum_</span></span>

##### <span data-ttu-id="76079-821"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Nastavení kvora určující sdílené složky souborů ve Správci clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="76079-821"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Set the file share witness quorum in Failover Cluster Manager</span></span>

1.  <span data-ttu-id="76079-822">Otevřete kvora nastavení Průvodce konfigurací.</span><span class="sxs-lookup"><span data-stu-id="76079-822">Open the Configure Quorum Setting Wizard.</span></span>

  ![Obrázek 34: Spuštění Průvodce konfigurace kvora clusteru nastavení][sap-ha-guide-figure-3023]

  <span data-ttu-id="76079-824">_**Obrázek 34:** spustit Průvodce konfigurace kvora clusteru nastavení_</span><span class="sxs-lookup"><span data-stu-id="76079-824">_**Figure 34:** Start the Configure Cluster Quorum Setting Wizard_</span></span>

2.  <span data-ttu-id="76079-825">Na **vyberte konfiguraci kvora** vyberte **vybrat určující disk kvora**.</span><span class="sxs-lookup"><span data-stu-id="76079-825">On the **Select Quorum Configuration** page, select **Select the quorum witness**.</span></span>

  ![Obrázek 35: Konfigurací kvora, které můžete vybrat z][sap-ha-guide-figure-3024]

  <span data-ttu-id="76079-827">_**Obrázek 35:** konfigurací kvora, můžete si vybrat z_</span><span class="sxs-lookup"><span data-stu-id="76079-827">_**Figure 35:** Quorum configurations you can choose from_</span></span>

3.  <span data-ttu-id="76079-828">Na **vybrat určující disk kvora** vyberte **nakonfigurovat určující sdílenou složku**.</span><span class="sxs-lookup"><span data-stu-id="76079-828">On the **Select Quorum Witness** page, select **Configure a file share witness**.</span></span>

  ![Obrázek 36: Vyberte sdílenou složku][sap-ha-guide-figure-3025]

  <span data-ttu-id="76079-830">_**Obrázek 36:** vyberte sdílenou složku_</span><span class="sxs-lookup"><span data-stu-id="76079-830">_**Figure 36:** Select the file share witness_</span></span>

4.  <span data-ttu-id="76079-831">Zadejte cestu UNC ke sdílené složce (v našem příkladu \\domcontr 0\FSW).</span><span class="sxs-lookup"><span data-stu-id="76079-831">Enter the UNC path to the file share (in our example, \\domcontr-0\FSW).</span></span> <span data-ttu-id="76079-832">Pokud chcete zobrazit seznam změn, můžete provést, vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="76079-832">To see a list of the changes you can make, select **Next**.</span></span>

  ![Obrázek 37: Zadejte umístění pro sdílení souborů pro sdílenou složku s kopií clusteru][sap-ha-guide-figure-3026]

  <span data-ttu-id="76079-834">_**Obrázek 37:** zadejte umístění pro sdílení souborů pro sdílenou složku s kopií clusteru_</span><span class="sxs-lookup"><span data-stu-id="76079-834">_**Figure 37:** Define the file share location for the witness share_</span></span>

5.  <span data-ttu-id="76079-835">Vyberte požadované změny a pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="76079-835">Select the changes you want, and then select **Next**.</span></span> <span data-ttu-id="76079-836">Je třeba úspěšně znovu nakonfigurovat konfiguraci clusteru, jak ukazuje obrázek 38.</span><span class="sxs-lookup"><span data-stu-id="76079-836">You need to successfully reconfigure the cluster configuration as shown in Figure 38.</span></span>  

  ![Obrázek 38: Potvrzení, že jste jste změnili konfiguraci clusteru][sap-ha-guide-figure-3027]

  <span data-ttu-id="76079-838">_**Obrázek 38:** potvrzení, že jste jste změnili konfiguraci clusteru_</span><span class="sxs-lookup"><span data-stu-id="76079-838">_**Figure 38:** Confirmation that you've reconfigured the cluster_</span></span>

<span data-ttu-id="76079-839">Po úspěšném nainstalování clusteru převzetí služeb při selhání systému Windows, třeba změny provedené některé prahové hodnoty pro přizpůsobení převzetí služeb při selhání detekce podmínky v Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-839">After installing the Windows Failover Cluster successfully, changes need to be made to some thresholds to adapt failover detection to conditions in Azure.</span></span> <span data-ttu-id="76079-840">Parametry, které chcete změnit, jsou popsané v tomto blogu: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/.</span><span class="sxs-lookup"><span data-stu-id="76079-840">The parameters to be changed are documented in this blog: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/ .</span></span> <span data-ttu-id="76079-841">Za předpokladu, že dva virtuální počítače, které sestavení konfigurace clusteru systému Windows pro ASC nebo SCS jsou ve stejné podsíti, je nutné změnit tak, aby tyto hodnoty následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="76079-841">Assuming that your two VMs that build the Windows Cluster Configuration for ASCS/SCS are in the same SubNet, the following parameters need to be changed to these values:</span></span>
- <span data-ttu-id="76079-842">SameSubNetDelay = 2</span><span class="sxs-lookup"><span data-stu-id="76079-842">SameSubNetDelay = 2</span></span>
- <span data-ttu-id="76079-843">SameSubNetThreshold = 15</span><span class="sxs-lookup"><span data-stu-id="76079-843">SameSubNetThreshold = 15</span></span>

<span data-ttu-id="76079-844">Tato nastavení byly testovány s zákazníků a poskytuje dobrý ohrožení chcete být odolní dostatečně na jedné straně.</span><span class="sxs-lookup"><span data-stu-id="76079-844">These settings were tested with customers and provided a good compromise to be resilient enough on the one side.</span></span> <span data-ttu-id="76079-845">Na druhé straně se tato nastavení fast poskytuje dostatek převzetí služeb při selhání ve skutečné chybové stavy na SAP selhání uzlu nebo virtuálního počítače nebo softwaru.</span><span class="sxs-lookup"><span data-stu-id="76079-845">On the other hand those settings were providing fast enough failover in real error conditions on SAP software or node/VM failure.</span></span> 

### <span data-ttu-id="76079-846"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>Instalace s DataKeeper Cluster Edition pro SAP ASC nebo SCS disk clusteru sdílené složky</span><span class="sxs-lookup"><span data-stu-id="76079-846"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> Install SIOS DataKeeper Cluster Edition for the SAP ASCS/SCS cluster share disk</span></span>

<span data-ttu-id="76079-847">Teď máte fungující konfiguraci služby Windows Server Failover Clustering v Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-847">You now have a working Windows Server Failover Clustering configuration in Azure.</span></span> <span data-ttu-id="76079-848">Ale instalaci instance SAP ASC nebo SCS, budete potřebovat prostředek sdíleného disku.</span><span class="sxs-lookup"><span data-stu-id="76079-848">But, to install an SAP ASCS/SCS instance, you need a shared disk resource.</span></span> <span data-ttu-id="76079-849">Nelze vytvořit sdílené síťové prostředky, které potřebujete v Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-849">You cannot create the shared disk resources you need in Azure.</span></span> <span data-ttu-id="76079-850">S DataKeeper Cluster Edition je řešení třetí strany, které můžete použít k vytvoření sdílené síťové prostředky.</span><span class="sxs-lookup"><span data-stu-id="76079-850">SIOS DataKeeper Cluster Edition is a third-party solution you can use to create shared disk resources.</span></span>

<span data-ttu-id="76079-851">Instalace s DataKeeper Cluster Edition pro sdílenou složku disk clusteru SAP ASC nebo SCS zahrnuje tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="76079-851">Installing SIOS DataKeeper Cluster Edition for the SAP ASCS/SCS cluster share disk involves these tasks:</span></span>

- <span data-ttu-id="76079-852">Přidání rozhraní .NET Framework 3.5</span><span class="sxs-lookup"><span data-stu-id="76079-852">Adding the .NET Framework 3.5</span></span>
- <span data-ttu-id="76079-853">Instalace SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="76079-853">Installing SIOS DataKeeper</span></span>
- <span data-ttu-id="76079-854">Nastavení služby s DataKeeper</span><span class="sxs-lookup"><span data-stu-id="76079-854">Setting up SIOS DataKeeper</span></span>

#### <span data-ttu-id="76079-855"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>Přidání rozhraní .NET Framework 3.5</span><span class="sxs-lookup"><span data-stu-id="76079-855"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> Add the .NET Framework 3.5</span></span>
<span data-ttu-id="76079-856">Rozhraní Microsoft .NET Framework 3.5 se automaticky aktivovat nebo není nainstalovaná v systému Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="76079-856">The Microsoft .NET Framework 3.5 isn't automatically activated or installed on Windows Server 2012 R2.</span></span> <span data-ttu-id="76079-857">Protože DataKeeper s vyžaduje rozhraní .NET Framework na všech uzlech, jež nainstalujete DataKeeper, musíte nainstalovat rozhraní .NET Framework 3.5 v hostovaném operačním systému všech virtuálních počítačů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-857">Because SIOS DataKeeper requires the .NET Framework to be on all nodes that you install DataKeeper on, you must install the .NET Framework 3.5 on the guest operating system of all virtual machines in the cluster.</span></span>

<span data-ttu-id="76079-858">Chcete-li přidat rozhraní .NET Framework 3.5 dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="76079-858">There are two ways to add the .NET Framework 3.5:</span></span>

- <span data-ttu-id="76079-859">Použití funkce Průvodce přidáním rolí a v systému Windows, jak ukazuje obrázek 39.</span><span class="sxs-lookup"><span data-stu-id="76079-859">Use the Add Roles and Features Wizard in Windows as shown in Figure 39.</span></span>

  ![Obrázek 39: Instalace rozhraní .NET Framework 3.5 pomocí funkce Průvodce přidáním rolí a][sap-ha-guide-figure-3028]

  <span data-ttu-id="76079-861">_**Obrázek 39:** instalace rozhraní .NET Framework 3.5 pomocí funkce Průvodce přidáním rolí a_</span><span class="sxs-lookup"><span data-stu-id="76079-861">_**Figure 39:** Install the .NET Framework 3.5 by using the Add Roles and Features Wizard_</span></span>

  ![Obrázek 40: Průběh instalace panelu při instalaci rozhraní .NET Framework 3.5 pomocí Přidat role a funkce Průvodce][sap-ha-guide-figure-3029]

  <span data-ttu-id="76079-863">_**Obrázek 40:** průběh instalace panelu při instalaci rozhraní .NET Framework 3.5 pomocí Přidat role a funkce Průvodce_</span><span class="sxs-lookup"><span data-stu-id="76079-863">_**Figure 40:** Installation progress bar when you install the .NET Framework 3.5 by using the Add Roles and Features Wizard_</span></span>

- <span data-ttu-id="76079-864">Použijte nástroj příkazového řádku nástroje dism.exe.</span><span class="sxs-lookup"><span data-stu-id="76079-864">Use the command-line tool dism.exe.</span></span> <span data-ttu-id="76079-865">Pro tento typ instalace musíte pro přístup k adresáři SxS na instalačním médiu systému Windows.</span><span class="sxs-lookup"><span data-stu-id="76079-865">For this type of installation, you need to access the SxS directory on the Windows installation media.</span></span> <span data-ttu-id="76079-866">Na příkazovém řádku se zvýšenými oprávněními zadejte:</span><span class="sxs-lookup"><span data-stu-id="76079-866">At an elevated command prompt, type:</span></span>

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <span data-ttu-id="76079-867"><a name="dd41d5a2-8083-415b-9878-839652812102"></a>Nainstalujte SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="76079-867"><a name="dd41d5a2-8083-415b-9878-839652812102"></a> Install SIOS DataKeeper</span></span>

<span data-ttu-id="76079-868">Nainstalujte na každém uzlu v clusteru s DataKeeper Cluster Edition.</span><span class="sxs-lookup"><span data-stu-id="76079-868">Install SIOS DataKeeper Cluster Edition on each node in the cluster.</span></span> <span data-ttu-id="76079-869">Pokud chcete vytvořit virtuální sdílené úložiště s s DataKeeper, vytvořte synchronizoval zrcadlení a pak simulovat sdílené úložiště clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-869">To create virtual shared storage with SIOS DataKeeper, create a synced mirror and then simulate cluster shared storage.</span></span>

<span data-ttu-id="76079-870">Před instalací softwaru SIOS vytvořit uživatele domény **DataKeeperSvc**.</span><span class="sxs-lookup"><span data-stu-id="76079-870">Before you install the SIOS software, create the domain user **DataKeeperSvc**.</span></span>

> [!NOTE]
> <span data-ttu-id="76079-871">Přidat **DataKeeperSvc** uživateli **místního správce** v obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-871">Add the **DataKeeperSvc** user to the **Local Administrator** group on both cluster nodes.</span></span>
>
>

<span data-ttu-id="76079-872">Instalace s DataKeeper:</span><span class="sxs-lookup"><span data-stu-id="76079-872">To install SIOS DataKeeper:</span></span>

1.  <span data-ttu-id="76079-873">Nainstalujte SIOS software do obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-873">Install the SIOS software on both cluster nodes.</span></span>

  ![Instalační program SIOS][sap-ha-guide-figure-3030]

  ![41 obrázek: První stránka instalace s DataKeeper][sap-ha-guide-figure-3031]

  <span data-ttu-id="76079-876">_**Obrázek 41:** první stránka s DataKeeper instalace_</span><span class="sxs-lookup"><span data-stu-id="76079-876">_**Figure 41:** First page of the SIOS DataKeeper installation_</span></span>

2.  <span data-ttu-id="76079-877">V dialogovém okně znázorňuje obrázek 42 vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="76079-877">In the dialog box shown in Figure 42, select **Yes**.</span></span>

  ![Obrázek 42: DataKeeper vás upozorní, že služba bude zakázán][sap-ha-guide-figure-3032]

  <span data-ttu-id="76079-879">_**Obrázek 42:** DataKeeper informuje, že služba bude zakázán_</span><span class="sxs-lookup"><span data-stu-id="76079-879">_**Figure 42:** DataKeeper informs you that a service will be disabled_</span></span>

3.  <span data-ttu-id="76079-880">V dialogovém okně zobrazí obrázek 43 doporučujeme, abyste vybrali **účet domény nebo serveru**.</span><span class="sxs-lookup"><span data-stu-id="76079-880">In the dialog box shown in Figure 43, we recommend that you select **Domain or Server account**.</span></span>

  ![Obrázek 43: Výběr uživatele s DataKeeper][sap-ha-guide-figure-3033]

  <span data-ttu-id="76079-882">_**Obrázek 43:** výběr uživatele s DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="76079-882">_**Figure 43:** User selection for SIOS DataKeeper_</span></span>

4.  <span data-ttu-id="76079-883">Zadejte uživatelské jméno pro účet domény a hesla, které jste vytvořili pro DataKeeper s.</span><span class="sxs-lookup"><span data-stu-id="76079-883">Enter the domain account user name and passwords that you created for SIOS DataKeeper.</span></span>

  ![Obrázek 44: Zadejte uživatelské jméno domény a heslo pro instalace s DataKeeper][sap-ha-guide-figure-3034]

  <span data-ttu-id="76079-885">_**Obrázek 44:** zadejte uživatelské jméno domény a heslo pro instalaci s DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="76079-885">_**Figure 44:** Enter the domain user name and password for the SIOS DataKeeper installation_</span></span>

5.  <span data-ttu-id="76079-886">Licenční klíč pro instanci s DataKeeper nainstalujte, jak ukazuje obrázek 45.</span><span class="sxs-lookup"><span data-stu-id="76079-886">Install the license key for your SIOS DataKeeper instance as shown in Figure 45.</span></span>

  ![45 obrázek: Zadejte klíč licence s DataKeeper][sap-ha-guide-figure-3035]

  <span data-ttu-id="76079-888">_**Obrázek 45:** zadejte klíč DataKeeper s licencí_</span><span class="sxs-lookup"><span data-stu-id="76079-888">_**Figure 45:** Enter your SIOS DataKeeper license key_</span></span>

6.  <span data-ttu-id="76079-889">Po zobrazení výzvy restartujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="76079-889">When prompted, restart the virtual machine.</span></span>

#### <span data-ttu-id="76079-890"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>Nastavení pro zařízení s DataKeeper</span><span class="sxs-lookup"><span data-stu-id="76079-890"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> Set up SIOS DataKeeper</span></span>

<span data-ttu-id="76079-891">Po instalaci s DataKeeper oba uzly, budete muset spustit konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="76079-891">After you install SIOS DataKeeper on both nodes, you need to start the configuration.</span></span> <span data-ttu-id="76079-892">Cílem konfigurace je tak, aby měl synchronní data replikace mezi další disky připojené ke každému z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="76079-892">The goal of the configuration is to have synchronous data replication between the additional disks attached to each of the virtual machines.</span></span>

1.  <span data-ttu-id="76079-893">Spusťte nástroj Správa DataKeeper a konfigurace a potom vyberte **připojit Server**.</span><span class="sxs-lookup"><span data-stu-id="76079-893">Start the DataKeeper Management and Configuration tool, and then select **Connect Server**.</span></span> <span data-ttu-id="76079-894">(V obrázku 46, tato možnost je v kroužku červeně.)</span><span class="sxs-lookup"><span data-stu-id="76079-894">(In Figure 46, this option is circled in red.)</span></span>

  ![46 obrázek: Nástroj Konfigurace a SIOS DataKeeper správy][sap-ha-guide-figure-3036]

  <span data-ttu-id="76079-896">_**Obrázek 46:** s DataKeeper správu a konfiguraci nástroje_</span><span class="sxs-lookup"><span data-stu-id="76079-896">_**Figure 46:** SIOS DataKeeper Management and Configuration tool_</span></span>

2.  <span data-ttu-id="76079-897">Zadejte název nebo adresu TCP/IP prvního uzlu, který nástroj Správa a konfigurace by se měly připojit k a v druhém kroku, ve druhém uzlu.</span><span class="sxs-lookup"><span data-stu-id="76079-897">Enter the name or TCP/IP address of the first node the Management and Configuration tool should connect to, and, in a second step, the second node.</span></span>

  ![Obrázek 47: Vložit název nebo adresu TCP/IP prvního uzlu pro správu a konfigurační nástroj by měl připojit a v druhém kroku, ve druhém uzlu][sap-ha-guide-figure-3037]

  <span data-ttu-id="76079-899">_**Obrázek 47:** vložit název nebo adresu TCP/IP prvního uzlu nástroj pro správu a konfiguraci by měl připojit a v druhém kroku, ve druhém uzlu_</span><span class="sxs-lookup"><span data-stu-id="76079-899">_**Figure 47:** Insert the name or TCP/IP address of the first node the Management and Configuration tool should connect to, and in a second step, the second node_</span></span>

3.  <span data-ttu-id="76079-900">Vytvoření úlohy replikace mezi dvěma uzly.</span><span class="sxs-lookup"><span data-stu-id="76079-900">Create the replication job between the two nodes.</span></span>

  ![Obrázek 48: Vytvoření úlohy replikace][sap-ha-guide-figure-3038]

  <span data-ttu-id="76079-902">_**Obrázek 48:** vytvořit úlohu replikace_</span><span class="sxs-lookup"><span data-stu-id="76079-902">_**Figure 48:** Create a replication job_</span></span>

  <span data-ttu-id="76079-903">Průvodce vás provede procesem vytvoření úlohy replikace.</span><span class="sxs-lookup"><span data-stu-id="76079-903">A wizard guides you through the process of creating a replication job.</span></span>
4.  <span data-ttu-id="76079-904">Zadejte název, adresa TCP/IP a svazku zdrojový uzel.</span><span class="sxs-lookup"><span data-stu-id="76079-904">Define the name, TCP/IP address, and disk volume of the source node.</span></span>

  ![Obrázek 49: Definujte název úlohy replikace][sap-ha-guide-figure-3039]

  <span data-ttu-id="76079-906">_**Obrázek 49:** definujte název úlohy replikace_</span><span class="sxs-lookup"><span data-stu-id="76079-906">_**Figure 49:** Define the name of the replication job_</span></span>

  ![Obrázek 50: Zadejte základní data pro uzel, které by měly být aktuální zdrojový uzel][sap-ha-guide-figure-3040]

  <span data-ttu-id="76079-908">_**Obrázek 50:** zadat základní data pro uzel, které by měly být aktuální zdrojový uzel_</span><span class="sxs-lookup"><span data-stu-id="76079-908">_**Figure 50:** Define the base data for the node, which should be the current source node_</span></span>

5.  <span data-ttu-id="76079-909">Zadejte název, adresa TCP/IP a svazku cílový uzel.</span><span class="sxs-lookup"><span data-stu-id="76079-909">Define the name, TCP/IP address, and disk volume of the target node.</span></span>

  ![Obrázek 51: Zadejte základní data pro uzel, které by měly být aktuální cílový uzel][sap-ha-guide-figure-3041]

  <span data-ttu-id="76079-911">_**Obrázek 51:** zadat základní data pro uzel, které by měly být aktuální cílový uzel_</span><span class="sxs-lookup"><span data-stu-id="76079-911">_**Figure 51:** Define the base data for the node, which should be the current target node_</span></span>

6.  <span data-ttu-id="76079-912">Definujte algoritmy komprese.</span><span class="sxs-lookup"><span data-stu-id="76079-912">Define the compression algorithms.</span></span> <span data-ttu-id="76079-913">V našem příkladu doporučujeme kompresi datového proudu replikace.</span><span class="sxs-lookup"><span data-stu-id="76079-913">In our example, we recommend that you compress the replication stream.</span></span> <span data-ttu-id="76079-914">Hlavně v situacích, opakované synchronizace kompresi datového proudu replikace výrazně snižuje dobu Opakovaná synchronizace.</span><span class="sxs-lookup"><span data-stu-id="76079-914">Especially in resynchronization situations, the compression of the replication stream dramatically reduces resynchronization time.</span></span> <span data-ttu-id="76079-915">Všimněte si, že komprese používá prostředky procesoru a paměť RAM virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="76079-915">Note that compression uses the CPU and RAM resources of a virtual machine.</span></span> <span data-ttu-id="76079-916">Jako hodnota se zvyšuje rychlost komprese, takže nemá objem prostředků procesoru, které používá.</span><span class="sxs-lookup"><span data-stu-id="76079-916">As the compression rate increases, so does the volume of CPU resources used.</span></span> <span data-ttu-id="76079-917">Můžete také upravit toto nastavení později.</span><span class="sxs-lookup"><span data-stu-id="76079-917">You also can adjust this setting later.</span></span>

7.  <span data-ttu-id="76079-918">Další nastavení, které je potřeba zkontrolovat se, zda dojde k replikaci synchronně nebo asynchronně.</span><span class="sxs-lookup"><span data-stu-id="76079-918">Another setting you need to check is whether the replication occurs asynchronously or synchronously.</span></span> <span data-ttu-id="76079-919">*Pokud budete chránit SAP ASC nebo SCS konfigurace, je nutné použít synchronní replikace*.</span><span class="sxs-lookup"><span data-stu-id="76079-919">*When you protect SAP ASCS/SCS configurations, you must use synchronous replication*.</span></span>  

  ![Obrázek 52: Definování podrobnosti k replikaci][sap-ha-guide-figure-3042]

  <span data-ttu-id="76079-921">_**Obrázek 52:** definovat podrobnosti k replikaci_</span><span class="sxs-lookup"><span data-stu-id="76079-921">_**Figure 52:** Define replication details_</span></span>

8.  <span data-ttu-id="76079-922">Zadejte, zda svazek, který se replikují úlohou replikace by měly být zastoupeny ke konfiguraci clusteru Windows Server Failover Clustering jako sdílený disk.</span><span class="sxs-lookup"><span data-stu-id="76079-922">Define whether the volume that is replicated by the replication job should be represented to a Windows Server Failover Clustering cluster configuration as a shared disk.</span></span> <span data-ttu-id="76079-923">Pro konfiguraci SAP ASC nebo SCS vyberte **Ano** tak, aby Windows cluster uvidí replikované svazek jako sdílený disk, který můžete použít jako svazek clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-923">For the SAP ASCS/SCS configuration, select **Yes** so that the Windows cluster sees the replicated volume as a shared disk that it can use as a cluster volume.</span></span>

  ![Obrázek 53: Vyberte možnost Ano nastavit replikované svazek jako svazek clusteru][sap-ha-guide-figure-3043]

  <span data-ttu-id="76079-925">_**Obrázek 53:** vyberte **Ano** nastavit replikované svazek jako svazek clusteru_</span><span class="sxs-lookup"><span data-stu-id="76079-925">_**Figure 53:** Select **Yes** to set the replicated volume as a cluster volume_</span></span>

  <span data-ttu-id="76079-926">Po vytvoření svazku nástroj DataKeeper Správa a konfigurace ukazuje, že je úloha replikace aktivní.</span><span class="sxs-lookup"><span data-stu-id="76079-926">After the volume is created, the DataKeeper Management and Configuration tool shows that the replication job is active.</span></span>

  ![Obrázek 54: DataKeeper synchronní zrcadlení pro sdílenou složku disk SAP ASC nebo SCS je aktivní][sap-ha-guide-figure-3044]

  <span data-ttu-id="76079-928">_**Obrázek 54:** DataKeeper synchronní zrcadlení pro SAP ASC nebo SCS sdílet disk je aktivní_</span><span class="sxs-lookup"><span data-stu-id="76079-928">_**Figure 54:** DataKeeper synchronous mirroring for the SAP ASCS/SCS share disk is active_</span></span>

  <span data-ttu-id="76079-929">Správce clusteru převzetí služeb při selhání teď disk jako disk s DataKeeper ukazuje, jak ukazuje obrázek 55.</span><span class="sxs-lookup"><span data-stu-id="76079-929">Failover Cluster Manager now shows the disk as a DataKeeper disk, as shown in Figure 55.</span></span>

  ![Obrázek 55: Správce clusteru převzetí služeb při selhání zobrazuje na disk, který DataKeeper replikovat][sap-ha-guide-figure-3045]

  <span data-ttu-id="76079-931">_**Obrázek 55:** Správce clusteru převzetí služeb při selhání disku ukazuje, že DataKeeper replikovat_</span><span class="sxs-lookup"><span data-stu-id="76079-931">_**Figure 55:** Failover Cluster Manager shows the disk that DataKeeper replicated_</span></span>

## <span data-ttu-id="76079-932"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Instalace systému SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="76079-932"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a> Install the SAP NetWeaver system</span></span>

<span data-ttu-id="76079-933">Instalace databázového systému jsme nebude popisují nastavení lišit v závislosti na tom, které můžete použít systém databázového systému.</span><span class="sxs-lookup"><span data-stu-id="76079-933">We won’t describe the DBMS setup because setups vary depending on the DBMS system you use.</span></span> <span data-ttu-id="76079-934">Ale předpokládáme, že jsou aspekty vysoké dostupnosti s databázového systému řešit pomocí funkce, které různých výrobců databázového systému podpora pro Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-934">However, we assume that high-availability concerns with the DBMS are addressed with the functionalities the different DBMS vendors support for Azure.</span></span> <span data-ttu-id="76079-935">Například Always On nebo zrcadlení databáze systému SQL Server a Oracle Data Guard pro databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="76079-935">For example, Always On or database mirroring for SQL Server, and Oracle Data Guard for Oracle databases.</span></span> <span data-ttu-id="76079-936">Ve scénáři, které používáme v tomto článku jsme nebyla přidat další ochranu do databázového systému.</span><span class="sxs-lookup"><span data-stu-id="76079-936">In the scenario we use in this article, we didn't add more protection to the DBMS.</span></span>

<span data-ttu-id="76079-937">Když různé služby databázového systému interakci s tímto typem Clusterované konfigurace SAP ASC nebo SCS v Azure neexistují žádné zvláštní požadavky.</span><span class="sxs-lookup"><span data-stu-id="76079-937">There are no special considerations when different DBMS services interact with this kind of clustered SAP ASCS/SCS configuration in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="76079-938">Postupy instalace systémů SAP NetWeaver ABAP, Java systémy a systémy ABAP + Java jsou téměř shodné.</span><span class="sxs-lookup"><span data-stu-id="76079-938">The installation procedures of SAP NetWeaver ABAP systems, Java systems, and ABAP+Java systems are almost identical.</span></span> <span data-ttu-id="76079-939">Nejdůležitější rozdíl je, že systému SAP ABAP má jednu instanci ASC.</span><span class="sxs-lookup"><span data-stu-id="76079-939">The most significant difference is that an SAP ABAP system has one ASCS instance.</span></span> <span data-ttu-id="76079-940">V systému SAP Java má jednu instanci SCS.</span><span class="sxs-lookup"><span data-stu-id="76079-940">The SAP Java system has one SCS instance.</span></span> <span data-ttu-id="76079-941">V systému SAP ABAP + Java obsahuje jednu instanci ASC a jednu instanci SCS spuštěné ve stejné skupině clusteru převzetí služeb při selhání Microsoft.</span><span class="sxs-lookup"><span data-stu-id="76079-941">The SAP ABAP+Java system has one ASCS instance and one SCS instance running in the same Microsoft failover cluster group.</span></span> <span data-ttu-id="76079-942">Případné rozdíly instalace pro každou SAP NetWeaver instalace zásobníku se výslovně uveden.</span><span class="sxs-lookup"><span data-stu-id="76079-942">Any installation differences for each SAP NetWeaver installation stack are explicitly mentioned.</span></span> <span data-ttu-id="76079-943">Můžete předpokládat, že všechny ostatní části jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="76079-943">You can assume that all other parts are the same.</span></span>  
>
>

### <span data-ttu-id="76079-944"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Instalace s vysokou dostupností ASC nebo SCS instance SAP</span><span class="sxs-lookup"><span data-stu-id="76079-944"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a> Install SAP with a high-availability ASCS/SCS instance</span></span>

> [!IMPORTANT]
> <span data-ttu-id="76079-945">Nezapomeňte toto stránkovacím souboru na svazcích DataKeeper zrcadlení.</span><span class="sxs-lookup"><span data-stu-id="76079-945">Be sure not to place your page file on DataKeeper mirrored volumes.</span></span> <span data-ttu-id="76079-946">DataKeeper nepodporuje zrcadlové svazky.</span><span class="sxs-lookup"><span data-stu-id="76079-946">DataKeeper does not support mirrored volumes.</span></span> <span data-ttu-id="76079-947">Můžete ponechat stránkovacím souboru na dočasné jednotce D Azure virtuálního počítače, který je výchozí.</span><span class="sxs-lookup"><span data-stu-id="76079-947">You can leave your page file on the temporary drive D of an Azure virtual machine, which is the default.</span></span> <span data-ttu-id="76079-948">Pokud není již existuje, přesuňte stránkovací soubor Windows jednotku D: virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-948">If it's not already there, move the Windows page file to drive D: of your Azure virtual machine.</span></span>
>
>

<span data-ttu-id="76079-949">Instalace s vysokou dostupností ASC nebo SCS instance SAP zahrnuje tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="76079-949">Installing SAP with a high-availability ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="76079-950">Vytváření název virtuálního hostitele pro SAP ASC nebo SCS skupinu prostředků clusteru</span><span class="sxs-lookup"><span data-stu-id="76079-950">Creating a virtual host name for the clustered SAP ASCS/SCS instance</span></span>
- <span data-ttu-id="76079-951">Instalace prvního uzlu clusteru SAP</span><span class="sxs-lookup"><span data-stu-id="76079-951">Installing the SAP first cluster node</span></span>
- <span data-ttu-id="76079-952">Úprava profilu SAP ASC nebo SCS instance</span><span class="sxs-lookup"><span data-stu-id="76079-952">Modifying the SAP profile of the ASCS/SCS instance</span></span>
- <span data-ttu-id="76079-953">Přidání port testu</span><span class="sxs-lookup"><span data-stu-id="76079-953">Adding a probe port</span></span>
- <span data-ttu-id="76079-954">Otevřete port testu brány firewall systému Windows</span><span class="sxs-lookup"><span data-stu-id="76079-954">Opening the Windows firewall probe port</span></span>

#### <span data-ttu-id="76079-955"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Vytvořte název virtuálního hostitele pro SAP ASC nebo SCS skupinu prostředků clusteru</span><span class="sxs-lookup"><span data-stu-id="76079-955"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a> Create a virtual host name for the clustered SAP ASCS/SCS instance</span></span>

1.  <span data-ttu-id="76079-956">Ve Správci DNS systému Windows vytvořte záznam DNS pro název virtuálního hostitele ASC nebo SCS instance.</span><span class="sxs-lookup"><span data-stu-id="76079-956">In the Windows DNS manager, create a DNS entry for the virtual host name of the ASCS/SCS instance.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="76079-957">IP adresa, která přiřadíte název virtuálního hostitele ASC nebo SCS instance musí být stejná jako adresa IP, který jste přiřadili k vyrovnávání zatížení Azure (**<*SID*> - lb - ASC **).</span><span class="sxs-lookup"><span data-stu-id="76079-957">The IP address that you assign to the virtual host name of the ASCS/SCS instance must be the same as the IP address that you assigned to Azure Load Balancer (**<*SID*>-lb-ascs**).</span></span>  
  >
  >

  <span data-ttu-id="76079-958">IP adresa virtuální název hostitele SAP ASC nebo SCS (**pr1. ASC sap**) je stejný jako IP adresu služby Vyrovnávání zatížení Azure (**pr1-lb Asc**).</span><span class="sxs-lookup"><span data-stu-id="76079-958">The IP address of the virtual SAP ASCS/SCS host name (**pr1-ascs-sap**) is the same as the IP address of Azure Load Balancer (**pr1-lb-ascs**).</span></span>

  ![Obrázek 56: Definování položky DNS pro virtuální název clusteru SAP ASC nebo SCS a adresa TCP/IP][sap-ha-guide-figure-3046]

  <span data-ttu-id="76079-960">_**Obrázek 56:** definovat položku DNS pro virtuální název clusteru SAP ASC nebo SCS a adresa TCP/IP_</span><span class="sxs-lookup"><span data-stu-id="76079-960">_**Figure 56:** Define the DNS entry for the SAP ASCS/SCS cluster virtual name and TCP/IP address_</span></span>

2.  <span data-ttu-id="76079-961">Chcete-li definovat přiřazené název virtuálního hostitele IP adresy, vyberte **Správce DNS** > **domény**.</span><span class="sxs-lookup"><span data-stu-id="76079-961">To define the IP address assigned to the virtual host name, select **DNS Manager** > **Domain**.</span></span>

  ![Obrázek 57: Nový virtuální název a adresu TCP/IP pro konfiguraci clusteru SAP ASC nebo SCS][sap-ha-guide-figure-3047]

  <span data-ttu-id="76079-963">_**Obrázek 57:** nový virtuální název a TCP/IP adres pro konfiguraci clusteru SAP ASC nebo SCS_</span><span class="sxs-lookup"><span data-stu-id="76079-963">_**Figure 57:** New virtual name and TCP/IP address for SAP ASCS/SCS cluster configuration_</span></span>

#### <span data-ttu-id="76079-964"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Instalace prvního uzlu clusteru SAP</span><span class="sxs-lookup"><span data-stu-id="76079-964"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> Install the SAP first cluster node</span></span>

1.  <span data-ttu-id="76079-965">Provést první možnost uzlu clusteru na uzlu clusteru A. Například **pr1-ASC-0** hostitele.</span><span class="sxs-lookup"><span data-stu-id="76079-965">Execute the first cluster node option on cluster node A. For example, on the **pr1-ascs-0** host.</span></span>
2.  <span data-ttu-id="76079-966">Ponechat výchozí porty pro vyrovnávání zatížení Azure interní, vyberte:</span><span class="sxs-lookup"><span data-stu-id="76079-966">To keep the default ports for the Azure internal load balancer, select:</span></span>

  * <span data-ttu-id="76079-967">**Systém ABAP**: **Asc** instance číslo **00**</span><span class="sxs-lookup"><span data-stu-id="76079-967">**ABAP system**: **ASCS** instance number **00**</span></span>
  * <span data-ttu-id="76079-968">**Java systému**: **SCS** instance číslo **01**</span><span class="sxs-lookup"><span data-stu-id="76079-968">**Java system**: **SCS** instance number **01**</span></span>
  * <span data-ttu-id="76079-969">**ABAP + Java systému**: **Asc** instance číslo **00** a **SCS** instance číslo **01**</span><span class="sxs-lookup"><span data-stu-id="76079-969">**ABAP+Java system**: **ASCS** instance number **00** and **SCS** instance number **01**</span></span>

  <span data-ttu-id="76079-970">Pokud chcete použít instanci čísla než 00 pro instanci ABAP ASC a 01 pro instanci Java SCS, nejdřív je potřeba změnit Azure interní výchozí Vyrovnávání zatížení pravidel, popsaných v [změňte pravidla pro vyrovnávání zatížení výchozí ASC nebo SCS Nástroje pro vyrovnávání zatížení Azure interní][sap-ha-guide-8.9].</span><span class="sxs-lookup"><span data-stu-id="76079-970">To use instance numbers other than 00 for the ABAP ASCS instance and 01 for the Java SCS instance, first you need to change the Azure internal load balancer default load balancing rules, described in [Change the ASCS/SCS default load balancing rules for the Azure internal load balancer][sap-ha-guide-8.9].</span></span>

<span data-ttu-id="76079-971">Další několik úloh nejsou popsané v dokumentaci standardní instalace SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-971">The next few tasks aren't described in the standard SAP installation documentation.</span></span>

> [!NOTE]
> <span data-ttu-id="76079-972">Dokumentaci k instalaci SAP popisuje postup instalace prvního uzlu clusteru ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="76079-972">The SAP installation documentation describes how to install the first ASCS/SCS cluster node.</span></span>
>
>

#### <span data-ttu-id="76079-973"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Upravit profil SAP ASC nebo SCS instance</span><span class="sxs-lookup"><span data-stu-id="76079-973"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> Modify the SAP profile of the ASCS/SCS instance</span></span>

<span data-ttu-id="76079-974">Je nutné přidat nový parametr profilu.</span><span class="sxs-lookup"><span data-stu-id="76079-974">You need to add a new profile parameter.</span></span> <span data-ttu-id="76079-975">Parametr profil zabrání připojení mezi SAP pracovní procesy a serverem zařazování zavření po nečinnosti příliš dlouho.</span><span class="sxs-lookup"><span data-stu-id="76079-975">The profile parameter prevents connections between SAP work processes and the enqueue server from closing when they are idle for too long.</span></span> <span data-ttu-id="76079-976">Jsme uvedených scénář v [přidat položky registru na obou uzlů clusteru instance SAP ASC nebo SCS][sap-ha-guide-8.11].</span><span class="sxs-lookup"><span data-stu-id="76079-976">We mentioned the problem scenario in [Add registry entries on both cluster nodes of the SAP ASCS/SCS instance][sap-ha-guide-8.11].</span></span> <span data-ttu-id="76079-977">V této části zavedli jsme taky dvě změny některé základní parametry připojení TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="76079-977">In that section, we also introduced two changes to some basic TCP/IP connection parameters.</span></span> <span data-ttu-id="76079-978">V druhém kroku, je nutné nastavit zařadit server k odeslání `keep_alive` signál, aby připojení není dosáhl nástroje pro vyrovnávání zatížení Azure vnitřní limit nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="76079-978">In a second step, you need to set the enqueue server to send a `keep_alive` signal so that the connections don't hit the Azure internal load balancer's idle threshold.</span></span>

<span data-ttu-id="76079-979">Úprava profilu SAP instance ASC nebo SCS:</span><span class="sxs-lookup"><span data-stu-id="76079-979">To modify the SAP profile of the ASCS/SCS instance:</span></span>

1.  <span data-ttu-id="76079-980">Tento parametr profil přidáte do profilu instance SAP ASC nebo SCS:</span><span class="sxs-lookup"><span data-stu-id="76079-980">Add this profile parameter to the SAP ASCS/SCS instance profile:</span></span>

  ```
  enque/encni/set_so_keepalive = true
  ```
  <span data-ttu-id="76079-981">V našem příkladu je cesta:</span><span class="sxs-lookup"><span data-stu-id="76079-981">In our example, the path is:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  <span data-ttu-id="76079-982">Například pro SAP SCS instance profilu a odpovídajících cesta:</span><span class="sxs-lookup"><span data-stu-id="76079-982">For example, to the SAP SCS instance profile and corresponding path:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  <span data-ttu-id="76079-983">Aby se změny projevily, restartujte instanci /SCS ASC SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-983">To apply the changes, restart the SAP ASCS /SCS instance.</span></span>

#### <span data-ttu-id="76079-984"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Přidat port testu</span><span class="sxs-lookup"><span data-stu-id="76079-984"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Add a probe port</span></span>

<span data-ttu-id="76079-985">Pomocí nástroje pro vyrovnávání zatížení interní test funkce usnadňují konfiguraci celý cluster pracovat s Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="76079-985">Use the internal load balancer's probe functionality to make the entire cluster configuration work with Azure Load Balancer.</span></span> <span data-ttu-id="76079-986">Nástroje pro vyrovnávání zatížení Azure interní obvykle distribuuje příchozí zatížení rovnoměrně mezi zúčastněných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="76079-986">The Azure internal load balancer usually distributes the incoming workload equally between participating virtual machines.</span></span> <span data-ttu-id="76079-987">Ale tato funkce nebude pracovat v některé konfigurace clusteru vzhledem k tomu, že pouze jedna instance je aktivní.</span><span class="sxs-lookup"><span data-stu-id="76079-987">However, this won't work in some cluster configurations because only one instance is active.</span></span> <span data-ttu-id="76079-988">Druhá instance je pasivní a nemůže přijímat úlohy.</span><span class="sxs-lookup"><span data-stu-id="76079-988">The other instance is passive and can’t accept any of the workload.</span></span> <span data-ttu-id="76079-989">Funkce kontroly pomáhá při nástroje pro vyrovnávání zatížení Azure interní přiřadí pracovní jenom na aktivní instance.</span><span class="sxs-lookup"><span data-stu-id="76079-989">A probe functionality helps when the Azure internal load balancer assigns work only to an active instance.</span></span> <span data-ttu-id="76079-990">Pomocí funkce testu nástroje pro vyrovnávání zatížení pro vnitřní může zjistit, které instancí jsou aktivní a pak cíle pouze instance se zatížením.</span><span class="sxs-lookup"><span data-stu-id="76079-990">With the probe functionality, the internal load balancer can detect which instances are active, and then target only the instance with the workload.</span></span>

<span data-ttu-id="76079-991">Chcete-li přidat port testu:</span><span class="sxs-lookup"><span data-stu-id="76079-991">To add a probe port:</span></span>

1.  <span data-ttu-id="76079-992">Zkontrolujte aktuální **ProbePort** nastavení spuštěním následujícího příkazu Powershellu.</span><span class="sxs-lookup"><span data-stu-id="76079-992">Check the current **ProbePort** setting by running the following PowerShell command.</span></span> <span data-ttu-id="76079-993">Spouštění ho do jedné z virtuálních počítačů v konfiguraci clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-993">Execute it from within one of the virtual machines in the cluster configuration.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  <span data-ttu-id="76079-994">Definujte port testu.</span><span class="sxs-lookup"><span data-stu-id="76079-994">Define a probe port.</span></span> <span data-ttu-id="76079-995">Výchozí číslo portu testu je **0**.</span><span class="sxs-lookup"><span data-stu-id="76079-995">The default probe port number is **0**.</span></span> <span data-ttu-id="76079-996">V našem příkladu používáme port testu **62000**.</span><span class="sxs-lookup"><span data-stu-id="76079-996">In our example, we use probe port **62000**.</span></span>

  ![Obrázek 58: Port testu konfigurace clusteru je 0. ve výchozím nastavení][sap-ha-guide-figure-3048]

  <span data-ttu-id="76079-998">_**Obrázek 58:** výchozí port test konfigurace clusteru je 0._</span><span class="sxs-lookup"><span data-stu-id="76079-998">_**Figure 58:** The default cluster configuration probe port is 0_</span></span>

  <span data-ttu-id="76079-999">Číslo portu je definována v šablonách SAP Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="76079-999">The port number is defined in SAP Azure Resource Manager templates.</span></span> <span data-ttu-id="76079-1000">Můžete přiřadit číslo portu v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="76079-1000">You can assign the port number in PowerShell.</span></span>

  <span data-ttu-id="76079-1001">Chcete-li nastavit novou hodnotu ProbePort  **SAP <*SID*> IP ** prostředek clusteru, spusťte následující skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="76079-1001">To set a new ProbePort value for the **SAP <*SID*> IP** cluster resource, run the following PowerShell script.</span></span> <span data-ttu-id="76079-1002">Aktualizujte proměnné prostředí PowerShell pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="76079-1002">Update the PowerShell variables for your environment.</span></span> <span data-ttu-id="76079-1003">Po spuštění skriptu budete vyzváni, restartujte skupina clusteru SAP k aktivaci změny.</span><span class="sxs-lookup"><span data-stu-id="76079-1003">After the script runs, you'll be prompted to restart the SAP cluster group to activate the changes.</span></span>

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

  <span data-ttu-id="76079-1004">Po přepnutí  **SAP <*SID*> ** clusteru role online, ověřte, že **ProbePort** nastavena na novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="76079-1004">After you bring the **SAP <*SID*>** cluster role online, verify that **ProbePort** is set to the new value.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![Obrázek 59: Probe port clusteru po nastavení nová hodnota][sap-ha-guide-figure-3049]

  <span data-ttu-id="76079-1006">_**Obrázek 59:** testu port clusteru po nastavení nová hodnota_</span><span class="sxs-lookup"><span data-stu-id="76079-1006">_**Figure 59:** Probe the cluster port after you set the new value_</span></span>

#### <span data-ttu-id="76079-1007"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Otevřete port testu brány firewall systému Windows</span><span class="sxs-lookup"><span data-stu-id="76079-1007"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a> Open the Windows firewall probe port</span></span>

<span data-ttu-id="76079-1008">Budete muset otevřít port testu brány firewall systému Windows na obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-1008">You need to open a Windows firewall probe port on both cluster nodes.</span></span> <span data-ttu-id="76079-1009">Pomocí následujícího skriptu otevřít port testu brány firewall systému Windows.</span><span class="sxs-lookup"><span data-stu-id="76079-1009">Use the following script to open a Windows firewall probe port.</span></span> <span data-ttu-id="76079-1010">Aktualizujte proměnné prostředí PowerShell pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="76079-1010">Update the PowerShell variables for your environment.</span></span>

  ```PowerShell
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

<span data-ttu-id="76079-1011">**ProbePort** je nastaven na **62000**.</span><span class="sxs-lookup"><span data-stu-id="76079-1011">The **ProbePort** is set to **62000**.</span></span> <span data-ttu-id="76079-1012">Teď můžete přístup ke sdílené složce  **\\\ascsha-clsap\sapmnt** z jiných hostitelů, například jako z **ascsha dbas**.</span><span class="sxs-lookup"><span data-stu-id="76079-1012">Now you can access the file share **\\\ascsha-clsap\sapmnt** from other hosts, such as from **ascsha-dbas**.</span></span>

### <span data-ttu-id="76079-1013"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Instalovat instanci databáze</span><span class="sxs-lookup"><span data-stu-id="76079-1013"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Install the database instance</span></span>

<span data-ttu-id="76079-1014">Chcete-li nainstalovat instanci databáze, postupujte podle procesu popsaného v dokumentaci k instalaci SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-1014">To install the database instance, follow the process described in the SAP installation documentation.</span></span>

### <span data-ttu-id="76079-1015"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Instalaci druhého uzlu clusteru</span><span class="sxs-lookup"><span data-stu-id="76079-1015"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> Install the second cluster node</span></span>

<span data-ttu-id="76079-1016">K instalaci druhé clusteru, postupujte podle kroků v Průvodci instalací SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-1016">To install the second cluster, follow the steps in the SAP installation guide.</span></span>

### <span data-ttu-id="76079-1017"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Změnit typ spuštění instance služby Windows YBRAT SAP</span><span class="sxs-lookup"><span data-stu-id="76079-1017"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> Change the start type of the SAP ERS Windows service instance</span></span>

<span data-ttu-id="76079-1018">Změnit typ spuštění služby Windows YBRAT SAP **automaticky (zpožděné spuštění)** na obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="76079-1018">Change the start type of the SAP ERS Windows service to **Automatic (Delayed Start)** on both cluster nodes.</span></span>

![Obrázek 60: Změňte typ služby pro instance SAP YBRAT na zpožděné automaticky][sap-ha-guide-figure-3050]

<span data-ttu-id="76079-1020">_**Obrázek 60:** změnit typ služby pro instance SAP YBRAT pro odložené automatické_</span><span class="sxs-lookup"><span data-stu-id="76079-1020">_**Figure 60:** Change the service type for the SAP ERS instance to delayed automatic_</span></span>

### <span data-ttu-id="76079-1021"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>Instalace serveru primární aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="76079-1021"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> Install the SAP Primary Application Server</span></span>

<span data-ttu-id="76079-1022">Nainstalovat instanci serveru primární aplikace (Pa) <*SID*> - di - 0 na virtuálním počítači, který jste určili pro hostování Pa ADRESAMI.</span><span class="sxs-lookup"><span data-stu-id="76079-1022">Install the Primary Application Server (PAS) instance <*SID*>-di-0 on the virtual machine that you've designated to host the PAS.</span></span> <span data-ttu-id="76079-1023">Neexistují žádné závislosti na Azure nebo DataKeeper specifická nastavení.</span><span class="sxs-lookup"><span data-stu-id="76079-1023">There are no dependencies on Azure or DataKeeper-specific settings.</span></span>

### <span data-ttu-id="76079-1024"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>Instalace serveru SAP další aplikace</span><span class="sxs-lookup"><span data-stu-id="76079-1024"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> Install the SAP Additional Application Server</span></span>

<span data-ttu-id="76079-1025">Nainstalujte SAP serveru pro další aplikace (AAS) na všechny virtuální počítače, které jste označili k hostování instance aplikačního serveru SAP.</span><span class="sxs-lookup"><span data-stu-id="76079-1025">Install an SAP Additional Application Server (AAS) on all the virtual machines that you've designated to host an SAP Application Server instance.</span></span> <span data-ttu-id="76079-1026">Například na <*SID*> - di - 1 pro <*SID*> - di -&lt;n&gt;.</span><span class="sxs-lookup"><span data-stu-id="76079-1026">For example, on <*SID*>-di-1 to <*SID*>-di-&lt;n&gt;.</span></span>

> [!NOTE]
> <span data-ttu-id="76079-1027">To dokončí instalaci systému SAP NetWeaver vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="76079-1027">This finishes the installation of a high-availability SAP NetWeaver system.</span></span> <span data-ttu-id="76079-1028">Pokračujte dále testování převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="76079-1028">Next, proceed with failover testing.</span></span>
>


## <span data-ttu-id="76079-1029"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Testovací převzetí služeb při selhání SAP ASC nebo SCS instance a SIOS replikace</span><span class="sxs-lookup"><span data-stu-id="76079-1029"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> Test the SAP ASCS/SCS instance failover and SIOS replication</span></span>
<span data-ttu-id="76079-1030">Je snadné pro testování a monitorování SAP ASC nebo SCS instance převzetí služeb při selhání a replikaci disku SIOS pomocí nástroje Správce clusteru převzetí služeb při selhání a s DataKeeper správy a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="76079-1030">It's easy to test and monitor an SAP ASCS/SCS instance failover and SIOS disk replication by using Failover Cluster Manager and the SIOS DataKeeper Management and Configuration tool.</span></span>

### <span data-ttu-id="76079-1031"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>V uzlu clusteru A je spuštěna instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="76079-1031"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> SAP ASCS/SCS instance is running on cluster node A</span></span>

<span data-ttu-id="76079-1032">**SAP PR1** skupina clusteru běží na uzlu clusteru A. Například na **pr1-ASC-0**.</span><span class="sxs-lookup"><span data-stu-id="76079-1032">The **SAP PR1** cluster group is running on cluster node A. For example, on **pr1-ascs-0**.</span></span> <span data-ttu-id="76079-1033">Přiřadit sdílené diskové jednotce S, která je součástí systému **SAP PR1** skupina clusteru a který instance ASC nebo SCS používá A. uzlu v clusteru</span><span class="sxs-lookup"><span data-stu-id="76079-1033">Assign the shared disk drive S, which is part of the **SAP PR1** cluster group, and which the ASCS/SCS instance uses, to cluster node A.</span></span>

![Obrázek 61: Správce clusteru převzetí služeb při selhání: Skupina clusteru SAP < SID > běží na uzlu clusteru A][sap-ha-guide-figure-5000]

<span data-ttu-id="76079-1035">_**Obrázek 61:** Správce clusteru převzetí služeb při selhání: SAP <*SID*> Skupina clusteru běží na uzlu clusteru A_</span><span class="sxs-lookup"><span data-stu-id="76079-1035">_**Figure 61:** Failover Cluster Manager: The SAP <*SID*> cluster group is running on cluster node A_</span></span>

<span data-ttu-id="76079-1036">Nástroje pro správu s DataKeeper a konfiguraci uvidíte, že dat sdíleného disku je synchronně replikovat ze zdrojového svazku jednotky S na uzlu clusteru A jednotky cílový svazek S na uzlu clusteru B. Například se replikují z **pr1-ASC-0 [10.0.0.40]** k **pr1-ASC-1 [10.0.0.41]**.</span><span class="sxs-lookup"><span data-stu-id="76079-1036">In the SIOS DataKeeper Management and Configuration tool, you can see that the shared disk data is synchronously replicated from the source volume drive S on cluster node A to the target volume drive S on cluster node B. For example, it's replicated from **pr1-ascs-0 [10.0.0.40]** to **pr1-ascs-1 [10.0.0.41]**.</span></span>

![Obrázek 62: V DataKeeper s replikovat místní svazek z uzlu clusteru A k uzlu clusteru B][sap-ha-guide-figure-5001]

<span data-ttu-id="76079-1038">_**Obrázek 62:** v DataKeeper s replikovat místní svazek z uzlu clusteru A k uzlu clusteru B_</span><span class="sxs-lookup"><span data-stu-id="76079-1038">_**Figure 62:** In SIOS DataKeeper, replicate the local volume from cluster node A to cluster node B_</span></span>

### <span data-ttu-id="76079-1039"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Převzetí služeb při selhání z uzlu A k uzlu B</span><span class="sxs-lookup"><span data-stu-id="76079-1039"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Failover from node A to node B</span></span>

1.  <span data-ttu-id="76079-1040">Vyberte jednu z těchto možností k zahájení převzetí služeb při selhání SAP <*SID*> Skupina clusteru z uzlu clusteru A k uzlu clusteru B:</span><span class="sxs-lookup"><span data-stu-id="76079-1040">Choose one of these options to initiate a failover of the SAP <*SID*> cluster group from cluster node A to cluster node B:</span></span>
  - <span data-ttu-id="76079-1041">Pomocí Správce clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="76079-1041">Use Failover Cluster Manager</span></span>  
  - <span data-ttu-id="76079-1042">Pomocí prostředí PowerShell pro Cluster převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="76079-1042">Use Failover Cluster PowerShell</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  <span data-ttu-id="76079-1043">Restartovat A uzlu clusteru v rámci hostovaného operačního systému Windows (tím se vyvolá automatické převzetí služeb při selhání z SAP <*SID*> Skupina clusteru z uzlu A k uzlu B).</span><span class="sxs-lookup"><span data-stu-id="76079-1043">Restart cluster node A within the Windows guest operating system (this initiates an automatic failover of the SAP <*SID*> cluster group from node A to node B).</span></span>  
3.  <span data-ttu-id="76079-1044">Restartovat uzel A cluster z portálu Azure (tím se vyvolá automatické převzetí služeb při selhání z SAP <*SID*> Skupina clusteru z uzlu A k uzlu B).</span><span class="sxs-lookup"><span data-stu-id="76079-1044">Restart cluster node A from the Azure portal (this initiates an automatic failover of the SAP <*SID*> cluster group from node A to node B).</span></span>  
4.  <span data-ttu-id="76079-1045">Restartujte počítač A uzlu clusteru pomocí prostředí Azure PowerShell (tím se vyvolá automatické převzetí služeb při selhání z SAP <*SID*> Skupina clusteru z uzlu A k uzlu B).</span><span class="sxs-lookup"><span data-stu-id="76079-1045">Restart cluster node A by using Azure PowerShell (this initiates an automatic failover of the SAP <*SID*> cluster group from node A to node B).</span></span>

  <span data-ttu-id="76079-1046">Po převzetí služeb při selhání, SAP <*SID*> Skupina clusteru běží na uzlu clusteru B. Například je spuštěn na **pr1-ASC-1**.</span><span class="sxs-lookup"><span data-stu-id="76079-1046">After failover, the SAP <*SID*> cluster group is running on cluster node B. For example, it's running on **pr1-ascs-1**.</span></span>

  ![Obrázek 63: Ve Správci clusteru převzetí služeb při selhání skupiny clusteru SAP < SID > běží na uzlu clusteru B][sap-ha-guide-figure-5002]

  <span data-ttu-id="76079-1048">_**Obrázek 63**: ve Správci clusteru převzetí služeb při selhání, SAP <*SID*> Skupina clusteru běží na uzlu clusteru B_</span><span class="sxs-lookup"><span data-stu-id="76079-1048">_**Figure 63**: In Failover Cluster Manager, the SAP <*SID*> cluster group is running on cluster node B_</span></span>

  <span data-ttu-id="76079-1049">Sdílený disk je teď připojené v clusteru uzlu B. s DataKeeper se replikuje data ze zdrojového svazku jednotky S na uzlu clusteru B na cílový svazek jednotky S na uzlu clusteru A. Například je replikace z **pr1-ASC-1 [10.0.0.41]** k **pr1-ASC-0 [10.0.0.40]**.</span><span class="sxs-lookup"><span data-stu-id="76079-1049">The shared disk is now mounted on cluster node B. SIOS DataKeeper is replicating data from source volume drive S on cluster node B to target volume drive S on cluster node A. For example, it's replicating from **pr1-ascs-1 [10.0.0.41]** to **pr1-ascs-0 [10.0.0.40]**.</span></span>

  ![Obrázek 64: SIOS DataKeeper z uzlu clusteru B A uzlu v clusteru se replikuje místního svazku][sap-ha-guide-figure-5003]

  <span data-ttu-id="76079-1051">_**Obrázek 64:** s DataKeeper replikuje místní svazek z uzlu clusteru B A uzlu v clusteru_</span><span class="sxs-lookup"><span data-stu-id="76079-1051">_**Figure 64:** SIOS DataKeeper replicates the local volume from cluster node B to cluster node A_</span></span>
