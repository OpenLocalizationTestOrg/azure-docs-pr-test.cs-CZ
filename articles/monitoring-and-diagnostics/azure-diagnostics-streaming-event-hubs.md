---
title: "aaaStreaming dat diagnostiky Azure v hello aktivní trase pomocí služby Event Hubs | Microsoft Docs"
description: "Konfigurace Azure Diagnostics službou Event Hubs ukončení tooend, včetně pokyny pro běžné scénáře."
services: event-hubs
documentationcenter: na
author: rboucher
manager: carmonm
editor: 
ms.assetid: edeebaac-1c47-4b43-9687-f28e7e1e446a
ms.service: monitoring-and-diagnostics
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: robb
ms.openlocfilehash: a2528ddd0688d1c23a8631e769ca016dd79e4159
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-azure-diagnostics-data-in-hello-hot-path-by-using-event-hubs"></a><span data-ttu-id="5ab1b-103">Azure Diagnostics dat v hello aktivní trase pomocí služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5ab1b-103">Streaming Azure Diagnostics data in hello hot path by using Event Hubs</span></span>
<span data-ttu-id="5ab1b-104">Azure Diagnostics poskytuje flexibilní způsoby toocollect metriky a protokoly z cloudové služby virtuálních počítačů (VM) a přenos výsledky tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-104">Azure Diagnostics provides flexible ways toocollect metrics and logs from cloud services virtual machines (VMs) and transfer results tooAzure Storage.</span></span> <span data-ttu-id="5ab1b-105">Počínaje hello března 2016 (SDK 2.9) časový rámec, můžete odesílat diagnostiky toocustom zdroje dat a přenos dat aktivní trase v sekundách pomocí [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="5ab1b-105">Starting in hello March 2016 (SDK 2.9) time frame, you can send Diagnostics toocustom data sources and transfer hot path data in seconds by using [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).</span></span>

<span data-ttu-id="5ab1b-106">Podporované datové typy patří:</span><span class="sxs-lookup"><span data-stu-id="5ab1b-106">Supported data types include:</span></span>

* <span data-ttu-id="5ab1b-107">Události Trasování událostí pro Windows</span><span class="sxs-lookup"><span data-stu-id="5ab1b-107">Event Tracing for Windows (ETW) events</span></span>
* <span data-ttu-id="5ab1b-108">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="5ab1b-108">Performance counters</span></span>
* <span data-ttu-id="5ab1b-109">Protokoly událostí systému Windows</span><span class="sxs-lookup"><span data-stu-id="5ab1b-109">Windows event logs</span></span>
* <span data-ttu-id="5ab1b-110">Protokoly aplikací</span><span class="sxs-lookup"><span data-stu-id="5ab1b-110">Application logs</span></span>
* <span data-ttu-id="5ab1b-111">Protokoly infrastruktury Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="5ab1b-111">Azure Diagnostics infrastructure logs</span></span>

<span data-ttu-id="5ab1b-112">Tento článek ukazuje, jak Azure Diagnostics tooconfigure službou Event Hubs z ukončení tooend.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-112">This article shows you how tooconfigure Azure Diagnostics with Event Hubs from end tooend.</span></span> <span data-ttu-id="5ab1b-113">Příručka je taky pokyny pro následující běžné scénáře hello:</span><span class="sxs-lookup"><span data-stu-id="5ab1b-113">Guidance is also provided for hello following common scenarios:</span></span>

* <span data-ttu-id="5ab1b-114">Jak toocustomize hello protokoly a metriky, získat odeslaný tooEvent rozbočovače</span><span class="sxs-lookup"><span data-stu-id="5ab1b-114">How toocustomize hello logs and metrics that get sent tooEvent Hubs</span></span>
* <span data-ttu-id="5ab1b-115">Jak toochange konfigurace v každé prostředí</span><span class="sxs-lookup"><span data-stu-id="5ab1b-115">How toochange configurations in each environment</span></span>
* <span data-ttu-id="5ab1b-116">Jak tooview Event Hubs Streamovat data</span><span class="sxs-lookup"><span data-stu-id="5ab1b-116">How tooview Event Hubs stream data</span></span>
* <span data-ttu-id="5ab1b-117">Jak tootroubleshoot hello připojení</span><span class="sxs-lookup"><span data-stu-id="5ab1b-117">How tootroubleshoot hello connection</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="5ab1b-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5ab1b-118">Prerequisites</span></span>
<span data-ttu-id="5ab1b-119">Event Hubs receieving dat z Azure Diagnostics je podporována v cloudové služby, virtuální počítače, sady škálování virtuálního počítače a počínaje hello sadu Azure SDK 2.9 a hello odpovídající nástroje Azure pro sadu Visual Studio Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-119">Event Hubs receieving data from Azure Diagnostics is supported in Cloud Services, VMs, Virtual Machine Scale Sets, and Service Fabric starting in hello Azure SDK 2.9 and hello corresponding Azure Tools for Visual Studio.</span></span>

* <span data-ttu-id="5ab1b-120">Rozšíření diagnostiky Azure 1.6 ([Azure SDK pro .NET 2.9 nebo novější](https://azure.microsoft.com/downloads/) cílem to ve výchozím nastavení)</span><span class="sxs-lookup"><span data-stu-id="5ab1b-120">Azure Diagnostics extension 1.6 ([Azure SDK for .NET 2.9 or later](https://azure.microsoft.com/downloads/) targets this by default)</span></span>
* [<span data-ttu-id="5ab1b-121">Visual Studio 2013 nebo novější</span><span class="sxs-lookup"><span data-stu-id="5ab1b-121">Visual Studio 2013 or later</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* <span data-ttu-id="5ab1b-122">Stávající konfigurace Azure Diagnostics v aplikaci pomocí *.wadcfgx* souboru a jeden z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="5ab1b-122">Existing configurations of Azure Diagnostics in an application by using a *.wadcfgx* file and one of hello following methods:</span></span>
  * <span data-ttu-id="5ab1b-123">Visual Studio: [konfigurace diagnostiky pro cloudové služby Azure a virtuální počítače](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)</span><span class="sxs-lookup"><span data-stu-id="5ab1b-123">Visual Studio: [Configuring Diagnostics for Azure Cloud Services and Virtual Machines](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)</span></span>
  * <span data-ttu-id="5ab1b-124">Prostředí Windows PowerShell: [zapněte diagnostiku ve službě Azure Cloud Services pomocí prostředí PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="5ab1b-124">Windows PowerShell: [Enable diagnostics in Azure Cloud Services using PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)</span></span>
* <span data-ttu-id="5ab1b-125">Obor názvů centra událostí zřídit na článek hello [Začínáme se službou Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)</span><span class="sxs-lookup"><span data-stu-id="5ab1b-125">Event Hubs namespace provisioned per hello article, [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)</span></span>

## <a name="connect-azure-diagnostics-tooevent-hubs-sink"></a><span data-ttu-id="5ab1b-126">Připojení Azure Diagnostics tooEvent centra jímka</span><span class="sxs-lookup"><span data-stu-id="5ab1b-126">Connect Azure Diagnostics tooEvent Hubs sink</span></span>
<span data-ttu-id="5ab1b-127">Ve výchozím nastavení odešle Azure Diagnostics vždy protokoly a metriky tooan účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-127">By default, Azure Diagnostics always sends logs and metrics tooan Azure Storage account.</span></span> <span data-ttu-id="5ab1b-128">Aplikace může také odeslat data tooEvent centra přidáním nové **jímky** oddílu v části hello **PublicConfig** / **WadCfg** element hello *.wadcfgx* souboru.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-128">An application may also send data tooEvent Hubs by adding a new **Sinks** section under hello **PublicConfig** / **WadCfg** element of hello *.wadcfgx* file.</span></span> <span data-ttu-id="5ab1b-129">V sadě Visual Studio, hello *.wadcfgx* soubor je uložen v hello následující cestu: **projekt cloudové služby** > **role** > **() RoleName)** > **diagnostics.wadcfgx** souboru.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-129">In Visual Studio, hello *.wadcfgx* file is stored in hello following path: **Cloud Service Project** > **Roles** > **(RoleName)** > **diagnostics.wadcfgx** file.</span></span>

```xml
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "HotPath",
            "EventHub": {
                "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
                "SharedAccessKeyName": "SendRule"
            }
        }
    ]
}
```

<span data-ttu-id="5ab1b-130">V tomto příkladu centra událostí hello nastavení adresy URL toohello plně kvalifikovaných názvů centra událostí hello: obor názvů služby Event Hubs + "/" + název centra událostí.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-130">In this example, hello event hub URL is set toohello fully qualified namespace of hello event hub: Event Hubs namespace  + "/" + event hub name.</span></span>  

<span data-ttu-id="5ab1b-131">Adresa URL se zobrazí v hello centra událostí Hello [portál Azure](http://go.microsoft.com/fwlink/?LinkID=213885) na řídicím panelu služby Event Hubs hello.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-131">hello event hub URL is displayed in hello [Azure portal](http://go.microsoft.com/fwlink/?LinkID=213885) on hello Event Hubs dashboard.</span></span>  

<span data-ttu-id="5ab1b-132">Hello **jímky** název lze nastavit tooany platný řetězec, dokud hello stejnou hodnotu se používá konzistentně napříč hello konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-132">hello **Sink** name can be set tooany valid string as long as hello same value is used consistently throughout hello config file.</span></span>

> [!NOTE]
> <span data-ttu-id="5ab1b-133">Mohou existovat další jímky, jako například *applicationInsights* nakonfigurované v této části.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-133">There may be additional sinks, such as *applicationInsights* configured in this section.</span></span> <span data-ttu-id="5ab1b-134">Azure Diagnostics umožňuje jeden nebo více jímky toobe definována, pokud se v hello je také deklarováno každý podřízený **PrivateConfig** části.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-134">Azure Diagnostics allows one or more sinks toobe defined if each sink is also declared in hello **PrivateConfig** section.</span></span>  
>
>

<span data-ttu-id="5ab1b-135">Hello Event Hubs podřízený musí také být deklarován a definované v hello **PrivateConfig** části hello *.wadcfgx* konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-135">hello Event Hubs sink must also be declared and defined in hello **PrivateConfig** section of hello *.wadcfgx* config file.</span></span>

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="{account name}" key="{account key}" endpoint="{optional storage endpoint}" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
</PrivateConfig>
```
```JSON
{
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{optional storage endpoint}",
    "EventHub": {
        "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    }
}
```

<span data-ttu-id="5ab1b-136">Hello `SharedAccessKeyName` hodnota musí odpovídat klíč sdíleného přístupového podpisu (SAS) a zásad, která byla definována v hello **Event Hubs** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-136">hello `SharedAccessKeyName` value must match a Shared Access Signature (SAS) key and policy that has been defined in hello **Event Hubs** namespace.</span></span> <span data-ttu-id="5ab1b-137">Procházet toohello Event Hubs řídicího panelu hello [portál Azure](https://manage.windowsazure.com), klikněte na tlačítko hello **konfigurace** kartě a nastavte s názvem zásady (například "SendRule"), který má *odeslat* oprávnění.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-137">Browse toohello Event Hubs dashboard in hello [Azure portal](https://manage.windowsazure.com), click hello **Configure** tab, and set up a named policy (for example, "SendRule") that has *Send* permissions.</span></span> <span data-ttu-id="5ab1b-138">Hello **StorageAccount** také deklarovaného v **PrivateConfig**.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-138">hello **StorageAccount** is also declared in **PrivateConfig**.</span></span> <span data-ttu-id="5ab1b-139">Není nutné toochange hodnoty zde Pokud pracují.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-139">There is no need toochange values here if they are working.</span></span> <span data-ttu-id="5ab1b-140">V tomto příkladu jsme ponechte hello hodnoty prázdná, což je znaménkem podřízené asset nastaví hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-140">In this example, we leave hello values empty, which is a sign that a downstream asset will set hello values.</span></span> <span data-ttu-id="5ab1b-141">Například hello *ServiceConfiguration.Cloud.cscfg* prostředí konfigurační soubor nastaví hello příslušné prostředí názvů a klíče.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-141">For example, hello *ServiceConfiguration.Cloud.cscfg* environment configuration file sets hello environment-appropriate names and keys.</span></span>  

> [!WARNING]
> <span data-ttu-id="5ab1b-142">klíč SAS centra událostí Hello je uložen v prostém textu v hello *.wadcfgx* souboru.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-142">hello Event Hubs SAS key is stored in plain text in hello *.wadcfgx* file.</span></span> <span data-ttu-id="5ab1b-143">Často tento klíč se změnami kódu toosource nebo není k dispozici jako prostředek na vašem serveru sestavení, takže byste měli chránit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-143">Often, this key is checked in toosource code control or is available as an asset in your build server, so you should protect it as appropriate.</span></span> <span data-ttu-id="5ab1b-144">Doporučujeme používat klíč SAS se zde *odesílat pouze* oprávnění tak, aby uživatel se zlými úmysly nelze zapsat toohello centra událostí, ale naslouchání tooit nebo k její správě.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-144">We recommend that you use a SAS key here with *Send only* permissions so that a malicious user can write toohello event hub, but not listen tooit or manage it.</span></span>
>
>

## <a name="configure-azure-diagnostics-toosend-logs-and-metrics-tooevent-hubs"></a><span data-ttu-id="5ab1b-145">Konfigurace Azure Diagnostics toosend protokoly a metriky tooEvent rozbočovače</span><span class="sxs-lookup"><span data-stu-id="5ab1b-145">Configure Azure Diagnostics toosend logs and metrics tooEvent Hubs</span></span>
<span data-ttu-id="5ab1b-146">Jak je popsáno dříve, všechny výchozí a vlastní diagnostická data, to znamená, metriky a protokoly, je automaticky odeslán tooAzure úložiště v intervalech hello nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-146">As discussed previously, all default and custom diagnostics data, that is, metrics and logs, is automatically sent tooAzure Storage in hello configured intervals.</span></span> <span data-ttu-id="5ab1b-147">Event Hubs a všechny další podřízený můžete zadat libovolný uzel kořenovou nebo listu v toobe hierarchie hello odeslané toohello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-147">With Event Hubs and any additional sink, you can specify any root or leaf node in hello hierarchy toobe sent toohello event hub.</span></span> <span data-ttu-id="5ab1b-148">To zahrnuje události trasování událostí, čítače výkonu, protokoly událostí systému Windows a protokolů aplikace.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-148">This includes ETW events, performance counters, Windows event logs, and application logs.</span></span>   

<span data-ttu-id="5ab1b-149">Je důležité tooconsider, kolik datových bodů ve skutečnosti by měla být přenesena tooEvent rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-149">It is important tooconsider how many data points should actually be transferred tooEvent Hubs.</span></span> <span data-ttu-id="5ab1b-150">Vývojáři obvykle přenášet data za provozu cesty s nízkou latencí, která musí využívat a rychle interpretovat.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-150">Typically, developers transfer low-latency hot-path data that must be consumed and interpreted quickly.</span></span> <span data-ttu-id="5ab1b-151">Systémy, které monitorují výstrahy nebo pravidel automatického škálování jsou příklady.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-151">Systems that monitor alerts or autoscale rules are examples.</span></span> <span data-ttu-id="5ab1b-152">Vývojář může také nakonfigurovat úložišti alternativní analýzy nebo hledání úložiště – například Azure Stream Analytics, Elasticsearch, vlastní monitorování systému nebo oblíbených monitorování systému od jiných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-152">A developer might also configure an alternate analysis store or search store -- for example, Azure Stream Analytics, Elasticsearch, a custom monitoring system, or a favorite monitoring system from others.</span></span>

<span data-ttu-id="5ab1b-153">Hello Následují některé příklad konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-153">hello following are some example configurations.</span></span>

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "sinks": "HotPath",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        }
    ]
}
```

<span data-ttu-id="5ab1b-154">V hello výše příklad, je podřízený hello nadřazené použité toohello **čítače výkonu** uzlu v hierarchii hello, takže všechny podřízené **čítače výkonu** odešle tooEvent rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-154">In hello above example, hello sink is applied toohello parent **PerformanceCounters** node in hello hierarchy, which means all child **PerformanceCounters** will be sent tooEvent Hubs.</span></span>  

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Rejected",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Queued",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        }
    ]
}
```

<span data-ttu-id="5ab1b-155">V předchozím příkladu hello podřízený hello je použité tooonly tři čítače: **požadavky ve frontě**, **požadavky zamítnuty**, a **% času procesoru**.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-155">In hello previous example, hello sink is applied tooonly three counters: **Requests Queued**, **Requests Rejected**, and **% Processor time**.</span></span>  

<span data-ttu-id="5ab1b-156">Hello následující příklad ukazuje, jak může vývojář omezit hello množství odeslaná data toobe hello kritické metriky, které se používají pro tuto službu stavu.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-156">hello following example shows how a developer can limit hello amount of sent data toobe hello critical metrics that are used for this service’s health.</span></span>  

```XML
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```
```JSON
"Logs": {
    "scheduledTransferPeriod": "PT1M",
    "scheduledTransferLogLevelFilter": "Error",
    "sinks": "HotPath"
}
```

<span data-ttu-id="5ab1b-157">V tomto příkladu podřízený hello je použité toologs a je filtrovaná pouze tooerror úrovně trasování.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-157">In this example, hello sink is applied toologs and is filtered only tooerror level trace.</span></span>

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a><span data-ttu-id="5ab1b-158">Nasazení a aktualizace konfigurace aplikace a Diagnostika cloudové služby</span><span class="sxs-lookup"><span data-stu-id="5ab1b-158">Deploy and update a Cloud Services application and diagnostics config</span></span>
<span data-ttu-id="5ab1b-159">Visual Studio poskytuje hello nejjednodušší cesta toodeploy hello aplikace a služby Event Hubs podřízený konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-159">Visual Studio provides hello easiest path toodeploy hello application and Event Hubs sink configuration.</span></span> <span data-ttu-id="5ab1b-160">tooview a úpravy hello souboru, otevřete hello *.wadcfgx* souborů v sadě Visual Studio, upravovat a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-160">tooview and edit hello file, open hello *.wadcfgx* file in Visual Studio, edit it, and save it.</span></span> <span data-ttu-id="5ab1b-161">Cesta Hello je **projekt cloudové služby** > **role** > **(RoleName)** > **diagnostics.wadcfgx**.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-161">hello path is **Cloud Service Project** > **Roles** > **(RoleName)** > **diagnostics.wadcfgx**.</span></span>  

<span data-ttu-id="5ab1b-162">V tomto okamžiku všechny nasazení a nasazení aktualizací akcí v sadě Visual Studio, Visual Studio Team System a všechny příkazy nebo skripty, které jsou založeny na MSBuild a používat hello **/t: publikování** cíl zahrnují hello *.wadcfgx*  v procesu balení hello.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-162">At this point, all deployment and deployment update actions in Visual Studio, Visual Studio Team System, and all commands or scripts that are based on MSBuild and use hello **/t:publish** target include hello *.wadcfgx* in hello packaging process.</span></span> <span data-ttu-id="5ab1b-163">Kromě toho nasazení a aktualizace nasadit hello souboru tooAzure pomocí hello odpovídající Azure Diagnostics agenta rozšíření na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-163">In addition, deployments and updates deploy hello file tooAzure by using hello appropriate Azure Diagnostics agent extension on your VMs.</span></span>

<span data-ttu-id="5ab1b-164">Po nasazení aplikace hello a konfigurace Azure Diagnostics, zobrazí se okamžitě aktivity na řídicím panelu hello hello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-164">After you deploy hello application and Azure Diagnostics configuration, you will immediately see activity in hello dashboard of hello event hub.</span></span> <span data-ttu-id="5ab1b-165">To znamená, že jste připravené toomove na tooviewing hello horkou cesta dat v nástroji klienta nebo analysis naslouchací proces hello podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-165">This indicates that you're ready toomove on tooviewing hello hot-path data in hello listener client or analysis tool of your choice.</span></span>  

<span data-ttu-id="5ab1b-166">V hello následující obrázek hello Event Hubs řídicího panelu ukazuje pořádku odesílání diagnostiky dat toohello události rozbočovače spuštění nějakou dobu zopakovat po 23: 00.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-166">In hello following figure, hello Event Hubs dashboard shows healthy sending of diagnostics data toohello event hub starting sometime after 11 PM.</span></span> <span data-ttu-id="5ab1b-167">Kdy je nasazená aplikace hello s aktualizované *.wadcfgx* souborů a hello podřízený byla nakonfigurována správně.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-167">That's when hello application was deployed with an updated *.wadcfgx* file, and hello sink was configured properly.</span></span>

![][0]  

> [!NOTE]
> <span data-ttu-id="5ab1b-168">Pokud provedete aktualizace toohello Azure Diagnostics konfigurační soubor (.wadcfgx), se doporučuje push hello aktualizace toohello celá aplikace, jakož i konfigurace hello pomocí sady Visual Studio publikování nebo skript prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-168">When you make updates toohello Azure Diagnostics config file (.wadcfgx), it's recommended that you push hello updates toohello entire application as well as hello configuration by using either Visual Studio publishing, or a Windows PowerShell script.</span></span>  
>
>

## <a name="view-hot-path-data"></a><span data-ttu-id="5ab1b-169">Data za provozu cesty zobrazení</span><span class="sxs-lookup"><span data-stu-id="5ab1b-169">View hot-path data</span></span>
<span data-ttu-id="5ab1b-170">Jak je popsáno dříve, existuje mnoho případů použití pro naslouchání tooand zpracování dat služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-170">As discussed previously, there are many use cases for listening tooand processing Event Hubs data.</span></span>

<span data-ttu-id="5ab1b-171">Jeden ze způsobů jednoduché je toocreate testu malých konzole aplikace toolisten toohello centra událostí a tisku hello výstupního datového proudu.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-171">One simple approach is toocreate a small test console application toolisten toohello event hub and print hello output stream.</span></span> <span data-ttu-id="5ab1b-172">Můžete umístit hello následující kód, který je vysvětlené podrobněji v [Začínáme se službou Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), v konzolové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-172">You can place hello following code, which is explained in more detail in [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), in a console application.</span></span>  

<span data-ttu-id="5ab1b-173">Všimněte si, že hello konzolové aplikace musí obsahovat hello [balíček NuGet hostitele procesor událostí](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span><span class="sxs-lookup"><span data-stu-id="5ab1b-173">Note that hello console application must include hello [Event Processor Host NuGet package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span></span>  

<span data-ttu-id="5ab1b-174">Mějte na paměti, tooreplace hello hodnoty v lomených závorkách v hello **hlavní** funkce s hodnotami pro vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-174">Remember tooreplace hello values in angle brackets in hello **Main** function with values for your resources.</span></span>   

```csharp
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key toostop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sinks"></a><span data-ttu-id="5ab1b-175">Řešení potíží s jímky služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5ab1b-175">Troubleshoot Event Hubs sinks</span></span>
* <span data-ttu-id="5ab1b-176">centra událostí Hello nezobrazuje aktivity příchozích nebo odchozích události podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-176">hello event hub does not show incoming or outgoing event activity as expected.</span></span>

    <span data-ttu-id="5ab1b-177">Zkontrolujte, že je úspěšně zřízený Centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-177">Check that your event hub is successfully provisioned.</span></span> <span data-ttu-id="5ab1b-178">Všechny informace o připojení v hello **PrivateConfig** části *.wadcfgx* musí odpovídat hello hodnoty prostředku, jak je vidět na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-178">All connection info in hello **PrivateConfig** section of *.wadcfgx* must match hello values of your resource as seen in hello portal.</span></span> <span data-ttu-id="5ab1b-179">Ujistěte se, že máte SAS zásady definované ("SendRule" v příkladu hello) v hello portál a které *odeslat* je povoleno.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-179">Make sure that you have a SAS policy defined ("SendRule" in hello example) in hello portal and that *Send* permission is granted.</span></span>  
* <span data-ttu-id="5ab1b-180">Po aktualizaci centra událostí hello přestane zobrazovat aktivity příchozích nebo odchozích události.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-180">After an update, hello event hub no longer shows incoming or outgoing event activity.</span></span>

    <span data-ttu-id="5ab1b-181">Zkontrolujte, že hello centra událostí a informace o konfiguraci jsou správné, jak je popsáno dříve.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-181">First, make sure that hello event hub and configuration info is correct as explained previously.</span></span> <span data-ttu-id="5ab1b-182">Někdy hello **PrivateConfig** v aktualizaci nasazení se resetuje.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-182">Sometimes hello **PrivateConfig** is reset in a deployment update.</span></span> <span data-ttu-id="5ab1b-183">Hello doporučený, opravte je toomake všechny změny příliš*.wadcfgx* v hello projektu a pak poslat aktualizaci dokončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-183">hello recommended fix is toomake all changes too*.wadcfgx* in hello project and then push a complete application update.</span></span> <span data-ttu-id="5ab1b-184">Pokud to není možné, ujistěte se, že aktualizace diagnostiky hello nabízených oznámení úplná **PrivateConfig** který obsahuje klíč SAS hello.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-184">If this is not possible, make sure that hello diagnostics update pushes a complete **PrivateConfig** that includes hello SAS key.</span></span>  
* <span data-ttu-id="5ab1b-185">Byl proveden o hello návrhy a hello centra událostí, ale stále nefunguje.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-185">I tried hello suggestions, and hello event hub is still not working.</span></span>

    <span data-ttu-id="5ab1b-186">Podívejte se do v hello Azure Storage tabulku, která obsahuje chyby a protokolování Azure Diagnostics samotné: **WADDiagnosticInfrastructureLogsTable**.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-186">Try looking in hello Azure Storage table that contains logs and errors for Azure Diagnostics itself: **WADDiagnosticInfrastructureLogsTable**.</span></span> <span data-ttu-id="5ab1b-187">Jednou z možností nástroje, jako je toouse [Azure Storage Explorer](http://www.storageexplorer.com) účet úložiště toothis tooconnect, zobrazit tuto tabulku a přidat dotaz pro časové razítko v hello posledních 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-187">One option is toouse a tool such as [Azure Storage Explorer](http://www.storageexplorer.com) tooconnect toothis storage account, view this table, and add a query for TimeStamp in hello last 24 hours.</span></span> <span data-ttu-id="5ab1b-188">Můžete použít nástroj tooexport hello soubor .csv a otevřete jej v aplikaci, jako je například aplikace Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-188">You can use hello tool tooexport a .csv file and open it in an application such as Microsoft Excel.</span></span> <span data-ttu-id="5ab1b-189">Aplikace Excel umožňuje snadno toosearch pro volací karty řetězce, jako například **EventHubs**, toosee, jaké se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-189">Excel makes it easy toosearch for calling-card strings, such as **EventHubs**, toosee what error is reported.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="5ab1b-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ab1b-190">Next steps</span></span>
<span data-ttu-id="5ab1b-191">• [Další informace o službě Event Hubs](https://azure.microsoft.com/services/event-hubs/)</span><span class="sxs-lookup"><span data-stu-id="5ab1b-191">•    [Learn more about Event Hubs](https://azure.microsoft.com/services/event-hubs/)</span></span>

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a><span data-ttu-id="5ab1b-192">Dodatek: Dokončení příkladu Azure Diagnostics konfigurační soubor (.wadcfgx)</span><span class="sxs-lookup"><span data-stu-id="5ab1b-192">Appendix: Complete Azure Diagnostics configuration file (.wadcfgx) example</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount>ACCOUNT_NAME</StorageAccount>
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="{account name}" key="{account key}" endpoint="{storage endpoint}" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

<span data-ttu-id="5ab1b-193">Hello doplňkové *ServiceConfiguration.Cloud.cscfg* pro tento příklad vypadá jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="5ab1b-193">hello complementary *ServiceConfiguration.Cloud.cscfg* for this example looks like hello following.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<span data-ttu-id="5ab1b-194">Ekvivalentní Json na základě nastavení pro virtuální počítače je následující:</span><span class="sxs-lookup"><span data-stu-id="5ab1b-194">Equivalent Json based settings for virtual machines is as follows:</span></span>
```JSON
"settings": {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 4096,
            "sinks": "applicationInsights.errors",
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "Directories": {
                "scheduledTransferPeriod": "PT1M",
                "IISLogs": {
                    "containerName": "wad-iis-logfiles"
                },
                "FailedRequestLogs": {
                    "containerName": "wad-failedrequestlogs"
                }
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "sinks": "HotPath",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Memory\\Available MBytes",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\Bytes Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Queued",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Rejected",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT3M"
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT1M",
                "DataSource": [
                    {
                        "name": "Application!*"
                    }
                ]
            },
            "Logs": {
                "scheduledTransferPeriod": "PT1M",
                "scheduledTransferLogLevelFilter": "Error",
                "sinks": "HotPath"
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "HotPath",
                    "EventHub": {
                        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
                        "SharedAccessKeyName": "SendRule"
                    }
                },
                {
                    "name": "applicationInsights",
                    "ApplicationInsights": "",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "errors"
                            }
                        ]
                    }
                }
            ]
        }
    },
    "StorageAccount": "{account name}"
}


"protectedSettings": {
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{storage endpoint}",
    "EventHub": {
        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "YOUR_KEY_HERE"
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="5ab1b-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ab1b-195">Next steps</span></span>
<span data-ttu-id="5ab1b-196">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="5ab1b-196">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="5ab1b-197">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5ab1b-197">Event Hubs overview</span></span>](../event-hubs/event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="5ab1b-198">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="5ab1b-198">Create an event hub</span></span>](../event-hubs/event-hubs-create.md)
* [<span data-ttu-id="5ab1b-199">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5ab1b-199">Event Hubs FAQ</span></span>](../event-hubs/event-hubs-faq.md)

<!-- Images. -->
[0]: ../event-hubs/media/event-hubs-streaming-azure-diags-data/dashboard.png
