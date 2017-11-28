---
title: "tok hello aaaTrace v cloudových služeb aplikací s Azure Diagnostics | Microsoft Docs"
description: "Přidejte trasování zprávy tooan aplikaci Azure toohelp ladění, měření výkonu, monitorování, analýza provozu a další."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 09934772-cc07-4fd2-ba88-b224ca192f8e
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/20/2016
ms.author: robb
ms.openlocfilehash: d2ed7b5997ae1d298115b4ce593bb5051a9a0c75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="trace-hello-flow-of-a-cloud-services-application-with-azure-diagnostics"></a><span data-ttu-id="050f6-103">Sledování toku hello aplikace cloudových služeb s Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="050f6-103">Trace hello flow of a Cloud Services application with Azure Diagnostics</span></span>
<span data-ttu-id="050f6-104">Trasování je způsob, jak můžete toomonitor hello provádění aplikace, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="050f6-104">Tracing is a way for you toomonitor hello execution of your application while it is running.</span></span> <span data-ttu-id="050f6-105">Můžete použít hello [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), a [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) třídy toorecord informace o chybách a spuštění aplikace v protokolech, textové soubory nebo jiné zařízení pro pozdější analýzu.</span><span class="sxs-lookup"><span data-stu-id="050f6-105">You can use hello [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), and [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) classes toorecord information about errors and application execution in logs, text files, or other devices for later analysis.</span></span> <span data-ttu-id="050f6-106">Další informace o trasování najdete v tématu [trasování a instrumentace aplikací](https://msdn.microsoft.com/library/zs6s4h68.aspx).</span><span class="sxs-lookup"><span data-stu-id="050f6-106">For more information about tracing, see [Tracing and Instrumenting Applications](https://msdn.microsoft.com/library/zs6s4h68.aspx).</span></span>

## <a name="use-trace-statements-and-trace-switches"></a><span data-ttu-id="050f6-107">Pomocí příkazů trasování a trasování – přepínače</span><span class="sxs-lookup"><span data-stu-id="050f6-107">Use trace statements and trace switches</span></span>
<span data-ttu-id="050f6-108">Implementace trasování v aplikaci cloudové služby přidáním hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) toohello konfiguraci aplikací a provádění volá tooSystem.Diagnostics.Trace nebo System.Diagnostics.Debug ve vaší kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="050f6-108">Implement tracing in your Cloud Services application by adding hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) toohello application configuration and making calls tooSystem.Diagnostics.Trace or System.Diagnostics.Debug in your application code.</span></span> <span data-ttu-id="050f6-109">Použití hello konfigurační soubor *app.config* rolí pracovního procesu a hello *web.config* pro webové role.</span><span class="sxs-lookup"><span data-stu-id="050f6-109">Use hello configuration file *app.config* for worker roles and hello *web.config* for web roles.</span></span> <span data-ttu-id="050f6-110">Když vytvoříte novou hostovanou službu pomocí šablony sady Visual Studio, Azure Diagnostics se automaticky přidá toohello projektu a hello DiagnosticMonitorTraceListener je přidána toohello odpovídající konfigurační soubor pro hello role, které přidáte.</span><span class="sxs-lookup"><span data-stu-id="050f6-110">When you create a new hosted service using a Visual Studio template, Azure Diagnostics is automatically added toohello project and hello DiagnosticMonitorTraceListener is added toohello appropriate configuration file for hello roles that you add.</span></span>

<span data-ttu-id="050f6-111">Informace o umístění trasovacích příkazů najdete v tématu [postupy: Přidání příkazů trasování tooApplication kód](https://msdn.microsoft.com/library/zd83saa2.aspx).</span><span class="sxs-lookup"><span data-stu-id="050f6-111">For information on placing trace statements, see [How to: Add Trace Statements tooApplication Code](https://msdn.microsoft.com/library/zd83saa2.aspx).</span></span>

<span data-ttu-id="050f6-112">Tím, že umístíte [trasování – přepínače](https://msdn.microsoft.com/library/3at424ac.aspx) ve vašem kódu můžete ovládat, zda dojde k trasování a jak rozsáhlé je.</span><span class="sxs-lookup"><span data-stu-id="050f6-112">By placing [Trace Switches](https://msdn.microsoft.com/library/3at424ac.aspx) in your code, you can control whether tracing occurs and how extensive it is.</span></span> <span data-ttu-id="050f6-113">To vám umožňuje monitorovat stav hello vaší aplikace v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="050f6-113">This lets you monitor hello status of your application in a production environment.</span></span> <span data-ttu-id="050f6-114">To je obzvláště důležité v obchodní aplikace, která používá několik součástí běžící na více počítačích.</span><span class="sxs-lookup"><span data-stu-id="050f6-114">This is especially important in a business application that uses multiple components running on multiple computers.</span></span> <span data-ttu-id="050f6-115">Další informace najdete v tématu [postupy: Konfigurace trasování – přepínače](https://msdn.microsoft.com/library/t06xyy08.aspx).</span><span class="sxs-lookup"><span data-stu-id="050f6-115">For more information, see [How to: Configure Trace Switches](https://msdn.microsoft.com/library/t06xyy08.aspx).</span></span>

## <a name="configure-hello-trace-listener-in-an-azure-application"></a><span data-ttu-id="050f6-116">Konfigurace naslouchací proces trasování hello v aplikaci Azure</span><span class="sxs-lookup"><span data-stu-id="050f6-116">Configure hello trace listener in an Azure application</span></span>
<span data-ttu-id="050f6-117">Trasování ladění a TraceSource, musíte nastavit toocollect "naslouchací procesy" a záznamů hello zpráv, které jsou odeslány.</span><span class="sxs-lookup"><span data-stu-id="050f6-117">Trace, Debug and TraceSource, require you set up "listeners" toocollect and record hello messages that are sent.</span></span> <span data-ttu-id="050f6-118">Naslouchací procesy shromažďování, ukládání a směrovat trasovací zprávy.</span><span class="sxs-lookup"><span data-stu-id="050f6-118">Listeners collect, store, and route tracing messages.</span></span> <span data-ttu-id="050f6-119">Budou směrovat hello trasování výstup tooan příslušná cílová, jako je například protokol, okno nebo textového souboru.</span><span class="sxs-lookup"><span data-stu-id="050f6-119">They direct hello tracing output tooan appropriate target, such as a log, window, or text file.</span></span> <span data-ttu-id="050f6-120">Azure Diagnostics používá hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="050f6-120">Azure Diagnostics uses hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) class.</span></span>

<span data-ttu-id="050f6-121">Ještě před dokončením hello postupem je nutné inicializovat hello Azure monitorování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="050f6-121">Before you complete hello following procedure, you must initialize hello Azure diagnostic monitor.</span></span> <span data-ttu-id="050f6-122">toodo, najdete v tématu [povolení diagnostiky v Microsoft Azure](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="050f6-122">toodo this, see [Enabling Diagnostics in Microsoft Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="050f6-123">Všimněte si, že pokud používáte hello šablony, které jsou k dispozici Visual Studio, konfigurace hello hello naslouchacího procesu se automaticky přidá za vás.</span><span class="sxs-lookup"><span data-stu-id="050f6-123">Note that if you use hello templates that are provided by Visual Studio, hello configuration of hello listener is added automatically for you.</span></span>

### <a name="add-a-trace-listener"></a><span data-ttu-id="050f6-124">Přidejte naslouchací proces trasování</span><span class="sxs-lookup"><span data-stu-id="050f6-124">Add a trace listener</span></span>
1. <span data-ttu-id="050f6-125">Otevřete soubor web.config nebo app.config hello pro vaši roli.</span><span class="sxs-lookup"><span data-stu-id="050f6-125">Open hello web.config or app.config file for your role.</span></span>
2. <span data-ttu-id="050f6-126">Přidejte následující kód toohello soubor hello.</span><span class="sxs-lookup"><span data-stu-id="050f6-126">Add hello following code toohello file.</span></span> <span data-ttu-id="050f6-127">Změňte hello atribut toouse hello verze číslo verze sestavení hello, na které je odkazováno na.</span><span class="sxs-lookup"><span data-stu-id="050f6-127">Change hello Version attribute toouse hello version number of hello assembly you are referencing.</span></span> <span data-ttu-id="050f6-128">verze sestavení Hello nezmění nutně při každém vydání sady Azure SDK Pokud tooit aktualizace.</span><span class="sxs-lookup"><span data-stu-id="050f6-128">hello assembly version does not necessarily change with each Azure SDK release unless there are updates tooit.</span></span>
   
    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                    <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
   > [!IMPORTANT]
   > <span data-ttu-id="050f6-129">Ujistěte se, že máte projektu odkaz toohello Microsoft.WindowsAzure.Diagnostics sestavení.</span><span class="sxs-lookup"><span data-stu-id="050f6-129">Make sure you have a project reference toohello Microsoft.WindowsAzure.Diagnostics assembly.</span></span> <span data-ttu-id="050f6-130">Číslo verze aktualizace hello v xml hello výše toomatch hello verzi hello odkazovat Microsoft.WindowsAzure.Diagnostics sestavení.</span><span class="sxs-lookup"><span data-stu-id="050f6-130">Update hello version number in hello xml above toomatch hello version of hello referenced Microsoft.WindowsAzure.Diagnostics assembly.</span></span>
   > 
   > 
3. <span data-ttu-id="050f6-131">Uložte soubor konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="050f6-131">Save hello config file.</span></span>

<span data-ttu-id="050f6-132">Další informace o naslouchací procesy najdete v tématu [trasování – moduly naslouchání](https://msdn.microsoft.com/library/4y5y10s7.aspx).</span><span class="sxs-lookup"><span data-stu-id="050f6-132">For more information about listeners, see [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx).</span></span>

<span data-ttu-id="050f6-133">Po dokončení hello kroky tooadd hello naslouchací proces, můžete přidat kód tooyour příkazy trasování.</span><span class="sxs-lookup"><span data-stu-id="050f6-133">After you complete hello steps tooadd hello listener, you can add trace statements tooyour code.</span></span>

### <a name="tooadd-trace-statement-tooyour-code"></a><span data-ttu-id="050f6-134">kód pro příkaz tooyour tooadd trasování</span><span class="sxs-lookup"><span data-stu-id="050f6-134">tooadd trace statement tooyour code</span></span>
1. <span data-ttu-id="050f6-135">Otevřete zdrojový soubor pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="050f6-135">Open a source file for your application.</span></span> <span data-ttu-id="050f6-136">Například hello <RoleName>soubor cs pro roli pracovního procesu hello nebo webové role.</span><span class="sxs-lookup"><span data-stu-id="050f6-136">For example, hello <RoleName>.cs file for hello worker role or web role.</span></span>
2. <span data-ttu-id="050f6-137">Přidejte následující hello pomocí příkazu, pokud již nebyly přidané:</span><span class="sxs-lookup"><span data-stu-id="050f6-137">Add hello following using statement if it has not already been added:</span></span>
    ```
        using System.Diagnostics;
    ```
3. <span data-ttu-id="050f6-138">Přidání příkazů trasování, kam chcete toocapture informace o stavu hello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="050f6-138">Add Trace statements where you want toocapture information about hello state of your application.</span></span> <span data-ttu-id="050f6-139">Můžete použít různé metody tooformat hello výstup hello příkaz trasování.</span><span class="sxs-lookup"><span data-stu-id="050f6-139">You can use a variety of methods tooformat hello output of hello Trace statement.</span></span> <span data-ttu-id="050f6-140">Další informace najdete v tématu [postupy: Přidání příkazů trasování tooApplication kód](https://msdn.microsoft.com/library/zd83saa2.aspx).</span><span class="sxs-lookup"><span data-stu-id="050f6-140">For more information, see [How to: Add Trace Statements tooApplication Code](https://msdn.microsoft.com/library/zd83saa2.aspx).</span></span>
4. <span data-ttu-id="050f6-141">Uložte hello zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="050f6-141">Save hello source file.</span></span>

