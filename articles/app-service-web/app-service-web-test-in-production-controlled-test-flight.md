---
title: "Flighting nasazení (testování verze beta) v Azure App Service"
description: "Další informace o letu nové funkce v aplikaci nebo verze beta testování vaše aktualizace v tomto kurzu začátku do konce. Spojuje služby App Service funkcí, jako je nepřetržitá publikování, sloty, směrování provozu a integrace Application Insights."
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
ms.openlocfilehash: 83e3247310461ac148fff3c4ade3aa7216478537
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a><span data-ttu-id="b4753-104">Flighting nasazení (testování verze beta) v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b4753-104">Flighting deployment (beta testing) in Azure App Service</span></span>
<span data-ttu-id="b4753-105">V tomto kurzu se dozvíte, jak udělat *flighting nasazení* integrací různé možnosti [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) a [Azure Application Insights](/services/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="b4753-105">This tutorial shows you how to do *flighting deployments* by integrating the various capabilities of [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) and [Azure Application Insights](/services/application-insights/).</span></span>

<span data-ttu-id="b4753-106">*Flighting* je proces nasazení, která ověřuje nové funkce nebo s omezený počet zákazníků skutečné změnit, a hlavní testování v produkční scénář.</span><span class="sxs-lookup"><span data-stu-id="b4753-106">*Flighting* is a deployment process that validates a new feature or change with a limited number of real customers, and is a major testing in production scenario.</span></span> <span data-ttu-id="b4753-107">Se podobají testování beta a se někdy označuje jako "řízené testovací letu".</span><span class="sxs-lookup"><span data-stu-id="b4753-107">It is akin to beta testing and is sometimes known as "controlled test flight".</span></span> <span data-ttu-id="b4753-108">Tuto metodu použijte, mnoho velkých podnicích se webová služba získat časná ověření na jejich aktualizace aplikací v jejich postup [agile vývoj](https://en.wikipedia.org/wiki/Agile_software_development).</span><span class="sxs-lookup"><span data-stu-id="b4753-108">Many large enterprises with a web presence use this approach to get early validation on their app updates in their practice of [agile development](https://en.wikipedia.org/wiki/Agile_software_development).</span></span> <span data-ttu-id="b4753-109">Azure App Service umožňuje integraci se službou Application Insights implementovat stejné scénář DevOps a publikování nepřetržité testování v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b4753-109">Azure App Service enables you to integrate test in production with continous publishing and Application Insights to implement the same DevOps scenario.</span></span> <span data-ttu-id="b4753-110">Výhody tohoto přístupu patří:</span><span class="sxs-lookup"><span data-stu-id="b4753-110">Benefits of this approach include:</span></span>

* <span data-ttu-id="b4753-111">**Získání zpětné vazby skutečné *před* aktualizace vydávají do produkčního prostředí** -lepší, než získají zpětnou vazbu, jakmile uvolnění jediné, co je získání zpětné vazby před uvolněním.</span><span class="sxs-lookup"><span data-stu-id="b4753-111">**Gain real feedback *before* updates are released to production** - The only thing better than gaining feedback as soon as you release is gaining feedback before you release.</span></span> <span data-ttu-id="b4753-112">Aktualizace s přenosy reálný uživatel a chování můžete otestovat časná si přejete v životním cyklu produktu.</span><span class="sxs-lookup"><span data-stu-id="b4753-112">You can test updates with real user traffic and behaviors as early as you desire in the product life cycle.</span></span>
* <span data-ttu-id="b4753-113">**Vylepšení [průběžné testy řízený vývoj (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)**  – díky integraci testování v produkčním prostředí se průběžnou integraci a instrumentace s Application Insights, ověření uživatele se již v rané fázi stane a automaticky v životního cyklu produktu.</span><span class="sxs-lookup"><span data-stu-id="b4753-113">**Enhance [continuous test-driven development (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)** - By integrating test in production with continuous integration and instrumentation with Application Insights, user validation happens early and automatically in your product life cycle.</span></span> <span data-ttu-id="b4753-114">To pomáhá zkrátit čas investice do spuštění manuálního testu.</span><span class="sxs-lookup"><span data-stu-id="b4753-114">This helps reduce time investments in manual test execution.</span></span>
* <span data-ttu-id="b4753-115">**Optimalizace pracovního postupu testování** -automatizací testování v produkčním prostředí s nepřetržité monitorování instrumentace můžete potenciálně provést cílů různé druhy testů v jednom procesu, jako například [integrace](https://en.wikipedia.org/wiki/Integration_testing), [regrese](https://en.wikipedia.org/wiki/Regression_testing), [použitelnost](https://en.wikipedia.org/wiki/Usability_testing), usnadnění, lokalizace, [výkonu](https://en.wikipedia.org/wiki/Software_performance_testing), [zabezpečení](https://en.wikipedia.org/wiki/Security_testing), a [ přijetí](https://en.wikipedia.org/wiki/Acceptance_testing).</span><span class="sxs-lookup"><span data-stu-id="b4753-115">**Optimize test workflow** - By automating test in production with continuous monitoring instrumentation, you can potentially accomplish the goals of various kinds of tests in a single process, such as [integration](https://en.wikipedia.org/wiki/Integration_testing), [regression](https://en.wikipedia.org/wiki/Regression_testing), [usability](https://en.wikipedia.org/wiki/Usability_testing), accessibility, localization, [performance](https://en.wikipedia.org/wiki/Software_performance_testing), [security](https://en.wikipedia.org/wiki/Security_testing), and [acceptance](https://en.wikipedia.org/wiki/Acceptance_testing).</span></span>

<span data-ttu-id="b4753-116">Flighting nasazení není jenom o směrování provoz za provozu.</span><span class="sxs-lookup"><span data-stu-id="b4753-116">A flighting deployment is not just about routing live traffic.</span></span> <span data-ttu-id="b4753-117">Při takovémto nasazení chcete získat přehled co nejrychleji je použít neočekávané chyby, snížení výkonu, problémy uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b4753-117">In such a deployment you want to gain insight as quickly as possible, whether it be an unexpected bug, performance degradation, user experience issues.</span></span> <span data-ttu-id="b4753-118">Mějte na paměti, kterou chcete pracovat se skutečnou zákazníků.</span><span class="sxs-lookup"><span data-stu-id="b4753-118">Remember, you are dealing with real customers.</span></span> <span data-ttu-id="b4753-119">Proto to udělat správné, můžete musí se ujistěte, že jste nastavili flighting nasazení, ke shromažďování všech dat, je nutné, aby bylo možné provést informované rozhodnutí pro další krok.</span><span class="sxs-lookup"><span data-stu-id="b4753-119">So to do it right, you must make sure that you have set up your flighting deployment to gather all the data you need in order to make an informed decision for your next step.</span></span> <span data-ttu-id="b4753-120">V tomto kurzu se dozvíte, jak ke shromažďování dat pomocí služby Application Insights, ale můžete použít New Relic nebo jiných technologií, které vyhovuje vašemu scénáři.</span><span class="sxs-lookup"><span data-stu-id="b4753-120">This tutorial shows you how to collect data with Application Insights, but you can use New Relic or other technologies that suits your scenario.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="b4753-121">Co provedete</span><span class="sxs-lookup"><span data-stu-id="b4753-121">What you will do</span></span>
<span data-ttu-id="b4753-122">V tomto kurzu se dozvíte, jak probouzet následující scénáře společně k testování aplikace služby App Service v produkčním prostředí:</span><span class="sxs-lookup"><span data-stu-id="b4753-122">In this tutorial, you will learn how to bring the following scenarios together to test your App Service app in production:</span></span>

* <span data-ttu-id="b4753-123">[Směrování provozu produkční](app-service-web-test-in-production-get-start.md) do vaší aplikace beta</span><span class="sxs-lookup"><span data-stu-id="b4753-123">[Route production traffic](app-service-web-test-in-production-get-start.md) to your beta app</span></span>
* <span data-ttu-id="b4753-124">[Instrumentace aplikace](../application-insights/app-insights-web-track-usage.md) získat užitečné metriky</span><span class="sxs-lookup"><span data-stu-id="b4753-124">[Instrument your app](../application-insights/app-insights-web-track-usage.md) to obtain useful metrics</span></span>
* <span data-ttu-id="b4753-125">Nepřetržitě nasazení vaší aplikace beta a sledovat metriky za provozu aplikace</span><span class="sxs-lookup"><span data-stu-id="b4753-125">Continuously deploy your beta app and track live app metrics</span></span>
* <span data-ttu-id="b4753-126">Porovnání metriky mezi na produkční aplikaci a beta aplikaci najdete v části jak změny kódu převede výsledky</span><span class="sxs-lookup"><span data-stu-id="b4753-126">Compare metrics between the production app and the beta app to see how code changes translate to results</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="b4753-127">Co budete potřebovat</span><span class="sxs-lookup"><span data-stu-id="b4753-127">What you will need</span></span>
* <span data-ttu-id="b4753-128">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="b4753-128">An Azure account</span></span>
* <span data-ttu-id="b4753-129">A [Githubu](https://github.com/) účtu</span><span class="sxs-lookup"><span data-stu-id="b4753-129">A [GitHub](https://github.com/) account</span></span>
* <span data-ttu-id="b4753-130">Visual Studio 2015 - si můžete stáhnout [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4753-130">Visual Studio 2015 - you can download the [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).</span></span>
* <span data-ttu-id="b4753-131">Git prostředí (nainstalované s [GitHub pro Windows](https://windows.github.com/)) – umožňuje spustit příkazy Git i prostředí PowerShell ve stejné relaci</span><span class="sxs-lookup"><span data-stu-id="b4753-131">Git Shell (installed with [GitHub for Windows](https://windows.github.com/)) - this enables you to run both the Git and PowerShell commands in the same session</span></span>
* <span data-ttu-id="b4753-132">Nejnovější [prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits</span><span class="sxs-lookup"><span data-stu-id="b4753-132">Latest [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits</span></span>
* <span data-ttu-id="b4753-133">Základní znalosti o následující:</span><span class="sxs-lookup"><span data-stu-id="b4753-133">Basic understanding of the following:</span></span>
  * <span data-ttu-id="b4753-134">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) nasazení šablony (viz [nasazení komplexní aplikace předvídatelné v Azure](app-service-deploy-complex-application-predictably.md))</span><span class="sxs-lookup"><span data-stu-id="b4753-134">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) template deployment (see [Deploy a complex application predictably in Azure](app-service-deploy-complex-application-predictably.md))</span></span>
  * [<span data-ttu-id="b4753-135">Git</span><span class="sxs-lookup"><span data-stu-id="b4753-135">Git</span></span>](http://git-scm.com/documentation)
  * [<span data-ttu-id="b4753-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b4753-136">PowerShell</span></span>](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> <span data-ttu-id="b4753-137">K dokončení tohoto kurzu potřebujete mít účet Azure:</span><span class="sxs-lookup"><span data-stu-id="b4753-137">You need an Azure account to complete this tutorial:</span></span>
>
> * <span data-ttu-id="b4753-138">Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití můžete účet ponechat a používat bezplatné služby Azure, jako jsou webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4753-138">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services, such as Web Apps.</span></span>
> * <span data-ttu-id="b4753-139">Můžete [aktivovat výhody předplatitele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -si Visual Studio předplatné vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="b4753-139">You can [activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) - Your Visual Studio subscription gives you credits every month that you can use for paid Azure services.</span></span>
>
> <span data-ttu-id="b4753-140">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b4753-140">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="b4753-141">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="b4753-141">No credit cards required; no commitments.</span></span>
>
>

## <a name="set-up-your-production-web-app"></a><span data-ttu-id="b4753-142">Nastavení webové aplikace produkční</span><span class="sxs-lookup"><span data-stu-id="b4753-142">Set up your production web app</span></span>
> [!NOTE]
> <span data-ttu-id="b4753-143">Skript používá v tomto kurzu bude automaticky nakonfigurovat nepřetržité publikování z úložiště Githubu.</span><span class="sxs-lookup"><span data-stu-id="b4753-143">The script used in this tutorial will automatically configure continuous publishing from your GitHub repository.</span></span> <span data-ttu-id="b4753-144">To vyžaduje, aby vaše přihlašovací údaje Githubu jsou již uloženy v Azure, jinak skriptované instalace se nezdaří, při pokusu o konfiguraci nastavení správy zdrojů pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4753-144">This requires that your GitHub credentials are already stored in Azure, otherwise the scripted deployment will fail when attempting to configure source control settings for the web apps.</span></span>
>
> <span data-ttu-id="b4753-145">K ukládání přihlašovacích údajů Githubu v Azure, vytvořit webovou aplikaci v [portálu Azure](https://portal.azure.com/) a [konfigurace nasazení Githubu](app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="b4753-145">To store your GitHub credentials in Azure, create a web app in the [Azure Portal](https://portal.azure.com/) and [configure GitHub deployment](app-service-continuous-deployment.md).</span></span> <span data-ttu-id="b4753-146">Stačí k tomu jednou.</span><span class="sxs-lookup"><span data-stu-id="b4753-146">You only need to do this once.</span></span>
>
>

<span data-ttu-id="b4753-147">V případě typické DevOps máte aplikaci, která běží v Azure za provozu a chcete provést změny prostřednictvím průběžné publikování.</span><span class="sxs-lookup"><span data-stu-id="b4753-147">In a typical DevOps scenario, you have an application that’s running live in Azure, and you want to make changes to it through continuous publishing.</span></span> <span data-ttu-id="b4753-148">V tomto scénáři se do produkčního prostředí nasadit šablonu, která mají vývoji a testování.</span><span class="sxs-lookup"><span data-stu-id="b4753-148">In this scenario, you will deploy to production a template that you have developed and tested.</span></span>

1. <span data-ttu-id="b4753-149">Vytvoření vlastního pokračovatelem [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) úložiště.</span><span class="sxs-lookup"><span data-stu-id="b4753-149">Create your own fork of the [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) repository.</span></span> <span data-ttu-id="b4753-150">Informace o vytváření vašeho rozvětvení najdete v tématu [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/).</span><span class="sxs-lookup"><span data-stu-id="b4753-150">For information on creating your fork, see [Fork a Repo](https://help.github.com/articles/fork-a-repo/).</span></span> <span data-ttu-id="b4753-151">Po vytvoření vaší rozvětvení, zobrazí se v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="b4753-151">Once your fork is created, you can see it in your browser.</span></span>

   ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. <span data-ttu-id="b4753-152">Otevřete relaci prostředí Git.</span><span class="sxs-lookup"><span data-stu-id="b4753-152">Open a Git Shell session.</span></span> <span data-ttu-id="b4753-153">Pokud ještě nemáte Git prostředí, nainstalujte [GitHub pro Windows](https://windows.github.com/) nyní.</span><span class="sxs-lookup"><span data-stu-id="b4753-153">If you don't have Git Shell yet, install [GitHub for Windows](https://windows.github.com/) now.</span></span>
3. <span data-ttu-id="b4753-154">Vytvořte místní vaší rozvětvení spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="b4753-154">Create a local clone of your fork by executing the following command:</span></span>

     <span data-ttu-id="b4753-155">https://github.com/<your_fork>/ToDoApp.git klon Git</span><span class="sxs-lookup"><span data-stu-id="b4753-155">git clone https://github.com/<your_fork>/ToDoApp.git</span></span>
4. <span data-ttu-id="b4753-156">Až budete mít vaše místní klonování, přejděte na  *&lt;repository_root >*\ARMTemplates a spustit deploy.ps1 skript s jedinečnou příponu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="b4753-156">Once you have your local clone, navigate to *&lt;repository_root>*\ARMTemplates, and run the deploy.ps1 script with a unique suffix, as shown below:</span></span>

     <span data-ttu-id="b4753-157">.\deploy.ps1 – RepoUrl https://github.com/<your_fork>/todoapp.git - ResourceGroupSuffix < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="b4753-157">.\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix></span></span>
5. <span data-ttu-id="b4753-158">Po zobrazení výzvy zadejte požadované uživatelské jméno a heslo pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="b4753-158">When prompted, type in the desired username and password for database access.</span></span> <span data-ttu-id="b4753-159">Mějte na paměti svoje přihlašovací údaje databáze, protože budete muset zadávat znovu při aktualizaci skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="b4753-159">Remember your database credentials because you will need to specify them again when updating the resource group.</span></span>

   <span data-ttu-id="b4753-160">Měli byste vidět zřizování průběh různých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b4753-160">You should see the provisioning progress of various Azure resources.</span></span> <span data-ttu-id="b4753-161">Po dokončení nasazení, skript bude spuštění aplikace v prohlížeči a získáte popisný zvukový signál.</span><span class="sxs-lookup"><span data-stu-id="b4753-161">When deployment completes, the script will launch the application in the browser and give you a friendly beep.</span></span>
   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)
6. <span data-ttu-id="b4753-162">Zpět v relaci prostředí Git, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="b4753-162">Back in your Git Shell session, run:</span></span>

     <span data-ttu-id="b4753-163">. \swap – název ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="b4753-163">.\swap –Name ToDoApp<your_suffix></span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)
7. <span data-ttu-id="b4753-164">Po dokončení skriptu, přejděte zpět a přejděte na adresu front-endu (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) zobrazíte aplikace běžící v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b4753-164">When the script finishes, go back to browse to the frontend’s address (http://ToDoApp*&lt;your_suffix>*.azurewebsites.net/) to see the application running in production.</span></span>
8. <span data-ttu-id="b4753-165">Přihlaste se [portálu Azure](https://portal.azure.com/) a prohlédněte si co je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="b4753-165">Log into the [Azure Portal](https://portal.azure.com/) and take a look at what’s created.</span></span>

   <span data-ttu-id="b4753-166">Byste měli vidět dvě webové aplikace ve stejné skupině prostředků, jeden s `Api` přípony v názvu.</span><span class="sxs-lookup"><span data-stu-id="b4753-166">You should be able to see two web apps in the same resource group, one with the `Api` suffix in the name.</span></span> <span data-ttu-id="b4753-167">Pokud se podíváte na zobrazení skupiny prostředků, zobrazí se také SQL Database a serveru, plán služby App Service a přípravné sloty pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4753-167">If you look at the resource group view, you will also see the SQL Database and server, the App Service plan, and the staging slots for the web apps.</span></span> <span data-ttu-id="b4753-168">Procházet různých prostředků a porovnejte je s  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json zobrazíte, jak jsou nakonfigurované v šabloně.</span><span class="sxs-lookup"><span data-stu-id="b4753-168">Browse through the different resources and compare them with *&lt;repository_root>*\ARMTemplates\ProdAndStage.json to see how they are configured in the template.</span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

<span data-ttu-id="b4753-169">Jste nastavili na produkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b4753-169">You have set up the production app.</span></span>  <span data-ttu-id="b4753-170">Nyní Pojďme Představte si, že nedostanete zpětnou vazbu, použitelnost je špatná pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b4753-170">Now, let's imagine that you receive feedback that usability is poor for the app.</span></span> <span data-ttu-id="b4753-171">Proto se rozhodnete prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="b4753-171">So you decide to investigate.</span></span> <span data-ttu-id="b4753-172">Budete k instrumentaci aplikace tak, abyste získali zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="b4753-172">You're going to instrument your app to give you feedback.</span></span>

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a><span data-ttu-id="b4753-173">Prozkoumat: Instrumentace vaší klientské aplikace pro monitorování/metriky</span><span class="sxs-lookup"><span data-stu-id="b4753-173">Investigate: Instrument your client app for monitoring/metrics</span></span>
1. <span data-ttu-id="b4753-174">Otevřete  *&lt;repository_root >*\src\MultiChannelToDo.sln v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b4753-174">Open *&lt;repository_root>*\src\MultiChannelToDo.sln in Visual Studio.</span></span>
2. <span data-ttu-id="b4753-175">Obnovení všech balíčků Nuget kliknutím pravým tlačítkem na řešení > **spravovat balíčky NuGet pro řešení** > **obnovení**.</span><span class="sxs-lookup"><span data-stu-id="b4753-175">Restore all Nuget packages by right-clicking solution > **Manage NuGet Packages for Solution** > **Restore**.</span></span>
3. <span data-ttu-id="b4753-176">Klikněte pravým tlačítkem na **MultiChannelToDo.Web** > **přidat Telemetrii Application Insights** > **konfigurovat nastavení** > změnit skupinu prostředků. ToDoApp*&lt;your_suffix >* > **přidat službu Application Insights do projektu**.</span><span class="sxs-lookup"><span data-stu-id="b4753-176">Right-click **MultiChannelToDo.Web** > **Add Application Insights Telemetry** > **Configure Settings** > Change resource group to ToDoApp*&lt;your_suffix>* > **Add Application Insights to Project**.</span></span>
4. <span data-ttu-id="b4753-177">Na portálu Azure otevřete okno **MultiChannelToDo.Web** aplikace přehled prostředků.</span><span class="sxs-lookup"><span data-stu-id="b4753-177">In the Azure Portal, open the blade for the **MultiChannelToDo.Web** Application Insight resource.</span></span> <span data-ttu-id="b4753-178">Potom v **stavu aplikací** součástí, klikněte na tlačítko **zjistěte, jak shromažďovat data zatížení stránky prohlížeče** > zkopírujte kód.</span><span class="sxs-lookup"><span data-stu-id="b4753-178">Then in the **Application health** part, click **Learn how to collect browser page load data** > copy code.</span></span>
5. <span data-ttu-id="b4753-179">Přidejte zkopírované kód instrumentace JS k  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml těsně před uzavírací `<heading>` značky.</span><span class="sxs-lookup"><span data-stu-id="b4753-179">Add the copied JS instrumentation code to *&lt;repository_root>*\src\MultiChannelToDo.Web\app\Index.cshtml, just before the closing `<heading>` tag.</span></span> <span data-ttu-id="b4753-180">Měl by obsahovat klíč instrumentace jedinečný prostředek přehled aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4753-180">It should contain the unique instrumentation key of your Application Insight resource.</span></span>

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>
6. <span data-ttu-id="b4753-181">Odeslat vlastní události Application Insights pro myš kliknutí přidáním následující kód do dolní části textu:</span><span class="sxs-lookup"><span data-stu-id="b4753-181">Send custom events to Application Insights for mouse clicks by adding the following code to the bottom of body:</span></span>

       <script>
           $(document.body).find("*").click(function(event) {

               appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
           });
       </script>

   <span data-ttu-id="b4753-182">Vlastní události Application Insights pokaždé, když uživatel klikne kdekoli ve webové aplikaci odešle tento fragment kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b4753-182">This JavaScript snippet sends a custom event to Application Insights every time a user clicks anywhere in the web app.</span></span>
7. <span data-ttu-id="b4753-183">V prostředí Git potvrďte a odešlete změny do vaší rozvětvení v Githubu.</span><span class="sxs-lookup"><span data-stu-id="b4753-183">In Git Shell, commit and push your changes to your fork in GitHub.</span></span> <span data-ttu-id="b4753-184">Potom počkejte klientů aktualizujte prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b4753-184">Then, wait for clients to refresh browser.</span></span>

       git add -A :/
       git commit -m "add AI configuration for client app"
       git push origin master
8. <span data-ttu-id="b4753-185">Swap – změny aplikace nasazené do produkčního prostředí:</span><span class="sxs-lookup"><span data-stu-id="b4753-185">Swap the deployed app changes to production:</span></span>

     <span data-ttu-id="b4753-186">. \swap – název ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="b4753-186">.\swap –Name ToDoApp<your_suffix></span></span>
9. <span data-ttu-id="b4753-187">Procházejte do zdroje Application Insights, který jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="b4753-187">Browse to the Application Insights resource that you configured.</span></span> <span data-ttu-id="b4753-188">Klikněte na tlačítko Vlastní události.</span><span class="sxs-lookup"><span data-stu-id="b4753-188">Click Custom events.</span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

   <span data-ttu-id="b4753-189">Pokud nevidíte metriky pro vlastní události, počkejte několik minut a klikněte na tlačítko **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="b4753-189">If you don't see metrics for custom events, wait a few minutes and click **Refresh**.</span></span>

<span data-ttu-id="b4753-190">Předpokládejme, že se zobrazí graf jako níže:</span><span class="sxs-lookup"><span data-stu-id="b4753-190">Suppose you see a chart like below:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

<span data-ttu-id="b4753-191">A pod ním mřížky událostí:</span><span class="sxs-lookup"><span data-stu-id="b4753-191">And the event grid below it:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

<span data-ttu-id="b4753-192">Podle kódu aplikace ToDoApp **tlačítko** události odpovídá tlačítko pro odeslání a **vstup** události odpovídá textové pole.</span><span class="sxs-lookup"><span data-stu-id="b4753-192">According to your ToDoApp application code, the **BUTTON** event corresponds to the submit button, and the **INPUT** event corresponds to the textbox.</span></span> <span data-ttu-id="b4753-193">Zatím věcí smysl.</span><span class="sxs-lookup"><span data-stu-id="b4753-193">So far, things make sense.</span></span> <span data-ttu-id="b4753-194">Ale to vypadá, je dobré množství kliknutí a jen několik kliknutí položkami seznamu úkolů ( **LI** událostí).</span><span class="sxs-lookup"><span data-stu-id="b4753-194">However, it looks like there's a good amount of clicks and very few clicks on the to-do items (the **LI** events).</span></span>

<span data-ttu-id="b4753-195">Založené na tomto formuláři je vaše předpoklad, že někteří uživatelé nerozumíte, kterou část uživatelského rozhraní je můžete kliknout a je to proto kurzor je navržen pro výběr textu je vždy na položky seznamu a jejich ikony, když se.</span><span class="sxs-lookup"><span data-stu-id="b4753-195">Based on this, you form your hypothesis that some users are confused which part of the UI is clickable and it is because the cursor is styled for text selection when it hovers on the list items and their icons.</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

<span data-ttu-id="b4753-196">To může být contrived příklad.</span><span class="sxs-lookup"><span data-stu-id="b4753-196">This might be a contrived example.</span></span> <span data-ttu-id="b4753-197">Nicméně budete provádět zlepšení do vaší aplikace a pak proveďte flighting nasazení získání použitelnost zpětné vazby od zákazníků za provozu.</span><span class="sxs-lookup"><span data-stu-id="b4753-197">Nevertheless, you're going to make an improvement to your app, and then perform a flighting deployment to get usability feedback from live customers.</span></span>

### <a name="instrument-your-server-app-for-monitoringmetrics"></a><span data-ttu-id="b4753-198">Aplikace serveru pro monitorování/metriky instrumentace</span><span class="sxs-lookup"><span data-stu-id="b4753-198">Instrument your server app for monitoring/metrics</span></span>
<span data-ttu-id="b4753-199">To je tangens, protože tento scénář ukázáno v tomto kurzu zabývá pouze klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4753-199">This is a tangent since the scenario demonstrated in this tutorial only deals with the client app.</span></span> <span data-ttu-id="b4753-200">Ale pro úplnost nastavíte serverové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4753-200">However, for completeness you will set up the server-side app.</span></span>

1. <span data-ttu-id="b4753-201">Klikněte pravým tlačítkem na **MultiChannelToDo** > **přidat Telemetrii Application Insights** > **konfigurovat nastavení** > změnit skupinu prostředků. ToDoApp*&lt;your_suffix >* > **přidat službu Application Insights do projektu**.</span><span class="sxs-lookup"><span data-stu-id="b4753-201">Right-click **MultiChannelToDo** > **Add Application Insights Telemetry** > **Configure Settings** > Change resource group to ToDoApp*&lt;your_suffix>* > **Add Application Insights to Project**.</span></span>
2. <span data-ttu-id="b4753-202">V prostředí Git potvrďte a odešlete změny do vaší rozvětvení v Githubu.</span><span class="sxs-lookup"><span data-stu-id="b4753-202">In Git Shell, commit and push your changes to your fork in GitHub.</span></span> <span data-ttu-id="b4753-203">Potom počkejte klientů aktualizujte prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b4753-203">Then, wait for clients to refresh browser.</span></span>

       git add -A :/
       git commit -m "add AI configuration for server app"
       git push origin master
3. <span data-ttu-id="b4753-204">Swap – změny aplikace nasazené do produkčního prostředí:</span><span class="sxs-lookup"><span data-stu-id="b4753-204">Swap the deployed app changes to production:</span></span>

     <span data-ttu-id="b4753-205">. \swap – název ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="b4753-205">.\swap –Name ToDoApp<your_suffix></span></span>

<span data-ttu-id="b4753-206">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="b4753-206">That's it!</span></span>

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a><span data-ttu-id="b4753-207">Prozkoumat: Přidání značek specifické pro slot pro vaše aplikace metriky klienta</span><span class="sxs-lookup"><span data-stu-id="b4753-207">Investigate: Add slot-specific tags to your client app metrics</span></span>
<span data-ttu-id="b4753-208">V této části můžete nakonfigurovat různé nasazovací sloty k odesílání telemetrie specifické pro slot do stejného zdroje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="b4753-208">In this section, you will configure the different deployment slots to send slot-specific telemetry to the same Application Insights resource.</span></span> <span data-ttu-id="b4753-209">Tímto způsobem snadno projevily provedené změny aplikace můžete porovnat telemetrická data mezi přenosy dat z různých sloty (nasazení prostředí).</span><span class="sxs-lookup"><span data-stu-id="b4753-209">This way, you can compare telemetry data between traffic from different slots (deployment environments) to easily see the effect of your app changes.</span></span> <span data-ttu-id="b4753-210">Ve stejnou dobu můžete oddělit provoz produkční od ostatních, můžete nadále sledovat vaše produkční aplikace podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="b4753-210">At the same time, you can separate the production traffic from the rest so you can continue to monitor your production app as needed.</span></span>

<span data-ttu-id="b4753-211">Vzhledem k tomu, že shromažďování dat na chování klienta bude [přidat inicializátoru telemetrie do kódu jazyka JavaScript](../application-insights/app-insights-api-filtering-sampling.md) v index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="b4753-211">Since you're gathering data on client behavior, you will [add a telemetry initializer to your JavaScript code](../application-insights/app-insights-api-filtering-sampling.md) in index.cshtml.</span></span> <span data-ttu-id="b4753-212">Pokud chcete k testování výkonu na straně serveru, například můžete provést také podobně jako v kódu serveru (viz [Application Insights API pro vlastní události a metriky](../application-insights/app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="b4753-212">If you want to test server-side performance, for example, you can also do similarly in your server code (see [Application Insights API for custom events and metrics](../application-insights/app-insights-api-custom-events-metrics.md).</span></span>

1. <span data-ttu-id="b4753-213">Nejprve přidejte kód bewteen dva `//` blokovat komentáře níže v jazyka JavaScript, který jste přidali do `<heading>` značky dříve.</span><span class="sxs-lookup"><span data-stu-id="b4753-213">First, add the code bewteen the two `//` comments below in the JavaScript block that you added to the `<heading>` tag earlier.</span></span>

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

    <span data-ttu-id="b4753-214">Tento kód inicializátoru způsobí, že `appInsights` objekt, který chcete přidat vlastní vlastnost s názvem `Environment` k každá část telemetrie odešle.</span><span class="sxs-lookup"><span data-stu-id="b4753-214">This initializer code causes the `appInsights` object to add the a custom property called `Environment` to every piece of telemetry it sends.</span></span>
2. <span data-ttu-id="b4753-215">Dál přidejte tuto vlastní vlastnost jako [pozice nastavení](web-sites-staged-publishing.md#AboutConfiguration) pro webovou aplikaci v Azure.</span><span class="sxs-lookup"><span data-stu-id="b4753-215">Next, add this custom property as a [slot setting](web-sites-staged-publishing.md#AboutConfiguration) for your web app in Azure.</span></span> <span data-ttu-id="b4753-216">Chcete-li to provést, spusťte následující příkazy v relaci prostředí Git.</span><span class="sxs-lookup"><span data-stu-id="b4753-216">To do this, run the following commands in your Git Shell session.</span></span>

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    <span data-ttu-id="b4753-217">Soubor Web.config v projektu již definuje `environment` nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4753-217">The Web.config in your project already defines the `environment` app setting.</span></span> <span data-ttu-id="b4753-218">Při tomto nastavení se při testování aplikaci místně, vaše metriky budou označené `VS Debugger`.</span><span class="sxs-lookup"><span data-stu-id="b4753-218">With this setting, when you test the app locally, your metrics will be tagged with `VS Debugger`.</span></span> <span data-ttu-id="b4753-219">Ale pokud je odešlete své změny do Azure, Azure bude najít a používat `environment` aplikace radši nastavení v konfiguraci webové aplikace a vaše metriky budou označené `Production`.</span><span class="sxs-lookup"><span data-stu-id="b4753-219">However, when you push your changes to Azure, Azure will find and use the `environment` app setting in the web app's configuration instead, and your metrics will be tagged with `Production`.</span></span>
3. <span data-ttu-id="b4753-220">Potvrďte a odešlete své změny kódu do vaší větve na Githubu a potom počkejte, než pro uživatele, aby používali nové aplikace (třeba aktualizujte stránku prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="b4753-220">Commit and push your code changes to your fork on GitHub, and then wait for your users to use the new app (need to refresh the browser).</span></span> <span data-ttu-id="b4753-221">Jak dlouho trvá asi 15 minut pro novou vlastnost objeví ve vaší službě Application Insights `MultiChannelToDo.Web` prostředků.</span><span class="sxs-lookup"><span data-stu-id="b4753-221">It takes about 15 minutes for the new property to show up in your Application Insights `MultiChannelToDo.Web` resource.</span></span>

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master
4. <span data-ttu-id="b4753-222">Nyní, přejděte na **vlastní události** okno znovu a filtrujte metriky `Environment=Production`.</span><span class="sxs-lookup"><span data-stu-id="b4753-222">Now, go to the **Custom events** blade again and filter the metrics on `Environment=Production`.</span></span> <span data-ttu-id="b4753-223">Teď by měla být vidět všechny nové vlastní události v produkčním slotu s Tento filtr.</span><span class="sxs-lookup"><span data-stu-id="b4753-223">You should now be able to see all the new custom events in the production slot with this filter.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)
5. <span data-ttu-id="b4753-224">Klikněte na tlačítko **Oblíbené** tlačítko Uložit aktuální nastavení Průzkumníku metrik například **vlastní události: produkční**.</span><span class="sxs-lookup"><span data-stu-id="b4753-224">Click the **Favorites** button to save the current Metrics Explorer settings to something like **Custom events: Production**.</span></span> <span data-ttu-id="b4753-225">Můžete snadno přepnout mezi tato zobrazení a zobrazení slotu nasazení později.</span><span class="sxs-lookup"><span data-stu-id="b4753-225">You can easily switch between this view and a deployment slot view later.</span></span>

   > [!TIP]
   > <span data-ttu-id="b4753-226">Ještě účinnější analýzy, vezměte v úvahu [integraci prostředku Application Insights do Power BI](../application-insights/app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="b4753-226">For even more powerful analytics, consider [integrating your Application Insights resource with Power BI](../application-insights/app-insights-export-power-bi.md).</span></span>
   >
   >

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a><span data-ttu-id="b4753-227">Přidání značek slotu specifické pro vaše aplikace metriky serveru</span><span class="sxs-lookup"><span data-stu-id="b4753-227">Add slot-specific tags to your server app metrics</span></span>
<span data-ttu-id="b4753-228">Pro úplnost znovu, bude nastavit aplikaci na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="b4753-228">Again, for completeness you will set up the server-side app.</span></span> <span data-ttu-id="b4753-229">Na rozdíl od klientské aplikace, které je instrumentovány v jazyce JavaScript je značky slotu specifické pro aplikace serveru instrumentována kódem .NET.</span><span class="sxs-lookup"><span data-stu-id="b4753-229">Unlike the client app which is instrumented in JavaScript, slot-specific tags for the server app is instrumented with .NET code.</span></span>

1. <span data-ttu-id="b4753-230">Otevřete  *&lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="b4753-230">Open *&lt;repository_root>*\src\MultiChannelToDo\Global.asax.cs.</span></span> <span data-ttu-id="b4753-231">Přidat blok kódu níže, těsně před uzavírací obor názvů složených závorek.</span><span class="sxs-lookup"><span data-stu-id="b4753-231">Add the code block below, just before the closing namespace curly brace.</span></span>

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
2. <span data-ttu-id="b4753-232">Opravte chyby překladu názvů přidáním `using` příkazy níže na začátek souboru:</span><span class="sxs-lookup"><span data-stu-id="b4753-232">Correct the name resolution errors by adding the `using` statements below to the beginning of the file:</span></span>

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;
3. <span data-ttu-id="b4753-233">Kód níže přidejte na začátek `Application_Start()` metoda:</span><span class="sxs-lookup"><span data-stu-id="b4753-233">Add the code below to the beginning of the `Application_Start()` method:</span></span>

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());
4. <span data-ttu-id="b4753-234">Potvrďte a odešlete své změny kódu do vaší větve na Githubu a potom počkejte, než pro uživatele, aby používali nové aplikace (třeba aktualizujte stránku prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="b4753-234">Commit and push your code changes to your fork on GitHub, and then wait for your users to use the new app (need to refresh the browser).</span></span> <span data-ttu-id="b4753-235">Jak dlouho trvá asi 15 minut pro novou vlastnost objeví ve vaší službě Application Insights `MultiChannelToDo` prostředků.</span><span class="sxs-lookup"><span data-stu-id="b4753-235">It takes about 15 minutes for the new property to show up in your Application Insights `MultiChannelToDo` resource.</span></span>

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a><span data-ttu-id="b4753-236">Aktualizace: Nastavení větev beta</span><span class="sxs-lookup"><span data-stu-id="b4753-236">Update: Set up your beta branch</span></span>
1. <span data-ttu-id="b4753-237">Otevřete  *&lt;repository_root >*\ARMTemplates\ProdAndStagetest.json a najít `appsettings` prostředky (vyhledejte `"name": "appsettings"`).</span><span class="sxs-lookup"><span data-stu-id="b4753-237">Open *&lt;repository_root>*\ARMTemplates\ProdAndStagetest.json and find the `appsettings` resources (search for `"name": "appsettings"`).</span></span> <span data-ttu-id="b4753-238">Existují 4 z nich, jeden pro každou slot.</span><span class="sxs-lookup"><span data-stu-id="b4753-238">There are 4 of them, one for each slot.</span></span>
2. <span data-ttu-id="b4753-239">Pro každou `appsettings` prostředků, přidejte `"environment": "[parameters('slotName')]"` nastavení aplikace nastavte na konci `properties` pole.</span><span class="sxs-lookup"><span data-stu-id="b4753-239">For each `appsettings` resource, add an  `"environment": "[parameters('slotName')]"` app setting to the end of the `properties` array.</span></span> <span data-ttu-id="b4753-240">Nezapomeňte si ukončení předchozí řádek oddělujte čárkami.</span><span class="sxs-lookup"><span data-stu-id="b4753-240">Don't forget to end the previous line with a comma.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)

    <span data-ttu-id="b4753-241">Jste právě přidali `environment` nastavení aplikace nastavte na všechny sloty v šabloně.</span><span class="sxs-lookup"><span data-stu-id="b4753-241">You have just added the `environment` app setting to all the slots in the template.</span></span>
3. <span data-ttu-id="b4753-242">Ve stejném souboru najít `slotconfignames` prostředky (vyhledejte `"name": "slotconfignames"`).</span><span class="sxs-lookup"><span data-stu-id="b4753-242">In the same file, find the `slotconfignames` resources (search for `"name": "slotconfignames"`).</span></span> <span data-ttu-id="b4753-243">Existují 2 z nich, jeden pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b4753-243">There are 2 of them, one for each app.</span></span>
4. <span data-ttu-id="b4753-244">Pro každou `slotconfignames` prostředků, přidat `"environment"` na konec `appSettingNames` pole.</span><span class="sxs-lookup"><span data-stu-id="b4753-244">For each `slotconfignames` resource, add `"environment"` to the end of the `appSettingNames` array.</span></span> <span data-ttu-id="b4753-245">Nezapomeňte si ukončení předchozí řádek oddělujte čárkami.</span><span class="sxs-lookup"><span data-stu-id="b4753-245">Don't forget to end the previous line with a comma.</span></span>

    <span data-ttu-id="b4753-246">Právě jste provedli `environment` aplikace nastavení Flash disk do jeho příslušného nasazovací slot pro obě aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4753-246">You have just made the `environment` app setting stick to its respective deployment slot for both apps.</span></span>  
5. <span data-ttu-id="b4753-247">V relaci prostředí Git s příponou stejné skupiny prostředků, které jste používali před spuštěním následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="b4753-247">In your Git Shell session, run the following commands with the same resource group suffix that you used before.</span></span>

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta
6. <span data-ttu-id="b4753-248">Po zobrazení výzvy zadejte stejné SQL databáze přihlašovací údaje jako před.</span><span class="sxs-lookup"><span data-stu-id="b4753-248">When prompted, specify the same SQL database credentials as before.</span></span> <span data-ttu-id="b4753-249">Potom zadejte dotaz, chcete-li aktualizovat skupinu prostředků, `Y`, pak `ENTER`.</span><span class="sxs-lookup"><span data-stu-id="b4753-249">Then, when asked to update the resource group, type `Y`, then `ENTER`.</span></span>

    <span data-ttu-id="b4753-250">Po dokončení skriptu, se zachovají všechny prostředky v původní skupinu prostředků, ale nový slot s názvem "beta" je vytvořen se stejnou konfiguraci jako slotu "Přípravy", který byl vytvořen v začátku.</span><span class="sxs-lookup"><span data-stu-id="b4753-250">Once the script finishes, all your resources in the original resource group are retained, but a new slot named "beta" is created in it with the same configuration as the "Staging" slot that was created in the beginning.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b4753-251">Tato metoda vytvoření různých prostředí se liší od metody v [Agile software development službou Azure App Service](app-service-agile-software-development.md).</span><span class="sxs-lookup"><span data-stu-id="b4753-251">This method of creating different deployment environments is different from the method in [Agile software development with Azure App Service](app-service-agile-software-development.md).</span></span> <span data-ttu-id="b4753-252">Zde vytvoříte nasazení prostředí s slotů nasazení, kde jako tam vytvořit nasazení prostředí ke skupinám prostředků.</span><span class="sxs-lookup"><span data-stu-id="b4753-252">Here, you create deployment environments with deployment slots, where as there you create deployment environments with resource groups.</span></span> <span data-ttu-id="b4753-253">Správa prostředí nasazení pomocí skupin prostředků slouží k udržování produkčního prostředí off-limits pro vývojáře, ale není snadné provést testování v produkčním prostředí, které můžete provést snadno sloty.</span><span class="sxs-lookup"><span data-stu-id="b4753-253">Managing deployment environments with resource groups enables you to keep the production environment off-limits to developers, but it's not easy to do testing in production, which you can do easily with slots.</span></span>
   >
   >

<span data-ttu-id="b4753-254">Pokud chcete, můžete také vytvořit aplikaci alfa spuštěním</span><span class="sxs-lookup"><span data-stu-id="b4753-254">If you wish, you can also create an alpha app by running</span></span>

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

<span data-ttu-id="b4753-255">V tomto kurzu jste se právě zachovat pomocí beta verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4753-255">For this tutorial, you will just keep using your beta app.</span></span>

## <a name="update-push-your-updates-to-the-beta-app"></a><span data-ttu-id="b4753-256">Aktualizace: Nabízená vaše aktualizace aplikace beta</span><span class="sxs-lookup"><span data-stu-id="b4753-256">Update: Push your updates to the beta app</span></span>
<span data-ttu-id="b4753-257">Zpět do aplikace, kterou chcete vylepšit.</span><span class="sxs-lookup"><span data-stu-id="b4753-257">Back to your app that you want to improve.</span></span>

1. <span data-ttu-id="b4753-258">Ujistěte se, že nyní jste v větev beta</span><span class="sxs-lookup"><span data-stu-id="b4753-258">Make sure you're now in your beta branch</span></span>

        git checkout beta
2. <span data-ttu-id="b4753-259">V  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, Najít `<li>` značky a přidat `style="cursor:pointer"` atributu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="b4753-259">In *&lt;repository_root>*\src\MultiChannelToDo.Web\app\Index.cshtml, find the `<li>` tag and add the `style="cursor:pointer"` attribute, as shown below.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)
3. <span data-ttu-id="b4753-260">potvrzení a push na Azure.</span><span class="sxs-lookup"><span data-stu-id="b4753-260">commit and push to Azure.</span></span>
4. <span data-ttu-id="b4753-261">Ověřte, že změny teď se ve slotu beta přechodem na http://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/.</span><span class="sxs-lookup"><span data-stu-id="b4753-261">Verify that the change is now reflected in the beta slot by navigating to http://todoapp*&lt;your_suffix>*-beta.azurewebsites.net/.</span></span> <span data-ttu-id="b4753-262">Pokud nevidíte změnu ještě, aktualizujte webový prohlížeč k získání nového kódu javascript.</span><span class="sxs-lookup"><span data-stu-id="b4753-262">If you don't see the change yet, refresh your browser to get the new javascript code.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

<span data-ttu-id="b4753-263">Teď, když máte změny spuštěna ve slotu beta, jste připravení na provedení flighting nasazení.</span><span class="sxs-lookup"><span data-stu-id="b4753-263">Now that you have your change running in the beta slot, you are ready to perform a flighting deployment.</span></span>

## <a name="validate-route-traffic-to-the-beta-app"></a><span data-ttu-id="b4753-264">Ověření: Směrovat provoz beta verze aplikace</span><span class="sxs-lookup"><span data-stu-id="b4753-264">Validate: Route traffic to the beta app</span></span>
<span data-ttu-id="b4753-265">V této části bude směrovat provoz beta verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4753-265">In this section, you will route traffic to the beta app.</span></span> <span data-ttu-id="b4753-266">For sake of přehlednost ukázku budete ho směrovat podstatnou část provozu generovaného uživateli.</span><span class="sxs-lookup"><span data-stu-id="b4753-266">For sake of clarity of demonstration, you're going to route a significant portion of the user traffic to it.</span></span> <span data-ttu-id="b4753-267">Ve skutečnosti objem provozu, kterou chcete směrovat bude záviset na konkrétní situaci.</span><span class="sxs-lookup"><span data-stu-id="b4753-267">In reality, the amount of traffic you want to route will depend on your specific situation.</span></span> <span data-ttu-id="b4753-268">Například pokud váš webový server je ve velkém měřítku Microsoft.com, pak musíte méně než jeden procent celkového provozu s cílem získat užitečné data.</span><span class="sxs-lookup"><span data-stu-id="b4753-268">For example, if your site is at the scale of microsoft.com, then you may need less than one percent of your total traffic in order to gain useful data.</span></span>

1. <span data-ttu-id="b4753-269">V relaci prostředí Git spusťte následující příkazy polovinu produkční provoz směrovat na beta slot:</span><span class="sxs-lookup"><span data-stu-id="b4753-269">In your Git Shell session, run the following commands to route half of the production traffic to the beta slot:</span></span>

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

   <span data-ttu-id="b4753-270">`ReroutePercentage=50` Vlastnost určuje, že 50 % produkční provozu budou směrované na adresu URL aplikace beta (určená elementem `ActionHostName` vlastnost).</span><span class="sxs-lookup"><span data-stu-id="b4753-270">The `ReroutePercentage=50` property specifies that 50% of the production traffic will be routed to the beta app's URL (specified by the `ActionHostName` property).</span></span>
2. <span data-ttu-id="b4753-271">Nyní přejděte na http://ToDoApp*&lt;your_suffix >*. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="b4753-271">Now navigate to http://ToDoApp*&lt;your_suffix>*.azurewebsites.net.</span></span> <span data-ttu-id="b4753-272">50 % provozu teď přesměrovat na beta slot.</span><span class="sxs-lookup"><span data-stu-id="b4753-272">50% of the traffic should now be redirected to the beta slot.</span></span>
3. <span data-ttu-id="b4753-273">V prostředku Application Insights, filtrovat podle prostředí metriky = "beta".</span><span class="sxs-lookup"><span data-stu-id="b4753-273">In your Application Insights resource, filter the metrics by environment="beta".</span></span>

   > [!NOTE]
   > <span data-ttu-id="b4753-274">Pokud toto filtrované zobrazení uložit jako oblíbenou jiný, můžete snadno překlopit zobrazení metriky explorer mezi produkčního prostředí a zobrazení beta.</span><span class="sxs-lookup"><span data-stu-id="b4753-274">If you save this filtered view as another favorite, then you can easily flip the metric explorer views between production and beta views.</span></span>
   >
   >

<span data-ttu-id="b4753-275">Předpokládejme, že ve službě Application Insights byste vidět něco podobného jako následující:</span><span class="sxs-lookup"><span data-stu-id="b4753-275">Suppose in Application Insights you see something similar to the following:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

<span data-ttu-id="b4753-276">Ne jenom se to zobrazuje, že jsou na mnoho další kliknutí `<li>` značky, ale nejspíš nárůst kliknutí na `<li>` značky.</span><span class="sxs-lookup"><span data-stu-id="b4753-276">Not only is this showing that there are many more clicks on the `<li>` tags, but there seems to be a surge of clicks on `<li>` tags.</span></span> <span data-ttu-id="b4753-277">Můžete pak dokončete, že lidé mají zjišťuje nové `<li>` značky kliknout a jsou nyní zrušíte všechny dříve dokončit úkoly v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b4753-277">You can then conclude that people have discovered the new `<li>` tags are clickable and are now clearing all their previously-completed tasks in the app.</span></span>

<span data-ttu-id="b4753-278">Na základě dat flighting nasazení, je rozhodnout, že je připravená pro vydání nové uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b4753-278">Based on the data of your flighting deployment, you decide that your new UI is ready for production.</span></span>

## <a name="go-live-move-your-new-code-into-production"></a><span data-ttu-id="b4753-279">Přejděte za provozu: Přesunutí váš nový kód do výroby</span><span class="sxs-lookup"><span data-stu-id="b4753-279">Go live: Move your new code into production</span></span>
<span data-ttu-id="b4753-280">Nyní jste připravení přejít aktualizace do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="b4753-280">You're now ready to move your update to production.</span></span> <span data-ttu-id="b4753-281">Co je skvělým je, že nyní víte, že aktualizace již byl ověřen *před* je vložena do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="b4753-281">What's great is that now you know that your update has already been validated *before* it is pushed to production.</span></span> <span data-ttu-id="b4753-282">Takže teď můžete ho můžou s jistotou nasazovat.</span><span class="sxs-lookup"><span data-stu-id="b4753-282">So now you can confidently deploy it.</span></span> <span data-ttu-id="b4753-283">Vzhledem k tomu, že jste provedli aktualizaci do klientské aplikace AngularJS, pouze ověřit kód straně klienta.</span><span class="sxs-lookup"><span data-stu-id="b4753-283">Since you made an update to the AngularJS client app, you only validated the client-side code.</span></span> <span data-ttu-id="b4753-284">Pokud byste chtěli změnit back-end aplikačním webového rozhraní API, může ověřit změny, snadno a podobně.</span><span class="sxs-lookup"><span data-stu-id="b4753-284">If you were to make changes to the back-end Web API app, you could validate your changes similarly and easily.</span></span>

1. <span data-ttu-id="b4753-285">V prostředí Git odeberte pravidlo směrování provozu tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b4753-285">In Git Shell, remove the traffic routing rule by running the following command:</span></span>

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()
2. <span data-ttu-id="b4753-286">Spusťte příkazy Git:</span><span class="sxs-lookup"><span data-stu-id="b4753-286">Run the Git commands:</span></span>

        git checkout master
        git pull origin master
        git merge beta
        git push origin master
3. <span data-ttu-id="b4753-287">Počkejte několik minut, než nový kód pro nasazení do přípravný slot a pak spusťte http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net k ověření, že je nová aktualizace jestli v přípravný slot.</span><span class="sxs-lookup"><span data-stu-id="b4753-287">Wait for a few minutes for the new code to be deployed to the staging slot, then launch http://ToDoApp*&lt;your_suffix>*-staging.azurewebsites.net to verify that the new update is warmed up in the staging slot.</span></span> <span data-ttu-id="b4753-288">Nezapomeňte, že vaše rozvětvení hlavní větve je propojený s přípravný slot vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4753-288">Remember that the your fork's master branch is linked to the staging slot of your app.</span></span>
4. <span data-ttu-id="b4753-289">Nyní swap přípravný slot na produkční</span><span class="sxs-lookup"><span data-stu-id="b4753-289">Now, swap the staging slot into production</span></span>

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a><span data-ttu-id="b4753-290">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b4753-290">Summary</span></span>
<span data-ttu-id="b4753-291">Aplikační služba Azure lze snadno pro malé - pro střední firmy něco otestovat jejich zákazníků směřujících aplikací v provozním prostředí, které tradičně provedeno ve velkých podnicích.</span><span class="sxs-lookup"><span data-stu-id="b4753-291">Azure App Service makes it easy for small- to medium-sized businesses to test their customer-facing apps in production, something that has been traditionally done in big enterprises.</span></span> <span data-ttu-id="b4753-292">Doufáme v tomto kurzu jste obdrželi znalosti, že potřebujete shromáždit služba App Service a Application Insights, aby možné flighting nasazení a i další testu v produkčních scénářích vaší DevOps světě.</span><span class="sxs-lookup"><span data-stu-id="b4753-292">Hopefully, this tutorial has given you the knowledge you need to bring together App Service and Application Insights to make possible flighting deployment, and even other test-in-production scenarios, in your DevOps world.</span></span>

## <a name="more-resources"></a><span data-ttu-id="b4753-293">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="b4753-293">More resources</span></span>
* [<span data-ttu-id="b4753-294">Agile software development službou Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b4753-294">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)
* [<span data-ttu-id="b4753-295">Nastavení přípravných prostředí pro webové aplikace v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b4753-295">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)
* [<span data-ttu-id="b4753-296">Nasazení aplikace komplexní předvídatelné v Azure</span><span class="sxs-lookup"><span data-stu-id="b4753-296">Deploy a complex application predictably in Azure</span></span>](app-service-deploy-complex-application-predictably.md)
* [<span data-ttu-id="b4753-297">Vytváření šablon Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b4753-297">Authoring Azure Resource Manager Templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)
* [<span data-ttu-id="b4753-298">JSONLint - validátor JSON</span><span class="sxs-lookup"><span data-stu-id="b4753-298">JSONLint - The JSON Validator</span></span>](http://jsonlint.com/)
* [<span data-ttu-id="b4753-299">Vytvoření větve Git – základní větvení a slučování</span><span class="sxs-lookup"><span data-stu-id="b4753-299">Git Branching – Basic Branching and Merging</span></span>](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [<span data-ttu-id="b4753-300">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b4753-300">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="b4753-301">Wiki Kudu projektu</span><span class="sxs-lookup"><span data-stu-id="b4753-301">Project Kudu Wiki</span></span>](https://github.com/projectkudu/kudu/wiki)
