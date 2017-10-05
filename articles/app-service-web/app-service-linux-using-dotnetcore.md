---
title: "Použít .NET Core ve webové aplikaci v systému Linux | Microsoft Docs"
description: "Pomocí .NET Core ve webové aplikaci v systému Linux."
keywords: "služby Azure app service, webové aplikace, dotnet, core, linux, operačních systémů"
services: app-service
documentationCenter: 
authors: michimune, rachelappel
manager: erikre
editor: 
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: aelnably;wesmc;mikono;rachelap
ms.openlocfilehash: 9226dfb90e52ac2cae2cfc4af7c0705a93f56b44
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-net-core-in-an-azure-web-app-on-linux"></a><span data-ttu-id="87669-104">Použití .NET Core v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="87669-104">Use .NET Core in an Azure web app on Linux</span></span> #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="87669-105">[Webová aplikace](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) v systému Linux nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby pomocí operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="87669-105">[Web App](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) on Linux provides a highly scalable, self-patching web hosting service using the Linux operating system.</span></span> <span data-ttu-id="87669-106">Tento kurz obsahuje podrobné pokyny znázorňující postup tvorby [.NET Core](https://docs.microsoft.com/aspnet/core/) aplikace na Azure webové aplikace v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="87669-106">This tutorial contains step-by-step instructions showing how to create a [.NET Core](https://docs.microsoft.com/aspnet/core/) app on Azure web app on Linux.</span></span> 

![Webová aplikace v Linuxu][10]

<span data-ttu-id="87669-108">Následující postup můžete použít v případě počítačů Mac, Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="87669-108">You can follow the steps below using a Mac, Windows, or Linux machine.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87669-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="87669-109">Prerequisites</span></span> ##

<span data-ttu-id="87669-110">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="87669-110">To complete this tutorial:</span></span> 

* <span data-ttu-id="87669-111">Nainstalujte [.NET Core SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="87669-111">Install the [.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>
* <span data-ttu-id="87669-112">Nainstalujte [Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="87669-112">Install [Git](https://git-scm.com/downloads).</span></span>

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-local-net-core-application"></a><span data-ttu-id="87669-113">Vytvořit místní aplikace .NET Core</span><span class="sxs-lookup"><span data-stu-id="87669-113">Create a local .NET Core application</span></span> ##

<span data-ttu-id="87669-114">Spusťte novou relaci terminálu.</span><span class="sxs-lookup"><span data-stu-id="87669-114">Start a new terminal session.</span></span> <span data-ttu-id="87669-115">Vytvořte adresář s názvem `hellodotnetcore`a k jeho změně aktuálního adresáře.</span><span class="sxs-lookup"><span data-stu-id="87669-115">Create a directory named `hellodotnetcore`, and change the current directory to it.</span></span> <span data-ttu-id="87669-116">Potom zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="87669-116">Then type the following:</span></span> 

```
dotnet new web
``` 

  <span data-ttu-id="87669-117">Tento příkaz vytvoří tři soubory (*hellodotnetcore.csproj*, *Program.cs*, a *Startup.cs*) a jeden prázdnou složku (*wwwroot /*) v aktuálním adresáři.</span><span class="sxs-lookup"><span data-stu-id="87669-117">This command creates three files (*hellodotnetcore.csproj*, *Program.cs*, and *Startup.cs*) and one empty folder (*wwwroot/*) under the current directory.</span></span> <span data-ttu-id="87669-118">Obsah `.csproj` soubor by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="87669-118">The content of `.csproj` file should look like the following:</span></span> 

```xml
  <!-- Empty lines are omitted. -->

  <Project Sdk="Microsoft.NET.Sdk.Web">
        <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        </PropertyGroup>
        <ItemGroup>
        <Folder Include="wwwroot\" />
        </ItemGroup>
        <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore" Version="1.1.2" />
        </ItemGroup>
  </Project>
```

<span data-ttu-id="87669-119">Vzhledem k tomu, že tato aplikace je webová aplikace, odkaz na balíček ASP.NET Core se automaticky přidá do *hellodotnetcore.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="87669-119">Since this app is a web application, a reference to an ASP.NET Core package was automatically added to the *hellodotnetcore.csproj* file.</span></span> <span data-ttu-id="87669-120">Číslo verze balíčku je nastavena podle zvolené framework.</span><span class="sxs-lookup"><span data-stu-id="87669-120">The version number of the package is set according to the chosen framework.</span></span> <span data-ttu-id="87669-121">V tomto příkladu odkazuje ASP.NET Core verze 1.1.2, protože se používá .NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="87669-121">This example is referencing ASP.NET Core version 1.1.2 because .NET Core 1.1 is used.</span></span>

## <a name="build-and-test-the-application-locally"></a><span data-ttu-id="87669-122">Vytvoření a testování aplikace místně</span><span class="sxs-lookup"><span data-stu-id="87669-122">Build and test the application locally</span></span> ##

<span data-ttu-id="87669-123">Můžete sestavit a spustit aplikace .NET Core pomocí `dotnet restore` příkazu s parametrem `dotnet run` příkaz, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="87669-123">You can build and run your .NET Core app with the `dotnet restore` command followed by the `dotnet run` command, as shown here:</span></span>

```
dotnet restore
dotnet run
```


<span data-ttu-id="87669-124">Při spuštění aplikace, zobrazí se zpráva s oznámením, že aplikace naslouchá na příchozí požadavky na port.</span><span class="sxs-lookup"><span data-stu-id="87669-124">When the application starts, it displays a message indicating the app is listening to incoming requests at a port.</span></span> 

```bash
Hosting environment: Production
Content root path: C:\hellodotnetcore
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="87669-125">Testování procházením `http://localhost:5000/` s prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="87669-125">Test it by browsing to `http://localhost:5000/` with your browser.</span></span> <span data-ttu-id="87669-126">Pokud se vše funguje bez problémů, se zobrazí "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="87669-126">If everything works fine, you see "Hello World!"</span></span> <span data-ttu-id="87669-127">jako výsledek text.</span><span class="sxs-lookup"><span data-stu-id="87669-127">as the result text.</span></span>

![Testování pomocí prohlížeče][7]

## <a name="create-a-net-core-app-in-the-azure-portal"></a><span data-ttu-id="87669-129">Vytvoření .NET Core aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="87669-129">Create a .NET Core app in the Azure Portal</span></span> ##

<span data-ttu-id="87669-130">Je třeba nejprve vytvořit prázdnou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="87669-130">First you need to create an empty web app.</span></span> <span data-ttu-id="87669-131">Přihlaste se k [portál Azure](https://portal.azure.com/) a vytvořte novou [webové aplikace v systému Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).</span><span class="sxs-lookup"><span data-stu-id="87669-131">Log in to the [Azure portal](https://portal.azure.com/) and create a new [Web App on Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).</span></span>

![Vytváření webové aplikace][1]

<span data-ttu-id="87669-133">Když **vytvořit** otevře se stránka, obsahují podrobné informace o vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="87669-133">When the **Create** page opens, provide details about your web app:</span></span>

![Výběr .NET Core runtime zásobníku][2]

<span data-ttu-id="87669-135">Následující tabulku použijte jako vodítko k vyplnění **vytvořit** stránky a pak vyberte **OK** a **vytvořit** k vytvoření dané aplikace.</span><span class="sxs-lookup"><span data-stu-id="87669-135">Use the following table as a guide to fill out the **Create** page, then select **OK** and **Create** to create the app.</span></span>

| <span data-ttu-id="87669-136">Nastavení</span><span class="sxs-lookup"><span data-stu-id="87669-136">Setting</span></span>      | <span data-ttu-id="87669-137">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="87669-137">Suggested value</span></span>  | <span data-ttu-id="87669-138">Popis</span><span class="sxs-lookup"><span data-stu-id="87669-138">Description</span></span>                                        |
| ------------ | ---------------- | -------------------------------------------------- |
| <span data-ttu-id="87669-139">Název aplikace.</span><span class="sxs-lookup"><span data-stu-id="87669-139">App name</span></span> | <span data-ttu-id="87669-140">hellodotnetcore</span><span class="sxs-lookup"><span data-stu-id="87669-140">hellodotnetcore</span></span>  | <span data-ttu-id="87669-141">Název aplikace.</span><span class="sxs-lookup"><span data-stu-id="87669-141">The name of your app.</span></span> <span data-ttu-id="87669-142">Tento název musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="87669-142">This name must be unique.</span></span> |
| <span data-ttu-id="87669-143">Předplatné</span><span class="sxs-lookup"><span data-stu-id="87669-143">Subscription</span></span> | <span data-ttu-id="87669-144">Vyberte stávající předplatné</span><span class="sxs-lookup"><span data-stu-id="87669-144">Choose an existing subscription</span></span> | <span data-ttu-id="87669-145">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="87669-145">The Azure subscription.</span></span> |
| <span data-ttu-id="87669-146">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="87669-146">Resource Group</span></span> | <span data-ttu-id="87669-147">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="87669-147">myResourceGroup</span></span> |  <span data-ttu-id="87669-148">Skupina prostředků Azure tak, aby obsahovala webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="87669-148">The Azure resource group to contain the web app.</span></span> |
| <span data-ttu-id="87669-149">Plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="87669-149">App Service Plan</span></span> | <span data-ttu-id="87669-150">Název existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="87669-150">Existing App Service Plan name</span></span> |  <span data-ttu-id="87669-151">Plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="87669-151">The App Service plan.</span></span>  |
| <span data-ttu-id="87669-152">Konfiguraci kontejneru</span><span class="sxs-lookup"><span data-stu-id="87669-152">Configure Container</span></span> | <span data-ttu-id="87669-153">.NET core 1.1</span><span class="sxs-lookup"><span data-stu-id="87669-153">.NET Core 1.1</span></span> | <span data-ttu-id="87669-154">Typ kontejneru pro tuto webovou aplikaci: integrované, Docker nebo privátní registru.</span><span class="sxs-lookup"><span data-stu-id="87669-154">The type of container for this web app: Built-in, Docker, or Private registry.</span></span> |
| <span data-ttu-id="87669-155">Zdroj bitové kopie</span><span class="sxs-lookup"><span data-stu-id="87669-155">Image source</span></span>  | <span data-ttu-id="87669-156">Integrované</span><span class="sxs-lookup"><span data-stu-id="87669-156">Built-in</span></span>  |  <span data-ttu-id="87669-157">Zdroj bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="87669-157">The source of the image.</span></span> |
| <span data-ttu-id="87669-158">Modul runtime zásobníku</span><span class="sxs-lookup"><span data-stu-id="87669-158">Runtime Stack</span></span>  | <span data-ttu-id="87669-159">.NET core 1.1</span><span class="sxs-lookup"><span data-stu-id="87669-159">.NET Core 1.1</span></span>  | <span data-ttu-id="87669-160">Modul runtime zásobníku a verze.</span><span class="sxs-lookup"><span data-stu-id="87669-160">The runtime stack and version.</span></span>  |

## <a name="deploy-your-application-via-git"></a><span data-ttu-id="87669-161">Nasazení aplikace prostřednictvím Git</span><span class="sxs-lookup"><span data-stu-id="87669-161">Deploy your application via Git</span></span> ##

<span data-ttu-id="87669-162">Pomocí Git Nasaďte aplikaci .NET Core do webové aplikace Azure App Service v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="87669-162">Use Git to deploy the .NET Core application to Azure App Service Web App on Linux.</span></span>

<span data-ttu-id="87669-163">Nové webové aplikace Azure je již nakonfigurován nasazení Git.</span><span class="sxs-lookup"><span data-stu-id="87669-163">The new Azure web app already has Git deployment configured.</span></span> <span data-ttu-id="87669-164">Adresy URL pro Git nasazení zjistíte tak, že přejdete na tuto adresu URL po vložení název vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="87669-164">You will find the Git deployment URL by navigating to the following URL after inserting your web app name:</span></span>

```https://{your web app name}.scm.azurewebsites.net/api/scm/info```

<span data-ttu-id="87669-165">Adresy URL pro Git má následující formulář založený na název vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="87669-165">The Git URL has the following form based on your web app name:</span></span>

```https://{your web app name}.scm.azurewebsites.net/{your web app name}.git```

<span data-ttu-id="87669-166">Spusťte následující příkazy k nasazení místní aplikace do Azure webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="87669-166">Run the following commands to deploy the local application to your Azure web app:</span></span> 
 
```bash
git init
git remote add azure <Git deployment URL from above>
git add *.csproj *.cs
git commit -m "Initial deployment commit"
git push azure master
```

<span data-ttu-id="87669-167">Nemusíte push všechny soubory pod *bin /* nebo *obj /* adresáře aplikace je integrovaná v cloudu, pokud zdrojové soubory aplikace se instaluje do Azure.</span><span class="sxs-lookup"><span data-stu-id="87669-167">You don't need to push any files under *bin/* or *obj/* directories because your application is built in the cloud when the application's source files are pushed to Azure.</span></span> <span data-ttu-id="87669-168">Po dokončení procesu sestavení binární soubory se zkopírují do adresáře aplikace v */home/server/wwwroot/*.</span><span class="sxs-lookup"><span data-stu-id="87669-168">After the build process is complete, binary files are copied into the application's directory at */home/site/wwwroot/*.</span></span>

<span data-ttu-id="87669-169">Potvrďte, že operace vzdálené nasazení sestav úspěch.</span><span class="sxs-lookup"><span data-stu-id="87669-169">Confirm that the remote deployment operations report success.</span></span> <span data-ttu-id="87669-170">Nabízená operace může trvat od balíčku řešení a sestavení procesu spouštění v cloudu.</span><span class="sxs-lookup"><span data-stu-id="87669-170">Push operations may take a while since package resolution and build process run in the cloud.</span></span> <span data-ttu-id="87669-171">Zobrazí se několik stavové zprávy, včetně těch, které jsou s oznámením, že soubory byly zkopírovány.</span><span class="sxs-lookup"><span data-stu-id="87669-171">You will see several status messages, including ones stating that files have been copied.</span></span> <span data-ttu-id="87669-172">Výstup by měl vypadat podobně jako následující:</span><span class="sxs-lookup"><span data-stu-id="87669-172">The output should look similar to the following:</span></span>

```bash
/* some output has been removed for brevity */
remote: Copying file: 'System.Net.Websockets.dll' 
remote: Copying file: 'System.Runtime.CompilerServices.Unsafe.dll' 
remote: Copying file: 'System.Runtime.Serialization.Primitives.dll' 
remote: Copying file: 'System.Text.Encodings.Web.dll' 
remote: Copying file: 'hellodotnetcore.deps.json' 
remote: Copying file: 'hellodotnetcore.dll' 
remote: Omitting next output lines...
remote: Finished successfully.
remote: Running post deployment commands...
remote: Deployment successful.
To https://hellodotnetcore.scm.azurewebsites.net/
 * [new branch]           master -> master

```

<span data-ttu-id="87669-173">Po dokončení nasazení restartování webové aplikace pro nasazení vstoupily v platnost.</span><span class="sxs-lookup"><span data-stu-id="87669-173">Once the deployment has completed, restart your web app for the deployment to take effect.</span></span> <span data-ttu-id="87669-174">Chcete-li to provést, přejděte na portál Azure a přejděte do **přehled** stránku vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="87669-174">To do this, go to the Azure portal and navigate to the **Overview** page of your web app.</span></span> <span data-ttu-id="87669-175">Vyberte **restartujte** tlačítka na stránce.</span><span class="sxs-lookup"><span data-stu-id="87669-175">Select the **Restart** button in the page.</span></span> <span data-ttu-id="87669-176">Když se zobrazí místní okno, vyberte **Ano** k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="87669-176">When a popup window shows up, select **Yes** to confirm.</span></span> <span data-ttu-id="87669-177">Potom můžete procházet vaší webové aplikaci, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="87669-177">You can then browse your web app, as shown here:</span></span>

![Procházení .NET Core aplikace nasazené do služby Azure App Service v systému Linux][10]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="87669-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87669-179">Next steps</span></span>
* [<span data-ttu-id="87669-180">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="87669-180">Azure App Service Web App on Linux FAQ</span></span>](./app-service-linux-faq.md)

[1]: ./media/app-service-linux-using-dotnetcore/top-level-create.png
[2]: ./media/app-service-linux-using-dotnetcore/dotnet-new-webapp.png
[7]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-local.png
[10]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-azure.png
