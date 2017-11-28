---
title: "aaaCreate webové aplikace ASP.NET Core v kódu Visual Studio"
description: "Tento kurz ukazuje, jak toocreate ASP.NET Core webové aplikaci pomocí Visual Studio Code."
services: app-service\web
documentationcenter: .net
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 877bff08-9ef7-405a-a1ca-1194f33c55f2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: cephalin
ms.openlocfilehash: 1c18c94984d71e88d2a5b792d68cb1c81e4a96d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a><span data-ttu-id="de121-103">Vytvoření webové aplikace ASP.NET Core v kódu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de121-103">Create an ASP.NET Core web app in Visual Studio Code</span></span>
## <a name="overview"></a><span data-ttu-id="de121-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="de121-104">Overview</span></span>
<span data-ttu-id="de121-105">Tento kurz ukazuje, jak toocreate ASP.NET Core webové aplikace pomocí [Visual Studio Code (kód VS)](http://code.visualstudio.com//Docs/whyvscode) a nasaďte ho příliš[Azure App Service](../app-service/app-service-value-prop-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="de121-105">This tutorial shows you how toocreate an ASP.NET Core web app using [Visual Studio Code (VS Code)](http://code.visualstudio.com//Docs/whyvscode) and deploy it too[Azure App Service](../app-service/app-service-value-prop-what-is.md).</span></span> 

> [!NOTE]
> <span data-ttu-id="de121-106">Přestože tento článek se týká tooweb aplikací, platí také tooAPI a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="de121-106">Although this article refers tooweb apps, it also applies tooAPI apps and mobile apps.</span></span> 
> 
> 

<span data-ttu-id="de121-107">ASP.NET Core je důležité změnit některé technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="de121-107">ASP.NET Core is a significant redesign of ASP.NET.</span></span> <span data-ttu-id="de121-108">ASP.NET Core je nové open source a napříč platformami architektura pro vytváření webových moderní cloudové aplikace pomocí rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="de121-108">ASP.NET Core is a new open-source and cross-platform framework for building modern cloud-based web apps using .NET.</span></span> <span data-ttu-id="de121-109">Další informace najdete v tématu [Úvod tooASP.NET základní](http://docs.asp.net/latest/conceptual-overview/aspnet.html).</span><span class="sxs-lookup"><span data-stu-id="de121-109">For more information, see [Introduction tooASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html).</span></span> <span data-ttu-id="de121-110">Informace o službě Azure App Service webových aplikací najdete v tématu [webových aplikací – přehled](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="de121-110">For information about Azure App Service web apps, see [Web Apps Overview](app-service-web-overview.md).</span></span>

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a><span data-ttu-id="de121-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="de121-111">Prerequisites</span></span>
* <span data-ttu-id="de121-112">Nainstalujte [VS Code](http://code.visualstudio.com/Docs/setup).</span><span class="sxs-lookup"><span data-stu-id="de121-112">Install [VS Code](http://code.visualstudio.com/Docs/setup).</span></span>
* <span data-ttu-id="de121-113">Nainstalovat Git – můžete ho nainstalovat pomocí kteréhokoli z těchto umístění: [Chocolatey](https://chocolatey.org/packages/git) nebo [git scm.com](http://git-scm.com/downloads). Pokud jste nový tooGit, vyberte možnost [git scm.com](http://git-scm.com/downloads) a vyberte možnost hello příliš**použití Git z příkazového řádku Windows hello**.</span><span class="sxs-lookup"><span data-stu-id="de121-113">Install Git - You can install it from either of these locations: [Chocolatey](https://chocolatey.org/packages/git) or [git-scm.com](http://git-scm.com/downloads). If you are new tooGit, choose [git-scm.com](http://git-scm.com/downloads) and select hello option too**Use Git from hello Windows Command Prompt**.</span></span> <span data-ttu-id="de121-114">Po instalaci Git, budete také potřebovat tooset hello Git uživatelské jméno a e-mailu to je nutné později v kurzu hello (při provádění potvrzení z VS Code).</span><span class="sxs-lookup"><span data-stu-id="de121-114">Once you install Git, you'll also need tooset hello Git user name and email as it's required later in hello tutorial (when performing a commit from VS Code).</span></span>  

## <a name="install-aspnet-core"></a><span data-ttu-id="de121-115">Instalace technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="de121-115">Install ASP.NET Core</span></span>
<span data-ttu-id="de121-116">ASP.NET Core je Štíhlá zásobníku .NET pro tvorbu moderní cloudové a webové aplikace, které běží na OS X, Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="de121-116">ASP.NET Core is a lean .NET stack for building modern cloud and web apps that run on OS X, Linux, and Windows.</span></span> <span data-ttu-id="de121-117">Byla postavena z hello pozadí až tooprovide rozhraní pro vývoj optimalizované pro aplikace, které jsou buď nasazené toohello cloudové nebo místní spouštění.</span><span class="sxs-lookup"><span data-stu-id="de121-117">It has been built from hello ground up tooprovide an optimized development framework for apps that are either deployed toohello cloud or run on-premises.</span></span> <span data-ttu-id="de121-118">Se skládá z modulární komponenty s minimální režií, takže zachovat flexibilitu při vytváření řešení.</span><span class="sxs-lookup"><span data-stu-id="de121-118">It consists of modular components with minimal overhead, so you retain flexibility while constructing your solutions.</span></span>

<span data-ttu-id="de121-119">Tento kurz je určen navrženou tooget zahájeno vytváření aplikací s hello nejnovější vývoj verze ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="de121-119">This tutorial is designed tooget you started building applications with hello latest development versions of ASP.NET Core.</span></span> <span data-ttu-id="de121-120">Hello následující pokyny jsou konkrétní tooWindows.</span><span class="sxs-lookup"><span data-stu-id="de121-120">hello following instructions are specific tooWindows.</span></span> <span data-ttu-id="de121-121">Pokyny k instalaci na OS X, Linux a Windows, najdete v části [Začínáme s ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started).</span><span class="sxs-lookup"><span data-stu-id="de121-121">For installation instructions on OS X, Linux, and Windows, see [Getting Started with ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started).</span></span> 


> [!NOTE]
> <span data-ttu-id="de121-122">Podrobné pokyny k instalaci pro OS X, Linux a Windows, najdete v části [instalaci ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).</span><span class="sxs-lookup"><span data-stu-id="de121-122">For more detailed installation instructions for OS X, Linux, and Windows, see [Installing ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).</span></span> 
> 
> 

## <a name="create-hello-web-app"></a><span data-ttu-id="de121-123">Vytvoření webové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="de121-123">Create hello web app</span></span>
<span data-ttu-id="de121-124">Tato část uvádí, jak tooscaffold novou aplikaci ASP.NET webové aplikace pomocí nástroje příkazového řádku .NET hello.</span><span class="sxs-lookup"><span data-stu-id="de121-124">This section shows you how tooscaffold a new app ASP.NET web app using hello .NET CLI tool.</span></span> 

1. <span data-ttu-id="de121-125">Zadejte následující hello v hello příkazového řádku toocreate hello projektu složky a vygenerované uživatelské rozhraní hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="de121-125">Enter hello following at hello command prompt toocreate hello project folder and scaffold hello app.</span></span>
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![rozhraní příkazového řádku - ASP.NET Core generátor DotNet.](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. <span data-ttu-id="de121-127">toorestore hello potřebné balíčky NuGet, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="de121-127">toorestore hello necessary NuGet packages, run hello following command:</span></span>
   
    ```terminal
    dotnet restore
    ```

## <a name="run-hello-web-app-locally"></a><span data-ttu-id="de121-128">Místní spuštění webové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="de121-128">Run hello web app locally</span></span>
<span data-ttu-id="de121-129">Teď, když jste vytvořili hello webové aplikace a načíst všechny balíčky NuGet hello hello aplikace, můžete spustit hello webovou aplikaci místně.</span><span class="sxs-lookup"><span data-stu-id="de121-129">Now that you have created hello web app and retrieved all hello NuGet packages for hello app, you can run hello web app locally.</span></span>

1. <span data-ttu-id="de121-130">Spuštění aplikace hello (hello `dotnet run` příkaz bude sestavení aplikace hello, pokud je zastaralý):</span><span class="sxs-lookup"><span data-stu-id="de121-130">Run hello app  (hello `dotnet run` command will build hello app when it's out of date):</span></span>
    ```terminal
    dotnet run
    ```
2. <span data-ttu-id="de121-131">Otevřete prohlížeč a přejděte toohello následující adresu URL.</span><span class="sxs-lookup"><span data-stu-id="de121-131">Open a browser and navigate toohello following URL.</span></span>
   
    <span data-ttu-id="de121-132">**http://localhost: 5000**</span><span class="sxs-lookup"><span data-stu-id="de121-132">**http://localhost:5000**</span></span>
   
    <span data-ttu-id="de121-133">takto se zobrazí stránka výchozí Hello hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="de121-133">hello default page of hello web app will appear as follows.</span></span>
   
    ![Místní webové aplikace v prohlížeči](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. <span data-ttu-id="de121-135">Zavřete webový prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="de121-135">Close your browser.</span></span> <span data-ttu-id="de121-136">V hello **příkazové okno**, stiskněte klávesu **Ctrl + C** tooshut dolů hello aplikace a zavřít hello **příkazové okno**.</span><span class="sxs-lookup"><span data-stu-id="de121-136">In hello **Command Window**, press **Ctrl+C** tooshut down hello application and close hello **Command Window**.</span></span> 

## <a name="create-a-web-app-in-hello-azure-portal"></a><span data-ttu-id="de121-137">Vytvoření webové aplikace v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="de121-137">Create a web app in hello Azure Portal</span></span>
<span data-ttu-id="de121-138">Hello následující postup vás provede vytvořením webové aplikace v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="de121-138">hello following steps will guide you through creating a web app in hello Azure Portal.</span></span>

1. <span data-ttu-id="de121-139">Přihlaste se toohello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="de121-139">Log in toohello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="de121-140">Klikněte na tlačítko **nový** v hello levém horním rohu hello portálu.</span><span class="sxs-lookup"><span data-stu-id="de121-140">Click **NEW** at hello top left of hello Portal.</span></span>
3. <span data-ttu-id="de121-141">Klikněte na tlačítko **webové aplikace > Webová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="de121-141">Click **Web Apps > Web App**.</span></span>
   
    ![Azure nové webové aplikace](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. <span data-ttu-id="de121-143">Zadejte hodnotu pro **název**, jako například **SampleWebAppDemo**.</span><span class="sxs-lookup"><span data-stu-id="de121-143">Enter a value for **Name**, such as **SampleWebAppDemo**.</span></span> <span data-ttu-id="de121-144">Upozorňujeme, že tento název musí toobe jedinečný a hello portál vynutí, který chcete-li název hello tooenter.</span><span class="sxs-lookup"><span data-stu-id="de121-144">Note that this name needs toobe unique, and hello portal will enforce that when you attempt tooenter hello name.</span></span> <span data-ttu-id="de121-145">Proto pokud vyberete, zadejte jinou hodnotu, budete potřebovat toosubstitute, která hodnotu pro každý výskyt **SampleWebAppDemo** , které vidíte v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="de121-145">Therefore, if you select a enter a different value, you'll need toosubstitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span> 
5. <span data-ttu-id="de121-146">Vyberte existující **plán služby App Service** nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="de121-146">Select an existing **App Service Plan** or create a new one.</span></span> <span data-ttu-id="de121-147">Pokud vytvoříte nový plán, vyberte hello cenová úroveň, umístění a další možnosti.</span><span class="sxs-lookup"><span data-stu-id="de121-147">If you create a new plan, select hello pricing tier, location, and other options.</span></span> <span data-ttu-id="de121-148">Další informace o plánech služby App Service najdete v článku hello, [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="de121-148">For more information on App Service plans, see hello article, [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
   
    ![Azure nové okno webové aplikace](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. <span data-ttu-id="de121-150">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="de121-150">Click **Create**.</span></span>
   
    ![okně webové aplikace](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-hello-new-web-app"></a><span data-ttu-id="de121-152">Povolení publikování Git pro hello nové webové aplikace</span><span class="sxs-lookup"><span data-stu-id="de121-152">Enable Git publishing for hello new web app</span></span>
<span data-ttu-id="de121-153">Git je distribuovaný systém správy verzí, které můžete použít toodeploy webové aplikace služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="de121-153">Git is a distributed version control system that you can use toodeploy your Azure App Service web app.</span></span> <span data-ttu-id="de121-154">Budete ukládat hello kód napsaný pro webovou aplikaci v místní úložiště Git a tooAzure váš kód budete nasazovat vynucením tooa vzdáleného úložiště.</span><span class="sxs-lookup"><span data-stu-id="de121-154">You'll store hello code you write for your web app in a local Git repository, and you'll deploy your code tooAzure by pushing tooa remote repository.</span></span>   

1. <span data-ttu-id="de121-155">Přihlaste se k hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="de121-155">Log into hello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="de121-156">Klikněte na **Browse** (Procházet).</span><span class="sxs-lookup"><span data-stu-id="de121-156">Click **Browse**.</span></span>
3. <span data-ttu-id="de121-157">Klikněte na tlačítko **webové aplikace** tooview seznam hello webové aplikace, která je spojená s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="de121-157">Click **Web Apps** tooview a list of hello web apps associated with your Azure subscription.</span></span>
4. <span data-ttu-id="de121-158">Vyberte hello webovou aplikaci, kterou jste vytvořili v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="de121-158">Select hello web app you created in this tutorial.</span></span>
5. <span data-ttu-id="de121-159">V okně webové aplikace hello, klikněte na **nastavení** > **průběžné nasazování**.</span><span class="sxs-lookup"><span data-stu-id="de121-159">In hello web app blade, click **Settings** > **Continuous deployment**.</span></span> 
   
    ![Službě Azure web app hostitele](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. <span data-ttu-id="de121-161">Klikněte na tlačítko **zvolit zdroj > místní úložiště Git**.</span><span class="sxs-lookup"><span data-stu-id="de121-161">Click **Choose Source > Local Git Repository**.</span></span>
7. <span data-ttu-id="de121-162">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="de121-162">Click **OK**.</span></span>
   
    ![Azure místní úložiště Git](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. <span data-ttu-id="de121-164">Pokud jste dříve vytvořili přihlašovací údaje nasazení pro publikování webové aplikace nebo jiná aplikace služby App Service, nastavte je nyní:</span><span class="sxs-lookup"><span data-stu-id="de121-164">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>
   
   * <span data-ttu-id="de121-165">Klikněte na tlačítko **nastavení** > **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="de121-165">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="de121-166">Hello **nastavit přihlašovací údaje nasazení** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="de121-166">hello **Set deployment credentials** blade will be displayed.</span></span>
   * <span data-ttu-id="de121-167">Vytvořte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="de121-167">Create a user name and password.</span></span>  <span data-ttu-id="de121-168">Toto heslo budete potřebovat později při nastavování Git.</span><span class="sxs-lookup"><span data-stu-id="de121-168">You'll need this password later when setting up Git.</span></span>
   * <span data-ttu-id="de121-169">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="de121-169">Click **Save**.</span></span>
9. <span data-ttu-id="de121-170">V okně vaší webové aplikace, klikněte na tlačítko **Nastavení > vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="de121-170">In your web app's blade, click **Settings > Properties**.</span></span> <span data-ttu-id="de121-171">Adresa URL Hello hello vzdáleného úložiště Git, které budete nasazovat toois v části **adresy URL pro GIT**.</span><span class="sxs-lookup"><span data-stu-id="de121-171">hello URL of hello remote Git repository that you'll deploy toois shown under **GIT URL**.</span></span>
10. <span data-ttu-id="de121-172">Kopírování hello **adresy URL pro GIT** hodnoty pro pozdější použití v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="de121-172">Copy hello **GIT URL** value for later use in hello tutorial.</span></span>
    
    ![Adresa URL Azure Git](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-tooazure-app-service"></a><span data-ttu-id="de121-174">Publikování tooAzure vaší webové aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="de121-174">Publish your web app tooAzure App Service</span></span>
<span data-ttu-id="de121-175">V této části vytvoříte místní úložiště Git a nabízet z tohoto úložiště tooAzure toodeploy tooAzure vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="de121-175">In this section, you will create a local Git repository and push from that repository tooAzure toodeploy your web app tooAzure.</span></span>

1. <span data-ttu-id="de121-176">V produktu VS Code, vyberte hello **Git** možnost hello levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="de121-176">In VS Code, select hello **Git** option in hello left navigation bar.</span></span>
   
    ![Ikona Git v produktu VS Code](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. <span data-ttu-id="de121-178">Vyberte **úložiště git inicializovat** toomake, že pracovní prostor je pod git zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="de121-178">Select **Initialize git repository** toomake sure your workspace is under git source control.</span></span> 
   
    ![Inicializace Git](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. <span data-ttu-id="de121-180">Otevřete příkazové okno hello a změnit adresář toohello adresáře vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="de121-180">Open hello Command Window and change directories toohello directory of your web app.</span></span> <span data-ttu-id="de121-181">Potom zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="de121-181">Then, enter hello following command:</span></span>
   
        git config core.autocrlf false
   
    <span data-ttu-id="de121-182">Tento příkaz zabrání problém o textu, které se podílejí Line FEED zakončení a LF zakončení.</span><span class="sxs-lookup"><span data-stu-id="de121-182">This command prevents an issue about text where CRLF endings and LF endings are involved.</span></span>
4. <span data-ttu-id="de121-183">V produktu VS Code přidat zprávu o potvrzení a klikněte na hello **potvrzení všechny** ikona zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="de121-183">In VS Code, add a commit message and click hello **Commit All** check icon.</span></span>
   
    ![Git potvrdit všechny](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. <span data-ttu-id="de121-185">Po dokončení zpracování Git, se zobrazí, že nejsou žádné soubory uvedené v okně Git hello pod **změny**.</span><span class="sxs-lookup"><span data-stu-id="de121-185">After Git has completed processing, you'll see that there are no files listed in hello Git window under **Changes**.</span></span> 
   
    ![Git žádné změny](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. <span data-ttu-id="de121-187">Změňte back toohello příkazové okno kde hello příkazového řádku body toohello adresáře, kde se nachází vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="de121-187">Change back toohello Command Window where hello command prompt points toohello directory where your web app is located.</span></span>
7. <span data-ttu-id="de121-188">Vytvořte vzdálený odkaz pro vkládání aktualizace tooyour webové aplikace pomocí hello URL Git (ukončuje v ".git"), který jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="de121-188">Create a remote reference for pushing updates tooyour web app by using hello Git URL (ending in ".git") that you copied earlier.</span></span>
   
        git remote add azure [URL for remote repository]
8. <span data-ttu-id="de121-189">Nakonfigurujte Git toosave vaše přihlašovací údaje místně, takže budou automaticky připojením tooyour nabízené příkazy vygenerovat z VS Code.</span><span class="sxs-lookup"><span data-stu-id="de121-189">Configure Git toosave your credentials locally so that they will be automatically appended tooyour push commands generated from VS Code.</span></span>
   
        git config credential.helper store
9. <span data-ttu-id="de121-190">Vaše změny tooAzure push tak, že zadáte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="de121-190">Push your changes tooAzure by entering hello following command.</span></span> <span data-ttu-id="de121-191">Po této tooAzure počáteční nabízené budete moct toodo všechny nabízené hello příkazy z kódu VS.</span><span class="sxs-lookup"><span data-stu-id="de121-191">After this initial push tooAzure, you will be able toodo all hello push commands from VS Code.</span></span> 
   
        git push -u azure master
   
    <span data-ttu-id="de121-192">Zobrazí se výzva k hello hesla, kterou jste vytvořili dříve v Azure.</span><span class="sxs-lookup"><span data-stu-id="de121-192">You are prompted for hello password you created earlier in Azure.</span></span> <span data-ttu-id="de121-193">**Poznámka: Hesla nezobrazí.**</span><span class="sxs-lookup"><span data-stu-id="de121-193">**Note: Your password will not be visible.**</span></span>
   
    <span data-ttu-id="de121-194">výstup Hello hello výše příkaz končí zprávu, že nasazení je úspěšné.</span><span class="sxs-lookup"><span data-stu-id="de121-194">hello output from hello above command ends with a message that deployment is successful.</span></span>
   
        remote: Deployment successful.
        toohttps://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> <span data-ttu-id="de121-195">Pokud provedete změny tooyour aplikace, můžete znovu publikovat přímo v produktu VS Code pomocí integrované funkce Git hello výběrem hello **potvrdit všechny** následovaný hello **Push** možnost.</span><span class="sxs-lookup"><span data-stu-id="de121-195">If you make changes tooyour app, you can republish directly in VS Code using hello built-in Git functionality by selecting hello **Commit All** option followed by hello **Push** option.</span></span> <span data-ttu-id="de121-196">Zjistíte, hello **Push** možnost k dispozici v další toohello hello rozevírací nabídky **potvrdit všechny** a **aktualizovat** tlačítka.</span><span class="sxs-lookup"><span data-stu-id="de121-196">You will find hello **Push** option available in hello drop-down menu next toohello **Commit All** and **Refresh** buttons.</span></span>
> 
> 

<span data-ttu-id="de121-197">Pokud potřebujete toocollaborate na projektu, byste měli zvážit, když zavedete tooGitHub mezi vkládání tooAzure.</span><span class="sxs-lookup"><span data-stu-id="de121-197">If you need toocollaborate on a project, you should consider pushing tooGitHub in between pushing tooAzure.</span></span>

## <a name="run-hello-app-in-azure"></a><span data-ttu-id="de121-198">Spuštění aplikace hello v Azure</span><span class="sxs-lookup"><span data-stu-id="de121-198">Run hello app in Azure</span></span>
<span data-ttu-id="de121-199">Teď, když jste nasadili vaší webové aplikace, umožňuje spuštění aplikace hello při hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="de121-199">Now that you have deployed your web app, let's run hello app while hosted in Azure.</span></span> 

<span data-ttu-id="de121-200">Tento krok můžete provést dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="de121-200">This can be done in two ways:</span></span>

* <span data-ttu-id="de121-201">Otevřete prohlížeč a zadejte název webové aplikace hello následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="de121-201">Open a browser and enter hello name of your web app as follows.</span></span>   
  
        http://SampleWebAppDemo.azurewebsites.net
* <span data-ttu-id="de121-202">V hello portálu Azure, vyhledejte hello webové aplikace okno pro vaši webovou aplikaci a klikněte na tlačítko **Procházet** tooview vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="de121-202">In hello Azure Portal, locate hello web app blade for your web app, and click **Browse** tooview your app</span></span> 
* <span data-ttu-id="de121-203">ve výchozím prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="de121-203">in your default browser.</span></span>

![Webové aplikace Azure](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a><span data-ttu-id="de121-205">Souhrn</span><span class="sxs-lookup"><span data-stu-id="de121-205">Summary</span></span>
<span data-ttu-id="de121-206">V tomto kurzu jste se naučili jak toocreate webové aplikace v produktu VS Code a nasaďte ho tooAzure.</span><span class="sxs-lookup"><span data-stu-id="de121-206">In this tutorial, you learned how toocreate a web app in VS Code and deploy it tooAzure.</span></span> <span data-ttu-id="de121-207">Další informace o VS Code, najdete v článku hello, [proč Visual Studio Code?](https://code.visualstudio.com/Docs/)</span><span class="sxs-lookup"><span data-stu-id="de121-207">For more information about VS Code, see hello article, [Why Visual Studio Code?](https://code.visualstudio.com/Docs/)</span></span> <span data-ttu-id="de121-208">Informace o webové aplikace služby App Service najdete v tématu [webových aplikací – přehled](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="de121-208">For information about App Service web apps, see [Web Apps Overview](app-service-web-overview.md).</span></span> 

