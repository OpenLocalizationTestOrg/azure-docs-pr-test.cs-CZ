---
title: "Skupiny prostředků pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Další informace o klíčových návrhu a implementace pokyny pro nasazení skupin prostředků v služby infrastruktury Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0fbcabcd-e96d-4d71-a526-431984887451
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0aca77af04f895d516f4943188d7890ae9586b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-resource-group-guidelines-for-windows-vms"></a><span data-ttu-id="5efed-103">Pokyny pro skupinu prostředků Azure pro virtuální počítače Windows</span><span class="sxs-lookup"><span data-stu-id="5efed-103">Azure resource group guidelines for Windows VMs</span></span>

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

<span data-ttu-id="5efed-104">Tento článek se zaměřuje na vědět, jak se logicky vytváří prostředí a skupiny všechny součásti ve skupinách prostředků.</span><span class="sxs-lookup"><span data-stu-id="5efed-104">This article focuses on understanding how to logically build out your environment and group all the components in Resource Groups.</span></span>

## <a name="implementation-guidelines-for-resource-groups"></a><span data-ttu-id="5efed-105">Postup implementace pro skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5efed-105">Implementation guidelines for Resource Groups</span></span>
<span data-ttu-id="5efed-106">Rozhodnutí:</span><span class="sxs-lookup"><span data-stu-id="5efed-106">Decisions:</span></span>

* <span data-ttu-id="5efed-107">Chystáte se vytváří skupiny prostředků základních komponent infrastruktury, nebo nasazení aplikace dokončena?</span><span class="sxs-lookup"><span data-stu-id="5efed-107">Are you going to build out Resource Groups by the core infrastructure components, or by complete application deployment?</span></span>
* <span data-ttu-id="5efed-108">Potřebujete omezit přístup ke skupinám prostředků pomocí řízení přístupu na základě rolí?</span><span class="sxs-lookup"><span data-stu-id="5efed-108">Do you need to restrict access to Resource Groups using Role-Based Access Controls?</span></span>

<span data-ttu-id="5efed-109">Úlohy:</span><span class="sxs-lookup"><span data-stu-id="5efed-109">Tasks:</span></span>

* <span data-ttu-id="5efed-110">Definujte, co základní součásti infrastruktury a vyhrazené skupiny prostředků, je nutné.</span><span class="sxs-lookup"><span data-stu-id="5efed-110">Define what core infrastructure components and dedicated Resource Groups you need.</span></span>
* <span data-ttu-id="5efed-111">Zkontrolujte, jak implementovat šablony Resource Manageru pro konzistentní a reprodukovatelné nasazení.</span><span class="sxs-lookup"><span data-stu-id="5efed-111">Review how to implement Resource Manager templates for consistent, reproducible deployments.</span></span>
* <span data-ttu-id="5efed-112">Definování rolí přístup jaké uživatele je nutné pro řízení přístupu ke skupinám prostředků.</span><span class="sxs-lookup"><span data-stu-id="5efed-112">Define what user access roles you need for controlling access to Resource Groups.</span></span>
* <span data-ttu-id="5efed-113">Vytvořte sadu skupin prostředků pomocí zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="5efed-113">Create the set of Resource Groups using your naming convention.</span></span> <span data-ttu-id="5efed-114">Můžete použít Azure PowerShell nebo portálu.</span><span class="sxs-lookup"><span data-stu-id="5efed-114">You can use Azure PowerShell or the portal.</span></span>

## <a name="resource-groups"></a><span data-ttu-id="5efed-115">Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5efed-115">Resource Groups</span></span>
<span data-ttu-id="5efed-116">V Azure logicky seskupit související prostředky, například účty úložiště, virtuální sítě a virtuální počítače (VM) nasadit, spravovat a udržovat je jako jedna entita.</span><span class="sxs-lookup"><span data-stu-id="5efed-116">In Azure, you logically group related resources such as storage accounts, virtual networks, and virtual machines (VMs) to deploy, manage, and maintain them as a single entity.</span></span> <span data-ttu-id="5efed-117">Tento přístup je jednodušší, k nasazení aplikací a zachovat přitom všechny související prostředky z hlediska správy nebo jiné udělit přístup k této skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="5efed-117">This approach makes it easier to deploy applications while keeping all the related resources together from a management perspective, or to grant others access to that group of resources.</span></span> <span data-ttu-id="5efed-118">Názvy skupin prostředků, může být maximálně 90 znaků.</span><span class="sxs-lookup"><span data-stu-id="5efed-118">Resource group names can be a maximum of 90 characters in length.</span></span> <span data-ttu-id="5efed-119">Získat komplexnější přehled o skupinách prostředků, přečtěte si [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5efed-119">For a more comprehensive understanding of Resource Groups, read the [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="5efed-120">Klíčovou funkcí na skupiny prostředků je možnost vytváření out prostředí pomocí šablon.</span><span class="sxs-lookup"><span data-stu-id="5efed-120">A key feature to Resource Groups is ability to build out your environment using templates.</span></span> <span data-ttu-id="5efed-121">Šablonu je jednoduše soubor JSON, který deklaruje úložiště, sítě a výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="5efed-121">A template is simply a JSON file that declares the storage, networking, and compute resources.</span></span> <span data-ttu-id="5efed-122">Můžete také definovat vlastní související skripty nebo konfigurace, které platí.</span><span class="sxs-lookup"><span data-stu-id="5efed-122">You can also define any related custom scripts or configurations to apply.</span></span> <span data-ttu-id="5efed-123">Pomocí těchto šablon, vytvořit konzistentní a reprodukovatelné nasazení pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="5efed-123">By using these templates, you create consistent, reproducible deployments for your applications.</span></span> <span data-ttu-id="5efed-124">Tento přístup umožňuje snadné sestavení se prostředí při vývoji a pak použít tento stejné šablony pro vytvoření produkční nasazení, nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="5efed-124">This approach makes it easy to build out an environment in development and then use that same template to create a production deployment, or vice versa.</span></span> <span data-ttu-id="5efed-125">Pro lepší porozumění pomocí šablon, přečtěte si [názorný Průvodce šablonou](../../azure-resource-manager/resource-manager-template-walkthrough.md) který vás provede každého kroku vytváření šablony.</span><span class="sxs-lookup"><span data-stu-id="5efed-125">For a better understanding using templates, read [the template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md) that guides you through each step of the building out a template.</span></span>

<span data-ttu-id="5efed-126">Existují dva různé přístupy, které můžete provést při návrhu prostředí ke skupinám prostředků:</span><span class="sxs-lookup"><span data-stu-id="5efed-126">There are two different approaches you can take when designing your environment with Resource Groups:</span></span>

* <span data-ttu-id="5efed-127">Skupiny prostředků pro každé nasazení aplikace, které kombinuje účty úložiště, virtuální sítě a podsítě virtuálních počítačů, načíst vyrovnávání atd.</span><span class="sxs-lookup"><span data-stu-id="5efed-127">Resource Groups for each application deployment that combines the storage accounts, virtual networks, and subnets, VMs, load balancers, etc.</span></span>
* <span data-ttu-id="5efed-128">Centralizované skupin prostředků, které obsahují vaše základní virtuální sítě a podsítě nebo úložiště účtů.</span><span class="sxs-lookup"><span data-stu-id="5efed-128">Centralized Resource Groups that contain your core virtual networking and subnets or storage accounts.</span></span> <span data-ttu-id="5efed-129">Aplikace se pak ve své vlastní skupiny prostředků, které obsahovat pouze virtuální počítače, nástroje pro vyrovnávání zatížení, síťových rozhraní, atd.</span><span class="sxs-lookup"><span data-stu-id="5efed-129">Your applications are then in their own Resource Groups that only contain VMs, load balancers, network interfaces, etc.</span></span>

<span data-ttu-id="5efed-130">Jak vám horizontální navýšení kapacity, vytváření centralizované skupiny prostředků pro virtuální sítě a podsítě usnadňuje sestavení mezi různými místy síťová připojení pro možnosti hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="5efed-130">As you scale out, creating centralized Resource Groups for your virtual networking and subnets makes it easier to build cross-premises network connections for hybrid connectivity options.</span></span> <span data-ttu-id="5efed-131">Alternativní způsob je pro každou aplikaci tak, aby měl své vlastní virtuální síť, která vyžaduje konfiguraci a údržbu.</span><span class="sxs-lookup"><span data-stu-id="5efed-131">The alternative approach is for each application to have their own virtual network that requires configuration and maintenance.</span></span>  <span data-ttu-id="5efed-132">[Řízení přístupu na základě rolí](../../active-directory/role-based-access-control-what-is.md) poskytnout podrobné způsob, jak řídit přístup ke skupinám prostředků.</span><span class="sxs-lookup"><span data-stu-id="5efed-132">[Role-Based Access Controls](../../active-directory/role-based-access-control-what-is.md) provide a granular way to control access to Resource Groups.</span></span> <span data-ttu-id="5efed-133">Aplikacích v produkčním prostředí můžete řídit uživatele, kteří mohou přistupovat k tyto prostředky, nebo pro prostředky infrastruktury základní můžete omezit pouze technici infrastruktury pro práci s nimi.</span><span class="sxs-lookup"><span data-stu-id="5efed-133">For production applications, you can control the users that may access those resources, or for the core infrastructure resources you can limit only infrastructure engineers to work with them.</span></span> <span data-ttu-id="5efed-134">Vaše vlastníci aplikace mají jenom přístup k součásti aplikace v rámci jejich skupiny prostředků a ne jádro infrastrukturu Azure vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="5efed-134">Your application owners only have access to the application components within their Resource Group and not the core Azure infrastructure of your environment.</span></span> <span data-ttu-id="5efed-135">Při navrhování prostředí, vezměte v úvahu uživatele, kteří potřebují přístup k prostředkům a odpovídajícím způsobem návrh vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5efed-135">As you design your environment, consider the users that require access to the resources and design your Resource Groups accordingly.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5efed-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5efed-136">Next steps</span></span>
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]
