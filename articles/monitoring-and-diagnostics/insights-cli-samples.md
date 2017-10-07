---
title: "Ukázky úvodní aaaAzure monitorování 1.0 rozhraní příkazového řádku. | Dokumentace Microsoftu"
description: "Vzorové příkazy rozhraní příkazového řádku 1.0 pro Azure monitorování funkce. Azure monitorování je služba Microsoft Azure, což vám umožní toosend oznámení o výstrahách, volání webové adresy URL založené na hodnotách nakonfigurované telemetrická data a škálování cloudové služby, virtuální počítače a webové aplikace."
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1653aa81-0ee6-4622-9c65-d4801ed9006f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.openlocfilehash: 6cd9cd62b3a1977276563f5e43f5384ccca66247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor--cross-platform-cli-10-quick-start-samples"></a><span data-ttu-id="1ac05-105">Ukázek Azure CLI monitorování napříč platformami 1.0 rychlý start</span><span class="sxs-lookup"><span data-stu-id="1ac05-105">Azure Monitor  Cross-platform CLI 1.0 quick start samples</span></span>
<span data-ttu-id="1ac05-106">Tento článek ukazuje ukázkové toohelp příkazy rozhraní příkazového řádku (CLI) měli přístup k funkcím monitorování Azure.</span><span class="sxs-lookup"><span data-stu-id="1ac05-106">This article shows you sample command-line interface (CLI) commands toohelp you access Azure Monitor features.</span></span> <span data-ttu-id="1ac05-107">Azure monitorování umožňuje tooAutoScale cloudové služby, virtuální počítače a webové aplikace a toosend oznámení výstrah nebo volání webové adresy URL založené na hodnotách nakonfigurované telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="1ac05-107">Azure Monitor allows you tooAutoScale Cloud Services, Virtual Machines, and Web Apps and toosend alert notifications or call web URLs based on values of configured telemetry data.</span></span>

> [!NOTE]
> <span data-ttu-id="1ac05-108">Azure monitorování je hello nový název pro co byla volána "Statistika Azure" až 25 září 2016.</span><span class="sxs-lookup"><span data-stu-id="1ac05-108">Azure Monitor is hello new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="1ac05-109">Ale hello obory názvů a proto níže uvedené příkazy hello stále obsahovat hello "insights".</span><span class="sxs-lookup"><span data-stu-id="1ac05-109">However, hello namespaces and thus hello commands below still contain hello "insights".</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="1ac05-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1ac05-110">Prerequisites</span></span>
<span data-ttu-id="1ac05-111">Pokud jste ještě nenainstalovali hello příkazového řádku Azure CLI, přečtěte si téma [hello instalace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="1ac05-111">If you haven't already installed hello Azure CLI, see [Install hello Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="1ac05-112">Pokud jste obeznámeni s rozhraní příkazového řádku Azure, můžete přečíst další informace naleznete na [hello použití Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](../xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="1ac05-112">If you're unfamiliar with Azure CLI, you can read more about it at [Use hello Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](../xplat-cli-azure-resource-manager.md).</span></span>

<span data-ttu-id="1ac05-113">V systému Windows, nainstalujte npm z hello [Node.js webu](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="1ac05-113">In Windows, install npm from hello [Node.js website](https://nodejs.org/).</span></span> <span data-ttu-id="1ac05-114">Po dokončení instalace hello CMD.exe pomocí oprávnění spustit jako správce, spusťte následující hello ze složky hello nainstalovanou npm:</span><span class="sxs-lookup"><span data-stu-id="1ac05-114">After you complete hello installation, using CMD.exe with Run As Administrator privileges, execute hello following from hello folder where npm is installed:</span></span>

```console
npm install azure-cli --global
```

<span data-ttu-id="1ac05-115">Dále přejděte tooany složku nebo umístění chcete a zadejte hello příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="1ac05-115">Next, navigate tooany folder/location you want and type at hello command-line:</span></span>

```console
azure help
```

## <a name="log-in-tooazure"></a><span data-ttu-id="1ac05-116">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="1ac05-116">Log in tooAzure</span></span>
<span data-ttu-id="1ac05-117">prvním krokem Hello je toologin tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="1ac05-117">hello first step is toologin tooyour Azure account.</span></span>

```console
azure login
```

<span data-ttu-id="1ac05-118">Po spuštění tohoto příkazu, máte toosign v prostřednictvím hello pokyny na obrazovce hello.</span><span class="sxs-lookup"><span data-stu-id="1ac05-118">After running this command, you have toosign in via hello instructions on hello screen.</span></span> <span data-ttu-id="1ac05-119">Potom zobrazí váš účet, TenantId a výchozí ID předplatného. Všechny příkazy lze použít v kontextu hello předplatného výchozí.</span><span class="sxs-lookup"><span data-stu-id="1ac05-119">Afterward, you see your Account, TenantId, and default Subscription Id. All commands work in hello context of your default subscription.</span></span>

<span data-ttu-id="1ac05-120">toolist hello podrobnosti o vaším aktuálním předplatným, použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="1ac05-120">toolist hello details of your current subscription, use hello following command.</span></span>

```console
azure account show
```

<span data-ttu-id="1ac05-121">toochange pracovní kontext tooa jiného předplatného, hello použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="1ac05-121">toochange working context tooa different subscription, use hello following command.</span></span>

```console
azure account set "subscription ID or subscription name"
```

<span data-ttu-id="1ac05-122">toouse Azure Resource Manager a monitorování Azure příkazů, je nutné toobe v režimu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1ac05-122">toouse Azure Resource Manager and Azure Monitor commands, you need toobe in Azure Resource Manager mode.</span></span>

```console
azure config mode arm
```

<span data-ttu-id="1ac05-123">tooview seznam všechny podporované příkazy Azure monitorování, proveďte následující hello.</span><span class="sxs-lookup"><span data-stu-id="1ac05-123">tooview a list of all supported Azure Monitor commands, perform hello following.</span></span>

```console
azure insights
```

## <a name="view-activity-log-for-a-subscription"></a><span data-ttu-id="1ac05-124">Zobrazit protokol aktivity pro předplatné</span><span class="sxs-lookup"><span data-stu-id="1ac05-124">View activity log for a subscription</span></span>
<span data-ttu-id="1ac05-125">tooview seznam aktivity protokolu událostí, proveďte následující hello.</span><span class="sxs-lookup"><span data-stu-id="1ac05-125">tooview a list of activity log events, perform hello following.</span></span>

```console
azure insights logs list [options]
```

<span data-ttu-id="1ac05-126">Zkuste hello následující tooview všechny dostupné možnosti.</span><span class="sxs-lookup"><span data-stu-id="1ac05-126">Try hello following tooview all available options.</span></span>

```console
azure insights logs list -help
```

<span data-ttu-id="1ac05-127">Tady je příklad toolist protokoly podle resourceGroup</span><span class="sxs-lookup"><span data-stu-id="1ac05-127">Here is an example toolist logs by a resourceGroup</span></span>

```console
azure insights logs list --resourceGroup "myrg1"
```

<span data-ttu-id="1ac05-128">Příklad toolist protokoly volající</span><span class="sxs-lookup"><span data-stu-id="1ac05-128">Example toolist logs by caller</span></span>

```console
azure insights logs list --caller "myname@company.com"
```

<span data-ttu-id="1ac05-129">Příklad toolist protokoly volající na typ prostředku, v rámci počáteční a koncové datum</span><span class="sxs-lookup"><span data-stu-id="1ac05-129">Example toolist logs by caller on a resource type, within a start and end date</span></span>

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a><span data-ttu-id="1ac05-130">Práce s výstrahami</span><span class="sxs-lookup"><span data-stu-id="1ac05-130">Work with alerts</span></span>
<span data-ttu-id="1ac05-131">V části toowork hello s výstrahami, můžete použít informace hello.</span><span class="sxs-lookup"><span data-stu-id="1ac05-131">You can use hello information in hello section toowork with alerts.</span></span>

### <a name="get-alert-rules-in-a-resource-group"></a><span data-ttu-id="1ac05-132">Získat pravidla výstrah ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="1ac05-132">Get alert rules in a resource group</span></span>
```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a><span data-ttu-id="1ac05-133">Vytvoření metriky pravidlo výstrahy</span><span class="sxs-lookup"><span data-stu-id="1ac05-133">Create a metric alert rule</span></span>
```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-webtest-alert-rule"></a><span data-ttu-id="1ac05-134">Vytvořit pravidlo výstrahy testu webu</span><span class="sxs-lookup"><span data-stu-id="1ac05-134">Create webtest alert rule</span></span>
```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a><span data-ttu-id="1ac05-135">Odstranit pravidlo výstrahy</span><span class="sxs-lookup"><span data-stu-id="1ac05-135">Delete an alert rule</span></span>
```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a><span data-ttu-id="1ac05-136">Profily protokolu</span><span class="sxs-lookup"><span data-stu-id="1ac05-136">Log profiles</span></span>
<span data-ttu-id="1ac05-137">Použijte hello informace v této části toowork pomocí protokolu profilů.</span><span class="sxs-lookup"><span data-stu-id="1ac05-137">Use hello information in this section toowork with log profiles.</span></span>

### <a name="get-a-log-profile"></a><span data-ttu-id="1ac05-138">Získat profil protokolu</span><span class="sxs-lookup"><span data-stu-id="1ac05-138">Get a log profile</span></span>
```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a><span data-ttu-id="1ac05-139">Přidat profil protokolu bez uchování</span><span class="sxs-lookup"><span data-stu-id="1ac05-139">Add a log profile without retention</span></span>
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a><span data-ttu-id="1ac05-140">Odebrat profil protokolu</span><span class="sxs-lookup"><span data-stu-id="1ac05-140">Remove a log profile</span></span>
```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a><span data-ttu-id="1ac05-141">Přidat profil protokolu s dobou uchování</span><span class="sxs-lookup"><span data-stu-id="1ac05-141">Add a log profile with retention</span></span>
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a><span data-ttu-id="1ac05-142">Přidat profil protokolu s uchovávání a EventHub</span><span class="sxs-lookup"><span data-stu-id="1ac05-142">Add a log profile with retention and EventHub</span></span>
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a><span data-ttu-id="1ac05-143">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="1ac05-143">Diagnostics</span></span>
<span data-ttu-id="1ac05-144">Použijte hello informace v této části toowork se nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="1ac05-144">Use hello information in this section toowork with diagnostic settings.</span></span>

### <a name="get-a-diagnostic-setting"></a><span data-ttu-id="1ac05-145">Získat nastavení diagnostiky</span><span class="sxs-lookup"><span data-stu-id="1ac05-145">Get a diagnostic setting</span></span>
```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a><span data-ttu-id="1ac05-146">Zakažte nastavení diagnostiky</span><span class="sxs-lookup"><span data-stu-id="1ac05-146">Disable a diagnostic setting</span></span>
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a><span data-ttu-id="1ac05-147">Povolit nastavení diagnostiky bez uchování</span><span class="sxs-lookup"><span data-stu-id="1ac05-147">Enable a diagnostic setting without retention</span></span>
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a><span data-ttu-id="1ac05-148">Automatické škálování</span><span class="sxs-lookup"><span data-stu-id="1ac05-148">Autoscale</span></span>
<span data-ttu-id="1ac05-149">Použijte hello informace v této části toowork s nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="1ac05-149">Use hello information in this section toowork with autoscale settings.</span></span> <span data-ttu-id="1ac05-150">Je nutné toomodify tyto příklady.</span><span class="sxs-lookup"><span data-stu-id="1ac05-150">You need toomodify these examples.</span></span>

### <a name="get-autoscale-settings-for-a-resource-group"></a><span data-ttu-id="1ac05-151">Získat nastavení automatického škálování pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="1ac05-151">Get autoscale settings for a resource group</span></span>
```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a><span data-ttu-id="1ac05-152">Získat nastavení automatického škálování podle názvu ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="1ac05-152">Get autoscale settings by name in a resource group</span></span>
```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a><span data-ttu-id="1ac05-153">Nastavení auotoscale</span><span class="sxs-lookup"><span data-stu-id="1ac05-153">Set auotoscale settings</span></span>
```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
