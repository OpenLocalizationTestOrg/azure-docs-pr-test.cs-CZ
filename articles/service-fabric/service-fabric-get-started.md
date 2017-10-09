---
title: "aaaSet vývojového prostředí pro Azure mikroslužeb | Microsoft Docs"
description: "Nainstalujte hello runtime, sadu SDK a nástroje a vytvořte místní vývojový cluster. Po dokončení této instalace, bude připravená toobuild aplikace."
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
ms.openlocfilehash: 9b0442778999d4c3d2b99adb98f6596dcbdc36d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-development-environment"></a><span data-ttu-id="2bee4-104">Příprava vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="2bee4-104">Prepare your development environment</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2bee4-105">Windows</span><span class="sxs-lookup"><span data-stu-id="2bee4-105">Windows</span></span>](service-fabric-get-started.md) 
> * [<span data-ttu-id="2bee4-106">Linux</span><span class="sxs-lookup"><span data-stu-id="2bee4-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="2bee4-107">OSX</span><span class="sxs-lookup"><span data-stu-id="2bee4-107">OSX</span></span>](service-fabric-get-started-mac.md)
> 
> 

 <span data-ttu-id="2bee4-108">toobuild a spusťte [aplikace Azure Service Fabric] [ 1] na vývojovém počítači, nainstalujte hello runtime, sadu SDK a nástroje.</span><span class="sxs-lookup"><span data-stu-id="2bee4-108">toobuild and run [Azure Service Fabric applications][1] on your development machine, install hello runtime, SDK, and tools.</span></span> <span data-ttu-id="2bee4-109">Budete také potřebovat tooenable spouštění skriptů prostředí Windows PowerShell hello součástí hello SDK.</span><span class="sxs-lookup"><span data-stu-id="2bee4-109">You also need tooenable execution of hello Windows PowerShell scripts included in hello SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2bee4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2bee4-110">Prerequisites</span></span>
### <a name="supported-operating-system-versions"></a><span data-ttu-id="2bee4-111">Podporované verze operačních systémů</span><span class="sxs-lookup"><span data-stu-id="2bee4-111">Supported operating system versions</span></span>
<span data-ttu-id="2bee4-112">pro vývoj jsou podporovány následující verze operačního systému Hello:</span><span class="sxs-lookup"><span data-stu-id="2bee4-112">hello following operating system versions are supported for development:</span></span>

* <span data-ttu-id="2bee4-113">Windows 7</span><span class="sxs-lookup"><span data-stu-id="2bee4-113">Windows 7</span></span>
* <span data-ttu-id="2bee4-114">Windows 8 / Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="2bee4-114">Windows 8/Windows 8.1</span></span>
* <span data-ttu-id="2bee4-115">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="2bee4-115">Windows Server 2012 R2</span></span>
* <span data-ttu-id="2bee4-116">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="2bee4-116">Windows Server 2016</span></span>
* <span data-ttu-id="2bee4-117">Windows 10</span><span class="sxs-lookup"><span data-stu-id="2bee4-117">Windows 10</span></span>

> [!NOTE]
> <span data-ttu-id="2bee4-118">Windows 7 ve výchozím nastavení obsahuje jenom prostředí Windows PowerShell 2.0</span><span class="sxs-lookup"><span data-stu-id="2bee4-118">Windows 7 only includes Windows PowerShell 2.0 by default.</span></span> <span data-ttu-id="2bee4-119">Rutiny prostředí PowerShell pro Service Fabric vyžadují PowerShell 3.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2bee4-119">Service Fabric PowerShell cmdlets requires PowerShell 3.0 or higher.</span></span> <span data-ttu-id="2bee4-120">Můžete [stáhnout prostředí Windows PowerShell 5.0] [ powershell5-download] z hello Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="2bee4-120">You can [download Windows PowerShell 5.0][powershell5-download] from hello Microsoft Download Center.</span></span>
> 
> 

## <a name="install-hello-sdk-and-tools"></a><span data-ttu-id="2bee4-121">Instalace hello SDK a nástrojů</span><span class="sxs-lookup"><span data-stu-id="2bee4-121">Install hello SDK and tools</span></span>
### <a name="toouse-visual-studio-2017"></a><span data-ttu-id="2bee4-122">toouse Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2bee4-122">toouse Visual Studio 2017</span></span>
<span data-ttu-id="2bee4-123">Nástroje pro služby prostředků infrastruktury jsou součástí hello Azure vývoj a správu úloh ve Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="2bee4-123">Service Fabric Tools are part of hello Azure Development and Management workload in Visual Studio 2017.</span></span> <span data-ttu-id="2bee4-124">Povolte tuto úlohu jako součást instalace sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2bee4-124">Enable this workload as part of your Visual Studio installation.</span></span>
<span data-ttu-id="2bee4-125">Kromě toho musíte tooinstall hello Microsoft Azure Service Fabric SDK, pomocí instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="2bee4-125">In addition, you need tooinstall hello Microsoft Azure Service Fabric SDK, using Web Platform Installer.</span></span>

* <span data-ttu-id="2bee4-126">[Nainstalujte hello Microsoft Azure Service Fabric SDK][core-sdk]</span><span class="sxs-lookup"><span data-stu-id="2bee4-126">[Install hello Microsoft Azure Service Fabric SDK][core-sdk]</span></span>

### <a name="toouse-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a><span data-ttu-id="2bee4-127">toouse Visual Studio 2015 (vyžaduje Visual Studio 2015 Update 2 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="2bee4-127">toouse Visual Studio 2015 (requires Visual Studio 2015 Update 2 or later)</span></span>
<span data-ttu-id="2bee4-128">Pro Visual Studio 2015 jsou nainstalované nástroje Service Fabric společně s hello SDK, pomocí hello instalačního programu webové platformy:</span><span class="sxs-lookup"><span data-stu-id="2bee4-128">For Visual Studio 2015, Service Fabric tools are installed together with hello SDK, using hello Web Platform Installer:</span></span>

* <span data-ttu-id="2bee4-129">[Instalace hello Microsoft Azure Service Fabric SDK a nástrojů][full-bundle-vs2015]</span><span class="sxs-lookup"><span data-stu-id="2bee4-129">[Install hello Microsoft Azure Service Fabric SDK and Tools][full-bundle-vs2015]</span></span>

### <a name="sdk-installation-only"></a><span data-ttu-id="2bee4-130">Jenom instalace sady SDK</span><span class="sxs-lookup"><span data-stu-id="2bee4-130">SDK installation only</span></span>
<span data-ttu-id="2bee4-131">Pokud potřebujete jenom hello SDK, můžete instalaci tohoto balíčku:</span><span class="sxs-lookup"><span data-stu-id="2bee4-131">If you only need hello SDK, you can install this package:</span></span>
* <span data-ttu-id="2bee4-132">[Nainstalujte hello Microsoft Azure Service Fabric SDK][core-sdk]</span><span class="sxs-lookup"><span data-stu-id="2bee4-132">[Install hello Microsoft Azure Service Fabric SDK][core-sdk]</span></span>

<span data-ttu-id="2bee4-133">Hello aktuální verze jsou:</span><span class="sxs-lookup"><span data-stu-id="2bee4-133">hello current versions are:</span></span>
* <span data-ttu-id="2bee4-134">Sada Service Fabric SDK 2.7.198</span><span class="sxs-lookup"><span data-stu-id="2bee4-134">Service Fabric SDK 2.7.198</span></span>
* <span data-ttu-id="2bee4-135">Modul runtime Service Fabric 5.7.198</span><span class="sxs-lookup"><span data-stu-id="2bee4-135">Service Fabric runtime 5.7.198</span></span>
* <span data-ttu-id="2bee4-136">Service Fabric Tools for Visual Studio 2015 1.7.50721</span><span class="sxs-lookup"><span data-stu-id="2bee4-136">Service Fabric Tools for Visual Studio 2015 1.7.50721</span></span>
* <span data-ttu-id="2bee4-137">Visual Studio 2017 s aktualizací Update 2 obsahuje Service Fabric Tools for Visual Studio 1.6.20170504</span><span class="sxs-lookup"><span data-stu-id="2bee4-137">Visual Studio 2017 Update 2 includes Service Fabric Tools for Visual Studio 1.6.20170504</span></span>
* <span data-ttu-id="2bee4-138">Visual Studio 2017 s aktualizací Update 3 ve verzi Preview 7 (15.3.0 Preview 7.0) obsahuje Service Fabric Tools for Visual Studio 1.7.20170721</span><span class="sxs-lookup"><span data-stu-id="2bee4-138">Visual Studio 2017 Update 3 Preview 7 (15.3.0 Preview 7.0) includes Service Fabric Tools for Visual Studio 1.7.20170721</span></span>

<span data-ttu-id="2bee4-139">Seznam podporovaných verzí najdete v tématu [Podpora pro Service Fabric](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="2bee4-139">For a list of supported versions, see [Service Fabric support](service-fabric-support.md)</span></span>

## <a name="enable-powershell-script-execution"></a><span data-ttu-id="2bee4-140">Povolení spouštění skriptů prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2bee4-140">Enable PowerShell script execution</span></span>
<span data-ttu-id="2bee4-141">Platforma Service Fabric používá skripty prostředí Windows PowerShell k vytvoření místního vývojového clusteru a k nasazení aplikací ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2bee4-141">Service Fabric uses Windows PowerShell scripts for creating a local development cluster and for deploying applications from Visual Studio.</span></span> <span data-ttu-id="2bee4-142">Systém Windows ve výchozím nastavení spouštění těchto skriptů blokuje.</span><span class="sxs-lookup"><span data-stu-id="2bee4-142">By default, Windows blocks these scripts from running.</span></span> <span data-ttu-id="2bee4-143">tooenable je, je třeba upravit zásady spouštění prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2bee4-143">tooenable them, you must modify your PowerShell execution policy.</span></span> <span data-ttu-id="2bee4-144">Otevřete PowerShell jako správce a zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="2bee4-144">Open PowerShell as an administrator and enter hello following command:</span></span>

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a><span data-ttu-id="2bee4-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2bee4-145">Next steps</span></span>
<span data-ttu-id="2bee4-146">Teď, když jste dokončili nastavení vývojového prostředí, můžete začít sestavovat a spouštět aplikace.</span><span class="sxs-lookup"><span data-stu-id="2bee4-146">Now that you've finished setting up your development environment, start building and running apps.</span></span>

* [<span data-ttu-id="2bee4-147">Vytvořte první aplikaci Service Fabric v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2bee4-147">Create your first Service Fabric application in Visual Studio</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
* [<span data-ttu-id="2bee4-148">Zjistěte, jak toodeploy a spravovat aplikace v místním clusteru</span><span class="sxs-lookup"><span data-stu-id="2bee4-148">Learn how toodeploy and manage applications on your local cluster</span></span>](service-fabric-get-started-with-a-local-cluster.md)
* [<span data-ttu-id="2bee4-149">Další informace o hello programovací modely: Reliable Services a Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="2bee4-149">Learn about hello programming models: Reliable Services and Reliable Actors</span></span>](service-fabric-choose-framework.md)
* [<span data-ttu-id="2bee4-150">Podívejte se na hello Service Fabric ukázky kódu na Githubu</span><span class="sxs-lookup"><span data-stu-id="2bee4-150">Check out hello Service Fabric code samples on GitHub</span></span>](https://aka.ms/servicefabricsamples)
* [<span data-ttu-id="2bee4-151">Vizualizujte cluster pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="2bee4-151">Visualize your cluster by using Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)
* [<span data-ttu-id="2bee4-152">Postupujte podle hello Service Fabric učení cesta tooget platformu toohello obecný úvod</span><span class="sxs-lookup"><span data-stu-id="2bee4-152">Follow hello Service Fabric learning path tooget a broad introduction toohello platform</span></span>](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* <span data-ttu-id="2bee4-153">Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="2bee4-153">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Stránka kampaně Service Fabric"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Odkaz na VS 2015 WebPI"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Odkaz na Dev15 WebPI"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Odkaz na Core SDK WebPI"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
