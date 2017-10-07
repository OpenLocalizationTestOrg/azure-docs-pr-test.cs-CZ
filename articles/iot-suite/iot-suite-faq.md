---
title: "aaaAzure IoT Suite – nejčastější dotazy | Microsoft Docs"
description: "Nejčastější dotazy k sadě IoT Suite"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: cb39e24af6d1ce2afea554285512d05b2d7c721e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-iot-suite"></a><span data-ttu-id="8db42-103">Nejčastější dotazy k sadě IoT Suite</span><span class="sxs-lookup"><span data-stu-id="8db42-103">Frequently asked questions for IoT Suite</span></span>

<span data-ttu-id="8db42-104">Viz také konkrétní připojené factory hello [– nejčastější dotazy](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="8db42-104">See also, hello connected factory specific [FAQ](iot-suite-faq-cf.md).</span></span>

### <a name="where-can-i-find-hello-source-code-for-hello-preconfigured-solutions"></a><span data-ttu-id="8db42-105">Kde najdu hello zdrojového kódu pro hello předkonfigurované řešení?</span><span class="sxs-lookup"><span data-stu-id="8db42-105">Where can I find hello source code for hello preconfigured solutions?</span></span>

<span data-ttu-id="8db42-106">Hello zdrojového kódu se ukládají v hello následující úložišť GitHub:</span><span class="sxs-lookup"><span data-stu-id="8db42-106">hello source code is stored in hello following GitHub repositories:</span></span>
* <span data-ttu-id="8db42-107">[Předkonfigurované řešení vzdáleného monitorování][lnk-remote-monitoring-github]</span><span class="sxs-lookup"><span data-stu-id="8db42-107">[Remote monitoring preconfigured solution][lnk-remote-monitoring-github]</span></span>
* <span data-ttu-id="8db42-108">[Předkonfigurované řešení prediktivní údržby][lnk-predictive-maintenance-github]</span><span class="sxs-lookup"><span data-stu-id="8db42-108">[Predictive maintenance preconfigured solution][lnk-predictive-maintenance-github]</span></span>
* [<span data-ttu-id="8db42-109">Připojené objekt pro vytváření předkonfigurovaného řešení</span><span class="sxs-lookup"><span data-stu-id="8db42-109">Connected factory preconfigured solution</span></span>](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-do-i-update-toohello-latest-version-of-hello-remote-monitoring-preconfigured-solution-that-uses-hello-iot-hub-device-management-features"></a><span data-ttu-id="8db42-110">Jak aktualizovat toohello nejnovější verzi hello předkonfigurovaného řešení vzdáleného monitorování, používá hello funkce správy zařízení IoT Hub?</span><span class="sxs-lookup"><span data-stu-id="8db42-110">How do I update toohello latest version of hello remote monitoring preconfigured solution that uses hello IoT Hub device management features?</span></span>

* <span data-ttu-id="8db42-111">Pokud nasadíte předkonfigurované řešení z lokality https://www.azureiotsuite.com/ hello, vždy nasadí novou instanci třídy hello nejnovější verzi hello řešení.</span><span class="sxs-lookup"><span data-stu-id="8db42-111">If you deploy a preconfigured solution from hello https://www.azureiotsuite.com/ site, it always deploys a new instance of hello latest version of hello solution.</span></span>
* <span data-ttu-id="8db42-112">Pokud nasadíte předkonfigurované řešení hello příkazového řádku, můžete aktualizovat existující nasazení s nový kód.</span><span class="sxs-lookup"><span data-stu-id="8db42-112">If you deploy a preconfigured solution using hello command line, you can update an existing deployment with new code.</span></span> <span data-ttu-id="8db42-113">V tématu [cloudové nasazení] [ lnk-cloud-deployment] v hello Githubu [úložiště][lnk-remote-monitoring-github].</span><span class="sxs-lookup"><span data-stu-id="8db42-113">See [Cloud deployment][lnk-cloud-deployment] in hello GitHub [repository][lnk-remote-monitoring-github].</span></span>

### <a name="how-can-i-add-support-for-a-new-device-method-toohello-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="8db42-114">Jak můžete přidat podporu pro nové zařízení metoda toohello předkonfigurovanému řešení vzdáleného monitorování?</span><span class="sxs-lookup"><span data-stu-id="8db42-114">How can I add support for a new device method toohello remote monitoring preconfigured solution?</span></span>

<span data-ttu-id="8db42-115">Části hello [přidat podporu pro nové simulátoru toohello metoda] [ lnk-add-method] v hello [přizpůsobení předkonfigurovaného řešení] [ lnk-customize] článku.</span><span class="sxs-lookup"><span data-stu-id="8db42-115">See hello section [Add support for a new method toohello simulator][lnk-add-method] in hello [Customize a preconfigured solution][lnk-customize] article.</span></span>

### <a name="hello-simulated-device-is-ignoring-my-desired-property-changes-why"></a><span data-ttu-id="8db42-116">Simulované zařízení Hello provedené změny požadovanou vlastnost, proč ignoruje?</span><span class="sxs-lookup"><span data-stu-id="8db42-116">hello simulated device is ignoring my desired property changes, why?</span></span>
<span data-ttu-id="8db42-117">V hello předkonfigurované řešení vzdáleného monitorování, kód hello simulované zařízení používá jenom hello **Desired.Config.TemperatureMeanValue** a **Desired.Config.TelemetryInterval** požadovaných vlastností tooupdate hello hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8db42-117">In hello remote monitoring preconfigured solution, hello simulated device code only uses hello **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties tooupdate hello reported properties.</span></span> <span data-ttu-id="8db42-118">Všechny ostatní žádosti o změnu požadovanou vlastnost se ignorují.</span><span class="sxs-lookup"><span data-stu-id="8db42-118">All other desired property change requests are ignored.</span></span>

### <a name="my-device-does-not-appear-in-hello-list-of-devices-in-hello-solution-dashboard-why"></a><span data-ttu-id="8db42-119">Moje zařízení nezobrazí v seznamu hello zařízení na řídicím panelu řešení hello, proč?</span><span class="sxs-lookup"><span data-stu-id="8db42-119">My device does not appear in hello list of devices in hello solution dashboard, why?</span></span>

<span data-ttu-id="8db42-120">Hello seznam zařízení na řídicím panelu řešení hello používá dotaz tooreturn hello seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="8db42-120">hello list of devices in hello solution dashboard uses a query tooreturn hello list of devices.</span></span> <span data-ttu-id="8db42-121">V současné době dotaz nemůže vrátit více než 10 tisíc zařízení.</span><span class="sxs-lookup"><span data-stu-id="8db42-121">Currently, a query cannot return more than 10K devices.</span></span> <span data-ttu-id="8db42-122">Pokuste více omezující hello kritérií vyhledávání pro svůj dotaz.</span><span class="sxs-lookup"><span data-stu-id="8db42-122">Try making hello search criteria for your query more restrictive.</span></span>

### <a name="whats-hello-difference-between-deleting-a-resource-group-in-hello-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a><span data-ttu-id="8db42-123">Co je hello rozdíl mezi odstranění skupiny prostředků v hello Azure portálu a kliknutím na Odstranit v předkonfigurovaném řešení na stránkách azureiotsuite.com?</span><span class="sxs-lookup"><span data-stu-id="8db42-123">What's hello difference between deleting a resource group in hello Azure portal and clicking delete on a preconfigured solution in azureiotsuite.com?</span></span>

* <span data-ttu-id="8db42-124">Pokud odstraníte hello předkonfigurované řešení v [azureiotsuite.com][lnk-azureiotsuite], odstranit všechny prostředky hello, které byly zřízeny při vytváření hello předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="8db42-124">If you delete hello preconfigured solution in [azureiotsuite.com][lnk-azureiotsuite], you delete all hello resources that were provisioned when you created hello preconfigured solution.</span></span> <span data-ttu-id="8db42-125">Pokud jste přidali skupinu prostředků toohello další prostředky, tyto prostředky budou také odstraněny.</span><span class="sxs-lookup"><span data-stu-id="8db42-125">If you added additional resources toohello resource group, these resources are also deleted.</span></span> 
* <span data-ttu-id="8db42-126">Pokud odstraníte skupinu prostředků hello hello [portál Azure][lnk-azure-portal], se odstraní pouze prostředky hello v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="8db42-126">If you delete hello resource group in hello [Azure portal][lnk-azure-portal], you only delete hello resources in that resource group.</span></span> <span data-ttu-id="8db42-127">Musíte taky toodelete hello Azure Active Directory aplikace spojené s hello předkonfigurované řešení v hello [portál Azure classic][lnk-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="8db42-127">You also need toodelete hello Azure Active Directory application associated with hello preconfigured solution in hello [Azure classic portal][lnk-classic-portal].</span></span>

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a><span data-ttu-id="8db42-128">Kolik instancí služby IoT Hub můžete zřídit v jednom předplatném?</span><span class="sxs-lookup"><span data-stu-id="8db42-128">How many IoT Hub instances can I provision in a subscription?</span></span>

<span data-ttu-id="8db42-129">Ve výchozím nastavení můžete zřídit [10 centra IoT na jedno předplatné][link-azuresublimits].</span><span class="sxs-lookup"><span data-stu-id="8db42-129">By default you can provision [10 IoT hubs per subscription][link-azuresublimits].</span></span> <span data-ttu-id="8db42-130">Můžete vytvořit [lístek podpory Azure] [ link-azuresupportticket] tooraise toto omezení.</span><span class="sxs-lookup"><span data-stu-id="8db42-130">You can create an [Azure support ticket][link-azuresupportticket] tooraise this limit.</span></span> <span data-ttu-id="8db42-131">V důsledku toho od každé předkonfigurované řešení zřizuje novou službu IoT Hub, můžete zřídit až too10 předkonfigurovaných řešení v daném předplatném.</span><span class="sxs-lookup"><span data-stu-id="8db42-131">As a result, since every preconfigured solution provisions a new IoT Hub, you can only provision up too10 preconfigured solutions in a given subscription.</span></span> 

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a><span data-ttu-id="8db42-132">Kolik instancí Azure Cosmos DB můžete zřídit v jednom předplatném?</span><span class="sxs-lookup"><span data-stu-id="8db42-132">How many Azure Cosmos DB instances can I provision in a subscription?</span></span>

<span data-ttu-id="8db42-133">Padesát.</span><span class="sxs-lookup"><span data-stu-id="8db42-133">Fifty.</span></span> <span data-ttu-id="8db42-134">Můžete vytvořit [lístek podpory Azure] [ link-azuresupportticket] tooraise toto omezení, ale ve výchozím nastavení, můžete zřídit 50 instancí Cosmos DB za předplatné.</span><span class="sxs-lookup"><span data-stu-id="8db42-134">You can create an [Azure support ticket][link-azuresupportticket] tooraise this limit, but by default, you can only provision 50 Cosmos DB instances per subscription.</span></span> 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a><span data-ttu-id="8db42-135">Kolik bezplatných rozhraní API Map Bing můžu zřídit v jednom předplatném?</span><span class="sxs-lookup"><span data-stu-id="8db42-135">How many Free Bing Maps APIs can I provision in a subscription?</span></span>

<span data-ttu-id="8db42-136">Dvě.</span><span class="sxs-lookup"><span data-stu-id="8db42-136">Two.</span></span> <span data-ttu-id="8db42-137">Můžete vytvořit pouze dvě vnitřní transakce úroveň 1 mapy Bing pro podnikových plánů v předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="8db42-137">You can create only two Internal Transactions Level 1 Bing Maps for Enterprise plans in an Azure subscription.</span></span> <span data-ttu-id="8db42-138">ve výchozím nastavení s plánem hello vnitřní transakce úrovně 1 se zřídí řešení vzdáleného monitorování Hello.</span><span class="sxs-lookup"><span data-stu-id="8db42-138">hello remote monitoring solution is provisioned by default with hello Internal Transactions Level 1 plan.</span></span> <span data-ttu-id="8db42-139">V důsledku toho můžete zřídit až tootwo vzdáleného sledování řešení v předplatném beze změn.</span><span class="sxs-lookup"><span data-stu-id="8db42-139">As a result, you can only provision up tootwo remote monitoring solutions in a subscription with no modifications.</span></span>

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a><span data-ttu-id="8db42-140">Mám nasazené řešení vzdáleného monitorování se statickou mapou, jak přidám interaktivní mapu Bing?</span><span class="sxs-lookup"><span data-stu-id="8db42-140">I have a remote monitoring solution deployment with a static map, how do I add an interactive Bing map?</span></span>

1. <span data-ttu-id="8db42-141">Získání rozhraní API map Bing pro QueryKey Enterprise z [portál Azure][lnk-azure-portal]:</span><span class="sxs-lookup"><span data-stu-id="8db42-141">Get your Bing Maps API for Enterprise QueryKey from [Azure portal][lnk-azure-portal]:</span></span> 
   
   1. <span data-ttu-id="8db42-142">Přejděte toohello skupinu prostředků, je-li vaše rozhraní API map Bing pro podniky hello [portál Azure][lnk-azure-portal].</span><span class="sxs-lookup"><span data-stu-id="8db42-142">Navigate toohello Resource Group where your Bing Maps API for Enterprise is in hello [Azure portal][lnk-azure-portal].</span></span>
   2. <span data-ttu-id="8db42-143">Klikněte na tlačítko **všechna nastavení**, pak **Správa klíčů**.</span><span class="sxs-lookup"><span data-stu-id="8db42-143">Click **All Settings**, then **Key Management**.</span></span> 
   3. <span data-ttu-id="8db42-144">Zobrazí se dva klíče: **MasterKey** a **QueryKey**.</span><span class="sxs-lookup"><span data-stu-id="8db42-144">You can see two keys: **MasterKey** and **QueryKey**.</span></span> <span data-ttu-id="8db42-145">Zkopírujte hodnotu hello **QueryKey**.</span><span class="sxs-lookup"><span data-stu-id="8db42-145">Copy hello value for **QueryKey**.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="8db42-146">Nemáte účet rozhraní API Map Bing pro podniky?</span><span class="sxs-lookup"><span data-stu-id="8db42-146">Don't have a Bing Maps API for Enterprise account?</span></span> <span data-ttu-id="8db42-147">Vytvořit v hello [portál Azure] [ lnk-azure-portal] kliknutím na + nové, vyhledáte rozhraní API map Bing pro podniky a postupujte podle výzvy toocreate.</span><span class="sxs-lookup"><span data-stu-id="8db42-147">Create one in hello [Azure portal][lnk-azure-portal] by clicking + New, searching for Bing Maps API for Enterprise and follow prompts toocreate.</span></span>
      > 
      > 
2. <span data-ttu-id="8db42-148">Stáhněte dolů hello nejnovější kód z hello [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].</span><span class="sxs-lookup"><span data-stu-id="8db42-148">Pull down hello latest code from hello [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].</span></span>
3. <span data-ttu-id="8db42-149">Spusťte místní nebo cloudové nasazení podle pokynů hello příkazového řádku nasazení hello složce /docs/ v úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="8db42-149">Run a local or cloud deployment following hello command-line deployment guidance in hello /docs/ folder in hello repository.</span></span> 
4. <span data-ttu-id="8db42-150">Po spuštění místního nebo cloudového nasazení vyhledejte v kořenové složce hello *. User.config vytvořený během nasazení.</span><span class="sxs-lookup"><span data-stu-id="8db42-150">After you've run a local or cloud deployment, look in your root folder for hello *.user.config file created during deployment.</span></span> <span data-ttu-id="8db42-151">Otevřete tento soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="8db42-151">Open this file in a text editor.</span></span> 
5. <span data-ttu-id="8db42-152">Změna hello následující řádek tooinclude hello hodnotu, kterou jste zkopírovali z vaší **QueryKey**:</span><span class="sxs-lookup"><span data-stu-id="8db42-152">Change hello following line tooinclude hello value you copied from your **QueryKey**:</span></span> 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a><span data-ttu-id="8db42-153">Můžu vytvořit předkonfigurované řešení, když mám Microsoft Azure pro DreamSpark?</span><span class="sxs-lookup"><span data-stu-id="8db42-153">Can I create a preconfigured solution if I have Microsoft Azure for DreamSpark?</span></span>

<span data-ttu-id="8db42-154">V současné době nelze vytvořit předkonfigurované řešení s [Microsoft Azure pro DreamSpark] [ lnk-dreamspark] účtu.</span><span class="sxs-lookup"><span data-stu-id="8db42-154">Currently, you cannot create a preconfigured solution with a [Microsoft Azure for DreamSpark][lnk-dreamspark] account.</span></span> <span data-ttu-id="8db42-155">Ale můžete vytvořit [Bezplatný zkušební účet Azure] [ lnk-30daytrial] si během několika minut, který vám umožní vytvořit předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="8db42-155">However, you can create a [free trial account for Azure][lnk-30daytrial] in just a couple of minutes that enables you create a preconfigured solution.</span></span>

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a><span data-ttu-id="8db42-156">Můžete vytvořit předkonfigurované řešení, pokud mám předplatné Cloud Solution Provider (CSP)?</span><span class="sxs-lookup"><span data-stu-id="8db42-156">Can I create a preconfigured solution if I have Cloud Solution Provider (CSP) subscription?</span></span>

<span data-ttu-id="8db42-157">V současné době nelze vytvořit předkonfigurované řešení s předplatným Cloud Solution Provider (CSP).</span><span class="sxs-lookup"><span data-stu-id="8db42-157">Currently, you cannot create a preconfigured solution with a Cloud Solution Provider (CSP) subscription.</span></span> <span data-ttu-id="8db42-158">Ale můžete vytvořit [Bezplatný zkušební účet Azure] [ lnk-30daytrial] si během několika minut, který vám umožní vytvořit předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="8db42-158">However, you can create a [free trial account for Azure][lnk-30daytrial] in just a couple of minutes that enables you create a preconfigured solution.</span></span>

### <a name="how-do-i-delete-an-aad-tenant"></a><span data-ttu-id="8db42-159">Jak se odstraním klienta AAD?</span><span class="sxs-lookup"><span data-stu-id="8db42-159">How do I delete an AAD tenant?</span></span>

<span data-ttu-id="8db42-160">Najdete v příspěvku blogu od Erica Golpeho [návod odstranění klient služby Azure AD][lnk-delete-aad-tennant].</span><span class="sxs-lookup"><span data-stu-id="8db42-160">See Eric Golpe's blog post [Walkthrough of Deleting an Azure AD Tenant][lnk-delete-aad-tennant].</span></span>

### <a name="next-steps"></a><span data-ttu-id="8db42-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8db42-161">Next steps</span></span>

<span data-ttu-id="8db42-162">Můžete také prozkoumat některé hello další funkce a možnosti hello předkonfigurovaná řešení IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="8db42-162">You can also explore some of hello other features and capabilities of hello IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="8db42-163">[Přehled řešení předkonfigurované prediktivní údržby][lnk-predictive-overview]</span><span class="sxs-lookup"><span data-stu-id="8db42-163">[Predictive maintenance preconfigured solution overview][lnk-predictive-overview]</span></span>
* [<span data-ttu-id="8db42-164">Přehled připojené objekt pro vytváření předkonfigurovaného řešení</span><span class="sxs-lookup"><span data-stu-id="8db42-164">Connected factory preconfigured solution overview</span></span>](iot-suite-connected-factory-overview.md)
* <span data-ttu-id="8db42-165">[Zabezpečení IoT z hello pozadí][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="8db42-165">[IoT security from hello ground up][lnk-security-groundup]</span></span>

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
[lnk-cloud-deployment]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
[lnk-add-method]: iot-suite-guidance-on-customizing-preconfigured-solutions.md#add-support-for-a-new-method-to-the-simulator
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-predictive-maintenance-github]: https://github.com/Azure/azure-iot-predictive-maintenance
