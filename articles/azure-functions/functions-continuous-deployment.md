---
title: "Průběžné nasazování pro Azure Functions | Microsoft Docs"
description: "Průběžné nasazování vlastnosti služby Azure App Service použijte k publikování Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: 3756f1a039730bfd99b0375ce9bfeaf27178f2e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-deployment-for-azure-functions"></a><span data-ttu-id="38bd9-103">Průběžné nasazování se službou Azure Functions</span><span class="sxs-lookup"><span data-stu-id="38bd9-103">Continuous deployment for Azure Functions</span></span>
<span data-ttu-id="38bd9-104">Azure Functions usnadňuje nasazení funkce aplikace pomocí průběžnou integraci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="38bd9-104">Azure Functions makes it easy to deploy your function app using App Service continuous integration.</span></span> <span data-ttu-id="38bd9-105">Funkce se integruje s BitBucket, Dropbox, Githubu a Visual Studio Team Services (VSTS).</span><span class="sxs-lookup"><span data-stu-id="38bd9-105">Functions integrates with BitBucket, Dropbox, GitHub, and Visual Studio Team Services (VSTS).</span></span> <span data-ttu-id="38bd9-106">To umožňuje pracovní kde kód funkce aktualizace provedené pomocí jedné z těchto integrovaných služeb aktivační událost nasazení do Azure.</span><span class="sxs-lookup"><span data-stu-id="38bd9-106">This enables a workflow where function code updates made by using one of these integrated services trigger deployment to Azure.</span></span> <span data-ttu-id="38bd9-107">Pokud začínáte na Azure Functions, začněte s [přehled Azure Functions](functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="38bd9-107">If you are new to Azure Functions, start with [Azure Functions Overview](functions-overview.md).</span></span>

<span data-ttu-id="38bd9-108">Průběžné nasazování je skvělou možností pro projekty, u kterých se integruje více příspěvků nebo u kterých integrace probíhá často.</span><span class="sxs-lookup"><span data-stu-id="38bd9-108">Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated.</span></span> <span data-ttu-id="38bd9-109">To také umožňuje udržovat zdrojového kódu na váš kód funkce.</span><span class="sxs-lookup"><span data-stu-id="38bd9-109">It also lets you maintain source control on your functions code.</span></span> <span data-ttu-id="38bd9-110">Následující zdroje nasazení jsou aktuálně podporovány:</span><span class="sxs-lookup"><span data-stu-id="38bd9-110">The following deployment sources are currently supported:</span></span>

* [<span data-ttu-id="38bd9-111">Bitbucket</span><span class="sxs-lookup"><span data-stu-id="38bd9-111">Bitbucket</span></span>](https://bitbucket.org/)
* [<span data-ttu-id="38bd9-112">Dropbox</span><span class="sxs-lookup"><span data-stu-id="38bd9-112">Dropbox</span></span>](https://www.dropbox.com/)
* <span data-ttu-id="38bd9-113">Externího úložiště (Git nebo Mercurial)</span><span class="sxs-lookup"><span data-stu-id="38bd9-113">External repository (Git or Mercurial)</span></span>
* [<span data-ttu-id="38bd9-114">Místní úložiště Git</span><span class="sxs-lookup"><span data-stu-id="38bd9-114">Git local repository</span></span>](../app-service-web/app-service-deploy-local-git.md)
* [<span data-ttu-id="38bd9-115">GitHub</span><span class="sxs-lookup"><span data-stu-id="38bd9-115">GitHub</span></span>](https://github.com)
* [<span data-ttu-id="38bd9-116">OneDrive</span><span class="sxs-lookup"><span data-stu-id="38bd9-116">OneDrive</span></span>](https://onedrive.live.com/)
* [<span data-ttu-id="38bd9-117">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="38bd9-117">Visual Studio Team Services</span></span>](https://www.visualstudio.com/team-services/)

<span data-ttu-id="38bd9-118">Nasazení jsou nakonfigurované na základě aplikací na funkce.</span><span class="sxs-lookup"><span data-stu-id="38bd9-118">Deployments are configured on a per-function app basis.</span></span> <span data-ttu-id="38bd9-119">Po povolení průběžné nasazování přístup ke kódu funkce portálu je nastavena na *jen pro čtení*.</span><span class="sxs-lookup"><span data-stu-id="38bd9-119">After continuous deployment is enabled, access to function code in the portal is set to *read-only*.</span></span>

## <a name="continuous-deployment-requirements"></a><span data-ttu-id="38bd9-120">Průběžné nasazování požadavky</span><span class="sxs-lookup"><span data-stu-id="38bd9-120">Continuous deployment requirements</span></span>

<span data-ttu-id="38bd9-121">Váš zdroj nasazení nakonfigurované a váš kód funkce musí mít v zdroj nasazení před nastavit průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="38bd9-121">You must have your deployment source configured and your functions code in the deployment source before you set up continuous deployment.</span></span> <span data-ttu-id="38bd9-122">V nasazení aplikace danou funkci jednotlivé funkce žije v podadresáři s názvem, kde název adresáře je název funkce.</span><span class="sxs-lookup"><span data-stu-id="38bd9-122">In a given function app deployment, each function lives in a named subdirectory, where the directory name is the name of the function.</span></span>  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="set-up-continuous-deployment"></a><span data-ttu-id="38bd9-123">Nastavení nepřetržitého nasazování</span><span class="sxs-lookup"><span data-stu-id="38bd9-123">Set up continuous deployment</span></span>
<span data-ttu-id="38bd9-124">Pomocí tohoto postupu ke konfiguraci průběžné nasazování pro existující aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="38bd9-124">Use this procedure to configure continuous deployment for an existing function app.</span></span> <span data-ttu-id="38bd9-125">Tyto kroky ukazují integrace s úložišti GitHub, ale podobným způsobem platí pro Visual Studio Team Services nebo jiné služby pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="38bd9-125">These steps demonstrate integration with a GitHub repository, but similar steps apply for Visual Studio Team Services or other deployment services.</span></span>

1. <span data-ttu-id="38bd9-126">V aplikaci funkce v [portál Azure](https://portal.azure.com), klikněte na tlačítko **funkce** a **možnosti nasazení**.</span><span class="sxs-lookup"><span data-stu-id="38bd9-126">In your function app in the [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment options**.</span></span> 
   
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/setup-deployment.png)
 
2. <span data-ttu-id="38bd9-128">Potom v **nasazení** okně klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="38bd9-128">Then in the **Deployments** blade click **Setup**.</span></span>
 
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. <span data-ttu-id="38bd9-130">V **zdroj nasazení** okně klikněte na tlačítko **vybrat zdroj**, pak zadejte informace pro váš zdroj vybraný nasazení a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="38bd9-130">In the **Deployment source** blade, click **Choose source**, then fill in the information for your chosen deployment source and click **OK**.</span></span>
   
    ![Vyberte zdroj nasazení.](./media/functions-continuous-deployment/choose-deployment-source.png)

<span data-ttu-id="38bd9-132">Po dokončení konfigurace průběžné nasazování, všechny změny souboru ve zdroji nasazení se zkopírují do aplikaci funkce a aktivaci nasazení plnou verzi webu.</span><span class="sxs-lookup"><span data-stu-id="38bd9-132">After continuous deployment is configured, all file changes in your deployment source are copied to the function app and a full site deployment is triggered.</span></span> <span data-ttu-id="38bd9-133">Když jsou aktualizovány soubory ve zdrojovém se je znovu nasadil webu.</span><span class="sxs-lookup"><span data-stu-id="38bd9-133">The site is redeployed when files in the source are updated.</span></span>

## <a name="deployment-options"></a><span data-ttu-id="38bd9-134">Možnosti nasazení</span><span class="sxs-lookup"><span data-stu-id="38bd9-134">Deployment options</span></span>

<span data-ttu-id="38bd9-135">Tady jsou některé typické nasazení scénáře:</span><span class="sxs-lookup"><span data-stu-id="38bd9-135">The following are some typical deployment scenarios:</span></span>

- [<span data-ttu-id="38bd9-136">Vytvořit pracovní nasazení</span><span class="sxs-lookup"><span data-stu-id="38bd9-136">Create a staging deployment</span></span>](#staging)
- [<span data-ttu-id="38bd9-137">Průběžné nasazování budou přesunuty existující funkce</span><span class="sxs-lookup"><span data-stu-id="38bd9-137">Move existing functions to continuous deployment</span></span>](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a><span data-ttu-id="38bd9-138">Vytvořit pracovní nasazení</span><span class="sxs-lookup"><span data-stu-id="38bd9-138">Create a staging deployment</span></span>

<span data-ttu-id="38bd9-139">Funkce aplikace ještě nepodporuje nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="38bd9-139">Function Apps doesn't yet support deployment slots.</span></span> <span data-ttu-id="38bd9-140">Samostatná pracovní a provozní nasazení však můžete spravovat pomocí nepřetržité integrace.</span><span class="sxs-lookup"><span data-stu-id="38bd9-140">However, you can still manage separate staging and production deployments by using continuous integration.</span></span>

<span data-ttu-id="38bd9-141">Postup konfigurace a práce s pracovní nasazení se obecně vypadat třeba takto:</span><span class="sxs-lookup"><span data-stu-id="38bd9-141">The process to configure and work with a staging deployment looks generally like this:</span></span>

1. <span data-ttu-id="38bd9-142">Vaše předplatné, jeden pro kód produkční a jeden pro pracovní vytvořte dvě funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="38bd9-142">Create two function apps in your subscription, one for the production code and one for staging.</span></span> 

2. <span data-ttu-id="38bd9-143">Pokud jste ještě nemáte, vytvořte zdroj nasazení.</span><span class="sxs-lookup"><span data-stu-id="38bd9-143">Create a deployment source, if you don't already have one.</span></span> <span data-ttu-id="38bd9-144">Tento příklad používá [Githubu].</span><span class="sxs-lookup"><span data-stu-id="38bd9-144">This example uses [GitHub].</span></span>

3. <span data-ttu-id="38bd9-145">Pro vaše produkční funkce aplikace podle předchozích dokončení kroků **nastavit průběžné nasazování** a nastavit nasazení větve do hlavní větve ve vašem úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="38bd9-145">For your production function app, complete the preceding steps in **Set up continuous deployment** and set the deployment branch to the master branch of your GitHub repository.</span></span>
   
    ![Vyberte větev nasazení](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. <span data-ttu-id="38bd9-147">Tento krok opakujte pro pracovní funkce aplikace, ale místo toho vyberte pracovní větev ve vašem úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="38bd9-147">Repeat this step for the staging function app, but choose the staging branch instead in your GitHub repo.</span></span> <span data-ttu-id="38bd9-148">Pokud vaše zdrojová nasazení nepodporuje vytvoření větve, použijte jinou složku.</span><span class="sxs-lookup"><span data-stu-id="38bd9-148">If your deployment source doesn't support branching, use a different folder.</span></span>
    
5. <span data-ttu-id="38bd9-149">Provést aktualizace kódu v pracovní větev nebo složku a potom ověřte, že tyto změny se projeví v pracovní nasazení.</span><span class="sxs-lookup"><span data-stu-id="38bd9-149">Make updates to your code in the staging branch or folder, then verify that those changes are reflected in the staging deployment.</span></span>

6. <span data-ttu-id="38bd9-150">Po testování sloučit změny z pracovní větve do hlavní větve.</span><span class="sxs-lookup"><span data-stu-id="38bd9-150">After testing, merge changes from the staging branch into the master branch.</span></span> <span data-ttu-id="38bd9-151">Tato sloučení aktivuje nasazení na produkční aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="38bd9-151">This merge triggers deployment to the production function app.</span></span> <span data-ttu-id="38bd9-152">Pokud vaše zdrojová nasazení nepodporuje větví, přepište soubory ve složce produkční soubory z pracovní složky.</span><span class="sxs-lookup"><span data-stu-id="38bd9-152">If your deployment source doesn't support branches, overwrite the files in the production folder with the files from the staging folder.</span></span>

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a><span data-ttu-id="38bd9-153">Průběžné nasazování budou přesunuty existující funkce</span><span class="sxs-lookup"><span data-stu-id="38bd9-153">Move existing functions to continuous deployment</span></span>
<span data-ttu-id="38bd9-154">Pokud máte existující funkce, které jste vytvořili a udržuje na portálu, budete muset stáhnout existující soubory s kódem funkce pomocí protokolu FTP, nebo místní úložiště Git předtím, než můžete nastavit průběžné nasazování, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="38bd9-154">When you have existing functions that you have created and maintained in the portal, you need to download your existing function code files using FTP or the local Git repository before you can set up continuous deployment as described above.</span></span> <span data-ttu-id="38bd9-155">To provedete v nastavení služby App Service pro vaši aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="38bd9-155">You can do this in the App Service settings for your function app.</span></span> <span data-ttu-id="38bd9-156">Po stažení jsou vaše soubory, můžete je načíst do zdroje zvolené průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="38bd9-156">After your files are downloaded, you can upload them to your chosen continuous deployment source.</span></span>

> [!NOTE]
> <span data-ttu-id="38bd9-157">Po dokončení konfigurace průběžnou integraci, jste už nebude moci upravit zdrojové soubory vašeho funkce portálu.</span><span class="sxs-lookup"><span data-stu-id="38bd9-157">After you configure continuous integration, you will no longer be able to edit your source files in the Functions portal.</span></span>

- [<span data-ttu-id="38bd9-158">Postupy: konfigurace přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="38bd9-158">How to: Configure deployment credentials</span></span>](#credentials)
- [<span data-ttu-id="38bd9-159">Postupy: stažení souborů pomocí protokolu FTP</span><span class="sxs-lookup"><span data-stu-id="38bd9-159">How to: Download files using FTP</span></span>](#downftp)
- [<span data-ttu-id="38bd9-160">Postupy: stahovat soubory s použitím místní úložiště Git</span><span class="sxs-lookup"><span data-stu-id="38bd9-160">How to: Download files using the local Git repository</span></span>](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a><span data-ttu-id="38bd9-161">Postupy: konfigurace přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="38bd9-161">How to: Configure deployment credentials</span></span>
<span data-ttu-id="38bd9-162">Než si můžete stáhnout soubory z funkce aplikace pomocí protokol FTP nebo místní úložiště Git, je nutné nakonfigurovat vaše přihlašovací údaje pro přístup k webu.</span><span class="sxs-lookup"><span data-stu-id="38bd9-162">Before you can download files from your function app with FTP or local Git repository, you must configure your credentials to access the site.</span></span> <span data-ttu-id="38bd9-163">Přihlašovací údaje jsou nastavené na úrovni aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="38bd9-163">Credentials are set at the Function app level.</span></span> <span data-ttu-id="38bd9-164">Chcete-li nastavit přihlašovací údaje pro nasazení na portálu Azure pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="38bd9-164">Use the following steps to set deployment credentials in the Azure portal:</span></span>

1. <span data-ttu-id="38bd9-165">V aplikaci funkce v [portál Azure](https://portal.azure.com), klikněte na tlačítko **funkce** a **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="38bd9-165">In your function app in the [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment credentials**.</span></span>
   
    ![Nastavte přihlašovací údaje pro místní nasazení](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. <span data-ttu-id="38bd9-167">Zadejte uživatelské jméno a heslo a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="38bd9-167">Type in a username and password, then click **Save**.</span></span> <span data-ttu-id="38bd9-168">Nyní můžete tyto přihlašovací údaje pro přístup k vaší aplikaci funkce z protokol FTP nebo integrované úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="38bd9-168">You can now use these credentials to access your function app from FTP or the built-in Git repo.</span></span>

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a><span data-ttu-id="38bd9-169">Postupy: stažení souborů pomocí protokolu FTP</span><span class="sxs-lookup"><span data-stu-id="38bd9-169">How to: Download files using FTP</span></span>

1. <span data-ttu-id="38bd9-170">V aplikaci funkce v [portál Azure](https://portal.azure.com), klikněte na tlačítko **funkce** a **vlastnosti**, zkopírujte hodnoty **uživatel FTP/nasazení**, **název hostitele FTP**, a **FTPS název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="38bd9-170">In your function app in the [Azure portal](https://portal.azure.com), click **Platform features** and **Properties**, then copy the values for **FTP/Deployment User**, **FTP Host Name**, and **FTPS Host Name**.</span></span>  

    <span data-ttu-id="38bd9-171">**Uživatel FTP/nasazení** musí být zadané jako zobrazí na portálu, včetně názvu aplikace, zajistit správné kontextu pro FTP server.</span><span class="sxs-lookup"><span data-stu-id="38bd9-171">**FTP/Deployment User** must be entered as displayed in the portal, including the app name, to provide proper context for the FTP server.</span></span>
   
    ![Získat informace o nasazení](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. <span data-ttu-id="38bd9-173">Z vašeho klienta FTP, použijte informace o připojení jste shromáždili pro připojení k vaší aplikace a stáhnout zdrojové soubory pro funkce.</span><span class="sxs-lookup"><span data-stu-id="38bd9-173">From your FTP client, use the connection information you gathered to connect to your app and download the source files for your functions.</span></span>

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a><span data-ttu-id="38bd9-174">Postupy: stahovat soubory s použitím místní úložiště Git</span><span class="sxs-lookup"><span data-stu-id="38bd9-174">How to: Download files using a local Git repository</span></span>

1. <span data-ttu-id="38bd9-175">V aplikaci funkce v [portál Azure](https://portal.azure.com), klikněte na tlačítko **funkce** a **možnosti nasazení**.</span><span class="sxs-lookup"><span data-stu-id="38bd9-175">In your function app in the [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment options**.</span></span> 
   
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/setup-deployment.png)
 
2. <span data-ttu-id="38bd9-177">Potom v **nasazení** okně klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="38bd9-177">Then in the **Deployments** blade click **Setup**.</span></span>
 
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. <span data-ttu-id="38bd9-179">V **zdroj nasazení** okně klikněte na tlačítko **úložiště místní Git** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="38bd9-179">In the **Deployment source** blade, click **Local Git repository** and then click **OK**.</span></span>

3. <span data-ttu-id="38bd9-180">V **funkce**, klikněte na tlačítko **vlastnosti** a poznamenejte si hodnotu adresy URL Git.</span><span class="sxs-lookup"><span data-stu-id="38bd9-180">In **Platform features**, click **Properties** and note the value of Git URL.</span></span> 
   
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. <span data-ttu-id="38bd9-182">Naklonujte úložiště na místním počítači pomocí Git využívající příkazový řádek nebo vaše oblíbené nástroje Git.</span><span class="sxs-lookup"><span data-stu-id="38bd9-182">Clone the repository on your local machine using a Git-aware command prompt or your favorite Git tool.</span></span> <span data-ttu-id="38bd9-183">Příkaz Git clone vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="38bd9-183">The Git clone command looks like this:</span></span>
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. <span data-ttu-id="38bd9-184">Načítání souborů z aplikace funkce klonu v místním počítači, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="38bd9-184">Fetch files from your function app to the clone on your local computer, as in the following example:</span></span>
   
        git pull origin master
   
    <span data-ttu-id="38bd9-185">Pokud požadovaný, zadejte vaše [nakonfigurovat přihlašovací údaje nasazení](#credentials).</span><span class="sxs-lookup"><span data-stu-id="38bd9-185">If requested, supply your [configured deployment credentials](#credentials).</span></span>  

<span data-ttu-id="38bd9-186">[Githubu]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="38bd9-186">[GitHub]: https://github.com/</span></span>
