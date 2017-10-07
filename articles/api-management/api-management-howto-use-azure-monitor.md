---
title: "aaaMonitor s monitorováním Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toomonitor Azure API Management služby pomocí Azure monitorování."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 5012d8ed57ea4f94ea6bc1b7c4e1102516ec4414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-api-management-with-azure-monitor"></a>Monitorování API Management s Azure monitorování
Azure monitorování je služba Azure, která poskytuje jednoho zdroje pro monitorování všech prostředků Azure. S Azure monitorování můžete vizualizovat, dotaz, směrovat, archivaci a provádět akce na hello metriky a protokoly pocházejících z prostředků Azure, jako je například Správa rozhraní API. 

následující video ukazuje, jak Hello toomonitor pomocí monitorování Azure API Management. Další informace o monitorování Azure najdete v tématu [Začínáme s Azure monitorování]. 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a>Metriky
API Management aktuálně vysílá pět metriky a plánujeme tooadd další v budoucnu hello. Tyto metriky jsou vygenerované každou minutu, která poskytuje téměř v reálném čase přehled hello stavu a stavu vašich rozhraní API. Následuje souhrn hello metriky:
* Celkový počet požadavků brány: hello počet rozhraní API požadavků v období hello. 
* Úspěšné požadavky brány: hello počet žádostí o rozhraní API, které přijaly kódy úspěšné odpovědi HTTP včetně 304, 307 a nic menší než 301 (například 200). 
* Neúspěšné požadavky brány: hello počet žádostí o rozhraní API, které obdržel chybné kódy odpovědi HTTP včetně 400 a nic větší než 500.
* Neoprávněné žádosti brány: hello počet žádostí o rozhraní API, které přijaly, včetně 401, 403 a 429 kódy odpovědi HTTP. 
* Další požadavky na brány: hello počet žádostí o rozhraní API, které přijaly kódy odpovědí protokolu HTTP, které nejsou součástí tooany hello předchozích kategorií (například 418).

Můžete přistupovat metriky ve službě API Management, nebo přístup metrik Azure prostředky v Azure monitorování. Metrika tooview ve službě API Management:
1. Otevřete hello portálu Azure.
2. Přejděte tooyour služba API Management.
3. Klikněte na tlačítko **metriky**.

![Okno metriky][metrics-blade]

Další informace o tom, najdete v části toouse metriky, [přehled metriky].

## <a name="activity-logs"></a>Protokoly aktivit
Protokoly aktivity získat přehled o hello operace, které byly provedeny na vaše služby API Management. Se dřív označovala jako "protokoly auditu" nebo "operační protokoly". Pomocí protokolů z aktivity, můžete určit hello ", kdo a kdy" pro všechny operace (PUT, POST, DELETE) na vaše rozhraní API správy služby zápisu. 

> [!NOTE]
> Protokoly aktivity nezahrnují operace čtení (GET) nebo operace provedené v hello portálu classic vydavatele nebo pomocí hello původní rozhraní API pro správu.

Lze zobrazit protokoly aktivit ve službě API Management nebo přístup k protokolům všechny prostředky Azure v Azure monitorování. Aktivita tooview protokolů ve službě API Management:
1. Otevřete hello portálu Azure.
2. Přejděte tooyour služba API Management.
3. Klikněte na tlačítko **protokol aktivit**.

![Okno protokoly aktivit][activity-logs-blade]

Další informace o tom, najdete v části toouse metriky, [protokoly aktivity přehled].

## <a name="alerts"></a>Výstrahy
Můžete nakonfigurovat tooreceive výstrahy založené na protokolech metriky a aktivity. Azure monitorování umožňuje tooconfigure výstrahy toodo hello následující při aktivaci:

* Odeslat oznámení e-mailu
* Volat webhook, jehož
* Vyvolání aplikace logiky Azure

Pravidla výstrah můžete nakonfigurovat ve službě API Management, nebo v Azure monitorování. tooconfigure je ve službě API Management: 
1. Otevřete hello portálu Azure.
2. Přejděte tooyour služba API Management.
3. Klikněte na tlačítko **výstrah pravidla**.

![Okno pravidla výstrah][alert-rules-blade]

Další informace o používání výstrah najdete v tématu [přehled výstrah].

## <a name="diagnostic-logs"></a>Diagnostické protokoly
Diagnostické protokoly poskytují bohaté informace o operacích a chyby, které jsou důležité pro auditování i pro účely odstraňování potíží. Diagnostické protokoly se liší od protokoly aktivity. Protokoly aktivity poskytují přehled o hello operace, které byly provedeny na vašich prostředků Azure. Diagnostické protokoly získat přehled o operace, aby prostředku provedeny sám sebe.

API Management aktuálně poskytuje diagnostické protokoly (každou hodinu dávkově) o jednotlivých API žádosti s každou položku s hello strukturu:

```
{
    "Tenant": "",
      "DeploymentName": "",
      "time": "",
      "resourceId": "",
      "category": "GatewayLogs",
      "operationName": "Microsoft.ApiManagement/GatewayLogs",
      "durationMs": ,
      "Level": ,
      "properties": "{
          "ApiId": "",
          "OperationId": "",
          "ProductId": "",
          "SubscriptionId": "",
          "Method": "",
          "Url": "",
          "RequestSize": ,
          "ServiceTime": "",
          "BackendMethod": "",
          "BackendUrl": "",
          "BackendResponseCode": ,
          "ResponseCode": ,
          "ResponseSize": ,
          "Cache": "",
          "UserId"
      }"
 }
```

Můžete přístup k diagnostickým protokolům ve službě API Management, nebo přístup k protokolům všechny prostředky Azure v Azure monitorování. diagnostické protokoly tooview ve službě API Management:
1. Otevřete hello portálu Azure.
2. Přejděte tooyour služba API Management.
3. Klikněte na tlačítko **protokolů diagnostiky**.

![Okno Diagnostické protokoly][diagnostic-logs-blade]

Další informace o tom, najdete v části toouse metriky, [přehled diagnostické protokoly].

## <a name="next-step"></a>Další krok

* [Začínáme s Azure monitorování]
* [přehled metriky]
* [protokoly aktivity přehled]
* [přehled diagnostické protokoly]
* [přehled výstrah]

[Začínáme s Azure monitorování]: ../monitoring-and-diagnostics/monitoring-get-started.md
[přehled metriky]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md
[protokoly aktivity přehled]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[přehled diagnostické protokoly]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[přehled výstrah]: ../monitoring-and-diagnostics/insights-alerts-portal.md



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png
