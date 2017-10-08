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
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 662dd793390d7f6138b160ed86259d13391336aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-high-availability-for-sap-netweaver"></a><span data-ttu-id="48ca0-103">Azure vysoké dostupnosti virtuálních počítačů pro SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="48ca0-103">Azure Virtual Machines high availability for SAP NetWeaver</span></span>

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

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



<span data-ttu-id="48ca0-109">Virtuální počítače Azure je hello řešení pro organizace, které potřebují výpočty, úložiště a síťové prostředky ve minimálního čas a bez zdlouhavé nákup cykly.</span><span class="sxs-lookup"><span data-stu-id="48ca0-109">Azure Virtual Machines is hello solution for organizations that need compute, storage, and network resources, in minimal time, and without lengthy procurement cycles.</span></span> <span data-ttu-id="48ca0-110">Můžete vytvořit virtuální počítače Azure toodeploy classic aplikace jako na základě SAP NetWeaver ABAP, Java a ABAP + Java zásobníku.</span><span class="sxs-lookup"><span data-stu-id="48ca0-110">You can use Azure Virtual Machines toodeploy classic applications like SAP NetWeaver-based ABAP, Java, and an ABAP+Java stack.</span></span> <span data-ttu-id="48ca0-111">Rozšiřte spolehlivosti a dostupnosti bez dalších místních prostředků.</span><span class="sxs-lookup"><span data-stu-id="48ca0-111">Extend reliability and availability without additional on-premises resources.</span></span> <span data-ttu-id="48ca0-112">Virtuální počítače Azure podporuje připojení mezi různými místy, takže virtuální počítače Azure můžete integrovat do místní domény vaší organizace, privátních cloudů a šířku systému SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-112">Azure Virtual Machines supports cross-premises connectivity, so you can integrate Azure Virtual Machines into your organization's on-premises domains, private clouds, and SAP system landscape.</span></span>

<span data-ttu-id="48ca0-113">V tomto článku jsme zahrnují hello kroky můžete podniknout toodeploy SAP systémů s vysokou dostupností v Azure pomocí modelu nasazení Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-113">In this article, we cover hello steps that you can take toodeploy high-availability SAP systems in Azure by using hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="48ca0-114">Můžeme vás provede procesem tyto hlavní úlohy:</span><span class="sxs-lookup"><span data-stu-id="48ca0-114">We walk you through these major tasks:</span></span>

* <span data-ttu-id="48ca0-115">Najde hello správné příručky SAP poznámky a instalace, uvedené v hello [prostředky] [ sap-ha-guide-2] části.</span><span class="sxs-lookup"><span data-stu-id="48ca0-115">Find hello right SAP Notes and installation guides, listed in hello [Resources][sap-ha-guide-2] section.</span></span> <span data-ttu-id="48ca0-116">Tento článek doplňuje SAP instalace dokumentace a poznámky k SAP, které jsou hello primární prostředky, které vám mohou pomoci instalace a nasazení SAP software na specifické platformy.</span><span class="sxs-lookup"><span data-stu-id="48ca0-116">This article complements SAP installation documentation and SAP Notes, which are hello primary resources that can help you install and deploy SAP software on specific platforms.</span></span>
* <span data-ttu-id="48ca0-117">Přečtěte si hello rozdíly mezi hello modelu nasazení Azure Resource Manager a modelu nasazení Azure classic hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-117">Learn hello differences between hello Azure Resource Manager deployment model and hello Azure classic deployment model.</span></span>
* <span data-ttu-id="48ca0-118">Další informace o Windows Server Failover Clustering režimů kvora, takže můžete vybrat hello model, který je nejvhodnější pro vaše nasazení Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-118">Learn about Windows Server Failover Clustering quorum modes, so you can select hello model that is right for your Azure deployment.</span></span>
* <span data-ttu-id="48ca0-119">Další informace o Windows Server Failover Clustering sdíleného úložiště do služby Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-119">Learn about Windows Server Failover Clustering shared storage in Azure services.</span></span>
* <span data-ttu-id="48ca0-120">Informace o ochraně toohelp jeden bod o selhání součásti jako Advanced obchodní aplikace programování (ABAP) SAP centrální služby (ASC) / SAP centrální služby (SCS) a databázové systémy (databázového systému) a redundantní komponenty, například SAP Aplikační Server v Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-120">Learn how toohelp protect single-point-of-failure components like Advanced Business Application Programming (ABAP) SAP Central Services (ASCS)/SAP Central Services (SCS) and database management systems (DBMS), and redundant components like SAP Application Server, in Azure.</span></span>
* <span data-ttu-id="48ca0-121">Postupujte podle podrobný příklad k instalaci a konfiguraci systému SAP vysoké dostupnosti v clusteru Windows Server Failover Clustering v Azure pomocí Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="48ca0-121">Follow a step-by-step example of an installation and configuration of a high-availability SAP system in a Windows Server Failover Clustering cluster in Azure by using Azure Resource Manager.</span></span>
* <span data-ttu-id="48ca0-122">Další informace o další kroky požadované toouse Windows Server Failover Clustering v Azure, ale které nejsou potřebné v místním nasazení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-122">Learn about additional steps required toouse Windows Server Failover Clustering in Azure, but which are not needed in an on-premises deployment.</span></span>

<span data-ttu-id="48ca0-123">toosimplify nasazení a konfigurace, v tomto článku používáme hello SAP třívrstvá vysoké dostupnosti správce prostředků šablony.</span><span class="sxs-lookup"><span data-stu-id="48ca0-123">toosimplify deployment and configuration, in this article, we use hello SAP three-tier high-availability Resource Manager templates.</span></span> <span data-ttu-id="48ca0-124">šablony Hello automatizovat nasazení hello celé infrastruktury, které potřebujete pro SAP systém vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="48ca0-124">hello templates automate deployment of hello entire infrastructure that you need for a high-availability SAP system.</span></span> <span data-ttu-id="48ca0-125">Infrastruktura Hello také podporuje nastavení velikosti SAP aplikace výkonu standardní (protokoly SAP) systému SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-125">hello infrastructure also supports SAP Application Performance Standard (SAPS) sizing of your SAP system.</span></span>

## <span data-ttu-id="48ca0-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="48ca0-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a> Prerequisites</span></span>
<span data-ttu-id="48ca0-127">Než začnete, ujistěte se, že splňujete požadavky hello, které jsou popsány v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-127">Before you start, make sure that you meet hello prerequisites that are described in hello following sections.</span></span> <span data-ttu-id="48ca0-128">Být také, zda toocheck všechny prostředky uvedené v hello [prostředky] [ sap-ha-guide-2] části.</span><span class="sxs-lookup"><span data-stu-id="48ca0-128">Also, be sure toocheck all resources listed in hello [Resources][sap-ha-guide-2] section.</span></span>

<span data-ttu-id="48ca0-129">V tomto článku používáme šablon Azure Resource Manageru pro [třívrstvá SAP NetWeaver pomocí disků spravované](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md/).</span><span class="sxs-lookup"><span data-stu-id="48ca0-129">In this article, we use Azure Resource Manager templates for [three-tier SAP NetWeaver using Managed Disks](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md/).</span></span> <span data-ttu-id="48ca0-130">Užitečné Přehled šablon naleznete v tématu [šablon SAP Azure Resource Manageru](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span><span class="sxs-lookup"><span data-stu-id="48ca0-130">For a helpful overview of templates, see [SAP Azure Resource Manager templates](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span></span>

## <span data-ttu-id="48ca0-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Prostředky</span><span class="sxs-lookup"><span data-stu-id="48ca0-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a> Resources</span></span>
<span data-ttu-id="48ca0-132">Tyto články popisuje nasazení SAP v Azure:</span><span class="sxs-lookup"><span data-stu-id="48ca0-132">These articles cover SAP deployments in Azure:</span></span>

* <span data-ttu-id="48ca0-133">[Azure virtuálních počítačů, plánování a implementace pro SAP NetWeaver][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="48ca0-133">[Azure Virtual Machines planning and implementation for SAP NetWeaver][planning-guide]</span></span>
* <span data-ttu-id="48ca0-134">[Nasazení virtuálních počítačů Azure pro SAP NetWeaver][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="48ca0-134">[Azure Virtual Machines deployment for SAP NetWeaver][deployment-guide]</span></span>
* <span data-ttu-id="48ca0-135">[Nasazení virtuálních počítačů databázového systému Azure pro SAP NetWeaver][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="48ca0-135">[Azure Virtual Machines DBMS deployment for SAP NetWeaver][dbms-guide]</span></span>
* <span data-ttu-id="48ca0-136">[Azure vysoké dostupnosti virtuálních počítačů pro SAP NetWeaver (Tato příručka)][sap-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="48ca0-136">[Azure Virtual Machines high availability for SAP NetWeaver (this guide)][sap-ha-guide]</span></span>

> [!NOTE]
> <span data-ttu-id="48ca0-137">Kdykoli je to možné, jsme získáte na odkazu toohello odkazující Instalační příručka (viz hello [SAP instalace příručky][sap-installation-guides]).</span><span class="sxs-lookup"><span data-stu-id="48ca0-137">Whenever possible, we give you a link toohello referring SAP installation guide (see hello [SAP installation guides][sap-installation-guides]).</span></span> <span data-ttu-id="48ca0-138">Požadavky a informace o procesu instalace hello, jeho, které provede instalaci s vhodné hello tooread SAP NetWeaver pečlivě.</span><span class="sxs-lookup"><span data-stu-id="48ca0-138">For prerequisites and information about hello installation process, it's a good idea tooread hello SAP NetWeaver installation guides carefully.</span></span> <span data-ttu-id="48ca0-139">Tento článek se týká pouze konkrétní úlohy pro počítače se systémem SAP NetWeaver, které můžete použít s virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-139">This article covers only specific tasks for SAP NetWeaver-based systems that you can use with Azure Virtual Machines.</span></span>
>
>

<span data-ttu-id="48ca0-140">Tyto poznámky SAP jsou související toohello tématem SAP v Azure:</span><span class="sxs-lookup"><span data-stu-id="48ca0-140">These SAP Notes are related toohello topic of SAP in Azure:</span></span>

| <span data-ttu-id="48ca0-141">Poznámka: číslo</span><span class="sxs-lookup"><span data-stu-id="48ca0-141">Note number</span></span> | <span data-ttu-id="48ca0-142">Název</span><span class="sxs-lookup"><span data-stu-id="48ca0-142">Title</span></span> |
| --- | --- |
| <span data-ttu-id="48ca0-143">[1928533]</span><span class="sxs-lookup"><span data-stu-id="48ca0-143">[1928533]</span></span> |<span data-ttu-id="48ca0-144">Aplikace SAP v Azure: podporované produkty a velikosti</span><span class="sxs-lookup"><span data-stu-id="48ca0-144">SAP Applications on Azure: Supported Products and Sizing</span></span> |
| <span data-ttu-id="48ca0-145">[2015553]</span><span class="sxs-lookup"><span data-stu-id="48ca0-145">[2015553]</span></span> |<span data-ttu-id="48ca0-146">SAP na platformě Microsoft Azure: podporovat požadavky</span><span class="sxs-lookup"><span data-stu-id="48ca0-146">SAP on Microsoft Azure: Support Prerequisites</span></span> |
| <span data-ttu-id="48ca0-147">[1999351]</span><span class="sxs-lookup"><span data-stu-id="48ca0-147">[1999351]</span></span> |<span data-ttu-id="48ca0-148">Rozšířené monitorování Azure pro SAP</span><span class="sxs-lookup"><span data-stu-id="48ca0-148">Enhanced Azure Monitoring for SAP</span></span> |
| <span data-ttu-id="48ca0-149">[2178632]</span><span class="sxs-lookup"><span data-stu-id="48ca0-149">[2178632]</span></span> |<span data-ttu-id="48ca0-150">Klíč monitorování metriky pro SAP na platformě Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="48ca0-150">Key Monitoring Metrics for SAP on Microsoft Azure</span></span> |
| <span data-ttu-id="48ca0-151">[1999351]</span><span class="sxs-lookup"><span data-stu-id="48ca0-151">[1999351]</span></span> |<span data-ttu-id="48ca0-152">Virtualizace v systému Windows: rozšířené monitorování</span><span class="sxs-lookup"><span data-stu-id="48ca0-152">Virtualization on Windows: Enhanced Monitoring</span></span> |
| <span data-ttu-id="48ca0-153">[2243692]</span><span class="sxs-lookup"><span data-stu-id="48ca0-153">[2243692]</span></span> |<span data-ttu-id="48ca0-154">Používání úložiště Azure Premium SSD pro instanci databázového systému SAP</span><span class="sxs-lookup"><span data-stu-id="48ca0-154">Use of Azure Premium SSD Storage for SAP DBMS Instance</span></span> |

<span data-ttu-id="48ca0-155">Další informace o hello [omezení předplatná Azure][azure-subscription-service-limits-subscription], včetně obecné výchozí omezení a maximální omezení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-155">Learn more about hello [limitations of Azure subscriptions][azure-subscription-service-limits-subscription], including general default limitations and maximum limitations.</span></span>

## <span data-ttu-id="48ca0-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Vysoká dostupnost SAP s Azure Resource Manager a modelu nasazení Azure classic hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>High-availability SAP with Azure Resource Manager vs. hello Azure classic deployment model</span></span>
<span data-ttu-id="48ca0-157">Hello Azure Resource Manager a modelech nasazení Azure classic se liší v hello následující oblasti:</span><span class="sxs-lookup"><span data-stu-id="48ca0-157">hello Azure Resource Manager and Azure classic deployment models are different in hello following areas:</span></span>

- <span data-ttu-id="48ca0-158">Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="48ca0-158">Resource groups</span></span>
- <span data-ttu-id="48ca0-159">Azure interní načíst vyrovnávání závislost na skupinu prostředků Azure hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-159">Azure internal load balancer dependency on hello Azure resource group</span></span>
- <span data-ttu-id="48ca0-160">Podpora pro scénáře více SID SAP</span><span class="sxs-lookup"><span data-stu-id="48ca0-160">Support for SAP multi-SID scenarios</span></span>

### <span data-ttu-id="48ca0-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="48ca0-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a> Resource groups</span></span>
<span data-ttu-id="48ca0-162">V Azure Resource Manager, můžete použít toomanage skupiny prostředků všechny prostředky aplikace hello ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-162">In Azure Resource Manager, you can use resource groups toomanage all hello application resources in your Azure subscription.</span></span> <span data-ttu-id="48ca0-163">Integrovaný přístup, ve skupině prostředků, všechny prostředky mají hello stejný životní cyklus.</span><span class="sxs-lookup"><span data-stu-id="48ca0-163">An integrated approach, in a resource group, all resources have hello same life cycle.</span></span> <span data-ttu-id="48ca0-164">Například všechny prostředky jsou vytvořené na hello stejný čas a že jsou při hello odstraněny stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="48ca0-164">For example, all resources are created at hello same time and they are deleted at hello same time.</span></span> <span data-ttu-id="48ca0-165">Další informace o [skupinách prostředků](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="48ca0-165">Learn more about [resource groups](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>

### <span data-ttu-id="48ca0-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Azure interní načíst vyrovnávání závislost na skupinu prostředků Azure hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a> Azure internal load balancer dependency on hello Azure resource group</span></span>

<span data-ttu-id="48ca0-167">V modelu nasazení Azure classic hello je závislost mezi hello Azure interní nástroj pro vyrovnávání zatížení (služba Vyrovnávání zatížení Azure) a hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="48ca0-167">In hello Azure classic deployment model, there is a dependency between hello Azure internal load balancer (Azure Load Balancer service) and hello cloud service.</span></span> <span data-ttu-id="48ca0-168">Každý nástroj pro vyrovnávání zatížení interní musí jeden cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="48ca0-168">Every internal load balancer needs one cloud service.</span></span>

<span data-ttu-id="48ca0-169">V Azure Resource Manager, každý prostředků Azure potřebuje toobe umístit do skupiny prostředků Azure, a to je platný pro vyrovnávání zatížení Azure také.</span><span class="sxs-lookup"><span data-stu-id="48ca0-169">In Azure Resource Manager, every Azure resource needs toobe placed into an Azure resource group, and this is valid for Azure Load Balancer as well.</span></span> <span data-ttu-id="48ca0-170">Ale skupina nutné toohave jeden prostředek Azure za nástroj pro vyrovnávání zatížení Azure, například jedna skupina prostředků Azure může obsahovat více Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-170">However, there is no need toohave one Azure resource group per Azure load balancer, e.g. one Azure resource group can contain multiple Azure Load Balancers.</span></span> <span data-ttu-id="48ca0-171">Hello prostředí je jednodušší a flexibilnější.</span><span class="sxs-lookup"><span data-stu-id="48ca0-171">hello environment is simpler and more flexible.</span></span> 

### <a name="support-for-sap-multi-sid-scenarios"></a><span data-ttu-id="48ca0-172">Podpora pro scénáře více SID SAP</span><span class="sxs-lookup"><span data-stu-id="48ca0-172">Support for SAP multi-SID scenarios</span></span>

<span data-ttu-id="48ca0-173">V Azure Resource Manager, můžete nainstalovat víc SAP systému identifikátor (SID) ASC nebo SCS instancí v jednom clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-173">In Azure Resource Manager, you can install multiple SAP system identifier (SID) ASCS/SCS instances in one cluster.</span></span> <span data-ttu-id="48ca0-174">SID více instancí je možné, protože obsahuje podporu pro více IP adres pro každý nástroj pro vyrovnávání zatížení Azure interní.</span><span class="sxs-lookup"><span data-stu-id="48ca0-174">Multi-SID instances are possible because of support for multiple IP addresses for each Azure internal load balancer.</span></span>

<span data-ttu-id="48ca0-175">toouse hello modelu nasazení Azure classic, postupujte podle hello postupů popsaných v [SAP NetWeaver v Azure: instancí clusteringu ASC nebo SCS SAP pomocí služby Windows Server Failover Clustering v Azure pomocí s DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).</span><span class="sxs-lookup"><span data-stu-id="48ca0-175">toouse hello Azure classic deployment model, follow hello procedures described in [SAP NetWeaver in Azure: Clustering SAP ASCS/SCS instances by using Windows Server Failover Clustering in Azure with SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="48ca0-176">Důrazně doporučujeme, abyste používali hello modelu nasazení Azure Resource Manager pro SAP instalací.</span><span class="sxs-lookup"><span data-stu-id="48ca0-176">We strongly recommend that you use hello Azure Resource Manager deployment model for your SAP installations.</span></span> <span data-ttu-id="48ca0-177">Nabízí spoustu výhod, které nejsou k dispozici v modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-177">It offers many benefits that are not available in hello classic deployment model.</span></span> <span data-ttu-id="48ca0-178">Další informace o Azure [modely nasazení][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span><span class="sxs-lookup"><span data-stu-id="48ca0-178">Learn more about Azure [deployment models][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span></span>   
>
>

## <span data-ttu-id="48ca0-179"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server Failover Clustering</span><span class="sxs-lookup"><span data-stu-id="48ca0-179"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a> Windows Server Failover Clustering</span></span>
<span data-ttu-id="48ca0-180">Windows Server Failover Clustering je hello foundation instalace SAP ASC nebo SCS vysokou dostupnost a databázového systému v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="48ca0-180">Windows Server Failover Clustering is hello foundation of a high-availability SAP ASCS/SCS installation and DBMS in Windows.</span></span>

<span data-ttu-id="48ca0-181">Cluster s podporou převzetí služeb při selhání je skupina 1 + n nezávislých serverů (uzlů) které tooincrease hello dostupnosti aplikací a služeb vzájemně spolupracují.</span><span class="sxs-lookup"><span data-stu-id="48ca0-181">A failover cluster is a group of 1+n independent servers (nodes) that work together tooincrease hello availability of applications and services.</span></span> <span data-ttu-id="48ca0-182">Pokud dojde k selhání uzlu, Windows Server Failover Clustering vypočítá hello počet chyb, které můžou nastat při zachování pořádku clusteru tooprovide aplikací a služeb.</span><span class="sxs-lookup"><span data-stu-id="48ca0-182">If a node failure occurs, Windows Server Failover Clustering calculates hello number of failures that can occur while maintaining a healthy cluster tooprovide applications and services.</span></span> <span data-ttu-id="48ca0-183">Můžete se z různých kvora režimy tooachieve převzetí služeb clusteringu.</span><span class="sxs-lookup"><span data-stu-id="48ca0-183">You can choose from different quorum modes tooachieve failover clustering.</span></span>

### <span data-ttu-id="48ca0-184"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Režim kvora</span><span class="sxs-lookup"><span data-stu-id="48ca0-184"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a> Quorum modes</span></span>
<span data-ttu-id="48ca0-185">Při použití služby Windows Server Failover Clustering, můžete vybrat ze čtyř režimů kvora:</span><span class="sxs-lookup"><span data-stu-id="48ca0-185">You can choose from four quorum modes when you use Windows Server Failover Clustering:</span></span>

* <span data-ttu-id="48ca0-186">**Většina uzlů**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-186">**Node Majority**.</span></span> <span data-ttu-id="48ca0-187">Každý uzel clusteru hello hlasovat.</span><span class="sxs-lookup"><span data-stu-id="48ca0-187">Each node of hello cluster can vote.</span></span> <span data-ttu-id="48ca0-188">Hello clusteru funguje jenom s většinou hlasů, který je s víc než poloviny hello hlasy.</span><span class="sxs-lookup"><span data-stu-id="48ca0-188">hello cluster functions only with a majority of votes, that is, with more than half hello votes.</span></span> <span data-ttu-id="48ca0-189">Doporučujeme tuto možnost pro clustery, které mají lichý počet uzlů.</span><span class="sxs-lookup"><span data-stu-id="48ca0-189">We recommend this option for clusters that have an uneven number of nodes.</span></span> <span data-ttu-id="48ca0-190">Například může selhat tři uzly v clusteru s podporou sedmi a hello clusteru nepřesahuje dosahuje většině a pokračuje toorun.</span><span class="sxs-lookup"><span data-stu-id="48ca0-190">For example, three nodes in a seven-node cluster can fail, and hello cluster stills achieves a majority and continues toorun.</span></span>  
* <span data-ttu-id="48ca0-191">**Většina uzlů a disků**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-191">**Node and Disk Majority**.</span></span> <span data-ttu-id="48ca0-192">Každý uzel a určený disk (disk s kopií clusteru) v úložišti clusteru hello hlasovat když jsou k dispozici a komunikace.</span><span class="sxs-lookup"><span data-stu-id="48ca0-192">Each node and a designated disk (a disk witness) in hello cluster storage can vote when they are available and in communication.</span></span> <span data-ttu-id="48ca0-193">Hello clusteru funguje jenom s většinou hello hlasů, tedy s víc než poloviny hello hlasy.</span><span class="sxs-lookup"><span data-stu-id="48ca0-193">hello cluster functions only with a majority of hello votes, that is, with more than half hello votes.</span></span> <span data-ttu-id="48ca0-194">Tento režim má smysl v prostředí clusteru s sudé číslo uzlů.</span><span class="sxs-lookup"><span data-stu-id="48ca0-194">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="48ca0-195">Pokud poloviční hello uzlů a disků hello jsou online, hello cluster zůstane v dobrém stavu.</span><span class="sxs-lookup"><span data-stu-id="48ca0-195">If half hello nodes and hello disk are online, hello cluster remains in a healthy state.</span></span>
* <span data-ttu-id="48ca0-196">**Uzel a sdílených složek většina**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-196">**Node and File Share Majority**.</span></span> <span data-ttu-id="48ca0-197">Každý uzel, plus určené sdílené složky (soubor určující sdílenou složku), který hello správce vytvoří hlasovat, bez ohledu na to, jestli jsou k dispozici hello uzlů a sdílení souborů a v komunikaci.</span><span class="sxs-lookup"><span data-stu-id="48ca0-197">Each node plus a designated file share (a file share witness) that hello administrator creates can vote, regardless of whether hello nodes and file share are available and in communication.</span></span> <span data-ttu-id="48ca0-198">Hello clusteru funguje jenom s většinou hello hlasů, tedy s víc než poloviny hello hlasy.</span><span class="sxs-lookup"><span data-stu-id="48ca0-198">hello cluster functions only with a majority of hello votes, that is, with more than half hello votes.</span></span> <span data-ttu-id="48ca0-199">Tento režim má smysl v prostředí clusteru s sudé číslo uzlů.</span><span class="sxs-lookup"><span data-stu-id="48ca0-199">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="48ca0-200">Je podobné toohello uzlu a většina disku režim, ale používá určující sdílené složky namísto disku s kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-200">It's similar toohello Node and Disk Majority mode, but it uses a witness file share instead of a witness disk.</span></span> <span data-ttu-id="48ca0-201">Tento režim je snadno tooimplement, ale pokud sdílená složka hello samotné není vysoce dostupný, může být jediný bod selhání.</span><span class="sxs-lookup"><span data-stu-id="48ca0-201">This mode is easy tooimplement, but if hello file share itself is not highly available, it might become a single point of failure.</span></span>
* <span data-ttu-id="48ca0-202">**Bez většiny: Na disku jenom**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-202">**No Majority: Disk Only**.</span></span> <span data-ttu-id="48ca0-203">Hello cluster má kvorum, pokud je k dispozici a komunikace s konkrétním diskem v úložišti clusteru hello jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="48ca0-203">hello cluster has a quorum if one node is available and in communication with a specific disk in hello cluster storage.</span></span> <span data-ttu-id="48ca0-204">Pouze hello uzlů, které jsou také v komunikaci s tento disk může připojit hello cluster.</span><span class="sxs-lookup"><span data-stu-id="48ca0-204">Only hello nodes that also are in communication with that disk can join hello cluster.</span></span> <span data-ttu-id="48ca0-205">Doporučujeme vám, nepoužívejte tento režim.</span><span class="sxs-lookup"><span data-stu-id="48ca0-205">We recommend that you do not use this mode.</span></span>
 

## <span data-ttu-id="48ca0-206"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server převzetí služeb při selhání Clustering na místě</span><span class="sxs-lookup"><span data-stu-id="48ca0-206"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a> Windows Server Failover Clustering on-premises</span></span>
<span data-ttu-id="48ca0-207">Obrázek 1 ukazuje dva uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-207">Figure 1 shows a cluster of two nodes.</span></span> <span data-ttu-id="48ca0-208">Pokud hello síťové připojení mezi uzly hello selže a oba uzly zůstat nahoru a spuštěna, kvora disku nebo sdílené složky Určuje, který uzel bude pokračovat, aplikacím a službám tooprovide hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-208">If hello network connection between hello nodes fails and both nodes stay up and running, a quorum disk or file share determines which node will continue tooprovide hello cluster's applications and services.</span></span> <span data-ttu-id="48ca0-209">Hello uzlu, který má přístup k disku kvora toohello nebo sdílených složek je hello uzlu, který zajistí, že služby pokračovat.</span><span class="sxs-lookup"><span data-stu-id="48ca0-209">hello node that has access toohello quorum disk or file share is hello node that ensures that services continue.</span></span>

<span data-ttu-id="48ca0-210">Protože tento příklad používá dva uzly clusteru, použijeme hello uzlu a režim kvora Majority sdílené složky souborů.</span><span class="sxs-lookup"><span data-stu-id="48ca0-210">Because this example uses a two-node cluster, we use hello Node and File Share Majority quorum mode.</span></span> <span data-ttu-id="48ca0-211">Většina disku a Hello uzlu je taky platná možnost.</span><span class="sxs-lookup"><span data-stu-id="48ca0-211">hello Node and Disk Majority also is a valid option.</span></span> <span data-ttu-id="48ca0-212">V produkčním prostředí doporučujeme použít disk kvora.</span><span class="sxs-lookup"><span data-stu-id="48ca0-212">In a production environment, we recommend that you use a quorum disk.</span></span> <span data-ttu-id="48ca0-213">Můžete použít sítě a úložiště toomake technologie systému je vysoce dostupný.</span><span class="sxs-lookup"><span data-stu-id="48ca0-213">You can use network and storage system technology toomake it highly available.</span></span>

![Obrázek 1: Příklad konfigurace služby systému Windows Server Failover Clustering pro SAP ASC nebo SCS v Azure][sap-ha-guide-figure-1000]

<span data-ttu-id="48ca0-215">_**Obrázek 1:** příklad konfigurace služby systému Windows Server Failover Clustering pro SAP ASC nebo SCS v Azure_</span><span class="sxs-lookup"><span data-stu-id="48ca0-215">_**Figure 1:** Example of a Windows Server Failover Clustering configuration for SAP ASCS/SCS in Azure_</span></span>

### <span data-ttu-id="48ca0-216"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Sdílené úložiště</span><span class="sxs-lookup"><span data-stu-id="48ca0-216"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a> Shared storage</span></span>
<span data-ttu-id="48ca0-217">Obrázek 1 zobrazuje i sdílené úložiště dvěma uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-217">Figure 1 also shows a two-node shared storage cluster.</span></span> <span data-ttu-id="48ca0-218">V clusteru služby místní sdílené úložiště rozpoznat všechny uzly v clusteru hello sdílené úložiště.</span><span class="sxs-lookup"><span data-stu-id="48ca0-218">In an on-premises shared storage cluster, all nodes in hello cluster detect shared storage.</span></span> <span data-ttu-id="48ca0-219">Mechanismus uzamčení chrání hello data před poškozením.</span><span class="sxs-lookup"><span data-stu-id="48ca0-219">A locking mechanism protects hello data from corruption.</span></span> <span data-ttu-id="48ca0-220">Všechny uzly, můžete zjistit, pokud jiný uzel selže.</span><span class="sxs-lookup"><span data-stu-id="48ca0-220">All nodes can detect if another node fails.</span></span> <span data-ttu-id="48ca0-221">Pokud selže jeden uzel, zbývající uzel hello vlastníkem hello prostředků úložiště a zajišťuje hello dostupnost služeb.</span><span class="sxs-lookup"><span data-stu-id="48ca0-221">If one node fails, hello remaining node takes ownership of hello storage resources and ensures hello availability of services.</span></span>

> [!NOTE]
> <span data-ttu-id="48ca0-222">Nepotřebujete sdílené disky pro zajištění vysoké dostupnosti s některými aplikacemi databázového systému, třeba s SQL serverem.</span><span class="sxs-lookup"><span data-stu-id="48ca0-222">You don't need shared disks for high availability with some DBMS applications, like with SQL Server.</span></span> <span data-ttu-id="48ca0-223">SQL serveru Always On replikuje databázového systému souborů protokolu a data z místního disku hello jeden cluster uzlu toohello místní disk jiným uzlem clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-223">SQL Server Always On replicates DBMS data and log files from hello local disk of one cluster node toohello local disk of another cluster node.</span></span> <span data-ttu-id="48ca0-224">V takovém případě konfigurace clusteru Windows hello nepotřebuje sdíleného disku.</span><span class="sxs-lookup"><span data-stu-id="48ca0-224">In that case, hello Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="48ca0-225"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Sítě a překladu</span><span class="sxs-lookup"><span data-stu-id="48ca0-225"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a> Networking and name resolution</span></span>
<span data-ttu-id="48ca0-226">Klientské počítače přístup hello clusteru přes virtuální IP adresu a název virtuálního hostitele tohoto hello, které poskytuje DNS server.</span><span class="sxs-lookup"><span data-stu-id="48ca0-226">Client computers reach hello cluster over a virtual IP address and a virtual host name that hello DNS server provides.</span></span> <span data-ttu-id="48ca0-227">Hello místní uzly a hello DNS server může zpracovat více IP adres.</span><span class="sxs-lookup"><span data-stu-id="48ca0-227">hello on-premises nodes and hello DNS server can handle multiple IP addresses.</span></span>

<span data-ttu-id="48ca0-228">V typické instalace můžete používat dva nebo více připojení k síti:</span><span class="sxs-lookup"><span data-stu-id="48ca0-228">In a typical setup, you use two or more network connections:</span></span>

* <span data-ttu-id="48ca0-229">Vyhrazené připojení toohello úložiště</span><span class="sxs-lookup"><span data-stu-id="48ca0-229">A dedicated connection toohello storage</span></span>
* <span data-ttu-id="48ca0-230">Cluster interní síťové připojení pro prezenčního signálu hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-230">A cluster-internal network connection for hello heartbeat</span></span>
* <span data-ttu-id="48ca0-231">Veřejné síti, které využívají klienti tooconnect toohello clusteru</span><span class="sxs-lookup"><span data-stu-id="48ca0-231">A public network that clients use tooconnect toohello cluster</span></span>

## <span data-ttu-id="48ca0-232"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Server Failover Clustering v Azure</span><span class="sxs-lookup"><span data-stu-id="48ca0-232"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a> Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="48ca0-233">Porovnání toobare systému nebo privátního cloudu nasazení virtuálních počítačů Azure vyžaduje další kroky tooconfigure Windows Server Failover Clustering.</span><span class="sxs-lookup"><span data-stu-id="48ca0-233">Compared toobare metal or private cloud deployments, Azure Virtual Machines requires additional steps tooconfigure Windows Server Failover Clustering.</span></span> <span data-ttu-id="48ca0-234">Při sestavování sdíleném disku clusteru, musíte pro instance SAP ASC nebo SCS hello tooset několik IP adres a virtuální hostitel názvy.</span><span class="sxs-lookup"><span data-stu-id="48ca0-234">When you build a shared cluster disk, you need tooset several IP addresses and virtual host names for hello SAP ASCS/SCS instance.</span></span>

<span data-ttu-id="48ca0-235">V tomto článku jsme popisují klíčové koncepty a hello další kroky požadované toobuild clusteru služby SAP centrální služby vysoké dostupnosti v Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-235">In this article, we discuss key concepts and hello additional steps required toobuild an SAP high-availability central services cluster in Azure.</span></span> <span data-ttu-id="48ca0-236">Můžeme vám ukážou, jak tooset až nástroj třetí strany hello s DataKeeper a jak tooconfigure hello Azure interní nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-236">We show you how tooset up hello third-party tool SIOS DataKeeper, and how tooconfigure hello Azure internal load balancer.</span></span> <span data-ttu-id="48ca0-237">Tyto nástroje toocreate clusteru s podporou převzetí služeb při selhání systému Windows můžete použít s určující sdílené složce v Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-237">You can use these tools toocreate a Windows failover cluster with a file share witness in Azure.</span></span>

![Obrázek 2: Windows Server Failover Clustering konfiguraci v Azure bez sdíleného disku][sap-ha-guide-figure-1001]

<span data-ttu-id="48ca0-239">_**Obrázek 2:** konfiguraci služby Windows Server Failover Clustering v Azure bez sdíleného disku_</span><span class="sxs-lookup"><span data-stu-id="48ca0-239">_**Figure 2:** Windows Server Failover Clustering configuration in Azure without a shared disk_</span></span>

### <span data-ttu-id="48ca0-240"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>Sdílený disk v Azure s DataKeeper s</span><span class="sxs-lookup"><span data-stu-id="48ca0-240"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a> Shared disk in Azure with SIOS DataKeeper</span></span>
<span data-ttu-id="48ca0-241">Třeba clusteru sdíleného úložiště pro instanci SAP ASC nebo SCS vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="48ca0-241">You need cluster shared storage for a high-availability SAP ASCS/SCS instance.</span></span> <span data-ttu-id="48ca0-242">Jako září 2016 Azure nenabízí sdílené úložiště, můžete použít toocreate clusteru sdíleného úložiště.</span><span class="sxs-lookup"><span data-stu-id="48ca0-242">As of September 2016, Azure doesn't offer shared storage that you can use toocreate a shared storage cluster.</span></span> <span data-ttu-id="48ca0-243">Pomocí softwaru třetí strany s DataKeeper Cluster Edition toocreate zrcadlené úložiště, která simuluje sdílené úložiště clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-243">You can use third-party software SIOS DataKeeper Cluster Edition toocreate a mirrored storage that simulates cluster shared storage.</span></span> <span data-ttu-id="48ca0-244">Hello SIOS řešení poskytuje data v reálném čase synchronní replikace.</span><span class="sxs-lookup"><span data-stu-id="48ca0-244">hello SIOS solution provides real-time synchronous data replication.</span></span> <span data-ttu-id="48ca0-245">Toto je, jak můžete vytvořit prostředek sdíleného disku pro cluster:</span><span class="sxs-lookup"><span data-stu-id="48ca0-245">This is how you can create a shared disk resource for a cluster:</span></span>

1. <span data-ttu-id="48ca0-246">Připojte další disk tooeach hello virtuálních počítačů (VM) v konfiguraci clusteru systému Windows.</span><span class="sxs-lookup"><span data-stu-id="48ca0-246">Attach an additional disk tooeach of hello virtual machines (VMs) in a Windows cluster configuration.</span></span>
2. <span data-ttu-id="48ca0-247">V obou uzlech virtuální počítač spusťte s DataKeeper Cluster Edition.</span><span class="sxs-lookup"><span data-stu-id="48ca0-247">Run SIOS DataKeeper Cluster Edition on both virtual machine nodes.</span></span>
3. <span data-ttu-id="48ca0-248">Nakonfigurujte s DataKeeper Cluster Edition, aby ho odpovídá obsahu hello hello další disku připojeného svazku z hello zdrojový virtuální počítač toohello další disk připojený svazek hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="48ca0-248">Configure SIOS DataKeeper Cluster Edition so that it mirrors hello content of hello additional disk attached volume from hello source virtual machine toohello additional disk attached volume of hello target virtual machine.</span></span> <span data-ttu-id="48ca0-249">S DataKeeper abstrahuje hello zdrojové a cílové místní svazky a uvede je tooWindows Server Clustering převzetí služeb při selhání jako jeden sdílený disk.</span><span class="sxs-lookup"><span data-stu-id="48ca0-249">SIOS DataKeeper abstracts hello source and target local volumes, and then presents them tooWindows Server Failover Clustering as one shared disk.</span></span>

<span data-ttu-id="48ca0-250">Přečtěte si další informace o [s DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span><span class="sxs-lookup"><span data-stu-id="48ca0-250">Get more information about [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span></span>

![Obrázek 3: Konfigurace služby systému Windows Server Failover Clustering v Azure pomocí DataKeeper s][sap-ha-guide-figure-1002]

<span data-ttu-id="48ca0-252">_**Obrázek 3:** konfigurace služby systému Windows Server Failover Clustering v Azure pomocí DataKeeper s_</span><span class="sxs-lookup"><span data-stu-id="48ca0-252">_**Figure 3:** Windows Server Failover Clustering configuration in Azure with SIOS DataKeeper_</span></span>

> [!NOTE]
> <span data-ttu-id="48ca0-253">Pro zajištění vysoké dostupnosti se některé produkty, databázového systému, jako je SQL Server není třeba sdílené disky.</span><span class="sxs-lookup"><span data-stu-id="48ca0-253">You don't need shared disks for high availability with some DBMS products, like SQL Server.</span></span> <span data-ttu-id="48ca0-254">SQL serveru Always On replikuje databázového systému souborů protokolu a data z místního disku hello jeden cluster uzlu toohello místní disk jiným uzlem clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-254">SQL Server Always On replicates DBMS data and log files from hello local disk of one cluster node toohello local disk of another cluster node.</span></span> <span data-ttu-id="48ca0-255">V takovém případě konfigurace clusteru Windows hello nepotřebuje sdíleného disku.</span><span class="sxs-lookup"><span data-stu-id="48ca0-255">In this case, hello Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="48ca0-256"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>Rozpoznání názvu v Azure</span><span class="sxs-lookup"><span data-stu-id="48ca0-256"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a> Name resolution in Azure</span></span>
<span data-ttu-id="48ca0-257">Hello Azure Cloudová platforma nenabízí hello možnost tooconfigure virtuální IP adresy, například plovoucí IP adresy.</span><span class="sxs-lookup"><span data-stu-id="48ca0-257">hello Azure cloud platform doesn't offer hello option tooconfigure virtual IP addresses, such as floating IP addresses.</span></span> <span data-ttu-id="48ca0-258">Budete potřebovat tooset alternativní řešení se virtuální IP adresy tooreach hello prostředku clusteru v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-258">You need an alternative solution tooset up a virtual IP address tooreach hello cluster resource in hello cloud.</span></span>
<span data-ttu-id="48ca0-259">Azure má interní nástroj v hello služby Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-259">Azure has an internal load balancer in hello Azure Load Balancer service.</span></span> <span data-ttu-id="48ca0-260">Pomocí nástroje pro vyrovnávání zatížení pro vnitřní hello klienti moci hello clusteru připojit přes virtuální IP adresu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-260">With hello internal load balancer, clients reach hello cluster over hello cluster virtual IP address.</span></span>
<span data-ttu-id="48ca0-261">Je nutné toodeploy hello interní vyrovnávání zátěže ve skupině prostředků hello, který obsahuje hello uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-261">You need toodeploy hello internal load balancer in hello resource group that contains hello cluster nodes.</span></span> <span data-ttu-id="48ca0-262">Potom nakonfigurujte všechny nezbytné port předávání pravidla s hello testu porty nástroje pro vyrovnávání zatížení pro vnitřní hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-262">Then, configure all necessary port forwarding rules with hello probe ports of hello internal load balancer.</span></span>
<span data-ttu-id="48ca0-263">Hello klienti mohou připojit prostřednictvím hello název virtuálního hostitele.</span><span class="sxs-lookup"><span data-stu-id="48ca0-263">hello clients can connect via hello virtual host name.</span></span> <span data-ttu-id="48ca0-264">Hello DNS server přeloží IP adresu clusteru hello a hello interní služby load vyrovnávání popisovače port předávání toohello aktivní uzel clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-264">hello DNS server resolves hello cluster IP address, and hello internal load balancer handles port forwarding toohello active node of hello cluster.</span></span>

## <span data-ttu-id="48ca0-265"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>SAP NetWeaver vysoké dostupnosti v Azure infrastruktury jako služba (IaaS)</span><span class="sxs-lookup"><span data-stu-id="48ca0-265"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a> SAP NetWeaver high availability in Azure Infrastructure-as-a-Service (IaaS)</span></span>
<span data-ttu-id="48ca0-266">tooachieve SAP vysokou dostupnost aplikace, například pro SAP softwarové komponenty, je třeba tooprotect hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="48ca0-266">tooachieve SAP application high availability, such as for SAP software components, you need tooprotect hello following components:</span></span>

* <span data-ttu-id="48ca0-267">Instance aplikačního serveru SAP</span><span class="sxs-lookup"><span data-stu-id="48ca0-267">SAP Application Server instance</span></span>
* <span data-ttu-id="48ca0-268">Instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="48ca0-268">SAP ASCS/SCS instance</span></span>
* <span data-ttu-id="48ca0-269">Server databázového systému</span><span class="sxs-lookup"><span data-stu-id="48ca0-269">DBMS server</span></span>

<span data-ttu-id="48ca0-270">Další informace o ochraně SAP součásti ve scénářích s vysokou dostupností najdete v tématu [virtuální počítače Azure plánování a implementace pro SAP NetWeaver][planning-guide-11].</span><span class="sxs-lookup"><span data-stu-id="48ca0-270">For more information about protecting SAP components in high-availability scenarios, see [Azure Virtual Machines planning and implementation for SAP NetWeaver][planning-guide-11].</span></span>

### <span data-ttu-id="48ca0-271"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Vysoká dostupnost SAP aplikační Server</span><span class="sxs-lookup"><span data-stu-id="48ca0-271"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a> High-availability SAP Application Server</span></span>
<span data-ttu-id="48ca0-272">Obvykle nepotřebujete specifického řešení vysoké dostupnosti pro instance aplikačního serveru SAP a dialogové okno hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-272">You usually don't need a specific high-availability solution for hello SAP Application Server and dialog instances.</span></span> <span data-ttu-id="48ca0-273">Dosažení vysoké dostupnosti pomocí redundance a nakonfigurujete více instancí dialogové okno v různých instancí virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-273">You achieve high availability by redundancy, and you'll configure multiple dialog instances in different instances of Azure Virtual Machines.</span></span> <span data-ttu-id="48ca0-274">Měli byste mít aspoň dvě instance SAP aplikace nainstalované v dvě instance virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-274">You should have at least two SAP application instances installed in two instances of Azure Virtual Machines.</span></span>

![Obrázek 4: Vysoké dostupnosti SAP aplikační Server][sap-ha-guide-figure-2000]

<span data-ttu-id="48ca0-276">_**Obrázek 4:** vysokou dostupnost SAP aplikační Server_</span><span class="sxs-lookup"><span data-stu-id="48ca0-276">_**Figure 4:** High-availability SAP Application Server_</span></span>

<span data-ttu-id="48ca0-277">Je nutné umístit všechny virtuální počítače, že hello instance aplikačního serveru SAP hostitele ve stejné skupině dostupnosti Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-277">You must place all virtual machines that host SAP Application Server instances in hello same Azure availability set.</span></span> <span data-ttu-id="48ca0-278">Nastavení Azure dostupnosti zajišťuje, že:</span><span class="sxs-lookup"><span data-stu-id="48ca0-278">An Azure availability set ensures that:</span></span>

* <span data-ttu-id="48ca0-279">Všechny virtuální počítače jsou součástí hello stejné domény upgradu.</span><span class="sxs-lookup"><span data-stu-id="48ca0-279">All virtual machines are part of hello same upgrade domain.</span></span> <span data-ttu-id="48ca0-280">Upgradovací doméně, například zajišťuje, že hello virtuální počítače se neaktualizují v hello současně během plánované údržby. výpadků.</span><span class="sxs-lookup"><span data-stu-id="48ca0-280">An upgrade domain, for example, makes sure that hello virtual machines aren't updated at hello same time during planned maintenance downtime.</span></span>
* <span data-ttu-id="48ca0-281">Všechny virtuální počítače jsou součástí hello stejné domény selhání.</span><span class="sxs-lookup"><span data-stu-id="48ca0-281">All virtual machines are part of hello same fault domain.</span></span> <span data-ttu-id="48ca0-282">Domény selhání, například zajišťuje nasazení virtuálních počítačů, tak, aby žádný jediný bod selhání ovlivňuje hello dostupnosti všech virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="48ca0-282">A fault domain, for example, makes sure that virtual machines are deployed so that no single point of failure affects hello availability of all virtual machines.</span></span>

<span data-ttu-id="48ca0-283">Další informace o příliš[spravovat hello dostupnosti virtuálních počítačů][virtual-machines-manage-availability].</span><span class="sxs-lookup"><span data-stu-id="48ca0-283">Learn more about how too[manage hello availability of virtual machines][virtual-machines-manage-availability].</span></span>

<span data-ttu-id="48ca0-284">Pouze nespravovaná disk: hello účtu úložiště Azure je potenciální jediný bod selhání, a proto je důležité toohave alespoň dva účty úložiště Azure, ve kterých jsou distribuovány aspoň dva virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="48ca0-284">Unmanaged disk only: Because hello Azure storage account is a potential single point of failure, it's important toohave at least two Azure storage accounts, in which at least two virtual machines are distributed.</span></span> <span data-ttu-id="48ca0-285">V ideální instalační program by se nasadí hello disky každého virtuálního počítače, na kterém běží instance SAP dialogové okno jiný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="48ca0-285">In an ideal setup, hello disks of each virtual machine that is running an SAP dialog instance would be deployed in a different storage account.</span></span>

### <span data-ttu-id="48ca0-286"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Instance SAP ASC nebo SCS vysokou dostupnost</span><span class="sxs-lookup"><span data-stu-id="48ca0-286"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a> High-availability SAP ASCS/SCS instance</span></span>
<span data-ttu-id="48ca0-287">Obrázek 5 je příklad instance SAP ASC nebo SCS vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="48ca0-287">Figure 5 is an example of a high-availability SAP ASCS/SCS instance.</span></span>

![Obrázek 5: Vysoká dostupnost SAP ASC nebo SCS instance][sap-ha-guide-figure-2001]

<span data-ttu-id="48ca0-289">_**Obrázek 5:** vysokou dostupnost SAP ASC nebo SCS instance_</span><span class="sxs-lookup"><span data-stu-id="48ca0-289">_**Figure 5:** High-availability SAP ASCS/SCS instance_</span></span>

#### <span data-ttu-id="48ca0-290"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>SAP ASC nebo SCS instance vysoká dostupnost s Windows Server Failover Clustering v Azure</span><span class="sxs-lookup"><span data-stu-id="48ca0-290"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a> SAP ASCS/SCS instance high availability with Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="48ca0-291">Porovnání toobare systému nebo privátního cloudu nasazení virtuálních počítačů Azure vyžaduje další kroky tooconfigure Windows Server Failover Clustering.</span><span class="sxs-lookup"><span data-stu-id="48ca0-291">Compared toobare metal or private cloud deployments, Azure Virtual Machines requires additional steps tooconfigure Windows Server Failover Clustering.</span></span> <span data-ttu-id="48ca0-292">toobuild clusteru převzetí služeb při selhání se systémem Windows, budete potřebovat sdíleném disku clusteru, několik IP adres, několik názvy virtuálních hostitelů a k nástroji pro vyrovnávání zatížení Azure interní pro clustering SAP ASC nebo SCS instance.</span><span class="sxs-lookup"><span data-stu-id="48ca0-292">toobuild a Windows failover cluster, you need a shared cluster disk, several IP addresses, several virtual host names, and an Azure internal load balancer for clustering an SAP ASCS/SCS instance.</span></span> <span data-ttu-id="48ca0-293">To probereme podrobněji dále v článku hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-293">We discuss this in more detail later in hello article.</span></span>

![Obrázek 6: Windows Server Failover Clustering pro konfiguraci SAP ASC nebo SCS v Azure pomocí DataKeeper s][sap-ha-guide-figure-1002]

<span data-ttu-id="48ca0-295">_**Obrázek 6:** Windows Server Failover Clustering SAP ASC nebo SCS konfigurace v Azure pomocí DataKeeper s_</span><span class="sxs-lookup"><span data-stu-id="48ca0-295">_**Figure 6:** Windows Server Failover Clustering for an SAP ASCS/SCS configuration in Azure with SIOS DataKeeper_</span></span>

### <span data-ttu-id="48ca0-296"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Instance databázového systému vysokou dostupnost</span><span class="sxs-lookup"><span data-stu-id="48ca0-296"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>High-availability DBMS instance</span></span>
<span data-ttu-id="48ca0-297">Hello databázového systému je také jeden kontaktní bod v systému SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-297">hello DBMS also is a single point of contact in an SAP system.</span></span> <span data-ttu-id="48ca0-298">Je třeba tooprotect ho pomocí řešení vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="48ca0-298">You need tooprotect it by using a high-availability solution.</span></span> <span data-ttu-id="48ca0-299">Obrázek 7 znázorňuje řešení vysoké dostupnosti SQL serveru Always On v Azure, Windows Server Failover Clustering a hello Azure interní nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-299">Figure 7 shows a SQL Server Always On high-availability solution in Azure, with Windows Server Failover Clustering and hello Azure internal load balancer.</span></span> <span data-ttu-id="48ca0-300">SQL serveru Always On replikuje pomocí replikace vlastní databázového systému databázového systému data a soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="48ca0-300">SQL Server Always On replicates DBMS data and log files by using its own DBMS replication.</span></span> <span data-ttu-id="48ca0-301">V takovém případě můžete nebudete potřebovat clusteru sdílené disky, což zjednodušuje nastavení celý hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-301">In this case, you don't need cluster shared disks, which simplifies hello entire setup.</span></span>

![Obrázek 7: Příklad databázového systému vysoké dostupnosti SAP, s SQL serveru Always On][sap-ha-guide-figure-2003]

<span data-ttu-id="48ca0-303">_**Obrázek 7:** příklad databázového systému vysoké dostupnosti SAP, s SQL serveru Always On_</span><span class="sxs-lookup"><span data-stu-id="48ca0-303">_**Figure 7:** Example of a high-availability SAP DBMS, with SQL Server Always On_</span></span>

<span data-ttu-id="48ca0-304">Další informace o clusteringu SQL Server v Azure pomocí modelu nasazení Azure Resource Manager hello najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="48ca0-304">For more information about clustering SQL Server in Azure by using hello Azure Resource Manager deployment model, see these articles:</span></span>

* <span data-ttu-id="48ca0-305">[Konfigurace skupiny dostupnosti Always On v Azure Virtual Machines ručně pomocí Resource Manager] [virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span><span class="sxs-lookup"><span data-stu-id="48ca0-305">[Configure Always On availability group in Azure Virtual Machines manually by using Resource Manager][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span></span>
* <span data-ttu-id="48ca0-306">[Konfigurace k nástroji pro vyrovnávání zatížení Azure interní pro skupiny dostupnosti Always On v Azure] [virtual-machines-windows-portal-sql-alwayson-int-listener]</span><span class="sxs-lookup"><span data-stu-id="48ca0-306">[Configure an Azure internal load balancer for an Always On availability group in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]</span></span>

## <span data-ttu-id="48ca0-307"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>Scénáře nasazení vysoké dostupnosti začátku do konce</span><span class="sxs-lookup"><span data-stu-id="48ca0-307"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a> End-to-end high-availability deployment scenarios</span></span>

### <a name="deployment-scenario-using-architectural-template-1"></a><span data-ttu-id="48ca0-308">Scénář nasazení pomocí architektury šablony 1</span><span class="sxs-lookup"><span data-stu-id="48ca0-308">Deployment scenario using Architectural Template 1</span></span>

<span data-ttu-id="48ca0-309">Obrázek 8 ukazuje příklad architektury SAP NetWeaver vysoké dostupnosti v Azure pro **jeden** systému SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-309">Figure 8 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="48ca0-310">Tento scénář je nastavit takto:</span><span class="sxs-lookup"><span data-stu-id="48ca0-310">This scenario is set up as follows:</span></span>

- <span data-ttu-id="48ca0-311">Jeden vyhrazený cluster se používá pro instance SAP ASC nebo SCS hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-311">One dedicated cluster is used for hello SAP ASCS/SCS instance.</span></span>
- <span data-ttu-id="48ca0-312">Jeden vyhrazený cluster se používá pro instanci databázového systému hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-312">One dedicated cluster is used for hello DBMS instance.</span></span>
- <span data-ttu-id="48ca0-313">Instance aplikačního serveru SAP nasazených ve svých vlastních vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="48ca0-313">SAP Application Server instances are deployed in their own dedicated VMs.</span></span>

![Obrázek 8: SAP vysokou dostupnost architektury šablony 1 vyhrazeném clusteru pro ASC nebo SCS a databázového systému][sap-ha-guide-figure-2004]

<span data-ttu-id="48ca0-315">_**Obrázek 8:** SAP architektury 1 šablony vysokou dostupnost, vyhrazené clustery pro ASC nebo SCS a databázového systému_</span><span class="sxs-lookup"><span data-stu-id="48ca0-315">_**Figure 8:** SAP high-availability Architectural Template 1, dedicated clusters for ASCS/SCS and DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-2"></a><span data-ttu-id="48ca0-316">Scénář nasazení pomocí architektury šablony 2</span><span class="sxs-lookup"><span data-stu-id="48ca0-316">Deployment scenario using Architectural Template 2</span></span>

<span data-ttu-id="48ca0-317">Obrázek 9 ukazuje příklad architektury SAP NetWeaver vysoké dostupnosti v Azure pro **jeden** systému SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-317">Figure 9 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="48ca0-318">Tento scénář je nastavit takto:</span><span class="sxs-lookup"><span data-stu-id="48ca0-318">This scenario is set up as follows:</span></span>

- <span data-ttu-id="48ca0-319">Jeden vyhrazený cluster se používá pro **i** hello SAP ASC nebo SCS instance a hello databázového systému.</span><span class="sxs-lookup"><span data-stu-id="48ca0-319">One dedicated cluster is used for **both** hello SAP ASCS/SCS instance and hello DBMS.</span></span>
- <span data-ttu-id="48ca0-320">Instance aplikačního serveru SAP jsou nasazeny v vlastní vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="48ca0-320">SAP Application Server instances are deployed in own dedicated VMs.</span></span>

![Obrázek 9: SAP vysokou dostupnost architektury šablony 2, přičemž vyhrazeném clusteru pro ASC nebo SCS a vyhrazeném clusteru pro databázového systému][sap-ha-guide-figure-2005]

<span data-ttu-id="48ca0-322">_**Obrázek 9:** SAP vysokou dostupnost architektury šablony 2, přičemž vyhrazeném clusteru pro ASC nebo SCS a vyhrazeném clusteru pro databázového systému_</span><span class="sxs-lookup"><span data-stu-id="48ca0-322">_**Figure 9:** SAP high-availability Architectural Template 2, with a dedicated cluster for ASCS/SCS and a dedicated cluster for DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-3"></a><span data-ttu-id="48ca0-323">Scénář nasazení pomocí architektury 3 šablony</span><span class="sxs-lookup"><span data-stu-id="48ca0-323">Deployment scenario using Architectural Template 3</span></span>

<span data-ttu-id="48ca0-324">Obrázek 10 ukazuje příklad architektury SAP NetWeaver vysoké dostupnosti v Azure pro **dva** SAP systémy, s &lt;SID1&gt; a &lt;SID2&gt;.</span><span class="sxs-lookup"><span data-stu-id="48ca0-324">Figure 10 shows an example of an SAP NetWeaver high-availability architecture in Azure for **two** SAP systems, with &lt;SID1&gt; and &lt;SID2&gt;.</span></span> <span data-ttu-id="48ca0-325">Tento scénář je nastavit takto:</span><span class="sxs-lookup"><span data-stu-id="48ca0-325">This scenario is set up as follows:</span></span>

- <span data-ttu-id="48ca0-326">Jeden vyhrazený cluster se používá pro **i** instance hello SAP ASC nebo SCS SID1 *a* hello SAP ASC nebo SCS SID2 instance (jeden cluster).</span><span class="sxs-lookup"><span data-stu-id="48ca0-326">One dedicated cluster is used for **both** hello SAP ASCS/SCS SID1 instance *and* hello SAP ASCS/SCS SID2 instance (one cluster).</span></span>
- <span data-ttu-id="48ca0-327">Jeden vyhrazený cluster se používá pro SID1 databázového systému a jiné vyhrazeném clusteru se používá pro SID2 databázového systému (dva clustery).</span><span class="sxs-lookup"><span data-stu-id="48ca0-327">One dedicated cluster is used for DBMS SID1, and another dedicated cluster is used for DBMS SID2 (two clusters).</span></span>
- <span data-ttu-id="48ca0-328">Instance aplikačního serveru SAP pro hello systému SAP SID1 mají své vlastní vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="48ca0-328">SAP Application Server instances for hello SAP system SID1 have their own dedicated VMs.</span></span>
- <span data-ttu-id="48ca0-329">Instance aplikačního serveru SAP pro hello systému SAP SID2 mají své vlastní vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="48ca0-329">SAP Application Server instances for hello SAP system SID2 have their own dedicated VMs.</span></span>

![Obrázek 10: Vysoká dostupnost architektury šablony 3, s clusterem vyhrazené pro různé ASC nebo SCS instance SAP][sap-ha-guide-figure-6003]

<span data-ttu-id="48ca0-331">_**Obrázek 10:** SAP vysokou dostupnost architektury šablony 3, s clusterem vyhrazené pro různé instance ASC nebo SCS_</span><span class="sxs-lookup"><span data-stu-id="48ca0-331">_**Figure 10:** SAP high-availability Architectural Template 3, with a dedicated cluster for different ASCS/SCS instances_</span></span>

## <span data-ttu-id="48ca0-332"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>Příprava infrastruktury hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-332"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a> Prepare hello infrastructure</span></span>

### <a name="prepare-hello-infrastructure-for-architectural-template-1"></a><span data-ttu-id="48ca0-333">Příprava hello infrastruktury pro architektury šablony 1</span><span class="sxs-lookup"><span data-stu-id="48ca0-333">Prepare hello infrastructure for Architectural Template 1</span></span>
<span data-ttu-id="48ca0-334">Šablony Azure Resource Manageru pro SAP zjednodušit nasazení požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="48ca0-334">Azure Resource Manager templates for SAP help simplify deployment of required resources.</span></span>

<span data-ttu-id="48ca0-335">Hello třívrstvá šablony ve službě Správce prostředků Azure také podporují scénáře vysoké dostupnosti, například architektury 1 šablony, která má dva clustery.</span><span class="sxs-lookup"><span data-stu-id="48ca0-335">hello three-tier templates in Azure Resource Manager also support high-availability scenarios, such as in Architectural Template 1, which has two clusters.</span></span> <span data-ttu-id="48ca0-336">Každý cluster je SAP jediný bod selhání pro SAP ASC nebo SCS a databázového systému.</span><span class="sxs-lookup"><span data-stu-id="48ca0-336">Each cluster is an SAP single point of failure for SAP ASCS/SCS and DBMS.</span></span>

<span data-ttu-id="48ca0-337">Zde je, kde můžete získat šablon Azure Resource Manageru hello ukázkový scénář, který jsme popisují v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="48ca0-337">Here's where you can get Azure Resource Manager templates for hello example scenario we describe in this article:</span></span>

* [<span data-ttu-id="48ca0-338">Obrázek pro Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="48ca0-338">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [<span data-ttu-id="48ca0-339">Obrázek pro Azure Marketplace pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="48ca0-339">Azure Marketplace image using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md)  
* [<span data-ttu-id="48ca0-340">Vlastní image</span><span class="sxs-lookup"><span data-stu-id="48ca0-340">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)
* [<span data-ttu-id="48ca0-341">Vlastní image pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="48ca0-341">Custom image using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-md)

<span data-ttu-id="48ca0-342">tooprepare hello infrastruktury pro architektury šablona 1:</span><span class="sxs-lookup"><span data-stu-id="48ca0-342">tooprepare hello infrastructure for Architectural Template 1:</span></span>

- <span data-ttu-id="48ca0-343">V portálu Azure, na hello hello **parametry** okno, v hello **SYSTEMAVAILABILITY** vyberte **HA**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-343">In hello Azure portal, on hello **Parameters** blade, in hello **SYSTEMAVAILABILITY** box, select **HA**.</span></span>

  ![Obrázek 11: Nastavit SAP vysokou dostupnost Azure Resource Manageru parametry][sap-ha-guide-figure-3000]

<span data-ttu-id="48ca0-345">_**Obrázek 11:** nastavit parametry SAP vysokou dostupnost Azure Resource Manager_</span><span class="sxs-lookup"><span data-stu-id="48ca0-345">_**Figure 11:** Set SAP high-availability Azure Resource Manager parameters_</span></span>


  <span data-ttu-id="48ca0-346">Vytvoření šablony Hello:</span><span class="sxs-lookup"><span data-stu-id="48ca0-346">hello templates create:</span></span>

  * <span data-ttu-id="48ca0-347">**Virtuální počítače**:</span><span class="sxs-lookup"><span data-stu-id="48ca0-347">**Virtual machines**:</span></span>
    * <span data-ttu-id="48ca0-348">Virtuální počítače aplikačního serveru SAP: <*SAPSystemSID*> - di - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="48ca0-348">SAP Application Server virtual machines: <*SAPSystemSID*>-di-<*Number*></span></span>
    * <span data-ttu-id="48ca0-349">ASC nebo SCS clusteru virtuálních počítačů: <*SAPSystemSID*> - ASC - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="48ca0-349">ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-ascs-<*Number*></span></span>
    * <span data-ttu-id="48ca0-350">Cluster databázového systému: <*SAPSystemSID*> - db - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="48ca0-350">DBMS cluster: <*SAPSystemSID*>-db-<*Number*></span></span>

  * <span data-ttu-id="48ca0-351">**Síťové karty pro všechny virtuální počítače s přidružené IP adresy**:</span><span class="sxs-lookup"><span data-stu-id="48ca0-351">**Network cards for all virtual machines, with associated IP addresses**:</span></span>
    * <span data-ttu-id="48ca0-352"><*SAPSystemSID*> - nic - di - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="48ca0-352"><*SAPSystemSID*>-nic-di-<*Number*></span></span>
    * <span data-ttu-id="48ca0-353"><*SAPSystemSID*> - nic - ASC - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="48ca0-353"><*SAPSystemSID*>-nic-ascs-<*Number*></span></span>
    * <span data-ttu-id="48ca0-354"><*SAPSystemSID*> - nic - db - <*číslo*></span><span class="sxs-lookup"><span data-stu-id="48ca0-354"><*SAPSystemSID*>-nic-db-<*Number*></span></span>

  * <span data-ttu-id="48ca0-355">**Účty služby Azure storage (pouze nespravovaná disků)**</span><span class="sxs-lookup"><span data-stu-id="48ca0-355">**Azure storage accounts (unmanaged disks only)**</span></span>

  * <span data-ttu-id="48ca0-356">**Skupiny dostupnosti** pro:</span><span class="sxs-lookup"><span data-stu-id="48ca0-356">**Availability groups** for:</span></span>
    * <span data-ttu-id="48ca0-357">Virtuální počítače aplikačního serveru SAP: <*SAPSystemSID*> - avset - di</span><span class="sxs-lookup"><span data-stu-id="48ca0-357">SAP Application Server virtual machines: <*SAPSystemSID*>-avset-di</span></span>
    * <span data-ttu-id="48ca0-358">SAP ASC nebo SCS clusteru virtuálních počítačů: <*SAPSystemSID*> - avset - ASC</span><span class="sxs-lookup"><span data-stu-id="48ca0-358">SAP ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-avset-ascs</span></span>
    * <span data-ttu-id="48ca0-359">Virtuální počítače clusteru databázového systému: <*SAPSystemSID*> - avset - db</span><span class="sxs-lookup"><span data-stu-id="48ca0-359">DBMS cluster virtual machines: <*SAPSystemSID*>-avset-db</span></span>

  * <span data-ttu-id="48ca0-360">**Nástroje pro vyrovnávání zatížení Azure interní**:</span><span class="sxs-lookup"><span data-stu-id="48ca0-360">**Azure internal load balancer**:</span></span>
    * <span data-ttu-id="48ca0-361">S všechny porty pro instanci ASC nebo SCS hello a IP adresu <*SAPSystemSID*> - lb - ASC</span><span class="sxs-lookup"><span data-stu-id="48ca0-361">With all ports for hello ASCS/SCS instance and IP address <*SAPSystemSID*>-lb-ascs</span></span>
    * <span data-ttu-id="48ca0-362">S všechny porty pro hello databázového systému SQL Server a IP adresu <*SAPSystemSID*> - lb - db</span><span class="sxs-lookup"><span data-stu-id="48ca0-362">With all ports for hello SQL Server DBMS and IP address <*SAPSystemSID*>-lb-db</span></span>

  * <span data-ttu-id="48ca0-363">**Skupina zabezpečení sítě**: <*SAPSystemSID*> - nsg - ASC-0</span><span class="sxs-lookup"><span data-stu-id="48ca0-363">**Network security group**: <*SAPSystemSID*>-nsg-ascs-0</span></span>  
    * <span data-ttu-id="48ca0-364">Pomocí otevřete externí toohello portu protokolu RDP (Remote Desktop) <*SAPSystemSID*> - ASC - 0 virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="48ca0-364">With an open external Remote Desktop Protocol (RDP) port toohello <*SAPSystemSID*>-ascs-0 virtual machine</span></span>

> [!NOTE]
> <span data-ttu-id="48ca0-365">Jsou všechny IP adresy hello síťové karty a nástroje pro vyrovnávání zatížení Azure interní **dynamické** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-365">All IP addresses of hello network cards and Azure internal load balancers are **dynamic** by default.</span></span> <span data-ttu-id="48ca0-366">Změna je příliš**statické** IP adresy.</span><span class="sxs-lookup"><span data-stu-id="48ca0-366">Change them too**static** IP addresses.</span></span> <span data-ttu-id="48ca0-367">Jsme popisují, jak toodo to později v článku hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-367">We describe how toodo this later in hello article.</span></span>
>
>

### <span data-ttu-id="48ca0-368"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Nasadit virtuální počítače s toouse (mezi různými místy) připojení k podnikové síti v produkčním prostředí</span><span class="sxs-lookup"><span data-stu-id="48ca0-368"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Deploy virtual machines with corporate network connectivity (cross-premises) toouse in production</span></span>
<span data-ttu-id="48ca0-369">Pro produkční systémy SAP, nasaďte virtuální počítače Azure s [připojení k podnikové síti (mezi různými místy)] [ planning-guide-2.2] pomocí Azure Site-to-Site VPN nebo Azure ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="48ca0-369">For production SAP systems, deploy Azure virtual machines with [corporate network connectivity (cross-premises)][planning-guide-2.2] by using Azure Site-to-Site VPN or Azure ExpressRoute.</span></span>

> [!NOTE]
> <span data-ttu-id="48ca0-370">Můžete použít instanci Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="48ca0-370">You can use your Azure Virtual Network instance.</span></span> <span data-ttu-id="48ca0-371">Hello virtuální síť a podsíť již byly vytvořeny a připravený.</span><span class="sxs-lookup"><span data-stu-id="48ca0-371">hello virtual network and subnet have already been created and prepared.</span></span>
>
>

1.  <span data-ttu-id="48ca0-372">V portálu Azure, na hello hello **parametry** okno, v hello **NEWOREXISTINGSUBNET** vyberte **existující**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-372">In hello Azure portal, on hello **Parameters** blade, in hello **NEWOREXISTINGSUBNET** box, select **existing**.</span></span>
2.  <span data-ttu-id="48ca0-373">V hello **SUBNETID** pole, přidejte hello úplný řetězec připravené Azure sítě SubnetID, kam budete toodeploy, virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-373">In hello **SUBNETID** box, add hello full string of your prepared Azure network SubnetID where you plan toodeploy your Azure virtual machines.</span></span>
3.  <span data-ttu-id="48ca0-374">tooget seznam všech podsítí síť Azure, spusťte tento příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="48ca0-374">tooget a list of all Azure network subnets, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  <span data-ttu-id="48ca0-375">Hello **ID** poli se zobrazí hello **SUBNETID**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-375">hello **ID** field shows hello **SUBNETID**.</span></span>
4. <span data-ttu-id="48ca0-376">seznam všech tooget **SUBNETID** hodnoty, spusťte tento příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="48ca0-376">tooget a list of all **SUBNETID** values, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  <span data-ttu-id="48ca0-377">Hello **SUBNETID** vypadá podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="48ca0-377">hello **SUBNETID** looks like this:</span></span>

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

### <span data-ttu-id="48ca0-378"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Nasadit jenom pro cloud instance SAP pro testovací a demo</span><span class="sxs-lookup"><span data-stu-id="48ca0-378"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Deploy cloud-only SAP instances for test and demo</span></span>
<span data-ttu-id="48ca0-379">Můžete nasadit systém SAP vysoké dostupnosti v modelu nasazení jenom cloudu.</span><span class="sxs-lookup"><span data-stu-id="48ca0-379">You can deploy your high-availability SAP system in a cloud-only deployment model.</span></span> <span data-ttu-id="48ca0-380">Tento typ nasazení je primárně užitečné pro ukázku a testovací případy použití.</span><span class="sxs-lookup"><span data-stu-id="48ca0-380">This kind of deployment primarily is useful for demo and test use cases.</span></span> <span data-ttu-id="48ca0-381">Ji není vhodná pro produkční případy použití.</span><span class="sxs-lookup"><span data-stu-id="48ca0-381">It's not suited for production use cases.</span></span>

- <span data-ttu-id="48ca0-382">V portálu Azure, na hello hello **parametry** okno, v hello **NEWOREXISTINGSUBNET** vyberte **nové**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-382">In hello Azure portal, on hello **Parameters** blade, in hello **NEWOREXISTINGSUBNET** box, select **new**.</span></span> <span data-ttu-id="48ca0-383">Nechte hello **SUBNETID** pole prázdná.</span><span class="sxs-lookup"><span data-stu-id="48ca0-383">Leave hello **SUBNETID** field empty.</span></span>

  <span data-ttu-id="48ca0-384">Hello SAP Azure Resource Manager automaticky vytvoří šablona hello virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="48ca0-384">hello SAP Azure Resource Manager template automatically creates hello Azure virtual network and subnet.</span></span>

> [!NOTE]
> <span data-ttu-id="48ca0-385">Musíte taky toodeploy virtuální počítač alespoň jeden vyhrazený pro službu Active Directory a DNS v hello stejnou instanci Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="48ca0-385">You also need toodeploy at least one dedicated virtual machine for Active Directory and DNS in hello same Azure Virtual Network instance.</span></span> <span data-ttu-id="48ca0-386">Šablona Hello nepodporuje vytvoření těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="48ca0-386">hello template doesn't create these virtual machines.</span></span>
>
>


### <a name="prepare-hello-infrastructure-for-architectural-template-2"></a><span data-ttu-id="48ca0-387">Příprava infrastruktury hello architektury šablony 2</span><span class="sxs-lookup"><span data-stu-id="48ca0-387">Prepare hello infrastructure for Architectural Template 2</span></span>

<span data-ttu-id="48ca0-388">Tato šablona Azure Resource Manager můžete použít pro SAP toohelp zjednodušit nasazování požadované infrastruktury prostředků pro SAP architektury šablony 2.</span><span class="sxs-lookup"><span data-stu-id="48ca0-388">You can use this Azure Resource Manager template for SAP toohelp simplify deployment of required infrastructure resources for SAP Architectural Template 2.</span></span>

<span data-ttu-id="48ca0-389">Zde je, kde můžete získat šablon Azure Resource Manageru pro tento scénář nasazení:</span><span class="sxs-lookup"><span data-stu-id="48ca0-389">Here's where you can get Azure Resource Manager templates for this deployment scenario:</span></span>

* [<span data-ttu-id="48ca0-390">Obrázek pro Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="48ca0-390">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [<span data-ttu-id="48ca0-391">Obrázek pro Azure Marketplace pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="48ca0-391">Azure Marketplace image using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged-md)  
* [<span data-ttu-id="48ca0-392">Vlastní image</span><span class="sxs-lookup"><span data-stu-id="48ca0-392">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)
* [<span data-ttu-id="48ca0-393">Vlastní image pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="48ca0-393">Custom image using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged-md)


### <a name="prepare-hello-infrastructure-for-architectural-template-3"></a><span data-ttu-id="48ca0-394">Příprava hello infrastruktury pro architektury 3 šablony</span><span class="sxs-lookup"><span data-stu-id="48ca0-394">Prepare hello infrastructure for Architectural Template 3</span></span>

<span data-ttu-id="48ca0-395">Můžete připravit hello infrastruktury a nakonfigurovat SAP pro **více SID**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-395">You can prepare hello infrastructure and configure SAP for **multi-SID**.</span></span> <span data-ttu-id="48ca0-396">Například můžete přidat další instance SAP ASC nebo SCS do *existující* konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-396">For example, you can add an additional SAP ASCS/SCS instance into an *existing* cluster configuration.</span></span> <span data-ttu-id="48ca0-397">Další informace najdete v tématu [nakonfigurovat další instance SAP ASC nebo SCS do existující toocreate konfigurace clusteru více SID konfigurace aplikace SAP ve službě Správce prostředků Azure][sap-ha-multi-sid-guide].</span><span class="sxs-lookup"><span data-stu-id="48ca0-397">For more information, see [Configure an additional SAP ASCS/SCS instance into an existing cluster configuration toocreate an SAP multi-SID configuration in Azure Resource Manager][sap-ha-multi-sid-guide].</span></span>

<span data-ttu-id="48ca0-398">Pokud chcete, aby toocreate do nového clusteru více SID, můžete použít více hello-identifikátor SID [šablony rychlý start na Githubu](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="48ca0-398">If you want toocreate a new multi-SID cluster, you can use hello multi-SID [quickstart templates on GitHub](https://github.com/Azure/azure-quickstart-templates).</span></span>
<span data-ttu-id="48ca0-399">toocreate do nového clusteru více SID, je třeba toodeploy hello následující tři šablony:</span><span class="sxs-lookup"><span data-stu-id="48ca0-399">toocreate a new multi-SID cluster, you need toodeploy hello following three templates:</span></span>

* [<span data-ttu-id="48ca0-400">ASC nebo SCS šablony</span><span class="sxs-lookup"><span data-stu-id="48ca0-400">ASCS/SCS template</span></span>](#ASCS-SCS-template)
* [<span data-ttu-id="48ca0-401">Šablona databáze</span><span class="sxs-lookup"><span data-stu-id="48ca0-401">Database template</span></span>](#database-template)
* [<span data-ttu-id="48ca0-402">Šablona servery aplikací</span><span class="sxs-lookup"><span data-stu-id="48ca0-402">Application servers template</span></span>](#application-servers-template)

<span data-ttu-id="48ca0-403">Hello následující sekce obsahují další podrobnosti o hello šablony a parametry hello potřebujete tooprovide v šablonách hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-403">hello following sections have more details about hello templates and hello parameters you need tooprovide in hello templates.</span></span>

#### <span data-ttu-id="48ca0-404"><a name="ASCS-SCS-template"></a>ASC nebo SCS šablony</span><span class="sxs-lookup"><span data-stu-id="48ca0-404"><a name="ASCS-SCS-template"></a> ASCS/SCS template</span></span>

<span data-ttu-id="48ca0-405">Šablona ASC nebo SCS Hello nasadí dva virtuální počítače, které můžete použít toocreate cluster převzetí služeb při selhání systému Windows Server, který je hostitelem více instancí ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="48ca0-405">hello ASCS/SCS template deploys two virtual machines that you can use toocreate a Windows Server failover cluster that hosts multiple ASCS/SCS instances.</span></span>

<span data-ttu-id="48ca0-406">tooset až hello ASC nebo SCS více SID šabloně hello [ASC nebo SCS více SID šablony] [ sap-templates-3-tier-multisid-xscs-marketplace-image] nebo [ASC nebo SCS více SID šablony pomocí disků spravované] [ sap-templates-3-tier-multisid-xscs-marketplace-image-md], zadejte hodnoty pro hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="48ca0-406">tooset up hello ASCS/SCS multi-SID template, in hello [ASCS/SCS multi-SID template][sap-templates-3-tier-multisid-xscs-marketplace-image] or [ASCS/SCS multi-SID template using Managed Disks][sap-templates-3-tier-multisid-xscs-marketplace-image-md], enter values for hello following parameters:</span></span>

  - <span data-ttu-id="48ca0-407">**Předpona prostředků**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-407">**Resource Prefix**.</span></span>  <span data-ttu-id="48ca0-408">Nastavte předpona hello prostředek, což je použité tooprefix všechny prostředky, které jsou vytvořeny při nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-408">Set hello resource prefix, which is used tooprefix all resources that are created during hello deployment.</span></span> <span data-ttu-id="48ca0-409">Protože hello prostředky nepatří tooonly jednoho systému SAP, předpona hello hello prostředku není hello SID systému SAP jeden.</span><span class="sxs-lookup"><span data-stu-id="48ca0-409">Because hello resources do not belong tooonly one SAP system, hello prefix of hello resource is not hello SID of one SAP system.</span></span>  <span data-ttu-id="48ca0-410">Předpona Hello musí být mezi **tři až šest znaků**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-410">hello prefix must be between **three and six characters**.</span></span>
  - <span data-ttu-id="48ca0-411">**Stack – typ**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-411">**Stack Type**.</span></span> <span data-ttu-id="48ca0-412">Vyberte typ zásobníku hello hello systému SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-412">Select hello stack type of hello SAP system.</span></span> <span data-ttu-id="48ca0-413">V závislosti na typu hello zásobníku nástroj pro vyrovnávání zatížení Azure má jeden (ABAP nebo Java pouze) nebo dvě (ABAP + Java) privátní IP adresy na systému SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-413">Depending on hello stack type, Azure Load Balancer has one (ABAP or Java only) or two (ABAP+Java) private IP addresses per SAP system.</span></span>
  -  <span data-ttu-id="48ca0-414">**Typ operačního systému**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-414">**OS Type**.</span></span> <span data-ttu-id="48ca0-415">Vyberte operační systém hello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="48ca0-415">Select hello operating system of hello virtual machines.</span></span>
  -  <span data-ttu-id="48ca0-416">**Počet systému SAP**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-416">**SAP System Count**.</span></span> <span data-ttu-id="48ca0-417">Vyberte číslo hello systémů SAP chcete tooinstall v tomto clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-417">Select hello number of SAP systems you want tooinstall in this cluster.</span></span>
  -  <span data-ttu-id="48ca0-418">**Dostupnost systému**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-418">**System Availability**.</span></span> <span data-ttu-id="48ca0-419">Vyberte **HA**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-419">Select **HA**.</span></span>
  -  <span data-ttu-id="48ca0-420">**Uživatelské jméno správce a heslo správce**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-420">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="48ca0-421">Vytvořte nového uživatele, který lze použít toosign toohello počítači.</span><span class="sxs-lookup"><span data-stu-id="48ca0-421">Create a new user that can be used toosign in toohello machine.</span></span>
  -  <span data-ttu-id="48ca0-422">**Nový nebo existující podsíť**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-422">**New Or Existing Subnet**.</span></span> <span data-ttu-id="48ca0-423">Nastavte, jestli mají být vytvořeny nové virtuální sítě a podsítě, nebo se použije existující podsítí.</span><span class="sxs-lookup"><span data-stu-id="48ca0-423">Set whether a new virtual network and subnet should be created, or an existing subnet should be used.</span></span> <span data-ttu-id="48ca0-424">Pokud již máte virtuální síť, která je připojená tooyour do místní sítě, vyberte **existující**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-424">If you already have a virtual network that is connected tooyour on-premises network, select **existing**.</span></span>
  -  <span data-ttu-id="48ca0-425">**Id podsítě**. ID sady hello hello podsíť toowhich hello virtuálních počítačů by měly být připojené.</span><span class="sxs-lookup"><span data-stu-id="48ca0-425">**Subnet Id**. Set hello ID of hello subnet toowhich hello virtual machines should be connected.</span></span> <span data-ttu-id="48ca0-426">Vyberte podsíť hello virtuální privátní sítě (VPN) nebo ExpressRoute virtuální sítě tooconnect hello virtuálního počítače tooyour do místní sítě.</span><span class="sxs-lookup"><span data-stu-id="48ca0-426">Select hello subnet of your virtual private network (VPN) or ExpressRoute virtual network tooconnect hello virtual machine tooyour on-premises network.</span></span> <span data-ttu-id="48ca0-427">Hello ID obvykle vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="48ca0-427">hello ID usually looks like this:</span></span>

   <span data-ttu-id="48ca0-428">/subscriptions/ <*id předplatného*> /resourceGroups/ <*název skupiny prostředků*> /providers/Microsoft.Network/virtualNetworks/ <*název virtuální sítě*> /subnets/ <*název podsítě.*></span><span class="sxs-lookup"><span data-stu-id="48ca0-428">/subscriptions/<*subscription id*>/resourceGroups/<*resource group name*>/providers/Microsoft.Network/virtualNetworks/<*virtual network name*>/subnets/<*subnet name*></span></span>

<span data-ttu-id="48ca0-429">Šablona Hello nasadí jednu instanci nástroj pro vyrovnávání zatížení Azure, která podporuje více systémů SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-429">hello template deploys one Azure Load Balancer instance, which supports multiple SAP systems.</span></span>

- <span data-ttu-id="48ca0-430">Hello ASC instance jsou nakonfigurované pro instance číslo 00, 10, 20...</span><span class="sxs-lookup"><span data-stu-id="48ca0-430">hello ASCS instances are configured for instance number 00, 10, 20...</span></span>
- <span data-ttu-id="48ca0-431">Hello SCS instance jsou nakonfigurované pro instance číslo 01, 11, 21...</span><span class="sxs-lookup"><span data-stu-id="48ca0-431">hello SCS instances are configured for instance number 01, 11, 21...</span></span>
- <span data-ttu-id="48ca0-432">instance Hello ASC zařazování replikace serveru (YBRAT) (pouze Linux) jsou nakonfigurované pro čísla instance 02, 12, 22...</span><span class="sxs-lookup"><span data-stu-id="48ca0-432">hello ASCS Enqueue Replication Server (ERS) (Linux only) instances are configured for instance number 02, 12, 22...</span></span>
- <span data-ttu-id="48ca0-433">instance Hello SCS YBRAT (pouze Linux) jsou nakonfigurované pro instance číslo 03, 13, 23...</span><span class="sxs-lookup"><span data-stu-id="48ca0-433">hello SCS ERS (Linux only) instances are configured for instance number 03, 13, 23...</span></span>

<span data-ttu-id="48ca0-434">Hello nástroj pro vyrovnávání zatížení obsahuje 1 (2 pro Linux) VIP(s), 1 x virtuální IP adresa pro ASC nebo SCS a 1 x virtuální IP adresa pro YBRAT (pouze Linux).</span><span class="sxs-lookup"><span data-stu-id="48ca0-434">hello load balancer contains 1 (2 for Linux) VIP(s), 1x VIP for ASCS/SCS and 1x VIP for ERS (Linux only).</span></span>

<span data-ttu-id="48ca0-435">Hello následující seznam obsahuje všechny pravidla (kde x je číslo hello hello SAP systému, například 1, 2, 3...) pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="48ca0-435">hello following list contains all load balancing rules (where x is hello number of hello SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="48ca0-436">Porty specifické pro systém Windows pro každý systém SAP: 445, 5985</span><span class="sxs-lookup"><span data-stu-id="48ca0-436">Windows-specific ports for every SAP system: 445, 5985</span></span>
- <span data-ttu-id="48ca0-437">Porty ASC (čísla instance x0): 32 × 0, 36 x 0, 39 x 0, 81 x 0, 5 x 013, 5 x 014, 5 x 016</span><span class="sxs-lookup"><span data-stu-id="48ca0-437">ASCS ports (instance number x0): 32x0, 36x0, 39x0, 81x0, 5x013, 5x014, 5x016</span></span>
- <span data-ttu-id="48ca0-438">Porty SCS (čísla instance x1): 32 x 1, 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116</span><span class="sxs-lookup"><span data-stu-id="48ca0-438">SCS ports (instance number x1): 32x1, 33x1, 39x1, 81x1, 5x113, 5x114, 5x116</span></span>
- <span data-ttu-id="48ca0-439">ASC YBRAT portů v systému Linux (čísla instance x2): 33 x 2, 5 x 213, 5 x 214, 5 x 216</span><span class="sxs-lookup"><span data-stu-id="48ca0-439">ASCS ERS ports on Linux (instance number x2): 33x2, 5x213, 5x214, 5x216</span></span>
- <span data-ttu-id="48ca0-440">SCS YBRAT portů v systému Linux (čísla instance x3): 33 x 3, 5 x 313, 5 x 314, 5 x 316</span><span class="sxs-lookup"><span data-stu-id="48ca0-440">SCS ERS ports on Linux (instance number x3): 33x3, 5x313, 5x314, 5x316</span></span>

<span data-ttu-id="48ca0-441">Nástroj pro vyrovnávání zatížení Hello je nakonfigurované toouse hello následující porty testu (kde x je číslo hello hello SAP systému, například 1, 2, 3...):</span><span class="sxs-lookup"><span data-stu-id="48ca0-441">hello load balancer is configured toouse hello following probe ports (where x is hello number of hello SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="48ca0-442">Port testu nástroje pro vyrovnávání zatížení ASC nebo SCS interní: 620 x 0</span><span class="sxs-lookup"><span data-stu-id="48ca0-442">ASCS/SCS internal load balancer probe port: 620x0</span></span>
- <span data-ttu-id="48ca0-443">Interní YBRAT načíst port testu vyrovnávání (pouze Linux): 621 x 2</span><span class="sxs-lookup"><span data-stu-id="48ca0-443">ERS internal load balancer probe port (Linux only): 621x2</span></span>

#### <span data-ttu-id="48ca0-444"><a name="database-template"></a>Šablona databáze</span><span class="sxs-lookup"><span data-stu-id="48ca0-444"><a name="database-template"></a> Database template</span></span>

<span data-ttu-id="48ca0-445">Šablona databáze Hello nasadí jeden nebo dva virtuální počítače, které můžete použít tooinstall hello systému správy relačních databází (RDBMS) pro systém SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-445">hello database template deploys one or two virtual machines that you can use tooinstall hello relational database management system (RDBMS) for one SAP system.</span></span> <span data-ttu-id="48ca0-446">Například pokud nasadíte šablonu ASC nebo SCS pro pět systémy SAP, je třeba toodeploy této šablony pětkrát.</span><span class="sxs-lookup"><span data-stu-id="48ca0-446">For example, if you deploy an ASCS/SCS template for five SAP systems, you need toodeploy this template five times.</span></span>

<span data-ttu-id="48ca0-447">tooset až hello databáze více SID šablony, v hello [databáze více SID šablony] [ sap-templates-3-tier-multisid-db-marketplace-image] nebo [databáze více SID šablony pomocí disků spravované] [ sap-templates-3-tier-multisid-db-marketplace-image-md], zadejte hodnoty pro hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="48ca0-447">tooset up hello database multi-SID template, in hello [database multi-SID template][sap-templates-3-tier-multisid-db-marketplace-image] or [database multi-SID template using Managed Disks][sap-templates-3-tier-multisid-db-marketplace-image-md], enter values for hello following parameters:</span></span>

  -  <span data-ttu-id="48ca0-448">**Id systému SAP**. Zadejte ID systému SAP hello hello chcete tooinstall systému SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-448">**Sap System Id**. Enter hello SAP system ID of hello SAP system you want tooinstall.</span></span> <span data-ttu-id="48ca0-449">Hello ID se použije jako předpona pro hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="48ca0-449">hello ID will be used as a prefix for hello resources that are deployed.</span></span>
  -  <span data-ttu-id="48ca0-450">**Typ operačního systému**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-450">**Os Type**.</span></span> <span data-ttu-id="48ca0-451">Vyberte operační systém hello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="48ca0-451">Select hello operating system of hello virtual machines.</span></span>
  -  <span data-ttu-id="48ca0-452">**Hodnota DbType**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-452">**Dbtype**.</span></span> <span data-ttu-id="48ca0-453">Vyberte typ hello hello databáze, kterou chcete tooinstall v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-453">Select hello type of hello database you want tooinstall on hello cluster.</span></span> <span data-ttu-id="48ca0-454">Vyberte **SQL** Pokud chcete, aby tooinstall Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="48ca0-454">Select **SQL** if you want tooinstall Microsoft SQL Server.</span></span> <span data-ttu-id="48ca0-455">Vyberte **HANA** Pokud máte v plánu tooinstall SAP HANA hello virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="48ca0-455">Select **HANA** if you plan tooinstall SAP HANA on hello virtual machines.</span></span> <span data-ttu-id="48ca0-456">Zajistěte, aby tooselect hello správný typ operačního systému: vyberte **Windows** pro SQL a vyberte distribuční Linux pro HANA.</span><span class="sxs-lookup"><span data-stu-id="48ca0-456">Make sure tooselect hello correct operating system type: select **Windows** for SQL, and select a Linux distribution for HANA.</span></span> <span data-ttu-id="48ca0-457">Hello Vyrovnávání zatížení Azure, která je připojená toohello, které virtuální počítače budou nakonfigurované toosupport hello vybrané databáze typu:</span><span class="sxs-lookup"><span data-stu-id="48ca0-457">hello Azure Load Balancer that is connected toohello virtual machines will be configured toosupport hello selected database type:</span></span>
    * <span data-ttu-id="48ca0-458">**SQL**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-458">**SQL**.</span></span> <span data-ttu-id="48ca0-459">Nástroj pro vyrovnávání zatížení Hello bude Vyrovnávání zatížení port 1433.</span><span class="sxs-lookup"><span data-stu-id="48ca0-459">hello load balancer will load-balance port 1433.</span></span> <span data-ttu-id="48ca0-460">Ujistěte se, že toouse tento port pro váš instalační program SQL serveru Always On.</span><span class="sxs-lookup"><span data-stu-id="48ca0-460">Make sure toouse this port for your SQL Server Always On setup.</span></span>
    * <span data-ttu-id="48ca0-461">**HANA**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-461">**HANA**.</span></span> <span data-ttu-id="48ca0-462">Nástroj pro vyrovnávání zatížení Hello bude porty 35015 a 35017 Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-462">hello load balancer will load-balance ports 35015 and 35017.</span></span> <span data-ttu-id="48ca0-463">Ujistěte se, že tooinstall SAP HANA číslem instance **50**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-463">Make sure tooinstall SAP HANA with instance number **50**.</span></span>
    <span data-ttu-id="48ca0-464">Nástroj pro vyrovnávání zatížení Hello použije port testu 62550.</span><span class="sxs-lookup"><span data-stu-id="48ca0-464">hello load balancer will use probe port 62550.</span></span>
  -  <span data-ttu-id="48ca0-465">**Velikost systému SAP**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-465">**Sap System Size**.</span></span> <span data-ttu-id="48ca0-466">Poskytne sadu hello počet protokoly SAP hello nový systém.</span><span class="sxs-lookup"><span data-stu-id="48ca0-466">Set hello number of SAPS hello new system will provide.</span></span> <span data-ttu-id="48ca0-467">Pokud si nejste jisti, kolik systému hello protokoly SAP bude vyžadovat, požádejte SAP technologie partnera nebo systémový integrátor.</span><span class="sxs-lookup"><span data-stu-id="48ca0-467">If you are not sure how many SAPS hello system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="48ca0-468">**Dostupnost systému**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-468">**System Availability**.</span></span> <span data-ttu-id="48ca0-469">Vyberte **HA**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-469">Select **HA**.</span></span>
  -  <span data-ttu-id="48ca0-470">**Uživatelské jméno správce a heslo správce**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-470">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="48ca0-471">Vytvořte nového uživatele, který lze použít toosign toohello počítači.</span><span class="sxs-lookup"><span data-stu-id="48ca0-471">Create a new user that can be used toosign in toohello machine.</span></span>
  -  <span data-ttu-id="48ca0-472">**Id podsítě**. Zadejte ID hello hello podsítě, který jste použili při nasazení hello hello ASC nebo SCS šablony nebo ID hello hello podsítě, který byl vytvořen jako součást hello ASC nebo SCS šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-472">**Subnet Id**. Enter hello ID of hello subnet that you used during hello deployment of hello ASCS/SCS template, or hello ID of hello subnet that was created as part of hello ASCS/SCS template deployment.</span></span>

#### <span data-ttu-id="48ca0-473"><a name="application-servers-template"></a>Šablona servery aplikací</span><span class="sxs-lookup"><span data-stu-id="48ca0-473"><a name="application-servers-template"></a> Application servers template</span></span>

<span data-ttu-id="48ca0-474">šablony servery aplikace Hello nasadí dvě nebo více virtuálních počítačů, které lze použít jako instance aplikačního serveru SAP jeden systému SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-474">hello application servers template deploys two or more virtual machines that can be used as SAP Application Server instances for one SAP system.</span></span> <span data-ttu-id="48ca0-475">Například pokud nasadíte šablonu ASC nebo SCS pro pět systémy SAP, je třeba toodeploy této šablony pětkrát.</span><span class="sxs-lookup"><span data-stu-id="48ca0-475">For example, if you deploy an ASCS/SCS template for five SAP systems, you need toodeploy this template five times.</span></span>

<span data-ttu-id="48ca0-476">tooset až hello servery více SID šablona aplikací v hello [šablony SID více serverů aplikace] [ sap-templates-3-tier-multisid-apps-marketplace-image] nebo [šablony SID více serverů aplikace pomocí spravovaných disků] [ sap-templates-3-tier-multisid-apps-marketplace-image-md], zadejte hodnoty pro hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="48ca0-476">tooset up hello application servers multi-SID template, in hello [application servers multi-SID template][sap-templates-3-tier-multisid-apps-marketplace-image] or [application servers multi-SID template using Managed Disks][sap-templates-3-tier-multisid-apps-marketplace-image-md], enter values for hello following parameters:</span></span>

  -  <span data-ttu-id="48ca0-477">**Id systému SAP**. Zadejte ID systému SAP hello hello chcete tooinstall systému SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-477">**Sap System Id**. Enter hello SAP system ID of hello SAP system you want tooinstall.</span></span> <span data-ttu-id="48ca0-478">Hello ID se použije jako předpona pro hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="48ca0-478">hello ID will be used as a prefix for hello resources that are deployed.</span></span>
  -  <span data-ttu-id="48ca0-479">**Typ operačního systému**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-479">**Os Type**.</span></span> <span data-ttu-id="48ca0-480">Vyberte operační systém hello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="48ca0-480">Select hello operating system of hello virtual machines.</span></span>
  -  <span data-ttu-id="48ca0-481">**Velikost systému SAP**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-481">**Sap System Size**.</span></span> <span data-ttu-id="48ca0-482">poskytne Hello počet protokoly SAP hello nový systém.</span><span class="sxs-lookup"><span data-stu-id="48ca0-482">hello number of SAPS hello new system will provide.</span></span> <span data-ttu-id="48ca0-483">Pokud si nejste jisti, kolik systému hello protokoly SAP bude vyžadovat, požádejte SAP technologie partnera nebo systémový integrátor.</span><span class="sxs-lookup"><span data-stu-id="48ca0-483">If you are not sure how many SAPS hello system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="48ca0-484">**Dostupnost systému**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-484">**System Availability**.</span></span> <span data-ttu-id="48ca0-485">Vyberte **HA**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-485">Select **HA**.</span></span>
  -  <span data-ttu-id="48ca0-486">**Uživatelské jméno správce a heslo správce**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-486">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="48ca0-487">Vytvořte nového uživatele, který lze použít toosign toohello počítači.</span><span class="sxs-lookup"><span data-stu-id="48ca0-487">Create a new user that can be used toosign in toohello machine.</span></span>
  -  <span data-ttu-id="48ca0-488">**Id podsítě**. Zadejte ID hello hello podsítě, který jste použili při nasazení hello hello ASC nebo SCS šablony nebo ID hello hello podsítě, který byl vytvořen jako součást hello ASC nebo SCS šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-488">**Subnet Id**. Enter hello ID of hello subnet that you used during hello deployment of hello ASCS/SCS template, or hello ID of hello subnet that was created as part of hello ASCS/SCS template deployment.</span></span>


### <span data-ttu-id="48ca0-489"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="48ca0-489"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure virtual network</span></span>
<span data-ttu-id="48ca0-490">V našem příkladu hello adresním prostoru hello virtuální síť Azure je 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="48ca0-490">In our example, hello address space of hello Azure virtual network is 10.0.0.0/16.</span></span> <span data-ttu-id="48ca0-491">Existuje jedna podsíť s názvem **podsíť**, rozsah adres 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="48ca0-491">There is one subnet called **Subnet**, with an address range of 10.0.0.0/24.</span></span> <span data-ttu-id="48ca0-492">Všechny virtuální počítače a interní služby load balancer nasazených v této virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="48ca0-492">All virtual machines and internal load balancers are deployed in this virtual network.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="48ca0-493">Nemáte nic měnit nastavení sítě toohello uvnitř hello hostovaného operačního systému.</span><span class="sxs-lookup"><span data-stu-id="48ca0-493">Don't make any changes toohello network settings inside hello guest operating system.</span></span> <span data-ttu-id="48ca0-494">To zahrnuje IP adresy, servery DNS a podsítě.</span><span class="sxs-lookup"><span data-stu-id="48ca0-494">This includes IP addresses, DNS servers, and subnet.</span></span> <span data-ttu-id="48ca0-495">Nakonfigurujte všechna nastavení sítě v Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-495">Configure all your network settings in Azure.</span></span> <span data-ttu-id="48ca0-496">Hello služby Dynamic Host Configuration Protocol (DHCP) rozšíří nastavení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-496">hello Dynamic Host Configuration Protocol (DHCP) service propagates your settings.</span></span>
>
>

### <span data-ttu-id="48ca0-497"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS IP adresy</span><span class="sxs-lookup"><span data-stu-id="48ca0-497"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS IP addresses</span></span>

<span data-ttu-id="48ca0-498">tooset hello požadované že DNS IP adresy, hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="48ca0-498">tooset hello required DNS IP addresses, do hello following steps.</span></span>

1.  <span data-ttu-id="48ca0-499">V portálu Azure, na hello hello **servery DNS** okno, ujistěte se, že virtuální sítě **servery DNS** je možnost nastavena příliš**vlastní DNS**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-499">In hello Azure portal, on hello **DNS servers** blade, make sure that your virtual network **DNS servers** option is set too**Custom DNS**.</span></span>
2.  <span data-ttu-id="48ca0-500">Vyberte nastavení na základě typu hello sítě, které máte.</span><span class="sxs-lookup"><span data-stu-id="48ca0-500">Select your settings based on hello type of network you have.</span></span> <span data-ttu-id="48ca0-501">Další informace najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="48ca0-501">For more information, see hello following resources:</span></span>
    * <span data-ttu-id="48ca0-502">[Připojení k podnikové síti (mezi různými místy)][planning-guide-2.2]: Přidat hello IP adresy serverů DNS místní hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-502">[Corporate network connectivity (cross-premises)][planning-guide-2.2]: Add hello IP addresses of hello on-premises DNS servers.</span></span>  
    <span data-ttu-id="48ca0-503">Můžete rozšířit místní DNS servery toohello virtuálních počítačů, které jsou spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-503">You can extend on-premises DNS servers toohello virtual machines that are running in Azure.</span></span> <span data-ttu-id="48ca0-504">V tomto scénáři, můžete přidat hello IP adresy hello Azure virtuální počítače, na které spouštíte služba DNS hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-504">In that scenario, you can add hello IP addresses of hello Azure virtual machines on which you run hello DNS service.</span></span>
    * <span data-ttu-id="48ca0-505">[Čistě cloudové nasazení][planning-guide-2.1]: nasadit další virtuální počítač, pomocí něhož v hello stejnou instanci virtuální sítě, která slouží jako DNS server.</span><span class="sxs-lookup"><span data-stu-id="48ca0-505">[Cloud-only deployment][planning-guide-2.1]: Deploy an additional virtual machine in hello same Virtual Network instance that serves as a DNS server.</span></span> <span data-ttu-id="48ca0-506">Přidejte hello Azure hello IP adresy virtuálních počítačů, které jste nastavili toorun služba DNS.</span><span class="sxs-lookup"><span data-stu-id="48ca0-506">Add hello IP addresses of hello Azure virtual machines that you've set up toorun DNS service.</span></span>

    ![Obrázek 12: Servery DNS nakonfigurujte pro virtuální síť Azure][sap-ha-guide-figure-3001]

    <span data-ttu-id="48ca0-508">_**Obrázek 12:** konfigurace DNS serverů pro virtuální síť Azure_</span><span class="sxs-lookup"><span data-stu-id="48ca0-508">_**Figure 12:** Configure DNS servers for Azure Virtual Network_</span></span>

  > [!NOTE]
  > <span data-ttu-id="48ca0-509">Pokud změníte hello IP adresy serverů DNS hello, je třeba toorestart hello virtuální počítače Azure tooapply hello změn a šířit hello nových serverů DNS.</span><span class="sxs-lookup"><span data-stu-id="48ca0-509">If you change hello IP addresses of hello DNS servers, you need toorestart hello Azure virtual machines tooapply hello change and propagate hello new DNS servers.</span></span>
  >
  >

<span data-ttu-id="48ca0-510">V našem příkladu hello služba DNS je nainstalovaný a nakonfigurovaný na těchto virtuálních počítačích Windows:</span><span class="sxs-lookup"><span data-stu-id="48ca0-510">In our example, hello DNS service is installed and configured on these Windows virtual machines:</span></span>

| <span data-ttu-id="48ca0-511">Role virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="48ca0-511">Virtual machine role</span></span> | <span data-ttu-id="48ca0-512">Název hostitele virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="48ca0-512">Virtual machine host name</span></span> | <span data-ttu-id="48ca0-513">Název síťové karty</span><span class="sxs-lookup"><span data-stu-id="48ca0-513">Network card name</span></span> | <span data-ttu-id="48ca0-514">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="48ca0-514">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="48ca0-515">První server DNS</span><span class="sxs-lookup"><span data-stu-id="48ca0-515">First DNS server</span></span> |<span data-ttu-id="48ca0-516">domcontr-0</span><span class="sxs-lookup"><span data-stu-id="48ca0-516">domcontr-0</span></span> |<span data-ttu-id="48ca0-517">PR1-seskupování domcontr-0</span><span class="sxs-lookup"><span data-stu-id="48ca0-517">pr1-nic-domcontr-0</span></span> |<span data-ttu-id="48ca0-518">10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="48ca0-518">10.0.0.10</span></span> |
| <span data-ttu-id="48ca0-519">Druhý server DNS</span><span class="sxs-lookup"><span data-stu-id="48ca0-519">Second DNS server</span></span> |<span data-ttu-id="48ca0-520">domcontr-1</span><span class="sxs-lookup"><span data-stu-id="48ca0-520">domcontr-1</span></span> |<span data-ttu-id="48ca0-521">PR1-seskupování domcontr-1</span><span class="sxs-lookup"><span data-stu-id="48ca0-521">pr1-nic-domcontr-1</span></span> |<span data-ttu-id="48ca0-522">10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="48ca0-522">10.0.0.11</span></span> |

### <span data-ttu-id="48ca0-523"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Názvy hostitelů a statické IP adresy pro clusterové instance SAP ASC nebo SCS hello a Clusterové instance databázového systému</span><span class="sxs-lookup"><span data-stu-id="48ca0-523"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Host names and static IP addresses for hello SAP ASCS/SCS clustered instance and DBMS clustered instance</span></span>

<span data-ttu-id="48ca0-524">Pro místní nasazení je třeba tyto názvy vyhrazené hostitele a IP adresy:</span><span class="sxs-lookup"><span data-stu-id="48ca0-524">For on-premises deployment, you need these reserved host names and IP addresses:</span></span>

| <span data-ttu-id="48ca0-525">Název role virtuálních hostitelů</span><span class="sxs-lookup"><span data-stu-id="48ca0-525">Virtual host name role</span></span> | <span data-ttu-id="48ca0-526">Název virtuálního hostitele</span><span class="sxs-lookup"><span data-stu-id="48ca0-526">Virtual host name</span></span> | <span data-ttu-id="48ca0-527">Virtuální statickou IP adresu</span><span class="sxs-lookup"><span data-stu-id="48ca0-527">Virtual static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="48ca0-528">SAP ASC nebo SCS první clusteru virtuální hostitel název (pro správu clusteru)</span><span class="sxs-lookup"><span data-stu-id="48ca0-528">SAP ASCS/SCS first cluster virtual host name (for cluster management)</span></span> |<span data-ttu-id="48ca0-529">PR1. ASC vir</span><span class="sxs-lookup"><span data-stu-id="48ca0-529">pr1-ascs-vir</span></span> |<span data-ttu-id="48ca0-530">10.0.0.42</span><span class="sxs-lookup"><span data-stu-id="48ca0-530">10.0.0.42</span></span> |
| <span data-ttu-id="48ca0-531">Název virtuálního hostitele instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="48ca0-531">SAP ASCS/SCS instance virtual host name</span></span> |<span data-ttu-id="48ca0-532">PR1. ASC sap</span><span class="sxs-lookup"><span data-stu-id="48ca0-532">pr1-ascs-sap</span></span> |<span data-ttu-id="48ca0-533">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="48ca0-533">10.0.0.43</span></span> |
| <span data-ttu-id="48ca0-534">Databázového systému SAP druhý cluster virtuálního hostitele název (Správa clusteru)</span><span class="sxs-lookup"><span data-stu-id="48ca0-534">SAP DBMS second cluster virtual host name (cluster management)</span></span> |<span data-ttu-id="48ca0-535">PR1. databázového systému vir</span><span class="sxs-lookup"><span data-stu-id="48ca0-535">pr1-dbms-vir</span></span> |<span data-ttu-id="48ca0-536">10.0.0.32</span><span class="sxs-lookup"><span data-stu-id="48ca0-536">10.0.0.32</span></span> |

<span data-ttu-id="48ca0-537">Při vytváření clusteru hello vytvořit hello názvy virtuálních hostitelů **pr1. ASC vir** a **pr1. databázového systému vir** a hello přidružené IP adresy, které spravují samotného clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-537">When you create hello cluster, create hello virtual host names **pr1-ascs-vir** and **pr1-dbms-vir** and hello associated IP addresses that manage hello cluster itself.</span></span> <span data-ttu-id="48ca0-538">Informace o toodo, najdete v tématu [shromažďovat uzly clusteru v konfiguraci clusteru][sap-ha-guide-8.12.1].</span><span class="sxs-lookup"><span data-stu-id="48ca0-538">For information about how toodo this, see [Collect cluster nodes in a cluster configuration][sap-ha-guide-8.12.1].</span></span>

<span data-ttu-id="48ca0-539">Můžete ručně vytvořit hello další dva názvy virtuálních hostitelů **pr1. ASC sap** a **pr1. databázového systému sap**, a hello přidružené IP adresy, na serveru DNS hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-539">You can manually create hello other two virtual host names, **pr1-ascs-sap** and **pr1-dbms-sap**, and hello associated IP addresses, on hello DNS server.</span></span> <span data-ttu-id="48ca0-540">Hello Clusterové instance SAP ASC nebo SCS a instance databázového systému hello v clusteru používat tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="48ca0-540">hello clustered SAP ASCS/SCS instance and hello clustered DBMS instance use these resources.</span></span> <span data-ttu-id="48ca0-541">Informace o toodo, najdete v tématu [vytvořte název virtuálního hostitele pro clusterovou instanci SAP ASC nebo SCS][sap-ha-guide-9.1.1].</span><span class="sxs-lookup"><span data-stu-id="48ca0-541">For information about how toodo this, see [Create a virtual host name for a clustered SAP ASCS/SCS instance][sap-ha-guide-9.1.1].</span></span>

### <span data-ttu-id="48ca0-542"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Nastavení statické IP adresy pro virtuální počítače, hello SAP</span><span class="sxs-lookup"><span data-stu-id="48ca0-542"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> Set static IP addresses for hello SAP virtual machines</span></span>
<span data-ttu-id="48ca0-543">Poté, co nasadíte toouse hello virtuální počítače v clusteru, musíte tooset statické IP adresy pro všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="48ca0-543">After you deploy hello virtual machines toouse in your cluster, you need tooset static IP addresses for all virtual machines.</span></span> <span data-ttu-id="48ca0-544">To udělat v hello konfigurace Azure Virtual Network a není v hello hostovaného operačního systému.</span><span class="sxs-lookup"><span data-stu-id="48ca0-544">Do this in hello Azure Virtual Network configuration, and not in hello guest operating system.</span></span>

1.  <span data-ttu-id="48ca0-545">V hello portálu Azure, vyberte **skupiny prostředků** > **síťové karty** > **nastavení** > **IP adresa** .</span><span class="sxs-lookup"><span data-stu-id="48ca0-545">In hello Azure portal, select **Resource Group** > **Network Card** > **Settings** > **IP Address**.</span></span>
2.  <span data-ttu-id="48ca0-546">Na hello **IP adresy** okno, v části **přiřazení**, vyberte **statické**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-546">On hello **IP addresses** blade, under **Assignment**, select **Static**.</span></span> <span data-ttu-id="48ca0-547">V hello **IP adresu** zadejte hello IP adresu, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="48ca0-547">In hello **IP address** box, enter hello IP address that you want toouse.</span></span>

  > [!NOTE]
  > <span data-ttu-id="48ca0-548">Pokud změníte hello IP adresu hello síťové karty, je třeba toorestart hello virtuální počítače Azure tooapply hello změnu.</span><span class="sxs-lookup"><span data-stu-id="48ca0-548">If you change hello IP address of hello network card, you need toorestart hello Azure virtual machines tooapply hello change.</span></span>  
  >
  >

  ![Obrázek 13: Nastavení statické IP adresy pro síťové karty hello každého virtuálního počítače][sap-ha-guide-figure-3002]

  <span data-ttu-id="48ca0-550">_**Obrázek 13:** nastavení statické IP adresy pro síťové karty hello každého virtuálního počítače_</span><span class="sxs-lookup"><span data-stu-id="48ca0-550">_**Figure 13:** Set static IP addresses for hello network card of each virtual machine_</span></span>

  <span data-ttu-id="48ca0-551">Opakujte tento krok pro všechna síťová rozhraní, který je pro všechny virtuální počítače, včetně virtuálních počítačů má toouse služby Active Directory a DNS.</span><span class="sxs-lookup"><span data-stu-id="48ca0-551">Repeat this step for all network interfaces, that is, for all virtual machines, including virtual machines that you want toouse for your Active Directory/DNS service.</span></span>

<span data-ttu-id="48ca0-552">V našem příkladu máme tyto virtuální počítače a statické IP adresy:</span><span class="sxs-lookup"><span data-stu-id="48ca0-552">In our example, we have these virtual machines and static IP addresses:</span></span>

| <span data-ttu-id="48ca0-553">Role virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="48ca0-553">Virtual machine role</span></span> | <span data-ttu-id="48ca0-554">Název hostitele virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="48ca0-554">Virtual machine host name</span></span> | <span data-ttu-id="48ca0-555">Název síťové karty</span><span class="sxs-lookup"><span data-stu-id="48ca0-555">Network card name</span></span> | <span data-ttu-id="48ca0-556">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="48ca0-556">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="48ca0-557">První instance aplikačního serveru SAP</span><span class="sxs-lookup"><span data-stu-id="48ca0-557">First SAP Application Server instance</span></span> |<span data-ttu-id="48ca0-558">PR1-di-0</span><span class="sxs-lookup"><span data-stu-id="48ca0-558">pr1-di-0</span></span> |<span data-ttu-id="48ca0-559">PR1-seskupování di-0</span><span class="sxs-lookup"><span data-stu-id="48ca0-559">pr1-nic-di-0</span></span> |<span data-ttu-id="48ca0-560">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="48ca0-560">10.0.0.50</span></span> |
| <span data-ttu-id="48ca0-561">Druhá instance aplikačního serveru SAP</span><span class="sxs-lookup"><span data-stu-id="48ca0-561">Second SAP Application Server instance</span></span> |<span data-ttu-id="48ca0-562">PR1-di-1</span><span class="sxs-lookup"><span data-stu-id="48ca0-562">pr1-di-1</span></span> |<span data-ttu-id="48ca0-563">PR1-seskupování di-1</span><span class="sxs-lookup"><span data-stu-id="48ca0-563">pr1-nic-di-1</span></span> |<span data-ttu-id="48ca0-564">10.0.0.51</span><span class="sxs-lookup"><span data-stu-id="48ca0-564">10.0.0.51</span></span> |
| <span data-ttu-id="48ca0-565">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="48ca0-565">...</span></span> |<span data-ttu-id="48ca0-566">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="48ca0-566">...</span></span> |<span data-ttu-id="48ca0-567">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="48ca0-567">...</span></span> |<span data-ttu-id="48ca0-568">Tlačítka ...</span><span class="sxs-lookup"><span data-stu-id="48ca0-568">...</span></span> |
| <span data-ttu-id="48ca0-569">Poslední instance aplikačního serveru SAP</span><span class="sxs-lookup"><span data-stu-id="48ca0-569">Last SAP Application Server instance</span></span> |<span data-ttu-id="48ca0-570">PR1-di-5</span><span class="sxs-lookup"><span data-stu-id="48ca0-570">pr1-di-5</span></span> |<span data-ttu-id="48ca0-571">PR1 seskupování di 5</span><span class="sxs-lookup"><span data-stu-id="48ca0-571">pr1-nic-di-5</span></span> |<span data-ttu-id="48ca0-572">10.0.0.55</span><span class="sxs-lookup"><span data-stu-id="48ca0-572">10.0.0.55</span></span> |
| <span data-ttu-id="48ca0-573">Prvním uzlu clusteru pro instanci ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="48ca0-573">First cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="48ca0-574">PR1-ASC-0</span><span class="sxs-lookup"><span data-stu-id="48ca0-574">pr1-ascs-0</span></span> |<span data-ttu-id="48ca0-575">PR1-seskupování ASC-0</span><span class="sxs-lookup"><span data-stu-id="48ca0-575">pr1-nic-ascs-0</span></span> |<span data-ttu-id="48ca0-576">10.0.0.40</span><span class="sxs-lookup"><span data-stu-id="48ca0-576">10.0.0.40</span></span> |
| <span data-ttu-id="48ca0-577">Druhý uzel clusteru pro instanci ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="48ca0-577">Second cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="48ca0-578">PR1-ASC-1</span><span class="sxs-lookup"><span data-stu-id="48ca0-578">pr1-ascs-1</span></span> |<span data-ttu-id="48ca0-579">PR1-seskupování ASC-1</span><span class="sxs-lookup"><span data-stu-id="48ca0-579">pr1-nic-ascs-1</span></span> |<span data-ttu-id="48ca0-580">10.0.0.41</span><span class="sxs-lookup"><span data-stu-id="48ca0-580">10.0.0.41</span></span> |
| <span data-ttu-id="48ca0-581">Prvním uzlu clusteru pro instanci databázového systému</span><span class="sxs-lookup"><span data-stu-id="48ca0-581">First cluster node for DBMS instance</span></span> |<span data-ttu-id="48ca0-582">PR1-db-0</span><span class="sxs-lookup"><span data-stu-id="48ca0-582">pr1-db-0</span></span> |<span data-ttu-id="48ca0-583">PR1-seskupování db-0</span><span class="sxs-lookup"><span data-stu-id="48ca0-583">pr1-nic-db-0</span></span> |<span data-ttu-id="48ca0-584">10.0.0.30</span><span class="sxs-lookup"><span data-stu-id="48ca0-584">10.0.0.30</span></span> |
| <span data-ttu-id="48ca0-585">Druhý uzel clusteru pro instanci databázového systému</span><span class="sxs-lookup"><span data-stu-id="48ca0-585">Second cluster node for DBMS instance</span></span> |<span data-ttu-id="48ca0-586">PR1-db-1</span><span class="sxs-lookup"><span data-stu-id="48ca0-586">pr1-db-1</span></span> |<span data-ttu-id="48ca0-587">PR1-seskupování db-1</span><span class="sxs-lookup"><span data-stu-id="48ca0-587">pr1-nic-db-1</span></span> |<span data-ttu-id="48ca0-588">10.0.0.31</span><span class="sxs-lookup"><span data-stu-id="48ca0-588">10.0.0.31</span></span> |

### <span data-ttu-id="48ca0-589"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Nastavení statické IP adresy pro vyrovnávání zatížení Azure interní hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-589"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Set a static IP address for hello Azure internal load balancer</span></span>

<span data-ttu-id="48ca0-590">Vytvoří Hello SAP Azure Resource Manager šablona pro vyrovnávání zatížení Azure interní, který se používá pro hello SAP ASC nebo SCS instance clusteru a clusteru hello databázového systému.</span><span class="sxs-lookup"><span data-stu-id="48ca0-590">hello SAP Azure Resource Manager template creates an Azure internal load balancer that is used for hello SAP ASCS/SCS instance cluster and hello DBMS cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="48ca0-591">Hello IP adresu název virtuálního hostitele hello hello je SAP ASC nebo SCS hello stejné jako IP adresa hello hello SAP ASC nebo SCS interní vyrovnávání zátěže: **pr1-lb Asc**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-591">hello IP address of hello virtual host name of hello SAP ASCS/SCS is hello same as hello IP address of hello SAP ASCS/SCS internal load balancer: **pr1-lb-ascs**.</span></span>
> <span data-ttu-id="48ca0-592">Hello IP adresu hello virtuální název tohoto hello databázového systému je hello stejné jako IP adresa hello hello databázového systému interní vyrovnávání zátěže: **databázového systému pr1 lb**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-592">hello IP address of hello virtual name of hello DBMS is hello same as hello IP address of hello DBMS internal load balancer: **pr1-lb-dbms**.</span></span>
>
>

<span data-ttu-id="48ca0-593">tooset statickou IP adresu pro hello Azure interní nástroj pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="48ca0-593">tooset a static IP address for hello Azure internal load balancer:</span></span>

1.  <span data-ttu-id="48ca0-594">Hello počáteční nasazení nastaví IP adresa služby Vyrovnávání zatížení pro vnitřní hello příliš**dynamické**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-594">hello initial deployment sets hello internal load balancer IP address too**Dynamic**.</span></span> <span data-ttu-id="48ca0-595">V portálu Azure, na hello hello **IP adresy** okno, v části **přiřazení**, vyberte **statické**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-595">In hello Azure portal, on hello **IP addresses** blade, under **Assignment**, select **Static**.</span></span>
2.  <span data-ttu-id="48ca0-596">Nastavení hello IP adresy služby Vyrovnávání zatížení interní hello **pr1-lb Asc** toohello IP adresu název virtuálního hostitele hello instance SAP ASC nebo SCS hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-596">Set hello IP address of hello internal load balancer **pr1-lb-ascs** toohello IP address of hello virtual host name of hello SAP ASCS/SCS instance.</span></span>
3.  <span data-ttu-id="48ca0-597">Nastavení hello IP adresy služby Vyrovnávání zatížení interní hello **databázového systému pr1 lb** toohello IP adresu název virtuálního hostitele hello hello instance databázového systému.</span><span class="sxs-lookup"><span data-stu-id="48ca0-597">Set hello IP address of hello internal load balancer **pr1-lb-dbms** toohello IP address of hello virtual host name of hello DBMS instance.</span></span>

  ![Obrázek 14: Nastavení statické IP adresy pro službu Vyrovnávání zatížení interní hello instance SAP ASC nebo SCS hello][sap-ha-guide-figure-3003]

  <span data-ttu-id="48ca0-599">_**Obrázek 14:** nastavení statické IP adresy pro službu Vyrovnávání zatížení interní hello instance SAP ASC nebo SCS hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-599">_**Figure 14:** Set static IP addresses for hello internal load balancer for hello SAP ASCS/SCS instance_</span></span>

<span data-ttu-id="48ca0-600">V našem příkladu máme dvě Azure interní služby load balancer, které mají tyto statické IP adresy:</span><span class="sxs-lookup"><span data-stu-id="48ca0-600">In our example, we have two Azure internal load balancers that have these static IP addresses:</span></span>

| <span data-ttu-id="48ca0-601">Role služby Vyrovnávání zatížení Azure interní</span><span class="sxs-lookup"><span data-stu-id="48ca0-601">Azure internal load balancer role</span></span> | <span data-ttu-id="48ca0-602">Název nástroje pro vyrovnávání zatížení Azure interní</span><span class="sxs-lookup"><span data-stu-id="48ca0-602">Azure internal load balancer name</span></span> | <span data-ttu-id="48ca0-603">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="48ca0-603">Static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="48ca0-604">SAP ASC nebo SCS instanci interní zátěže.</span><span class="sxs-lookup"><span data-stu-id="48ca0-604">SAP ASCS/SCS instance internal load balancer</span></span> |<span data-ttu-id="48ca0-605">PR1-lb ASC</span><span class="sxs-lookup"><span data-stu-id="48ca0-605">pr1-lb-ascs</span></span> |<span data-ttu-id="48ca0-606">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="48ca0-606">10.0.0.43</span></span> |
| <span data-ttu-id="48ca0-607">Databázového systému SAP interní nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="48ca0-607">SAP DBMS internal load balancer</span></span> |<span data-ttu-id="48ca0-608">PR1-lb-databázového systému</span><span class="sxs-lookup"><span data-stu-id="48ca0-608">pr1-lb-dbms</span></span> |<span data-ttu-id="48ca0-609">10.0.0.33</span><span class="sxs-lookup"><span data-stu-id="48ca0-609">10.0.0.33</span></span> |


### <span data-ttu-id="48ca0-610"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Výchozí pravidla pro vyrovnávání zatížení Azure interní hello Vyrovnávání zatížení ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="48ca0-610"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Default ASCS/SCS load balancing rules for hello Azure internal load balancer</span></span>

<span data-ttu-id="48ca0-611">Hello SAP Azure Resource Manager šablona vytváří hello porty, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="48ca0-611">hello SAP Azure Resource Manager template creates hello ports you need:</span></span>
* <span data-ttu-id="48ca0-612">Instance ABAP ASC s hello výchozí instance číslo **00**</span><span class="sxs-lookup"><span data-stu-id="48ca0-612">An ABAP ASCS instance, with hello default instance number **00**</span></span>
* <span data-ttu-id="48ca0-613">Instance Java SCS, s hello výchozí instance číslo **01**</span><span class="sxs-lookup"><span data-stu-id="48ca0-613">A Java SCS instance, with hello default instance number **01**</span></span>

<span data-ttu-id="48ca0-614">Pokud nainstalujete instanci SAP ASC nebo SCS, je nutné použít hello výchozí instance číslo **00** pro vaše číslo ABAP ASC instance a hello výchozí instance **01** instance Java SCS.</span><span class="sxs-lookup"><span data-stu-id="48ca0-614">When you install your SAP ASCS/SCS instance, you must use hello default instance number **00** for your ABAP ASCS instance and hello default instance number **01** for your Java SCS instance.</span></span>

<span data-ttu-id="48ca0-615">Dále vytvořte požadované interní koncové body pro hello SAP NetWeaver porty pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-615">Next, create required internal load balancing endpoints for hello SAP NetWeaver ports.</span></span>

<span data-ttu-id="48ca0-616">toocreate požadované interní služby load vyrovnávání koncových bodů, nejprve vytvořte tyto koncové body pro hello SAP NetWeaver ABAP ASC porty pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="48ca0-616">toocreate required internal load balancing endpoints, first, create these load balancing endpoints for hello SAP NetWeaver ABAP ASCS ports:</span></span>

| <span data-ttu-id="48ca0-617">Název pravidla vyrovnávání služby nebo zatížení</span><span class="sxs-lookup"><span data-stu-id="48ca0-617">Service/load balancing rule name</span></span> | <span data-ttu-id="48ca0-618">Výchozí čísla portů</span><span class="sxs-lookup"><span data-stu-id="48ca0-618">Default port numbers</span></span> | <span data-ttu-id="48ca0-619">Konkrétní porty (ASC instance číslem instance 00) (YBRAT s 10)</span><span class="sxs-lookup"><span data-stu-id="48ca0-619">Concrete ports for (ASCS instance with instance number 00) (ERS with 10)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="48ca0-620">Zařadit Server / *lbrule3200*</span><span class="sxs-lookup"><span data-stu-id="48ca0-620">Enqueue Server / *lbrule3200*</span></span> |<span data-ttu-id="48ca0-621">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="48ca0-621">32<*InstanceNumber*></span></span> |<span data-ttu-id="48ca0-622">3200</span><span class="sxs-lookup"><span data-stu-id="48ca0-622">3200</span></span> |
| <span data-ttu-id="48ca0-623">Server zpráv ABAP / *lbrule3600*</span><span class="sxs-lookup"><span data-stu-id="48ca0-623">ABAP Message Server / *lbrule3600*</span></span> |<span data-ttu-id="48ca0-624">36 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="48ca0-624">36<*InstanceNumber*></span></span> |<span data-ttu-id="48ca0-625">3600</span><span class="sxs-lookup"><span data-stu-id="48ca0-625">3600</span></span> |
| <span data-ttu-id="48ca0-626">Zpráva Vnitřní ABAP / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="48ca0-626">Internal ABAP Message / *lbrule3900*</span></span> |<span data-ttu-id="48ca0-627">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="48ca0-627">39<*InstanceNumber*></span></span> |<span data-ttu-id="48ca0-628">3900</span><span class="sxs-lookup"><span data-stu-id="48ca0-628">3900</span></span> |
| <span data-ttu-id="48ca0-629">Server HTTP zpráv nebo *Lbrule8100*</span><span class="sxs-lookup"><span data-stu-id="48ca0-629">Message Server HTTP / *Lbrule8100*</span></span> |<span data-ttu-id="48ca0-630">81 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="48ca0-630">81<*InstanceNumber*></span></span> |<span data-ttu-id="48ca0-631">8100</span><span class="sxs-lookup"><span data-stu-id="48ca0-631">8100</span></span> |
| <span data-ttu-id="48ca0-632">SAP spuštění služby ASC HTTP / *Lbrule50013*</span><span class="sxs-lookup"><span data-stu-id="48ca0-632">SAP Start Service ASCS HTTP / *Lbrule50013*</span></span> |<span data-ttu-id="48ca0-633">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="48ca0-633">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="48ca0-634">50013</span><span class="sxs-lookup"><span data-stu-id="48ca0-634">50013</span></span> |
| <span data-ttu-id="48ca0-635">SAP spuštění služby ASC HTTPS nebo *Lbrule50014*</span><span class="sxs-lookup"><span data-stu-id="48ca0-635">SAP Start Service ASCS HTTPS / *Lbrule50014*</span></span> |<span data-ttu-id="48ca0-636">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="48ca0-636">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="48ca0-637">50014</span><span class="sxs-lookup"><span data-stu-id="48ca0-637">50014</span></span> |
| <span data-ttu-id="48ca0-638">Zařazování replikace nebo *Lbrule50016*</span><span class="sxs-lookup"><span data-stu-id="48ca0-638">Enqueue Replication / *Lbrule50016*</span></span> |<span data-ttu-id="48ca0-639">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="48ca0-639">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="48ca0-640">50016</span><span class="sxs-lookup"><span data-stu-id="48ca0-640">50016</span></span> |
| <span data-ttu-id="48ca0-641">SAP spuštění služby YBRAT HTTP *Lbrule51013*</span><span class="sxs-lookup"><span data-stu-id="48ca0-641">SAP Start Service ERS HTTP *Lbrule51013*</span></span> |<span data-ttu-id="48ca0-642">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="48ca0-642">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="48ca0-643">51013</span><span class="sxs-lookup"><span data-stu-id="48ca0-643">51013</span></span> |
| <span data-ttu-id="48ca0-644">SAP spuštění služby YBRAT HTTP *Lbrule51014*</span><span class="sxs-lookup"><span data-stu-id="48ca0-644">SAP Start Service ERS HTTP *Lbrule51014*</span></span> |<span data-ttu-id="48ca0-645">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="48ca0-645">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="48ca0-646">51014</span><span class="sxs-lookup"><span data-stu-id="48ca0-646">51014</span></span> |
| <span data-ttu-id="48ca0-647">Win RM *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="48ca0-647">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="48ca0-648">5985</span><span class="sxs-lookup"><span data-stu-id="48ca0-648">5985</span></span> |
| <span data-ttu-id="48ca0-649">Sdílení souborů *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="48ca0-649">File Share *Lbrule445*</span></span> | |<span data-ttu-id="48ca0-650">445</span><span class="sxs-lookup"><span data-stu-id="48ca0-650">445</span></span> |

<span data-ttu-id="48ca0-651">_**Tabulka 1:** portu čísla instance SAP NetWeaver ABAP ASC hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-651">_**Table 1:** Port numbers of hello SAP NetWeaver ABAP ASCS instances_</span></span>

<span data-ttu-id="48ca0-652">Pak vytvořte tyto koncové body pro hello SAP NetWeaver Java SCS porty pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="48ca0-652">Then, create these load balancing endpoints for hello SAP NetWeaver Java SCS ports:</span></span>

| <span data-ttu-id="48ca0-653">Název pravidla vyrovnávání služby nebo zatížení</span><span class="sxs-lookup"><span data-stu-id="48ca0-653">Service/load balancing rule name</span></span> | <span data-ttu-id="48ca0-654">Výchozí čísla portů</span><span class="sxs-lookup"><span data-stu-id="48ca0-654">Default port numbers</span></span> | <span data-ttu-id="48ca0-655">Konkrétní porty (SCS instance číslem instance 01) (YBRAT s 11)</span><span class="sxs-lookup"><span data-stu-id="48ca0-655">Concrete ports for (SCS instance with instance number 01) (ERS with 11)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="48ca0-656">Zařadit Server / *lbrule3201*</span><span class="sxs-lookup"><span data-stu-id="48ca0-656">Enqueue Server / *lbrule3201*</span></span> |<span data-ttu-id="48ca0-657">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="48ca0-657">32<*InstanceNumber*></span></span> |<span data-ttu-id="48ca0-658">3201</span><span class="sxs-lookup"><span data-stu-id="48ca0-658">3201</span></span> |
| <span data-ttu-id="48ca0-659">Server brány nebo *lbrule3301*</span><span class="sxs-lookup"><span data-stu-id="48ca0-659">Gateway Server / *lbrule3301*</span></span> |<span data-ttu-id="48ca0-660">33 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="48ca0-660">33<*InstanceNumber*></span></span> |<span data-ttu-id="48ca0-661">3301</span><span class="sxs-lookup"><span data-stu-id="48ca0-661">3301</span></span> |
| <span data-ttu-id="48ca0-662">Server zpráv Java / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="48ca0-662">Java Message Server / *lbrule3900*</span></span> |<span data-ttu-id="48ca0-663">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="48ca0-663">39<*InstanceNumber*></span></span> |<span data-ttu-id="48ca0-664">3901</span><span class="sxs-lookup"><span data-stu-id="48ca0-664">3901</span></span> |
| <span data-ttu-id="48ca0-665">Server HTTP zpráv nebo *Lbrule8101*</span><span class="sxs-lookup"><span data-stu-id="48ca0-665">Message Server HTTP / *Lbrule8101*</span></span> |<span data-ttu-id="48ca0-666">81 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="48ca0-666">81<*InstanceNumber*></span></span> |<span data-ttu-id="48ca0-667">8101</span><span class="sxs-lookup"><span data-stu-id="48ca0-667">8101</span></span> |
| <span data-ttu-id="48ca0-668">SAP spuštění služby SCS HTTP / *Lbrule50113*</span><span class="sxs-lookup"><span data-stu-id="48ca0-668">SAP Start Service SCS HTTP / *Lbrule50113*</span></span> |<span data-ttu-id="48ca0-669">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="48ca0-669">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="48ca0-670">50113</span><span class="sxs-lookup"><span data-stu-id="48ca0-670">50113</span></span> |
| <span data-ttu-id="48ca0-671">SAP spuštění služby SCS HTTPS nebo *Lbrule50114*</span><span class="sxs-lookup"><span data-stu-id="48ca0-671">SAP Start Service SCS HTTPS / *Lbrule50114*</span></span> |<span data-ttu-id="48ca0-672">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="48ca0-672">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="48ca0-673">50114</span><span class="sxs-lookup"><span data-stu-id="48ca0-673">50114</span></span> |
| <span data-ttu-id="48ca0-674">Zařazování replikace nebo *Lbrule50116*</span><span class="sxs-lookup"><span data-stu-id="48ca0-674">Enqueue Replication / *Lbrule50116*</span></span> |<span data-ttu-id="48ca0-675">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="48ca0-675">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="48ca0-676">50116</span><span class="sxs-lookup"><span data-stu-id="48ca0-676">50116</span></span> |
| <span data-ttu-id="48ca0-677">SAP spuštění služby YBRAT HTTP *Lbrule51113*</span><span class="sxs-lookup"><span data-stu-id="48ca0-677">SAP Start Service ERS HTTP *Lbrule51113*</span></span> |<span data-ttu-id="48ca0-678">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="48ca0-678">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="48ca0-679">51113</span><span class="sxs-lookup"><span data-stu-id="48ca0-679">51113</span></span> |
| <span data-ttu-id="48ca0-680">SAP spuštění služby YBRAT HTTP *Lbrule51114*</span><span class="sxs-lookup"><span data-stu-id="48ca0-680">SAP Start Service ERS HTTP *Lbrule51114*</span></span> |<span data-ttu-id="48ca0-681">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="48ca0-681">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="48ca0-682">51114</span><span class="sxs-lookup"><span data-stu-id="48ca0-682">51114</span></span> |
| <span data-ttu-id="48ca0-683">Win RM *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="48ca0-683">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="48ca0-684">5985</span><span class="sxs-lookup"><span data-stu-id="48ca0-684">5985</span></span> |
| <span data-ttu-id="48ca0-685">Sdílení souborů *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="48ca0-685">File Share *Lbrule445*</span></span> | |<span data-ttu-id="48ca0-686">445</span><span class="sxs-lookup"><span data-stu-id="48ca0-686">445</span></span> |

<span data-ttu-id="48ca0-687">_**Tabulka 2:** portu čísla instance SAP NetWeaver Java SCS hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-687">_**Table 2:** Port numbers of hello SAP NetWeaver Java SCS instances_</span></span>

![Obrázek 15: Pravidla pro vyrovnávání zatížení Azure interní hello Vyrovnávání zatížení výchozí ASC nebo SCS][sap-ha-guide-figure-3004]

<span data-ttu-id="48ca0-689">_**Obrázek 15:** ASC nebo SCS výchozí pravidla pro vyrovnávání zatížení Azure interní hello Vyrovnávání zatížení_</span><span class="sxs-lookup"><span data-stu-id="48ca0-689">_**Figure 15:** Default ASCS/SCS load balancing rules for hello Azure internal load balancer_</span></span>

<span data-ttu-id="48ca0-690">Nastavení hello IP adresy služby Vyrovnávání zatížení hello **databázového systému pr1 lb** toohello IP adresu název virtuálního hostitele hello hello instance databázového systému.</span><span class="sxs-lookup"><span data-stu-id="48ca0-690">Set hello IP address of hello load balancer **pr1-lb-dbms** toohello IP address of hello virtual host name of hello DBMS instance.</span></span>

### <span data-ttu-id="48ca0-691"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Změňte pravidla pro vyrovnávání zatížení Azure interní hello Vyrovnávání zatížení pro výchozí hello ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="48ca0-691"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> Change hello ASCS/SCS default load balancing rules for hello Azure internal load balancer</span></span>

<span data-ttu-id="48ca0-692">Pokud chcete pro hello SAP ASC nebo instance SCS toouse různá čísla, je třeba změnit hello názvy a hodnoty jejich porty z výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="48ca0-692">If you want toouse different numbers for hello SAP ASCS or SCS instances, you must change hello names and values of their ports from default values.</span></span>

1.  <span data-ttu-id="48ca0-693">V hello portálu Azure, vyberte  **<* SID*> - lb - ASC načíst vyrovnávání ** > **pravidla Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-693">In hello Azure portal, select **<*SID*>-lb-ascs load balancer** > **Load Balancing Rules**.</span></span>
2.  <span data-ttu-id="48ca0-694">Pro všechna pravidla, které patří toohello SAP ASC nebo SCS instance Vyrovnávání zatížení změňte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="48ca0-694">For all load balancing rules that belong toohello SAP ASCS or SCS instance, change these values:</span></span>

  * <span data-ttu-id="48ca0-695">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="48ca0-695">Name</span></span>
  * <span data-ttu-id="48ca0-696">Port</span><span class="sxs-lookup"><span data-stu-id="48ca0-696">Port</span></span>
  * <span data-ttu-id="48ca0-697">Back-end port</span><span class="sxs-lookup"><span data-stu-id="48ca0-697">Back-end port</span></span>

  <span data-ttu-id="48ca0-698">Například pokud chcete, aby toochange hello výchozí ASC instance číslo od 00 too31, musíte změny hello toomake pro všechny porty uvedené v tabulce 1.</span><span class="sxs-lookup"><span data-stu-id="48ca0-698">For example, if you want toochange hello default ASCS instance number from 00 too31, you need toomake hello changes for all ports listed in Table 1.</span></span>

  <span data-ttu-id="48ca0-699">Tady je příklad aktualizace pro port *lbrule3200*.</span><span class="sxs-lookup"><span data-stu-id="48ca0-699">Here's an example of an update for port *lbrule3200*.</span></span>

  ![Obrázek 16: Pravidla pro vyrovnávání zatížení Azure interní hello Vyrovnávání zatížení pro výchozí hello ASC nebo SCS změňte][sap-ha-guide-figure-3005]

  <span data-ttu-id="48ca0-701">_**Obrázek 16:** změnu hello ASC nebo SCS výchozí Vyrovnávání zatížení, pravidla pro vyrovnávání zatížení Azure interní hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-701">_**Figure 16:** Change hello ASCS/SCS default load balancing rules for hello Azure internal load balancer_</span></span>

### <span data-ttu-id="48ca0-702"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Přidání domény toohello virtuální počítače Windows</span><span class="sxs-lookup"><span data-stu-id="48ca0-702"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Add Windows virtual machines toohello domain</span></span>

<span data-ttu-id="48ca0-703">Po přiřazení počítačů toohello virtuální sady statických IP adres, přidejte domény toohello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="48ca0-703">After you assign a static IP address toohello virtual machines, add hello virtual machines toohello domain.</span></span>

![Obrázek 17: Přidání domény tooa virtuálního počítače][sap-ha-guide-figure-3006]

<span data-ttu-id="48ca0-705">_**Obrázek 17:** přidat doménu tooa virtuálního počítače_</span><span class="sxs-lookup"><span data-stu-id="48ca0-705">_**Figure 17:** Add a virtual machine tooa domain_</span></span>

### <span data-ttu-id="48ca0-706"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Přidat položky registru na obou uzlů clusteru instance SAP ASC nebo SCS hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-706"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> Add registry entries on both cluster nodes of hello SAP ASCS/SCS instance</span></span>

<span data-ttu-id="48ca0-707">Azure Vyrovnávání zatížení má interní nástroj, zavře připojení při hello připojení jsou nastavte dobu nečinnosti, po času (časový limit nečinnosti).</span><span class="sxs-lookup"><span data-stu-id="48ca0-707">Azure Load Balancer has an internal load balancer that closes connections when hello connections are idle for a set period of time (an idle timeout).</span></span> <span data-ttu-id="48ca0-708">SAP pracovních procesů v dialogovém okně instancí otevřená připojení toohello SAP zařazování zpracovat co nejrychleji hello první zařazování nebo vyřazení z fronty požadavků toobe potřeby odeslat.</span><span class="sxs-lookup"><span data-stu-id="48ca0-708">SAP work processes in dialog instances open connections toohello SAP enqueue process as soon as hello first enqueue/dequeue request needs toobe sent.</span></span> <span data-ttu-id="48ca0-709">Tato připojení obvykle zůstat zavedené do hello pracovní proces nebo hello zařazování proces restartuje.</span><span class="sxs-lookup"><span data-stu-id="48ca0-709">These connections usually remain established until hello work process or hello enqueue process restarts.</span></span> <span data-ttu-id="48ca0-710">Je-li připojení hello na nastavenou dobu nečinnosti, ale hello připojení hello zavře nástroj pro vyrovnávání zatížení Azure interní.</span><span class="sxs-lookup"><span data-stu-id="48ca0-710">However, if hello connection is idle for a set period of time, hello Azure internal load balancer closes hello connections.</span></span> <span data-ttu-id="48ca0-711">To není problém, protože hello SAP pracovní proces obnoví proces zařazování toohello hello připojení, pokud už existuje.</span><span class="sxs-lookup"><span data-stu-id="48ca0-711">This isn't a problem because hello SAP work process reestablishes hello connection toohello enqueue process if it no longer exists.</span></span> <span data-ttu-id="48ca0-712">Tyto aktivity jsou popsané v trasování vývojáře hello procesů SAP, ale uživatel vytvořit velké množství další obsah v těchto trasování.</span><span class="sxs-lookup"><span data-stu-id="48ca0-712">These activities are documented in hello developer traces of SAP processes, but they create a large amount of extra content in those traces.</span></span> <span data-ttu-id="48ca0-713">Je vhodné toochange hello TCP/IP `KeepAliveTime` a `KeepAliveInterval` na obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-713">It's a good idea toochange hello TCP/IP `KeepAliveTime` and `KeepAliveInterval` on both cluster nodes.</span></span> <span data-ttu-id="48ca0-714">Kombinací těchto změnách ve parametry protokolu TCP/IP hello s parametry profil SAP, popsány dále v článku hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-714">Combine these changes in hello TCP/IP parameters with SAP profile parameters, described later in hello article.</span></span>

<span data-ttu-id="48ca0-715">položky registru tooadd na obou uzlů clusteru instance SAP ASC nebo SCS hello, nejprve přidejte tyto položky registru systému Windows na obou uzlů clusteru systému Windows pro SAP ASC nebo SCS:</span><span class="sxs-lookup"><span data-stu-id="48ca0-715">tooadd registry entries on both cluster nodes of hello SAP ASCS/SCS instance, first, add these Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="48ca0-716">Cesta</span><span class="sxs-lookup"><span data-stu-id="48ca0-716">Path</span></span> | <span data-ttu-id="48ca0-717">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="48ca0-717">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="48ca0-718">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="48ca0-718">Variable name</span></span> |`KeepAliveTime` |
| <span data-ttu-id="48ca0-719">Typ proměnné</span><span class="sxs-lookup"><span data-stu-id="48ca0-719">Variable type</span></span> |<span data-ttu-id="48ca0-720">REG_DWORD (decimální):</span><span class="sxs-lookup"><span data-stu-id="48ca0-720">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="48ca0-721">Hodnota</span><span class="sxs-lookup"><span data-stu-id="48ca0-721">Value</span></span> |<span data-ttu-id="48ca0-722">120000</span><span class="sxs-lookup"><span data-stu-id="48ca0-722">120000</span></span> |
| <span data-ttu-id="48ca0-723">Odkaz toodocumentation</span><span class="sxs-lookup"><span data-stu-id="48ca0-723">Link toodocumentation</span></span> |[<span data-ttu-id="48ca0-724">https://technet.microsoft.com/en-us/library/cc957549.aspx</span><span class="sxs-lookup"><span data-stu-id="48ca0-724">https://technet.microsoft.com/en-us/library/cc957549.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957549.aspx) |

<span data-ttu-id="48ca0-725">_**Tabulka 3:** změnu hello první parametr TCP/IP_</span><span class="sxs-lookup"><span data-stu-id="48ca0-725">_**Table 3:** Change hello first TCP/IP parameter_</span></span>

<span data-ttu-id="48ca0-726">Pak přidejte této položky registru systému Windows na obou uzlů clusteru systému Windows pro SAP ASC nebo SCS:</span><span class="sxs-lookup"><span data-stu-id="48ca0-726">Then, add this Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="48ca0-727">Cesta</span><span class="sxs-lookup"><span data-stu-id="48ca0-727">Path</span></span> | <span data-ttu-id="48ca0-728">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="48ca0-728">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="48ca0-729">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="48ca0-729">Variable name</span></span> |`KeepAliveInterval` |
| <span data-ttu-id="48ca0-730">Typ proměnné</span><span class="sxs-lookup"><span data-stu-id="48ca0-730">Variable type</span></span> |<span data-ttu-id="48ca0-731">REG_DWORD (decimální):</span><span class="sxs-lookup"><span data-stu-id="48ca0-731">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="48ca0-732">Hodnota</span><span class="sxs-lookup"><span data-stu-id="48ca0-732">Value</span></span> |<span data-ttu-id="48ca0-733">120000</span><span class="sxs-lookup"><span data-stu-id="48ca0-733">120000</span></span> |
| <span data-ttu-id="48ca0-734">Odkaz toodocumentation</span><span class="sxs-lookup"><span data-stu-id="48ca0-734">Link toodocumentation</span></span> |[<span data-ttu-id="48ca0-735">https://technet.microsoft.com/en-us/library/cc957548.aspx</span><span class="sxs-lookup"><span data-stu-id="48ca0-735">https://technet.microsoft.com/en-us/library/cc957548.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

<span data-ttu-id="48ca0-736">_**Tabulka 4:** změnu hello druhý parametr TCP/IP_</span><span class="sxs-lookup"><span data-stu-id="48ca0-736">_**Table 4:** Change hello second TCP/IP parameter_</span></span>

<span data-ttu-id="48ca0-737">**změny hello tooapply, restartujte obou uzlů clusteru**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-737">**tooapply hello changes, restart both cluster nodes**.</span></span>

### <span data-ttu-id="48ca0-738"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Nastavení clusteru Windows Server Failover Clustering pro instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="48ca0-738"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Set up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance</span></span>

<span data-ttu-id="48ca0-739">Nastavení clusteru s podporou služby Windows Server Failover Clustering pro instance SAP ASC nebo SCS zahrnuje tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="48ca0-739">Setting up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="48ca0-740">Shromažďování hello uzly clusteru v konfiguraci clusteru</span><span class="sxs-lookup"><span data-stu-id="48ca0-740">Collecting hello cluster nodes in a cluster configuration</span></span>
- <span data-ttu-id="48ca0-741">Konfigurace určující sdílenou složku clusteru</span><span class="sxs-lookup"><span data-stu-id="48ca0-741">Configuring a cluster file share witness</span></span>

#### <span data-ttu-id="48ca0-742"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Shromažďovat hello uzly clusteru v konfiguraci clusteru</span><span class="sxs-lookup"><span data-stu-id="48ca0-742"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Collect hello cluster nodes in a cluster configuration</span></span>

1.  <span data-ttu-id="48ca0-743">Hello přidat Role a funkce průvodce přidejte clustering tooboth uzly clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="48ca0-743">In hello Add Role and Features Wizard, add failover clustering tooboth cluster nodes.</span></span>
2.  <span data-ttu-id="48ca0-744">Nastavení clusteru převzetí služeb při selhání hello pomocí Správce clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="48ca0-744">Set up hello failover cluster by using Failover Cluster Manager.</span></span> <span data-ttu-id="48ca0-745">Ve Správci clusteru převzetí služeb při selhání vyberte **vytvořením clusteru**a poté přidejte pouze hello název hello první clusteru, uzel A. Nepřidávejte druhého uzlu hello ještě; přidáte hello druhého uzlu do pozdějšího kroku.</span><span class="sxs-lookup"><span data-stu-id="48ca0-745">In Failover Cluster Manager, select **Create Cluster**, and then add only hello name of hello first cluster, node A. Do not add hello second node yet; you'll add hello second node in a later step.</span></span>

  ![Obrázek 18: Přidání hello serveru nebo název virtuálního počítače hello prvního uzlu clusteru][sap-ha-guide-figure-3007]

  <span data-ttu-id="48ca0-747">_**Obrázek 18:** hello přidat server nebo virtuální počítač název prvního uzlu clusteru hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-747">_**Figure 18:** Add hello server or virtual machine name of hello first cluster node_</span></span>

3.  <span data-ttu-id="48ca0-748">Zadejte název sítě hello (název hostitele virtuálního) clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-748">Enter hello network name (virtual host name) of hello cluster.</span></span>

  ![Obrázek 19: Zadejte název clusteru hello][sap-ha-guide-figure-3008]

  <span data-ttu-id="48ca0-750">_**Obrázek 19:** zadejte název clusteru hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-750">_**Figure 19:** Enter hello cluster name_</span></span>

4.  <span data-ttu-id="48ca0-751">Po vytvoření clusteru hello, spusťte test ověření clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-751">After you've created hello cluster, run a cluster validation test.</span></span>

  ![Obrázek 20: Spustit kontrolu ověření clusteru hello][sap-ha-guide-figure-3009]

  <span data-ttu-id="48ca0-753">_**Obrázek 20:** spustit kontrolu pro ověření clusteru hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-753">_**Figure 20:** Run hello cluster validation check_</span></span>

  <span data-ttu-id="48ca0-754">Můžete ignorovat jakékoli upozornění o discích v tomto okamžiku v procesu hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-754">You can ignore any warnings about disks at this point in hello process.</span></span> <span data-ttu-id="48ca0-755">Přidáte, že určující sdílenou složku a hello SIOS sdílené disky později.</span><span class="sxs-lookup"><span data-stu-id="48ca0-755">You'll add a file share witness and hello SIOS shared disks later.</span></span> <span data-ttu-id="48ca0-756">V této fázi není nutný tooworry o jako kvorum.</span><span class="sxs-lookup"><span data-stu-id="48ca0-756">At this stage, you don't need tooworry about having a quorum.</span></span>

  ![Obrázek 21: Nenajde žádný disk kvora][sap-ha-guide-figure-3010]

  <span data-ttu-id="48ca0-758">_**Obrázek 21:** nenajde žádný disk kvora_</span><span class="sxs-lookup"><span data-stu-id="48ca0-758">_**Figure 21:** No quorum disk is found_</span></span>

  ![Obrázek 22: Prostředek clusteru jádra potřebuje novou IP adresu][sap-ha-guide-figure-3011]

  <span data-ttu-id="48ca0-760">_**Obrázek 22:** prostředku clusteru jádra potřebuje novou IP adresu_</span><span class="sxs-lookup"><span data-stu-id="48ca0-760">_**Figure 22:** Core cluster resource needs a new IP address_</span></span>

5.  <span data-ttu-id="48ca0-761">Změna IP adresy hello hello základní Clusterové služby.</span><span class="sxs-lookup"><span data-stu-id="48ca0-761">Change hello IP address of hello core cluster service.</span></span> <span data-ttu-id="48ca0-762">Hello clusteru nelze spustit, dokud změnit IP adresu hello hello základní clusteru služby, protože hello IP adresa serveru hello body tooone uzly hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="48ca0-762">hello cluster can't start until you change hello IP address of hello core cluster service, because hello IP address of hello server points tooone of hello virtual machine nodes.</span></span> <span data-ttu-id="48ca0-763">To udělat na hello **vlastnosti** stránku hello základní Clusterovou službu na IP prostředku.</span><span class="sxs-lookup"><span data-stu-id="48ca0-763">Do this on hello **Properties** page of hello core cluster service's IP resource.</span></span>

  <span data-ttu-id="48ca0-764">Například potřebujeme tooassign IP adresu (v našem příkladu **10.0.0.42**) pro název virtuálního hostitele clusteru hello **pr1. ASC vir**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-764">For example, we need tooassign an IP address (in our example, **10.0.0.42**) for hello cluster virtual host name **pr1-ascs-vir**.</span></span>

  ![Obrázek 23: V dialogové okno Vlastnosti hello, změna hello IP adresy][sap-ha-guide-figure-3012]

  <span data-ttu-id="48ca0-766">_**Obrázek 23:** v hello **vlastnosti** dialogové okno, změna hello IP adresu_</span><span class="sxs-lookup"><span data-stu-id="48ca0-766">_**Figure 23:** In hello **Properties** dialog box, change hello IP address_</span></span>

  ![Obrázek 24: Přiřadit hello IP adresu, která je vyhrazena pro hello cluster][sap-ha-guide-figure-3013]

  <span data-ttu-id="48ca0-768">_**Obrázek 24:** přiřadit hello IP adresu, která je vyhrazena pro hello cluster_</span><span class="sxs-lookup"><span data-stu-id="48ca0-768">_**Figure 24:** Assign hello IP address that is reserved for hello cluster_</span></span>

6.  <span data-ttu-id="48ca0-769">Přepněte hello virtuální hostitel název clusteru online.</span><span class="sxs-lookup"><span data-stu-id="48ca0-769">Bring hello cluster virtual host name online.</span></span>

  ![Obrázek 25: Základní služby clusteru je zapnutý a běží a s hello opravte IP adresu][sap-ha-guide-figure-3014]

  <span data-ttu-id="48ca0-771">_**Obrázek 25:** je základní služba clusteru a spuštěný a s hello opravte IP adresu_</span><span class="sxs-lookup"><span data-stu-id="48ca0-771">_**Figure 25:** Cluster core service is up and running, and with hello correct IP address_</span></span>

7.  <span data-ttu-id="48ca0-772">Přidání druhého uzlu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-772">Add hello second cluster node.</span></span>

  <span data-ttu-id="48ca0-773">Nyní když hello základní Clusterová služba spuštěná, můžete přidat hello druhého uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-773">Now that hello core cluster service is up and running, you can add hello second cluster node.</span></span>

  ![Obrázek 26: Přidání druhého uzlu clusteru hello][sap-ha-guide-figure-3015]

  <span data-ttu-id="48ca0-775">_**Obrázek 26:** druhého uzlu clusteru přidat hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-775">_**Figure 26:** Add hello second cluster node_</span></span>

8.  <span data-ttu-id="48ca0-776">Zadejte název pro hostitele na druhého uzlu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-776">Enter a name for hello second cluster node host.</span></span>

  ![Obrázek 27: Zadejte název hostitele uzlu druhý clusteru hello][sap-ha-guide-figure-3016]

  <span data-ttu-id="48ca0-778">_**Obrázek 27:** Enter hello druhý název uzlu clusteru hostitele_</span><span class="sxs-lookup"><span data-stu-id="48ca0-778">_**Figure 27:** Enter hello second cluster node host name_</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="48ca0-779">Ujistěte se, že hello **přidejte všechna vhodná úložiště toohello cluster** zaškrtávací políčko je **není** vybrané.</span><span class="sxs-lookup"><span data-stu-id="48ca0-779">Be sure that hello **Add all eligible storage toohello cluster** check box is **NOT** selected.</span></span>  
  >
  >

  ![Obrázek 28: Nezaškrtávejte políčko hello][sap-ha-guide-figure-3017]

  <span data-ttu-id="48ca0-781">_**Obrázek 28:** provést **není** hello vyberte zaškrtávací políčko_</span><span class="sxs-lookup"><span data-stu-id="48ca0-781">_**Figure 28:** Do **not** select hello check box_</span></span>

  <span data-ttu-id="48ca0-782">Můžete ignorovat upozornění o kvora a disky.</span><span class="sxs-lookup"><span data-stu-id="48ca0-782">You can ignore warnings about quorum and disks.</span></span> <span data-ttu-id="48ca0-783">Budete nastavit hello disk kvora a sdílenou složku hello později, jak je popsáno v [instalace s DataKeeper Cluster Edition u disku clusteru sdílení, SAP ASC nebo SCS][sap-ha-guide-8.12.3].</span><span class="sxs-lookup"><span data-stu-id="48ca0-783">You'll set hello quorum and share hello disk later, as described in [Installing SIOS DataKeeper Cluster Edition for SAP ASCS/SCS cluster share disk][sap-ha-guide-8.12.3].</span></span>

  ![Obrázek 29: Ignorujte upozornění o hello disku kvora][sap-ha-guide-figure-3018]

  <span data-ttu-id="48ca0-785">_**Obrázek 29:** ignorovat upozornění o hello disku kvora_</span><span class="sxs-lookup"><span data-stu-id="48ca0-785">_**Figure 29:** Ignore warnings about hello disk quorum_</span></span>


#### <span data-ttu-id="48ca0-786"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Konfigurovat určující sdílenou složku clusteru</span><span class="sxs-lookup"><span data-stu-id="48ca0-786"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Configure a cluster file share witness</span></span>

<span data-ttu-id="48ca0-787">Konfigurace určující sdílenou složku clusteru zahrnuje tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="48ca0-787">Configuring a cluster file share witness involves these tasks:</span></span>

- <span data-ttu-id="48ca0-788">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="48ca0-788">Creating a file share</span></span>
- <span data-ttu-id="48ca0-789">Nastavení hello soubor sdílené složky s kopií clusteru kvora ve Správci clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="48ca0-789">Setting hello file share witness quorum in Failover Cluster Manager</span></span>

##### <span data-ttu-id="48ca0-790"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="48ca0-790"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Create a file share</span></span>

1.  <span data-ttu-id="48ca0-791">Vyberte složku s kopií místo disk kvora.</span><span class="sxs-lookup"><span data-stu-id="48ca0-791">Select a file share witness instead of a quorum disk.</span></span> <span data-ttu-id="48ca0-792">Tuto volbu podporuje DataKeeper s.</span><span class="sxs-lookup"><span data-stu-id="48ca0-792">SIOS DataKeeper supports this option.</span></span>

  <span data-ttu-id="48ca0-793">V hello příklady v tomto článku je hello určující sdílenou složku na serveru hello Active Directory a DNS, který běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-793">In hello examples in this article, hello file share witness is on hello Active Directory/DNS server that is running in Azure.</span></span> <span data-ttu-id="48ca0-794">Hello určující sdílená složka se označuje jako **domcontr-0**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-794">hello file share witness is called **domcontr-0**.</span></span> <span data-ttu-id="48ca0-795">Vzhledem k tomu, že byste nakonfigurovali tooAzure připojení VPN (prostřednictvím sítě Site-to-Site VPN nebo Azure ExpressRoute), vaše/DNS služby Active Directory service je na místě a není vhodný toorun soubor sdílet s kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-795">Because you would have configured a VPN connection tooAzure (via Site-to-Site VPN or Azure ExpressRoute), your Active Directory/DNS service is on-premises and isn't suitable toorun a file share witness.</span></span>

  > [!NOTE]
  > <span data-ttu-id="48ca0-796">Pokud služby Active Directory a DNS používá jenom v místě, není konfigurace vaší určující sdílenou složku na hello Active Directory a DNS Windows operační systém, který běží na místě.</span><span class="sxs-lookup"><span data-stu-id="48ca0-796">If your Active Directory/DNS service runs only on-premises, don't configure your file share witness on hello Active Directory/DNS Windows operating system that is running on-premises.</span></span> <span data-ttu-id="48ca0-797">Latence sítě mezi uzly clusteru, které jsou spuštěné v Azure a Active Directory a DNS v místním může být příliš velký a způsobit problémy s připojením.</span><span class="sxs-lookup"><span data-stu-id="48ca0-797">Network latency between cluster nodes running in Azure and Active Directory/DNS on-premises might be too large and cause connectivity issues.</span></span> <span data-ttu-id="48ca0-798">Být jisti tooconfigure hello určující sdílenou složku na virtuální počítač Azure se systémem zavřít toohello uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-798">Be sure tooconfigure hello file share witness on an Azure virtual machine that is running close toohello cluster node.</span></span>  
  >
  >

  <span data-ttu-id="48ca0-799">disk kvora Hello potřebuje alespoň 1 024 MB volného místa.</span><span class="sxs-lookup"><span data-stu-id="48ca0-799">hello quorum drive needs at least 1,024 MB of free space.</span></span> <span data-ttu-id="48ca0-800">Doporučujeme, abyste hodnotu 2 048 MB volného místa pro disk kvora hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-800">We recommend 2,048 MB of free space for hello quorum drive.</span></span>

2.  <span data-ttu-id="48ca0-801">Přidejte objekt názvu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-801">Add hello cluster name object.</span></span>

  ![Obrázek 30: Přiřadit oprávnění hello hello sdílené složky pro objekt názvu clusteru hello][sap-ha-guide-figure-3019]

  <span data-ttu-id="48ca0-803">_**Obrázek 30:** přiřadit oprávnění hello hello sdílené složky pro objekt názvu clusteru hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-803">_**Figure 30:** Assign hello permissions on hello share for hello cluster name object_</span></span>

  <span data-ttu-id="48ca0-804">Ujistěte se, že oprávnění hello zahrnout hello autority toochange data hello sdílené složky pro objekt názvu clusteru hello (v našem příkladu **pr1. ASC vir$**).</span><span class="sxs-lookup"><span data-stu-id="48ca0-804">Be sure that hello permissions include hello authority toochange data in hello share for hello cluster name object (in our example, **pr1-ascs-vir$**).</span></span>

3.  <span data-ttu-id="48ca0-805">tooadd hello clusteru název toohello seznam objektů, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-805">tooadd hello cluster name object toohello list, select **Add**.</span></span> <span data-ttu-id="48ca0-806">Změnit hello toocheck filtru pro počítačových objektů ve toothose přidání znázorňuje obrázek 31.</span><span class="sxs-lookup"><span data-stu-id="48ca0-806">Change hello filter toocheck for computer objects, in addition toothose shown in Figure 31.</span></span>

  ![Obrázek 31: Změna počítače tooinclude hello typy objektů][sap-ha-guide-figure-3020]

  <span data-ttu-id="48ca0-808">_**Obrázek 31:** změnit hello typy objektů tooinclude počítače_</span><span class="sxs-lookup"><span data-stu-id="48ca0-808">_**Figure 31:** Change hello Object Types tooinclude computers_</span></span>

  ![Obrázek 32: Políčko hello počítače][sap-ha-guide-figure-3021]

  <span data-ttu-id="48ca0-810">_**Obrázek 32:** vyberte hello **počítače** zaškrtávací políčko_</span><span class="sxs-lookup"><span data-stu-id="48ca0-810">_**Figure 32:** Select hello **Computers** check box_</span></span>

4.  <span data-ttu-id="48ca0-811">Zadejte objekt názvu clusteru hello jak ukazuje obrázek 31.</span><span class="sxs-lookup"><span data-stu-id="48ca0-811">Enter hello cluster name object as shown in Figure 31.</span></span> <span data-ttu-id="48ca0-812">Protože byl vytvořen záznam hello, můžete změnit hello oprávnění, jak ukazuje obrázek 30.</span><span class="sxs-lookup"><span data-stu-id="48ca0-812">Because hello record has already been created, you can change hello permissions, as shown in Figure 30.</span></span>

5.  <span data-ttu-id="48ca0-813">Vyberte hello **zabezpečení** podrobnější hello sdílenou složku a potom nastavte oprávnění pro objekt názvu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-813">Select hello **Security** tab of hello share, and then set more detailed permissions for hello cluster name object.</span></span>

  ![Obrázek 33: Nastavení atributů hello zabezpečení pro objekt názvu clusteru hello hello souboru sdílenou složku kvora][sap-ha-guide-figure-3022]

  <span data-ttu-id="48ca0-815">_**Obrázek 33:** nastavit atributy hello zabezpečení pro objekt názvu clusteru hello na hello souboru sdílenou složku kvora_</span><span class="sxs-lookup"><span data-stu-id="48ca0-815">_**Figure 33:** Set hello security attributes for hello cluster name object on hello file share quorum_</span></span>

##### <span data-ttu-id="48ca0-816"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Nastavení hello soubor sdílené složky s kopií clusteru kvora ve Správci clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="48ca0-816"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Set hello file share witness quorum in Failover Cluster Manager</span></span>

1.  <span data-ttu-id="48ca0-817">Otevřete hello Průvodce nastavení konfigurace kvora.</span><span class="sxs-lookup"><span data-stu-id="48ca0-817">Open hello Configure Quorum Setting Wizard.</span></span>

  ![Obrázek 34: Spuštění hello konfigurace Průvodce nastavení kvora clusteru][sap-ha-guide-figure-3023]

  <span data-ttu-id="48ca0-819">_**Obrázek 34:** počáteční hello Průvodce nastavení konfigurace kvora clusteru_</span><span class="sxs-lookup"><span data-stu-id="48ca0-819">_**Figure 34:** Start hello Configure Cluster Quorum Setting Wizard_</span></span>

2.  <span data-ttu-id="48ca0-820">Na hello **vyberte konfiguraci kvora** vyberte **vybrat určující disk kvora hello**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-820">On hello **Select Quorum Configuration** page, select **Select hello quorum witness**.</span></span>

  ![Obrázek 35: Konfigurací kvora, které můžete vybrat z][sap-ha-guide-figure-3024]

  <span data-ttu-id="48ca0-822">_**Obrázek 35:** konfigurací kvora, můžete si vybrat z_</span><span class="sxs-lookup"><span data-stu-id="48ca0-822">_**Figure 35:** Quorum configurations you can choose from_</span></span>

3.  <span data-ttu-id="48ca0-823">Na hello **vybrat určující disk kvora** vyberte **nakonfigurovat určující sdílenou složku**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-823">On hello **Select Quorum Witness** page, select **Configure a file share witness**.</span></span>

  ![36 obrázek: Určující sdílená složka vyberte hello][sap-ha-guide-figure-3025]

  <span data-ttu-id="48ca0-825">_**Obrázek 36:** vybrat určující sdílenou složku hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-825">_**Figure 36:** Select hello file share witness_</span></span>

4.  <span data-ttu-id="48ca0-826">Zadejte hello UNC cestu toohello sdílené složky (v našem příkladu \\domcontr 0\FSW).</span><span class="sxs-lookup"><span data-stu-id="48ca0-826">Enter hello UNC path toohello file share (in our example, \\domcontr-0\FSW).</span></span> <span data-ttu-id="48ca0-827">toosee seznam změn hello můžete provést, vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-827">toosee a list of hello changes you can make, select **Next**.</span></span>

  ![Obrázek 37: Definování hello umístění sdílené složky pro sdílenou složku určující hello][sap-ha-guide-figure-3026]

  <span data-ttu-id="48ca0-829">_**Obrázek 37:** definovat hello umístění sdílené složky pro sdílenou složku určující hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-829">_**Figure 37:** Define hello file share location for hello witness share_</span></span>

5.  <span data-ttu-id="48ca0-830">Vyberte hello změny a pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-830">Select hello changes you want, and then select **Next**.</span></span> <span data-ttu-id="48ca0-831">Je třeba toosuccessfully překonfigurovat hello konfiguraci clusteru, jak ukazuje obrázek 38.</span><span class="sxs-lookup"><span data-stu-id="48ca0-831">You need toosuccessfully reconfigure hello cluster configuration as shown in Figure 38.</span></span>  

  ![Obrázek 38: Potvrzení, že jste překonfigurovat hello clusteru][sap-ha-guide-figure-3027]

  <span data-ttu-id="48ca0-833">_**Obrázek 38:** potvrzení, že jste překonfigurovat hello clusteru_</span><span class="sxs-lookup"><span data-stu-id="48ca0-833">_**Figure 38:** Confirmation that you've reconfigured hello cluster_</span></span>

<span data-ttu-id="48ca0-834">Po úspěšném nainstalování hello clusteru převzetí služeb při selhání se systémem Windows, třeba změny toobe provedené toosome prahové hodnoty tooadapt převzetí služeb při selhání detekce tooconditions v Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-834">After installing hello Windows Failover Cluster successfully, changes need toobe made toosome thresholds tooadapt failover detection tooconditions in Azure.</span></span> <span data-ttu-id="48ca0-835">Hello změnit toobe parametry jsou popsané v tomto blogu: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/.</span><span class="sxs-lookup"><span data-stu-id="48ca0-835">hello parameters toobe changed are documented in this blog: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/ .</span></span> <span data-ttu-id="48ca0-836">Za předpokladu, že dva virtuální počítače, který sestavení hello konfigurace clusteru systému Windows pro ASC nebo SCS jsou v hello stejné podsíti, hello následující parametry potřebovat změnit toobe toothese hodnoty:</span><span class="sxs-lookup"><span data-stu-id="48ca0-836">Assuming that your two VMs that build hello Windows Cluster Configuration for ASCS/SCS are in hello same SubNet, hello following parameters need toobe changed toothese values:</span></span>
- <span data-ttu-id="48ca0-837">SameSubNetDelay = 2</span><span class="sxs-lookup"><span data-stu-id="48ca0-837">SameSubNetDelay = 2</span></span>
- <span data-ttu-id="48ca0-838">SameSubNetThreshold = 15</span><span class="sxs-lookup"><span data-stu-id="48ca0-838">SameSubNetThreshold = 15</span></span>

<span data-ttu-id="48ca0-839">Tato nastavení byly testovány s zákazníků a k dispozici dobrý ohrožení toobe dostatečně odolný na jedné straně hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-839">These settings were tested with customers and provided a good compromise toobe resilient enough on hello one side.</span></span> <span data-ttu-id="48ca0-840">Na hello druhé straně tato nastavení se poskytuje rychlé dostatek převzetí služeb při selhání ve skutečné chybové stavy v SAP selhání uzlu nebo virtuálního počítače nebo softwaru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-840">On hello other hand those settings were providing fast enough failover in real error conditions on SAP software or node/VM failure.</span></span> 

### <span data-ttu-id="48ca0-841"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>Instalace s DataKeeper Cluster Edition pro disk sdílené složky clusteru SAP ASC nebo SCS hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-841"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> Install SIOS DataKeeper Cluster Edition for hello SAP ASCS/SCS cluster share disk</span></span>

<span data-ttu-id="48ca0-842">Teď máte fungující konfiguraci služby Windows Server Failover Clustering v Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-842">You now have a working Windows Server Failover Clustering configuration in Azure.</span></span> <span data-ttu-id="48ca0-843">Ale tooinstall instance SAP ASC nebo SCS, budete potřebovat prostředek sdíleného disku.</span><span class="sxs-lookup"><span data-stu-id="48ca0-843">But, tooinstall an SAP ASCS/SCS instance, you need a shared disk resource.</span></span> <span data-ttu-id="48ca0-844">Nelze vytvořit hello sdílené diskové prostředky, které potřebujete v Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-844">You cannot create hello shared disk resources you need in Azure.</span></span> <span data-ttu-id="48ca0-845">S DataKeeper Cluster Edition je řešení třetí strany, můžete použít toocreate sdílené diskové prostředky.</span><span class="sxs-lookup"><span data-stu-id="48ca0-845">SIOS DataKeeper Cluster Edition is a third-party solution you can use toocreate shared disk resources.</span></span>

<span data-ttu-id="48ca0-846">Instalace s DataKeeper Cluster Edition pro hello SAP ASC nebo SCS disk sdílené složky clusteru zahrnuje tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="48ca0-846">Installing SIOS DataKeeper Cluster Edition for hello SAP ASCS/SCS cluster share disk involves these tasks:</span></span>

- <span data-ttu-id="48ca0-847">Přidání hello rozhraní .NET Framework 3.5</span><span class="sxs-lookup"><span data-stu-id="48ca0-847">Adding hello .NET Framework 3.5</span></span>
- <span data-ttu-id="48ca0-848">Instalace SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="48ca0-848">Installing SIOS DataKeeper</span></span>
- <span data-ttu-id="48ca0-849">Nastavení služby s DataKeeper</span><span class="sxs-lookup"><span data-stu-id="48ca0-849">Setting up SIOS DataKeeper</span></span>

#### <span data-ttu-id="48ca0-850"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>Přidat hello rozhraní .NET Framework 3.5</span><span class="sxs-lookup"><span data-stu-id="48ca0-850"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> Add hello .NET Framework 3.5</span></span>
<span data-ttu-id="48ca0-851">Hello rozhraní Microsoft .NET Framework 3.5 se automaticky aktivovat nebo není nainstalovaná v systému Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="48ca0-851">hello Microsoft .NET Framework 3.5 isn't automatically activated or installed on Windows Server 2012 R2.</span></span> <span data-ttu-id="48ca0-852">Protože DataKeeper s vyžaduje rozhraní .NET Framework toobe hello na všech uzlech, které nainstalujete DataKeeper na, je nutné nainstalovat na hello hostovaný operační systém všech virtuálních počítačů v clusteru hello hello rozhraní .NET Framework 3.5.</span><span class="sxs-lookup"><span data-stu-id="48ca0-852">Because SIOS DataKeeper requires hello .NET Framework toobe on all nodes that you install DataKeeper on, you must install hello .NET Framework 3.5 on hello guest operating system of all virtual machines in hello cluster.</span></span>

<span data-ttu-id="48ca0-853">Existují dva způsoby tooadd hello rozhraní .NET Framework 3.5:</span><span class="sxs-lookup"><span data-stu-id="48ca0-853">There are two ways tooadd hello .NET Framework 3.5:</span></span>

- <span data-ttu-id="48ca0-854">Použití Průvodce hello přidání rolí a funkcí v systému Windows, jak ukazuje obrázek 39.</span><span class="sxs-lookup"><span data-stu-id="48ca0-854">Use hello Add Roles and Features Wizard in Windows as shown in Figure 39.</span></span>

  ![Obrázek 39: Instalace hello rozhraní .NET Framework 3.5 pomocí Průvodce hello přidání rolí a funkcí][sap-ha-guide-figure-3028]

  <span data-ttu-id="48ca0-856">_**Obrázek 39:** hello instalace rozhraní .NET Framework 3.5 pomocí Průvodce hello přidání rolí a funkcí_</span><span class="sxs-lookup"><span data-stu-id="48ca0-856">_**Figure 39:** Install hello .NET Framework 3.5 by using hello Add Roles and Features Wizard_</span></span>

  ![Obrázek 40: Průběh instalace panelu při instalaci hello rozhraní .NET Framework 3.5 pomocí Průvodce hello přidání rolí a funkcí][sap-ha-guide-figure-3029]

  <span data-ttu-id="48ca0-858">_**Obrázek 40:** průběh instalace panelu při instalaci hello rozhraní .NET Framework 3.5 pomocí Průvodce hello přidání rolí a funkcí_</span><span class="sxs-lookup"><span data-stu-id="48ca0-858">_**Figure 40:** Installation progress bar when you install hello .NET Framework 3.5 by using hello Add Roles and Features Wizard_</span></span>

- <span data-ttu-id="48ca0-859">Použijte nástroj příkazového řádku nástroje dism.exe hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-859">Use hello command-line tool dism.exe.</span></span> <span data-ttu-id="48ca0-860">Pro tento typ instalace musíte tooaccess hello SxS adresáře na instalačním médiu systému Windows hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-860">For this type of installation, you need tooaccess hello SxS directory on hello Windows installation media.</span></span> <span data-ttu-id="48ca0-861">Na příkazovém řádku se zvýšenými oprávněními zadejte:</span><span class="sxs-lookup"><span data-stu-id="48ca0-861">At an elevated command prompt, type:</span></span>

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <span data-ttu-id="48ca0-862"><a name="dd41d5a2-8083-415b-9878-839652812102"></a>Nainstalujte SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="48ca0-862"><a name="dd41d5a2-8083-415b-9878-839652812102"></a> Install SIOS DataKeeper</span></span>

<span data-ttu-id="48ca0-863">Nainstalujte na každém uzlu v clusteru hello s DataKeeper Cluster Edition.</span><span class="sxs-lookup"><span data-stu-id="48ca0-863">Install SIOS DataKeeper Cluster Edition on each node in hello cluster.</span></span> <span data-ttu-id="48ca0-864">toocreate virtuální sdílené úložiště s s DataKeeper, vytvořte synchronizoval zrcadlení a pak simulovat sdílené úložiště clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-864">toocreate virtual shared storage with SIOS DataKeeper, create a synced mirror and then simulate cluster shared storage.</span></span>

<span data-ttu-id="48ca0-865">Před instalací softwaru SIOS hello vytvořit uživatele domény hello **DataKeeperSvc**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-865">Before you install hello SIOS software, create hello domain user **DataKeeperSvc**.</span></span>

> [!NOTE]
> <span data-ttu-id="48ca0-866">Přidat hello **DataKeeperSvc** uživatele toohello **místního správce** v obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-866">Add hello **DataKeeperSvc** user toohello **Local Administrator** group on both cluster nodes.</span></span>
>
>

<span data-ttu-id="48ca0-867">tooinstall s DataKeeper:</span><span class="sxs-lookup"><span data-stu-id="48ca0-867">tooinstall SIOS DataKeeper:</span></span>

1.  <span data-ttu-id="48ca0-868">Nainstalujte hello SIOS software do obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-868">Install hello SIOS software on both cluster nodes.</span></span>

  ![Instalační program SIOS][sap-ha-guide-figure-3030]

  ![Obrázek 41: První stránku hello s DataKeeper instalace][sap-ha-guide-figure-3031]

  <span data-ttu-id="48ca0-871">_**Obrázek 41:** první stránku hello s DataKeeper instalace_</span><span class="sxs-lookup"><span data-stu-id="48ca0-871">_**Figure 41:** First page of hello SIOS DataKeeper installation_</span></span>

2.  <span data-ttu-id="48ca0-872">V dialogu vyberte hello znázorňuje obrázek 42 vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-872">In hello dialog box shown in Figure 42, select **Yes**.</span></span>

  ![Obrázek 42: DataKeeper vás upozorní, že služba bude zakázán][sap-ha-guide-figure-3032]

  <span data-ttu-id="48ca0-874">_**Obrázek 42:** DataKeeper informuje, že služba bude zakázán_</span><span class="sxs-lookup"><span data-stu-id="48ca0-874">_**Figure 42:** DataKeeper informs you that a service will be disabled_</span></span>

3.  <span data-ttu-id="48ca0-875">V hello dialogové okno zobrazí obrázek 43, doporučujeme, abyste vybrali **účet domény nebo serveru**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-875">In hello dialog box shown in Figure 43, we recommend that you select **Domain or Server account**.</span></span>

  ![Obrázek 43: Výběr uživatele s DataKeeper][sap-ha-guide-figure-3033]

  <span data-ttu-id="48ca0-877">_**Obrázek 43:** výběr uživatele s DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="48ca0-877">_**Figure 43:** User selection for SIOS DataKeeper_</span></span>

4.  <span data-ttu-id="48ca0-878">Zadejte uživatelské jméno hello domény účtu a hesla, které jste vytvořili pro DataKeeper s.</span><span class="sxs-lookup"><span data-stu-id="48ca0-878">Enter hello domain account user name and passwords that you created for SIOS DataKeeper.</span></span>

  ![Obrázek 44: Zadejte hello domény uživatelské jméno a heslo pro hello s DataKeeper instalace][sap-ha-guide-figure-3034]

  <span data-ttu-id="48ca0-880">_**Obrázek 44:** zadejte hello domény uživatelské jméno a heslo pro hello s DataKeeper instalace_</span><span class="sxs-lookup"><span data-stu-id="48ca0-880">_**Figure 44:** Enter hello domain user name and password for hello SIOS DataKeeper installation_</span></span>

5.  <span data-ttu-id="48ca0-881">Nainstalujte klíč hello licencí pro vaše instance s DataKeeper, jak ukazuje obrázek 45.</span><span class="sxs-lookup"><span data-stu-id="48ca0-881">Install hello license key for your SIOS DataKeeper instance as shown in Figure 45.</span></span>

  ![45 obrázek: Zadejte klíč licence s DataKeeper][sap-ha-guide-figure-3035]

  <span data-ttu-id="48ca0-883">_**Obrázek 45:** zadejte klíč DataKeeper s licencí_</span><span class="sxs-lookup"><span data-stu-id="48ca0-883">_**Figure 45:** Enter your SIOS DataKeeper license key_</span></span>

6.  <span data-ttu-id="48ca0-884">Po zobrazení výzvy restartujte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="48ca0-884">When prompted, restart hello virtual machine.</span></span>

#### <span data-ttu-id="48ca0-885"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>Nastavení pro zařízení s DataKeeper</span><span class="sxs-lookup"><span data-stu-id="48ca0-885"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> Set up SIOS DataKeeper</span></span>

<span data-ttu-id="48ca0-886">Po instalaci s DataKeeper oba uzly, je třeba toostart hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="48ca0-886">After you install SIOS DataKeeper on both nodes, you need toostart hello configuration.</span></span> <span data-ttu-id="48ca0-887">cílem Hello hello konfigurace je, že toohave synchronní data replikace mezi hello další disky připojené tooeach hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="48ca0-887">hello goal of hello configuration is toohave synchronous data replication between hello additional disks attached tooeach of hello virtual machines.</span></span>

1.  <span data-ttu-id="48ca0-888">Spusťte konfigurační nástroj a hello DataKeeper správy a potom vyberte **připojit Server**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-888">Start hello DataKeeper Management and Configuration tool, and then select **Connect Server**.</span></span> <span data-ttu-id="48ca0-889">(V obrázku 46, tato možnost je v kroužku červeně.)</span><span class="sxs-lookup"><span data-stu-id="48ca0-889">(In Figure 46, this option is circled in red.)</span></span>

  ![46 obrázek: Nástroj Konfigurace a SIOS DataKeeper správy][sap-ha-guide-figure-3036]

  <span data-ttu-id="48ca0-891">_**Obrázek 46:** s DataKeeper správu a konfiguraci nástroje_</span><span class="sxs-lookup"><span data-stu-id="48ca0-891">_**Figure 46:** SIOS DataKeeper Management and Configuration tool_</span></span>

2.  <span data-ttu-id="48ca0-892">Zadejte hello název nebo adresa TCP/IP hello první uzel hello správu a konfiguraci nástroje by se měly připojit k a v druhém kroku, hello druhého uzlu.</span><span class="sxs-lookup"><span data-stu-id="48ca0-892">Enter hello name or TCP/IP address of hello first node hello Management and Configuration tool should connect to, and, in a second step, hello second node.</span></span>

  ![Obrázek 47: Vložení hello název nebo adresa TCP/IP hello první uzel hello správu a konfiguraci nástroje by měl připojit a v druhém kroku, druhý uzel hello][sap-ha-guide-figure-3037]

  <span data-ttu-id="48ca0-894">_**Obrázek 47:** vložení hello název nebo adresa TCP/IP hello první uzel hello správu a konfiguraci nástroje by měl připojit a v druhém kroku, druhý uzel hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-894">_**Figure 47:** Insert hello name or TCP/IP address of hello first node hello Management and Configuration tool should connect to, and in a second step, hello second node_</span></span>

3.  <span data-ttu-id="48ca0-895">Vytvořte úlohu hello replikace mezi dvěma uzly hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-895">Create hello replication job between hello two nodes.</span></span>

  ![Obrázek 48: Vytvoření úlohy replikace][sap-ha-guide-figure-3038]

  <span data-ttu-id="48ca0-897">_**Obrázek 48:** vytvořit úlohu replikace_</span><span class="sxs-lookup"><span data-stu-id="48ca0-897">_**Figure 48:** Create a replication job_</span></span>

  <span data-ttu-id="48ca0-898">Průvodce vás provede procesem vytvoření úlohy replikace hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-898">A wizard guides you through hello process of creating a replication job.</span></span>
4.  <span data-ttu-id="48ca0-899">Zadejte název hello, adresa TCP/IP a svazku hello zdrojový uzel.</span><span class="sxs-lookup"><span data-stu-id="48ca0-899">Define hello name, TCP/IP address, and disk volume of hello source node.</span></span>

  ![Obrázek 49: Definujte hello název úlohy replikace hello][sap-ha-guide-figure-3039]

  <span data-ttu-id="48ca0-901">_**Obrázek 49:** definovat hello název úlohy replikace hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-901">_**Figure 49:** Define hello name of hello replication job_</span></span>

  ![Obrázek 50: Definování hello základní data pro uzel hello, které by se měly hello aktuální zdrojový uzel][sap-ha-guide-figure-3040]

  <span data-ttu-id="48ca0-903">_**Obrázek 50:** zadat hello základní data pro uzel hello, které by se měly hello aktuální zdrojový uzel_</span><span class="sxs-lookup"><span data-stu-id="48ca0-903">_**Figure 50:** Define hello base data for hello node, which should be hello current source node_</span></span>

5.  <span data-ttu-id="48ca0-904">Zadejte název hello, adresa TCP/IP a svazku hello cílový uzel.</span><span class="sxs-lookup"><span data-stu-id="48ca0-904">Define hello name, TCP/IP address, and disk volume of hello target node.</span></span>

  ![Obrázek 51: Definování hello základní data pro uzel hello, které by měly být cílový uzel aktuální hello][sap-ha-guide-figure-3041]

  <span data-ttu-id="48ca0-906">_**Obrázek 51:** zadat hello základní data pro uzel hello, které by měly být cílový uzel aktuální hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-906">_**Figure 51:** Define hello base data for hello node, which should be hello current target node_</span></span>

6.  <span data-ttu-id="48ca0-907">Definujte algoritmy komprese hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-907">Define hello compression algorithms.</span></span> <span data-ttu-id="48ca0-908">V našem příkladu doporučujeme kompresi hello replikačního streamu.</span><span class="sxs-lookup"><span data-stu-id="48ca0-908">In our example, we recommend that you compress hello replication stream.</span></span> <span data-ttu-id="48ca0-909">Hlavně v situacích, opakované synchronizace hello komprese hello replikačního streamu výrazně snižuje dobu Opakovaná synchronizace.</span><span class="sxs-lookup"><span data-stu-id="48ca0-909">Especially in resynchronization situations, hello compression of hello replication stream dramatically reduces resynchronization time.</span></span> <span data-ttu-id="48ca0-910">Všimněte si, že komprese používá prostředky procesoru a paměti RAM hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="48ca0-910">Note that compression uses hello CPU and RAM resources of a virtual machine.</span></span> <span data-ttu-id="48ca0-911">Jak zvyšuje rychlost komprese hello, takže hello objem prostředků procesoru, které používá.</span><span class="sxs-lookup"><span data-stu-id="48ca0-911">As hello compression rate increases, so does hello volume of CPU resources used.</span></span> <span data-ttu-id="48ca0-912">Můžete také upravit toto nastavení později.</span><span class="sxs-lookup"><span data-stu-id="48ca0-912">You also can adjust this setting later.</span></span>

7.  <span data-ttu-id="48ca0-913">Jiné nastavení budete potřebovat toocheck se, zda text hello replikace probíhá synchronně nebo asynchronně.</span><span class="sxs-lookup"><span data-stu-id="48ca0-913">Another setting you need toocheck is whether hello replication occurs asynchronously or synchronously.</span></span> <span data-ttu-id="48ca0-914">*Pokud budete chránit SAP ASC nebo SCS konfigurace, je nutné použít synchronní replikace*.</span><span class="sxs-lookup"><span data-stu-id="48ca0-914">*When you protect SAP ASCS/SCS configurations, you must use synchronous replication*.</span></span>  

  ![Obrázek 52: Definování podrobnosti k replikaci][sap-ha-guide-figure-3042]

  <span data-ttu-id="48ca0-916">_**Obrázek 52:** definovat podrobnosti k replikaci_</span><span class="sxs-lookup"><span data-stu-id="48ca0-916">_**Figure 52:** Define replication details_</span></span>

8.  <span data-ttu-id="48ca0-917">Zadejte, zda hello svazek, který se replikují úlohou hello replikace by měla být reprezentována tooa konfigurace clusteru Windows Server Failover Clustering jako sdílený disk.</span><span class="sxs-lookup"><span data-stu-id="48ca0-917">Define whether hello volume that is replicated by hello replication job should be represented tooa Windows Server Failover Clustering cluster configuration as a shared disk.</span></span> <span data-ttu-id="48ca0-918">Pro konfiguraci SAP ASC nebo SCS hello, vyberte **Ano** tak, aby Windows hello clusteru uvidí hello replikované svazek jako sdílený disk, který můžete použít jako svazek clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-918">For hello SAP ASCS/SCS configuration, select **Yes** so that hello Windows cluster sees hello replicated volume as a shared disk that it can use as a cluster volume.</span></span>

  ![Obrázek 53: Vyberte Ano tooset hello replikovat svazek jako svazek clusteru][sap-ha-guide-figure-3043]

  <span data-ttu-id="48ca0-920">_**Obrázek 53:** vyberte **Ano** tooset hello replikovat svazek jako svazek clusteru_</span><span class="sxs-lookup"><span data-stu-id="48ca0-920">_**Figure 53:** Select **Yes** tooset hello replicated volume as a cluster volume_</span></span>

  <span data-ttu-id="48ca0-921">Po vytvoření svazku hello hello DataKeeper správu a konfiguraci nástroje ukazuje, že tuto úlohu hello replikace aktivní.</span><span class="sxs-lookup"><span data-stu-id="48ca0-921">After hello volume is created, hello DataKeeper Management and Configuration tool shows that hello replication job is active.</span></span>

  ![Obrázek 54: DataKeeper synchronní zrcadlení pro hello SAP ASC nebo SCS sdílené složky disku je aktivní][sap-ha-guide-figure-3044]

  <span data-ttu-id="48ca0-923">_**Obrázek 54:** DataKeeper synchronní zrcadlení pro hello SAP ASC nebo SCS sdílet disk je aktivní_</span><span class="sxs-lookup"><span data-stu-id="48ca0-923">_**Figure 54:** DataKeeper synchronous mirroring for hello SAP ASCS/SCS share disk is active_</span></span>

  <span data-ttu-id="48ca0-924">Správce clusteru převzetí služeb při selhání teď hello disk jako disk s DataKeeper ukazuje, jak ukazuje obrázek 55.</span><span class="sxs-lookup"><span data-stu-id="48ca0-924">Failover Cluster Manager now shows hello disk as a DataKeeper disk, as shown in Figure 55.</span></span>

  ![Obrázek 55: Správce clusteru převzetí služeb při selhání zobrazuje hello disk, který DataKeeper replikovat][sap-ha-guide-figure-3045]

  <span data-ttu-id="48ca0-926">_**Obrázek 55:** Správce clusteru převzetí služeb při selhání zobrazuje hello disku tohoto DataKeeper replikovat_</span><span class="sxs-lookup"><span data-stu-id="48ca0-926">_**Figure 55:** Failover Cluster Manager shows hello disk that DataKeeper replicated_</span></span>

## <span data-ttu-id="48ca0-927"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Instalace systému SAP NetWeaver hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-927"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a> Install hello SAP NetWeaver system</span></span>

<span data-ttu-id="48ca0-928">Instalace databázového systému hello jsme nebude popisují nastavení lišit v závislosti na hello databázového systému systému, které používáte.</span><span class="sxs-lookup"><span data-stu-id="48ca0-928">We won’t describe hello DBMS setup because setups vary depending on hello DBMS system you use.</span></span> <span data-ttu-id="48ca0-929">Ale předpokládáme, že jsou aspekty vysoké dostupnosti s hello databázového systému řešit pomocí hello funkce, které hello různých výrobců databázového systému podpora pro Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-929">However, we assume that high-availability concerns with hello DBMS are addressed with hello functionalities hello different DBMS vendors support for Azure.</span></span> <span data-ttu-id="48ca0-930">Například Always On nebo zrcadlení databáze systému SQL Server a Oracle Data Guard pro databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="48ca0-930">For example, Always On or database mirroring for SQL Server, and Oracle Data Guard for Oracle databases.</span></span> <span data-ttu-id="48ca0-931">V případě hello, které používáme v tomto článku jsme nepřidali další ochrany toohello databázového systému.</span><span class="sxs-lookup"><span data-stu-id="48ca0-931">In hello scenario we use in this article, we didn't add more protection toohello DBMS.</span></span>

<span data-ttu-id="48ca0-932">Když různé služby databázového systému interakci s tímto typem Clusterované konfigurace SAP ASC nebo SCS v Azure neexistují žádné zvláštní požadavky.</span><span class="sxs-lookup"><span data-stu-id="48ca0-932">There are no special considerations when different DBMS services interact with this kind of clustered SAP ASCS/SCS configuration in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="48ca0-933">postupy instalace Hello SAP NetWeaver ABAP systémů, Java systémy a systémy ABAP + Java jsou téměř shodné.</span><span class="sxs-lookup"><span data-stu-id="48ca0-933">hello installation procedures of SAP NetWeaver ABAP systems, Java systems, and ABAP+Java systems are almost identical.</span></span> <span data-ttu-id="48ca0-934">Hello nejdůležitější rozdíl je, že systému SAP ABAP má jednu instanci ASC.</span><span class="sxs-lookup"><span data-stu-id="48ca0-934">hello most significant difference is that an SAP ABAP system has one ASCS instance.</span></span> <span data-ttu-id="48ca0-935">Hello systému SAP Java má jednu instanci SCS.</span><span class="sxs-lookup"><span data-stu-id="48ca0-935">hello SAP Java system has one SCS instance.</span></span> <span data-ttu-id="48ca0-936">Hello systému SAP ABAP + Java má jedna instance ASC a jedna instance SCS spuštěné v hello stejné skupiny clusteru převzetí služeb při selhání Microsoft.</span><span class="sxs-lookup"><span data-stu-id="48ca0-936">hello SAP ABAP+Java system has one ASCS instance and one SCS instance running in hello same Microsoft failover cluster group.</span></span> <span data-ttu-id="48ca0-937">Případné rozdíly instalace pro každou SAP NetWeaver instalace zásobníku se výslovně uveden.</span><span class="sxs-lookup"><span data-stu-id="48ca0-937">Any installation differences for each SAP NetWeaver installation stack are explicitly mentioned.</span></span> <span data-ttu-id="48ca0-938">Můžete předpokládat, že jsou všechny ostatní části hello stejné.</span><span class="sxs-lookup"><span data-stu-id="48ca0-938">You can assume that all other parts are hello same.</span></span>  
>
>

### <span data-ttu-id="48ca0-939"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Instalace s vysokou dostupností ASC nebo SCS instance SAP</span><span class="sxs-lookup"><span data-stu-id="48ca0-939"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a> Install SAP with a high-availability ASCS/SCS instance</span></span>

> [!IMPORTANT]
> <span data-ttu-id="48ca0-940">Ujistěte se, není tooplace stránku souborů na DataKeeper zrcadleny svazky.</span><span class="sxs-lookup"><span data-stu-id="48ca0-940">Be sure not tooplace your page file on DataKeeper mirrored volumes.</span></span> <span data-ttu-id="48ca0-941">DataKeeper nepodporuje zrcadlové svazky.</span><span class="sxs-lookup"><span data-stu-id="48ca0-941">DataKeeper does not support mirrored volumes.</span></span> <span data-ttu-id="48ca0-942">Můžete ponechat stránkovacím souboru na dočasné jednotce hello D Azure virtuálního počítače, což je výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-942">You can leave your page file on hello temporary drive D of an Azure virtual machine, which is hello default.</span></span> <span data-ttu-id="48ca0-943">Pokud není již existuje, přesuňte hello Windows stránky souboru toodrive D: virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-943">If it's not already there, move hello Windows page file toodrive D: of your Azure virtual machine.</span></span>
>
>

<span data-ttu-id="48ca0-944">Instalace s vysokou dostupností ASC nebo SCS instance SAP zahrnuje tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="48ca0-944">Installing SAP with a high-availability ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="48ca0-945">Vytváření název virtuálního hostitele pro instance SAP ASC nebo SCS hello v clusteru</span><span class="sxs-lookup"><span data-stu-id="48ca0-945">Creating a virtual host name for hello clustered SAP ASCS/SCS instance</span></span>
- <span data-ttu-id="48ca0-946">Instalace hello SAP prvním uzlu clusteru</span><span class="sxs-lookup"><span data-stu-id="48ca0-946">Installing hello SAP first cluster node</span></span>
- <span data-ttu-id="48ca0-947">Úprava profilu SAP hello hello ASC nebo SCS instance</span><span class="sxs-lookup"><span data-stu-id="48ca0-947">Modifying hello SAP profile of hello ASCS/SCS instance</span></span>
- <span data-ttu-id="48ca0-948">Přidání port testu</span><span class="sxs-lookup"><span data-stu-id="48ca0-948">Adding a probe port</span></span>
- <span data-ttu-id="48ca0-949">Otevřete port testu brány firewall systému Windows hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-949">Opening hello Windows firewall probe port</span></span>

#### <span data-ttu-id="48ca0-950"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Vytvořte název virtuálního hostitele pro instance SAP ASC nebo SCS hello v clusteru</span><span class="sxs-lookup"><span data-stu-id="48ca0-950"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a> Create a virtual host name for hello clustered SAP ASCS/SCS instance</span></span>

1.  <span data-ttu-id="48ca0-951">Ve správci Windows DNS hello vytvořte záznam DNS pro název virtuálního hostitele hello hello ASC nebo SCS instance.</span><span class="sxs-lookup"><span data-stu-id="48ca0-951">In hello Windows DNS manager, create a DNS entry for hello virtual host name of hello ASCS/SCS instance.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="48ca0-952">Hello IP adresu, která přiřadíte název virtuálního hostitele toohello hello ASC nebo SCS instance musí být hello stejné jako hello IP adresa, který jste přiřadili tooAzure nástroj pro vyrovnávání zatížení (**<*SID*> - lb - ASC **).</span><span class="sxs-lookup"><span data-stu-id="48ca0-952">hello IP address that you assign toohello virtual host name of hello ASCS/SCS instance must be hello same as hello IP address that you assigned tooAzure Load Balancer (**<*SID*>-lb-ascs**).</span></span>  
  >
  >

  <span data-ttu-id="48ca0-953">Hello IP adresu hello virtuální SAP ASC nebo SCS název hostitele (**pr1. ASC sap**) hello stejné jako hello IP adresa služby Vyrovnávání zatížení Azure (**pr1-lb Asc**).</span><span class="sxs-lookup"><span data-stu-id="48ca0-953">hello IP address of hello virtual SAP ASCS/SCS host name (**pr1-ascs-sap**) is hello same as hello IP address of Azure Load Balancer (**pr1-lb-ascs**).</span></span>

  ![Obrázek 56: Definování hello položky DNS pro virtuální název clusteru hello SAP ASC nebo SCS a adresa TCP/IP][sap-ha-guide-figure-3046]

  <span data-ttu-id="48ca0-955">_**Obrázek 56:** definovat hello položku DNS pro virtuální název clusteru hello SAP ASC nebo SCS adresa TCP/IP_</span><span class="sxs-lookup"><span data-stu-id="48ca0-955">_**Figure 56:** Define hello DNS entry for hello SAP ASCS/SCS cluster virtual name and TCP/IP address_</span></span>

2.  <span data-ttu-id="48ca0-956">toodefine hello IP adresu přiřazenou toohello název virtuálního hostitele, vyberte **Správce DNS** > **domény**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-956">toodefine hello IP address assigned toohello virtual host name, select **DNS Manager** > **Domain**.</span></span>

  ![Obrázek 57: Nový virtuální název a adresu TCP/IP pro konfiguraci clusteru SAP ASC nebo SCS][sap-ha-guide-figure-3047]

  <span data-ttu-id="48ca0-958">_**Obrázek 57:** nový virtuální název a TCP/IP adres pro konfiguraci clusteru SAP ASC nebo SCS_</span><span class="sxs-lookup"><span data-stu-id="48ca0-958">_**Figure 57:** New virtual name and TCP/IP address for SAP ASCS/SCS cluster configuration_</span></span>

#### <span data-ttu-id="48ca0-959"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Nainstalujte hello SAP prvním uzlu clusteru</span><span class="sxs-lookup"><span data-stu-id="48ca0-959"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> Install hello SAP first cluster node</span></span>

1.  <span data-ttu-id="48ca0-960">Spuštění hello první clusteru uzlu možnost na uzlu clusteru A. Například na hello **pr1-ASC-0** hostitele.</span><span class="sxs-lookup"><span data-stu-id="48ca0-960">Execute hello first cluster node option on cluster node A. For example, on hello **pr1-ascs-0** host.</span></span>
2.  <span data-ttu-id="48ca0-961">tookeep hello výchozí porty pro hello Azure interní nástroj pro vyrovnávání zatížení, vyberte:</span><span class="sxs-lookup"><span data-stu-id="48ca0-961">tookeep hello default ports for hello Azure internal load balancer, select:</span></span>

  * <span data-ttu-id="48ca0-962">**Systém ABAP**: **Asc** instance číslo **00**</span><span class="sxs-lookup"><span data-stu-id="48ca0-962">**ABAP system**: **ASCS** instance number **00**</span></span>
  * <span data-ttu-id="48ca0-963">**Java systému**: **SCS** instance číslo **01**</span><span class="sxs-lookup"><span data-stu-id="48ca0-963">**Java system**: **SCS** instance number **01**</span></span>
  * <span data-ttu-id="48ca0-964">**ABAP + Java systému**: **Asc** instance číslo **00** a **SCS** instance číslo **01**</span><span class="sxs-lookup"><span data-stu-id="48ca0-964">**ABAP+Java system**: **ASCS** instance number **00** and **SCS** instance number **01**</span></span>

  <span data-ttu-id="48ca0-965">toouse instance čísla, jiné než 00 pro hello ABAP ASC instance a 01 hello Java SCS instanci, je třeba nejprve toochange hello Azure interní výchozí Vyrovnávání zatížení pravidel, popsaných v [změnu hello ASC nebo SCS výchozí zatížení pravidla pro vyrovnávání zatížení Azure interní hello vyrovnávání][sap-ha-guide-8.9].</span><span class="sxs-lookup"><span data-stu-id="48ca0-965">toouse instance numbers other than 00 for hello ABAP ASCS instance and 01 for hello Java SCS instance, first you need toochange hello Azure internal load balancer default load balancing rules, described in [Change hello ASCS/SCS default load balancing rules for hello Azure internal load balancer][sap-ha-guide-8.9].</span></span>

<span data-ttu-id="48ca0-966">Hello další několik úloh nejsou popsané v dokumentaci hello standardní SAP instalace.</span><span class="sxs-lookup"><span data-stu-id="48ca0-966">hello next few tasks aren't described in hello standard SAP installation documentation.</span></span>

> [!NOTE]
> <span data-ttu-id="48ca0-967">Hello SAP instalace dokumentace popisuje, jak tooinstall hello prvního uzlu clusteru ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="48ca0-967">hello SAP installation documentation describes how tooinstall hello first ASCS/SCS cluster node.</span></span>
>
>

#### <span data-ttu-id="48ca0-968"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Upravit profil SAP hello hello ASC nebo SCS instance</span><span class="sxs-lookup"><span data-stu-id="48ca0-968"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> Modify hello SAP profile of hello ASCS/SCS instance</span></span>

<span data-ttu-id="48ca0-969">Je nutné tooadd nový parametr profilu.</span><span class="sxs-lookup"><span data-stu-id="48ca0-969">You need tooadd a new profile parameter.</span></span> <span data-ttu-id="48ca0-970">Parametr profil Hello zabrání připojení mezi SAP pracovní procesy a server hello zařazování zavření po nečinnosti příliš dlouho.</span><span class="sxs-lookup"><span data-stu-id="48ca0-970">hello profile parameter prevents connections between SAP work processes and hello enqueue server from closing when they are idle for too long.</span></span> <span data-ttu-id="48ca0-971">Jsme uvedených hello scénář v [přidat položky registru na obou uzlů clusteru instance SAP ASC nebo SCS hello][sap-ha-guide-8.11].</span><span class="sxs-lookup"><span data-stu-id="48ca0-971">We mentioned hello problem scenario in [Add registry entries on both cluster nodes of hello SAP ASCS/SCS instance][sap-ha-guide-8.11].</span></span> <span data-ttu-id="48ca0-972">V této části zavedli jsme taky dvě změny toosome základní parametry protokolu TCP/IP připojení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-972">In that section, we also introduced two changes toosome basic TCP/IP connection parameters.</span></span> <span data-ttu-id="48ca0-973">V druhém kroku, je nutné tooset hello zařadit server toosend `keep_alive` signál, aby hello připojení není dosáhl limit nečinnosti Vyrovnávání zatížení Azure interní hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-973">In a second step, you need tooset hello enqueue server toosend a `keep_alive` signal so that hello connections don't hit hello Azure internal load balancer's idle threshold.</span></span>

<span data-ttu-id="48ca0-974">toomodify hello SAP profil instance ASC nebo SCS hello:</span><span class="sxs-lookup"><span data-stu-id="48ca0-974">toomodify hello SAP profile of hello ASCS/SCS instance:</span></span>

1.  <span data-ttu-id="48ca0-975">Přidejte tento profil parametr toohello SAP ASC nebo SCS instance profil:</span><span class="sxs-lookup"><span data-stu-id="48ca0-975">Add this profile parameter toohello SAP ASCS/SCS instance profile:</span></span>

  ```
  enque/encni/set_so_keepalive = true
  ```
  <span data-ttu-id="48ca0-976">V našem příkladu hello cesta je:</span><span class="sxs-lookup"><span data-stu-id="48ca0-976">In our example, hello path is:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  <span data-ttu-id="48ca0-977">Například profil instance SAP SCS toohello a odpovídající cesta:</span><span class="sxs-lookup"><span data-stu-id="48ca0-977">For example, toohello SAP SCS instance profile and corresponding path:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  <span data-ttu-id="48ca0-978">změny hello tooapply, restartujte instanci SAP ASC /SCS hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-978">tooapply hello changes, restart hello SAP ASCS /SCS instance.</span></span>

#### <span data-ttu-id="48ca0-979"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Přidat port testu</span><span class="sxs-lookup"><span data-stu-id="48ca0-979"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Add a probe port</span></span>

<span data-ttu-id="48ca0-980">Vyrovnávání zatížení pro vnitřní hello testu funkce toomake hello celý cluster konfigurace pracovní pomocí vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="48ca0-980">Use hello internal load balancer's probe functionality toomake hello entire cluster configuration work with Azure Load Balancer.</span></span> <span data-ttu-id="48ca0-981">Nástroje pro vyrovnávání zatížení Azure interní Hello obvykle distribuuje hello příchozí zatížení rovnoměrně mezi zúčastněných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="48ca0-981">hello Azure internal load balancer usually distributes hello incoming workload equally between participating virtual machines.</span></span> <span data-ttu-id="48ca0-982">Ale tato funkce nebude pracovat v některé konfigurace clusteru vzhledem k tomu, že pouze jedna instance je aktivní.</span><span class="sxs-lookup"><span data-stu-id="48ca0-982">However, this won't work in some cluster configurations because only one instance is active.</span></span> <span data-ttu-id="48ca0-983">Hello další instance je pasivní a nemůže přijímat hello zatížení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-983">hello other instance is passive and can’t accept any of hello workload.</span></span> <span data-ttu-id="48ca0-984">Funkce kontroly pomáhá při přiřadí nástroje pro vyrovnávání zatížení Azure interní hello fungovat pouze tooan aktivní instance.</span><span class="sxs-lookup"><span data-stu-id="48ca0-984">A probe functionality helps when hello Azure internal load balancer assigns work only tooan active instance.</span></span> <span data-ttu-id="48ca0-985">Pomocí funkce testu hello hello interní vyrovnávání zátěže může zjistit, které instance jsou aktivní a pak cíle pouze hello instance hello zatížení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-985">With hello probe functionality, hello internal load balancer can detect which instances are active, and then target only hello instance with hello workload.</span></span>

<span data-ttu-id="48ca0-986">tooadd port testu:</span><span class="sxs-lookup"><span data-stu-id="48ca0-986">tooadd a probe port:</span></span>

1.  <span data-ttu-id="48ca0-987">Zkontrolujte aktuální hello **ProbePort** nastavení tak, že spustíte následující příkaz prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-987">Check hello current **ProbePort** setting by running hello following PowerShell command.</span></span> <span data-ttu-id="48ca0-988">Spouštění ho do jedné hello virtuálních počítačů v konfiguraci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-988">Execute it from within one of hello virtual machines in hello cluster configuration.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  <span data-ttu-id="48ca0-989">Definujte port testu.</span><span class="sxs-lookup"><span data-stu-id="48ca0-989">Define a probe port.</span></span> <span data-ttu-id="48ca0-990">Hello výchozí číslo portu testu je **0**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-990">hello default probe port number is **0**.</span></span> <span data-ttu-id="48ca0-991">V našem příkladu používáme port testu **62000**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-991">In our example, we use probe port **62000**.</span></span>

  ![Obrázek 58: port testu konfigurace clusteru hello je 0. ve výchozím nastavení][sap-ha-guide-figure-3048]

  <span data-ttu-id="48ca0-993">_**Obrázek 58:** hello výchozí clusteru Konfigurace testu port je 0._</span><span class="sxs-lookup"><span data-stu-id="48ca0-993">_**Figure 58:** hello default cluster configuration probe port is 0_</span></span>

  <span data-ttu-id="48ca0-994">číslo portu Hello je definována v šablonách SAP Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="48ca0-994">hello port number is defined in SAP Azure Resource Manager templates.</span></span> <span data-ttu-id="48ca0-995">Můžete přiřadit číslo portu hello v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="48ca0-995">You can assign hello port number in PowerShell.</span></span>

  <span data-ttu-id="48ca0-996">tooset novou hodnotu ProbePort hello  **SAP <*SID*> IP ** prostředek clusteru, spusťte následující skript prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-996">tooset a new ProbePort value for hello **SAP <*SID*> IP** cluster resource, run hello following PowerShell script.</span></span> <span data-ttu-id="48ca0-997">Aktualizujte hello proměnné prostředí PowerShell pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="48ca0-997">Update hello PowerShell variables for your environment.</span></span> <span data-ttu-id="48ca0-998">Po spuštění skriptu hello budete výzvami toorestart hello SAP clusteru skupiny tooactivate hello změny.</span><span class="sxs-lookup"><span data-stu-id="48ca0-998">After hello script runs, you'll be prompted toorestart hello SAP cluster group tooactivate hello changes.</span></span>

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

  <span data-ttu-id="48ca0-999">Po přepnutí hello  **SAP <*SID*> ** clusteru role online, ověřte, že **ProbePort** nastavena toohello novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="48ca0-999">After you bring hello **SAP <*SID*>** cluster role online, verify that **ProbePort** is set toohello new value.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![Obrázek 59: Probe hello clusteru port po nastavení novou hodnotu hello][sap-ha-guide-figure-3049]

  <span data-ttu-id="48ca0-1001">_**Obrázek 59:** testu hello clusteru port po nastavení novou hodnotu hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-1001">_**Figure 59:** Probe hello cluster port after you set hello new value_</span></span>

#### <span data-ttu-id="48ca0-1002"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Otevřete port testu brány firewall systému Windows hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-1002"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a> Open hello Windows firewall probe port</span></span>

<span data-ttu-id="48ca0-1003">Je nutné tooopen testu port brány firewall systému Windows na obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1003">You need tooopen a Windows firewall probe port on both cluster nodes.</span></span> <span data-ttu-id="48ca0-1004">Použijte následující tooopen skript port testu brány firewall systému Windows hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1004">Use hello following script tooopen a Windows firewall probe port.</span></span> <span data-ttu-id="48ca0-1005">Aktualizujte hello proměnné prostředí PowerShell pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1005">Update hello PowerShell variables for your environment.</span></span>

  ```PowerShell
  $ProbePort = 62000   # ProbePort of hello Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

<span data-ttu-id="48ca0-1006">Hello **ProbePort** je nastaven příliš**62000**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1006">hello **ProbePort** is set too**62000**.</span></span> <span data-ttu-id="48ca0-1007">Teď můžete přístup hello sdílené složky  **\\\ascsha-clsap\sapmnt** z jiných hostitelů, například jako z **ascsha dbas**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1007">Now you can access hello file share **\\\ascsha-clsap\sapmnt** from other hosts, such as from **ascsha-dbas**.</span></span>

### <span data-ttu-id="48ca0-1008"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Instalovat instanci databáze hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-1008"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Install hello database instance</span></span>

<span data-ttu-id="48ca0-1009">databáze hello tooinstall instance, postupujte podle procesu hello popsané v dokumentaci k instalaci SAP hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1009">tooinstall hello database instance, follow hello process described in hello SAP installation documentation.</span></span>

### <span data-ttu-id="48ca0-1010"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Nainstalujte hello druhého uzlu clusteru</span><span class="sxs-lookup"><span data-stu-id="48ca0-1010"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> Install hello second cluster node</span></span>

<span data-ttu-id="48ca0-1011">tooinstall hello druhý cluster, postupujte podle kroků hello v hello Instalační příručka.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1011">tooinstall hello second cluster, follow hello steps in hello SAP installation guide.</span></span>

### <span data-ttu-id="48ca0-1012"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Změňte typ spouštění hello instance služby SAP YBRAT Windows hello</span><span class="sxs-lookup"><span data-stu-id="48ca0-1012"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> Change hello start type of hello SAP ERS Windows service instance</span></span>

<span data-ttu-id="48ca0-1013">Změňte typ spouštění hello hello služba systému Windows YBRAT SAP příliš**automaticky (zpožděné spuštění)** na obou uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1013">Change hello start type of hello SAP ERS Windows service too**Automatic (Delayed Start)** on both cluster nodes.</span></span>

![Obrázek 60: Změnit typ hello service pro automatické toodelayed instance SAP YBRAT hello][sap-ha-guide-figure-3050]

<span data-ttu-id="48ca0-1015">_**Obrázek 60:** změnit typ hello service pro automatické toodelayed instance SAP YBRAT hello_</span><span class="sxs-lookup"><span data-stu-id="48ca0-1015">_**Figure 60:** Change hello service type for hello SAP ERS instance toodelayed automatic_</span></span>

### <span data-ttu-id="48ca0-1016"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>Nainstalujte hello primární Server aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="48ca0-1016"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> Install hello SAP Primary Application Server</span></span>

<span data-ttu-id="48ca0-1017">Instalovat instanci serveru primární aplikace (Pa) hello <*SID*> - di - 0 hello virtuálního počítače, který jste určili toohost hello Pa adresy.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1017">Install hello Primary Application Server (PAS) instance <*SID*>-di-0 on hello virtual machine that you've designated toohost hello PAS.</span></span> <span data-ttu-id="48ca0-1018">Neexistují žádné závislosti na Azure nebo DataKeeper specifická nastavení.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1018">There are no dependencies on Azure or DataKeeper-specific settings.</span></span>

### <span data-ttu-id="48ca0-1019"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>Nainstalujte hello SAP další aplikační Server</span><span class="sxs-lookup"><span data-stu-id="48ca0-1019"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> Install hello SAP Additional Application Server</span></span>

<span data-ttu-id="48ca0-1020">Nainstalujte SAP serveru pro další aplikace (AAS) na všechny hello virtuální počítače, které jste označili toohost instance aplikačního serveru SAP.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1020">Install an SAP Additional Application Server (AAS) on all hello virtual machines that you've designated toohost an SAP Application Server instance.</span></span> <span data-ttu-id="48ca0-1021">Například na <*SID*> - di - 1 příliš <*SID*> - di -&lt;n&gt;.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1021">For example, on <*SID*>-di-1 too<*SID*>-di-&lt;n&gt;.</span></span>

> [!NOTE]
> <span data-ttu-id="48ca0-1022">To dokončí instalaci hello systému SAP NetWeaver vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1022">This finishes hello installation of a high-availability SAP NetWeaver system.</span></span> <span data-ttu-id="48ca0-1023">Pokračujte dále testování převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1023">Next, proceed with failover testing.</span></span>
>


## <span data-ttu-id="48ca0-1024"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Testovací převzetí služeb při selhání hello SAP ASC nebo SCS instance a SIOS replikace</span><span class="sxs-lookup"><span data-stu-id="48ca0-1024"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> Test hello SAP ASCS/SCS instance failover and SIOS replication</span></span>
<span data-ttu-id="48ca0-1025">Je snadno tootest a monitorování SAP ASC nebo SCS instance převzetí služeb při selhání a SIOS replikaci disku pomocí Správce clusteru převzetí služeb při selhání a s DataKeeper správu a konfiguraci nástroje hello.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1025">It's easy tootest and monitor an SAP ASCS/SCS instance failover and SIOS disk replication by using Failover Cluster Manager and hello SIOS DataKeeper Management and Configuration tool.</span></span>

### <span data-ttu-id="48ca0-1026"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>V uzlu clusteru A je spuštěna instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="48ca0-1026"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> SAP ASCS/SCS instance is running on cluster node A</span></span>

<span data-ttu-id="48ca0-1027">Hello **SAP PR1** skupina clusteru běží na uzlu clusteru A. Například na **pr1-ASC-0**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1027">hello **SAP PR1** cluster group is running on cluster node A. For example, on **pr1-ascs-0**.</span></span> <span data-ttu-id="48ca0-1028">Přiřadit hello sdíleného disku S, který je součástí hello **SAP PR1** skupiny, a kterou instanci ASC nebo SCS hello používá, A. toocluster uzlu clusteru</span><span class="sxs-lookup"><span data-stu-id="48ca0-1028">Assign hello shared disk drive S, which is part of hello **SAP PR1** cluster group, and which hello ASCS/SCS instance uses, toocluster node A.</span></span>

![Obrázek 61: Správce clusteru převzetí služeb při selhání: Skupina clusteru SAP < SID > hello běží na uzlu clusteru A][sap-ha-guide-figure-5000]

<span data-ttu-id="48ca0-1030">_**Obrázek 61:** Správce clusteru převzetí služeb při selhání: hello SAP <*SID*> Skupina clusteru běží na uzlu clusteru A_</span><span class="sxs-lookup"><span data-stu-id="48ca0-1030">_**Figure 61:** Failover Cluster Manager: hello SAP <*SID*> cluster group is running on cluster node A_</span></span>

<span data-ttu-id="48ca0-1031">V hello s DataKeeper správu a konfigurační nástroj se zobrazí tento hello sdílený disk, který synchronně replikace dat z hello zdrojového svazku jednotky S na uzlu clusteru toohello cílového svazku jednotky S na uzlu clusteru B. Například se replikují z **pr1-ASC-0 [10.0.0.40]** příliš**pr1-ASC-1 [10.0.0.41]**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1031">In hello SIOS DataKeeper Management and Configuration tool, you can see that hello shared disk data is synchronously replicated from hello source volume drive S on cluster node A toohello target volume drive S on cluster node B. For example, it's replicated from **pr1-ascs-0 [10.0.0.40]** too**pr1-ascs-1 [10.0.0.41]**.</span></span>

![Obrázek 62: V DataKeeper s replikovat místní svazek hello z uzlu clusteru toocluster uzlu B][sap-ha-guide-figure-5001]

<span data-ttu-id="48ca0-1033">_**Obrázek 62:** v DataKeeper s replikovat místní svazek hello z uzlu clusteru toocluster uzlu B_</span><span class="sxs-lookup"><span data-stu-id="48ca0-1033">_**Figure 62:** In SIOS DataKeeper, replicate hello local volume from cluster node A toocluster node B_</span></span>

### <span data-ttu-id="48ca0-1034"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Převzetí služeb při selhání uzlu A toonode B</span><span class="sxs-lookup"><span data-stu-id="48ca0-1034"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Failover from node A toonode B</span></span>

1.  <span data-ttu-id="48ca0-1035">Vyberte jednu z těchto možností tooinitiate převzetí služeb při selhání hello SAP <*SID*> Skupina clusteru z uzlu clusteru uzel A toocluster B:</span><span class="sxs-lookup"><span data-stu-id="48ca0-1035">Choose one of these options tooinitiate a failover of hello SAP <*SID*> cluster group from cluster node A toocluster node B:</span></span>
  - <span data-ttu-id="48ca0-1036">Pomocí Správce clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="48ca0-1036">Use Failover Cluster Manager</span></span>  
  - <span data-ttu-id="48ca0-1037">Pomocí prostředí PowerShell pro Cluster převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="48ca0-1037">Use Failover Cluster PowerShell</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  <span data-ttu-id="48ca0-1038">Restartovat A uzlu clusteru v rámci operačního systému hosta Windows hello (tím se vyvolá automatické převzetí služeb při selhání systému hello SAP <*SID*> Skupina clusteru z uzlu A toonode B).</span><span class="sxs-lookup"><span data-stu-id="48ca0-1038">Restart cluster node A within hello Windows guest operating system (this initiates an automatic failover of hello SAP <*SID*> cluster group from node A toonode B).</span></span>  
3.  <span data-ttu-id="48ca0-1039">Restartovat A uzel clusteru z hello portálu Azure (tím se vyvolá automatické převzetí služeb při selhání systému hello SAP <*SID*> Skupina clusteru z uzlu A toonode B).</span><span class="sxs-lookup"><span data-stu-id="48ca0-1039">Restart cluster node A from hello Azure portal (this initiates an automatic failover of hello SAP <*SID*> cluster group from node A toonode B).</span></span>  
4.  <span data-ttu-id="48ca0-1040">Restartujte počítač A uzlu clusteru pomocí prostředí Azure PowerShell (tím se vyvolá automatické převzetí služeb při selhání systému hello SAP <*SID*> Skupina clusteru z uzlu A toonode B).</span><span class="sxs-lookup"><span data-stu-id="48ca0-1040">Restart cluster node A by using Azure PowerShell (this initiates an automatic failover of hello SAP <*SID*> cluster group from node A toonode B).</span></span>

  <span data-ttu-id="48ca0-1041">Po převzetí služeb při selhání, hello SAP <*SID*> Skupina clusteru běží na uzlu clusteru B. Například je spuštěn na **pr1-ASC-1**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1041">After failover, hello SAP <*SID*> cluster group is running on cluster node B. For example, it's running on **pr1-ascs-1**.</span></span>

  ![Obrázek 63: Ve Správci clusteru převzetí služeb při selhání skupiny clusteru SAP < SID > hello běží na uzlu clusteru B][sap-ha-guide-figure-5002]

  <span data-ttu-id="48ca0-1043">_**Obrázek 63**: V převzetí služeb při selhání modulu Správce clusteru hello SAP <*SID*> Skupina clusteru běží na uzlu clusteru B_</span><span class="sxs-lookup"><span data-stu-id="48ca0-1043">_**Figure 63**: In Failover Cluster Manager, hello SAP <*SID*> cluster group is running on cluster node B_</span></span>

  <span data-ttu-id="48ca0-1044">Hello sdílený disk je teď připojený v clusteru je uzel B. s DataKeeper replikaci dat ze zdrojového svazku jednotky S v clusteru uzlu B tootarget svazku jednotky S na uzlu clusteru A. Například je replikace z **pr1-ASC-1 [10.0.0.41]** příliš**pr1-ASC-0 [10.0.0.40]**.</span><span class="sxs-lookup"><span data-stu-id="48ca0-1044">hello shared disk is now mounted on cluster node B. SIOS DataKeeper is replicating data from source volume drive S on cluster node B tootarget volume drive S on cluster node A. For example, it's replicating from **pr1-ascs-1 [10.0.0.41]** too**pr1-ascs-0 [10.0.0.40]**.</span></span>

  ![Obrázek 64: SIOS DataKeeper replikuje hello místního svazku z clusteru uzlu B toocluster uzlu A][sap-ha-guide-figure-5003]

  <span data-ttu-id="48ca0-1046">_**Obrázek 64:** s DataKeeper replikuje hello místního svazku z uzlu clusteru, toocluster uzlu B A_</span><span class="sxs-lookup"><span data-stu-id="48ca0-1046">_**Figure 64:** SIOS DataKeeper replicates hello local volume from cluster node B toocluster node A_</span></span>
