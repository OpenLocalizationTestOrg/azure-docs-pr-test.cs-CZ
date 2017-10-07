---
title: "aaaStream hello protokol činnosti Azure tooEvent Hubs | Microsoft Docs"
description: "Zjistěte, jak toostream hello protokol činnosti Azure tooEvent rozbočovače."
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
ms.openlocfilehash: 336f92771b9d4379ad9dbcadc6997dfae7fae7bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="stream-hello-azure-activity-log-tooevent-hubs"></a><span data-ttu-id="6ca26-103">Stream hello protokol činnosti Azure tooEvent rozbočovače</span><span class="sxs-lookup"><span data-stu-id="6ca26-103">Stream hello Azure Activity Log tooEvent Hubs</span></span>
<span data-ttu-id="6ca26-104">Hello [ **protokol činnosti Azure** ](monitoring-overview-activity-logs.md) Streamovat v téměř v reálném čase tooany aplikace pomocí hello integrované možnosti "Export" hello portálu nebo povolením hello Service Bus pravidlo Id v profilu protokolu prostřednictvím hello Rutiny Azure PowerShell nebo Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6ca26-104">hello [**Azure Activity Log**](monitoring-overview-activity-logs.md) can be streamed in near real time tooany application using hello built-in “Export” option in hello portal, or by enabling hello Service Bus Rule Id in a Log Profile via hello Azure PowerShell Cmdlets or Azure CLI.</span></span>

## <a name="what-you-can-do-with-hello-activity-log-and-event-hubs"></a><span data-ttu-id="6ca26-105">Co můžete dělat s hello protokol aktivit a Event Hubs</span><span class="sxs-lookup"><span data-stu-id="6ca26-105">What you can do with hello Activity Log and Event Hubs</span></span>
<span data-ttu-id="6ca26-106">Můžete použít hello streamování funkce pro hello protokol aktivit několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="6ca26-106">Here are just a few ways you might use hello streaming capability for hello Activity Log:</span></span>

* <span data-ttu-id="6ca26-107">**Stream protokolování a telemetrie systémů výrobců toothird** – v čase, streamování Event Hubs se stane hello mechanismus toopipe aktivity protokolu do jiných systémů Siem a řešení pro analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="6ca26-107">**Stream toothird-party logging and telemetry systems** – Over time, Event Hubs streaming will become hello mechanism toopipe your Activity Log into third-party SIEMs and log analytics solutions.</span></span>
* <span data-ttu-id="6ca26-108">**Vytvoření vlastní telemetrii a protokolování platformy** – Pokud už máte uživatelské telemetrie platformy nebo jsou právě přemýšlíte o vytváření jeden hello vysoce škálovatelné publikování a odběru povaha Event Hubs vám umožní tooflexibly ingestování hello Protokol aktivit.</span><span class="sxs-lookup"><span data-stu-id="6ca26-108">**Build a custom telemetry and logging platform** – If you already have a custom-built telemetry platform or are just thinking about building one, hello highly scalable publish-subscribe nature of Event Hubs allows you tooflexibly ingest hello activity log.</span></span> [<span data-ttu-id="6ca26-109">V tématu Dana Rosanova Průvodce toousing Event Hubs telemetrie platformy globálním měřítku sem.</span><span class="sxs-lookup"><span data-stu-id="6ca26-109">See Dan Rosanova’s guide toousing Event Hubs in a global scale telemetry platform here.</span></span>](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-hello-activity-log"></a><span data-ttu-id="6ca26-110">Povolit vysílání datového proudu hello protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="6ca26-110">Enable streaming of hello Activity Log</span></span>
<span data-ttu-id="6ca26-111">Můžete povolit buď programově, nebo přes portál hello streamování hello protokol aktivit.</span><span class="sxs-lookup"><span data-stu-id="6ca26-111">You can enable streaming of hello Activity Log either programmatically or via hello portal.</span></span> <span data-ttu-id="6ca26-112">V obou případech můžete vybrat Namespace sběrnice služby a zásady sdíleného přístupu pro tento obor názvů a centra událostí je v daném oboru názvů vytvořena, když hello první nové aktivity protokolu událostí.</span><span class="sxs-lookup"><span data-stu-id="6ca26-112">Either way, you pick a Service Bus Namespace and a shared access policy for that namespace, and an Event Hub is created in that namespace when hello first new Activity Log event occurs.</span></span> <span data-ttu-id="6ca26-113">Pokud nemáte Namespace Service Bus, je nutné nejprve toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="6ca26-113">If you do not have a Service Bus Namespace, you first need toocreate one.</span></span> <span data-ttu-id="6ca26-114">Pokud jste dříve streamování aktivity protokolu události toothis Namespace Service Bus, bude znovu použita hello centra událostí, která byla dříve vytvořena.</span><span class="sxs-lookup"><span data-stu-id="6ca26-114">If you have previously streamed Activity Log events toothis Service Bus Namespace, hello Event Hub that was previously created will be reused.</span></span> <span data-ttu-id="6ca26-115">zásady přístupu Hello sdílené definuje hello oprávnění, která má streamování mechanismus hello.</span><span class="sxs-lookup"><span data-stu-id="6ca26-115">hello shared access policy defines hello permissions that hello streaming mechanism has.</span></span> <span data-ttu-id="6ca26-116">V současné době vyžadují streamování tooan Event Hubs **spravovat**, **odeslat**, a **naslouchání** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="6ca26-116">Today, streaming tooan Event Hubs requires **Manage**, **Send**, and **Listen** permissions.</span></span> <span data-ttu-id="6ca26-117">Můžete vytvářet nebo upravovat Service Bus Namespace sdílené zásady přístupu na portálu classic hello kartě hello "Konfigurace" pro vaše Namespace Service Bus.</span><span class="sxs-lookup"><span data-stu-id="6ca26-117">You can create or modify Service Bus Namespace shared access policies in hello classic portal under hello “Configure” tab for your Service Bus Namespace.</span></span> <span data-ttu-id="6ca26-118">tooupdate hello vysílání datového proudu tooinclude protokol aktivit protokolu profilu, uživatel hello provedení změny hello musí mít oprávnění ListKey hello že Service Bus autorizační pravidlo.</span><span class="sxs-lookup"><span data-stu-id="6ca26-118">tooupdate hello Activity Log log profile tooinclude streaming, hello user making hello change must have hello ListKey permission on that Service Bus Authorization Rule.</span></span>

<span data-ttu-id="6ca26-119">Hello názvů služby bus nebo event hub nemá toobe v hello stejnému předplatnému jako hello předplatné emitování protokoly tak dlouho, dokud hello uživatel, který konfiguruje nastavení hello má příslušné předplatné tooboth přístupu RBAC.</span><span class="sxs-lookup"><span data-stu-id="6ca26-119">hello service bus or event hub namespace does not have toobe in hello same subscription as hello subscription emitting logs as long as hello user who configures hello setting has appropriate RBAC access tooboth subscriptions.</span></span>

### <a name="via-azure-portal"></a><span data-ttu-id="6ca26-120">Prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6ca26-120">Via Azure portal</span></span>
1. <span data-ttu-id="6ca26-121">Přejděte toohello **protokol aktivit** okno pomocí hello nabídky na levé straně hello portálu hello.</span><span class="sxs-lookup"><span data-stu-id="6ca26-121">Navigate toohello **Activity Log** blade using hello menu on hello left side of hello portal.</span></span>
   
    ![Přejděte tooActivity protokolu portálu](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. <span data-ttu-id="6ca26-123">Klikněte na tlačítko hello **exportovat** tlačítko hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="6ca26-123">Click hello **Export** button at hello top of hello blade.</span></span>
   
    ![Tlačítko Exportovat portálu](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. <span data-ttu-id="6ca26-125">V zobrazeném okně hello můžete vybrat hello oblasti, pro které chcete toostream události a hello Service Bus Namespace, ve kterém chcete toobe centra událostí vytvořit pro streamování tyto události.</span><span class="sxs-lookup"><span data-stu-id="6ca26-125">In hello blade that appears, you can select hello regions for which you would like toostream events and hello Service Bus Namespace in which you would like an Event Hub toobe created for streaming these events.</span></span>
   
    ![Export protokolu aktivit okno](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. <span data-ttu-id="6ca26-127">Klikněte na tlačítko **Uložit** toosave tato nastavení.</span><span class="sxs-lookup"><span data-stu-id="6ca26-127">Click **Save** toosave these settings.</span></span> <span data-ttu-id="6ca26-128">nastavení Hello jsou okamžitě být použité tooyour předplatné.</span><span class="sxs-lookup"><span data-stu-id="6ca26-128">hello settings are immediately be applied tooyour subscription.</span></span>

### <a name="via-powershell-cmdlets"></a><span data-ttu-id="6ca26-129">Pomocí rutin prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6ca26-129">Via PowerShell Cmdlets</span></span>
<span data-ttu-id="6ca26-130">Pokud profil protokolu již existuje, je nutné nejprve tooremove tento profil.</span><span class="sxs-lookup"><span data-stu-id="6ca26-130">If a log profile already exists, you first need tooremove that profile.</span></span>

1. <span data-ttu-id="6ca26-131">Použití `Get-AzureRmLogProfile` tooidentify, pokud existuje profil protokolu</span><span class="sxs-lookup"><span data-stu-id="6ca26-131">Use `Get-AzureRmLogProfile` tooidentify if a log profile exists</span></span>
2. <span data-ttu-id="6ca26-132">Pokud ano, použít `Remove-AzureRmLogProfile` tooremove ho.</span><span class="sxs-lookup"><span data-stu-id="6ca26-132">If so, use `Remove-AzureRmLogProfile` tooremove it.</span></span>
3. <span data-ttu-id="6ca26-133">Použití `Set-AzureRmLogProfile` toocreate profil:</span><span class="sxs-lookup"><span data-stu-id="6ca26-133">Use `Set-AzureRmLogProfile` toocreate a profile:</span></span>

```
Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

<span data-ttu-id="6ca26-134">Hello ID pravidla Service Bus je řetězec s Tento formát: {service bus ID prostředku} /authorizationrules/ {název klíče}, např.</span><span class="sxs-lookup"><span data-stu-id="6ca26-134">hello Service Bus Rule ID is a string with this format: {service bus resource ID}/authorizationrules/{key name}, for example</span></span> 

### <a name="via-azure-cli"></a><span data-ttu-id="6ca26-135">Prostřednictvím rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="6ca26-135">Via Azure CLI</span></span>
<span data-ttu-id="6ca26-136">Pokud profil protokolu již existuje, je nutné nejprve tooremove tento profil.</span><span class="sxs-lookup"><span data-stu-id="6ca26-136">If a log profile already exists, you first need tooremove that profile.</span></span>

1. <span data-ttu-id="6ca26-137">Použití `azure insights logprofile list` tooidentify, pokud existuje profil protokolu</span><span class="sxs-lookup"><span data-stu-id="6ca26-137">Use `azure insights logprofile list` tooidentify if a log profile exists</span></span>
2. <span data-ttu-id="6ca26-138">Pokud ano, použít `azure insights logprofile delete` tooremove ho.</span><span class="sxs-lookup"><span data-stu-id="6ca26-138">If so, use `azure insights logprofile delete` tooremove it.</span></span>
3. <span data-ttu-id="6ca26-139">Použití `azure insights logprofile add` toocreate profil:</span><span class="sxs-lookup"><span data-stu-id="6ca26-139">Use `azure insights logprofile add` toocreate a profile:</span></span>

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

<span data-ttu-id="6ca26-140">Hello ID pravidla Service Bus je řetězec s Tento formát: `{service bus resource ID}/authorizationrules/{key name}`.</span><span class="sxs-lookup"><span data-stu-id="6ca26-140">hello Service Bus Rule ID is a string with this format: `{service bus resource ID}/authorizationrules/{key name}`.</span></span>

## <a name="how-do-i-consume-hello-log-data-from-event-hubs"></a><span data-ttu-id="6ca26-141">Způsob, jakým využívají data protokolu hello ze služby Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="6ca26-141">How do I consume hello log data from Event Hubs?</span></span>
<span data-ttu-id="6ca26-142">[Zde jsou k dispozici Hello schéma pro hello protokol aktivit](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="6ca26-142">[hello schema for hello Activity Log is available here](monitoring-overview-activity-logs.md).</span></span> <span data-ttu-id="6ca26-143">Každá událost je v pole objektů JSON BLOB názvem "záznamů".</span><span class="sxs-lookup"><span data-stu-id="6ca26-143">Each event is in an array of JSON blobs called “records.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ca26-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6ca26-144">Next Steps</span></span>
* [<span data-ttu-id="6ca26-145">Archiv hello účet úložiště tooa protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="6ca26-145">Archive hello Activity Log tooa storage account</span></span>](monitoring-archive-activity-log.md)
* [<span data-ttu-id="6ca26-146">Přečtěte si přehled hello hello protokol činnosti Azure</span><span class="sxs-lookup"><span data-stu-id="6ca26-146">Read hello overview of hello Azure Activity Log</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="6ca26-147">Nastavit výstrahy na základě aktivity protokolu události</span><span class="sxs-lookup"><span data-stu-id="6ca26-147">Set up an alert based on an Activity Log event</span></span>](insights-auditlog-to-webhook-email.md)

