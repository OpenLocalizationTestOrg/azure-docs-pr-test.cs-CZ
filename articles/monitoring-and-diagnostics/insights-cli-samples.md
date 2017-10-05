---
title: "Ukázek Azure CLI 1.0 monitorování rychlý start. | Dokumentace Microsoftu"
description: "Vzorové příkazy rozhraní příkazového řádku 1.0 pro Azure monitorování funkce. Azure monitorování je služba Microsoft Azure, který umožňuje odeslat oznámení o výstrahách, volání webové adresy URL založené na hodnotách nakonfigurované telemetrická data a škálování cloudové služby, virtuální počítače a webové aplikace."
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
ms.openlocfilehash: ec4512500dc3c77a40d2ebd1e6b460d5bb005811
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-monitor--cross-platform-cli-10-quick-start-samples"></a><span data-ttu-id="f045a-105">Ukázek Azure CLI monitorování napříč platformami 1.0 rychlý start</span><span class="sxs-lookup"><span data-stu-id="f045a-105">Azure Monitor  Cross-platform CLI 1.0 quick start samples</span></span>
<span data-ttu-id="f045a-106">Tento článek ukazuje ukázku, že příkazy rozhraní příkazového řádku (CLI) můžete získat přístup k funkcím Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="f045a-106">This article shows you sample command-line interface (CLI) commands to help you access Azure Monitor features.</span></span> <span data-ttu-id="f045a-107">Azure monitorování umožňuje škálování cloudové služby, virtuální počítače a webových aplikací a odesílat oznámení o výstrahách nebo volání webové adresy URL založené na hodnotách nakonfigurované telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="f045a-107">Azure Monitor allows you to AutoScale Cloud Services, Virtual Machines, and Web Apps and to send alert notifications or call web URLs based on values of configured telemetry data.</span></span>

> [!NOTE]
> <span data-ttu-id="f045a-108">Azure monitorování je nový název pro co byla volána "Statistika Azure" až 25 září 2016.</span><span class="sxs-lookup"><span data-stu-id="f045a-108">Azure Monitor is the new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="f045a-109">Však obory názvů a proto níže uvedených příkazů stále obsahovat "insights".</span><span class="sxs-lookup"><span data-stu-id="f045a-109">However, the namespaces and thus the commands below still contain the "insights".</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="f045a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f045a-110">Prerequisites</span></span>
<span data-ttu-id="f045a-111">Pokud jste ještě nenainstalovali Azure CLI, přečtěte si téma [nainstalovat Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="f045a-111">If you haven't already installed the Azure CLI, see [Install the Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="f045a-112">Pokud jste obeznámeni s rozhraní příkazového řádku Azure, můžete přečíst další informace naleznete na [pomocí rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](../xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="f045a-112">If you're unfamiliar with Azure CLI, you can read more about it at [Use the Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](../xplat-cli-azure-resource-manager.md).</span></span>

<span data-ttu-id="f045a-113">V systému Windows, nainstalujte npm z [Node.js webu](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="f045a-113">In Windows, install npm from the [Node.js website](https://nodejs.org/).</span></span> <span data-ttu-id="f045a-114">Po dokončení instalace pomocí CMD.exe s oprávněním spustit jako správce, spusťte ze složky, kde je nainstalován npm následující:</span><span class="sxs-lookup"><span data-stu-id="f045a-114">After you complete the installation, using CMD.exe with Run As Administrator privileges, execute the following from the folder where npm is installed:</span></span>

```console
npm install azure-cli --global
```

<span data-ttu-id="f045a-115">Potom přejděte na všechny složky nebo umístění chcete a zadejte v příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="f045a-115">Next, navigate to any folder/location you want and type at the command-line:</span></span>

```console
azure help
```

## <a name="log-in-to-azure"></a><span data-ttu-id="f045a-116">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="f045a-116">Log in to Azure</span></span>
<span data-ttu-id="f045a-117">Prvním krokem je přihlášení k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="f045a-117">The first step is to login to your Azure account.</span></span>

```console
azure login
```

<span data-ttu-id="f045a-118">Po spuštění tohoto příkazu, budete muset přihlásit pomocí pokynů na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="f045a-118">After running this command, you have to sign in via the instructions on the screen.</span></span> <span data-ttu-id="f045a-119">Potom zobrazí váš účet, TenantId a výchozí ID předplatného. Všechny příkazy fungovat v rámci vašeho předplatného výchozí.</span><span class="sxs-lookup"><span data-stu-id="f045a-119">Afterward, you see your Account, TenantId, and default Subscription Id. All commands work in the context of your default subscription.</span></span>

<span data-ttu-id="f045a-120">K zobrazení seznamu podrobnosti vaším aktuálním předplatným, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="f045a-120">To list the details of your current subscription, use the following command.</span></span>

```console
azure account show
```

<span data-ttu-id="f045a-121">Chcete-li změnit pracovní kontext do jiného předplatného, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="f045a-121">To change working context to a different subscription, use the following command.</span></span>

```console
azure account set "subscription ID or subscription name"
```

<span data-ttu-id="f045a-122">Pokud chcete používat příkazy Azure Resource Manager a monitorování Azure, musíte být v režimu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f045a-122">To use Azure Resource Manager and Azure Monitor commands, you need to be in Azure Resource Manager mode.</span></span>

```console
azure config mode arm
```

<span data-ttu-id="f045a-123">Chcete-li zobrazit seznam všech podporovaných příkazů monitorování Azure, postupujte takto.</span><span class="sxs-lookup"><span data-stu-id="f045a-123">To view a list of all supported Azure Monitor commands, perform the following.</span></span>

```console
azure insights
```

## <a name="view-activity-log-for-a-subscription"></a><span data-ttu-id="f045a-124">Zobrazit protokol aktivity pro předplatné</span><span class="sxs-lookup"><span data-stu-id="f045a-124">View activity log for a subscription</span></span>
<span data-ttu-id="f045a-125">Chcete-li zobrazit seznam aktivity protokolu události, postupujte takto.</span><span class="sxs-lookup"><span data-stu-id="f045a-125">To view a list of activity log events, perform the following.</span></span>

```console
azure insights logs list [options]
```

<span data-ttu-id="f045a-126">Zkuste následující příkaz a zobrazí všechny dostupné možnosti.</span><span class="sxs-lookup"><span data-stu-id="f045a-126">Try the following to view all available options.</span></span>

```console
azure insights logs list -help
```

<span data-ttu-id="f045a-127">Tady je příklad do seznamu protokolů podle resourceGroup</span><span class="sxs-lookup"><span data-stu-id="f045a-127">Here is an example to list logs by a resourceGroup</span></span>

```console
azure insights logs list --resourceGroup "myrg1"
```

<span data-ttu-id="f045a-128">Příklad do seznamu protokolů volající</span><span class="sxs-lookup"><span data-stu-id="f045a-128">Example to list logs by caller</span></span>

```console
azure insights logs list --caller "myname@company.com"
```

<span data-ttu-id="f045a-129">Příklad seznamu protokolů volající na typ prostředku v rámci počáteční a koncové datum</span><span class="sxs-lookup"><span data-stu-id="f045a-129">Example to list logs by caller on a resource type, within a start and end date</span></span>

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a><span data-ttu-id="f045a-130">Práce s výstrahami</span><span class="sxs-lookup"><span data-stu-id="f045a-130">Work with alerts</span></span>
<span data-ttu-id="f045a-131">Informace v části slouží k práci s výstrahami.</span><span class="sxs-lookup"><span data-stu-id="f045a-131">You can use the information in the section to work with alerts.</span></span>

### <a name="get-alert-rules-in-a-resource-group"></a><span data-ttu-id="f045a-132">Získat pravidla výstrah ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="f045a-132">Get alert rules in a resource group</span></span>
```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a><span data-ttu-id="f045a-133">Vytvoření metriky pravidlo výstrahy</span><span class="sxs-lookup"><span data-stu-id="f045a-133">Create a metric alert rule</span></span>
```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-webtest-alert-rule"></a><span data-ttu-id="f045a-134">Vytvořit pravidlo výstrahy testu webu</span><span class="sxs-lookup"><span data-stu-id="f045a-134">Create webtest alert rule</span></span>
```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a><span data-ttu-id="f045a-135">Odstranit pravidlo výstrahy</span><span class="sxs-lookup"><span data-stu-id="f045a-135">Delete an alert rule</span></span>
```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a><span data-ttu-id="f045a-136">Profily protokolu</span><span class="sxs-lookup"><span data-stu-id="f045a-136">Log profiles</span></span>
<span data-ttu-id="f045a-137">Použijte informace v této části pro práci s profily protokolu.</span><span class="sxs-lookup"><span data-stu-id="f045a-137">Use the information in this section to work with log profiles.</span></span>

### <a name="get-a-log-profile"></a><span data-ttu-id="f045a-138">Získat profil protokolu</span><span class="sxs-lookup"><span data-stu-id="f045a-138">Get a log profile</span></span>
```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a><span data-ttu-id="f045a-139">Přidat profil protokolu bez uchování</span><span class="sxs-lookup"><span data-stu-id="f045a-139">Add a log profile without retention</span></span>
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a><span data-ttu-id="f045a-140">Odebrat profil protokolu</span><span class="sxs-lookup"><span data-stu-id="f045a-140">Remove a log profile</span></span>
```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a><span data-ttu-id="f045a-141">Přidat profil protokolu s dobou uchování</span><span class="sxs-lookup"><span data-stu-id="f045a-141">Add a log profile with retention</span></span>
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a><span data-ttu-id="f045a-142">Přidat profil protokolu s uchovávání a EventHub</span><span class="sxs-lookup"><span data-stu-id="f045a-142">Add a log profile with retention and EventHub</span></span>
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a><span data-ttu-id="f045a-143">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="f045a-143">Diagnostics</span></span>
<span data-ttu-id="f045a-144">Použijte informace v této části pro práci s nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="f045a-144">Use the information in this section to work with diagnostic settings.</span></span>

### <a name="get-a-diagnostic-setting"></a><span data-ttu-id="f045a-145">Získat nastavení diagnostiky</span><span class="sxs-lookup"><span data-stu-id="f045a-145">Get a diagnostic setting</span></span>
```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a><span data-ttu-id="f045a-146">Zakažte nastavení diagnostiky</span><span class="sxs-lookup"><span data-stu-id="f045a-146">Disable a diagnostic setting</span></span>
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a><span data-ttu-id="f045a-147">Povolit nastavení diagnostiky bez uchování</span><span class="sxs-lookup"><span data-stu-id="f045a-147">Enable a diagnostic setting without retention</span></span>
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a><span data-ttu-id="f045a-148">Automatické škálování</span><span class="sxs-lookup"><span data-stu-id="f045a-148">Autoscale</span></span>
<span data-ttu-id="f045a-149">Použijte informace v této části pro práci s nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="f045a-149">Use the information in this section to work with autoscale settings.</span></span> <span data-ttu-id="f045a-150">Budete muset upravit tyto příklady.</span><span class="sxs-lookup"><span data-stu-id="f045a-150">You need to modify these examples.</span></span>

### <a name="get-autoscale-settings-for-a-resource-group"></a><span data-ttu-id="f045a-151">Získat nastavení automatického škálování pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="f045a-151">Get autoscale settings for a resource group</span></span>
```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a><span data-ttu-id="f045a-152">Získat nastavení automatického škálování podle názvu ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="f045a-152">Get autoscale settings by name in a resource group</span></span>
```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a><span data-ttu-id="f045a-153">Nastavení auotoscale</span><span class="sxs-lookup"><span data-stu-id="f045a-153">Set auotoscale settings</span></span>
```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
