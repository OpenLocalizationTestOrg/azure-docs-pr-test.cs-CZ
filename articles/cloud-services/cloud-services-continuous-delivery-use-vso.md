---
title: "Nastavené průběžné doručování s Visual Studio Team Services v Azure | Microsoft Docs"
description: "Naučte se konfigurovat týmových projektech Visual Studio Team Services automaticky vytvářet a nasazovat do funkci webové aplikace v Azure App Service nebo cloudové služby."
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
ms.openlocfilehash: d80ce63eb7ddfd7c45726be887a772f9a7594b28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a><span data-ttu-id="282cb-103">Průběžné doručování do Azure pomocí služby Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="282cb-103">Continuous delivery to Azure using Visual Studio Team Services</span></span>
<span data-ttu-id="282cb-104">Můžete nakonfigurovat týmových projektech Visual Studio Team Services k automatickému vytvoření a nasazení do webové aplikace Azure nebo cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="282cb-104">You can configure your Visual Studio Team Services team projects to automatically build and deploy to Azure web apps or cloud services.</span></span>  <span data-ttu-id="282cb-105">(Informace o tom, jak nastavit průběžné sestavení a nasadit pomocí systému *místní* Team Foundation Server najdete v části [nastavené průběžné doručování pro cloudové služby v Azure](cloud-services-dotnet-continuous-delivery.md).)</span><span class="sxs-lookup"><span data-stu-id="282cb-105">(For information on how to set up a continuous build and deploy system using an *on-premises* Team Foundation Server, see [Continuous Delivery for Cloud Services in Azure](cloud-services-dotnet-continuous-delivery.md).)</span></span>

<span data-ttu-id="282cb-106">Tento kurz předpokládá, že máte Visual Studio 2013 a Azure SDK nainstalována.</span><span class="sxs-lookup"><span data-stu-id="282cb-106">This tutorial assumes you have Visual Studio 2013 and the Azure SDK installed.</span></span> <span data-ttu-id="282cb-107">Pokud ještě nemáte Visual Studio 2013, stáhněte ho tak, že zvolíte **začněte zadarmo** odkaz na [www.visualstudio.com](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="282cb-107">If you don't already have Visual Studio 2013, download it by choosing the **Get started for free** link at [www.visualstudio.com](http://www.visualstudio.com).</span></span> <span data-ttu-id="282cb-108">Nainstalovat sadu Azure SDK z [zde](http://go.microsoft.com/fwlink/?LinkId=239540).</span><span class="sxs-lookup"><span data-stu-id="282cb-108">Install the Azure SDK from [here](http://go.microsoft.com/fwlink/?LinkId=239540).</span></span>

> [!NOTE]
> <span data-ttu-id="282cb-109">Účet Visual Studio Team Services k dokončení tohoto kurzu potřebujete: můžete [zdarma otevřít účet Visual Studio Team Services](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span><span class="sxs-lookup"><span data-stu-id="282cb-109">You need an Visual Studio Team Services account to complete this tutorial: You can [open a Visual Studio Team Services account for free](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span></span>
> 
> 

<span data-ttu-id="282cb-110">Pokud chcete nastavit Cloudová služba pro automatické vytvoření a nasazení do Azure pomocí sady Visual Studio Team Services, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="282cb-110">To set up a cloud service to automatically build and deploy to Azure by using Visual Studio Team Services, follow these steps.</span></span>

## <a name="1-create-a-team-project"></a><span data-ttu-id="282cb-111">1: Vytvoření týmového projektu</span><span class="sxs-lookup"><span data-stu-id="282cb-111">1: Create a team project</span></span>
<span data-ttu-id="282cb-112">Postupujte podle pokynů [sem](http://go.microsoft.com/fwlink/?LinkId=512980) k vytvoření týmového projektu a tu propojit na Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="282cb-112">Follow the instructions [here](http://go.microsoft.com/fwlink/?LinkId=512980) to create your team project and link it to Visual Studio.</span></span> <span data-ttu-id="282cb-113">Tento návod předpokládá, že používáte jako řešení pro řízení zdrojového serveru Team Foundation verze ovládacího prvku (TFVC).</span><span class="sxs-lookup"><span data-stu-id="282cb-113">This walkthrough assumes you are using Team Foundation Version Control (TFVC) as your source control solution.</span></span> <span data-ttu-id="282cb-114">Pokud chcete použít pro správu verzí Git, přečtěte si téma [Git verze tohoto návodu](http://go.microsoft.com/fwlink/p/?LinkId=397358).</span><span class="sxs-lookup"><span data-stu-id="282cb-114">If you want to use Git for version control, see [the Git version of this walkthrough](http://go.microsoft.com/fwlink/p/?LinkId=397358).</span></span>

## <a name="2-check-in-a-project-to-source-control"></a><span data-ttu-id="282cb-115">2: projektu do správy zdrojového kódu se změnami</span><span class="sxs-lookup"><span data-stu-id="282cb-115">2: Check in a project to source control</span></span>
1. <span data-ttu-id="282cb-116">V sadě Visual Studio otevřete řešení, které chcete nasadit, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="282cb-116">In Visual Studio, open the solution you want to deploy, or create a new one.</span></span>
   <span data-ttu-id="282cb-117">Podle kroků v tomto názorném postupu můžete nasadit webovou aplikaci nebo cloudové služby (aplikace Azure).</span><span class="sxs-lookup"><span data-stu-id="282cb-117">You can deploy a web app or a cloud service (Azure Application) by following the steps in this walkthrough.</span></span>
   <span data-ttu-id="282cb-118">Pokud chcete vytvořit nové řešení, vytvořte nový projekt Azure Cloud Service, nebo nový projekt ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="282cb-118">If you want to create a new solution, create a new Azure Cloud Service project, or a new ASP.NET MVC project.</span></span> <span data-ttu-id="282cb-119">Ujistěte se, že cílem projektu rozhraní .NET Framework 4 nebo 4.5 a pokud vytvoříte projekt cloudové služby, přidat webová role ASP.NET MVC a roli pracovního procesu a zvolte Internetové aplikace pro webovou roli.</span><span class="sxs-lookup"><span data-stu-id="282cb-119">Make sure that the project targets .NET Framework 4 or 4.5, and if you are creating a cloud service project, add an ASP.NET MVC web role and a worker role, and choose Internet application for the web role.</span></span> <span data-ttu-id="282cb-120">Po zobrazení výzvy vyberte **Internetové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="282cb-120">When prompted, choose **Internet Application**.</span></span>
   <span data-ttu-id="282cb-121">Pokud chcete vytvořit webovou aplikaci, zvolte šablonu projektu webové aplikace ASP.NET a potom zvolte MVC.</span><span class="sxs-lookup"><span data-stu-id="282cb-121">If you want to create a web app, choose the ASP.NET Web Application project template, and then choose MVC.</span></span> <span data-ttu-id="282cb-122">V tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="282cb-122">See [Create an ASP.NET web app in Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="282cb-123">Visual Studio Team Services nyní podporují pouze CI nasazení webových aplikací, Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="282cb-123">Visual Studio Team Services only support CI deployments of Visual Studio Web Applications at this time.</span></span> <span data-ttu-id="282cb-124">Webové projekty jsou mimo rozsah.</span><span class="sxs-lookup"><span data-stu-id="282cb-124">Web Site projects are out of scope.</span></span>
   > 
   > 
2. <span data-ttu-id="282cb-125">Otevřete kontextu nabídku pro řešení a zvolte **přidat řešení správy zdrojového kódu**.</span><span class="sxs-lookup"><span data-stu-id="282cb-125">Open the context menu for the solution, and choose **Add Solution to Source Control**.</span></span>
   
    ![][5]
3. <span data-ttu-id="282cb-126">Potvrďte nebo změňte výchozí hodnoty a zvolte **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="282cb-126">Accept or change the defaults and choose the **OK** button.</span></span> <span data-ttu-id="282cb-127">Po dokončení procesu zdroj ovládacího prvku ikony zobrazovat v **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="282cb-127">Once the process completes, source control icons appear in **Solution Explorer**.</span></span>
   
    ![][6]
4. <span data-ttu-id="282cb-128">Otevřete místní nabídku pro řešení a zvolte **změnami**.</span><span class="sxs-lookup"><span data-stu-id="282cb-128">Open the shortcut menu for the solution, and choose **Check In**.</span></span>
   
    ![][7]
5. <span data-ttu-id="282cb-129">V **čekající změny** oblasti **Team Explorer**zadejte poznámku k vrácení se změnami a zvolte **změnami** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="282cb-129">In the **Pending Changes** area of **Team Explorer**, type a comment for the check-in and choose the **Check In** button.</span></span>
   
    ![][8]
   
    <span data-ttu-id="282cb-130">Všimněte si možnosti, které chcete zahrnout nebo vyloučit konkrétní změny při se změnami.</span><span class="sxs-lookup"><span data-stu-id="282cb-130">Note the options to include or exclude specific changes when you check in.</span></span> <span data-ttu-id="282cb-131">Pokud chcete, jsou vyloučeny změny, zvolte **zahrnují všechny** odkaz.</span><span class="sxs-lookup"><span data-stu-id="282cb-131">If desired changes are excluded, choose the **Include All** link.</span></span>
   
    ![][9]

## <a name="3-connect-the-project-to-azure"></a><span data-ttu-id="282cb-132">3: připojení projekt do Azure</span><span class="sxs-lookup"><span data-stu-id="282cb-132">3: Connect the project to Azure</span></span>
1. <span data-ttu-id="282cb-133">Nyní když máte Visual Studio Team Services týmového projektu s některé zdrojového kódu v ní, jste připraveni se připojit k Azure týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="282cb-133">Now that you have a VS Team Services team project with some source code in it, you are ready to connect your team project to Azure.</span></span>  <span data-ttu-id="282cb-134">V [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), vyberte svou cloudové služby nebo webovou aplikaci nebo vytvořte novou výběrem  **+**  ikona ve spodní levé a výběr **Cloudová služba**nebo **webová aplikace** a potom **rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="282cb-134">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), select your cloud service or web app, or create a new one by choosing the **+** icon at the bottom left and choosing **Cloud Service** or **Web App** and then **Quick Create**.</span></span> <span data-ttu-id="282cb-135">Vyberte **nastavit publikování pomocí Visual Studio Team Services** odkaz.</span><span class="sxs-lookup"><span data-stu-id="282cb-135">Choose the **Set up publishing with Visual Studio Team Services** link.</span></span>
   
    ![][10]
2. <span data-ttu-id="282cb-136">V průvodci zadejte název účtu Visual Studio Team Services textové pole a klikněte na **autorizovat teď** odkaz.</span><span class="sxs-lookup"><span data-stu-id="282cb-136">In the wizard, type the name of your Visual Studio Team Services account in the textbox and click the **Authorize Now** link.</span></span> <span data-ttu-id="282cb-137">Můžete být požádáni o přihlášení.</span><span class="sxs-lookup"><span data-stu-id="282cb-137">You might be asked to sign in.</span></span>
   
    ![][11]
3. <span data-ttu-id="282cb-138">V **požadavku na připojení** automaticky otevíraný dialog, vyberte **přijmout** tlačítko Autorizovat Azure nakonfigurovat týmového projektu v Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="282cb-138">In the **Connection Request** pop-up dialog, choose the **Accept** button to authorize Azure to configure your team project in VS Team Services.</span></span>
   
    ![][12]
4. <span data-ttu-id="282cb-139">Při ověřování úspěšné, uvidíte rozevírací seznam obsahující seznam týmových projektech Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="282cb-139">When authorization succeeds, you see a dropdown containing a list of your Visual Studio Team Services team projects.</span></span> <span data-ttu-id="282cb-140">Vyberte název týmového projektu, který jste vytvořili v předchozím kroku a pak zvolte tlačítko zaškrtnutí v průvodci.</span><span class="sxs-lookup"><span data-stu-id="282cb-140">Choose  the name of team project that you created in the previous steps, and then choose the wizard's checkmark button.</span></span>
   
    ![][13]
5. <span data-ttu-id="282cb-141">Po projektu je propojená, zobrazí se některé pokyny k vrácení se změnami změny k vašemu týmovému projektu Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="282cb-141">After your project is linked, you will see some instructions for checking in changes to your Visual Studio Team Services team project.</span></span>  <span data-ttu-id="282cb-142">Na vaše další vrácení se změnami Visual Studio Team Services bude sestavovat a nasazení projektu do Azure.</span><span class="sxs-lookup"><span data-stu-id="282cb-142">On your next check-in, Visual Studio Team Services will build and deploy your project to Azure.</span></span>  <span data-ttu-id="282cb-143">Zkuste tento teď pomocí klepnutím na tlačítko **změnami ze sady Visual Studio** odkaz a potom **spusťte Visual Studio** odkaz (nebo ekvivalentní **Visual Studio** tlačítko v dolní části portál obrazovky).</span><span class="sxs-lookup"><span data-stu-id="282cb-143">Try this now by clicking the **Check In from Visual Studio** link, and then the **Launch Visual Studio** link (or the equivalent **Visual Studio** button at the bottom of the portal screen).</span></span>
   
    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a><span data-ttu-id="282cb-144">4: aktivovat opětovném sestavení a znovu nasaďte projekt</span><span class="sxs-lookup"><span data-stu-id="282cb-144">4: Trigger a rebuild and redeploy your project</span></span>
1. <span data-ttu-id="282cb-145">V sadě Visual Studio **Team Explorer**, vyberte **Průzkumník správy zdrojového kódu** odkaz.</span><span class="sxs-lookup"><span data-stu-id="282cb-145">In Visual Studio's **Team Explorer**, choose the **Source Control Explorer** link.</span></span>
   
    ![][15]
2. <span data-ttu-id="282cb-146">Přejděte k souboru řešení a otevřete jej.</span><span class="sxs-lookup"><span data-stu-id="282cb-146">Navigate to your solution file and open it.</span></span>
   
    ![][16]
3. <span data-ttu-id="282cb-147">V **Průzkumníku**, otevřete soubor a ho změnit.</span><span class="sxs-lookup"><span data-stu-id="282cb-147">In **Solution Explorer**, open up a file and change it.</span></span> <span data-ttu-id="282cb-148">Můžete například změnit soubor `_Layout.cshtml` v zobrazení\\sdílené složky MVC webové role.</span><span class="sxs-lookup"><span data-stu-id="282cb-148">For example, change the file `_Layout.cshtml` under the Views\\Shared folder in an MVC web role.</span></span>
   
    ![][17]
4. <span data-ttu-id="282cb-149">Upravit logo pro lokalitu a zvolte **Ctrl + S** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="282cb-149">Edit the logo for the site and choose **Ctrl+S** to save the file.</span></span>
   
    ![][18]
5. <span data-ttu-id="282cb-150">V **Team Explorer**, vyberte **čekající změny** odkaz.</span><span class="sxs-lookup"><span data-stu-id="282cb-150">In **Team Explorer**, choose the **Pending Changes** link.</span></span>
   
    ![][19]
6. <span data-ttu-id="282cb-151">Zadejte komentář a potom zvolte **změnami** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="282cb-151">Enter a comment and then choose the **Check In** button.</span></span>
   
    ![][20]
7. <span data-ttu-id="282cb-152">Zvolte **domácí** tlačítko se vrátíte do **Team Explorer** domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="282cb-152">Choose the **Home** button to return to the **Team Explorer** home page.</span></span>
   
    ![][21]
8. <span data-ttu-id="282cb-153">Vyberte **sestavení** odkaz zobrazíte nastavení v průběhu.</span><span class="sxs-lookup"><span data-stu-id="282cb-153">Choose the **Builds** link to view the builds in progress.</span></span>
   
    ![][22]
   
    <span data-ttu-id="282cb-154">**Team Explorer** ukazuje, že byla spuštěna sestavení pro vaše vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="282cb-154">**Team Explorer** shows that a build has been triggered for your check-in.</span></span>
   
    ![][23]
9. <span data-ttu-id="282cb-155">Dvakrát klikněte na název sestavení v průběhu zobrazíte podrobné protokolu v průběhu sestavení.</span><span class="sxs-lookup"><span data-stu-id="282cb-155">Double-click the name of the build in progress to view a detailed log as the build progresses.</span></span>
   
    ![][24]
10. <span data-ttu-id="282cb-156">Při sestavení je v průběhu, podívejte se na definici sestavení, která byla vytvořena, když jste propojili TFS do Azure pomocí průvodce.</span><span class="sxs-lookup"><span data-stu-id="282cb-156">While the build is in-progress, take a look at the build definition that was created when you linked TFS to Azure by using the wizard.</span></span>  <span data-ttu-id="282cb-157">Otevřete místní nabídku pro definici sestavení a zvolte **upravit definici sestavení**.</span><span class="sxs-lookup"><span data-stu-id="282cb-157">Open the shortcut menu for the build definition and choose **Edit Build Definition**.</span></span>
    
     ![][25]
    
     <span data-ttu-id="282cb-158">V **aktivační událost** karty, zobrazí se, že je ve výchozím nastavení na každé vrácení se změnami definici sestavení.</span><span class="sxs-lookup"><span data-stu-id="282cb-158">In the **Trigger** tab, you will see that the build definition is set to build on every check-in by default.</span></span>
    
     ![][26]
    
     <span data-ttu-id="282cb-159">V **proces** kartě uvidíte prostředí nasazení je nastavena na název cloudové služby nebo webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="282cb-159">In the **Process** tab, you can see the deployment environment is set to the name of your cloud service or web app.</span></span> <span data-ttu-id="282cb-160">Pokud pracujete s webovými aplikacemi, bude vlastnosti, které vidíte liší od těch, které tady uvedené.</span><span class="sxs-lookup"><span data-stu-id="282cb-160">If you are working with web apps, the properties you see will be different from those shown here.</span></span>
    
     ![][27]
11. <span data-ttu-id="282cb-161">Zadejte hodnoty vlastností, pokud chcete jiné hodnoty než výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="282cb-161">Specify values for the properties if you want different values than the defaults.</span></span> <span data-ttu-id="282cb-162">Vlastnosti pro publikování v Azure jsou ve **nasazení** části.</span><span class="sxs-lookup"><span data-stu-id="282cb-162">The properties for Azure publishing are in the **Deployment** section.</span></span>
    
     <span data-ttu-id="282cb-163">V následující tabulce jsou uvedeny dostupné vlastnosti v **nasazení** části:</span><span class="sxs-lookup"><span data-stu-id="282cb-163">The following table shows the available properties in the **Deployment** section:</span></span>
    
    | <span data-ttu-id="282cb-164">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="282cb-164">Property</span></span> | <span data-ttu-id="282cb-165">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="282cb-165">Default Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="282cb-166">Povolit nedůvěryhodné certifikáty</span><span class="sxs-lookup"><span data-stu-id="282cb-166">Allow Untrusted Certificates</span></span> |<span data-ttu-id="282cb-167">Pokud je hodnota false, musí být podepsané certifikáty SSL kořenovou autoritou.</span><span class="sxs-lookup"><span data-stu-id="282cb-167">If false, SSL certificates must be signed by a root authority.</span></span> |
    | <span data-ttu-id="282cb-168">Povolit upgradu</span><span class="sxs-lookup"><span data-stu-id="282cb-168">Allow Upgrade</span></span> |<span data-ttu-id="282cb-169">Umožňuje nasazení k aktualizaci stávajícího nasazení místo vytvoření nové.</span><span class="sxs-lookup"><span data-stu-id="282cb-169">Allows the deployment to update an existing deployment instead of creating a new one.</span></span> <span data-ttu-id="282cb-170">Zachovává IP adresu.</span><span class="sxs-lookup"><span data-stu-id="282cb-170">Preserves the IP address.</span></span> |
    | <span data-ttu-id="282cb-171">Neodstraňujte</span><span class="sxs-lookup"><span data-stu-id="282cb-171">Do Not Delete</span></span> |<span data-ttu-id="282cb-172">V případě hodnoty true Nepřepisovat existující nesouvisejícími nasazení (upgrade je povolena.).</span><span class="sxs-lookup"><span data-stu-id="282cb-172">If true, do not overwrite an existing unrelated deployment (upgrade is allowed).</span></span> |
    | <span data-ttu-id="282cb-173">Cesta k nastavení nasazení</span><span class="sxs-lookup"><span data-stu-id="282cb-173">Path to Deployment Settings</span></span> |<span data-ttu-id="282cb-174">Cesta k souboru .pubxml pro webovou aplikaci, relativně ke kořenové složce úložiště.</span><span class="sxs-lookup"><span data-stu-id="282cb-174">The path to your .pubxml file for a web app, relative to the root folder of the repo.</span></span> <span data-ttu-id="282cb-175">Ignorovat pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="282cb-175">Ignored for cloud services.</span></span> |
    | <span data-ttu-id="282cb-176">Prostředí nasazení služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="282cb-176">Sharepoint Deployment Environment</span></span> |<span data-ttu-id="282cb-177">Stejný jako název služby.</span><span class="sxs-lookup"><span data-stu-id="282cb-177">The same as the service name.</span></span> |
    | <span data-ttu-id="282cb-178">Prostředí nasazení Azure</span><span class="sxs-lookup"><span data-stu-id="282cb-178">Azure Deployment Environment</span></span> |<span data-ttu-id="282cb-179">Webové aplikace nebo cloudové název služby.</span><span class="sxs-lookup"><span data-stu-id="282cb-179">The web app or cloud service name.</span></span> |
12. <span data-ttu-id="282cb-180">Pokud používáte více konfigurace služby (soubory .cscfg), abyste mohli zadat konfiguraci požadované služby v **sestavení, Upřesnit, argumenty MSBuild** nastavení.</span><span class="sxs-lookup"><span data-stu-id="282cb-180">If you are using multiple service configurations (.cscfg files), you can specify the desired service configuration in the **Build, Advanced, MSBuild arguments** setting.</span></span> <span data-ttu-id="282cb-181">Například pokud chcete používat ServiceConfiguration.Test.cscfg, nastavit argumenty MSBuild řádku `/p:TargetProfile=Test`.</span><span class="sxs-lookup"><span data-stu-id="282cb-181">For example, to use ServiceConfiguration.Test.cscfg, set MSBuild arguments line option `/p:TargetProfile=Test`.</span></span>
    
     ![][38]
    
     <span data-ttu-id="282cb-182">Buildu té doby by se měly dokončit úspěšně.</span><span class="sxs-lookup"><span data-stu-id="282cb-182">By this time, your build should be completed successfully.</span></span>
    
     ![][28]
13. <span data-ttu-id="282cb-183">Pokud dvakrát kliknete na název sestavení, Visual Studio se zobrazí **souhrnu sestavení**, včetně všech výsledků testu z přidružené projektů testování částí.</span><span class="sxs-lookup"><span data-stu-id="282cb-183">If you double-click the build name, Visual Studio shows a **Build Summary**, including any test results from associated unit test projects.</span></span>
    
     ![][29]
14. <span data-ttu-id="282cb-184">V [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), přidružené nasazení můžete zobrazit na **nasazení** kartě, pokud je vybrána pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="282cb-184">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), you can view the associated deployment on the **Deployments** tab when the staging environment is selected.</span></span>
    
     ![][30]
15. <span data-ttu-id="282cb-185">Přejděte na adresu URL vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="282cb-185">Browse to your site's URL.</span></span> <span data-ttu-id="282cb-186">Pro webovou aplikaci, klepněte **Procházet** tlačítka na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="282cb-186">For a web app, just click the **Browse** button on the command bar.</span></span> <span data-ttu-id="282cb-187">Cloudové služby, vyberte adresu URL v **rychlý přehled** části **řídicí panel** stránky, která obsahuje pracovní prostředí pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="282cb-187">For a cloud service, choose the URL in the **Quick Glance** section of the **Dashboard** page that shows the Staging environment for a cloud service.</span></span> <span data-ttu-id="282cb-188">Nasazení z průběžnou integraci pro cloudové služby jsou publikovány do prostředí pracovní ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="282cb-188">Deployments from continuous integration for cloud services are published to the Staging environment by default.</span></span> <span data-ttu-id="282cb-189">Toto můžete změnit nastavením **alternativní prostředí cloudové služby** vlastnost **produkční**.</span><span class="sxs-lookup"><span data-stu-id="282cb-189">You can change this by setting the **Alternate Cloud Service Environment** property to **Production**.</span></span> <span data-ttu-id="282cb-190">Tento snímek obrazovky ukazuje, kde je adresa URL webu na stránce řídicího panelu cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="282cb-190">This screenshot shows where the site URL is on the cloud service's dashboard page.</span></span>
    
    ![][31]
    
    <span data-ttu-id="282cb-191">Na nich spuštěné webu se otevře novou kartu prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="282cb-191">A new browser tab will open to reveal your running site.</span></span>
    
    ![][32]
    
    <span data-ttu-id="282cb-192">Pro cloudové služby, pokud udělat jiné změny do projektu, aktivační události je více sestavení a budou se hromadit více nasazení.</span><span class="sxs-lookup"><span data-stu-id="282cb-192">For cloud services, if you make other changes to your project, you trigger more builds, and you will accumulate multiple deployments.</span></span> <span data-ttu-id="282cb-193">Nejnovější označen jako aktivní.</span><span class="sxs-lookup"><span data-stu-id="282cb-193">The latest one marked as Active.</span></span>
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a><span data-ttu-id="282cb-194">5: znovu nasaďte starší sestavení</span><span class="sxs-lookup"><span data-stu-id="282cb-194">5: Redeploy an earlier build</span></span>
<span data-ttu-id="282cb-195">Tento krok platí pro cloudové služby a je volitelný.</span><span class="sxs-lookup"><span data-stu-id="282cb-195">This step applies to cloud services and is optional.</span></span> <span data-ttu-id="282cb-196">Na portálu Azure classic, zvolte k předchozímu nasazení a pak **znovu nasaďte** tlačítko rewind váš web, dřívější vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="282cb-196">In the Azure classic portal, choose an earlier deployment and then choose the **Redeploy** button to rewind your site to an earlier check-in.</span></span>  <span data-ttu-id="282cb-197">Všimněte si, že to aktivují nového sestavení v sadě TFS a vytvořit nový záznam v historii nasazení.</span><span class="sxs-lookup"><span data-stu-id="282cb-197">Note that this will trigger a new build in TFS and create a new entry in your deployment history.</span></span>

![][34]

## <a name="6-change-the-production-deployment"></a><span data-ttu-id="282cb-198">6: Změňte produkční nasazení</span><span class="sxs-lookup"><span data-stu-id="282cb-198">6: Change the Production deployment</span></span>
<span data-ttu-id="282cb-199">Tento krok platí jenom pro cloudové služby, není webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="282cb-199">This step applies only to cloud services, not web apps.</span></span> <span data-ttu-id="282cb-200">Jakmile budete připraveni, můžete zvýšit úroveň pracovního prostředí do produkčního prostředí, tak, že zvolíte **Prohodit** tlačítka na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="282cb-200">When you are ready, you can promote the Staging environment to the production environment by choosing the **Swap** button in the Azure classic portal.</span></span> <span data-ttu-id="282cb-201">Nově nasazené pracovní prostředí se zvýší úroveň na produkční a předchozí provozním prostředí, pokud existuje, bude pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="282cb-201">The newly deployed Staging environment is promoted to Production, and the previous Production environment, if any, becomes a Staging environment.</span></span> <span data-ttu-id="282cb-202">Aktivní nasazení může být různé pro produkční a pracovní prostředí, ale historii nasazení poslední sestavení je stejný bez ohledu na prostředí.</span><span class="sxs-lookup"><span data-stu-id="282cb-202">The Active deployment may be different for the Production and Staging environments, but the deployment history of recent builds is the same regardless of environment.</span></span>

![][35]

## <a name="7-run-unit-tests"></a><span data-ttu-id="282cb-203">7: spouštění testů jednotek</span><span class="sxs-lookup"><span data-stu-id="282cb-203">7: Run unit tests</span></span>
<span data-ttu-id="282cb-204">Tento krok platí pouze pro webové aplikace, není cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="282cb-204">This step applies only to web apps, not cloud services.</span></span> <span data-ttu-id="282cb-205">Brány kvality uvést na vaše nasazení, můžete spustit testy jednotek a jejich případné selhání, můžete zastavit nasazení.</span><span class="sxs-lookup"><span data-stu-id="282cb-205">To put a quality gate on your deployment, you can run unit tests and if they fail, you can stop the deployment.</span></span>

1. <span data-ttu-id="282cb-206">V sadě Visual Studio přidejte projektu testování částí.</span><span class="sxs-lookup"><span data-stu-id="282cb-206">In Visual Studio, add a unit test project.</span></span>
   
   ![][39]
2. <span data-ttu-id="282cb-207">Přidejte odkazy na projekt na projekt, který chcete testovat.</span><span class="sxs-lookup"><span data-stu-id="282cb-207">Add project references to the project you want to test.</span></span>
   
   ![][40]
3. <span data-ttu-id="282cb-208">Přidejte některé testy jednotek.</span><span class="sxs-lookup"><span data-stu-id="282cb-208">Add some unit tests.</span></span> <span data-ttu-id="282cb-209">Chcete-li začít, zkuste fiktivní test, který bude vždycky předávat.</span><span class="sxs-lookup"><span data-stu-id="282cb-209">To get started, try a dummy test that will always pass.</span></span>
   
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
4. <span data-ttu-id="282cb-210">Upravit definici sestavení, vyberte **proces** a rozbalte **Test** uzlu.</span><span class="sxs-lookup"><span data-stu-id="282cb-210">Edit the build definition, choose the **Process** tab, and expand the **Test** node.</span></span>
5. <span data-ttu-id="282cb-211">Nastavte **sestavení selhání při testu selhání** na hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="282cb-211">Set the **Fail build on test failure** to True.</span></span> <span data-ttu-id="282cb-212">To znamená, že nedojde k nasazení, pokud jsou testy úspěšné.</span><span class="sxs-lookup"><span data-stu-id="282cb-212">This means that the deployment won't occur unless the tests pass.</span></span>
   
   ![][41]
6. <span data-ttu-id="282cb-213">Fronta nové sestavení.</span><span class="sxs-lookup"><span data-stu-id="282cb-213">Queue a new build.</span></span>
   
   ![][42]
   
   ![][43]
7. <span data-ttu-id="282cb-214">Při sestavení je budete pokračovat, zkontrolujte v jeho průběhu.</span><span class="sxs-lookup"><span data-stu-id="282cb-214">While the build is proceeding, check on its progress.</span></span>
   
    ![][44]
   
    ![][45]
8. <span data-ttu-id="282cb-215">Pokud se provádí sestavení, zkontrolujte výsledky.</span><span class="sxs-lookup"><span data-stu-id="282cb-215">When the build is done, check the test results.</span></span>
   
    ![][46]
   
    ![][47]
9. <span data-ttu-id="282cb-216">Zkuste vytvořit test, který se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="282cb-216">Try creating a test that will fail.</span></span> <span data-ttu-id="282cb-217">Přidat nový test zkopírováním první z nich, přejmenujte ji a Zakomentovat řádek kódu, který s oznámením, že NotImplementedException – jde o očekávané výjimku.</span><span class="sxs-lookup"><span data-stu-id="282cb-217">Add a new test by copying the first one, rename it, and comment out the line of code that states NotImplementedException is an expected exception.</span></span>
   
       ```
       [TestMethod]
       //[ExpectedException(typeof(NotImplementedException))]
       public void TestMethod2()
       {
           throw new NotImplementedException();
       }
       ```
10. <span data-ttu-id="282cb-218">Zkontrolujte v změn do fronty nové sestavení.</span><span class="sxs-lookup"><span data-stu-id="282cb-218">Check in the change to queue a new build.</span></span>
    
     ![][48]
11. <span data-ttu-id="282cb-219">Zobrazení výsledků testu zobrazíte podrobnosti o selhání.</span><span class="sxs-lookup"><span data-stu-id="282cb-219">View the test results to see details about the failure.</span></span>
    
     ![][49]
    
     ![][50]

## <a name="next-steps"></a><span data-ttu-id="282cb-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="282cb-220">Next steps</span></span>
<span data-ttu-id="282cb-221">Další informace o jednotce testování v sadě Visual Studio Team Services najdete v tématu [spouštění testování částí v buildu](http://go.microsoft.com/fwlink/p/?LinkId=510474).</span><span class="sxs-lookup"><span data-stu-id="282cb-221">For more about unit testing in Visual Studio Team Services, see [Run unit tests in your build](http://go.microsoft.com/fwlink/p/?LinkId=510474).</span></span> <span data-ttu-id="282cb-222">Pokud používáte Git, přečtěte si téma [sdílet kódu v úložišti Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) a [průběžné nasazování do služby Azure App Service](../app-service-web/app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="282cb-222">If you're using Git, see [Share your code in Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) and [Continuous deployment to Azure App Service](../app-service-web/app-service-continuous-deployment.md).</span></span>  <span data-ttu-id="282cb-223">Další informace o sadě Visual Studio Team Services najdete v tématu [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span><span class="sxs-lookup"><span data-stu-id="282cb-223">For more information about Visual Studio Team Services, see [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span></span>

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
