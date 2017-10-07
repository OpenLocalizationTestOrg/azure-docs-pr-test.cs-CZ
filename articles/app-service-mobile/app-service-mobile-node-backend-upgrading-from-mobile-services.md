---
title: "aaaUpgrade z mobilní služby tooAzure aplikační služby - Node.js"
description: "Zjistěte, jak tooeasily upgradu vaší aplikace tooan s Mobile Services aplikace služby mobilní aplikace"
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
ms.openlocfilehash: 722cda244d4f633247827f58ea6f1397137ea600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-existing-nodejs-azure-mobile-service-tooapp-service"></a><span data-ttu-id="85aa4-103">Upgrade existující Node.js Azure Mobile Service tooApp služby</span><span class="sxs-lookup"><span data-stu-id="85aa4-103">Upgrade your existing Node.js Azure Mobile Service tooApp Service</span></span>
<span data-ttu-id="85aa4-104">Mobile App Service je nový způsob toobuild mobilních aplikací pomocí Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="85aa4-104">App Service Mobile is a new way toobuild mobile applications using Microsoft Azure.</span></span> <span data-ttu-id="85aa4-105">Další, najdete v části toolearn [co jsou Mobile Apps?].</span><span class="sxs-lookup"><span data-stu-id="85aa4-105">toolearn more, see [What are Mobile Apps?].</span></span>

<span data-ttu-id="85aa4-106">Toto téma popisuje, jak tooupgrade existující aplikaci back-end Node.js z Azure Mobile Services tooa nové App Service Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="85aa4-106">This topic describes how tooupgrade an existing Node.js backend application from Azure Mobile Services tooa new App Service Mobile Apps.</span></span> <span data-ttu-id="85aa4-107">Při provádění tohoto upgradu, aplikace pro Mobile Services můžete dál toooperate.</span><span class="sxs-lookup"><span data-stu-id="85aa4-107">While you perform this upgrade, your existing Mobile Services application can continue toooperate.</span></span>  <span data-ttu-id="85aa4-108">Pokud potřebujete tooupgrade aplikace back-end Node.js, podívejte se příliš[upgradu vaší .NET Mobile Services](app-service-mobile-net-upgrading-from-mobile-services.md).</span><span class="sxs-lookup"><span data-stu-id="85aa4-108">If you need tooupgrade a Node.js backend application, refer too[Upgrading your .NET Mobile Services](app-service-mobile-net-upgrading-from-mobile-services.md).</span></span>

<span data-ttu-id="85aa4-109">Když mobilní back-end je upgradovaná tooAzure služby App Service, má přístup tooall funkce služby App Service a se účtují podle příliš[služby App Service – ceny], není Mobile Services ceny.</span><span class="sxs-lookup"><span data-stu-id="85aa4-109">When a mobile backend is upgraded tooAzure App Service, it has access tooall App Service features and are billed according too[App Service pricing], not Mobile Services pricing.</span></span>

## <a name="migrate-vs-upgrade"></a><span data-ttu-id="85aa4-110">Migrace a upgrade</span><span class="sxs-lookup"><span data-stu-id="85aa4-110">Migrate vs. upgrade</span></span>
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> <span data-ttu-id="85aa4-111">Je doporučeno, které [provést migraci](app-service-mobile-migrating-from-mobile-services.md) před zahájením upgradu.</span><span class="sxs-lookup"><span data-stu-id="85aa4-111">It is recommended that you [perform a migration](app-service-mobile-migrating-from-mobile-services.md) before going through an upgrade.</span></span> <span data-ttu-id="85aa4-112">Tímto způsobem můžete vložit obě verze aplikace hello stejný plán služby App Service a způsobit bez dalších nákladů.</span><span class="sxs-lookup"><span data-stu-id="85aa4-112">This way, you can put both versions of your application on hello same App Service Plan and incur no additional cost.</span></span>
>
>

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a><span data-ttu-id="85aa4-113">Vylepšení v serveru mobilní aplikace Node.js SDK</span><span class="sxs-lookup"><span data-stu-id="85aa4-113">Improvements in Mobile Apps Node.js server SDK</span></span>
<span data-ttu-id="85aa4-114">Upgrade toohello nové [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) poskytuje mnoho vylepšení, včetně:</span><span class="sxs-lookup"><span data-stu-id="85aa4-114">Upgrading toohello new [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) provides a lot of improvements, including:</span></span>

* <span data-ttu-id="85aa4-115">Podle hello [Express framework](http://expressjs.com/en/index.html), hello novou sadu SDK uzlu je šedé – a je navržená tak tookeep si nové verze uzlu jako pocházejí. Můžete přizpůsobit chování aplikace hello s Express middleware.</span><span class="sxs-lookup"><span data-stu-id="85aa4-115">Based on hello [Express framework](http://expressjs.com/en/index.html), hello new Node SDK is light-weight and designed tookeep up with new Node versions as they come out. You can customize hello application behavior with Express middleware.</span></span>
* <span data-ttu-id="85aa4-116">Významné zlepšení výkonu porovná toohello Mobile Services SDK.</span><span class="sxs-lookup"><span data-stu-id="85aa4-116">Significant performance improvements compared toohello Mobile Services SDK.</span></span>
* <span data-ttu-id="85aa4-117">Web můžete hostovat spolu s vaší mobilní back-end; Podobně je snadno tooadd hello Azure Mobile SDK tooany existující express.v4 aplikace.</span><span class="sxs-lookup"><span data-stu-id="85aa4-117">You can now host a website together with your mobile backend; similarly, it's easy tooadd hello Azure Mobile SDK tooany existing express.v4 application.</span></span>
* <span data-ttu-id="85aa4-118">Vytvořené pro vývoj pro různé platformy a místní, hello Mobile Apps SDK může být vyvinuté a spustit místně na platformách Windows, Linux a OSX.</span><span class="sxs-lookup"><span data-stu-id="85aa4-118">Built for cross-platform and local development, hello Mobile Apps SDK can be developed and run locally on Windows, Linux, and OSX platforms.</span></span> <span data-ttu-id="85aa4-119">Nyní je snadno toouse běžné uzlu vývoj techniky jako spuštěná [téměř jako](https://mochajs.org/) testy předchozí toodeployment.</span><span class="sxs-lookup"><span data-stu-id="85aa4-119">It's now easy toouse common Node development techniques like running [Mocha](https://mochajs.org/) tests prior toodeployment.</span></span>

## <span data-ttu-id="85aa4-120"><a name="overview"></a>Základní přehled upgradu</span><span class="sxs-lookup"><span data-stu-id="85aa4-120"><a name="overview"></a>Basic upgrade overview</span></span>
<span data-ttu-id="85aa4-121">tooaid v rámci aktualizace back-end Node.js, Azure App Service poskytl balíček kompatibility.</span><span class="sxs-lookup"><span data-stu-id="85aa4-121">tooaid in upgrading a Node.js backend, Azure App Service has provided a compatibility package.</span></span>  <span data-ttu-id="85aa4-122">Po upgradu budete mít niew lokality, které můžou být nasazené tooa nový Web App Service.</span><span class="sxs-lookup"><span data-stu-id="85aa4-122">After upgrade, you will have a niew site that can be deployed tooa new App Service site.</span></span>

<span data-ttu-id="85aa4-123">Hello klientem Mobile Services SDK jsou **není** kompatibilní s hello nový Mobile Apps server SDK.</span><span class="sxs-lookup"><span data-stu-id="85aa4-123">hello Mobile Services client SDKs are **not** compatible with hello new Mobile Apps server SDK.</span></span> <span data-ttu-id="85aa4-124">V pořadí tooprovide kontinuitu poskytování služeb pro vaši aplikaci by neměl publikovat změny tooa lokality aktuálně obsluhující publikované klientů.</span><span class="sxs-lookup"><span data-stu-id="85aa4-124">In order tooprovide continuity of service for your app, you should not publish changes tooa site currently serving published clients.</span></span> <span data-ttu-id="85aa4-125">Místo toho by měla vytvoříte novou mobilní aplikaci, která slouží jako duplicitní.</span><span class="sxs-lookup"><span data-stu-id="85aa4-125">Instead, you should create a new mobile app that serves as a duplicate.</span></span> <span data-ttu-id="85aa4-126">Tuto aplikaci můžete umístit na hello stejné služby App Service plánování tooavoid by docházelo k další finanční náklady.</span><span class="sxs-lookup"><span data-stu-id="85aa4-126">You can put this application on hello same App Service plan tooavoid incurring additional financial cost.</span></span>

<span data-ttu-id="85aa4-127">Pak bude mít dvě verze aplikace hello: ten, který zůstává stejný hello slouží publikované aplikace v hello zástupné a jiných, která pak můžete upgradovat a cíl s nového klienta verze.</span><span class="sxs-lookup"><span data-stu-id="85aa4-127">You will then have two versions of hello application: one that stays hello same and serves published apps in hello wild, and the other which you can then upgrade and target with a new client release.</span></span> <span data-ttu-id="85aa4-128">Můžete přesunout a otestujte svůj kód vaší tempem, ale ujistěte se, že všechny opravy chyb, které provedete získat použité tooboth.</span><span class="sxs-lookup"><span data-stu-id="85aa4-128">You can move and test your code at your pace, but you should make sure that any bug fixes you make get applied tooboth.</span></span> <span data-ttu-id="85aa4-129">Jakmile si myslíte, že požadovaný počet klientských aplikací v zástupné aktualizovaly toohello nejnovější verzi, můžete odstranit původní migrovanou aplikaci hello vyžadujete-li.</span><span class="sxs-lookup"><span data-stu-id="85aa4-129">Once you feel that a desired number of client apps in the wild have updated toohello latest version, you can delete hello original migrated app if you desire.</span></span> <span data-ttu-id="85aa4-130">Ji není vynakládá žádné další peněžní, pokud je hostovaná v hello stejné služby App Service plánování jako mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="85aa4-130">It doesn't incur any additional monetary costs, if hosted in hello same App Service plan as your Mobile App.</span></span>

<span data-ttu-id="85aa4-131">Hello úplné osnovy pro proces upgradu hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="85aa4-131">hello full outline for hello upgrade process is as follows:</span></span>

1. <span data-ttu-id="85aa4-132">Stáhněte si existující Mobile Service (migrované) Azure.</span><span class="sxs-lookup"><span data-stu-id="85aa4-132">Download your existing (migrated) Azure Mobile Service.</span></span>
2. <span data-ttu-id="85aa4-133">Převést hello projektu tooan mobilní aplikace Azure pomocí balíčku kompatibility hello.</span><span class="sxs-lookup"><span data-stu-id="85aa4-133">Convert hello project tooan Azure Mobile App using hello compatibility package.</span></span>
3. <span data-ttu-id="85aa4-134">Opravte případné rozdíly (například nastavení ověřování).</span><span class="sxs-lookup"><span data-stu-id="85aa4-134">Correct any differences (such as authentication settings).</span></span>
4. <span data-ttu-id="85aa4-135">Nasazení vaší převedený projektu tooa mobilní aplikace Azure nové služby App Service.</span><span class="sxs-lookup"><span data-stu-id="85aa4-135">Deploy your converted Azure Mobile App project tooa new App Service.</span></span>
5. <span data-ttu-id="85aa4-136">Vydání nové verze klienta aplikace, použijte hello nové mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="85aa4-136">Release a new version of your client application that use hello new Mobile App.</span></span>
6. <span data-ttu-id="85aa4-137">(Volitelné) Odstraňte původní aplikace migrované mobilní služby.</span><span class="sxs-lookup"><span data-stu-id="85aa4-137">(Optional) Delete your original migrated mobile service app.</span></span>

<span data-ttu-id="85aa4-138">Odstranění může dojít, když nevidíte přenosy dat na původní migrované mobilní službě.</span><span class="sxs-lookup"><span data-stu-id="85aa4-138">Deletion can occur when you don't see any traffic on your original migrated mobile service.</span></span>

## <span data-ttu-id="85aa4-139"><a name="install-npm-package"></a>Nainstalujte požadavky hello</span><span class="sxs-lookup"><span data-stu-id="85aa4-139"><a name="install-npm-package"></a> Install hello Pre-requisites</span></span>
<span data-ttu-id="85aa4-140">[Uzel] musíte nainstalovat na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="85aa4-140">You should install [Node] on your local machine.</span></span>  <span data-ttu-id="85aa4-141">Také byste měli nainstalovat balíček kompatibility hello.</span><span class="sxs-lookup"><span data-stu-id="85aa4-141">You should also install hello compatibility package.</span></span>  <span data-ttu-id="85aa4-142">Po instalaci uzlu můžete spustit hello z nové cmd nebo příkazovém řádku prostředí PowerShell následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="85aa4-142">After Node is installed, you can run hello following command from a new cmd or PowerShell prompt:</span></span>

```npm i -g azure-mobile-apps-compatibility```

## <span data-ttu-id="85aa4-143"><a name="obtain-ams-scripts"></a>Získat skripty Azure Mobile Services</span><span class="sxs-lookup"><span data-stu-id="85aa4-143"><a name="obtain-ams-scripts"></a> Obtain your Azure Mobile Services Scripts</span></span>
* <span data-ttu-id="85aa4-144">Přihlaste se toohello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="85aa4-144">Log in toohello [Azure Portal].</span></span>
* <span data-ttu-id="85aa4-145">Pomocí **všechny prostředky** nebo **App Services**, najít váš web Mobile Services.</span><span class="sxs-lookup"><span data-stu-id="85aa4-145">Using **All Resources** or **App Services**, find your Mobile Services site.</span></span>
* <span data-ttu-id="85aa4-146">V rámci lokality hello, klikněte na **nástroje** -> **Kudu** -> **přejděte** tooopen hello Kudu lokality.</span><span class="sxs-lookup"><span data-stu-id="85aa4-146">Within hello site, click on **Tools** -> **Kudu** -> **Go** tooopen hello Kudu site.</span></span>
* <span data-ttu-id="85aa4-147">Klikněte na **ladění konzoly** -> **prostředí PowerShell** konzolou pro ladění tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="85aa4-147">Click on **Debug Console** -> **PowerShell** tooopen hello Debug console.</span></span>
* <span data-ttu-id="85aa4-148">Přejděte příliš`site/wwwroot/App_Data/config` pak kliknutím na každý adresář</span><span class="sxs-lookup"><span data-stu-id="85aa4-148">Navigate too`site/wwwroot/App_Data/config` by clicking on each directory in turn</span></span>
* <span data-ttu-id="85aa4-149">Klikněte na další toohello ikonu hello stažení `scripts` adresáře.</span><span class="sxs-lookup"><span data-stu-id="85aa4-149">Click on hello download icon next toohello `scripts` directory.</span></span>

<span data-ttu-id="85aa4-150">To bude stahovat hello skripty ve formátu ZIP.</span><span class="sxs-lookup"><span data-stu-id="85aa4-150">This will download hello scripts in ZIP format.</span></span>  <span data-ttu-id="85aa4-151">Vytvořte nový adresář na místním počítači a rozbalte hello `scripts.ZIP` soubor v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="85aa4-151">Create a new directory on your local machine and unpack hello `scripts.ZIP` file within hello directory.</span></span>  <span data-ttu-id="85aa4-152">Tím se vytvoří `scripts` adresáře.</span><span class="sxs-lookup"><span data-stu-id="85aa4-152">This will create a `scripts` directory.</span></span>

## <span data-ttu-id="85aa4-153"><a name="scaffold-app"></a>Vygenerované uživatelské rozhraní hello nový Azure Mobile Apps back-end</span><span class="sxs-lookup"><span data-stu-id="85aa4-153"><a name="scaffold-app"></a> Scaffold hello new Azure Mobile Apps backend</span></span>
<span data-ttu-id="85aa4-154">Spusťte následující příkaz z adresáře hello, která obsahuje adresář skripty hello hello:</span><span class="sxs-lookup"><span data-stu-id="85aa4-154">Run hello following command from hello directory containing hello scripts directory:</span></span>

```scaffold-mobile-app scripts out```

<span data-ttu-id="85aa4-155">Tím se vytvoří vygenerované back-end Azure Mobile Apps v hello `out` adresáře.</span><span class="sxs-lookup"><span data-stu-id="85aa4-155">This will create a scaffolded Azure Mobile Apps backend in hello `out` directory.</span></span>  <span data-ttu-id="85aa4-156">Ačkoli to není vyžadováno, je vhodné toocheck hello `out` adresář do úložiště zdrojového kódu podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="85aa4-156">Although not required, it's a good idea toocheck hello `out` directory into a source code repository of your choice.</span></span>

## <span data-ttu-id="85aa4-157"><a name="deploy-ama-app"></a>Nasazení váš back-end Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="85aa4-157"><a name="deploy-ama-app"></a> Deploy your Azure Mobile Apps backend</span></span>
<span data-ttu-id="85aa4-158">Během nasazení budete potřebovat toodo hello následující:</span><span class="sxs-lookup"><span data-stu-id="85aa4-158">During deployment, you will need toodo hello following:</span></span>

1. <span data-ttu-id="85aa4-159">Vytvoření nové mobilní aplikace v hello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="85aa4-159">Create a new Mobile App in hello [Azure Portal].</span></span>
2. <span data-ttu-id="85aa4-160">Spustit hello `createViews.sql` skript v připojené databázi.</span><span class="sxs-lookup"><span data-stu-id="85aa4-160">Run hello `createViews.sql` script on your connected database.</span></span>
3. <span data-ttu-id="85aa4-161">Odkaz hello databázi, která je propojená tooyour Mobile Service tooyour nové služby App Service.</span><span class="sxs-lookup"><span data-stu-id="85aa4-161">Link hello database that is linked tooyour Mobile Service tooyour new App Service.</span></span>
4. <span data-ttu-id="85aa4-162">Odkaz jiné prostředky (například Notification Hubs) toohello nové služby App Service.</span><span class="sxs-lookup"><span data-stu-id="85aa4-162">Link any other resources (such as Notification Hubs) toohello new App Service.</span></span>
5. <span data-ttu-id="85aa4-163">Nasaďte nové lokality tooyour hello vygenerovat kód.</span><span class="sxs-lookup"><span data-stu-id="85aa4-163">Deploy hello generated code tooyour new site.</span></span>

### <a name="create-a-new-mobile-app"></a><span data-ttu-id="85aa4-164">Vytvoření nové mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="85aa4-164">Create a new Mobile App</span></span>
1. <span data-ttu-id="85aa4-165">Přihlaste se na hello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="85aa4-165">Log in at hello [Azure Portal].</span></span>
2. <span data-ttu-id="85aa4-166">Klikněte na **+NOVÝ** > **Web + mobilní zařízení** > **Mobilní aplikace** a potom zadejte název back-endu mobilní aplikace .</span><span class="sxs-lookup"><span data-stu-id="85aa4-166">Click **+NEW** > **Web + Mobile** > **Mobile App**, then provide a name for your Mobile App backend.</span></span>
3. <span data-ttu-id="85aa4-167">Pro hello **skupiny prostředků**, vyberte existující skupinu prostředků nebo vytvořte novou (pomocí hello stejný název jako vaše aplikace.)</span><span class="sxs-lookup"><span data-stu-id="85aa4-167">For hello **Resource Group**, select an existing resource group, or create a new one (using hello same name as your app.)</span></span>

    <span data-ttu-id="85aa4-168">Můžete buď vybrat jiný plán služby App Service nebo vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="85aa4-168">You can either select another App Service plan or create a new one.</span></span> <span data-ttu-id="85aa4-169">Další informace o plánech aplikační služby a jak toocreate nového plánu v různých cenových vrstvy a v požadovaném umístění najdete v části [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="85aa4-169">For more about App Services plans and how toocreate a new plan in a different pricing tier and in your desired location, see [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
4. <span data-ttu-id="85aa4-170">Pro hello **plán služby App Service**, hello výchozí plán (v hello [úrovně Standard](https://azure.microsoft.com/pricing/details/app-service/)) je vybrána.</span><span class="sxs-lookup"><span data-stu-id="85aa4-170">For hello **App Service plan**, hello default plan (in hello [Standard tier](https://azure.microsoft.com/pricing/details/app-service/)) is selected.</span></span> <span data-ttu-id="85aa4-171">Můžete také vybrat jiný plán nebo [vytvořit nový](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="85aa4-171">You can also  select a different plan, or [create a new one](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan).</span></span> <span data-ttu-id="85aa4-172">Hello plán služby App Service určuje nastavení hello [umístění, funkce a nákladové a výpočetní prostředky](https://azure.microsoft.com/pricing/details/app-service/) přidružené k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="85aa4-172">hello App Service plan's settings determine hello [location, features, cost and compute resources](https://azure.microsoft.com/pricing/details/app-service/) associated with your app.</span></span>

    <span data-ttu-id="85aa4-173">Jakmile se rozhodnete hello plán, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="85aa4-173">After you decide on hello plan, click **Create**.</span></span> <span data-ttu-id="85aa4-174">Tím vytvoříte back-end mobilní aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="85aa4-174">This creates hello Mobile App backend.</span></span>

### <a name="run-createviewssql"></a><span data-ttu-id="85aa4-175">Spustit CreateViews.SQL</span><span class="sxs-lookup"><span data-stu-id="85aa4-175">Run CreateViews.SQL</span></span>
<span data-ttu-id="85aa4-176">Hello vygenerované aplikaci, která obsahuje soubor s názvem `createViews.sql`.</span><span class="sxs-lookup"><span data-stu-id="85aa4-176">hello scaffolded app contains a file called `createViews.sql`.</span></span>  <span data-ttu-id="85aa4-177">Tento skript je třeba spustit v cílové databázi.</span><span class="sxs-lookup"><span data-stu-id="85aa4-177">This script must be executed against the target database.</span></span>  <span data-ttu-id="85aa4-178">Hello připojovací řetězec pro hello cílová databáze můžete získat z migrovaných mobilní služby z hello **nastavení** okno pod **připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="85aa4-178">hello connection string for hello target database can be obtained from your migrated mobile service from hello **Settings** blade under **Connection Strings**.</span></span>  <span data-ttu-id="85aa4-179">Je název `MS_TableConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="85aa4-179">It is named `MS_TableConnectionString`.</span></span>

<span data-ttu-id="85aa4-180">Můžete spustit tento skript v SQL Server Management Studio nebo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="85aa4-180">You can run this script from within SQL Server Management Studio or Visual Studio.</span></span>

### <a name="link-hello-database-tooyour-app-service"></a><span data-ttu-id="85aa4-181">Odkaz hello tooyour databáze služby App Service</span><span class="sxs-lookup"><span data-stu-id="85aa4-181">Link hello Database tooyour App Service</span></span>
<span data-ttu-id="85aa4-182">Odkaz hello existující databáze tooyour služby App Service:</span><span class="sxs-lookup"><span data-stu-id="85aa4-182">Link hello existing database tooyour App Service:</span></span>

* <span data-ttu-id="85aa4-183">V hello [portálu Azure], otevřete App Service.</span><span class="sxs-lookup"><span data-stu-id="85aa4-183">In hello [Azure Portal], open your App Service.</span></span>
* <span data-ttu-id="85aa4-184">Vyberte **všechna nastavení** -> **datová připojení**.</span><span class="sxs-lookup"><span data-stu-id="85aa4-184">Select **All Settings** -> **Data Connections**.</span></span>
* <span data-ttu-id="85aa4-185">Klikněte na **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="85aa4-185">Click on **+ Add**.</span></span>
* <span data-ttu-id="85aa4-186">V hello rozevíracího seznamu vyberte **databáze SQL**</span><span class="sxs-lookup"><span data-stu-id="85aa4-186">In hello drop-down, select **SQL Database**</span></span>
* <span data-ttu-id="85aa4-187">V části **SQL Database**, vyberte existující databázi a pak klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="85aa4-187">Under **SQL Database**, select your existing database, then click on **Select**.</span></span>
* <span data-ttu-id="85aa4-188">V části **připojovací řetězec**, zadejte hello uživatelské jméno a heslo pro hello databáze a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="85aa4-188">Under **Connection string**, enter hello username and password for hello database, then click on **OK**.</span></span>
* <span data-ttu-id="85aa4-189">V hello **přidat datová připojení** okně klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="85aa4-189">In hello **Add data connections** blade, click on **OK**.</span></span>

<span data-ttu-id="85aa4-190">zobrazením hello připojovací řetězec pro hello cílová databáze ve službě migrované Mobile lze nalézt Hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="85aa4-190">hello username and password can be found by viewing hello Connection String for hello target database in your migrated Mobile Service.</span></span>

### <a name="set-up-authentication"></a><span data-ttu-id="85aa4-191">Nastavení ověřování</span><span class="sxs-lookup"><span data-stu-id="85aa4-191">Set up authentication</span></span>
<span data-ttu-id="85aa4-192">Azure Mobile Apps umožňuje tooconfigure Azure Active Directory, Facebook, Google, Microsoft a Twitter, ověřování v rámci služby hello.</span><span class="sxs-lookup"><span data-stu-id="85aa4-192">Azure Mobile Apps allows you tooconfigure Azure Active Directory, Facebook, Google, Microsoft and Twitter authentication within hello service.</span></span>  <span data-ttu-id="85aa4-193">Vlastní ověřování bude potřebovat toobe vyvinuté samostatně.</span><span class="sxs-lookup"><span data-stu-id="85aa4-193">Custom authentication will need toobe developed separately.</span></span>  <span data-ttu-id="85aa4-194">Odkazovat na hello [konceptů ověřování] dokumentaci a [rychlý start ověřování] Další informace naleznete v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="85aa4-194">Refer to hello [Authentication Concepts] documentation and [Authentication Quickstart] documentation for more information.</span></span>  

## <span data-ttu-id="85aa4-195"><a name="updating-clients"></a>Aktualizace mobilních klientů</span><span class="sxs-lookup"><span data-stu-id="85aa4-195"><a name="updating-clients"></a>Update Mobile clients</span></span>
<span data-ttu-id="85aa4-196">Až budete mít provozní back-end mobilní aplikace, můžete pracovat na novou verzi klientskou aplikaci, která se využívá.</span><span class="sxs-lookup"><span data-stu-id="85aa4-196">Once you have an operational Mobile App backend, you can work on a new version of your client application which consumes it.</span></span> <span data-ttu-id="85aa4-197">Mobilní aplikace také zahrnuje novou verzi klienta hello sady SDK a podobné upgrade serveru v toohello výše, budete potřebovat tooremove všech odkazech toohello Mobile Services SDK před instalací verze mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="85aa4-197">Mobile Apps also includes a new version of hello client SDKs, and similar toohello server upgrade above, you will need tooremove all references toohello Mobile Services SDKs before installing the Mobile Apps versions.</span></span>

<span data-ttu-id="85aa4-198">Jednou z hlavních změn hello mezi verzemi hello je hello konstruktory už nebudou potřebovat klíč aplikace.</span><span class="sxs-lookup"><span data-stu-id="85aa4-198">One of hello main changes between hello versions is that hello constructors no longer require an application key.</span></span>
<span data-ttu-id="85aa4-199">Nyní jednoduše předáte v adrese URL hello mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="85aa4-199">You now simply pass in hello URL of your Mobile App.</span></span> <span data-ttu-id="85aa4-200">Například na hello klientů .NET, hello `MobileServiceClient` konstruktor je nyní:</span><span class="sxs-lookup"><span data-stu-id="85aa4-200">For example, on hello .NET clients, hello `MobileServiceClient` constructor is now:</span></span>

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net" // URL of hello Mobile App
        );

<span data-ttu-id="85aa4-201">Další informace o instalaci hello nové sady SDK a použitím hello nové struktury prostřednictvím hello odkazy níže:</span><span class="sxs-lookup"><span data-stu-id="85aa4-201">You can read about installing hello new SDKs and using hello new structure via hello links below:</span></span>

* [<span data-ttu-id="85aa4-202">Verzi systému Android 2.2 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="85aa4-202">Android version 2.2 or later</span></span>](app-service-mobile-android-how-to-use-client-library.md)
* [<span data-ttu-id="85aa4-203">iOS verze 3.0.0 nebo novější</span><span class="sxs-lookup"><span data-stu-id="85aa4-203">iOS version 3.0.0 or later</span></span>](app-service-mobile-ios-how-to-use-client-library.md)
* [<span data-ttu-id="85aa4-204">Rozhraní .NET (Windows nebo Xamarin) verze 2.0.0 nebo novější</span><span class="sxs-lookup"><span data-stu-id="85aa4-204">.NET (Windows/Xamarin) version 2.0.0 or later</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)
* [<span data-ttu-id="85aa4-205">Apache Cordova verze 2.0 nebo novější</span><span class="sxs-lookup"><span data-stu-id="85aa4-205">Apache Cordova version 2.0 or later</span></span>](app-service-mobile-cordova-how-to-use-client-library.md)

<span data-ttu-id="85aa4-206">Pokud vaše aplikace provede pomocí nabízených oznámení, poznamenejte si hello registrace konkrétní pokyny pro každou platformu, protože byly některé změny existuje také.</span><span class="sxs-lookup"><span data-stu-id="85aa4-206">If your application makes use of push notifications, make note of hello specific registration instructions for each platform, as there have been some changes there as well.</span></span>

<span data-ttu-id="85aa4-207">Až budete mít hello novou verzi klienta připraven, vyzkoušejte ji proti projektu upgradovaný server.</span><span class="sxs-lookup"><span data-stu-id="85aa4-207">When you have hello new client version ready, try it out against your upgraded server project.</span></span> <span data-ttu-id="85aa4-208">Po ověření, že bude fungovat, můžete vydat novou verzi toocustomers vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="85aa4-208">After validating that it works, you can release a new version of your application toocustomers.</span></span> <span data-ttu-id="85aa4-209">Nakonec Jakmile vaši zákazníci mohli dříve prvního tooreceive těchto aktualizací, můžete odstranit hello Mobile Services verzi vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="85aa4-209">Eventually, once your customers have had a chance tooreceive these updates, you can delete hello Mobile Services version of your app.</span></span> <span data-ttu-id="85aa4-210">V tomto okamžiku jste zcela upgradovali tooan aplikace služby mobilní aplikace pomocí hello nejnovější Mobile Apps serveru SDK.</span><span class="sxs-lookup"><span data-stu-id="85aa4-210">At this point, you have completely upgraded tooan App Service Mobile App using hello latest Mobile Apps server SDK.</span></span>

<!-- URLs. -->

[Azure Portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[co jsou Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How toouse hello .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services tooan App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service tooApp Service]: app-service-mobile-migrating-from-mobile-services.md
[služby App Service – ceny]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[konceptů ověřování]: ../app-service/app-service-authentication-overview.md
[rychlý start ověřování]: app-service-mobile-auth.md

[portálu Azure]: https://portal.azure.com/
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
