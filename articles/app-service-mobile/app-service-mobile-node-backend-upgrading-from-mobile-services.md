---
title: "Upgrade z mobilní služby Azure App Service - Node.js"
description: "Zjistěte, jak snadno upgradovat aplikaci Mobile Services pro aplikaci služby mobilní aplikace"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: yochayk
editor: 
ms.assetid: c58f6df0-5aad-40a3-bddc-319c378218e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: ce0572e85c258aa377c3eea7923d43a30c935bb2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a><span data-ttu-id="9d9b5-103">Upgrade existující Mobile Service Node.js Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9d9b5-103">Upgrade your existing Node.js Azure Mobile Service to App Service</span></span>
<span data-ttu-id="9d9b5-104">Mobile App Service je nový způsob vytváření mobilních aplikací pomocí Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-104">App Service Mobile is a new way to build mobile applications using Microsoft Azure.</span></span> <span data-ttu-id="9d9b5-105">Další informace najdete v tématu [co jsou Mobile Apps?].</span><span class="sxs-lookup"><span data-stu-id="9d9b5-105">To learn more, see [What are Mobile Apps?].</span></span>

<span data-ttu-id="9d9b5-106">Toto téma popisuje postup upgradu existující aplikaci back-end Node.js z Azure Mobile Services na nové App Service Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-106">This topic describes how to upgrade an existing Node.js backend application from Azure Mobile Services to a new App Service Mobile Apps.</span></span> <span data-ttu-id="9d9b5-107">Při provádění tohoto upgradu, aplikace pro Mobile Services můžete nadále fungovat.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-107">While you perform this upgrade, your existing Mobile Services application can continue to operate.</span></span>  <span data-ttu-id="9d9b5-108">Pokud je třeba upgradovat aplikace back-end Node.js, podívejte se na [upgradu vaší .NET Mobile Services](app-service-mobile-net-upgrading-from-mobile-services.md).</span><span class="sxs-lookup"><span data-stu-id="9d9b5-108">If you need to upgrade a Node.js backend application, refer to [Upgrading your .NET Mobile Services](app-service-mobile-net-upgrading-from-mobile-services.md).</span></span>

<span data-ttu-id="9d9b5-109">Při upgradu mobilního back-endu do služby Azure App Service má přístup ke všem funkcím služby App Service a se účtují podle [služby App Service – ceny], není Mobile Services ceny.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-109">When a mobile backend is upgraded to Azure App Service, it has access to all App Service features and are billed according to [App Service pricing], not Mobile Services pricing.</span></span>

## <a name="migrate-vs-upgrade"></a><span data-ttu-id="9d9b5-110">Migrace a upgrade</span><span class="sxs-lookup"><span data-stu-id="9d9b5-110">Migrate vs. upgrade</span></span>
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> <span data-ttu-id="9d9b5-111">Je doporučeno, které [provést migraci](app-service-mobile-migrating-from-mobile-services.md) před zahájením upgradu.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-111">It is recommended that you [perform a migration](app-service-mobile-migrating-from-mobile-services.md) before going through an upgrade.</span></span> <span data-ttu-id="9d9b5-112">Tímto způsobem můžete umístit obě verze vaší aplikace stejném plánu služby App Service a způsobit bez dalších nákladů.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-112">This way, you can put both versions of your application on the same App Service Plan and incur no additional cost.</span></span>
>
>

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a><span data-ttu-id="9d9b5-113">Vylepšení v serveru mobilní aplikace Node.js SDK</span><span class="sxs-lookup"><span data-stu-id="9d9b5-113">Improvements in Mobile Apps Node.js server SDK</span></span>
<span data-ttu-id="9d9b5-114">Upgrade na novou [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) poskytuje mnoho vylepšení, včetně:</span><span class="sxs-lookup"><span data-stu-id="9d9b5-114">Upgrading to the new [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) provides a lot of improvements, including:</span></span>

* <span data-ttu-id="9d9b5-115">Na základě [Express framework](http://expressjs.com/en/index.html), novou sadu SDK uzlu je šedé – a navržené tak, aby nové verze uzlu jako se.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-115">Based on the [Express framework](http://expressjs.com/en/index.html), the new Node SDK is light-weight and designed to keep up with new Node versions as they come out.</span></span> <span data-ttu-id="9d9b5-116">Můžete přizpůsobit chování aplikace s Express middleware.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-116">You can customize the application behavior with Express middleware.</span></span>
* <span data-ttu-id="9d9b5-117">Významné zlepšení výkonu ve srovnání s Mobile Services SDK.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-117">Significant performance improvements compared to the Mobile Services SDK.</span></span>
* <span data-ttu-id="9d9b5-118">Web můžete hostovat spolu s vaší mobilní back-end; Podobně je snadno přidat sadu SDK Azure Mobile pro všechny existující express.v4 aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-118">You can now host a website together with your mobile backend; similarly, it's easy to add the Azure Mobile SDK to any existing express.v4 application.</span></span>
* <span data-ttu-id="9d9b5-119">Vytvořené pro vývoj pro různé platformy a místní, Mobile Apps SDK může být vyvinuté a spustit místně na platformách Windows, Linux a OSX.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-119">Built for cross-platform and local development, the Mobile Apps SDK can be developed and run locally on Windows, Linux, and OSX platforms.</span></span> <span data-ttu-id="9d9b5-120">Nyní je snadno použitelný běžné technik vývoje uzlu, jako je spuštění [téměř jako](https://mochajs.org/) testy před jejich nasazením.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-120">It's now easy to use common Node development techniques like running [Mocha](https://mochajs.org/) tests prior to deployment.</span></span>

## <span data-ttu-id="9d9b5-121"><a name="overview"></a>Základní přehled upgradu</span><span class="sxs-lookup"><span data-stu-id="9d9b5-121"><a name="overview"></a>Basic upgrade overview</span></span>
<span data-ttu-id="9d9b5-122">Chcete-li pomoci při upgradování back-end Node.js, Azure App Service poskytl balíček kompatibility.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-122">To aid in upgrading a Node.js backend, Azure App Service has provided a compatibility package.</span></span>  <span data-ttu-id="9d9b5-123">Po upgradu bude mít niew lokalitu, která se dá nasadit na nový Web App Service.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-123">After upgrade, you will have a niew site that can be deployed to a new App Service site.</span></span>

<span data-ttu-id="9d9b5-124">Klientem Mobile Services SDK jsou **není** kompatibilní s novým serverem Mobile Apps SDK.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-124">The Mobile Services client SDKs are **not** compatible with the new Mobile Apps server SDK.</span></span> <span data-ttu-id="9d9b5-125">Chcete-li zajistit kontinuitu služeb pro vaši aplikaci, by neměl publikovat změny v lokalitě aktuálně obsluhující publikované klientů.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-125">In order to provide continuity of service for your app, you should not publish changes to a site currently serving published clients.</span></span> <span data-ttu-id="9d9b5-126">Místo toho by měla vytvoříte novou mobilní aplikaci, která slouží jako duplicitní.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-126">Instead, you should create a new mobile app that serves as a duplicate.</span></span> <span data-ttu-id="9d9b5-127">Tuto aplikaci můžete umístit na stejný plán služby App Service účtovány dodatečné finanční náklady.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-127">You can put this application on the same App Service plan to avoid incurring additional financial cost.</span></span>

<span data-ttu-id="9d9b5-128">Pak bude mít dvě verze této aplikace: ten, který zůstává stejný a slouží v zástupné a druhé, které potom můžete upgradovat a cíl s novou verzi klienta publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-128">You will then have two versions of the application: one that stays the same and serves published apps in the wild, and the other which you can then upgrade and target with a new client release.</span></span> <span data-ttu-id="9d9b5-129">Můžete přesunout a otestujte svůj kód vaší tempem, ale ujistěte se, že všechny opravy chyb, které použije na obě.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-129">You can move and test your code at your pace, but you should make sure that any bug fixes you make get applied to both.</span></span> <span data-ttu-id="9d9b5-130">Jakmile si myslíte, že požadovaný počet klientských aplikací v zástupné byly aktualizovány na nejnovější verzi, můžete odstranit původní migrovanou aplikaci vyžadujete-li.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-130">Once you feel that a desired number of client apps in the wild have updated to the latest version, you can delete the original migrated app if you desire.</span></span> <span data-ttu-id="9d9b5-131">Ho nebude vynakládá žádné další peněžní, pokud hostované ve stejném plán služby App Service jako mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-131">It doesn't incur any additional monetary costs, if hosted in the same App Service plan as your Mobile App.</span></span>

<span data-ttu-id="9d9b5-132">Úplné obrys pro proces upgradu je následující:</span><span class="sxs-lookup"><span data-stu-id="9d9b5-132">The full outline for the upgrade process is as follows:</span></span>

1. <span data-ttu-id="9d9b5-133">Stáhněte si existující Mobile Service (migrované) Azure.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-133">Download your existing (migrated) Azure Mobile Service.</span></span>
2. <span data-ttu-id="9d9b5-134">Převeďte projekt na mobilní aplikace Azure pomocí balíčku pro kompatibilitu.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-134">Convert the project to an Azure Mobile App using the compatibility package.</span></span>
3. <span data-ttu-id="9d9b5-135">Opravte případné rozdíly (například nastavení ověřování).</span><span class="sxs-lookup"><span data-stu-id="9d9b5-135">Correct any differences (such as authentication settings).</span></span>
4. <span data-ttu-id="9d9b5-136">Nasazení projektu převedený mobilní aplikace Azure do nové služby App Service.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-136">Deploy your converted Azure Mobile App project to a new App Service.</span></span>
5. <span data-ttu-id="9d9b5-137">Vydání nové verze klienta aplikace, který pomocí nové mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-137">Release a new version of your client application that use the new Mobile App.</span></span>
6. <span data-ttu-id="9d9b5-138">(Volitelné) Odstraňte původní aplikace migrované mobilní služby.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-138">(Optional) Delete your original migrated mobile service app.</span></span>

<span data-ttu-id="9d9b5-139">Odstranění může dojít, když nevidíte přenosy dat na původní migrované mobilní službě.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-139">Deletion can occur when you don't see any traffic on your original migrated mobile service.</span></span>

## <span data-ttu-id="9d9b5-140"><a name="install-npm-package"></a>Nainstalujte požadavky</span><span class="sxs-lookup"><span data-stu-id="9d9b5-140"><a name="install-npm-package"></a> Install the Pre-requisites</span></span>
<span data-ttu-id="9d9b5-141">[Uzel] musíte nainstalovat na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-141">You should install [Node] on your local machine.</span></span>  <span data-ttu-id="9d9b5-142">Musíte také nainstalovat balíček kompatibility.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-142">You should also install the compatibility package.</span></span>  <span data-ttu-id="9d9b5-143">Po instalaci uzlu spuštěním následujícího příkazu z nové cmd nebo příkazovém řádku prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="9d9b5-143">After Node is installed, you can run the following command from a new cmd or PowerShell prompt:</span></span>

```npm i -g azure-mobile-apps-compatibility```

## <span data-ttu-id="9d9b5-144"><a name="obtain-ams-scripts"></a>Získat skripty Azure Mobile Services</span><span class="sxs-lookup"><span data-stu-id="9d9b5-144"><a name="obtain-ams-scripts"></a> Obtain your Azure Mobile Services Scripts</span></span>
* <span data-ttu-id="9d9b5-145">Přihlaste se k [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="9d9b5-145">Log in to the [Azure Portal].</span></span>
* <span data-ttu-id="9d9b5-146">Pomocí **všechny prostředky** nebo **App Services**, najít váš web Mobile Services.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-146">Using **All Resources** or **App Services**, find your Mobile Services site.</span></span>
* <span data-ttu-id="9d9b5-147">V rámci lokality, klikněte na **nástroje** -> **Kudu** -> **přejděte** otevřete Kudu lokality.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-147">Within the site, click on **Tools** -> **Kudu** -> **Go** to open the Kudu site.</span></span>
* <span data-ttu-id="9d9b5-148">Klikněte na **ladění konzoly** -> **prostředí PowerShell** otevřete konzolu pro ladění.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-148">Click on **Debug Console** -> **PowerShell** to open the Debug console.</span></span>
* <span data-ttu-id="9d9b5-149">Přejděte na `site/wwwroot/App_Data/config` pak kliknutím na každý adresář</span><span class="sxs-lookup"><span data-stu-id="9d9b5-149">Navigate to `site/wwwroot/App_Data/config` by clicking on each directory in turn</span></span>
* <span data-ttu-id="9d9b5-150">Klikněte na ikonu stahování vedle `scripts` adresáře.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-150">Click on the download icon next to the `scripts` directory.</span></span>

<span data-ttu-id="9d9b5-151">To bude stahovat skripty ve formátu ZIP.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-151">This will download the scripts in ZIP format.</span></span>  <span data-ttu-id="9d9b5-152">Vytvořte nový adresář na místním počítači a rozbalte `scripts.ZIP` soubor v adresáři.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-152">Create a new directory on your local machine and unpack the `scripts.ZIP` file within the directory.</span></span>  <span data-ttu-id="9d9b5-153">Tím se vytvoří `scripts` adresáře.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-153">This will create a `scripts` directory.</span></span>

## <span data-ttu-id="9d9b5-154"><a name="scaffold-app"></a>Vygenerovat nový back-end Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="9d9b5-154"><a name="scaffold-app"></a> Scaffold the new Azure Mobile Apps backend</span></span>
<span data-ttu-id="9d9b5-155">Spusťte následující příkaz z adresáře, která obsahuje adresář skriptů:</span><span class="sxs-lookup"><span data-stu-id="9d9b5-155">Run the following command from the directory containing the scripts directory:</span></span>

```scaffold-mobile-app scripts out```

<span data-ttu-id="9d9b5-156">Tím se vytvoří vygenerované Azure Mobile Apps back-end v `out` adresáře.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-156">This will create a scaffolded Azure Mobile Apps backend in the `out` directory.</span></span>  <span data-ttu-id="9d9b5-157">Ačkoli to není vyžadováno, je vhodné zkontrolovat `out` adresář do úložiště zdrojového kódu podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-157">Although not required, it's a good idea to check the `out` directory into a source code repository of your choice.</span></span>

## <span data-ttu-id="9d9b5-158"><a name="deploy-ama-app"></a>Nasazení váš back-end Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="9d9b5-158"><a name="deploy-ama-app"></a> Deploy your Azure Mobile Apps backend</span></span>
<span data-ttu-id="9d9b5-159">Během nasazení budete muset provést následující:</span><span class="sxs-lookup"><span data-stu-id="9d9b5-159">During deployment, you will need to do the following:</span></span>

1. <span data-ttu-id="9d9b5-160">Vytvoření nové mobilní aplikace v [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="9d9b5-160">Create a new Mobile App in the [Azure Portal].</span></span>
2. <span data-ttu-id="9d9b5-161">Spustit `createViews.sql` skript v připojené databázi.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-161">Run the `createViews.sql` script on your connected database.</span></span>
3. <span data-ttu-id="9d9b5-162">Připojit databázi, který bude propojen služby Mobile do služby App Service.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-162">Link the database that is linked to your Mobile Service to your new App Service.</span></span>
4. <span data-ttu-id="9d9b5-163">Žádné další prostředky (například Notification Hubs) propojte nové služby App Service.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-163">Link any other resources (such as Notification Hubs) to the new App Service.</span></span>
5. <span data-ttu-id="9d9b5-164">Nasaďte generovaný kód do nové lokality.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-164">Deploy the generated code to your new site.</span></span>

### <a name="create-a-new-mobile-app"></a><span data-ttu-id="9d9b5-165">Vytvoření nové mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="9d9b5-165">Create a new Mobile App</span></span>
1. <span data-ttu-id="9d9b5-166">Přihlaste se na [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="9d9b5-166">Log in at the [Azure Portal].</span></span>
2. <span data-ttu-id="9d9b5-167">Klikněte na **+NOVÝ** > **Web + mobilní zařízení** > **Mobilní aplikace** a potom zadejte název back-endu mobilní aplikace .</span><span class="sxs-lookup"><span data-stu-id="9d9b5-167">Click **+NEW** > **Web + Mobile** > **Mobile App**, then provide a name for your Mobile App backend.</span></span>
3. <span data-ttu-id="9d9b5-168">V případě **skupiny prostředků** vyberte existující skupinu prostředků nebo vytvořte novou (použijte stejný název, jaký má aplikace).</span><span class="sxs-lookup"><span data-stu-id="9d9b5-168">For the **Resource Group**, select an existing resource group, or create a new one (using the same name as your app.)</span></span>

    <span data-ttu-id="9d9b5-169">Můžete buď vybrat jiný plán služby App Service nebo vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-169">You can either select another App Service plan or create a new one.</span></span> <span data-ttu-id="9d9b5-170">Další informace o App Services plány a postup vytvoření nového plánu v různých cenových vrstvy a v požadovaném umístění najdete v části [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9d9b5-170">For more about App Services plans and how to create a new plan in a different pricing tier and in your desired location, see [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
4. <span data-ttu-id="9d9b5-171">V případě **plánu služby App Service** je vybraný výchozí plán (na [úrovni Standard](https://azure.microsoft.com/pricing/details/app-service/)).</span><span class="sxs-lookup"><span data-stu-id="9d9b5-171">For the **App Service plan**, the default plan (in the [Standard tier](https://azure.microsoft.com/pricing/details/app-service/)) is selected.</span></span> <span data-ttu-id="9d9b5-172">Můžete také vybrat jiný plán nebo [vytvořit nový](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="9d9b5-172">You can also  select a different plan, or [create a new one](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan).</span></span> <span data-ttu-id="9d9b5-173">Nastavení plánu služby App Service určují [umístění, funkce a nákladové a výpočetní prostředky](https://azure.microsoft.com/pricing/details/app-service/) přidružené k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-173">The App Service plan's settings determine the [location, features, cost and compute resources](https://azure.microsoft.com/pricing/details/app-service/) associated with your app.</span></span>

    <span data-ttu-id="9d9b5-174">Až se rozhodnete pro konkrétní plán, klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-174">After you decide on the plan, click **Create**.</span></span> <span data-ttu-id="9d9b5-175">Tím vytvoříte back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-175">This creates the Mobile App backend.</span></span>

### <a name="run-createviewssql"></a><span data-ttu-id="9d9b5-176">Spustit CreateViews.SQL</span><span class="sxs-lookup"><span data-stu-id="9d9b5-176">Run CreateViews.SQL</span></span>
<span data-ttu-id="9d9b5-177">Vygenerované aplikaci, která obsahuje soubor s názvem `createViews.sql`.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-177">The scaffolded app contains a file called `createViews.sql`.</span></span>  <span data-ttu-id="9d9b5-178">Tento skript je třeba spustit v cílové databázi.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-178">This script must be executed against the target database.</span></span>  <span data-ttu-id="9d9b5-179">Nelze získat připojovací řetězec pro cílovou databázi z migrovaných mobilní služby z **nastavení** okno pod **připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-179">The connection string for the target database can be obtained from your migrated mobile service from the **Settings** blade under **Connection Strings**.</span></span>  <span data-ttu-id="9d9b5-180">Je název `MS_TableConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-180">It is named `MS_TableConnectionString`.</span></span>

<span data-ttu-id="9d9b5-181">Můžete spustit tento skript v SQL Server Management Studio nebo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-181">You can run this script from within SQL Server Management Studio or Visual Studio.</span></span>

### <a name="link-the-database-to-your-app-service"></a><span data-ttu-id="9d9b5-182">Propojení databáze do vaší služby App Service</span><span class="sxs-lookup"><span data-stu-id="9d9b5-182">Link the Database to your App Service</span></span>
<span data-ttu-id="9d9b5-183">Připojit existující databázi do vaší služby App Service:</span><span class="sxs-lookup"><span data-stu-id="9d9b5-183">Link the existing database to your App Service:</span></span>

* <span data-ttu-id="9d9b5-184">V [portálu Azure], otevřete App Service.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-184">In the [Azure Portal], open your App Service.</span></span>
* <span data-ttu-id="9d9b5-185">Vyberte **všechna nastavení** -> **datová připojení**.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-185">Select **All Settings** -> **Data Connections**.</span></span>
* <span data-ttu-id="9d9b5-186">Klikněte na **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-186">Click on **+ Add**.</span></span>
* <span data-ttu-id="9d9b5-187">V rozevíracím seznamu vyberte **databáze SQL**</span><span class="sxs-lookup"><span data-stu-id="9d9b5-187">In the drop-down, select **SQL Database**</span></span>
* <span data-ttu-id="9d9b5-188">V části **SQL Database**, vyberte existující databázi a pak klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-188">Under **SQL Database**, select your existing database, then click on **Select**.</span></span>
* <span data-ttu-id="9d9b5-189">V části **připojovací řetězec**, zadejte uživatelské jméno a heslo pro databázi a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-189">Under **Connection string**, enter the username and password for the database, then click on **OK**.</span></span>
* <span data-ttu-id="9d9b5-190">V **přidat datová připojení** okně klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-190">In the **Add data connections** blade, click on **OK**.</span></span>

<span data-ttu-id="9d9b5-191">Uživatelské jméno a heslo lze nalézt zobrazením připojovací řetězec pro cílovou databázi na migrované služby Mobile.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-191">The username and password can be found by viewing the Connection String for the target database in your migrated Mobile Service.</span></span>

### <a name="set-up-authentication"></a><span data-ttu-id="9d9b5-192">Nastavení ověřování</span><span class="sxs-lookup"><span data-stu-id="9d9b5-192">Set up authentication</span></span>
<span data-ttu-id="9d9b5-193">Azure Mobile Apps můžete konfigurovat Azure Active Directory, Facebook, Google, Microsoft a Twitter, ověřování v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-193">Azure Mobile Apps allows you to configure Azure Active Directory, Facebook, Google, Microsoft and Twitter authentication within the service.</span></span>  <span data-ttu-id="9d9b5-194">Vlastní ověřování bude muset být vyvinutá samostatně.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-194">Custom authentication will need to be developed separately.</span></span>  <span data-ttu-id="9d9b5-195">Odkazovat [konceptů ověřování] dokumentaci a [rychlý start ověřování] Další informace naleznete v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-195">Refer to the [Authentication Concepts] documentation and [Authentication Quickstart] documentation for more information.</span></span>  

## <span data-ttu-id="9d9b5-196"><a name="updating-clients"></a>Aktualizace mobilních klientů</span><span class="sxs-lookup"><span data-stu-id="9d9b5-196"><a name="updating-clients"></a>Update Mobile clients</span></span>
<span data-ttu-id="9d9b5-197">Až budete mít provozní back-end mobilní aplikace, můžete pracovat na novou verzi klientskou aplikaci, která se využívá.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-197">Once you have an operational Mobile App backend, you can work on a new version of your client application which consumes it.</span></span> <span data-ttu-id="9d9b5-198">Mobilní aplikace také zahrnuje novou verzi klienta sady SDK a podobně jako u upgrade serveru výše, budete muset odebrat všechny odkazy na sady Mobile Services SDK před instalací verze mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-198">Mobile Apps also includes a new version of the client SDKs, and similar to the server upgrade above, you will need to remove all references to the Mobile Services SDKs before installing the Mobile Apps versions.</span></span>

<span data-ttu-id="9d9b5-199">Jednou z hlavních změn mezi verzemi je, že konstruktorů už nebudou potřebovat klíčem aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-199">One of the main changes between the versions is that the constructors no longer require an application key.</span></span>
<span data-ttu-id="9d9b5-200">Nyní jednoduše předáte v adrese URL mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-200">You now simply pass in the URL of your Mobile App.</span></span> <span data-ttu-id="9d9b5-201">Například na klientů .NET `MobileServiceClient` konstruktor je nyní:</span><span class="sxs-lookup"><span data-stu-id="9d9b5-201">For example, on the .NET clients, the `MobileServiceClient` constructor is now:</span></span>

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net" // URL of the Mobile App
        );

<span data-ttu-id="9d9b5-202">Další informace o instalaci nové sady SDK a použití nového strukturu přes odkazy níže:</span><span class="sxs-lookup"><span data-stu-id="9d9b5-202">You can read about installing the new SDKs and using the new structure via the links below:</span></span>

* [<span data-ttu-id="9d9b5-203">Verzi systému Android 2.2 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="9d9b5-203">Android version 2.2 or later</span></span>](app-service-mobile-android-how-to-use-client-library.md)
* [<span data-ttu-id="9d9b5-204">iOS verze 3.0.0 nebo novější</span><span class="sxs-lookup"><span data-stu-id="9d9b5-204">iOS version 3.0.0 or later</span></span>](app-service-mobile-ios-how-to-use-client-library.md)
* [<span data-ttu-id="9d9b5-205">Rozhraní .NET (Windows nebo Xamarin) verze 2.0.0 nebo novější</span><span class="sxs-lookup"><span data-stu-id="9d9b5-205">.NET (Windows/Xamarin) version 2.0.0 or later</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)
* [<span data-ttu-id="9d9b5-206">Apache Cordova verze 2.0 nebo novější</span><span class="sxs-lookup"><span data-stu-id="9d9b5-206">Apache Cordova version 2.0 or later</span></span>](app-service-mobile-cordova-how-to-use-client-library.md)

<span data-ttu-id="9d9b5-207">Pokud vaše aplikace provede pomocí nabízených oznámení, poznamenejte si konkrétní registrace pokyny pro každou platformu, protože byly některé změny existuje také.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-207">If your application makes use of push notifications, make note of the specific registration instructions for each platform, as there have been some changes there as well.</span></span>

<span data-ttu-id="9d9b5-208">Pokud máte novou verzi klienta, která je připraveno, vyzkoušejte ji proti projektu upgradovaný server.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-208">When you have the new client version ready, try it out against your upgraded server project.</span></span> <span data-ttu-id="9d9b5-209">Po ověření, že bude fungovat, můžete vydat novou verzi vaší aplikace pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-209">After validating that it works, you can release a new version of your application to customers.</span></span> <span data-ttu-id="9d9b5-210">Nakonec Jakmile vaši zákazníci jste využili příležitost a získat tyto aktualizace, můžete odstranit Mobile Services verzi vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-210">Eventually, once your customers have had a chance to receive these updates, you can delete the Mobile Services version of your app.</span></span> <span data-ttu-id="9d9b5-211">V tomto okamžiku úplně upgradu na aplikaci služby mobilní aplikace pomocí nejnovější Mobile Apps serveru SDK.</span><span class="sxs-lookup"><span data-stu-id="9d9b5-211">At this point, you have completely upgraded to an App Service Mobile App using the latest Mobile Apps server SDK.</span></span>

<!-- URLs. -->

<span data-ttu-id="9d9b5-212">[Azure Portal]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="9d9b5-212">[Azure portal]: https://portal.azure.com/</span></span>
[Azure classic portal]: https://manage.windowsazure.com/
<span data-ttu-id="9d9b5-213">[co jsou Mobile Apps?]: app-service-mobile-value-prop.md</span><span class="sxs-lookup"><span data-stu-id="9d9b5-213">[What are Mobile Apps?]: app-service-mobile-value-prop.md</span></span>
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
<span data-ttu-id="9d9b5-214">[služby App Service – ceny]: https://azure.microsoft.com/en-us/pricing/details/app-service/</span><span class="sxs-lookup"><span data-stu-id="9d9b5-214">[App Service pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/</span></span>
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
<span data-ttu-id="9d9b5-215">[konceptů ověřování]: ../app-service/app-service-authentication-overview.md</span><span class="sxs-lookup"><span data-stu-id="9d9b5-215">[Authentication Concepts]: ../app-service/app-service-authentication-overview.md</span></span>
<span data-ttu-id="9d9b5-216">[rychlý start ověřování]: app-service-mobile-auth.md</span><span class="sxs-lookup"><span data-stu-id="9d9b5-216">[Authentication Quickstart]: app-service-mobile-auth.md</span></span>

<span data-ttu-id="9d9b5-217">[portálu Azure]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="9d9b5-217">[Azure Portal]: https://portal.azure.com/</span></span>
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
