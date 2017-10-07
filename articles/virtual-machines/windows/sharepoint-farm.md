---
title: "serverové farmy služby SharePoint aaaCreate v Azure | Microsoft Docs"
description: "Rychle vytvořte novou farmu služby SharePoint 2013 nebo SharePoint 2016 v Azure pomocí portálu Azure marketplace hello."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 89b124da-019d-4179-86dd-ad418d05a4f2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d71c7177d9b99cf377bab767342508007285b452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-sharepoint-server-farms-using-hello-azure-portal-marketplace"></a><span data-ttu-id="81ac2-103">Vytvoření serverové farmy služby SharePoint pomocí hello portálu Azure marketplace</span><span class="sxs-lookup"><span data-stu-id="81ac2-103">Create SharePoint server farms using hello Azure portal marketplace</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="sharepoint-2013-farms"></a><span data-ttu-id="81ac2-104">Farmy služby SharePoint 2013</span><span class="sxs-lookup"><span data-stu-id="81ac2-104">SharePoint 2013 farms</span></span>
<span data-ttu-id="81ac2-105">Hello Microsoft Azure Marketplace portálu můžete rychle vytvořit předem nakonfigurované farmy služby SharePoint Server 2013.</span><span class="sxs-lookup"><span data-stu-id="81ac2-105">With hello Microsoft Azure portal marketplace, you can quickly create pre-configured SharePoint Server 2013 farms.</span></span> <span data-ttu-id="81ac2-106">To můžete ušetřit mnoho času když potřebujete základní nebo vysokou dostupností farmy služby SharePoint pro prostředí pro vývoj/testování nebo hodnocení SharePoint Server 2013 jako řešení spolupráce pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="81ac2-106">This can save you a lot of time when you need a basic or high-availability SharePoint farm for a dev/test environment or if you are evaluating SharePoint Server 2013 as a collaboration solution for your organization.</span></span>

> [!NOTE]
> <span data-ttu-id="81ac2-107">Hello **serverové farmy služby SharePoint** byla odebrána položka v hello Azure Marketplace hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="81ac2-107">hello **SharePoint Server Farm** item in hello Azure Marketplace of hello Azure portal has been removed.</span></span> <span data-ttu-id="81ac2-108">Byla nahrazena hello **jiný - HA farmy serverů Sharepointu 2013** a **farmy služby SharePoint 2013 HA** položky.</span><span class="sxs-lookup"><span data-stu-id="81ac2-108">It has been replaced with hello **SharePoint 2013 non-HA Farm** and **SharePoint 2013 HA Farm** items.</span></span>
>
>

<span data-ttu-id="81ac2-109">Hello základní farmy služby SharePoint se skládá z tři virtuální počítače v této konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="81ac2-109">hello basic SharePoint farm consists of three virtual machines in this configuration.</span></span>

![sharepointfarm](./media/sharepoint-farm/Non-HAFarm.png)

<span data-ttu-id="81ac2-111">Tuto konfiguraci farmy můžete použít pro zjednodušené nastavení pro vývoj aplikací služby SharePoint nebo prvního hodnocení produktu SharePoint 2013.</span><span class="sxs-lookup"><span data-stu-id="81ac2-111">You can use this farm configuration for a simplified setup for SharePoint app development or your first-time evaluation of SharePoint 2013.</span></span>

<span data-ttu-id="81ac2-112">toocreate hello základní (Tříserverová) farmy služby SharePoint:</span><span class="sxs-lookup"><span data-stu-id="81ac2-112">toocreate hello basic (three-server) SharePoint farm:</span></span>

1. <span data-ttu-id="81ac2-113">Klikněte na tlačítko [zde](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).</span><span class="sxs-lookup"><span data-stu-id="81ac2-113">Click [here](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).</span></span>
2. <span data-ttu-id="81ac2-114">Klikněte na tlačítko **nasazení**.</span><span class="sxs-lookup"><span data-stu-id="81ac2-114">Click **Deploy**.</span></span>
3. <span data-ttu-id="81ac2-115">Na hello **jiný - HA farmy serverů Sharepointu 2013** podokně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="81ac2-115">On hello **SharePoint 2013 non-HA Farm** pane, click **Create**.</span></span>
4. <span data-ttu-id="81ac2-116">Zadat nastavení na hello kroky hello **vytvořit jiný - HA farmy serverů Sharepointu 2013** podokně a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="81ac2-116">Specify settings on hello steps of hello **Create SharePoint 2013 non-HA Farm** pane, and then click **Create**.</span></span>

<span data-ttu-id="81ac2-117">devět virtuálních počítačů v této konfiguraci se skládá farmy služby SharePoint Hello vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="81ac2-117">hello high-availability SharePoint farm consists of nine virtual machines in this configuration.</span></span>

![sharepointfarm](./media/sharepoint-farm/HAFarm.png)

<span data-ttu-id="81ac2-119">Tato konfigurace farmy tootest vyšší klientů zatížení, vysokou dostupnost hello externí web služby SharePoint a skupin dostupnosti AlwaysOn systému SQL Server můžete použít pro farmu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="81ac2-119">You can use this farm configuration tootest higher client loads, high availability of hello external SharePoint site, and SQL Server AlwaysOn Availability Groups for a SharePoint farm.</span></span> <span data-ttu-id="81ac2-120">Tuto konfiguraci můžete použít také pro vývoj aplikací pro SharePoint v prostředí s vysokou dostupností.</span><span class="sxs-lookup"><span data-stu-id="81ac2-120">You can also use this configuration for SharePoint app development in a high-availability environment.</span></span>

<span data-ttu-id="81ac2-121">farma služby SharePoint se toocreate hello vysokou dostupnost (devět servery):</span><span class="sxs-lookup"><span data-stu-id="81ac2-121">toocreate hello high-availability (nine-server) SharePoint farm:</span></span>

1. <span data-ttu-id="81ac2-122">Klikněte na tlačítko [zde](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).</span><span class="sxs-lookup"><span data-stu-id="81ac2-122">Click [here](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).</span></span>
2. <span data-ttu-id="81ac2-123">Klikněte na tlačítko **nasazení**.</span><span class="sxs-lookup"><span data-stu-id="81ac2-123">Click **Deploy**.</span></span>
3. <span data-ttu-id="81ac2-124">Na hello **farmy služby SharePoint 2013 HA** podokně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="81ac2-124">On hello **SharePoint 2013 HA Farm** pane, click **Create**.</span></span>
4. <span data-ttu-id="81ac2-125">Zadat nastavení na hello sedm kroků hello **vytvořit farmu služby SharePoint 2013 HA** podokně a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="81ac2-125">Specify settings on hello seven steps of hello **Create SharePoint 2013 HA Farm** pane, and then click **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="81ac2-126">Nelze vytvořit hello **jiný - HA farmy serverů Sharepointu 2013** nebo **farmy služby SharePoint 2013 HA** Azure bezplatnou zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="81ac2-126">You cannot create hello **SharePoint 2013 non-HA Farm** or **SharePoint 2013 HA Farm** with an Azure Free Trial.</span></span>
>
>

<span data-ttu-id="81ac2-127">Hello portál Azure vytvoří obě tyto farmy v čistě cloudové virtuální síť s straně Internetu webová služba.</span><span class="sxs-lookup"><span data-stu-id="81ac2-127">hello Azure portal creates both of these farms in a cloud-only virtual network with an Internet-facing web presence.</span></span> <span data-ttu-id="81ac2-128">Neexistuje žádné site-to-site VPN nebo ExpressRoute připojení back tooyour síti organizace.</span><span class="sxs-lookup"><span data-stu-id="81ac2-128">There is no site-to-site VPN or ExpressRoute connection back tooyour organization network.</span></span>

> [!NOTE]
> <span data-ttu-id="81ac2-129">Pokud vytvoříte hello základní nebo farmy služby SharePoint vysokou dostupností pomocí hello portálu Azure, nemůžete zadat existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="81ac2-129">When you create hello basic or high-availability SharePoint farms using hello Azure portal, you cannot specify an existing resource group.</span></span> <span data-ttu-id="81ac2-130">toowork obejít toto omezení, vytvořte tyto farmy s prostředím Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="81ac2-130">toowork around this limitation, create these farms with Azure PowerShell.</span></span> <span data-ttu-id="81ac2-131">Další informace najdete v tématu [vytvořit SharePoint 2013 pro vývoj/testování farmy s prostředím Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).</span><span class="sxs-lookup"><span data-stu-id="81ac2-131">For more information, see [Create SharePoint 2013 dev/test farms with Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).</span></span>
>
>

## <a name="sharepoint-2016-farms"></a><span data-ttu-id="81ac2-132">Farmy služby SharePoint 2016</span><span class="sxs-lookup"><span data-stu-id="81ac2-132">SharePoint 2016 farms</span></span>
<span data-ttu-id="81ac2-133">V tématu [v tomto článku](https://technet.microsoft.com/library/mt723354.aspx) hello pokyny toobuild hello po jednom serveru SharePoint Server 2016 farmy.</span><span class="sxs-lookup"><span data-stu-id="81ac2-133">See [this article](https://technet.microsoft.com/library/mt723354.aspx) for hello instructions toobuild hello following single-server SharePoint Server 2016 farm.</span></span>

![sharepointfarm](./media/sharepoint-farm/SP2016Farm.png)

## <a name="managing-hello-sharepoint-farms"></a><span data-ttu-id="81ac2-135">Správa farmy služby SharePoint hello</span><span class="sxs-lookup"><span data-stu-id="81ac2-135">Managing hello SharePoint farms</span></span>
<span data-ttu-id="81ac2-136">Můžete spravovat servery hello tyto farmy prostřednictvím připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="81ac2-136">You can administer hello servers of these farms through Remote Desktop connections.</span></span> <span data-ttu-id="81ac2-137">Další informace najdete v tématu [protokolu na virtuálním počítači toohello](quick-create-portal.md#connect-to-virtual-machine).</span><span class="sxs-lookup"><span data-stu-id="81ac2-137">For more information, see [Log on toohello virtual machine](quick-create-portal.md#connect-to-virtual-machine).</span></span>

<span data-ttu-id="81ac2-138">Z lokality centrální správy SharePoint hello můžete nakonfigurovat svoje lokality aplikací služby SharePoint a další funkce.</span><span class="sxs-lookup"><span data-stu-id="81ac2-138">From hello Central Administration SharePoint site, you can configure My sites, SharePoint applications, and other functionality.</span></span> <span data-ttu-id="81ac2-139">Další informace najdete v tématu [konfigurace služby SharePoint](http://technet.microsoft.com/library/ee836142.aspx).</span><span class="sxs-lookup"><span data-stu-id="81ac2-139">For more information, see [Configure SharePoint](http://technet.microsoft.com/library/ee836142.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="81ac2-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81ac2-140">Next steps</span></span>
* <span data-ttu-id="81ac2-141">Zjišťovat další [konfigurace služby SharePoint](https://technet.microsoft.com/library/dn635309.aspx) ve službách infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="81ac2-141">Discover additional [SharePoint configurations](https://technet.microsoft.com/library/dn635309.aspx) in Azure infrastructure services.</span></span>
