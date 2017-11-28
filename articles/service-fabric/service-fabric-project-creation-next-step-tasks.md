---
title: "Další kroky pro vytvoření projektu Service Fabric | Microsoft Docs"
description: "Tento článek obsahuje odkazy na sadu úloh vývoj jádra pro Service Fabric"
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
ms.openlocfilehash: 74019850c507902d9ef7ec47a364fff234aaf32b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a><span data-ttu-id="494b3-103">Vaše aplikace Service Fabric a další kroky</span><span class="sxs-lookup"><span data-stu-id="494b3-103">Your Service Fabric application and next steps</span></span>
<span data-ttu-id="494b3-104">Vaše aplikace Azure Service Fabric byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="494b3-104">Your Azure Service Fabric application has been created.</span></span> <span data-ttu-id="494b3-105">Tento článek popisuje způsob vytvoření projektu a některé potenciální další kroky.</span><span class="sxs-lookup"><span data-stu-id="494b3-105">This article describes the makeup of your project and some potential next steps.</span></span>

## <a name="your-application"></a><span data-ttu-id="494b3-106">Vaše aplikace</span><span class="sxs-lookup"><span data-stu-id="494b3-106">Your application</span></span>
<span data-ttu-id="494b3-107">Každé nové aplikace zahrnuje projekt aplikace.</span><span class="sxs-lookup"><span data-stu-id="494b3-107">Every new application includes an application project.</span></span> <span data-ttu-id="494b3-108">Může mít jeden nebo dva další projekty, v závislosti na typu služby vybrali.</span><span class="sxs-lookup"><span data-stu-id="494b3-108">There may be one or two additional projects, depending on the type of service chosen.</span></span>

### <a name="the-application-project"></a><span data-ttu-id="494b3-109">Projekt aplikace</span><span class="sxs-lookup"><span data-stu-id="494b3-109">The application project</span></span>
<span data-ttu-id="494b3-110">Projekt aplikace se skládá z:</span><span class="sxs-lookup"><span data-stu-id="494b3-110">The application project consists of:</span></span>

* <span data-ttu-id="494b3-111">Sada odkazů na služby, které tvoří vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="494b3-111">A set of references to the services that make up your application.</span></span>
* <span data-ttu-id="494b3-112">Tři publikační profily (1uzlu místní, 5uzlu místní a cloudové), které můžete použít ke správě předvoleb pro práci s různých prostředích – například předvolby týkající se koncový bod clusteru a zda k provedení upgradu nasazení ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="494b3-112">Three publish profiles (1-Node Local, 5-Node Local, and Cloud) that you can use to maintain preferences for working with different environments--such as preferences related to a cluster endpoint and whether to perform upgrade deployments by default.</span></span>
* <span data-ttu-id="494b3-113">Soubory tři parametr aplikace (stejné jako výše), můžete použít ke správě konfigurace specifické pro prostředí aplikace, například počet oddílů pro vytvoření pro službu.</span><span class="sxs-lookup"><span data-stu-id="494b3-113">Three application parameter files (same as above) that you can use to maintain environment-specific application configurations, such as the number of partitions to create for a service.</span></span>
* <span data-ttu-id="494b3-114">Skript nasazení, který můžete použít k nasazení aplikace z příkazového řádku nebo jako součást automatizované průběžnou integraci a nasazení kanálu.</span><span class="sxs-lookup"><span data-stu-id="494b3-114">A deployment script that you can use to deploy your application from the command line or as part of an automated continuous integration and deployment pipeline.</span></span>
* <span data-ttu-id="494b3-115">Manifest aplikace, která popisuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="494b3-115">The application manifest, which describes the application.</span></span> <span data-ttu-id="494b3-116">Ve složce ApplicationPackageRoot můžete najít v manifestu.</span><span class="sxs-lookup"><span data-stu-id="494b3-116">You can find the manifest under the ApplicationPackageRoot folder.</span></span>

### <a name="stateless-service"></a><span data-ttu-id="494b3-117">Bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="494b3-117">Stateless service</span></span>
<span data-ttu-id="494b3-118">Při přidání nové bezstavové služby Visual Studio přidá do vašeho řešení, které zahrnuje typ následníky projektu služby `StatelessService`.</span><span class="sxs-lookup"><span data-stu-id="494b3-118">When you add a new stateless service, Visual Studio adds a service project to your solution that includes a type descended from `StatelessService`.</span></span> <span data-ttu-id="494b3-119">Služba zvýší místní proměnné v čítače.</span><span class="sxs-lookup"><span data-stu-id="494b3-119">The service increments a local variable in a counter.</span></span>

### <a name="stateful-service"></a><span data-ttu-id="494b3-120">Stavové služby</span><span class="sxs-lookup"><span data-stu-id="494b3-120">Stateful service</span></span>
<span data-ttu-id="494b3-121">Když přidáte novou stavové služby, Visual Studio přidá do vašeho řešení, které zahrnuje typ následníky projektu služby `StatefulService`.</span><span class="sxs-lookup"><span data-stu-id="494b3-121">When you add a new stateful service, Visual Studio adds a service project to your solution that includes a type descended from `StatefulService`.</span></span> <span data-ttu-id="494b3-122">Služba zvýší čítače v jeho `RunAsync` metoda a ukládá výsledky v `ReliableDictionary`.</span><span class="sxs-lookup"><span data-stu-id="494b3-122">The service increments a counter in its `RunAsync` method and stores the result in a `ReliableDictionary`.</span></span>

### <a name="actor-service"></a><span data-ttu-id="494b3-123">Služby objektu actor</span><span class="sxs-lookup"><span data-stu-id="494b3-123">Actor service</span></span>
<span data-ttu-id="494b3-124">Při přidání nového objektu actor spolehlivé, Visual Studio přidá do vašeho řešení dva projekty: objektu actor projekt a projekt rozhraní.</span><span class="sxs-lookup"><span data-stu-id="494b3-124">When you add a new reliable actor, Visual Studio adds two projects to your solution: an actor project and an interface project.</span></span>

<span data-ttu-id="494b3-125">Poskytuje metody pro nastavení projektu objektu actor a získávání hodnotu počítadla, která je spolehlivě uloženy v rámci objektu actor stavu.</span><span class="sxs-lookup"><span data-stu-id="494b3-125">The actor project provides methods for setting and getting the value of a counter that is reliably persisted within the actor's state.</span></span> <span data-ttu-id="494b3-126">Rozhraní projektu poskytuje rozhraní, které má být vyvolán objektu actor můžete použít jiné služby.</span><span class="sxs-lookup"><span data-stu-id="494b3-126">The interface project provides an interface that other services can use to invoke the actor.</span></span>

### <a name="stateless-web-api"></a><span data-ttu-id="494b3-127">Bezstavové webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="494b3-127">Stateless Web API</span></span>
<span data-ttu-id="494b3-128">Bezstavové projekt webového rozhraní API poskytuje základní webová služba, která slouží k otevření aplikace externím klientům.</span><span class="sxs-lookup"><span data-stu-id="494b3-128">The stateless Web API project provides a basic web service that you can use to open your application to external clients.</span></span> <span data-ttu-id="494b3-129">Další informace o projektu strukturovaná, najdete v části [Service Fabric webového rozhraní API služby s vlastním hostování OWIN](service-fabric-reliable-services-communication-webapi.md).</span><span class="sxs-lookup"><span data-stu-id="494b3-129">For more information about how the project structured, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md).</span></span>


### <a name="aspnet-core"></a><span data-ttu-id="494b3-130">Jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="494b3-130">ASP.NET core</span></span>
<span data-ttu-id="494b3-131">Sada Service Fabric SDK obsahuje stejnou sadu ASP.NET Core šablony, které jsou k dispozici pro samostatné projektů ASP.NET Core: prázdná, [webového rozhraní API][aspnet-webapi], a [webové aplikace][aspnet-webapp].</span><span class="sxs-lookup"><span data-stu-id="494b3-131">The Service Fabric SDK provides the same set of ASP.NET Core templates that are available for standalone ASP.NET Core projects: empty, [Web API][aspnet-webapi], and [Web Application][aspnet-webapp].</span></span>

### <a name="guest-executables-and-guest-containers"></a><span data-ttu-id="494b3-132">Spustitelné soubory a hostů kontejnery hosta</span><span class="sxs-lookup"><span data-stu-id="494b3-132">Guest executables and guest containers</span></span>

<span data-ttu-id="494b3-133">Service Fabric, guest, je služba, která není vytvořené s nástroji programovací modely platformu.</span><span class="sxs-lookup"><span data-stu-id="494b3-133">A Service Fabric 'guest' is a service that is not built with the platform's programming models.</span></span> <span data-ttu-id="494b3-134">Binární soubory pro Host můžete balíček buď [přímo v balíčku aplikace](service-fabric-deploy-existing-app.md) nebo [prostřednictvím bitovou kopii kontejneru](service-fabric-deploy-container.md).</span><span class="sxs-lookup"><span data-stu-id="494b3-134">You can package the binaries for a guest either [directly in the application package](service-fabric-deploy-existing-app.md) or [through a container image](service-fabric-deploy-container.md).</span></span> <span data-ttu-id="494b3-135">V obou případech Visual Studio vytvoří artefakty potřebné v **ApplicationPackageRoot** složku projekt aplikace.</span><span class="sxs-lookup"><span data-stu-id="494b3-135">In both cases, Visual Studio creates the necessary artifacts in the **ApplicationPackageRoot** folder of the application project.</span></span> <span data-ttu-id="494b3-136">Visual Studio nebude vytvořit nový projekt služby, protože kód již existuje jinde.</span><span class="sxs-lookup"><span data-stu-id="494b3-136">Visual Studio will not create a new service project because the code already exists elsewhere.</span></span> <span data-ttu-id="494b3-137">Pokud chcete ke správě vašich projektů hosta spolu s projekt aplikace Service Fabric, můžete je přidat do stejného řešení, Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="494b3-137">If you would like to manage your guest projects alongside the Service Fabric application project, you can add them to the same Visual Studio solution.</span></span>

## <a name="next-steps"></a><span data-ttu-id="494b3-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="494b3-138">Next steps</span></span>
### <a name="create-an-azure-cluster"></a><span data-ttu-id="494b3-139">Vytvoření clusteru služby Azure</span><span class="sxs-lookup"><span data-stu-id="494b3-139">Create an Azure cluster</span></span>
<span data-ttu-id="494b3-140">Sada Service Fabric SDK poskytuje místní cluster pro vývoj a testování.</span><span class="sxs-lookup"><span data-stu-id="494b3-140">The Service Fabric SDK provides a local cluster for development and testing.</span></span> <span data-ttu-id="494b3-141">Pokud chcete vytvořit cluster v Azure, najdete v části [nastavení cluster Service Fabric na portálu Azure][create-cluster-in-portal].</span><span class="sxs-lookup"><span data-stu-id="494b3-141">To create a cluster in Azure, see [Setting up a Service Fabric cluster from the Azure portal][create-cluster-in-portal].</span></span>

### <a name="publish-your-application-to-azure"></a><span data-ttu-id="494b3-142">Publikování aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="494b3-142">Publish your application to Azure</span></span>
<span data-ttu-id="494b3-143">Můžete publikovat svoji aplikaci přímo ze sady Visual Studio do clusteru služby Azure.</span><span class="sxs-lookup"><span data-stu-id="494b3-143">You can publish your application directly from Visual Studio to an Azure cluster.</span></span> <span data-ttu-id="494b3-144">Další informace, jak zjistit, [publikování aplikace do Azure][publish-app-to-azure].</span><span class="sxs-lookup"><span data-stu-id="494b3-144">To learn how, see [Publishing your application to Azure][publish-app-to-azure].</span></span>

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a><span data-ttu-id="494b3-145">Vizualizujte cluster pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="494b3-145">Use Service Fabric Explorer to visualize your cluster</span></span>
<span data-ttu-id="494b3-146">Service Fabric Explorer nabízí snadný způsob, jak vizualizace clusteru, včetně nasazené aplikace a fyzické rozložení.</span><span class="sxs-lookup"><span data-stu-id="494b3-146">Service Fabric Explorer offers an easy way to visualize your cluster, including deployed applications and physical layout.</span></span> <span data-ttu-id="494b3-147">Další informace najdete v tématu [vizualizace vašeho clusteru pomocí Service Fabric Explorer][visualize-with-sfx].</span><span class="sxs-lookup"><span data-stu-id="494b3-147">To learn more, see [Visualizing your cluster by using Service Fabric Explorer][visualize-with-sfx].</span></span>

### <a name="version-and-upgrade-your-services"></a><span data-ttu-id="494b3-148">Verze a upgradujte vašim službám</span><span class="sxs-lookup"><span data-stu-id="494b3-148">Version and upgrade your services</span></span>
<span data-ttu-id="494b3-149">Service Fabric umožňuje nezávislé Správa verzí a upgrade nezávislých služeb v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="494b3-149">Service Fabric enables independent versioning and upgrading of independent services in an application.</span></span> <span data-ttu-id="494b3-150">Další informace najdete v tématu [Správa verzí a upgrade vašich služeb][app-upgrade-tutorial].</span><span class="sxs-lookup"><span data-stu-id="494b3-150">To learn more, see [Versioning and upgrading your services][app-upgrade-tutorial].</span></span>

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a><span data-ttu-id="494b3-151">Konfigurace průběžnou integraci s Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="494b3-151">Configure continuous integration with Visual Studio Team Services</span></span>
<span data-ttu-id="494b3-152">Informace o tom, jak můžete nastavit průběžnou integraci proces pro vaši aplikaci Service Fabric, najdete v části [nakonfigurovat průběžnou integraci s Visual Studio Team Services][ci-with-vso].</span><span class="sxs-lookup"><span data-stu-id="494b3-152">To learn how you can set up a continuous integration process for your Service Fabric application, see [Configure continuous integration with Visual Studio Team Services][ci-with-vso].</span></span>

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
