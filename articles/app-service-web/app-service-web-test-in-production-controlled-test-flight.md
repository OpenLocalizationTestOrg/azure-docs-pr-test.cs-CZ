---
title: "nasazení aaaFlighting (testování verze beta) v Azure App Service"
description: "Zjistěte, jak tooflight nové funkce v aplikaci nebo beta otestovat vaše aktualizace v tomto kurzu začátku do konce. Spojuje služby App Service funkcí, jako je nepřetržitá publikování, sloty, směrování provozu a integrace Application Insights."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 17953c51-38f8-442d-bb0b-f69c1542f0e9
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2016
ms.author: cephalin
ms.openlocfilehash: e83477b1fe46be09e5baa7bc2bd239b840b05cf7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a><span data-ttu-id="a567b-104">Flighting nasazení (testování verze beta) v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a567b-104">Flighting deployment (beta testing) in Azure App Service</span></span>
<span data-ttu-id="a567b-105">Tento kurz ukazuje, jak toodo *flighting nasazení* integrací hello různé možnosti [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) a [Azure Application Insights](/services/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="a567b-105">This tutorial shows you how toodo *flighting deployments* by integrating hello various capabilities of [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) and [Azure Application Insights](/services/application-insights/).</span></span>

<span data-ttu-id="a567b-106">*Flighting* je proces nasazení, která ověřuje nové funkce nebo s omezený počet zákazníků skutečné změnit, a hlavní testování v produkční scénář.</span><span class="sxs-lookup"><span data-stu-id="a567b-106">*Flighting* is a deployment process that validates a new feature or change with a limited number of real customers, and is a major testing in production scenario.</span></span> <span data-ttu-id="a567b-107">Je podobná toobeta testování a se někdy označuje jako "řízené testovací letu".</span><span class="sxs-lookup"><span data-stu-id="a567b-107">It is akin toobeta testing and is sometimes known as "controlled test flight".</span></span> <span data-ttu-id="a567b-108">Tuto metodu použijte, mnoho velkých podnicích se webová služba získat časná ověření na jejich aktualizace aplikací v jejich postup [agile vývoj](https://en.wikipedia.org/wiki/Agile_software_development).</span><span class="sxs-lookup"><span data-stu-id="a567b-108">Many large enterprises with a web presence use this approach to get early validation on their app updates in their practice of [agile development](https://en.wikipedia.org/wiki/Agile_software_development).</span></span> <span data-ttu-id="a567b-109">Aplikační služba Azure vám umožní toointegrate testování v produkčním prostředí s publikováním nepřetržité nebo Application Insights tooimplement hello stejné DevOps scénář.</span><span class="sxs-lookup"><span data-stu-id="a567b-109">Azure App Service enables you toointegrate test in production with continous publishing and Application Insights tooimplement hello same DevOps scenario.</span></span> <span data-ttu-id="a567b-110">Výhody tohoto přístupu patří:</span><span class="sxs-lookup"><span data-stu-id="a567b-110">Benefits of this approach include:</span></span>

* <span data-ttu-id="a567b-111">**Získání zpětné vazby skutečné *před* aktualizace jsou vydané tooproduction** -hello pouze jedinou lepší, než získají zpětnou vazbu při uvolnění je získání zpětné vazby před uvolněním.</span><span class="sxs-lookup"><span data-stu-id="a567b-111">**Gain real feedback *before* updates are released tooproduction** - hello only thing better than gaining feedback as soon as you release is gaining feedback before you release.</span></span> <span data-ttu-id="a567b-112">Aktualizace s přenosy reálný uživatel a chování můžete otestovat časná požadavky hello produktu životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="a567b-112">You can test updates with real user traffic and behaviors as early as you desire in hello product life cycle.</span></span>
* <span data-ttu-id="a567b-113">**Vylepšení [průběžné testy řízený vývoj (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)**  – díky integraci testování v produkčním prostředí se průběžnou integraci a instrumentace s Application Insights, ověření uživatele se již v rané fázi stane a automaticky v životního cyklu produktu.</span><span class="sxs-lookup"><span data-stu-id="a567b-113">**Enhance [continuous test-driven development (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)** - By integrating test in production with continuous integration and instrumentation with Application Insights, user validation happens early and automatically in your product life cycle.</span></span> <span data-ttu-id="a567b-114">To pomáhá zkrátit čas investice do spuštění manuálního testu.</span><span class="sxs-lookup"><span data-stu-id="a567b-114">This helps reduce time investments in manual test execution.</span></span>
* <span data-ttu-id="a567b-115">**Optimalizace pracovního postupu testování** -automatizací testování v produkčním prostředí s nepřetržité monitorování instrumentace můžete potenciálně provést hello cílů různé druhy testů v jednom procesu, jako například [integrace](https://en.wikipedia.org/wiki/Integration_testing), [regrese](https://en.wikipedia.org/wiki/Regression_testing), [použitelnost](https://en.wikipedia.org/wiki/Usability_testing), usnadnění, lokalizace, [výkonu](https://en.wikipedia.org/wiki/Software_performance_testing), [zabezpečení](https://en.wikipedia.org/wiki/Security_testing), a [ přijetí](https://en.wikipedia.org/wiki/Acceptance_testing).</span><span class="sxs-lookup"><span data-stu-id="a567b-115">**Optimize test workflow** - By automating test in production with continuous monitoring instrumentation, you can potentially accomplish hello goals of various kinds of tests in a single process, such as [integration](https://en.wikipedia.org/wiki/Integration_testing), [regression](https://en.wikipedia.org/wiki/Regression_testing), [usability](https://en.wikipedia.org/wiki/Usability_testing), accessibility, localization, [performance](https://en.wikipedia.org/wiki/Software_performance_testing), [security](https://en.wikipedia.org/wiki/Security_testing), and [acceptance](https://en.wikipedia.org/wiki/Acceptance_testing).</span></span>

<span data-ttu-id="a567b-116">Flighting nasazení není jenom o směrování provoz za provozu.</span><span class="sxs-lookup"><span data-stu-id="a567b-116">A flighting deployment is not just about routing live traffic.</span></span> <span data-ttu-id="a567b-117">Při takovémto nasazení budete chtít toogain přehledy co nejrychleji, ať to neočekávané chyby, snížení výkonu, problémy uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a567b-117">In such a deployment you want toogain insight as quickly as possible, whether it be an unexpected bug, performance degradation, user experience issues.</span></span> <span data-ttu-id="a567b-118">Mějte na paměti, kterou chcete pracovat se skutečnou zákazníků.</span><span class="sxs-lookup"><span data-stu-id="a567b-118">Remember, you are dealing with real customers.</span></span> <span data-ttu-id="a567b-119">Ano toodo ho práva, ujistěte se, že jste nastavili tak vaše flighting toogather nasazení všechna data hello budete potřebovat v pořadí toomake informované rozhodnutí pro další krok.</span><span class="sxs-lookup"><span data-stu-id="a567b-119">So toodo it right, you must make sure that you have set up your flighting deployment toogather all hello data you need in order toomake an informed decision for your next step.</span></span> <span data-ttu-id="a567b-120">Tento kurz ukazuje, jak toocollect dat pomocí Application Insights, ale můžete použít New Relic ani jiné technologie, které vyhovuje vašemu scénáři.</span><span class="sxs-lookup"><span data-stu-id="a567b-120">This tutorial shows you how toocollect data with Application Insights, but you can use New Relic or other technologies that suits your scenario.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="a567b-121">Co provedete</span><span class="sxs-lookup"><span data-stu-id="a567b-121">What you will do</span></span>
<span data-ttu-id="a567b-122">V tomto kurzu se dozvíte, jak toobring hello následující scénáře společně tootest aplikaci aplikační služby v produkčním prostředí:</span><span class="sxs-lookup"><span data-stu-id="a567b-122">In this tutorial, you will learn how toobring hello following scenarios together tootest your App Service app in production:</span></span>

* <span data-ttu-id="a567b-123">[Směrování provozu produkční](app-service-web-test-in-production-get-start.md) tooyour beta verze aplikace</span><span class="sxs-lookup"><span data-stu-id="a567b-123">[Route production traffic](app-service-web-test-in-production-get-start.md) tooyour beta app</span></span>
* <span data-ttu-id="a567b-124">[Instrumentace aplikace](../application-insights/app-insights-web-track-usage.md) tooobtain užitečné metriky</span><span class="sxs-lookup"><span data-stu-id="a567b-124">[Instrument your app](../application-insights/app-insights-web-track-usage.md) tooobtain useful metrics</span></span>
* <span data-ttu-id="a567b-125">Nepřetržitě nasazení vaší aplikace beta a sledovat metriky za provozu aplikace</span><span class="sxs-lookup"><span data-stu-id="a567b-125">Continuously deploy your beta app and track live app metrics</span></span>
* <span data-ttu-id="a567b-126">Metriky porovnání mezi hello produkční aplikace a hello beta aplikace toosee jak kód změní převede tooresults</span><span class="sxs-lookup"><span data-stu-id="a567b-126">Compare metrics between hello production app and hello beta app toosee how code changes translate tooresults</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="a567b-127">Co budete potřebovat</span><span class="sxs-lookup"><span data-stu-id="a567b-127">What you will need</span></span>
* <span data-ttu-id="a567b-128">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="a567b-128">An Azure account</span></span>
* <span data-ttu-id="a567b-129">A [Githubu](https://github.com/) účtu</span><span class="sxs-lookup"><span data-stu-id="a567b-129">A [GitHub](https://github.com/) account</span></span>
* <span data-ttu-id="a567b-130">Visual Studio 2015 - si můžete stáhnout hello [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="a567b-130">Visual Studio 2015 - you can download hello [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).</span></span>
* <span data-ttu-id="a567b-131">Git prostředí (nainstalované s [GitHub pro Windows](https://windows.github.com/)) – díky tomu můžete toorun obou hello Git a prostředí PowerShell příkazy v hello stejné relace</span><span class="sxs-lookup"><span data-stu-id="a567b-131">Git Shell (installed with [GitHub for Windows](https://windows.github.com/)) - this enables you toorun both hello Git and PowerShell commands in hello same session</span></span>
* <span data-ttu-id="a567b-132">Nejnovější [prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits</span><span class="sxs-lookup"><span data-stu-id="a567b-132">Latest [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits</span></span>
* <span data-ttu-id="a567b-133">Základní znalosti o hello následující:</span><span class="sxs-lookup"><span data-stu-id="a567b-133">Basic understanding of hello following:</span></span>
  * <span data-ttu-id="a567b-134">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) nasazení šablony (viz [nasazení komplexní aplikace předvídatelné v Azure](app-service-deploy-complex-application-predictably.md))</span><span class="sxs-lookup"><span data-stu-id="a567b-134">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) template deployment (see [Deploy a complex application predictably in Azure](app-service-deploy-complex-application-predictably.md))</span></span>
  * [<span data-ttu-id="a567b-135">Git</span><span class="sxs-lookup"><span data-stu-id="a567b-135">Git</span></span>](http://git-scm.com/documentation)
  * [<span data-ttu-id="a567b-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a567b-136">PowerShell</span></span>](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> <span data-ttu-id="a567b-137">Je třeba účtu Azure toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="a567b-137">You need an Azure account toocomplete this tutorial:</span></span>
>
> * <span data-ttu-id="a567b-138">Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/) -získáte kredity, můžete použít tootry na placené služby Azure a i po jejich použití můžete ponechat hello účet a používat bezplatné služby Azure, jako jsou webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-138">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/) - You get credits you can use tootry out paid Azure services, and even after they're used up you can keep hello account and use free Azure services, such as Web Apps.</span></span>
> * <span data-ttu-id="a567b-139">Můžete [aktivovat výhody předplatitele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -si Visual Studio předplatné vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="a567b-139">You can [activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) - Your Visual Studio subscription gives you credits every month that you can use for paid Azure services.</span></span>
>
> <span data-ttu-id="a567b-140">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="a567b-140">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="a567b-141">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="a567b-141">No credit cards required; no commitments.</span></span>
>
>

## <a name="set-up-your-production-web-app"></a><span data-ttu-id="a567b-142">Nastavení webové aplikace produkční</span><span class="sxs-lookup"><span data-stu-id="a567b-142">Set up your production web app</span></span>
> [!NOTE]
> <span data-ttu-id="a567b-143">skript Hello použili v tomto kurzu bude automaticky nakonfigurovat nepřetržité publikování z úložiště Githubu.</span><span class="sxs-lookup"><span data-stu-id="a567b-143">hello script used in this tutorial will automatically configure continuous publishing from your GitHub repository.</span></span> <span data-ttu-id="a567b-144">To vyžaduje, aby vaše přihlašovací údaje Githubu jsou již uloženy v Azure, jinak hello skripty nasazení selže při pokusu o nastavení ovládacího prvku tooconfigure zdroje pro hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-144">This requires that your GitHub credentials are already stored in Azure, otherwise hello scripted deployment will fail when attempting tooconfigure source control settings for hello web apps.</span></span>
>
> <span data-ttu-id="a567b-145">toostore vaše Githubu přihlašovací údaje v Azure, vytvořit webovou aplikaci v hello [portálu Azure](https://portal.azure.com/) a [konfigurace nasazení Githubu](app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="a567b-145">toostore your GitHub credentials in Azure, create a web app in hello [Azure Portal](https://portal.azure.com/) and [configure GitHub deployment](app-service-continuous-deployment.md).</span></span> <span data-ttu-id="a567b-146">Potřebujete jenom toodo tomto jednou.</span><span class="sxs-lookup"><span data-stu-id="a567b-146">You only need toodo this once.</span></span>
>
>

<span data-ttu-id="a567b-147">V případě typické DevOps máte aplikaci, která běží v Azure za provozu a chcete toomake tooit změny prostřednictvím průběžné publikování.</span><span class="sxs-lookup"><span data-stu-id="a567b-147">In a typical DevOps scenario, you have an application that’s running live in Azure, and you want toomake changes tooit through continuous publishing.</span></span> <span data-ttu-id="a567b-148">V tomto scénáři nasadíte tooproduction šablonu, která mají vývoji a testování.</span><span class="sxs-lookup"><span data-stu-id="a567b-148">In this scenario, you will deploy tooproduction a template that you have developed and tested.</span></span>

1. <span data-ttu-id="a567b-149">Vytvoření vlastního rozvětvení hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) úložiště.</span><span class="sxs-lookup"><span data-stu-id="a567b-149">Create your own fork of hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) repository.</span></span> <span data-ttu-id="a567b-150">Informace o vytváření vašeho rozvětvení najdete v tématu [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/).</span><span class="sxs-lookup"><span data-stu-id="a567b-150">For information on creating your fork, see [Fork a Repo](https://help.github.com/articles/fork-a-repo/).</span></span> <span data-ttu-id="a567b-151">Po vytvoření vaší rozvětvení, zobrazí se v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a567b-151">Once your fork is created, you can see it in your browser.</span></span>

   ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. <span data-ttu-id="a567b-152">Otevřete relaci prostředí Git.</span><span class="sxs-lookup"><span data-stu-id="a567b-152">Open a Git Shell session.</span></span> <span data-ttu-id="a567b-153">Pokud ještě nemáte Git prostředí, nainstalujte [GitHub pro Windows](https://windows.github.com/) nyní.</span><span class="sxs-lookup"><span data-stu-id="a567b-153">If you don't have Git Shell yet, install [GitHub for Windows](https://windows.github.com/) now.</span></span>
3. <span data-ttu-id="a567b-154">Vytvořte místní vaší rozvětvení spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a567b-154">Create a local clone of your fork by executing hello following command:</span></span>

     <span data-ttu-id="a567b-155">https://github.com/<your_fork>/ToDoApp.git klon Git</span><span class="sxs-lookup"><span data-stu-id="a567b-155">git clone https://github.com/<your_fork>/ToDoApp.git</span></span>
4. <span data-ttu-id="a567b-156">Až budete mít vaše místní klonování, přejděte příliš*&lt;repository_root >*\ARMTemplates a spusťte hello deploy.ps1 skript s jedinečnou příponu, jak je uvedené níže:</span><span class="sxs-lookup"><span data-stu-id="a567b-156">Once you have your local clone, navigate too*&lt;repository_root>*\ARMTemplates, and run hello deploy.ps1 script with a unique suffix, as shown below:</span></span>

     <span data-ttu-id="a567b-157">.\deploy.ps1 – RepoUrl https://github.com/<your_fork>/todoapp.git - ResourceGroupSuffix < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="a567b-157">.\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix></span></span>
5. <span data-ttu-id="a567b-158">Po zobrazení výzvy zadejte hello požadované uživatelské jméno a heslo pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="a567b-158">When prompted, type in hello desired username and password for database access.</span></span> <span data-ttu-id="a567b-159">Mějte na paměti, protože je budete potřebovat toospecify přihlašovací údaje databáze je znovu při aktualizaci hello skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a567b-159">Remember your database credentials because you will need toospecify them again when updating hello resource group.</span></span>

   <span data-ttu-id="a567b-160">Měli byste vidět hello zřizování průběh různých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="a567b-160">You should see hello provisioning progress of various Azure resources.</span></span> <span data-ttu-id="a567b-161">Po dokončení nasazení hello skript spuštění aplikace hello v prohlížeči hello a získáte popisný zvukový signál.</span><span class="sxs-lookup"><span data-stu-id="a567b-161">When deployment completes, hello script will launch hello application in hello browser and give you a friendly beep.</span></span>
   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)
6. <span data-ttu-id="a567b-162">Zpět v relaci prostředí Git, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="a567b-162">Back in your Git Shell session, run:</span></span>

     <span data-ttu-id="a567b-163">. \swap – název ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="a567b-163">.\swap –Name ToDoApp<your_suffix></span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)
7. <span data-ttu-id="a567b-164">Po dokončení skriptu hello vraťte toobrowse toohello front-end adresa (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) toosee hello aplikace běžící v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a567b-164">When hello script finishes, go back toobrowse toohello frontend’s address (http://ToDoApp*&lt;your_suffix>*.azurewebsites.net/) toosee hello application running in production.</span></span>
8. <span data-ttu-id="a567b-165">Přihlaste se k hello [portálu Azure](https://portal.azure.com/) a prohlédněte si co je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="a567b-165">Log into hello [Azure Portal](https://portal.azure.com/) and take a look at what’s created.</span></span>

   <span data-ttu-id="a567b-166">Musí být schopný toosee dva webové aplikace v hello stejnou skupinu prostředků, s hello `Api` přípony v názvu hello.</span><span class="sxs-lookup"><span data-stu-id="a567b-166">You should be able toosee two web apps in hello same resource group, one with hello `Api` suffix in hello name.</span></span> <span data-ttu-id="a567b-167">Pokud se podíváte na zobrazení skupiny prostředků hello, zobrazí se také hello SQL Database a serveru, hello plán služby App Service a hello přípravné sloty pro hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-167">If you look at hello resource group view, you will also see hello SQL Database and server, hello App Service plan, and hello staging slots for hello web apps.</span></span> <span data-ttu-id="a567b-168">Procházet hello různých prostředků a porovnejte je s  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json toosee, jak jsou nakonfigurované v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="a567b-168">Browse through hello different resources and compare them with *&lt;repository_root>*\ARMTemplates\ProdAndStage.json toosee how they are configured in hello template.</span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

<span data-ttu-id="a567b-169">Jste nastavili hello produkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a567b-169">You have set up hello production app.</span></span>  <span data-ttu-id="a567b-170">Nyní Pojďme Představte si, že nedostanete zpětnou vazbu, že je špatná aplikace hello použitelnost.</span><span class="sxs-lookup"><span data-stu-id="a567b-170">Now, let's imagine that you receive feedback that usability is poor for hello app.</span></span> <span data-ttu-id="a567b-171">Proto rozhodnete tooinvestigate.</span><span class="sxs-lookup"><span data-stu-id="a567b-171">So you decide tooinvestigate.</span></span> <span data-ttu-id="a567b-172">Budete tooinstrument toogive vaší aplikaci můžete zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="a567b-172">You're going tooinstrument your app toogive you feedback.</span></span>

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a><span data-ttu-id="a567b-173">Prozkoumat: Instrumentace vaší klientské aplikace pro monitorování/metriky</span><span class="sxs-lookup"><span data-stu-id="a567b-173">Investigate: Instrument your client app for monitoring/metrics</span></span>
1. <span data-ttu-id="a567b-174">Otevřete  *&lt;repository_root >*\src\MultiChannelToDo.sln v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a567b-174">Open *&lt;repository_root>*\src\MultiChannelToDo.sln in Visual Studio.</span></span>
2. <span data-ttu-id="a567b-175">Obnovení všech balíčků Nuget kliknutím pravým tlačítkem na řešení > **spravovat balíčky NuGet pro řešení** > **obnovení**.</span><span class="sxs-lookup"><span data-stu-id="a567b-175">Restore all Nuget packages by right-clicking solution > **Manage NuGet Packages for Solution** > **Restore**.</span></span>
3. <span data-ttu-id="a567b-176">Klikněte pravým tlačítkem na **MultiChannelToDo.Web** > **přidat Telemetrii Application Insights** > **konfigurovat nastavení** > změnit skupinu prostředků tooToDoApp*&lt;your_suffix >* > **přidat službu Application Insights tooProject**.</span><span class="sxs-lookup"><span data-stu-id="a567b-176">Right-click **MultiChannelToDo.Web** > **Add Application Insights Telemetry** > **Configure Settings** > Change resource group tooToDoApp*&lt;your_suffix>* > **Add Application Insights tooProject**.</span></span>
4. <span data-ttu-id="a567b-177">V hello portálu Azure, otevřete okno hello hello **MultiChannelToDo.Web** aplikace přehled prostředků.</span><span class="sxs-lookup"><span data-stu-id="a567b-177">In hello Azure Portal, open hello blade for hello **MultiChannelToDo.Web** Application Insight resource.</span></span> <span data-ttu-id="a567b-178">Potom v hello **stavu aplikací** součástí, klikněte na tlačítko **zjistěte, jak toocollect prohlížeče stránku načíst data** > zkopírujte kód.</span><span class="sxs-lookup"><span data-stu-id="a567b-178">Then in hello **Application health** part, click **Learn how toocollect browser page load data** > copy code.</span></span>
5. <span data-ttu-id="a567b-179">Přidat hello zkopírovat kód instrumentace JS příliš*&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml těsně před uzavírací hello `<heading>` značky.</span><span class="sxs-lookup"><span data-stu-id="a567b-179">Add hello copied JS instrumentation code too*&lt;repository_root>*\src\MultiChannelToDo.Web\app\Index.cshtml, just before hello closing `<heading>` tag.</span></span> <span data-ttu-id="a567b-180">Měl by obsahovat klíč instrumentace jedinečný hello prostředku přehled aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-180">It should contain hello unique instrumentation key of your Application Insight resource.</span></span>

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>
6. <span data-ttu-id="a567b-181">Odeslat vlastní události tooApplication Insights pro kliknutí myší přidáním následující kód toohello dolní části textu hello:</span><span class="sxs-lookup"><span data-stu-id="a567b-181">Send custom events tooApplication Insights for mouse clicks by adding hello following code toohello bottom of body:</span></span>

       <script>
           $(document.body).find("*").click(function(event) {

               appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
           });
       </script>

   <span data-ttu-id="a567b-182">Tento fragment kódu JavaScript odešle vlastní události tooApplication Statistika pokaždé, když uživatel klikne kdekoli v hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-182">This JavaScript snippet sends a custom event tooApplication Insights every time a user clicks anywhere in hello web app.</span></span>
7. <span data-ttu-id="a567b-183">V prostředí Git potvrďte a odešlete rozvětvení tooyour vaše změny v Githubu.</span><span class="sxs-lookup"><span data-stu-id="a567b-183">In Git Shell, commit and push your changes tooyour fork in GitHub.</span></span> <span data-ttu-id="a567b-184">Potom počkejte, než klienti toorefresh prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="a567b-184">Then, wait for clients toorefresh browser.</span></span>

       git add -A :/
       git commit -m "add AI configuration for client app"
       git push origin master
8. <span data-ttu-id="a567b-185">Swap hello nasadit tooproduction změny aplikace:</span><span class="sxs-lookup"><span data-stu-id="a567b-185">Swap hello deployed app changes tooproduction:</span></span>

     <span data-ttu-id="a567b-186">. \swap – název ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="a567b-186">.\swap –Name ToDoApp<your_suffix></span></span>
9. <span data-ttu-id="a567b-187">Vyhledejte prostředek Application Insights toohello, který jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="a567b-187">Browse toohello Application Insights resource that you configured.</span></span> <span data-ttu-id="a567b-188">Klikněte na tlačítko Vlastní události.</span><span class="sxs-lookup"><span data-stu-id="a567b-188">Click Custom events.</span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

   <span data-ttu-id="a567b-189">Pokud nevidíte metriky pro vlastní události, počkejte několik minut a klikněte na tlačítko **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="a567b-189">If you don't see metrics for custom events, wait a few minutes and click **Refresh**.</span></span>

<span data-ttu-id="a567b-190">Předpokládejme, že se zobrazí graf jako níže:</span><span class="sxs-lookup"><span data-stu-id="a567b-190">Suppose you see a chart like below:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

<span data-ttu-id="a567b-191">A mřížky hello událostí pod ním:</span><span class="sxs-lookup"><span data-stu-id="a567b-191">And hello event grid below it:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

<span data-ttu-id="a567b-192">Podle kódu aplikace ToDoApp tooyour, hello **tlačítko** události odpovídá tlačítko Odeslat toohello a hello **vstup** události odpovídá toohello textové pole.</span><span class="sxs-lookup"><span data-stu-id="a567b-192">According tooyour ToDoApp application code, hello **BUTTON** event corresponds toohello submit button, and hello **INPUT** event corresponds toohello textbox.</span></span> <span data-ttu-id="a567b-193">Zatím věcí smysl.</span><span class="sxs-lookup"><span data-stu-id="a567b-193">So far, things make sense.</span></span> <span data-ttu-id="a567b-194">Ale to vypadá, je dobré množství kliknutí a jen několik kliknutí položkami seznamu úkolů hello (hello **LI** událostí).</span><span class="sxs-lookup"><span data-stu-id="a567b-194">However, it looks like there's a good amount of clicks and very few clicks on hello to-do items (hello **LI** events).</span></span>

<span data-ttu-id="a567b-195">Na základě toho je formuláře vaše předpoklad, že někteří uživatelé nerozumíte kterou část hello uživatelského rozhraní je můžete kliknout a je to proto kurzor hello je navržen pro výběr textu ho na hello položky seznamu a jejich ikony vždy, když se.</span><span class="sxs-lookup"><span data-stu-id="a567b-195">Based on this, you form your hypothesis that some users are confused which part of hello UI is clickable and it is because hello cursor is styled for text selection when it hovers on hello list items and their icons.</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

<span data-ttu-id="a567b-196">To může být contrived příklad.</span><span class="sxs-lookup"><span data-stu-id="a567b-196">This might be a contrived example.</span></span> <span data-ttu-id="a567b-197">Nicméně jste probíhající toomake zlepšování tooyour aplikace a pak proveďte flighting nasazení tooget použitelnosti odezva od zákazníků za provozu.</span><span class="sxs-lookup"><span data-stu-id="a567b-197">Nevertheless, you're going toomake an improvement tooyour app, and then perform a flighting deployment tooget usability feedback from live customers.</span></span>

### <a name="instrument-your-server-app-for-monitoringmetrics"></a><span data-ttu-id="a567b-198">Aplikace serveru pro monitorování/metriky instrumentace</span><span class="sxs-lookup"><span data-stu-id="a567b-198">Instrument your server app for monitoring/metrics</span></span>
<span data-ttu-id="a567b-199">To je tangens, protože hello scénář ukázáno v tomto kurzu pouze zabývá hello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a567b-199">This is a tangent since hello scenario demonstrated in this tutorial only deals with hello client app.</span></span> <span data-ttu-id="a567b-200">Ale pro úplnost nastavíte hello serverové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-200">However, for completeness you will set up hello server-side app.</span></span>

1. <span data-ttu-id="a567b-201">Klikněte pravým tlačítkem na **MultiChannelToDo** > **přidat Telemetrii Application Insights** > **konfigurovat nastavení** > změnit skupinu prostředků tooToDoApp*&lt;your_suffix >* > **přidat službu Application Insights tooProject**.</span><span class="sxs-lookup"><span data-stu-id="a567b-201">Right-click **MultiChannelToDo** > **Add Application Insights Telemetry** > **Configure Settings** > Change resource group tooToDoApp*&lt;your_suffix>* > **Add Application Insights tooProject**.</span></span>
2. <span data-ttu-id="a567b-202">V prostředí Git potvrďte a odešlete rozvětvení tooyour vaše změny v Githubu.</span><span class="sxs-lookup"><span data-stu-id="a567b-202">In Git Shell, commit and push your changes tooyour fork in GitHub.</span></span> <span data-ttu-id="a567b-203">Potom počkejte, než klienti toorefresh prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="a567b-203">Then, wait for clients toorefresh browser.</span></span>

       git add -A :/
       git commit -m "add AI configuration for server app"
       git push origin master
3. <span data-ttu-id="a567b-204">Swap hello nasadit tooproduction změny aplikace:</span><span class="sxs-lookup"><span data-stu-id="a567b-204">Swap hello deployed app changes tooproduction:</span></span>

     <span data-ttu-id="a567b-205">. \swap – název ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="a567b-205">.\swap –Name ToDoApp<your_suffix></span></span>

<span data-ttu-id="a567b-206">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="a567b-206">That's it!</span></span>

## <a name="investigate-add-slot-specific-tags-tooyour-client-app-metrics"></a><span data-ttu-id="a567b-207">Prozkoumat: Přidáte značky specifické pro slot tooyour klienta aplikace metriky</span><span class="sxs-lookup"><span data-stu-id="a567b-207">Investigate: Add slot-specific tags tooyour client app metrics</span></span>
<span data-ttu-id="a567b-208">V této části budete konfigurovat hello jiné nasazení sloty toosend specifické pro slot telemetrie toohello stejný prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a567b-208">In this section, you will configure hello different deployment slots toosend slot-specific telemetry toohello same Application Insights resource.</span></span> <span data-ttu-id="a567b-209">Tímto způsobem můžete porovnat telemetrická data mezi přenosy dat z různých sloty (nasazení prostředí) tooeasily najdete v části hello účinku změny aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-209">This way, you can compare telemetry data between traffic from different slots (deployment environments) tooeasily see hello effect of your app changes.</span></span> <span data-ttu-id="a567b-210">Na hello stejný čas, abyste mohli pokračovat toomonitor produkční aplikace podle potřeby můžete oddělit provoz produkční hello z hello rest.</span><span class="sxs-lookup"><span data-stu-id="a567b-210">At hello same time, you can separate hello production traffic from hello rest so you can continue toomonitor your production app as needed.</span></span>

<span data-ttu-id="a567b-211">Vzhledem k tomu, že shromažďování dat na chování klienta bude [přidat telemetrie inicializátoru tooyour kódu jazyka JavaScript](../application-insights/app-insights-api-filtering-sampling.md) v index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="a567b-211">Since you're gathering data on client behavior, you will [add a telemetry initializer tooyour JavaScript code](../application-insights/app-insights-api-filtering-sampling.md) in index.cshtml.</span></span> <span data-ttu-id="a567b-212">Pokud chcete výkonu na straně serveru tootest, například můžete provést také podobně jako v kódu serveru (viz [Application Insights API pro vlastní události a metriky](../application-insights/app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="a567b-212">If you want tootest server-side performance, for example, you can also do similarly in your server code (see [Application Insights API for custom events and metrics](../application-insights/app-insights-api-custom-events-metrics.md).</span></span>

1. <span data-ttu-id="a567b-213">Nejprve přidejte hello kód bewteen hello dva `//` komentáře níže v bloku hello jazyka JavaScript, zda jste přidali toohello `<heading>` dříve značky.</span><span class="sxs-lookup"><span data-stu-id="a567b-213">First, add hello code bewteen hello two `//` comments below in hello JavaScript block that you added toohello `<heading>` tag earlier.</span></span>

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    <span data-ttu-id="a567b-214">Tento kód inicializátoru způsobí, že hello `appInsights` tooadd objekt hello vlastní vlastnost s názvem `Environment` tooevery část telemetrie odešle.</span><span class="sxs-lookup"><span data-stu-id="a567b-214">This initializer code causes hello `appInsights` object tooadd hello a custom property called `Environment` tooevery piece of telemetry it sends.</span></span>
2. <span data-ttu-id="a567b-215">Dál přidejte tuto vlastní vlastnost jako [pozice nastavení](web-sites-staged-publishing.md#AboutConfiguration) pro webovou aplikaci v Azure.</span><span class="sxs-lookup"><span data-stu-id="a567b-215">Next, add this custom property as a [slot setting](web-sites-staged-publishing.md#AboutConfiguration) for your web app in Azure.</span></span> <span data-ttu-id="a567b-216">toodo se spuštění hello následující příkazy v relaci prostředí Git.</span><span class="sxs-lookup"><span data-stu-id="a567b-216">toodo this, run hello following commands in your Git Shell session.</span></span>

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    <span data-ttu-id="a567b-217">Hello Web.config v projektu již definuje hello `environment` nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-217">hello Web.config in your project already defines hello `environment` app setting.</span></span> <span data-ttu-id="a567b-218">Při tomto nastavení se při testování hello aplikaci místně, vaše metriky budou označené `VS Debugger`.</span><span class="sxs-lookup"><span data-stu-id="a567b-218">With this setting, when you test hello app locally, your metrics will be tagged with `VS Debugger`.</span></span> <span data-ttu-id="a567b-219">Ale po stisknutí tooAzure vaše změny, Azure bude nalezne a použije hello `environment` aplikace nastavení v konfiguraci webové aplikace hello místo a vaše metriky budou označené `Production`.</span><span class="sxs-lookup"><span data-stu-id="a567b-219">However, when you push your changes tooAzure, Azure will find and use hello `environment` app setting in hello web app's configuration instead, and your metrics will be tagged with `Production`.</span></span>
3. <span data-ttu-id="a567b-220">Potvrzení a váš kód změny tooyour rozvětvení push na Githubu a potom počkejte, než pro uživatele toouse hello novou aplikaci (třeba toorefresh hello prohlížeč).</span><span class="sxs-lookup"><span data-stu-id="a567b-220">Commit and push your code changes tooyour fork on GitHub, and then wait for your users toouse hello new app (need toorefresh hello browser).</span></span> <span data-ttu-id="a567b-221">Trvá asi 15 minut pro novou vlastnost tooshow hello v Application Insights `MultiChannelToDo.Web` prostředků.</span><span class="sxs-lookup"><span data-stu-id="a567b-221">It takes about 15 minutes for hello new property tooshow up in your Application Insights `MultiChannelToDo.Web` resource.</span></span>

        git add -A :/
        git commit -m "add environment property tooAI events for client app"
        git push origin master
4. <span data-ttu-id="a567b-222">Nyní přejděte toohello **vlastní události** okno znovu a filtr hello metriky na `Environment=Production`.</span><span class="sxs-lookup"><span data-stu-id="a567b-222">Now, go toohello **Custom events** blade again and filter hello metrics on `Environment=Production`.</span></span> <span data-ttu-id="a567b-223">Nyní byste měli mít toosee všechny hello nové události vlastní v produkčním prostředí hello pozici s Tento filtr.</span><span class="sxs-lookup"><span data-stu-id="a567b-223">You should now be able toosee all hello new custom events in hello production slot with this filter.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)
5. <span data-ttu-id="a567b-224">Klikněte na tlačítko hello **Oblíbené** jako tlačítko toosave hello aktuální Průzkumníku metrik nastavení toosomething **vlastní události: produkční**.</span><span class="sxs-lookup"><span data-stu-id="a567b-224">Click hello **Favorites** button toosave hello current Metrics Explorer settings toosomething like **Custom events: Production**.</span></span> <span data-ttu-id="a567b-225">Můžete snadno přepnout mezi tato zobrazení a zobrazení slotu nasazení později.</span><span class="sxs-lookup"><span data-stu-id="a567b-225">You can easily switch between this view and a deployment slot view later.</span></span>

   > [!TIP]
   > <span data-ttu-id="a567b-226">Ještě účinnější analýzy, vezměte v úvahu [integraci prostředku Application Insights do Power BI](../application-insights/app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="a567b-226">For even more powerful analytics, consider [integrating your Application Insights resource with Power BI](../application-insights/app-insights-export-power-bi.md).</span></span>
   >
   >

### <a name="add-slot-specific-tags-tooyour-server-app-metrics"></a><span data-ttu-id="a567b-227">Přidání značky specifické pro slot tooyour serveru aplikace metriky</span><span class="sxs-lookup"><span data-stu-id="a567b-227">Add slot-specific tags tooyour server app metrics</span></span>
<span data-ttu-id="a567b-228">Znovu pro úplnost nastavíte hello serverové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-228">Again, for completeness you will set up hello server-side app.</span></span> <span data-ttu-id="a567b-229">Na rozdíl od hello klientskou aplikaci, která je instrumentovány v jazyce JavaScript je značky slotu specifické pro aplikace server hello instrumentována kódem .NET.</span><span class="sxs-lookup"><span data-stu-id="a567b-229">Unlike hello client app which is instrumented in JavaScript, slot-specific tags for hello server app is instrumented with .NET code.</span></span>

1. <span data-ttu-id="a567b-230">Otevřete  *&lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="a567b-230">Open *&lt;repository_root>*\src\MultiChannelToDo\Global.asax.cs.</span></span> <span data-ttu-id="a567b-231">Přidejte hello blok kódu níže, těsně před hello zavřením obor názvů složených závorek.</span><span class="sxs-lookup"><span data-stu-id="a567b-231">Add hello code block below, just before hello closing namespace curly brace.</span></span>

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }
2. <span data-ttu-id="a567b-232">Opravte chyby překladu názvů hello přidáním hello `using` příkazy níže toohello začátku hello souboru:</span><span class="sxs-lookup"><span data-stu-id="a567b-232">Correct hello name resolution errors by adding hello `using` statements below toohello beginning of hello file:</span></span>

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;
3. <span data-ttu-id="a567b-233">Přidat kód hello pod toohello začátku hello `Application_Start()` metoda:</span><span class="sxs-lookup"><span data-stu-id="a567b-233">Add hello code below toohello beginning of hello `Application_Start()` method:</span></span>

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());
4. <span data-ttu-id="a567b-234">Potvrzení a váš kód změny tooyour rozvětvení push na Githubu a potom počkejte, než pro uživatele toouse hello novou aplikaci (třeba toorefresh hello prohlížeč).</span><span class="sxs-lookup"><span data-stu-id="a567b-234">Commit and push your code changes tooyour fork on GitHub, and then wait for your users toouse hello new app (need toorefresh hello browser).</span></span> <span data-ttu-id="a567b-235">Trvá asi 15 minut pro novou vlastnost tooshow hello v Application Insights `MultiChannelToDo` prostředků.</span><span class="sxs-lookup"><span data-stu-id="a567b-235">It takes about 15 minutes for hello new property tooshow up in your Application Insights `MultiChannelToDo` resource.</span></span>

        git add -A :/
        git commit -m "add environment property tooAI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a><span data-ttu-id="a567b-236">Aktualizace: Nastavení větev beta</span><span class="sxs-lookup"><span data-stu-id="a567b-236">Update: Set up your beta branch</span></span>
1. <span data-ttu-id="a567b-237">Otevřete  *&lt;repository_root >*\ARMTemplates\ProdAndStagetest.json a najít hello `appsettings` prostředky (vyhledejte `"name": "appsettings"`).</span><span class="sxs-lookup"><span data-stu-id="a567b-237">Open *&lt;repository_root>*\ARMTemplates\ProdAndStagetest.json and find hello `appsettings` resources (search for `"name": "appsettings"`).</span></span> <span data-ttu-id="a567b-238">Existují 4 z nich, jeden pro každou slot.</span><span class="sxs-lookup"><span data-stu-id="a567b-238">There are 4 of them, one for each slot.</span></span>
2. <span data-ttu-id="a567b-239">Pro každou `appsettings` prostředků, přidejte `"environment": "[parameters('slotName')]"` aplikace nastavení toohello konec hello `properties` pole.</span><span class="sxs-lookup"><span data-stu-id="a567b-239">For each `appsettings` resource, add an  `"environment": "[parameters('slotName')]"` app setting toohello end of hello `properties` array.</span></span> <span data-ttu-id="a567b-240">Nezapomeňte tooend hello předchozí řádek oddělujte čárkami.</span><span class="sxs-lookup"><span data-stu-id="a567b-240">Don't forget tooend hello previous line with a comma.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)

    <span data-ttu-id="a567b-241">Jste právě přidali hello `environment` aplikace nastavení tooall hello sloty v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="a567b-241">You have just added hello `environment` app setting tooall hello slots in hello template.</span></span>
3. <span data-ttu-id="a567b-242">V hello stejného souboru, najde hello `slotconfignames` prostředky (vyhledejte `"name": "slotconfignames"`).</span><span class="sxs-lookup"><span data-stu-id="a567b-242">In hello same file, find hello `slotconfignames` resources (search for `"name": "slotconfignames"`).</span></span> <span data-ttu-id="a567b-243">Existují 2 z nich, jeden pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a567b-243">There are 2 of them, one for each app.</span></span>
4. <span data-ttu-id="a567b-244">Pro každou `slotconfignames` prostředků, přidat `"environment"` toohello konec hello `appSettingNames` pole.</span><span class="sxs-lookup"><span data-stu-id="a567b-244">For each `slotconfignames` resource, add `"environment"` toohello end of hello `appSettingNames` array.</span></span> <span data-ttu-id="a567b-245">Nezapomeňte tooend hello předchozí řádek oddělujte čárkami.</span><span class="sxs-lookup"><span data-stu-id="a567b-245">Don't forget tooend hello previous line with a comma.</span></span>

    <span data-ttu-id="a567b-246">Právě jste provedli hello `environment` slot nasazení příslušné aplikace nastavení Flash disk tooits pro obě aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-246">You have just made hello `environment` app setting stick tooits respective deployment slot for both apps.</span></span>  
5. <span data-ttu-id="a567b-247">V relaci prostředí Git hello spusťte následující příkazy pomocí hello stejnou příponu skupiny prostředků, kterou jste používali před.</span><span class="sxs-lookup"><span data-stu-id="a567b-247">In your Git Shell session, run hello following commands with hello same resource group suffix that you used before.</span></span>

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta
6. <span data-ttu-id="a567b-248">Po zobrazení výzvy zadejte hello stejné přihlašovací údaje databáze SQL jako předtím.</span><span class="sxs-lookup"><span data-stu-id="a567b-248">When prompted, specify hello same SQL database credentials as before.</span></span> <span data-ttu-id="a567b-249">Poté, jakmile se zobrazí dotaz tooupdate, hello skupinu prostředků, typ `Y`, pak `ENTER`.</span><span class="sxs-lookup"><span data-stu-id="a567b-249">Then, when asked tooupdate hello resource group, type `Y`, then `ENTER`.</span></span>

    <span data-ttu-id="a567b-250">Po dokončení skriptu hello, všechny prostředky ve skupině prostředků původní hello se zachovají, ale nový slot s názvem "beta" je vytvořen v ní s hello stejné konfigurace jako slotu "Přípravy" hello, který byl vytvořen v začátku hello.</span><span class="sxs-lookup"><span data-stu-id="a567b-250">Once hello script finishes, all your resources in hello original resource group are retained, but a new slot named "beta" is created in it with hello same configuration as hello "Staging" slot that was created in hello beginning.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a567b-251">Tato metoda vytvoření různých prostředí se liší od metody hello v [Agile software development službou Azure App Service](app-service-agile-software-development.md).</span><span class="sxs-lookup"><span data-stu-id="a567b-251">This method of creating different deployment environments is different from hello method in [Agile software development with Azure App Service](app-service-agile-software-development.md).</span></span> <span data-ttu-id="a567b-252">Zde vytvoříte nasazení prostředí s slotů nasazení, kde jako tam vytvořit nasazení prostředí ke skupinám prostředků.</span><span class="sxs-lookup"><span data-stu-id="a567b-252">Here, you create deployment environments with deployment slots, where as there you create deployment environments with resource groups.</span></span> <span data-ttu-id="a567b-253">Správa prostředí nasazení pomocí skupin prostředků vám umožní off-limits toodevelopers tookeep hello produkční prostředí, ale není snadno toodo testování v produkčním prostředí, které můžete provést snadno sloty.</span><span class="sxs-lookup"><span data-stu-id="a567b-253">Managing deployment environments with resource groups enables you tookeep hello production environment off-limits toodevelopers, but it's not easy toodo testing in production, which you can do easily with slots.</span></span>
   >
   >

<span data-ttu-id="a567b-254">Pokud chcete, můžete také vytvořit aplikaci alfa spuštěním</span><span class="sxs-lookup"><span data-stu-id="a567b-254">If you wish, you can also create an alpha app by running</span></span>

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

<span data-ttu-id="a567b-255">V tomto kurzu jste se právě zachovat pomocí beta verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-255">For this tutorial, you will just keep using your beta app.</span></span>

## <a name="update-push-your-updates-toohello-beta-app"></a><span data-ttu-id="a567b-256">Aktualizace: Aplikace aktualizace toohello beta Push</span><span class="sxs-lookup"><span data-stu-id="a567b-256">Update: Push your updates toohello beta app</span></span>
<span data-ttu-id="a567b-257">Zpět tooyour aplikace, které chcete tooimprove.</span><span class="sxs-lookup"><span data-stu-id="a567b-257">Back tooyour app that you want tooimprove.</span></span>

1. <span data-ttu-id="a567b-258">Ujistěte se, že nyní jste v větev beta</span><span class="sxs-lookup"><span data-stu-id="a567b-258">Make sure you're now in your beta branch</span></span>

        git checkout beta
2. <span data-ttu-id="a567b-259">V  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, najít hello `<li>` značky a přidejte hello `style="cursor:pointer"` atributu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="a567b-259">In *&lt;repository_root>*\src\MultiChannelToDo.Web\app\Index.cshtml, find hello `<li>` tag and add hello `style="cursor:pointer"` attribute, as shown below.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)
3. <span data-ttu-id="a567b-260">Potvrďte a odešlete tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a567b-260">commit and push tooAzure.</span></span>
4. <span data-ttu-id="a567b-261">Ověřte, že hello změny teď se ve slotu beta hello přechodem toohttp://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/.</span><span class="sxs-lookup"><span data-stu-id="a567b-261">Verify that hello change is now reflected in hello beta slot by navigating toohttp://todoapp*&lt;your_suffix>*-beta.azurewebsites.net/.</span></span> <span data-ttu-id="a567b-262">Pokud nevidíte hello změnit ještě aktualizujte prohlížeč tooget hello nového kódu jazyka javascript.</span><span class="sxs-lookup"><span data-stu-id="a567b-262">If you don't see hello change yet, refresh your browser tooget hello new javascript code.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

<span data-ttu-id="a567b-263">Teď, když máte změny spuštěna ve slotu hello beta, jste připravené tooperform flighting nasazení.</span><span class="sxs-lookup"><span data-stu-id="a567b-263">Now that you have your change running in hello beta slot, you are ready tooperform a flighting deployment.</span></span>

## <a name="validate-route-traffic-toohello-beta-app"></a><span data-ttu-id="a567b-264">Ověření: Směrování provozu toohello beta verze aplikace</span><span class="sxs-lookup"><span data-stu-id="a567b-264">Validate: Route traffic toohello beta app</span></span>
<span data-ttu-id="a567b-265">V této části bude směrovat provoz toohello beta verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-265">In this section, you will route traffic toohello beta app.</span></span> <span data-ttu-id="a567b-266">For sake of přehlednost ukázku budete tooroute podstatnou část tooit provoz hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="a567b-266">For sake of clarity of demonstration, you're going tooroute a significant portion of hello user traffic tooit.</span></span> <span data-ttu-id="a567b-267">Ve skutečnosti hello objem provozu, který má být, že tooroute bude záviset na konkrétní situaci.</span><span class="sxs-lookup"><span data-stu-id="a567b-267">In reality, hello amount of traffic you want tooroute will depend on your specific situation.</span></span> <span data-ttu-id="a567b-268">Například pokud váš webový server je ve velkém měřítku hello Microsoft.com, pak musíte méně než jeden procent celkového provozu v pořadí toogain užitečné data.</span><span class="sxs-lookup"><span data-stu-id="a567b-268">For example, if your site is at hello scale of microsoft.com, then you may need less than one percent of your total traffic in order toogain useful data.</span></span>

1. <span data-ttu-id="a567b-269">V relaci prostředí Git spusťte následující příkazy tooroute hello polovinu hello produkční provoz toohello beta slot:</span><span class="sxs-lookup"><span data-stu-id="a567b-269">In your Git Shell session, run hello following commands tooroute half of hello production traffic toohello beta slot:</span></span>

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

   <span data-ttu-id="a567b-270">Hello `ReroutePercentage=50` vlastnost určuje, že 50 % hello produkční přenosů bude adresa URL aplikace beta směrované toohello (určeného hello `ActionHostName` vlastnost).</span><span class="sxs-lookup"><span data-stu-id="a567b-270">hello `ReroutePercentage=50` property specifies that 50% of hello production traffic will be routed toohello beta app's URL (specified by hello `ActionHostName` property).</span></span>
2. <span data-ttu-id="a567b-271">Nyní přejděte toohttp://ToDoApp*&lt;your_suffix >*. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="a567b-271">Now navigate toohttp://ToDoApp*&lt;your_suffix>*.azurewebsites.net.</span></span> <span data-ttu-id="a567b-272">50 % hello přenosů by teď měly být přesměrovaného toohello beta slot.</span><span class="sxs-lookup"><span data-stu-id="a567b-272">50% of hello traffic should now be redirected toohello beta slot.</span></span>
3. <span data-ttu-id="a567b-273">V prostředku Application Insights, filtrovat podle prostředí metriky hello = "beta".</span><span class="sxs-lookup"><span data-stu-id="a567b-273">In your Application Insights resource, filter hello metrics by environment="beta".</span></span>

   > [!NOTE]
   > <span data-ttu-id="a567b-274">Pokud toto filtrované zobrazení uložit jako oblíbenou jiný, můžete snadno překlopit zobrazení metriky explorer hello mezi produkčního prostředí a zobrazení beta.</span><span class="sxs-lookup"><span data-stu-id="a567b-274">If you save this filtered view as another favorite, then you can easily flip hello metric explorer views between production and beta views.</span></span>
   >
   >

<span data-ttu-id="a567b-275">Předpokládejme, že ve službě Application Insights se zobrazí něco podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="a567b-275">Suppose in Application Insights you see something similar toohello following:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

<span data-ttu-id="a567b-276">Ne jenom se to zobrazuje, že existuje mnoho další kliknutí na hello `<li>` značky, ale nejspíš toobe nárůst kliknutí na `<li>` značky.</span><span class="sxs-lookup"><span data-stu-id="a567b-276">Not only is this showing that there are many more clicks on hello `<li>` tags, but there seems toobe a surge of clicks on `<li>` tags.</span></span> <span data-ttu-id="a567b-277">Můžete pak dokončete, že lidé byly zjištěny hello nové `<li>` značky kliknout a jsou nyní zrušíte všechny dříve dokončit úkoly v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="a567b-277">You can then conclude that people have discovered hello new `<li>` tags are clickable and are now clearing all their previously-completed tasks in hello app.</span></span>

<span data-ttu-id="a567b-278">Na základě hello dat flighting nasazení, je rozhodnout, že je připravená pro vydání nové uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a567b-278">Based on hello data of your flighting deployment, you decide that your new UI is ready for production.</span></span>

## <a name="go-live-move-your-new-code-into-production"></a><span data-ttu-id="a567b-279">Přejděte za provozu: Přesunutí váš nový kód do výroby</span><span class="sxs-lookup"><span data-stu-id="a567b-279">Go live: Move your new code into production</span></span>
<span data-ttu-id="a567b-280">Můžete se teď připravena toomove vaše tooproduction aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a567b-280">You're now ready toomove your update tooproduction.</span></span> <span data-ttu-id="a567b-281">Co je skvělým je, že nyní víte, že aktualizace již byl ověřen *před* tooproduction je vložena.</span><span class="sxs-lookup"><span data-stu-id="a567b-281">What's great is that now you know that your update has already been validated *before* it is pushed tooproduction.</span></span> <span data-ttu-id="a567b-282">Takže teď můžete ho můžou s jistotou nasazovat.</span><span class="sxs-lookup"><span data-stu-id="a567b-282">So now you can confidently deploy it.</span></span> <span data-ttu-id="a567b-283">Vzhledem k tomu, že jste provedli aktualizaci toohello AngularJS klienta aplikace, pouze ověření kódu na straně klienta hello.</span><span class="sxs-lookup"><span data-stu-id="a567b-283">Since you made an update toohello AngularJS client app, you only validated hello client-side code.</span></span> <span data-ttu-id="a567b-284">Pokud byste byli toomake změny toohello back endové webové rozhraní API aplikace, může ověřit změny, snadno a podobně.</span><span class="sxs-lookup"><span data-stu-id="a567b-284">If you were toomake changes toohello back-end Web API app, you could validate your changes similarly and easily.</span></span>

1. <span data-ttu-id="a567b-285">V prostředí Git odeberte pravidlo směrování provozu hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a567b-285">In Git Shell, remove hello traffic routing rule by running hello following command:</span></span>

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()
2. <span data-ttu-id="a567b-286">Spusťte příkazy Gitu hello:</span><span class="sxs-lookup"><span data-stu-id="a567b-286">Run hello Git commands:</span></span>

        git checkout master
        git pull origin master
        git merge beta
        git push origin master
3. <span data-ttu-id="a567b-287">Počkejte několik minut, než hello nové code toohello toobe nasazený přípravné slot a pak spusťte http://ToDoApp*&lt;your_suffix >*-tooverify staging.azurewebsites.net, který hello nové aktualizace je provozní teplotu v pracovní hello slot.</span><span class="sxs-lookup"><span data-stu-id="a567b-287">Wait for a few minutes for hello new code toobe deployed toohello staging slot, then launch http://ToDoApp*&lt;your_suffix>*-staging.azurewebsites.net tooverify that hello new update is warmed up in hello staging slot.</span></span> <span data-ttu-id="a567b-288">Mějte na paměti, že hello vaší rozvětvení hlavní větve je propojená toohello pracovní pozici vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a567b-288">Remember that hello your fork's master branch is linked toohello staging slot of your app.</span></span>
4. <span data-ttu-id="a567b-289">Nyní Prohodit hello pracovní pozici v produkčním prostředí</span><span class="sxs-lookup"><span data-stu-id="a567b-289">Now, swap hello staging slot into production</span></span>

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a><span data-ttu-id="a567b-290">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a567b-290">Summary</span></span>
<span data-ttu-id="a567b-291">Aplikační služba Azure usnadňuje malá toomedium velké firmy tootest jejich zákazníků směřujících aplikací v provozním prostředí, něco byla tradičně udělat ve velkých podnicích.</span><span class="sxs-lookup"><span data-stu-id="a567b-291">Azure App Service makes it easy for small- toomedium-sized businesses tootest their customer-facing apps in production, something that has been traditionally done in big enterprises.</span></span> <span data-ttu-id="a567b-292">Doufáme, v tomto kurzu jste obdrželi hello znalostní báze, budete potřebovat toobring společně služby App Service a Application Insights toomake možné flighting nasazení a i další testu v produkčních scénářích vaší DevOps světě.</span><span class="sxs-lookup"><span data-stu-id="a567b-292">Hopefully, this tutorial has given you hello knowledge you need toobring together App Service and Application Insights toomake possible flighting deployment, and even other test-in-production scenarios, in your DevOps world.</span></span>

## <a name="more-resources"></a><span data-ttu-id="a567b-293">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="a567b-293">More resources</span></span>
* [<span data-ttu-id="a567b-294">Agile software development službou Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a567b-294">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)
* [<span data-ttu-id="a567b-295">Nastavení přípravných prostředí pro webové aplikace v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a567b-295">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)
* [<span data-ttu-id="a567b-296">Nasazení aplikace komplexní předvídatelné v Azure</span><span class="sxs-lookup"><span data-stu-id="a567b-296">Deploy a complex application predictably in Azure</span></span>](app-service-deploy-complex-application-predictably.md)
* [<span data-ttu-id="a567b-297">Vytváření šablon Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a567b-297">Authoring Azure Resource Manager Templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)
* [<span data-ttu-id="a567b-298">JSONLint - hello validátoru JSON</span><span class="sxs-lookup"><span data-stu-id="a567b-298">JSONLint - hello JSON Validator</span></span>](http://jsonlint.com/)
* [<span data-ttu-id="a567b-299">Vytvoření větve Git – základní větvení a slučování</span><span class="sxs-lookup"><span data-stu-id="a567b-299">Git Branching – Basic Branching and Merging</span></span>](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [<span data-ttu-id="a567b-300">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a567b-300">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="a567b-301">Wiki Kudu projektu</span><span class="sxs-lookup"><span data-stu-id="a567b-301">Project Kudu Wiki</span></span>](https://github.com/projectkudu/kudu/wiki)
