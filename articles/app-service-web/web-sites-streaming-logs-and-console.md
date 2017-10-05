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
# <a name="streaming-logs-and-the-console"></a><span data-ttu-id="f9802-103">Protokoly streamování a konzoly</span><span class="sxs-lookup"><span data-stu-id="f9802-103">Streaming Logs and the Console</span></span>
## <a name="streaming-logs"></a><span data-ttu-id="f9802-104">Protokoly streamování</span><span class="sxs-lookup"><span data-stu-id="f9802-104">Streaming Logs</span></span>
<span data-ttu-id="f9802-105">**Portál Azure** poskytuje integrované streamování protokolu prohlížeče, který umožňuje zobrazit události trasování z vaší **služby App Service** aplikace v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="f9802-105">The **Azure portal** provides an integrated streaming log viewer that lets you view tracing events from your **App Service** apps in real time.</span></span>  

<span data-ttu-id="f9802-106">Nastavení této funkce vyžaduje pár jednoduchých kroků:</span><span class="sxs-lookup"><span data-stu-id="f9802-106">Setting up this feature requires a few simple steps:</span></span>

* <span data-ttu-id="f9802-107">Zapsat trasování do vašeho kódu</span><span class="sxs-lookup"><span data-stu-id="f9802-107">Write traces in your code</span></span>
* <span data-ttu-id="f9802-108">Povolit aplikaci **diagnostické protokoly** pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f9802-108">Enable Application **Diagnostic Logs** for your app</span></span>
* <span data-ttu-id="f9802-109">Zobrazení datového proudu z integrovaného **streamování protokoly** uživatelského rozhraní v **portál Azure**.</span><span class="sxs-lookup"><span data-stu-id="f9802-109">View the stream from the built-in **Streaming Logs** UI in the **Azure portal**.</span></span>

### <a name="how-to-write-traces-in-your-code"></a><span data-ttu-id="f9802-110">Jak napsat trasování v kódu</span><span class="sxs-lookup"><span data-stu-id="f9802-110">How to write traces in your code</span></span>
<span data-ttu-id="f9802-111">Zápis trasování ve vašem kódu je snadné.</span><span class="sxs-lookup"><span data-stu-id="f9802-111">Writing traces in your code is easy.</span></span>  <span data-ttu-id="f9802-112">V jazyce C# je stejně snadná jako zápis následující kód:</span><span class="sxs-lookup"><span data-stu-id="f9802-112">In C# it's as easy as writing the following code:</span></span>

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

<span data-ttu-id="f9802-113">Třída trasování je umístěn v oboru názvů System.Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="f9802-113">The Trace class lives in the System.Diagnostics namespace.</span></span>

<span data-ttu-id="f9802-114">V aplikaci node.js můžete napsat tento kód k dosažení stejného výsledku:</span><span class="sxs-lookup"><span data-stu-id="f9802-114">In a node.js app you can write this code to achieve the same result:</span></span>

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a><span data-ttu-id="f9802-115">Povolení a zobrazení, streamování protokolů</span><span class="sxs-lookup"><span data-stu-id="f9802-115">How to enable and view the streaming logs</span></span>
<span data-ttu-id="f9802-116">![][BrowseSitesScreenshot]Diagnostika jsou povolené na základě na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f9802-116">![][BrowseSitesScreenshot] Diagnostics are enabled on a per app basis.</span></span> <span data-ttu-id="f9802-117">Spustit tak, že přejde k lokalitě, kterou chcete povolit tuto funkci na.</span><span class="sxs-lookup"><span data-stu-id="f9802-117">Start by browsing to the site you would like to enable this feature on.</span></span>  

<span data-ttu-id="f9802-118">![][DiagnosticsLogs]Z nabídky nastavení, přejděte dolů k položce **monitorování** části a klikněte na **(1) diagnostické protokoly**.</span><span class="sxs-lookup"><span data-stu-id="f9802-118">![][DiagnosticsLogs] From settings menu, scroll down to the **Monitoring** section and click on **(1) Diagnostic Logs**.</span></span> <span data-ttu-id="f9802-119">Potom **(2) povolte** **protokolování aplikace (systém souborů)** nebo **protokolování aplikace (binární rozsáhlý objekt)** **úroveň** možnost vám umožňuje změnit úroveň závažnosti trasování pro zachycení.</span><span class="sxs-lookup"><span data-stu-id="f9802-119">Then **(2) enable** **Application Logging (Filesystem)** or **Application Logging (blob)** The **Level** option lets you change the severity level of traces to capture.</span></span> <span data-ttu-id="f9802-120">Pokud jste právě se seznamte se s funkcí, nastavení úrovně na **podrobné** zajistit jsou shromažďovány všechny trasovacích příkazů.</span><span class="sxs-lookup"><span data-stu-id="f9802-120">If you're just trying to get familiar with the feature, set the level to **Verbose** to ensure all of your trace statements are collected.</span></span>

<span data-ttu-id="f9802-121">Klikněte na tlačítko **Uložit** v horní části okna a jste připraveni k zobrazení protokolů.</span><span class="sxs-lookup"><span data-stu-id="f9802-121">Click **SAVE** at the top of the blade and you're ready to view logs.</span></span>

> [!NOTE]
> <span data-ttu-id="f9802-122">Vyšší **úroveň závažnosti** do protokolu se spotřebovávají více prostředků a vytváří další trasování.</span><span class="sxs-lookup"><span data-stu-id="f9802-122">The higher the **severity level** the more resources are consumed to log and the more traces are produced.</span></span> <span data-ttu-id="f9802-123">Zajistěte, aby **úroveň závažnosti** nakonfigurovaný tak, aby správné podrobností pro produkční nebo intenzivní provoz lokality.</span><span class="sxs-lookup"><span data-stu-id="f9802-123">Make sure **severity level** is configured to the correct verbosity for a production or high traffic site.</span></span> 
> 
> 

<span data-ttu-id="f9802-124">![][StreamingLogsScreenshot]Chcete-li zobrazit **protokoly streamování** z portálu Azure, klikněte na **(1) datový proud protokolu** taky ve **monitorování** části nabídky nastavení.</span><span class="sxs-lookup"><span data-stu-id="f9802-124">![][StreamingLogsScreenshot] To view the **streaming logs** from within the Azure portal, click on **(1) Log Stream** also in the **Monitoring** section of the settings menu.</span></span> <span data-ttu-id="f9802-125">Pokud vaše aplikace je aktivně psaní příkazů trasování, pak byste měli vidět v **(2) streamování protokoly uživatelského rozhraní** skoro v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="f9802-125">If your app is actively writing trace statements, then you should see them in the **(2) streaming logs UI** in near real time.</span></span>

## <a name="console"></a><span data-ttu-id="f9802-126">Konzola</span><span class="sxs-lookup"><span data-stu-id="f9802-126">Console</span></span>
<span data-ttu-id="f9802-127">**Portál Azure** poskytuje přístup ke konzole do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f9802-127">The **Azure portal** provides console access to your app.</span></span> <span data-ttu-id="f9802-128">Vám umožní zkoumat systém souborů vaší aplikace a spouštění skriptů prostředí powershell nebo cmd.</span><span class="sxs-lookup"><span data-stu-id="f9802-128">You can explore your app's file system and run powershell/cmd scripts.</span></span> <span data-ttu-id="f9802-129">Jsou vázány stejná oprávnění nastavená jako spuštěného kódu aplikace při provádění příkazů konzoly.</span><span class="sxs-lookup"><span data-stu-id="f9802-129">You are bound by the same permissions set as your running app code when executing console commands.</span></span> <span data-ttu-id="f9802-130">Přístup k chráněné adresáře nebo spouštění skriptů, které vyžadují oprávnění vyšší úrovně je blokovaný.</span><span class="sxs-lookup"><span data-stu-id="f9802-130">Access to protected directories or running scripts that require elevated permissions is blocked.</span></span>  

<span data-ttu-id="f9802-131">![][ConsoleScreenshot]Z nabídky nastavení, přejděte dolů k položce **nástroje pro vývoj** části a klikněte na **(1) konzoly** a **(2) konzoly** uživatelského rozhraní se otevře napravo.</span><span class="sxs-lookup"><span data-stu-id="f9802-131">![][ConsoleScreenshot] From settings menu, scroll down to **Development Tools** section and click on **(1) Console** and the **(2) console** UI opens to the right.</span></span>

<span data-ttu-id="f9802-132">Abyste se seznámili s **konzoly**, zkuste základní příkazy, jako je:</span><span class="sxs-lookup"><span data-stu-id="f9802-132">To get familiar with the **console**, try basic commands like:</span></span>

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
