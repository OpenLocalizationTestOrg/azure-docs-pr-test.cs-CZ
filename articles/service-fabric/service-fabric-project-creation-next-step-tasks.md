---
title: "Další kroky aaaService Fabric projektu vytvoření | Microsoft Docs"
description: "Tento článek obsahuje odkazy tooa sadu základní vývojářských úloh pro Service Fabric"
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: rwike77
ms.openlocfilehash: 45598bfabedf280fba8af449ef920f40b409a609
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a><span data-ttu-id="c48e7-103">Vaše aplikace Service Fabric a další kroky</span><span class="sxs-lookup"><span data-stu-id="c48e7-103">Your Service Fabric application and next steps</span></span>
<span data-ttu-id="c48e7-104">Vaše aplikace Azure Service Fabric byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="c48e7-104">Your Azure Service Fabric application has been created.</span></span> <span data-ttu-id="c48e7-105">Tento článek popisuje způsob vytvoření hello projektu a některé potenciální další kroky.</span><span class="sxs-lookup"><span data-stu-id="c48e7-105">This article describes hello makeup of your project and some potential next steps.</span></span>

## <a name="your-application"></a><span data-ttu-id="c48e7-106">Vaše aplikace</span><span class="sxs-lookup"><span data-stu-id="c48e7-106">Your application</span></span>
<span data-ttu-id="c48e7-107">Každé nové aplikace zahrnuje projekt aplikace.</span><span class="sxs-lookup"><span data-stu-id="c48e7-107">Every new application includes an application project.</span></span> <span data-ttu-id="c48e7-108">Může mít jeden nebo dva další projekty, v závislosti na typu hello služby vybrali.</span><span class="sxs-lookup"><span data-stu-id="c48e7-108">There may be one or two additional projects, depending on hello type of service chosen.</span></span>

### <a name="hello-application-project"></a><span data-ttu-id="c48e7-109">projekt aplikace Hello</span><span class="sxs-lookup"><span data-stu-id="c48e7-109">hello application project</span></span>
<span data-ttu-id="c48e7-110">projekt aplikace Hello se skládá z:</span><span class="sxs-lookup"><span data-stu-id="c48e7-110">hello application project consists of:</span></span>

* <span data-ttu-id="c48e7-111">Sada služeb toohello odkazy, které tvoří vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c48e7-111">A set of references toohello services that make up your application.</span></span>
* <span data-ttu-id="c48e7-112">Tři publikační profily (1uzlu místní, 5uzlu místní a cloudové), které můžete použít toomaintain předvolby pro práci s různých prostředích – například koncový bod clusteru související tooa předvolby a jestli tooperform upgradujte nasazení ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c48e7-112">Three publish profiles (1-Node Local, 5-Node Local, and Cloud) that you can use toomaintain preferences for working with different environments--such as preferences related tooa cluster endpoint and whether tooperform upgrade deployments by default.</span></span>
* <span data-ttu-id="c48e7-113">Tři soubory parametrů aplikace (stejné jako výše), které můžete použít konfigurace toomaintain konkrétní prostředí aplikace, například hello počet oddílů toocreate pro službu.</span><span class="sxs-lookup"><span data-stu-id="c48e7-113">Three application parameter files (same as above) that you can use toomaintain environment-specific application configurations, such as hello number of partitions toocreate for a service.</span></span>
* <span data-ttu-id="c48e7-114">Skript nasazení, které můžete použít toodeploy aplikace z příkazového řádku hello nebo jako součást automatizované průběžnou integraci a nasazení kanálu.</span><span class="sxs-lookup"><span data-stu-id="c48e7-114">A deployment script that you can use toodeploy your application from hello command line or as part of an automated continuous integration and deployment pipeline.</span></span>
* <span data-ttu-id="c48e7-115">manifest aplikace Hello, která popisuje aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c48e7-115">hello application manifest, which describes hello application.</span></span> <span data-ttu-id="c48e7-116">Hello manifest najdete ve složce ApplicationPackageRoot hello.</span><span class="sxs-lookup"><span data-stu-id="c48e7-116">You can find hello manifest under hello ApplicationPackageRoot folder.</span></span>

### <a name="stateless-service"></a><span data-ttu-id="c48e7-117">Bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="c48e7-117">Stateless service</span></span>
<span data-ttu-id="c48e7-118">Při přidání nové bezstavové služby Visual Studio. přidá řešení tooyour projekt služby, který zahrnuje typ následníky `StatelessService`.</span><span class="sxs-lookup"><span data-stu-id="c48e7-118">When you add a new stateless service, Visual Studio adds a service project tooyour solution that includes a type descended from `StatelessService`.</span></span> <span data-ttu-id="c48e7-119">Služba Hello zvýší místní proměnné v čítače.</span><span class="sxs-lookup"><span data-stu-id="c48e7-119">hello service increments a local variable in a counter.</span></span>

### <a name="stateful-service"></a><span data-ttu-id="c48e7-120">Stavové služby</span><span class="sxs-lookup"><span data-stu-id="c48e7-120">Stateful service</span></span>
<span data-ttu-id="c48e7-121">Když přidáte novou stavové služby, Visual Studio přidá řešení tooyour projekt služby, který zahrnuje typ následníky `StatefulService`.</span><span class="sxs-lookup"><span data-stu-id="c48e7-121">When you add a new stateful service, Visual Studio adds a service project tooyour solution that includes a type descended from `StatefulService`.</span></span> <span data-ttu-id="c48e7-122">Hello služby zvýší čítače v jeho `RunAsync` metoda a úložišť hello vést `ReliableDictionary`.</span><span class="sxs-lookup"><span data-stu-id="c48e7-122">hello service increments a counter in its `RunAsync` method and stores hello result in a `ReliableDictionary`.</span></span>

### <a name="actor-service"></a><span data-ttu-id="c48e7-123">Služby objektu actor</span><span class="sxs-lookup"><span data-stu-id="c48e7-123">Actor service</span></span>
<span data-ttu-id="c48e7-124">Při přidání nového objektu actor spolehlivé, Visual Studio přidá dva projekty tooyour řešení: objektu actor projekt a projekt rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c48e7-124">When you add a new reliable actor, Visual Studio adds two projects tooyour solution: an actor project and an interface project.</span></span>

<span data-ttu-id="c48e7-125">poskytuje metody pro nastavení Hello objektu actor projektu a získávání hello hodnotu počítadla, která je spolehlivě uloženy v rámci stavu objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="c48e7-125">hello actor project provides methods for setting and getting hello value of a counter that is reliably persisted within hello actor's state.</span></span> <span data-ttu-id="c48e7-126">Hello rozhraní projektu poskytuje rozhraní, aby další služby můžete použít objektu actor tooinvoke hello.</span><span class="sxs-lookup"><span data-stu-id="c48e7-126">hello interface project provides an interface that other services can use tooinvoke hello actor.</span></span>

### <a name="stateless-web-api"></a><span data-ttu-id="c48e7-127">Bezstavové webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c48e7-127">Stateless Web API</span></span>
<span data-ttu-id="c48e7-128">Hello bezstavové projekt webového rozhraní API poskytuje základní webové služby, které můžete použít tooopen vaši klienti tooexternal aplikace.</span><span class="sxs-lookup"><span data-stu-id="c48e7-128">hello stateless Web API project provides a basic web service that you can use tooopen your application tooexternal clients.</span></span> <span data-ttu-id="c48e7-129">Další informace o tom, jak hello projektu strukturovaná, najdete v části [Service Fabric webového rozhraní API služby s vlastním hostování OWIN](service-fabric-reliable-services-communication-webapi.md).</span><span class="sxs-lookup"><span data-stu-id="c48e7-129">For more information about how hello project structured, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md).</span></span>


### <a name="aspnet-core"></a><span data-ttu-id="c48e7-130">Jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c48e7-130">ASP.NET core</span></span>
<span data-ttu-id="c48e7-131">Hello Service Fabric SDK poskytuje stejné sady ASP.NET Core šablon, které jsou k dispozici pro samostatnou projektů ASP.NET Core hello: prázdná, [webového rozhraní API][aspnet-webapi], a [webové aplikace][aspnet-webapp].</span><span class="sxs-lookup"><span data-stu-id="c48e7-131">hello Service Fabric SDK provides hello same set of ASP.NET Core templates that are available for standalone ASP.NET Core projects: empty, [Web API][aspnet-webapi], and [Web Application][aspnet-webapp].</span></span>

### <a name="guest-executables-and-guest-containers"></a><span data-ttu-id="c48e7-132">Spustitelné soubory a hostů kontejnery hosta</span><span class="sxs-lookup"><span data-stu-id="c48e7-132">Guest executables and guest containers</span></span>

<span data-ttu-id="c48e7-133">Service Fabric, guest, je služba, která není vytvořené s nástroji programovací modely hello platformy.</span><span class="sxs-lookup"><span data-stu-id="c48e7-133">A Service Fabric 'guest' is a service that is not built with hello platform's programming models.</span></span> <span data-ttu-id="c48e7-134">Binární soubory hello můžete balíček pro hosta buď [přímo v balíčku aplikace hello](service-fabric-deploy-existing-app.md) nebo [prostřednictvím bitovou kopii kontejneru](service-fabric-deploy-container.md).</span><span class="sxs-lookup"><span data-stu-id="c48e7-134">You can package hello binaries for a guest either [directly in hello application package](service-fabric-deploy-existing-app.md) or [through a container image](service-fabric-deploy-container.md).</span></span> <span data-ttu-id="c48e7-135">V obou případech Visual Studio vytvoří hello artefakty potřebné v hello **ApplicationPackageRoot** složku projekt aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c48e7-135">In both cases, Visual Studio creates hello necessary artifacts in hello **ApplicationPackageRoot** folder of hello application project.</span></span> <span data-ttu-id="c48e7-136">Visual Studio nebude vytvořit nový projekt služby, protože hello kód již existuje jinde.</span><span class="sxs-lookup"><span data-stu-id="c48e7-136">Visual Studio will not create a new service project because hello code already exists elsewhere.</span></span> <span data-ttu-id="c48e7-137">Pokud chcete toomanage vaše hosta projekty spolu s hello projekt aplikace Service Fabric, můžete je přidat toohello stejné řešení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c48e7-137">If you would like toomanage your guest projects alongside hello Service Fabric application project, you can add them toohello same Visual Studio solution.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c48e7-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c48e7-138">Next steps</span></span>
### <a name="create-an-azure-cluster"></a><span data-ttu-id="c48e7-139">Vytvoření clusteru služby Azure</span><span class="sxs-lookup"><span data-stu-id="c48e7-139">Create an Azure cluster</span></span>
<span data-ttu-id="c48e7-140">Hello Service Fabric SDK poskytuje místní cluster pro vývoj a testování.</span><span class="sxs-lookup"><span data-stu-id="c48e7-140">hello Service Fabric SDK provides a local cluster for development and testing.</span></span> <span data-ttu-id="c48e7-141">toocreate cluster v Azure, najdete v části [nastavení cluster Service Fabric z portálu Azure hello][create-cluster-in-portal].</span><span class="sxs-lookup"><span data-stu-id="c48e7-141">toocreate a cluster in Azure, see [Setting up a Service Fabric cluster from hello Azure portal][create-cluster-in-portal].</span></span>

### <a name="publish-your-application-tooazure"></a><span data-ttu-id="c48e7-142">Publikování tooAzure vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="c48e7-142">Publish your application tooAzure</span></span>
<span data-ttu-id="c48e7-143">Můžete publikovat aplikaci přímo v sadě Visual Studio tooan clusteru Azure.</span><span class="sxs-lookup"><span data-stu-id="c48e7-143">You can publish your application directly from Visual Studio tooan Azure cluster.</span></span> <span data-ttu-id="c48e7-144">Postup naleznete v tématu toolearn [publikování vaší aplikace tooAzure][publish-app-to-azure].</span><span class="sxs-lookup"><span data-stu-id="c48e7-144">toolearn how, see [Publishing your application tooAzure][publish-app-to-azure].</span></span>

### <a name="use-service-fabric-explorer-toovisualize-your-cluster"></a><span data-ttu-id="c48e7-145">Použijte váš cluster Service Fabric Explorer toovisualize</span><span class="sxs-lookup"><span data-stu-id="c48e7-145">Use Service Fabric Explorer toovisualize your cluster</span></span>
<span data-ttu-id="c48e7-146">Service Fabric Explorer nabízí snadný způsob toovisualize cluster, včetně nasazené aplikace a fyzické rozložení.</span><span class="sxs-lookup"><span data-stu-id="c48e7-146">Service Fabric Explorer offers an easy way toovisualize your cluster, including deployed applications and physical layout.</span></span> <span data-ttu-id="c48e7-147">Další, najdete v části toolearn [vizualizace vašeho clusteru pomocí Service Fabric Explorer][visualize-with-sfx].</span><span class="sxs-lookup"><span data-stu-id="c48e7-147">toolearn more, see [Visualizing your cluster by using Service Fabric Explorer][visualize-with-sfx].</span></span>

### <a name="version-and-upgrade-your-services"></a><span data-ttu-id="c48e7-148">Verze a upgradujte vašim službám</span><span class="sxs-lookup"><span data-stu-id="c48e7-148">Version and upgrade your services</span></span>
<span data-ttu-id="c48e7-149">Service Fabric umožňuje nezávislé Správa verzí a upgrade nezávislých služeb v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c48e7-149">Service Fabric enables independent versioning and upgrading of independent services in an application.</span></span> <span data-ttu-id="c48e7-150">Další, najdete v části toolearn [Správa verzí a upgrade vašich služeb][app-upgrade-tutorial].</span><span class="sxs-lookup"><span data-stu-id="c48e7-150">toolearn more, see [Versioning and upgrading your services][app-upgrade-tutorial].</span></span>

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a><span data-ttu-id="c48e7-151">Konfigurace průběžnou integraci s Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="c48e7-151">Configure continuous integration with Visual Studio Team Services</span></span>
<span data-ttu-id="c48e7-152">toolearn způsobu průběžnou integraci procesu můžete nastavit pro vaši aplikaci Service Fabric najdete v [nakonfigurovat průběžnou integraci s Visual Studio Team Services][ci-with-vso].</span><span class="sxs-lookup"><span data-stu-id="c48e7-152">toolearn how you can set up a continuous integration process for your Service Fabric application, see [Configure continuous integration with Visual Studio Team Services][ci-with-vso].</span></span>

<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
