---
title: "aaaContinuous tooAzure nasazení služby App Service | Microsoft Docs"
description: "Zjistěte, jak tooenable průběžné nasazování tooAzure služby App Service."
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
ms.openlocfilehash: 62a22cbda354fd5b0a1b9729c8c375408e75049f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-tooazure-app-service"></a><span data-ttu-id="4ccd1-103">Průběžné tooAzure nasazení služby App Service</span><span class="sxs-lookup"><span data-stu-id="4ccd1-103">Continuous Deployment tooAzure App Service</span></span>
<span data-ttu-id="4ccd1-104">Tento kurz ukazuje, jak tooconfigure pracovního postupu průběžné nasazování pro vaše [Azure App Service] aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-104">This tutorial shows you how tooconfigure a continuous deployment workflow for your [Azure App Service] app.</span></span> <span data-ttu-id="4ccd1-105">Služby App Service integrace s BitBucket, Githubu, a [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) umožňuje průběžně pracovní postup nasazení, kde Azure vyžaduje nejnovější aktualizace hello z projektu publikované tooone z těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-105">App Service integration with BitBucket, GitHub, and [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) enables a continuous deployment workflow where Azure pulls in hello most recent updates from your project published tooone of these services.</span></span> <span data-ttu-id="4ccd1-106">Průběžné nasazování je skvělou možností pro projekty, u kterých se integruje více příspěvků nebo u kterých integrace probíhá často.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-106">Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated.</span></span>

<span data-ttu-id="4ccd1-107">toofind out jak hello tooconfigure průběžné nasazování ručně z úložiště v cloudu nejsou uvedené pomocí portálu Azure (například [GitLab](https://gitlab.com/)), najdete v části [nastavení průběžné nasazování pomocí ruční kroky](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).</span><span class="sxs-lookup"><span data-stu-id="4ccd1-107">toofind out how tooconfigure continuous deployment manually from a cloud repository not listed by hello Azure Portal (such as [GitLab](https://gitlab.com/)), see [Setting up continuous deployment using manual steps](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).</span></span>

## <span data-ttu-id="4ccd1-108"><a name="overview"></a>Povolení průběžného nasazování</span><span class="sxs-lookup"><span data-stu-id="4ccd1-108"><a name="overview"></a>Enable continuous deployment</span></span>
<span data-ttu-id="4ccd1-109">průběžné nasazování tooenable,</span><span class="sxs-lookup"><span data-stu-id="4ccd1-109">tooenable continuous deployment,</span></span>

1. <span data-ttu-id="4ccd1-110">Publikujte úložiště obsahu toohello vaší aplikace, který se použije pro průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-110">Publish your app content toohello repository that will be used for continuous deployment.</span></span>  
    <span data-ttu-id="4ccd1-111">Další informace o publikování vašeho projektu toothese služby najdete v tématu [vytvořit úložiště (Githubu)], [vytvořit úložiště (BitBucket)], a [začít pracovat s služby VSTS].</span><span class="sxs-lookup"><span data-stu-id="4ccd1-111">For more information on publishing your project toothese services, see [Create a repo (GitHub)], [Create a repo (BitBucket)], and [Get started with VSTS].</span></span>
2. <span data-ttu-id="4ccd1-112">V okně vaší aplikace nabídky v hello [portál Azure], klikněte na tlačítko **nasazení aplikace > Možnosti nasazení**.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-112">In your app's menu blade in hello [Azure portal], click **APP DEPLOYMENT > Deployment options**.</span></span> <span data-ttu-id="4ccd1-113">Klikněte na tlačítko **zvolit zdroj**, zvolte položku zdroj nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-113">Click **Choose Source**, then select hello deployment source.</span></span>  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > <span data-ttu-id="4ccd1-114">tooconfigure účet služby VSTS pro nasazení služby App Service najdete v tématu to [kurzu](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).</span><span class="sxs-lookup"><span data-stu-id="4ccd1-114">tooconfigure a VSTS account for App Service deployment please see this [tutorial](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).</span></span>
   > 
   > 
3. <span data-ttu-id="4ccd1-115">Dokončení pracovního postupu hello autorizace.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-115">Complete hello authorization workflow.</span></span>
4. <span data-ttu-id="4ccd1-116">V hello **zdroj nasazení** okně Zvolit hello projekt a rozvětvují toodeploy z.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-116">In hello **Deployment source** blade, choose hello project and branch toodeploy from.</span></span> <span data-ttu-id="4ccd1-117">Až to budete mít, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-117">When you're done, click **OK**.</span></span>
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > <span data-ttu-id="4ccd1-118">Při povolování průběžného nasazování s použitím GitHubu nebo BitBucketu se zobrazí veřejné i soukromé projekty.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-118">When enabling continuous deployment with GitHub or BitBucket, both public and private projects will be displayed.</span></span>
   > 
   > 
   
    <span data-ttu-id="4ccd1-119">Služby App Service vytvoří přidružení se hello vybrané úložiště, který vrátí v souborech hello z hello zadaný větve a udržuje klon úložiště pro aplikaci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-119">App Service creates an association with hello selected repository, pulls in hello files from hello specified branch, and maintains a clone of your repository for your App Service app.</span></span> <span data-ttu-id="4ccd1-120">Při konfiguraci služby VSTS průběžné nasazování z hello portálu Azure, integrace hello používá hello služby App Service [modul nasazení Kudu](https://github.com/projectkudu/kudu/wiki), které již automatizuje úlohy sestavení a nasazení s každou `git push`.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-120">When you configure VSTS continuous deployment from hello Azure portal, hello integration uses hello App Service [Kudu deployment engine](https://github.com/projectkudu/kudu/wiki), which already automates build and deployment tasks with every `git push`.</span></span> <span data-ttu-id="4ccd1-121">Není nutné tooseparately nastavit průběžné nasazování v služby VSTS.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-121">You do not need tooseparately set up continuous deployment in VSTS.</span></span> <span data-ttu-id="4ccd1-122">Po dokončení tohoto procesu se hello **možnosti nasazení** okně aplikace se zobrazí aktivní nasazení, která určuje nasazení proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-122">After this process completes, hello **Deployment options** app blade will show an active deployment that indicates deployment has succeeded.</span></span>
5. <span data-ttu-id="4ccd1-123">tooverify hello aplikace je úspěšně nasazená, klikněte na tlačítko hello **URL** hello horní části okna hello aplikací v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-123">tooverify hello app is successfully deployed, click hello **URL** at hello top of hello app's blade in hello Azure portal.</span></span>
6. <span data-ttu-id="4ccd1-124">tooverify, který průběžné nasazování dochází z hello úložiště podle vašeho výběru push úložiště toohello změnu.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-124">tooverify that continuous deployment is occurring from hello repository of your choice, push a change toohello repository.</span></span> <span data-ttu-id="4ccd1-125">Aplikace by měl aktualizovat změny hello tooreflect krátce po dokončení hello nabízené toohello úložiště.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-125">Your app should update tooreflect hello changes shortly after hello push toohello repository completes.</span></span> <span data-ttu-id="4ccd1-126">Můžete ověřit, že má vyžádat v aktualizaci hello v hello **možnosti nasazení** okno vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-126">You can verify that it has pulled in hello update in hello **Deployment options** blade of your app.</span></span>

## <span data-ttu-id="4ccd1-127"><a name="VSsolution"></a>Průběžné nasazování řešení sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ccd1-127"><a name="VSsolution"></a>Continuous deployment of a Visual Studio solution</span></span>
<span data-ttu-id="4ccd1-128">Vkládání tooAzure řešení sady Visual Studio služby App Service je stejně jednoduché jako vkládání jednoduché index.html souboru.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-128">Pushing a Visual Studio solution tooAzure App Service is just as easy as pushing a simple index.html file.</span></span> <span data-ttu-id="4ccd1-129">Hello procesu nasazení služby App Service zjednodušuje všechny hello podrobnosti, včetně závislostí NuGet obnovení a vytváření binární soubory aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-129">hello App Service deployment process streamlines all hello details, including restoring NuGet dependencies and building hello application binaries.</span></span> <span data-ttu-id="4ccd1-130">Můžete dodržujte doporučené postupy řízení hello zdroj Správa kód pouze do úložiště Git a umožní nasazení služby App Service se postará o hello rest.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-130">You can follow hello source control best practices of maintaining code only in your Git repository, and let App Service deployment take care of hello rest.</span></span>

<span data-ttu-id="4ccd1-131">Hello kroky pro vkládání vaší tooApp řešení sady Visual Studio jsou služby hello stejné jako hello [předchozí části](#overview), za předpokladu, že můžete nakonfigurovat následujícím způsobem řešení a úložiště:</span><span class="sxs-lookup"><span data-stu-id="4ccd1-131">hello steps for pushing your Visual Studio solution tooApp Service are hello same as in hello [previous section](#overview), provided that you configure your solution and repository as follows:</span></span>

* <span data-ttu-id="4ccd1-132">Použít hello Visual Studio zdroj ovládacího prvku možnost toogenerate `.gitignore` soubor například hello bitovou kopii níže nebo přidejte ručně `.gitignore` soubor v kořenovém úložišti s obsahu podobné toothis [.gitignore ukázka](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).</span><span class="sxs-lookup"><span data-stu-id="4ccd1-132">Use hello Visual Studio source control option toogenerate a `.gitignore` file such as hello image below or manually add a `.gitignore` file in your repository root with content similar toothis [.gitignore sample](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).</span></span>
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* <span data-ttu-id="4ccd1-133">Přidejte hello celé řešení directory stromu tooyour úložiště, se soubor .sln hello v kořenovém úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-133">Add hello entire solution's directory tree tooyour repository, with hello .sln file in hello repository root.</span></span>

<span data-ttu-id="4ccd1-134">Po nastavení úložiště bez vyžádání, jak je popsáno a aplikace v Azure nakonfigurovaný pro nepřetržitou publikování z jednoho z hello online úložiště Git, můžete vyvíjet aplikace ASP.NET místně v sadě Visual Studio a průběžně nasadíte tak svůj kód jednoduše načte Když zavedete vaše změny tooyour online úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-134">Once you have set up your repository as described, and configured your app in Azure for continuous publishing from one of hello online Git repositories, you can develop your ASP.NET application locally in Visual Studio and continuously deploy your code simply by pushing your changes tooyour online Git repository.</span></span>

## <span data-ttu-id="4ccd1-135"><a name="disableCD"></a>Zakázání průběžného nasazování</span><span class="sxs-lookup"><span data-stu-id="4ccd1-135"><a name="disableCD"></a>Disable continuous deployment</span></span>
<span data-ttu-id="4ccd1-136">průběžné nasazování toodisable,</span><span class="sxs-lookup"><span data-stu-id="4ccd1-136">toodisable continuous deployment,</span></span>

1. <span data-ttu-id="4ccd1-137">V okně vaší aplikace nabídky v hello [portál Azure], klikněte na tlačítko **nasazení aplikace > Možnosti nasazení**.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-137">In your app's menu blade in hello [Azure portal], click **APP DEPLOYMENT > Deployment options**.</span></span> <span data-ttu-id="4ccd1-138">Pak klikněte na tlačítko **odpojení** v hello **možnosti nasazení** okno.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-138">Then click **Disconnect** in hello **Deployment options** blade.</span></span>
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. <span data-ttu-id="4ccd1-139">Po odpovídání **Ano** toohello potvrzující zpráva, se můžete vrátit okně tooyour aplikace a klikněte na tlačítko **nasazení aplikace > Možnosti nasazení** Pokud byste chtěli tooset až publikování z jiného zdroje.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-139">After answering **Yes** toohello confirmation message, you can return tooyour app's blade and click **APP DEPLOYMENT > Deployment options** if you would like tooset up publishing from another source.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4ccd1-140">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4ccd1-140">Additional Resources</span></span>
* [<span data-ttu-id="4ccd1-141">Jak tooinvestigate běžné problémy se průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-141">How tooinvestigate common issues with continuous deployment</span></span>](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* <span data-ttu-id="4ccd1-142">[Jak toouse prostředí PowerShell pro Azure]</span><span class="sxs-lookup"><span data-stu-id="4ccd1-142">[How toouse PowerShell for Azure]</span></span>
* <span data-ttu-id="4ccd1-143">[Jak toouse hello nástroje příkazového řádku Azure pro Mac a Linux]</span><span class="sxs-lookup"><span data-stu-id="4ccd1-143">[How toouse hello Azure Command-Line Tools for Mac and Linux]</span></span>
* <span data-ttu-id="4ccd1-144">[Dokumentace pro Git]</span><span class="sxs-lookup"><span data-stu-id="4ccd1-144">[Git documentation]</span></span>
* [<span data-ttu-id="4ccd1-145">Projekt Kudu</span><span class="sxs-lookup"><span data-stu-id="4ccd1-145">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
* [<span data-ttu-id="4ccd1-146">Použití Azure tooautomatically generovat CI/CD kanálu toodeploy ASP.NET 4 aplikace</span><span class="sxs-lookup"><span data-stu-id="4ccd1-146">Use Azure tooautomatically generate a CI/CD pipeline toodeploy an ASP.NET 4 app</span></span>](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

> [!NOTE]
> <span data-ttu-id="4ccd1-147">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-147">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="4ccd1-148">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="4ccd1-148">No credit cards required; no commitments.</span></span>
> 
> 

[Azure App Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/
[portál Azure]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Jak toouse prostředí PowerShell pro Azure]: /powershell/azureps-cmdlets-docs
[Jak toouse hello nástroje příkazového řádku Azure pro Mac a Linux]:../cli-install-nodejs.md
[Dokumentace pro Git]: http://git-scm.com/documentation

[vytvořit úložiště (Githubu)]: https://help.github.com/articles/create-a-repo
[vytvořit úložiště (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[začít pracovat s služby VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview
