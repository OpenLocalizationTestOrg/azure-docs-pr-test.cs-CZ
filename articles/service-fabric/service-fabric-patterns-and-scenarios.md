---
title: "Azure Service Fabric vzory a scénáře | Microsoft Docs"
description: "Další informace osvědčených postupů a předpokládá se, opakovaně použitelné vzory návrhu, vývoj a provoz vaší mikroslužeb v Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: d5aa75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2017
ms.author: ryanwi
ms.openlocfilehash: fb2fa495758433e357722427b1c162420935955d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="service-fabric-patterns-and-scenarios"></a><span data-ttu-id="cfa91-103">Vzory Service Fabric a scénáře</span><span class="sxs-lookup"><span data-stu-id="cfa91-103">Service Fabric patterns and scenarios</span></span>
<span data-ttu-id="cfa91-104">Pokud zvažujete sestavování rozsáhlých mikroslužeb pomocí Azure Service Fabric, přečtěte si od odborníků, kteří navržen a sestaven Tato platforma jako služba (PaaS).</span><span class="sxs-lookup"><span data-stu-id="cfa91-104">If you’re looking at building large-scale microservices using Azure Service Fabric, learn from the experts who designed and built this platform as a service (PaaS).</span></span> <span data-ttu-id="cfa91-105">Začínáme s správnou architekturu a pak zjistěte, jak optimalizovat prostředky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cfa91-105">Get started with proper architecture, and then learn how to optimize resources for your application.</span></span> <span data-ttu-id="cfa91-106">[Service Fabric Patterns and Practices](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) kurzu odpovídá na dotazy nejčastěji požaduje od zákazníků reálného o scénáře Service Fabric a v oblastech aplikace.</span><span class="sxs-lookup"><span data-stu-id="cfa91-106">The [Service Fabric Patterns and Practices](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) course answers the questions most often asked by real-world customers about Service Fabric scenarios and application areas.</span></span>
 
<span data-ttu-id="cfa91-107">Prostřednictvím doporučených postupů a prověřených opakovatelně použitelných vzorů se naučíte navrhovat, vyvíjet a provozovat mikroslužby na platformě Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="cfa91-107">Find out how to design, develop, and operate your microservices on Service Fabric using best practices and proven, reusable patterns.</span></span> <span data-ttu-id="cfa91-108">Získat přehled o Service Fabric a pak podrobné informace o tématech, které se týkají optimalizace clusteru a zabezpečení, migrace starší verze aplikace, IoT ve velkém měřítku, hostování herní moduly a další.</span><span class="sxs-lookup"><span data-stu-id="cfa91-108">Get an overview of Service Fabric and then dive deep into topics that cover cluster optimization and security, migrating legacy apps, IoT at scale, hosting game engines, and more.</span></span> <span data-ttu-id="cfa91-109">Podívejte se na nastavené průběžné doručování pro různé úlohy a i získat podrobnosti o podpora Linuxu a kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="cfa91-109">Look at continuous delivery for various workloads, and even get the details on Linux support and containers.</span></span> 

## <a name="introduction"></a><span data-ttu-id="cfa91-110">Úvod</span><span class="sxs-lookup"><span data-stu-id="cfa91-110">Introduction</span></span>
<span data-ttu-id="cfa91-111">Prozkoumejte osvědčené postupy a informace o výběru platforma jako služba (PaaS) přes infrastruktury jako služby (IaaS).</span><span class="sxs-lookup"><span data-stu-id="cfa91-111">Explore best practices, and learn about choosing platform as a service (PaaS) over infrastructure as a service (IaaS).</span></span> <span data-ttu-id="cfa91-112">Získáte podrobnosti o následující zásady designu ověřené aplikací.</span><span class="sxs-lookup"><span data-stu-id="cfa91-112">Get the details on following proven application design principles.</span></span>

<table><tr><th><span data-ttu-id="cfa91-113">Video</span><span class="sxs-lookup"><span data-stu-id="cfa91-113">Video</span></span></th><th><span data-ttu-id="cfa91-114">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="cfa91-114">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=N2KwbbSGD_6405167344">
<img src="./media/service-fabric-patterns-and-scenarios/intro.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="cfa91-115"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Úvod do Service Fabric</a></span><span class="sxs-lookup"><span data-stu-id="cfa91-115"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Introduction to Service Fabric</a></span></span></td></tr>
</table>

## <a name="cluster-planning-and-management"></a><span data-ttu-id="cfa91-116">Plánování a správu clusteru</span><span class="sxs-lookup"><span data-stu-id="cfa91-116">Cluster planning and management</span></span>
<span data-ttu-id="cfa91-117">Další informace o plánování kapacity, optimalizace clusteru a zabezpečení clusteru v této pohled na Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="cfa91-117">Learn about capacity planning, cluster optimization, and cluster security, in this look at Azure Service Fabric.</span></span>

<table><tr><th><span data-ttu-id="cfa91-118">Video</span><span class="sxs-lookup"><span data-stu-id="cfa91-118">Video</span></span></th><th><span data-ttu-id="cfa91-119">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="cfa91-119">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=cyDYZcSGD_2805167344">
<img src="./media/service-fabric-patterns-and-scenarios/cluster.png" WIDTH="360" HEIGHT="244">
</a></td><td> <span data-ttu-id="cfa91-120"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Plánování clusteru a správy</a></span><span class="sxs-lookup"><span data-stu-id="cfa91-120"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Cluster Planning and Management</a></span></span></td></tr>
</table>

## <a name="hyper-scale-web"></a><span data-ttu-id="cfa91-121">Webové technologie Hyper škálování</span><span class="sxs-lookup"><span data-stu-id="cfa91-121">Hyper-scale web</span></span>
<span data-ttu-id="cfa91-122">Seznamte se s koncepty kolem flexibilně škálovatelné webové, včetně dostupnost a spolehlivost, flexibilně škálovatelné a správy stavu.</span><span class="sxs-lookup"><span data-stu-id="cfa91-122">Review concepts around hyper-scale web, including availability and reliability, hyper-scale, and state management.</span></span>

<table><tr><th><span data-ttu-id="cfa91-123">Video</span><span class="sxs-lookup"><span data-stu-id="cfa91-123">Video</span></span></th><th><span data-ttu-id="cfa91-124">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="cfa91-124">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=NgldAdSGD_405167344">
<img src="./media/service-fabric-patterns-and-scenarios/hyperscaleweb.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="cfa91-125"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Webové technologie Hyper škálování</a></span><span class="sxs-lookup"><span data-stu-id="cfa91-125"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Hyper-scale web</a></span></span></td></tr>
</table>

## <a name="iot"></a><span data-ttu-id="cfa91-126">IoT</span><span class="sxs-lookup"><span data-stu-id="cfa91-126">IoT</span></span>
<span data-ttu-id="cfa91-127">Prozkoumejte Internet věcí (IoT) v rámci Azure Service Fabric, včetně kanálu Azure IoT, víceklientský a IoT ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="cfa91-127">Explore the Internet of Things (IoT) in the context of Azure Service Fabric, including the Azure IoT pipeline, multi-tenancy, and IoT at scale.</span></span>

<table><tr><th><span data-ttu-id="cfa91-128">Video</span><span class="sxs-lookup"><span data-stu-id="cfa91-128">Video</span></span></th><th><span data-ttu-id="cfa91-129">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="cfa91-129">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=naFUVeSGD_1505167344">
<img src="./media/service-fabric-patterns-and-scenarios/iot.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="cfa91-130"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></span><span class="sxs-lookup"><span data-stu-id="cfa91-130"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></span></span></td></tr>
</table>

## <a name="gaming"></a><span data-ttu-id="cfa91-131">Hraní her</span><span class="sxs-lookup"><span data-stu-id="cfa91-131">Gaming</span></span>
<span data-ttu-id="cfa91-132">Podívejte se na na základě zapnout hry, interaktivní hry a hostování existující herní moduly.</span><span class="sxs-lookup"><span data-stu-id="cfa91-132">Look at turn-based games, interactive games, and hosting existing game engines.</span></span>

<table><tr><th><span data-ttu-id="cfa91-133">Video</span><span class="sxs-lookup"><span data-stu-id="cfa91-133">Video</span></span></th><th><span data-ttu-id="cfa91-134">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="cfa91-134">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=6wECzeSGD_3805167344">
<img src="./media/service-fabric-patterns-and-scenarios/gaming.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="cfa91-135"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Herní</a></span><span class="sxs-lookup"><span data-stu-id="cfa91-135"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Gaming</a></span></span></td></tr>
</table>

## <a name="continuous-delivery"></a><span data-ttu-id="cfa91-136">Nepřetržité doručování</span><span class="sxs-lookup"><span data-stu-id="cfa91-136">Continuous delivery</span></span>
<span data-ttu-id="cfa91-137">Prozkoumejte koncepty, včetně nepřetržité integrace/průběžné doručování s Visual Studio Team Services, sestavení, balení/publikování pracovního postupu, nastavení více prostředí a služby nebo sdílené složky balíčku.</span><span class="sxs-lookup"><span data-stu-id="cfa91-137">Explore concepts, including continuous integration/continuous delivery with Visual Studio Team Services, build/package/publish workflow, multi-environment setup, and service package/share.</span></span>

<table><tr><th><span data-ttu-id="cfa91-138">Video</span><span class="sxs-lookup"><span data-stu-id="cfa91-138">Video</span></span></th><th><span data-ttu-id="cfa91-139">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="cfa91-139">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=78h5ofSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/cd.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="cfa91-140"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Nastavené průběžné doručování</a></span><span class="sxs-lookup"><span data-stu-id="cfa91-140"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Continuous delivery</a></span></span></td></tr>
</table>

## <a name="migration"></a><span data-ttu-id="cfa91-141">Migrace</span><span class="sxs-lookup"><span data-stu-id="cfa91-141">Migration</span></span>
<span data-ttu-id="cfa91-142">Další informace o migraci z cloudové služby, kromě migrace starší verze aplikací.</span><span class="sxs-lookup"><span data-stu-id="cfa91-142">Learn about migrating from a cloud service, in addition to migration of legacy apps.</span></span>

<table><tr><th><span data-ttu-id="cfa91-143">Video</span><span class="sxs-lookup"><span data-stu-id="cfa91-143">Video</span></span></th><th><span data-ttu-id="cfa91-144">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="cfa91-144">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=hd1cMgSGD_5605167344">
<img src="./media/service-fabric-patterns-and-scenarios/migration.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="cfa91-145"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Migrace</a></span><span class="sxs-lookup"><span data-stu-id="cfa91-145"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Migration</a></span></span></td></tr>
</table>

## <a name="containers-and-linux-support"></a><span data-ttu-id="cfa91-146">Kontejnery a podpora Linuxu</span><span class="sxs-lookup"><span data-stu-id="cfa91-146">Containers and Linux support</span></span>
<span data-ttu-id="cfa91-147">Získat odpověď na otázku "Proč kontejnery?"</span><span class="sxs-lookup"><span data-stu-id="cfa91-147">Get the answer to the question, "Why containers?"</span></span> <span data-ttu-id="cfa91-148">Další informace o verzi preview pro kontejnery Windows, Linux podporuje a orchestraci kontejnerů Linux.</span><span class="sxs-lookup"><span data-stu-id="cfa91-148">Learn about the preview for Windows containers, Linux supports, and Linux containers orchestration.</span></span> <span data-ttu-id="cfa91-149">Navíc můžete zjistit, jak migrovat aplikace .NET Core Linux.</span><span class="sxs-lookup"><span data-stu-id="cfa91-149">Plus, find out how to migrate .NET Core apps to Linux.</span></span>

<table><tr><th><span data-ttu-id="cfa91-150">Video</span><span class="sxs-lookup"><span data-stu-id="cfa91-150">Video</span></span></th><th><span data-ttu-id="cfa91-151">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="cfa91-151">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=V1ERJhSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/containers.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="cfa91-152"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Kontejnery a podpora Linuxu</a></span><span class="sxs-lookup"><span data-stu-id="cfa91-152"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Containers and Linux support</a></span></span></td></tr>
</table>

## <a name="next-steps"></a><span data-ttu-id="cfa91-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cfa91-153">Next steps</span></span>
<span data-ttu-id="cfa91-154">Teď, když jste se naučili o vzory Service Fabric a scénáře, přečtěte si více o tom, jak [vytváření a správě clusterů](service-fabric-deploy-anywhere.md), [migrace aplikací cloudové služby do Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [nastavení nastavené průběžné doručování](service-fabric-set-up-continuous-integration.md), a [nasazení kontejnerů](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cfa91-154">Now that you've learned about Service Fabric patterns and scenarios, read more about how to [create and manage clusters](service-fabric-deploy-anywhere.md), [migrate Cloud Services apps to Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [set up continuous delivery](service-fabric-set-up-continuous-integration.md), and [deploy containers](service-fabric-containers-overview.md).</span></span>
