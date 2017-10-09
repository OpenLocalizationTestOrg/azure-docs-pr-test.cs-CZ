---
title: "doručení aaaContinuous s Visual Studio Team Services v Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Visual Studio Team Services týmové projekty tooautomatically sestavení a nasazení funkce toohello webové aplikace v Azure App Service nebo cloudové služby."
services: cloud-services
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: 797f67ad-e4d4-4063-ae91-41cbdf154191
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/06/2016
ms.author: mlearned
ms.openlocfilehash: eae75729e1c1a55f9bc3375604a8192f329d0042
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-tooazure-using-visual-studio-team-services"></a><span data-ttu-id="25ebd-103">Pomocí Visual Studio Team Services tooAzure nastavené průběžné doručování</span><span class="sxs-lookup"><span data-stu-id="25ebd-103">Continuous delivery tooAzure using Visual Studio Team Services</span></span>
<span data-ttu-id="25ebd-104">Konfigurace buildu tooautomatically týmových projektů Visual Studio Team Services a nasazením tooAzure webové aplikace nebo cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="25ebd-104">You can configure your Visual Studio Team Services team projects tooautomatically build and deploy tooAzure web apps or cloud services.</span></span>  <span data-ttu-id="25ebd-105">(Informace o tom, jak tooset až průběžné sestavení a nasadit pomocí systému *místní* Team Foundation Server najdete v části [nastavené průběžné doručování pro cloudové služby v Azure](cloud-services-dotnet-continuous-delivery.md).)</span><span class="sxs-lookup"><span data-stu-id="25ebd-105">(For information on how tooset up a continuous build and deploy system using an *on-premises* Team Foundation Server, see [Continuous Delivery for Cloud Services in Azure](cloud-services-dotnet-continuous-delivery.md).)</span></span>

<span data-ttu-id="25ebd-106">Tento kurz předpokládá, že máte Visual Studio 2013 a Azure SDK nainstalovat hello.</span><span class="sxs-lookup"><span data-stu-id="25ebd-106">This tutorial assumes you have Visual Studio 2013 and hello Azure SDK installed.</span></span> <span data-ttu-id="25ebd-107">Pokud ještě nemáte Visual Studio 2013, stáhněte ho tak, že zvolíte hello **začněte zadarmo** odkaz na [www.visualstudio.com](http://www.visualstudio.com). Instalace hello Azure SDK z [zde](http://go.microsoft.com/fwlink/?LinkId=239540).</span><span class="sxs-lookup"><span data-stu-id="25ebd-107">If you don't already have Visual Studio 2013, download it by choosing hello **Get started for free** link at [www.visualstudio.com](http://www.visualstudio.com). Install hello Azure SDK from [here](http://go.microsoft.com/fwlink/?LinkId=239540).</span></span>

> [!NOTE]
> <span data-ttu-id="25ebd-108">Budete potřebovat Visual Studio Team Services toocomplete účet v tomto kurzu: můžete [zdarma otevřít účet Visual Studio Team Services](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span><span class="sxs-lookup"><span data-stu-id="25ebd-108">You need an Visual Studio Team Services account toocomplete this tutorial: You can [open a Visual Studio Team Services account for free](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span></span>
> 
> 

<span data-ttu-id="25ebd-109">tooset nahoru cloudové služby tooautomatically sestavení a nasazení tooAzure pomocí Visual Studio Team Services, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="25ebd-109">tooset up a cloud service tooautomatically build and deploy tooAzure by using Visual Studio Team Services, follow these steps.</span></span>

## <a name="1-create-a-team-project"></a><span data-ttu-id="25ebd-110">1: Vytvoření týmového projektu</span><span class="sxs-lookup"><span data-stu-id="25ebd-110">1: Create a team project</span></span>
<span data-ttu-id="25ebd-111">Postupujte podle pokynů hello [sem](http://go.microsoft.com/fwlink/?LinkId=512980) toocreate váš tým projektu a propojit jej tooVisual Studio.</span><span class="sxs-lookup"><span data-stu-id="25ebd-111">Follow hello instructions [here](http://go.microsoft.com/fwlink/?LinkId=512980) toocreate your team project and link it tooVisual Studio.</span></span> <span data-ttu-id="25ebd-112">Tento návod předpokládá, že používáte jako řešení pro řízení zdrojového serveru Team Foundation verze ovládacího prvku (TFVC).</span><span class="sxs-lookup"><span data-stu-id="25ebd-112">This walkthrough assumes you are using Team Foundation Version Control (TFVC) as your source control solution.</span></span> <span data-ttu-id="25ebd-113">Pokud chcete toouse Git pro správu verzí, najdete v části [hello Git verze tohoto návodu](http://go.microsoft.com/fwlink/p/?LinkId=397358).</span><span class="sxs-lookup"><span data-stu-id="25ebd-113">If you want toouse Git for version control, see [hello Git version of this walkthrough](http://go.microsoft.com/fwlink/p/?LinkId=397358).</span></span>

## <a name="2-check-in-a-project-toosource-control"></a><span data-ttu-id="25ebd-114">2: Zkontrolujte v ovládacím prvku toosource projektu</span><span class="sxs-lookup"><span data-stu-id="25ebd-114">2: Check in a project toosource control</span></span>
1. <span data-ttu-id="25ebd-115">V sadě Visual Studio otevřete řešení hello sami toodeploy, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="25ebd-115">In Visual Studio, open hello solution you want toodeploy, or create a new one.</span></span>
   <span data-ttu-id="25ebd-116">Můžete nasadit webovou aplikaci nebo cloudové služby (aplikace Azure) podle následujících hello kroky v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="25ebd-116">You can deploy a web app or a cloud service (Azure Application) by following hello steps in this walkthrough.</span></span>
   <span data-ttu-id="25ebd-117">Pokud chcete toocreate nové řešení, vytvořte nový projekt Azure Cloud Service, nebo nový projekt ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="25ebd-117">If you want toocreate a new solution, create a new Azure Cloud Service project, or a new ASP.NET MVC project.</span></span> <span data-ttu-id="25ebd-118">Ujistěte se, který hello cíle projektu rozhraní .NET Framework 4 nebo 4.5 a při vytváření projektu cloudové služby, přidat webová role ASP.NET MVC a roli pracovního procesu a zvolte Internetové aplikace hello webové role.</span><span class="sxs-lookup"><span data-stu-id="25ebd-118">Make sure that hello project targets .NET Framework 4 or 4.5, and if you are creating a cloud service project, add an ASP.NET MVC web role and a worker role, and choose Internet application for hello web role.</span></span> <span data-ttu-id="25ebd-119">Po zobrazení výzvy vyberte **Internetové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="25ebd-119">When prompted, choose **Internet Application**.</span></span>
   <span data-ttu-id="25ebd-120">Pokud chcete toocreate webovou aplikaci, zvolte šablona projektu webové aplikace ASP.NET hello a potom zvolte MVC.</span><span class="sxs-lookup"><span data-stu-id="25ebd-120">If you want toocreate a web app, choose hello ASP.NET Web Application project template, and then choose MVC.</span></span> <span data-ttu-id="25ebd-121">V tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="25ebd-121">See [Create an ASP.NET web app in Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="25ebd-122">Visual Studio Team Services nyní podporují pouze CI nasazení webových aplikací, Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25ebd-122">Visual Studio Team Services only support CI deployments of Visual Studio Web Applications at this time.</span></span> <span data-ttu-id="25ebd-123">Webové projekty jsou mimo rozsah.</span><span class="sxs-lookup"><span data-stu-id="25ebd-123">Web Site projects are out of scope.</span></span>
   > 
   > 
2. <span data-ttu-id="25ebd-124">Otevřete řešení hello hello kontextovou nabídku a vyberte **tooSource přidat řešení řízení**.</span><span class="sxs-lookup"><span data-stu-id="25ebd-124">Open hello context menu for hello solution, and choose **Add Solution tooSource Control**.</span></span>
   
    ![][5]
3. <span data-ttu-id="25ebd-125">Potvrďte nebo změňte výchozí nastavení hello a zvolte hello **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="25ebd-125">Accept or change hello defaults and choose hello **OK** button.</span></span> <span data-ttu-id="25ebd-126">Po dokončení procesu hello zdroj ovládacího prvku ikony zobrazovat v **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="25ebd-126">Once hello process completes, source control icons appear in **Solution Explorer**.</span></span>
   
    ![][6]
4. <span data-ttu-id="25ebd-127">Otevřete hello místní nabídku pro hello řešení a zvolte **změnami**.</span><span class="sxs-lookup"><span data-stu-id="25ebd-127">Open hello shortcut menu for hello solution, and choose **Check In**.</span></span>
   
    ![][7]
5. <span data-ttu-id="25ebd-128">V hello **čekající změny** oblasti **Team Explorer**, zadejte komentář pro hello vrácení se změnami a zvolte hello **změnami** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="25ebd-128">In hello **Pending Changes** area of **Team Explorer**, type a comment for hello check-in and choose hello **Check In** button.</span></span>
   
    ![][8]
   
    <span data-ttu-id="25ebd-129">Poznámka: možnosti tooinclude hello nebo vyloučit konkrétní změny při se změnami.</span><span class="sxs-lookup"><span data-stu-id="25ebd-129">Note hello options tooinclude or exclude specific changes when you check in.</span></span> <span data-ttu-id="25ebd-130">Pokud chcete, jsou vyloučeny změny, zvolte hello **zahrnují všechny** odkaz.</span><span class="sxs-lookup"><span data-stu-id="25ebd-130">If desired changes are excluded, choose hello **Include All** link.</span></span>
   
    ![][9]

## <a name="3-connect-hello-project-tooazure"></a><span data-ttu-id="25ebd-131">3: připojení tooAzure projektu hello</span><span class="sxs-lookup"><span data-stu-id="25ebd-131">3: Connect hello project tooAzure</span></span>
1. <span data-ttu-id="25ebd-132">Nyní když máte Visual Studio Team Services týmového projektu s některé zdrojového kódu v ní, se připravené tooconnect váš tým projektu tooAzure.</span><span class="sxs-lookup"><span data-stu-id="25ebd-132">Now that you have a VS Team Services team project with some source code in it, you are ready tooconnect your team project tooAzure.</span></span>  <span data-ttu-id="25ebd-133">V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), vyberte svou cloudové služby nebo webovou aplikaci nebo vytvořte novou výběrem hello  **+**  ikonu v dolní hello vlevo a výběr **cloudovéslužby** nebo **webová aplikace** a potom **rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="25ebd-133">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), select your cloud service or web app, or create a new one by choosing hello **+** icon at hello bottom left and choosing **Cloud Service** or **Web App** and then **Quick Create**.</span></span> <span data-ttu-id="25ebd-134">Zvolte hello **nastavit publikování pomocí Visual Studio Team Services** odkaz.</span><span class="sxs-lookup"><span data-stu-id="25ebd-134">Choose hello **Set up publishing with Visual Studio Team Services** link.</span></span>
   
    ![][10]
2. <span data-ttu-id="25ebd-135">V Průvodci hello hello název svého účtu Visual Studio Team Services v textovém poli hello a klikněte na hello **autorizovat teď** odkaz.</span><span class="sxs-lookup"><span data-stu-id="25ebd-135">In hello wizard, type hello name of your Visual Studio Team Services account in hello textbox and click hello **Authorize Now** link.</span></span> <span data-ttu-id="25ebd-136">Můžete být požádáni toosign v.</span><span class="sxs-lookup"><span data-stu-id="25ebd-136">You might be asked toosign in.</span></span>
   
    ![][11]
3. <span data-ttu-id="25ebd-137">V hello **požadavku na připojení** automaticky otevíraný dialog, zvolte hello **přijmout** tooauthorize tlačítko Azure tooconfigure váš tým projektu ve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="25ebd-137">In hello **Connection Request** pop-up dialog, choose hello **Accept** button tooauthorize Azure tooconfigure your team project in VS Team Services.</span></span>
   
    ![][12]
4. <span data-ttu-id="25ebd-138">Při ověřování úspěšné, uvidíte rozevírací seznam obsahující seznam týmových projektech Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="25ebd-138">When authorization succeeds, you see a dropdown containing a list of your Visual Studio Team Services team projects.</span></span> <span data-ttu-id="25ebd-139">Zvolte název hello týmový projekt, který jste vytvořili v předchozích krocích hello a potom zvolte tlačítko zaškrtnutí hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="25ebd-139">Choose  hello name of team project that you created in hello previous steps, and then choose hello wizard's checkmark button.</span></span>
   
    ![][13]
5. <span data-ttu-id="25ebd-140">Po projektu je propojená, zobrazí se některé pokyny k vrácení se změnami změny tooyour Visual Studio Team Services týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="25ebd-140">After your project is linked, you will see some instructions for checking in changes tooyour Visual Studio Team Services team project.</span></span>  <span data-ttu-id="25ebd-141">Na vaše další vrácení se změnami Visual Studio Team Services bude sestavovat a nasadit tooAzure vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="25ebd-141">On your next check-in, Visual Studio Team Services will build and deploy your project tooAzure.</span></span>  <span data-ttu-id="25ebd-142">Zkuste tuto chybu kliknutím hello **změnami ze sady Visual Studio** propojit a pak hello **spusťte Visual Studio** propojení (nebo ekvivalentní hello **Visual Studio** tlačítko dole hello úvodní portálu obrazovka).</span><span class="sxs-lookup"><span data-stu-id="25ebd-142">Try this now by clicking hello **Check In from Visual Studio** link, and then hello **Launch Visual Studio** link (or hello equivalent **Visual Studio** button at hello bottom of hello portal screen).</span></span>
   
    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a><span data-ttu-id="25ebd-143">4: aktivovat opětovném sestavení a znovu nasaďte projekt</span><span class="sxs-lookup"><span data-stu-id="25ebd-143">4: Trigger a rebuild and redeploy your project</span></span>
1. <span data-ttu-id="25ebd-144">V sadě Visual Studio **Team Explorer**, zvolte hello **Průzkumník správy zdrojového kódu** odkaz.</span><span class="sxs-lookup"><span data-stu-id="25ebd-144">In Visual Studio's **Team Explorer**, choose hello **Source Control Explorer** link.</span></span>
   
    ![][15]
2. <span data-ttu-id="25ebd-145">Přejděte na soubor řešení tooyour a otevřete jej.</span><span class="sxs-lookup"><span data-stu-id="25ebd-145">Navigate tooyour solution file and open it.</span></span>
   
    ![][16]
3. <span data-ttu-id="25ebd-146">V **Průzkumníku**, otevřete soubor a ho změnit.</span><span class="sxs-lookup"><span data-stu-id="25ebd-146">In **Solution Explorer**, open up a file and change it.</span></span> <span data-ttu-id="25ebd-147">Můžete například změnit soubor hello `_Layout.cshtml` pod zobrazení hello\\sdílené složky MVC webové role.</span><span class="sxs-lookup"><span data-stu-id="25ebd-147">For example, change hello file `_Layout.cshtml` under hello Views\\Shared folder in an MVC web role.</span></span>
   
    ![][17]
4. <span data-ttu-id="25ebd-148">Upravit hello logo hello lokality a zvolte **Ctrl + S** toosave hello souboru.</span><span class="sxs-lookup"><span data-stu-id="25ebd-148">Edit hello logo for hello site and choose **Ctrl+S** toosave hello file.</span></span>
   
    ![][18]
5. <span data-ttu-id="25ebd-149">V **Team Explorer**, zvolte hello **čekající změny** odkaz.</span><span class="sxs-lookup"><span data-stu-id="25ebd-149">In **Team Explorer**, choose hello **Pending Changes** link.</span></span>
   
    ![][19]
6. <span data-ttu-id="25ebd-150">Zadejte komentář a potom zvolte hello **změnami** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="25ebd-150">Enter a comment and then choose hello **Check In** button.</span></span>
   
    ![][20]
7. <span data-ttu-id="25ebd-151">Zvolte hello **domácí** tlačítko tooreturn toohello **Team Explorer** domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="25ebd-151">Choose hello **Home** button tooreturn toohello **Team Explorer** home page.</span></span>
   
    ![][21]
8. <span data-ttu-id="25ebd-152">Zvolte hello **sestavení** hello tooview odkaz sestavení v průběhu.</span><span class="sxs-lookup"><span data-stu-id="25ebd-152">Choose hello **Builds** link tooview hello builds in progress.</span></span>
   
    ![][22]
   
    <span data-ttu-id="25ebd-153">**Team Explorer** ukazuje, že byla spuštěna sestavení pro vaše vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="25ebd-153">**Team Explorer** shows that a build has been triggered for your check-in.</span></span>
   
    ![][23]
9. <span data-ttu-id="25ebd-154">Dvakrát klikněte na název hello hello sestavení v průběhu tooview podrobné protokolu sestavení průběhu hello.</span><span class="sxs-lookup"><span data-stu-id="25ebd-154">Double-click hello name of hello build in progress tooview a detailed log as hello build progresses.</span></span>
   
    ![][24]
10. <span data-ttu-id="25ebd-155">Probíhá při sestavení hello prohlédněte hello definice sestavení, která byla vytvořena, když jste propojili TFS tooAzure pomocí Průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="25ebd-155">While hello build is in-progress, take a look at hello build definition that was created when you linked TFS tooAzure by using hello wizard.</span></span>  <span data-ttu-id="25ebd-156">Otevřete hello místní nabídku pro definici sestavení hello a zvolte **upravit definici sestavení**.</span><span class="sxs-lookup"><span data-stu-id="25ebd-156">Open hello shortcut menu for hello build definition and choose **Edit Build Definition**.</span></span>
    
     ![][25]
    
     <span data-ttu-id="25ebd-157">V hello **aktivační událost** karty, zobrazí se, že definici sestavení hello toobuild na každý vrácení se změnami ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="25ebd-157">In hello **Trigger** tab, you will see that hello build definition is set toobuild on every check-in by default.</span></span>
    
     ![][26]
    
     <span data-ttu-id="25ebd-158">V hello **proces** kartě uvidíte prostředí nasazení hello nastavena toohello název cloudové služby nebo webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="25ebd-158">In hello **Process** tab, you can see hello deployment environment is set toohello name of your cloud service or web app.</span></span> <span data-ttu-id="25ebd-159">Pokud pracujete s webovými aplikacemi, bude hello vlastnosti, které vidíte liší od těch, které tady uvedené.</span><span class="sxs-lookup"><span data-stu-id="25ebd-159">If you are working with web apps, hello properties you see will be different from those shown here.</span></span>
    
     ![][27]
11. <span data-ttu-id="25ebd-160">Zadejte hodnoty pro vlastnosti hello, pokud chcete jiné hodnoty než výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="25ebd-160">Specify values for hello properties if you want different values than hello defaults.</span></span> <span data-ttu-id="25ebd-161">Hello vlastnosti pro publikování v Azure jsou ve hello **nasazení** části.</span><span class="sxs-lookup"><span data-stu-id="25ebd-161">hello properties for Azure publishing are in hello **Deployment** section.</span></span>
    
     <span data-ttu-id="25ebd-162">Hello následující tabulce jsou uvedeny dostupné vlastnosti hello v hello **nasazení** části:</span><span class="sxs-lookup"><span data-stu-id="25ebd-162">hello following table shows hello available properties in hello **Deployment** section:</span></span>
    
    | <span data-ttu-id="25ebd-163">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="25ebd-163">Property</span></span> | <span data-ttu-id="25ebd-164">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="25ebd-164">Default Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="25ebd-165">Povolit nedůvěryhodné certifikáty</span><span class="sxs-lookup"><span data-stu-id="25ebd-165">Allow Untrusted Certificates</span></span> |<span data-ttu-id="25ebd-166">Pokud je hodnota false, musí být podepsané certifikáty SSL kořenovou autoritou.</span><span class="sxs-lookup"><span data-stu-id="25ebd-166">If false, SSL certificates must be signed by a root authority.</span></span> |
    | <span data-ttu-id="25ebd-167">Povolit upgradu</span><span class="sxs-lookup"><span data-stu-id="25ebd-167">Allow Upgrade</span></span> |<span data-ttu-id="25ebd-168">Umožňuje nasazení tooupdate hello stávajícího nasazení místo vytvoření nové.</span><span class="sxs-lookup"><span data-stu-id="25ebd-168">Allows hello deployment tooupdate an existing deployment instead of creating a new one.</span></span> <span data-ttu-id="25ebd-169">Zachovává hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="25ebd-169">Preserves hello IP address.</span></span> |
    | <span data-ttu-id="25ebd-170">Neodstraňujte</span><span class="sxs-lookup"><span data-stu-id="25ebd-170">Do Not Delete</span></span> |<span data-ttu-id="25ebd-171">V případě hodnoty true Nepřepisovat existující nesouvisejícími nasazení (upgrade je povolena.).</span><span class="sxs-lookup"><span data-stu-id="25ebd-171">If true, do not overwrite an existing unrelated deployment (upgrade is allowed).</span></span> |
    | <span data-ttu-id="25ebd-172">Cesta tooDeployment nastavení</span><span class="sxs-lookup"><span data-stu-id="25ebd-172">Path tooDeployment Settings</span></span> |<span data-ttu-id="25ebd-173">Hello cesta tooyour .pubxml soubor pro webovou aplikaci, relativní toohello kořenové složky hello úložišti.</span><span class="sxs-lookup"><span data-stu-id="25ebd-173">hello path tooyour .pubxml file for a web app, relative toohello root folder of hello repo.</span></span> <span data-ttu-id="25ebd-174">Ignorovat pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="25ebd-174">Ignored for cloud services.</span></span> |
    | <span data-ttu-id="25ebd-175">Prostředí nasazení služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="25ebd-175">Sharepoint Deployment Environment</span></span> |<span data-ttu-id="25ebd-176">Hello stejný jako název služby hello.</span><span class="sxs-lookup"><span data-stu-id="25ebd-176">hello same as hello service name.</span></span> |
    | <span data-ttu-id="25ebd-177">Prostředí nasazení Azure</span><span class="sxs-lookup"><span data-stu-id="25ebd-177">Azure Deployment Environment</span></span> |<span data-ttu-id="25ebd-178">Hello aplikace nebo cloudové název webové služby.</span><span class="sxs-lookup"><span data-stu-id="25ebd-178">hello web app or cloud service name.</span></span> |
12. <span data-ttu-id="25ebd-179">Pokud používáte více konfigurace služby (soubory .cscfg), abyste mohli zadat konfiguraci hello požadované služby v hello **sestavení, Upřesnit, argumenty MSBuild** nastavení.</span><span class="sxs-lookup"><span data-stu-id="25ebd-179">If you are using multiple service configurations (.cscfg files), you can specify hello desired service configuration in hello **Build, Advanced, MSBuild arguments** setting.</span></span> <span data-ttu-id="25ebd-180">Například toouse ServiceConfiguration.Test.cscfg, nastavte argumenty MSBuild řádku `/p:TargetProfile=Test`.</span><span class="sxs-lookup"><span data-stu-id="25ebd-180">For example, toouse ServiceConfiguration.Test.cscfg, set MSBuild arguments line option `/p:TargetProfile=Test`.</span></span>
    
     ![][38]
    
     <span data-ttu-id="25ebd-181">Buildu té doby by se měly dokončit úspěšně.</span><span class="sxs-lookup"><span data-stu-id="25ebd-181">By this time, your build should be completed successfully.</span></span>
    
     ![][28]
13. <span data-ttu-id="25ebd-182">Pokud dvakrát kliknete na název sestavení hello Visual Studio se zobrazí **souhrnu sestavení**, včetně všech výsledků testu z přidružené projektů testování částí.</span><span class="sxs-lookup"><span data-stu-id="25ebd-182">If you double-click hello build name, Visual Studio shows a **Build Summary**, including any test results from associated unit test projects.</span></span>
    
     ![][29]
14. <span data-ttu-id="25ebd-183">V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), hello přidružené nasazení můžete zobrazit na hello **nasazení** kartě, pokud je vybrána hello pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="25ebd-183">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), you can view hello associated deployment on hello **Deployments** tab when hello staging environment is selected.</span></span>
    
     ![][30]
15. <span data-ttu-id="25ebd-184">Procházejte tooyour adresy URL.</span><span class="sxs-lookup"><span data-stu-id="25ebd-184">Browse tooyour site's URL.</span></span> <span data-ttu-id="25ebd-185">Pro webovou aplikaci, stačí kliknout na hello **Procházet** tlačítka na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="25ebd-185">For a web app, just click hello **Browse** button on hello command bar.</span></span> <span data-ttu-id="25ebd-186">Pro cloudovou službu, zvolte hello URL hello **rychlý přehled** části hello **řídicí panel** stránky, která obsahuje hello pracovní prostředí pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="25ebd-186">For a cloud service, choose hello URL in hello **Quick Glance** section of hello **Dashboard** page that shows hello Staging environment for a cloud service.</span></span> <span data-ttu-id="25ebd-187">Nasazení z průběžnou integraci pro cloudové služby jsou publikované toohello pracovní prostředí ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="25ebd-187">Deployments from continuous integration for cloud services are published toohello Staging environment by default.</span></span> <span data-ttu-id="25ebd-188">Můžete to změnit nastavení hello **alternativní prostředí cloudové služby** vlastnost příliš**produkční**.</span><span class="sxs-lookup"><span data-stu-id="25ebd-188">You can change this by setting hello **Alternate Cloud Service Environment** property too**Production**.</span></span> <span data-ttu-id="25ebd-189">Tento snímek obrazovky ukazuje, kde hello že adresa URL webu je na stránce řídicího panelu hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="25ebd-189">This screenshot shows where hello site URL is on hello cloud service's dashboard page.</span></span>
    
    ![][31]
    
    <span data-ttu-id="25ebd-190">Na nové záložce prohlížeče otevřete tooreveal spuštěné webu.</span><span class="sxs-lookup"><span data-stu-id="25ebd-190">A new browser tab will open tooreveal your running site.</span></span>
    
    ![][32]
    
    <span data-ttu-id="25ebd-191">Pro cloudové služby Pokud provedete projektu tooyour další změny, aktivační události je více sestavení a budou se hromadit více nasazení.</span><span class="sxs-lookup"><span data-stu-id="25ebd-191">For cloud services, if you make other changes tooyour project, you trigger more builds, and you will accumulate multiple deployments.</span></span> <span data-ttu-id="25ebd-192">Hello nejnovější jeden označen jako aktivní.</span><span class="sxs-lookup"><span data-stu-id="25ebd-192">hello latest one marked as Active.</span></span>
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a><span data-ttu-id="25ebd-193">5: znovu nasaďte starší sestavení</span><span class="sxs-lookup"><span data-stu-id="25ebd-193">5: Redeploy an earlier build</span></span>
<span data-ttu-id="25ebd-194">Tento krok platí toocloud služby a je volitelný.</span><span class="sxs-lookup"><span data-stu-id="25ebd-194">This step applies toocloud services and is optional.</span></span> <span data-ttu-id="25ebd-195">V hello portál Azure classic, vyberte k předchozímu nasazení a potom zvolte hello **znovu nasaďte** tlačítko toorewind vaší lokality tooan dříve vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="25ebd-195">In hello Azure classic portal, choose an earlier deployment and then choose hello **Redeploy** button toorewind your site tooan earlier check-in.</span></span>  <span data-ttu-id="25ebd-196">Všimněte si, že to aktivují nového sestavení v sadě TFS a vytvořit nový záznam v historii nasazení.</span><span class="sxs-lookup"><span data-stu-id="25ebd-196">Note that this will trigger a new build in TFS and create a new entry in your deployment history.</span></span>

![][34]

## <a name="6-change-hello-production-deployment"></a><span data-ttu-id="25ebd-197">6: změnit hello produkčního nasazení</span><span class="sxs-lookup"><span data-stu-id="25ebd-197">6: Change hello Production deployment</span></span>
<span data-ttu-id="25ebd-198">Tento krok platí jenom toocloud služby, není webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="25ebd-198">This step applies only toocloud services, not web apps.</span></span> <span data-ttu-id="25ebd-199">Jakmile budete připraveni, můžete zvýšit úroveň hello pracovní prostředí toohello produkčního prostředí, tak, že zvolíte hello **Prohodit** tlačítka na hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="25ebd-199">When you are ready, you can promote hello Staging environment toohello production environment by choosing hello **Swap** button in hello Azure classic portal.</span></span> <span data-ttu-id="25ebd-200">Hello nově nasazené pracovní prostředí se zvýšenou úrovní tooProduction a hello předchozí provozním prostředí, pokud existuje, bude pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="25ebd-200">hello newly deployed Staging environment is promoted tooProduction, and hello previous Production environment, if any, becomes a Staging environment.</span></span> <span data-ttu-id="25ebd-201">Hello aktivní nasazení může být různé pro hello produkční a pracovní prostředí, ale historii nasazení hello poslední sestavení je hello stejný bez ohledu na to prostředí.</span><span class="sxs-lookup"><span data-stu-id="25ebd-201">hello Active deployment may be different for hello Production and Staging environments, but hello deployment history of recent builds is hello same regardless of environment.</span></span>

![][35]

## <a name="7-run-unit-tests"></a><span data-ttu-id="25ebd-202">7: spouštění testů jednotek</span><span class="sxs-lookup"><span data-stu-id="25ebd-202">7: Run unit tests</span></span>
<span data-ttu-id="25ebd-203">Tento krok platí jenom tooweb aplikace, není cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="25ebd-203">This step applies only tooweb apps, not cloud services.</span></span> <span data-ttu-id="25ebd-204">tooput brány kvality na vaše nasazení, můžete spustit testy jednotek a jejich případné selhání, můžete zastavit nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="25ebd-204">tooput a quality gate on your deployment, you can run unit tests and if they fail, you can stop hello deployment.</span></span>

1. <span data-ttu-id="25ebd-205">V sadě Visual Studio přidejte projektu testování částí.</span><span class="sxs-lookup"><span data-stu-id="25ebd-205">In Visual Studio, add a unit test project.</span></span>
   
   ![][39]
2. <span data-ttu-id="25ebd-206">Přidáte projekt toohello odkazy na projekt chcete tootest.</span><span class="sxs-lookup"><span data-stu-id="25ebd-206">Add project references toohello project you want tootest.</span></span>
   
   ![][40]
3. <span data-ttu-id="25ebd-207">Přidejte některé testy jednotek.</span><span class="sxs-lookup"><span data-stu-id="25ebd-207">Add some unit tests.</span></span> <span data-ttu-id="25ebd-208">tooget spustili, zkuste fiktivní test, který bude vždycky předávat.</span><span class="sxs-lookup"><span data-stu-id="25ebd-208">tooget started, try a dummy test that will always pass.</span></span>
   
       ```
       using System;
       using Microsoft.VisualStudio.TestTools.UnitTesting;
   
       namespace UnitTestProject1
       {
           [TestClass]
           public class UnitTest1
           {
               [TestMethod]
               [ExpectedException(typeof(NotImplementedException))]
               public void TestMethod1()
               {
                   throw new NotImplementedException();
               }
           }
       }
       ```
4. <span data-ttu-id="25ebd-209">Upravit definici sestavení hello, zvolte hello **proces** a rozbalte hello **Test** uzlu.</span><span class="sxs-lookup"><span data-stu-id="25ebd-209">Edit hello build definition, choose hello **Process** tab, and expand hello **Test** node.</span></span>
5. <span data-ttu-id="25ebd-210">Sada hello **sestavení selhání při testu selhání** tooTrue.</span><span class="sxs-lookup"><span data-stu-id="25ebd-210">Set hello **Fail build on test failure** tooTrue.</span></span> <span data-ttu-id="25ebd-211">To znamená, že nasazení hello nedojde, pokud hello testy průchodu.</span><span class="sxs-lookup"><span data-stu-id="25ebd-211">This means that hello deployment won't occur unless hello tests pass.</span></span>
   
   ![][41]
6. <span data-ttu-id="25ebd-212">Fronta nové sestavení.</span><span class="sxs-lookup"><span data-stu-id="25ebd-212">Queue a new build.</span></span>
   
   ![][42]
   
   ![][43]
7. <span data-ttu-id="25ebd-213">Při sestavení hello je budete pokračovat, zkontrolujte v jeho průběhu.</span><span class="sxs-lookup"><span data-stu-id="25ebd-213">While hello build is proceeding, check on its progress.</span></span>
   
    ![][44]
   
    ![][45]
8. <span data-ttu-id="25ebd-214">Pokud se provádí hello sestavení, zkontrolujte výsledky testů hello.</span><span class="sxs-lookup"><span data-stu-id="25ebd-214">When hello build is done, check hello test results.</span></span>
   
    ![][46]
   
    ![][47]
9. <span data-ttu-id="25ebd-215">Zkuste vytvořit test, který se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="25ebd-215">Try creating a test that will fail.</span></span> <span data-ttu-id="25ebd-216">Přidat nový test zkopírováním hello první z nich, přejmenujte ji a komentář hello řádek kódu, který s oznámením, že NotImplementedException – jde o očekávané výjimku.</span><span class="sxs-lookup"><span data-stu-id="25ebd-216">Add a new test by copying hello first one, rename it, and comment out hello line of code that states NotImplementedException is an expected exception.</span></span>
   
       ```
       [TestMethod]
       //[ExpectedException(typeof(NotImplementedException))]
       public void TestMethod2()
       {
           throw new NotImplementedException();
       }
       ```
10. <span data-ttu-id="25ebd-217">Vrácení se změnami hello změnit tooqueue nové sestavení.</span><span class="sxs-lookup"><span data-stu-id="25ebd-217">Check in hello change tooqueue a new build.</span></span>
    
     ![][48]
11. <span data-ttu-id="25ebd-218">Zobrazit hello testovací výsledky toosee podrobnosti o selhání hello.</span><span class="sxs-lookup"><span data-stu-id="25ebd-218">View hello test results toosee details about hello failure.</span></span>
    
     ![][49]
    
     ![][50]

## <a name="next-steps"></a><span data-ttu-id="25ebd-219">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25ebd-219">Next steps</span></span>
<span data-ttu-id="25ebd-220">Další informace o jednotce testování v sadě Visual Studio Team Services najdete v tématu [spouštění testování částí v buildu](http://go.microsoft.com/fwlink/p/?LinkId=510474).</span><span class="sxs-lookup"><span data-stu-id="25ebd-220">For more about unit testing in Visual Studio Team Services, see [Run unit tests in your build](http://go.microsoft.com/fwlink/p/?LinkId=510474).</span></span> <span data-ttu-id="25ebd-221">Pokud používáte Git, přečtěte si téma [sdílet kódu v úložišti Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) a [tooAzure průběžné nasazování služby App Service](../app-service-web/app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="25ebd-221">If you're using Git, see [Share your code in Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) and [Continuous deployment tooAzure App Service](../app-service-web/app-service-continuous-deployment.md).</span></span>  <span data-ttu-id="25ebd-222">Další informace o sadě Visual Studio Team Services najdete v tématu [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span><span class="sxs-lookup"><span data-stu-id="25ebd-222">For more information about Visual Studio Team Services, see [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span></span>

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
