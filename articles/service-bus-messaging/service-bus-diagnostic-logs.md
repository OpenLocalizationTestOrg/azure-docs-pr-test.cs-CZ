---
title: "diagnostické protokoly aaaAzure Service Bus | Microsoft Docs"
description: "Zjistěte, jak tooset až diagnostické protokoly pro Service Bus v Azure."
keywords: 
documentationcenter: .net
services: service-bus-messaging
author: banisadr
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: babanisa;sethm
ms.openlocfilehash: e48d6eaba6e865ae39f5b07ed6cd53d74c92e2ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-diagnostic-logs"></a>Diagnostické protokoly služby Service Bus

Pro Azure Service Bus můžete zobrazit dva typy protokolů:
* **[Protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**. Tyto protokoly obsahují informace o operace provedené na úlohu. Hello protokoly jsou vždy povolena.
* **[Diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**. Můžete nakonfigurovat diagnostické protokoly bohatší informace o všechno, co se děje v rámci úlohy. Diagnostické protokoly titulní aktivity z hello, když se vytvoří úloha hello až do odstranění hello úlohy, včetně aktualizací a aktivity, ke kterým dojde během hello úloha běží.

## <a name="turn-on-diagnostic-logs"></a>Zapnout diagnostické protokoly

Diagnostické protokoly jsou zakázané ve výchozím nastavení. diagnostické protokoly tooenable, proveďte následující kroky hello:

1.  V hello [portál Azure](https://portal.azure.com)v části **monitorování + správu**, klikněte na tlačítko **protokolů diagnostiky**.

    ![Protokoly toodiagnostic navigačním okně](./media/service-bus-diagnostic-logs/image1.png)

2. Klikněte na tlačítko hello prostředků, které chcete toomonitor.  

3.  Klikněte na tlačítko **zapněte diagnostiku**.

    ![Zapnout diagnostické protokoly](./media/service-bus-diagnostic-logs/image2.png)

4.  Pro **stav**, klikněte na tlačítko **na**.

    ![změnit stav diagnostických protokolů](./media/service-bus-diagnostic-logs/image3.png)

5.  Sada hello archivu cíl, který chcete zjistit. například účet úložiště, centra událostí nebo analýza protokolů Azure.

6.  Uložte nové nastavení diagnostiky hello.

Nové nastavení se projeví ve přibližně 10 minut. Potom protokolů se objeví v archivace cílové hello nakonfigurované na hello **protokolů diagnostiky** okno.

Další informace o konfiguraci diagnostiky najdete v tématu hello [přehled Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

## <a name="diagnostic-logs-schema"></a>Diagnostické protokoly schématu

Všechny protokoly se ukládají ve formátu JavaScript Object Notation (JSON). Každá položka má polí s řetězcem, které používají formát hello je popsaný v následující části hello.

## <a name="operational-logs-schema"></a>Schéma operační protokoly

Přihlásí hello **OperationalLogs** kategorie zachycení, co se stane, že během operace služby Service Bus. Tyto protokoly konkrétně zaznamenání hello typ operace, včetně vytváření fronty, prostředky používá a hello stav operace hello.

Řetězce formátu JSON operační protokol obsahovat prvky uvedené v následující tabulce hello:

Name (Název) | Popis
------- | -------
ID aktivity | Interní ID, použité pro sledování
EventName | Název operace           
resourceId | ID prostředku Azure Resource Manager
SubscriptionId | ID předplatného
EventTimeString | Operace čas
EventProperties | Vlastnosti operace
Status | Stav operace
Volající | Volající operace (Azure portal nebo správu klienta)
category | OperationalLogs

Tady je příklad protokol provozní řetězce formátu JSON:

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Další kroky

Navštivte hello následující odkazy toolearn více o službě Service Bus:

* [Úvod tooService sběrnice](service-bus-messaging-overview.md)
* [Začínáme se službou Service Bus](service-bus-dotnet-get-started-with-queues.md)
