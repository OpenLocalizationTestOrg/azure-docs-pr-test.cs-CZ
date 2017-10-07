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
# <a name="stream-hello-azure-activity-log-tooevent-hubs"></a>Stream hello protokol činnosti Azure tooEvent rozbočovače
Hello [ **protokol činnosti Azure** ](monitoring-overview-activity-logs.md) Streamovat v téměř v reálném čase tooany aplikace pomocí hello integrované možnosti "Export" hello portálu nebo povolením hello Service Bus pravidlo Id v profilu protokolu prostřednictvím hello Rutiny Azure PowerShell nebo Azure CLI.

## <a name="what-you-can-do-with-hello-activity-log-and-event-hubs"></a>Co můžete dělat s hello protokol aktivit a Event Hubs
Můžete použít hello streamování funkce pro hello protokol aktivit několika způsoby:

* **Stream protokolování a telemetrie systémů výrobců toothird** – v čase, streamování Event Hubs se stane hello mechanismus toopipe aktivity protokolu do jiných systémů Siem a řešení pro analýzu protokolu.
* **Vytvoření vlastní telemetrii a protokolování platformy** – Pokud už máte uživatelské telemetrie platformy nebo jsou právě přemýšlíte o vytváření jeden hello vysoce škálovatelné publikování a odběru povaha Event Hubs vám umožní tooflexibly ingestování hello Protokol aktivit. [V tématu Dana Rosanova Průvodce toousing Event Hubs telemetrie platformy globálním měřítku sem.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-hello-activity-log"></a>Povolit vysílání datového proudu hello protokol aktivit
Můžete povolit buď programově, nebo přes portál hello streamování hello protokol aktivit. V obou případech můžete vybrat Namespace sběrnice služby a zásady sdíleného přístupu pro tento obor názvů a centra událostí je v daném oboru názvů vytvořena, když hello první nové aktivity protokolu událostí. Pokud nemáte Namespace Service Bus, je nutné nejprve toocreate jeden. Pokud jste dříve streamování aktivity protokolu události toothis Namespace Service Bus, bude znovu použita hello centra událostí, která byla dříve vytvořena. zásady přístupu Hello sdílené definuje hello oprávnění, která má streamování mechanismus hello. V současné době vyžadují streamování tooan Event Hubs **spravovat**, **odeslat**, a **naslouchání** oprávnění. Můžete vytvářet nebo upravovat Service Bus Namespace sdílené zásady přístupu na portálu classic hello kartě hello "Konfigurace" pro vaše Namespace Service Bus. tooupdate hello vysílání datového proudu tooinclude protokol aktivit protokolu profilu, uživatel hello provedení změny hello musí mít oprávnění ListKey hello že Service Bus autorizační pravidlo.

Hello názvů služby bus nebo event hub nemá toobe v hello stejnému předplatnému jako hello předplatné emitování protokoly tak dlouho, dokud hello uživatel, který konfiguruje nastavení hello má příslušné předplatné tooboth přístupu RBAC.

### <a name="via-azure-portal"></a>Prostřednictvím portálu Azure
1. Přejděte toohello **protokol aktivit** okno pomocí hello nabídky na levé straně hello portálu hello.
   
    ![Přejděte tooActivity protokolu portálu](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klikněte na tlačítko hello **exportovat** tlačítko hello horní části okna hello.
   
    ![Tlačítko Exportovat portálu](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. V zobrazeném okně hello můžete vybrat hello oblasti, pro které chcete toostream události a hello Service Bus Namespace, ve kterém chcete toobe centra událostí vytvořit pro streamování tyto události.
   
    ![Export protokolu aktivit okno](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klikněte na tlačítko **Uložit** toosave tato nastavení. nastavení Hello jsou okamžitě být použité tooyour předplatné.

### <a name="via-powershell-cmdlets"></a>Pomocí rutin prostředí PowerShell
Pokud profil protokolu již existuje, je nutné nejprve tooremove tento profil.

1. Použití `Get-AzureRmLogProfile` tooidentify, pokud existuje profil protokolu
2. Pokud ano, použít `Remove-AzureRmLogProfile` tooremove ho.
3. Použití `Set-AzureRmLogProfile` toocreate profil:

```
Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

Hello ID pravidla Service Bus je řetězec s Tento formát: {service bus ID prostředku} /authorizationrules/ {název klíče}, např. 

### <a name="via-azure-cli"></a>Prostřednictvím rozhraní příkazového řádku Azure
Pokud profil protokolu již existuje, je nutné nejprve tooremove tento profil.

1. Použití `azure insights logprofile list` tooidentify, pokud existuje profil protokolu
2. Pokud ano, použít `azure insights logprofile delete` tooremove ho.
3. Použití `azure insights logprofile add` toocreate profil:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

Hello ID pravidla Service Bus je řetězec s Tento formát: `{service bus resource ID}/authorizationrules/{key name}`.

## <a name="how-do-i-consume-hello-log-data-from-event-hubs"></a>Způsob, jakým využívají data protokolu hello ze služby Event Hubs?
[Zde jsou k dispozici Hello schéma pro hello protokol aktivit](monitoring-overview-activity-logs.md). Každá událost je v pole objektů JSON BLOB názvem "záznamů".

## <a name="next-steps"></a>Další kroky
* [Archiv hello účet úložiště tooa protokol aktivit](monitoring-archive-activity-log.md)
* [Přečtěte si přehled hello hello protokol činnosti Azure](monitoring-overview-activity-logs.md)
* [Nastavit výstrahy na základě aktivity protokolu události](insights-auditlog-to-webhook-email.md)

