---
title: "aaaAzure Service Fabric vzory a scénáře | Microsoft Docs"
description: "Další informace osvědčených postupů a předpokládá se, opakovaně použitelné vzory toodesign vývoj a provoz vaší mikroslužeb v Service Fabric."
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
ms.openlocfilehash: 3811420eb53d9a49e490dd2e2e5319d8dea5629c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-patterns-and-scenarios"></a><span data-ttu-id="182b9-103">Vzory Service Fabric a scénáře</span><span class="sxs-lookup"><span data-stu-id="182b9-103">Service Fabric patterns and scenarios</span></span>
<span data-ttu-id="182b9-104">Pokud zvažujete sestavování rozsáhlých mikroslužeb pomocí Azure Service Fabric, přečtěte si od odborníků hello, kteří navržen a sestaven Tato platforma jako služba (PaaS).</span><span class="sxs-lookup"><span data-stu-id="182b9-104">If you’re looking at building large-scale microservices using Azure Service Fabric, learn from hello experts who designed and built this platform as a service (PaaS).</span></span> <span data-ttu-id="182b9-105">Začínáme s správnou architekturu a dozvíte se, jak toooptimize prostředky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="182b9-105">Get started with proper architecture, and then learn how toooptimize resources for your application.</span></span> <span data-ttu-id="182b9-106">Hello [Service Fabric Patterns and Practices](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) kurzu odpovědi nejčastěji kladené reálného zákazníci scénáře Service Fabric a v oblastech aplikace hello otázky.</span><span class="sxs-lookup"><span data-stu-id="182b9-106">hello [Service Fabric Patterns and Practices](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) course answers hello questions most often asked by real-world customers about Service Fabric scenarios and application areas.</span></span>
 
<span data-ttu-id="182b9-107">Zjistěte, jak toodesign, vývoj a provoz vaší mikroslužeb v Service Fabric pomocí osvědčené postupy a principy, opakovaně použitelný vzorů.</span><span class="sxs-lookup"><span data-stu-id="182b9-107">Find out how toodesign, develop, and operate your microservices on Service Fabric using best practices and proven, reusable patterns.</span></span> <span data-ttu-id="182b9-108">Získat přehled o Service Fabric a pak podrobné informace o tématech, které se týkají optimalizace clusteru a zabezpečení, migrace starší verze aplikace, IoT ve velkém měřítku, hostování herní moduly a další.</span><span class="sxs-lookup"><span data-stu-id="182b9-108">Get an overview of Service Fabric and then dive deep into topics that cover cluster optimization and security, migrating legacy apps, IoT at scale, hosting game engines, and more.</span></span> <span data-ttu-id="182b9-109">Podívejte se na nastavené průběžné doručování pro různé úlohy a dokonce získat podrobnosti o hello na podporu Linux a kontejnery.</span><span class="sxs-lookup"><span data-stu-id="182b9-109">Look at continuous delivery for various workloads, and even get hello details on Linux support and containers.</span></span> 

## <a name="introduction"></a><span data-ttu-id="182b9-110">Úvod</span><span class="sxs-lookup"><span data-stu-id="182b9-110">Introduction</span></span>
<span data-ttu-id="182b9-111">Prozkoumejte osvědčené postupy a informace o výběru platforma jako služba (PaaS) přes infrastruktury jako služby (IaaS).</span><span class="sxs-lookup"><span data-stu-id="182b9-111">Explore best practices, and learn about choosing platform as a service (PaaS) over infrastructure as a service (IaaS).</span></span> <span data-ttu-id="182b9-112">Získáte podrobnosti o hello na následující zásady designu ověřené aplikací.</span><span class="sxs-lookup"><span data-stu-id="182b9-112">Get hello details on following proven application design principles.</span></span>

<table><tr><th><span data-ttu-id="182b9-113">Video</span><span class="sxs-lookup"><span data-stu-id="182b9-113">Video</span></span></th><th><span data-ttu-id="182b9-114">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="182b9-114">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=N2KwbbSGD_6405167344">
<img src="./media/service-fabric-patterns-and-scenarios/intro.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="182b9-115"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Úvod tooService prostředků infrastruktury</a></span><span class="sxs-lookup"><span data-stu-id="182b9-115"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Introduction tooService Fabric</a></span></span></td></tr>
</table>

## <a name="cluster-planning-and-management"></a><span data-ttu-id="182b9-116">Plánování a správu clusteru</span><span class="sxs-lookup"><span data-stu-id="182b9-116">Cluster planning and management</span></span>
<span data-ttu-id="182b9-117">Další informace o plánování kapacity, optimalizace clusteru a zabezpečení clusteru v této pohled na Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="182b9-117">Learn about capacity planning, cluster optimization, and cluster security, in this look at Azure Service Fabric.</span></span>

<table><tr><th><span data-ttu-id="182b9-118">Video</span><span class="sxs-lookup"><span data-stu-id="182b9-118">Video</span></span></th><th><span data-ttu-id="182b9-119">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="182b9-119">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=cyDYZcSGD_2805167344">
<img src="./media/service-fabric-patterns-and-scenarios/cluster.png" WIDTH="360" HEIGHT="244">
</a></td><td> <span data-ttu-id="182b9-120"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Plánování clusteru a správy</a></span><span class="sxs-lookup"><span data-stu-id="182b9-120"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Cluster Planning and Management</a></span></span></td></tr>
</table>

## <a name="hyper-scale-web"></a><span data-ttu-id="182b9-121">Webové technologie Hyper škálování</span><span class="sxs-lookup"><span data-stu-id="182b9-121">Hyper-scale web</span></span>
<span data-ttu-id="182b9-122">Seznamte se s koncepty kolem flexibilně škálovatelné webové, včetně dostupnost a spolehlivost, flexibilně škálovatelné a správy stavu.</span><span class="sxs-lookup"><span data-stu-id="182b9-122">Review concepts around hyper-scale web, including availability and reliability, hyper-scale, and state management.</span></span>

<table><tr><th><span data-ttu-id="182b9-123">Video</span><span class="sxs-lookup"><span data-stu-id="182b9-123">Video</span></span></th><th><span data-ttu-id="182b9-124">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="182b9-124">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=NgldAdSGD_405167344">
<img src="./media/service-fabric-patterns-and-scenarios/hyperscaleweb.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="182b9-125"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Webové technologie Hyper škálování</a></span><span class="sxs-lookup"><span data-stu-id="182b9-125"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Hyper-scale web</a></span></span></td></tr>
</table>

## <a name="iot"></a><span data-ttu-id="182b9-126">IoT</span><span class="sxs-lookup"><span data-stu-id="182b9-126">IoT</span></span>
<span data-ttu-id="182b9-127">Prozkoumejte hello Internet věcí (IoT) v kontextu hello Azure Service Fabric, včetně hello Azure IoT kanálu, víceklientský a IoT ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="182b9-127">Explore hello Internet of Things (IoT) in hello context of Azure Service Fabric, including hello Azure IoT pipeline, multi-tenancy, and IoT at scale.</span></span>

<table><tr><th><span data-ttu-id="182b9-128">Video</span><span class="sxs-lookup"><span data-stu-id="182b9-128">Video</span></span></th><th><span data-ttu-id="182b9-129">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="182b9-129">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=naFUVeSGD_1505167344">
<img src="./media/service-fabric-patterns-and-scenarios/iot.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="182b9-130"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></span><span class="sxs-lookup"><span data-stu-id="182b9-130"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></span></span></td></tr>
</table>

## <a name="gaming"></a><span data-ttu-id="182b9-131">Hraní her</span><span class="sxs-lookup"><span data-stu-id="182b9-131">Gaming</span></span>
<span data-ttu-id="182b9-132">Podívejte se na na základě zapnout hry, interaktivní hry a hostování existující herní moduly.</span><span class="sxs-lookup"><span data-stu-id="182b9-132">Look at turn-based games, interactive games, and hosting existing game engines.</span></span>

<table><tr><th><span data-ttu-id="182b9-133">Video</span><span class="sxs-lookup"><span data-stu-id="182b9-133">Video</span></span></th><th><span data-ttu-id="182b9-134">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="182b9-134">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=6wECzeSGD_3805167344">
<img src="./media/service-fabric-patterns-and-scenarios/gaming.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="182b9-135"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Herní</a></span><span class="sxs-lookup"><span data-stu-id="182b9-135"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Gaming</a></span></span></td></tr>
</table>

## <a name="continuous-delivery"></a><span data-ttu-id="182b9-136">Nepřetržité doručování</span><span class="sxs-lookup"><span data-stu-id="182b9-136">Continuous delivery</span></span>
<span data-ttu-id="182b9-137">Prozkoumejte koncepty, včetně nepřetržité integrace/průběžné doručování s Visual Studio Team Services, sestavení, balení/publikování pracovního postupu, nastavení více prostředí a služby nebo sdílené složky balíčku.</span><span class="sxs-lookup"><span data-stu-id="182b9-137">Explore concepts, including continuous integration/continuous delivery with Visual Studio Team Services, build/package/publish workflow, multi-environment setup, and service package/share.</span></span>

<table><tr><th><span data-ttu-id="182b9-138">Video</span><span class="sxs-lookup"><span data-stu-id="182b9-138">Video</span></span></th><th><span data-ttu-id="182b9-139">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="182b9-139">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=78h5ofSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/cd.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="182b9-140"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Nastavené průběžné doručování</a></span><span class="sxs-lookup"><span data-stu-id="182b9-140"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Continuous delivery</a></span></span></td></tr>
</table>

## <a name="migration"></a><span data-ttu-id="182b9-141">Migrace</span><span class="sxs-lookup"><span data-stu-id="182b9-141">Migration</span></span>
<span data-ttu-id="182b9-142">Další informace o migraci z cloudové služby, kromě toomigration starší verze aplikací.</span><span class="sxs-lookup"><span data-stu-id="182b9-142">Learn about migrating from a cloud service, in addition toomigration of legacy apps.</span></span>

<table><tr><th><span data-ttu-id="182b9-143">Video</span><span class="sxs-lookup"><span data-stu-id="182b9-143">Video</span></span></th><th><span data-ttu-id="182b9-144">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="182b9-144">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=hd1cMgSGD_5605167344">
<img src="./media/service-fabric-patterns-and-scenarios/migration.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="182b9-145"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Migrace</a></span><span class="sxs-lookup"><span data-stu-id="182b9-145"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Migration</a></span></span></td></tr>
</table>

## <a name="containers-and-linux-support"></a><span data-ttu-id="182b9-146">Kontejnery a podpora Linuxu</span><span class="sxs-lookup"><span data-stu-id="182b9-146">Containers and Linux support</span></span>
<span data-ttu-id="182b9-147">Získat hello odpovědí toohello otázku, "Proč kontejnery?"</span><span class="sxs-lookup"><span data-stu-id="182b9-147">Get hello answer toohello question, "Why containers?"</span></span> <span data-ttu-id="182b9-148">Další informace o hello preview pro kontejnery Windows, Linux podporuje a orchestraci kontejnerů Linux.</span><span class="sxs-lookup"><span data-stu-id="182b9-148">Learn about hello preview for Windows containers, Linux supports, and Linux containers orchestration.</span></span> <span data-ttu-id="182b9-149">Zjistit plus, jak tooLinux aplikace toomigrate .NET Core.</span><span class="sxs-lookup"><span data-stu-id="182b9-149">Plus, find out how toomigrate .NET Core apps tooLinux.</span></span>

<table><tr><th><span data-ttu-id="182b9-150">Video</span><span class="sxs-lookup"><span data-stu-id="182b9-150">Video</span></span></th><th><span data-ttu-id="182b9-151">Balíček aplikace PowerPoint</span><span class="sxs-lookup"><span data-stu-id="182b9-151">PowerPoint deck</span></span></th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=V1ERJhSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/containers.png" WIDTH="360" HEIGHT="244">
</a></td><td><span data-ttu-id="182b9-152"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Kontejnery a podpora Linuxu</a></span><span class="sxs-lookup"><span data-stu-id="182b9-152"><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Containers and Linux support</a></span></span></td></tr>
</table>

## <a name="next-steps"></a><span data-ttu-id="182b9-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="182b9-153">Next steps</span></span>
<span data-ttu-id="182b9-154">Teď, když jste se naučili o vzory Service Fabric a scénáře, další informace o tom, jak příliš[vytváření a správě clusterů](service-fabric-deploy-anywhere.md), [migrovat tooService aplikace cloudové služby Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [nastavení nastavené průběžné doručování](service-fabric-set-up-continuous-integration.md), a [nasazení kontejnerů](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="182b9-154">Now that you've learned about Service Fabric patterns and scenarios, read more about how too[create and manage clusters](service-fabric-deploy-anywhere.md), [migrate Cloud Services apps tooService Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [set up continuous delivery](service-fabric-set-up-continuous-integration.md), and [deploy containers](service-fabric-containers-overview.md).</span></span>
