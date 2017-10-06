---
title: "doručení aaaContinuous s Git a Visual Studio Team Services v Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Visual Studio Team Services týmové projekty toouse Git tooautomatically sestavení a nasazení funkce toohello webové aplikace v Azure App Service nebo cloudové služby."
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
ms.openlocfilehash: 936c42194f45be55597a77f9a3a6deb4480ed94b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-tooazure-using-visual-studio-team-services-and-git"></a><span data-ttu-id="bcf52-103">Nastavené průběžné doručování tooAzure pomocí Visual Studio Team Services a Git</span><span class="sxs-lookup"><span data-stu-id="bcf52-103">Continuous delivery tooAzure using Visual Studio Team Services and Git</span></span>
<span data-ttu-id="bcf52-104">Můžete použít Visual Studio Team Services týmové projekty toohost úložiště Git pro zdrojový kód a automaticky sestavení a nasadit tooAzure webové aplikace nebo cloudové služby vždy, když push úložiště toohello potvrzení.</span><span class="sxs-lookup"><span data-stu-id="bcf52-104">You can use Visual Studio Team Services team projects toohost a Git repository for your source code, and automatically build and deploy tooAzure web apps or cloud services whenever you push a commit toohello repository.</span></span>

<span data-ttu-id="bcf52-105">Budete potřebovat Visual Studio 2013 a Azure SDK nainstalovat hello.</span><span class="sxs-lookup"><span data-stu-id="bcf52-105">You'll need Visual Studio 2013 and hello Azure SDK installed.</span></span> <span data-ttu-id="bcf52-106">Pokud ještě nemáte Visual Studio 2013, stáhněte ho tak, že zvolíte hello **začněte zadarmo** odkaz na [www.visualstudio.com](http://www.visualstudio.com). Instalace hello Azure SDK z [zde](http://go.microsoft.com/fwlink/?LinkId=239540).</span><span class="sxs-lookup"><span data-stu-id="bcf52-106">If you don't already have Visual Studio 2013, download it by choosing hello **Get started for free** link at [www.visualstudio.com](http://www.visualstudio.com). Install hello Azure SDK from [here](http://go.microsoft.com/fwlink/?LinkId=239540).</span></span>

> [!NOTE]
> <span data-ttu-id="bcf52-107">Budete potřebovat Visual Studio Team Services toocomplete účet v tomto kurzu: můžete [zdarma otevřít účet Visual Studio Team Services](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span><span class="sxs-lookup"><span data-stu-id="bcf52-107">You need an Visual Studio Team Services account toocomplete this tutorial: You can [open a Visual Studio Team Services account for free](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span></span>
> 
> 

<span data-ttu-id="bcf52-108">tooset nahoru cloudové služby tooautomatically sestavení a nasazení tooAzure pomocí Visual Studio Team Services, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="bcf52-108">tooset up a cloud service tooautomatically build and deploy tooAzure by using Visual Studio Team Services, follow these steps.</span></span>

## <a name="1-create-a-git-repository"></a><span data-ttu-id="bcf52-109">1: Vytvoření úložiště Git</span><span class="sxs-lookup"><span data-stu-id="bcf52-109">1: Create a Git repository</span></span>
1. <span data-ttu-id="bcf52-110">Pokud nemáte účet Visual Studio Team Services, můžete jej získat [zde](http://go.microsoft.com/fwlink/?LinkId=397665).</span><span class="sxs-lookup"><span data-stu-id="bcf52-110">If you don’t already have a Visual Studio Team Services account, you can get one  [here](http://go.microsoft.com/fwlink/?LinkId=397665).</span></span> <span data-ttu-id="bcf52-111">Když vytvoříte týmový projekt, vyberte Git jako systém správce zdrojů.</span><span class="sxs-lookup"><span data-stu-id="bcf52-111">When you create your team project, choose Git as your source control system.</span></span> <span data-ttu-id="bcf52-112">Postupujte podle hello pokyny tooconnect Visual Studio tooyour týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="bcf52-112">Follow hello instructions tooconnect Visual Studio tooyour team project.</span></span>
2. <span data-ttu-id="bcf52-113">V **Team Explorer**, zvolte hello **klonovat toto úložiště** odkaz.</span><span class="sxs-lookup"><span data-stu-id="bcf52-113">In **Team Explorer**, choose hello **Clone this repository** link.</span></span>
   
    ![][3]
3. <span data-ttu-id="bcf52-114">Zadejte umístění hello hello místní kopie a potom zvolte hello **klon** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bcf52-114">Specify hello location of hello local copy and then choose hello **Clone** button.</span></span>

## <a name="2-create-a-project-and-commit-it-toohello-repository"></a><span data-ttu-id="bcf52-115">2: Vytvořte projekt a potvrdit ho toohello úložiště</span><span class="sxs-lookup"><span data-stu-id="bcf52-115">2: Create a project and commit it toohello repository</span></span>
1. <span data-ttu-id="bcf52-116">V **Team Explorer**, v hello **řešení** zvolte hello **nový** odkaz toocreate nový projekt v místním úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="bcf52-116">In **Team Explorer**, in hello **Solutions** section, choose hello **New** link toocreate a new project in hello local repository.</span></span>
   
    ![][4]
2. <span data-ttu-id="bcf52-117">Můžete nasadit webovou aplikaci nebo cloudové služby (aplikace Azure) podle následujících hello kroky v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="bcf52-117">You can deploy a web app or a cloud service (Azure Application) by following hello steps in this walkthrough.</span></span> <span data-ttu-id="bcf52-118">Vytvořte nový projekt Azure Cloud Service, nebo nový projekt ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bcf52-118">Create a new Azure Cloud Service project, or a new ASP.NET MVC project.</span></span> <span data-ttu-id="bcf52-119">Přesvědčte se, že cíle projektu hello hello rozhraní .NET Framework 4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="bcf52-119">Make sure that hello project targets hello .NET Framework 4 or later.</span></span> <span data-ttu-id="bcf52-120">Pokud vytváříte projekt cloudové služby, přidejte webová role ASP.NET MVC a roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="bcf52-120">If you are creating a cloud service project, add an ASP.NET MVC web role and a worker role.</span></span>
   <span data-ttu-id="bcf52-121">Pokud chcete, aby toocreate webovou aplikaci, vyberte hello **webové aplikace ASP.NET** projektu šablony a potom vyberte **MVC**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-121">If you want toocreate a web app, choose hello **ASP.NET Web Application** project template, and then choose **MVC**.</span></span> <span data-ttu-id="bcf52-122">V tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bcf52-122">See [Create an ASP.NET web app in Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) for more information.</span></span>
3. <span data-ttu-id="bcf52-123">Otevřete hello místní nabídku pro hello řešení a zvolte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-123">Open hello shortcut menu for hello solution, and choose **Commit**.</span></span>
   
    ![][7]
4. <span data-ttu-id="bcf52-124">Pokud je to hello poprvé Git jste používali ve Visual Studio Team Services, budete potřebovat tooprovide některé informace tooidentify sami v úložišti Git.</span><span class="sxs-lookup"><span data-stu-id="bcf52-124">If this is hello first time you've used Git in Visual Studio Team Services, you'll need tooprovide some information tooidentify yourself in Git.</span></span> <span data-ttu-id="bcf52-125">V hello **čekající změny** oblasti **Team Explorer**, zadejte své uživatelské jméno a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="bcf52-125">In hello **Pending Changes** area of **Team Explorer**, enter your username and email address.</span></span> <span data-ttu-id="bcf52-126">Potvrzení hello zadejte komentář a potom vyberte hello **potvrzení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bcf52-126">Enter a comment for hello commit and then choose hello **Commit** button.</span></span>
   
    ![][8]
5. <span data-ttu-id="bcf52-127">Poznámka: možnosti tooinclude hello nebo vyloučit konkrétní změny při se změnami.</span><span class="sxs-lookup"><span data-stu-id="bcf52-127">Note hello options tooinclude or exclude specific changes when you check in.</span></span> <span data-ttu-id="bcf52-128">Pokud hello změny, které chcete jsou vyloučeny, zvolte **zahrnují všechny**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-128">If hello changes you want are excluded, choose **Include All**.</span></span>
6. <span data-ttu-id="bcf52-129">Jste nyní hello potvrdit změny v místní kopii hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="bcf52-129">You've now committed hello changes in your local copy of hello repository.</span></span> <span data-ttu-id="bcf52-130">V dalším kroku provedené změny synchronizovat s hello server tak, že zvolíte hello **synchronizace** odkaz.</span><span class="sxs-lookup"><span data-stu-id="bcf52-130">Next, sync those changes with hello server by choosing hello **Sync** link.</span></span>

## <a name="3-connect-hello-project-tooazure"></a><span data-ttu-id="bcf52-131">3: připojení tooAzure projektu hello</span><span class="sxs-lookup"><span data-stu-id="bcf52-131">3: Connect hello project tooAzure</span></span>
1. <span data-ttu-id="bcf52-132">Teď, když máte úložiště Git v sadě Visual Studio Team Services s některé zdrojového kódu v ní, se připravené tooconnect vaše tooAzure úložiště git.</span><span class="sxs-lookup"><span data-stu-id="bcf52-132">Now that you have a Git repository in Visual Studio Team Services with some source code in it, you are ready tooconnect your git repository tooAzure.</span></span>  <span data-ttu-id="bcf52-133">V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), vyberte svou cloudové služby nebo webovou aplikaci nebo vytvořte novou výběrem hello + ikonu v dolní hello vlevo a výběr **Cloudová služba** nebo **webové aplikace**a potom **rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-133">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), select your cloud service or web app, or create a new one by choosing hello + icon at hello bottom left and choosing **Cloud Service** or **Web App** and then **Quick Create**.</span></span>
   
    ![][9]
2. <span data-ttu-id="bcf52-134">Pro cloudové služby, zvolte hello **nastavit publikování pomocí Visual Studio Team Services** odkaz.</span><span class="sxs-lookup"><span data-stu-id="bcf52-134">For cloud services, choose hello **Set up publishing with Visual Studio Team Services** link.</span></span> <span data-ttu-id="bcf52-135">Pro webové aplikace, vyberte hello **nastavení nasazení od správy zdrojového kódu** odkaz.</span><span class="sxs-lookup"><span data-stu-id="bcf52-135">For web apps, choose hello **Set up deployment from source control** link.</span></span>
   
    ![][10]
3. <span data-ttu-id="bcf52-136">V Průvodci hello, zadejte název účtu Visual Studio Team Services hello v textovém poli hello a zvolte hello **autorizovat teď** odkaz.</span><span class="sxs-lookup"><span data-stu-id="bcf52-136">In hello wizard, type hello name of your Visual Studio Team Services account in hello textbox and choose hello **Authorize Now** link.</span></span> <span data-ttu-id="bcf52-137">Můžete být požádáni toosign v.</span><span class="sxs-lookup"><span data-stu-id="bcf52-137">You might be asked toosign in.</span></span>
   
    ![][11]
4. <span data-ttu-id="bcf52-138">V hello **požadavku na připojení** automaticky otevíraný dialog, zvolte **přijmout** tooauthorize Azure tooconfigure váš tým projektu ve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="bcf52-138">In hello **Connection Request** pop-up dialog, choose **Accept** tooauthorize Azure tooconfigure your team project in Visual Studio Team Services.</span></span>
   
    ![][12]
5. <span data-ttu-id="bcf52-139">Po úspěšné ověřování, zobrazí rozevírací seznam obsahující týmových projektech Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="bcf52-139">After authorization succeeds, you see a dropdown list that contains your Visual Studio Team Services team projects.</span></span>  <span data-ttu-id="bcf52-140">Vyberte název týmového projektu, který jste vytvořili v předchozích krocích hello hello a zvolte tlačítko zaškrtnutí hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="bcf52-140">Select hello name of team project that you created in hello previous steps, and choose hello wizard's checkmark button.</span></span>
   
    ![][13]
   
    <span data-ttu-id="bcf52-141">Hello příštím push potvrzení tooyour úložiště, Visual Studio Team Services bude sestavovat a nasadit tooAzure vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="bcf52-141">hello next time you push a commit tooyour repository, Visual Studio Team Services will build and deploy your project tooAzure.</span></span>

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a><span data-ttu-id="bcf52-142">4: aktivovat opětovném sestavení a znovu nasaďte projekt</span><span class="sxs-lookup"><span data-stu-id="bcf52-142">4: Trigger a rebuild and redeploy your project</span></span>
1. <span data-ttu-id="bcf52-143">V sadě Visual Studio otevřete soubor a ho změnit.</span><span class="sxs-lookup"><span data-stu-id="bcf52-143">In Visual Studio, open up a file and change it.</span></span> <span data-ttu-id="bcf52-144">Můžete například změnit soubor hello `_Layout.cshtml` pod zobrazení hello\\sdílené složky MVC webové role.</span><span class="sxs-lookup"><span data-stu-id="bcf52-144">For example, change hello file `_Layout.cshtml` under hello Views\\Shared folder in an MVC web role.</span></span>
   
    ![][17]
2. <span data-ttu-id="bcf52-145">Upravit text zápatí hello hello lokality a uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="bcf52-145">Edit hello footer text for hello site and save hello file.</span></span>
   
    ![][18]
3. <span data-ttu-id="bcf52-146">V **Průzkumníku řešení**, otevřete hello místní nabídky pro hello řešení uzel, uzel projektu nebo hello souborů můžete změnit a potom zvolte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-146">In **Solution Explorer**, open hello shortcut menu for hello solution node, project node, or hello file you changed, and then choose **Commit**.</span></span>
4. <span data-ttu-id="bcf52-147">Zadejte komentář a zvolte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-147">Type in a comment and choose **Commit**.</span></span>
   
    ![][20]
5. <span data-ttu-id="bcf52-148">Zvolte hello **synchronizace** odkaz.</span><span class="sxs-lookup"><span data-stu-id="bcf52-148">Choose hello **Sync** link.</span></span>
   
    ![][38]
6. <span data-ttu-id="bcf52-149">Zvolte hello **Push** odkaz toopush úložiště toohello potvrzení ve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="bcf52-149">Choose hello **Push** link toopush your commit toohello repository in Visual Studio Team Services.</span></span> <span data-ttu-id="bcf52-150">(Můžete použít také hello **synchronizace** tlačítko toocopy úložiště toohello potvrzení.</span><span class="sxs-lookup"><span data-stu-id="bcf52-150">(You can also use hello **Sync** button toocopy your commits toohello repository.</span></span> <span data-ttu-id="bcf52-151">Hello rozdílem je, že **synchronizace** také si hello nejnovějších změn z hello úložiště.)</span><span class="sxs-lookup"><span data-stu-id="bcf52-151">hello difference is that **Sync** also pulls hello latest changes from hello repository.)</span></span>
   
    ![][39]
7. <span data-ttu-id="bcf52-152">Zvolte hello **domácí** tlačítko tooreturn toohello **Team Explorer** domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="bcf52-152">Choose hello **Home** button tooreturn toohello **Team Explorer** home page.</span></span>
   
    ![][21]
8. <span data-ttu-id="bcf52-153">Zvolte **sestavení** tooview hello sestavení v průběhu.</span><span class="sxs-lookup"><span data-stu-id="bcf52-153">Choose **Builds** tooview hello builds in progress.</span></span>
   
    ![][22]
   
    <span data-ttu-id="bcf52-154">**Team Explorer** ukazuje, že byla spuštěna sestavení pro vaše vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="bcf52-154">**Team Explorer** shows that a build has been triggered for your check-in.</span></span>
   
    ![][23]
9. <span data-ttu-id="bcf52-155">tooview podrobný protokol jako hello sestavení postupuje, dvakrát klikněte na název hello hello sestavení v průběhu.</span><span class="sxs-lookup"><span data-stu-id="bcf52-155">tooview a detailed log as hello build progresses, double-click hello name of hello build in progress.</span></span>
10. <span data-ttu-id="bcf52-156">Probíhá při sestavení hello prohlédněte hello definice sestavení, která byla vytvořena, když jste použili tooAzure toolink Průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="bcf52-156">While hello build is in-progress, take a look at hello build definition that was created when you used hello wizard toolink tooAzure.</span></span>  <span data-ttu-id="bcf52-157">Otevřete hello místní nabídku pro definici sestavení hello a zvolte **upravit definici sestavení**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-157">Open hello shortcut menu for hello build definition and choose **Edit Build Definition**.</span></span>
    
    ![][25]
11. <span data-ttu-id="bcf52-158">V hello **aktivační událost** karty, zobrazí se, že definici sestavení hello toobuild na každý vrácení se změnami, ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="bcf52-158">In hello **Trigger** tab, you will see that hello build definition is set toobuild on every check-in, by default.</span></span> <span data-ttu-id="bcf52-159">(Pro cloudové služby Visual Studio Team Services vytvoří a nasadí hello hlavní větve toohello pracovní prostředí automaticky.</span><span class="sxs-lookup"><span data-stu-id="bcf52-159">(For a cloud service, Visual Studio Team Services builds and deploys hello master branch toohello staging environment automatically.</span></span> <span data-ttu-id="bcf52-160">Musíte ještě toodo živý web manuální krok toodeploy toohello.</span><span class="sxs-lookup"><span data-stu-id="bcf52-160">You still have toodo a manual step toodeploy toohello live site.</span></span> <span data-ttu-id="bcf52-161">Pro webovou aplikaci, která nemá pracovní prostředí nasazení hlavní větve hello přímo toohello live lokality.</span><span class="sxs-lookup"><span data-stu-id="bcf52-161">For a web app that doesn't have staging environment, it deploys hello master branch directly toohello live site.</span></span>
    
    ![][26]
12. <span data-ttu-id="bcf52-162">V hello **proces** kartě uvidíte prostředí nasazení hello nastavena toohello název cloudové služby nebo webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bcf52-162">In hello **Process** tab, you can see hello deployment environment is set toohello name of your cloud service or web app.</span></span>
    
     ![][27]
13. <span data-ttu-id="bcf52-163">Zadejte hodnoty pro vlastnosti hello, pokud chcete jiné hodnoty než výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="bcf52-163">Specify values for hello properties if you want different values than hello defaults.</span></span> <span data-ttu-id="bcf52-164">Hello vlastnosti pro publikování v Azure jsou ve hello **nasazení** části a může být také nutné tooset MSBuild parametry.</span><span class="sxs-lookup"><span data-stu-id="bcf52-164">hello properties for Azure publishing are in hello **Deployment** section, and you might also need tooset MSBuild parameters.</span></span> <span data-ttu-id="bcf52-165">Například v projektu cloudové služby, toospecify konfigurace služby než "Cloud", nastavte parametry nástroje MSbuild hello příliš`/p:TargetProfile=[YourProfile]` kde *[YourProfile]* odpovídá konfigurační soubor služby s názvem, jako je Objekt ServiceConfiguration. *YourProfile*.cscfg.</span><span class="sxs-lookup"><span data-stu-id="bcf52-165">For example, in a cloud service project, toospecify a service configuration other than "Cloud", set hello MSbuild parameters too`/p:TargetProfile=[YourProfile]` where *[YourProfile]* matches a service configuration file with a name like ServiceConfiguration.*YourProfile*.cscfg.</span></span>
    
     <span data-ttu-id="bcf52-166">Hello následující tabulce jsou uvedeny dostupné vlastnosti hello v hello **nasazení** části:</span><span class="sxs-lookup"><span data-stu-id="bcf52-166">hello following table shows hello available properties in hello **Deployment** section:</span></span>
    
    | <span data-ttu-id="bcf52-167">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="bcf52-167">Property</span></span> | <span data-ttu-id="bcf52-168">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="bcf52-168">Default Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="bcf52-169">Povolit nedůvěryhodné certifikáty</span><span class="sxs-lookup"><span data-stu-id="bcf52-169">Allow Untrusted Certificates</span></span> |<span data-ttu-id="bcf52-170">Pokud je hodnota false, musí být podepsané certifikáty SSL kořenovou autoritou.</span><span class="sxs-lookup"><span data-stu-id="bcf52-170">If false, SSL certificates must be signed by a root authority.</span></span> |
    | <span data-ttu-id="bcf52-171">Povolit upgradu</span><span class="sxs-lookup"><span data-stu-id="bcf52-171">Allow Upgrade</span></span> |<span data-ttu-id="bcf52-172">Umožňuje nasazení tooupdate hello stávajícího nasazení místo vytvoření nové.</span><span class="sxs-lookup"><span data-stu-id="bcf52-172">Allows hello deployment tooupdate an existing deployment instead of creating a new one.</span></span> <span data-ttu-id="bcf52-173">Zachovává hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="bcf52-173">Preserves hello IP address.</span></span> |
    | <span data-ttu-id="bcf52-174">Neodstraňujte</span><span class="sxs-lookup"><span data-stu-id="bcf52-174">Do Not Delete</span></span> |<span data-ttu-id="bcf52-175">V případě hodnoty true Nepřepisovat existující nesouvisejícími nasazení (upgrade je povolena.).</span><span class="sxs-lookup"><span data-stu-id="bcf52-175">If true, do not overwrite an existing unrelated deployment (upgrade is allowed).</span></span> |
    | <span data-ttu-id="bcf52-176">Cesta tooDeployment nastavení</span><span class="sxs-lookup"><span data-stu-id="bcf52-176">Path tooDeployment Settings</span></span> |<span data-ttu-id="bcf52-177">Hello cesta tooyour .pubxml soubor pro webovou aplikaci, relativní toohello kořenové složky hello úložišti.</span><span class="sxs-lookup"><span data-stu-id="bcf52-177">hello path tooyour .pubxml file for a web app, relative toohello root folder of hello repo.</span></span> <span data-ttu-id="bcf52-178">Ignorovat pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="bcf52-178">Ignored for cloud services.</span></span> |
    | <span data-ttu-id="bcf52-179">Prostředí nasazení služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="bcf52-179">Sharepoint Deployment Environment</span></span> |<span data-ttu-id="bcf52-180">Hello stejný jako název služby hello.</span><span class="sxs-lookup"><span data-stu-id="bcf52-180">hello same as hello service name.</span></span> |
    | <span data-ttu-id="bcf52-181">Prostředí nasazení Azure</span><span class="sxs-lookup"><span data-stu-id="bcf52-181">Azure Deployment Environment</span></span> |<span data-ttu-id="bcf52-182">Hello aplikace nebo cloudové název webové služby.</span><span class="sxs-lookup"><span data-stu-id="bcf52-182">hello web app or cloud service name.</span></span> |
14. <span data-ttu-id="bcf52-183">Buildu té doby by se měly dokončit úspěšně.</span><span class="sxs-lookup"><span data-stu-id="bcf52-183">By this time, your build should be completed successfully.</span></span>
    
     ![][28]
15. <span data-ttu-id="bcf52-184">Pokud dvakrát kliknete na název sestavení hello Visual Studio se zobrazí **souhrnu sestavení**, včetně všech výsledků testu z přidružené projektů testování částí.</span><span class="sxs-lookup"><span data-stu-id="bcf52-184">If you double-click hello build name, Visual Studio shows a **Build Summary**, including any test results from associated unit test projects.</span></span>
    
     ![][29]
16. <span data-ttu-id="bcf52-185">V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), hello přidružené nasazení můžete zobrazit na hello **nasazení** kartě, pokud je vybrána hello pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="bcf52-185">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), you can view hello associated deployment on hello **Deployments** tab when hello staging environment is selected.</span></span>
    
     ![][30]
17. <span data-ttu-id="bcf52-186">Procházejte tooyour adresy URL.</span><span class="sxs-lookup"><span data-stu-id="bcf52-186">Browse tooyour site's URL.</span></span> <span data-ttu-id="bcf52-187">Pro webovou aplikaci, vyberte právě hello **Procházet** tlačítko hello portálu.</span><span class="sxs-lookup"><span data-stu-id="bcf52-187">For a web app, just choose  hello **Browse** button in hello portal.</span></span> <span data-ttu-id="bcf52-188">Pro cloudovou službu, zvolte hello URL hello **rychlý přehled** části hello **řídicí panel** stránky, která obsahuje hello pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="bcf52-188">For a cloud service, choose hello URL in hello **Quick Glance** section of hello **Dashboard** page that shows hello Staging environment.</span></span>
    
    <span data-ttu-id="bcf52-189">Nasazení z průběžnou integraci pro cloudové služby jsou publikované toohello pracovní prostředí ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="bcf52-189">Deployments from continuous integration for cloud services are published toohello Staging environment by default.</span></span> <span data-ttu-id="bcf52-190">Můžete to změnit nastavení hello **alternativní prostředí cloudové služby** vlastnost příliš**produkční**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-190">You can change this by setting hello **Alternate Cloud Service Environment** property too**Production**.</span></span> <span data-ttu-id="bcf52-191">Zde je, kde je adresa URL webu hello na stránce řídicího panelu hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="bcf52-191">Here's where hello site URL is on hello cloud service's dashboard page.</span></span>
    
    ![][31]
    
    <span data-ttu-id="bcf52-192">Na nové záložce prohlížeče otevřete tooreveal spuštěné webu.</span><span class="sxs-lookup"><span data-stu-id="bcf52-192">A new browser tab will open tooreveal your running site.</span></span>
    
    ![][32]
18. <span data-ttu-id="bcf52-193">Provedete-li jiné změny tooyour projektu, aktivační události je více sestavení a budou se hromadit více nasazení.</span><span class="sxs-lookup"><span data-stu-id="bcf52-193">If you make other changes tooyour project, you trigger more builds, and you will accumulate multiple deployments.</span></span> <span data-ttu-id="bcf52-194">Hello nejnovější jeden je označen jako aktivní.</span><span class="sxs-lookup"><span data-stu-id="bcf52-194">hello latest one is marked as Active.</span></span>
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a><span data-ttu-id="bcf52-195">5: znovu nasaďte starší sestavení</span><span class="sxs-lookup"><span data-stu-id="bcf52-195">5: Redeploy an earlier build</span></span>
<span data-ttu-id="bcf52-196">Tento krok je volitelný.</span><span class="sxs-lookup"><span data-stu-id="bcf52-196">This step is optional.</span></span> <span data-ttu-id="bcf52-197">V hello portál Azure classic, zvolte k předchozímu nasazení a **znovu nasaďte** toorewind vaší lokality tooan dříve vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="bcf52-197">In hello Azure classic portal, choose an earlier deployment and choose **Redeploy** toorewind your site tooan earlier check-in.</span></span> <span data-ttu-id="bcf52-198">Všimněte si, že to aktivují nového sestavení v sadě TFS a vytvořit nový záznam v historii nasazení.</span><span class="sxs-lookup"><span data-stu-id="bcf52-198">Note that this will trigger a new build in TFS and create a new entry in your deployment history.</span></span>

![][34]

## <a name="6-change-hello-production-deployment"></a><span data-ttu-id="bcf52-199">6: změnit hello produkčního nasazení</span><span class="sxs-lookup"><span data-stu-id="bcf52-199">6: Change hello Production deployment</span></span>
<span data-ttu-id="bcf52-200">Jakmile budete připraveni, můžete zvýšit úroveň hello pracovní prostředí toohello produkčního prostředí, tak, že zvolíte **odkládacího souboru** v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="bcf52-200">When you are ready, you can promote hello Staging environment toohello Production environment by choosing **Swap** in hello Azure classic portal.</span></span> <span data-ttu-id="bcf52-201">Hello nově nasazené pracovní prostředí se zvýšenou úrovní tooProduction a hello předchozí provozním prostředí, pokud existuje, bude pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="bcf52-201">hello newly deployed Staging environment is promoted tooProduction, and hello previous Production environment, if any, becomes a Staging environment.</span></span> <span data-ttu-id="bcf52-202">Hello aktivní nasazení může být různé pro hello produkční a pracovní prostředí, ale historii nasazení hello poslední sestavení je hello stejný bez ohledu na to prostředí.</span><span class="sxs-lookup"><span data-stu-id="bcf52-202">hello Active deployment may be different for hello Production and Staging environments, but hello deployment history of recent builds is hello same regardless of environment.</span></span>

![][35]

## <a name="7-deploy-from-a-working-branch"></a><span data-ttu-id="bcf52-203">7: nasazení z pracovní větev.</span><span class="sxs-lookup"><span data-stu-id="bcf52-203">7: Deploy from a working branch.</span></span>
<span data-ttu-id="bcf52-204">Při použití Git, obvykle provedete změny v pracovní větev a integrovat do hlavní větve hello, pokud váš vývojový dosáhne konečného stavu.</span><span class="sxs-lookup"><span data-stu-id="bcf52-204">When you use Git, you usually make changes in a working branch and integrate into hello master branch when your development reaches a finished state.</span></span> <span data-ttu-id="bcf52-205">V průběhu fáze vývoje hello projektu budete má toobuild a nasaďte hello pracovní větev tooAzure.</span><span class="sxs-lookup"><span data-stu-id="bcf52-205">During hello development phase of a project, you'll want toobuild and deploy hello working branch tooAzure.</span></span>

1. <span data-ttu-id="bcf52-206">V **Team Explorer**, zvolte hello **Domů** tlačítko a potom zvolte hello **větví** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bcf52-206">In **Team Explorer**, choose hello **Home** button and then choose hello **Branches** button.</span></span>
   
    ![][40]
2. <span data-ttu-id="bcf52-207">Zvolte hello **nové větve** odkaz.</span><span class="sxs-lookup"><span data-stu-id="bcf52-207">Choose hello **New Branch** link.</span></span>
   
    ![][41]
3. <span data-ttu-id="bcf52-208">Zadejte název hello hello větve, jako je například "fungoval," a zvolte **vytvoření větve**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-208">Enter hello name of hello branch, such as "working," and choose **Create Branch**.</span></span> <span data-ttu-id="bcf52-209">Tím se vytvoří nové místní větve.</span><span class="sxs-lookup"><span data-stu-id="bcf52-209">This creates a new local branch.</span></span>
   
    ![][42]
4. <span data-ttu-id="bcf52-210">Publikujte hello větev.</span><span class="sxs-lookup"><span data-stu-id="bcf52-210">Publish hello branch.</span></span> <span data-ttu-id="bcf52-211">Zvolte název větve hello v **Nepublikováno větví**a zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-211">Choose hello branch name in **Unpublished branches**, and choose **Publish**.</span></span>
   
    ![][44]
5. <span data-ttu-id="bcf52-212">Ve výchozím nastavení pouze změní aktivační událost hlavní větve toohello průběžné sestavení.</span><span class="sxs-lookup"><span data-stu-id="bcf52-212">By default, only changes toohello master branch trigger a continuous build.</span></span> <span data-ttu-id="bcf52-213">tooset až průběžné sestavení pro pracovní větve, zvolte hello **sestavení** stránky v **Team Explorer**a zvolte **upravit definici sestavení**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-213">tooset up continuous build for a working branch, choose hello **Builds** page in **Team Explorer**, and choose **Edit Build Definition**.</span></span>
6. <span data-ttu-id="bcf52-214">Otevřete hello **nastavení zdroje** kartě. V části **monitorovat větve pro průběžnou integraci a sestavení**, zvolte **tooadd nový řádek, klikněte sem**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-214">Open hello **Source Settings** tab. Under **Monitored branches for continuous integration and build**, choose **Click here tooadd a new row**.</span></span>
   
    ![][47]
7. <span data-ttu-id="bcf52-215">Zadejte hello větev, kterou jste vytvořili, například odolný systém souborů nebo oznámení nebo pracovní.</span><span class="sxs-lookup"><span data-stu-id="bcf52-215">Specify hello branch you created, such as refs/heads/working.</span></span>
   
    ![][48]
8. <span data-ttu-id="bcf52-216">Provést změny v kódu hello, otevřete hello místní nabídky souboru hello změnit a potom zvolte **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="bcf52-216">Make a change in hello code, open hello shortcut menu for hello changed file, and then choose **Commit**.</span></span>
   
    ![][43]
9. <span data-ttu-id="bcf52-217">Zvolte hello **nesynchronizované potvrzení** propojit a zvolte hello **synchronizace** tlačítko nebo hello **Push** odkaz toocopy hello změny toohello kopii hello pracovní větev v sadě Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="bcf52-217">Choose hello **Unsynced Commits** link, and choose  hello **Sync** button or hello **Push** link toocopy hello changes toohello copy of hello working branch in Visual Studio Team Services.</span></span>
   
   ![][45]
10. <span data-ttu-id="bcf52-218">Přejděte toohello **sestavení** zobrazení a najít hello sestavení, který právě získali aktivoval pro hello pracovní větev.</span><span class="sxs-lookup"><span data-stu-id="bcf52-218">Navigate toohello **Builds** view and find hello build that just got triggered for hello working branch.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bcf52-219">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bcf52-219">Next steps</span></span>
<span data-ttu-id="bcf52-220">najdete v části Další tipy pro Visual Studio Team Services, pomocí Git toolearn [vývoj a sdílet kódu v úložišti Git pomocí sady Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) a informace o použití úložiště Git, který není spravován nástrojem Visual Studio Team Services toopublish tooAzure, najdete v části [tooAzure průběžné nasazování služby App Service](../app-service-web/app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="bcf52-220">toolearn more tips on using Git with Visual Studio Team Services, see [Develop and share your code in Git using Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) and for information about using a Git repository that's not managed by Visual Studio Team Services toopublish tooAzure, see [Continuous Deployment tooAzure App Service](../app-service-web/app-service-continuous-deployment.md).</span></span> <span data-ttu-id="bcf52-221">Další informace o sadě Visual Studio Team Services najdete v tématu [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span><span class="sxs-lookup"><span data-stu-id="bcf52-221">For more information on Visual Studio Team Services, see [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span></span>

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
