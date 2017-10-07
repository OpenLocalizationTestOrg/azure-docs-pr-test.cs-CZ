---
title: "Kurz: DevOps s hello portálu Azure | Microsoft Docs"
description: "Další informace hello různých pracovních postupů DevOps v hello portálu Azure."
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
ms.openlocfilehash: 4c32dbbd4e4b1c3809ef4b01e1496e350183ebde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-devops-with-hello-azure-portal"></a><span data-ttu-id="0284f-103">Kurz: DevOps s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0284f-103">Tutorial: DevOps with hello Azure Portal</span></span>
<span data-ttu-id="0284f-104">Hello platformy Azure je plná flexibilní DevOps pracovních postupů.</span><span class="sxs-lookup"><span data-stu-id="0284f-104">hello Azure platform is full of flexible DevOps workflows.</span></span> <span data-ttu-id="0284f-105">V tomto kurzu zjistíte, jak tooleverage hello možnosti hello toodevelop portálu Azure, testování, nasazení, řešení potíží s, sledovat a spravovat aplikace spuštěné.</span><span class="sxs-lookup"><span data-stu-id="0284f-105">In this tutorial, you learn how tooleverage hello capabilities of hello Azure Portal toodevelop, test, deploy, troubleshoot, monitor, and manage running applications.</span></span> <span data-ttu-id="0284f-106">Tento kurz se zaměřuje na hello následující:</span><span class="sxs-lookup"><span data-stu-id="0284f-106">This tutorial focuses on hello following:</span></span>

1. <span data-ttu-id="0284f-107">Vytvoření webové aplikace a povolení průběžného nasazování</span><span class="sxs-lookup"><span data-stu-id="0284f-107">Creating a web app and enabling continuous deployment</span></span>
2. <span data-ttu-id="0284f-108">Vývoj a testování aplikace</span><span class="sxs-lookup"><span data-stu-id="0284f-108">Develop and test an app</span></span>
3. <span data-ttu-id="0284f-109">Sledování a řešení potíží s aplikací</span><span class="sxs-lookup"><span data-stu-id="0284f-109">Monitoring and Troubleshooting an app</span></span>
4. <span data-ttu-id="0284f-110">Úlohy správy obecné aplikace</span><span class="sxs-lookup"><span data-stu-id="0284f-110">General application management tasks</span></span>

## <a name="creating-a-web-app-and-enabling-continuous-deployment"></a><span data-ttu-id="0284f-111">Vytvoření webové aplikace a povolení průběžného nasazování</span><span class="sxs-lookup"><span data-stu-id="0284f-111">Creating a web app and enabling continuous deployment</span></span>
<span data-ttu-id="0284f-112">Vytvoření webové aplikace s [Azure App Service](https://azure.microsoft.com/services/app-service/), které budete používat v hello zbytek tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0284f-112">Create a Web app with [Azure App Service](https://azure.microsoft.com/services/app-service/), which you’ll use in hello rest of this tutorial.</span></span> <span data-ttu-id="0284f-113">Na začátku povolíte průběžné nasazování z úložiště zdrojového kódu do našeho spuštěného prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="0284f-113">You’ll initially enable continuous deployment from your source code repository into our running Azure environment.</span></span>

1. <span data-ttu-id="0284f-114">Přihlaste se k portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="0284f-114">Sign into hello Azure Portal</span></span>
2. <span data-ttu-id="0284f-115">Zvolte **App Services** &gt; **ikonu Přidat** a zadejte název, vyberte předplatné a vytvořte nové tooserve skupiny prostředků jako hello kontejneru služby hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-115">Choose **App Services** &gt; **Add icon** and enter a name, choose your subscription, and create a new resource group tooserve as hello container for hello service.</span></span>
   
   <span data-ttu-id="0284f-116">Skupiny prostředků můžete povolit toomanage různé aspekty řešení hello například k fakturaci, nasazení a sledování všech jako jednu skupinu prostřednictvím [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0284f-116">Resource groups allow you toomanage various aspects of hello solution such as billing, deployments and monitoring all as a single group via [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span></span>
   
   ![image1][image1]
3. <span data-ttu-id="0284f-118">Za malou chvíli se vytvoří vaše aplikační služba.</span><span class="sxs-lookup"><span data-stu-id="0284f-118">After a few moments, your app service is created.</span></span> <span data-ttu-id="0284f-119">Trvat několik minut tooexplore hello různé možnosti nabídky služby hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="0284f-119">Take a few minutes tooexplore hello various menu options for hello service in hello portal.</span></span>
   
   ![image2][image2]    
4. <span data-ttu-id="0284f-121">Klikněte na adresu URL hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-121">Click hello URL.</span></span> <span data-ttu-id="0284f-122">Všimněte si hello různé dostupné možnosti nástroje a úložiště.</span><span class="sxs-lookup"><span data-stu-id="0284f-122">Notice hello variety of available choices for tools and repositories.</span></span> <span data-ttu-id="0284f-123">Můžete také použít hello jazyků a rozhraní zvoleného včetně .NET, Java a Ruby.</span><span class="sxs-lookup"><span data-stu-id="0284f-123">You can also use hello languages and frameworks of your choice including .NET, Java, and Ruby.</span></span>
   
   ![image3][image3]    
5. <span data-ttu-id="0284f-125">Hello portál Azure umožňuje průběžné nasazování jednoduchý proces, který zahrnuje několik jednoduchých kroků.</span><span class="sxs-lookup"><span data-stu-id="0284f-125">hello Azure portal makes continuous deployment an easy process that involves only a few simple steps.</span></span> <span data-ttu-id="0284f-126">V hello portálu Azure zvolte nastavení z hello ikony pro hello app service, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0284f-126">In hello Azure portal, choose settings from hello icon for hello app service you just created.</span></span>
   
   ![image4][image4]
   
   <span data-ttu-id="0284f-128">V okně hello, které se otevře na hello správné posuňte toohello publikování části.</span><span class="sxs-lookup"><span data-stu-id="0284f-128">From hello blade that opens on hello right, scroll toohello publishing section.</span></span>
   
   ![image5][image5]
6. <span data-ttu-id="0284f-130">V dalším kroku nakonfigurujte některé nastavení tooenable průběžné nasazování pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-130">Next, configure some settings tooenable continuous deployment for hello app.</span></span> <span data-ttu-id="0284f-131">Klikněte na Zdroj nasazení a potom klikněte na tlačítko Vybrat zdroj.</span><span class="sxs-lookup"><span data-stu-id="0284f-131">Click Deployment Source and then click Choose Source.</span></span> <span data-ttu-id="0284f-132">Všimněte si hello různé možnosti, které máte pro úložiště zdroje.</span><span class="sxs-lookup"><span data-stu-id="0284f-132">Notice hello variety of options you have for repository sources.</span></span>
   
   ![image6][image6]
7. <span data-ttu-id="0284f-134">V tomto příkladu vyberte GitHub.</span><span class="sxs-lookup"><span data-stu-id="0284f-134">For this example choose GitHub.</span></span> <span data-ttu-id="0284f-135">Volitelně vyberte hello úložiště podle vašeho výběru a nastavit přihlašovací údaje pro autorizaci hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-135">Optionally choose hello repository of your choice and setup hello authorization credentials.</span></span>
   
   ![image7][image7]
8. <span data-ttu-id="0284f-137">Po povolení tooyour úložiště pak můžete projekt a chcete toodeploy firemní pobočky.</span><span class="sxs-lookup"><span data-stu-id="0284f-137">After authorization tooyour repository, you can then choose a project and branch you wish toodeploy.</span></span> <span data-ttu-id="0284f-138">Dole je několik smyšlených příkladů.</span><span class="sxs-lookup"><span data-stu-id="0284f-138">There are several fictitious sample examples listed below.</span></span>
   
   ![image8][image8]
9. <span data-ttu-id="0284f-140">Když si vyberete projekt a větev, klikněte na OK.</span><span class="sxs-lookup"><span data-stu-id="0284f-140">Once you choose your project and branch, click ok.</span></span> <span data-ttu-id="0284f-141">Měli byste začít toosee oznámení o nasazení.</span><span class="sxs-lookup"><span data-stu-id="0284f-141">You should start toosee notifications of a deployment.</span></span>
   
   ![image9][image9]
10. <span data-ttu-id="0284f-143">Přejděte zpět tooGitHub toosee hello webhooku, který byl vytvořený toointegrate hello zdroj ovládacího prvku úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="0284f-143">Navigate back tooGitHub toosee hello webhook that was created toointegrate hello source control repo with Azure.</span></span> <span data-ttu-id="0284f-144">Hello portálu Azure umožňuje integraci s Githubu se jenom pár jednoduchých kroků.</span><span class="sxs-lookup"><span data-stu-id="0284f-144">hello Azure Portal enables integration with GitHub with only a few simple steps.</span></span>
    
    ![image10][image10]
11. <span data-ttu-id="0284f-146">průběžné nasazování toodemonstrate, rychle přidáte některé obsahu toohello úložiště.</span><span class="sxs-lookup"><span data-stu-id="0284f-146">toodemonstrate continuous deployment, you quickly add some content toohello repository.</span></span> <span data-ttu-id="0284f-147">Jednoduchý příklad přidejte úložiště GitHub tooa ukázkový textový soubor.</span><span class="sxs-lookup"><span data-stu-id="0284f-147">For a simple example, add a sample text file tooa GitHub repo.</span></span> <span data-ttu-id="0284f-148">Jste volné toouse .NET, Ruby, Python nebo jiný typ aplikace se App Service.</span><span class="sxs-lookup"><span data-stu-id="0284f-148">You are free toouse .NET, Ruby, Python, or some other type of application with App Service.</span></span> <span data-ttu-id="0284f-149">Myslíte, že volné tooadd s textovým souborem, ASP.NET MVC, Java nebo Ruby úložišti toohello aplikace podle vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="0284f-149">Feel free tooadd a text file, ASP.NET MVC, Java, or Ruby application toohello repo of your choice.</span></span>
    
    ![image11][image11]
12. <span data-ttu-id="0284f-151">Po potvrzení změn tooyour úložiště, uvidíte nový zahájit nasazení v oblasti portálu oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-151">After committing changes tooyour repository, you see a new deployment initiate in hello portal notifications area.</span></span> <span data-ttu-id="0284f-152">Pokud se nezobrazí rychle změny po potvrzení tooyour úložiště, klikněte na tlačítko synchronizovat.</span><span class="sxs-lookup"><span data-stu-id="0284f-152">Click Sync if you do not quickly see changes after committing tooyour repository.</span></span>
    
    ![image12][image12]
13. <span data-ttu-id="0284f-154">V tomto okamžiku Pokud akci a načíst stránku hello hello aplikace služby, může se zobrazit Chyba 403.</span><span class="sxs-lookup"><span data-stu-id="0284f-154">At this point, if you try and load hello page for hello app service, you may receive a 403 error.</span></span> <span data-ttu-id="0284f-155">V tomto příkladu je vzhledem k tomu, že žádné nastavení typický výchozí dokument pro stránku hello jako soubor jako index.htm nebo default.html.</span><span class="sxs-lookup"><span data-stu-id="0284f-155">In this example, it is because there is no typical default document setup for hello page such as a file like index.htm or default.html.</span></span> <span data-ttu-id="0284f-156">Můžete to rychle napravit s hello tooling v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0284f-156">You can quickly remedy this with hello tooling in hello Azure Portal.</span></span>  <span data-ttu-id="0284f-157">V hello portálu Azure vyberte nastavení &gt; nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="0284f-157">In hello Azure Portal choose Settings &gt; Application Settings.</span></span>
    
     ![image13][image13]
14. <span data-ttu-id="0284f-159">Otevře se okno s nastavením aplikace.</span><span class="sxs-lookup"><span data-stu-id="0284f-159">A blade opens for application settings.</span></span> <span data-ttu-id="0284f-160">Zadejte název hello hello stránky "SamplePage.html" a klikněte na Uložit.</span><span class="sxs-lookup"><span data-stu-id="0284f-160">Enter hello name of hello page “SamplePage.html” and click Save.</span></span> <span data-ttu-id="0284f-161">Trvat několik minut tooexplore hello další nastavení.</span><span class="sxs-lookup"><span data-stu-id="0284f-161">Take a few minutes tooexplore hello other settings.</span></span>
    
    ![image14][image14]
15. <span data-ttu-id="0284f-163">Volitelně můžete aktualizujte vaše tooensure adresu URL prohlížeče zobrazí hello očekává změny.</span><span class="sxs-lookup"><span data-stu-id="0284f-163">Optionally refresh your browser URL tooensure you see hello expected changes.</span></span> <span data-ttu-id="0284f-164">V takovém případě není jednoduchý text teď naplnění stránku hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-164">In this case, there is some simple text now populating hello page.</span></span> <span data-ttu-id="0284f-165">Každý další změny toohello úložiště by způsobilo nové automatického nasazení.</span><span class="sxs-lookup"><span data-stu-id="0284f-165">Each additional change toohello repository would result in a new automatic deployment.</span></span>
    
    ![image15][image15]
    
    <span data-ttu-id="0284f-167">Povolení průběžné nasazování pomocí hello portál Azure je snadný prostředí.</span><span class="sxs-lookup"><span data-stu-id="0284f-167">Enabling continuous deployment with hello Azure Portal is an easy experience.</span></span> <span data-ttu-id="0284f-168">Můžete také vytvořit složitější kanálů verze a použít mnoho jinými technikami s existující zdrojového kódu a průběžnou integraci tooAzure toodeploy systémy, jako je například využití automatizované sestavení a systémy správy verzí.</span><span class="sxs-lookup"><span data-stu-id="0284f-168">You can also build more complex release pipelines and use many other techniques with existing source control and continuous integration systems toodeploy tooAzure, such as leveraging automated build and release management systems.</span></span>

## <a name="develop-and-test-an-app"></a><span data-ttu-id="0284f-169">Vývoj a testování aplikace</span><span class="sxs-lookup"><span data-stu-id="0284f-169">Develop and test an app</span></span>
<span data-ttu-id="0284f-170">V dalším kroku provedeme některé změny kódu toohello základní a rychle nasadit tyto změny.</span><span class="sxs-lookup"><span data-stu-id="0284f-170">Next, make some changes toohello code base and rapidly deploy those changes.</span></span> <span data-ttu-id="0284f-171">Bude také nastavit některé testování výkonu pro hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0284f-171">You will also setup up some performance testing for hello Web app.</span></span>

1. <span data-ttu-id="0284f-172">V hello portálu Azure App Services vybírat hello navigačním podokně a vyhledejte App Service.</span><span class="sxs-lookup"><span data-stu-id="0284f-172">In hello Azure Portal choose App Services from hello navigation pane, and locate your App Service.</span></span>
   
   ![image16][image16]
2. <span data-ttu-id="0284f-174">Klikněte na Nástroje</span><span class="sxs-lookup"><span data-stu-id="0284f-174">Click Tools</span></span>
   
   ![image17][image17]
3. <span data-ttu-id="0284f-176">Všimněte si hello vyvíjet kategorie v nabídce Nástroje.</span><span class="sxs-lookup"><span data-stu-id="0284f-176">Notice hello develop category under Tools.</span></span> <span data-ttu-id="0284f-177">Existuje několik užitečné nástroje zde která nám umožňují toowork s aplikací bez opuštění hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0284f-177">There are several useful tools here that allow us toowork with apps without leaving hello Azure Portal.</span></span> <span data-ttu-id="0284f-178">Klikněte na Konzolu.</span><span class="sxs-lookup"><span data-stu-id="0284f-178">Click on Console.</span></span>
   
   ![image18][image18]
4. <span data-ttu-id="0284f-180">V okně konzoly hello můžete vydávat za provozu příkazy pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0284f-180">In hello console window, you can issue live commands for your app.</span></span> <span data-ttu-id="0284f-181">Typ hello dir příkaz a stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="0284f-181">Type hello dir command and hit enter.</span></span> <span data-ttu-id="0284f-182">Všimněte si, že příkazy, které potřebují zvýšená oprávnění, nebudou fungovat.</span><span class="sxs-lookup"><span data-stu-id="0284f-182">Note that commands requiring elevated privileges do not work.</span></span>
   
   ![image19][image19]
5. <span data-ttu-id="0284f-184">Přesunout zpět toohello vývoj kategorie a zvolte Visual Studio Online.</span><span class="sxs-lookup"><span data-stu-id="0284f-184">Move back toohello Develop category and choose Visual Studio Online.</span></span> <span data-ttu-id="0284f-185">Poznámka: Visual Studio Online je teď jmenuje Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="0284f-185">Note: Visual Studio Online is now named Visual Studio Team Services.</span></span>
   
   ![image20][image20]
6. <span data-ttu-id="0284f-187">Přepnutí na hello prostředí úprav v prohlížeči pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0284f-187">Toggle on hello in-browser editing experience for your App.</span></span>
   
   ![image21][image21]
7. <span data-ttu-id="0284f-189">Nainstaluje se webové rozšíření pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0284f-189">A web extension installs for your app.</span></span> <span data-ttu-id="0284f-190">Rozšíření rychle a snadno přidat funkce tooapps v Azure.</span><span class="sxs-lookup"><span data-stu-id="0284f-190">Extensions quickly and easily add functionality tooapps in Azure.</span></span> <span data-ttu-id="0284f-191">Všimněte si některé hello jiné typy rozšíření k dispozici v následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-191">Notice some of hello other extension types available in hello screenshot below.</span></span>
   
   ![image22][image22]
8. <span data-ttu-id="0284f-193">Po instalaci sady Visual Studio Online rozšíření hello kliknutím na Přejít.</span><span class="sxs-lookup"><span data-stu-id="0284f-193">Once hello Visual Studio Online extension installs, click Go.</span></span>
   
   ![image23][image23]
9. <span data-ttu-id="0284f-195">Prohlížeč kartě otevře, kde uvidíte vývoj IDE přímo v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-195">A browser tab opens where you see a development IDE directly in hello browser.</span></span> <span data-ttu-id="0284f-196">V prohlížeči Chrome je prostředí hello oznámení níže.</span><span class="sxs-lookup"><span data-stu-id="0284f-196">Notice hello experience below is in Chrome.</span></span>
   
   ![image24][image24]
10. <span data-ttu-id="0284f-198">Můžete provést několik aktivity, například upravit soubory, přidat soubory a složky a stahovat obsah z hello živý web.</span><span class="sxs-lookup"><span data-stu-id="0284f-198">You can perform several activities such as edit files, add files and folders, and download content from hello live site.</span></span> <span data-ttu-id="0284f-199">Zkontrolujte soubor SamplePage.html toohello rychlé upravit.</span><span class="sxs-lookup"><span data-stu-id="0284f-199">Make a quick edit toohello SamplePage.html file.</span></span>
    
    ![image25][image25]
11. <span data-ttu-id="0284f-201">Ve chvíli se automaticky uloží změny hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-201">In a few moments, hello changes are automatically saved.</span></span> <span data-ttu-id="0284f-202">Pokud přejdete zpět toohello stránky, uvidíte hello změny.</span><span class="sxs-lookup"><span data-stu-id="0284f-202">If you navigate back toohello page, you can see hello changes.</span></span> <span data-ttu-id="0284f-203">Nezapomeňte, že takovéto živé úpravy, nejspíš nejsou vhodné pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="0284f-203">Keep in mind live edits like these are most likely not suitable for production environments.</span></span> <span data-ttu-id="0284f-204">Nástroje hello však provádějte je velmi snadné toomake rychlé změny pro vývojáře a testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="0284f-204">However, hello tools make it very easy toomake quick changes for dev and test environments.</span></span>
    
    ![image26][image26]
    
    ![image27][image27]
12. <span data-ttu-id="0284f-207">Přesunout zpět toohello okno nástroje a v kategorii hello vývoj, klikněte na Test výkonnosti.</span><span class="sxs-lookup"><span data-stu-id="0284f-207">Move back toohello tools blade and under hello Develop category, click on Performance Test.</span></span>
    
    ![image28][image28]
13. <span data-ttu-id="0284f-209">Je nutné tooset účet team services.</span><span class="sxs-lookup"><span data-stu-id="0284f-209">You need tooset a team services account.</span></span> <span data-ttu-id="0284f-210">Podrobnosti najdete tady: [Vytvoření účtu služby Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)</span><span class="sxs-lookup"><span data-stu-id="0284f-210">See here for more details: [Create a Team Services Account](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)</span></span>
14. <span data-ttu-id="0284f-211">Klikněte na nový toocreate testu výkonnosti.</span><span class="sxs-lookup"><span data-stu-id="0284f-211">Click on New toocreate a performance test.</span></span>
    
    ![image29][image29]
    
    <span data-ttu-id="0284f-213">Konfigurace hello různé hodnoty a klikněte na tlačítko Spustit Test dole hello hello dialogu tooinitiate testu výkonnosti.</span><span class="sxs-lookup"><span data-stu-id="0284f-213">Configure hello various values and click Run Test at hello bottom of hello dialogue tooinitiate a performance test.</span></span>
    
    ![image30][image30]
    
    ![image31][image31]
15. <span data-ttu-id="0284f-216">Jakmile hello testovací spuštění, můžete monitorovat stav hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-216">Once hello test starts running, you can monitor hello state.</span></span>
    
    ![image32][image32]
    
    <span data-ttu-id="0284f-218">Po dokončení testu hello kliknutím na hello výsledek obsahuje další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="0284f-218">Once hello test finishes, clicking on hello result shows more details.</span></span>
    
    ![image33][image33]
16. <span data-ttu-id="0284f-220">V tomto příkladu jste vytvořili malý test spustit, je omezený data tooanalyze, takže je může zobrazit různé metriky, stejně jako znovu spustit test z tohoto zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0284f-220">In this example, you created a small test run, so there is limited data tooanalyze, but you can see various metrics as well as rerun your test from this view.</span></span> <span data-ttu-id="0284f-221">Hello portálu Azure umožňuje vytváření, provádění a analýza testy výkonnosti webu jednoduchý proces.</span><span class="sxs-lookup"><span data-stu-id="0284f-221">hello Azure Portal makes creating, executing, and analyzing web performance tests an easy process.</span></span> <span data-ttu-id="0284f-222">snímky obrazovky Hello níže zobrazte údaje o výkonu hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-222">hello screenshots below display hello performance data.</span></span>
    
    ![image34][image34]
    
    ![image35][image35]
    
    ![image36][image36]

## <a name="monitoring-and-troubleshooting-an-app"></a><span data-ttu-id="0284f-226">Sledování a řešení potíží s aplikací</span><span class="sxs-lookup"><span data-stu-id="0284f-226">Monitoring and troubleshooting an app</span></span>
<span data-ttu-id="0284f-227">Azure poskytuje mnoho funkcí pro sledování a řešení potíží se spuštěnými aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="0284f-227">Azure provides many capabilities for monitoring and troubleshooting running applications.</span></span>

1. <span data-ttu-id="0284f-228">V hello portál Azure pro webovou aplikaci příkaz nástroje.</span><span class="sxs-lookup"><span data-stu-id="0284f-228">In hello Azure Portal for our Web app choose Tools.</span></span>
   
   ![image37][image37]
2. <span data-ttu-id="0284f-230">V kategorii hello Poradce při potížích Všimněte si hello různé možnosti pro použití nástroje tootroubleshoot potenciální problémy s spuštěné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0284f-230">Under hello Troubleshoot category, notice hello various choices for using tools tootroubleshoot potential issues with a running app.</span></span> <span data-ttu-id="0284f-231">Můžete třeba monitorovat živé přenosy HTTP, povolit samoopravování, zobrazit protokoly a další.</span><span class="sxs-lookup"><span data-stu-id="0284f-231">You can do things like monitor Live HTTP traffic, enable self-healing, view logs, and more.</span></span>
   
   ![image38][image38]
3. <span data-ttu-id="0284f-233">Vyberte zobrazení některé kódy HTTP get tooquickly metriky lokality.</span><span class="sxs-lookup"><span data-stu-id="0284f-233">Choose Site Metrics tooquickly get a view of some HTTP codes.</span></span>
   
   ![image39][image39]
4. <span data-ttu-id="0284f-235">Vyberte Diagnostics as a Service.</span><span class="sxs-lookup"><span data-stu-id="0284f-235">Choose Diagnostics as a Service.</span></span> <span data-ttu-id="0284f-236">Vyberte typ své aplikace, pak klikněte na tlačítko Spustit.</span><span class="sxs-lookup"><span data-stu-id="0284f-236">Choose your application type, then choose Run.</span></span>
   
   ![image40][image40]
   
   <span data-ttu-id="0284f-238">Začne sběr.</span><span class="sxs-lookup"><span data-stu-id="0284f-238">A collection begins.</span></span>
   
   ![image41][image41]
5. <span data-ttu-id="0284f-240">Můžete se rozhodnout hello příslušný protokol toodiagnose potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="0284f-240">You may choose hello appropriate log toodiagnose potential issues.</span></span> <span data-ttu-id="0284f-241">Je nutné toosee protokolování tooenable všechna dostupná data hello možnosti, jako jsou protokoly HTTP.</span><span class="sxs-lookup"><span data-stu-id="0284f-241">You need tooenable logging toosee all of hello available data options such as HTTP Logs.</span></span>
   
   ![image42][image42]
   
   <span data-ttu-id="0284f-243">Kliknutím na soubor výpis stavu paměti hello můžete stáhnout a analyzovat nástroj DebugDiag toohelp sestavy analýzy najít potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="0284f-243">By clicking on hello Memory Dump file you can download and analyze a DebugDiag analysis report toohelp find potential issues.</span></span>
   
   ![image43][image43]
6. <span data-ttu-id="0284f-245">tooview více dat, budete potřebovat další protokolování tooenable.</span><span class="sxs-lookup"><span data-stu-id="0284f-245">tooview more data, you need tooenable additional logging.</span></span> <span data-ttu-id="0284f-246">V hello portálu Azure přejděte toohello webové aplikace a zvolte nastavení.</span><span class="sxs-lookup"><span data-stu-id="0284f-246">In hello Azure Portal, navigate toohello Web app and choose Settings.</span></span>
   
   ![image44][image44]
7. <span data-ttu-id="0284f-248">Posuňte se dolů toohello funkce kategorie a zvolte diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="0284f-248">Scroll down toohello features category, and choose Diagnostic logs.</span></span>
   
      ![image45][image45]
8. <span data-ttu-id="0284f-250">Všimněte si, hello různé možnosti pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="0284f-250">Notice hello various options for logging.</span></span> <span data-ttu-id="0284f-251">Zapněte protokolování webového serveru a klikněte na Uložit.</span><span class="sxs-lookup"><span data-stu-id="0284f-251">Toggle on Web server logging and click save.</span></span>
   
   ![image46][image46]
9. <span data-ttu-id="0284f-253">Přesunout zpět toohello nástroje oblast pro hello aplikace a zvolte diagnostiky jako službu a klikněte na shromažďování dat hello toorerun spustit.</span><span class="sxs-lookup"><span data-stu-id="0284f-253">Move back toohello tools area for hello app and choose Diagnostics as a service and click Run toorerun hello data collection.</span></span>
   
   ![image47][image47]
10. <span data-ttu-id="0284f-255">S povoleným nastavením protokolování hello HTTP, zobrazí data shromážděná pro protokoly HTTP.</span><span class="sxs-lookup"><span data-stu-id="0284f-255">With hello HTTP logging setting enabled, you now see data collected for HTTP Logs.</span></span>
    
    ![image48][image48]
11. <span data-ttu-id="0284f-257">Kliknutím na soubor protokolu hello HTML, vytvořit bohaté sestavy založené na prohlížeči pro další šetření.</span><span class="sxs-lookup"><span data-stu-id="0284f-257">By clicking hello HTML file log, you produce a rich browser-based report for further investigation.</span></span>
    
    ![image49][image49]
12. <span data-ttu-id="0284f-259">Přesun toohello nástroje kapitoly hello portál Azure pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-259">Move back toohello tools section in hello Azure Portal for hello app.</span></span> <span data-ttu-id="0284f-260">Posuňte toohello nástroje oddílu a vyberte Průzkumníka procesů.</span><span class="sxs-lookup"><span data-stu-id="0284f-260">Scroll toohello Tools section and choose Process Explorer.</span></span>
    
    ![image50][image50]
13. <span data-ttu-id="0284f-262">Když vyberete Process Explorer, můžete se podívat na podrobnosti o spuštěných procesech.</span><span class="sxs-lookup"><span data-stu-id="0284f-262">By choosing Process Explorer, you can view details about running processes.</span></span> <span data-ttu-id="0284f-263">Všimněte si níže můžete přejít k podrobnostem procesy a i ukončit procesy z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0284f-263">Notice below you can drill into processes and even kill processes all from hello Azure Portal.</span></span>
    
    ![image51][image51]
    
    ![image52][image52]
14. <span data-ttu-id="0284f-266">Přesunete okno nastavení toohello zpět na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-266">Move back toohello Settings blade on hello left.</span></span> <span data-ttu-id="0284f-267">Klikněte na Novou žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="0284f-267">Click New support request.</span></span>
    
    ![image53][image53]
15. <span data-ttu-id="0284f-269">V okně hello na hello správné můžete vyplnit podrobnosti o problémech s hello, zadejte kontaktní informace a i odeslání diagnostických dat.</span><span class="sxs-lookup"><span data-stu-id="0284f-269">From hello blade on hello right, you can fill out details about hello issues, enter contact information, and even upload diagnostic data.</span></span> <span data-ttu-id="0284f-270">Hello portálu Azure umožňuje práce s podporu společnosti Microsoft integrované prostředí.</span><span class="sxs-lookup"><span data-stu-id="0284f-270">hello Azure Portal enables working with Microsoft support a seamless experience.</span></span>
    
    ![image54][image54]
    
    ![image55][image55]
    
    <span data-ttu-id="0284f-273">Hello portálu Azure pomáhá zajistit výkonný a známých nástrojů prostředí toohelp monitorování a řešení potíží s naše spuštěné aplikace.</span><span class="sxs-lookup"><span data-stu-id="0284f-273">hello Azure Portal helps provide powerful and familiar tooling experiences toohelp monitor and troubleshoot our running applications.</span></span> <span data-ttu-id="0284f-274">Jste také možné tootake akce rychle provedením úloh, jako je například recyklace procesů, povolování a zakazování různých kolekcí dat a i integrace s profesionální podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0284f-274">You are also able tootake action quickly by performing tasks such as recycling processes, enabling and disabling various data collections, and even integrating with Microsoft professional support.</span></span>

## <a name="general-application-management"></a><span data-ttu-id="0284f-275">Obecná správa aplikací</span><span class="sxs-lookup"><span data-stu-id="0284f-275">General Application Management</span></span>
<span data-ttu-id="0284f-276">Při správě aplikací, často potřebují tooperform širokou škálu aktivity, například konfigurace strategii zálohování, implementace a Správa poskytovatelů identit a konfigurace řízení přístupu na základě rolí.</span><span class="sxs-lookup"><span data-stu-id="0284f-276">When managing applications, you often need tooperform a broad variety of activities such as configuring backup strategies, implementing and managing identity providers, and configuring Role-based access control.</span></span> <span data-ttu-id="0284f-277">Jako s hello dalších činnostech DevOps, hello platformy Azure integruje přímo do portálu hello tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="0284f-277">As with hello other DevOps experiences, hello Azure platform integrates these tasks directly into hello portal.</span></span>

1. <span data-ttu-id="0284f-278">tooensure se udržuje hello webové aplikace před únikem potřebujete tooconfigure zálohy.</span><span class="sxs-lookup"><span data-stu-id="0284f-278">tooensure you are keeping hello Web App safe from data loss you need tooconfigure backups.</span></span> <span data-ttu-id="0284f-279">Přejděte toohello nastavení oblast pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0284f-279">Navigate toohello Settings area for your Web app.</span></span>
   
   ![image56][image56]
2. <span data-ttu-id="0284f-281">V okně hello hello správné posuňte se dolů toohello funkce kategorie.</span><span class="sxs-lookup"><span data-stu-id="0284f-281">In hello blade on hello right, scroll down toohello Features category.</span></span>
   
    ![image57][image57]
3. <span data-ttu-id="0284f-283">Vyberte zálohování; Otevře se okno na hello správné.</span><span class="sxs-lookup"><span data-stu-id="0284f-283">Choose Backups; a blade opens on hello right.</span></span>
   
   ![image58][image58]
4. <span data-ttu-id="0284f-285">Kliknutím na tlačítko Konfigurovat, vyberte účet úložiště v okně hello na hello správné.</span><span class="sxs-lookup"><span data-stu-id="0284f-285">Click Configure, choose a storage account from hello blade on hello right.</span></span>
   
   ![image59][image59]
5. <span data-ttu-id="0284f-287">Teď vytvořte a vyberte toohold kontejneru úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="0284f-287">Now create and choose a storage container toohold your backups.</span></span> <span data-ttu-id="0284f-288">Kliknutím na vytvořit dole hello v okně hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-288">Click create at hello bottom of hello blade.</span></span> <span data-ttu-id="0284f-289">Pak vyberte kontejner hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-289">Then select hello container.</span></span>
   
   ![image60][image60]
6. <span data-ttu-id="0284f-291">Po zvolení hello kontejneru, můžete nakonfigurovat plány, stejně jako nastavení zálohy pro vaše databáze.</span><span class="sxs-lookup"><span data-stu-id="0284f-291">Once you have chosen hello container, you can configure schedules, as well as setup backups for your databases.</span></span> <span data-ttu-id="0284f-292">Pro tento scénář klikněte na tlačítko hello uložit ikonu.</span><span class="sxs-lookup"><span data-stu-id="0284f-292">For this scenario, click hello save icon.</span></span>
   
    ![image61][image61]
7. <span data-ttu-id="0284f-294">Po uložení, posuňte se zpět toohello okně na levém hello pro zálohy.</span><span class="sxs-lookup"><span data-stu-id="0284f-294">After saving, scroll back toohello blade on hello left for Backups.</span></span> <span data-ttu-id="0284f-295">Klikněte na tlačítko Zálohovat nyní tooback hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0284f-295">Click Backup Now tooback hello application.</span></span>
   
    ![image62][image62]
8. <span data-ttu-id="0284f-297">Za chvíli uvidíte, že se vytvořila záloha.</span><span class="sxs-lookup"><span data-stu-id="0284f-297">In a few moments, you see a backup created.</span></span> <span data-ttu-id="0284f-298">Všimněte si hello obnovit nyní možnost na hello snímek obrazovky níže.</span><span class="sxs-lookup"><span data-stu-id="0284f-298">Notice hello Restore Now option on hello screen-shot below.</span></span>
   
    ![image63][image63]
9. <span data-ttu-id="0284f-300">Klikněte na obnovit a prozkoumat hello možnosti toohello okně hello správné.</span><span class="sxs-lookup"><span data-stu-id="0284f-300">Click on Restore Now and examine hello options toohello blade on hello right.</span></span> <span data-ttu-id="0284f-301">Je možné, že že příslušné zálohování a snadno tooan obnovení dříve stavu podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="0284f-301">You can choose an appropriate backup and easily restore tooan earlier state as necessary.</span></span> <span data-ttu-id="0284f-302">Hello portál Azure pomůže nám snadno povolit strategie obnovení po havárii jednoduché aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-302">hello Azure portal has helped us easily enable a simple disaster recovery strategy for hello app.</span></span>
   
    ![image64][image64]
10. <span data-ttu-id="0284f-304">Přesunout zpět okno nastavení toohello na levé straně hello a v části funkce a zvolte ověřování/autorizace.</span><span class="sxs-lookup"><span data-stu-id="0284f-304">Move back toohello Settings blade on hello left, and under Features and choose Authentication/Authorization.</span></span>
    
     ![image65][image65]
11. <span data-ttu-id="0284f-306">V okně hello na pravém hello vyberte aplikaci služby ověřování.</span><span class="sxs-lookup"><span data-stu-id="0284f-306">In hello blade on hello right choose App Service Authentication.</span></span> <span data-ttu-id="0284f-307">Všimněte si hello různé možnosti, které můžete konfigurovat pomocí Oblíbené zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="0284f-307">Notice hello variety of options you can configure with popular providers.</span></span>
    
     ![image66][image66]
12. <span data-ttu-id="0284f-309">Vyberte poskytovatele správy hello podle svého výběru a Všimněte si hello možnosti oboru hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-309">Choose hello provider of your choice and notice hello options for hello scope.</span></span> <span data-ttu-id="0284f-310">Můžete zadejte ID aplikace a tajný klíč aplikace a rychle povolit ověřování Facebook pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-310">You can provide an App ID and App Secret and quickly enable Facebook authentication for hello app.</span></span> <span data-ttu-id="0284f-311">Hello portálu Azure umožňuje ověřování, jako to řešení na klíč pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="0284f-311">hello Azure Portal enables authentication as a turnkey solution for apps.</span></span>
    
     ![image67][image67]
13. <span data-ttu-id="0284f-313">Okno nastavení toohello přesunout zpět a vyberte uživatele v kategorii Správa prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-313">Move back toohello Settings blade and choose Users under hello Resource Management category.</span></span>
    
     ![image68][image68]
14. <span data-ttu-id="0284f-315">V okně hello na pravém hello zkontrolujte hello různé možnosti pro přidání rolí a uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0284f-315">In hello blade on hello right examine hello various options for adding roles and users.</span></span> <span data-ttu-id="0284f-316">Hello portálu Azure můžete snadno nastavit RBAC (řízení přístupu na základě rolí) pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0284f-316">hello Azure Portal lets you easily control RBAC (Role-based access control) for hello application.</span></span>
    
     ![image69][image69]

## <a name="summary"></a><span data-ttu-id="0284f-318">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0284f-318">Summary</span></span>
<span data-ttu-id="0284f-319">V tomto kurzu ukázán některé hello výkonu hello platformy Azure rychle povolením průběžné nasazování pro webovou aplikaci, provádění různých vývoj a testování aktivity, monitorování a řešení potíží s živé aplikace a nakonec správě klíče strategie například zotavení po havárii, identity a řízení přístupu na základě rolí.</span><span class="sxs-lookup"><span data-stu-id="0284f-319">This tutorial demonstrated some of hello power with hello Azure platform by quickly enabling continuous deployment for a web app, performing various development and testing activities, monitoring and troubleshooting a live app, and finally managing key strategies such as disaster recovery, identity, and role-based access control.</span></span> <span data-ttu-id="0284f-320">Hello platformy Azure umožňuje integrované možnosti pro tyto pracovní postupy DevOps a můžou pracovat efektivně protože zůstává v kontextu pro hello úlohy po ruce.</span><span class="sxs-lookup"><span data-stu-id="0284f-320">hello Azure platform enables an integrated experience for these DevOps workflows, and you can work efficiently by staying in context for hello task at hand.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0284f-321">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0284f-321">Next steps</span></span>
* <span data-ttu-id="0284f-322">Azure Resource Manager je důležité pro povolení DevOps na hello platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="0284f-322">Azure Resource Manager is important for enabling DevOps on hello Azure platform.</span></span>  <span data-ttu-id="0284f-323">toolearn více navštivte [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0284f-323">toolearn more visit [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
* <span data-ttu-id="0284f-324">informace o nasazení služby Azure App Service najdete toolearn [nasazení vaší aplikace tooAzure služby App Service](../app-service-web/web-sites-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="0284f-324">toolearn more about Azure App Service deployment visit [Deploy your app tooAzure App Service](../app-service-web/web-sites-deploy.md)</span></span>

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
