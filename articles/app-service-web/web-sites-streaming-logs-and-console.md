---
title: protokoly aaaStreaming a konzoly
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
ms.openlocfilehash: bb4b8ce5358da12041e164dfff8f43790dd67924
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-logs-and-hello-console"></a>Protokoly streamování a hello konzoly
## <a name="streaming-logs"></a>Protokoly streamování
Hello **portál Azure** poskytuje integrované streamování protokolu prohlížeče, který umožňuje zobrazit události trasování z vaší **služby App Service** aplikace v reálném čase.  

Nastavení této funkce vyžaduje pár jednoduchých kroků:

* Zapsat trasování do vašeho kódu
* Povolit aplikaci **diagnostické protokoly** pro vaši aplikaci.
* Datový proud hello zobrazení z předdefinovaných hello **streamování protokoly** uživatelského rozhraní v hello **portál Azure**.

### <a name="how-toowrite-traces-in-your-code"></a>Jak toowrite trasování v kódu
Zápis trasování ve vašem kódu je snadné.  V jazyce C# je stejně snadná jako zápis hello následující kód:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Hello trasování třída žije v oboru názvů System.Diagnostics hello.

V aplikaci node.js můžete tento kód napsat tooachieve hello stejný výsledek:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-tooenable-and-view-hello-streaming-logs"></a>Jak tooenable a zobrazení hello datový proud protokolů
![][BrowseSitesScreenshot]Diagnostika jsou povolené na základě na aplikaci. Spustit procházení webu toohello tooenable chcete tuto funkci na.  

![][DiagnosticsLogs]Z nabídky nastavení, posuňte se dolů toohello **monitorování** části a klikněte na **(1) diagnostické protokoly**. Potom **(2) povolte** **protokolování aplikace (systém souborů)** nebo **protokolování aplikace (binární rozsáhlý objekt)** hello **úroveň** možnost umožňuje změnit hello úroveň závažnosti toocapture trasování. Pokud zkoušíte právě tooget obeznámeni s funkcí hello, nastavte úroveň hello příliš**podrobné** tooensure jsou shromažďovány všechny trasovacích příkazů.

Klikněte na tlačítko **Uložit** hello horní části okna hello a vy máte připravené tooview protokoly.

> [!NOTE]
> vyšší hello Hello **úroveň závažnosti** hello jsou další prostředky spotřebované toolog a hello vytváří další trasování. Zajistěte, aby **úroveň závažnosti** je nakonfigurované toohello správné podrobností pro produkční nebo intenzivní provoz lokality. 
> 
> 

![][StreamingLogsScreenshot]tooview hello **protokoly streamování** z v rámci hello portál Azure, klikněte na **(1) datový proud protokolu** i v hello **monitorování** části nabídky nastavení hello. Pokud vaše aplikace je aktivně psaní příkazů trasování, pak byste měli vidět v hello **(2) streamování protokoly uživatelského rozhraní** skoro v reálném čase.

## <a name="console"></a>Konzola
Hello **portál Azure** poskytuje konzolovou aplikaci tooyour přístup. Vám umožní zkoumat systém souborů vaší aplikace a spouštění skriptů prostředí powershell nebo cmd. Jsou vázány hello stejná oprávnění nastavit jako spuštěného kódu aplikace při provádění příkazů konzoly. Je blokován přístup tooprotected adresáře nebo spuštěné skripty, které vyžadují oprávnění vyšší úrovně.  

![][ConsoleScreenshot]Z nabídky nastavení, posuňte se dolů příliš**nástroje pro vývoj** části a klikněte na **(1) konzoly** a hello **(2) konzoly** uživatelského rozhraní otevře toohello vpravo.

tooget obeznámeni s hello **konzoly**, zkuste základní příkazy, jako je:

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
