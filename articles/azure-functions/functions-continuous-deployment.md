---
title: "aaaContinuous nasazení pro Azure Functions | Microsoft Docs"
description: "Průběžné nasazování vlastnosti služby Azure App Service toopublish použijte Azure Functions."
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
ms.openlocfilehash: 28c44f737dad3feab3cf54f7dd42b6a978d0617e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-for-azure-functions"></a><span data-ttu-id="699ba-103">Průběžné nasazování se službou Azure Functions</span><span class="sxs-lookup"><span data-stu-id="699ba-103">Continuous deployment for Azure Functions</span></span>
<span data-ttu-id="699ba-104">Azure Functions umožňuje snadno toodeploy funkce aplikace pomocí průběžnou integraci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="699ba-104">Azure Functions makes it easy toodeploy your function app using App Service continuous integration.</span></span> <span data-ttu-id="699ba-105">Funkce se integruje s BitBucket, Dropbox, Githubu a Visual Studio Team Services (VSTS).</span><span class="sxs-lookup"><span data-stu-id="699ba-105">Functions integrates with BitBucket, Dropbox, GitHub, and Visual Studio Team Services (VSTS).</span></span> <span data-ttu-id="699ba-106">To umožňuje pracovní kde kód funkce aktualizace provedené pomocí jedné z těchto tooAzure nasazení aktivační událost integrovaných služeb.</span><span class="sxs-lookup"><span data-stu-id="699ba-106">This enables a workflow where function code updates made by using one of these integrated services trigger deployment tooAzure.</span></span> <span data-ttu-id="699ba-107">Pokud jste nové funkce tooAzure, začínat [přehled Azure Functions](functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="699ba-107">If you are new tooAzure Functions, start with [Azure Functions Overview](functions-overview.md).</span></span>

<span data-ttu-id="699ba-108">Průběžné nasazování je skvělou možností pro projekty, u kterých se integruje více příspěvků nebo u kterých integrace probíhá často.</span><span class="sxs-lookup"><span data-stu-id="699ba-108">Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated.</span></span> <span data-ttu-id="699ba-109">To také umožňuje udržovat zdrojového kódu na váš kód funkce.</span><span class="sxs-lookup"><span data-stu-id="699ba-109">It also lets you maintain source control on your functions code.</span></span> <span data-ttu-id="699ba-110">Hello následující nasazení zdroje jsou aktuálně podporovány:</span><span class="sxs-lookup"><span data-stu-id="699ba-110">hello following deployment sources are currently supported:</span></span>

* [<span data-ttu-id="699ba-111">Bitbucket</span><span class="sxs-lookup"><span data-stu-id="699ba-111">Bitbucket</span></span>](https://bitbucket.org/)
* [<span data-ttu-id="699ba-112">Dropbox</span><span class="sxs-lookup"><span data-stu-id="699ba-112">Dropbox</span></span>](https://www.dropbox.com/)
* <span data-ttu-id="699ba-113">Externího úložiště (Git nebo Mercurial)</span><span class="sxs-lookup"><span data-stu-id="699ba-113">External repository (Git or Mercurial)</span></span>
* [<span data-ttu-id="699ba-114">Místní úložiště Git</span><span class="sxs-lookup"><span data-stu-id="699ba-114">Git local repository</span></span>](../app-service-web/app-service-deploy-local-git.md)
* [<span data-ttu-id="699ba-115">GitHub</span><span class="sxs-lookup"><span data-stu-id="699ba-115">GitHub</span></span>](https://github.com)
* [<span data-ttu-id="699ba-116">OneDrive</span><span class="sxs-lookup"><span data-stu-id="699ba-116">OneDrive</span></span>](https://onedrive.live.com/)
* [<span data-ttu-id="699ba-117">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="699ba-117">Visual Studio Team Services</span></span>](https://www.visualstudio.com/team-services/)

<span data-ttu-id="699ba-118">Nasazení jsou nakonfigurované na základě aplikací na funkce.</span><span class="sxs-lookup"><span data-stu-id="699ba-118">Deployments are configured on a per-function app basis.</span></span> <span data-ttu-id="699ba-119">Po povolení průběžné nasazování kód toofunction přístup k portálu hello je nastaven příliš*jen pro čtení*.</span><span class="sxs-lookup"><span data-stu-id="699ba-119">After continuous deployment is enabled, access toofunction code in hello portal is set too*read-only*.</span></span>

## <a name="continuous-deployment-requirements"></a><span data-ttu-id="699ba-120">Průběžné nasazování požadavky</span><span class="sxs-lookup"><span data-stu-id="699ba-120">Continuous deployment requirements</span></span>

<span data-ttu-id="699ba-121">Váš zdroj nasazení nakonfigurované a váš kód funkce musí mít v zdroj nasazení hello před nastavit průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="699ba-121">You must have your deployment source configured and your functions code in hello deployment source before you set up continuous deployment.</span></span> <span data-ttu-id="699ba-122">V nasazení aplikace danou funkci jednotlivé funkce žije v podadresáři s názvem, kde je název adresáře hello hello název funkce hello.</span><span class="sxs-lookup"><span data-stu-id="699ba-122">In a given function app deployment, each function lives in a named subdirectory, where hello directory name is hello name of hello function.</span></span>  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="set-up-continuous-deployment"></a><span data-ttu-id="699ba-123">Nastavení nepřetržitého nasazování</span><span class="sxs-lookup"><span data-stu-id="699ba-123">Set up continuous deployment</span></span>
<span data-ttu-id="699ba-124">Použijte tento postup tooconfigure průběžné nasazování pro existující aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="699ba-124">Use this procedure tooconfigure continuous deployment for an existing function app.</span></span> <span data-ttu-id="699ba-125">Tyto kroky ukazují integrace s úložišti GitHub, ale podobným způsobem platí pro Visual Studio Team Services nebo jiné služby pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="699ba-125">These steps demonstrate integration with a GitHub repository, but similar steps apply for Visual Studio Team Services or other deployment services.</span></span>

1. <span data-ttu-id="699ba-126">V aplikaci funkce v hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **funkce** a **možnosti nasazení**.</span><span class="sxs-lookup"><span data-stu-id="699ba-126">In your function app in hello [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment options**.</span></span> 
   
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/setup-deployment.png)
 
2. <span data-ttu-id="699ba-128">Potom v hello **nasazení** okně klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="699ba-128">Then in hello **Deployments** blade click **Setup**.</span></span>
 
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. <span data-ttu-id="699ba-130">V hello **zdroj nasazení** okně klikněte na tlačítko **vybrat zdroj**, potom vyplňte hello informace pro váš zdroj vybraný nasazení a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="699ba-130">In hello **Deployment source** blade, click **Choose source**, then fill in hello information for your chosen deployment source and click **OK**.</span></span>
   
    ![Vyberte zdroj nasazení.](./media/functions-continuous-deployment/choose-deployment-source.png)

<span data-ttu-id="699ba-132">Po dokončení konfigurace průběžné nasazování, všechny změny souboru ve zdroji nasazení jsou zkopírovaný toohello funkce aplikace a nasazení plnou verzi webu se aktivuje.</span><span class="sxs-lookup"><span data-stu-id="699ba-132">After continuous deployment is configured, all file changes in your deployment source are copied toohello function app and a full site deployment is triggered.</span></span> <span data-ttu-id="699ba-133">Hello lokality je znovu nasazena, když jsou aktualizovány soubory ve zdroji hello.</span><span class="sxs-lookup"><span data-stu-id="699ba-133">hello site is redeployed when files in hello source are updated.</span></span>

## <a name="deployment-options"></a><span data-ttu-id="699ba-134">Možnosti nasazení</span><span class="sxs-lookup"><span data-stu-id="699ba-134">Deployment options</span></span>

<span data-ttu-id="699ba-135">Hello Následují některé typické nasazení scénáře:</span><span class="sxs-lookup"><span data-stu-id="699ba-135">hello following are some typical deployment scenarios:</span></span>

- [<span data-ttu-id="699ba-136">Vytvořit pracovní nasazení</span><span class="sxs-lookup"><span data-stu-id="699ba-136">Create a staging deployment</span></span>](#staging)
- [<span data-ttu-id="699ba-137">Přesunutí existující funkce toocontinuous nasazení</span><span class="sxs-lookup"><span data-stu-id="699ba-137">Move existing functions toocontinuous deployment</span></span>](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a><span data-ttu-id="699ba-138">Vytvořit pracovní nasazení</span><span class="sxs-lookup"><span data-stu-id="699ba-138">Create a staging deployment</span></span>

<span data-ttu-id="699ba-139">Funkce aplikace ještě nepodporuje nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="699ba-139">Function Apps doesn't yet support deployment slots.</span></span> <span data-ttu-id="699ba-140">Samostatná pracovní a provozní nasazení však můžete spravovat pomocí nepřetržité integrace.</span><span class="sxs-lookup"><span data-stu-id="699ba-140">However, you can still manage separate staging and production deployments by using continuous integration.</span></span>

<span data-ttu-id="699ba-141">Hello tooconfigure procesu a pracovat s pracovní nasazení obecně vypadat třeba takto:</span><span class="sxs-lookup"><span data-stu-id="699ba-141">hello process tooconfigure and work with a staging deployment looks generally like this:</span></span>

1. <span data-ttu-id="699ba-142">Vaše předplatné, jeden pro hello produkčním kódu a jeden pro pracovní vytvořte dvě funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="699ba-142">Create two function apps in your subscription, one for hello production code and one for staging.</span></span> 

2. <span data-ttu-id="699ba-143">Pokud jste ještě nemáte, vytvořte zdroj nasazení.</span><span class="sxs-lookup"><span data-stu-id="699ba-143">Create a deployment source, if you don't already have one.</span></span> <span data-ttu-id="699ba-144">Tento příklad používá [Githubu].</span><span class="sxs-lookup"><span data-stu-id="699ba-144">This example uses [GitHub].</span></span>

3. <span data-ttu-id="699ba-145">Pro funkce aplikace produkční dokončení hello předchozí kroky **nastavit průběžné nasazování** a sadu hello nasazení větve toohello hlavní větve ve vašem úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="699ba-145">For your production function app, complete hello preceding steps in **Set up continuous deployment** and set hello deployment branch toohello master branch of your GitHub repository.</span></span>
   
    ![Vyberte větev nasazení](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. <span data-ttu-id="699ba-147">Tento krok opakujte pro hello pracovní funkce aplikace, ale zvolte hello pracovní větev místo ve vašem úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="699ba-147">Repeat this step for hello staging function app, but choose hello staging branch instead in your GitHub repo.</span></span> <span data-ttu-id="699ba-148">Pokud vaše zdrojová nasazení nepodporuje vytvoření větve, použijte jinou složku.</span><span class="sxs-lookup"><span data-stu-id="699ba-148">If your deployment source doesn't support branching, use a different folder.</span></span>
    
5. <span data-ttu-id="699ba-149">Proveďte aktualizace tooyour kódu v hello pracovní větev nebo složku a potom ověřte, že tyto změny se projeví v hello pracovní nasazení.</span><span class="sxs-lookup"><span data-stu-id="699ba-149">Make updates tooyour code in hello staging branch or folder, then verify that those changes are reflected in hello staging deployment.</span></span>

6. <span data-ttu-id="699ba-150">Po testování sloučit změny z hello pracovní větve do hlavní větve hello.</span><span class="sxs-lookup"><span data-stu-id="699ba-150">After testing, merge changes from hello staging branch into hello master branch.</span></span> <span data-ttu-id="699ba-151">Tato sloučení aktivuje nasazení toohello produkční funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="699ba-151">This merge triggers deployment toohello production function app.</span></span> <span data-ttu-id="699ba-152">Pokud vaše zdrojová nasazení nepodporuje větví, přepište hello souborů ve složce produkční hello hello soubory z hello pracovní složku.</span><span class="sxs-lookup"><span data-stu-id="699ba-152">If your deployment source doesn't support branches, overwrite hello files in hello production folder with hello files from hello staging folder.</span></span>

<a name="existing"></a>
### <a name="move-existing-functions-toocontinuous-deployment"></a><span data-ttu-id="699ba-153">Přesunutí existující funkce toocontinuous nasazení</span><span class="sxs-lookup"><span data-stu-id="699ba-153">Move existing functions toocontinuous deployment</span></span>
<span data-ttu-id="699ba-154">Pokud máte existující funkce, které jste vytvořili a udržovat hello portálu, je nutné toodownload stávající funkce soubory kódu pomocí protokol FTP nebo hello místní úložiště Git před průběžné nasazování můžete nastavit, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="699ba-154">When you have existing functions that you have created and maintained in hello portal, you need toodownload your existing function code files using FTP or hello local Git repository before you can set up continuous deployment as described above.</span></span> <span data-ttu-id="699ba-155">To provedete v hello nastavení služby App Service pro vaši aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="699ba-155">You can do this in hello App Service settings for your function app.</span></span> <span data-ttu-id="699ba-156">Po stažení jsou vaše soubory, můžete je načíst zdroj tooyour vybrali průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="699ba-156">After your files are downloaded, you can upload them tooyour chosen continuous deployment source.</span></span>

> [!NOTE]
> <span data-ttu-id="699ba-157">Po dokončení konfigurace průběžnou integraci, už nebudete moct tooedit vaší zdrojové soubory hello funkce portálu.</span><span class="sxs-lookup"><span data-stu-id="699ba-157">After you configure continuous integration, you will no longer be able tooedit your source files in hello Functions portal.</span></span>

- [<span data-ttu-id="699ba-158">Postupy: konfigurace přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="699ba-158">How to: Configure deployment credentials</span></span>](#credentials)
- [<span data-ttu-id="699ba-159">Postupy: stažení souborů pomocí protokolu FTP</span><span class="sxs-lookup"><span data-stu-id="699ba-159">How to: Download files using FTP</span></span>](#downftp)
- [<span data-ttu-id="699ba-160">Postupy: stahovat soubory s použitím hello místní úložiště Git</span><span class="sxs-lookup"><span data-stu-id="699ba-160">How to: Download files using hello local Git repository</span></span>](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a><span data-ttu-id="699ba-161">Postupy: konfigurace přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="699ba-161">How to: Configure deployment credentials</span></span>
<span data-ttu-id="699ba-162">Před soubory si můžete stáhnout z funkce aplikace pomocí protokol FTP nebo místní úložiště Git, je nutné nakonfigurovat lokalitu hello tooaccess přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="699ba-162">Before you can download files from your function app with FTP or local Git repository, you must configure your credentials tooaccess hello site.</span></span> <span data-ttu-id="699ba-163">Přihlašovací údaje jsou nastavené na úrovni aplikace hello funkce.</span><span class="sxs-lookup"><span data-stu-id="699ba-163">Credentials are set at hello Function app level.</span></span> <span data-ttu-id="699ba-164">Použijte následující kroky přihlašovací údaje nasazení tooset v hello portál Azure hello:</span><span class="sxs-lookup"><span data-stu-id="699ba-164">Use hello following steps tooset deployment credentials in hello Azure portal:</span></span>

1. <span data-ttu-id="699ba-165">V aplikaci funkce v hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **funkce** a **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="699ba-165">In your function app in hello [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment credentials**.</span></span>
   
    ![Nastavte přihlašovací údaje pro místní nasazení](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. <span data-ttu-id="699ba-167">Zadejte uživatelské jméno a heslo a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="699ba-167">Type in a username and password, then click **Save**.</span></span> <span data-ttu-id="699ba-168">Nyní můžete tyto přihlašovací údaje tooaccess funkce aplikaci z protokol FTP nebo hello integrované úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="699ba-168">You can now use these credentials tooaccess your function app from FTP or hello built-in Git repo.</span></span>

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a><span data-ttu-id="699ba-169">Postupy: stažení souborů pomocí protokolu FTP</span><span class="sxs-lookup"><span data-stu-id="699ba-169">How to: Download files using FTP</span></span>

1. <span data-ttu-id="699ba-170">V aplikaci funkce v hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **funkce** a **vlastnosti**, zkopírujte hodnoty hello **uživatel FTP/nasazení**, **Název hostitele FTP**, a **název hostitele FTPS**.</span><span class="sxs-lookup"><span data-stu-id="699ba-170">In your function app in hello [Azure portal](https://portal.azure.com), click **Platform features** and **Properties**, then copy hello values for **FTP/Deployment User**, **FTP Host Name**, and **FTPS Host Name**.</span></span>  

    <span data-ttu-id="699ba-171">**Uživatel FTP/nasazení** musí zadat, jak je uvedeno v hello portál, včetně názvu aplikace hello, tooprovide správný kontext hello FTP serveru.</span><span class="sxs-lookup"><span data-stu-id="699ba-171">**FTP/Deployment User** must be entered as displayed in hello portal, including hello app name, tooprovide proper context for hello FTP server.</span></span>
   
    ![Získat informace o nasazení](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. <span data-ttu-id="699ba-173">Z vašeho klienta FTP hello připojení informací, které jste shromáždili tooconnect tooyour aplikace a stáhnout hello zdrojové soubory pro funkce.</span><span class="sxs-lookup"><span data-stu-id="699ba-173">From your FTP client, use hello connection information you gathered tooconnect tooyour app and download hello source files for your functions.</span></span>

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a><span data-ttu-id="699ba-174">Postupy: stahovat soubory s použitím místní úložiště Git</span><span class="sxs-lookup"><span data-stu-id="699ba-174">How to: Download files using a local Git repository</span></span>

1. <span data-ttu-id="699ba-175">V aplikaci funkce v hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **funkce** a **možnosti nasazení**.</span><span class="sxs-lookup"><span data-stu-id="699ba-175">In your function app in hello [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment options**.</span></span> 
   
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/setup-deployment.png)
 
2. <span data-ttu-id="699ba-177">Potom v hello **nasazení** okně klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="699ba-177">Then in hello **Deployments** blade click **Setup**.</span></span>
 
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. <span data-ttu-id="699ba-179">V hello **zdroj nasazení** okně klikněte na tlačítko **úložiště místní Git** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="699ba-179">In hello **Deployment source** blade, click **Local Git repository** and then click **OK**.</span></span>

3. <span data-ttu-id="699ba-180">V **funkce**, klikněte na tlačítko **vlastnosti** a poznamenejte si hodnotu hello adresu URL pro Git.</span><span class="sxs-lookup"><span data-stu-id="699ba-180">In **Platform features**, click **Properties** and note hello value of Git URL.</span></span> 
   
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. <span data-ttu-id="699ba-182">Klon hello úložiště na místním počítači pomocí Git využívající příkazový řádek nebo vaše oblíbené nástroje Git.</span><span class="sxs-lookup"><span data-stu-id="699ba-182">Clone hello repository on your local machine using a Git-aware command prompt or your favorite Git tool.</span></span> <span data-ttu-id="699ba-183">Hello příkaz Git clone vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="699ba-183">hello Git clone command looks like this:</span></span>
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. <span data-ttu-id="699ba-184">Načtení souborů z vaší funkce aplikace toohello klonu v místním počítači, jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="699ba-184">Fetch files from your function app toohello clone on your local computer, as in hello following example:</span></span>
   
        git pull origin master
   
    <span data-ttu-id="699ba-185">Pokud požadovaný, zadejte vaše [nakonfigurovat přihlašovací údaje nasazení](#credentials).</span><span class="sxs-lookup"><span data-stu-id="699ba-185">If requested, supply your [configured deployment credentials](#credentials).</span></span>  

[Githubu]: https://github.com/
