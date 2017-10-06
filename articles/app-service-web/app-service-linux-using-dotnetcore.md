---
title: "aaaUse .NET Core ve webové aplikaci v systému Linux | Microsoft Docs"
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
ms.openlocfilehash: 9b7fb7185dff2c99ed88e7937d455177504937b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-net-core-in-an-azure-web-app-on-linux"></a><span data-ttu-id="ecc9f-104">Použití .NET Core v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="ecc9f-104">Use .NET Core in an Azure web app on Linux</span></span> #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="ecc9f-105">[Webová aplikace](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) v systému Linux poskytuje službu webové vysoce škálovatelnou a automatických oprav hostingu pomocí operační systém Linux hello.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-105">[Web App](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) on Linux provides a highly scalable, self-patching web hosting service using hello Linux operating system.</span></span> <span data-ttu-id="ecc9f-106">Tento kurz obsahuje podrobné pokyny, které se zobrazuje jak toocreate [.NET Core](https://docs.microsoft.com/aspnet/core/) aplikace na Azure webové aplikace v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-106">This tutorial contains step-by-step instructions showing how toocreate a [.NET Core](https://docs.microsoft.com/aspnet/core/) app on Azure web app on Linux.</span></span> 

![Webová aplikace v Linuxu][10]

<span data-ttu-id="ecc9f-108">Můžete provést kroky hello níže používání počítačů Mac, Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ecc9f-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ecc9f-109">Prerequisites</span></span> ##

<span data-ttu-id="ecc9f-110">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="ecc9f-110">toocomplete this tutorial:</span></span> 

* <span data-ttu-id="ecc9f-111">Nainstalujte hello [.NET Core SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="ecc9f-111">Install hello [.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>
* <span data-ttu-id="ecc9f-112">Nainstalujte [Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="ecc9f-112">Install [Git](https://git-scm.com/downloads).</span></span>

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-local-net-core-application"></a><span data-ttu-id="ecc9f-113">Vytvořit místní aplikace .NET Core</span><span class="sxs-lookup"><span data-stu-id="ecc9f-113">Create a local .NET Core application</span></span> ##

<span data-ttu-id="ecc9f-114">Spusťte novou relaci terminálu.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-114">Start a new terminal session.</span></span> <span data-ttu-id="ecc9f-115">Vytvořte adresář s názvem `hellodotnetcore`a změňte aktuální adresář tooit hello.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-115">Create a directory named `hellodotnetcore`, and change hello current directory tooit.</span></span> <span data-ttu-id="ecc9f-116">Poté zadejte hello následující:</span><span class="sxs-lookup"><span data-stu-id="ecc9f-116">Then type hello following:</span></span> 

```
dotnet new web
``` 

  <span data-ttu-id="ecc9f-117">Tento příkaz vytvoří tři soubory (*hellodotnetcore.csproj*, *Program.cs*, a *Startup.cs*) a jeden prázdnou složku (*wwwroot /*) v aktuálním adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-117">This command creates three files (*hellodotnetcore.csproj*, *Program.cs*, and *Startup.cs*) and one empty folder (*wwwroot/*) under hello current directory.</span></span> <span data-ttu-id="ecc9f-118">Hello obsah `.csproj` soubor by měl vypadat jako následující hello:</span><span class="sxs-lookup"><span data-stu-id="ecc9f-118">hello content of `.csproj` file should look like hello following:</span></span> 

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

<span data-ttu-id="ecc9f-119">Vzhledem k tomu, že je tato aplikace k webové aplikaci, odkaz tooan ASP.NET Core balíček je automaticky přidán toohello *hellodotnetcore.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-119">Since this app is a web application, a reference tooan ASP.NET Core package was automatically added toohello *hellodotnetcore.csproj* file.</span></span> <span data-ttu-id="ecc9f-120">číslo verze Hello hello balíčku je nastavena podle toohello vybrali framework.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-120">hello version number of hello package is set according toohello chosen framework.</span></span> <span data-ttu-id="ecc9f-121">V tomto příkladu odkazuje ASP.NET Core verze 1.1.2, protože se používá .NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-121">This example is referencing ASP.NET Core version 1.1.2 because .NET Core 1.1 is used.</span></span>

## <a name="build-and-test-hello-application-locally"></a><span data-ttu-id="ecc9f-122">Vytvoření a testování aplikace hello místně</span><span class="sxs-lookup"><span data-stu-id="ecc9f-122">Build and test hello application locally</span></span> ##

<span data-ttu-id="ecc9f-123">Můžete sestavit a spustit aplikaci .NET Core s hello `dotnet restore` příkazu s parametrem hello `dotnet run` příkaz, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="ecc9f-123">You can build and run your .NET Core app with hello `dotnet restore` command followed by hello `dotnet run` command, as shown here:</span></span>

```
dotnet restore
dotnet run
```


<span data-ttu-id="ecc9f-124">Při spuštění aplikace hello, zobrazí se zpráva s oznámením, že aplikace hello naslouchá tooincoming žádosti na port.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-124">When hello application starts, it displays a message indicating hello app is listening tooincoming requests at a port.</span></span> 

```bash
Hosting environment: Production
Content root path: C:\hellodotnetcore
Now listening on: http://localhost:5000
Application started. Press Ctrl+C tooshut down.
```

<span data-ttu-id="ecc9f-125">Testování procházením příliš`http://localhost:5000/` s prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-125">Test it by browsing too`http://localhost:5000/` with your browser.</span></span> <span data-ttu-id="ecc9f-126">Pokud se vše funguje bez problémů, se zobrazí "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="ecc9f-126">If everything works fine, you see "Hello World!"</span></span> <span data-ttu-id="ecc9f-127">jako text hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-127">as hello result text.</span></span>

![Testování pomocí prohlížeče][7]

## <a name="create-a-net-core-app-in-hello-azure-portal"></a><span data-ttu-id="ecc9f-129">Vytvořit aplikaci .NET Core hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ecc9f-129">Create a .NET Core app in hello Azure Portal</span></span> ##

<span data-ttu-id="ecc9f-130">Je třeba nejprve toocreate prázdnou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-130">First you need toocreate an empty web app.</span></span> <span data-ttu-id="ecc9f-131">Přihlaste se toohello [portál Azure](https://portal.azure.com/) a vytvořte novou [webové aplikace v systému Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).</span><span class="sxs-lookup"><span data-stu-id="ecc9f-131">Log in toohello [Azure portal](https://portal.azure.com/) and create a new [Web App on Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).</span></span>

![Vytváření webové aplikace][1]

<span data-ttu-id="ecc9f-133">Když hello **vytvořit** otevře se stránka, obsahují podrobné informace o vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="ecc9f-133">When hello **Create** page opens, provide details about your web app:</span></span>

![Výběr .NET Core runtime zásobníku][2]

<span data-ttu-id="ecc9f-135">Použití hello následující tabulky jako průvodce toofill out hello **vytvořit** stránky a pak vyberte **OK** a **vytvořit** toocreate hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-135">Use hello following table as a guide toofill out hello **Create** page, then select **OK** and **Create** toocreate hello app.</span></span>

| <span data-ttu-id="ecc9f-136">Nastavení</span><span class="sxs-lookup"><span data-stu-id="ecc9f-136">Setting</span></span>      | <span data-ttu-id="ecc9f-137">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="ecc9f-137">Suggested value</span></span>  | <span data-ttu-id="ecc9f-138">Popis</span><span class="sxs-lookup"><span data-stu-id="ecc9f-138">Description</span></span>                                        |
| ------------ | ---------------- | -------------------------------------------------- |
| <span data-ttu-id="ecc9f-139">Název aplikace.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-139">App name</span></span> | <span data-ttu-id="ecc9f-140">hellodotnetcore</span><span class="sxs-lookup"><span data-stu-id="ecc9f-140">hellodotnetcore</span></span>  | <span data-ttu-id="ecc9f-141">Hello název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-141">hello name of your app.</span></span> <span data-ttu-id="ecc9f-142">Tento název musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-142">This name must be unique.</span></span> |
| <span data-ttu-id="ecc9f-143">Předplatné</span><span class="sxs-lookup"><span data-stu-id="ecc9f-143">Subscription</span></span> | <span data-ttu-id="ecc9f-144">Vyberte stávající předplatné</span><span class="sxs-lookup"><span data-stu-id="ecc9f-144">Choose an existing subscription</span></span> | <span data-ttu-id="ecc9f-145">Hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-145">hello Azure subscription.</span></span> |
| <span data-ttu-id="ecc9f-146">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="ecc9f-146">Resource Group</span></span> | <span data-ttu-id="ecc9f-147">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="ecc9f-147">myResourceGroup</span></span> |  <span data-ttu-id="ecc9f-148">Hello prostředků Azure skupiny toocontain hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-148">hello Azure resource group toocontain hello web app.</span></span> |
| <span data-ttu-id="ecc9f-149">Plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="ecc9f-149">App Service Plan</span></span> | <span data-ttu-id="ecc9f-150">Název existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="ecc9f-150">Existing App Service Plan name</span></span> |  <span data-ttu-id="ecc9f-151">Hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-151">hello App Service plan.</span></span>  |
| <span data-ttu-id="ecc9f-152">Konfiguraci kontejneru</span><span class="sxs-lookup"><span data-stu-id="ecc9f-152">Configure Container</span></span> | <span data-ttu-id="ecc9f-153">.NET core 1.1</span><span class="sxs-lookup"><span data-stu-id="ecc9f-153">.NET Core 1.1</span></span> | <span data-ttu-id="ecc9f-154">typ kontejneru pro tuto webovou aplikaci Hello: integrované, Docker nebo privátní registru.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-154">hello type of container for this web app: Built-in, Docker, or Private registry.</span></span> |
| <span data-ttu-id="ecc9f-155">Zdroj bitové kopie</span><span class="sxs-lookup"><span data-stu-id="ecc9f-155">Image source</span></span>  | <span data-ttu-id="ecc9f-156">Integrované</span><span class="sxs-lookup"><span data-stu-id="ecc9f-156">Built-in</span></span>  |  <span data-ttu-id="ecc9f-157">Hello zdroj obrázku hello.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-157">hello source of hello image.</span></span> |
| <span data-ttu-id="ecc9f-158">Modul runtime zásobníku</span><span class="sxs-lookup"><span data-stu-id="ecc9f-158">Runtime Stack</span></span>  | <span data-ttu-id="ecc9f-159">.NET core 1.1</span><span class="sxs-lookup"><span data-stu-id="ecc9f-159">.NET Core 1.1</span></span>  | <span data-ttu-id="ecc9f-160">Hello runtime zásobníku a verze.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-160">hello runtime stack and version.</span></span>  |

## <a name="deploy-your-application-via-git"></a><span data-ttu-id="ecc9f-161">Nasazení aplikace prostřednictvím Git</span><span class="sxs-lookup"><span data-stu-id="ecc9f-161">Deploy your application via Git</span></span> ##

<span data-ttu-id="ecc9f-162">V systému Linux pomocí Git toodeploy hello .NET Core aplikace tooAzure webové aplikace App Service.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-162">Use Git toodeploy hello .NET Core application tooAzure App Service Web App on Linux.</span></span>

<span data-ttu-id="ecc9f-163">Hello novou webovou aplikaci Azure je již nakonfigurován nasazení Git.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-163">hello new Azure web app already has Git deployment configured.</span></span> <span data-ttu-id="ecc9f-164">Adresa URL nasazení Git hello zjistíte tak, že přejdete toohello následující adresu URL po vložení název vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="ecc9f-164">You will find hello Git deployment URL by navigating toohello following URL after inserting your web app name:</span></span>

```https://{your web app name}.scm.azurewebsites.net/api/scm/info```

<span data-ttu-id="ecc9f-165">Hello adresy URL pro Git má hello následující formulář založený na název vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="ecc9f-165">hello Git URL has hello following form based on your web app name:</span></span>

```https://{your web app name}.scm.azurewebsites.net/{your web app name}.git```

<span data-ttu-id="ecc9f-166">Spusťte následující příkazy toodeploy hello místních aplikací tooyour webové aplikace Azure hello:</span><span class="sxs-lookup"><span data-stu-id="ecc9f-166">Run hello following commands toodeploy hello local application tooyour Azure web app:</span></span> 
 
```bash
git init
git remote add azure <Git deployment URL from above>
git add *.csproj *.cs
git commit -m "Initial deployment commit"
git push azure master
```

<span data-ttu-id="ecc9f-167">Nepotřebujete toopush všechny soubory pod *bin /* nebo *obj /* adresáře aplikace je integrovaná v cloudu hello při hello aplikace zdrojové soubory jsou nabídnutých tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-167">You don't need toopush any files under *bin/* or *obj/* directories because your application is built in hello cloud when hello application's source files are pushed tooAzure.</span></span> <span data-ttu-id="ecc9f-168">Po dokončení procesu sestavení hello binární soubory se zkopírují do adresáře aplikace hello v */home/server/wwwroot/*.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-168">After hello build process is complete, binary files are copied into hello application's directory at */home/site/wwwroot/*.</span></span>

<span data-ttu-id="ecc9f-169">Potvrďte, že operace vzdálené nasazení hello sestavy úspěch.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-169">Confirm that hello remote deployment operations report success.</span></span> <span data-ttu-id="ecc9f-170">Nabízená operace může trvat od balíčku řešení a proces spuštění v cloudu hello sestavení.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-170">Push operations may take a while since package resolution and build process run in hello cloud.</span></span> <span data-ttu-id="ecc9f-171">Zobrazí se několik stavové zprávy, včetně těch, které jsou s oznámením, že soubory byly zkopírovány.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-171">You will see several status messages, including ones stating that files have been copied.</span></span> <span data-ttu-id="ecc9f-172">výstup Hello by měl vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="ecc9f-172">hello output should look similar toohello following:</span></span>

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
toohttps://hellodotnetcore.scm.azurewebsites.net/
 * [new branch]           master -> master

```

<span data-ttu-id="ecc9f-173">Po dokončení nasazení hello restartování webové aplikace pro hello nasazení tootake vliv.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-173">Once hello deployment has completed, restart your web app for hello deployment tootake effect.</span></span> <span data-ttu-id="ecc9f-174">toodo, přejděte toohello portál Azure a přejděte toohello **přehled** stránku vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-174">toodo this, go toohello Azure portal and navigate toohello **Overview** page of your web app.</span></span> <span data-ttu-id="ecc9f-175">Vyberte hello **restartujte** tlačítka na stránku hello.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-175">Select hello **Restart** button in hello page.</span></span> <span data-ttu-id="ecc9f-176">Když se zobrazí místní okno, vyberte **Ano** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="ecc9f-176">When a popup window shows up, select **Yes** tooconfirm.</span></span> <span data-ttu-id="ecc9f-177">Potom můžete procházet vaší webové aplikaci, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="ecc9f-177">You can then browse your web app, as shown here:</span></span>

![Procházení .NET Core aplikace nasazená tooAzure služby App Service v systému Linux][10]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="ecc9f-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ecc9f-179">Next steps</span></span>
* [<span data-ttu-id="ecc9f-180">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="ecc9f-180">Azure App Service Web App on Linux FAQ</span></span>](./app-service-linux-faq.md)

[1]: ./media/app-service-linux-using-dotnetcore/top-level-create.png
[2]: ./media/app-service-linux-using-dotnetcore/dotnet-new-webapp.png
[7]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-local.png
[10]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-azure.png
