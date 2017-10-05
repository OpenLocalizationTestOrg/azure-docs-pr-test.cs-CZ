---
title: "Stream protokol činnosti Azure do centra událostí | Microsoft Docs"
description: "Zjistěte, jak k vysílání datového proudu protokol činnosti Azure do centra událostí."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ec4c2d2c-8907-484f-a910-712403a06829
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/06/2017
ms.author: johnkem
ms.openlocfilehash: 88c5701279f370914fac68872d67b02a7571748a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a><span data-ttu-id="879f1-103">Datový proud protokolu Azure činnosti do centra událostí</span><span class="sxs-lookup"><span data-stu-id="879f1-103">Stream the Azure Activity Log to Event Hubs</span></span>
<span data-ttu-id="879f1-104">[ **Protokol činnosti Azure** ](monitoring-overview-activity-logs.md) Streamovat skoro v reálném čase pro všechny aplikace pomocí předdefinované možnosti "Export" na portálu nebo povolením Id pravidla Service Bus v profilu protokolu prostřednictvím rutin prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="879f1-104">The [**Azure Activity Log**](monitoring-overview-activity-logs.md) can be streamed in near real time to any application using the built-in “Export” option in the portal, or by enabling the Service Bus Rule Id in a Log Profile via the Azure PowerShell Cmdlets or Azure CLI.</span></span>

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a><span data-ttu-id="879f1-105">Co můžete dělat s protokol aktivit a Event Hubs</span><span class="sxs-lookup"><span data-stu-id="879f1-105">What you can do with the Activity Log and Event Hubs</span></span>
<span data-ttu-id="879f1-106">Můžete použít možnost streamování pro protokol činnosti několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="879f1-106">Here are just a few ways you might use the streaming capability for the Activity Log:</span></span>

* <span data-ttu-id="879f1-107">**Datový proud na protokolování a telemetrie systémů jiných výrobců** – v čase, streamování Event Hubs se stane mechanismus kanálem protokolu aktivit do jiných systémů Siem a řešení pro analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="879f1-107">**Stream to third-party logging and telemetry systems** – Over time, Event Hubs streaming will become the mechanism to pipe your Activity Log into third-party SIEMs and log analytics solutions.</span></span>
* <span data-ttu-id="879f1-108">**Vytvoření vlastní telemetrii a protokolování platformy** – Pokud už máte uživatelské telemetrie platformy nebo jsou jenom přemýšlíte o vytváření jeden vysoce škálovatelné, publikování a odběru na povaze služby Event Hubs umožňuje flexibilně ingestování protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="879f1-108">**Build a custom telemetry and logging platform** – If you already have a custom-built telemetry platform or are just thinking about building one, the highly scalable publish-subscribe nature of Event Hubs allows you to flexibly ingest the activity log.</span></span> [<span data-ttu-id="879f1-109">V příručce Dana Rosanova pomocí služby Event Hubs telemetrie platformy globálním měřítku sem.</span><span class="sxs-lookup"><span data-stu-id="879f1-109">See Dan Rosanova’s guide to using Event Hubs in a global scale telemetry platform here.</span></span>](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a><span data-ttu-id="879f1-110">Povolit vysílání datového proudu protokolu aktivit</span><span class="sxs-lookup"><span data-stu-id="879f1-110">Enable streaming of the Activity Log</span></span>
<span data-ttu-id="879f1-111">Můžete povolit vysílání datového proudu protokolu aktivit buď programově, nebo prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="879f1-111">You can enable streaming of the Activity Log either programmatically or via the portal.</span></span> <span data-ttu-id="879f1-112">V obou případech můžete vybrat Namespace sběrnice služby a zásady sdíleného přístupu pro tento obor názvů a centra událostí je v daném oboru názvů vytvořena, když dojde k první nové aktivity protokolu události.</span><span class="sxs-lookup"><span data-stu-id="879f1-112">Either way, you pick a Service Bus Namespace and a shared access policy for that namespace, and an Event Hub is created in that namespace when the first new Activity Log event occurs.</span></span> <span data-ttu-id="879f1-113">Pokud nemáte Namespace Service Bus, musíte nejprve vytvořit.</span><span class="sxs-lookup"><span data-stu-id="879f1-113">If you do not have a Service Bus Namespace, you first need to create one.</span></span> <span data-ttu-id="879f1-114">Pokud jste dříve streamování aktivity protokolu události tento Namespace Service Bus, bude znovu použita centra událostí, která byla dříve vytvořena.</span><span class="sxs-lookup"><span data-stu-id="879f1-114">If you have previously streamed Activity Log events to this Service Bus Namespace, the Event Hub that was previously created will be reused.</span></span> <span data-ttu-id="879f1-115">Zásada sdíleného přístupu definuje oprávnění, která má streamování mechanismus.</span><span class="sxs-lookup"><span data-stu-id="879f1-115">The shared access policy defines the permissions that the streaming mechanism has.</span></span> <span data-ttu-id="879f1-116">V současné době vyžadují streamování Event Hubs **spravovat**, **odeslat**, a **naslouchání** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="879f1-116">Today, streaming to an Event Hubs requires **Manage**, **Send**, and **Listen** permissions.</span></span> <span data-ttu-id="879f1-117">Můžete vytvořit nebo upravit pro vaše Namespace sběrnice služby Service Bus Namespace sdílené zásady přístupu na portálu classic na kartě "Konfigurace".</span><span class="sxs-lookup"><span data-stu-id="879f1-117">You can create or modify Service Bus Namespace shared access policies in the classic portal under the “Configure” tab for your Service Bus Namespace.</span></span> <span data-ttu-id="879f1-118">Aktualizovat profil protokolu protokol aktivit zahrnout streamování, uživatele, provedení změny, musí mít oprávnění ListKey na tomto Service Bus autorizační pravidlo.</span><span class="sxs-lookup"><span data-stu-id="879f1-118">To update the Activity Log log profile to include streaming, the user making the change must have the ListKey permission on that Service Bus Authorization Rule.</span></span>

<span data-ttu-id="879f1-119">Oboru názvů služby bus nebo event hub nemusí být ve stejném předplatném jako předplatné emitování protokoly tak dlouho, dokud uživatel, který konfiguruje nastavení, má odpovídající přístup RBAC do oba odběry.</span><span class="sxs-lookup"><span data-stu-id="879f1-119">The service bus or event hub namespace does not have to be in the same subscription as the subscription emitting logs as long as the user who configures the setting has appropriate RBAC access to both subscriptions.</span></span>

### <a name="via-azure-portal"></a><span data-ttu-id="879f1-120">Prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="879f1-120">Via Azure portal</span></span>
1. <span data-ttu-id="879f1-121">Přejděte na **protokol aktivit** okno pomocí nabídky na levé straně na portálu.</span><span class="sxs-lookup"><span data-stu-id="879f1-121">Navigate to the **Activity Log** blade using the menu on the left side of the portal.</span></span>
   
    ![Přejděte na protokol aktivit v portálu](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. <span data-ttu-id="879f1-123">Klikněte **exportovat** tlačítka v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="879f1-123">Click the **Export** button at the top of the blade.</span></span>
   
    ![Tlačítko Exportovat portálu](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. <span data-ttu-id="879f1-125">V okně, který se zobrazí můžete vybrat oblasti, pro které chcete datového proudu událostí a Namespace Service Bus, ve kterém chcete vytvořit pro streamování tyto události centra událostí.</span><span class="sxs-lookup"><span data-stu-id="879f1-125">In the blade that appears, you can select the regions for which you would like to stream events and the Service Bus Namespace in which you would like an Event Hub to be created for streaming these events.</span></span>
   
    ![Export protokolu aktivit okno](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. <span data-ttu-id="879f1-127">Klikněte na tlačítko **Uložit** uložit tato nastavení.</span><span class="sxs-lookup"><span data-stu-id="879f1-127">Click **Save** to save these settings.</span></span> <span data-ttu-id="879f1-128">Nastavení se použije okamžitě do vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="879f1-128">The settings are immediately be applied to your subscription.</span></span>

### <a name="via-powershell-cmdlets"></a><span data-ttu-id="879f1-129">Pomocí rutin prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="879f1-129">Via PowerShell Cmdlets</span></span>
<span data-ttu-id="879f1-130">Pokud profil protokolu již existuje, musíte nejprve odebrat tento profil.</span><span class="sxs-lookup"><span data-stu-id="879f1-130">If a log profile already exists, you first need to remove that profile.</span></span>

1. <span data-ttu-id="879f1-131">Použití `Get-AzureRmLogProfile` a zjistit, zda existuje profil protokolu</span><span class="sxs-lookup"><span data-stu-id="879f1-131">Use `Get-AzureRmLogProfile` to identify if a log profile exists</span></span>
2. <span data-ttu-id="879f1-132">Pokud ano, použít `Remove-AzureRmLogProfile` jeho odebrání.</span><span class="sxs-lookup"><span data-stu-id="879f1-132">If so, use `Remove-AzureRmLogProfile` to remove it.</span></span>
3. <span data-ttu-id="879f1-133">Použití `Set-AzureRmLogProfile` k vytvoření profilu:</span><span class="sxs-lookup"><span data-stu-id="879f1-133">Use `Set-AzureRmLogProfile` to create a profile:</span></span>

```
Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

<span data-ttu-id="879f1-134">ID pravidla Service Bus je řetězec s Tento formát: {service bus ID prostředku} /authorizationrules/ {název klíče}, např.</span><span class="sxs-lookup"><span data-stu-id="879f1-134">The Service Bus Rule ID is a string with this format: {service bus resource ID}/authorizationrules/{key name}, for example</span></span> 

### <a name="via-azure-cli"></a><span data-ttu-id="879f1-135">Prostřednictvím rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="879f1-135">Via Azure CLI</span></span>
<span data-ttu-id="879f1-136">Pokud profil protokolu již existuje, musíte nejprve odebrat tento profil.</span><span class="sxs-lookup"><span data-stu-id="879f1-136">If a log profile already exists, you first need to remove that profile.</span></span>

1. <span data-ttu-id="879f1-137">Použití `azure insights logprofile list` a zjistit, zda existuje profil protokolu</span><span class="sxs-lookup"><span data-stu-id="879f1-137">Use `azure insights logprofile list` to identify if a log profile exists</span></span>
2. <span data-ttu-id="879f1-138">Pokud ano, použít `azure insights logprofile delete` jeho odebrání.</span><span class="sxs-lookup"><span data-stu-id="879f1-138">If so, use `azure insights logprofile delete` to remove it.</span></span>
3. <span data-ttu-id="879f1-139">Použití `azure insights logprofile add` k vytvoření profilu:</span><span class="sxs-lookup"><span data-stu-id="879f1-139">Use `azure insights logprofile add` to create a profile:</span></span>

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

<span data-ttu-id="879f1-140">ID pravidla Service Bus je řetězec s Tento formát: `{service bus resource ID}/authorizationrules/{key name}`.</span><span class="sxs-lookup"><span data-stu-id="879f1-140">The Service Bus Rule ID is a string with this format: `{service bus resource ID}/authorizationrules/{key name}`.</span></span>

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a><span data-ttu-id="879f1-141">Způsob, jakým využívají data protokolu ze služby Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="879f1-141">How do I consume the log data from Event Hubs?</span></span>
<span data-ttu-id="879f1-142">[Zde jsou k dispozici schéma pro protokol aktivit](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="879f1-142">[The schema for the Activity Log is available here](monitoring-overview-activity-logs.md).</span></span> <span data-ttu-id="879f1-143">Každá událost je v pole objektů JSON BLOB názvem "záznamů".</span><span class="sxs-lookup"><span data-stu-id="879f1-143">Each event is in an array of JSON blobs called “records.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="879f1-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="879f1-144">Next Steps</span></span>
* [<span data-ttu-id="879f1-145">Archiv protokol aktivit na účet úložiště</span><span class="sxs-lookup"><span data-stu-id="879f1-145">Archive the Activity Log to a storage account</span></span>](monitoring-archive-activity-log.md)
* [<span data-ttu-id="879f1-146">Přečtěte si přehled protokol činnosti Azure</span><span class="sxs-lookup"><span data-stu-id="879f1-146">Read the overview of the Azure Activity Log</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="879f1-147">Nastavit výstrahy na základě aktivity protokolu události</span><span class="sxs-lookup"><span data-stu-id="879f1-147">Set up an alert based on an Activity Log event</span></span>](insights-auditlog-to-webhook-email.md)

