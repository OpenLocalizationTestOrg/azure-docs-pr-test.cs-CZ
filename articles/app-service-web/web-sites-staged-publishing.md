---
title: "Nastavení přípravných prostředí pro webové aplikace v Azure App Service | Microsoft Docs"
description: "Naučte se používat dvoufázové instalace publikování pro webové aplikace v Azure App Service."
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
ms.openlocfilehash: ca27c55eaaceb3109b1450c550330dfc416fdf55
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="set-up-staging-environments-in-azure-app-service"></a><span data-ttu-id="eeea5-103">Nastavení přípravných prostředí v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="eeea5-103">Set up staging environments in Azure App Service</span></span>
<a name="Overview"></a>

<span data-ttu-id="eeea5-104">Při nasazení vaší webové aplikace, webové aplikace na Linuxu, mobilní back-end a aplikaci API [služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714), můžete nasadit do samostatné nasazovací slot místo výchozí produkční slot při spuštění v **standardní** nebo **Premium** režimu plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="eeea5-104">When you deploy your web app, web app on Linux, mobile back end, and API app to [App Service](http://go.microsoft.com/fwlink/?LinkId=529714), you can deploy to a separate deployment slot instead of the default production slot when running in the **Standard** or **Premium** App Service plan mode.</span></span> <span data-ttu-id="eeea5-105">Nasazovací sloty jsou ve skutečnosti za provozu aplikace s vlastní názvy hostitelů.</span><span class="sxs-lookup"><span data-stu-id="eeea5-105">Deployment slots are actually live apps with their own hostnames.</span></span> <span data-ttu-id="eeea5-106">Můžete si místo aplikace obsahu a konfigurace – elementy mezi dvěma sloty nasazení, včetně produkční slot.</span><span class="sxs-lookup"><span data-stu-id="eeea5-106">App content and configurations elements can be swapped between two deployment slots, including the production slot.</span></span> <span data-ttu-id="eeea5-107">Nasazení aplikace na slot nasazení má následující výhody:</span><span class="sxs-lookup"><span data-stu-id="eeea5-107">Deploying your application to a deployment slot has the following benefits:</span></span>

* <span data-ttu-id="eeea5-108">Můžete ověřit změny aplikace v přípravný slot nasazení před odkládací s produkční slot.</span><span class="sxs-lookup"><span data-stu-id="eeea5-108">You can validate app changes in a staging deployment slot before swapping it with the production slot.</span></span>
* <span data-ttu-id="eeea5-109">Nasazení aplikace do patice nejprve a odkládací do provozu zajišťuje, že jsou všechny instance přihrádky jestli před prohazují do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="eeea5-109">Deploying an app to a slot first and swapping it into production ensures that all instances of the slot are warmed up before being swapped into production.</span></span> <span data-ttu-id="eeea5-110">Tím se eliminuje výpadek při nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="eeea5-110">This eliminates downtime when you deploy your app.</span></span> <span data-ttu-id="eeea5-111">Přesměrování provozu je bezproblémové a jsou v důsledku operace prohození vyřadit žádné požadavky.</span><span class="sxs-lookup"><span data-stu-id="eeea5-111">The traffic redirection is seamless, and no requests are dropped as a result of swap operations.</span></span> <span data-ttu-id="eeea5-112">Tento celý pracovní postup je možné automatizovat tak, že nakonfigurujete [Prohodit automaticky](#Auto-Swap) při swap předběžné ověření není potřeba.</span><span class="sxs-lookup"><span data-stu-id="eeea5-112">This entire workflow can be automated by configuring [Auto Swap](#Auto-Swap) when pre-swap validation is not needed.</span></span>
* <span data-ttu-id="eeea5-113">Po prohození slot s dříve dvoufázové instalace aplikace teď má předchozí produkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eeea5-113">After a swap, the slot with previously staged app now has the previous production app.</span></span> <span data-ttu-id="eeea5-114">Pokud změny, které jsou vzájemně zaměněny na produkční slot není podle očekávání, můžete provést stejný prohození okamžitě k získání "poslední známé funkční web" zpět.</span><span class="sxs-lookup"><span data-stu-id="eeea5-114">If the changes swapped into the production slot are not as you expected, you can perform the same swap immediately to get your "last known good site" back.</span></span>

<span data-ttu-id="eeea5-115">Každý režim plán služby App Service podporuje různé počty nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="eeea5-115">Each App Service plan mode supports a different number of deployment slots.</span></span> <span data-ttu-id="eeea5-116">Chcete-li zjistit počet přihrádek podporuje režim vaší aplikace naleznete v tématu [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="eeea5-116">To find out the number of slots your app's mode supports, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

* <span data-ttu-id="eeea5-117">Pokud vaše aplikace obsahuje více sloty, nelze změnit režim.</span><span class="sxs-lookup"><span data-stu-id="eeea5-117">When your app has multiple slots, you cannot change the mode.</span></span>
* <span data-ttu-id="eeea5-118">Škálování není k dispozici pro nevýrobní prostředí sloty.</span><span class="sxs-lookup"><span data-stu-id="eeea5-118">Scaling is not available for non-production slots.</span></span>
* <span data-ttu-id="eeea5-119">Správa propojeného prostředku není podporován pro nevýrobní sloty.</span><span class="sxs-lookup"><span data-stu-id="eeea5-119">Linked resource management is not supported for non-production slots.</span></span> <span data-ttu-id="eeea5-120">V [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715) pouze se vyhnout této potenciální dopad na produkční slot dočasně přesunutím mimo produkční slot jiný režim plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="eeea5-120">In the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) only, you can avoid this potential impact on a production slot by temporarily moving the non-production slot to a different App Service plan mode.</span></span> <span data-ttu-id="eeea5-121">Všimněte si, že jiný produkční slot musí znovu sdílet stejný režim s produkční slot předtím, než můžete zaměnit dva sloty.</span><span class="sxs-lookup"><span data-stu-id="eeea5-121">Note that the non-production slot must once again share the same mode with the production slot before you can swap the two slots.</span></span>

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a><span data-ttu-id="eeea5-122">Přidat slot nasazení</span><span class="sxs-lookup"><span data-stu-id="eeea5-122">Add a deployment slot</span></span>
<span data-ttu-id="eeea5-123">Aplikace musí být spuštěna v **standardní** nebo **Premium** režimu v pořadí, můžete povolit více nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="eeea5-123">The app must be running in the **Standard** or **Premium** mode in order for you to enable multiple deployment slots.</span></span>

1. <span data-ttu-id="eeea5-124">V [portálu Azure](https://portal.azure.com/), otevřete v této aplikaci [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="eeea5-124">In the [Azure Portal](https://portal.azure.com/), open your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span>
2. <span data-ttu-id="eeea5-125">Vyberte **nasazovací sloty** možnost a potom klikněte na **přidat Slot**.</span><span class="sxs-lookup"><span data-stu-id="eeea5-125">Choose the **Deployment slots** option, then click **Add Slot**.</span></span>
   
    ![Přidat nový slot nasazení][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > <span data-ttu-id="eeea5-127">Pokud aplikace ještě není v **standardní** nebo **Premium** režimu, obdržíte zprávu s upozorněním, podporované režimy pro povolení publikování dvoufázové instalace.</span><span class="sxs-lookup"><span data-stu-id="eeea5-127">If the app is not already in the **Standard** or **Premium** mode, you will receive a message indicating the supported modes for enabling staged publishing.</span></span> <span data-ttu-id="eeea5-128">Nyní máte možnost vybrat **Upgrade** a přejděte do **škálování** kartě vaší aplikace, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="eeea5-128">At this point, you have the option to select **Upgrade** and navigate to the **Scale** tab of your app before continuing.</span></span>
   > 
   > 
3. <span data-ttu-id="eeea5-129">V **přidat slot** , pojmenujte přihrádky a vyberte, zda chcete klonovat konfigurace aplikací z jiného existujícího slotu nasazení.</span><span class="sxs-lookup"><span data-stu-id="eeea5-129">In the **Add a slot** blade, give the slot a name, and select whether to clone app configuration from another existing deployment slot.</span></span> <span data-ttu-id="eeea5-130">Kliknutím na značku zaškrtnutí pokračujte.</span><span class="sxs-lookup"><span data-stu-id="eeea5-130">Click the check mark to continue.</span></span>
   
    ![Zdroj konfigurace][ConfigurationSource1]
   
    <span data-ttu-id="eeea5-132">Při prvním přidání slotu, budou mít pouze dvě možnosti: klonování konfigurace z výchozí přihrádky v produkčním prostředí nebo vůbec.</span><span class="sxs-lookup"><span data-stu-id="eeea5-132">The first time you add a slot, you will only have two choices: clone configuration from the default slot in production or not at all.</span></span>
    <span data-ttu-id="eeea5-133">Po vytvoření několik sloty, bude možné klonovat konfiguraci z slotu než v provozním prostředí:</span><span class="sxs-lookup"><span data-stu-id="eeea5-133">After you have created several slots, you will be able to clone configuration from a slot other than the one in production:</span></span>
   
    ![Konfigurace zdroje][MultipleConfigurationSources]
4. <span data-ttu-id="eeea5-135">V okně prostředků aplikace, klikněte na tlačítko **nasazovací sloty**, pak klikněte na tlačítko slotu nasazení tohoto slotu okna prostředků, otevřete sadu metriky a konfigurace stejně jako kterákoli jiná aplikace.</span><span class="sxs-lookup"><span data-stu-id="eeea5-135">In your app's resource blade, click  **Deployment slots**, then click a deployment slot to open that slot's resource blade, with a set of metrics and configuration just like any other app.</span></span> <span data-ttu-id="eeea5-136">Název slotu se zobrazí v horní části okna s upozorněním, že jsou zobrazeny slotu nasazení.</span><span class="sxs-lookup"><span data-stu-id="eeea5-136">The name of the slot is shown at the top of the blade to remind you that you are viewing the deployment slot.</span></span>
   
    ![Název slotu nasazení][StagingTitle]
5. <span data-ttu-id="eeea5-138">Klikněte na adresu URL aplikace v okně na slot.</span><span class="sxs-lookup"><span data-stu-id="eeea5-138">Click the app URL in the slot's blade.</span></span> <span data-ttu-id="eeea5-139">Všimněte si slotu nasazení má svůj vlastní název hostitele a také za provozu aplikace.</span><span class="sxs-lookup"><span data-stu-id="eeea5-139">Notice the deployment slot has its own hostname and is also a live app.</span></span> <span data-ttu-id="eeea5-140">Chcete-li omezit veřejný přístup k slotu nasazení, přečtěte si téma [webové aplikace App Service – blok webového přístupu k testovacím nasazovací sloty](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).</span><span class="sxs-lookup"><span data-stu-id="eeea5-140">To limit public access to the deployment slot, see [App Service Web App – block web access to non-production deployment slots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).</span></span>

<span data-ttu-id="eeea5-141">Po vytvoření slotu nasazení není k dispozici žádný obsah.</span><span class="sxs-lookup"><span data-stu-id="eeea5-141">There is no content after deployment slot creation.</span></span> <span data-ttu-id="eeea5-142">Můžete nasadit na slot z různých úložiště větev nebo úplně jiné úložiště.</span><span class="sxs-lookup"><span data-stu-id="eeea5-142">You can deploy to the slot from a different repository branch, or an altogether different repository.</span></span> <span data-ttu-id="eeea5-143">Také můžete změnit konfiguraci na slot.</span><span class="sxs-lookup"><span data-stu-id="eeea5-143">You can also change the slot's configuration.</span></span> <span data-ttu-id="eeea5-144">Použití profilu nebo nasazení pověření publikovat přidružené slotu nasazení pro aktualizace obsahu.</span><span class="sxs-lookup"><span data-stu-id="eeea5-144">Use the publish profile or deployment credentials associated with the deployment slot for content updates.</span></span>  <span data-ttu-id="eeea5-145">Například můžete [publikovat do této pozici s gitem](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="eeea5-145">For example, you can [publish to this slot with git](app-service-deploy-local-git.md).</span></span>

<a name="AboutConfiguration"></a>

## <a name="configuration-for-deployment-slots"></a><span data-ttu-id="eeea5-146">Konfigurace pro nasazovací sloty</span><span class="sxs-lookup"><span data-stu-id="eeea5-146">Configuration for deployment slots</span></span>
<span data-ttu-id="eeea5-147">Když naklonovat konfiguraci z jiného slotu nasazení klonovaného konfigurace je upravovat.</span><span class="sxs-lookup"><span data-stu-id="eeea5-147">When you clone configuration from another deployment slot, the cloned configuration is editable.</span></span> <span data-ttu-id="eeea5-148">Kromě toho některé konfigurace – elementy bude postupovat podle obsahu mezi prohození (ne slotu konkrétní) při další konfigurace – elementy zůstanou v stejný slot. po prohození (slotu konkrétní).</span><span class="sxs-lookup"><span data-stu-id="eeea5-148">Furthermore, some configuration elements will follow the content across a swap (not slot specific) while other configuration elements will stay in the same slot after a swap (slot specific).</span></span> <span data-ttu-id="eeea5-149">Následující seznamy shrnují konfiguraci, která se změní při Prohodit sloty.</span><span class="sxs-lookup"><span data-stu-id="eeea5-149">The following lists show the configuration that will change when you swap slots.</span></span>

<span data-ttu-id="eeea5-150">**Nastavení, které jsou vzájemně zaměněny**:</span><span class="sxs-lookup"><span data-stu-id="eeea5-150">**Settings that are swapped**:</span></span>

* <span data-ttu-id="eeea5-151">Obecné nastavení – například framework verze 32/64-bit, webové sokety</span><span class="sxs-lookup"><span data-stu-id="eeea5-151">General settings - such as framework version, 32/64-bit, Web sockets</span></span>
* <span data-ttu-id="eeea5-152">Nastavení aplikace (může být nakonfigurován chtěl slot)</span><span class="sxs-lookup"><span data-stu-id="eeea5-152">App settings (can be configured to stick to a slot)</span></span>
* <span data-ttu-id="eeea5-153">Připojovací řetězce (může být nakonfigurován chtěl slot)</span><span class="sxs-lookup"><span data-stu-id="eeea5-153">Connection strings (can be configured to stick to a slot)</span></span>
* <span data-ttu-id="eeea5-154">Mapování obslužných rutin</span><span class="sxs-lookup"><span data-stu-id="eeea5-154">Handler mappings</span></span>
* <span data-ttu-id="eeea5-155">Nastavení monitorování a diagnostiky</span><span class="sxs-lookup"><span data-stu-id="eeea5-155">Monitoring and diagnostic settings</span></span>
* <span data-ttu-id="eeea5-156">Obsah webové úlohy</span><span class="sxs-lookup"><span data-stu-id="eeea5-156">WebJobs content</span></span>

<span data-ttu-id="eeea5-157">**Nastavení, které nejsou vzájemně zaměněny**:</span><span class="sxs-lookup"><span data-stu-id="eeea5-157">**Settings that are not swapped**:</span></span>

* <span data-ttu-id="eeea5-158">Publikování koncové body</span><span class="sxs-lookup"><span data-stu-id="eeea5-158">Publishing endpoints</span></span>
* <span data-ttu-id="eeea5-159">Názvy vlastních domén</span><span class="sxs-lookup"><span data-stu-id="eeea5-159">Custom Domain Names</span></span>
* <span data-ttu-id="eeea5-160">Certifikáty SSL a vazeb</span><span class="sxs-lookup"><span data-stu-id="eeea5-160">SSL certificates and bindings</span></span>
* <span data-ttu-id="eeea5-161">Nastavení škálování</span><span class="sxs-lookup"><span data-stu-id="eeea5-161">Scale settings</span></span>
* <span data-ttu-id="eeea5-162">Webové úlohy plánovače</span><span class="sxs-lookup"><span data-stu-id="eeea5-162">WebJobs schedulers</span></span>

<span data-ttu-id="eeea5-163">Konfigurace aplikaci nastavení nebo připojovací řetězec chtěl slot (není vzájemně zaměněny), přístup **nastavení aplikace** okno pro konkrétní pozici, pak vyberte **nastavení slotu** pole pro konfigurační prvky, které by měl přilepit přihrádky.</span><span class="sxs-lookup"><span data-stu-id="eeea5-163">To configure an app setting or connection string to stick to a slot (not swapped), access the **Application Settings** blade for a specific slot, then select the **Slot Setting** box for the configuration elements that should stick the slot.</span></span> <span data-ttu-id="eeea5-164">Všimněte si, že označíte konfigurační prvek jako slotu konkrétní má za následek vytvoření tohoto prvku jako není swappable napříč všechny nasazovací sloty přidružené aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eeea5-164">Note that marking a configuration element as slot specific has the effect of establishing that element as not swappable across all the deployment slots associated with the app.</span></span>

![Nastavení slotu][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a><span data-ttu-id="eeea5-166">Prohodit sloty nasazení</span><span class="sxs-lookup"><span data-stu-id="eeea5-166">Swap deployment slots</span></span> 
<span data-ttu-id="eeea5-167">Můžete Prohodit sloty nasazení v **přehled** nebo **nasazovací sloty** zobrazení okna prostředků vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="eeea5-167">You can swap deployment slots in the **Overview** or **Deployment slots** view of your app's resource blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eeea5-168">Než můžete odkládacího souboru aplikace z slotu nasazení do produkčního prostředí, ujistěte se, že všechna bez slotu konkrétní nastavení jsou nakonfigurována přesně tak, jak budete chtít mít v cílové odkládacího souboru.</span><span class="sxs-lookup"><span data-stu-id="eeea5-168">Before you swap an app from a deployment slot into production, make sure that all non-slot specific settings are configured exactly as you want to have it in the swap target.</span></span>
> 
> 

1. <span data-ttu-id="eeea5-169">Pokud chcete Prohodit sloty nasazení, klikněte na tlačítko **Swap** tlačítka na panelu příkazů aplikace nebo na panelu příkazů slot nasazení.</span><span class="sxs-lookup"><span data-stu-id="eeea5-169">To swap deployment slots, click the **Swap** button in the command bar of the app or in the command bar of a deployment slot.</span></span>
   
    ![Swap – tlačítko][SwapButtonBar]

2. <span data-ttu-id="eeea5-171">Ujistěte se, že jsou správně nastaveny swap zdrojové a cílové odkládacího souboru.</span><span class="sxs-lookup"><span data-stu-id="eeea5-171">Make sure that the swap source and swap target are set properly.</span></span> <span data-ttu-id="eeea5-172">Cíl odkládacího souboru je obvykle produkční slot.</span><span class="sxs-lookup"><span data-stu-id="eeea5-172">Usually, the swap target is the production slot.</span></span> <span data-ttu-id="eeea5-173">Klikněte na tlačítko **OK** k dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="eeea5-173">Click **OK** to complete the operation.</span></span> <span data-ttu-id="eeea5-174">Po dokončení operace mít byla vzájemně zaměněny nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="eeea5-174">When the operation finishes, the deployment slots have been swapped.</span></span>

    ![Dokončit](./media/web-sites-staged-publishing/SwapImmediately.png)

    <span data-ttu-id="eeea5-176">Pro **prohození s náhledem** Prohodit typu, najdete v části [prohození s náhledem (více fáze prohození)](#Multi-Phase).</span><span class="sxs-lookup"><span data-stu-id="eeea5-176">For the **Swap with preview** swap type, see [Swap with preview (multi-phase swap)](#Multi-Phase).</span></span>  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a><span data-ttu-id="eeea5-177">Prohození s náhledem (více fáze swap)</span><span class="sxs-lookup"><span data-stu-id="eeea5-177">Swap with preview (multi-phase swap)</span></span>

<span data-ttu-id="eeea5-178">Prohození s náhledem nebo více fáze prohození zjednodušit ověření konfigurace slotu specifické prvky, jako jsou například připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="eeea5-178">Swap with preview, or multi-phase swap, simplify validation of slot-specific configuration elements, such as connection strings.</span></span>
<span data-ttu-id="eeea5-179">Pro klíčové úlohy, kterou chcete ověřit které aplikace se chová podle očekávání při použití konfigurace produkční slot a je třeba provést takové ověření *před* aplikace je prohodily do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="eeea5-179">For mission-critical workloads, you want to validate that the app behaves as expected when the production slot's configuration is applied, and you must perform such validation *before* the app is swapped into production.</span></span> <span data-ttu-id="eeea5-180">Prohození s náhledem je, co potřebujete.</span><span class="sxs-lookup"><span data-stu-id="eeea5-180">Swap with preview is what you need.</span></span>

> [!NOTE]
> <span data-ttu-id="eeea5-181">Prohození s náhledem nepodporuje webové aplikace v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="eeea5-181">Swap with preview is not supported in web apps on Linux.</span></span>

<span data-ttu-id="eeea5-182">Při použití **prohození s náhledem** možnost (najdete v části [Prohodit sloty nasazení](#Swap)), služby App Service provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="eeea5-182">When you use the **Swap with preview** option (see [Swap deployment slots](#Swap)), App Service does the following:</span></span>

- <span data-ttu-id="eeea5-183">Uchová cílový slot beze změny, není ovlivněná existující zatížení na této pozici (například výroba).</span><span class="sxs-lookup"><span data-stu-id="eeea5-183">Keeps the destination slot unchanged so existing workload on that slot (e.g. production) is not impacted.</span></span>
- <span data-ttu-id="eeea5-184">Aplikuje konfigurační prvky z cílového slotu na zdrojový slot, včetně specifické pro slot připojovací řetězce a nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="eeea5-184">Applies the configuration elements of the destination slot to the source slot, including the slot-specific connection strings and app settings.</span></span>
- <span data-ttu-id="eeea5-185">Restartování pracovních procesů na zdrojový slot, pomocí těchto zmíněnými konfigurace – elementy.</span><span class="sxs-lookup"><span data-stu-id="eeea5-185">Restarts the worker processes on the source slot using these aforementioned configuration elements.</span></span>
- <span data-ttu-id="eeea5-186">Po dokončení prohození: Přesune warmed vedlejší-up zdrojový slot v cílovém slotu.</span><span class="sxs-lookup"><span data-stu-id="eeea5-186">When you complete the swap: Moves the pre-warmed-up source slot into the destination slot.</span></span> <span data-ttu-id="eeea5-187">Přesunutí cílového slotu na zdrojový slot jako ruční prohození.</span><span class="sxs-lookup"><span data-stu-id="eeea5-187">The destination slot is moved into the source slot as in a manual swap.</span></span>
- <span data-ttu-id="eeea5-188">Když zrušíte prohození: znovu použije konfigurace prvků na zdrojový slot na zdrojový slot.</span><span class="sxs-lookup"><span data-stu-id="eeea5-188">When you cancel the swap: Reapplies the configuration elements of the source slot to the source slot.</span></span>

<span data-ttu-id="eeea5-189">Můžete zobrazit náhled, přesně jak aplikace se bude chovat s konfigurací cílový slot.</span><span class="sxs-lookup"><span data-stu-id="eeea5-189">You can preview exactly how the app will behave with the destination slot's configuration.</span></span> <span data-ttu-id="eeea5-190">Po dokončení ověření dokončíte odkládacího souboru v samostatném kroku.</span><span class="sxs-lookup"><span data-stu-id="eeea5-190">Once you complete validation, you complete the swap in a separate step.</span></span> <span data-ttu-id="eeea5-191">Tento krok má výhodu na zdrojový slot již jestli s požadovanou konfigurací a klienti nebudou mít žádné výpadky.</span><span class="sxs-lookup"><span data-stu-id="eeea5-191">This step has the added advantage that the source slot is already warmed up with the desired configuration, and clients will not experience any downtime.</span></span>  

<span data-ttu-id="eeea5-192">Ukázky pro rutiny prostředí Azure PowerShell k dispozici pro více fáze prohození jsou součástí rutin Azure Powershellu pro část sloty nasazení.</span><span class="sxs-lookup"><span data-stu-id="eeea5-192">Samples for the Azure PowerShell cmdlets available for multi-phase swap are included in the Azure PowerShell cmdlets for deployment slots section.</span></span>

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a><span data-ttu-id="eeea5-193">Konfigurace automatického prohození</span><span class="sxs-lookup"><span data-stu-id="eeea5-193">Configure Auto Swap</span></span>
<span data-ttu-id="eeea5-194">Automatické prohození zjednodušuje DevOps scénáře, ve které chcete nepřetržitě nasazení vaší aplikace pomocí nulové studený start a brání výpadkům pro koncové zákazníky aplikace.</span><span class="sxs-lookup"><span data-stu-id="eeea5-194">Auto Swap streamlines DevOps scenarios where you want to continuously deploy your app with zero cold start and zero downtime for end customers of the app.</span></span> <span data-ttu-id="eeea5-195">Když slot nasazení je nakonfigurována pro automatické prohození do produkčního prostředí, pokaždé, když nabízená aktualizace kódu na tomto slot, služby App Service automaticky Prohodit aplikace do produkčního prostředí poté, co se má již provozní teplotu ve slotu.</span><span class="sxs-lookup"><span data-stu-id="eeea5-195">When a deployment slot is configured for Auto Swap into production, every time you push your code update to that slot, App Service will automatically swap the app into production after it has already warmed up in the slot.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eeea5-196">Když povolíte automaticky Prohodit slot, ujistěte se, že konfigurace slotu přesně konfigurace určené pro cílový slot (obvykle produkční slot).</span><span class="sxs-lookup"><span data-stu-id="eeea5-196">When you enable Auto Swap for a slot, make sure the slot configuration is exactly the configuration intended for the target slot (usually the production slot).</span></span>
> 
> 

> [!NOTE]
> <span data-ttu-id="eeea5-197">Nepodporuje automatické prohození webové aplikace v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="eeea5-197">Auto swap is not supported in web apps on Linux.</span></span>

<span data-ttu-id="eeea5-198">Konfigurace automatického Prohodit pro slot je snadné.</span><span class="sxs-lookup"><span data-stu-id="eeea5-198">Configuring Auto Swap for a slot is easy.</span></span> <span data-ttu-id="eeea5-199">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="eeea5-199">Follow the steps below:</span></span>

1. <span data-ttu-id="eeea5-200">V **nasazovací sloty**, vyberte mimo produkční slot a zvolit **nastavení aplikace** v okně prostředků tento slot.</span><span class="sxs-lookup"><span data-stu-id="eeea5-200">In **Deployment Slots**, select a non-production slot, and choose **Application Settings** in that slot's resource blade.</span></span>  
   
    ![][Autoswap1]
2. <span data-ttu-id="eeea5-201">Vyberte **na** pro **Prohodit automaticky**, vyberte požadovaný cíl slot v **automaticky Prohodit Slot**a klikněte na tlačítko **Uložit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="eeea5-201">Select **On** for **Auto Swap**, select the desired target slot in **Auto Swap Slot**, and click **Save** in the command bar.</span></span> <span data-ttu-id="eeea5-202">Ujistěte se, že konfigurace pro přihrádky přesně konfigurace určené pro cílový slot.</span><span class="sxs-lookup"><span data-stu-id="eeea5-202">Make sure configuration for the slot is exactly the configuration intended for the target slot.</span></span>
   
    <span data-ttu-id="eeea5-203">**Oznámení** karta bude flash zelená **úspěch** po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="eeea5-203">The **Notifications** tab will flash a green **SUCCESS** once the operation is complete.</span></span>
   
    ![][Autoswap2]
   
   > [!NOTE]
   > <span data-ttu-id="eeea5-204">K testování Prohodit automaticky pro vaši aplikaci, vyberte nejprve mimo produkční cílový slot v **automaticky Prohodit Slot** se seznámit s funkcí.</span><span class="sxs-lookup"><span data-stu-id="eeea5-204">To test Auto Swap for your app, you can first select a non-production target slot in **Auto Swap Slot** to become familiar with the feature.</span></span>  
   > 
   > 
3. <span data-ttu-id="eeea5-205">Spusťte push kódu na tomto slot nasazení.</span><span class="sxs-lookup"><span data-stu-id="eeea5-205">Execute a code push to that deployment slot.</span></span> <span data-ttu-id="eeea5-206">Automatického prohození se stane po krátkou dobu a aktualizace se projeví na adrese URL vaší cílový slot.</span><span class="sxs-lookup"><span data-stu-id="eeea5-206">Auto Swap will happen after a short time and the update will be reflected at your target slot's URL.</span></span>

<a name="Rollback"></a>

## <a name="to-rollback-a-production-app-after-swap"></a><span data-ttu-id="eeea5-207">Vrátit zpět produkční aplikaci po swap</span><span class="sxs-lookup"><span data-stu-id="eeea5-207">To rollback a production app after swap</span></span>
<span data-ttu-id="eeea5-208">Pokud žádné chyby se zjišťují v produkčním prostředí po prohození slotů, vrátit přihrádkách zpátky do stavu před swap odkládací stejné dva sloty okamžitě.</span><span class="sxs-lookup"><span data-stu-id="eeea5-208">If any errors are identified in production after a slot swap, roll the slots back to their pre-swap states by swapping the same two slots immediately.</span></span>

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a><span data-ttu-id="eeea5-209">Vlastní warm-up před swap</span><span class="sxs-lookup"><span data-stu-id="eeea5-209">Custom warm-up before swap</span></span>
<span data-ttu-id="eeea5-210">Některé aplikace může vyžadovat vlastní warm-up akce.</span><span class="sxs-lookup"><span data-stu-id="eeea5-210">Some apps may require custom warm-up actions.</span></span> <span data-ttu-id="eeea5-211">`applicationInitialization` Konfiguračního prvku v souboru web.config. můžete zadat vlastní inicializaci akce dříve, než je žádost o přijetí.</span><span class="sxs-lookup"><span data-stu-id="eeea5-211">The `applicationInitialization` configuration element in web.config allows you to specify custom initialization actions to be performed before a request is received.</span></span> <span data-ttu-id="eeea5-212">Operace přepnutí vyčká pro tuto vlastní warm-up pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="eeea5-212">The swap operation will wait for this custom warm-up to complete.</span></span> <span data-ttu-id="eeea5-213">Zde je fragment ukázkový soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="eeea5-213">Here is a sample web.config fragment.</span></span>

    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostname="[app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>

## <a name="to-delete-a-deployment-slot"></a><span data-ttu-id="eeea5-214">Chcete-li odstranit slotu nasazení</span><span class="sxs-lookup"><span data-stu-id="eeea5-214">To delete a deployment slot</span></span>
<span data-ttu-id="eeea5-215">V okně pro slot nasazení, otevřete okno slotu nasazení, klikněte na **přehled** (výchozí stránka) a klikněte na tlačítko **odstranit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="eeea5-215">In the blade for a deployment slot, open the deployment slot's blade, click **Overview** (the default page), and click **Delete** in the command bar.</span></span>  

![Odstranit slotu nasazení][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="azure-powershell-cmdlets-for-deployment-slots"></a><span data-ttu-id="eeea5-217">Rutiny Azure PowerShell pro nasazovací sloty</span><span class="sxs-lookup"><span data-stu-id="eeea5-217">Azure PowerShell cmdlets for deployment slots</span></span>
<span data-ttu-id="eeea5-218">Prostředí Azure PowerShell je modul, který poskytuje rutiny pro správu Azure pomocí prostředí Windows PowerShell, včetně podpory pro správu nasazovací sloty ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="eeea5-218">Azure PowerShell is a module that provides cmdlets to manage Azure through Windows PowerShell, including support for managing deployment slots in Azure App Service.</span></span>

* <span data-ttu-id="eeea5-219">Informace o instalaci a konfiguraci prostředí Azure PowerShell a týkající se ověřování Azure PowerShell s předplatným Azure najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell Microsoft](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="eeea5-219">For information on installing and configuring Azure PowerShell, and on authenticating Azure PowerShell with your Azure subscription, see [How to install and configure Microsoft Azure PowerShell](/powershell/azure/overview).</span></span>  

- - -
### <a name="create-a-web-app"></a><span data-ttu-id="eeea5-220">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="eeea5-220">Create a web app</span></span>
```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a><span data-ttu-id="eeea5-221">Vytvořte slot nasazení</span><span class="sxs-lookup"><span data-stu-id="eeea5-221">Create a deployment slot</span></span>
```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-review-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a><span data-ttu-id="eeea5-222">Zahájení prohození s zkontrolujte (více fáze prohození) a použít konfiguraci cílového slotu na zdrojový slot</span><span class="sxs-lookup"><span data-stu-id="eeea5-222">Initiate a swap with review (multi-phase swap) and apply destination slot configuration to source slot</span></span>
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a><span data-ttu-id="eeea5-223">Zrušit čeká na prohození (prohození s zkontrolujte) a obnovit konfiguraci slotu zdroje</span><span class="sxs-lookup"><span data-stu-id="eeea5-223">Cancel a pending swap (swap with review) and restore source slot configuration</span></span>
```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a><span data-ttu-id="eeea5-224">Prohodit sloty nasazení</span><span class="sxs-lookup"><span data-stu-id="eeea5-224">Swap deployment slots</span></span>
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="delete-deployment-slot"></a><span data-ttu-id="eeea5-225">Odstranit nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="eeea5-225">Delete deployment slot</span></span>
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="azure-command-line-interface-azure-cli-commands-for-deployment-slots"></a><span data-ttu-id="eeea5-226">Azure příkazy rozhraní příkazového řádku (Azure CLI) pro nasazovací sloty</span><span class="sxs-lookup"><span data-stu-id="eeea5-226">Azure Command-Line Interface (Azure CLI) commands for Deployment Slots</span></span>
<span data-ttu-id="eeea5-227">Rozhraní příkazového řádku Azure poskytuje příkazy a platformy pro práci s Azure, včetně podpory pro správu služby App Service nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="eeea5-227">The Azure CLI provides cross-platform commands for working with Azure, including support for managing App Service deployment slots.</span></span>

* <span data-ttu-id="eeea5-228">Pokyny týkající se instalace a konfigurace rozhraní příkazového řádku Azure, včetně informací o tom, jak připojit k předplatnému Azure, rozhraní příkazového řádku Azure najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="eeea5-228">For instructions on installing and configuring the Azure CLI, including information on how to connect Azure CLI to your Azure subscription, see [Install and Configure the Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="eeea5-229">Chcete-li seznam příkazů, které jsou k dispozici pro službu Azure App Service v Azure CLI, volejte `azure site -h`.</span><span class="sxs-lookup"><span data-stu-id="eeea5-229">To list the commands available for Azure App Service in the Azure CLI, call `azure site -h`.</span></span>

> [!NOTE] 
> <span data-ttu-id="eeea5-230">Pro [Azure CLI 2.0](https://github.com/Azure/azure-cli) příkazy pro nasazovací sloty, najdete v části [slot nasazení webové služby App Service az](/cli/azure/appservice/web/deployment/slot).</span><span class="sxs-lookup"><span data-stu-id="eeea5-230">For [Azure CLI 2.0](https://github.com/Azure/azure-cli) commands for deployment slots, see [az appservice web deployment slot](/cli/azure/appservice/web/deployment/slot).</span></span>

- - -
### <a name="azure-site-list"></a><span data-ttu-id="eeea5-231">seznam webů Azure</span><span class="sxs-lookup"><span data-stu-id="eeea5-231">azure site list</span></span>
<span data-ttu-id="eeea5-232">Informace o aplikacích v aktuálním předplatném volání **seznam webů azure**, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="eeea5-232">For information about the apps in the current subscription, call **azure site list**, as in the following example.</span></span>

`azure site list webappslotstest`

- - -
### <a name="azure-site-create"></a><span data-ttu-id="eeea5-233">Vytvoření Azure lokality</span><span class="sxs-lookup"><span data-stu-id="eeea5-233">azure site create</span></span>
<span data-ttu-id="eeea5-234">Chcete-li vytvořit slotu nasazení, volejte **azure lokality vytvořit** a zadejte název existující aplikaci a název slotu chcete vytvořit, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="eeea5-234">To create a deployment slot, call **azure site create** and specify the name of an existing app and the name of the slot to create, as in the following example.</span></span>

`azure site create webappslotstest --slot staging`

<span data-ttu-id="eeea5-235">Pokud chcete povolit zdrojového kódu pro nový slot, použijte **– git** možnost, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="eeea5-235">To enable source control for the new slot, use the **--git** option, as in the following example.</span></span>

`azure site create --git webappslotstest --slot staging`

- - -
### <a name="azure-site-swap"></a><span data-ttu-id="eeea5-236">swap Azure lokality</span><span class="sxs-lookup"><span data-stu-id="eeea5-236">azure site swap</span></span>
<span data-ttu-id="eeea5-237">Chcete-li aktualizované nasazení slot na produkční aplikaci, použijte **azure lokality swap** příkazu provést operaci přepnutí, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="eeea5-237">To make the updated deployment slot the production app, use the **azure site swap** command to perform a swap operation, as in the following example.</span></span> <span data-ttu-id="eeea5-238">Na produkční aplikaci nebude mít žádný výpadek ani ho určitým studený start.</span><span class="sxs-lookup"><span data-stu-id="eeea5-238">The production app will not experience any down time, nor will it undergo a cold start.</span></span>

`azure site swap webappslotstest`

- - -
### <a name="azure-site-delete"></a><span data-ttu-id="eeea5-239">odstranění webu Azure</span><span class="sxs-lookup"><span data-stu-id="eeea5-239">azure site delete</span></span>
<span data-ttu-id="eeea5-240">Pokud chcete odstranit slot nasazení, který již není potřeba, použijte **odstranění azure webu** příkazu, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="eeea5-240">To delete a deployment slot that is no longer needed, use the **azure site delete** command, as in the following example.</span></span>

`azure site delete webappslotstest --slot staging`

- - -
> [!NOTE]
> <span data-ttu-id="eeea5-241">Sledujte webovou aplikaci v akci.</span><span class="sxs-lookup"><span data-stu-id="eeea5-241">See a web app in action.</span></span> <span data-ttu-id="eeea5-242">[Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/) okamžitě a vytvořit krátkodobou úvodní aplikaci – nepožaduje se žádná platební karta a bez jakýchkoli závazků.</span><span class="sxs-lookup"><span data-stu-id="eeea5-242">[Try App Service](https://azure.microsoft.com/try/app-service/) immediately and create a short-lived starter app—no credit card required, no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="eeea5-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eeea5-243">Next Steps</span></span>
<span data-ttu-id="eeea5-244">[Azure App Service webové aplikace – blok webového přístupu k testovacím nasazovací sloty](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[Úvod do služby App Service v systému Linux](./app-service-linux-intro.md)
[bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="eeea5-244">[Azure App Service Web App – block web access to non-production deployment slots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[Introduction to App Service on Linux](./app-service-linux-intro.md)
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

