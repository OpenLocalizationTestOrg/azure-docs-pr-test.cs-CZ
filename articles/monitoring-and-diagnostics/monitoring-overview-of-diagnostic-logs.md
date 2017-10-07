---
title: "aaaOverview diagnostických protokolů Azure | Microsoft Docs"
description: "Zjistěte, jaké jsou Azure diagnostické protokoly a jak lze využít toounderstand událostí v rámci prostředek služby Azure."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem; magoedte
ms.openlocfilehash: e38991c540626b4bb5b5b9a995276881ee58f368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a><span data-ttu-id="bb3ac-103">Shromažďování a využití dat protokolu z vašich prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="bb3ac-103">Collect and consume log data from your Azure resources</span></span>

## <a name="what-are-azure-resource-diagnostic-logs"></a><span data-ttu-id="bb3ac-104">Jaké jsou diagnostické protokoly prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="bb3ac-104">What are Azure resource diagnostic logs</span></span>
<span data-ttu-id="bb3ac-105">**Diagnostické protokoly Azure úrovni prostředků** vygenerované prostředek protokoly, které poskytují bohatou a často data o operaci hello tohoto prostředku.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-105">**Azure resource-level diagnostic logs** are logs emitted by a resource that provide rich, frequent data about hello operation of that resource.</span></span> <span data-ttu-id="bb3ac-106">obsah Hello tyto protokoly se liší podle typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-106">hello content of these logs varies by resource type.</span></span> <span data-ttu-id="bb3ac-107">Například pravidlo čítače skupinu zabezpečení sítě a audity Key Vault jsou dvě kategorie protokoly prostředku.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-107">For example, Network Security Group rule counters and Key Vault audits are two categories of resource logs.</span></span>

<span data-ttu-id="bb3ac-108">Diagnostické protokoly na úrovni prostředků se liší od hello [protokol aktivit](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="bb3ac-108">Resource-level diagnostic logs differ from hello [Activity Log](monitoring-overview-activity-logs.md).</span></span> <span data-ttu-id="bb3ac-109">Hello protokol aktivit poskytuje náhled do hello operací, které byly provedeny v prostředky ve vašem předplatném pomocí Resource Manager, například vytváření virtuálního počítače nebo odstranění aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-109">hello Activity Log provides insight into hello operations that were performed on resources in your subscription using Resource Manager, for example, creating a virtual machine or deleting a logic app.</span></span> <span data-ttu-id="bb3ac-110">Hello protokol aktivit je protokolu úrovni předplatného.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-110">hello Activity Log is a subscription-level log.</span></span> <span data-ttu-id="bb3ac-111">Diagnostické protokoly na úrovni prostředků získat přehled o operace, které byly provedeny v rámci prostředku samostatně, například získání tajného klíče z Key Vault.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-111">Resource-level diagnostic logs provide insight into operations that were performed within that resource itself, for example, getting a secret from a Key Vault.</span></span>

<span data-ttu-id="bb3ac-112">Diagnostické protokoly na úrovni prostředků se také liší od hostovaného operačního systému na úrovni diagnostických protokolů.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-112">Resource-level diagnostic logs also differ from guest OS-level diagnostic logs.</span></span> <span data-ttu-id="bb3ac-113">Diagnostické protokoly hostovaného operačního systému jsou tyto shromažďují agenta spuštěného v rámci virtuálního počítače nebo jiné podporované typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-113">Guest OS diagnostic logs are those collected by an agent running inside of a virtual machine or other supported resource type.</span></span> <span data-ttu-id="bb3ac-114">Diagnostické protokoly na úrovni prostředků vyžadují specifické pro zdroje data z hello platformy Azure, samostatně, bez agenta a zachycení, zatímco hostovaného operačního systému na úrovni diagnostických protokolů zaznamenání dat z hello operační systém a aplikace běžící na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-114">Resource-level diagnostic logs require no agent and capture resource-specific data from hello Azure platform itself, while guest OS-level diagnostic logs capture data from hello operating system and applications running on a virtual machine.</span></span>

<span data-ttu-id="bb3ac-115">Ne všechny prostředky podporovat hello nového typu prostředku diagnostické protokoly zde popsané.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-115">Not all resources support hello new type of resource diagnostic logs described here.</span></span> <span data-ttu-id="bb3ac-116">Tento článek obsahuje v části seznamu, které typy prostředků podporují hello nové úrovni prostředků diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-116">This article contains a section listing which resource types support hello new resource-level diagnostic logs.</span></span>

![<span data-ttu-id="bb3ac-117">Diagnostika prostředků protokoly vs jiné typy protokolů</span><span class="sxs-lookup"><span data-stu-id="bb3ac-117">Resource diagnostics logs vs other types of logs</span></span> ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

## <a name="what-you-can-do-with-resource-level-diagnostic-logs"></a><span data-ttu-id="bb3ac-118">Co můžete dělat s diagnostické protokoly na úrovni prostředků</span><span class="sxs-lookup"><span data-stu-id="bb3ac-118">What you can do with resource-level diagnostic logs</span></span>
<span data-ttu-id="bb3ac-119">Tady jsou některé hello věcí, které můžete provést prostředků diagnostických protokolů:</span><span class="sxs-lookup"><span data-stu-id="bb3ac-119">Here are some of hello things you can do with resource diagnostic logs:</span></span>

![Logické umístění prostředků diagnostických protokolů](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)


* <span data-ttu-id="bb3ac-121">Uložíte tooa [ **účet úložiště** ](monitoring-archive-diagnostic-logs.md) pro auditování nebo ruční kontroly.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-121">Save them tooa [**Storage Account**](monitoring-archive-diagnostic-logs.md) for auditing or manual inspection.</span></span> <span data-ttu-id="bb3ac-122">Můžete zadat hello uchování (ve dnech) pomocí **nastavení diagnostiky pro prostředek**.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-122">You can specify hello retention time (in days) using **resource diagnostic settings**.</span></span>
* <span data-ttu-id="bb3ac-123">[Stream je příliš**Event Hubs** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) pro přijímání službu třetí strany nebo řešení vlastní analýzy, jako je například PowerBI.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-123">[Stream them too**Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md) for ingestion by a third-party service or custom analytics solution such as PowerBI.</span></span>
* <span data-ttu-id="bb3ac-124">Analyzovat, s [OMS analýzy protokolů](../log-analytics/log-analytics-azure-storage.md)</span><span class="sxs-lookup"><span data-stu-id="bb3ac-124">Analyze them with [OMS Log Analytics](../log-analytics/log-analytics-azure-storage.md)</span></span>

<span data-ttu-id="bb3ac-125">Můžete použít účet úložiště nebo obor názvů služby Event Hubs, který není součástí hello stejného předplatného jako jeden emitování protokoly hello.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-125">You can use a storage account or Event Hubs namespace that is not in hello same subscription as hello one emitting logs.</span></span> <span data-ttu-id="bb3ac-126">Hello uživatel, který konfiguruje nastavení hello musí mít hello příslušné RBAC přístup tooboth předplatné.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-126">hello user who configures hello setting must have hello appropriate RBAC access tooboth subscriptions.</span></span>

## <a name="resource-diagnostic-settings"></a><span data-ttu-id="bb3ac-127">Nastavení diagnostiky pro prostředek</span><span class="sxs-lookup"><span data-stu-id="bb3ac-127">Resource diagnostic settings</span></span>
<span data-ttu-id="bb3ac-128">Diagnostické protokoly prostředků pro jiné výpočetní, že prostředky jsou nakonfigurovány pomocí nastavení diagnostiky pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-128">Resource diagnostic logs for non-Compute resources are configured using resource diagnostic settings.</span></span> <span data-ttu-id="bb3ac-129">**Nastavení diagnostiky pro prostředek** pro ovládací prvek prostředku:</span><span class="sxs-lookup"><span data-stu-id="bb3ac-129">**Resource diagnostic settings** for a resource control:</span></span>

* <span data-ttu-id="bb3ac-130">Kde prostředků diagnostické protokoly a metriky se odesílají (účet úložiště, Event Hubs nebo analýzy protokolů OMS).</span><span class="sxs-lookup"><span data-stu-id="bb3ac-130">Where resource diagnostic logs and metrics are sent (Storage Account, Event Hubs, and/or OMS Log Analytics).</span></span>
* <span data-ttu-id="bb3ac-131">Kategorie protokolu, které se odesílají a jestli metriky data jsou taktéž odeslána.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-131">Which log categories are sent and whether metric data is also sent.</span></span>
* <span data-ttu-id="bb3ac-132">Jak dlouho každou kategorii protokolu se uchovávají v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="bb3ac-132">How long each log category should be retained in a storage account</span></span>
    - <span data-ttu-id="bb3ac-133">Uchování nulový počet dnů znamená, že jsou protokoly v nekonečné smyčce.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-133">A retention of zero days means logs are kept forever.</span></span> <span data-ttu-id="bb3ac-134">Hodnota hello, jinak hodnota může být libovolný počet dnů od 1 do 2147483647.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-134">Otherwise, hello value can be any number of days between 1 and 2147483647.</span></span>
    - <span data-ttu-id="bb3ac-135">Pokud nejsou nastavené zásady uchovávání informací, ale ukládání protokolů v účtu úložiště je zakázaný (například pokud jenom jsou vybrané možnosti služby Event Hubs nebo OMS), zásady uchovávání informací hello nemají žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-135">If retention policies are set but storing logs in a Storage Account is disabled (for example, if only Event Hubs or OMS options are selected), hello retention policies have no effect.</span></span>
    - <span data-ttu-id="bb3ac-136">Zásady uchovávání informací jsou použité za den, tak při hello Konec dne (UTC), zaprotokoluje hello dni, který je teď nad rámec zásad uchovávání informací hello se odstraní.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-136">Retention policies are applied per-day, so at hello end of a day (UTC), logs from hello day that is now beyond hello retention policy are deleted.</span></span> <span data-ttu-id="bb3ac-137">Například pokud jste měli zásady uchovávání informací jeden den, od začátku hello dnešní den hello hello protokoly z hello den před včerejšek by odstraněn.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-137">For example, if you had a retention policy of one day, at hello beginning of hello day today hello logs from hello day before yesterday would be deleted.</span></span>

<span data-ttu-id="bb3ac-138">Tato nastavení jsou konfigurována snadno prostřednictvím hello nastavení diagnostiky pro prostředek v hello portálu Azure, pomocí příkazů prostředí Azure PowerShell a rozhraní příkazového řádku nebo prostřednictvím hello [REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb3ac-138">These settings are easily configured via hello diagnostic settings for a resource in hello Azure portal, via Azure PowerShell and CLI commands, or via hello [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>

> [!WARNING]
> <span data-ttu-id="bb3ac-139">Diagnostické protokoly a metriky pro z hello hostovaného operačního systému vrstvy výpočetní prostředky (například virtuální počítače nebo Service Fabric) použití [samostatné mechanismus konfigurace a výběr výstupy](../azure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="bb3ac-139">Diagnostic logs and metrics for from hello guest OS layer of Compute resources (for example, VMs or Service Fabric) use [a separate mechanism for configuration and selection of outputs](../azure-diagnostics.md).</span></span>
>
>

## <a name="how-tooenable-collection-of-resource-diagnostic-logs"></a><span data-ttu-id="bb3ac-140">Jak tooenable shromažďování diagnostických protokolů prostředků</span><span class="sxs-lookup"><span data-stu-id="bb3ac-140">How tooenable collection of resource diagnostic logs</span></span>
<span data-ttu-id="bb3ac-141">Povolením shromažďování diagnostických protokolů prostředků [jako součást vytvoření prostředku v šabloně Resource Manager](./monitoring-enable-diagnostic-logs-using-template.md) nebo po vytvoření prostředku z tohoto zdroje stránky portálu hello.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-141">Collection of resource diagnostic logs can be enabled [as part of creating a resource in a Resource Manager template](./monitoring-enable-diagnostic-logs-using-template.md) or after a resource is created from that resource's page in hello portal.</span></span> <span data-ttu-id="bb3ac-142">Můžete také povolit kolekce v libovolném bodě pomocí příkazů prostředí Azure PowerShell nebo rozhraní příkazového řádku nebo pomocí hello REST API služby Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-142">You can also enable collection at any point using Azure PowerShell or CLI commands, or using hello Azure Monitor REST API.</span></span>

> [!TIP]
> <span data-ttu-id="bb3ac-143">Tyto pokyny nemusí vztahovat přímo tooevery prostředků.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-143">These instructions may not apply directly tooevery resource.</span></span> <span data-ttu-id="bb3ac-144">Naleznete hello schématu odkazů na hello dolní části této stránky toounderstand speciální kroky, které se mohou vztahovat toocertain typy prostředků.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-144">See hello schema links at hello bottom of this page toounderstand special steps that may apply toocertain resource types.</span></span>
>
>

### <a name="enable-collection-of-resource-diagnostic-logs-in-hello-portal"></a><span data-ttu-id="bb3ac-145">Povolit shromažďování diagnostických protokolů prostředků hello portálu</span><span class="sxs-lookup"><span data-stu-id="bb3ac-145">Enable collection of resource diagnostic logs in hello portal</span></span>
<span data-ttu-id="bb3ac-146">Můžete povolit shromažďování diagnostických protokolů prostředků v hello Azure portal po vytvoření prostředku budete tooa konkrétní prostředek nebo přechodem tooAzure monitorování.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-146">You can enable collection of resource diagnostic logs in hello Azure portal after a resource has been created either by going tooa specific resource or by navigating tooAzure Monitor.</span></span> <span data-ttu-id="bb3ac-147">tooenable prostřednictvím Azure monitorování:</span><span class="sxs-lookup"><span data-stu-id="bb3ac-147">tooenable this via Azure Monitor:</span></span>

1. <span data-ttu-id="bb3ac-148">V hello [portál Azure](http://portal.azure.com), přejděte tooAzure monitorování a klikněte na **nastavení diagnostiky**</span><span class="sxs-lookup"><span data-stu-id="bb3ac-148">In hello [Azure portal](http://portal.azure.com), navigate tooAzure Monitor and click on **Diagnostic Settings**</span></span>

    ![Monitorování části monitorování Azure](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

2. <span data-ttu-id="bb3ac-150">Volitelně můžete filtrovat seznam hello skupinu prostředků nebo typ prostředku, a potom klikněte na hello prostředků, pro kterou chcete tooset nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-150">Optionally filter hello list by resource group or resource type, then click on hello resource for which you would like tooset a diagnostic setting.</span></span>

3. <span data-ttu-id="bb3ac-151">Pokud neexistuje žádné nastavení na hello prostředku, který jste zvolili, jste výzvami toocreate nastavení.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-151">If no settings exist on hello resource you have selected, you are prompted toocreate a setting.</span></span> <span data-ttu-id="bb3ac-152">Klikněte na tlačítko "Zapnout diagnostiky."</span><span class="sxs-lookup"><span data-stu-id="bb3ac-152">Click "Turn on diagnostics."</span></span>

   ![Přidat nastavení diagnostiky - žádná existující nastavení](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-none.png)

   <span data-ttu-id="bb3ac-154">Pokud máte stávající nastavení na hello prostředku, zobrazí se seznam nastavení, které jsou již nakonfigurována na tomto prostředku.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-154">If there are existing settings on hello resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="bb3ac-155">Klikněte na tlačítko "Přidat nastavení diagnostiky."</span><span class="sxs-lookup"><span data-stu-id="bb3ac-155">Click "Add diagnostic setting."</span></span>

   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="bb3ac-157">Zadejte název nastavení, zaškrtněte políčka hello pro každé cílové toowhich chcete vytvořit toosend dat a konfigurace, který prostředek se používá pro jednotlivé cíle.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-157">Give your setting a name, check hello boxes for each destination toowhich you would like toosend data, and configure which resource is used for each destination.</span></span> <span data-ttu-id="bb3ac-158">Volitelně můžete nastavit počet dní tooretain tyto protokoly s použitím hello **uchovávání dat (dny)** posuvníky (pouze použít toohello úložiště účet cíl).</span><span class="sxs-lookup"><span data-stu-id="bb3ac-158">Optionally, set a number of days tooretain these logs by using hello **Retention (days)** sliders (only applicable toohello storage account destination).</span></span> <span data-ttu-id="bb3ac-159">Uchování nulový počet dnů po neomezenou dobu ukládá protokoly hello.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-159">A retention of zero days stores hello logs indefinitely.</span></span>
   
   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-configure.png)
    
4. <span data-ttu-id="bb3ac-161">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-161">Click **Save**.</span></span>

<span data-ttu-id="bb3ac-162">Po chvíli se nové nastavení se zobrazí v seznamu nastavení pro tento prostředek a diagnostické protokoly jsou odesílány toohello hello zadat cíle také se vygeneruje nová data události.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-162">After a few moments, hello new setting appears in your list of settings for this resource, and diagnostic logs are sent toohello specified destinations as soon as new event data is generated.</span></span>

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a><span data-ttu-id="bb3ac-163">Povolit shromažďování diagnostických protokolů prostředků prostřednictvím PowerShell</span><span class="sxs-lookup"><span data-stu-id="bb3ac-163">Enable collection of resource diagnostic logs via PowerShell</span></span>
<span data-ttu-id="bb3ac-164">tooenable shromažďování diagnostických protokolů prostředků pomocí Azure PowerShell, hello použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="bb3ac-164">tooenable collection of resource diagnostic logs via Azure PowerShell, use hello following commands:</span></span>

<span data-ttu-id="bb3ac-165">úložiště tooenable diagnostických protokolů v účtu úložiště, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="bb3ac-165">tooenable storage of diagnostic logs in a storage account, use this command:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

<span data-ttu-id="bb3ac-166">Hello úložiště, je ID účtu hello ID prostředku pro protokoly toowhich účet úložiště hello chcete toosend hello.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-166">hello storage account ID is hello resource ID for hello storage account toowhich you want toosend hello logs.</span></span>

<span data-ttu-id="bb3ac-167">tooenable vysílání datového proudu centra událostí tooan diagnostických protokolů, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="bb3ac-167">tooenable streaming of diagnostic logs tooan event hub, use this command:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

<span data-ttu-id="bb3ac-168">ID pravidla sběrnice služby Hello je řetězec s Tento formát: `{Service Bus resource ID}/authorizationrules/{key name}`.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-168">hello service bus rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`.</span></span>

<span data-ttu-id="bb3ac-169">tooenable odesílání diagnostických protokolů pracovní prostor analýzy protokolů tooa, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="bb3ac-169">tooenable sending of diagnostic logs tooa Log Analytics workspace, use this command:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of hello log analytics workspace] -Enabled $true
```

<span data-ttu-id="bb3ac-170">Můžete získat ID prostředku hello pracovní prostor analýzy protokolů pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bb3ac-170">You can obtain hello resource ID of your Log Analytics workspace using hello following command:</span></span>

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

<span data-ttu-id="bb3ac-171">Tyto parametry tooenable můžete kombinovat více možností výstup.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-171">You can combine these parameters tooenable multiple output options.</span></span>

### <a name="enable-collection-of-resource-diagnostic-logs-via-cli"></a><span data-ttu-id="bb3ac-172">Povolit shromažďování diagnostických protokolů prostředků prostřednictvím rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="bb3ac-172">Enable collection of resource diagnostic logs via CLI</span></span>
<span data-ttu-id="bb3ac-173">tooenable shromažďování diagnostických protokolů prostředků prostřednictvím hello rozhraní příkazového řádku Azure, použijte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="bb3ac-173">tooenable collection of resource diagnostic logs via hello Azure CLI, use hello following commands:</span></span>

<span data-ttu-id="bb3ac-174">úložiště tooenable diagnostických protokolů v účtu úložiště, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="bb3ac-174">tooenable storage of diagnostic logs in a Storage Account, use this command:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
```

<span data-ttu-id="bb3ac-175">Hello úložiště, je ID účtu hello ID prostředku pro protokoly toowhich účet úložiště hello chcete toosend hello.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-175">hello storage account ID is hello resource ID for hello storage account toowhich you want toosend hello logs.</span></span>

<span data-ttu-id="bb3ac-176">tooenable vysílání datového proudu centra událostí tooan diagnostických protokolů, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="bb3ac-176">tooenable streaming of diagnostic logs tooan event hub, use this command:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

<span data-ttu-id="bb3ac-177">ID pravidla sběrnice služby Hello je řetězec s Tento formát: `{Service Bus resource ID}/authorizationrules/{key name}`.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-177">hello service bus rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`.</span></span>

<span data-ttu-id="bb3ac-178">tooenable odesílání diagnostických protokolů pracovní prostor analýzy protokolů tooa, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="bb3ac-178">tooenable sending of diagnostic logs tooa Log Analytics workspace, use this command:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of hello log analytics workspace> --enabled true
```

<span data-ttu-id="bb3ac-179">Tyto parametry tooenable můžete kombinovat více možností výstup.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-179">You can combine these parameters tooenable multiple output options.</span></span>

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a><span data-ttu-id="bb3ac-180">Povolit shromažďování diagnostických protokolů prostředků prostřednictvím REST API</span><span class="sxs-lookup"><span data-stu-id="bb3ac-180">Enable collection of resource diagnostic logs via REST API</span></span>
<span data-ttu-id="bb3ac-181">nastavení pro diagnostiku toochange pomocí hello REST API služby Azure monitorování, najdete v části [tento dokument](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb3ac-181">toochange diagnostic settings using hello Azure Monitor REST API, see [this document](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span></span>

## <a name="manage-resource-diagnostic-settings-in-hello-portal"></a><span data-ttu-id="bb3ac-182">Spravovat nastavení pro diagnostiku prostředků hello portálu</span><span class="sxs-lookup"><span data-stu-id="bb3ac-182">Manage resource diagnostic settings in hello portal</span></span>
<span data-ttu-id="bb3ac-183">Ujistěte se, že všechny vaše prostředky nastavení je nastavení pro diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-183">Ensure that all of your resources are set up with diagnostic settings.</span></span> <span data-ttu-id="bb3ac-184">Přejděte příliš**monitorování** v hello portálu a otevřete **nastavení pro diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-184">Navigate too**Monitor** in hello portal and open **Diagnostic settings**.</span></span>

![Diagnostické protokoly okna portálu hello](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

<span data-ttu-id="bb3ac-186">Můžete mít tooclick "Další služby" toofind hello monitorování části.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-186">You may have tooclick "More services" toofind hello Monitor section.</span></span>

<span data-ttu-id="bb3ac-187">Zde si můžete zobrazit a filtrovat všechny prostředky, které podporují toosee nastavení diagnostiky, pokud mají povoleno diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-187">Here you can view and filter all resources that support diagnostic settings toosee if they have diagnostics enabled.</span></span> <span data-ttu-id="bb3ac-188">Můžete také podrobně toosee, pokud několik nastavení jsou nastavené na prostředku zkontrolujte, které účet úložiště, obor názvů služby Event Hubs a pracovní prostor analýzy protokolů, které jsou k toku dat</span><span class="sxs-lookup"><span data-stu-id="bb3ac-188">You can also drill down toosee if multiple settings are set on a resource and check which storage account, Event Hubs namespace, and/or Log Analytics workspace that data are flowing to.</span></span>

![Diagnostické protokoly výsledky v portálu](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

<span data-ttu-id="bb3ac-190">Přidání že nastavení diagnostiky se vyvolá hello nastavení pro diagnostiku zobrazení, kde můžete povolit, zakázat nebo změnit nastavení diagnostiky pro hello vybrali zdroj.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-190">Adding a diagnostic setting brings up hello Diagnostic Settings view, where you can enable, disable, or modify your diagnostic settings for hello selected resource.</span></span>

## <a name="supported-services-categories-and-schemas-for-resource-diagnostic-logs"></a><span data-ttu-id="bb3ac-191">Podporované služby, kategorie a schémat pro diagnostické protokoly prostředků</span><span class="sxs-lookup"><span data-stu-id="bb3ac-191">Supported services, categories, and schemas for resource diagnostic logs</span></span>
<span data-ttu-id="bb3ac-192">[Najdete v článku](monitoring-diagnostic-logs-schema.md) úplný seznam podporovaných služeb a hello protokolu kategorií a schémata v těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="bb3ac-192">[See this article](monitoring-diagnostic-logs-schema.md) for a complete list of supported services and hello log categories and schemas used by those services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb3ac-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bb3ac-193">Next steps</span></span>

* [<span data-ttu-id="bb3ac-194">Stream diagnostické protokoly prostředků příliš**Event Hubs**</span><span class="sxs-lookup"><span data-stu-id="bb3ac-194">Stream resource diagnostic logs too**Event Hubs**</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [<span data-ttu-id="bb3ac-195">Změňte nastavení pro diagnostiku prostředků pomocí hello REST API služby Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="bb3ac-195">Change resource diagnostic settings using hello Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [<span data-ttu-id="bb3ac-196">Analýza protokolů z úložiště Azure s analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="bb3ac-196">Analyze logs from Azure storage with Log Analytics</span></span>](../log-analytics/log-analytics-azure-storage.md)
