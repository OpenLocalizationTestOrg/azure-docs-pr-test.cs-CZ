---
title: "Streamování dat diagnostiky Azure v aktivní trase pomocí služby Event Hubs | Microsoft Docs"
description: "Konfigurace Azure Diagnostics službou Event Hubs začátku do konce, včetně pokyny pro běžné scénáře."
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
ms.openlocfilehash: 1c05bd6dc4c4d394aa043b9995de9c184e4f14c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a><span data-ttu-id="96158-103">Streamování dat diagnostiky Azure v aktivní trase pomocí služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="96158-103">Streaming Azure Diagnostics data in the hot path by using Event Hubs</span></span>
<span data-ttu-id="96158-104">Azure Diagnostics poskytuje flexibilní způsoby, jak shromažďovat metriky a protokoly z cloudové služby virtuálních počítačů (VM) a přenos výsledků do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="96158-104">Azure Diagnostics provides flexible ways to collect metrics and logs from cloud services virtual machines (VMs) and transfer results to Azure Storage.</span></span> <span data-ttu-id="96158-105">Spouštění v časovém intervalu. března 2016 (SDK 2.9), můžete odeslání diagnostiky do vlastní zdroje dat a přenos dat aktivní trase v sekundách pomocí [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="96158-105">Starting in the March 2016 (SDK 2.9) time frame, you can send Diagnostics to custom data sources and transfer hot path data in seconds by using [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).</span></span>

<span data-ttu-id="96158-106">Podporované datové typy patří:</span><span class="sxs-lookup"><span data-stu-id="96158-106">Supported data types include:</span></span>

* <span data-ttu-id="96158-107">Události Trasování událostí pro Windows</span><span class="sxs-lookup"><span data-stu-id="96158-107">Event Tracing for Windows (ETW) events</span></span>
* <span data-ttu-id="96158-108">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="96158-108">Performance counters</span></span>
* <span data-ttu-id="96158-109">Protokoly událostí systému Windows</span><span class="sxs-lookup"><span data-stu-id="96158-109">Windows event logs</span></span>
* <span data-ttu-id="96158-110">Protokoly aplikací</span><span class="sxs-lookup"><span data-stu-id="96158-110">Application logs</span></span>
* <span data-ttu-id="96158-111">Protokoly infrastruktury Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="96158-111">Azure Diagnostics infrastructure logs</span></span>

<span data-ttu-id="96158-112">Tento článek ukazuje, jak nakonfigurovat Azure Diagnostics službou Event Hubs a provést tak kompletní.</span><span class="sxs-lookup"><span data-stu-id="96158-112">This article shows you how to configure Azure Diagnostics with Event Hubs from end to end.</span></span> <span data-ttu-id="96158-113">Příručka je taky pokyny pro následující běžné scénáře:</span><span class="sxs-lookup"><span data-stu-id="96158-113">Guidance is also provided for the following common scenarios:</span></span>

* <span data-ttu-id="96158-114">Postup přizpůsobení protokoly a metriky, které získat odeslaných do centra událostí</span><span class="sxs-lookup"><span data-stu-id="96158-114">How to customize the logs and metrics that get sent to Event Hubs</span></span>
* <span data-ttu-id="96158-115">Postup změny konfigurace v každé prostředí</span><span class="sxs-lookup"><span data-stu-id="96158-115">How to change configurations in each environment</span></span>
* <span data-ttu-id="96158-116">Postup zobrazení dat datového proudu Event Hubs</span><span class="sxs-lookup"><span data-stu-id="96158-116">How to view Event Hubs stream data</span></span>
* <span data-ttu-id="96158-117">Řešení potíží s připojení</span><span class="sxs-lookup"><span data-stu-id="96158-117">How to troubleshoot the connection</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="96158-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="96158-118">Prerequisites</span></span>
<span data-ttu-id="96158-119">Event Hubs receieving dat z Azure Diagnostics je podporována v cloudové služby, virtuální počítače, sady škálování virtuálního počítače a Service Fabric od Azure SDK 2.9 a odpovídající nástroje Azure pro sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="96158-119">Event Hubs receieving data from Azure Diagnostics is supported in Cloud Services, VMs, Virtual Machine Scale Sets, and Service Fabric starting in the Azure SDK 2.9 and the corresponding Azure Tools for Visual Studio.</span></span>

* <span data-ttu-id="96158-120">Rozšíření diagnostiky Azure 1.6 ([Azure SDK pro .NET 2.9 nebo novější](https://azure.microsoft.com/downloads/) cílem to ve výchozím nastavení)</span><span class="sxs-lookup"><span data-stu-id="96158-120">Azure Diagnostics extension 1.6 ([Azure SDK for .NET 2.9 or later](https://azure.microsoft.com/downloads/) targets this by default)</span></span>
* [<span data-ttu-id="96158-121">Visual Studio 2013 nebo novější</span><span class="sxs-lookup"><span data-stu-id="96158-121">Visual Studio 2013 or later</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* <span data-ttu-id="96158-122">Stávající konfigurace Azure Diagnostics v aplikaci pomocí *.wadcfgx* souboru a jeden z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="96158-122">Existing configurations of Azure Diagnostics in an application by using a *.wadcfgx* file and one of the following methods:</span></span>
  * <span data-ttu-id="96158-123">Visual Studio: [konfigurace diagnostiky pro cloudové služby Azure a virtuální počítače](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)</span><span class="sxs-lookup"><span data-stu-id="96158-123">Visual Studio: [Configuring Diagnostics for Azure Cloud Services and Virtual Machines](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)</span></span>
  * <span data-ttu-id="96158-124">Prostředí Windows PowerShell: [zapněte diagnostiku ve službě Azure Cloud Services pomocí prostředí PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="96158-124">Windows PowerShell: [Enable diagnostics in Azure Cloud Services using PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)</span></span>
* <span data-ttu-id="96158-125">Obor názvů centra událostí zřízený za v článku [Začínáme se službou Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)</span><span class="sxs-lookup"><span data-stu-id="96158-125">Event Hubs namespace provisioned per the article, [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)</span></span>

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a><span data-ttu-id="96158-126">Připojení Azure Diagnostics k podřízený Event Hubs</span><span class="sxs-lookup"><span data-stu-id="96158-126">Connect Azure Diagnostics to Event Hubs sink</span></span>
<span data-ttu-id="96158-127">Ve výchozím nastavení Azure Diagnostics vždycky odešle protokoly a metriky k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="96158-127">By default, Azure Diagnostics always sends logs and metrics to an Azure Storage account.</span></span> <span data-ttu-id="96158-128">Aplikace může přidáním nové také odesílat data do centra událostí **jímky** oddílu pod **PublicConfig** / **WadCfg** element *. wadcfgx* souboru.</span><span class="sxs-lookup"><span data-stu-id="96158-128">An application may also send data to Event Hubs by adding a new **Sinks** section under the **PublicConfig** / **WadCfg** element of the *.wadcfgx* file.</span></span> <span data-ttu-id="96158-129">V sadě Visual Studio *.wadcfgx* soubor je uložen v následující cestě: **projekt cloudové služby** > **role** > **() RoleName)** > **diagnostics.wadcfgx** souboru.</span><span class="sxs-lookup"><span data-stu-id="96158-129">In Visual Studio, the *.wadcfgx* file is stored in the following path: **Cloud Service Project** > **Roles** > **(RoleName)** > **diagnostics.wadcfgx** file.</span></span>

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

<span data-ttu-id="96158-130">V tomto příkladu nastavení adresy URL centra událostí do centra událostí, plně kvalifikovaný oboru názvů: obor názvů služby Event Hubs + "/" + název centra událostí.</span><span class="sxs-lookup"><span data-stu-id="96158-130">In this example, the event hub URL is set to the fully qualified namespace of the event hub: Event Hubs namespace  + "/" + event hub name.</span></span>  

<span data-ttu-id="96158-131">Adresa URL se zobrazí v centra událostí [portál Azure](http://go.microsoft.com/fwlink/?LinkID=213885) na řídicím panelu služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="96158-131">The event hub URL is displayed in the [Azure portal](http://go.microsoft.com/fwlink/?LinkID=213885) on the Event Hubs dashboard.</span></span>  

<span data-ttu-id="96158-132">**Jímky** název můžete nastavit na libovolný platný řetězec, tak dlouho, dokud se stejnou hodnotou se používá konzistentně napříč do konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="96158-132">The **Sink** name can be set to any valid string as long as the same value is used consistently throughout the config file.</span></span>

> [!NOTE]
> <span data-ttu-id="96158-133">Mohou existovat další jímky, jako například *applicationInsights* nakonfigurované v této části.</span><span class="sxs-lookup"><span data-stu-id="96158-133">There may be additional sinks, such as *applicationInsights* configured in this section.</span></span> <span data-ttu-id="96158-134">Azure Diagnostics umožňuje jeden nebo více jímky k být definována, pokud každý podřízený také deklarovaného v souboru **PrivateConfig** části.</span><span class="sxs-lookup"><span data-stu-id="96158-134">Azure Diagnostics allows one or more sinks to be defined if each sink is also declared in the **PrivateConfig** section.</span></span>  
>
>

<span data-ttu-id="96158-135">Event Hubs podřízený musí být také deklarovaný a definované v **PrivateConfig** části *.wadcfgx* konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="96158-135">The Event Hubs sink must also be declared and defined in the **PrivateConfig** section of the *.wadcfgx* config file.</span></span>

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

<span data-ttu-id="96158-136">`SharedAccessKeyName` Hodnota musí odpovídat klíč sdíleného přístupového podpisu (SAS) a zásad, která byla definována v **Event Hubs** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="96158-136">The `SharedAccessKeyName` value must match a Shared Access Signature (SAS) key and policy that has been defined in the **Event Hubs** namespace.</span></span> <span data-ttu-id="96158-137">Přejděte do centra událostí řídicího panelu [portál Azure](https://manage.windowsazure.com), klikněte na tlačítko **konfigurace** kartě a nastavte s názvem zásady (například "SendRule"), který má *odeslat* oprávnění.</span><span class="sxs-lookup"><span data-stu-id="96158-137">Browse to the Event Hubs dashboard in the [Azure portal](https://manage.windowsazure.com), click the **Configure** tab, and set up a named policy (for example, "SendRule") that has *Send* permissions.</span></span> <span data-ttu-id="96158-138">**StorageAccount** také deklarovaného v **PrivateConfig**.</span><span class="sxs-lookup"><span data-stu-id="96158-138">The **StorageAccount** is also declared in **PrivateConfig**.</span></span> <span data-ttu-id="96158-139">Není nutné ke změně hodnot v tomto poli, pokud pracují.</span><span class="sxs-lookup"><span data-stu-id="96158-139">There is no need to change values here if they are working.</span></span> <span data-ttu-id="96158-140">V tomto příkladu jsme ponechte hodnoty prázdná, což je přihlašovací, že podřízené asset bude nastavit hodnoty.</span><span class="sxs-lookup"><span data-stu-id="96158-140">In this example, we leave the values empty, which is a sign that a downstream asset will set the values.</span></span> <span data-ttu-id="96158-141">Například *ServiceConfiguration.Cloud.cscfg* prostředí konfigurační soubor nastaví příslušné prostředí názvů a klíče.</span><span class="sxs-lookup"><span data-stu-id="96158-141">For example, the *ServiceConfiguration.Cloud.cscfg* environment configuration file sets the environment-appropriate names and keys.</span></span>  

> [!WARNING]
> <span data-ttu-id="96158-142">Event Hubs SAS klíč, který je uložený ve formátu prostého textu v *.wadcfgx* souboru.</span><span class="sxs-lookup"><span data-stu-id="96158-142">The Event Hubs SAS key is stored in plain text in the *.wadcfgx* file.</span></span> <span data-ttu-id="96158-143">Často tento klíč se změnami do správy zdrojového kódu, nebo není k dispozici jako prostředek na vašem serveru sestavení, takže byste měli chránit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="96158-143">Often, this key is checked in to source code control or is available as an asset in your build server, so you should protect it as appropriate.</span></span> <span data-ttu-id="96158-144">Doporučujeme používat klíč SAS se zde *odesílat pouze* oprávnění tak, aby uživatel se zlými úmysly nelze zapisovat do centra událostí, ale naslouchání k němu nebo k její správě.</span><span class="sxs-lookup"><span data-stu-id="96158-144">We recommend that you use a SAS key here with *Send only* permissions so that a malicious user can write to the event hub, but not listen to it or manage it.</span></span>
>
>

## <a name="configure-azure-diagnostics-to-send-logs-and-metrics-to-event-hubs"></a><span data-ttu-id="96158-145">Konfigurace Azure Diagnostics odeslat protokoly a metriky do centra událostí</span><span class="sxs-lookup"><span data-stu-id="96158-145">Configure Azure Diagnostics to send logs and metrics to Event Hubs</span></span>
<span data-ttu-id="96158-146">Jak je popsáno dříve, všechny výchozí a vlastní diagnostická data, to znamená, metriky a protokoly, je automaticky odeslán do služby Azure Storage v nakonfigurovaných intervalech.</span><span class="sxs-lookup"><span data-stu-id="96158-146">As discussed previously, all default and custom diagnostics data, that is, metrics and logs, is automatically sent to Azure Storage in the configured intervals.</span></span> <span data-ttu-id="96158-147">Event Hubs a všechny další jímka můžete zadat libovolný uzel kořenovou nebo listu v hierarchii k odeslání do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="96158-147">With Event Hubs and any additional sink, you can specify any root or leaf node in the hierarchy to be sent to the event hub.</span></span> <span data-ttu-id="96158-148">To zahrnuje události trasování událostí, čítače výkonu, protokoly událostí systému Windows a protokolů aplikace.</span><span class="sxs-lookup"><span data-stu-id="96158-148">This includes ETW events, performance counters, Windows event logs, and application logs.</span></span>   

<span data-ttu-id="96158-149">Je důležité vzít v úvahu, kolik datových bodů by ve skutečnosti se měly převést do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="96158-149">It is important to consider how many data points should actually be transferred to Event Hubs.</span></span> <span data-ttu-id="96158-150">Vývojáři obvykle přenášet data za provozu cesty s nízkou latencí, která musí využívat a rychle interpretovat.</span><span class="sxs-lookup"><span data-stu-id="96158-150">Typically, developers transfer low-latency hot-path data that must be consumed and interpreted quickly.</span></span> <span data-ttu-id="96158-151">Systémy, které monitorují výstrahy nebo pravidel automatického škálování jsou příklady.</span><span class="sxs-lookup"><span data-stu-id="96158-151">Systems that monitor alerts or autoscale rules are examples.</span></span> <span data-ttu-id="96158-152">Vývojář může také nakonfigurovat úložišti alternativní analýzy nebo hledání úložiště – například Azure Stream Analytics, Elasticsearch, vlastní monitorování systému nebo oblíbených monitorování systému od jiných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="96158-152">A developer might also configure an alternate analysis store or search store -- for example, Azure Stream Analytics, Elasticsearch, a custom monitoring system, or a favorite monitoring system from others.</span></span>

<span data-ttu-id="96158-153">Následují některé příklad konfigurace.</span><span class="sxs-lookup"><span data-stu-id="96158-153">The following are some example configurations.</span></span>

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

<span data-ttu-id="96158-154">Ve výše uvedeném příkladu jímky použije s nadřazeným **čítače výkonu** uzlu v hierarchii, což znamená, všechny podřízené **čítače výkonu** odešle do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="96158-154">In the above example, the sink is applied to the parent **PerformanceCounters** node in the hierarchy, which means all child **PerformanceCounters** will be sent to Event Hubs.</span></span>  

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

<span data-ttu-id="96158-155">V předchozím příkladu jímky použije jenom tři čítače: **požadavky ve frontě**, **požadavky zamítnuty**, a **% času procesoru**.</span><span class="sxs-lookup"><span data-stu-id="96158-155">In the previous example, the sink is applied to only three counters: **Requests Queued**, **Requests Rejected**, and **% Processor time**.</span></span>  

<span data-ttu-id="96158-156">Následující příklad ukazuje, jak může vývojář omezit množství odeslaných dat jako kritické metriky, které se používají pro tuto službu stavu.</span><span class="sxs-lookup"><span data-stu-id="96158-156">The following example shows how a developer can limit the amount of sent data to be the critical metrics that are used for this service’s health.</span></span>  

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

<span data-ttu-id="96158-157">V tomto příkladu jímky se použije pro protokoly a vyfiltrovaná jenom na úrovni trasování chyby.</span><span class="sxs-lookup"><span data-stu-id="96158-157">In this example, the sink is applied to logs and is filtered only to error level trace.</span></span>

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a><span data-ttu-id="96158-158">Nasazení a aktualizace konfigurace aplikace a Diagnostika cloudové služby</span><span class="sxs-lookup"><span data-stu-id="96158-158">Deploy and update a Cloud Services application and diagnostics config</span></span>
<span data-ttu-id="96158-159">Visual Studio poskytuje nejjednodušší cesta k nasazení aplikace a služby Event Hubs podřízený konfigurace.</span><span class="sxs-lookup"><span data-stu-id="96158-159">Visual Studio provides the easiest path to deploy the application and Event Hubs sink configuration.</span></span> <span data-ttu-id="96158-160">Chcete-li zobrazit a upravit soubor, otevřete *.wadcfgx* souborů v sadě Visual Studio, upravovat a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="96158-160">To view and edit the file, open the *.wadcfgx* file in Visual Studio, edit it, and save it.</span></span> <span data-ttu-id="96158-161">Cesta je **projekt cloudové služby** > **role** > **(RoleName)** > **diagnostics.wadcfgx**.</span><span class="sxs-lookup"><span data-stu-id="96158-161">The path is **Cloud Service Project** > **Roles** > **(RoleName)** > **diagnostics.wadcfgx**.</span></span>  

<span data-ttu-id="96158-162">V tomto okamžiku všechny nasazení a nasazení aktualizací akcí v sadě Visual Studio, Visual Studio Team System a všechny příkazy nebo skripty, které jsou založeny na MSBuild a použít **/t: publikování** zahrnout cíl *.wadcfgx* v procesu vytváření balíčků.</span><span class="sxs-lookup"><span data-stu-id="96158-162">At this point, all deployment and deployment update actions in Visual Studio, Visual Studio Team System, and all commands or scripts that are based on MSBuild and use the **/t:publish** target include the *.wadcfgx* in the packaging process.</span></span> <span data-ttu-id="96158-163">Kromě toho nasazení a aktualizace nasazení souboru do Azure pomocí příslušné rozšíření agenta Azure Diagnostics na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="96158-163">In addition, deployments and updates deploy the file to Azure by using the appropriate Azure Diagnostics agent extension on your VMs.</span></span>

<span data-ttu-id="96158-164">Po nasazení aplikace a konfigurace Azure Diagnostics, zobrazí se okamžitě aktivity na řídicím panelu Centra událostí.</span><span class="sxs-lookup"><span data-stu-id="96158-164">After you deploy the application and Azure Diagnostics configuration, you will immediately see activity in the dashboard of the event hub.</span></span> <span data-ttu-id="96158-165">To znamená, že jste připravení přejít k zobrazení dat za běhu cestu v nástroji klienta nebo analysis naslouchací proces podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="96158-165">This indicates that you're ready to move on to viewing the hot-path data in the listener client or analysis tool of your choice.</span></span>  

<span data-ttu-id="96158-166">Na následujícím obrázku Event Hubs řídicího panelu ukazuje pořádku odesílání diagnostická data do centra událostí spouštění zopakovat po 23: 00.</span><span class="sxs-lookup"><span data-stu-id="96158-166">In the following figure, the Event Hubs dashboard shows healthy sending of diagnostics data to the event hub starting sometime after 11 PM.</span></span> <span data-ttu-id="96158-167">Pokud je aplikace nasazená s aktualizované *.wadcfgx* soubor a jímky byla nakonfigurována správně.</span><span class="sxs-lookup"><span data-stu-id="96158-167">That's when the application was deployed with an updated *.wadcfgx* file, and the sink was configured properly.</span></span>

![][0]  

> [!NOTE]
> <span data-ttu-id="96158-168">Pokud provedete aktualizace konfiguračního souboru Azure Diagnostics (.wadcfgx), se doporučuje push aktualizace bude celá aplikace a také konfigurace pomocí sady Visual Studio publikování nebo skript prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="96158-168">When you make updates to the Azure Diagnostics config file (.wadcfgx), it's recommended that you push the updates to the entire application as well as the configuration by using either Visual Studio publishing, or a Windows PowerShell script.</span></span>  
>
>

## <a name="view-hot-path-data"></a><span data-ttu-id="96158-169">Data za provozu cesty zobrazení</span><span class="sxs-lookup"><span data-stu-id="96158-169">View hot-path data</span></span>
<span data-ttu-id="96158-170">Jak je popsáno dříve, existuje mnoho případy použití pro příjem a zpracování dat služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="96158-170">As discussed previously, there are many use cases for listening to and processing Event Hubs data.</span></span>

<span data-ttu-id="96158-171">Jeden jednoduchý přístup je vytvoření konzolové aplikace testu malých k naslouchání do centra událostí a tisk do výstupního datového proudu.</span><span class="sxs-lookup"><span data-stu-id="96158-171">One simple approach is to create a small test console application to listen to the event hub and print the output stream.</span></span> <span data-ttu-id="96158-172">Můžete umístit následující kód, který je vysvětlené podrobněji v [Začínáme se službou Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), v konzolové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="96158-172">You can place the following code, which is explained in more detail in [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), in a console application.</span></span>  

<span data-ttu-id="96158-173">Všimněte si, že musí obsahovat konzolové aplikace [balíček NuGet hostitele procesor událostí](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span><span class="sxs-lookup"><span data-stu-id="96158-173">Note that the console application must include the [Event Processor Host NuGet package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span></span>  

<span data-ttu-id="96158-174">Nezapomeňte nahradit hodnoty v lomených závorkách v **hlavní** funkce s hodnotami pro vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="96158-174">Remember to replace the values in angle brackets in the **Main** function with values for your resources.</span></span>   

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

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sinks"></a><span data-ttu-id="96158-175">Řešení potíží s jímky služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="96158-175">Troubleshoot Event Hubs sinks</span></span>
* <span data-ttu-id="96158-176">Centra událostí nezobrazuje aktivity příchozích nebo odchozích události podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="96158-176">The event hub does not show incoming or outgoing event activity as expected.</span></span>

    <span data-ttu-id="96158-177">Zkontrolujte, že je úspěšně zřízený Centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="96158-177">Check that your event hub is successfully provisioned.</span></span> <span data-ttu-id="96158-178">Všechny informace o připojení ve **PrivateConfig** části *.wadcfgx* musí shodovat s hodnotami prostředku, jak je vidět na portálu.</span><span class="sxs-lookup"><span data-stu-id="96158-178">All connection info in the **PrivateConfig** section of *.wadcfgx* must match the values of your resource as seen in the portal.</span></span> <span data-ttu-id="96158-179">Ujistěte se, že máte SAS zásady definované ("SendRule" v příkladu) v portálu a které *odeslat* je povoleno.</span><span class="sxs-lookup"><span data-stu-id="96158-179">Make sure that you have a SAS policy defined ("SendRule" in the example) in the portal and that *Send* permission is granted.</span></span>  
* <span data-ttu-id="96158-180">Po aktualizaci už centra událostí zobrazuje aktivity příchozích nebo odchozích události.</span><span class="sxs-lookup"><span data-stu-id="96158-180">After an update, the event hub no longer shows incoming or outgoing event activity.</span></span>

    <span data-ttu-id="96158-181">Nejprve se ujistěte, zda je správný, jak je popsáno dříve rozbočovače a konfigurační informace o události.</span><span class="sxs-lookup"><span data-stu-id="96158-181">First, make sure that the event hub and configuration info is correct as explained previously.</span></span> <span data-ttu-id="96158-182">Někdy **PrivateConfig** v aktualizaci nasazení se resetuje.</span><span class="sxs-lookup"><span data-stu-id="96158-182">Sometimes the **PrivateConfig** is reset in a deployment update.</span></span> <span data-ttu-id="96158-183">Doporučené opravy je to, aby všechny změny *.wadcfgx* v projektu a pak nabízené aktualizaci dokončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="96158-183">The recommended fix is to make all changes to *.wadcfgx* in the project and then push a complete application update.</span></span> <span data-ttu-id="96158-184">Pokud to není možné, ujistěte se, že aktualizace diagnostiky nabízených oznámení úplná **PrivateConfig** který obsahuje klíč SAS.</span><span class="sxs-lookup"><span data-stu-id="96158-184">If this is not possible, make sure that the diagnostics update pushes a complete **PrivateConfig** that includes the SAS key.</span></span>  
* <span data-ttu-id="96158-185">Byl proveden o návrhy a centra událostí, ale stále nefunguje.</span><span class="sxs-lookup"><span data-stu-id="96158-185">I tried the suggestions, and the event hub is still not working.</span></span>

    <span data-ttu-id="96158-186">Podívejte se do v Azure Storage tabulka, která obsahuje chyby a protokolování Azure Diagnostics samotné: **WADDiagnosticInfrastructureLogsTable**.</span><span class="sxs-lookup"><span data-stu-id="96158-186">Try looking in the Azure Storage table that contains logs and errors for Azure Diagnostics itself: **WADDiagnosticInfrastructureLogsTable**.</span></span> <span data-ttu-id="96158-187">Jednou z možností je použít nástroj, jako například [Azure Storage Explorer](http://www.storageexplorer.com) se pokud chcete připojit k tomuto účtu úložiště, zobrazit tuto tabulku a přidat dotaz pro časové razítko za posledních 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="96158-187">One option is to use a tool such as [Azure Storage Explorer](http://www.storageexplorer.com) to connect to this storage account, view this table, and add a query for TimeStamp in the last 24 hours.</span></span> <span data-ttu-id="96158-188">Nástroj můžete exportovat soubor .csv a otevře ji v aplikaci, jako je například aplikace Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="96158-188">You can use the tool to export a .csv file and open it in an application such as Microsoft Excel.</span></span> <span data-ttu-id="96158-189">Excel usnadňuje hledat řetězce volací karty, například **EventHubs**, pokud chcete zobrazit, jaké se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="96158-189">Excel makes it easy to search for calling-card strings, such as **EventHubs**, to see what error is reported.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="96158-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96158-190">Next steps</span></span>
<span data-ttu-id="96158-191">• [Další informace o službě Event Hubs](https://azure.microsoft.com/services/event-hubs/)</span><span class="sxs-lookup"><span data-stu-id="96158-191">•    [Learn more about Event Hubs](https://azure.microsoft.com/services/event-hubs/)</span></span>

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a><span data-ttu-id="96158-192">Dodatek: Dokončení příkladu Azure Diagnostics konfigurační soubor (.wadcfgx)</span><span class="sxs-lookup"><span data-stu-id="96158-192">Appendix: Complete Azure Diagnostics configuration file (.wadcfgx) example</span></span>
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

<span data-ttu-id="96158-193">Doplňkové *ServiceConfiguration.Cloud.cscfg* pro tento příklad vypadá jako následující.</span><span class="sxs-lookup"><span data-stu-id="96158-193">The complementary *ServiceConfiguration.Cloud.cscfg* for this example looks like the following.</span></span>

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

<span data-ttu-id="96158-194">Ekvivalentní Json na základě nastavení pro virtuální počítače je následující:</span><span class="sxs-lookup"><span data-stu-id="96158-194">Equivalent Json based settings for virtual machines is as follows:</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="96158-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96158-195">Next steps</span></span>
<span data-ttu-id="96158-196">Další informace o službě Event Hubs najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="96158-196">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="96158-197">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="96158-197">Event Hubs overview</span></span>](../event-hubs/event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="96158-198">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="96158-198">Create an event hub</span></span>](../event-hubs/event-hubs-create.md)
* [<span data-ttu-id="96158-199">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="96158-199">Event Hubs FAQ</span></span>](../event-hubs/event-hubs-faq.md)

<!-- Images. -->
[0]: ../event-hubs/media/event-hubs-streaming-azure-diags-data/dashboard.png
