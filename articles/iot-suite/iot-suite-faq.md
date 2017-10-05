---
title: "Azure IoT Suite – nejčastější dotazy | Microsoft Docs"
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
ms.openlocfilehash: 85867fb8d18377637b3aa848555831a8d9b53512
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="frequently-asked-questions-for-iot-suite"></a><span data-ttu-id="f0f33-103">Nejčastější dotazy k sadě IoT Suite</span><span class="sxs-lookup"><span data-stu-id="f0f33-103">Frequently asked questions for IoT Suite</span></span>

<span data-ttu-id="f0f33-104">Viz také konkrétní připojené factory [– nejčastější dotazy](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="f0f33-104">See also, the connected factory specific [FAQ](iot-suite-faq-cf.md).</span></span>

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a><span data-ttu-id="f0f33-105">Kde najdu zdrojový kód pro předkonfigurované řešení?</span><span class="sxs-lookup"><span data-stu-id="f0f33-105">Where can I find the source code for the preconfigured solutions?</span></span>

<span data-ttu-id="f0f33-106">Zdrojový kód je uložena v následující úložišť GitHub:</span><span class="sxs-lookup"><span data-stu-id="f0f33-106">The source code is stored in the following GitHub repositories:</span></span>
* <span data-ttu-id="f0f33-107">[Předkonfigurované řešení vzdáleného monitorování][lnk-remote-monitoring-github]</span><span class="sxs-lookup"><span data-stu-id="f0f33-107">[Remote monitoring preconfigured solution][lnk-remote-monitoring-github]</span></span>
* <span data-ttu-id="f0f33-108">[Předkonfigurované řešení prediktivní údržby][lnk-predictive-maintenance-github]</span><span class="sxs-lookup"><span data-stu-id="f0f33-108">[Predictive maintenance preconfigured solution][lnk-predictive-maintenance-github]</span></span>
* [<span data-ttu-id="f0f33-109">Připojené objekt pro vytváření předkonfigurovaného řešení</span><span class="sxs-lookup"><span data-stu-id="f0f33-109">Connected factory preconfigured solution</span></span>](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-do-i-update-to-the-latest-version-of-the-remote-monitoring-preconfigured-solution-that-uses-the-iot-hub-device-management-features"></a><span data-ttu-id="f0f33-110">Jak aktualizovat na nejnovější verzi předkonfigurovaného řešení vzdáleného monitorování používající funkce správy zařízení IoT Hub?</span><span class="sxs-lookup"><span data-stu-id="f0f33-110">How do I update to the latest version of the remote monitoring preconfigured solution that uses the IoT Hub device management features?</span></span>

* <span data-ttu-id="f0f33-111">Pokud nasadíte předkonfigurované řešení z webu https://www.azureiotsuite.com/, vždy nasadí novou instanci třídy nejnovější verzi řešení.</span><span class="sxs-lookup"><span data-stu-id="f0f33-111">If you deploy a preconfigured solution from the https://www.azureiotsuite.com/ site, it always deploys a new instance of the latest version of the solution.</span></span>
* <span data-ttu-id="f0f33-112">Pokud nasadíte předkonfigurované řešení pomocí příkazového řádku, můžete aktualizovat existující nasazení s nový kód.</span><span class="sxs-lookup"><span data-stu-id="f0f33-112">If you deploy a preconfigured solution using the command line, you can update an existing deployment with new code.</span></span> <span data-ttu-id="f0f33-113">V tématu [cloudové nasazení] [ lnk-cloud-deployment] v Githubu [úložiště][lnk-remote-monitoring-github].</span><span class="sxs-lookup"><span data-stu-id="f0f33-113">See [Cloud deployment][lnk-cloud-deployment] in the GitHub [repository][lnk-remote-monitoring-github].</span></span>

### <a name="how-can-i-add-support-for-a-new-device-method-to-the-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="f0f33-114">Jak můžete přidat podporu pro nové zařízení metody pro předkonfigurované řešení vzdáleného monitorování?</span><span class="sxs-lookup"><span data-stu-id="f0f33-114">How can I add support for a new device method to the remote monitoring preconfigured solution?</span></span>

<span data-ttu-id="f0f33-115">Najdete v části [přidat podporu pro nové metody pro simulátoru] [ lnk-add-method] v [přizpůsobení předkonfigurovaného řešení] [ lnk-customize] článku.</span><span class="sxs-lookup"><span data-stu-id="f0f33-115">See the section [Add support for a new method to the simulator][lnk-add-method] in the [Customize a preconfigured solution][lnk-customize] article.</span></span>

### <a name="the-simulated-device-is-ignoring-my-desired-property-changes-why"></a><span data-ttu-id="f0f33-116">Simulované zařízení provedené změny požadovanou vlastnost, proč ignoruje?</span><span class="sxs-lookup"><span data-stu-id="f0f33-116">The simulated device is ignoring my desired property changes, why?</span></span>
<span data-ttu-id="f0f33-117">V předkonfigurovaného řešení vzdáleného monitorování, simulované zařízení kód používá jenom **Desired.Config.TemperatureMeanValue** a **Desired.Config.TelemetryInterval** potřeby vlastnosti, které chcete aktualizovat hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f0f33-117">In the remote monitoring preconfigured solution, the simulated device code only uses the **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties to update the reported properties.</span></span> <span data-ttu-id="f0f33-118">Všechny ostatní žádosti o změnu požadovanou vlastnost se ignorují.</span><span class="sxs-lookup"><span data-stu-id="f0f33-118">All other desired property change requests are ignored.</span></span>

### <a name="my-device-does-not-appear-in-the-list-of-devices-in-the-solution-dashboard-why"></a><span data-ttu-id="f0f33-119">Moje zařízení nezobrazí v seznamu zařízení na řídicím panelu řešení, proč?</span><span class="sxs-lookup"><span data-stu-id="f0f33-119">My device does not appear in the list of devices in the solution dashboard, why?</span></span>

<span data-ttu-id="f0f33-120">Seznam zařízení na řídicím panelu řešení používá dotaz vrátit seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="f0f33-120">The list of devices in the solution dashboard uses a query to return the list of devices.</span></span> <span data-ttu-id="f0f33-121">V současné době dotaz nemůže vrátit více než 10 tisíc zařízení.</span><span class="sxs-lookup"><span data-stu-id="f0f33-121">Currently, a query cannot return more than 10K devices.</span></span> <span data-ttu-id="f0f33-122">Pokuste konkrétnější kritéria hledání pro svůj dotaz.</span><span class="sxs-lookup"><span data-stu-id="f0f33-122">Try making the search criteria for your query more restrictive.</span></span>

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a><span data-ttu-id="f0f33-123">Jaký je rozdíl mezi odstranění skupiny prostředků na portálu Azure a kliknutím na Odstranit v předkonfigurovaném řešení na stránkách azureiotsuite.com?</span><span class="sxs-lookup"><span data-stu-id="f0f33-123">What's the difference between deleting a resource group in the Azure portal and clicking delete on a preconfigured solution in azureiotsuite.com?</span></span>

* <span data-ttu-id="f0f33-124">Pokud odstraníte předkonfigurované řešení na [azureiotsuite.com][lnk-azureiotsuite], odstranit všechny prostředky, které byly zřízeny při vytváření předkonfigurovaného řešení.</span><span class="sxs-lookup"><span data-stu-id="f0f33-124">If you delete the preconfigured solution in [azureiotsuite.com][lnk-azureiotsuite], you delete all the resources that were provisioned when you created the preconfigured solution.</span></span> <span data-ttu-id="f0f33-125">Pokud jste do skupiny prostředků přidali další prostředky, tyto prostředky budou také odstraněny.</span><span class="sxs-lookup"><span data-stu-id="f0f33-125">If you added additional resources to the resource group, these resources are also deleted.</span></span> 
* <span data-ttu-id="f0f33-126">Pokud odstraníte skupinu prostředků [portál Azure][lnk-azure-portal], se odstraní pouze prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="f0f33-126">If you delete the resource group in the [Azure portal][lnk-azure-portal], you only delete the resources in that resource group.</span></span> <span data-ttu-id="f0f33-127">Musíte také odstranit aplikaci Azure Active Directory, které jsou přidružené k předkonfigurovanému řešení [portál Azure classic][lnk-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="f0f33-127">You also need to delete the Azure Active Directory application associated with the preconfigured solution in the [Azure classic portal][lnk-classic-portal].</span></span>

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a><span data-ttu-id="f0f33-128">Kolik instancí služby IoT Hub můžete zřídit v jednom předplatném?</span><span class="sxs-lookup"><span data-stu-id="f0f33-128">How many IoT Hub instances can I provision in a subscription?</span></span>

<span data-ttu-id="f0f33-129">Ve výchozím nastavení můžete zřídit [10 centra IoT na jedno předplatné][link-azuresublimits].</span><span class="sxs-lookup"><span data-stu-id="f0f33-129">By default you can provision [10 IoT hubs per subscription][link-azuresublimits].</span></span> <span data-ttu-id="f0f33-130">Můžete vytvořit [lístek podpory Azure] [ link-azuresupportticket] chcete tento limit zvýšit.</span><span class="sxs-lookup"><span data-stu-id="f0f33-130">You can create an [Azure support ticket][link-azuresupportticket] to raise this limit.</span></span> <span data-ttu-id="f0f33-131">V důsledku toho vzhledem k tomu, že každé předkonfigurované řešení zřídí novou službu IoT Hub, můžete zřídit až 10 předkonfigurovaných řešení v daném předplatném.</span><span class="sxs-lookup"><span data-stu-id="f0f33-131">As a result, since every preconfigured solution provisions a new IoT Hub, you can only provision up to 10 preconfigured solutions in a given subscription.</span></span> 

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a><span data-ttu-id="f0f33-132">Kolik instancí Azure Cosmos DB můžete zřídit v jednom předplatném?</span><span class="sxs-lookup"><span data-stu-id="f0f33-132">How many Azure Cosmos DB instances can I provision in a subscription?</span></span>

<span data-ttu-id="f0f33-133">Padesát.</span><span class="sxs-lookup"><span data-stu-id="f0f33-133">Fifty.</span></span> <span data-ttu-id="f0f33-134">Můžete vytvořit [lístek podpory Azure] [ link-azuresupportticket] chcete tento limit zvýšit, ale ve výchozím nastavení, můžete zřídit 50 instancí Cosmos DB za předplatné.</span><span class="sxs-lookup"><span data-stu-id="f0f33-134">You can create an [Azure support ticket][link-azuresupportticket] to raise this limit, but by default, you can only provision 50 Cosmos DB instances per subscription.</span></span> 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a><span data-ttu-id="f0f33-135">Kolik bezplatných rozhraní API Map Bing můžu zřídit v jednom předplatném?</span><span class="sxs-lookup"><span data-stu-id="f0f33-135">How many Free Bing Maps APIs can I provision in a subscription?</span></span>

<span data-ttu-id="f0f33-136">Dvě.</span><span class="sxs-lookup"><span data-stu-id="f0f33-136">Two.</span></span> <span data-ttu-id="f0f33-137">Můžete vytvořit pouze dvě vnitřní transakce úroveň 1 mapy Bing pro podnikových plánů v předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="f0f33-137">You can create only two Internal Transactions Level 1 Bing Maps for Enterprise plans in an Azure subscription.</span></span> <span data-ttu-id="f0f33-138">Ve výchozím nastavení s plánem vnitřní transakce úrovně 1 se zřídí řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="f0f33-138">The remote monitoring solution is provisioned by default with the Internal Transactions Level 1 plan.</span></span> <span data-ttu-id="f0f33-139">V důsledku toho můžete v daném předplatném zřídit nanejvýš dvě předkonfigurovaná řešení vzdáleného monitorování bez možnosti úprav.</span><span class="sxs-lookup"><span data-stu-id="f0f33-139">As a result, you can only provision up to two remote monitoring solutions in a subscription with no modifications.</span></span>

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a><span data-ttu-id="f0f33-140">Mám nasazené řešení vzdáleného monitorování se statickou mapou, jak přidám interaktivní mapu Bing?</span><span class="sxs-lookup"><span data-stu-id="f0f33-140">I have a remote monitoring solution deployment with a static map, how do I add an interactive Bing map?</span></span>

1. <span data-ttu-id="f0f33-141">Získání rozhraní API map Bing pro QueryKey Enterprise z [portál Azure][lnk-azure-portal]:</span><span class="sxs-lookup"><span data-stu-id="f0f33-141">Get your Bing Maps API for Enterprise QueryKey from [Azure portal][lnk-azure-portal]:</span></span> 
   
   1. <span data-ttu-id="f0f33-142">Přejděte do skupiny prostředků, kde je vaše rozhraní API map Bing pro podniky v [portál Azure][lnk-azure-portal].</span><span class="sxs-lookup"><span data-stu-id="f0f33-142">Navigate to the Resource Group where your Bing Maps API for Enterprise is in the [Azure portal][lnk-azure-portal].</span></span>
   2. <span data-ttu-id="f0f33-143">Klikněte na tlačítko **všechna nastavení**, pak **Správa klíčů**.</span><span class="sxs-lookup"><span data-stu-id="f0f33-143">Click **All Settings**, then **Key Management**.</span></span> 
   3. <span data-ttu-id="f0f33-144">Zobrazí se dva klíče: **MasterKey** a **QueryKey**.</span><span class="sxs-lookup"><span data-stu-id="f0f33-144">You can see two keys: **MasterKey** and **QueryKey**.</span></span> <span data-ttu-id="f0f33-145">Zkopírujte hodnotu pro **QueryKey**.</span><span class="sxs-lookup"><span data-stu-id="f0f33-145">Copy the value for **QueryKey**.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="f0f33-146">Nemáte účet rozhraní API Map Bing pro podniky?</span><span class="sxs-lookup"><span data-stu-id="f0f33-146">Don't have a Bing Maps API for Enterprise account?</span></span> <span data-ttu-id="f0f33-147">Vytvořit v [portál Azure] [ lnk-azure-portal] nástrojem kliknutím na + nové, vyhledáte rozhraní API map Bing pro podniky a postupujte podle výzev a vytvořte.</span><span class="sxs-lookup"><span data-stu-id="f0f33-147">Create one in the [Azure portal][lnk-azure-portal] by clicking + New, searching for Bing Maps API for Enterprise and follow prompts to create.</span></span>
      > 
      > 
2. <span data-ttu-id="f0f33-148">Stáhněte dolů nejnovější kód [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].</span><span class="sxs-lookup"><span data-stu-id="f0f33-148">Pull down the latest code from the [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].</span></span>
3. <span data-ttu-id="f0f33-149">Spusťte místní nebo cloudové nasazení podle pokynů v nasazení příkazového řádku ve složce /docs/ v úložišti.</span><span class="sxs-lookup"><span data-stu-id="f0f33-149">Run a local or cloud deployment following the command-line deployment guidance in the /docs/ folder in the repository.</span></span> 
4. <span data-ttu-id="f0f33-150">Po spuštění místního nebo cloudového nasazení vyhledejte v kořenové složce soubor *. user.config vytvořený během nasazení.</span><span class="sxs-lookup"><span data-stu-id="f0f33-150">After you've run a local or cloud deployment, look in your root folder for the *.user.config file created during deployment.</span></span> <span data-ttu-id="f0f33-151">Otevřete tento soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="f0f33-151">Open this file in a text editor.</span></span> 
5. <span data-ttu-id="f0f33-152">Změňte následující řádek zahrnout hodnotu, kterou jste zkopírovali z vaší **QueryKey**:</span><span class="sxs-lookup"><span data-stu-id="f0f33-152">Change the following line to include the value you copied from your **QueryKey**:</span></span> 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a><span data-ttu-id="f0f33-153">Můžu vytvořit předkonfigurované řešení, když mám Microsoft Azure pro DreamSpark?</span><span class="sxs-lookup"><span data-stu-id="f0f33-153">Can I create a preconfigured solution if I have Microsoft Azure for DreamSpark?</span></span>

<span data-ttu-id="f0f33-154">V současné době nelze vytvořit předkonfigurované řešení s [Microsoft Azure pro DreamSpark] [ lnk-dreamspark] účtu.</span><span class="sxs-lookup"><span data-stu-id="f0f33-154">Currently, you cannot create a preconfigured solution with a [Microsoft Azure for DreamSpark][lnk-dreamspark] account.</span></span> <span data-ttu-id="f0f33-155">Ale můžete vytvořit [Bezplatný zkušební účet Azure] [ lnk-30daytrial] si během několika minut, který vám umožní vytvořit předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="f0f33-155">However, you can create a [free trial account for Azure][lnk-30daytrial] in just a couple of minutes that enables you create a preconfigured solution.</span></span>

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a><span data-ttu-id="f0f33-156">Můžete vytvořit předkonfigurované řešení, pokud mám předplatné Cloud Solution Provider (CSP)?</span><span class="sxs-lookup"><span data-stu-id="f0f33-156">Can I create a preconfigured solution if I have Cloud Solution Provider (CSP) subscription?</span></span>

<span data-ttu-id="f0f33-157">V současné době nelze vytvořit předkonfigurované řešení s předplatným Cloud Solution Provider (CSP).</span><span class="sxs-lookup"><span data-stu-id="f0f33-157">Currently, you cannot create a preconfigured solution with a Cloud Solution Provider (CSP) subscription.</span></span> <span data-ttu-id="f0f33-158">Ale můžete vytvořit [Bezplatný zkušební účet Azure] [ lnk-30daytrial] si během několika minut, který vám umožní vytvořit předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="f0f33-158">However, you can create a [free trial account for Azure][lnk-30daytrial] in just a couple of minutes that enables you create a preconfigured solution.</span></span>

### <a name="how-do-i-delete-an-aad-tenant"></a><span data-ttu-id="f0f33-159">Jak se odstraním klienta AAD?</span><span class="sxs-lookup"><span data-stu-id="f0f33-159">How do I delete an AAD tenant?</span></span>

<span data-ttu-id="f0f33-160">Najdete v příspěvku blogu od Erica Golpeho [návod odstranění klient služby Azure AD][lnk-delete-aad-tennant].</span><span class="sxs-lookup"><span data-stu-id="f0f33-160">See Eric Golpe's blog post [Walkthrough of Deleting an Azure AD Tenant][lnk-delete-aad-tennant].</span></span>

### <a name="next-steps"></a><span data-ttu-id="f0f33-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0f33-161">Next steps</span></span>

<span data-ttu-id="f0f33-162">Můžete si taky prostudovat některé další funkce a možnosti předkonfigurovaných řešení sady IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="f0f33-162">You can also explore some of the other features and capabilities of the IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="f0f33-163">[Přehled řešení předkonfigurované prediktivní údržby][lnk-predictive-overview]</span><span class="sxs-lookup"><span data-stu-id="f0f33-163">[Predictive maintenance preconfigured solution overview][lnk-predictive-overview]</span></span>
* [<span data-ttu-id="f0f33-164">Přehled připojené objekt pro vytváření předkonfigurovaného řešení</span><span class="sxs-lookup"><span data-stu-id="f0f33-164">Connected factory preconfigured solution overview</span></span>](iot-suite-connected-factory-overview.md)
* <span data-ttu-id="f0f33-165">[Zabezpečení IoT od počátku][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="f0f33-165">[IoT security from the ground up][lnk-security-groundup]</span></span>

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
