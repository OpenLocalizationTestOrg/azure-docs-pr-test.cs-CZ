---
title: "Vytvoření vývojového prostředí pro mikroslužby Azure | Dokumentace Microsoftu"
description: "Nainstalujte modul runtime, sadu SDK a nástroje a vytvořte místní vývojový cluster. Po dokončení této instalace a nastavení budete moci sestavovat aplikace."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/10/2017
ms.author: ryanwi, mikhegn
ms.openlocfilehash: f0c6957217c21bdfd76498944e248fc808f2d271
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="prepare-your-development-environment"></a><span data-ttu-id="74a0a-104">Příprava vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="74a0a-104">Prepare your development environment</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="74a0a-105">Windows</span><span class="sxs-lookup"><span data-stu-id="74a0a-105">Windows</span></span>](service-fabric-get-started.md) 
> * [<span data-ttu-id="74a0a-106">Linux</span><span class="sxs-lookup"><span data-stu-id="74a0a-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="74a0a-107">OSX</span><span class="sxs-lookup"><span data-stu-id="74a0a-107">OSX</span></span>](service-fabric-get-started-mac.md)
> 
> 

 <span data-ttu-id="74a0a-108">Pokud chcete sestavovat a spouštět [aplikace Azure Service Fabric][1] na vývojovém počítači, musíte nainstalovat modul runtime, sadu SDK a nástroje.</span><span class="sxs-lookup"><span data-stu-id="74a0a-108">To build and run [Azure Service Fabric applications][1] on your development machine, install the runtime, SDK, and tools.</span></span> <span data-ttu-id="74a0a-109">Musíte taky povolit spouštění skriptů prostředí Windows PowerShell, které jsou součástí sady SDK.</span><span class="sxs-lookup"><span data-stu-id="74a0a-109">You also need to enable execution of the Windows PowerShell scripts included in the SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74a0a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="74a0a-110">Prerequisites</span></span>
### <a name="supported-operating-system-versions"></a><span data-ttu-id="74a0a-111">Podporované verze operačních systémů</span><span class="sxs-lookup"><span data-stu-id="74a0a-111">Supported operating system versions</span></span>
<span data-ttu-id="74a0a-112">Pro vývoj jsou podporovány tyto verze operačních systémů:</span><span class="sxs-lookup"><span data-stu-id="74a0a-112">The following operating system versions are supported for development:</span></span>

* <span data-ttu-id="74a0a-113">Windows 7</span><span class="sxs-lookup"><span data-stu-id="74a0a-113">Windows 7</span></span>
* <span data-ttu-id="74a0a-114">Windows 8 / Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="74a0a-114">Windows 8/Windows 8.1</span></span>
* <span data-ttu-id="74a0a-115">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="74a0a-115">Windows Server 2012 R2</span></span>
* <span data-ttu-id="74a0a-116">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="74a0a-116">Windows Server 2016</span></span>
* <span data-ttu-id="74a0a-117">Windows 10</span><span class="sxs-lookup"><span data-stu-id="74a0a-117">Windows 10</span></span>

> [!NOTE]
> <span data-ttu-id="74a0a-118">Windows 7 ve výchozím nastavení obsahuje jenom prostředí Windows PowerShell 2.0</span><span class="sxs-lookup"><span data-stu-id="74a0a-118">Windows 7 only includes Windows PowerShell 2.0 by default.</span></span> <span data-ttu-id="74a0a-119">Rutiny prostředí PowerShell pro Service Fabric vyžadují PowerShell 3.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="74a0a-119">Service Fabric PowerShell cmdlets requires PowerShell 3.0 or higher.</span></span> <span data-ttu-id="74a0a-120">Můžete si [stáhnout prostředí Windows PowerShell 5.0][powershell5-download] z webu Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="74a0a-120">You can [download Windows PowerShell 5.0][powershell5-download] from the Microsoft Download Center.</span></span>
> 
> 

## <a name="install-the-sdk-and-tools"></a><span data-ttu-id="74a0a-121">Instalace sady SDK a nástrojů</span><span class="sxs-lookup"><span data-stu-id="74a0a-121">Install the SDK and tools</span></span>
### <a name="to-use-visual-studio-2017"></a><span data-ttu-id="74a0a-122">Použití sady Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="74a0a-122">To use Visual Studio 2017</span></span>
<span data-ttu-id="74a0a-123">Nástroje Service Fabric jsou součástí úlohy Azure Development and Management v sadě Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="74a0a-123">Service Fabric Tools are part of the Azure Development and Management workload in Visual Studio 2017.</span></span> <span data-ttu-id="74a0a-124">Povolte tuto úlohu jako součást instalace sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74a0a-124">Enable this workload as part of your Visual Studio installation.</span></span>
<span data-ttu-id="74a0a-125">Kromě toho budete muset sadu Microsoft Azure Service Fabric SDK nainstalovat pomocí instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="74a0a-125">In addition, you need to install the Microsoft Azure Service Fabric SDK, using Web Platform Installer.</span></span>

* <span data-ttu-id="74a0a-126">[Instalace sady Microsoft Azure Service Fabric SDK][core-sdk]</span><span class="sxs-lookup"><span data-stu-id="74a0a-126">[Install the Microsoft Azure Service Fabric SDK][core-sdk]</span></span>

### <a name="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a><span data-ttu-id="74a0a-127">Použití sady Visual Studio 2015 (vyžaduje Visual Studio 2015 Update 2 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="74a0a-127">To use Visual Studio 2015 (requires Visual Studio 2015 Update 2 or later)</span></span>
<span data-ttu-id="74a0a-128">Pro sadu Visual Studio 2015 jsou nainstalované nástroje Service Fabric společně se sadou SDK, pomocí Instalace webové platformy:</span><span class="sxs-lookup"><span data-stu-id="74a0a-128">For Visual Studio 2015, Service Fabric tools are installed together with the SDK, using the Web Platform Installer:</span></span>

* <span data-ttu-id="74a0a-129">[Instalace sady Microsoft Azure Service Fabric SDK a nástrojů][full-bundle-vs2015]</span><span class="sxs-lookup"><span data-stu-id="74a0a-129">[Install the Microsoft Azure Service Fabric SDK and Tools][full-bundle-vs2015]</span></span>

### <a name="sdk-installation-only"></a><span data-ttu-id="74a0a-130">Jenom instalace sady SDK</span><span class="sxs-lookup"><span data-stu-id="74a0a-130">SDK installation only</span></span>
<span data-ttu-id="74a0a-131">Pokud potřebujete jenom sadu SDK, můžete nainstalovat tento balíček:</span><span class="sxs-lookup"><span data-stu-id="74a0a-131">If you only need the SDK, you can install this package:</span></span>
* <span data-ttu-id="74a0a-132">[Instalace sady Microsoft Azure Service Fabric SDK][core-sdk]</span><span class="sxs-lookup"><span data-stu-id="74a0a-132">[Install the Microsoft Azure Service Fabric SDK][core-sdk]</span></span>

<span data-ttu-id="74a0a-133">Aktuální verze jsou:</span><span class="sxs-lookup"><span data-stu-id="74a0a-133">The current versions are:</span></span>
* <span data-ttu-id="74a0a-134">Sada Service Fabric SDK 2.7.198</span><span class="sxs-lookup"><span data-stu-id="74a0a-134">Service Fabric SDK 2.7.198</span></span>
* <span data-ttu-id="74a0a-135">Modul runtime Service Fabric 5.7.198</span><span class="sxs-lookup"><span data-stu-id="74a0a-135">Service Fabric runtime 5.7.198</span></span>
* <span data-ttu-id="74a0a-136">Service Fabric Tools for Visual Studio 2015 1.7.50721</span><span class="sxs-lookup"><span data-stu-id="74a0a-136">Service Fabric Tools for Visual Studio 2015 1.7.50721</span></span>
* <span data-ttu-id="74a0a-137">Visual Studio 2017 s aktualizací Update 2 obsahuje Service Fabric Tools for Visual Studio 1.6.20170504</span><span class="sxs-lookup"><span data-stu-id="74a0a-137">Visual Studio 2017 Update 2 includes Service Fabric Tools for Visual Studio 1.6.20170504</span></span>
* <span data-ttu-id="74a0a-138">Visual Studio 2017 s aktualizací Update 3 ve verzi Preview 7 (15.3.0 Preview 7.0) obsahuje Service Fabric Tools for Visual Studio 1.7.20170721</span><span class="sxs-lookup"><span data-stu-id="74a0a-138">Visual Studio 2017 Update 3 Preview 7 (15.3.0 Preview 7.0) includes Service Fabric Tools for Visual Studio 1.7.20170721</span></span>

<span data-ttu-id="74a0a-139">Seznam podporovaných verzí najdete v tématu [Podpora pro Service Fabric](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="74a0a-139">For a list of supported versions, see [Service Fabric support](service-fabric-support.md)</span></span>

## <a name="enable-powershell-script-execution"></a><span data-ttu-id="74a0a-140">Povolení spouštění skriptů prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="74a0a-140">Enable PowerShell script execution</span></span>
<span data-ttu-id="74a0a-141">Platforma Service Fabric používá skripty prostředí Windows PowerShell k vytvoření místního vývojového clusteru a k nasazení aplikací ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74a0a-141">Service Fabric uses Windows PowerShell scripts for creating a local development cluster and for deploying applications from Visual Studio.</span></span> <span data-ttu-id="74a0a-142">Systém Windows ve výchozím nastavení spouštění těchto skriptů blokuje.</span><span class="sxs-lookup"><span data-stu-id="74a0a-142">By default, Windows blocks these scripts from running.</span></span> <span data-ttu-id="74a0a-143">Pokud je chcete povolit, musíte upravit zásady spouštění prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="74a0a-143">To enable them, you must modify your PowerShell execution policy.</span></span> <span data-ttu-id="74a0a-144">Otevřete prostředí PowerShell jako správce a zadejte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="74a0a-144">Open PowerShell as an administrator and enter the following command:</span></span>

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a><span data-ttu-id="74a0a-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74a0a-145">Next steps</span></span>
<span data-ttu-id="74a0a-146">Teď, když jste dokončili nastavení vývojového prostředí, můžete začít sestavovat a spouštět aplikace.</span><span class="sxs-lookup"><span data-stu-id="74a0a-146">Now that you've finished setting up your development environment, start building and running apps.</span></span>

* [<span data-ttu-id="74a0a-147">Vytvořte první aplikaci Service Fabric v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="74a0a-147">Create your first Service Fabric application in Visual Studio</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
* [<span data-ttu-id="74a0a-148">Naučte se nasadit a spravovat aplikace v místním clusteru</span><span class="sxs-lookup"><span data-stu-id="74a0a-148">Learn how to deploy and manage applications on your local cluster</span></span>](service-fabric-get-started-with-a-local-cluster.md)
* [<span data-ttu-id="74a0a-149">Seznamte se s programovacími modely: Reliable Services a Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="74a0a-149">Learn about the programming models: Reliable Services and Reliable Actors</span></span>](service-fabric-choose-framework.md)
* [<span data-ttu-id="74a0a-150">Prohlédněte si ukázky kódu Service Fabric na GitHubu</span><span class="sxs-lookup"><span data-stu-id="74a0a-150">Check out the Service Fabric code samples on GitHub</span></span>](https://aka.ms/servicefabricsamples)
* [<span data-ttu-id="74a0a-151">Vizualizujte cluster pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="74a0a-151">Visualize your cluster by using Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)
* [<span data-ttu-id="74a0a-152">Postupujte podle studijního plánu Service Fabric a získejte obecný úvod k platformě</span><span class="sxs-lookup"><span data-stu-id="74a0a-152">Follow the Service Fabric learning path to get a broad introduction to the platform</span></span>](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* <span data-ttu-id="74a0a-153">Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="74a0a-153">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>

<span data-ttu-id="74a0a-154">[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Stránka kampaně Service Fabric"</span><span class="sxs-lookup"><span data-stu-id="74a0a-154">[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric campaign page"</span></span>
<span data-ttu-id="74a0a-155">[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"</span><span class="sxs-lookup"><span data-stu-id="74a0a-155">[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"</span></span>
<span data-ttu-id="74a0a-156">[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Odkaz na VS 2015 WebPI"</span><span class="sxs-lookup"><span data-stu-id="74a0a-156">[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI link"</span></span>
<span data-ttu-id="74a0a-157">[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Odkaz na Dev15 WebPI"</span><span class="sxs-lookup"><span data-stu-id="74a0a-157">[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI link"</span></span>
<span data-ttu-id="74a0a-158">[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Odkaz na Core SDK WebPI"</span><span class="sxs-lookup"><span data-stu-id="74a0a-158">[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI link"</span></span>
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
