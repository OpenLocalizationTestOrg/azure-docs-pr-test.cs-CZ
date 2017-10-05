---
title: "Vytvoření webové aplikace ASP.NET Core v kódu Visual Studio"
description: "Tento kurz ukazuje, jak vytvořit webovou aplikaci ASP.NET Core pomocí Visual Studio Code."
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
ms.openlocfilehash: 46e3852dc84265de41bb358f482dec06608e7efa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a><span data-ttu-id="7374e-103">Vytvoření webové aplikace ASP.NET Core v kódu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7374e-103">Create an ASP.NET Core web app in Visual Studio Code</span></span>
## <a name="overview"></a><span data-ttu-id="7374e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="7374e-104">Overview</span></span>
<span data-ttu-id="7374e-105">V tomto kurzu se dozvíte, jak vytvořit ASP.NET Core webovou aplikaci pomocí [Visual Studio Code (kód VS)](http://code.visualstudio.com//Docs/whyvscode) a nasadit ji [Azure App Service](../app-service/app-service-value-prop-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="7374e-105">This tutorial shows you how to create an ASP.NET Core web app using [Visual Studio Code (VS Code)](http://code.visualstudio.com//Docs/whyvscode) and deploy it to [Azure App Service](../app-service/app-service-value-prop-what-is.md).</span></span> 

> [!NOTE]
> <span data-ttu-id="7374e-106">I když se tento článek týká webových aplikací, platí také pro aplikace API a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="7374e-106">Although this article refers to web apps, it also applies to API apps and mobile apps.</span></span> 
> 
> 

<span data-ttu-id="7374e-107">ASP.NET Core je důležité změnit některé technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7374e-107">ASP.NET Core is a significant redesign of ASP.NET.</span></span> <span data-ttu-id="7374e-108">ASP.NET Core je nové open source a napříč platformami architektura pro vytváření webových moderní cloudové aplikace pomocí rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="7374e-108">ASP.NET Core is a new open-source and cross-platform framework for building modern cloud-based web apps using .NET.</span></span> <span data-ttu-id="7374e-109">Další informace najdete v tématu [Úvod do ASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html).</span><span class="sxs-lookup"><span data-stu-id="7374e-109">For more information, see [Introduction to ASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html).</span></span> <span data-ttu-id="7374e-110">Informace o službě Azure App Service webových aplikací najdete v tématu [webových aplikací – přehled](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7374e-110">For information about Azure App Service web apps, see [Web Apps Overview](app-service-web-overview.md).</span></span>

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a><span data-ttu-id="7374e-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7374e-111">Prerequisites</span></span>
* <span data-ttu-id="7374e-112">Nainstalujte [VS Code](http://code.visualstudio.com/Docs/setup).</span><span class="sxs-lookup"><span data-stu-id="7374e-112">Install [VS Code](http://code.visualstudio.com/Docs/setup).</span></span>
* <span data-ttu-id="7374e-113">Nainstalovat Git – můžete ho nainstalovat pomocí kteréhokoli z těchto umístění: [Chocolatey](https://chocolatey.org/packages/git) nebo [git scm.com](http://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="7374e-113">Install Git - You can install it from either of these locations: [Chocolatey](https://chocolatey.org/packages/git) or [git-scm.com](http://git-scm.com/downloads).</span></span> <span data-ttu-id="7374e-114">Pokud jste ještě Git, vyberte možnost [git scm.com](http://git-scm.com/downloads) a vyberte možnost **použití Git z příkazového řádku Windows**.</span><span class="sxs-lookup"><span data-stu-id="7374e-114">If you are new to Git, choose [git-scm.com](http://git-scm.com/downloads) and select the option to **Use Git from the Windows Command Prompt**.</span></span> <span data-ttu-id="7374e-115">Po instalaci Git také budete muset nastavit Git uživatelské jméno a e-mailu, to je nutné později v tomto kurzu (při provádění potvrzení z VS Code).</span><span class="sxs-lookup"><span data-stu-id="7374e-115">Once you install Git, you'll also need to set the Git user name and email as it's required later in the tutorial (when performing a commit from VS Code).</span></span>  

## <a name="install-aspnet-core"></a><span data-ttu-id="7374e-116">Instalace technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7374e-116">Install ASP.NET Core</span></span>
<span data-ttu-id="7374e-117">ASP.NET Core je Štíhlá zásobníku .NET pro tvorbu moderní cloudové a webové aplikace, které běží na OS X, Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="7374e-117">ASP.NET Core is a lean .NET stack for building modern cloud and web apps that run on OS X, Linux, and Windows.</span></span> <span data-ttu-id="7374e-118">Byla postavena od základů zajistit představuje optimalizované vývoj rozhraní pro aplikace, které jsou nasazeny v cloudu nebo spustit místně.</span><span class="sxs-lookup"><span data-stu-id="7374e-118">It has been built from the ground up to provide an optimized development framework for apps that are either deployed to the cloud or run on-premises.</span></span> <span data-ttu-id="7374e-119">Se skládá z modulární komponenty s minimální režií, takže zachovat flexibilitu při vytváření řešení.</span><span class="sxs-lookup"><span data-stu-id="7374e-119">It consists of modular components with minimal overhead, so you retain flexibility while constructing your solutions.</span></span>

<span data-ttu-id="7374e-120">V tomto kurzu je navržená tak, abyste mohli začít vytváření aplikací s nejnovější verze vývoj ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7374e-120">This tutorial is designed to get you started building applications with the latest development versions of ASP.NET Core.</span></span> <span data-ttu-id="7374e-121">Následující pokyny jsou specifické pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="7374e-121">The following instructions are specific to Windows.</span></span> <span data-ttu-id="7374e-122">Pokyny k instalaci na OS X, Linux a Windows, najdete v části [Začínáme s ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started).</span><span class="sxs-lookup"><span data-stu-id="7374e-122">For installation instructions on OS X, Linux, and Windows, see [Getting Started with ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started).</span></span> 


> [!NOTE]
> <span data-ttu-id="7374e-123">Podrobné pokyny k instalaci pro OS X, Linux a Windows, najdete v části [instalaci ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).</span><span class="sxs-lookup"><span data-stu-id="7374e-123">For more detailed installation instructions for OS X, Linux, and Windows, see [Installing ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).</span></span> 
> 
> 

## <a name="create-the-web-app"></a><span data-ttu-id="7374e-124">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="7374e-124">Create the web app</span></span>
<span data-ttu-id="7374e-125">V této části se dozvíte, jak chcete vygenerovat novou aplikaci ASP.NET webovou aplikaci pomocí nástroje příkazového řádku .NET.</span><span class="sxs-lookup"><span data-stu-id="7374e-125">This section shows you how to scaffold a new app ASP.NET web app using the .NET CLI tool.</span></span> 

1. <span data-ttu-id="7374e-126">Zadejte na příkazovém řádku k vytvoření složky projektu a vygenerovat uživatelské rozhraní aplikace.</span><span class="sxs-lookup"><span data-stu-id="7374e-126">Enter the following at the command prompt to create the project folder and scaffold the app.</span></span>
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![rozhraní příkazového řádku - ASP.NET Core generátor DotNet.](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. <span data-ttu-id="7374e-128">Chcete-li obnovit potřebné balíčky NuGet, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7374e-128">To restore the necessary NuGet packages, run the following command:</span></span>
   
    ```terminal
    dotnet restore
    ```

## <a name="run-the-web-app-locally"></a><span data-ttu-id="7374e-129">Místní spuštění webové aplikace</span><span class="sxs-lookup"><span data-stu-id="7374e-129">Run the web app locally</span></span>
<span data-ttu-id="7374e-130">Teď, když jste vytvořili webovou aplikaci a načíst všechny balíčky NuGet pro aplikaci, můžete spustit webovou aplikaci místně.</span><span class="sxs-lookup"><span data-stu-id="7374e-130">Now that you have created the web app and retrieved all the NuGet packages for the app, you can run the web app locally.</span></span>

1. <span data-ttu-id="7374e-131">Spuštění aplikace ( `dotnet run` příkaz bude sestavení aplikace, když je zastaralý):</span><span class="sxs-lookup"><span data-stu-id="7374e-131">Run the app  (the `dotnet run` command will build the app when it's out of date):</span></span>
    ```terminal
    dotnet run
    ```
2. <span data-ttu-id="7374e-132">Otevřete prohlížeč a přejděte na následující adresu URL.</span><span class="sxs-lookup"><span data-stu-id="7374e-132">Open a browser and navigate to the following URL.</span></span>
   
    <span data-ttu-id="7374e-133">**http://localhost: 5000**</span><span class="sxs-lookup"><span data-stu-id="7374e-133">**http://localhost:5000**</span></span>
   
    <span data-ttu-id="7374e-134">Takto se zobrazí stránka výchozí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7374e-134">The default page of the web app will appear as follows.</span></span>
   
    ![Místní webové aplikace v prohlížeči](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. <span data-ttu-id="7374e-136">Zavřete webový prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="7374e-136">Close your browser.</span></span> <span data-ttu-id="7374e-137">V **příkazové okno**, stiskněte klávesu **Ctrl + C** vypnutí aplikace a zavřít **příkazové okno**.</span><span class="sxs-lookup"><span data-stu-id="7374e-137">In the **Command Window**, press **Ctrl+C** to shut down the application and close the **Command Window**.</span></span> 

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="7374e-138">Vytvořit webovou aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7374e-138">Create a web app in the Azure Portal</span></span>
<span data-ttu-id="7374e-139">Následující postup vás provede vytvořením webové aplikace na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7374e-139">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="7374e-140">Přihlaste se k [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7374e-140">Log in to the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7374e-141">Klikněte na tlačítko **nový** levé v horní části portálu.</span><span class="sxs-lookup"><span data-stu-id="7374e-141">Click **NEW** at the top left of the Portal.</span></span>
3. <span data-ttu-id="7374e-142">Klikněte na tlačítko **webové aplikace > Webová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7374e-142">Click **Web Apps > Web App**.</span></span>
   
    ![Azure nové webové aplikace](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. <span data-ttu-id="7374e-144">Zadejte hodnotu pro **název**, jako například **SampleWebAppDemo**.</span><span class="sxs-lookup"><span data-stu-id="7374e-144">Enter a value for **Name**, such as **SampleWebAppDemo**.</span></span> <span data-ttu-id="7374e-145">Všimněte si, že tento název musí být jedinečný a portálu vynutí, při pokusu o zadání názvu.</span><span class="sxs-lookup"><span data-stu-id="7374e-145">Note that this name needs to be unique, and the portal will enforce that when you attempt to enter the name.</span></span> <span data-ttu-id="7374e-146">Proto pokud vyberete, zadejte jinou hodnotu, budete muset nahraďte tuto hodnotu pro každý výskyt **SampleWebAppDemo** , které vidíte v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="7374e-146">Therefore, if you select a enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span> 
5. <span data-ttu-id="7374e-147">Vyberte existující **plán služby App Service** nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="7374e-147">Select an existing **App Service Plan** or create a new one.</span></span> <span data-ttu-id="7374e-148">Pokud vytvoříte nový plán, vyberte cenovou úroveň, umístění a další možnosti.</span><span class="sxs-lookup"><span data-stu-id="7374e-148">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="7374e-149">Další informace o plánech služby App Service najdete v článku [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7374e-149">For more information on App Service plans, see the article, [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
   
    ![Azure nové okno webové aplikace](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. <span data-ttu-id="7374e-151">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7374e-151">Click **Create**.</span></span>
   
    ![okně webové aplikace](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="7374e-153">Povolení publikování Git pro nové webové aplikace</span><span class="sxs-lookup"><span data-stu-id="7374e-153">Enable Git publishing for the new web app</span></span>
<span data-ttu-id="7374e-154">Git je systém správy distribuovaných verzí, který můžete použít k nasazení webové aplikace služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="7374e-154">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="7374e-155">Kód napsaný pro webovou aplikaci uložíte do místního úložiště Git a kód budete nasazovat do Azure nuceným doručením (push) do vzdáleného úložiště.</span><span class="sxs-lookup"><span data-stu-id="7374e-155">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>   

1. <span data-ttu-id="7374e-156">Přihlaste se [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7374e-156">Log into the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7374e-157">Klikněte na **Browse** (Procházet).</span><span class="sxs-lookup"><span data-stu-id="7374e-157">Click **Browse**.</span></span>
3. <span data-ttu-id="7374e-158">Klikněte na tlačítko **webové aplikace** zobrazíte seznam webové aplikace přidružené k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="7374e-158">Click **Web Apps** to view a list of the web apps associated with your Azure subscription.</span></span>
4. <span data-ttu-id="7374e-159">Vyberte webovou aplikaci, kterou jste vytvořili v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="7374e-159">Select the web app you created in this tutorial.</span></span>
5. <span data-ttu-id="7374e-160">V okně webové aplikace klikněte na **nastavení** > **průběžné nasazování**.</span><span class="sxs-lookup"><span data-stu-id="7374e-160">In the web app blade, click **Settings** > **Continuous deployment**.</span></span> 
   
    ![Službě Azure web app hostitele](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. <span data-ttu-id="7374e-162">Klikněte na tlačítko **zvolit zdroj > místní úložiště Git**.</span><span class="sxs-lookup"><span data-stu-id="7374e-162">Click **Choose Source > Local Git Repository**.</span></span>
7. <span data-ttu-id="7374e-163">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7374e-163">Click **OK**.</span></span>
   
    ![Azure místní úložiště Git](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. <span data-ttu-id="7374e-165">Pokud jste dříve vytvořili přihlašovací údaje nasazení pro publikování webové aplikace nebo jiná aplikace služby App Service, nastavte je nyní:</span><span class="sxs-lookup"><span data-stu-id="7374e-165">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>
   
   * <span data-ttu-id="7374e-166">Klikněte na tlačítko **nastavení** > **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="7374e-166">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="7374e-167">**Nastavit přihlašovací údaje nasazení** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="7374e-167">The **Set deployment credentials** blade will be displayed.</span></span>
   * <span data-ttu-id="7374e-168">Vytvořte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="7374e-168">Create a user name and password.</span></span>  <span data-ttu-id="7374e-169">Toto heslo budete potřebovat později při nastavování Git.</span><span class="sxs-lookup"><span data-stu-id="7374e-169">You'll need this password later when setting up Git.</span></span>
   * <span data-ttu-id="7374e-170">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7374e-170">Click **Save**.</span></span>
9. <span data-ttu-id="7374e-171">V okně vaší webové aplikace, klikněte na tlačítko **Nastavení > vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="7374e-171">In your web app's blade, click **Settings > Properties**.</span></span> <span data-ttu-id="7374e-172">Vzdálené úložiště Git, které budete nasazovat na adresu URL se zobrazí v části **adresy URL pro GIT**.</span><span class="sxs-lookup"><span data-stu-id="7374e-172">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>
10. <span data-ttu-id="7374e-173">Kopírování **adresy URL pro GIT** hodnoty pro pozdější použití v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="7374e-173">Copy the **GIT URL** value for later use in the tutorial.</span></span>
    
    ![Adresa URL Azure Git](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="7374e-175">Publikování webové aplikace do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7374e-175">Publish your web app to Azure App Service</span></span>
<span data-ttu-id="7374e-176">V této části vytvoříte místní úložiště Git a posílejte nabízená oznámení z tohoto úložiště do Azure k nasazení vaší webové aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="7374e-176">In this section, you will create a local Git repository and push from that repository to Azure to deploy your web app to Azure.</span></span>

1. <span data-ttu-id="7374e-177">V produktu VS Code, vyberte **Git** možnost v levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="7374e-177">In VS Code, select the **Git** option in the left navigation bar.</span></span>
   
    ![Ikona Git v produktu VS Code](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. <span data-ttu-id="7374e-179">Vyberte **úložiště git inicializovat** a ujistěte se, pracovní prostor je pod git zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="7374e-179">Select **Initialize git repository** to make sure your workspace is under git source control.</span></span> 
   
    ![Inicializace Git](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. <span data-ttu-id="7374e-181">Otevřete příkazové okno a přejděte do adresáře vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7374e-181">Open the Command Window and change directories to the directory of your web app.</span></span> <span data-ttu-id="7374e-182">Potom zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7374e-182">Then, enter the following command:</span></span>
   
        git config core.autocrlf false
   
    <span data-ttu-id="7374e-183">Tento příkaz zabrání problém o textu, které se podílejí Line FEED zakončení a LF zakončení.</span><span class="sxs-lookup"><span data-stu-id="7374e-183">This command prevents an issue about text where CRLF endings and LF endings are involved.</span></span>
4. <span data-ttu-id="7374e-184">V produktu VS Code přidat zprávu o potvrzení a klikněte na **potvrzení všechny** ikona zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="7374e-184">In VS Code, add a commit message and click the **Commit All** check icon.</span></span>
   
    ![Git potvrdit všechny](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. <span data-ttu-id="7374e-186">Po dokončení zpracování Git, se zobrazí, že neexistují žádné soubory uvedené v okně Git v části **změny**.</span><span class="sxs-lookup"><span data-stu-id="7374e-186">After Git has completed processing, you'll see that there are no files listed in the Git window under **Changes**.</span></span> 
   
    ![Git žádné změny](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. <span data-ttu-id="7374e-188">Změnit zpátky na příkazové okno kde příkazovém řádku odkazuje na adresář, kde se nachází vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7374e-188">Change back to the Command Window where the command prompt points to the directory where your web app is located.</span></span>
7. <span data-ttu-id="7374e-189">Vytvořte vzdálený odkaz pro vkládání aktualizací do webové aplikace pomocí adresy URL Git (ukončuje v ".git"), který jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="7374e-189">Create a remote reference for pushing updates to your web app by using the Git URL (ending in ".git") that you copied earlier.</span></span>
   
        git remote add azure [URL for remote repository]
8. <span data-ttu-id="7374e-190">Nakonfigurujte si Git svoje přihlašovací údaje uložit místně, tak, aby se automaticky připojí se k vaší nabízené příkazy vygenerovat z VS Code.</span><span class="sxs-lookup"><span data-stu-id="7374e-190">Configure Git to save your credentials locally so that they will be automatically appended to your push commands generated from VS Code.</span></span>
   
        git config credential.helper store
9. <span data-ttu-id="7374e-191">Doručte změny do Azure tak, že zadáte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="7374e-191">Push your changes to Azure by entering the following command.</span></span> <span data-ttu-id="7374e-192">Po této úvodní nabízená instalace Azure budete moci provádět všechny příkazy nabízené z VS Code.</span><span class="sxs-lookup"><span data-stu-id="7374e-192">After this initial push to Azure, you will be able to do all the push commands from VS Code.</span></span> 
   
        git push -u azure master
   
    <span data-ttu-id="7374e-193">Zobrazí se výzva k zadání hesla, které jste vytvořili dříve v Azure.</span><span class="sxs-lookup"><span data-stu-id="7374e-193">You are prompted for the password you created earlier in Azure.</span></span> <span data-ttu-id="7374e-194">**Poznámka: Hesla nezobrazí.**</span><span class="sxs-lookup"><span data-stu-id="7374e-194">**Note: Your password will not be visible.**</span></span>
   
    <span data-ttu-id="7374e-195">Výstup z výše uvedeném příkazu končí zprávu, že nasazení je úspěšné.</span><span class="sxs-lookup"><span data-stu-id="7374e-195">The output from the above command ends with a message that deployment is successful.</span></span>
   
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> <span data-ttu-id="7374e-196">Pokud provedete změny aplikace, můžete znovu publikovat přímo v produktu VS Code pomocí integrované funkce Git tak, že vyberete **potvrdit všechny** následovaný **Push** možnost.</span><span class="sxs-lookup"><span data-stu-id="7374e-196">If you make changes to your app, you can republish directly in VS Code using the built-in Git functionality by selecting the **Commit All** option followed by the **Push** option.</span></span> <span data-ttu-id="7374e-197">Zjistíte, **Push** možnost k dispozici v rozevírací nabídce vedle položky **potvrdit všechny** a **aktualizovat** tlačítka.</span><span class="sxs-lookup"><span data-stu-id="7374e-197">You will find the **Push** option available in the drop-down menu next to the **Commit All** and **Refresh** buttons.</span></span>
> 
> 

<span data-ttu-id="7374e-198">Pokud budete potřebovat spolupracovat na projektu, měli byste zvážit, když zavedete Githubu mezi vkládání do Azure.</span><span class="sxs-lookup"><span data-stu-id="7374e-198">If you need to collaborate on a project, you should consider pushing to GitHub in between pushing to Azure.</span></span>

## <a name="run-the-app-in-azure"></a><span data-ttu-id="7374e-199">Spusťte aplikaci v Azure</span><span class="sxs-lookup"><span data-stu-id="7374e-199">Run the app in Azure</span></span>
<span data-ttu-id="7374e-200">Teď, když jste nasadili vaší webové aplikace, umožňuje spuštění aplikace při hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="7374e-200">Now that you have deployed your web app, let's run the app while hosted in Azure.</span></span> 

<span data-ttu-id="7374e-201">Tento krok můžete provést dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="7374e-201">This can be done in two ways:</span></span>

* <span data-ttu-id="7374e-202">Otevřete prohlížeč a zadejte název webové aplikace následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="7374e-202">Open a browser and enter the name of your web app as follows.</span></span>   
  
        http://SampleWebAppDemo.azurewebsites.net
* <span data-ttu-id="7374e-203">Na portálu Azure, vyhledejte okně webové aplikace pro vaši webovou aplikaci a klikněte na tlačítko **Procházet** zobrazení vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="7374e-203">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app</span></span> 
* <span data-ttu-id="7374e-204">ve výchozím prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="7374e-204">in your default browser.</span></span>

![Webové aplikace Azure](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a><span data-ttu-id="7374e-206">Souhrn</span><span class="sxs-lookup"><span data-stu-id="7374e-206">Summary</span></span>
<span data-ttu-id="7374e-207">V tomto kurzu jste zjistili, jak vytvořit webovou aplikaci v produktu VS Code a nasadíte ho do Azure.</span><span class="sxs-lookup"><span data-stu-id="7374e-207">In this tutorial, you learned how to create a web app in VS Code and deploy it to Azure.</span></span> <span data-ttu-id="7374e-208">Další informace o VS Code, najdete v článku [proč Visual Studio Code?](https://code.visualstudio.com/Docs/)</span><span class="sxs-lookup"><span data-stu-id="7374e-208">For more information about VS Code, see the article, [Why Visual Studio Code?](https://code.visualstudio.com/Docs/)</span></span> <span data-ttu-id="7374e-209">Informace o webové aplikace služby App Service najdete v tématu [webových aplikací – přehled](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7374e-209">For information about App Service web apps, see [Web Apps Overview](app-service-web-overview.md).</span></span> 

