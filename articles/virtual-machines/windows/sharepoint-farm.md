---
title: "Vytvoření serverové farmy služby SharePoint v Azure | Microsoft Docs"
description: "Rychle vytvořte novou farmu služby SharePoint 2013 nebo SharePoint 2016 v Azure pomocí portálu Azure marketplace."
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
ms.openlocfilehash: 00648ee35962b22fb7eeceaa6d9df7305c801abf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-sharepoint-server-farms-using-the-azure-portal-marketplace"></a><span data-ttu-id="e1cd4-103">Vytvoření serverové farmy služby SharePoint pomocí portálu Azure marketplace</span><span class="sxs-lookup"><span data-stu-id="e1cd4-103">Create SharePoint server farms using the Azure portal marketplace</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="sharepoint-2013-farms"></a><span data-ttu-id="e1cd4-104">Farmy služby SharePoint 2013</span><span class="sxs-lookup"><span data-stu-id="e1cd4-104">SharePoint 2013 farms</span></span>
<span data-ttu-id="e1cd4-105">Marketplace portálu Microsoft Azure můžete rychle vytvořit předem nakonfigurované farmy služby SharePoint Server 2013.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-105">With the Microsoft Azure portal marketplace, you can quickly create pre-configured SharePoint Server 2013 farms.</span></span> <span data-ttu-id="e1cd4-106">To můžete ušetřit mnoho času když potřebujete základní nebo vysokou dostupností farmy služby SharePoint pro prostředí pro vývoj/testování nebo hodnocení SharePoint Server 2013 jako řešení spolupráce pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-106">This can save you a lot of time when you need a basic or high-availability SharePoint farm for a dev/test environment or if you are evaluating SharePoint Server 2013 as a collaboration solution for your organization.</span></span>

> [!NOTE]
> <span data-ttu-id="e1cd4-107">**Serverové farmy služby SharePoint** byla odebrána položka v Azure Marketplace portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-107">The **SharePoint Server Farm** item in the Azure Marketplace of the Azure portal has been removed.</span></span> <span data-ttu-id="e1cd4-108">Byla nahrazena **jiný - HA farmy serverů Sharepointu 2013** a **farmy služby SharePoint 2013 HA** položky.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-108">It has been replaced with the **SharePoint 2013 non-HA Farm** and **SharePoint 2013 HA Farm** items.</span></span>
>
>

<span data-ttu-id="e1cd4-109">Základní farmy služby SharePoint se skládá z tři virtuální počítače v této konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-109">The basic SharePoint farm consists of three virtual machines in this configuration.</span></span>

![sharepointfarm](./media/sharepoint-farm/Non-HAFarm.png)

<span data-ttu-id="e1cd4-111">Tuto konfiguraci farmy můžete použít pro zjednodušené nastavení pro vývoj aplikací služby SharePoint nebo prvního hodnocení produktu SharePoint 2013.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-111">You can use this farm configuration for a simplified setup for SharePoint app development or your first-time evaluation of SharePoint 2013.</span></span>

<span data-ttu-id="e1cd4-112">Pokud chcete vytvořit základní farmy služby SharePoint (třemi servery):</span><span class="sxs-lookup"><span data-stu-id="e1cd4-112">To create the basic (three-server) SharePoint farm:</span></span>

1. <span data-ttu-id="e1cd4-113">Klikněte na tlačítko [zde](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).</span><span class="sxs-lookup"><span data-stu-id="e1cd4-113">Click [here](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).</span></span>
2. <span data-ttu-id="e1cd4-114">Klikněte na tlačítko **nasazení**.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-114">Click **Deploy**.</span></span>
3. <span data-ttu-id="e1cd4-115">Na **jiný - HA farmy serverů Sharepointu 2013** podokně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-115">On the **SharePoint 2013 non-HA Farm** pane, click **Create**.</span></span>
4. <span data-ttu-id="e1cd4-116">Zadat nastavení na kroky **vytvořit jiný - HA farmy serverů Sharepointu 2013** podokně a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-116">Specify settings on the steps of the **Create SharePoint 2013 non-HA Farm** pane, and then click **Create**.</span></span>

<span data-ttu-id="e1cd4-117">Vysoká dostupnost farmy služby SharePoint se skládá z devíti virtuální počítače v této konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-117">The high-availability SharePoint farm consists of nine virtual machines in this configuration.</span></span>

![sharepointfarm](./media/sharepoint-farm/HAFarm.png)

<span data-ttu-id="e1cd4-119">Tuto konfiguraci farmy můžete použít k testování větší objemy klienta, vysokou dostupnost externí web služby SharePoint a skupin dostupnosti AlwaysOn SQL serveru pro farmu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-119">You can use this farm configuration to test higher client loads, high availability of the external SharePoint site, and SQL Server AlwaysOn Availability Groups for a SharePoint farm.</span></span> <span data-ttu-id="e1cd4-120">Tuto konfiguraci můžete použít také pro vývoj aplikací pro SharePoint v prostředí s vysokou dostupností.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-120">You can also use this configuration for SharePoint app development in a high-availability environment.</span></span>

<span data-ttu-id="e1cd4-121">Pokud chcete vytvořit farmu služby SharePoint vysokou dostupnost (devět server):</span><span class="sxs-lookup"><span data-stu-id="e1cd4-121">To create the high-availability (nine-server) SharePoint farm:</span></span>

1. <span data-ttu-id="e1cd4-122">Klikněte na tlačítko [zde](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).</span><span class="sxs-lookup"><span data-stu-id="e1cd4-122">Click [here](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).</span></span>
2. <span data-ttu-id="e1cd4-123">Klikněte na tlačítko **nasazení**.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-123">Click **Deploy**.</span></span>
3. <span data-ttu-id="e1cd4-124">Na **farmy služby SharePoint 2013 HA** podokně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-124">On the **SharePoint 2013 HA Farm** pane, click **Create**.</span></span>
4. <span data-ttu-id="e1cd4-125">Zadat nastavení na sedm kroky **vytvořit farmu služby SharePoint 2013 HA** podokně a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-125">Specify settings on the seven steps of the **Create SharePoint 2013 HA Farm** pane, and then click **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="e1cd4-126">Nelze vytvořit **jiný - HA farmy serverů Sharepointu 2013** nebo **farmy služby SharePoint 2013 HA** Azure bezplatnou zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-126">You cannot create the **SharePoint 2013 non-HA Farm** or **SharePoint 2013 HA Farm** with an Azure Free Trial.</span></span>
>
>

<span data-ttu-id="e1cd4-127">Portál Azure vytvoří obě tyto farmy v čistě cloudové virtuální síť s straně Internetu webová služba.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-127">The Azure portal creates both of these farms in a cloud-only virtual network with an Internet-facing web presence.</span></span> <span data-ttu-id="e1cd4-128">Neexistuje žádné připojení site-to-site VPN nebo ExpressRoute zpět k síti vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-128">There is no site-to-site VPN or ExpressRoute connection back to your organization network.</span></span>

> [!NOTE]
> <span data-ttu-id="e1cd4-129">Když vytvoříte základní nebo farmy služby SharePoint vysokou dostupností pomocí portálu Azure, nemůžete zadat existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-129">When you create the basic or high-availability SharePoint farms using the Azure portal, you cannot specify an existing resource group.</span></span> <span data-ttu-id="e1cd4-130">Toto omezení obejít, vytvořte tyto farmy s prostředím Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-130">To work around this limitation, create these farms with Azure PowerShell.</span></span> <span data-ttu-id="e1cd4-131">Další informace najdete v tématu [vytvořit SharePoint 2013 pro vývoj/testování farmy s prostředím Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).</span><span class="sxs-lookup"><span data-stu-id="e1cd4-131">For more information, see [Create SharePoint 2013 dev/test farms with Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).</span></span>
>
>

## <a name="sharepoint-2016-farms"></a><span data-ttu-id="e1cd4-132">Farmy služby SharePoint 2016</span><span class="sxs-lookup"><span data-stu-id="e1cd4-132">SharePoint 2016 farms</span></span>
<span data-ttu-id="e1cd4-133">V tématu [v tomto článku](https://technet.microsoft.com/library/mt723354.aspx) pokyny k vytvoření následujících farmy služby SharePoint Server 2016 jedním serverem.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-133">See [this article](https://technet.microsoft.com/library/mt723354.aspx) for the instructions to build the following single-server SharePoint Server 2016 farm.</span></span>

![sharepointfarm](./media/sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a><span data-ttu-id="e1cd4-135">Správa farmy služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="e1cd4-135">Managing the SharePoint farms</span></span>
<span data-ttu-id="e1cd4-136">Můžete spravovat servery tyto farmy prostřednictvím připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-136">You can administer the servers of these farms through Remote Desktop connections.</span></span> <span data-ttu-id="e1cd4-137">Další informace najdete v tématu [Přihlaste se k virtuálnímu počítači](quick-create-portal.md#connect-to-virtual-machine).</span><span class="sxs-lookup"><span data-stu-id="e1cd4-137">For more information, see [Log on to the virtual machine](quick-create-portal.md#connect-to-virtual-machine).</span></span>

<span data-ttu-id="e1cd4-138">Z lokality centrální správy SharePoint můžete nakonfigurovat svoje lokality aplikací služby SharePoint a další funkce.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-138">From the Central Administration SharePoint site, you can configure My sites, SharePoint applications, and other functionality.</span></span> <span data-ttu-id="e1cd4-139">Další informace najdete v tématu [konfigurace služby SharePoint](http://technet.microsoft.com/library/ee836142.aspx).</span><span class="sxs-lookup"><span data-stu-id="e1cd4-139">For more information, see [Configure SharePoint](http://technet.microsoft.com/library/ee836142.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1cd4-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e1cd4-140">Next steps</span></span>
* <span data-ttu-id="e1cd4-141">Zjišťovat další [konfigurace služby SharePoint](https://technet.microsoft.com/library/dn635309.aspx) ve službách infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-141">Discover additional [SharePoint configurations](https://technet.microsoft.com/library/dn635309.aspx) in Azure infrastructure services.</span></span>
