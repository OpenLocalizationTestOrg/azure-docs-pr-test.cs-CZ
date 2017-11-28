---
title: "aaaLocal nasazení Git tooAzure služby App Service"
description: "Zjistěte, jak tooenable místní Git nasazení tooAzure služby App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 1905e0b7acd58d8dd6496a14f6e4f38f9f8c0212
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="local-git-deployment-tooazure-app-service"></a><span data-ttu-id="6d94b-103">Místní nasazení Git tooAzure služby App Service</span><span class="sxs-lookup"><span data-stu-id="6d94b-103">Local Git Deployment tooAzure App Service</span></span>
<span data-ttu-id="6d94b-104">Tento kurz ukazuje, jak toodeploy aplikace příliš[Azure App Service] z úložiště Git v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="6d94b-104">This tutorial shows you how toodeploy your app too[Azure App Service] from a Git repository on your local computer.</span></span> <span data-ttu-id="6d94b-105">App Service podporuje tento přístup s hello **místní Git** možnost nasazení v hello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="6d94b-105">App Service supports this approach with hello **Local Git** deployment option in hello [Azure Portal].</span></span>  
<span data-ttu-id="6d94b-106">Mnoho příkazů Git hello popsané v tomto článku se provádí automaticky při vytváření aplikace služby App Service pomocí hello [rozhraní příkazového řádku Azure] popsané [zde](app-service-web-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6d94b-106">Many of hello Git commands described in this article are performed automatically when creating an App Service app using hello [Azure Command-Line Interface] as described [here](app-service-web-get-started.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6d94b-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6d94b-107">Prerequisites</span></span>
<span data-ttu-id="6d94b-108">toocomplete tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="6d94b-108">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="6d94b-109">Git.</span><span class="sxs-lookup"><span data-stu-id="6d94b-109">Git.</span></span> <span data-ttu-id="6d94b-110">Si můžete stáhnout hello instalace binární [zde](http://www.git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="6d94b-110">You can download hello installation binary [here](http://www.git-scm.com/downloads).</span></span>  
* <span data-ttu-id="6d94b-111">Základní znalosti o Git.</span><span class="sxs-lookup"><span data-stu-id="6d94b-111">Basic knowledge of Git.</span></span>
* <span data-ttu-id="6d94b-112">Účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6d94b-112">A Microsoft Azure account.</span></span> <span data-ttu-id="6d94b-113">Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).</span><span class="sxs-lookup"><span data-stu-id="6d94b-113">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).</span></span>

> [!NOTE]
> <span data-ttu-id="6d94b-114">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="6d94b-114">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter app in App Service.</span></span> <span data-ttu-id="6d94b-115">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="6d94b-115">No credit cards required; no commitments.</span></span>  
> 
> 

## <span data-ttu-id="6d94b-116"><a name="Step1"></a>Krok 1: Vytvoření místní úložiště</span><span class="sxs-lookup"><span data-stu-id="6d94b-116"><a name="Step1"></a>Step 1: Create a local repository</span></span>
<span data-ttu-id="6d94b-117">Proveďte následující úlohy toocreate nového úložiště Git hello.</span><span class="sxs-lookup"><span data-stu-id="6d94b-117">Perform hello following tasks toocreate a new Git repository.</span></span>

1. <span data-ttu-id="6d94b-118">Spusťte nástroj příkazového řádku, jako například **Git Bash** (Windows) nebo **Bash** (Unix prostředí).</span><span class="sxs-lookup"><span data-stu-id="6d94b-118">Start a command-line tool, such as **GitBash** (Windows) or **Bash** (Unix Shell).</span></span> <span data-ttu-id="6d94b-119">V systémech OS X můžete přistupovat prostřednictvím hello hello příkazového řádku **Terminálové** aplikace.</span><span class="sxs-lookup"><span data-stu-id="6d94b-119">On OS X systems you can access hello command-line through hello **Terminal** application.</span></span>
2. <span data-ttu-id="6d94b-120">Přejděte toohello adresáře, kde bude umístěn obsahu toodeploy hello.</span><span class="sxs-lookup"><span data-stu-id="6d94b-120">Navigate toohello directory where hello content toodeploy would be located.</span></span>
3. <span data-ttu-id="6d94b-121">Použijte následující příkaz tooinitialize nového úložiště Git hello:</span><span class="sxs-lookup"><span data-stu-id="6d94b-121">Use hello following command tooinitialize a new Git repository:</span></span>

```bash  
git init
```

## <span data-ttu-id="6d94b-122"><a name="Step2"></a>Krok 2: Potvrzení obsahu</span><span class="sxs-lookup"><span data-stu-id="6d94b-122"><a name="Step2"></a>Step 2: Commit your content</span></span>
<span data-ttu-id="6d94b-123">App Service podporuje aplikace vytvořené v různých programovacích jazyků.</span><span class="sxs-lookup"><span data-stu-id="6d94b-123">App Service supports applications created in a variety of programming languages.</span></span> 

1. <span data-ttu-id="6d94b-124">Pokud úložiště už obsahuje tento bod obsahu přeskočit a přesunout toopoint 2 níže.</span><span class="sxs-lookup"><span data-stu-id="6d94b-124">If your repository already includes content skip this point and move toopoint 2 below.</span></span> <span data-ttu-id="6d94b-125">Pokud úložiště už neobsahuje obsah jednoduše naplnění soubor statické .html následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6d94b-125">If your repository does not already include content simply populate with a static .html file as follows:</span></span> 
   
   * <span data-ttu-id="6d94b-126">Pomocí textového editoru, vytvořte nový soubor s názvem **index.html** v kořenovém adresáři úložiště Git hello hello</span><span class="sxs-lookup"><span data-stu-id="6d94b-126">Using a text editor, create a new file named **index.html** at hello root of hello Git repository</span></span>
   * <span data-ttu-id="6d94b-127">Přidejte následující text jako hello obsah pro hello index.html souboru a uložte ho hello: *Hello Git!*</span><span class="sxs-lookup"><span data-stu-id="6d94b-127">Add hello following text as hello contents for hello index.html file and save it: *Hello Git!*</span></span>
2. <span data-ttu-id="6d94b-128">Z příkazového řádku hello ověřte, že jste v kořenovém adresáři hello úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="6d94b-128">From hello command-line, verify that you are under hello root of your Git repository.</span></span> <span data-ttu-id="6d94b-129">Pak použijte následující příkaz tooadd soubory tooyour úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="6d94b-129">Then use hello following command tooadd files tooyour repository:</span></span>

```bash  
git add -A
```
3. <span data-ttu-id="6d94b-130">V dalším kroku potvrdit hello změny toohello úložiště pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6d94b-130">Next, commit hello changes toohello repository by using hello following command:</span></span>

```bash  
git commit -m "Hello Azure App Service"
```  

## <span data-ttu-id="6d94b-131"><a name="Step3"></a>Krok 3: Povolení hello úložiště aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="6d94b-131"><a name="Step3"></a>Step 3: Enable hello App Service app repository</span></span>
<span data-ttu-id="6d94b-132">Proveďte následující kroky tooenable úložiště Git pro vaši aplikaci služby App Service hello.</span><span class="sxs-lookup"><span data-stu-id="6d94b-132">Perform hello following steps tooenable a Git repository for your App Service app.</span></span>

1. <span data-ttu-id="6d94b-133">Přihlaste se toohello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="6d94b-133">Log in toohello [Azure Portal].</span></span>
2. <span data-ttu-id="6d94b-134">V okně aplikace App Service, klikněte na tlačítko **Nastavení > zdroj nasazení**.</span><span class="sxs-lookup"><span data-stu-id="6d94b-134">In your App Service app's blade, click **Settings > Deployment source**.</span></span> <span data-ttu-id="6d94b-135">Klikněte na tlačítko **vybrat zdroj**, pak klikněte na tlačítko **místní úložiště Git**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6d94b-135">Click **Choose source**, then click **Local Git Repository**, and then click **OK**.</span></span>  
   
    ![Místní úložiště Git](./media/app-service-deploy-local-git/local_git_selection.png)
3. <span data-ttu-id="6d94b-137">Pokud je vaše první čas nastavení úložiště v Azure, musíte pro něj toocreate přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="6d94b-137">If this is your first time setting up a repository in Azure, you need toocreate login credentials for it.</span></span> <span data-ttu-id="6d94b-138">Použijete je toolog do hello úložiště Azure a změny z místního úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="6d94b-138">You will use them toolog into hello Azure repository and push changes from your local Git repository.</span></span> <span data-ttu-id="6d94b-139">V okně vaší aplikace, klikněte na tlačítko **Nastavení > přihlašovací údaje nasazení**, nakonfigurujte nasazení uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="6d94b-139">From your app's blade, click **Settings > Deployment credentials**, then configure your deployment username and password.</span></span> <span data-ttu-id="6d94b-140">Když jste hotovi, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6d94b-140">When you're done, click **Save**.</span></span>
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <span data-ttu-id="6d94b-141"><a name="Step4"></a>Krok 4: Nasazení projektu</span><span class="sxs-lookup"><span data-stu-id="6d94b-141"><a name="Step4"></a>Step 4: Deploy your project</span></span>
<span data-ttu-id="6d94b-142">Pomocí následujících kroků toopublish hello tooApp vaší aplikace pomocí Git místní služby.</span><span class="sxs-lookup"><span data-stu-id="6d94b-142">Use hello following steps toopublish your app tooApp Service using Local Git.</span></span>

1. <span data-ttu-id="6d94b-143">V okně vaší aplikace v hello portálu Azure, klikněte na tlačítko **Nastavení > vlastnosti** pro hello **adresy URL pro Git**.</span><span class="sxs-lookup"><span data-stu-id="6d94b-143">In your app's blade in hello Azure Portal, click **Settings > Properties** for hello **Git URL**.</span></span>
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    <span data-ttu-id="6d94b-144">**Adresy URL pro Git** je hello vzdáleném referenčním toodeploy toofrom místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="6d94b-144">**Git URL** is hello remote reference toodeploy toofrom your local repository.</span></span> <span data-ttu-id="6d94b-145">Tato adresa URL budete používat v hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="6d94b-145">You'll use this URL in hello following steps.</span></span>
2. <span data-ttu-id="6d94b-146">Pomocí příkazového řádku hello, ověřte, že jste v kořenovém hello místní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="6d94b-146">Using hello command-line, verify that you are in hello root of your local Git repository.</span></span>
3. <span data-ttu-id="6d94b-147">Použití `git remote` tooadd hello vzdáleném referenčním uvedené v **adresy URL pro Git** z kroku 1.</span><span class="sxs-lookup"><span data-stu-id="6d94b-147">Use `git remote` tooadd hello remote reference listed in **Git URL** from step 1.</span></span> <span data-ttu-id="6d94b-148">Váš příkaz bude vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="6d94b-148">Your command will look similar toohello following:</span></span>
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > <span data-ttu-id="6d94b-149">Hello **vzdáleného** příkaz přidá tooa vzdáleného úložiště s názvem odkaz.</span><span class="sxs-lookup"><span data-stu-id="6d94b-149">hello **remote** command adds a named reference tooa remote repository.</span></span> <span data-ttu-id="6d94b-150">V tomto příkladu se vytvoří odkaz úložiště vaší webové aplikace s názvem "azure".</span><span class="sxs-lookup"><span data-stu-id="6d94b-150">In this example, it creates a reference named 'azure' for your web app's repository.</span></span>
   > 
   > 
4. <span data-ttu-id="6d94b-151">Push vaší obsahu tooApp služby pomocí nové hello **azure** vzdálené jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6d94b-151">Push your content tooApp Service using hello new **azure** remote you just created.</span></span>

```bash  
git push azure master
```
    You will be prompted for hello password you created earlier when you reset your deployment credentials in hello Azure Portal. Enter hello password (note that Gitbash does not echo asterisks toohello console as you type your password). 
5. <span data-ttu-id="6d94b-152">Vraťte se zpátky tooyour aplikace v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6d94b-152">Go back tooyour app in hello Azure Portal.</span></span> <span data-ttu-id="6d94b-153">Položka protokolu z vaší poslední nabízené má být zobrazena v hello **nasazení** okno.</span><span class="sxs-lookup"><span data-stu-id="6d94b-153">A log entry of your most recent push should be displayed in hello **Deployments** blade.</span></span> 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. <span data-ttu-id="6d94b-154">Klikněte na tlačítko hello **Procházet** tlačítko hello horní části aplikace hello okno tooverify hello obsahu byla nasazena.</span><span class="sxs-lookup"><span data-stu-id="6d94b-154">Click hello **Browse** button at hello top of hello app's blade tooverify hello content has been deployed.</span></span> 

## <span data-ttu-id="6d94b-155"><a name="Step5"></a>Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="6d94b-155"><a name="Step5"></a>Troubleshooting</span></span>
<span data-ttu-id="6d94b-156">Hello následují chyby nebo problémy, které jsou běžně došlo při použití Git toopublish tooan aplikaci služby App Service v Azure:</span><span class="sxs-lookup"><span data-stu-id="6d94b-156">hello following are errors or problems commonly encountered when using Git toopublish tooan App Service app in Azure:</span></span>

- - -
<span data-ttu-id="6d94b-157">**Příznaky**: nelze tooaccess [siteURL]: nepodařilo tooconnect příliš [scmAddress]</span><span class="sxs-lookup"><span data-stu-id="6d94b-157">**Symptom**: Unable tooaccess '[siteURL]': Failed tooconnect too[scmAddress]</span></span>

<span data-ttu-id="6d94b-158">**Příčina**: k této chybě může dojít, pokud aplikace hello není spuštěná.</span><span class="sxs-lookup"><span data-stu-id="6d94b-158">**Cause**: This error can occur if hello app is not up and running.</span></span>

<span data-ttu-id="6d94b-159">**Řešení**: spuštění aplikace hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6d94b-159">**Resolution**: Start hello app in hello Azure Portal.</span></span> <span data-ttu-id="6d94b-160">Nasazení Git nebude fungovat, pokud běží aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6d94b-160">Git deployment will not work unless hello app is running.</span></span> 

- - -
<span data-ttu-id="6d94b-161">**Příznaky**: nelze přeložit název hostitele, hostitele.</span><span class="sxs-lookup"><span data-stu-id="6d94b-161">**Symptom**: Couldn't resolve host 'hostname'</span></span>

<span data-ttu-id="6d94b-162">**Příčina**: k této chybě může dojít, pokud informace o adrese hello zadali při vytváření hello "azure" vzdálené nebyla správná.</span><span class="sxs-lookup"><span data-stu-id="6d94b-162">**Cause**: This error can occur if hello address information entered when creating hello 'azure' remote was incorrect.</span></span>

<span data-ttu-id="6d94b-163">**Řešení**: použití hello `git remote -v` příkaz toolist dálkové všechny ovladače, společně s hello přidružené adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6d94b-163">**Resolution**: Use hello `git remote -v` command toolist all remotes, along with hello associated URL.</span></span> <span data-ttu-id="6d94b-164">Ověřte, že adresa URL hello hello "azure" vzdálené správné.</span><span class="sxs-lookup"><span data-stu-id="6d94b-164">Verify that hello URL for hello 'azure' remote is correct.</span></span> <span data-ttu-id="6d94b-165">V případě potřeby odeberte a znovu tento vzdálený pomocí hello správnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6d94b-165">If needed, remove and recreate this remote using hello correct URL.</span></span>

- - -
<span data-ttu-id="6d94b-166">**Příznaky**: žádné odolný systém souborů v běžné a žádná zadaný; žádným způsobem.</span><span class="sxs-lookup"><span data-stu-id="6d94b-166">**Symptom**: No refs in common and none specified; doing nothing.</span></span> <span data-ttu-id="6d94b-167">Možná musíte zadat větve, jako je například "hlavní".</span><span class="sxs-lookup"><span data-stu-id="6d94b-167">Perhaps you should specify a branch such as 'master'.</span></span>

<span data-ttu-id="6d94b-168">**Příčina**: k této chybě může dojít, pokud jste nezadávejte větev při provádění operace git push a mít není sada hello push.default hodnota používaná metodou Git.</span><span class="sxs-lookup"><span data-stu-id="6d94b-168">**Cause**: This error can occur if you do not specify a branch when performing a git push operation, and have not set hello push.default value used by Git.</span></span>

<span data-ttu-id="6d94b-169">**Řešení**: hello nabízené znovu operaci provést, zadání hello hlavní větve.</span><span class="sxs-lookup"><span data-stu-id="6d94b-169">**Resolution**: Perform hello push operation again, specifying hello master branch.</span></span> <span data-ttu-id="6d94b-170">Například:</span><span class="sxs-lookup"><span data-stu-id="6d94b-170">For example:</span></span>

```bash  
git push azure master
```
- - -
<span data-ttu-id="6d94b-171">**Příznaky**: src refspec [branchname] neodpovídá žádnému.</span><span class="sxs-lookup"><span data-stu-id="6d94b-171">**Symptom**: src refspec [branchname] does not match any.</span></span>

<span data-ttu-id="6d94b-172">**Příčina**: této chybě může dojít, pokud se pokusíte větve tooa toopush než hlavní server na hello "azure" vzdálené.</span><span class="sxs-lookup"><span data-stu-id="6d94b-172">**Cause**: This error can occur if you attempt toopush tooa branch other than master on hello 'azure' remote.</span></span>

<span data-ttu-id="6d94b-173">**Řešení**: hello nabízené znovu operaci provést, zadání hello hlavní větve.</span><span class="sxs-lookup"><span data-stu-id="6d94b-173">**Resolution**: Perform hello push operation again, specifying hello master branch.</span></span> <span data-ttu-id="6d94b-174">Například:</span><span class="sxs-lookup"><span data-stu-id="6d94b-174">For example:</span></span>

```bash  
git push azure master
```
- - -
<span data-ttu-id="6d94b-175">**Příznaky**: RPC se nezdařilo; způsobit = 22, kód HTTP = 502.</span><span class="sxs-lookup"><span data-stu-id="6d94b-175">**Symptom**: RPC failed; result=22, HTTP code = 502.</span></span>

<span data-ttu-id="6d94b-176">**Příčina**: této chybě může dojít, pokud se pokusíte toopush úložiště git velké přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6d94b-176">**Cause**: This error can occur if you attempt toopush a large git repository over HTTPS.</span></span>

<span data-ttu-id="6d94b-177">**Řešení**: Změna konfigurace git hello na hello místního počítače toomake hello postBuffer větší</span><span class="sxs-lookup"><span data-stu-id="6d94b-177">**Resolution**: Change hello git configuration on hello local machine toomake hello postBuffer bigger</span></span>

```bash  
git config --global http.postBuffer 524288000
```
- - -
<span data-ttu-id="6d94b-178">**Příznaky**: Chyba: změny potvrzeny tooremote úložiště ale vaší webové aplikace nejsou aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="6d94b-178">**Symptom**: Error - Changes committed tooremote repository but your web app not updated.</span></span>

<span data-ttu-id="6d94b-179">**Příčina**: této chybě může dojít, pokud nasazujete aplikaci Node.js obsahující soubor package.json, který určuje další požadované moduly.</span><span class="sxs-lookup"><span data-stu-id="6d94b-179">**Cause**: This error can occur if you are deploying a Node.js app containing a package.json file that specifies additional required modules.</span></span>

<span data-ttu-id="6d94b-180">**Řešení**: další zprávy obsahující 'npm ERR!.</span><span class="sxs-lookup"><span data-stu-id="6d94b-180">**Resolution**: Additional messages containing 'npm ERR!'</span></span> <span data-ttu-id="6d94b-181">musí být zaznamenané toothis předchozí chyby a může poskytnout další kontext na selhání hello.</span><span class="sxs-lookup"><span data-stu-id="6d94b-181">should be logged prior toothis error, and can provide additional context on hello failure.</span></span> <span data-ttu-id="6d94b-182">Následující Hello se ví, že příčiny této chyby a hello odpovídající 'npm ERR!.</span><span class="sxs-lookup"><span data-stu-id="6d94b-182">hello following are known causes of this error and hello corresponding 'npm ERR!'</span></span> <span data-ttu-id="6d94b-183">zpráva:</span><span class="sxs-lookup"><span data-stu-id="6d94b-183">message:</span></span>

* <span data-ttu-id="6d94b-184">**Soubor package.json chybná**: npm ERR!</span><span class="sxs-lookup"><span data-stu-id="6d94b-184">**Malformed package.json file**: npm ERR!</span></span> <span data-ttu-id="6d94b-185">Nepodařilo se přečíst závislosti.</span><span class="sxs-lookup"><span data-stu-id="6d94b-185">Couldn't read dependencies.</span></span>
* <span data-ttu-id="6d94b-186">**Nativní modul, který nemá binární distribuce pro systém Windows**:</span><span class="sxs-lookup"><span data-stu-id="6d94b-186">**Native module that does not have a binary distribution for Windows**:</span></span>
  
  * <span data-ttu-id="6d94b-187">npm ERR!</span><span class="sxs-lookup"><span data-stu-id="6d94b-187">npm ERR!</span></span> <span data-ttu-id="6d94b-188">\`cmd "/ c" "uzlu gyp opětovné sestavení"\` se nezdařilo s 1</span><span class="sxs-lookup"><span data-stu-id="6d94b-188">\`cmd "/c" "node-gyp rebuild"\` failed with 1</span></span>
    
      <span data-ttu-id="6d94b-189">NEBO</span><span class="sxs-lookup"><span data-stu-id="6d94b-189">OR</span></span>
  * <span data-ttu-id="6d94b-190">npm ERR!</span><span class="sxs-lookup"><span data-stu-id="6d94b-190">npm ERR!</span></span> <span data-ttu-id="6d94b-191">[modulename@version] předinstalovat: \`zkontrolujte || gmake\`</span><span class="sxs-lookup"><span data-stu-id="6d94b-191">[modulename@version] preinstall: \`make || gmake\`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6d94b-192">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6d94b-192">Additional Resources</span></span>
* [<span data-ttu-id="6d94b-193">Dokumentace pro Git</span><span class="sxs-lookup"><span data-stu-id="6d94b-193">Git documentation</span></span>](http://git-scm.com/documentation)
* [<span data-ttu-id="6d94b-194">Dokumentace Kudu projektu</span><span class="sxs-lookup"><span data-stu-id="6d94b-194">Project Kudu documentation</span></span>](https://github.com/projectkudu/kudu/wiki)
* [<span data-ttu-id="6d94b-195">Nepřetržité nasazení tooAzure služby App Service</span><span class="sxs-lookup"><span data-stu-id="6d94b-195">Continous Deployment tooAzure App Service</span></span>](app-service-continuous-deployment.md)
* [<span data-ttu-id="6d94b-196">Jak toouse prostředí PowerShell pro Azure</span><span class="sxs-lookup"><span data-stu-id="6d94b-196">How toouse PowerShell for Azure</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="6d94b-197">Jak toouse hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="6d94b-197">How toouse hello Azure Command-Line Interface</span></span>](../cli-install-nodejs.md)

[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[portálu Azure]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[rozhraní příkazového řádku Azure]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
