---
title: "Ladění aplikací v místním kontejner Docker | Microsoft Docs"
description: "Zjistěte, jak upravit aplikaci, která běží v místní kontejner Docker, aktualizujte kontejneru prostřednictvím úpravy a aktualizace a nastavit zarážky ladění"
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
ms.openlocfilehash: fcd58736d8915a61683a416fb9bf3892ba7b7bd8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="debugging-apps-in-a-local-docker-container"></a><span data-ttu-id="738d8-103">Ladění aplikací v místním kontejneru Dockeru</span><span class="sxs-lookup"><span data-stu-id="738d8-103">Debugging apps in a local Docker container</span></span>
## <a name="overview"></a><span data-ttu-id="738d8-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="738d8-104">Overview</span></span>
<span data-ttu-id="738d8-105">Visual Studio Tools for Docker zajišťuje konzistentní způsob pro vývoj v a ověřit svoji aplikaci místně v kontejner Linux Docker.</span><span class="sxs-lookup"><span data-stu-id="738d8-105">The Visual Studio Tools for Docker provides a consistent way to develop in and validate your application locally in a Linux Docker container.</span></span>
<span data-ttu-id="738d8-106">Nemáte restartuje kontejner pokaždé, když provedete kód změnit.</span><span class="sxs-lookup"><span data-stu-id="738d8-106">You don't have to restart the container each time you make a code change.</span></span>
<span data-ttu-id="738d8-107">Tento článek ukazuje, jak používat funkci "Upravit a aktualizovat" a spusťte ASP.NET Core webové aplikace v místním kontejner Docker, proveďte potřebné změny a pak aktualizujte prohlížeč a sledovat provedené změny.</span><span class="sxs-lookup"><span data-stu-id="738d8-107">This article illustrates how to use the "Edit and Refresh" feature to start an ASP.NET Core Web app in a local Docker container, make any necessary changes, and then refresh the browser to see those changes.</span></span>
<span data-ttu-id="738d8-108">Tento článek také ukazuje, jak nastavit zarážky pro ladění.</span><span class="sxs-lookup"><span data-stu-id="738d8-108">This article also shows you how to set breakpoints for debugging.</span></span>

> [!NOTE]
> <span data-ttu-id="738d8-109">Podpora kontejnerů Windows budou přicházet v budoucí verzi</span><span class="sxs-lookup"><span data-stu-id="738d8-109">Windows Container support will be coming in a future release</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="738d8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="738d8-110">Prerequisites</span></span>
<span data-ttu-id="738d8-111">Tyto nástroje musí být nainstalován.</span><span class="sxs-lookup"><span data-stu-id="738d8-111">The following tools must be installed.</span></span>

* [<span data-ttu-id="738d8-112">Nejnovější verze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="738d8-112">Latest version of Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="738d8-113">Microsoft ASP.NET Core 1.0 SDK</span><span class="sxs-lookup"><span data-stu-id="738d8-113">Microsoft ASP.NET Core 1.0 SDK</span></span>](https://go.microsoft.com/fwlink/?LinkID=809122)

<span data-ttu-id="738d8-114">Ke spuštění Docker kontejnery místně, budete potřebovat místní docker klienta.</span><span class="sxs-lookup"><span data-stu-id="738d8-114">To run Docker containers locally, you'll need a local docker client.</span></span>
<span data-ttu-id="738d8-115">Můžete použít [Docker nástrojů](https://www.docker.com/products/docker-toolbox), což vyžaduje, aby technologie Hyper-V se zakáže, nebo můžete použít [Docker pro systém Windows](https://www.docker.com/get-docker), která využívá technologie Hyper-V a vyžaduje Windows 10.</span><span class="sxs-lookup"><span data-stu-id="738d8-115">You can use the [Docker Toolbox](https://www.docker.com/products/docker-toolbox), which requires Hyper-V to be disabled, or you can use [Docker for Windows](https://www.docker.com/get-docker), which uses Hyper-V, and requires Windows 10.</span></span>

<span data-ttu-id="738d8-116">Pokud používáte Docker sada nástrojů, budete muset [konfigurace klienta Docker](vs-azure-tools-docker-setup.md)</span><span class="sxs-lookup"><span data-stu-id="738d8-116">If using Docker Toolbox, you'll need to [configure the Docker client](vs-azure-tools-docker-setup.md)</span></span>

## <a name="1-create-a-web-app"></a><span data-ttu-id="738d8-117">1. Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="738d8-117">1. Create a web app</span></span>
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a><span data-ttu-id="738d8-118">2. Přidání podpory Docker</span><span class="sxs-lookup"><span data-stu-id="738d8-118">2. Add Docker support</span></span>
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a><span data-ttu-id="738d8-119">3. Upravit kód a aktualizace</span><span class="sxs-lookup"><span data-stu-id="738d8-119">3. Edit your code and refresh</span></span>
<span data-ttu-id="738d8-120">Rychle iterovat změny, můžete spustit aplikaci v rámci kontejneru a nadále provádět změny, jako byste službou IIS Express je zobrazení.</span><span class="sxs-lookup"><span data-stu-id="738d8-120">To quickly iterate changes, you can start your application within a container, and continue to make changes, viewing them as you would with IIS Express.</span></span>

1. <span data-ttu-id="738d8-121">Nastavte konfiguraci řešení na `Debug` a stiskněte klávesu  **&lt;kombinaci kláves CTRL + F5 >** na vytvoření bitové kopie docker a spustit místně.</span><span class="sxs-lookup"><span data-stu-id="738d8-121">Set the Solution Configuration to `Debug` and press **&lt;CTRL + F5>** to build your docker image and run it locally.</span></span>

    <span data-ttu-id="738d8-122">Jakmile bitovou kopii kontejneru byla vytvořena a spuštěna v kontejner Docker, Visual Studio spustí webové aplikace ve výchozí prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="738d8-122">Once the container image has been built and is running in a Docker container, Visual Studio will launch the Web app in your default browser.</span></span>
    <span data-ttu-id="738d8-123">Pokud používáte prohlížeč Microsoft Edge nebo v opačném případě obsahuje chyby, přečtěte si téma [Poradce při potížích s](vs-azure-tools-docker-troubleshooting-docker-errors.md) části.</span><span class="sxs-lookup"><span data-stu-id="738d8-123">If you are using the Microsoft Edge browser or otherwise have errors, see [Troubleshooting](vs-azure-tools-docker-troubleshooting-docker-errors.md) section.</span></span>
2. <span data-ttu-id="738d8-124">Přejděte na stránku o, což je, kde vytvoříme naše změnit.</span><span class="sxs-lookup"><span data-stu-id="738d8-124">Go to the About page, which is where we're going to make our changes.</span></span>
3. <span data-ttu-id="738d8-125">Vraťte se na Visual Studio a otevřete `Views\Home\About.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="738d8-125">Return to Visual Studio and open `Views\Home\About.cshtml`.</span></span>
4. <span data-ttu-id="738d8-126">Přidejte do něj následující obsah HTML na konec souboru a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="738d8-126">Add the following HTML content to the end of the file and save the changes.</span></span>

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. <span data-ttu-id="738d8-127">Prohlížení ve výstupním okně po dokončení sestavení rozhraní .NET a zobrazit tyto řádky, přepněte zpět na prohlížeči a aktualizujte stránku o.</span><span class="sxs-lookup"><span data-stu-id="738d8-127">Viewing the output window, when the .NET build is completed and you see these lines, switch back to your browser and refresh the About page.</span></span>

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C to shut down
   ```
6. <span data-ttu-id="738d8-128">Změny se aplikovaly!</span><span class="sxs-lookup"><span data-stu-id="738d8-128">Your changes have been applied!</span></span>

## <a name="4-debug-with-breakpoints"></a><span data-ttu-id="738d8-129">4. Ladění pomocí zarážek</span><span class="sxs-lookup"><span data-stu-id="738d8-129">4. Debug with breakpoints</span></span>
<span data-ttu-id="738d8-130">Často změny potřebovat další kontroly, využívat funkce ladění sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="738d8-130">Often, changes will need further inspection, leveraging the debugging features of Visual Studio.</span></span>

1. <span data-ttu-id="738d8-131">Vraťte se na Visual Studio a otevřít`Controllers\HomeController.cs`</span><span class="sxs-lookup"><span data-stu-id="738d8-131">Return to Visual Studio and open `Controllers\HomeController.cs`</span></span>
2. <span data-ttu-id="738d8-132">Nahraďte obsah metody About() s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="738d8-132">Replace the contents of the About() method with the following:</span></span>

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. <span data-ttu-id="738d8-133">Nastavit zarážky nalevo od `string message`... řádku.</span><span class="sxs-lookup"><span data-stu-id="738d8-133">Set a breakpoint to the left of the `string message`... line.</span></span>
4. <span data-ttu-id="738d8-134">Stiskněte tlačítko  **&lt;F5 >** spustit ladění.</span><span class="sxs-lookup"><span data-stu-id="738d8-134">Hit **&lt;F5>** to start debugging.</span></span>
5. <span data-ttu-id="738d8-135">Přejděte na stránku o narazí vaší zarážce.</span><span class="sxs-lookup"><span data-stu-id="738d8-135">Navigate to the About page to hit your breakpoint.</span></span>
6. <span data-ttu-id="738d8-136">Přepněte do zobrazení zarážce sady Visual Studio a zjistit hodnotu zprávy.</span><span class="sxs-lookup"><span data-stu-id="738d8-136">Switch to Visual Studio to view the breakpoint, and inspect the value of message.</span></span>

   ![][2]

## <a name="summary"></a><span data-ttu-id="738d8-137">Souhrn</span><span class="sxs-lookup"><span data-stu-id="738d8-137">Summary</span></span>
<span data-ttu-id="738d8-138">S [2015 nástroje sady Visual Studio pro Docker](https://aka.ms/DockerToolsForVS), můžete získat na produktivitu místně, pracovat s produkční realism vývoje v rámci kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="738d8-138">With [Visual Studio 2015 Tools for Docker](https://aka.ms/DockerToolsForVS), you can get the productivity of working locally, with the production realism of developing within a Docker container.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="738d8-139">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="738d8-139">Troubleshooting</span></span>
[<span data-ttu-id="738d8-140">Řešení potíží s Visual Studio Docker vývoj</span><span class="sxs-lookup"><span data-stu-id="738d8-140">Troubleshooting Visual Studio Docker Development</span></span>](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a><span data-ttu-id="738d8-141">Další informace o Docker s Visual Studio, Windows a Azure</span><span class="sxs-lookup"><span data-stu-id="738d8-141">More about Docker with Visual Studio, Windows, and Azure</span></span>
* <span data-ttu-id="738d8-142">[Docker nástrojů pro Visual Studio](http://aka.ms/dockertoolsforvs) – vývoj kódu .NET Core v kontejneru</span><span class="sxs-lookup"><span data-stu-id="738d8-142">[Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) - Developing your .NET Core code in a container</span></span>
* <span data-ttu-id="738d8-143">[Docker nástrojů pro Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) – sestavení a nasazení kontejnerů docker</span><span class="sxs-lookup"><span data-stu-id="738d8-143">[Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - Build and Deploy docker containers</span></span>
* <span data-ttu-id="738d8-144">[Docker nástrojů pro Visual Studio Code](http://aka.ms/dockertoolsforvscode) -services jazyk pro úpravy docker soubory s více scénářů e2e přicházející</span><span class="sxs-lookup"><span data-stu-id="738d8-144">[Docker Tools for Visual Studio Code](http://aka.ms/dockertoolsforvscode) - Language services for editing docker files, with more e2e scenarios coming</span></span>
* <span data-ttu-id="738d8-145">[Informace o kontejneru systému Windows](http://aka.ms/containers)-informace o systému Windows Server a Nano Server</span><span class="sxs-lookup"><span data-stu-id="738d8-145">[Windows Container Information](http://aka.ms/containers)- Windows Server and Nano Server information</span></span>
* <span data-ttu-id="738d8-146">[Azure Container Service](https://azure.microsoft.com/services/container-service/) - [obsahu Azure Container Service](http://aka.ms/AzureContainerService)</span><span class="sxs-lookup"><span data-stu-id="738d8-146">[Azure Container Service](https://azure.microsoft.com/services/container-service/) - [Azure Container Service Content](http://aka.ms/AzureContainerService)</span></span>
* <span data-ttu-id="738d8-147">Další příklady práce s Docker, najdete v části [práce s Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) z [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 připojit [ukázku](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span><span class="sxs-lookup"><span data-stu-id="738d8-147">For more examples of working with Docker, see [Working with Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) from the [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span></span> <span data-ttu-id="738d8-148">Další rychlé starty z demoverze HealthClinic.biz najdete v článku [Rychlé starty nástrojů pro vývojáře Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span><span class="sxs-lookup"><span data-stu-id="738d8-148">For more quickstarts from the HealthClinic.biz demo, see [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span></span>

## <a name="various-docker-tools"></a><span data-ttu-id="738d8-149">Různé nástroje Docker</span><span class="sxs-lookup"><span data-stu-id="738d8-149">Various Docker tools</span></span>
[<span data-ttu-id="738d8-150">Některé skvělé docker nástroje (blog Steva Lasker)</span><span class="sxs-lookup"><span data-stu-id="738d8-150">Some great docker tools (Steve Lasker's blog)</span></span>](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a><span data-ttu-id="738d8-151">Dobrý články</span><span class="sxs-lookup"><span data-stu-id="738d8-151">Good articles</span></span>
[<span data-ttu-id="738d8-152">Úvod do Mikroslužeb z NGINX</span><span class="sxs-lookup"><span data-stu-id="738d8-152">Introduction to Microservices from NGINX</span></span>](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a><span data-ttu-id="738d8-153">Prezentace</span><span class="sxs-lookup"><span data-stu-id="738d8-153">Presentations</span></span>
* [<span data-ttu-id="738d8-154">Steve Lasker: VS Live Las Vegas 2016 - Docker e2e</span><span class="sxs-lookup"><span data-stu-id="738d8-154">Steve Lasker: VS Live Las Vegas 2016 - Docker e2e</span></span>](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [<span data-ttu-id="738d8-155">Úvod do ASP.NET Core @ sestavení 2016 – kde jste v ukázce</span><span class="sxs-lookup"><span data-stu-id="738d8-155">Introduction to ASP.NET Core @ build 2016 - Where You At Demo</span></span>](https://channel9.msdn.com/Events/Build/2016/B810)
* [<span data-ttu-id="738d8-156">Vývoj aplikací .NET v kontejnerech, Channel 9</span><span class="sxs-lookup"><span data-stu-id="738d8-156">Developing .NET apps in containers, Channel 9</span></span>](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
