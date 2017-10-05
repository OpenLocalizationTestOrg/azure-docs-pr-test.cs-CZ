---
title: "Sledování toku v aplikaci služby cloudu s Azure Diagnostics | Microsoft Docs"
description: "Přidání trasování zprávy do Azure aplikace usnadňuje ladění, měření výkonu, monitorování, analýza provozu a další."
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
ms.openlocfilehash: 35b4a4270846c54a1ca760e803ef7adba60cf03b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a><span data-ttu-id="00ce6-103">Trasování toku aplikace cloudových služeb s Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="00ce6-103">Trace the flow of a Cloud Services application with Azure Diagnostics</span></span>
<span data-ttu-id="00ce6-104">Trasování je způsob, jak můžete monitorovat aplikace, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="00ce6-104">Tracing is a way for you to monitor the execution of your application while it is running.</span></span> <span data-ttu-id="00ce6-105">Můžete použít [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), a [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) třídy k zaznamenání informací o chybách a spuštění aplikace v protokolech, textové soubory nebo jiné zařízení pro pozdější analýzu.</span><span class="sxs-lookup"><span data-stu-id="00ce6-105">You can use the [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), and [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) classes to record information about errors and application execution in logs, text files, or other devices for later analysis.</span></span> <span data-ttu-id="00ce6-106">Další informace o trasování najdete v tématu [trasování a instrumentace aplikací](https://msdn.microsoft.com/library/zs6s4h68.aspx).</span><span class="sxs-lookup"><span data-stu-id="00ce6-106">For more information about tracing, see [Tracing and Instrumenting Applications](https://msdn.microsoft.com/library/zs6s4h68.aspx).</span></span>

## <a name="use-trace-statements-and-trace-switches"></a><span data-ttu-id="00ce6-107">Pomocí příkazů trasování a trasování – přepínače</span><span class="sxs-lookup"><span data-stu-id="00ce6-107">Use trace statements and trace switches</span></span>
<span data-ttu-id="00ce6-108">Implementace trasování v aplikaci cloudové služby tak, že přidáte [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) konfiguraci aplikace a volání System.Diagnostics.Trace nebo System.Diagnostics.Debug v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="00ce6-108">Implement tracing in your Cloud Services application by adding the [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) to the application configuration and making calls to System.Diagnostics.Trace or System.Diagnostics.Debug in your application code.</span></span> <span data-ttu-id="00ce6-109">Použití konfiguračního souboru *app.config* pro role pracovního procesu a *web.config* pro webové role.</span><span class="sxs-lookup"><span data-stu-id="00ce6-109">Use the configuration file *app.config* for worker roles and the *web.config* for web roles.</span></span> <span data-ttu-id="00ce6-110">Když vytvoříte novou hostovanou službu pomocí šablony sady Visual Studio, Azure Diagnostics se automaticky přidá do projektu a DiagnosticMonitorTraceListener je přidán do příslušné konfiguračního souboru pro role, které přidáte.</span><span class="sxs-lookup"><span data-stu-id="00ce6-110">When you create a new hosted service using a Visual Studio template, Azure Diagnostics is automatically added to the project and the DiagnosticMonitorTraceListener is added to the appropriate configuration file for the roles that you add.</span></span>

<span data-ttu-id="00ce6-111">Informace o umístění trasovacích příkazů najdete v tématu [postupy: Přidání příkazů trasování do kódu aplikace](https://msdn.microsoft.com/library/zd83saa2.aspx).</span><span class="sxs-lookup"><span data-stu-id="00ce6-111">For information on placing trace statements, see [How to: Add Trace Statements to Application Code](https://msdn.microsoft.com/library/zd83saa2.aspx).</span></span>

<span data-ttu-id="00ce6-112">Tím, že umístíte [trasování – přepínače](https://msdn.microsoft.com/library/3at424ac.aspx) ve vašem kódu můžete ovládat, zda dojde k trasování a jak rozsáhlé je.</span><span class="sxs-lookup"><span data-stu-id="00ce6-112">By placing [Trace Switches](https://msdn.microsoft.com/library/3at424ac.aspx) in your code, you can control whether tracing occurs and how extensive it is.</span></span> <span data-ttu-id="00ce6-113">To vám umožňuje monitorovat stav aplikace v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="00ce6-113">This lets you monitor the status of your application in a production environment.</span></span> <span data-ttu-id="00ce6-114">To je obzvláště důležité v obchodní aplikace, která používá několik součástí běžící na více počítačích.</span><span class="sxs-lookup"><span data-stu-id="00ce6-114">This is especially important in a business application that uses multiple components running on multiple computers.</span></span> <span data-ttu-id="00ce6-115">Další informace najdete v tématu [postupy: Konfigurace trasování – přepínače](https://msdn.microsoft.com/library/t06xyy08.aspx).</span><span class="sxs-lookup"><span data-stu-id="00ce6-115">For more information, see [How to: Configure Trace Switches](https://msdn.microsoft.com/library/t06xyy08.aspx).</span></span>

## <a name="configure-the-trace-listener-in-an-azure-application"></a><span data-ttu-id="00ce6-116">Konfigurace trasování modulu naslouchání v aplikaci Azure</span><span class="sxs-lookup"><span data-stu-id="00ce6-116">Configure the trace listener in an Azure application</span></span>
<span data-ttu-id="00ce6-117">Trasování ladění a TraceSource, musíte nastavit "naslouchací procesy" ke sběru a zaznamenejte zprávy, které se odesílají.</span><span class="sxs-lookup"><span data-stu-id="00ce6-117">Trace, Debug and TraceSource, require you set up "listeners" to collect and record the messages that are sent.</span></span> <span data-ttu-id="00ce6-118">Naslouchací procesy shromažďování, ukládání a směrovat trasovací zprávy.</span><span class="sxs-lookup"><span data-stu-id="00ce6-118">Listeners collect, store, and route tracing messages.</span></span> <span data-ttu-id="00ce6-119">Jejich přesměrujte výstup trasování do příslušné cílové, jako je například protokol, okno nebo textového souboru.</span><span class="sxs-lookup"><span data-stu-id="00ce6-119">They direct the tracing output to an appropriate target, such as a log, window, or text file.</span></span> <span data-ttu-id="00ce6-120">Používá Azure Diagnostics [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="00ce6-120">Azure Diagnostics uses the [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) class.</span></span>

<span data-ttu-id="00ce6-121">Před provedením následujícího postupu, je nutné inicializovat Azure monitorování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="00ce6-121">Before you complete the following procedure, you must initialize the Azure diagnostic monitor.</span></span> <span data-ttu-id="00ce6-122">Chcete-li to provést, přečtěte si téma [povolení diagnostiky v Microsoft Azure](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="00ce6-122">To do this, see [Enabling Diagnostics in Microsoft Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="00ce6-123">Všimněte si, že pokud používáte šablony, které jsou k dispozici Visual Studio, konfiguraci naslouchacího procesu se automaticky přidá za vás.</span><span class="sxs-lookup"><span data-stu-id="00ce6-123">Note that if you use the templates that are provided by Visual Studio, the configuration of the listener is added automatically for you.</span></span>

### <a name="add-a-trace-listener"></a><span data-ttu-id="00ce6-124">Přidejte naslouchací proces trasování</span><span class="sxs-lookup"><span data-stu-id="00ce6-124">Add a trace listener</span></span>
1. <span data-ttu-id="00ce6-125">Otevřete soubor web.config nebo app.config pro vaši roli.</span><span class="sxs-lookup"><span data-stu-id="00ce6-125">Open the web.config or app.config file for your role.</span></span>
2. <span data-ttu-id="00ce6-126">Přidejte následující kód do souboru.</span><span class="sxs-lookup"><span data-stu-id="00ce6-126">Add the following code to the file.</span></span> <span data-ttu-id="00ce6-127">Změňte atribut verze používat číslo verze sestavení, ve kterém je odkazováno na.</span><span class="sxs-lookup"><span data-stu-id="00ce6-127">Change the Version attribute to use the version number of the assembly you are referencing.</span></span> <span data-ttu-id="00ce6-128">Verze sestavení nezmění nutně při každém vydání sady Azure SDK Pokud existují aktualizace.</span><span class="sxs-lookup"><span data-stu-id="00ce6-128">The assembly version does not necessarily change with each Azure SDK release unless there are updates to it.</span></span>
   
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
   > <span data-ttu-id="00ce6-129">Ujistěte se, že máte projektu odkaz na sestavení Microsoft.WindowsAzure.Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="00ce6-129">Make sure you have a project reference to the Microsoft.WindowsAzure.Diagnostics assembly.</span></span> <span data-ttu-id="00ce6-130">Aktualizujte číslo verze ve výše uvedené xml tak, aby odpovídala verzi odkazované sestavení Microsoft.WindowsAzure.Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="00ce6-130">Update the version number in the xml above to match the version of the referenced Microsoft.WindowsAzure.Diagnostics assembly.</span></span>
   > 
   > 
3. <span data-ttu-id="00ce6-131">Uložte do konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="00ce6-131">Save the config file.</span></span>

<span data-ttu-id="00ce6-132">Další informace o naslouchací procesy najdete v tématu [trasování – moduly naslouchání](https://msdn.microsoft.com/library/4y5y10s7.aspx).</span><span class="sxs-lookup"><span data-stu-id="00ce6-132">For more information about listeners, see [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx).</span></span>

<span data-ttu-id="00ce6-133">Po dokončení kroků pro přidání naslouchací proces, je přidání příkazů trasování do kódu.</span><span class="sxs-lookup"><span data-stu-id="00ce6-133">After you complete the steps to add the listener, you can add trace statements to your code.</span></span>

### <a name="to-add-trace-statement-to-your-code"></a><span data-ttu-id="00ce6-134">Chcete-li přidat příkaz trasování do kódu</span><span class="sxs-lookup"><span data-stu-id="00ce6-134">To add trace statement to your code</span></span>
1. <span data-ttu-id="00ce6-135">Otevřete zdrojový soubor pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="00ce6-135">Open a source file for your application.</span></span> <span data-ttu-id="00ce6-136">Například <RoleName>.cs soubor pro roli pracovního procesu, nebo webové role.</span><span class="sxs-lookup"><span data-stu-id="00ce6-136">For example, the <RoleName>.cs file for the worker role or web role.</span></span>
2. <span data-ttu-id="00ce6-137">Přidejte následující pomocí příkazu, pokud již nebyly přidané:</span><span class="sxs-lookup"><span data-stu-id="00ce6-137">Add the following using statement if it has not already been added:</span></span>
    ```
        using System.Diagnostics;
    ```
3. <span data-ttu-id="00ce6-138">Přidání příkazů trasování, kde chcete zaznamenat informace o stavu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="00ce6-138">Add Trace statements where you want to capture information about the state of your application.</span></span> <span data-ttu-id="00ce6-139">K formátování výstupu příkazu trasování můžete použít různé metody.</span><span class="sxs-lookup"><span data-stu-id="00ce6-139">You can use a variety of methods to format the output of the Trace statement.</span></span> <span data-ttu-id="00ce6-140">Další informace najdete v tématu [postupy: Přidání příkazů trasování do kódu aplikace](https://msdn.microsoft.com/library/zd83saa2.aspx).</span><span class="sxs-lookup"><span data-stu-id="00ce6-140">For more information, see [How to: Add Trace Statements to Application Code](https://msdn.microsoft.com/library/zd83saa2.aspx).</span></span>
4. <span data-ttu-id="00ce6-141">Zdrojový soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="00ce6-141">Save the source file.</span></span>

