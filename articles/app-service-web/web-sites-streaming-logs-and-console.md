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
# <a name="streaming-logs-and-hello-console"></a><span data-ttu-id="2a46c-103">Protokoly streamování a hello konzoly</span><span class="sxs-lookup"><span data-stu-id="2a46c-103">Streaming Logs and hello Console</span></span>
## <a name="streaming-logs"></a><span data-ttu-id="2a46c-104">Protokoly streamování</span><span class="sxs-lookup"><span data-stu-id="2a46c-104">Streaming Logs</span></span>
<span data-ttu-id="2a46c-105">Hello **portál Azure** poskytuje integrované streamování protokolu prohlížeče, který umožňuje zobrazit události trasování z vaší **služby App Service** aplikace v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="2a46c-105">hello **Azure portal** provides an integrated streaming log viewer that lets you view tracing events from your **App Service** apps in real time.</span></span>  

<span data-ttu-id="2a46c-106">Nastavení této funkce vyžaduje pár jednoduchých kroků:</span><span class="sxs-lookup"><span data-stu-id="2a46c-106">Setting up this feature requires a few simple steps:</span></span>

* <span data-ttu-id="2a46c-107">Zapsat trasování do vašeho kódu</span><span class="sxs-lookup"><span data-stu-id="2a46c-107">Write traces in your code</span></span>
* <span data-ttu-id="2a46c-108">Povolit aplikaci **diagnostické protokoly** pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2a46c-108">Enable Application **Diagnostic Logs** for your app</span></span>
* <span data-ttu-id="2a46c-109">Datový proud hello zobrazení z předdefinovaných hello **streamování protokoly** uživatelského rozhraní v hello **portál Azure**.</span><span class="sxs-lookup"><span data-stu-id="2a46c-109">View hello stream from hello built-in **Streaming Logs** UI in hello **Azure portal**.</span></span>

### <a name="how-toowrite-traces-in-your-code"></a><span data-ttu-id="2a46c-110">Jak toowrite trasování v kódu</span><span class="sxs-lookup"><span data-stu-id="2a46c-110">How toowrite traces in your code</span></span>
<span data-ttu-id="2a46c-111">Zápis trasování ve vašem kódu je snadné.</span><span class="sxs-lookup"><span data-stu-id="2a46c-111">Writing traces in your code is easy.</span></span>  <span data-ttu-id="2a46c-112">V jazyce C# je stejně snadná jako zápis hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="2a46c-112">In C# it's as easy as writing hello following code:</span></span>

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

<span data-ttu-id="2a46c-113">Hello trasování třída žije v oboru názvů System.Diagnostics hello.</span><span class="sxs-lookup"><span data-stu-id="2a46c-113">hello Trace class lives in hello System.Diagnostics namespace.</span></span>

<span data-ttu-id="2a46c-114">V aplikaci node.js můžete tento kód napsat tooachieve hello stejný výsledek:</span><span class="sxs-lookup"><span data-stu-id="2a46c-114">In a node.js app you can write this code tooachieve hello same result:</span></span>

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-tooenable-and-view-hello-streaming-logs"></a><span data-ttu-id="2a46c-115">Jak tooenable a zobrazení hello datový proud protokolů</span><span class="sxs-lookup"><span data-stu-id="2a46c-115">How tooenable and view hello streaming logs</span></span>
<span data-ttu-id="2a46c-116">![][BrowseSitesScreenshot]Diagnostika jsou povolené na základě na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2a46c-116">![][BrowseSitesScreenshot] Diagnostics are enabled on a per app basis.</span></span> <span data-ttu-id="2a46c-117">Spustit procházení webu toohello tooenable chcete tuto funkci na.</span><span class="sxs-lookup"><span data-stu-id="2a46c-117">Start by browsing toohello site you would like tooenable this feature on.</span></span>  

<span data-ttu-id="2a46c-118">![][DiagnosticsLogs]Z nabídky nastavení, posuňte se dolů toohello **monitorování** části a klikněte na **(1) diagnostické protokoly**.</span><span class="sxs-lookup"><span data-stu-id="2a46c-118">![][DiagnosticsLogs] From settings menu, scroll down toohello **Monitoring** section and click on **(1) Diagnostic Logs**.</span></span> <span data-ttu-id="2a46c-119">Potom **(2) povolte** **protokolování aplikace (systém souborů)** nebo **protokolování aplikace (binární rozsáhlý objekt)** hello **úroveň** možnost umožňuje změnit hello úroveň závažnosti toocapture trasování.</span><span class="sxs-lookup"><span data-stu-id="2a46c-119">Then **(2) enable** **Application Logging (Filesystem)** or **Application Logging (blob)** hello **Level** option lets you change hello severity level of traces toocapture.</span></span> <span data-ttu-id="2a46c-120">Pokud zkoušíte právě tooget obeznámeni s funkcí hello, nastavte úroveň hello příliš**podrobné** tooensure jsou shromažďovány všechny trasovacích příkazů.</span><span class="sxs-lookup"><span data-stu-id="2a46c-120">If you're just trying tooget familiar with hello feature, set hello level too**Verbose** tooensure all of your trace statements are collected.</span></span>

<span data-ttu-id="2a46c-121">Klikněte na tlačítko **Uložit** hello horní části okna hello a vy máte připravené tooview protokoly.</span><span class="sxs-lookup"><span data-stu-id="2a46c-121">Click **SAVE** at hello top of hello blade and you're ready tooview logs.</span></span>

> [!NOTE]
> <span data-ttu-id="2a46c-122">vyšší hello Hello **úroveň závažnosti** hello jsou další prostředky spotřebované toolog a hello vytváří další trasování.</span><span class="sxs-lookup"><span data-stu-id="2a46c-122">hello higher hello **severity level** hello more resources are consumed toolog and hello more traces are produced.</span></span> <span data-ttu-id="2a46c-123">Zajistěte, aby **úroveň závažnosti** je nakonfigurované toohello správné podrobností pro produkční nebo intenzivní provoz lokality.</span><span class="sxs-lookup"><span data-stu-id="2a46c-123">Make sure **severity level** is configured toohello correct verbosity for a production or high traffic site.</span></span> 
> 
> 

<span data-ttu-id="2a46c-124">![][StreamingLogsScreenshot]tooview hello **protokoly streamování** z v rámci hello portál Azure, klikněte na **(1) datový proud protokolu** i v hello **monitorování** části nabídky nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="2a46c-124">![][StreamingLogsScreenshot] tooview hello **streaming logs** from within hello Azure portal, click on **(1) Log Stream** also in hello **Monitoring** section of hello settings menu.</span></span> <span data-ttu-id="2a46c-125">Pokud vaše aplikace je aktivně psaní příkazů trasování, pak byste měli vidět v hello **(2) streamování protokoly uživatelského rozhraní** skoro v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="2a46c-125">If your app is actively writing trace statements, then you should see them in hello **(2) streaming logs UI** in near real time.</span></span>

## <a name="console"></a><span data-ttu-id="2a46c-126">Konzola</span><span class="sxs-lookup"><span data-stu-id="2a46c-126">Console</span></span>
<span data-ttu-id="2a46c-127">Hello **portál Azure** poskytuje konzolovou aplikaci tooyour přístup.</span><span class="sxs-lookup"><span data-stu-id="2a46c-127">hello **Azure portal** provides console access tooyour app.</span></span> <span data-ttu-id="2a46c-128">Vám umožní zkoumat systém souborů vaší aplikace a spouštění skriptů prostředí powershell nebo cmd.</span><span class="sxs-lookup"><span data-stu-id="2a46c-128">You can explore your app's file system and run powershell/cmd scripts.</span></span> <span data-ttu-id="2a46c-129">Jsou vázány hello stejná oprávnění nastavit jako spuštěného kódu aplikace při provádění příkazů konzoly.</span><span class="sxs-lookup"><span data-stu-id="2a46c-129">You are bound by hello same permissions set as your running app code when executing console commands.</span></span> <span data-ttu-id="2a46c-130">Je blokován přístup tooprotected adresáře nebo spuštěné skripty, které vyžadují oprávnění vyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="2a46c-130">Access tooprotected directories or running scripts that require elevated permissions is blocked.</span></span>  

<span data-ttu-id="2a46c-131">![][ConsoleScreenshot]Z nabídky nastavení, posuňte se dolů příliš**nástroje pro vývoj** části a klikněte na **(1) konzoly** a hello **(2) konzoly** uživatelského rozhraní otevře toohello vpravo.</span><span class="sxs-lookup"><span data-stu-id="2a46c-131">![][ConsoleScreenshot] From settings menu, scroll down too**Development Tools** section and click on **(1) Console** and hello **(2) console** UI opens toohello right.</span></span>

<span data-ttu-id="2a46c-132">tooget obeznámeni s hello **konzoly**, zkuste základní příkazy, jako je:</span><span class="sxs-lookup"><span data-stu-id="2a46c-132">tooget familiar with hello **console**, try basic commands like:</span></span>

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
