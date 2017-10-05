---
title: "Kurz:  DevOps s portálem Azure Portal | Dokumentace Microsoftu"
description: "Poznejte různé pracovní postupy pro DevOps v portálu Azure."
services: azure-portal
documentationcenter: 
author: mlearned
manager: douge
editor: mlearned
ms.assetid: 4f1c5bc1-c732-4d35-b5df-0fd68e547d38
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2016
ms.author: mlearned
ms.openlocfilehash: eec7d1402bdea4e5433c473dd713eed23aa80464
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-devops-with-the-azure-portal"></a><span data-ttu-id="604e9-103">Kurz: DevOps s portálem Azure</span><span class="sxs-lookup"><span data-stu-id="604e9-103">Tutorial: DevOps with the Azure Portal</span></span>
<span data-ttu-id="604e9-104">Platforma Azure je plná flexibilních pracovních postupů DevOps.</span><span class="sxs-lookup"><span data-stu-id="604e9-104">The Azure platform is full of flexible DevOps workflows.</span></span> <span data-ttu-id="604e9-105">V tomto kurzu poznáte, jak můžete využít možnosti portálu Azure pro vývoj, testování, nasazení, ladění, sledování a správu běžících aplikací.</span><span class="sxs-lookup"><span data-stu-id="604e9-105">In this tutorial, you learn how to leverage the capabilities of the Azure Portal to develop, test, deploy, troubleshoot, monitor, and manage running applications.</span></span> <span data-ttu-id="604e9-106">Tento kurz se zaměřuje na:</span><span class="sxs-lookup"><span data-stu-id="604e9-106">This tutorial focuses on the following:</span></span>

1. <span data-ttu-id="604e9-107">Vytvoření webové aplikace a povolení průběžného nasazování</span><span class="sxs-lookup"><span data-stu-id="604e9-107">Creating a web app and enabling continuous deployment</span></span>
2. <span data-ttu-id="604e9-108">Vývoj a testování aplikace</span><span class="sxs-lookup"><span data-stu-id="604e9-108">Develop and test an app</span></span>
3. <span data-ttu-id="604e9-109">Sledování a řešení potíží s aplikací</span><span class="sxs-lookup"><span data-stu-id="604e9-109">Monitoring and Troubleshooting an app</span></span>
4. <span data-ttu-id="604e9-110">Úlohy správy obecné aplikace</span><span class="sxs-lookup"><span data-stu-id="604e9-110">General application management tasks</span></span>

## <a name="creating-a-web-app-and-enabling-continuous-deployment"></a><span data-ttu-id="604e9-111">Vytvoření webové aplikace a povolení průběžného nasazování</span><span class="sxs-lookup"><span data-stu-id="604e9-111">Creating a web app and enabling continuous deployment</span></span>
<span data-ttu-id="604e9-112">Vytvořte webovou aplikaci pomocí služby [Azure App Service](https://azure.microsoft.com/services/app-service/), budete ji používat v dalších částech tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="604e9-112">Create a Web app with [Azure App Service](https://azure.microsoft.com/services/app-service/), which you’ll use in the rest of this tutorial.</span></span> <span data-ttu-id="604e9-113">Na začátku povolíte průběžné nasazování z úložiště zdrojového kódu do našeho spuštěného prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="604e9-113">You’ll initially enable continuous deployment from your source code repository into our running Azure environment.</span></span>

1. <span data-ttu-id="604e9-114">Přihlaste se k portálu Azure</span><span class="sxs-lookup"><span data-stu-id="604e9-114">Sign into the Azure Portal</span></span>
2. <span data-ttu-id="604e9-115">Vyberte **App Services** &gt; **Přidat ikonu** a zadejte název, vyberte předplatné a vytvořte novou skupinu prostředků, která bude sloužit jako kontejner pro službu.</span><span class="sxs-lookup"><span data-stu-id="604e9-115">Choose **App Services** &gt; **Add icon** and enter a name, choose your subscription, and create a new resource group to serve as the container for the service.</span></span>
   
   <span data-ttu-id="604e9-116">Skupiny prostředků umožňují spravovat různé aspekty řešení, jako třeba fakturaci, nasazení a sledování všeho jako jedné skupiny pomocí [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="604e9-116">Resource groups allow you to manage various aspects of the solution such as billing, deployments and monitoring all as a single group via [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span></span>
   
   ![image1][image1]
3. <span data-ttu-id="604e9-118">Za malou chvíli se vytvoří vaše aplikační služba.</span><span class="sxs-lookup"><span data-stu-id="604e9-118">After a few moments, your app service is created.</span></span> <span data-ttu-id="604e9-119">Projděte si různé nabídky a možnosti, které jsou v portálu pro službu dostupné. Dejte tomu několik minut.</span><span class="sxs-lookup"><span data-stu-id="604e9-119">Take a few minutes to explore the various menu options for the service in the portal.</span></span>
   
   ![image2][image2]    
4. <span data-ttu-id="604e9-121">Klikněte na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="604e9-121">Click the URL.</span></span> <span data-ttu-id="604e9-122">Všimněte si, že jsou pro nástroje a úložiště dostupné různé možnosti.</span><span class="sxs-lookup"><span data-stu-id="604e9-122">Notice the variety of available choices for tools and repositories.</span></span> <span data-ttu-id="604e9-123">Můžete taky použít jazyky a rozhraní, které chcete, včetně .NET, Java a Ruby.</span><span class="sxs-lookup"><span data-stu-id="604e9-123">You can also use the languages and frameworks of your choice including .NET, Java, and Ruby.</span></span>
   
   ![image3][image3]    
5. <span data-ttu-id="604e9-125">S portálem Azure je nasazování jednoduchý proces s několika málo kroky.</span><span class="sxs-lookup"><span data-stu-id="604e9-125">The Azure portal makes continuous deployment an easy process that involves only a few simple steps.</span></span> <span data-ttu-id="604e9-126">Na portálu Azure z ikony aplikace služby, kterou jste právě vytvořili, vyberte nastavení.</span><span class="sxs-lookup"><span data-stu-id="604e9-126">In the Azure portal, choose settings from the icon for the app service you just created.</span></span>
   
   ![image4][image4]
   
   <span data-ttu-id="604e9-128">Z okna, které se otevře na pravé straně, přejděte do části publikování.</span><span class="sxs-lookup"><span data-stu-id="604e9-128">From the blade that opens on the right, scroll to the publishing section.</span></span>
   
   ![image5][image5]
6. <span data-ttu-id="604e9-130">Teď změníte některá nastavení a povolíte pro aplikaci průběžné nasazení.</span><span class="sxs-lookup"><span data-stu-id="604e9-130">Next, configure some settings to enable continuous deployment for the app.</span></span> <span data-ttu-id="604e9-131">Klikněte na Zdroj nasazení a potom klikněte na tlačítko Vybrat zdroj.</span><span class="sxs-lookup"><span data-stu-id="604e9-131">Click Deployment Source and then click Choose Source.</span></span> <span data-ttu-id="604e9-132">Všimněte si, že si můžete vybrat z různých úložišť zdroje.</span><span class="sxs-lookup"><span data-stu-id="604e9-132">Notice the variety of options you have for repository sources.</span></span>
   
   ![image6][image6]
7. <span data-ttu-id="604e9-134">V tomto příkladu vyberte GitHub.</span><span class="sxs-lookup"><span data-stu-id="604e9-134">For this example choose GitHub.</span></span> <span data-ttu-id="604e9-135">Jinak si samozřejmě můžete vybrat jiné úložiště. Potom nastavte přihlašovací údaje pro ověření.</span><span class="sxs-lookup"><span data-stu-id="604e9-135">Optionally choose the repository of your choice and setup the authorization credentials.</span></span>
   
   ![image7][image7]
8. <span data-ttu-id="604e9-137">Po ověření přístupu úložiště pak můžete vybrat projekt a větev, kterou chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="604e9-137">After authorization to your repository, you can then choose a project and branch you wish to deploy.</span></span> <span data-ttu-id="604e9-138">Dole je několik smyšlených příkladů.</span><span class="sxs-lookup"><span data-stu-id="604e9-138">There are several fictitious sample examples listed below.</span></span>
   
   ![image8][image8]
9. <span data-ttu-id="604e9-140">Když si vyberete projekt a větev, klikněte na OK.</span><span class="sxs-lookup"><span data-stu-id="604e9-140">Once you choose your project and branch, click ok.</span></span> <span data-ttu-id="604e9-141">Měla by se vám začít objevovat upozornění na nasazení.</span><span class="sxs-lookup"><span data-stu-id="604e9-141">You should start to see notifications of a deployment.</span></span>
   
   ![image9][image9]
10. <span data-ttu-id="604e9-143">Přejděte zpět na GitHub, kde uvidíte webhook vytvořený pro integraci úložiště správy zdrojového kódu s Azure.</span><span class="sxs-lookup"><span data-stu-id="604e9-143">Navigate back to GitHub to see the webhook that was created to integrate the source control repo with Azure.</span></span> <span data-ttu-id="604e9-144">Azure Portal umožňuje integraci s GitHubem v několika jednoduchých krocích.</span><span class="sxs-lookup"><span data-stu-id="604e9-144">The Azure Portal enables integration with GitHub with only a few simple steps.</span></span>
    
    ![image10][image10]
11. <span data-ttu-id="604e9-146">Abychom si ukázali průběžné nasazování, rychle do úložiště přidáte nějaký obsah.</span><span class="sxs-lookup"><span data-stu-id="604e9-146">To demonstrate continuous deployment, you quickly add some content to the repository.</span></span> <span data-ttu-id="604e9-147">Snadné je třeba přidání ukázkového textového souboru do úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="604e9-147">For a simple example, add a sample text file to a GitHub repo.</span></span> <span data-ttu-id="604e9-148">Se službou App Service můžete použít aplikaci .NET, Ruby, Python nebo i jiný druh.</span><span class="sxs-lookup"><span data-stu-id="604e9-148">You are free to use .NET, Ruby, Python, or some other type of application with App Service.</span></span> <span data-ttu-id="604e9-149">Do vybraného úložiště můžete přidat textový soubor nebo aplikaci napsanou v ASP.NET MVC, Javě nebo Ruby.</span><span class="sxs-lookup"><span data-stu-id="604e9-149">Feel free to add a text file, ASP.NET MVC, Java, or Ruby application to the repo of your choice.</span></span>
    
    ![image11][image11]
12. <span data-ttu-id="604e9-151">Po potvrzení změn do úložiště uvidíte, že se v oblasti upozornění portálu oznámí zahájení nasazení.</span><span class="sxs-lookup"><span data-stu-id="604e9-151">After committing changes to your repository, you see a new deployment initiate in the portal notifications area.</span></span> <span data-ttu-id="604e9-152">Pokud do několika chvil po odeslání do úložiště, neuvidíte změny, klikněte na Synchronizovat.</span><span class="sxs-lookup"><span data-stu-id="604e9-152">Click Sync if you do not quickly see changes after committing to your repository.</span></span>
    
    ![image12][image12]
13. <span data-ttu-id="604e9-154">Pokud se teď pokusíte načíst stránku pro službu aplikace, může se zobrazit chyba 403.</span><span class="sxs-lookup"><span data-stu-id="604e9-154">At this point, if you try and load the page for the app service, you may receive a 403 error.</span></span> <span data-ttu-id="604e9-155">V tomto příkladu to nastalo, protože pro tuto stránku nejsou dostupná výchozí nastavení dokumentů, jako je třeba soubor index.htm nebo default.html.</span><span class="sxs-lookup"><span data-stu-id="604e9-155">In this example, it is because there is no typical default document setup for the page such as a file like index.htm or default.html.</span></span> <span data-ttu-id="604e9-156">To můžete rychle napravit pomocí nástrojů na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="604e9-156">You can quickly remedy this with the tooling in the Azure Portal.</span></span>  <span data-ttu-id="604e9-157">Na portálu Azure vyberte Nastavení &gt; Nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="604e9-157">In the Azure Portal choose Settings &gt; Application Settings.</span></span>
    
     ![image13][image13]
14. <span data-ttu-id="604e9-159">Otevře se okno s nastavením aplikace.</span><span class="sxs-lookup"><span data-stu-id="604e9-159">A blade opens for application settings.</span></span> <span data-ttu-id="604e9-160">Zadejte název stránky „SamplePage.html“ a klikněte na Uložit.</span><span class="sxs-lookup"><span data-stu-id="604e9-160">Enter the name of the page “SamplePage.html” and click Save.</span></span> <span data-ttu-id="604e9-161">Podívejte se i na další nastavení, dejte tomu několik minut.</span><span class="sxs-lookup"><span data-stu-id="604e9-161">Take a few minutes to explore the other settings.</span></span>
    
    ![image14][image14]
15. <span data-ttu-id="604e9-163">Pokud chcete, můžete v prohlížeči obnovit stránku a zkontrolovat, že se zobrazí očekávané změny.</span><span class="sxs-lookup"><span data-stu-id="604e9-163">Optionally refresh your browser URL to ensure you see the expected changes.</span></span> <span data-ttu-id="604e9-164">V tomto případě se teď na stránce zobrazí jednoduchý text.</span><span class="sxs-lookup"><span data-stu-id="604e9-164">In this case, there is some simple text now populating the page.</span></span> <span data-ttu-id="604e9-165">S každou další změnou v úložišti by se mělo automaticky provést nové nasazení.</span><span class="sxs-lookup"><span data-stu-id="604e9-165">Each additional change to the repository would result in a new automatic deployment.</span></span>
    
    ![image15][image15]
    
    <span data-ttu-id="604e9-167">S portálem Azure je povolení průběžného nasazení snadná záležitost.</span><span class="sxs-lookup"><span data-stu-id="604e9-167">Enabling continuous deployment with the Azure Portal is an easy experience.</span></span> <span data-ttu-id="604e9-168">Pro nasazení do Azure můžete vytvořit i složitější kanály pro vydávání a použít spoustu jiných postupů s existující správou zdrojového kódu a systémů průběžné integrace, jako je například využití automatizovaných systémů správy sestavení a vydávání.</span><span class="sxs-lookup"><span data-stu-id="604e9-168">You can also build more complex release pipelines and use many other techniques with existing source control and continuous integration systems to deploy to Azure, such as leveraging automated build and release management systems.</span></span>

## <a name="develop-and-test-an-app"></a><span data-ttu-id="604e9-169">Vývoj a testování aplikace</span><span class="sxs-lookup"><span data-stu-id="604e9-169">Develop and test an app</span></span>
<span data-ttu-id="604e9-170">Teď uděláme nějaké pár změn v základu kódu a tyto změny rychle nasadíme.</span><span class="sxs-lookup"><span data-stu-id="604e9-170">Next, make some changes to the code base and rapidly deploy those changes.</span></span> <span data-ttu-id="604e9-171">Také pro webovou aplikaci připravíte testování výkonnosti.</span><span class="sxs-lookup"><span data-stu-id="604e9-171">You will also setup up some performance testing for the Web app.</span></span>

1. <span data-ttu-id="604e9-172">Na portálu Azure v navigačním podokně vyberte App Services a vyhledejte svoji aplikační službu.</span><span class="sxs-lookup"><span data-stu-id="604e9-172">In the Azure Portal choose App Services from the navigation pane, and locate your App Service.</span></span>
   
   ![image16][image16]
2. <span data-ttu-id="604e9-174">Klikněte na Nástroje</span><span class="sxs-lookup"><span data-stu-id="604e9-174">Click Tools</span></span>
   
   ![image17][image17]
3. <span data-ttu-id="604e9-176">Všimněte si kategorie vývoje v části Nástroje.</span><span class="sxs-lookup"><span data-stu-id="604e9-176">Notice the develop category under Tools.</span></span> <span data-ttu-id="604e9-177">Je tu několik užitečných nástrojů, které nám umožňují pracovat s aplikacemi bez toho, abychom museli portál Azure opustit.</span><span class="sxs-lookup"><span data-stu-id="604e9-177">There are several useful tools here that allow us to work with apps without leaving the Azure Portal.</span></span> <span data-ttu-id="604e9-178">Klikněte na Konzolu.</span><span class="sxs-lookup"><span data-stu-id="604e9-178">Click on Console.</span></span>
   
   ![image18][image18]
4. <span data-ttu-id="604e9-180">V okně konzoly může v reálném čase zadávat příkazy pro svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="604e9-180">In the console window, you can issue live commands for your app.</span></span> <span data-ttu-id="604e9-181">Zadejte příkaz dir a stiskněte enter.</span><span class="sxs-lookup"><span data-stu-id="604e9-181">Type the dir command and hit enter.</span></span> <span data-ttu-id="604e9-182">Všimněte si, že příkazy, které potřebují zvýšená oprávnění, nebudou fungovat.</span><span class="sxs-lookup"><span data-stu-id="604e9-182">Note that commands requiring elevated privileges do not work.</span></span>
   
   ![image19][image19]
5. <span data-ttu-id="604e9-184">Přejděte zpět do kategorie Vývoj a vyberte Visual Studio Online.</span><span class="sxs-lookup"><span data-stu-id="604e9-184">Move back to the Develop category and choose Visual Studio Online.</span></span> <span data-ttu-id="604e9-185">Poznámka: Visual Studio Online je teď jmenuje Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="604e9-185">Note: Visual Studio Online is now named Visual Studio Team Services.</span></span>
   
   ![image20][image20]
6. <span data-ttu-id="604e9-187">Ve své aplikaci zapněte prostředí pro úpravy v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="604e9-187">Toggle on the in-browser editing experience for your App.</span></span>
   
   ![image21][image21]
7. <span data-ttu-id="604e9-189">Nainstaluje se webové rozšíření pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="604e9-189">A web extension installs for your app.</span></span> <span data-ttu-id="604e9-190">Rozšíření můžou rychle a snadno přidat funkce do aplikací v Azure.</span><span class="sxs-lookup"><span data-stu-id="604e9-190">Extensions quickly and easily add functionality to apps in Azure.</span></span> <span data-ttu-id="604e9-191">Na snímku obrazovky dole si všimněte některých dalších dostupných typů rozšíření.</span><span class="sxs-lookup"><span data-stu-id="604e9-191">Notice some of the other extension types available in the screenshot below.</span></span>
   
   ![image22][image22]
8. <span data-ttu-id="604e9-193">Až se rozšíření Visual Studio Online nainstaluje, klikněte na Přejít.</span><span class="sxs-lookup"><span data-stu-id="604e9-193">Once the Visual Studio Online extension installs, click Go.</span></span>
   
   ![image23][image23]
9. <span data-ttu-id="604e9-195">V prohlížeči se otevře karta, kde přímo v prohlížeči uvidíte integrované vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="604e9-195">A browser tab opens where you see a development IDE directly in the browser.</span></span> <span data-ttu-id="604e9-196">Všimněte si, že dole je použitý prohlížeč Chrome.</span><span class="sxs-lookup"><span data-stu-id="604e9-196">Notice the experience below is in Chrome.</span></span>
   
   ![image24][image24]
10. <span data-ttu-id="604e9-198">Můžete provádět několik různých akcí, jako třeba upravit soubory, přidat soubory a složky a stahovat obsah z živého webu.</span><span class="sxs-lookup"><span data-stu-id="604e9-198">You can perform several activities such as edit files, add files and folders, and download content from the live site.</span></span> <span data-ttu-id="604e9-199">Udělejte pár rychlých změn v souboru SamplePage.html.</span><span class="sxs-lookup"><span data-stu-id="604e9-199">Make a quick edit to the SamplePage.html file.</span></span>
    
    ![image25][image25]
11. <span data-ttu-id="604e9-201">Za chvíli se změny automaticky uloží.</span><span class="sxs-lookup"><span data-stu-id="604e9-201">In a few moments, the changes are automatically saved.</span></span> <span data-ttu-id="604e9-202">Když přejdete na zpět stránku, uvidíte změny.</span><span class="sxs-lookup"><span data-stu-id="604e9-202">If you navigate back to the page, you can see the changes.</span></span> <span data-ttu-id="604e9-203">Nezapomeňte, že takovéto živé úpravy, nejspíš nejsou vhodné pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="604e9-203">Keep in mind live edits like these are most likely not suitable for production environments.</span></span> <span data-ttu-id="604e9-204">Tyto nástroje ale výrazně usnadňují provádění rychlé změn pro vývojové a testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="604e9-204">However, the tools make it very easy to make quick changes for dev and test environments.</span></span>
    
    ![image26][image26]
    
    ![image27][image27]
12. <span data-ttu-id="604e9-207">Přejděte zpět do okna nástrojů a v kategorii Vývoj klikněte na Test výkonnosti.</span><span class="sxs-lookup"><span data-stu-id="604e9-207">Move back to the tools blade and under the Develop category, click on Performance Test.</span></span>
    
    ![image28][image28]
13. <span data-ttu-id="604e9-209">Musíte nastavit účet služby Team Services.</span><span class="sxs-lookup"><span data-stu-id="604e9-209">You need to set a team services account.</span></span> <span data-ttu-id="604e9-210">Podrobnosti najdete tady: [Vytvoření účtu služby Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)</span><span class="sxs-lookup"><span data-stu-id="604e9-210">See here for more details: [Create a Team Services Account](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)</span></span>
14. <span data-ttu-id="604e9-211">Klikněte na Nový, tím vytvoříte test výkonnosti.</span><span class="sxs-lookup"><span data-stu-id="604e9-211">Click on New to create a performance test.</span></span>
    
    ![image29][image29]
    
    <span data-ttu-id="604e9-213">Nakonfigurujte různé hodnoty a klikněte na Spustit Test v dolní části dialogu spuštění testu výkonnosti.</span><span class="sxs-lookup"><span data-stu-id="604e9-213">Configure the various values and click Run Test at the bottom of the dialogue to initiate a performance test.</span></span>
    
    ![image30][image30]
    
    ![image31][image31]
15. <span data-ttu-id="604e9-216">Když se test spustí, můžete sledovat stav.</span><span class="sxs-lookup"><span data-stu-id="604e9-216">Once the test starts running, you can monitor the state.</span></span>
    
    ![image32][image32]
    
    <span data-ttu-id="604e9-218">Když test skončí, klikněte na výsledek, tak zobrazíte další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="604e9-218">Once the test finishes, clicking on the result shows more details.</span></span>
    
    ![image33][image33]
16. <span data-ttu-id="604e9-220">V tomto příkladu jste vytvořili malý testovací běh, takže k analýze máte jen omezené množství dat, ale můžete se podívat na různé metriky a z tohoto zobrazení test spustit znovu.</span><span class="sxs-lookup"><span data-stu-id="604e9-220">In this example, you created a small test run, so there is limited data to analyze, but you can see various metrics as well as rerun your test from this view.</span></span> <span data-ttu-id="604e9-221">S portálem Azure je vytváření, spouštění a analýza testů výkonnosti webu jednoduchý proces.</span><span class="sxs-lookup"><span data-stu-id="604e9-221">The Azure Portal makes creating, executing, and analyzing web performance tests an easy process.</span></span> <span data-ttu-id="604e9-222">Dole na snímcích jsou vidět výkonnostní data.</span><span class="sxs-lookup"><span data-stu-id="604e9-222">The screenshots below display the performance data.</span></span>
    
    ![image34][image34]
    
    ![image35][image35]
    
    ![image36][image36]

## <a name="monitoring-and-troubleshooting-an-app"></a><span data-ttu-id="604e9-226">Sledování a řešení potíží s aplikací</span><span class="sxs-lookup"><span data-stu-id="604e9-226">Monitoring and troubleshooting an app</span></span>
<span data-ttu-id="604e9-227">Azure poskytuje mnoho funkcí pro sledování a řešení potíží se spuštěnými aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="604e9-227">Azure provides many capabilities for monitoring and troubleshooting running applications.</span></span>

1. <span data-ttu-id="604e9-228">Na portálu Azure pro naši webovou aplikaci vyberte Nástroje.</span><span class="sxs-lookup"><span data-stu-id="604e9-228">In the Azure Portal for our Web app choose Tools.</span></span>
   
   ![image37][image37]
2. <span data-ttu-id="604e9-230">V kategorii Řešení potíží si všimněte si různých možností pro řešení potíží s potenciální problémy se spuštěnými aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="604e9-230">Under the Troubleshoot category, notice the various choices for using tools to troubleshoot potential issues with a running app.</span></span> <span data-ttu-id="604e9-231">Můžete třeba monitorovat živé přenosy HTTP, povolit samoopravování, zobrazit protokoly a další.</span><span class="sxs-lookup"><span data-stu-id="604e9-231">You can do things like monitor Live HTTP traffic, enable self-healing, view logs, and more.</span></span>
   
   ![image38][image38]
3. <span data-ttu-id="604e9-233">Vyberte Metriku lokality a uvidíte rychlé zobrazení některých kódů HTTP.</span><span class="sxs-lookup"><span data-stu-id="604e9-233">Choose Site Metrics to quickly get a view of some HTTP codes.</span></span>
   
   ![image39][image39]
4. <span data-ttu-id="604e9-235">Vyberte Diagnostics as a Service.</span><span class="sxs-lookup"><span data-stu-id="604e9-235">Choose Diagnostics as a Service.</span></span> <span data-ttu-id="604e9-236">Vyberte typ své aplikace, pak klikněte na tlačítko Spustit.</span><span class="sxs-lookup"><span data-stu-id="604e9-236">Choose your application type, then choose Run.</span></span>
   
   ![image40][image40]
   
   <span data-ttu-id="604e9-238">Začne sběr.</span><span class="sxs-lookup"><span data-stu-id="604e9-238">A collection begins.</span></span>
   
   ![image41][image41]
5. <span data-ttu-id="604e9-240">Můžete vybrat odpovídající protokol a diagnostikovat potenciální potíže.</span><span class="sxs-lookup"><span data-stu-id="604e9-240">You may choose the appropriate log to diagnose potential issues.</span></span> <span data-ttu-id="604e9-241">Abyste mohli vidět všechny možnosti pro data, jako je například Budete muset povolit protokolování zobrazíte všechny možnosti dostupných dat, jako například Protokoly HTTP.</span><span class="sxs-lookup"><span data-stu-id="604e9-241">You need to enable logging to see all of the available data options such as HTTP Logs.</span></span>
   
   ![image42][image42]
   
   <span data-ttu-id="604e9-243">Kliknutím na soubor Výpis stavu paměti můžete stáhnout a analyzovat sestavu analýzy nástroje DebugDiag, která vám pomůže najít potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="604e9-243">By clicking on the Memory Dump file you can download and analyze a DebugDiag analysis report to help find potential issues.</span></span>
   
   ![image43][image43]
6. <span data-ttu-id="604e9-245">Pokud chcete zobrazit víc dat, musíte povolit další protokolování.</span><span class="sxs-lookup"><span data-stu-id="604e9-245">To view more data, you need to enable additional logging.</span></span> <span data-ttu-id="604e9-246">Na portálu Azure přejděte na webovou aplikaci a vyberte Nastavení.</span><span class="sxs-lookup"><span data-stu-id="604e9-246">In the Azure Portal, navigate to the Web app and choose Settings.</span></span>
   
   ![image44][image44]
7. <span data-ttu-id="604e9-248">Přejděte dolů do kategorie funkcí a vyberte Diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="604e9-248">Scroll down to the features category, and choose Diagnostic logs.</span></span>
   
      ![image45][image45]
8. <span data-ttu-id="604e9-250">Všimněte si, že jsou různé možností pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="604e9-250">Notice the various options for logging.</span></span> <span data-ttu-id="604e9-251">Zapněte protokolování webového serveru a klikněte na Uložit.</span><span class="sxs-lookup"><span data-stu-id="604e9-251">Toggle on Web server logging and click save.</span></span>
   
   ![image46][image46]
9. <span data-ttu-id="604e9-253">Přejděte zpět do oblasti nástroje pro aplikaci, vyberte Diagnostics as a service a klikněte na Spustit, tím znovu spustíte shromažďování dat.</span><span class="sxs-lookup"><span data-stu-id="604e9-253">Move back to the tools area for the app and choose Diagnostics as a service and click Run to rerun the data collection.</span></span>
   
   ![image47][image47]
10. <span data-ttu-id="604e9-255">Když je protokolování HTTP povolené, uvidíte data shromážděná pro protokoly HTTP.</span><span class="sxs-lookup"><span data-stu-id="604e9-255">With the HTTP logging setting enabled, you now see data collected for HTTP Logs.</span></span>
    
    ![image48][image48]
11. <span data-ttu-id="604e9-257">Kliknutím na soubor protokolu HTML můžete vytvářet bohaté sestavy na bázi prohlížeče pro další zkoumání.</span><span class="sxs-lookup"><span data-stu-id="604e9-257">By clicking the HTML file log, you produce a rich browser-based report for further investigation.</span></span>
    
    ![image49][image49]
12. <span data-ttu-id="604e9-259">Přejděte zpět do části nástroje na portálu Azure pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="604e9-259">Move back to the tools section in the Azure Portal for the app.</span></span> <span data-ttu-id="604e9-260">Přejděte do části Nástroje a vyberte Process Explorer.</span><span class="sxs-lookup"><span data-stu-id="604e9-260">Scroll to the Tools section and choose Process Explorer.</span></span>
    
    ![image50][image50]
13. <span data-ttu-id="604e9-262">Když vyberete Process Explorer, můžete se podívat na podrobnosti o spuštěných procesech.</span><span class="sxs-lookup"><span data-stu-id="604e9-262">By choosing Process Explorer, you can view details about running processes.</span></span> <span data-ttu-id="604e9-263">Na snímku obrazovky dole si všimněte, že můžete procházet podrobné informace o procesech a dokonce je ukončit, a to všechno v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="604e9-263">Notice below you can drill into processes and even kill processes all from the Azure Portal.</span></span>
    
    ![image51][image51]
    
    ![image52][image52]
14. <span data-ttu-id="604e9-266">Přejděte zpět do levého okna nastavení.</span><span class="sxs-lookup"><span data-stu-id="604e9-266">Move back to the Settings blade on the left.</span></span> <span data-ttu-id="604e9-267">Klikněte na Novou žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="604e9-267">Click New support request.</span></span>
    
    ![image53][image53]
15. <span data-ttu-id="604e9-269">V pravém okně můžete zadat podrobnosti o problémech, kontaktní informace a dokonce odeslat diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="604e9-269">From the blade on the right, you can fill out details about the issues, enter contact information, and even upload diagnostic data.</span></span> <span data-ttu-id="604e9-270">S portálem Azure jde spolupráce s podporou společnosti Microsoft úplně hladce.</span><span class="sxs-lookup"><span data-stu-id="604e9-270">The Azure Portal enables working with Microsoft support a seamless experience.</span></span>
    
    ![image54][image54]
    
    ![image55][image55]
    
    <span data-ttu-id="604e9-273">Portál Azure poskytuje výkonné a povědomé nástroje a prostředí, které vám pomůžou sledovat naše spuštěné aplikace a řešit potíže s nimi.</span><span class="sxs-lookup"><span data-stu-id="604e9-273">The Azure Portal helps provide powerful and familiar tooling experiences to help monitor and troubleshoot our running applications.</span></span> <span data-ttu-id="604e9-274">Můžete taky rychle provádět různé akce pomocí úloh, jako je například recyklace procesů, povolování a zakazování různých sběrů dat a dokonce integrace s profesionální podporou společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="604e9-274">You are also able to take action quickly by performing tasks such as recycling processes, enabling and disabling various data collections, and even integrating with Microsoft professional support.</span></span>

## <a name="general-application-management"></a><span data-ttu-id="604e9-275">Obecná správa aplikací</span><span class="sxs-lookup"><span data-stu-id="604e9-275">General Application Management</span></span>
<span data-ttu-id="604e9-276">Při správě aplikací často potřebujete provádět množství různých činností, jako je například konfigurace strategií zálohování, implementace a správa poskytovatelů identit a konfigurace řízení přístupu na základě rolí.</span><span class="sxs-lookup"><span data-stu-id="604e9-276">When managing applications, you often need to perform a broad variety of activities such as configuring backup strategies, implementing and managing identity providers, and configuring Role-based access control.</span></span> <span data-ttu-id="604e9-277">Stejně jiná prostředí DevOps i platforma Azure integruje tyto úlohy přímo do portálu.</span><span class="sxs-lookup"><span data-stu-id="604e9-277">As with the other DevOps experiences, the Azure platform integrates these tasks directly into the portal.</span></span>

1. <span data-ttu-id="604e9-278">Pokud chcete zajistit, že nemůže dojít ke ztrátě dat webové aplikace, musíte nastavit zálohování.</span><span class="sxs-lookup"><span data-stu-id="604e9-278">To ensure you are keeping the Web App safe from data loss you need to configure backups.</span></span> <span data-ttu-id="604e9-279">Přejděte do oblasti Nastavení pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="604e9-279">Navigate to the Settings area for your Web app.</span></span>
   
   ![image56][image56]
2. <span data-ttu-id="604e9-281">V pravém okně přejděte v kategorie Funkce.</span><span class="sxs-lookup"><span data-stu-id="604e9-281">In the blade on the right, scroll down to the Features category.</span></span>
   
    ![image57][image57]
3. <span data-ttu-id="604e9-283">Vyberte Zálohy. Na pravé straně se otevře okno.</span><span class="sxs-lookup"><span data-stu-id="604e9-283">Choose Backups; a blade opens on the right.</span></span>
   
   ![image58][image58]
4. <span data-ttu-id="604e9-285">Klikněte na Konfigurovat a v pravém okně vyberte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="604e9-285">Click Configure, choose a storage account from the blade on the right.</span></span>
   
   ![image59][image59]
5. <span data-ttu-id="604e9-287">Teď vytvořte a vyberte kontejner úložiště pro uložení záloh.</span><span class="sxs-lookup"><span data-stu-id="604e9-287">Now create and choose a storage container to hold your backups.</span></span> <span data-ttu-id="604e9-288">Dole na formuláři klikněte na Vytvořit.</span><span class="sxs-lookup"><span data-stu-id="604e9-288">Click create at the bottom of the blade.</span></span> <span data-ttu-id="604e9-289">Potom vyberte kontejner.</span><span class="sxs-lookup"><span data-stu-id="604e9-289">Then select the container.</span></span>
   
   ![image60][image60]
6. <span data-ttu-id="604e9-291">Když vyberete kontejner, můžete nakonfigurovat plány a vytvářet zálohy databází.</span><span class="sxs-lookup"><span data-stu-id="604e9-291">Once you have chosen the container, you can configure schedules, as well as setup backups for your databases.</span></span> <span data-ttu-id="604e9-292">V tomto scénáři, klikněte na ikonu Uložit.</span><span class="sxs-lookup"><span data-stu-id="604e9-292">For this scenario, click the save icon.</span></span>
   
    ![image61][image61]
7. <span data-ttu-id="604e9-294">Po uložení přejděte zpět do levého okna pro Zálohy.</span><span class="sxs-lookup"><span data-stu-id="604e9-294">After saving, scroll back to the blade on the left for Backups.</span></span> <span data-ttu-id="604e9-295">Klikněte na Zálohovat a aplikace se zazálohuje.</span><span class="sxs-lookup"><span data-stu-id="604e9-295">Click Backup Now to back the application.</span></span>
   
    ![image62][image62]
8. <span data-ttu-id="604e9-297">Za chvíli uvidíte, že se vytvořila záloha.</span><span class="sxs-lookup"><span data-stu-id="604e9-297">In a few moments, you see a backup created.</span></span> <span data-ttu-id="604e9-298">Na snímku obrazovky dole si všimněte možnosti Obnovit.</span><span class="sxs-lookup"><span data-stu-id="604e9-298">Notice the Restore Now option on the screen-shot below.</span></span>
   
    ![image63][image63]
9. <span data-ttu-id="604e9-300">Klikněte na Obnovit a podívejte se na možnosti v pravém okně.</span><span class="sxs-lookup"><span data-stu-id="604e9-300">Click on Restore Now and examine the options to the blade on the right.</span></span> <span data-ttu-id="604e9-301">Můžete vybrat zálohu, kterou chcete, a podle potřeby snadno obnovit předchozí stav.</span><span class="sxs-lookup"><span data-stu-id="604e9-301">You can choose an appropriate backup and easily restore to an earlier state as necessary.</span></span> <span data-ttu-id="604e9-302">Portál Azure nám pomohl pro aplikaci jednoduše povolit strategie obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="604e9-302">The Azure portal has helped us easily enable a simple disaster recovery strategy for the app.</span></span>
   
    ![image64][image64]
10. <span data-ttu-id="604e9-304">Přejděte zpět do levého okna nastavení a v části Funkce vyberte možnost Ověřování / autorizace.</span><span class="sxs-lookup"><span data-stu-id="604e9-304">Move back to the Settings blade on the left, and under Features and choose Authentication/Authorization.</span></span>
    
     ![image65][image65]
11. <span data-ttu-id="604e9-306">V pravém okně vyberte Ověřování pomocí služby App Service.</span><span class="sxs-lookup"><span data-stu-id="604e9-306">In the blade on the right choose App Service Authentication.</span></span> <span data-ttu-id="604e9-307">Všimněte si, že máte na výběr různé možnosti, které můžete konfigurovat pro oblíbené zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="604e9-307">Notice the variety of options you can configure with popular providers.</span></span>
    
     ![image66][image66]
12. <span data-ttu-id="604e9-309">Vyberte poskytovatele, kterého chcete, a všimněte si možností pro obor.</span><span class="sxs-lookup"><span data-stu-id="604e9-309">Choose the provider of your choice and notice the options for the scope.</span></span> <span data-ttu-id="604e9-310">Můžete zadat ID aplikace a Tajný klíč aplikace a snadno pro aplikaci povolit ověření na Facebooku.</span><span class="sxs-lookup"><span data-stu-id="604e9-310">You can provide an App ID and App Secret and quickly enable Facebook authentication for the app.</span></span> <span data-ttu-id="604e9-311">Portál Azure pro aplikace umožňuje ověřování jako řešení na klíč.</span><span class="sxs-lookup"><span data-stu-id="604e9-311">The Azure Portal enables authentication as a turnkey solution for apps.</span></span>
    
     ![image67][image67]
13. <span data-ttu-id="604e9-313">Přejděte zpět do okna Nastavení a v kategorii Správa prostředků vyberte možnost Uživatelé.</span><span class="sxs-lookup"><span data-stu-id="604e9-313">Move back to the Settings blade and choose Users under the Resource Management category.</span></span>
    
     ![image68][image68]
14. <span data-ttu-id="604e9-315">V pravém okně se podívejte na různé možnosti pro přidávání rolí a uživatelů.</span><span class="sxs-lookup"><span data-stu-id="604e9-315">In the blade on the right examine the various options for adding roles and users.</span></span> <span data-ttu-id="604e9-316">Portál Azure vám umožňuje snadno ovládat RBAC (řízení přístupu na základě role) pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="604e9-316">The Azure Portal lets you easily control RBAC (Role-based access control) for the application.</span></span>
    
     ![image69][image69]

## <a name="summary"></a><span data-ttu-id="604e9-318">Souhrn</span><span class="sxs-lookup"><span data-stu-id="604e9-318">Summary</span></span>
<span data-ttu-id="604e9-319">V tomto kurzu jsme vám ukázali některé schopnosti platformy Azure – rychlé zapnutí průběžného nasazování pro webovou aplikaci, provádění různých činností souvisejících s vývojem a testováním, sledování a řešení potíží s živou aplikací, a nakonec správu klíčových strategií, jako je zotavení po havárii, identita a řízení přístupu na základě rolí.</span><span class="sxs-lookup"><span data-stu-id="604e9-319">This tutorial demonstrated some of the power with the Azure platform by quickly enabling continuous deployment for a web app, performing various development and testing activities, monitoring and troubleshooting a live app, and finally managing key strategies such as disaster recovery, identity, and role-based access control.</span></span> <span data-ttu-id="604e9-320">Platforma Azure poskytuje integrované prostředí pro tyto pracovní postupy DevOps a vy můžete efektivně pracovat, protože budete mít přehled a všechny potřebné úkoly po ruce.</span><span class="sxs-lookup"><span data-stu-id="604e9-320">The Azure platform enables an integrated experience for these DevOps workflows, and you can work efficiently by staying in context for the task at hand.</span></span>

## <a name="next-steps"></a><span data-ttu-id="604e9-321">Další kroky</span><span class="sxs-lookup"><span data-stu-id="604e9-321">Next steps</span></span>
* <span data-ttu-id="604e9-322">Azure Resource Manager je důležitý k tomu, aby DevOps mohl fungovat na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="604e9-322">Azure Resource Manager is important for enabling DevOps on the Azure platform.</span></span>  <span data-ttu-id="604e9-323">Další informace najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="604e9-323">To learn more visit [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
* <span data-ttu-id="604e9-324">Další informace o nasazování do Azure App Service najdete v tématu [Nasazení vaší aplikace do Azure App Service](../app-service-web/web-sites-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="604e9-324">To learn more about Azure App Service deployment visit [Deploy your app to Azure App Service](../app-service-web/web-sites-deploy.md)</span></span>

[image1]: ./media/tutorial-azureportal-devops/image1.png
[image2]: ./media/tutorial-azureportal-devops/image2.png
[image3]: ./media/tutorial-azureportal-devops/image3.png
[image4]: ./media/tutorial-azureportal-devops/image4.png
[image5]: ./media/tutorial-azureportal-devops/image5.png
[image6]: ./media/tutorial-azureportal-devops/image6.png
[image7]: ./media/tutorial-azureportal-devops/image7.png
[image8]: ./media/tutorial-azureportal-devops/image8.png
[image9]: ./media/tutorial-azureportal-devops/image9.png
[image10]: ./media/tutorial-azureportal-devops/image10.png
[image11]: ./media/tutorial-azureportal-devops/image11.png
[image12]: ./media/tutorial-azureportal-devops/image12.png
[image13]: ./media/tutorial-azureportal-devops/image13.png
[image14]: ./media/tutorial-azureportal-devops/image14.png
[image15]: ./media/tutorial-azureportal-devops/image15.png
[image16]: ./media/tutorial-azureportal-devops/image16.png
[image17]: ./media/tutorial-azureportal-devops/image17.png
[image18]: ./media/tutorial-azureportal-devops/image18.png
[image19]: ./media/tutorial-azureportal-devops/image19.png
[image20]: ./media/tutorial-azureportal-devops/image20.png
[image21]: ./media/tutorial-azureportal-devops/image21.png
[image22]: ./media/tutorial-azureportal-devops/image22.png
[image23]: ./media/tutorial-azureportal-devops/image23.png
[image24]: ./media/tutorial-azureportal-devops/image24.png
[image25]: ./media/tutorial-azureportal-devops/image25.png
[image26]: ./media/tutorial-azureportal-devops/image26.png
[image27]: ./media/tutorial-azureportal-devops/image27.png
[image28]: ./media/tutorial-azureportal-devops/image28.png
[image29]: ./media/tutorial-azureportal-devops/image29.png
[image30]: ./media/tutorial-azureportal-devops/image30.png
[image31]: ./media/tutorial-azureportal-devops/image31.png
[image32]: ./media/tutorial-azureportal-devops/image32.png
[image33]: ./media/tutorial-azureportal-devops/image33.png
[image34]: ./media/tutorial-azureportal-devops/image34.png
[image35]: ./media/tutorial-azureportal-devops/image35.png
[image36]: ./media/tutorial-azureportal-devops/image36.png
[image37]: ./media/tutorial-azureportal-devops/image37.png
[image38]: ./media/tutorial-azureportal-devops/image38.png
[image39]: ./media/tutorial-azureportal-devops/image39.png
[image40]: ./media/tutorial-azureportal-devops/image40.png
[image41]: ./media/tutorial-azureportal-devops/image41.png
[image42]: ./media/tutorial-azureportal-devops/image42.png
[image43]: ./media/tutorial-azureportal-devops/image43.png
[image44]: ./media/tutorial-azureportal-devops/image44.png
[image45]: ./media/tutorial-azureportal-devops/image45.png
[image46]: ./media/tutorial-azureportal-devops/image46.png
[image47]: ./media/tutorial-azureportal-devops/image47.png
[image48]: ./media/tutorial-azureportal-devops/image48.png
[image49]: ./media/tutorial-azureportal-devops/image49.png
[image50]: ./media/tutorial-azureportal-devops/image50.png
[image51]: ./media/tutorial-azureportal-devops/image51.png
[image52]: ./media/tutorial-azureportal-devops/image52.png
[image53]: ./media/tutorial-azureportal-devops/image53.png
[image54]: ./media/tutorial-azureportal-devops/image54.png
[image55]: ./media/tutorial-azureportal-devops/image55.png
[image56]: ./media/tutorial-azureportal-devops/image56.png
[image57]: ./media/tutorial-azureportal-devops/image57.png
[image58]: ./media/tutorial-azureportal-devops/image58.png
[image59]: ./media/tutorial-azureportal-devops/image59.png
[image60]: ./media/tutorial-azureportal-devops/image60.png
[image61]: ./media/tutorial-azureportal-devops/image61.png
[image62]: ./media/tutorial-azureportal-devops/image62.png
[image63]: ./media/tutorial-azureportal-devops/image63.png
[image64]: ./media/tutorial-azureportal-devops/image64.png
[image65]: ./media/tutorial-azureportal-devops/image65.png
[image66]: ./media/tutorial-azureportal-devops/image66.png
[image67]: ./media/tutorial-azureportal-devops/image67.png
[image68]: ./media/tutorial-azureportal-devops/image68.png
[image69]: ./media/tutorial-azureportal-devops/image69.png
