---
title: "aaaDebugging aplikací v místním kontejner Docker | Microsoft Docs"
description: "Zjistěte, jak toomodify aplikaci, která běží v místní kontejner Docker, aktualizujte hello kontejneru prostřednictvím úpravy a aktualizace a nastavit zarážky ladění"
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/22/2016
ms.author: mlearned
ms.openlocfilehash: ff64e62fbb93901a29b5496bd5e17d2c4ea5ca99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-apps-in-a-local-docker-container"></a><span data-ttu-id="0f5ab-103">Ladění aplikací v místním kontejneru Dockeru</span><span class="sxs-lookup"><span data-stu-id="0f5ab-103">Debugging apps in a local Docker container</span></span>
## <a name="overview"></a><span data-ttu-id="0f5ab-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="0f5ab-104">Overview</span></span>
<span data-ttu-id="0f5ab-105">Hello Visual Studio Tools for Docker zajišťuje konzistentní způsob toodevelop v a ověřte svoji aplikaci místně v kontejner Linux Docker.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-105">hello Visual Studio Tools for Docker provides a consistent way toodevelop in and validate your application locally in a Linux Docker container.</span></span>
<span data-ttu-id="0f5ab-106">Nemáte kontejner hello toorestart pokaždé, když provedete kód změnit.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-106">You don't have toorestart hello container each time you make a code change.</span></span>
<span data-ttu-id="0f5ab-107">Tento článek ukazuje, jak toostart funkce pro "Upravit a aktualizovat" hello toouse ASP.NET Core webové aplikace v místním kontejner Docker, proveďte potřebné změny a potom aktualizujte prohlížeč toosee hello tyto změny.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-107">This article illustrates how toouse hello "Edit and Refresh" feature toostart an ASP.NET Core Web app in a local Docker container, make any necessary changes, and then refresh hello browser toosee those changes.</span></span>
<span data-ttu-id="0f5ab-108">Tento článek také ukazuje, jak tooset zarážky pro ladění.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-108">This article also shows you how tooset breakpoints for debugging.</span></span>

> [!NOTE]
> <span data-ttu-id="0f5ab-109">Podpora kontejnerů Windows budou přicházet v budoucí verzi</span><span class="sxs-lookup"><span data-stu-id="0f5ab-109">Windows Container support will be coming in a future release</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="0f5ab-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0f5ab-110">Prerequisites</span></span>
<span data-ttu-id="0f5ab-111">musí být nainstalován Hello následující nástroje.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-111">hello following tools must be installed.</span></span>

* [<span data-ttu-id="0f5ab-112">Nejnovější verze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0f5ab-112">Latest version of Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="0f5ab-113">Microsoft ASP.NET Core 1.0 SDK</span><span class="sxs-lookup"><span data-stu-id="0f5ab-113">Microsoft ASP.NET Core 1.0 SDK</span></span>](https://go.microsoft.com/fwlink/?LinkID=809122)

<span data-ttu-id="0f5ab-114">toorun místně Docker kontejnery, budete potřebovat místní docker klienta.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-114">toorun Docker containers locally, you'll need a local docker client.</span></span>
<span data-ttu-id="0f5ab-115">Můžete použít hello [Docker nástrojů](https://www.docker.com/products/docker-toolbox), což vyžaduje, aby zakázáno toobe technologie Hyper-V, nebo můžete použít [Docker pro systém Windows](https://www.docker.com/get-docker), která využívá technologie Hyper-V a vyžaduje Windows 10.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-115">You can use hello [Docker Toolbox](https://www.docker.com/products/docker-toolbox), which requires Hyper-V toobe disabled, or you can use [Docker for Windows](https://www.docker.com/get-docker), which uses Hyper-V, and requires Windows 10.</span></span>

<span data-ttu-id="0f5ab-116">Pokud používáte Docker sada nástrojů, budete potřebovat příliš[konfigurace klienta Docker hello](vs-azure-tools-docker-setup.md)</span><span class="sxs-lookup"><span data-stu-id="0f5ab-116">If using Docker Toolbox, you'll need too[configure hello Docker client](vs-azure-tools-docker-setup.md)</span></span>

## <a name="1-create-a-web-app"></a><span data-ttu-id="0f5ab-117">1. Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="0f5ab-117">1. Create a web app</span></span>
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a><span data-ttu-id="0f5ab-118">2. Přidání podpory Docker</span><span class="sxs-lookup"><span data-stu-id="0f5ab-118">2. Add Docker support</span></span>
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a><span data-ttu-id="0f5ab-119">3. Upravit kód a aktualizace</span><span class="sxs-lookup"><span data-stu-id="0f5ab-119">3. Edit your code and refresh</span></span>
<span data-ttu-id="0f5ab-120">tooquickly iteraci změny, můžete spustit aplikaci v rámci kontejneru a pokračovat toomake změny, jako byste službou IIS Express je zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-120">tooquickly iterate changes, you can start your application within a container, and continue toomake changes, viewing them as you would with IIS Express.</span></span>

1. <span data-ttu-id="0f5ab-121">Nastavit hello konfigurace řešení příliš`Debug` a stiskněte klávesu  **&lt;kombinaci kláves CTRL + F5 >** toobuild vaše docker bitovou kopii a spustit místně.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-121">Set hello Solution Configuration too`Debug` and press **&lt;CTRL + F5>** toobuild your docker image and run it locally.</span></span>

    <span data-ttu-id="0f5ab-122">Po hello kontejneru image byla vytvořena a spuštěna v kontejner Docker, spustí se v sadě Visual Studio hello webové aplikace ve výchozí prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-122">Once hello container image has been built and is running in a Docker container, Visual Studio will launch hello Web app in your default browser.</span></span>
    <span data-ttu-id="0f5ab-123">Pokud používáte prohlížeč Microsoft Edge hello nebo v opačném případě obsahuje chyby, přečtěte si téma [Poradce při potížích s](vs-azure-tools-docker-troubleshooting-docker-errors.md) části.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-123">If you are using hello Microsoft Edge browser or otherwise have errors, see [Troubleshooting](vs-azure-tools-docker-troubleshooting-docker-errors.md) section.</span></span>
2. <span data-ttu-id="0f5ab-124">Přejděte toohello o stránky, kde vytvoříme toomake naše změny.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-124">Go toohello About page, which is where we're going toomake our changes.</span></span>
3. <span data-ttu-id="0f5ab-125">Vrátí tooVisual Studio a otevřete `Views\Home\About.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-125">Return tooVisual Studio and open `Views\Home\About.cshtml`.</span></span>
4. <span data-ttu-id="0f5ab-126">Přidejte následující HTML obsahu toohello konec souboru hello hello a uložit změny hello.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-126">Add hello following HTML content toohello end of hello file and save hello changes.</span></span>

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. <span data-ttu-id="0f5ab-127">Okno výstup hello, zobrazení, jakmile bude dokončena hello sestavení rozhraní .NET a zobrazit tyto řádky, přepněte zpět tooyour prohlížeče a aktualizujte hello o stránce.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-127">Viewing hello output window, when hello .NET build is completed and you see these lines, switch back tooyour browser and refresh hello About page.</span></span>

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C tooshut down
   ```
6. <span data-ttu-id="0f5ab-128">Změny se aplikovaly!</span><span class="sxs-lookup"><span data-stu-id="0f5ab-128">Your changes have been applied!</span></span>

## <a name="4-debug-with-breakpoints"></a><span data-ttu-id="0f5ab-129">4. Ladění pomocí zarážek</span><span class="sxs-lookup"><span data-stu-id="0f5ab-129">4. Debug with breakpoints</span></span>
<span data-ttu-id="0f5ab-130">Často změny potřebovat další kontroly, využití hello ladění funkce sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-130">Often, changes will need further inspection, leveraging hello debugging features of Visual Studio.</span></span>

1. <span data-ttu-id="0f5ab-131">Vrátí tooVisual Studio a otevřete`Controllers\HomeController.cs`</span><span class="sxs-lookup"><span data-stu-id="0f5ab-131">Return tooVisual Studio and open `Controllers\HomeController.cs`</span></span>
2. <span data-ttu-id="0f5ab-132">Nahraďte obsah hello hello About() metody hello následující:</span><span class="sxs-lookup"><span data-stu-id="0f5ab-132">Replace hello contents of hello About() method with hello following:</span></span>

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. <span data-ttu-id="0f5ab-133">Sada toohello zarážku pravé hello `string message`... řádku.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-133">Set a breakpoint toohello left of hello `string message`... line.</span></span>
4. <span data-ttu-id="0f5ab-134">Stiskněte tlačítko  **&lt;F5 >** toostart ladění.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-134">Hit **&lt;F5>** toostart debugging.</span></span>
5. <span data-ttu-id="0f5ab-135">Přejděte toohello o stránku toohit vaší zarážce.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-135">Navigate toohello About page toohit your breakpoint.</span></span>
6. <span data-ttu-id="0f5ab-136">Přepnout zarážku tooVisual Studio tooview hello – a kontrola hello hodnotu zprávy.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-136">Switch tooVisual Studio tooview hello breakpoint, and inspect hello value of message.</span></span>

   ![][2]

## <a name="summary"></a><span data-ttu-id="0f5ab-137">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0f5ab-137">Summary</span></span>
<span data-ttu-id="0f5ab-138">S [2015 nástroje sady Visual Studio pro Docker](https://aka.ms/DockerToolsForVS), můžete získat hello produktivitu místně, pracovat s produkční realism hello vývoje v rámci kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="0f5ab-138">With [Visual Studio 2015 Tools for Docker](https://aka.ms/DockerToolsForVS), you can get hello productivity of working locally, with hello production realism of developing within a Docker container.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="0f5ab-139">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="0f5ab-139">Troubleshooting</span></span>
[<span data-ttu-id="0f5ab-140">Řešení potíží s Visual Studio Docker vývoj</span><span class="sxs-lookup"><span data-stu-id="0f5ab-140">Troubleshooting Visual Studio Docker Development</span></span>](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a><span data-ttu-id="0f5ab-141">Další informace o Docker s Visual Studio, Windows a Azure</span><span class="sxs-lookup"><span data-stu-id="0f5ab-141">More about Docker with Visual Studio, Windows, and Azure</span></span>
* <span data-ttu-id="0f5ab-142">[Docker nástrojů pro Visual Studio](http://aka.ms/dockertoolsforvs) – vývoj kódu .NET Core v kontejneru</span><span class="sxs-lookup"><span data-stu-id="0f5ab-142">[Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) - Developing your .NET Core code in a container</span></span>
* <span data-ttu-id="0f5ab-143">[Docker nástrojů pro Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) – sestavení a nasazení kontejnerů docker</span><span class="sxs-lookup"><span data-stu-id="0f5ab-143">[Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - Build and Deploy docker containers</span></span>
* <span data-ttu-id="0f5ab-144">[Docker nástrojů pro Visual Studio Code](http://aka.ms/dockertoolsforvscode) -services jazyk pro úpravy docker soubory s více scénářů e2e přicházející</span><span class="sxs-lookup"><span data-stu-id="0f5ab-144">[Docker Tools for Visual Studio Code](http://aka.ms/dockertoolsforvscode) - Language services for editing docker files, with more e2e scenarios coming</span></span>
* <span data-ttu-id="0f5ab-145">[Informace o kontejneru systému Windows](http://aka.ms/containers)-informace o systému Windows Server a Nano Server</span><span class="sxs-lookup"><span data-stu-id="0f5ab-145">[Windows Container Information](http://aka.ms/containers)- Windows Server and Nano Server information</span></span>
* <span data-ttu-id="0f5ab-146">[Azure Container Service](https://azure.microsoft.com/services/container-service/) - [obsahu Azure Container Service](http://aka.ms/AzureContainerService)</span><span class="sxs-lookup"><span data-stu-id="0f5ab-146">[Azure Container Service](https://azure.microsoft.com/services/container-service/) - [Azure Container Service Content](http://aka.ms/AzureContainerService)</span></span>
* <span data-ttu-id="0f5ab-147">Další příklady práce s Docker, najdete v části [práce s Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) z hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 připojit [ukázku](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span><span class="sxs-lookup"><span data-stu-id="0f5ab-147">For more examples of working with Docker, see [Working with Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) from hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span></span> <span data-ttu-id="0f5ab-148">Další quickstarts z hello HealthClinic.biz ukázku, najdete v části [Quickstarts nástroje pro vývojáře Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span><span class="sxs-lookup"><span data-stu-id="0f5ab-148">For more quickstarts from hello HealthClinic.biz demo, see [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span></span>

## <a name="various-docker-tools"></a><span data-ttu-id="0f5ab-149">Různé nástroje Docker</span><span class="sxs-lookup"><span data-stu-id="0f5ab-149">Various Docker tools</span></span>
[<span data-ttu-id="0f5ab-150">Některé skvělé docker nástroje (blog Steva Lasker)</span><span class="sxs-lookup"><span data-stu-id="0f5ab-150">Some great docker tools (Steve Lasker's blog)</span></span>](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a><span data-ttu-id="0f5ab-151">Dobrý články</span><span class="sxs-lookup"><span data-stu-id="0f5ab-151">Good articles</span></span>
[<span data-ttu-id="0f5ab-152">Úvod tooMicroservices z NGINX</span><span class="sxs-lookup"><span data-stu-id="0f5ab-152">Introduction tooMicroservices from NGINX</span></span>](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a><span data-ttu-id="0f5ab-153">Prezentace</span><span class="sxs-lookup"><span data-stu-id="0f5ab-153">Presentations</span></span>
* [<span data-ttu-id="0f5ab-154">Steve Lasker: VS Live Las Vegas 2016 - Docker e2e</span><span class="sxs-lookup"><span data-stu-id="0f5ab-154">Steve Lasker: VS Live Las Vegas 2016 - Docker e2e</span></span>](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [<span data-ttu-id="0f5ab-155">Úvod tooASP.NET základní @ sestavení 2016 – kde jste v ukázce</span><span class="sxs-lookup"><span data-stu-id="0f5ab-155">Introduction tooASP.NET Core @ build 2016 - Where You At Demo</span></span>](https://channel9.msdn.com/Events/Build/2016/B810)
* [<span data-ttu-id="0f5ab-156">Vývoj aplikací .NET v kontejnerech, Channel 9</span><span class="sxs-lookup"><span data-stu-id="0f5ab-156">Developing .NET apps in containers, Channel 9</span></span>](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
