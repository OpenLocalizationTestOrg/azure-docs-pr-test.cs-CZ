---
title: "Průběžné nasazování do Azure App Service | Microsoft Docs"
description: "Naučte se povolit průběžné nasazování do služby Azure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 7d978e623aaff25a9400090bd3ac18ed560d2ebf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="continuous-deployment-to-azure-app-service"></a><span data-ttu-id="25504-103">Průběžné nasazování do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="25504-103">Continuous Deployment to Azure App Service</span></span>
<span data-ttu-id="25504-104">Tento kurz ilustruje postup při konfiguraci pracovního postupu průběžného nasazování pro aplikaci služby [Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="25504-104">This tutorial shows you how to configure a continuous deployment workflow for your [Azure App Service] app.</span></span> <span data-ttu-id="25504-105">Služby App Service integrace s BitBucket, Githubu, a [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) umožňuje průběžné nasazování workflow, kde Azure vyžaduje nejnovější aktualizace z projektu publikována do některého z těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="25504-105">App Service integration with BitBucket, GitHub, and [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) enables a continuous deployment workflow where Azure pulls in the most recent updates from your project published to one of these services.</span></span> <span data-ttu-id="25504-106">Průběžné nasazování je skvělou možností pro projekty, u kterých se integruje více příspěvků nebo u kterých integrace probíhá často.</span><span class="sxs-lookup"><span data-stu-id="25504-106">Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated.</span></span>

<span data-ttu-id="25504-107">Chcete zjistit, jak nakonfigurovat průběžné nasazování ručně z úložiště v cloudu nejsou uvedené pomocí portálu Azure (například [GitLab](https://gitlab.com/)), najdete v části [nastavení průběžné nasazování pomocí ruční kroky](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).</span><span class="sxs-lookup"><span data-stu-id="25504-107">To find out how to configure continuous deployment manually from a cloud repository not listed by the Azure Portal (such as [GitLab](https://gitlab.com/)), see [Setting up continuous deployment using manual steps](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).</span></span>

## <span data-ttu-id="25504-108"><a name="overview"></a>Povolení průběžného nasazování</span><span class="sxs-lookup"><span data-stu-id="25504-108"><a name="overview"></a>Enable continuous deployment</span></span>
<span data-ttu-id="25504-109">Pokud chcete povolit průběžné nasazování, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="25504-109">To enable continuous deployment,</span></span>

1. <span data-ttu-id="25504-110">Publikujte obsah aplikace do úložiště, které se bude používat pro průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="25504-110">Publish your app content to the repository that will be used for continuous deployment.</span></span>  
    <span data-ttu-id="25504-111">Další informace o publikování projektu v těchto službách najdete v článcích [Vytvoření úložiště (GitHub)], [Vytvoření úložiště (BitBucket)] a [Začínáme se službou VSTS].</span><span class="sxs-lookup"><span data-stu-id="25504-111">For more information on publishing your project to these services, see [Create a repo (GitHub)], [Create a repo (BitBucket)], and [Get started with VSTS].</span></span>
2. <span data-ttu-id="25504-112">V okně s nabídkou aplikace na [Azure Portal] klikněte na tlačítko **NASAZENÍ APLIKACE > Možnosti nasazení**.</span><span class="sxs-lookup"><span data-stu-id="25504-112">In your app's menu blade in the [Azure portal], click **APP DEPLOYMENT > Deployment options**.</span></span> <span data-ttu-id="25504-113">Klikněte na **Zvolit zdroj** a pak vyberte zdroj nasazení.</span><span class="sxs-lookup"><span data-stu-id="25504-113">Click **Choose Source**, then select the deployment source.</span></span>  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > <span data-ttu-id="25504-114">Pokud chcete konfigurovat účet služby VSTS pro nasazení služby App Service, prostudujte si tento [kurz](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).</span><span class="sxs-lookup"><span data-stu-id="25504-114">To configure a VSTS account for App Service deployment please see this [tutorial](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).</span></span>
   > 
   > 
3. <span data-ttu-id="25504-115">Dokončete pracovní postup ověřování.</span><span class="sxs-lookup"><span data-stu-id="25504-115">Complete the authorization workflow.</span></span>
4. <span data-ttu-id="25504-116">V okně **Zdroj nasazení** zvolte projekt a větev, ze které se má nasazení provádět.</span><span class="sxs-lookup"><span data-stu-id="25504-116">In the **Deployment source** blade, choose the project and branch to deploy from.</span></span> <span data-ttu-id="25504-117">Až to budete mít, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="25504-117">When you're done, click **OK**.</span></span>
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > <span data-ttu-id="25504-118">Při povolování průběžného nasazování s použitím GitHubu nebo BitBucketu se zobrazí veřejné i soukromé projekty.</span><span class="sxs-lookup"><span data-stu-id="25504-118">When enabling continuous deployment with GitHub or BitBucket, both public and private projects will be displayed.</span></span>
   > 
   > 
   
    <span data-ttu-id="25504-119">Služba App Service vytvoří přidružení s vybraným úložištěm, vyžádá si soubory z určené větve a bude udržovat klon úložiště pro vaši aplikaci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="25504-119">App Service creates an association with the selected repository, pulls in the files from the specified branch, and maintains a clone of your repository for your App Service app.</span></span> <span data-ttu-id="25504-120">Při konfiguraci průběžného nasazování služby VSTS z portálu Azure Portal integrace využívá [stroj pro nasazení Kudu](https://github.com/projectkudu/kudu/wiki) služby App Service, který s každým příkazem `git push` již automatizuje úlohy sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="25504-120">When you configure VSTS continuous deployment from the Azure portal, the integration uses the App Service [Kudu deployment engine](https://github.com/projectkudu/kudu/wiki), which already automates build and deployment tasks with every `git push`.</span></span> <span data-ttu-id="25504-121">Průběžné nasazování ve službě VSTS nemusíte nastavovat samostatně.</span><span class="sxs-lookup"><span data-stu-id="25504-121">You do not need to separately set up continuous deployment in VSTS.</span></span> <span data-ttu-id="25504-122">Po dokončení tohoto procesu se v okně aplikace **Možnosti nasazení** zobrazí aktivní nasazení, které indikuje, že nasazení proběhlo úspěšně.</span><span class="sxs-lookup"><span data-stu-id="25504-122">After this process completes, the **Deployment options** app blade will show an active deployment that indicates deployment has succeeded.</span></span>
5. <span data-ttu-id="25504-123">Pokud chcete ověřit úspěšné nasazení aplikace, klikněte na možnost **Adresa URL** v horní části okna aplikace na portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="25504-123">To verify the app is successfully deployed, click the **URL** at the top of the app's blade in the Azure portal.</span></span>
6. <span data-ttu-id="25504-124">Pokud chcete ověřit, že se průběžné nasazování provádí ze zvoleného úložiště, vložte do úložiště nějakou změnu.</span><span class="sxs-lookup"><span data-stu-id="25504-124">To verify that continuous deployment is occurring from the repository of your choice, push a change to the repository.</span></span> <span data-ttu-id="25504-125">Aplikace by se měla krátce po dokončení vkládání do úložiště aktualizovat a změny by se měly projevit.</span><span class="sxs-lookup"><span data-stu-id="25504-125">Your app should update to reflect the changes shortly after the push to the repository completes.</span></span> <span data-ttu-id="25504-126">Vložení aktualizace můžete ověřit v okně **Možnosti nasazení** v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="25504-126">You can verify that it has pulled in the update in the **Deployment options** blade of your app.</span></span>

## <span data-ttu-id="25504-127"><a name="VSsolution"></a>Průběžné nasazování řešení sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="25504-127"><a name="VSsolution"></a>Continuous deployment of a Visual Studio solution</span></span>
<span data-ttu-id="25504-128">Vložení řešení sady Visual Studio do služby Azure App Service může představovat prosté vložení jednoduchého souboru index.html.</span><span class="sxs-lookup"><span data-stu-id="25504-128">Pushing a Visual Studio solution to Azure App Service is just as easy as pushing a simple index.html file.</span></span> <span data-ttu-id="25504-129">Proces nasazení služby App Service zvyšuje efektivitu všech podrobností, včetně obnovení závislostí NuGet sestavení binárních souborů aplikace.</span><span class="sxs-lookup"><span data-stu-id="25504-129">The App Service deployment process streamlines all the details, including restoring NuGet dependencies and building the application binaries.</span></span> <span data-ttu-id="25504-130">Stačí, abyste doporučené postupy správy a údržby zdrojového kódu dodržovali jen ve svém úložišti Git, a nasazení aplikace App Service se postará o všechno ostatní.</span><span class="sxs-lookup"><span data-stu-id="25504-130">You can follow the source control best practices of maintaining code only in your Git repository, and let App Service deployment take care of the rest.</span></span>

<span data-ttu-id="25504-131">Kroky při vkládání řešení sady Visual Studio do služby App Service jsou stejné jako v [předchozí části](#overview), pokud řešení a úložiště nakonfigurujete takto:</span><span class="sxs-lookup"><span data-stu-id="25504-131">The steps for pushing your Visual Studio solution to App Service are the same as in the [previous section](#overview), provided that you configure your solution and repository as follows:</span></span>

* <span data-ttu-id="25504-132">Pomocí možnosti správy zdrojového kódu sady Visual Studio vygenerujte soubor `.gitignore` podobně jako na následujícím obrázku nebo do kořenového adresáře úložiště ručně přidejte soubor `.gitignore` s obsahem podobným této [ukázce souboru .gitignore](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).</span><span class="sxs-lookup"><span data-stu-id="25504-132">Use the Visual Studio source control option to generate a `.gitignore` file such as the image below or manually add a `.gitignore` file in your repository root with content similar to this [.gitignore sample](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).</span></span>
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* <span data-ttu-id="25504-133">Přidejte adresářový strom celého řešení do úložiště tak, aby se soubor .sln nacházel v kořenovém adresáři úložiště.</span><span class="sxs-lookup"><span data-stu-id="25504-133">Add the entire solution's directory tree to your repository, with the .sln file in the repository root.</span></span>

<span data-ttu-id="25504-134">Po nastavení úložiště podle uvedeného popisu a konfiguraci aplikace v Azure tak, aby probíhalo průběžné nasazování z jednoho z úložišť Git online, můžete vyvíjet aplikaci ASP.NET místně v sadě Visual Studio a průběžně kód nasazovat prostým vložením změn do příslušného úložiště Git online.</span><span class="sxs-lookup"><span data-stu-id="25504-134">Once you have set up your repository as described, and configured your app in Azure for continuous publishing from one of the online Git repositories, you can develop your ASP.NET application locally in Visual Studio and continuously deploy your code simply by pushing your changes to your online Git repository.</span></span>

## <span data-ttu-id="25504-135"><a name="disableCD"></a>Zakázání průběžného nasazování</span><span class="sxs-lookup"><span data-stu-id="25504-135"><a name="disableCD"></a>Disable continuous deployment</span></span>
<span data-ttu-id="25504-136">Pokud chcete průběžné nasazování zakázat, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="25504-136">To disable continuous deployment,</span></span>

1. <span data-ttu-id="25504-137">V okně s nabídkou aplikace na [Azure Portal] klikněte na tlačítko **NASAZENÍ APLIKACE > Možnosti nasazení**.</span><span class="sxs-lookup"><span data-stu-id="25504-137">In your app's menu blade in the [Azure portal], click **APP DEPLOYMENT > Deployment options**.</span></span> <span data-ttu-id="25504-138">Klikněte na tlačítko **Odpojit** v okně **Možnosti nasazení**.</span><span class="sxs-lookup"><span data-stu-id="25504-138">Then click **Disconnect** in the **Deployment options** blade.</span></span>
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. <span data-ttu-id="25504-139">Po zvolení odpovědi **Ano** na potvrzovací zprávu se můžete vrátit do okna aplikace a kliknout na **NASAZENÍ APLIKACE > Možnosti nasazení**, pokud chcete nastavit publikování z jiného zdroje.</span><span class="sxs-lookup"><span data-stu-id="25504-139">After answering **Yes** to the confirmation message, you can return to your app's blade and click **APP DEPLOYMENT > Deployment options** if you would like to set up publishing from another source.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25504-140">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="25504-140">Additional Resources</span></span>
* [<span data-ttu-id="25504-141">Postup při zkoumání běžných problémů s průběžným nasazováním</span><span class="sxs-lookup"><span data-stu-id="25504-141">How to investigate common issues with continuous deployment</span></span>](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* <span data-ttu-id="25504-142">[Způsob používání prostředí PowerShell pro Azure]</span><span class="sxs-lookup"><span data-stu-id="25504-142">[How to use PowerShell for Azure]</span></span>
* <span data-ttu-id="25504-143">[Způsob používání nástrojů příkazového řádku Azure pro Mac a Linux]</span><span class="sxs-lookup"><span data-stu-id="25504-143">[How to use the Azure Command-Line Tools for Mac and Linux]</span></span>
* <span data-ttu-id="25504-144">[Dokumentace pro Git]</span><span class="sxs-lookup"><span data-stu-id="25504-144">[Git documentation]</span></span>
* [<span data-ttu-id="25504-145">Projekt Kudu</span><span class="sxs-lookup"><span data-stu-id="25504-145">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
* [<span data-ttu-id="25504-146">Použití Azure automaticky generovat kanálu CI nebo CD pro nasazení aplikace ASP.NET 4</span><span class="sxs-lookup"><span data-stu-id="25504-146">Use Azure to automatically generate a CI/CD pipeline to deploy an ASP.NET 4 app</span></span>](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

> [!NOTE]
> <span data-ttu-id="25504-147">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="25504-147">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="25504-148">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="25504-148">No credit cards required; no commitments.</span></span>
> 
> 

<span data-ttu-id="25504-149">[Azure App Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/</span><span class="sxs-lookup"><span data-stu-id="25504-149">[Azure App Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/</span></span>
<span data-ttu-id="25504-150">[Azure Portal]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="25504-150">[Azure portal]: https://portal.azure.com</span></span>
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
<span data-ttu-id="25504-151">[Způsob používání prostředí PowerShell pro Azure]: /powershell/azureps-cmdlets-docs</span><span class="sxs-lookup"><span data-stu-id="25504-151">[How to use PowerShell for Azure]: /powershell/azureps-cmdlets-docs</span></span>
<span data-ttu-id="25504-152">[Způsob používání nástrojů příkazového řádku Azure pro Mac a Linux]:../cli-install-nodejs.md</span><span class="sxs-lookup"><span data-stu-id="25504-152">[How to use the Azure Command-Line Tools for Mac and Linux]:../cli-install-nodejs.md</span></span>
<span data-ttu-id="25504-153">[Dokumentace pro Git]: http://git-scm.com/documentation</span><span class="sxs-lookup"><span data-stu-id="25504-153">[Git Documentation]: http://git-scm.com/documentation</span></span>

<span data-ttu-id="25504-154">[Vytvoření úložiště (GitHub)]: https://help.github.com/articles/create-a-repo</span><span class="sxs-lookup"><span data-stu-id="25504-154">[Create a repo (GitHub)]: https://help.github.com/articles/create-a-repo</span></span>
<span data-ttu-id="25504-155">[Vytvoření úložiště (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo</span><span class="sxs-lookup"><span data-stu-id="25504-155">[Create a repo (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo</span></span>
<span data-ttu-id="25504-156">[Začínáme se službou VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview</span><span class="sxs-lookup"><span data-stu-id="25504-156">[Get started with VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview</span></span>
