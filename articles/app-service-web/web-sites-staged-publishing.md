---
title: "aaaSet až přípravná prostředí pro webové aplikace v Azure App Service | Microsoft Docs"
description: "Zjistěte, jak toouse připravený publikování pro webové aplikace v Azure App Service."
services: app-service
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: 338424100a20bf823323313fb6699e439f367421
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-staging-environments-in-azure-app-service"></a><span data-ttu-id="c2278-103">Nastavení přípravných prostředí v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c2278-103">Set up staging environments in Azure App Service</span></span>
<a name="Overview"></a>

<span data-ttu-id="c2278-104">Při nasazení vaší webové aplikace, webové aplikace na Linuxu, mobilní back-end a aplikace API příliš[služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714), můžete nasadit tooa samostatné nasazovací slot místo produkčního slotu. hello výchozí při spuštění v hello **Standard**nebo **Premium** režimu plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="c2278-104">When you deploy your web app, web app on Linux, mobile back end, and API app too[App Service](http://go.microsoft.com/fwlink/?LinkId=529714), you can deploy tooa separate deployment slot instead of hello default production slot when running in hello **Standard** or **Premium** App Service plan mode.</span></span> <span data-ttu-id="c2278-105">Nasazovací sloty jsou ve skutečnosti za provozu aplikace s vlastní názvy hostitelů.</span><span class="sxs-lookup"><span data-stu-id="c2278-105">Deployment slots are actually live apps with their own hostnames.</span></span> <span data-ttu-id="c2278-106">Můžete si místo aplikace obsahu a konfigurace – elementy mezi dvěma sloty nasazení, včetně hello produkční slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-106">App content and configurations elements can be swapped between two deployment slots, including hello production slot.</span></span> <span data-ttu-id="c2278-107">Nasazení vaší aplikace tooa nasazovací slot má hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c2278-107">Deploying your application tooa deployment slot has hello following benefits:</span></span>

* <span data-ttu-id="c2278-108">Můžete ověřit změny aplikace v přípravný slot nasazení před odkládací s hello produkční slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-108">You can validate app changes in a staging deployment slot before swapping it with hello production slot.</span></span>
* <span data-ttu-id="c2278-109">První nasazení slot tooa aplikace a odkládací do produkčního prostředí zajistí, jsou všechny instance hello slotu jestli před prohazují do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="c2278-109">Deploying an app tooa slot first and swapping it into production ensures that all instances of hello slot are warmed up before being swapped into production.</span></span> <span data-ttu-id="c2278-110">Tím se eliminuje výpadek při nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c2278-110">This eliminates downtime when you deploy your app.</span></span> <span data-ttu-id="c2278-111">přesměrování provozu Hello je bezproblémové a jsou v důsledku operace prohození vyřadit žádné požadavky.</span><span class="sxs-lookup"><span data-stu-id="c2278-111">hello traffic redirection is seamless, and no requests are dropped as a result of swap operations.</span></span> <span data-ttu-id="c2278-112">Tento celý pracovní postup je možné automatizovat tak, že nakonfigurujete [Prohodit automaticky](#Auto-Swap) při swap předběžné ověření není potřeba.</span><span class="sxs-lookup"><span data-stu-id="c2278-112">This entire workflow can be automated by configuring [Auto Swap](#Auto-Swap) when pre-swap validation is not needed.</span></span>
* <span data-ttu-id="c2278-113">Po prohození hello slot s dříve dvoufázové instalace aplikace teď má hello předchozí produkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c2278-113">After a swap, hello slot with previously staged app now has hello previous production app.</span></span> <span data-ttu-id="c2278-114">Pokud jsou tyto změny hello vzájemně zaměněny do produkčního slotu. hello není podle očekávání, můžete provést stejný Prohodit okamžitě zpět tooget "poslední známé funkční web" hello.</span><span class="sxs-lookup"><span data-stu-id="c2278-114">If hello changes swapped into hello production slot are not as you expected, you can perform hello same swap immediately tooget your "last known good site" back.</span></span>

<span data-ttu-id="c2278-115">Každý režim plán služby App Service podporuje různé počty nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="c2278-115">Each App Service plan mode supports a different number of deployment slots.</span></span> <span data-ttu-id="c2278-116">toofind hello snížit počet sloty režim vaše aplikace podporuje, najdete v části [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="c2278-116">toofind out hello number of slots your app's mode supports, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

* <span data-ttu-id="c2278-117">Pokud vaše aplikace obsahuje více sloty, nelze změnit režim hello.</span><span class="sxs-lookup"><span data-stu-id="c2278-117">When your app has multiple slots, you cannot change hello mode.</span></span>
* <span data-ttu-id="c2278-118">Škálování není k dispozici pro nevýrobní prostředí sloty.</span><span class="sxs-lookup"><span data-stu-id="c2278-118">Scaling is not available for non-production slots.</span></span>
* <span data-ttu-id="c2278-119">Správa propojeného prostředku není podporován pro nevýrobní sloty.</span><span class="sxs-lookup"><span data-stu-id="c2278-119">Linked resource management is not supported for non-production slots.</span></span> <span data-ttu-id="c2278-120">V hello [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715) pouze se vyhnout této potenciální dopad na produkční slot dočasně přesunutím hello mimo produkční slot tooa různé služby App Service plán režimu.</span><span class="sxs-lookup"><span data-stu-id="c2278-120">In hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) only, you can avoid this potential impact on a production slot by temporarily moving hello non-production slot tooa different App Service plan mode.</span></span> <span data-ttu-id="c2278-121">Všimněte si, že hello mimo produkční slot musejí sdílet znovu hello stejný režim s hello produkčního slotu. předtím, než můžete Prohodit sloty hello dva.</span><span class="sxs-lookup"><span data-stu-id="c2278-121">Note that hello non-production slot must once again share hello same mode with hello production slot before you can swap hello two slots.</span></span>

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a><span data-ttu-id="c2278-122">Přidat slot nasazení</span><span class="sxs-lookup"><span data-stu-id="c2278-122">Add a deployment slot</span></span>
<span data-ttu-id="c2278-123">Hello aplikace musí být spuštěna v hello **standardní** nebo **Premium** režimu v pořadí pro tooenable můžete více nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="c2278-123">hello app must be running in hello **Standard** or **Premium** mode in order for you tooenable multiple deployment slots.</span></span>

1. <span data-ttu-id="c2278-124">V hello [portálu Azure](https://portal.azure.com/), otevřete v této aplikaci [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="c2278-124">In hello [Azure Portal](https://portal.azure.com/), open your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span>
2. <span data-ttu-id="c2278-125">Zvolte hello **nasazovací sloty** možnost a potom klikněte na **přidat Slot**.</span><span class="sxs-lookup"><span data-stu-id="c2278-125">Choose hello **Deployment slots** option, then click **Add Slot**.</span></span>
   
    ![Přidat nový slot nasazení][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > <span data-ttu-id="c2278-127">Pokud aplikace hello ještě není v hello **standardní** nebo **Premium** režimu, obdržíte zprávu s upozorněním hello podporované režimy pro povolení publikování dvoufázové instalace.</span><span class="sxs-lookup"><span data-stu-id="c2278-127">If hello app is not already in hello **Standard** or **Premium** mode, you will receive a message indicating hello supported modes for enabling staged publishing.</span></span> <span data-ttu-id="c2278-128">Nyní máte možnost tooselect hello **Upgrade** a přejděte toohello **škálování** kartě vaší aplikace, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c2278-128">At this point, you have hello option tooselect **Upgrade** and navigate toohello **Scale** tab of your app before continuing.</span></span>
   > 
   > 
3. <span data-ttu-id="c2278-129">V hello **přidat slot** okně pojmenujte hello slot a vyberte zda tooclone konfigurace aplikací z jiného existujícího slotu nasazení.</span><span class="sxs-lookup"><span data-stu-id="c2278-129">In hello **Add a slot** blade, give hello slot a name, and select whether tooclone app configuration from another existing deployment slot.</span></span> <span data-ttu-id="c2278-130">Klikněte na tlačítko zaškrtnutí toocontinue hello.</span><span class="sxs-lookup"><span data-stu-id="c2278-130">Click hello check mark toocontinue.</span></span>
   
    ![Zdroj konfigurace][ConfigurationSource1]
   
    <span data-ttu-id="c2278-132">Hello poprvé přidat slot, budete mít pouze dvě možnosti: klonování konfiguraci z hello výchozí slotu v produkčním prostředí nebo vůbec.</span><span class="sxs-lookup"><span data-stu-id="c2278-132">hello first time you add a slot, you will only have two choices: clone configuration from hello default slot in production or not at all.</span></span>
    <span data-ttu-id="c2278-133">Po vytvoření několik sloty, budete moct tooclone konfigurace z slotu než hello, jeden v produkčním prostředí:</span><span class="sxs-lookup"><span data-stu-id="c2278-133">After you have created several slots, you will be able tooclone configuration from a slot other than hello one in production:</span></span>
   
    ![Konfigurace zdroje][MultipleConfigurationSources]
4. <span data-ttu-id="c2278-135">V okně prostředků aplikace, klikněte na tlačítko **nasazovací sloty**, klikněte tooopen slotu nasazení tohoto slotu okna prostředků, sadu metriky a konfigurace stejně jako kterákoli jiná aplikace.</span><span class="sxs-lookup"><span data-stu-id="c2278-135">In your app's resource blade, click  **Deployment slots**, then click a deployment slot tooopen that slot's resource blade, with a set of metrics and configuration just like any other app.</span></span> <span data-ttu-id="c2278-136">Hello název slotu hello zobrazuje hello horní části okna tooremind hello je, že kliknete hello nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-136">hello name of hello slot is shown at hello top of hello blade tooremind you that you are viewing hello deployment slot.</span></span>
   
    ![Název slotu nasazení][StagingTitle]
5. <span data-ttu-id="c2278-138">Klikněte na adresu URL aplikace hello v okně hello slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-138">Click hello app URL in hello slot's blade.</span></span> <span data-ttu-id="c2278-139">Všimněte si hello nasazovací slot má svůj vlastní název hostitele a také za provozu aplikace.</span><span class="sxs-lookup"><span data-stu-id="c2278-139">Notice hello deployment slot has its own hostname and is also a live app.</span></span> <span data-ttu-id="c2278-140">slot nasazení toohello toolimit veřejný přístup, najdete v části [webové aplikace App Service – blok webového přístupu toonon produkční nasazovací sloty](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).</span><span class="sxs-lookup"><span data-stu-id="c2278-140">toolimit public access toohello deployment slot, see [App Service Web App – block web access toonon-production deployment slots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).</span></span>

<span data-ttu-id="c2278-141">Po vytvoření slotu nasazení není k dispozici žádný obsah.</span><span class="sxs-lookup"><span data-stu-id="c2278-141">There is no content after deployment slot creation.</span></span> <span data-ttu-id="c2278-142">Můžete nasadit toohello slotu z různých úložiště větev nebo úplně jiné úložiště.</span><span class="sxs-lookup"><span data-stu-id="c2278-142">You can deploy toohello slot from a different repository branch, or an altogether different repository.</span></span> <span data-ttu-id="c2278-143">Můžete také změnit konfiguraci slotu hello.</span><span class="sxs-lookup"><span data-stu-id="c2278-143">You can also change hello slot's configuration.</span></span> <span data-ttu-id="c2278-144">Použití hello publikovat profilu nebo nasazení přihlašovací údaje spojené s hello slot nasazení pro aktualizace obsahu.</span><span class="sxs-lookup"><span data-stu-id="c2278-144">Use hello publish profile or deployment credentials associated with hello deployment slot for content updates.</span></span>  <span data-ttu-id="c2278-145">Například můžete [publikování slot toothis s gitem](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="c2278-145">For example, you can [publish toothis slot with git](app-service-deploy-local-git.md).</span></span>

<a name="AboutConfiguration"></a>

## <a name="configuration-for-deployment-slots"></a><span data-ttu-id="c2278-146">Konfigurace pro nasazovací sloty</span><span class="sxs-lookup"><span data-stu-id="c2278-146">Configuration for deployment slots</span></span>
<span data-ttu-id="c2278-147">Když naklonovat konfiguraci z jiného slotu nasazení klonovaného konfigurace hello je upravovat.</span><span class="sxs-lookup"><span data-stu-id="c2278-147">When you clone configuration from another deployment slot, hello cloned configuration is editable.</span></span> <span data-ttu-id="c2278-148">Kromě toho některé konfigurace – elementy bude postupovat podle obsahu hello napříč prohození (ne slotu konkrétní) při další konfigurace – elementy zůstanou v hello stejné pozici po prohození (slotu konkrétní).</span><span class="sxs-lookup"><span data-stu-id="c2278-148">Furthermore, some configuration elements will follow hello content across a swap (not slot specific) while other configuration elements will stay in hello same slot after a swap (slot specific).</span></span> <span data-ttu-id="c2278-149">Hello následující seznamy shrnují hello konfigurace, která se změní při Prohodit sloty.</span><span class="sxs-lookup"><span data-stu-id="c2278-149">hello following lists show hello configuration that will change when you swap slots.</span></span>

<span data-ttu-id="c2278-150">**Nastavení, které jsou vzájemně zaměněny**:</span><span class="sxs-lookup"><span data-stu-id="c2278-150">**Settings that are swapped**:</span></span>

* <span data-ttu-id="c2278-151">Obecné nastavení – například framework verze 32/64-bit, webové sokety</span><span class="sxs-lookup"><span data-stu-id="c2278-151">General settings - such as framework version, 32/64-bit, Web sockets</span></span>
* <span data-ttu-id="c2278-152">Nastavení aplikace (může být nakonfigurované toostick tooa slot)</span><span class="sxs-lookup"><span data-stu-id="c2278-152">App settings (can be configured toostick tooa slot)</span></span>
* <span data-ttu-id="c2278-153">Připojovací řetězce (může být nakonfigurované toostick tooa slot)</span><span class="sxs-lookup"><span data-stu-id="c2278-153">Connection strings (can be configured toostick tooa slot)</span></span>
* <span data-ttu-id="c2278-154">Mapování obslužných rutin</span><span class="sxs-lookup"><span data-stu-id="c2278-154">Handler mappings</span></span>
* <span data-ttu-id="c2278-155">Nastavení monitorování a diagnostiky</span><span class="sxs-lookup"><span data-stu-id="c2278-155">Monitoring and diagnostic settings</span></span>
* <span data-ttu-id="c2278-156">Obsah webové úlohy</span><span class="sxs-lookup"><span data-stu-id="c2278-156">WebJobs content</span></span>

<span data-ttu-id="c2278-157">**Nastavení, které nejsou vzájemně zaměněny**:</span><span class="sxs-lookup"><span data-stu-id="c2278-157">**Settings that are not swapped**:</span></span>

* <span data-ttu-id="c2278-158">Publikování koncové body</span><span class="sxs-lookup"><span data-stu-id="c2278-158">Publishing endpoints</span></span>
* <span data-ttu-id="c2278-159">Názvy vlastních domén</span><span class="sxs-lookup"><span data-stu-id="c2278-159">Custom Domain Names</span></span>
* <span data-ttu-id="c2278-160">Certifikáty SSL a vazeb</span><span class="sxs-lookup"><span data-stu-id="c2278-160">SSL certificates and bindings</span></span>
* <span data-ttu-id="c2278-161">Nastavení škálování</span><span class="sxs-lookup"><span data-stu-id="c2278-161">Scale settings</span></span>
* <span data-ttu-id="c2278-162">Webové úlohy plánovače</span><span class="sxs-lookup"><span data-stu-id="c2278-162">WebJobs schedulers</span></span>

<span data-ttu-id="c2278-163">tooconfigure nastavení nebo připojení řetězec toostick tooa slot aplikace (není prohodily), přístup hello **nastavení aplikace** okno pro konkrétní pozici a potom vyberte hello **nastavení slotu** pole pro hello konfigurační prvky, které by měl přilepit hello slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-163">tooconfigure an app setting or connection string toostick tooa slot (not swapped), access hello **Application Settings** blade for a specific slot, then select hello **Slot Setting** box for hello configuration elements that should stick hello slot.</span></span> <span data-ttu-id="c2278-164">Všimněte si, že označíte konfigurační prvek jako slotu konkrétní má vliv hello vytvoření tohoto prvku jako není swappable napříč všechny hello nasazovací sloty spojená s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="c2278-164">Note that marking a configuration element as slot specific has hello effect of establishing that element as not swappable across all hello deployment slots associated with hello app.</span></span>

![Nastavení slotu][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a><span data-ttu-id="c2278-166">Prohodit sloty nasazení</span><span class="sxs-lookup"><span data-stu-id="c2278-166">Swap deployment slots</span></span> 
<span data-ttu-id="c2278-167">Můžete Prohodit sloty nasazení v hello **přehled** nebo **nasazovací sloty** zobrazení okna prostředků vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c2278-167">You can swap deployment slots in hello **Overview** or **Deployment slots** view of your app's resource blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c2278-168">Předtím, než jste odkládacího souboru aplikace z slotu nasazení do produkčního prostředí, zajistit, aby všechny bez slotu konkrétní nastavení jsou nakonfigurovaná přesně tak, jak chcete, aby toohave v cílové swap hello.</span><span class="sxs-lookup"><span data-stu-id="c2278-168">Before you swap an app from a deployment slot into production, make sure that all non-slot specific settings are configured exactly as you want toohave it in hello swap target.</span></span>
> 
> 

1. <span data-ttu-id="c2278-169">tooswap nasazovací sloty, klikněte na tlačítko hello **Prohodit** tlačítko panelu příkazů hello hello aplikace nebo v řádku nabídek hello slot nasazení.</span><span class="sxs-lookup"><span data-stu-id="c2278-169">tooswap deployment slots, click hello **Swap** button in hello command bar of hello app or in hello command bar of a deployment slot.</span></span>
   
    ![Swap – tlačítko][SwapButtonBar]

2. <span data-ttu-id="c2278-171">Ujistěte se, že jsou správně nastaveny swap hello cílených zdroje a velikost odkládacího souboru.</span><span class="sxs-lookup"><span data-stu-id="c2278-171">Make sure that hello swap source and swap target are set properly.</span></span> <span data-ttu-id="c2278-172">Cíl swap hello je obvykle hello produkční slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-172">Usually, hello swap target is hello production slot.</span></span> <span data-ttu-id="c2278-173">Klikněte na tlačítko **OK** toocomplete hello operaci.</span><span class="sxs-lookup"><span data-stu-id="c2278-173">Click **OK** toocomplete hello operation.</span></span> <span data-ttu-id="c2278-174">Po dokončení operace hello mít byla vzájemně zaměněny hello nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="c2278-174">When hello operation finishes, hello deployment slots have been swapped.</span></span>

    ![Dokončit](./media/web-sites-staged-publishing/SwapImmediately.png)

    <span data-ttu-id="c2278-176">Pro hello **prohození s náhledem** Prohodit typu, najdete v části [prohození s náhledem (více fáze prohození)](#Multi-Phase).</span><span class="sxs-lookup"><span data-stu-id="c2278-176">For hello **Swap with preview** swap type, see [Swap with preview (multi-phase swap)](#Multi-Phase).</span></span>  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a><span data-ttu-id="c2278-177">Prohození s náhledem (více fáze swap)</span><span class="sxs-lookup"><span data-stu-id="c2278-177">Swap with preview (multi-phase swap)</span></span>

<span data-ttu-id="c2278-178">Prohození s náhledem nebo více fáze prohození zjednodušit ověření konfigurace slotu specifické prvky, jako jsou například připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="c2278-178">Swap with preview, or multi-phase swap, simplify validation of slot-specific configuration elements, such as connection strings.</span></span>
<span data-ttu-id="c2278-179">Pro klíčové úlohy chcete toovalidate, který hello aplikace chová podle očekávání při použití konfigurace hello produkčním slotu a je třeba provést takové ověření *před* aplikace hello je prohodily do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="c2278-179">For mission-critical workloads, you want toovalidate that hello app behaves as expected when hello production slot's configuration is applied, and you must perform such validation *before* hello app is swapped into production.</span></span> <span data-ttu-id="c2278-180">Prohození s náhledem je, co potřebujete.</span><span class="sxs-lookup"><span data-stu-id="c2278-180">Swap with preview is what you need.</span></span>

> [!NOTE]
> <span data-ttu-id="c2278-181">Prohození s náhledem nepodporuje webové aplikace v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="c2278-181">Swap with preview is not supported in web apps on Linux.</span></span>

<span data-ttu-id="c2278-182">Při použití hello **prohození s náhledem** možnost (viz [Prohodit sloty nasazení](#Swap)), služby App Service hello následující:</span><span class="sxs-lookup"><span data-stu-id="c2278-182">When you use hello **Swap with preview** option (see [Swap deployment slots](#Swap)), App Service does hello following:</span></span>

- <span data-ttu-id="c2278-183">Není ovlivněná udržuje hello cílový slot beze změny tak existující zatížení na této pozici (například výroba).</span><span class="sxs-lookup"><span data-stu-id="c2278-183">Keeps hello destination slot unchanged so existing workload on that slot (e.g. production) is not impacted.</span></span>
- <span data-ttu-id="c2278-184">Aplikuje hello konfigurační prvky hello cílový slot toohello zdrojový slot, včetně hello specifické pro slot připojovací řetězce a nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c2278-184">Applies hello configuration elements of hello destination slot toohello source slot, including hello slot-specific connection strings and app settings.</span></span>
- <span data-ttu-id="c2278-185">Restartuje hello pracovních procesů na zdrojový slot hello pomocí těchto zmíněnými konfigurace – elementy.</span><span class="sxs-lookup"><span data-stu-id="c2278-185">Restarts hello worker processes on hello source slot using these aforementioned configuration elements.</span></span>
- <span data-ttu-id="c2278-186">Po dokončení hello swap: Přesune hello warmed vedlejší-up zdrojový slot do hello cílový slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-186">When you complete hello swap: Moves hello pre-warmed-up source slot into hello destination slot.</span></span> <span data-ttu-id="c2278-187">hello zdrojový slot jako ruční prohození přesunutí Hello cílový slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-187">hello destination slot is moved into hello source slot as in a manual swap.</span></span>
- <span data-ttu-id="c2278-188">Když zrušíte hello swap: znovu použije hello konfigurace – elementy hello zdrojový slot toohello zdrojový slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-188">When you cancel hello swap: Reapplies hello configuration elements of hello source slot toohello source slot.</span></span>

<span data-ttu-id="c2278-189">Můžete zobrazit náhled, přesně jak hello aplikace se bude chovat s konfigurací hello cílový slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-189">You can preview exactly how hello app will behave with hello destination slot's configuration.</span></span> <span data-ttu-id="c2278-190">Po dokončení ověření dokončíte hello odkládacího souboru v samostatném kroku.</span><span class="sxs-lookup"><span data-stu-id="c2278-190">Once you complete validation, you complete hello swap in a separate step.</span></span> <span data-ttu-id="c2278-191">Tento krok má hello přidané výhodu hello zdrojový slot již jestli s hello požadované konfigurace, a klienti nebudou mít žádné výpadky.</span><span class="sxs-lookup"><span data-stu-id="c2278-191">This step has hello added advantage that hello source slot is already warmed up with hello desired configuration, and clients will not experience any downtime.</span></span>  

<span data-ttu-id="c2278-192">Ukázky pro hello rutin prostředí Azure PowerShell, které jsou k dispozici pro více fáze prohození jsou součástí hello rutin Azure Powershellu pro část sloty nasazení.</span><span class="sxs-lookup"><span data-stu-id="c2278-192">Samples for hello Azure PowerShell cmdlets available for multi-phase swap are included in hello Azure PowerShell cmdlets for deployment slots section.</span></span>

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a><span data-ttu-id="c2278-193">Konfigurace automatického prohození</span><span class="sxs-lookup"><span data-stu-id="c2278-193">Configure Auto Swap</span></span>
<span data-ttu-id="c2278-194">Automatické prohození zjednodušuje DevOps scénáře, kde chcete toocontinuously nasazení vaší aplikace pomocí nulové studený start a brání výpadkům pro koncové zákazníky aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c2278-194">Auto Swap streamlines DevOps scenarios where you want toocontinuously deploy your app with zero cold start and zero downtime for end customers of hello app.</span></span> <span data-ttu-id="c2278-195">Když slot nasazení je nakonfigurována pro automatické prohození do produkčního prostředí, pokaždé, když push vaší slotu toothat aktualizace kódu, služby App Service automaticky odkládacího hello aplikace do produkčního prostředí poté, co se má již provozní teplotu ve slotu hello.</span><span class="sxs-lookup"><span data-stu-id="c2278-195">When a deployment slot is configured for Auto Swap into production, every time you push your code update toothat slot, App Service will automatically swap hello app into production after it has already warmed up in hello slot.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c2278-196">Když povolíte automaticky Prohodit slot, ujistěte se, že konfigurace slotu hello přesně hello konfigurace určené pro cílový slot hello (obvykle hello produkční slot).</span><span class="sxs-lookup"><span data-stu-id="c2278-196">When you enable Auto Swap for a slot, make sure hello slot configuration is exactly hello configuration intended for hello target slot (usually hello production slot).</span></span>
> 
> 

> [!NOTE]
> <span data-ttu-id="c2278-197">Nepodporuje automatické prohození webové aplikace v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="c2278-197">Auto swap is not supported in web apps on Linux.</span></span>

<span data-ttu-id="c2278-198">Konfigurace automatického Prohodit pro slot je snadné.</span><span class="sxs-lookup"><span data-stu-id="c2278-198">Configuring Auto Swap for a slot is easy.</span></span> <span data-ttu-id="c2278-199">Postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="c2278-199">Follow hello steps below:</span></span>

1. <span data-ttu-id="c2278-200">V **nasazovací sloty**, vyberte mimo produkční slot a zvolit **nastavení aplikace** v okně prostředků tento slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-200">In **Deployment Slots**, select a non-production slot, and choose **Application Settings** in that slot's resource blade.</span></span>  
   
    ![][Autoswap1]
2. <span data-ttu-id="c2278-201">Vyberte **na** pro **Prohodit automaticky**, vyberte hello požadované cílový slot v **automaticky Prohodit Slot**a klikněte na tlačítko **Uložit** hello panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="c2278-201">Select **On** for **Auto Swap**, select hello desired target slot in **Auto Swap Slot**, and click **Save** in hello command bar.</span></span> <span data-ttu-id="c2278-202">Zajistěte, aby konfigurace pro hello slot je přesně hello konfigurace určené pro cílový slot hello.</span><span class="sxs-lookup"><span data-stu-id="c2278-202">Make sure configuration for hello slot is exactly hello configuration intended for hello target slot.</span></span>
   
    <span data-ttu-id="c2278-203">Hello **oznámení** karta bude flash zelená **úspěch** po dokončení operace hello.</span><span class="sxs-lookup"><span data-stu-id="c2278-203">hello **Notifications** tab will flash a green **SUCCESS** once hello operation is complete.</span></span>
   
    ![][Autoswap2]
   
   > [!NOTE]
   > <span data-ttu-id="c2278-204">tootest Prohodit automaticky pro vaši aplikaci, vyberte nejprve mimo produkční cílový slot v **automaticky Prohodit Slot** toobecome obeznámeni s funkcí hello.</span><span class="sxs-lookup"><span data-stu-id="c2278-204">tootest Auto Swap for your app, you can first select a non-production target slot in **Auto Swap Slot** toobecome familiar with hello feature.</span></span>  
   > 
   > 
3. <span data-ttu-id="c2278-205">Spusťte slotu nasazení toothat nabízené kódu.</span><span class="sxs-lookup"><span data-stu-id="c2278-205">Execute a code push toothat deployment slot.</span></span> <span data-ttu-id="c2278-206">Automatického prohození se stane po krátkou dobu a hello aktualizace se projeví na adrese URL vaší cílový slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-206">Auto Swap will happen after a short time and hello update will be reflected at your target slot's URL.</span></span>

<a name="Rollback"></a>

## <a name="toorollback-a-production-app-after-swap"></a><span data-ttu-id="c2278-207">toorollback produkční aplikace po swap</span><span class="sxs-lookup"><span data-stu-id="c2278-207">toorollback a production app after swap</span></span>
<span data-ttu-id="c2278-208">Pokud žádné chyby se zjišťují v produkčním prostředí po prohození slotů, vrátit hello sloty back tootheir před swap stavy odkládací hello stejné dva sloty okamžitě.</span><span class="sxs-lookup"><span data-stu-id="c2278-208">If any errors are identified in production after a slot swap, roll hello slots back tootheir pre-swap states by swapping hello same two slots immediately.</span></span>

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a><span data-ttu-id="c2278-209">Vlastní warm-up před swap</span><span class="sxs-lookup"><span data-stu-id="c2278-209">Custom warm-up before swap</span></span>
<span data-ttu-id="c2278-210">Některé aplikace může vyžadovat vlastní warm-up akce.</span><span class="sxs-lookup"><span data-stu-id="c2278-210">Some apps may require custom warm-up actions.</span></span> <span data-ttu-id="c2278-211">Hello `applicationInitialization` konfigurace element v souboru web.config umožňuje vám toospecify vlastní inicializaci akce toobe provést dřív, než je přijat požadavek.</span><span class="sxs-lookup"><span data-stu-id="c2278-211">hello `applicationInitialization` configuration element in web.config allows you toospecify custom initialization actions toobe performed before a request is received.</span></span> <span data-ttu-id="c2278-212">operace přepnutí Hello vyčká pro tuto vlastní warm-up toocomplete.</span><span class="sxs-lookup"><span data-stu-id="c2278-212">hello swap operation will wait for this custom warm-up toocomplete.</span></span> <span data-ttu-id="c2278-213">Zde je fragment ukázkový soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="c2278-213">Here is a sample web.config fragment.</span></span>

    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostname="[app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>

## <a name="toodelete-a-deployment-slot"></a><span data-ttu-id="c2278-214">toodelete slotu nasazení</span><span class="sxs-lookup"><span data-stu-id="c2278-214">toodelete a deployment slot</span></span>
<span data-ttu-id="c2278-215">V okně hello pro slot nasazení, okno otevřít hello nasazovací slot, klikněte na tlačítko **přehled** (hello výchozí stránka) a klikněte na tlačítko **odstranit** hello panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="c2278-215">In hello blade for a deployment slot, open hello deployment slot's blade, click **Overview** (hello default page), and click **Delete** in hello command bar.</span></span>  

![Odstranit slotu nasazení][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="azure-powershell-cmdlets-for-deployment-slots"></a><span data-ttu-id="c2278-217">Rutiny Azure PowerShell pro nasazovací sloty</span><span class="sxs-lookup"><span data-stu-id="c2278-217">Azure PowerShell cmdlets for deployment slots</span></span>
<span data-ttu-id="c2278-218">Prostředí Azure PowerShell je modul, který poskytuje toomanage rutiny Azure pomocí prostředí Windows PowerShell, včetně podpory pro správu nasazovací sloty ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c2278-218">Azure PowerShell is a module that provides cmdlets toomanage Azure through Windows PowerShell, including support for managing deployment slots in Azure App Service.</span></span>

* <span data-ttu-id="c2278-219">Informace o instalaci a konfiguraci prostředí Azure PowerShell a týkající se ověřování Azure PowerShell s předplatným Azure najdete v tématu [jak tooinstall a nakonfigurovat Microsoft Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c2278-219">For information on installing and configuring Azure PowerShell, and on authenticating Azure PowerShell with your Azure subscription, see [How tooinstall and configure Microsoft Azure PowerShell](/powershell/azure/overview).</span></span>  

- - -
### <a name="create-a-web-app"></a><span data-ttu-id="c2278-220">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c2278-220">Create a web app</span></span>
```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a><span data-ttu-id="c2278-221">Vytvořte slot nasazení</span><span class="sxs-lookup"><span data-stu-id="c2278-221">Create a deployment slot</span></span>
```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-review-multi-phase-swap-and-apply-destination-slot-configuration-toosource-slot"></a><span data-ttu-id="c2278-222">Zahájení prohození s zkontrolujte (více fáze prohození) a použít cílový slot konfigurace toosource slot</span><span class="sxs-lookup"><span data-stu-id="c2278-222">Initiate a swap with review (multi-phase swap) and apply destination slot configuration toosource slot</span></span>
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a><span data-ttu-id="c2278-223">Zrušit čeká na prohození (prohození s zkontrolujte) a obnovit konfiguraci slotu zdroje</span><span class="sxs-lookup"><span data-stu-id="c2278-223">Cancel a pending swap (swap with review) and restore source slot configuration</span></span>
```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a><span data-ttu-id="c2278-224">Prohodit sloty nasazení</span><span class="sxs-lookup"><span data-stu-id="c2278-224">Swap deployment slots</span></span>
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="delete-deployment-slot"></a><span data-ttu-id="c2278-225">Odstranit nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="c2278-225">Delete deployment slot</span></span>
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="azure-command-line-interface-azure-cli-commands-for-deployment-slots"></a><span data-ttu-id="c2278-226">Azure příkazy rozhraní příkazového řádku (Azure CLI) pro nasazovací sloty</span><span class="sxs-lookup"><span data-stu-id="c2278-226">Azure Command-Line Interface (Azure CLI) commands for Deployment Slots</span></span>
<span data-ttu-id="c2278-227">Hello rozhraní příkazového řádku Azure poskytuje příkazy a platformy pro práci s Azure, včetně podpory pro správu služby App Service nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="c2278-227">hello Azure CLI provides cross-platform commands for working with Azure, including support for managing App Service deployment slots.</span></span>

* <span data-ttu-id="c2278-228">Pro pokyny k instalaci a konfiguraci hello rozhraní příkazového řádku Azure, včetně informací o tom, rozhraní příkazového řádku Azure tooyour tooconnect předplatné Azure, najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="c2278-228">For instructions on installing and configuring hello Azure CLI, including information on how tooconnect Azure CLI tooyour Azure subscription, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="c2278-229">volání toolist hello příkazy dostupné pro službu Azure App Service v hello rozhraní příkazového řádku Azure, `azure site -h`.</span><span class="sxs-lookup"><span data-stu-id="c2278-229">toolist hello commands available for Azure App Service in hello Azure CLI, call `azure site -h`.</span></span>

> [!NOTE] 
> <span data-ttu-id="c2278-230">Pro [Azure CLI 2.0](https://github.com/Azure/azure-cli) příkazy pro nasazovací sloty, najdete v části [slot nasazení webové služby App Service az](/cli/azure/appservice/web/deployment/slot).</span><span class="sxs-lookup"><span data-stu-id="c2278-230">For [Azure CLI 2.0](https://github.com/Azure/azure-cli) commands for deployment slots, see [az appservice web deployment slot](/cli/azure/appservice/web/deployment/slot).</span></span>

- - -
### <a name="azure-site-list"></a><span data-ttu-id="c2278-231">seznam webů Azure</span><span class="sxs-lookup"><span data-stu-id="c2278-231">azure site list</span></span>
<span data-ttu-id="c2278-232">Informace o aplikacích hello v aktuálním předplatném hello volání **seznam webů azure**, jako v hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="c2278-232">For information about hello apps in hello current subscription, call **azure site list**, as in hello following example.</span></span>

`azure site list webappslotstest`

- - -
### <a name="azure-site-create"></a><span data-ttu-id="c2278-233">Vytvoření Azure lokality</span><span class="sxs-lookup"><span data-stu-id="c2278-233">azure site create</span></span>
<span data-ttu-id="c2278-234">volání toocreate nasazovací slot **azure lokality vytvořit** a zadejte název stávající aplikace hello a hello název slotu toocreate hello, stejně jako hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="c2278-234">toocreate a deployment slot, call **azure site create** and specify hello name of an existing app and hello name of hello slot toocreate, as in hello following example.</span></span>

`azure site create webappslotstest --slot staging`

<span data-ttu-id="c2278-235">tooenable zdrojového kódu pro nový slot hello, použijte hello **– git** možnost, stejně jako hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="c2278-235">tooenable source control for hello new slot, use hello **--git** option, as in hello following example.</span></span>

`azure site create --git webappslotstest --slot staging`

- - -
### <a name="azure-site-swap"></a><span data-ttu-id="c2278-236">swap Azure lokality</span><span class="sxs-lookup"><span data-stu-id="c2278-236">azure site swap</span></span>
<span data-ttu-id="c2278-237">toomake hello aktualizované nasazení slotu hello produkční aplikace, použijte hello **azure lokality swap** příkaz tooperform operaci přepnutí, stejně jako hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="c2278-237">toomake hello updated deployment slot hello production app, use hello **azure site swap** command tooperform a swap operation, as in hello following example.</span></span> <span data-ttu-id="c2278-238">Hello produkční aplikaci nebude mít žádný výpadek ani ho určitým studený start.</span><span class="sxs-lookup"><span data-stu-id="c2278-238">hello production app will not experience any down time, nor will it undergo a cold start.</span></span>

`azure site swap webappslotstest`

- - -
### <a name="azure-site-delete"></a><span data-ttu-id="c2278-239">odstranění webu Azure</span><span class="sxs-lookup"><span data-stu-id="c2278-239">azure site delete</span></span>
<span data-ttu-id="c2278-240">toodelete slot nasazení, která už není potřeba, použijte hello **odstranění azure webu** příkaz jako hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="c2278-240">toodelete a deployment slot that is no longer needed, use hello **azure site delete** command, as in hello following example.</span></span>

`azure site delete webappslotstest --slot staging`

- - -
> [!NOTE]
> <span data-ttu-id="c2278-241">Sledujte webovou aplikaci v akci.</span><span class="sxs-lookup"><span data-stu-id="c2278-241">See a web app in action.</span></span> <span data-ttu-id="c2278-242">[Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/) okamžitě a vytvořit krátkodobou úvodní aplikaci – nepožaduje se žádná platební karta a bez jakýchkoli závazků.</span><span class="sxs-lookup"><span data-stu-id="c2278-242">[Try App Service](https://azure.microsoft.com/try/app-service/) immediately and create a short-lived starter app—no credit card required, no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="c2278-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2278-243">Next Steps</span></span>
<span data-ttu-id="c2278-244">[Službě Azure App Service Web App – blokovat webový přístup k produkční toonon nasazovací sloty](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[Úvod tooApp služby v systému Linux](./app-service-linux-intro.md)
[bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="c2278-244">[Azure App Service Web App – block web access toonon-production deployment slots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[Introduction tooApp Service on Linux](./app-service-linux-intro.md)
[Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/)</span></span>

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png

