---
title: "Místní nasazení z Gitu do služby Azure App Service"
description: "Zjistěte, jak povolit místní nasazení Git do služby Azure App Service."
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
ms.openlocfilehash: f1c4911670d3aa32e74b3dfebd83cf3dc3830805
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="local-git-deployment-to-azure-app-service"></a><span data-ttu-id="08217-103">Místní nasazení z Gitu do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="08217-103">Local Git Deployment to Azure App Service</span></span>
<span data-ttu-id="08217-104">V tomto kurzu se dozvíte, jak nasadit aplikaci, kterou chcete [Azure App Service] z úložiště Git v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="08217-104">This tutorial shows you how to deploy your app to [Azure App Service] from a Git repository on your local computer.</span></span> <span data-ttu-id="08217-105">App Service podporuje tento přístup s **místní Git** v možnost nasazení [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="08217-105">App Service supports this approach with the **Local Git** deployment option in the [Azure Portal].</span></span>  
<span data-ttu-id="08217-106">Mnoho příkazů Git popsané v tomto článku se provádí automaticky při vytváření aplikace služby App Service pomocí [rozhraní příkazového řádku Azure] popsané [zde](app-service-web-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="08217-106">Many of the Git commands described in this article are performed automatically when creating an App Service app using the [Azure Command-Line Interface] as described [here](app-service-web-get-started.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08217-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="08217-107">Prerequisites</span></span>
<span data-ttu-id="08217-108">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="08217-108">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="08217-109">Git.</span><span class="sxs-lookup"><span data-stu-id="08217-109">Git.</span></span> <span data-ttu-id="08217-110">Můžete si stáhnout binární instalace [zde](http://www.git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="08217-110">You can download the installation binary [here](http://www.git-scm.com/downloads).</span></span>  
* <span data-ttu-id="08217-111">Základní znalosti o Git.</span><span class="sxs-lookup"><span data-stu-id="08217-111">Basic knowledge of Git.</span></span>
* <span data-ttu-id="08217-112">Účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="08217-112">A Microsoft Azure account.</span></span> <span data-ttu-id="08217-113">Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).</span><span class="sxs-lookup"><span data-stu-id="08217-113">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).</span></span>

> [!NOTE]
> <span data-ttu-id="08217-114">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte na [vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="08217-114">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter app in App Service.</span></span> <span data-ttu-id="08217-115">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="08217-115">No credit cards required; no commitments.</span></span>  
> 
> 

## <span data-ttu-id="08217-116"><a name="Step1"></a>Krok 1: Vytvoření místní úložiště</span><span class="sxs-lookup"><span data-stu-id="08217-116"><a name="Step1"></a>Step 1: Create a local repository</span></span>
<span data-ttu-id="08217-117">Proveďte následující kroky k vytvoření nového úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="08217-117">Perform the following tasks to create a new Git repository.</span></span>

1. <span data-ttu-id="08217-118">Spusťte nástroj příkazového řádku, jako například **Git Bash** (Windows) nebo **Bash** (Unix prostředí).</span><span class="sxs-lookup"><span data-stu-id="08217-118">Start a command-line tool, such as **GitBash** (Windows) or **Bash** (Unix Shell).</span></span> <span data-ttu-id="08217-119">V systémech OS X dostanete příkazového řádku pomocí **Terminálové** aplikace.</span><span class="sxs-lookup"><span data-stu-id="08217-119">On OS X systems you can access the command-line through the **Terminal** application.</span></span>
2. <span data-ttu-id="08217-120">Přejděte do adresáře, kde by umístění obsahu pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="08217-120">Navigate to the directory where the content to deploy would be located.</span></span>
3. <span data-ttu-id="08217-121">Použijte následující příkaz k chybě při inicializaci nového úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="08217-121">Use the following command to initialize a new Git repository:</span></span>

```bash  
git init
```

## <span data-ttu-id="08217-122"><a name="Step2"></a>Krok 2: Potvrzení obsahu</span><span class="sxs-lookup"><span data-stu-id="08217-122"><a name="Step2"></a>Step 2: Commit your content</span></span>
<span data-ttu-id="08217-123">App Service podporuje aplikace vytvořené v různých programovacích jazyků.</span><span class="sxs-lookup"><span data-stu-id="08217-123">App Service supports applications created in a variety of programming languages.</span></span> 

1. <span data-ttu-id="08217-124">Pokud úložiště už obsahuje obsahu Přeskočit tento bod a přesunout do bodu 2 níže.</span><span class="sxs-lookup"><span data-stu-id="08217-124">If your repository already includes content skip this point and move to point 2 below.</span></span> <span data-ttu-id="08217-125">Pokud úložiště už neobsahuje obsah jednoduše naplnění soubor statické .html následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08217-125">If your repository does not already include content simply populate with a static .html file as follows:</span></span> 
   
   * <span data-ttu-id="08217-126">Pomocí textového editoru, vytvořte nový soubor s názvem **index.html** v kořenovém adresáři úložiště Git</span><span class="sxs-lookup"><span data-stu-id="08217-126">Using a text editor, create a new file named **index.html** at the root of the Git repository</span></span>
   * <span data-ttu-id="08217-127">Přidejte následující text jako obsah pro index.html souboru a uložte jej: *Hello Git!*</span><span class="sxs-lookup"><span data-stu-id="08217-127">Add the following text as the contents for the index.html file and save it: *Hello Git!*</span></span>
2. <span data-ttu-id="08217-128">Z příkazového řádku ověřte, že jste v kořenovém adresáři úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="08217-128">From the command-line, verify that you are under the root of your Git repository.</span></span> <span data-ttu-id="08217-129">Pak použijte následující příkaz pro přidání souborů do úložiště:</span><span class="sxs-lookup"><span data-stu-id="08217-129">Then use the following command to add files to your repository:</span></span>

```bash  
git add -A
```
3. <span data-ttu-id="08217-130">V dalším kroku potvrďte změny do úložiště pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="08217-130">Next, commit the changes to the repository by using the following command:</span></span>

```bash  
git commit -m "Hello Azure App Service"
```  

## <span data-ttu-id="08217-131"><a name="Step3"></a>Krok 3: Povolení úložišti aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="08217-131"><a name="Step3"></a>Step 3: Enable the App Service app repository</span></span>
<span data-ttu-id="08217-132">Pomocí následujících kroků povolte úložiště Git pro vaši aplikaci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="08217-132">Perform the following steps to enable a Git repository for your App Service app.</span></span>

1. <span data-ttu-id="08217-133">Přihlaste se k [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="08217-133">Log in to the [Azure Portal].</span></span>
2. <span data-ttu-id="08217-134">V okně aplikace App Service, klikněte na tlačítko **Nastavení > zdroj nasazení**.</span><span class="sxs-lookup"><span data-stu-id="08217-134">In your App Service app's blade, click **Settings > Deployment source**.</span></span> <span data-ttu-id="08217-135">Klikněte na tlačítko **vybrat zdroj**, pak klikněte na tlačítko **místní úložiště Git**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="08217-135">Click **Choose source**, then click **Local Git Repository**, and then click **OK**.</span></span>  
   
    ![Místní úložiště Git](./media/app-service-deploy-local-git/local_git_selection.png)
3. <span data-ttu-id="08217-137">Pokud je vaše první čas nastavení úložiště v Azure, budete muset vytvořit přihlašovací údaje pro ni.</span><span class="sxs-lookup"><span data-stu-id="08217-137">If this is your first time setting up a repository in Azure, you need to create login credentials for it.</span></span> <span data-ttu-id="08217-138">Budete je používat k přihlášení do Azure změny úložiště a posílejte nabízená oznámení z místního úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="08217-138">You will use them to log into the Azure repository and push changes from your local Git repository.</span></span> <span data-ttu-id="08217-139">V okně vaší aplikace, klikněte na tlačítko **Nastavení > přihlašovací údaje nasazení**, nakonfigurujte nasazení uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="08217-139">From your app's blade, click **Settings > Deployment credentials**, then configure your deployment username and password.</span></span> <span data-ttu-id="08217-140">Když jste hotovi, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="08217-140">When you're done, click **Save**.</span></span>
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <span data-ttu-id="08217-141"><a name="Step4"></a>Krok 4: Nasazení projektu</span><span class="sxs-lookup"><span data-stu-id="08217-141"><a name="Step4"></a>Step 4: Deploy your project</span></span>
<span data-ttu-id="08217-142">Pomocí následujících kroků k publikování aplikace do služby App Service pomocí místní Git.</span><span class="sxs-lookup"><span data-stu-id="08217-142">Use the following steps to publish your app to App Service using Local Git.</span></span>

1. <span data-ttu-id="08217-143">V okně vaší aplikace na portálu Azure, klikněte na tlačítko **Nastavení > vlastnosti** pro **adresy URL pro Git**.</span><span class="sxs-lookup"><span data-stu-id="08217-143">In your app's blade in the Azure Portal, click **Settings > Properties** for the **Git URL**.</span></span>
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    <span data-ttu-id="08217-144">**Adresy URL pro Git** je vzdálený odkaz k nasazení z místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="08217-144">**Git URL** is the remote reference to deploy to from your local repository.</span></span> <span data-ttu-id="08217-145">V následujících krocích použijete tuto adresu URL.</span><span class="sxs-lookup"><span data-stu-id="08217-145">You'll use this URL in the following steps.</span></span>
2. <span data-ttu-id="08217-146">Pomocí příkazového řádku, ověřte, že jste v kořenu místní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="08217-146">Using the command-line, verify that you are in the root of your local Git repository.</span></span>
3. <span data-ttu-id="08217-147">Použití `git remote` přidat odkaz na vzdálené uvedené v **adresy URL pro Git** z kroku 1.</span><span class="sxs-lookup"><span data-stu-id="08217-147">Use `git remote` to add the remote reference listed in **Git URL** from step 1.</span></span> <span data-ttu-id="08217-148">Váš příkaz bude vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="08217-148">Your command will look similar to the following:</span></span>
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > <span data-ttu-id="08217-149">**Vzdáleného** příkaz přidá odkaz s názvem do vzdáleného úložiště.</span><span class="sxs-lookup"><span data-stu-id="08217-149">The **remote** command adds a named reference to a remote repository.</span></span> <span data-ttu-id="08217-150">V tomto příkladu se vytvoří odkaz úložiště vaší webové aplikace s názvem "azure".</span><span class="sxs-lookup"><span data-stu-id="08217-150">In this example, it creates a reference named 'azure' for your web app's repository.</span></span>
   > 
   > 
4. <span data-ttu-id="08217-151">Push svůj obsah do služby App Service pomocí nové **azure** vzdálené jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="08217-151">Push your content to App Service using the new **azure** remote you just created.</span></span>

```bash  
git push azure master
```
    You will be prompted for the password you created earlier when you reset your deployment credentials in the Azure Portal. Enter the password (note that Gitbash does not echo asterisks to the console as you type your password). 
5. <span data-ttu-id="08217-152">Vraťte se zpátky do vaší aplikace na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="08217-152">Go back to your app in the Azure Portal.</span></span> <span data-ttu-id="08217-153">Položka protokolu z vaší poslední nabízené má být zobrazena v **nasazení** okno.</span><span class="sxs-lookup"><span data-stu-id="08217-153">A log entry of your most recent push should be displayed in the **Deployments** blade.</span></span> 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. <span data-ttu-id="08217-154">Klikněte **Procházet** tlačítka v horní části okna aplikace k ověření nasazený obsah.</span><span class="sxs-lookup"><span data-stu-id="08217-154">Click the **Browse** button at the top of the app's blade to verify the content has been deployed.</span></span> 

## <span data-ttu-id="08217-155"><a name="Step5"></a>Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="08217-155"><a name="Step5"></a>Troubleshooting</span></span>
<span data-ttu-id="08217-156">Tady jsou chyby nebo problémy, které jsou běžně došlo při použití Git pro publikování do aplikace služby App Service v Azure:</span><span class="sxs-lookup"><span data-stu-id="08217-156">The following are errors or problems commonly encountered when using Git to publish to an App Service app in Azure:</span></span>

- - -
<span data-ttu-id="08217-157">**Příznaky**: Nelze získat přístup k [siteURL]: Nepodařilo se připojit k [scmAddress]</span><span class="sxs-lookup"><span data-stu-id="08217-157">**Symptom**: Unable to access '[siteURL]': Failed to connect to [scmAddress]</span></span>

<span data-ttu-id="08217-158">**Příčina**: této chybě může dojít, pokud aplikace není spuštěná.</span><span class="sxs-lookup"><span data-stu-id="08217-158">**Cause**: This error can occur if the app is not up and running.</span></span>

<span data-ttu-id="08217-159">**Řešení**: Spusťte aplikaci na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="08217-159">**Resolution**: Start the app in the Azure Portal.</span></span> <span data-ttu-id="08217-160">Nasazení Git nebude fungovat, pokud aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="08217-160">Git deployment will not work unless the app is running.</span></span> 

- - -
<span data-ttu-id="08217-161">**Příznaky**: nelze přeložit název hostitele, hostitele.</span><span class="sxs-lookup"><span data-stu-id="08217-161">**Symptom**: Couldn't resolve host 'hostname'</span></span>

<span data-ttu-id="08217-162">**Příčina**: k této chybě může dojít, pokud bylo nesprávné informace o adrese zadali při vytváření vzdálené služby azure.</span><span class="sxs-lookup"><span data-stu-id="08217-162">**Cause**: This error can occur if the address information entered when creating the 'azure' remote was incorrect.</span></span>

<span data-ttu-id="08217-163">**Řešení**: použití `git remote -v` příkazu zobrazíte všechny dálkové ovladače, společně s přidružené adresy URL.</span><span class="sxs-lookup"><span data-stu-id="08217-163">**Resolution**: Use the `git remote -v` command to list all remotes, along with the associated URL.</span></span> <span data-ttu-id="08217-164">Ověřte správnost adresy URL pro "azure" vzdálené.</span><span class="sxs-lookup"><span data-stu-id="08217-164">Verify that the URL for the 'azure' remote is correct.</span></span> <span data-ttu-id="08217-165">V případě potřeby odeberte a znovu tento vzdálené pomocí správnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="08217-165">If needed, remove and recreate this remote using the correct URL.</span></span>

- - -
<span data-ttu-id="08217-166">**Příznaky**: žádné odolný systém souborů v běžné a žádná zadaný; žádným způsobem.</span><span class="sxs-lookup"><span data-stu-id="08217-166">**Symptom**: No refs in common and none specified; doing nothing.</span></span> <span data-ttu-id="08217-167">Možná musíte zadat větve, jako je například "hlavní".</span><span class="sxs-lookup"><span data-stu-id="08217-167">Perhaps you should specify a branch such as 'master'.</span></span>

<span data-ttu-id="08217-168">**Příčina**: k této chybě může dojít, pokud při provádění git push operace a nebyly nastavte hodnotu push.default používá Git nezadávejte větev.</span><span class="sxs-lookup"><span data-stu-id="08217-168">**Cause**: This error can occur if you do not specify a branch when performing a git push operation, and have not set the push.default value used by Git.</span></span>

<span data-ttu-id="08217-169">**Řešení**: proveďte nabízenou akci, zadání hlavní větve.</span><span class="sxs-lookup"><span data-stu-id="08217-169">**Resolution**: Perform the push operation again, specifying the master branch.</span></span> <span data-ttu-id="08217-170">Například:</span><span class="sxs-lookup"><span data-stu-id="08217-170">For example:</span></span>

```bash  
git push azure master
```
- - -
<span data-ttu-id="08217-171">**Příznaky**: src refspec [branchname] neodpovídá žádnému.</span><span class="sxs-lookup"><span data-stu-id="08217-171">**Symptom**: src refspec [branchname] does not match any.</span></span>

<span data-ttu-id="08217-172">**Příčina**: k této chybě může dojít, pokud se pokusíte nabízená větev pouze hlavní na "azure" vzdálené.</span><span class="sxs-lookup"><span data-stu-id="08217-172">**Cause**: This error can occur if you attempt to push to a branch other than master on the 'azure' remote.</span></span>

<span data-ttu-id="08217-173">**Řešení**: proveďte nabízenou akci, zadání hlavní větve.</span><span class="sxs-lookup"><span data-stu-id="08217-173">**Resolution**: Perform the push operation again, specifying the master branch.</span></span> <span data-ttu-id="08217-174">Například:</span><span class="sxs-lookup"><span data-stu-id="08217-174">For example:</span></span>

```bash  
git push azure master
```
- - -
<span data-ttu-id="08217-175">**Příznaky**: RPC se nezdařilo; způsobit = 22, kód HTTP = 502.</span><span class="sxs-lookup"><span data-stu-id="08217-175">**Symptom**: RPC failed; result=22, HTTP code = 502.</span></span>

<span data-ttu-id="08217-176">**Příčina**: k této chybě může dojít, pokud se pokusíte úložiště velké git push přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="08217-176">**Cause**: This error can occur if you attempt to push a large git repository over HTTPS.</span></span>

<span data-ttu-id="08217-177">**Řešení**: Změna konfigurace git v místním počítači, chcete-li postBuffer zvětšit</span><span class="sxs-lookup"><span data-stu-id="08217-177">**Resolution**: Change the git configuration on the local machine to make the postBuffer bigger</span></span>

```bash  
git config --global http.postBuffer 524288000
```
- - -
<span data-ttu-id="08217-178">**Příznaky**: Chyba: změny potvrzeny do vzdáleného úložiště, ale neaktualizuje webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="08217-178">**Symptom**: Error - Changes committed to remote repository but your web app not updated.</span></span>

<span data-ttu-id="08217-179">**Příčina**: této chybě může dojít, pokud nasazujete aplikaci Node.js obsahující soubor package.json, který určuje další požadované moduly.</span><span class="sxs-lookup"><span data-stu-id="08217-179">**Cause**: This error can occur if you are deploying a Node.js app containing a package.json file that specifies additional required modules.</span></span>

<span data-ttu-id="08217-180">**Řešení**: další zprávy obsahující 'npm ERR!.</span><span class="sxs-lookup"><span data-stu-id="08217-180">**Resolution**: Additional messages containing 'npm ERR!'</span></span> <span data-ttu-id="08217-181">by měl být před přihlášeného k této chybě a může poskytnout další kontext při selhání.</span><span class="sxs-lookup"><span data-stu-id="08217-181">should be logged prior to this error, and can provide additional context on the failure.</span></span> <span data-ttu-id="08217-182">Následující seznam uvádí známé příčiny této chyby a odpovídající 'npm ERR!.</span><span class="sxs-lookup"><span data-stu-id="08217-182">The following are known causes of this error and the corresponding 'npm ERR!'</span></span> <span data-ttu-id="08217-183">zpráva:</span><span class="sxs-lookup"><span data-stu-id="08217-183">message:</span></span>

* <span data-ttu-id="08217-184">**Soubor package.json chybná**: npm ERR!</span><span class="sxs-lookup"><span data-stu-id="08217-184">**Malformed package.json file**: npm ERR!</span></span> <span data-ttu-id="08217-185">Nepodařilo se přečíst závislosti.</span><span class="sxs-lookup"><span data-stu-id="08217-185">Couldn't read dependencies.</span></span>
* <span data-ttu-id="08217-186">**Nativní modul, který nemá binární distribuce pro systém Windows**:</span><span class="sxs-lookup"><span data-stu-id="08217-186">**Native module that does not have a binary distribution for Windows**:</span></span>
  
  * <span data-ttu-id="08217-187">npm ERR!</span><span class="sxs-lookup"><span data-stu-id="08217-187">npm ERR!</span></span> <span data-ttu-id="08217-188">\`cmd "/ c" "uzlu gyp opětovné sestavení"\` se nezdařilo s 1</span><span class="sxs-lookup"><span data-stu-id="08217-188">\`cmd "/c" "node-gyp rebuild"\` failed with 1</span></span>
    
      <span data-ttu-id="08217-189">NEBO</span><span class="sxs-lookup"><span data-stu-id="08217-189">OR</span></span>
  * <span data-ttu-id="08217-190">npm ERR!</span><span class="sxs-lookup"><span data-stu-id="08217-190">npm ERR!</span></span> <span data-ttu-id="08217-191">[modulename@version] předinstalovat: \`zkontrolujte || gmake\`</span><span class="sxs-lookup"><span data-stu-id="08217-191">[modulename@version] preinstall: \`make || gmake\`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08217-192">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="08217-192">Additional Resources</span></span>
* [<span data-ttu-id="08217-193">Dokumentace pro Git</span><span class="sxs-lookup"><span data-stu-id="08217-193">Git documentation</span></span>](http://git-scm.com/documentation)
* [<span data-ttu-id="08217-194">Dokumentace Kudu projektu</span><span class="sxs-lookup"><span data-stu-id="08217-194">Project Kudu documentation</span></span>](https://github.com/projectkudu/kudu/wiki)
* [<span data-ttu-id="08217-195">Nepřetržité nasazení do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="08217-195">Continous Deployment to Azure App Service</span></span>](app-service-continuous-deployment.md)
* [<span data-ttu-id="08217-196">Způsob používání prostředí PowerShell pro Azure</span><span class="sxs-lookup"><span data-stu-id="08217-196">How to use PowerShell for Azure</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="08217-197">Jak používat rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="08217-197">How to use the Azure Command-Line Interface</span></span>](../cli-install-nodejs.md)

<span data-ttu-id="08217-198">[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/</span><span class="sxs-lookup"><span data-stu-id="08217-198">[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/</span></span>
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
<span data-ttu-id="08217-199">[portálu Azure]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="08217-199">[Azure Portal]: https://portal.azure.com</span></span>
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
<span data-ttu-id="08217-200">[rozhraní příkazového řádku Azure]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/</span><span class="sxs-lookup"><span data-stu-id="08217-200">[Azure Command-Line Interface]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/</span></span>

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
