---
title: "Přehled Azure diagnostické protokoly | Microsoft Docs"
description: "Zjistěte, co je Azure diagnostické protokoly a jak je můžete využít pochopit události, ke kterým došlo na prostředek služby Azure."
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
ms.openlocfilehash: d59abde29fc7b73a799e5bf3659b02f824b693de
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a><span data-ttu-id="d6148-103">Shromažďování a využití dat protokolu z vašich prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="d6148-103">Collect and consume log data from your Azure resources</span></span>

## <a name="what-are-azure-resource-diagnostic-logs"></a><span data-ttu-id="d6148-104">Jaké jsou diagnostické protokoly prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="d6148-104">What are Azure resource diagnostic logs</span></span>
<span data-ttu-id="d6148-105">**Diagnostické protokoly Azure úrovni prostředků** vygenerované prostředek protokoly, které poskytují bohatou a často data o operaci prostředku.</span><span class="sxs-lookup"><span data-stu-id="d6148-105">**Azure resource-level diagnostic logs** are logs emitted by a resource that provide rich, frequent data about the operation of that resource.</span></span> <span data-ttu-id="d6148-106">Obsah tyto protokoly se liší podle typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="d6148-106">The content of these logs varies by resource type.</span></span> <span data-ttu-id="d6148-107">Například pravidlo čítače skupinu zabezpečení sítě a audity Key Vault jsou dvě kategorie protokoly prostředku.</span><span class="sxs-lookup"><span data-stu-id="d6148-107">For example, Network Security Group rule counters and Key Vault audits are two categories of resource logs.</span></span>

<span data-ttu-id="d6148-108">Diagnostické protokoly na úrovni prostředků se liší od [protokol aktivit](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="d6148-108">Resource-level diagnostic logs differ from the [Activity Log](monitoring-overview-activity-logs.md).</span></span> <span data-ttu-id="d6148-109">Protokol aktivit poskytuje náhled do činnosti, které byly provedeny v prostředky ve vašem předplatném pomocí Resource Manager, například vytváření virtuálního počítače nebo odstranění aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="d6148-109">The Activity Log provides insight into the operations that were performed on resources in your subscription using Resource Manager, for example, creating a virtual machine or deleting a logic app.</span></span> <span data-ttu-id="d6148-110">Protokol aktivit je protokolu úrovni předplatného.</span><span class="sxs-lookup"><span data-stu-id="d6148-110">The Activity Log is a subscription-level log.</span></span> <span data-ttu-id="d6148-111">Diagnostické protokoly na úrovni prostředků získat přehled o operace, které byly provedeny v rámci prostředku samostatně, například získání tajného klíče z Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d6148-111">Resource-level diagnostic logs provide insight into operations that were performed within that resource itself, for example, getting a secret from a Key Vault.</span></span>

<span data-ttu-id="d6148-112">Diagnostické protokoly na úrovni prostředků se také liší od hostovaného operačního systému na úrovni diagnostických protokolů.</span><span class="sxs-lookup"><span data-stu-id="d6148-112">Resource-level diagnostic logs also differ from guest OS-level diagnostic logs.</span></span> <span data-ttu-id="d6148-113">Diagnostické protokoly hostovaného operačního systému jsou tyto shromažďují agenta spuštěného v rámci virtuálního počítače nebo jiné podporované typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="d6148-113">Guest OS diagnostic logs are those collected by an agent running inside of a virtual machine or other supported resource type.</span></span> <span data-ttu-id="d6148-114">Diagnostické protokoly na úrovni prostředků vyžadují specifické pro zdroje dat z Azure k samotné platformě nástroje, žádné agenta a zachycení, zatímco hostovaného operačního systému na úrovni diagnostických protokolů zaznamenání dat z operačního systému a aplikací, které běží na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="d6148-114">Resource-level diagnostic logs require no agent and capture resource-specific data from the Azure platform itself, while guest OS-level diagnostic logs capture data from the operating system and applications running on a virtual machine.</span></span>

<span data-ttu-id="d6148-115">Ne všechny prostředky podporu nového typu prostředku diagnostické protokoly zde popsané.</span><span class="sxs-lookup"><span data-stu-id="d6148-115">Not all resources support the new type of resource diagnostic logs described here.</span></span> <span data-ttu-id="d6148-116">Tento článek obsahuje oddíl výpis, které typy prostředků podporují nové diagnostické protokoly úrovni prostředků.</span><span class="sxs-lookup"><span data-stu-id="d6148-116">This article contains a section listing which resource types support the new resource-level diagnostic logs.</span></span>

![<span data-ttu-id="d6148-117">Diagnostika prostředků protokoly vs jiné typy protokolů</span><span class="sxs-lookup"><span data-stu-id="d6148-117">Resource diagnostics logs vs other types of logs</span></span> ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

## <a name="what-you-can-do-with-resource-level-diagnostic-logs"></a><span data-ttu-id="d6148-118">Co můžete dělat s diagnostické protokoly na úrovni prostředků</span><span class="sxs-lookup"><span data-stu-id="d6148-118">What you can do with resource-level diagnostic logs</span></span>
<span data-ttu-id="d6148-119">Tady jsou některé z akcí, které můžete provést prostředků diagnostických protokolů:</span><span class="sxs-lookup"><span data-stu-id="d6148-119">Here are some of the things you can do with resource diagnostic logs:</span></span>

![Logické umístění prostředků diagnostických protokolů](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)


* <span data-ttu-id="d6148-121">Uložte je do [ **účet úložiště** ](monitoring-archive-diagnostic-logs.md) pro auditování nebo ruční kontroly.</span><span class="sxs-lookup"><span data-stu-id="d6148-121">Save them to a [**Storage Account**](monitoring-archive-diagnostic-logs.md) for auditing or manual inspection.</span></span> <span data-ttu-id="d6148-122">Můžete zadat uchování (ve dnech) pomocí **nastavení diagnostiky pro prostředek**.</span><span class="sxs-lookup"><span data-stu-id="d6148-122">You can specify the retention time (in days) using **resource diagnostic settings**.</span></span>
* <span data-ttu-id="d6148-123">[Stream, aby **Event Hubs** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) pro přijímání službu třetí strany nebo řešení vlastní analýzy, jako je například PowerBI.</span><span class="sxs-lookup"><span data-stu-id="d6148-123">[Stream them to **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md) for ingestion by a third-party service or custom analytics solution such as PowerBI.</span></span>
* <span data-ttu-id="d6148-124">Analyzovat, s [OMS analýzy protokolů](../log-analytics/log-analytics-azure-storage.md)</span><span class="sxs-lookup"><span data-stu-id="d6148-124">Analyze them with [OMS Log Analytics](../log-analytics/log-analytics-azure-storage.md)</span></span>

<span data-ttu-id="d6148-125">Můžete vytvořit účet úložiště nebo obor názvů služby Event Hubs, který není ve stejném předplatném jako emitování protokoly.</span><span class="sxs-lookup"><span data-stu-id="d6148-125">You can use a storage account or Event Hubs namespace that is not in the same subscription as the one emitting logs.</span></span> <span data-ttu-id="d6148-126">Uživatel, který konfiguruje nastavení, musí mít odpovídající RBAC přístup na oba odběry.</span><span class="sxs-lookup"><span data-stu-id="d6148-126">The user who configures the setting must have the appropriate RBAC access to both subscriptions.</span></span>

## <a name="resource-diagnostic-settings"></a><span data-ttu-id="d6148-127">Nastavení diagnostiky pro prostředek</span><span class="sxs-lookup"><span data-stu-id="d6148-127">Resource diagnostic settings</span></span>
<span data-ttu-id="d6148-128">Diagnostické protokoly prostředků pro jiné výpočetní, že prostředky jsou nakonfigurovány pomocí nastavení diagnostiky pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="d6148-128">Resource diagnostic logs for non-Compute resources are configured using resource diagnostic settings.</span></span> <span data-ttu-id="d6148-129">**Nastavení diagnostiky pro prostředek** pro ovládací prvek prostředku:</span><span class="sxs-lookup"><span data-stu-id="d6148-129">**Resource diagnostic settings** for a resource control:</span></span>

* <span data-ttu-id="d6148-130">Kde prostředků diagnostické protokoly a metriky se odesílají (účet úložiště, Event Hubs nebo analýzy protokolů OMS).</span><span class="sxs-lookup"><span data-stu-id="d6148-130">Where resource diagnostic logs and metrics are sent (Storage Account, Event Hubs, and/or OMS Log Analytics).</span></span>
* <span data-ttu-id="d6148-131">Kategorie protokolu, které se odesílají a jestli metriky data jsou taktéž odeslána.</span><span class="sxs-lookup"><span data-stu-id="d6148-131">Which log categories are sent and whether metric data is also sent.</span></span>
* <span data-ttu-id="d6148-132">Jak dlouho každou kategorii protokolu se uchovávají v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="d6148-132">How long each log category should be retained in a storage account</span></span>
    - <span data-ttu-id="d6148-133">Uchování nulový počet dnů znamená, že jsou protokoly v nekonečné smyčce.</span><span class="sxs-lookup"><span data-stu-id="d6148-133">A retention of zero days means logs are kept forever.</span></span> <span data-ttu-id="d6148-134">Hodnota, jinak hodnota může být libovolný počet dnů od 1 do 2147483647.</span><span class="sxs-lookup"><span data-stu-id="d6148-134">Otherwise, the value can be any number of days between 1 and 2147483647.</span></span>
    - <span data-ttu-id="d6148-135">Pokud nejsou nastavené zásady uchovávání informací, ale ukládání protokolů v účtu úložiště je zakázaný (například pokud jenom jsou vybrané možnosti služby Event Hubs nebo OMS), zásady uchovávání informací nemají žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="d6148-135">If retention policies are set but storing logs in a Storage Account is disabled (for example, if only Event Hubs or OMS options are selected), the retention policies have no effect.</span></span>
    - <span data-ttu-id="d6148-136">Zásady uchovávání informací jsou použité denní, takže na konci za den (UTC), protokoly dnem, který je teď nad rámec uchovávání se zásada odstraní.</span><span class="sxs-lookup"><span data-stu-id="d6148-136">Retention policies are applied per-day, so at the end of a day (UTC), logs from the day that is now beyond the retention policy are deleted.</span></span> <span data-ttu-id="d6148-137">Například pokud jste měli zásady uchovávání informací jeden den, od začátku dnešní den protokoly z včerejšek před den by odstraněn.</span><span class="sxs-lookup"><span data-stu-id="d6148-137">For example, if you had a retention policy of one day, at the beginning of the day today the logs from the day before yesterday would be deleted.</span></span>

<span data-ttu-id="d6148-138">Tato nastavení jsou konfigurována snadno prostřednictvím nastavení diagnostiky pro prostředek v portálu Azure, pomocí prostředí Azure PowerShell a rozhraní příkazového řádku nebo pomocí [REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="d6148-138">These settings are easily configured via the diagnostic settings for a resource in the Azure portal, via Azure PowerShell and CLI commands, or via the [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>

> [!WARNING]
> <span data-ttu-id="d6148-139">Diagnostické protokoly a metriky pro z hostovaného operačního systému vrstvy výpočetní prostředky (například virtuální počítače nebo Service Fabric) použití [samostatné mechanismus konfigurace a výběr výstupy](../azure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="d6148-139">Diagnostic logs and metrics for from the guest OS layer of Compute resources (for example, VMs or Service Fabric) use [a separate mechanism for configuration and selection of outputs](../azure-diagnostics.md).</span></span>
>
>

## <a name="how-to-enable-collection-of-resource-diagnostic-logs"></a><span data-ttu-id="d6148-140">Postup povolení shromažďování diagnostických protokolů prostředků</span><span class="sxs-lookup"><span data-stu-id="d6148-140">How to enable collection of resource diagnostic logs</span></span>
<span data-ttu-id="d6148-141">Povolením shromažďování diagnostických protokolů prostředků [jako součást vytvoření prostředku v šabloně Resource Manager](./monitoring-enable-diagnostic-logs-using-template.md) nebo po vytvoření prostředku ze stránky tohoto prostředku na portálu.</span><span class="sxs-lookup"><span data-stu-id="d6148-141">Collection of resource diagnostic logs can be enabled [as part of creating a resource in a Resource Manager template](./monitoring-enable-diagnostic-logs-using-template.md) or after a resource is created from that resource's page in the portal.</span></span> <span data-ttu-id="d6148-142">Můžete také povolit kolekce v libovolném bodě pomocí příkazů prostředí Azure PowerShell nebo rozhraní příkazového řádku nebo pomocí rozhraní REST API Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="d6148-142">You can also enable collection at any point using Azure PowerShell or CLI commands, or using the Azure Monitor REST API.</span></span>

> [!TIP]
> <span data-ttu-id="d6148-143">Tyto pokyny nemusí vztahovat přímo na každý prostředek.</span><span class="sxs-lookup"><span data-stu-id="d6148-143">These instructions may not apply directly to every resource.</span></span> <span data-ttu-id="d6148-144">V tématech schématu v dolní části této stránky pochopit speciální kroky, které se mohou vztahovat k určitým typům prostředků.</span><span class="sxs-lookup"><span data-stu-id="d6148-144">See the schema links at the bottom of this page to understand special steps that may apply to certain resource types.</span></span>
>
>

### <a name="enable-collection-of-resource-diagnostic-logs-in-the-portal"></a><span data-ttu-id="d6148-145">Povolit shromažďování diagnostických protokolů prostředků na portálu</span><span class="sxs-lookup"><span data-stu-id="d6148-145">Enable collection of resource diagnostic logs in the portal</span></span>
<span data-ttu-id="d6148-146">Po vytvoření prostředku přechodem na konkrétní prostředek nebo přechodem na Azure monitorování můžete povolit shromažďování diagnostických protokolů prostředků na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d6148-146">You can enable collection of resource diagnostic logs in the Azure portal after a resource has been created either by going to a specific resource or by navigating to Azure Monitor.</span></span> <span data-ttu-id="d6148-147">Uděláte to pomocí Azure monitorování:</span><span class="sxs-lookup"><span data-stu-id="d6148-147">To enable this via Azure Monitor:</span></span>

1. <span data-ttu-id="d6148-148">V [portál Azure](http://portal.azure.com), přejděte do monitorování Azure a klikněte na **nastavení diagnostiky**</span><span class="sxs-lookup"><span data-stu-id="d6148-148">In the [Azure portal](http://portal.azure.com), navigate to Azure Monitor and click on **Diagnostic Settings**</span></span>

    ![Monitorování části monitorování Azure](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

2. <span data-ttu-id="d6148-150">Volitelně můžete seznam filtrovat podle skupiny prostředků nebo typ prostředku, a potom klikněte na prostředek, pro kterou chcete provést nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="d6148-150">Optionally filter the list by resource group or resource type, then click on the resource for which you would like to set a diagnostic setting.</span></span>

3. <span data-ttu-id="d6148-151">Pokud neexistuje žádné nastavení na prostředku jste vybrali, zobrazí se výzva k vytvoření nastavení.</span><span class="sxs-lookup"><span data-stu-id="d6148-151">If no settings exist on the resource you have selected, you are prompted to create a setting.</span></span> <span data-ttu-id="d6148-152">Klikněte na tlačítko "Zapnout diagnostiky."</span><span class="sxs-lookup"><span data-stu-id="d6148-152">Click "Turn on diagnostics."</span></span>

   ![Přidat nastavení diagnostiky - žádná existující nastavení](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-none.png)

   <span data-ttu-id="d6148-154">Pokud máte stávající nastavení na prostředek, zobrazí se seznam nastavení, které jsou již nakonfigurována na tomto prostředku.</span><span class="sxs-lookup"><span data-stu-id="d6148-154">If there are existing settings on the resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="d6148-155">Klikněte na tlačítko "Přidat nastavení diagnostiky."</span><span class="sxs-lookup"><span data-stu-id="d6148-155">Click "Add diagnostic setting."</span></span>

   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="d6148-157">Zadejte název nastavení, zaškrtněte políčka pro každý cíl, do které chcete odesílat data a konfigurace, který prostředek se používá pro jednotlivé cíle.</span><span class="sxs-lookup"><span data-stu-id="d6148-157">Give your setting a name, check the boxes for each destination to which you would like to send data, and configure which resource is used for each destination.</span></span> <span data-ttu-id="d6148-158">Volitelně můžete nastavit počet dní, abyste tyto protokoly uchovávat pomocí **uchovávání dat (dny)** posuvníky (platí jenom pro cílový účet úložiště).</span><span class="sxs-lookup"><span data-stu-id="d6148-158">Optionally, set a number of days to retain these logs by using the **Retention (days)** sliders (only applicable to the storage account destination).</span></span> <span data-ttu-id="d6148-159">Uchování nulový počet dnů po neomezenou dobu ukládá protokoly.</span><span class="sxs-lookup"><span data-stu-id="d6148-159">A retention of zero days stores the logs indefinitely.</span></span>
   
   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-configure.png)
    
4. <span data-ttu-id="d6148-161">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d6148-161">Click **Save**.</span></span>

<span data-ttu-id="d6148-162">Po chvíli se nové nastavení se zobrazí v seznamu nastavení pro tento prostředek a diagnostické protokoly jsou odeslány do zadaného umístění také se vygeneruje nová data události.</span><span class="sxs-lookup"><span data-stu-id="d6148-162">After a few moments, the new setting appears in your list of settings for this resource, and diagnostic logs are sent to the specified destinations as soon as new event data is generated.</span></span>

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a><span data-ttu-id="d6148-163">Povolit shromažďování diagnostických protokolů prostředků prostřednictvím PowerShell</span><span class="sxs-lookup"><span data-stu-id="d6148-163">Enable collection of resource diagnostic logs via PowerShell</span></span>
<span data-ttu-id="d6148-164">Chcete-li povolit shromažďování diagnostických protokolů prostředků pomocí Azure PowerShell, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d6148-164">To enable collection of resource diagnostic logs via Azure PowerShell, use the following commands:</span></span>

<span data-ttu-id="d6148-165">Pokud chcete povolit ukládání diagnostických protokolů v účtu úložiště, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="d6148-165">To enable storage of diagnostic logs in a storage account, use this command:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

<span data-ttu-id="d6148-166">ID účtu úložiště je ID prostředku pro účet úložiště, do kterého chcete odeslat protokoly.</span><span class="sxs-lookup"><span data-stu-id="d6148-166">The storage account ID is the resource ID for the storage account to which you want to send the logs.</span></span>

<span data-ttu-id="d6148-167">Pokud chcete povolit vysílání datového proudu diagnostických protokolů do centra událostí, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="d6148-167">To enable streaming of diagnostic logs to an event hub, use this command:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

<span data-ttu-id="d6148-168">ID pravidla service bus je řetězec s Tento formát: `{Service Bus resource ID}/authorizationrules/{key name}`.</span><span class="sxs-lookup"><span data-stu-id="d6148-168">The service bus rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`.</span></span>

<span data-ttu-id="d6148-169">Pokud chcete povolit odesílání diagnostických protokolů do pracovního prostoru analýzy protokolů, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="d6148-169">To enable sending of diagnostic logs to a Log Analytics workspace, use this command:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
```

<span data-ttu-id="d6148-170">Můžete získat ID prostředku pracovní prostor analýzy protokolů pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d6148-170">You can obtain the resource ID of your Log Analytics workspace using the following command:</span></span>

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

<span data-ttu-id="d6148-171">Tyto parametry povolení více možností výstupu můžete kombinovat.</span><span class="sxs-lookup"><span data-stu-id="d6148-171">You can combine these parameters to enable multiple output options.</span></span>

### <a name="enable-collection-of-resource-diagnostic-logs-via-cli"></a><span data-ttu-id="d6148-172">Povolit shromažďování diagnostických protokolů prostředků prostřednictvím rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="d6148-172">Enable collection of resource diagnostic logs via CLI</span></span>
<span data-ttu-id="d6148-173">Chcete-li povolit shromažďování diagnostických protokolů prostředků prostřednictvím rozhraní příkazového řádku Azure, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d6148-173">To enable collection of resource diagnostic logs via the Azure CLI, use the following commands:</span></span>

<span data-ttu-id="d6148-174">Pokud chcete povolit ukládání diagnostických protokolů v účtu úložiště, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="d6148-174">To enable storage of diagnostic logs in a Storage Account, use this command:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
```

<span data-ttu-id="d6148-175">ID účtu úložiště je ID prostředku pro účet úložiště, do kterého chcete odeslat protokoly.</span><span class="sxs-lookup"><span data-stu-id="d6148-175">The storage account ID is the resource ID for the storage account to which you want to send the logs.</span></span>

<span data-ttu-id="d6148-176">Pokud chcete povolit vysílání datového proudu diagnostických protokolů do centra událostí, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="d6148-176">To enable streaming of diagnostic logs to an event hub, use this command:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

<span data-ttu-id="d6148-177">ID pravidla service bus je řetězec s Tento formát: `{Service Bus resource ID}/authorizationrules/{key name}`.</span><span class="sxs-lookup"><span data-stu-id="d6148-177">The service bus rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`.</span></span>

<span data-ttu-id="d6148-178">Pokud chcete povolit odesílání diagnostických protokolů do pracovního prostoru analýzy protokolů, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="d6148-178">To enable sending of diagnostic logs to a Log Analytics workspace, use this command:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
```

<span data-ttu-id="d6148-179">Tyto parametry povolení více možností výstupu můžete kombinovat.</span><span class="sxs-lookup"><span data-stu-id="d6148-179">You can combine these parameters to enable multiple output options.</span></span>

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a><span data-ttu-id="d6148-180">Povolit shromažďování diagnostických protokolů prostředků prostřednictvím REST API</span><span class="sxs-lookup"><span data-stu-id="d6148-180">Enable collection of resource diagnostic logs via REST API</span></span>
<span data-ttu-id="d6148-181">Chcete-li změnit nastavení pro diagnostiku pomocí rozhraní REST API Azure monitorování, [tento dokument](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span><span class="sxs-lookup"><span data-stu-id="d6148-181">To change diagnostic settings using the Azure Monitor REST API, see [this document](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span></span>

## <a name="manage-resource-diagnostic-settings-in-the-portal"></a><span data-ttu-id="d6148-182">Spravovat nastavení pro diagnostiku prostředků na portálu</span><span class="sxs-lookup"><span data-stu-id="d6148-182">Manage resource diagnostic settings in the portal</span></span>
<span data-ttu-id="d6148-183">Ujistěte se, že všechny vaše prostředky nastavení je nastavení pro diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="d6148-183">Ensure that all of your resources are set up with diagnostic settings.</span></span> <span data-ttu-id="d6148-184">Přejděte na **monitorování** v portálu a otevřete **nastavení pro diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="d6148-184">Navigate to **Monitor** in the portal and open **Diagnostic settings**.</span></span>

![Diagnostické protokoly okno na portálu](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

<span data-ttu-id="d6148-186">Možná budete muset klikněte na tlačítko "Další služby" najít v části monitorování.</span><span class="sxs-lookup"><span data-stu-id="d6148-186">You may have to click "More services" to find the Monitor section.</span></span>

<span data-ttu-id="d6148-187">Zde si můžete zobrazit a vyfiltrovat všechny prostředky, které podporují diagnostické nastavení zobrazit, pokud mají diagnostiky povoleno.</span><span class="sxs-lookup"><span data-stu-id="d6148-187">Here you can view and filter all resources that support diagnostic settings to see if they have diagnostics enabled.</span></span> <span data-ttu-id="d6148-188">Můžete také přejít na najdete Pokud několik nastavení jsou nastavené na prostředku a zkontrolujte, které účet úložiště, obor názvů služby Event Hubs a pracovní prostor analýzy protokolů, které jsou k toku dat.</span><span class="sxs-lookup"><span data-stu-id="d6148-188">You can also drill down to see if multiple settings are set on a resource and check which storage account, Event Hubs namespace, and/or Log Analytics workspace that data are flowing to.</span></span>

![Diagnostické protokoly výsledky v portálu](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

<span data-ttu-id="d6148-190">Přidání nastavení diagnostiky se vyvolá nastavení pro diagnostiku zobrazení, kde můžete povolit, zakázat nebo změnit nastavení diagnostiky pro vybraný zdroj.</span><span class="sxs-lookup"><span data-stu-id="d6148-190">Adding a diagnostic setting brings up the Diagnostic Settings view, where you can enable, disable, or modify your diagnostic settings for the selected resource.</span></span>

## <a name="supported-services-categories-and-schemas-for-resource-diagnostic-logs"></a><span data-ttu-id="d6148-191">Podporované služby, kategorie a schémat pro diagnostické protokoly prostředků</span><span class="sxs-lookup"><span data-stu-id="d6148-191">Supported services, categories, and schemas for resource diagnostic logs</span></span>
<span data-ttu-id="d6148-192">[Najdete v článku](monitoring-diagnostic-logs-schema.md) úplný seznam podporovaných služeb a kategorií protokolu a schémata v těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="d6148-192">[See this article](monitoring-diagnostic-logs-schema.md) for a complete list of supported services and the log categories and schemas used by those services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6148-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d6148-193">Next steps</span></span>

* [<span data-ttu-id="d6148-194">Stream prostředků do diagnostickým protokolům **Event Hubs**</span><span class="sxs-lookup"><span data-stu-id="d6148-194">Stream resource diagnostic logs to **Event Hubs**</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [<span data-ttu-id="d6148-195">Změňte nastavení pro diagnostiku prostředků pomocí rozhraní REST API Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="d6148-195">Change resource diagnostic settings using the Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [<span data-ttu-id="d6148-196">Analýza protokolů z úložiště Azure s analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="d6148-196">Analyze logs from Azure storage with Log Analytics</span></span>](../log-analytics/log-analytics-azure-storage.md)
