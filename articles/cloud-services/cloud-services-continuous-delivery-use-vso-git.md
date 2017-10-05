---
title: "Nastavené průběžné doručování s Git a Visual Studio Team Services v Azure | Microsoft Docs"
description: "Naučte se konfigurovat týmových projektech Visual Studio Team Services používat Git automaticky vytvářet a nasazovat do funkci webové aplikace v Azure App Service nebo cloudové služby."
services: cloud-services
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: 4b3297ef-0de6-4d5f-925c-fcdacc3085ac
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/06/2016
ms.author: mlearned
ms.openlocfilehash: f4f5f231536bc381d17898ff2c592be821168a65
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a><span data-ttu-id="8f4d2-103">Průběžné doručování do Azure pomocí služby Visual Studio Team Services a Gitu</span><span class="sxs-lookup"><span data-stu-id="8f4d2-103">Continuous delivery to Azure using Visual Studio Team Services and Git</span></span>
<span data-ttu-id="8f4d2-104">Týmové projekty Visual Studio Team Services můžete použít k hostování úložiště Git pro váš zdrojový kód a automaticky sestavení a nasazení do webové aplikace Azure nebo cloudových služeb, vždy, když push potvrzení změn do úložiště.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-104">You can use Visual Studio Team Services team projects to host a Git repository for your source code, and automatically build and deploy to Azure web apps or cloud services whenever you push a commit to the repository.</span></span>

<span data-ttu-id="8f4d2-105">Budete potřebovat Visual Studio 2013 a Azure SDK nainstalována.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-105">You'll need Visual Studio 2013 and the Azure SDK installed.</span></span> <span data-ttu-id="8f4d2-106">Pokud ještě nemáte Visual Studio 2013, stáhněte ho tak, že zvolíte **začněte zadarmo** odkaz na [www.visualstudio.com](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="8f4d2-106">If you don't already have Visual Studio 2013, download it by choosing the **Get started for free** link at [www.visualstudio.com](http://www.visualstudio.com).</span></span> <span data-ttu-id="8f4d2-107">Nainstalovat sadu Azure SDK z [zde](http://go.microsoft.com/fwlink/?LinkId=239540).</span><span class="sxs-lookup"><span data-stu-id="8f4d2-107">Install the Azure SDK from [here](http://go.microsoft.com/fwlink/?LinkId=239540).</span></span>

> [!NOTE]
> <span data-ttu-id="8f4d2-108">Účet Visual Studio Team Services k dokončení tohoto kurzu potřebujete: můžete [zdarma otevřít účet Visual Studio Team Services](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span><span class="sxs-lookup"><span data-stu-id="8f4d2-108">You need an Visual Studio Team Services account to complete this tutorial: You can [open a Visual Studio Team Services account for free](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span></span>
> 
> 

<span data-ttu-id="8f4d2-109">Pokud chcete nastavit Cloudová služba pro automatické vytvoření a nasazení do Azure pomocí sady Visual Studio Team Services, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-109">To set up a cloud service to automatically build and deploy to Azure by using Visual Studio Team Services, follow these steps.</span></span>

## <a name="1-create-a-git-repository"></a><span data-ttu-id="8f4d2-110">1: Vytvoření úložiště Git</span><span class="sxs-lookup"><span data-stu-id="8f4d2-110">1: Create a Git repository</span></span>
1. <span data-ttu-id="8f4d2-111">Pokud nemáte účet Visual Studio Team Services, můžete jej získat [zde](http://go.microsoft.com/fwlink/?LinkId=397665).</span><span class="sxs-lookup"><span data-stu-id="8f4d2-111">If you don’t already have a Visual Studio Team Services account, you can get one  [here](http://go.microsoft.com/fwlink/?LinkId=397665).</span></span> <span data-ttu-id="8f4d2-112">Když vytvoříte týmový projekt, vyberte Git jako systém správce zdrojů.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-112">When you create your team project, choose Git as your source control system.</span></span> <span data-ttu-id="8f4d2-113">Postupujte podle pokynů pro připojení k vašemu týmovému projektu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-113">Follow the instructions to connect Visual Studio to your team project.</span></span>
2. <span data-ttu-id="8f4d2-114">V **Team Explorer**, vyberte **klonovat toto úložiště** odkaz.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-114">In **Team Explorer**, choose the **Clone this repository** link.</span></span>
   
    ![][3]
3. <span data-ttu-id="8f4d2-115">Zadejte umístění místní kopie a potom zvolte **klon** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-115">Specify the location of the local copy and then choose the **Clone** button.</span></span>

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a><span data-ttu-id="8f4d2-116">2: vytvoření projektu a potvrďte ho do úložiště</span><span class="sxs-lookup"><span data-stu-id="8f4d2-116">2: Create a project and commit it to the repository</span></span>
1. <span data-ttu-id="8f4d2-117">V **Team Explorer**v **řešení** zvolte **nový** odkaz pro vytvoření nového projektu v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-117">In **Team Explorer**, in the **Solutions** section, choose the **New** link to create a new project in the local repository.</span></span>
   
    ![][4]
2. <span data-ttu-id="8f4d2-118">Podle kroků v tomto názorném postupu můžete nasadit webovou aplikaci nebo cloudové služby (aplikace Azure).</span><span class="sxs-lookup"><span data-stu-id="8f4d2-118">You can deploy a web app or a cloud service (Azure Application) by following the steps in this walkthrough.</span></span> <span data-ttu-id="8f4d2-119">Vytvořte nový projekt Azure Cloud Service, nebo nový projekt ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-119">Create a new Azure Cloud Service project, or a new ASP.NET MVC project.</span></span> <span data-ttu-id="8f4d2-120">Přesvědčte se, že projekt cílí na rozhraní .NET Framework 4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-120">Make sure that the project targets the .NET Framework 4 or later.</span></span> <span data-ttu-id="8f4d2-121">Pokud vytváříte projekt cloudové služby, přidejte webová role ASP.NET MVC a roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-121">If you are creating a cloud service project, add an ASP.NET MVC web role and a worker role.</span></span>
   <span data-ttu-id="8f4d2-122">Pokud chcete vytvořit webovou aplikaci, vyberte **webové aplikace ASP.NET** projektu šablony a potom vyberte **MVC**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-122">If you want to create a web app, choose the **ASP.NET Web Application** project template, and then choose **MVC**.</span></span> <span data-ttu-id="8f4d2-123">V tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-123">See [Create an ASP.NET web app in Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) for more information.</span></span>
3. <span data-ttu-id="8f4d2-124">Otevřete místní nabídku pro řešení a zvolte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-124">Open the shortcut menu for the solution, and choose **Commit**.</span></span>
   
    ![][7]
4. <span data-ttu-id="8f4d2-125">Pokud jste použili Git ve Visual Studio Team Services poprvé, budete muset zadat nějaké informace, které Identifikujte se v úložišti Git.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-125">If this is the first time you've used Git in Visual Studio Team Services, you'll need to provide some information to identify yourself in Git.</span></span> <span data-ttu-id="8f4d2-126">V **čekající změny** oblasti **Team Explorer**, zadejte své uživatelské jméno a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-126">In the **Pending Changes** area of **Team Explorer**, enter your username and email address.</span></span> <span data-ttu-id="8f4d2-127">Pro potvrzení zadejte komentář a potom vyberte **potvrzení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-127">Enter a comment for the commit and then choose the **Commit** button.</span></span>
   
    ![][8]
5. <span data-ttu-id="8f4d2-128">Všimněte si možnosti, které chcete zahrnout nebo vyloučit konkrétní změny při se změnami.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-128">Note the options to include or exclude specific changes when you check in.</span></span> <span data-ttu-id="8f4d2-129">Pokud chcete změny jsou vyloučeny, zvolte **zahrnují všechny**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-129">If the changes you want are excluded, choose **Include All**.</span></span>
6. <span data-ttu-id="8f4d2-130">Jste nyní potvrzena změny v místní kopii úložiště.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-130">You've now committed the changes in your local copy of the repository.</span></span> <span data-ttu-id="8f4d2-131">V dalším kroku provedené změny se serverem synchronizovat tak, že zvolíte **synchronizace** odkaz.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-131">Next, sync those changes with the server by choosing the **Sync** link.</span></span>

## <a name="3-connect-the-project-to-azure"></a><span data-ttu-id="8f4d2-132">3: připojení projekt do Azure</span><span class="sxs-lookup"><span data-stu-id="8f4d2-132">3: Connect the project to Azure</span></span>
1. <span data-ttu-id="8f4d2-133">Teď, když máte úložiště Git v sadě Visual Studio Team Services s některé zdrojového kódu v ní, jste připraveni se připojit k Azure úložiště git.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-133">Now that you have a Git repository in Visual Studio Team Services with some source code in it, you are ready to connect your git repository to Azure.</span></span>  <span data-ttu-id="8f4d2-134">V [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), vyberte svou cloudové služby nebo webovou aplikaci nebo vytvořte novou výběrem + ikona ve spodní levé a výběr **Cloudová služba** nebo **webové aplikace** a potom **rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-134">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), select your cloud service or web app, or create a new one by choosing the + icon at the bottom left and choosing **Cloud Service** or **Web App** and then **Quick Create**.</span></span>
   
    ![][9]
2. <span data-ttu-id="8f4d2-135">Cloudové služby, zvolte **nastavit publikování pomocí Visual Studio Team Services** odkaz.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-135">For cloud services, choose the **Set up publishing with Visual Studio Team Services** link.</span></span> <span data-ttu-id="8f4d2-136">Pro webové aplikace, vyberte **nastavení nasazení od správy zdrojového kódu** odkaz.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-136">For web apps, choose the **Set up deployment from source control** link.</span></span>
   
    ![][10]
3. <span data-ttu-id="8f4d2-137">V průvodci, do textového pole zadejte název účtu Visual Studio Team Services a zvolte **autorizovat teď** odkaz.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-137">In the wizard, type the name of your Visual Studio Team Services account in the textbox and choose the **Authorize Now** link.</span></span> <span data-ttu-id="8f4d2-138">Můžete být požádáni o přihlášení.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-138">You might be asked to sign in.</span></span>
   
    ![][11]
4. <span data-ttu-id="8f4d2-139">V **požadavku na připojení** automaticky otevíraný dialog, zvolte **přijmout** autorizace Azure ke konfiguraci týmového projektu v sadě Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-139">In the **Connection Request** pop-up dialog, choose **Accept** to authorize Azure to configure your team project in Visual Studio Team Services.</span></span>
   
    ![][12]
5. <span data-ttu-id="8f4d2-140">Po úspěšné ověřování, zobrazí rozevírací seznam obsahující týmových projektech Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-140">After authorization succeeds, you see a dropdown list that contains your Visual Studio Team Services team projects.</span></span>  <span data-ttu-id="8f4d2-141">Vyberte název týmového projektu, který jste vytvořili v předchozím kroku a vyberte značku zaškrtnutí v průvodci.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-141">Select the name of team project that you created in the previous steps, and choose the wizard's checkmark button.</span></span>
   
    ![][13]
   
    <span data-ttu-id="8f4d2-142">Při příštím push potvrzení změn do úložiště, Visual Studio Team Services bude sestavovat a nasazení projektu do Azure.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-142">The next time you push a commit to your repository, Visual Studio Team Services will build and deploy your project to Azure.</span></span>

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a><span data-ttu-id="8f4d2-143">4: aktivovat opětovném sestavení a znovu nasaďte projekt</span><span class="sxs-lookup"><span data-stu-id="8f4d2-143">4: Trigger a rebuild and redeploy your project</span></span>
1. <span data-ttu-id="8f4d2-144">V sadě Visual Studio otevřete soubor a ho změnit.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-144">In Visual Studio, open up a file and change it.</span></span> <span data-ttu-id="8f4d2-145">Můžete například změnit soubor `_Layout.cshtml` v zobrazení\\sdílené složky MVC webové role.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-145">For example, change the file `_Layout.cshtml` under the Views\\Shared folder in an MVC web role.</span></span>
   
    ![][17]
2. <span data-ttu-id="8f4d2-146">Upravit text zápatí pro lokalitu a soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-146">Edit the footer text for the site and save the file.</span></span>
   
    ![][18]
3. <span data-ttu-id="8f4d2-147">V **Průzkumníku řešení**, otevřete místní nabídku pro řešení uzel, uzel projektu nebo změnit a potom zvolte soubor **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-147">In **Solution Explorer**, open the shortcut menu for the solution node, project node, or the file you changed, and then choose **Commit**.</span></span>
4. <span data-ttu-id="8f4d2-148">Zadejte komentář a zvolte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-148">Type in a comment and choose **Commit**.</span></span>
   
    ![][20]
5. <span data-ttu-id="8f4d2-149">Vyberte **synchronizace** odkaz.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-149">Choose the **Sync** link.</span></span>
   
    ![][38]
6. <span data-ttu-id="8f4d2-150">Vyberte **nabízené** odkaz tak, aby nabízel vaše potvrzení do úložiště v sadě Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-150">Choose the **Push** link to push your commit to the repository in Visual Studio Team Services.</span></span> <span data-ttu-id="8f4d2-151">(Můžete také **synchronizace** tlačítko chcete zkopírovat do úložiště vaše potvrzení.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-151">(You can also use the **Sync** button to copy your commits to the repository.</span></span> <span data-ttu-id="8f4d2-152">Rozdíl je, že **synchronizace** také vrátí nejnovějších změn z úložiště.)</span><span class="sxs-lookup"><span data-stu-id="8f4d2-152">The difference is that **Sync** also pulls the latest changes from the repository.)</span></span>
   
    ![][39]
7. <span data-ttu-id="8f4d2-153">Zvolte **domácí** tlačítko se vrátíte do **Team Explorer** domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-153">Choose the **Home** button to return to the **Team Explorer** home page.</span></span>
   
    ![][21]
8. <span data-ttu-id="8f4d2-154">Zvolte **sestavení** Chcete-li zobrazit nastavení v průběhu.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-154">Choose **Builds** to view the builds in progress.</span></span>
   
    ![][22]
   
    <span data-ttu-id="8f4d2-155">**Team Explorer** ukazuje, že byla spuštěna sestavení pro vaše vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-155">**Team Explorer** shows that a build has been triggered for your check-in.</span></span>
   
    ![][23]
9. <span data-ttu-id="8f4d2-156">Chcete-li zobrazit podrobné protokolu v průběhu sestavení, dvakrát klikněte na název sestavení v průběhu.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-156">To view a detailed log as the build progresses, double-click the name of the build in progress.</span></span>
10. <span data-ttu-id="8f4d2-157">Při sestavení je v průběhu, podívejte se na definici sestavení, která byla vytvořena, když jste použili Průvodce propojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-157">While the build is in-progress, take a look at the build definition that was created when you used the wizard to link to Azure.</span></span>  <span data-ttu-id="8f4d2-158">Otevřete místní nabídku pro definici sestavení a zvolte **upravit definici sestavení**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-158">Open the shortcut menu for the build definition and choose **Edit Build Definition**.</span></span>
    
    ![][25]
11. <span data-ttu-id="8f4d2-159">V **aktivační událost** karty, zobrazí se, že je ve výchozím nastavení na každé vrácení se změnami, definici sestavení.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-159">In the **Trigger** tab, you will see that the build definition is set to build on every check-in, by default.</span></span> <span data-ttu-id="8f4d2-160">(Pro cloudové služby Visual Studio Team Services vytvoří a nasadí hlavní větve do fázovacího prostředí automaticky.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-160">(For a cloud service, Visual Studio Team Services builds and deploys the master branch to the staging environment automatically.</span></span> <span data-ttu-id="8f4d2-161">Stále je nutné provést ruční krok k nasazení na živý web.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-161">You still have to do a manual step to deploy to the live site.</span></span> <span data-ttu-id="8f4d2-162">Pro webovou aplikaci, která nemá pracovní prostředí nasadí hlavní větve přímo na živý web.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-162">For a web app that doesn't have staging environment, it deploys the master branch directly to the live site.</span></span>
    
    ![][26]
12. <span data-ttu-id="8f4d2-163">V **proces** kartě uvidíte prostředí nasazení je nastavena na název cloudové služby nebo webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-163">In the **Process** tab, you can see the deployment environment is set to the name of your cloud service or web app.</span></span>
    
     ![][27]
13. <span data-ttu-id="8f4d2-164">Zadejte hodnoty vlastností, pokud chcete jiné hodnoty než výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-164">Specify values for the properties if you want different values than the defaults.</span></span> <span data-ttu-id="8f4d2-165">Vlastnosti pro publikování v Azure jsou ve **nasazení** části a může být také nutné nastavit MSBuild parametry.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-165">The properties for Azure publishing are in the **Deployment** section, and you might also need to set MSBuild parameters.</span></span> <span data-ttu-id="8f4d2-166">V cloudu projekt služby, můžete zadat konfiguraci služby než "Cloud", nastavte parametry nástroje MSbuild na `/p:TargetProfile=[YourProfile]` kde *[YourProfile]* odpovídá konfigurační soubor služby s názvem, jako je objekt ServiceConfiguration. *YourProfile*.cscfg.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-166">For example, in a cloud service project, to specify a service configuration other than "Cloud", set the MSbuild parameters to `/p:TargetProfile=[YourProfile]` where *[YourProfile]* matches a service configuration file with a name like ServiceConfiguration.*YourProfile*.cscfg.</span></span>
    
     <span data-ttu-id="8f4d2-167">V následující tabulce jsou uvedeny dostupné vlastnosti v **nasazení** části:</span><span class="sxs-lookup"><span data-stu-id="8f4d2-167">The following table shows the available properties in the **Deployment** section:</span></span>
    
    | <span data-ttu-id="8f4d2-168">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8f4d2-168">Property</span></span> | <span data-ttu-id="8f4d2-169">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="8f4d2-169">Default Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="8f4d2-170">Povolit nedůvěryhodné certifikáty</span><span class="sxs-lookup"><span data-stu-id="8f4d2-170">Allow Untrusted Certificates</span></span> |<span data-ttu-id="8f4d2-171">Pokud je hodnota false, musí být podepsané certifikáty SSL kořenovou autoritou.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-171">If false, SSL certificates must be signed by a root authority.</span></span> |
    | <span data-ttu-id="8f4d2-172">Povolit upgradu</span><span class="sxs-lookup"><span data-stu-id="8f4d2-172">Allow Upgrade</span></span> |<span data-ttu-id="8f4d2-173">Umožňuje nasazení k aktualizaci stávajícího nasazení místo vytvoření nové.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-173">Allows the deployment to update an existing deployment instead of creating a new one.</span></span> <span data-ttu-id="8f4d2-174">Zachovává IP adresu.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-174">Preserves the IP address.</span></span> |
    | <span data-ttu-id="8f4d2-175">Neodstraňujte</span><span class="sxs-lookup"><span data-stu-id="8f4d2-175">Do Not Delete</span></span> |<span data-ttu-id="8f4d2-176">V případě hodnoty true Nepřepisovat existující nesouvisejícími nasazení (upgrade je povolena.).</span><span class="sxs-lookup"><span data-stu-id="8f4d2-176">If true, do not overwrite an existing unrelated deployment (upgrade is allowed).</span></span> |
    | <span data-ttu-id="8f4d2-177">Cesta k nastavení nasazení</span><span class="sxs-lookup"><span data-stu-id="8f4d2-177">Path to Deployment Settings</span></span> |<span data-ttu-id="8f4d2-178">Cesta k souboru .pubxml pro webovou aplikaci, relativně ke kořenové složce úložiště.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-178">The path to your .pubxml file for a web app, relative to the root folder of the repo.</span></span> <span data-ttu-id="8f4d2-179">Ignorovat pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-179">Ignored for cloud services.</span></span> |
    | <span data-ttu-id="8f4d2-180">Prostředí nasazení služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="8f4d2-180">Sharepoint Deployment Environment</span></span> |<span data-ttu-id="8f4d2-181">Stejný jako název služby.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-181">The same as the service name.</span></span> |
    | <span data-ttu-id="8f4d2-182">Prostředí nasazení Azure</span><span class="sxs-lookup"><span data-stu-id="8f4d2-182">Azure Deployment Environment</span></span> |<span data-ttu-id="8f4d2-183">Webové aplikace nebo cloudové název služby.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-183">The web app or cloud service name.</span></span> |
14. <span data-ttu-id="8f4d2-184">Buildu té doby by se měly dokončit úspěšně.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-184">By this time, your build should be completed successfully.</span></span>
    
     ![][28]
15. <span data-ttu-id="8f4d2-185">Pokud dvakrát kliknete na název sestavení, Visual Studio se zobrazí **souhrnu sestavení**, včetně všech výsledků testu z přidružené projektů testování částí.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-185">If you double-click the build name, Visual Studio shows a **Build Summary**, including any test results from associated unit test projects.</span></span>
    
     ![][29]
16. <span data-ttu-id="8f4d2-186">V [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), přidružené nasazení můžete zobrazit na **nasazení** kartě, pokud je vybrána pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-186">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), you can view the associated deployment on the **Deployments** tab when the staging environment is selected.</span></span>
    
     ![][30]
17. <span data-ttu-id="8f4d2-187">Přejděte na adresu URL vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-187">Browse to your site's URL.</span></span> <span data-ttu-id="8f4d2-188">Pro webovou aplikaci, zvolte pouze **Procházet** tlačítko na portálu.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-188">For a web app, just choose  the **Browse** button in the portal.</span></span> <span data-ttu-id="8f4d2-189">Cloudové služby, vyberte adresu URL v **rychlý přehled** části **řídicí panel** stránky, která obsahuje pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-189">For a cloud service, choose the URL in the **Quick Glance** section of the **Dashboard** page that shows the Staging environment.</span></span>
    
    <span data-ttu-id="8f4d2-190">Nasazení z průběžnou integraci pro cloudové služby jsou publikovány do prostředí pracovní ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-190">Deployments from continuous integration for cloud services are published to the Staging environment by default.</span></span> <span data-ttu-id="8f4d2-191">Toto můžete změnit nastavením **alternativní prostředí cloudové služby** vlastnost **produkční**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-191">You can change this by setting the **Alternate Cloud Service Environment** property to **Production**.</span></span> <span data-ttu-id="8f4d2-192">Zde je, kde je adresa URL webu na stránce řídicího panelu cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-192">Here's where the site URL is on the cloud service's dashboard page.</span></span>
    
    ![][31]
    
    <span data-ttu-id="8f4d2-193">Na nich spuštěné webu se otevře novou kartu prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-193">A new browser tab will open to reveal your running site.</span></span>
    
    ![][32]
18. <span data-ttu-id="8f4d2-194">Pokud provedete další změny do projektu, aktivační události je více sestavení a budou se hromadit více nasazení.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-194">If you make other changes to your project, you trigger more builds, and you will accumulate multiple deployments.</span></span> <span data-ttu-id="8f4d2-195">Nejnovější je označena jako aktivní.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-195">The latest one is marked as Active.</span></span>
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a><span data-ttu-id="8f4d2-196">5: znovu nasaďte starší sestavení</span><span class="sxs-lookup"><span data-stu-id="8f4d2-196">5: Redeploy an earlier build</span></span>
<span data-ttu-id="8f4d2-197">Tento krok je volitelný.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-197">This step is optional.</span></span> <span data-ttu-id="8f4d2-198">Na portálu Azure classic, zvolte k předchozímu nasazení a **znovu nasaďte** převíjení váš web, dřívější vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-198">In the Azure classic portal, choose an earlier deployment and choose **Redeploy** to rewind your site to an earlier check-in.</span></span> <span data-ttu-id="8f4d2-199">Všimněte si, že to aktivují nového sestavení v sadě TFS a vytvořit nový záznam v historii nasazení.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-199">Note that this will trigger a new build in TFS and create a new entry in your deployment history.</span></span>

![][34]

## <a name="6-change-the-production-deployment"></a><span data-ttu-id="8f4d2-200">6: Změňte produkční nasazení</span><span class="sxs-lookup"><span data-stu-id="8f4d2-200">6: Change the Production deployment</span></span>
<span data-ttu-id="8f4d2-201">Jakmile budete připraveni, můžete zvýšit úroveň pracovního prostředí do produkčního prostředí, tak, že zvolíte **odkládacího souboru** na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-201">When you are ready, you can promote the Staging environment to the Production environment by choosing **Swap** in the Azure classic portal.</span></span> <span data-ttu-id="8f4d2-202">Nově nasazené pracovní prostředí se zvýší úroveň na produkční a předchozí provozním prostředí, pokud existuje, bude pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-202">The newly deployed Staging environment is promoted to Production, and the previous Production environment, if any, becomes a Staging environment.</span></span> <span data-ttu-id="8f4d2-203">Aktivní nasazení může být různé pro produkční a pracovní prostředí, ale historii nasazení poslední sestavení je stejný bez ohledu na prostředí.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-203">The Active deployment may be different for the Production and Staging environments, but the deployment history of recent builds is the same regardless of environment.</span></span>

![][35]

## <a name="7-deploy-from-a-working-branch"></a><span data-ttu-id="8f4d2-204">7: nasazení z pracovní větev.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-204">7: Deploy from a working branch.</span></span>
<span data-ttu-id="8f4d2-205">Při použití Git, obvykle provedete změny v pracovní větev a integrovat do hlavní větve, když váš vývojový dosáhne konečného stavu.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-205">When you use Git, you usually make changes in a working branch and integrate into the master branch when your development reaches a finished state.</span></span> <span data-ttu-id="8f4d2-206">V průběhu fáze vývoje projektu budete chtít vytvořit a nasadit pracovní větev do Azure.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-206">During the development phase of a project, you'll want to build and deploy the working branch to Azure.</span></span>

1. <span data-ttu-id="8f4d2-207">V **Team Explorer**, vyberte **Domů** tlačítko a potom zvolte **větví** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-207">In **Team Explorer**, choose the **Home** button and then choose the **Branches** button.</span></span>
   
    ![][40]
2. <span data-ttu-id="8f4d2-208">Vyberte **nové větve** odkaz.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-208">Choose the **New Branch** link.</span></span>
   
    ![][41]
3. <span data-ttu-id="8f4d2-209">Zadejte název větve, jako je například "fungoval," a zvolte **vytvoření větve**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-209">Enter the name of the branch, such as "working," and choose **Create Branch**.</span></span> <span data-ttu-id="8f4d2-210">Tím se vytvoří nové místní větve.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-210">This creates a new local branch.</span></span>
   
    ![][42]
4. <span data-ttu-id="8f4d2-211">Publikujte větev.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-211">Publish the branch.</span></span> <span data-ttu-id="8f4d2-212">Zvolte název větve v **Nepublikováno větví**a zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-212">Choose the branch name in **Unpublished branches**, and choose **Publish**.</span></span>
   
    ![][44]
5. <span data-ttu-id="8f4d2-213">Ve výchozím nastavení pouze změny do hlavní větve aktivovat průběžné build.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-213">By default, only changes to the master branch trigger a continuous build.</span></span> <span data-ttu-id="8f4d2-214">Chcete-li nastavit průběžné sestavení pro pracovní větev, zvolte **sestavení** stránky v **Team Explorer**a zvolte **upravit definici sestavení**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-214">To set up continuous build for a working branch, choose the **Builds** page in **Team Explorer**, and choose **Edit Build Definition**.</span></span>
6. <span data-ttu-id="8f4d2-215">Otevřete **nastavení zdroje** kartě.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-215">Open the **Source Settings** tab.</span></span> <span data-ttu-id="8f4d2-216">V části **monitorovat větve pro průběžnou integraci a sestavení**, zvolte **kliknutím sem přidejte nový řádek**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-216">Under **Monitored branches for continuous integration and build**, choose **Click here to add a new row**.</span></span>
   
    ![][47]
7. <span data-ttu-id="8f4d2-217">Zadejte, které jste vytvořili, například odolný systém souborů nebo oznámení nebo pracovní větev.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-217">Specify the branch you created, such as refs/heads/working.</span></span>
   
    ![][48]
8. <span data-ttu-id="8f4d2-218">V kódu změňte, otevřete místní nabídku pro změněný soubor a potom zvolte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-218">Make a change in the code, open the shortcut menu for the changed file, and then choose **Commit**.</span></span>
   
    ![][43]
9. <span data-ttu-id="8f4d2-219">Vyberte **nesynchronizované potvrzení** propojit a zvolte **synchronizace** tlačítko nebo **Push** odkaz zkopírovat změny do kopie pracovní větev ve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-219">Choose the **Unsynced Commits** link, and choose  the **Sync** button or the **Push** link to copy the changes to the copy of the working branch in Visual Studio Team Services.</span></span>
   
   ![][45]
10. <span data-ttu-id="8f4d2-220">Přejděte na **sestavení** zobrazení a najít sestavení, který právě získali aktivoval pro pracovní větev.</span><span class="sxs-lookup"><span data-stu-id="8f4d2-220">Navigate to the **Builds** view and find the build that just got triggered for the working branch.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f4d2-221">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8f4d2-221">Next steps</span></span>
<span data-ttu-id="8f4d2-222">Další tipy k používání Git s Visual Studio Team Services naleznete v tématu [vývoj a sdílet kódu v úložišti Git pomocí sady Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) a informace o použití úložiště Git, který není spravován nástrojem Visual Studio Team Services k publikování do Azure najdete v tématu [průběžné nasazování do služby Azure App Service](../app-service-web/app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="8f4d2-222">To learn more tips on using Git with Visual Studio Team Services, see [Develop and share your code in Git using Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) and for information about using a Git repository that's not managed by Visual Studio Team Services to publish to Azure, see [Continuous Deployment to Azure App Service](../app-service-web/app-service-continuous-deployment.md).</span></span> <span data-ttu-id="8f4d2-223">Další informace o sadě Visual Studio Team Services najdete v tématu [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span><span class="sxs-lookup"><span data-stu-id="8f4d2-223">For more information on Visual Studio Team Services, see [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span></span>

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
