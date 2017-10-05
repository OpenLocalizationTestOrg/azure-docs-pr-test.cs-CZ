---
title: "Protokoly streamování a konzoly"
description: "Protokoly streamování a přehled konzole"
author: btardif
manager: erikre
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: 3e50a287-781b-4c6a-8c53-eec261889d7a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/12/2016
ms.author: byvinyal
ms.openlocfilehash: 120ce6b115820728b9f853e9ff349beb0ef29c9d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="streaming-logs-and-the-console"></a>Protokoly streamování a konzoly
## <a name="streaming-logs"></a>Protokoly streamování
**Portál Azure** poskytuje integrované streamování protokolu prohlížeče, který umožňuje zobrazit události trasování z vaší **služby App Service** aplikace v reálném čase.  

Nastavení této funkce vyžaduje pár jednoduchých kroků:

* Zapsat trasování do vašeho kódu
* Povolit aplikaci **diagnostické protokoly** pro vaši aplikaci.
* Zobrazení datového proudu z integrovaného **streamování protokoly** uživatelského rozhraní v **portál Azure**.

### <a name="how-to-write-traces-in-your-code"></a>Jak napsat trasování v kódu
Zápis trasování ve vašem kódu je snadné.  V jazyce C# je stejně snadná jako zápis následující kód:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Třída trasování je umístěn v oboru názvů System.Diagnostics.

V aplikaci node.js můžete napsat tento kód k dosažení stejného výsledku:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Povolení a zobrazení, streamování protokolů
![][BrowseSitesScreenshot]Diagnostika jsou povolené na základě na aplikaci. Spustit tak, že přejde k lokalitě, kterou chcete povolit tuto funkci na.  

![][DiagnosticsLogs]Z nabídky nastavení, přejděte dolů k položce **monitorování** části a klikněte na **(1) diagnostické protokoly**. Potom **(2) povolte** **protokolování aplikace (systém souborů)** nebo **protokolování aplikace (binární rozsáhlý objekt)** **úroveň** možnost vám umožňuje změnit úroveň závažnosti trasování pro zachycení. Pokud jste právě se seznamte se s funkcí, nastavení úrovně na **podrobné** zajistit jsou shromažďovány všechny trasovacích příkazů.

Klikněte na tlačítko **Uložit** v horní části okna a jste připraveni k zobrazení protokolů.

> [!NOTE]
> Vyšší **úroveň závažnosti** do protokolu se spotřebovávají více prostředků a vytváří další trasování. Zajistěte, aby **úroveň závažnosti** nakonfigurovaný tak, aby správné podrobností pro produkční nebo intenzivní provoz lokality. 
> 
> 

![][StreamingLogsScreenshot]Chcete-li zobrazit **protokoly streamování** z portálu Azure, klikněte na **(1) datový proud protokolu** taky ve **monitorování** části nabídky nastavení. Pokud vaše aplikace je aktivně psaní příkazů trasování, pak byste měli vidět v **(2) streamování protokoly uživatelského rozhraní** skoro v reálném čase.

## <a name="console"></a>Konzola
**Portál Azure** poskytuje přístup ke konzole do vaší aplikace. Vám umožní zkoumat systém souborů vaší aplikace a spouštění skriptů prostředí powershell nebo cmd. Jsou vázány stejná oprávnění nastavená jako spuštěného kódu aplikace při provádění příkazů konzoly. Přístup k chráněné adresáře nebo spouštění skriptů, které vyžadují oprávnění vyšší úrovně je blokovaný.  

![][ConsoleScreenshot]Z nabídky nastavení, přejděte dolů k položce **nástroje pro vývoj** části a klikněte na **(1) konzoly** a **(2) konzoly** uživatelského rozhraní se otevře napravo.

Abyste se seznámili s **konzoly**, zkuste základní příkazy, jako je:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
