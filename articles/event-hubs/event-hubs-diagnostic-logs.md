---
title: "diagnostické protokoly aaaAzure Event Hubs | Microsoft Docs"
description: "Zjistěte, jak tooset až diagnostické protokoly pro event hubs v Azure."
keywords: 
documentationcenter: 
services: event-hubs
author: banisadr
manager: 
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: sethm;babanisa
ms.openlocfilehash: d2054e2e444e715e5077fe2608fe1e009e6c1d84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-diagnostic-logs"></a>Diagnostické protokoly událostí rozbočovače

Pro Azure Event Hubs můžete zobrazit dva typy protokolů:
* **[Protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**. Tyto protokoly mít informace o operacích na úlohu provést. Hello protokoly jsou vždy povolena.
* **[Diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**. Můžete nakonfigurovat diagnostické protokoly pro širší zobrazení všechno, co se děje s úlohou. Diagnostické protokoly titulní aktivity z hello, když se vytvoří úloha hello až do odstranění hello úlohy, včetně aktualizací a aktivity, ke kterým dojde během hello úloha běží.

## <a name="turn-on-diagnostic-logs"></a>Zapnout diagnostické protokoly
Diagnostické protokoly jsou zakázané ve výchozím nastavení. diagnostické protokoly tooenable:

1.  V hello [portál Azure](https://portal.azure.com)v části **monitorování + správu**, klikněte na tlačítko **protokolů diagnostiky**.

    ![Protokoly toodiagnostic navigačním okně](./media/event-hubs-diagnostic-logs/image1.png)

2.  Klikněte na tlačítko hello prostředků, které chcete toomonitor.

3.  Klikněte na tlačítko **zapněte diagnostiku**.

    ![Zapnout diagnostické protokoly](./media/event-hubs-diagnostic-logs/image2.png)

4.  Pro **stav**, klikněte na tlačítko **na**.

    ![Změnit stav hello diagnostických protokolů](./media/event-hubs-diagnostic-logs/image3.png)

5.  Sada hello archivu cíl, který chcete zjistit. například účet úložiště, centra událostí nebo analýza protokolů Azure.

6.  Uložte nové nastavení diagnostiky hello.

Nové nastavení se projeví ve přibližně 10 minut. Potom protokolů se objeví v archivace cílové hello nakonfigurované na hello **protokolů diagnostiky** okno.

Další informace o konfiguraci diagnostiky najdete v tématu hello [přehled Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

## <a name="diagnostic-logs-categories"></a>Diagnostické protokoly kategorií
Centra událostí jsou zaznamenány diagnostické protokoly pro dvou kategorií:

* **ArchiveLogs**: protokoly související s archivy tooEvent rozbočovače, konkrétně protokoly tooarchive související chyby.
* **OperationalLogs**: informace o co se děje během operace služby Event Hubs, konkrétně hello typ operace, včetně vytváření centra událostí, prostředky používá a hello stav operace hello.

## <a name="diagnostic-logs-schema"></a>Diagnostické protokoly schématu
Všechny protokoly se ukládají ve formátu JavaScript Object Notation (JSON). Každá položka má polí s řetězcem, které používají formát hello je popsaný v následující části hello.

### <a name="archive-logs-schema"></a>Schéma protokoly archivu

Řetězce formátu JSON protokolu archivu obsahovat prvky uvedené v následující tabulce hello:

Name (Název) | Popis
------- | -------
Název úlohy | Popis hello úlohu, která se nezdařila.
ID aktivity | Interní ID použité pro sledování.
trackingId | Interní ID použité pro sledování.
resourceId | Azure Resource Manager ID prostředku.
Centrum EventHub | Centra událostí celý název (včetně oboru názvů).
ID oddílu | Zapisuje do oddílu centra událostí.
archiveStep | ArchiveFlushWriter
startTime | Selhání spuštění.
selhání | Počet časy, kdy došlo k chybě.
durationInSeconds | Doba trvání selhání.
Zpráva | Chybová zpráva.
category | ArchiveLogs

Hello následující kód je příkladem protokol archivu řetězce formátu JSON:

```json
{
     "TaskName": "EventHubArchiveUserError",
     "ActivityId": "21b89a0b-8095-471a-9db8-d151d74ecf26",
     "trackingId": "21b89a0b-8095-471a-9db8-d151d74ecf26_B7",
     "resourceId": "/SUBSCRIPTIONS/854D368F-1828-428F-8F3C-F2AFFA9B2F7D/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
     "eventHub": "fbettati-opera-eventhub:eventhub:eh123~32766",
     "partitionId": "1",
     "archiveStep": "ArchiveFlushWriter",
     "startTime": "9/22/2016 5:11:21 AM",
     "failures": 3,
     "durationInSeconds": 360,
     "message": "Microsoft.WindowsAzure.Storage.StorageException: hello remote server returned an error: (404) Not Found. ---> System.Net.WebException: hello remote server returned an error: (404) Not Found.\r\n   at Microsoft.WindowsAzure.Storage.Shared.Protocol.HttpResponseParsers.ProcessExpectedStatusCodeNoException[T](HttpStatusCode expectedStatusCode, HttpStatusCode actualStatusCode, T retVal, StorageCommandBase`1 cmd, Exception ex)\r\n   at Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob.<PutBlockImpl>b__3e(RESTCommand`1 cmd, HttpWebResponse resp, Exception ex, OperationContext ctx)\r\n   at Microsoft.WindowsAzure.Storage.Core.Executor.Executor.EndGetResponse[T](IAsyncResult getResponseResult)\r\n   --- End of inner exception stack trace ---\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.StorageAsyncResult`1.End()\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.AsyncExtensions.<>c__DisplayClass4.<CreateCallbackVoid>b__3(IAsyncResult ar)\r\n--- End of stack trace from previous location where exception was thrown ---\r\n   at System.",
     "category": "ArchiveLogs"
}
```

### <a name="operational-logs-schema"></a>Schéma operační protokoly

Řetězce formátu JSON operační protokol obsahovat prvky uvedené v následující tabulce hello:

Name (Název) | Popis
------- | -------
ID aktivity | Vnitřní ID použít tootrack účel.
EventName | Název operace.  
resourceId | Azure Resource Manager ID prostředku.
SubscriptionId | ID předplatného.
EventTimeString | Operace čas.
EventProperties | Vlastnosti operace.
Status | Stav operace.
Volající | Volající operace (Azure portal nebo správu klienta).
category | OperationalLogs

Hello následující kód je příkladem řetězce JSON operační protokol:

```json
Example:
{
     "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
     "EventName": "Create EventHub",
     "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/SHOEBOXEHNS-CY4001",
     "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
     "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
     "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
     "Status": "Succeeded",
     "Caller": "ServiceBus Client",
     "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Další kroky
* [Úvod tooEvent rozbočovače](event-hubs-what-is-event-hubs.md)
* [Přehled rozhraní API služby Event Hubs](event-hubs-api-overview.md)
* [Začínáme s Event Hubs](event-hubs-csharp-ephcs-getstarted.md)
